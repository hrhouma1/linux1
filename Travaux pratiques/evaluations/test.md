# TP 2 – Déploiement multi-instance Wiki.js avec Reverse Proxy NGINX

## Table des matières

1. [Préparation de l’environnement](#1-preparation)
2. [Installation de PostgreSQL et création des bases](#2-postgresql)
3. [Téléchargement et configuration des trois instances Wiki.js](#3-wikijs)
4. [Création des services systemd](#4-systemd)
5. [Configuration complète de NGINX](#5-nginx)
6. [Modification de `/etc/hosts`](#6-hosts)
7. [Validation et dépannage](#7-validation)
8. [Comparaison des approches d’architecture](#8-analyse)

---

## 1. Préparation de l’environnement <a id="1-preparation"></a>

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install nginx curl unzip wget sudo gnupg2 git -y
```

Création des comptes systèmes :

```bash
sudo adduser wikijs_dev --gecos "" --disabled-password
sudo adduser wikijs_test --gecos "" --disabled-password
sudo adduser wikijs_prod --gecos "" --disabled-password
```

Création des répertoires d’installation :

```bash
sudo mkdir -p /var/www/dev.site1
sudo mkdir -p /var/www/test.site1
sudo mkdir -p /var/www/prod.site1
```

---

## 2. Installation de PostgreSQL et création des bases <a id="2-postgresql"></a>

```bash
sudo apt install postgresql postgresql-contrib -y
sudo systemctl enable --now postgresql
```

Création des utilisateurs et des bases :

```bash
sudo -u postgres psql
```

```sql
-- Environnement DEV
CREATE DATABASE wikidb_dev;
CREATE USER wikijs_dev WITH ENCRYPTED PASSWORD 'wikijs_dev_pass';
GRANT ALL PRIVILEGES ON DATABASE wikidb_dev TO wikijs_dev;

-- Environnement TEST
CREATE DATABASE wikidb_test;
CREATE USER wikijs_test WITH ENCRYPTED PASSWORD 'wikijs_test_pass';
GRANT ALL PRIVILEGES ON DATABASE wikidb_test TO wikijs_test;

-- Environnement PROD
CREATE DATABASE wikidb_prod;
CREATE USER wikijs_prod WITH ENCRYPTED PASSWORD 'wikijs_prod_pass';
GRANT ALL PRIVILEGES ON DATABASE wikidb_prod TO wikijs_prod;
\q
```

---

## 3. Téléchargement et configuration de Wiki.js <a id="3-wikijs"></a>

Script de déploiement :

```bash
install_wikijs () {
  SITE=$1
  PORT=$2
  USER=$3
  DBNAME=$4
  DBUSER=$5
  DBPASS=$6

  cd /var/www/${SITE}.site1

  sudo wget https://github.com/Requarks/wiki/releases/latest/download/wiki-js.tar.gz
  sudo tar xzf wiki-js.tar.gz
  sudo rm wiki-js.tar.gz
  sudo cp config.sample.yml config.yml

  sudo chown -R ${USER}:${USER} .

  sudo sed -i "s/port:.*/port: ${PORT}/" config.yml
  sudo sed -i "s/host:.*/host: 127.0.0.1/" config.yml
  sudo sed -i "s/type:.*/type: postgres/" config.yml
  sudo sed -i "s/user:.*/user: ${DBUSER}/" config.yml
  sudo sed -i "s/password:.*/password: '${DBPASS}'/" config.yml
  sudo sed -i "s/database:.*/database: ${DBNAME}/" config.yml
}
```

Exécuter pour chaque environnement :

```bash
install_wikijs dev 3000 wikijs_dev wikidb_dev wikijs_dev wikijs_dev_pass
install_wikijs test 3001 wikijs_test wikidb_test wikijs_test wikijs_test_pass
install_wikijs prod 3002 wikijs_prod wikidb_prod wikijs_prod wikijs_prod_pass
```

---

## 4. Création des services `systemd` <a id="4-systemd"></a>

Fichier `/etc/systemd/system/wikijs-dev.service` :

```ini
[Unit]
Description=Wiki.js - Dev
After=network.target

[Service]
Type=simple
User=wikijs_dev
WorkingDirectory=/var/www/dev.site1
ExecStart=/usr/bin/node server
Restart=always
Environment=NODE_ENV=production

[Install]
WantedBy=multi-user.target
```

Cloner et adapter pour les autres :

```bash
sudo cp /etc/systemd/system/wikijs-dev.service /etc/systemd/system/wikijs-test.service
sudo cp /etc/systemd/system/wikijs-dev.service /etc/systemd/system/wikijs-prod.service

sudo sed -i 's/dev/test/g' /etc/systemd/system/wikijs-test.service
sudo sed -i 's/dev/prod/g' /etc/systemd/system/wikijs-prod.service
```

Activation :

```bash
sudo systemctl daemon-reexec
sudo systemctl daemon-reload
sudo systemctl enable --now wikijs-dev
sudo systemctl enable --now wikijs-test
sudo systemctl enable --now wikijs-prod
```

---

## 5. Configuration complète de NGINX <a id="5-nginx"></a>

Créer le fichier `/etc/nginx/sites-available/site1.local` :

```nginx
server {
    listen 80;
    server_name dev.site1.local;

    location / {
        proxy_pass http://127.0.0.1:3000;
        proxy_http_version 1.1;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}

server {
    listen 80;
    server_name test.site1.local;

    location / {
        proxy_pass http://127.0.0.1:3001;
        proxy_http_version 1.1;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}

server {
    listen 80;
    server_name prod.site1.local;

    location / {
        proxy_pass http://127.0.0.1:3002;
        proxy_http_version 1.1;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
```

Activation :

```bash
sudo ln -s /etc/nginx/sites-available/site1.local /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl restart nginx
```

---

## 6. Modification du fichier `/etc/hosts` <a id="6-hosts"></a>

```bash
sudo nano /etc/hosts

# Ajouter à la fin :
127.0.0.1 dev.site1.local
127.0.0.1 test.site1.local
127.0.0.1 prod.site1.local
```



## 7. Validation et dépannage <a id="7-validation"></a>

Commandes de test :

```bash
curl http://dev.site1.local
curl http://test.site1.local
curl http://prod.site1.local
```

Erreurs fréquentes :

* `EADDRINUSE` : port déjà utilisé
* `ECONNREFUSED` : erreur PostgreSQL (mauvais mot de passe ou service non démarré)
* `permission denied` : droits sur `/var/www` incorrects
* Wiki.js ne démarre pas : consulter `journalctl -u wikijs-dev.service`



## 8. Comparaison des approches architecturales <a id="8-analyse"></a>

| Critère                    | Sous-domaines (`dev.site1.local`) | Chemins (`site1.local/dev`)     |
| -------------------------- | --------------------------------- | ------------------------------- |
| Cloisonnement réseau       | Excellent                         | Faible                          |
| Cookies / authentification | Indépendants                      | Partagés                        |
| Configuration DNS          | Native                            | Contournement via URL rewriting |
| Maintenance / débogage     | Simples (1 vhost = 1 app)         | Complexité croissante           |
| Déploiement CI/CD          | Par environnement                 | Dépend de la logique d’URL      |

**Recommandation** : Utiliser les sous-domaines pour une séparation propre, une évolutivité facile, et une meilleure sécurité.


