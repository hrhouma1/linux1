# Supprimer les Utilisateurs Existants et Ajouter de Nouveaux Utilisateurs pour Affichage dans les Options de Connexion

## Introduction
Ce guide vous explique comment supprimer des utilisateurs existants sur un système Linux, puis en ajouter de nouveaux de manière à ce qu'ils soient visibles dans les options de connexion après un redémarrage. Ce processus garantit que tous les utilisateurs sont correctement configurés avec leurs répertoires personnels.

## Étapes à Suivre

### 1. Se Connecter en Tant que Superutilisateur

Avant de commencer, assurez-vous d'avoir les privilèges nécessaires pour gérer les utilisateurs. Utilisez la commande suivante pour passer en superutilisateur (root) :

```bash
su
# Entrez le mot de passe du superutilisateur
```

### 2. Supprimer les Utilisateurs S'ils Existent Déjà

Pour éviter toute duplication ou conflit, commencez par supprimer les utilisateurs s'ils existent déjà. Remplacez `nom_utilisateur` par le nom de chaque utilisateur que vous souhaitez supprimer :

```bash
# Supprimer les utilisateurs et leurs répertoires personnels
userdel -r pierre
userdel -r marie
userdel -r luc
userdel -r sophie
```

Cette commande supprime les utilisateurs spécifiés ainsi que leurs répertoires personnels.

### 3. Ajouter de Nouveaux Utilisateurs avec `adduser`

Une fois les utilisateurs supprimés, vous pouvez créer de nouveaux utilisateurs. Utilisez `adduser` pour vous assurer que chaque utilisateur est correctement configuré avec un répertoire personnel et un mot de passe :

```bash
# Créer des utilisateurs
adduser pierre
adduser marie
adduser luc
adduser sophie
```

La commande `adduser` vous guidera à travers le processus interactif pour configurer le mot de passe et d'autres détails.

### 4. Vérifier la Création des Répertoires Personnels

Assurez-vous que les répertoires personnels pour chaque utilisateur ont été créés dans `/home` :

```bash
ls /home
```

Vous devriez voir un répertoire pour chaque utilisateur que vous avez créé.

### 5. Assigner un Shell de Connexion

Vérifiez que chaque utilisateur a un shell de connexion valide. Vous pouvez le vérifier en consultant le fichier `/etc/passwd` :

```bash
cat /etc/passwd | grep nom_utilisateur
```

Chaque utilisateur devrait avoir un shell de connexion valide (généralement `/bin/bash` ou `/bin/sh`).

### 6. Redémarrer le Système

Une fois les utilisateurs créés, redémarrez le système pour appliquer les changements et permettre l'affichage des nouveaux utilisateurs dans les options de connexion :

```bash
reboot
```

### 7. Connexion avec les Nouveaux Utilisateurs

Après le redémarrage, l'écran de connexion devrait afficher les nouveaux utilisateurs parmi les options. Vous pouvez maintenant sélectionner un utilisateur et vous connecter avec le mot de passe que vous avez configuré.

### 8. Résolution des Problèmes

Si les utilisateurs ne sont pas affichés :
- **Vérifiez les permissions** : Assurez-vous que les répertoires personnels ont les bonnes permissions.
- **Vérifiez les configurations du gestionnaire de connexion** : Selon le gestionnaire de connexion utilisé (GDM, LightDM, etc.), il peut y avoir des configurations spécifiques qui cachent certains utilisateurs.

### 9. Utilisation de `useradd` (Optionnel)

Si vous préférez utiliser `useradd`, assurez-vous d'utiliser l'option `-m` pour créer un répertoire personnel et suivez les étapes suivantes pour définir un mot de passe :

```bash
useradd -m julie
passwd julie
```

Ensuite, redémarrez le système pour permettre l'affichage de l'utilisateur dans les options de connexion.

