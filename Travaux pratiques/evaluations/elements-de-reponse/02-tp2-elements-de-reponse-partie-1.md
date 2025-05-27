TP 2 – *Gestion de plusieurs sites web avec un reverse proxy* (Wiki.js + NGINX sur Ubuntu 22.04)

<h1 id="partie1"> PARTIE 1 – INSTALLATION ET CONFIGURATION DE WIKI.JS (40 POINTS)</h1>

### 1. Créer les utilisateurs Linux dédiés

```bash
sudo adduser wikijs_dev
sudo adduser wikijs_test
sudo adduser wikijs_prod
```



### 2. Créer les bases de données MariaDB

```sql
CREATE DATABASE wikidb_dev;
CREATE DATABASE wikidb_test;
CREATE DATABASE wikidb_prod;

CREATE USER 'wikijs'@'localhost' IDENTIFIED BY 'StrongPassword123!';
GRANT ALL PRIVILEGES ON wikidb_dev.* TO 'wikijs'@'localhost';
GRANT ALL PRIVILEGES ON wikidb_test.* TO 'wikijs'@'localhost';
GRANT ALL PRIVILEGES ON wikidb_prod.* TO 'wikijs'@'localhost';
FLUSH PRIVILEGES;
```



### 3. Installer manuellement Wiki.js

Pour chaque instance (`dev`, `test`, `prod`) :

```bash
sudo mkdir -p /var/www/dev.site1
sudo chown wikijs_dev: /var/www/dev.site1
sudo -u wikijs_dev bash
cd /var/www/dev.site1
curl -sSo wiki.tar.gz https://github.com/Requarks/wiki/releases/latest/download/wiki-js.tar.gz
tar xzf wiki.tar.gz
cp config.sample.yml config.yml
exit
```



### 4. Modifier `config.yml`

Exemple pour `dev.site1` :

```yaml
host: 127.0.0.1
port: 3000
db:
  type: mariadb
  host: localhost
  port: 3306
  user: wikijs
  pass: StrongPassword123!
  db: wikidb_dev
```

Faire de même pour `test.site1` (port 3001, base `wikidb_test`) et `prod.site1` (port 3002, base `wikidb_prod`).



### 5. Créer les services systemd

Exemple : `/etc/systemd/system/wikijs-dev.service`

```ini
[Unit]
Description=Wiki.js - Dev
After=network.target mariadb.service

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

Activer les services :

```bash
sudo systemctl daemon-reload
sudo systemctl enable --now wikijs-dev
sudo systemctl enable --now wikijs-test
sudo systemctl enable --now wikijs-prod
```



<h1 id="partie2"> PARTIE 2 – CONFIGURATION DU REVERSE PROXY NGINX (30 POINTS)</h1>

### 6. Configurer NGINX avec sous-domaines

Exemple : `/etc/nginx/sites-available/dev.site1.local`

```nginx
server {
    listen 80;
    server_name dev.site1.local;

    location / {
        proxy_pass http://127.0.0.1:3000;
        proxy_http_version 1.1;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

Répéter pour `test.site1.local` (port 3001) et `prod.site1.local` (port 3002).

Créer les liens symboliques :

```bash
sudo ln -s /etc/nginx/sites-available/dev.site1.local /etc/nginx/sites-enabled/
sudo ln -s /etc/nginx/sites-available/test.site1.local /etc/nginx/sites-enabled/
sudo ln -s /etc/nginx/sites-available/prod.site1.local /etc/nginx/sites-enabled/
```



### 7. Modifier `/etc/hosts`

```bash
sudo nano /etc/hosts
```

Ajouter :

```
127.0.0.1 dev.site1.local test.site1.local prod.site1.local
```



### 8. Tester l'accès

```bash
curl http://dev.site1.local
curl http://test.site1.local
curl http://prod.site1.local
```



<h1 id="partie3"> PARTIE 3 – VALIDATION, DÉPANNAGE ET ANALYSE (30 POINTS)</h1>

### 9. Problèmes fréquents

* **Indentation YAML** : attention aux espaces, utiliser un éditeur comme `nano` ou `vim`.
* **Connexion DB** : tester avec `mysql -u wikijs -p wikidb_dev`.
* **Port déjà utilisé** : `sudo ss -tulnp | grep 300X`.
* **Droits** : vérifier propriétaire du dossier `ls -l /var/www`.



### 10. Justifications

* **Utilisateurs séparés** : sécurité, isolation, traçabilité.
* **Reverse proxy** : centralisation, un seul point d’entrée, HTTPS facile.
* **Ports exposés** : attaque directe, scan réseau, contournement des contrôles d’accès.



### 11. Sécurisation HTTPS

1. En local : certificat autosigné
2. En prod : utiliser `certbot` avec DNS réel

```bash
sudo apt install certbot python3-certbot-nginx
sudo certbot --nginx -d dev.site1.local -d test.site1.local -d prod.site1.local
```

Configurer également `bindIP: 127.0.0.1` dans chaque `config.yml`.








