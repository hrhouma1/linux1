# Variables et les Variables d’Environnement sous Linux


<br/>
<br/>

# 1. Qu’est-ce qu’une variable ?

Une **variable** est un conteneur qui permet de stocker une **valeur**.  
Sous Linux (et en shell Bash), elle peut contenir du texte, un nombre ou même le résultat d’une commande.

### Syntaxe :

```bash
nom_variable=valeur
```

>  **Aucun espace autour du signe `=` !**

###  Exemple :

```bash
prenom=Haythem
echo $prenom   # Affiche : Haythem
```



# 2. Qu’est-ce qu’une variable d’environnement ?

Une **variable d’environnement** est une variable **globale** : elle est visible par tous les processus lancés à partir de la session actuelle (shell, programmes, scripts...).

###  Comment rendre une variable accessible globalement ?

On utilise la commande `export` :

```bash
export LANG=fr_FR.UTF-8
export JAVA_HOME=/usr/lib/jvm/java-17-openjdk
```

###  Afficher une variable d’environnement :

```bash
echo $LANG
echo $JAVA_HOME
```



# 3. Tester et voir toutes les variables

### Variables normales (locales au shell) :

```bash
set      # Affiche toutes les variables locales
```

###  Variables d’environnement (globales) :

```bash
env      # Affiche toutes les variables d’environnement
printenv # Aussi utile pour les variables spécifiques
```

###  Rechercher un mot-clé :

```bash
env | grep proxy
```



# ❌ 4. Supprimer (unset) une variable

### Pour désactiver une variable :

```bash
unset NOM_VARIABLE
```

### Exemple :

```bash
unset http_proxy
unset JAVA_HOME
```



# 5. Variables utiles et classiques

| Variable         | Rôle                                |
|------------------|-------------------------------------|
| `PATH`           | Chemin des exécutables              |
| `HOME`           | Répertoire personnel de l’utilisateur |
| `USER`           | Nom de l’utilisateur connecté       |
| `SHELL`          | Type de shell utilisé               |
| `PWD`            | Répertoire courant                  |
| `LANG`           | Langue par défaut                   |



# 6. Exemple complet

```bash
# Déclaration locale
prenom=Sarah
echo $prenom      # Sarah

# Rendre global
export prenom
bash              # On lance un nouveau shell
echo $prenom      # Sarah (car exporté)

# Supprimer
unset prenom
```



# 7. Conclusion

Les variables sont essentielles pour automatiser, configurer et personnaliser votre environnement Linux.  
Les **variables d’environnement** permettent de propager des configurations à tous les processus de votre session.

> Conseil : utilisez `.bashrc` pour définir vos variables de façon permanente.
