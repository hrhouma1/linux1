<h1 id="intro">TP 2 — Guide et éléments de installer NGINX + trois Wiki.js (dev / test / prod) sur Ubuntu 22.04</h1>


## <h1 id="0">0. Schéma cible</h1>

| Sous-domaine     | Port interne | Dossier               | Compte Linux  | Base MariaDB  |
| ---------------- | ------------ | --------------------- | ------------- | ------------- |
| dev.site1.local  | 3000         | `/var/www/dev.site1`  | `wikijs_dev`  | `wikidb_dev`  |
| test.site1.local | 3001         | `/var/www/test.site1` | `wikijs_test` | `wikidb_test` |
| prod.site1.local | 3002         | `/var/www/prod.site1` | `wikijs_prod` | `wikidb_prod` |



## <h1 id="1">1. Pré-requis système</h1>

```bash
# 1.1 Connexion root ou sudo
ssh ubuntu@IP_DE_LA_VM
sudo -i                       # passe en root

# 1.2 Mise à jour de base
apt update && apt -y upgrade  # ( -y évite la question interactive )

# 1.3 Paquets utilitaires (nano, curl…)
apt install -y nano curl gnupg lsb-release ca-certificates

# 1.4 Ouvrir le pare-feu minimal (HTTP + SSH)
ufw allow OpenSSH
ufw allow 80/tcp
ufw enable
```

Vérification :

```bash
ufw status      # doit lister 22/tcp (OpenSSH) et 80/tcp (ALLOW)
```



## <h1 id="2">2. Installation **pas à pas** de NGINX</h1>

| Étape | Commande                                | Explication                                                                                 |
| ----- | --------------------------------------- | ------------------------------------------------------------------------------------------- |
| 2.1   | `apt install -y nginx`                  | Télécharge et installe le serveur Web                                                       |
| 2.2   | `systemctl start  nginx`                | Démarre immédiatement                                                                       |
| 2.3   | `systemctl enable nginx`                | Lance NGINX au prochain boot                                                                |
| 2.4   | `systemctl status nginx`                | Vérifie qu’il est **active (running)**                                                      |
| 2.5   | `nginx -t`                              | Teste la syntaxe de tous les fichiers ; doit afficher : *syntax is ok / test is successful* |
| 2.6   | Ouvrir un navigateur sur `http://IP_VM` | On doit voir *Welcome to nginx!*                                                            |



## <h1 id="3">3. Fichier `/etc/hosts` local</h1>

```bash
nano /etc/hosts
```

Ajouter ou compléter :

```
127.0.0.1  dev.site1.local test.site1.local prod.site1.local
```

> Cette ligne permet au navigateur **local** de « résoudre » les noms sans DNS externe.



## <h1 id="4">4. Installation des dépendances Wiki.js</h1>

### 4.1 Git

```bash
apt install -y git
git --version           # vérifie l’installation
```

### 4.2 Node .js LTS + Yarn

```bash
curl -fsSL https://deb.nodesource.com/setup_lts.x | bash -
apt install -y nodejs
npm install -g yarn
node -v && npm -v       # vérifie les versions
```

### 4.3 MariaDB

```bash
apt install -y mariadb-server
systemctl enable --now mariadb
systemctl status mariadb           # doit être **active**
mysql_secure_installation          # renforce l’instance (choix par défaut OK)
```

Créer les trois bases et l’utilisateur commun `wikijs` :

```bash
mysql -uroot -p        # entrez le mot de passe root choisi

CREATE DATABASE wikidb_dev  CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
CREATE DATABASE wikidb_test CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
CREATE DATABASE wikidb_prod CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;

CREATE USER 'wikijs'@'localhost' IDENTIFIED BY 'S3cUr3!';
GRANT ALL PRIVILEGES ON wikidb_dev.*  TO 'wikijs'@'localhost';
GRANT ALL PRIVILEGES ON wikidb_test.* TO 'wikijs'@'localhost';
GRANT ALL PRIVILEGES ON wikidb_prod.* TO 'wikijs'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```



