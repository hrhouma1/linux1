# **Cours Linux : Gestion des Utilisateurs, Permissions et Configuration**


# **1. Introduction à Linux**
- Présentation générale de Linux
- Avantages d'utiliser Linux pour l'administration système
- Concepts de base : noyau, shell, commandes



# **2. Système de Fichiers Linux**

- Arborescence des répertoires Linux
  - `/home`, `/etc`, `/var`, `/tmp`, `/bin`, `/usr`, etc.
- Exploration et navigation dans le système de fichiers
  - Commandes : `ls`, `cd`, `pwd`, `find`, `du`, `df`
- Bonnes pratiques de gestion des répertoires



# **3. Gestion des Utilisateurs et des Groupes**
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



# **4. Gestion des Permissions et de la Sécurité**
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



# **5. Les Fichiers de Configuration Shell**
- Utilité et rôle des fichiers de configuration
  - `.bashrc` : initialisation de l'environnement utilisateur
  - `.profile` : configuration d'environnement pour les sessions interactives
  - `.bash_profile` : script d'initialisation de session
  - `/etc/profile` : paramètres globaux pour tous les utilisateurs
- Variables d'environnement et alias
  - Comment les définir et les utiliser
- Bonnes pratiques pour la gestion des fichiers de configuration



# **6. Automatisation et Scripts Bash**
- Introduction aux scripts Bash
- Syntaxe de base : variables, conditions, boucles
- Exemples de scripts pour la gestion des utilisateurs et des permissions
- Sécurité des scripts



# **7. Sécurité des Systèmes Linux**
- Bonnes pratiques pour sécuriser les fichiers critiques
- Gestion des droits d'administration (`sudo`)
- Stratégies de gestion des accès
- Gestion des journaux d'audit
  - `journalctl`, `/var/log`



# **8. Exercices Pratiques et Quiz**
- Exercices guidés pour chaque chapitre
- Quiz pour évaluer les connaissances acquises
- Études de cas concrets



# **9. Conclusion**
- Récapitulatif des concepts abordés
- Conseils pour l'administration avancée



----
# **1. Introduction à Linux**
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
# **2. Système de Fichiers Linux**
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
# **3. Gestion des Utilisateurs et des Groupes**
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
# **5. Gestion des Permissions et de la Sécurité**
---

## 5.1. **Système de permissions sous Linux**

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



## 5.2. **Commandes de gestion des permissions**

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



## 5.3. **Les permissions numériques et symboliques**

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



## 5.4.**Permissions avancées**

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

