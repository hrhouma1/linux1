# **Exercice 1 : Déclaration d'une variable locale**

**Objectif** : Comprendre la déclaration et l’affichage d’une variable locale.

**Consignes** :  
1. Créez une variable appelée `ville` et affectez-lui la valeur `Montreal`.  
2. Affichez son contenu avec `echo`.  
3. Lancez un nouveau shell avec la commande `bash`.  
4. Essayez d’afficher à nouveau la variable `ville`.

**Questions** :  
- La variable est-elle encore disponible dans le nouveau shell ?  
- Pourquoi ne l’est-elle plus ?

<br/>

# **Exercice 2 : De la variable locale à la variable d’environnement**

**Objectif** : Comprendre le rôle de la commande `export`.

**Consignes** :  
1. Créez une variable appelée `universite` avec la valeur `UdeM`.  
2. Rendez-la globale avec la commande `export`.  
3. Lancez un nouveau shell avec la commande `bash`.  
4. Affichez la variable `universite`.

**Questions** :  
- Quelle est la différence entre une variable locale et une variable d’environnement ?  
- Pourquoi `export` est-il nécessaire ici ?

<br/>

# **Exercice 3 : Exploration des variables existantes**

**Objectif** : Observer les variables existantes dans le système.

**Consignes** :  
1. Affichez toutes les variables d’environnement avec `env`.  
2. Utilisez la commande `env | grep LANG` pour filtrer les variables de langue.  
3. Affichez la valeur des variables suivantes :  
   - `HOME`  
   - `USER`  
   - `PWD`  
   - `SHELL`

**Questions** :  
- Quelle est la différence entre `env` et `set` ?  
- À quoi servent les variables affichées ?

<br/>

# **Exercice 4 : Suppression d’une variable**

**Objectif** : Comprendre la suppression de variables avec `unset`.

**Consignes** :  
1. Créez une variable locale `testvar` avec la valeur `bonjour`.  
2. Affichez-la.  
3. Supprimez-la avec `unset testvar`.  
4. Réessayez de l’afficher.

**Extension** :  
- Répétez les étapes ci-dessus avec une variable exportée (`export testvar=bonjour`).
- Quelle différence constatez-vous dans le comportement ?

<br/>

# **Exercice 5 : Tester la portée des variables dans un script**

**Objectif** : Vérifier quelles variables sont visibles dans un script.

**Consignes** :  
1. Créez un script nommé `test_variable.sh` avec le contenu suivant :

```bash
#!/bin/bash
echo "Nom = $nom"
echo "Ville = $ville"
```

2. Dans le terminal, tapez les lignes suivantes :

```bash
nom="Alice"
ville="Paris"
export ville
bash test_variable.sh
```

**Questions** :  
- Quelle(s) variable(s) est/sont visibles dans le script ?  
- Pourquoi `nom` n’est-elle pas affichée correctement ?  
- Que faut-il faire pour rendre `nom` accessible dans le script ?

<br/>

# **Exercice 6 : Modification temporaire de la variable PATH**

**Objectif** : Comprendre l’effet de la modification temporaire d’une variable système.

**Consignes** :  
1. Affichez la variable `PATH`.  
2. Ajoutez temporairement un dossier fictif au début du chemin :

```bash
export PATH="/fake/bin:$PATH"
```

3. Vérifiez que le dossier `/fake/bin` est bien affiché au début de la variable `PATH`.  
4. Lancez un nouveau shell avec `bash` et affichez à nouveau `PATH`.

**Question** :  
- Pourquoi la modification n’est-elle pas conservée dans le nouveau shell ?

<br/>

# **Exercice 7 : Définir une variable de façon permanente**

**Objectif** : Apprendre à rendre une variable persistante d’une session à l’autre.

**Consignes** :  
1. Ouvrez le fichier `~/.bashrc` avec un éditeur de texte.  
2. Ajoutez à la fin du fichier la ligne suivante :

```bash
export COURS_LINUX=Oui
```

3. Rechargez le fichier avec :

```bash
source ~/.bashrc
```

4. Vérifiez que la variable est bien définie avec `echo $COURS_LINUX`.  
5. Fermez puis rouvrez le terminal et vérifiez à nouveau.

**Question** :  
- Pourquoi utilise-t-on `.bashrc` pour rendre cette variable permanente dans le shell Bash ?

<br/>

# **Tableau récapitulatif des commandes à retenir**

| Action                                 | Commande                              |
|----------------------------------------|----------------------------------------|
| Déclarer une variable locale           | `nom=valeur`                           |
| Rendre une variable globale            | `export nom`                           |
| Voir les variables locales             | `set`                                  |
| Voir les variables d’environnement     | `env` ou `printenv`                    |
| Supprimer une variable                 | `unset nom`                            |
| Définir une variable permanente        | Modifier `~/.bashrc` + `source`        |
