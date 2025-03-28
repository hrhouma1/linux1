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

