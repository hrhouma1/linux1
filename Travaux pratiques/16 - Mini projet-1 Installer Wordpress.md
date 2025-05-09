---
# Partie 1 - Installation de WordPress et GLPI sur un Serveur Linux (Ubuntu 22.04)
----

Ce guide fournit les instructions et les scripts shell pour installer WordPress et GLPI sur un serveur Linux Ubuntu 22.04. Les deux applications utiliseront des ports et des utilisateurs distincts pour éviter tout conflit.

## Prérequis
- Un serveur Linux (Ubuntu 22.04)
- Un serveur web (Apache)
- PHP et ses extensions
- Deux bases de données distinctes (MariaDB ou MySQL)



# Partie 1 : Installation de WordPress

### 1. Créez et exécutez le script shell pour WordPress

Enregistrez le script suivant dans un fichier, par exemple `install_wordpress.sh` :

```bash
nano install_wordpress.sh
```

### 2. Copier/coller le contenu de ce script, puis faites `CTRL + X`, tapez `Y` :

```bash
#!/bin/bash

# Mise à jour des paquets et installation des composants nécessaires
sudo apt update
sudo apt install -y apache2 php php-{mysql,xml,mbstring,curl,zip,gd} libapache2-mod-php mariadb-server wget

# Sécurisation de l'installation de MariaDB
sudo mysql_secure_installation

# Connexion à MariaDB et création de la base de données WordPress et de l'utilisateur
sudo mysql -u root <<EOF
CREATE DATABASE wordpressdb;
CREATE USER 'wordpressuser'@'localhost' IDENTIFIED BY 'securepassword1';
GRANT ALL PRIVILEGES ON wordpressdb.* TO 'wordpressuser'@'localhost';
FLUSH PRIVILEGES;
EXIT;
EOF

# Téléchargement et extraction de WordPress
cd /tmp
wget https://wordpress.org/latest.tar.gz
sudo tar xzvf latest.tar.gz -C /var/www/html/

# Configuration des permissions
sudo chown -R www-data:www-data /var/www/html/wordpress
sudo chmod -R 775 /var/www/html/wordpress

# Création du fichier de configuration pour WordPress dans Apache
sudo bash -c 'cat <<EOT > /etc/apache2/sites-available/wordpress.conf
<VirtualHost *:8080>
    ServerName localhost
    DocumentRoot /var/www/html/wordpress

    <Directory /var/www/html/wordpress>
        Options FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>

    ErrorLog \${APACHE_LOG_DIR}/wordpress_error.log
    CustomLog \${APACHE_LOG_DIR}/wordpress_access.log combined
</VirtualHost>
EOT'

# Activation de la configuration et redémarrage d'Apache
sudo a2ensite wordpress.conf
sudo a2enmod rewrite
sudo systemctl restart apache2

# Instructions pour l'installation via l'interface web
echo "Installation via l'interface web :"
echo "Ouvrez un navigateur et accédez à http://127.0.0.1:8080/wordpress"
echo "Suivez l'assistant d'installation en fournissant les informations de la base de données :"
echo "Nom de la base de données : wordpressdb"
echo "Nom d'utilisateur : wordpressuser"
echo "Mot de passe : securepassword1"
echo "Hôte de la base de données : localhost"
```

Rendez le script exécutable et exécutez-le :

```bash
chmod +x install_wordpress.sh
./install_wordpress.sh
```

### 3. Réponses aux configurations demandées lors de l'exécution de `./install_wordpress.sh`

Pendant l'exécution du script, vous serez invité à répondre à certaines questions pour sécuriser l'installation de MariaDB. Voici les réponses recommandées :

```bash
Enter current password for root : [Appuyez sur Entrée]
Switch to unix_socket authentication [Y/n] Y
Change the root password? [Y/n] n
Remove anonymous users? [Y/n] Y
Disallow root login remotely? [Y/n] n
Remove test database and access to it? [Y/n] Y
Reload privilege tables now? [Y/n] Y
```


### 4. Installation via l'interface web

