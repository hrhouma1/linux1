# **Travail Pratique – Installation et Configuration de Wiki.js sur Ubuntu 22.04**

# **1. Objectif**

Ce travail pratique vise à évaluer votre capacité à :

* Mettre en place un environnement Linux fonctionnel dans une machine virtuelle,
* Installer des logiciels essentiels (Git, Node.js, MariaDB),
* Configurer une base de données,
* Installer et configurer l'application Wiki.js,
* Automatiser le démarrage du service avec systemd.



# **2. Instructions Générales**

* Réalisez toutes les étapes sur une machine virtuelle Ubuntu Server 22.04.
* Documentez chaque commande utilisée.
* Commentez chaque étape (à quoi elle sert, comment on vérifie son bon fonctionnement).
* À la fin, regroupez votre documentation dans un **rapport PDF ou Markdown**.



## **Partie 2.1 – Mise en place de la machine virtuelle**

**Téléchargez et démarrez une VM Ubuntu 22.04.**
Indiquez :

* Le logiciel utilisé (VirtualBox, VMware, autre),
* La configuration (RAM, CPU, disque),
* Le nom d’utilisateur créé.


## **Partie 2.2 – Préparation de l'Environnement (20 points)**

### **2.2.1 Installation de Git (5 points)**

* Commande(s) utilisée(s) :

```bash
sudo apt update
sudo apt install git
```

* Vérification :

```bash
git --version
```

* Expliquez à quoi sert Git et comment valider que l'installation est réussie.



### **2.2.2 Installation de Node.js LTS (5 points)**

* Utilisez NodeSource :

```bash
curl -fsSL https://deb.nodesource.com/setup_lts.x | sudo -E bash -
sudo apt install -y nodejs
```

* Vérifiez les versions :

```bash
node -v
npm -v
```

* Expliquez pourquoi utiliser la version LTS et ce qu’est `npm`.



### **2.2.3 Installation de MariaDB (10 points)**

* Commandes utilisées :

```bash
sudo apt install mariadb-server
sudo systemctl start mariadb
sudo systemctl enable mariadb
sudo mysql_secure_installation
```

* Vérification :

```bash
sudo systemctl status mariadb
sudo mysql -u root -p
```

* Expliquez ce que fait `mysql_secure_installation` et comment tester la connexion.



## **Partie 2.3 – Configuration de la Base de Données (10 points)**

### **2.3.1 Création de la base Wiki.js**

* Commandes SQL à fournir :

```sql
CREATE DATABASE wikijs;
CREATE USER 'wikiuser'@'localhost' IDENTIFIED BY 'motdepassefort';
GRANT ALL PRIVILEGES ON wikijs.* TO 'wikiuser'@'localhost';
FLUSH PRIVILEGES;
```

* Expliquez chaque ligne.



## **Partie 2.4 – Installation de Wiki.js (30 points)**

### **2.4.1 Téléchargement et préparation (10 points)**

* Créez un utilisateur :

```bash
sudo adduser wikijs
```

* Placez-vous dans le répertoire :

```bash
sudo mkdir -p /opt/wikijs
sudo chown wikijs:wikijs /opt/wikijs
```

* Téléchargement :

```bash
cd /opt/wikijs
sudo -u wikijs wget https://github.com/Requarks/wiki/releases/latest/download/wiki-js.tar.gz
sudo -u wikijs tar xzf wiki-js.tar.gz
```

* Installez les dépendances :

```bash
cd /opt/wikijs
sudo -u wikijs npm install
```



### **2.4.2 Configuration du fichier `config.yml` (10 points)**

* Modifiez :

```yaml
db:
  type: mariadb
  host: localhost
  port: 3306
  user: wikiuser
  pass: motdepassefort
  db: wikijs
```

* Expliquez chaque paramètre modifié.



### **2.4.3 Lancement et test (10 points)**

* Démarrage :

```bash
sudo -u wikijs node server
```

* Vérifiez l’accès via navigateur à `http://<IP_VM>:3000`.

* Capturez une capture d’écran de l’interface Wiki.js.



## **Partie 2.5 – Automatisation et Sécurité (10 points)**

### **2.5.1 Création d’un service systemd**

* Créez `/etc/systemd/system/wikijs.service` :

```ini
[Unit]
Description=Wiki.js Service
After=network.target mariadb.service

[Service]
Type=simple
User=wikijs
WorkingDirectory=/opt/wikijs
ExecStart=/usr/bin/node server
Restart=always

[Install]
WantedBy=multi-user.target
```

* Activez le service :

```bash
sudo systemctl daemon-reexec
sudo systemctl enable wikijs
sudo systemctl start wikijs
```

* Vérifiez :

```bash
sudo systemctl status wikijs
```

* Créez un compte admin sur l’interface web.


# **Remise du travail**

Votre rapport doit contenir :

* Toutes les commandes utilisées,
* Les explications à chaque étape,
* Les vérifications de fonctionnement,
* Les captures d'écran principales (Git, Node, MariaDB, Wiki.js Web),
* Les éventuelles erreurs rencontrées et leur résolution.

