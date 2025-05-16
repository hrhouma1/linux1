# Installer Wiki.js sur Ubuntu 22.04 LTS

Ubuntu 22.04 LTS (iso)
[https://releases.ubuntu.com/22.04/ubuntu-22.04-live-server-amd64.iso](https://releases.ubuntu.com/22.04/ubuntu-22.04-live-server-amd64.iso)

## Introduction

Ce document guide à travers l'installation de Wiki.js, un moteur Wiki moderne et puissant basé sur Node.js, Git, et Markdown. Wiki.js est distribué sous la licence Affero GNU General Public License et supporte plusieurs systèmes de gestion de bases de données.

## Prérequis

- Ubuntu 22.04 LTS
- Accès à un terminal avec droits sudo
- Connexion internet

## Installation de Git

Wiki.js nécessite Git. Pour l'installer sur Ubuntu 22.04, suivez ces étapes :

```bash
sudo -s (ou su)
sudo apt update
sudo apt install git
```

Vérifiez l'installation :

```bash
git --version
```

## Installation de Node.js

Wiki.js fonctionne avec Node.js. Installez la version LTS de Node.js en utilisant le script de NodeSource :

```bash
curl -fsSL https://deb.nodesource.com/setup_lts.x | sudo -E bash -
sudo apt-get install -y nodejs
```

Vérifiez les versions installées :

```bash
node -v && npm -v
```

## Installation et configuration de MariaDB

Installez MariaDB pour gérer les données de Wiki.js :

```bash
sudo apt install mariadb-server mariadb-client
sudo systemctl start mariadb
sudo systemctl enable mariadb
sudo systemctl status mariadb
mariadb
exit;
mysql
exit;
sudo mariadb-secure-installation
```

Renforcez l'instance à l'aide de la commande :

```bash
sudo mariadb-secure-installation
```

![image](https://github.com/user-attachments/assets/3019525a-f7aa-4399-b070-07d971a9e44f)



Suivez les instructions pour sécuriser l'installation de MariaDB. Ensuite, connectez-vous à MariaDB et configurez la base de données :

```bash
sudo mysql -u root -p
```

Dans le shell MariaDB, exécutez les commandes suivantes :

```sql
CREATE DATABASE wikidb;
CREATE USER 'wikidb_user'@'localhost' IDENTIFIED BY 'PASSWORD';
GRANT ALL PRIVILEGES ON wikidb.* TO 'wikidb_user'@'localhost';
FLUSH PRIVILEGES;
show databses;
use wikidb;
show tables;
EXIT;
```

## Téléchargement et installation de Wiki.js

Créez un utilisateur et un dossier pour Wiki.js :

### Créez un utilisateur et un dossier pour Wiki.js :

```sh
sudo adduser --system --group wikijs
sudo mkdir -p /var/www/wikijs
 ls -la /var/www/
sudo chown -R wikijs:wikijs /var/www/wikijs
 ls -la /var/www/
ls -la /var/www/wikijs
```

### Modifiez le shell de l'utilisateur `wikijs` pour permettre la connexion :

```sh
sudo usermod -s /bin/bash wikijs
```

### Téléchargez et installez Wiki.js :

```sh
sudo su - wikijs
pwd
whoami
cd /var/www/wikijs
wget https://github.com/Requarks/wiki/releases/latest/download/wiki-js.tar.gz
tar xzf wiki-js.tar.gz
ls -la
rm wiki-js.tar.gz
cp config.sample.yml config.yml
nano config.yml
```

Configurez le fichier `config.yml` pour utiliser MariaDB :

```yaml
db:
  type: mariadb
  host: localhost
  port: 3306
  user: wikidb_user
  pass: PASSWORD
  db: wikidb
  ssl: false

bindIP: 127.0.0.1
```

## Création d'un service systemd

- Avant de créer le service, il est important de revenir à l'utilisateur principal root en utilisant la commande su
- Créez un service pour gérer Wiki.js :

```bash
su
sudo nano /etc/systemd/system/wikijs.service
```

## Section de Troubleshooting #1

- Une fois arrivé ici, vous allez devoir changer le mot de passe de l'utilistaeur wikijs et l'ajouter aux sudoers


```bash
whoami ==> wikijs
exit; ou (sudo -s) ou (su)
whoami ==> root
sudo passwd wikijs
sudo usermod -aG sudo wikijs
su - wikijs
sudo nano /etc/systemd/system/wikijs.service
```



```bash
su
sudo nano /etc/systemd/system/wikijs.service
```

Ajoutez le contenu suivant :

```ini
[Unit]
Description=Wiki.js
After=network.target

[Service]
Type=simple
ExecStart=/usr/bin/node server
Restart=always

User=wikijs
Environment=NODE_ENV=production
WorkingDirectory=/var/www/wikijs

[Install]
WantedBy=multi-user.target
```

Activez et démarrez le service :

```bash
sudo systemctl daemon-reload
sudo systemctl start wikijs
sudo systemctl enable wikijs
systemctl status wikijs
```

## Section de Troubleshooting #2


```bash
sudo journalctl -u wikijs.service --no-pager --lines=50




sudo systemctl daemon-reload
sudo systemctl start wikijs
sudo systemctl enable wikijs
systemctl status wikijs
```

### pb1
![image](https://github.com/user-attachments/assets/04fe428f-fed9-4d62-9406-73168c862a4e)

```bash
sudo journalctl -u wikijs.service --no-pager --lines=50
```

```ssh
May 14 13:29:17 wikiSrv node[5808]: bad indentation of a mapping entry at line 37, column 3:
May 14 13:29:17 wikiSrv node[5808]:       sslOptions:
May 14 13:29:17 wikiSrv node[5808]:       ^
May 14 13:29:17 wikiSrv node[5808]: >>> Unable to read configuration file! Did you create the config.yml file?
May 14 13:29:17 wikiSrv systemd[1]: wikijs.service: Main process exited, code=exited, status=1/FAILURE
May 14 13:29:17 wikiSrv systemd[1]: wikijs.service: Failed with result 'exit-code'.
May 14 13:29:17 wikiSrv systemd[1]: wikijs.service: Scheduled restart job, restart counter is at 5.
May 14 13:29:17 wikiSrv systemd[1]: Stopped Wiki.js.
May 14 13:29:17 wikiSrv systemd[1]: wikijs.service: Start request repeated too quickly.
May 14 13:29:17 wikiSrv systemd[1]: wikijs.service: Failed with result 'exit-code'.
May 14 13:29:17 wikiSrv systemd[1]: Failed to start Wiki.js.
```

![image](https://github.com/user-attachments/assets/baf8441a-0477-4e67-982d-ed73d8ee47e9)

```ssh
cd /var/www/wikijs
nano config.yml
commenter les deux lignes
#  sslOptions:
#    auto: true
    # rejectUnauthorized: false
```


```ssh
sudo systemctl daemon-reload
sudo systemctl start wikijs
sudo systemctl enable wikijs
systemctl status wikijs
```


### pb2
![image](https://github.com/user-attachments/assets/7982dad5-8b96-4e52-9a5f-7f9ad7e155fd)


```ssh
May 14 13:36:52 wikiSrv node[5954]: bad indentation of a mapping entry at line 47, column 3:
May 14 13:36:52 wikiSrv node[5954]:       schema: public
May 14 13:36:52 wikiSrv node[5954]:       ^
May 14 13:36:52 wikiSrv node[5954]: >>> Unable to read configuration file! Did you create the config.yml file?
May 14 13:36:52 wikiSrv systemd[1]: wikijs.service: Main process exited, code=exited, status=1/FAILURE
May 14 13:36:52 wikiSrv systemd[1]: wikijs.service: Failed with result 'exit-code'.
May 14 13:36:52 wikiSrv systemd[1]: wikijs.service: Scheduled restart job, restart counter is at 5.
May 14 13:36:52 wikiSrv systemd[1]: Stopped Wiki.js.
May 14 13:36:52 wikiSrv systemd[1]: wikijs.service: Start request repeated too quickly.
May 14 13:36:52 wikiSrv systemd[1]: wikijs.service: Failed with result 'exit-code'.
May 14 13:36:52 wikiSrv systemd[1]: Failed to start Wiki.js.
```




![image](https://github.com/user-attachments/assets/b380a06d-0d6a-4a74-af4c-62d3c08cae7a)




```ssh
cd /var/www/wikijs
nano config.yml
commenter les deux lignes
  # Optional - PostgreSQL only:
#  schema: public
```


```ssh
sudo systemctl daemon-reload
sudo systemctl start wikijs
sudo systemctl enable wikijs
systemctl status wikijs
```

### pb3
![image](https://github.com/user-attachments/assets/ea61e773-5d69-4438-80a4-c16845b3571f)

```ssh
May 14 13:38:54 wikiSrv node[6066]: bad indentation of a mapping entry at line 50, column 3:
May 14 13:38:54 wikiSrv node[6066]:       storage: path/to/database.sqlite
May 14 13:38:54 wikiSrv node[6066]:       ^
May 14 13:38:54 wikiSrv node[6066]: >>> Unable to read configuration file! Did you create the config.yml file?
May 14 13:38:54 wikiSrv systemd[1]: wikijs.service: Main process exited, code=exited, status=1/FAILURE
May 14 13:38:54 wikiSrv systemd[1]: wikijs.service: Failed with result 'exit-code'.
May 14 13:38:54 wikiSrv systemd[1]: wikijs.service: Scheduled restart job, restart counter is at 5.
May 14 13:38:54 wikiSrv systemd[1]: Stopped Wiki.js.
May 14 13:38:54 wikiSrv systemd[1]: wikijs.service: Start request repeated too quickly.
May 14 13:38:54 wikiSrv systemd[1]: wikijs.service: Failed with result 'exit-code'
```




![image](https://github.com/user-attachments/assets/940fdb35-3d60-4dbc-841f-2edbbb0a16aa)




```ssh
cd /var/www/wikijs
nano config.yml
commenter les deux lignes
  # SQLite only:
#  storage: path/to/database.sqlite
```

```ssh
sudo systemctl daemon-reload
sudo systemctl start wikijs
sudo systemctl enable wikijs
systemctl status wikijs
```


## Accès à Wiki.js

Accédez à Wiki.js via votre navigateur en utilisant l'URL : `http://<adresse_IP>:3000`. Suivez les instructions pour créer un compte administrateur et terminer l'installation.


## pb4

- Accès à azure http://<VOTRE-IP>:3000/
  
1. Il faut ajouter une règle de traffic entrant :

![image](https://github.com/user-attachments/assets/401656ca-2543-4798-a8fb-9fff35b761d4)

2. J'ai changé ici le paramètre bindIP: 0.0.0.0 à la place de bindIP: 127.0.0.1

wikijs@wikiSrv:/var/www/wikijs$ nano config.yml

![image](https://github.com/user-attachments/assets/e5b6ca2c-d2b0-440e-bb31-f6bcc8a39fc9)


```ssh
sudo systemctl daemon-reload
sudo systemctl start wikijs
sudo systemctl enable wikijs
systemctl status wikijs
```

et surtout surtout :

```ssh
sudo systemctl restart wikijs
```


 ## Félicitations ! ==> Accès à Wiki.js

Accédez à Wiki.js via votre navigateur en utilisant l'URL : `http://<adresse_IP>:3000`. Suivez les instructions pour créer un compte administrateur et terminer l'installation.



# HTTP ==> HTTPS

```ssh
ssh -i key1.pem azureuser@<adresse_ip_publique_de_votre_vm>
sudo -i
sudo apt update
sudo apt install certbot python3-certbot-apache
sudo ufw allow 'Apache Full'
sudo certbot --apache # Entrez votre nom de domaine lorsqu'il vous sera demandé comme ceci: haythemrehouma.eastus.cloudapp.azure.com
sudo systemctl status certbot.timer
sudo certbot renew --dry-run
```
