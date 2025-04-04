# Comprendre et Maîtriser les Commandes : su, sudo -s, sudo -i, sudo, passwd, sudoers

<br/> 

# 1. Introduction

Dans un système Linux, la gestion des permissions est cruciale pour la sécurité.  
Ce module vous apprendra à comprendre et à utiliser les commandes liées à l'élévation de privilèges, à la création d'utilisateurs et à la configuration de l'accès au superutilisateur (`root`).


<br/> 

# 2. Définitions des commandes

- `su` : Se connecter en tant qu’un autre utilisateur (souvent root). Demande le mot de passe du compte cible.
- `sudo` : Exécute une commande avec des privilèges élevés (root), si vous êtes dans le groupe `sudo`.
- `sudo -s` : Lance un shell root mais garde l’environnement de l’utilisateur courant.
- `sudo -i` : Lance un shell root avec l’environnement complet de `root`, comme si on se connectait avec `su -`.


<br/> 

# 3. Exemples pratiques

```bash
# Se connecter en tant que root (mot de passe root requis)
su

# Exécuter une commande avec élévation temporaire
sudo apt update

# Shell root avec environnement utilisateur
sudo -s

# Shell root avec environnement root complet
sudo -i
```

<br/> 

# 4. Création d’un nouvel utilisateur

```bash
# Création de l’utilisateur "etudiant"
sudo adduser etudiant

# Ajouter l'utilisateur au groupe sudo (accès permanent)
sudo usermod -aG sudo etudiant
```

> ✅ L'utilisateur `etudiant` pourra utiliser `sudo` après reconnexion.


<br/> 

# 5. Définir ou changer le mot de passe

```bash
# Définir un mot de passe pour un utilisateur
sudo passwd etudiant
```




<br/> 

# 6. Ajout temporaire dans sudoers (session actuelle seulement)

```bash
# Ouvrir le fichier sudoers avec l’éditeur sécurisé
su -
visudo
```

Ajoutez cette ligne tout en bas :

```bash
etudiant ALL=(ALL) NOPASSWD: ALL
```

> ⚠️ Ce type d’ajout est temporaire (non basé sur les groupes) et moins recommandé.



<br/> 

# 7. Ajout permanent via le groupe sudo

```bash
sudo usermod -aG sudo etudiant
```

> ✅ Cette méthode est persistante, propre et recommandée.




<br/> 

# 8. Résumé des différences

| Commande     | Effet                                           | Mot de passe demandé      | Contexte                   |
|--------------|--------------------------------------------------|----------------------------|----------------------------|
| `su`         | Se connecter en tant qu’un autre utilisateur     | Mot de passe de l'autre utilisateur (souvent root) | Changement d’utilisateur |
| `sudo`       | Exécuter une commande avec élévation             | Mot de passe de l’utilisateur courant | Commande unique |
| `sudo -s`    | Shell root avec env utilisateur                  | Mot de passe utilisateur   | Session temporaire         |
| `sudo -i`    | Shell root complet (comme `su -`)                | Mot de passe utilisateur   | Session root complète      |




<br/> 

# 9. Vérifier si un utilisateur a accès à sudo

```bash
groups etudiant
```

> Si `sudo` fait partie des groupes listés, l’accès est actif.



<br/> 

# 10. Bonnes pratiques

- ❌ Évitez de vous connecter directement en root.
- ✅ Préférez `sudo` pour tracer les commandes exécutées.
- ⚠️ Ne donnez pas l’accès `sudo` à tous les utilisateurs.
- ✅ Toujours éditer le fichier `sudoers` avec `visudo`.



<br/> 

# 11. TP à faire

1. Créez un utilisateur nommé `testsudo`.
2. Définissez un mot de passe pour ce nouvel utilisateur.
3. Testez une commande simple (`ls /root`) avec `su` et `sudo`.
4. Ajoutez temporairement `testsudo` au fichier sudoers via `visudo`.
5. Supprimez l’entrée dans `sudoers`.
6. Ajoutez `testsudo` au groupe `sudo`.
7. Comparez le comportement de `sudo`, `sudo -s`, `sudo -i`.
8. Réalisez un tableau comparatif comme celui vu en cours.
9. Incluez des captures d’écran dans votre rapport final.



# 12. Pour aller plus loin

- Consultez la documentation `man su`, `man sudo`, `man sudoers`.
- Expérimentez la configuration de `sudo` dans des environnements isolés (ex: VM ou conteneurs).