1. Ouvrez un navigateur et accédez à [http://127.0.0.1:8080/wordpress](http://127.0.0.1:8080/wordpress)
2. Suivez l'assistant d'installation en fournissant les informations de la base de données :
   - Nom de la base de données : wordpressdb
   - Nom d'utilisateur : wordpressuser
   - Mot de passe : securepassword1
   - Hôte de la base de données : localhost

### 5. Troubleshooting pour le 8080

1. **Créer un fichier de configuration pour le port 8080**:

   Vous pouvez créer un fichier de configuration spécifique dans le dossier `/etc/apache2/conf-available/` pour ajouter le port 8080.

   ```bash
   sudo bash -c 'echo "Listen 8080" > /etc/apache2/conf-available/port8080.conf'
   ```

2. **Activer cette configuration**:

   Activez ensuite cette configuration avec la commande suivante :

   ```bash
   sudo a2enconf port8080
   ```

3. **Redémarrer Apache**:

   Enfin, redémarrez Apache pour appliquer les modifications :

   ```bash
   sudo systemctl restart apache2
   ```


# Pour résumer :

  ```bash

# Méthode 1 : Utilisation d'un fichier de configuration séparé
sudo bash -c 'echo "Listen 8080" > /etc/apache2/conf-available/port8080.conf'
sudo a2enconf port8080
sudo systemctl restart apache2
 ```

## ou 

  ```bash
# Méthode 2 : Ajout direct dans le fichier ports.conf
echo "Listen 8081" | sudo tee -a /etc/apache2/ports.conf
sudo systemctl restart apache2

   ```

### 5. Finalisation

1. Une fois l'installation terminée, connectez-vous à votre tableau de bord WordPress avec les identifiants que vous avez créés.
2. Changez immédiatement le mot de passe administrateur si nécessaire.
3. Configurez votre site WordPress selon vos besoins.



<br/>
<br/>

# Partie 2 - Installation de GLPI sur un Serveur Linux (Ubuntu 22.04) 
----

### 1. Créez et exécutez le script shell pour GLPI

Enregistrez le script suivant dans un fichier, par exemple `install_glpi.sh` :

```bash
nano install_glpi.sh
```

### 2. Copier/coller le contenu de ce script, puis faites `CTRL + X`, tapez `Y` :

```bash
#!/bin/bash

# Mise à jour des paquets et installation des composants nécessaires
sudo apt update
sudo apt install -y apache2 php php-{apcu,cli,common,curl,gd,imap,ldap,mysql,xmlrpc,xml,mbstring,bcmath,intl,zip,bz2} libapache2-mod-php mariadb-server wget

# Sécurisation de l'installation de MariaDB
sudo mysql_secure_installation

# Connexion à MariaDB et création de la base de données GLPI et de l'utilisateur
sudo mysql -u root <<EOF
CREATE DATABASE glpidb;
CREATE USER 'glpiuser'@'localhost' IDENTIFIED BY 'securepassword2';
GRANT ALL PRIVILEGES ON glpidb.* TO 'glpiuser'@'localhost';
FLUSH PRIVILEGES;
EXIT;
EOF

# Téléchargement et extraction de GLPI
cd /tmp
wget https://github.com/glpi-project/glpi/releases/download/10.0.7/glpi-10.0.7.tgz
sudo tar xvf glpi-10.0.7.tgz -C /var/www/html/

# Configuration des permissions
sudo chown -R www-data:www-data /var/www/html/glpi
sudo chmod -R 775 /var/www/html/glpi

# Création du fichier de configuration pour GLPI dans Apache
sudo bash -c 'cat <<EOT > /etc/apache2/sites-available/glpi.conf
<VirtualHost *:8081>
    ServerName localhost
    DocumentRoot /var/www/html/glpi/public

    <Directory /var/www/html/glpi/public>
        Options FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>

    ErrorLog \${APACHE_LOG_DIR}/glpi_error.log
    CustomLog \${APACHE_LOG_DIR}/glpi_access.log combined
</VirtualHost>
EOT'

# Activation de la configuration et redémarrage d'Apache
sudo a2ensite glpi.conf
sudo a2enmod rewrite
sudo systemctl restart apache2

# Instructions pour l'installation via l'interface web
echo "Installation via l'interface web :"
echo "Ouvrez un navigateur et accédez à http://127.0.0.1:8081/glpi"
echo "Suivez l'assistant d'installation en fournissant les informations de la base de données :"
echo "Nom de la base de données : glpidb"
echo "Nom d'utilisateur : glpiuser"
echo "Mot de passe : securepassword2"
echo "Hôte de la base de données : localhost"
```

Rendez le script exécutable et exécutez-le :

```bash
chmod +x install_glpi.sh
./install_glpi.sh
```

### 3. Réponses aux configurations demandées lors de l'exécution de `./install_glpi.sh`

Pendant l'exécution du script, vous serez invité à répondre à certaines questions pour sécuriser l'installation de MariaDB. Voici les réponses recommandées :

```bash
Enter current password for root : [Appuyez sur Entrée]
Switch to unix_socket authentication [Y/n] Y
Change the root password? [Y/n] n
Remove anonymous users? [Y/n] Y
Disallow root login remotely? [Y/n] Y
Remove test database and access to it? [Y/n] Y
Reload privilege tables now? [Y/n] Y
```

### 4. Installation via l'interface web

1. Ouvrez un navigateur et accédez à [http://127.0.0.1:8081/glpi](http://127.0.0.1:8081/glpi)
2. Suivez l'assistant d'installation en fournissant les informations de la base de données :
   - Nom de la base de données : glpidb
   - Nom d'utilisateur : glpiuser
   - Mot de passe : securepassword2
   - Hôte de la base de données : localhost

### 5. Finalisation

1. Une fois l'installation terminée, connectez-vous avec les identifiants par défaut (glpi/glpi pour l'administrateur).
2. Changez immédiatement les mots de passe par défaut.
3. Supprimez le fichier `install.php` pour des raisons de sécurité :

   ```bash
   sudo rm /var/www/html/glpi/install/install.php
   ```

4. Vérifiez que votre installation fonctionne correctement en accédant à l'interface d'administration de GLPI via le navigateur.

---

## Conclusion

Vous avez maintenant installé WordPress et GLPI sur un serveur Ubuntu 22.04, chacun fonctionnant sur un port différent (WordPress sur `8080` et GLPI sur `8081`) et utilisant des utilisateurs et bases de données distincts. Ces instructions incluent toutes les réponses nécessaires aux questions posées lors de l'installation, pour guider vos étudiants débutants pas à pas. N'oubliez pas de configurer les sauvegardes régulières et de maintenir le système à jour.

---

## Questions et Instructions

1. **Suivez les Étapes :** Lisez attentivement les étapes suivantes et exécutez-les dans l'ordre pour installer WordPress et GLPI sur un serveur Ubuntu 22.04.

2. **Exécutez les Scripts :** Vous devez créer et exécuter les scripts shell fournis pour chaque application. Ces scripts automatisent le processus d'installation et de configuration.

3. **Captures d'Écran :** À chaque étape clé de l'installation (par exemple, après l'exécution de chaque script, la configuration de la base de données, et l'accès à l'interface web), prenez une capture d'écran pour prouver que l'installation a été effectuée correctement.

4. **Ajoutez des Commentaires :** Lorsque vous copiez les scripts dans les fichiers, ajoutez des commentaires (`#`) pour expliquer chaque ligne de code. Cela démontre votre compréhension de ce que fait chaque commande.

5. **Utilisez des Commandes Avancées :** Intégrez les commandes Linux avancées fournies ci-dessous pour renforcer votre installation et gestion du serveur :
   - **Gestion des fichiers et permissions :**
     - Utilisez `find /var/www/html -type f -exec chmod 644 {} \;` pour définir les permissions de fichiers de manière récursive.
     - Employez `setfacl` pour gérer les listes de contrôle d'accès et restreindre l'accès à des fichiers spécifiques.
   - **Sécurisation :**
     - Configurez `ufw` pour autoriser uniquement les ports nécessaires : `sudo ufw allow 8080/tcp` et `sudo ufw allow 8081/tcp`.
     - Mettez en place une surveillance de l’activité en temps réel avec `sudo tail -f /var/log/apache2/access.log`.
   - **Surveillance du système :**
     - Utilisez `htop` pour surveiller les processus en cours et vérifier la charge système lors de l’installation.
     - Mettez en place un monitoring en temps réel des ressources utilisées par MySQL avec `mysqladmin -u root -p status`.

6. **Questions de Réflexion :** Après avoir terminé les installations, répondez aux questions suivantes dans un document séparé :
   - Quelle est l'importance de configurer des utilisateurs et des bases de données distincts pour WordPress et GLPI ?
   - Que se passerait-il si WordPress et GLPI utilisaient tous les deux le port 80 ? Expliquez les implications.
   - Pourquoi est-il nécessaire de sécuriser l'installation de MariaDB pendant la configuration initiale ?
   - Quel lien existe-t-il entre la base de données et les fichiers de WordPress/GLPI téléchargés sur le serveur ?
   - Expliquez ce qui se passe lorsque vous accédez à WordPress ou GLPI via un navigateur après l'installation.

7. **Exercice Supplémentaire :** Changez les ports utilisés pour WordPress et GLPI de `8080` et `8081` à `8080` et `8082`. Après avoir effectué ce changement, expliquez les étapes que vous avez suivies et ce que cela implique pour l'accès aux deux applications.

---

## Partie 1 : Installation de WordPress

### Étape 1 : Créez et exécutez le script shell pour WordPress

1. Ouvrez un terminal et créez un nouveau fichier de script nommé `install_wordpress.sh`.
2. Copiez le script fourni dans ce fichier.
3. Ajoutez des commentaires expliquant chaque commande dans le script.

### Étape 2 : Exécutez le script `install_wordpress.sh`

1. Rendez le script exécutable avec la commande `chmod +x install_wordpress.sh`.
2. Exécutez le script avec la commande `./install_wordpress.sh`.
3. **Utilisez `htop` :** Exécutez `htop` dans une autre session pour surveiller les processus système pendant l’installation.
4. **Sécurisez les accès :** Configurez votre pare-feu avec `ufw` pour autoriser les connexions au port 8080.
5. Prenez des captures d'écran à chaque étape clé pour prouver l'exécution correcte.

### Étape 3 : Configurez WordPress via l'Interface Web

1. Accédez à `http://127.0.0.1:8080/wordpress` dans votre navigateur.
2. Suivez l'assistant d'installation pour configurer la base de données.
3. **Surveillez les logs :** Utilisez `sudo tail -f /var/log/apache2/access.log` pour surveiller les accès pendant la configuration.
4. Prenez une capture d'écran de la page de configuration finale.

---

## Partie 2 : Installation de GLPI

### Étape 1 : Créez et exécutez le script shell pour GLPI

1. Créez un nouveau fichier de script nommé `install_glpi.sh`.
2. Copiez le script fourni dans ce fichier.
3. Ajoutez des commentaires expliquant chaque commande dans le script.

### Étape 2 : Exécutez le script `install_glpi.sh`

1. Rendez le script exécutable avec la commande `chmod +x install_glpi.sh`.
2. Exécutez le script avec la commande `./install_glpi.sh`.
3. **Vérifiez les permissions :** Après l'installation, utilisez `find /var/www/html/glpi -type f -exec chmod 644 {} \;` pour configurer les permissions correctes.
4. **Configurer `ufw` :** Autorisez le port 8081 pour l'accès à GLPI.
5. **Surveillez l’activité MySQL :** Utilisez `mysqladmin -u root -p status` pour vérifier l'état de MySQL pendant l'installation.
6. Prenez des captures d'écran à chaque étape clé pour prouver l'exécution correcte.

### Étape 3 : Configurez GLPI via l'Interface Web

1. Accédez à `http://127.0.0.1:8081/glpi` dans votre navigateur.
2. Suivez l'assistant d'installation pour configurer la base de données.
3. **Surveillez les logs :** Utilisez `sudo tail -f /var/log/apache2/access.log` pour surveiller les accès pendant la configuration.
4. Prenez une capture d'écran de la page de configuration finale.

---

## Exercice Supplémentaire

1. **Changez les Ports :** Modifiez les fichiers de configuration Apache pour que WordPress et GLPI utilisent les ports `8080` et `8082`.
2. **Expliquez les Changements :** Documentez les étapes que vous avez suivies pour effectuer ce changement et discutez des implications de cette modification.
3. **Vérification :** Après le changement, vérifiez l'état des services avec `sudo systemctl status apache2` et assurez-vous que les applications fonctionnent correctement sur les nouveaux ports.

---

En suivant ces instructions, vous démontrerez non seulement votre capacité à installer et configurer WordPress et GLPI, mais aussi votre maîtrise des commandes avancées sous Linux, essentielles pour gérer un serveur de manière efficace et sécurisée.

- [Documentation WordPress](https://wordpress.org/support/article/how-to-install-wordpress/)
- [GLPI Documentation](https://glpi-install.readthedocs.io/en/latest/)
- [Apache Configuration](https://httpd.apache.org/docs/current/mod/mod_alias.html)
- [MariaDB Documentation](https://mariadb.com/kb/en/documentation/)





<br/>
<br/>

# Annexe 01: changer au port 8080




# Pour résumer :

# Méthode 1 : Utilisation d'un fichier de configuration séparé


```bash
sudo bash -c 'echo "Listen 8080" > /etc/apache2/conf-available/port8080.conf'
sudo a2enconf port8080
sudo systemctl restart apache2

```

## ou 

# Méthode 2 : Ajout direct dans le fichier ports.conf

```bash
echo "Listen 8081" | sudo tee -a /etc/apache2/ports.conf
sudo systemctl restart apache2
```

### Explications des corrections :

1. Dans la méthode 2, j'ai supprimé la ligne `sudo a2enconf port8080` car elle n'est pas nécessaire lorsque vous modifiez directement le fichier `ports.conf`. Cette commande était redondante pour la méthode d'ajout direct dans `ports.conf`.

2. Les deux méthodes sont maintenant correctes et montrent les deux approches possibles pour configurer Apache pour écouter sur un port spécifique.


<br/>
<br/>

# Annexe 02: La différence principale entre tee et sudo bash :

----

1. **`echo "Listen 8081" | sudo tee -a /etc/apache2/ports.conf`** :
   - Cette commande ajoute la ligne `Listen 8081` directement à la fin du fichier `ports.conf`.
   - Elle modifie le fichier principal de configuration des ports d'Apache (`ports.conf`), ce qui peut être moins flexible si vous avez besoin de gérer plusieurs configurations ou si vous voulez garder ce fichier intact pour faciliter les mises à jour ou le dépannage.
   - Cette méthode est simple et rapide, mais elle implique de modifier un fichier de configuration central, ce qui peut ne pas être idéal dans certains contextes où l'intégrité des fichiers de configuration par défaut doit être préservée.

2. **Créer un fichier de configuration spécifique avec `sudo bash -c 'echo "Listen 8080" > /etc/apache2/conf-available/port8080.conf'`** :
   - Cette méthode crée un nouveau fichier de configuration dans le répertoire `conf-available`, qui est dédié aux configurations supplémentaires.
   - Elle est plus modulaire et permet de garder les configurations spécifiques séparées du fichier principal `ports.conf`.
   - Cette approche est préférée si vous souhaitez gérer les configurations de manière plus structurée, car vous pouvez activer ou désactiver cette configuration spécifique sans toucher aux fichiers de configuration par défaut. Cela facilite également la maintenance et le débogage.

**Résumé**:
- **`tee` sur `ports.conf`** : Modifie directement le fichier de configuration principal, simple mais moins modulaire.
- **Fichier de configuration séparé** : Plus modulaire, facilite la gestion des configurations spécifiques sans altérer les fichiers principaux.