## <h1 id="5">5. Création des comptes Linux et dossiers</h1>

```bash
for env in dev test prod; do
  adduser --system --group --home /var/www/${env}.site1 wikijs_${env}
done
```

Vérifier :

```bash
ls -ld /var/www/*.site1     # trois dossiers vides (propriété wikijs_env)
```



## <h1 id="6">6. Téléchargement et configuration de Wiki.js (x3)</h1>

> **Astuce pédagogique** : on fait le **premier** ensemble (dev), puis on réplique à l’identique.

### 6.1 Instance **dev**

```bash
su - wikijs_dev
WIKI_VERSION=$(curl -s https://api.github.com/repos/requarks/wiki/releases/latest \
               | grep tag_name | cut -d '"' -f 4)
wget https://github.com/requarks/wiki/releases/download/${WIKI_VERSION}/wiki-js.tar.gz
tar xzf wiki-js.tar.gz && rm wiki-js.tar.gz
cp config.sample.yml config.yml
```

Éditer `config.yml` :

```yaml
bindIP: 127.0.0.1
port:   3000

db:
  type: mariadb
  host: 127.0.0.1
  port: 3306
  user: wikijs
  pass: S3cUr3!
  db:   wikidb_dev
  ssl:  false
```

> **Important :** supprimer ou commenter ***toutes*** les sections `sslOptions:`, `schema:`, `storage:` pour éviter les erreurs d’indentation.

Quitter l’utilisateur dev :

```bash
exit
```

### 6.2 Copier–coller pour **test** et **prod**

Rejouer exactement la même séquence en changeant seulement :

| Variable | test                  | prod                  |
| -------- | --------------------- | --------------------- |
| Compte   | `wikijs_test`         | `wikijs_prod`         |
| Dossier  | `/var/www/test.site1` | `/var/www/prod.site1` |
| Port     | `3001`                | `3002`                |
| Base     | `wikidb_test`         | `wikidb_prod`         |


## <h1 id="7">7. Service systemd générique</h1>

Créer `/etc/systemd/system/wikijs@.service` **une seule fois** :

```ini
[Unit]
Description=Wiki.js instance (%i)
After=network.target mariadb.service
StartLimitIntervalSec=0

[Service]
Type=simple
User=wikijs_%i
WorkingDirectory=/var/www/%i.site1
Environment=NODE_ENV=production
ExecStart=/usr/bin/node server
Restart=always

[Install]
WantedBy=multi-user.target
```

Activer les trois unités :

```bash
for env in dev test prod; do
  systemctl enable --now wikijs@${env}
  systemctl status wikijs@${env} --no-pager
done
```

Si `active (running)` → OK.
Sinon : `journalctl -u wikijs@dev -n 50` pour lire l’erreur (indentation, mot de passe…).



## <h1 id="8">8. Configuration NGINX ultra-commentée</h1>

```bash
nano /etc/nginx/sites-available/site1.local.conf
```

Copier intégralement :

```nginx
# Reverse proxy Wiki.js -- trois sous-domaines
map $http_upgrade $connection_upgrade {
    default upgrade;
    ''      close;
}

server {
    listen 80;
    server_name dev.site1.local test.site1.local prod.site1.local;

    # Bloc unique : route selon $host
    location / {
        if ($host = dev.site1.local)  { proxy_pass http://127.0.0.1:3000; }
        if ($host = test.site1.local) { proxy_pass http://127.0.0.1:3001; }
        if ($host = prod.site1.local) { proxy_pass http://127.0.0.1:3002; }

        proxy_http_version 1.1;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection $connection_upgrade;
    }

    access_log /var/log/nginx/site1.access.log;
    error_log  /var/log/nginx/site1.error.log warn;
}
```

Activer + test + reload :

