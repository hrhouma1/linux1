# **Cours Linux : Gestion des Utilisateurs, Permissions et Configuration**



[1. Introduction à Linux](#1-introduction-à-linux)



- Présentation générale de Linux
- Avantages d'utiliser Linux pour l'administration système
- Concepts de base : noyau, shell, commandes



[2. Système de Fichiers Linux](#2-système-de-fichiers-linux)



- Arborescence des répertoires Linux
  - `/home`, `/etc`, `/var`, `/tmp`, `/bin`, `/usr`, etc.
- Exploration et navigation dans le système de fichiers
  - Commandes : `ls`, `cd`, `pwd`, `find`, `du`, `df`
- Bonnes pratiques de gestion des répertoires



[3. Gestion des Utilisateurs et des Groupes](#3-gestion-des-utilisateurs-et-des-groupes)


- Création et gestion des utilisateurs
  - `useradd`, `adduser`, `usermod`, `deluser`, `userdel`
- Gestion des groupes
  - `groupadd`, `groupdel`, `usermod -aG`
- Vérification des utilisateurs existants
  - Fichier `/etc/passwd`
- Gestion des mots de passe
  - `passwd`, politique de mots de passe sécurisée
- Les fichiers de configuration utilisateur
  - `.bashrc`, `.profile`, `.bash_profile`, `/etc/profile`, `/etc/bash.bashrc`
  - Utilité, différences et configuration
- Cas pratiques



[4. Gestion des Permissions et de la Sécurité](#4-gestion-des-permissions-et-de-la-sécurite)


- Système de permissions sous Linux
  - Propriétaire, groupe, autres utilisateurs
  - Lecture, écriture, exécution
- Commandes de gestion des permissions
  - `chmod`, `chown`, `chgrp`
- Les permissions numériques et symboliques
  - Explications et exemples pratiques
- Permissions avancées
  - Sticky bit, Setuid, Setgid
- Cas pratiques et bonnes pratiques de sécurité



[5. Les Fichiers de Configuration Shell](#5-les-fichiers-de-configuration-shell)


- Utilité et rôle des fichiers de configuration
  - `.bashrc` : initialisation de l'environnement utilisateur
  - `.profile` : configuration d'environnement pour les sessions interactives
  - `.bash_profile` : script d'initialisation de session
  - `/etc/profile` : paramètres globaux pour tous les utilisateurs
- Variables d'environnement et alias
  - Comment les définir et les utiliser
- Bonnes pratiques pour la gestion des fichiers de configuration


[6. Sécurité des Systèmes Linux](#6-sécurité-des-systèmes-linux)




- Introduction aux scripts Bash
- Syntaxe de base : variables, conditions, boucles
- Exemples de scripts pour la gestion des utilisateurs et des permissions
- Sécurité des scripts



[7. Automatisation et Scripts Bash](#6-automatisation-et-scripts-bash)


- Bonnes pratiques pour sécuriser les fichiers critiques
- Gestion des droits d'administration (`sudo`)
- Stratégies de gestion des accès
- Gestion des journaux d'audit
  - `journalctl`, `/var/log`



[8. Exercices Pratiques et Quiz](#8-exercices-pratiques-et-quiz)


- Exercices guidés pour chaque chapitre
- Quiz pour évaluer les connaissances acquises
- Études de cas concrets



[9. Conclusion](#9-conclusion)

- Récapitulatif des concepts abordés
- Conseils pour l'administration avancée



----
# 1. Introduction à Linux
---

## 1.1. **Présentation générale de Linux**

Linux est un système d'exploitation open source qui est utilisé pour gérer et contrôler les ressources matérielles d'un ordinateur, tout en offrant une interface pour exécuter des applications. Créé initialement par Linus Torvalds en 1991, il est basé sur le noyau Unix et est distribué sous la licence GNU General Public License (GPL). Aujourd'hui, Linux est au cœur de millions de serveurs, postes de travail, dispositifs embarqués et supercalculateurs à travers le monde.

L'une des principales raisons de la popularité de Linux est sa flexibilité et sa robustesse. Contrairement aux systèmes d'exploitation propriétaires, Linux offre un contrôle total sur le système, ce qui en fait le choix privilégié pour les administrateurs systèmes, les développeurs et les passionnés de technologie.



## 1.2.**Avantages d'utiliser Linux pour l'administration système**

L'utilisation de Linux pour l'administration système présente de nombreux avantages, notamment :

### **1. Stabilité et Fiabilité**
- Les systèmes Linux sont connus pour leur stabilité. Ils peuvent fonctionner sans interruption pendant des années sans nécessiter de redémarrage.
- Les serveurs Linux sont souvent utilisés dans des environnements critiques en raison de leur robustesse et de leur capacité à gérer des charges de travail élevées.

### **2. Sécurité Avancée**
- La sécurité est au cœur de l'architecture de Linux. Les utilisateurs bénéficient de mécanismes de contrôle d'accès avancés tels que les permissions de fichiers et les listes de contrôle d'accès (ACL).
- La communauté open source surveille constamment les vulnérabilités et publie rapidement des correctifs.

### **3. Liberté et Personnalisation**
- Linux permet une personnalisation complète, que ce soit au niveau du noyau, des environnements de bureau ou des services système.
- Les distributions Linux varient en fonction des besoins spécifiques, offrant des options légères ou complètes.

### **4. Coût Réduit**
- La majorité des distributions Linux sont gratuites et open source. Cela réduit considérablement les coûts d'exploitation par rapport aux systèmes propriétaires.

### **5. Communauté Active**
- Linux est soutenu par une communauté dynamique et réactive, prête à aider et à proposer des solutions aux problèmes rencontrés.
- Les forums, wikis et documentations abondent, permettant aux utilisateurs de trouver des réponses rapidement.



## 1.3.**Concepts de base : noyau, shell, commandes**

Pour comprendre Linux, il est essentiel de connaître certains concepts fondamentaux :

### **1. Noyau (Kernel)**
Le noyau est le cœur du système d'exploitation Linux. Il agit comme un intermédiaire entre le matériel de l'ordinateur et les applications. Ses fonctions principales incluent :
- La gestion des processus.
- La gestion de la mémoire.
- La gestion des systèmes de fichiers.
- La gestion des périphériques.

Le noyau Linux est modulaire, ce qui permet de charger ou de décharger des modules dynamiquement pour ajouter des fonctionnalités sans redémarrer le système.

### **2. Shell**
Le shell est l'interface utilisateur qui permet d'interagir avec le noyau en utilisant des commandes. Il existe plusieurs types de shells sous Linux, notamment :
- **Bash (Bourne Again SHell)** : Le plus répandu et souvent utilisé par défaut.
- **Zsh (Z Shell)** : Offre des fonctionnalités avancées, telles que l'autocomplétion intelligente.
- **Fish (Friendly Interactive SHell)** : Connu pour sa simplicité et son interactivité.

Les utilisateurs saisissent des commandes dans le shell pour exécuter des tâches, telles que la gestion de fichiers ou l'administration système.

### **3. Commandes de Base**
Les commandes sont le principal moyen de communication avec le système via le shell. Voici quelques commandes essentielles :
- `ls` : Liste les fichiers et répertoires.
- `cd` : Change de répertoire.
- `pwd` : Affiche le répertoire courant.
- `mkdir` : Crée un nouveau répertoire.
- `rm` : Supprime des fichiers ou des répertoires.
- `cp` : Copie des fichiers ou des répertoires.
- `mv` : Déplace ou renomme des fichiers.
- `chmod` : Modifie les permissions d'un fichier.
- `chown` : Change le propriétaire d'un fichier.

Ces commandes permettent de naviguer dans le système de fichiers, de manipuler les objets et de gérer les droits d'accès. 



---
# 2. Système de Fichiers Linux
---

## 2.1. **Arborescence des répertoires Linux**

Le système de fichiers Linux est structuré de manière hiérarchique sous forme d’une arborescence. À la racine de cette hiérarchie se trouve `/`, qui est le point de départ de tous les fichiers et répertoires du système.

## 2.2. **Répertoires principaux :**

- `/home` : Répertoires personnels des utilisateurs. Chaque utilisateur a un sous-répertoire (`/home/nom_utilisateur`).
- `/etc` : Contient les fichiers de configuration du système et des applications.
- `/var` : Contient les fichiers variables comme les logs, les bases de données et les fichiers temporaires générés par le système.
- `/tmp` : Répertoire temporaire utilisé pour stocker des fichiers temporaires créés par les utilisateurs et les applications. Les fichiers peuvent être supprimés après un redémarrage.
- `/bin` : Contient les commandes binaires essentielles (par exemple : `ls`, `cp`, `mv`, etc.) accessibles par tous les utilisateurs.
- `/usr` : Contient la plupart des applications utilisateur et leurs bibliothèques. Il est souvent subdivisé en `/usr/bin`, `/usr/lib`, `/usr/local`, etc.
- `/boot` : Contient les fichiers nécessaires au démarrage du système, comme le noyau Linux.
- `/dev` : Contient des fichiers spéciaux représentant des périphériques (par exemple : disques, imprimantes, etc.).
- `/proc` : Un système de fichiers virtuel qui fournit des informations sur les processus en cours et sur l'état du noyau.
- `/sys` : Un autre système de fichiers virtuel utilisé pour interagir avec les périphériques matériels et les sous-systèmes du noyau.
- `/lib` : Bibliothèques essentielles partagées par les programmes du système.



## 2.3. **Exploration et navigation dans le système de fichiers**

L'exploration du système de fichiers Linux se fait principalement via le terminal, en utilisant des commandes spécifiques.

### **Commandes essentielles :**
- `ls` : Liste les fichiers et répertoires d'un dossier.
  - `ls -l` : Affiche une liste détaillée (permissions, propriétaire, taille, date de modification).
  - `ls -a` : Affiche tous les fichiers, y compris les fichiers cachés (commençant par un point `.`).

- `cd` : Change le répertoire courant.
  - Exemple : `cd /home/user/Documents` pour se déplacer dans le répertoire Documents de l'utilisateur.
  - `cd ..` : Remonte d'un niveau dans l'arborescence.

- `pwd` : Affiche le répertoire courant.

- `find` : Recherche des fichiers dans une hiérarchie de répertoires.
  - Exemple : `find /home/user -name "*.txt"` recherche tous les fichiers texte dans `/home/user`.

- `du` : Affiche l'utilisation de l'espace disque par des fichiers et répertoires.
  - `du -h` : Affiche la taille des fichiers dans un format lisible par l'humain (Ko, Mo, Go).

- `df` : Affiche l'espace disque disponible sur les systèmes de fichiers montés.
  - `df -h` : Affiche les informations sous un format lisible par l'humain.



## 2.4. **Bonnes pratiques de gestion des répertoires**

1. **Organisation cohérente :**
   - Gardez une arborescence bien structurée. Par exemple, placez les projets personnels dans `/home/user/Projects` et les fichiers temporaires dans `/tmp`.

2. **Permissions appropriées :**
   - Vérifiez et ajustez régulièrement les permissions d'accès aux fichiers avec les commandes `chmod` et `chown`.

3. **Nettoyage régulier :**
   - Supprimez les fichiers inutilisés pour libérer de l'espace disque (`rm`, `rmdir`).

4. **Utilisation de liens symboliques :**
   - Facilitez l'accès à certains fichiers ou dossiers via `ln -s source destination`.

5. **Sauvegarde des fichiers importants :**
   - Utilisez des outils comme `rsync` ou `tar` pour réaliser des sauvegardes régulières.






---
# 3. Gestion des Utilisateurs et des Groupes
---

# 3.1. **Création et gestion des utilisateurs**

La gestion des utilisateurs dans Linux est essentielle pour administrer un système de manière sécurisée et organisée. Voici les commandes les plus courantes pour la gestion des utilisateurs :

### **1. Création d'utilisateurs**
- `useradd` : Crée un nouvel utilisateur sans configurer automatiquement le répertoire personnel.
  - Exemple : `sudo useradd alice`
  - Pour créer un répertoire personnel : `sudo useradd -m alice`
- `adduser` : Outil plus convivial pour ajouter un utilisateur avec un répertoire personnel.
  - Exemple : `sudo adduser bob`
  - Ce script interactif demande un mot de passe et des informations supplémentaires.

### **2. Modification d’utilisateurs**
- `usermod` : Modifie les propriétés d’un utilisateur existant.
  - Ajouter un utilisateur à un groupe : `sudo usermod -aG developers alice`
  - Changer le répertoire personnel : `sudo usermod -d /nouveau/chemin alice`
  - Renommer un utilisateur : `sudo usermod -l nouveau_nom ancien_nom`

### **3. Suppression d’utilisateurs**
- `userdel` : Supprime un utilisateur.
  - Exemple : `sudo userdel alice`
  - Pour supprimer le répertoire personnel en même temps : `sudo userdel -r alice`
- `deluser` : Interface conviviale pour supprimer un utilisateur.
  - Exemple : `sudo deluser bob`



## 3.2. **Gestion des groupes**

Les groupes sont utilisés pour organiser les utilisateurs ayant des privilèges similaires. Voici les principales commandes pour gérer les groupes :

### **1. Création et suppression de groupes**
- `groupadd` : Crée un groupe.
  - Exemple : `sudo groupadd developers`
- `groupdel` : Supprime un groupe.
  - Exemple : `sudo groupdel developers`

### **2. Ajout d’un utilisateur à un groupe**
- Pour ajouter un utilisateur à un groupe existant :
  - `sudo usermod -aG developers alice`
- Pour vérifier les groupes d'un utilisateur :
  - `groups alice`
  - `id alice`



## 3.3. **Vérification des utilisateurs existants**

Le fichier **`/etc/passwd`** contient des informations sur tous les utilisateurs du système.

### **Visualiser les utilisateurs existants**
Pour afficher la liste des utilisateurs, utilisez :
```bash
cat /etc/passwd
```
Chaque ligne représente un utilisateur et contient les informations suivantes, séparées par des deux-points (`:`) :
1. Nom d'utilisateur
2. Mot de passe (x signifie que le mot de passe est chiffré dans `/etc/shadow`)
3. UID (User ID)
4. GID (Group ID)
5. Commentaire ou nom complet
6. Répertoire personnel
7. Shell par défaut



## 3.4. **Gestion des mots de passe**

La gestion des mots de passe est essentielle pour assurer la sécurité du système.

### **1. Modifier le mot de passe d'un utilisateur**
- Utiliser la commande `passwd` :
  ```bash
  sudo passwd alice
  ```
  Elle demande d'entrer le nouveau mot de passe deux fois.

### **2. Politique de mots de passe sécurisée**
Pour configurer une politique de mots de passe forte, modifiez le fichier **`/etc/login.defs`** :
```bash
PASS_MAX_DAYS 90   # Expiration maximale (90 jours)
PASS_MIN_DAYS 0    # Délai minimal entre deux changements
PASS_WARN_AGE 7    # Avertissement d'expiration (7 jours avant)
```



## 3.5. **Les fichiers de configuration utilisateur**

Les utilisateurs peuvent personnaliser leur environnement via des fichiers de configuration spécifiques.

### **Principaux fichiers de configuration :**
- **`.bashrc`** : Chargé lors de l'ouverture d'un terminal interactif. Contient des alias et des variables.
- **`.profile`** : Chargé lors de la connexion (environnement non interactif).
- **`.bash_profile`** : Chargé lors de la connexion par un shell de connexion.
- **`/etc/profile`** : Chargé globalement pour tous les utilisateurs à la connexion.
- **`/etc/bash.bashrc`** : Fichier global pour tous les shells interactifs.

### **Différences entre ces fichiers :**
- **`.bashrc`** : Adapté pour les commandes interactives et les alias.
- **`.profile`** et **`.bash_profile`** : Utilisés pour configurer l'environnement (variables PATH, par exemple).
- **`/etc/profile`** et **`/etc/bash.bashrc`** : Affectent tous les utilisateurs.



## 3.6. **Cas pratiques**

1. **Créer un utilisateur et l'ajouter à un groupe spécifique :**
   ```bash
   sudo adduser alice
   sudo groupadd developers
   sudo usermod -aG developers alice
   ```

2. **Modifier le mot de passe d'un utilisateur :**
   ```bash
   sudo passwd alice
   ```

3. **Vérifier les utilisateurs dans le système :**
   ```bash
   cat /etc/passwd
   ```

4. **Configurer un environnement utilisateur :**
   ```bash
   nano /home/alice/.bashrc
   ```
   Ajouter des alias et des variables pour personnaliser l'environnement.





---
# **4. Gestion des Permissions et de la Sécurité**
---

## 4.1. **Système de permissions sous Linux**

La gestion des permissions sous Linux est essentielle pour garantir la sécurité du système et le contrôle des accès aux fichiers et répertoires. Les permissions définissent qui peut lire, écrire ou exécuter un fichier ou un répertoire.

### **1. Les trois types de permissions**
Les permissions sous Linux sont divisées en trois catégories :
- **Lecture (r)** : Permet de lire le contenu d'un fichier ou de lister les fichiers d'un répertoire.
- **Écriture (w)** : Permet de modifier ou de supprimer un fichier, ou d'ajouter/supprimer des fichiers dans un répertoire.
- **Exécution (x)** : Permet d'exécuter un fichier binaire ou un script, ou d'accéder à un répertoire.

### **2. Les trois types d’utilisateurs**
Les permissions sont attribuées à trois types d'utilisateurs :
- **Propriétaire (user)** : L'utilisateur qui a créé le fichier ou le répertoire.
- **Groupe (group)** : Les membres du groupe associé au fichier.
- **Autres (others)** : Tous les autres utilisateurs qui ne sont ni le propriétaire ni membres du groupe.



## 4.2. **Commandes de gestion des permissions**

### **1. Commande `chmod` (Changer les permissions)**
La commande `chmod` permet de modifier les permissions d'un fichier ou d'un répertoire.
#### **Syntaxe :**
```bash
chmod [options] mode fichier
```
#### **Exemples :**
- Ajouter la permission d'exécution pour le propriétaire :
  ```bash
  chmod u+x script.sh
  ```
- Supprimer la permission d'écriture pour le groupe :
  ```bash
  chmod g-w fichier.txt
  ```
- Accorder toutes les permissions au propriétaire :
  ```bash
  chmod u+rwx fichier.txt
  ```



### **2. Commande `chown` (Changer le propriétaire)**
La commande `chown` permet de changer le propriétaire et/ou le groupe d'un fichier.
#### **Syntaxe :**
```bash
chown [propriétaire][:groupe] fichier
```
#### **Exemples :**
- Changer le propriétaire :
  ```bash
  sudo chown alice fichier.txt
  ```
- Changer le propriétaire et le groupe :
  ```bash
  sudo chown alice:developers fichier.txt
  ```



### **3. Commande `chgrp` (Changer le groupe)**
La commande `chgrp` permet de changer uniquement le groupe d'un fichier.
#### **Syntaxe :**
```bash
chgrp [groupe] fichier
```
#### **Exemple :**
```bash
sudo chgrp developers fichier.txt
```



## 4.3. **Les permissions numériques et symboliques**

Les permissions peuvent être définies de deux manières : numérique (octal) ou symbolique.

### **1. Représentation symbolique**
La représentation symbolique utilise des lettres pour chaque permission :
- **r** : Lecture (4)
- **w** : Écriture (2)
- **x** : Exécution (1)
- **-** : Pas de permission (0)

#### **Exemple :**
```bash
chmod u=rwx,g=rx,o=r fichier.txt
```
Cela donne les permissions **rwxr-xr--**.



### **2. Représentation numérique (octale)**
Chaque ensemble de trois permissions est représenté par un nombre :
- **4** : Lecture
- **2** : Écriture
- **1** : Exécution
- **0** : Aucune permission

#### **Exemple :**
```bash
chmod 754 fichier.txt
```
Cela correspond à :
- **7 (rwx)** : Propriétaire
- **5 (r-x)** : Groupe
- **4 (r--)** : Autres



## 4.4.**Permissions avancées**

Certaines permissions spéciales permettent d'ajouter des fonctionnalités avancées de sécurité.

### **1. Sticky Bit**
Le sticky bit empêche les utilisateurs de supprimer des fichiers qu'ils ne possèdent pas dans un répertoire partagé.
#### **Exemple :**
```bash
chmod +t /dossier
```
Les permissions apparaissent avec un **"t"** à la fin :
```
drwxrwxrwt
```



### **2. Setuid (SUID)**
Le bit SUID permet à un utilisateur d'exécuter un fichier avec les privilèges du propriétaire.
#### **Exemple :**
```bash
chmod u+s programme
```
Les permissions apparaissent avec un **"s"** au lieu de **"x"** :
```
-rwsr-xr-x
```



### **3. Setgid (SGID)**
Le bit SGID permet aux fichiers créés dans un répertoire de conserver le groupe du répertoire parent.
#### **Exemple :**
```bash
chmod g+s /dossier
```
Les permissions apparaissent avec un **"s"** dans les permissions de groupe :
```
drwxr-sr-x
```



## **Cas pratiques et bonnes pratiques de sécurité**

### **1. Vérifier les permissions d'un fichier**
Pour voir les permissions d'un fichier ou d'un répertoire, utilisez :
```bash
ls -l fichier.txt
```

### **2. Vérifier les fichiers avec des permissions SUID/SGID**
Pour lister les fichiers avec SUID ou SGID activés :
```bash
find / -perm /6000 -type f 2>/dev/null
```

### **3. Bonnes pratiques de sécurité**
- Limitez les permissions des fichiers critiques pour éviter les accès non autorisés.
- Utilisez le sticky bit pour les répertoires partagés comme `/tmp`.
- Vérifiez régulièrement les fichiers avec des bits SUID/SGID pour éviter les vulnérabilités.
- Privilégiez des permissions restreintes par défaut et accordez des droits supplémentaires uniquement si nécessaire.



---
# **5. Les Fichiers de Configuration Shell**
---

## 5.1. **Introduction : Utilité et rôle des fichiers de configuration**

Les fichiers de configuration du shell sont essentiels pour personnaliser l’environnement de travail sous Linux. Ils permettent de définir des variables d'environnement, des alias, d'exécuter des scripts d'initialisation et de configurer le comportement global ou utilisateur du shell. 

Ces fichiers sont généralement situés dans le répertoire personnel de chaque utilisateur (`/home/nom_utilisateur`) ou dans des répertoires globaux accessibles à tous les utilisateurs (`/etc/`).



## 5.2. **.bashrc : Initialisation de l'environnement utilisateur**

### **Qu'est-ce que `.bashrc` ?**
Le fichier `.bashrc` est un script qui est exécuté à chaque fois qu'un nouveau terminal interactif est ouvert. Il est couramment utilisé pour définir des alias, des fonctions personnalisées, des variables d'environnement et des messages d'accueil.

### **Où se trouve `.bashrc` ?**
Chaque utilisateur possède un fichier `.bashrc` situé dans son répertoire personnel :  
```
/home/nom_utilisateur/.bashrc
```

### **Exemple de `.bashrc` :**
```bash
# Définir des alias courants
alias ll='ls -alF'
alias la='ls -A'
alias l='ls -CF'

# Ajouter un répertoire au PATH
export PATH=$PATH:/usr/local/bin

# Fonction personnalisée
function bienvenue() {
    echo "Bienvenue, $USER !"
}
```

### **Quand utiliser `.bashrc` ?**
- Pour configurer des alias ou des raccourcis de commandes.
- Pour modifier le `$PATH`.
- Pour définir des fonctions personnalisées.



## 5.3. **.profile : Configuration d'environnement pour les sessions interactives**

### **Qu'est-ce que `.profile` ?**
Le fichier `.profile` est exécuté par le shell de connexion lors de l'ouverture d'une session interactive. Contrairement à `.bashrc`, il est utilisé pour les configurations qui doivent s'appliquer lors de la connexion à un système (par exemple via SSH).

### **Où se trouve `.profile` ?**
Généralement dans le répertoire personnel de l'utilisateur :  
```
/home/nom_utilisateur/.profile
```

### **Exemple de `.profile` :**
```bash
# Modifier le PATH
PATH="$HOME/bin:$PATH"
export PATH

# Définir des variables d'environnement globales
export EDITOR=nano
export LANG=fr_FR.UTF-8
```

### **Quand utiliser `.profile` ?**
- Pour configurer des variables d'environnement globales.
- Pour définir des chemins personnalisés accessibles à l’ensemble des sessions utilisateur.


## 5.4. **.bash_profile : Script d'initialisation de session**

### **Qu'est-ce que `.bash_profile` ?**
Le fichier `.bash_profile` est un script d’initialisation exécuté uniquement lors de l'ouverture d'une nouvelle session par un shell de connexion (connexion SSH, par exemple). Il n'est pas exécuté lors de l'ouverture d'un terminal graphique sur un environnement de bureau.

### **Où se trouve `.bash_profile` ?**
```
/home/nom_utilisateur/.bash_profile
```

### **Exemple de `.bash_profile` :**
```bash
# Exécuter .bashrc si présent
if [ -f ~/.bashrc ]; then
    . ~/.bashrc
fi

# Variables d'environnement spécifiques
export PATH="$HOME/.local/bin:$PATH"
```

### **Quand utiliser `.bash_profile` ?**
- Lorsqu’on souhaite personnaliser l’environnement utilisateur pour les connexions distantes (SSH).
- Pour importer les configurations du fichier `.bashrc`.



## 5.5. **/etc/profile : Paramètres globaux pour tous les utilisateurs**

### **Qu'est-ce que `/etc/profile` ?**
Le fichier `/etc/profile` est un script d'initialisation exécuté globalement pour tous les utilisateurs lors de la connexion par un shell de connexion.

### **Où se trouve `/etc/profile` ?**
```
/etc/profile
```

### **Exemple de `/etc/profile` :**
```bash
# Définir des chemins globaux
PATH="/usr/local/bin:/usr/bin:/bin:/usr/local/sbin:/usr/sbin:/sbin"
export PATH

# Paramétrer l’éditeur par défaut pour tous les utilisateurs
export EDITOR=vim
```

### **Quand utiliser `/etc/profile` ?**
- Pour configurer des variables d'environnement globales accessibles à tous les utilisateurs.
- Pour définir des comportements par défaut lors de l'ouverture d'une session par un shell de connexion.



## 5.6. **Variables d'environnement et alias**

### **1. Variables d'environnement**
Les variables d'environnement sont des paires clé-valeur qui influencent le comportement du système et des applications.

#### **Définir une variable d'environnement :**
```bash
export NOM_VARIABLE="valeur"
```

#### **Exemple :**
```bash
export JAVA_HOME="/usr/lib/jvm/java-11-openjdk"
export PATH="$JAVA_HOME/bin:$PATH"
```

#### **Voir les variables d'environnement actuelles :**
```bash
printenv
```
ou
```bash
env
```



### **2. Alias**
Les alias permettent de créer des raccourcis pour des commandes courantes.

#### **Définir un alias :**
```bash
alias nom_alias='commande'
```

#### **Exemple :**
```bash
alias ll='ls -alF'
alias rm='rm -i'  # Demande une confirmation avant suppression
```

#### **Supprimer un alias temporairement :**
```bash
unalias nom_alias
```



## 5.7. **Bonnes pratiques pour la gestion des fichiers de configuration**

1. **Séparer les configurations utilisateur et système :**
   - Les fichiers situés dans `/etc/` sont destinés aux configurations globales.
   - Les fichiers situés dans `/home/nom_utilisateur/` sont destinés aux configurations spécifiques à l'utilisateur.

2. **Utiliser `.bashrc` pour les alias et fonctions :**
   - `.bashrc` est chargé automatiquement à chaque fois qu'un terminal interactif est ouvert. Placez-y vos alias, fonctions et personnalisation de prompt.

3. **Importer `.bashrc` dans `.bash_profile` :**
   - Si vous voulez que vos alias et fonctions soient disponibles en connexion SSH, ajoutez ceci à votre `.bash_profile` :
     ```bash
     if [ -f ~/.bashrc ]; then
         . ~/.bashrc
     fi
     ```

4. **Ne modifiez pas directement `/etc/profile` :**
   - Si des modifications globales sont nécessaires, ajoutez-les plutôt dans `/etc/profile.d/` en créant un nouveau fichier `.sh`.

5. **Utiliser des commentaires :**
   - Commentez vos fichiers de configuration pour savoir à quoi sert chaque modification. Cela facilite la maintenance.








# 6. Sécurité des Systèmes Linux

## 6.1. **Introduction : Importance de la Sécurité sous Linux**

Linux est réputé pour sa sécurité robuste. Cependant, la sécurisation d'un système Linux ne se limite pas à ses configurations par défaut. Elle nécessite une gestion appropriée des permissions, des utilisateurs, des fichiers critiques, des accès administratifs et des journaux d’audit.



## 6.2. **Bonnes pratiques pour sécuriser les fichiers critiques**

La protection des fichiers critiques est une étape essentielle pour maintenir un système Linux sécurisé.

### **1. Restreindre les permissions d'accès**
- Les fichiers sensibles tels que `/etc/passwd` et `/etc/shadow` doivent avoir des permissions limitées.
  - **/etc/passwd** : Contient les informations des utilisateurs (lisible par tous mais modifiable uniquement par root).
  - **/etc/shadow** : Contient les mots de passe chiffrés (accessible uniquement par root).

#### **Exemples de permissions sécurisées :**
```bash
ls -l /etc/passwd     # -rw-r--r--
ls -l /etc/shadow     # -rw------- (Accessible uniquement par root)
```

### **2. Utiliser le Sticky Bit sur les répertoires partagés**
Empêcher les utilisateurs non privilégiés de supprimer les fichiers d’autres utilisateurs dans des répertoires communs.
```bash
chmod +t /dossier_partage
```

### **3. Configurer correctement les fichiers `.bashrc` et `.profile`**
- Éviter d’y inscrire des commandes exécutées automatiquement avec des droits élevés.
- Restreindre les permissions d’accès en lecture et en écriture par d’autres utilisateurs.
```bash
chmod 644 ~/.bashrc
chmod 644 ~/.profile
```



## 6.3. **Gestion des droits d'administration (sudo)**

L'utilisation de `sudo` permet de déléguer des privilèges spécifiques aux utilisateurs sans leur donner un accès complet au compte root.

### **1. Installation de `sudo` (si nécessaire)**
```bash
apt-get install sudo     # Pour les distributions basées sur Debian/Ubuntu
```

### **2. Ajouter un utilisateur au groupe sudo**
```bash
usermod -aG sudo nom_utilisateur
```

### **3. Modification du fichier de configuration sudoers**
Les règles de `sudo` sont définies dans le fichier **`/etc/sudoers`**. Pour éditer ce fichier en toute sécurité, utilisez :
```bash
sudo visudo
```

### **4. Exemple de configuration sudoers**
```bash
# Permettre à alice d'exécuter toutes les commandes comme root
alice ALL=(ALL) ALL

# Permettre à bob d'utiliser des commandes spécifiques uniquement
bob ALL=(ALL) /usr/bin/apt-get, /usr/bin/systemctl
```

### **5. Bonnes pratiques pour `sudo`**
- **Limiter l'utilisation de sudo :** Seuls les utilisateurs qui en ont réellement besoin devraient avoir un accès `sudo`.
- **Configurer des alias de commande :** Pour limiter les actions possibles via `sudo`.
- **Examiner régulièrement les utilisateurs autorisés à utiliser `sudo` :**
  ```bash
  grep '^sudo:' /etc/group
  ```



## 6.4. **Stratégies de gestion des accès**

### **1. Restreindre l'accès SSH**
- Désactiver les connexions SSH root en modifiant le fichier **`/etc/ssh/sshd_config`** :
  ```bash
  PermitRootLogin no
  ```
- Utiliser l’authentification par clé publique au lieu de l’authentification par mot de passe :
  ```bash
  PasswordAuthentication no
  ```

### **2. Implémenter des politiques d'accès stricte**
- Configurer des permissions minimales nécessaires (`chmod`, `chown`).
- Utiliser des listes de contrôle d'accès (ACL) pour des permissions spécifiques :
  ```bash
  setfacl -m u:alice:rwx fichier.txt
  getfacl fichier.txt
  ```



## 6.5. **Gestion des journaux d'audit**

Les journaux sont essentiels pour comprendre l'état de votre système, détecter des activités suspectes et diagnostiquer des problèmes.

### **1. `journalctl` : Consultation des journaux système**
La commande `journalctl` permet d'afficher et de filtrer les journaux du système générés par **`systemd`**.

#### **Exemples courants :**
- Afficher tous les journaux :
  ```bash
  sudo journalctl
  ```
- Afficher les logs du démarrage précédent :
  ```bash
  sudo journalctl -b -1
  ```
- Filtrer par service (ex. sshd) :
  ```bash
  sudo journalctl -u sshd
  ```
- Filtrer par date :
  ```bash
  sudo journalctl --since "2025-03-27 10:00:00"
  ```



### **2. `/var/log` : Répertoire des logs classiques**
Les journaux du système sont également stockés dans le répertoire `/var/log/`.

#### **Principaux fichiers de log :**
- `/var/log/auth.log` : Journalisation des authentifications et des tentatives d'accès (SSH, sudo, etc.)
- `/var/log/syslog` : Journalisation générale du système.
- `/var/log/kern.log` : Messages relatifs au noyau.
- `/var/log/dpkg.log` : Journal des installations de paquets sur Debian/Ubuntu.

#### **Vérifier un fichier de log :**
```bash
sudo cat /var/log/auth.log
```
ou en temps réel :
```bash
sudo tail -f /var/log/auth.log
```



### **3. Rotation des journaux (`logrotate`)**
Pour éviter l'accumulation excessive de fichiers journaux, le système Linux utilise un outil appelé **`logrotate`** qui archive automatiquement les logs anciens.

#### **Vérification de la configuration de `logrotate` :**
```bash
cat /etc/logrotate.conf
```
Les fichiers de configuration supplémentaires peuvent se trouver dans :
```
/etc/logrotate.d/
```



## 6.6. **Bonnes pratiques pour la sécurité**

1. **Limiter les accès `sudo` :**
   - Utiliser le fichier `sudoers` pour contrôler qui peut exécuter quelles commandes.

2. **Vérifier régulièrement les journaux d'audit :**
   - Utiliser `journalctl` et les fichiers de `/var/log/` pour détecter des activités suspectes.

3. **Configurer un système de rotation de journaux efficace :**
   - Garantir que les fichiers de logs ne remplissent pas inutilement l'espace disque.

4. **Restreindre l'accès aux fichiers critiques :**
   - Limiter les permissions aux utilisateurs de confiance uniquement.

5. **Désactiver les connexions root par SSH :**
   - Préférer l’utilisation de comptes utilisateur avec `sudo`.

6. **Surveiller les fichiers modifiés récemment :**
   ```bash
   sudo find / -type f -mtime -1
   ```
   Cela permet de détecter d'éventuelles modifications suspectes.

