# <h1 id="wikijs-postgres">Installer Wiki.js sur Ubuntu 22.04 LTS avec PostgreSQL</h1>

# <h2 id="prerequis">Prérequis</h2>

* Ubuntu 22.04 LTS ([télécharger l’ISO](https://releases.ubuntu.com/22.04/ubuntu-22.04-live-server-amd64.iso))
* Accès sudo
* Connexion Internet

---

## <h2 id="git">1. Installation de Git</h2>

```bash
sudo apt update
sudo apt install git -y
git --version
```

---

## <h2 id="nodejs">2. Installation de Node.js (LTS)</h2>

```bash
curl -fsSL https://deb.nodesource.com/setup_lts.x | sudo -E bash -
sudo apt install -y nodejs
node -v && npm -v
```

---

## <h2 id="postgres">3. Installation et configuration de PostgreSQL</h2>

```bash
sudo apt install postgresql postgresql-contrib -y
sudo systemctl start postgresql
sudo systemctl enable postgresql
```

### Création de la base de données et de l’utilisateur :

```bash
sudo -u postgres psql
```

Dans l’invite PostgreSQL :

```sql
CREATE DATABASE wikidb;
CREATE USER wikidb_user WITH PASSWORD 'StrongPassword123!';
GRANT ALL PRIVILEGES ON DATABASE wikidb TO wikidb_user;
\q
```

---

## <h2 id="wikijs-user">4. Création d’un utilisateur système pour Wiki.js</h2>

```bash
sudo adduser --system --group wikijs
sudo mkdir -p /var/www/wikijs
sudo chown -R wikijs:wikijs /var/www/wikijs
sudo usermod -s /bin/bash wikijs
```

---

## <h2 id="install-wikijs">5. Installation de Wiki.js</h2>

```bash
sudo su - wikijs
cd /var/www/wikijs
wget https://github.com/Requarks/wiki/releases/latest/download/wiki-js.tar.gz
tar xzf wiki-js.tar.gz
rm wiki-js.tar.gz
cp config.sample.yml config.yml
exit
```

---

## <h2 id="config-postgres">6. Configuration de Wiki.js pour PostgreSQL</h2>

Éditez `/var/www/wikijs/config.yml` :

```bash
sudo nano /var/www/wikijs/config.yml
```

### Remplacez la section `db:` par :

```yaml
db:
  type: postgres
  host: localhost
  port: 5432
  user: wikidb_user
  pass: StrongPassword123!
  db: wikidb
  ssl: false
```

Assurez-vous que les sections suivantes soient **commentées ou supprimées** si inutilisées :

```yaml
#  schema: public      # COMMENTEZ si non utilisé
#  storage: path/to/database.sqlite  # COMMENTEZ si non utilisé
#  sslOptions:         # COMMENTEZ si inutilisé
#    auto: true
#    rejectUnauthorized: false
```

Aussi, pour que Wiki.js soit accessible depuis l’extérieur :

```yaml
bindIP: 0.0.0.0
port: 3000
```

---

## <h2 id="systemd">7. Création du service systemd</h2>

### Ajoutez aux sudoers et définissez un mot de passe pour wikijs :

```bash
sudo passwd wikijs
sudo usermod -aG sudo wikijs
```

### Créez le service :

```bash
sudo nano /etc/systemd/system/wikijs.service
```

Contenu :

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

---

## <h2 id="demarrage">8. Démarrage et vérification</h2>

```bash
sudo systemctl daemon-reload
sudo systemctl start wikijs
sudo systemctl enable wikijs
systemctl status wikijs
```

---

## <h2 id="troubleshooting">9. Dépannage courant</h2>

Si vous voyez des erreurs de type :

* `bad indentation of a mapping entry`
* `Unable to read configuration file!`

Revérifiez que vous avez bien commenté ou supprimé les blocs suivants dans `config.yml` :

```yaml
#  sslOptions:
#    auto: true

#  schema: public

#  storage: path/to/database.sqlite
```

Puis redémarrez :

```bash
sudo systemctl restart wikijs
```

---

## <h2 id="acces">10. Accès à l’interface Wiki.js</h2>

Dans votre navigateur :
`http://<IP_de_votre_serveur>:3000`

---

## <h2 id="https">11. Passage en HTTPS (optionnel)</h2>

```bash
sudo apt update
sudo apt install certbot python3-certbot-nginx -y
sudo ufw allow 'Nginx Full'
sudo certbot --nginx
sudo certbot renew --dry-run
```



# <h1 id="fin">Félicitations !</h1>

Wiki.js est maintenant opérationnel avec PostgreSQL sur Ubuntu 22.04.