```bash
ln -s /etc/nginx/sites-available/site1.local.conf /etc/nginx/sites-enabled/
nginx -t                   # syntax is ok / test is successful
systemctl reload nginx
systemctl status nginx     # toujours running
```


## <h1 id="9">9. Validation finale</h1>

```bash
for h in dev test prod; do
  curl -I http://$h.site1.local
done
```

Chaque réponse doit comporter `HTTP/1.1 200 OK`.
Puis, dans le navigateur de la VM ou de votre poste (si hosts modifié) :

* `http://dev.site1.local` → interface d’installation Wiki.js (port 3000)
* `http://test.site1.local` → interface (3001)
* `http://prod.site1.local` → interface (3002)


## <h1 id="10">10. Dépannage rapide</h1>

| Message                            | Cause courante            | Correctif                                                      |
| ---------------------------------- | ------------------------- | -------------------------------------------------------------- |
| `502 Bad Gateway`                  | Service Wiki.js arrêté    | `systemctl restart wikijs@dev` puis `journalctl -u wikijs@dev` |
| `curl: (6) Could not resolve host` | /etc/hosts mal écrit      | Vérifier ligne `127.0.0.1  dev.site1.local …`                  |
| `ER_ACCESS_DENIED` dans les logs   | Mot de passe DB incorrect | Re-éditer `config.yml` + redémarrer service                    |



## <h1 id="11">11. Aller plus loin (optionnel)</h1>

1. Ajouter HTTPS autosigné ou Let’s Encrypt : créer un bloc `server` sur 443, paramétrer `ssl_certificate`/`ssl_certificate_key`, puis `certbot --nginx -d …` si domaine public.
2. Centraliser les logs dans **Grafana Loki** ou **Elastic**.
3. Superviser les trois services avec **Prometheus** (`node_exporter`) + **Alertmanager**.


<br/>

# Annexe 1 -  Pourquoi **un seul** fichier suffit ? — principe des « unités template » systemd



| Terme                                               | Signification rapide                                     |
| --------------------------------------------------- | -------------------------------------------------------- |
| **[wikijs@.service](mailto:wikijs@.service)**       | *Template* (gabarit) contenant un `@` avant l’extension. |
| **[wikijs@dev.service](mailto:wikijs@dev.service)** | *Instance* du template, identifiée par `dev` (= **%i**). |

1. **On écrit le gabarit *une seule fois*** :
   `/etc/systemd/system/wikijs@.service`
   Dans ce fichier, les variables `%i` (et éventuellement `%I`) seront remplacées à l’exécution par la partie qui suit le `@` dans le nom de l’instance.

2. **On déclare ensuite autant d’instances qu’on veut, sans recopier le fichier** :

```bash
# Création/activation immédiate de trois instances
sudo systemctl enable --now wikijs@dev
sudo systemctl enable --now wikijs@test
sudo systemctl enable --now wikijs@prod
```

* `wikijs@dev` ==> `%i = dev`
  → `User=wikijs_dev`
  → `WorkingDirectory=/var/www/dev.site1`

* `wikijs@test` ==> `%i = test`
  → `User=wikijs_test`
  → `WorkingDirectory=/var/www/test.site1`

* `wikijs@prod` ==> `%i = prod`
  → `User=wikijs_prod`
  → `WorkingDirectory=/var/www/prod.site1`

3. **Vérifier chaque instance** :

```bash
sudo systemctl status wikijs@dev
sudo systemctl status wikijs@test
sudo systemctl status wikijs@prod
```

4. **Redémarrer ou arrêter une instance précise** :

```bash
sudo systemctl restart wikijs@prod     # uniquement prod
sudo systemctl stop wikijs@test        # uniquement test
```

#### En résumé

* **Un seul fichier** `wikijs@.service` suffit.
* Le même gabarit est **réutilisé** pour chaque environnement grâce au nom d’instance après le `@`.
* Cela évite trois copies quasi identiques et rend la maintenance (mise à jour, options) bien plus simple.


