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

- Évitez de vous connecter directement en root (❌).
- Préférez `sudo` pour tracer les commandes exécutées (✅).
- Ne donnez pas l’accès `sudo` à tous les utilisateurs (⚠️).
- Toujours éditer le fichier `sudoers` avec `visudo` (✅).



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


# Correction du TP


### 11.1. Créer un utilisateur nommé `testsudo`
```bash
sudo adduser testsudo
```

### 11.2. Définir un mot de passe pour `testsudo`
```bash
sudo passwd testsudo
```

### 11.3. Tester une commande interdite par défaut
```bash
su - testsudo
ls /root
```
💡 Résultat attendu :  
> Permission denied (tests le fait que `testsudo` n’a aucun droit sudo pour le moment).



### 11.4. Ajout temporaire via `visudo`
```bash
sudo visudo
```

Ajouter cette ligne **temporairement** :
```bash
testsudo ALL=(ALL) NOPASSWD:ALL
```

**Test immédiat :**
```bash
su - testsudo
sudo ls /root
```

💡 Résultat attendu :  
> `ls /root` fonctionne (accès root via `sudo` temporaire car injecté manuellement).



### 5. Supprimer l’entrée manuelle de `sudoers`
```bash
sudo visudo
```

Supprimez la ligne :
```bash
testsudo ALL=(ALL) NOPASSWD:ALL
```

⛔️ Retestez :
```bash
su - testsudo
sudo ls /root
```

💡 Résultat attendu :  
> `sudo` ne fonctionne plus : preuve que **l’entrée via `visudo` était temporaire et volontaire**.



### 6. Ajout permanent via le groupe `sudo`
```bash
sudo usermod -aG sudo testsudo
```

Déconnectez / reconnectez `testsudo` ou :
```bash
su - testsudo
```

Test :
```bash
sudo ls /root
```

💡 Résultat attendu :  
> Le droit sudo est désormais **permanent** car hérité via le groupe.



### 7. Comparer `sudo`, `sudo -s`, `sudo -i`

Utilisez ces commandes sous `testsudo` après ajout au groupe `sudo` :

| Commande         | Description                                      | Prompt shell | Variables | Exemple de commande |
|------------------|--------------------------------------------------|--------------|-----------|----------------------|
| `sudo ls /root`  | Exécute une seule commande en root               | Non          | Inchangeées | Affiche `/root`     |
| `sudo -s`        | Lance un shell root avec **les variables de l’utilisateur** | Oui (root#) | utilisateur | `echo $HOME` → `/home/testsudo` |
| `sudo -i`        | Lance un shell root avec **environnement root**  | Oui (root#) | root       | `echo $HOME` → `/root`          |

**Captures à inclure dans le rapport** :
- Avant tout ajout → `sudo` échoue
- Après `visudo` → `sudo` fonctionne
- Après suppression `visudo` → `sudo` échoue de nouveau
- Après ajout au groupe sudo → `sudo` fonctionne à nouveau
- Comparatif `sudo`, `sudo -s`, `sudo -i`



## Tableau comparatif final (à inclure dans rapport)

| Test effectué                            | Résultat attendu                     | Résultat observé       |
|------------------------------------------|--------------------------------------|-------------------------|
| `su - testsudo` + `sudo ls /root` (avant) | Permission refusée                   | ✅                      |
| Ajout via `visudo`                        | Accès temporaire à sudo              | ✅                      |
| Suppression de l'entrée `visudo`          | Plus d'accès sudo                    | ✅                      |
| Ajout au groupe `sudo`                    | Accès permanent                      | ✅                      |
| `sudo`                                    | Exécute une commande                 | ✅                      |
| `sudo -s`                                 | Shell root, env utilisateur          | ✅                      |
| `sudo -i`                                 | Shell root, env root                 | ✅                      |




# 12. Pour aller plus loin

- Consultez la documentation `man su`, `man sudo`, `man sudoers`.
- Expérimentez la configuration de `sudo` dans des environnements isolés (ex: VM ou conteneurs).





---
# 📎 Annexe 1 – Options détaillées de `sudo -s` et `sudo -i`
---

## 1. `sudo -s` : Shell root avec environnement utilisateur

###  Description :
- Ouvre un shell avec les privilèges de `root`.
- **Conserve** l’environnement de l’utilisateur actuel (`$HOME`, `$PATH`, etc.).
- Pratique pour garder ses variables tout en ayant les droits root.

### Syntaxe générale :
```bash
sudo -s [OPTION] [--] [commande]
```

### Options utiles avec `sudo -s` :

| Option       | Description |
|--------------|-------------|
| `-s`         | Lance un shell root (en gardant l’environnement utilisateur). |
| `-u utilisateur` | Exécute le shell en tant qu’un autre utilisateur que root. |
| `-E`         | Préserve l’environnement complet (équivalent à `env_keep`). |
| `-k`         | Force la demande de mot de passe (ne pas réutiliser l’authentification). |
| `-K`         | Supprime le timestamp de session sudo (révoque l'accès temporaire). |
| `-H`         | Définit `$HOME` à celui de l’utilisateur cible (`root` par défaut). |
| `--login`    | Synonyme de `-i`. (Rarement utilisé avec `-s`) |
| `--`         | Sépare les options sudo des options passées à la commande appelée. |

###  Exemple :
```bash
sudo -s
sudo -s -E
sudo -s -u postgres
```



## 2. `sudo -i` : Shell root avec environnement root (login shell)

###  Description :
- Ouvre un **shell de connexion root**, comme `su -`.
- Charge le fichier `.profile`, `.bashrc` ou `.bash_profile` du root.
- Change le `$HOME` vers `/root`, le prompt, etc.

###  Syntaxe générale :
```bash
sudo -i [OPTION] [--] [commande]
```

###  Options utiles avec `sudo -i` :

| Option       | Description |
|--------------|-------------|
| `-i`         | Lance un shell interactif de connexion (`login shell`) comme si on se connectait directement en root. |
| `-u utilisateur` | Exécute le shell de connexion comme un autre utilisateur. |
| `-E`         | Préserve certaines variables d’environnement. |
| `-H`         | Définit `$HOME` à celui de l’utilisateur cible. |
| `-k`         | Réinitialise le timestamp sudo (demande à nouveau le mot de passe). |
| `-K`         | Invalide totalement l’accès sudo jusqu’à nouvelle authentification. |
| `--`         | Permet d’exécuter une commande complexe après sudo. |

###  Exemple :
```bash
sudo -i
sudo -i -u postgres
sudo -i -- ls /root
```



## 3. Différences résumées entre `-s` et `-i`

| Aspect                 | `sudo -s`                          | `sudo -i`                             |
|------------------------|------------------------------------|----------------------------------------|
| Type de shell          | Shell root non interactif complet | Shell de login root                    |
| Chargement des fichiers| Aucun (`.bashrc` utilisateur)     | Charge `.profile`, `.bashrc` de root   |
| Environnement (`$HOME`)| Conservé                         | Changé vers `/root`                    |
| Usage typique          | Pour faire quelques commandes     | Pour un vrai changement de session     |



>  **Conseil** : Pour expérimenter, créez deux scripts dans `/root/` et `/home/etudiant/`, puis observez lequel est chargé avec `sudo -s` vs `sudo -i`.