## Conclusion
En suivant ce guide, vous pourrez gérer les utilisateurs de manière à ce qu'ils soient visibles dans les options de connexion après un redémarrage du système. La suppression des utilisateurs existants avant d'en ajouter de nouveaux garantit un environnement propre et sans conflits.


# Annexe 01 

Lorsque vous redémarrez votre système et que vous ne voyez pas les autres utilisateurs, plusieurs raisons peuvent expliquer ce problème. Voici quelques points à vérifier, en relation avec les commandes `useradd` et `adduser` :

### 1. **Création des Répertoires d'Utilisateur :**
   - Lorsque vous utilisez la commande `useradd -m`, ou `adduser`, un répertoire personnel (par exemple, `/home/luc`) est automatiquement créé pour chaque utilisateur. Ce répertoire contient des fichiers de configuration initiaux.
   - Si vous avez créé un utilisateur sans l'option `-m` avec `useradd`, le répertoire personnel ne sera pas créé automatiquement, ce qui peut causer des problèmes lors de la connexion de l'utilisateur, car le système ne trouve pas le répertoire d'accueil.

### 2. **Vérification des Comptes Utilisateurs :**
   - Vous pouvez vérifier si les utilisateurs existent réellement sur le système en consultant le fichier `/etc/passwd`, qui liste tous les comptes utilisateurs :
     ```bash
     cat /etc/passwd | grep nom_utilisateur
     ```
     Remplacez `nom_utilisateur` par le nom d'un utilisateur que vous ne voyez pas.

### 3. **Vérification des Répertoires Personnels :**
   - Assurez-vous que les répertoires personnels des utilisateurs sont présents dans `/home`. Vous pouvez le vérifier avec la commande suivante :
     ```bash
     ls /home
     ```
     Si le répertoire personnel d'un utilisateur n'est pas présent, cela pourrait expliquer pourquoi l'utilisateur n'apparaît pas ou ne peut pas se connecter.

### 4. **Permissions et Groupes :**
   - Si les permissions ou les groupes des utilisateurs ne sont pas correctement configurés, cela peut également poser des problèmes. Assurez-vous que chaque utilisateur appartient à son propre groupe et que les permissions sur le répertoire personnel sont correctement définies :
     ```bash
     ls -ld /home/nom_utilisateur
     ```
     Cette commande affichera les permissions et le propriétaire du répertoire personnel de l'utilisateur.

### 5. **Vérification des Services Liés à la Connexion :**
   - Assurez-vous que les services de gestion des connexions (comme `lightdm`, `gdm3`, `sssd`, ou autre selon votre distribution) sont correctement configurés pour reconnaître les utilisateurs locaux. Parfois, des configurations spécifiques peuvent masquer certains utilisateurs.

### 6. **Différence entre `useradd` et `adduser` :**
   - **`useradd`** : Crée un utilisateur avec peu d'interactions. Si utilisé sans l'option `-m`, il ne crée pas de répertoire personnel.
   - **`adduser`** : Fournit un processus interactif qui, par défaut, crée un répertoire personnel pour l'utilisateur. Cette commande est recommandée si vous souhaitez vous assurer que toutes les configurations de l'utilisateur sont correctement appliquées.

### 7. **Rejet des Connexions (Possible)** :
   - Si un utilisateur est ajouté mais ne peut pas se connecter, il est possible que le shell par défaut ne soit pas défini, ou que l'utilisateur soit désactivé. Vous pouvez vérifier cela avec :
     ```bash
     sudo passwd -S nom_utilisateur
     ```
     Cela affichera l'état du compte utilisateur.

### Conclusion
Si vous avez créé les utilisateurs avec `useradd` sans l'option `-m`, ou si leur répertoire personnel n'a pas été configuré correctement, cela pourrait expliquer pourquoi ils ne sont pas visibles ou ne peuvent pas se connecter après le redémarrage du système. Utilisez de préférence `adduser` ou `useradd -m` pour vous assurer que toutes les configurations nécessaires sont mises en place.

