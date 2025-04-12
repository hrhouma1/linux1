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



# Exercice 8 - QCM


## **QCM 1 : Déclaration de variables**

**Question** :  
Laquelle des instructions suivantes permet de **déclarer correctement** une variable locale appelée `ville` avec la valeur `Paris` ?

a) `ville = Paris`  
b) `ville=Paris`  
c) `export ville Paris`  
d) `set ville=Paris`

**Bonne réponse** : **b**

### Justification :  
En Bash, on déclare une variable locale avec **`nom=valeur`** sans **aucun espace** autour du `=`. Les espaces sont interdits.

### Démonstration :

```bash
ville = Paris     # Donne une erreur
ville=Paris       # Correct
echo $ville       # Affiche : Paris
```



## **QCM 2 : Syntaxe interdite**

**Question** :  
Pourquoi l’instruction `prenom = Alice` est incorrecte en Bash ?

a) Il manque le mot-clé `var`  
b) Le nom de la variable doit être en majuscule  
c) Il y a un espace autour du `=`  
d) Les variables ne peuvent pas contenir de chaînes de caractères

**Bonne réponse** : **c**

### Justification :  
Un espace autour du `=` invalide l’affectation, car Bash interprète `prenom` comme une commande, et `=` comme un argument.

### Démonstration :

```bash
prenom = Alice    # Bash : commande inconnue « prenom »
prenom=Alice      # Fonctionne
echo $prenom      # Affiche : Alice
```



## **QCM 3 : Visibilité des variables**

**Question** :  
Après avoir défini une variable avec `prenom=Marie`, que se passe-t-il si on ouvre un **nouveau shell** avec `bash` ?

a) La variable est automatiquement transmise au nouveau shell  
b) La variable est visible uniquement si elle est exportée  
c) Toutes les variables sont réinitialisées par défaut  
d) Le nouveau shell demande la redéfinition manuelle

**Bonne réponse** : **b**

### Justification :  
Une variable locale est visible uniquement dans le shell courant. Pour qu’elle soit **héritée par les shells enfants**, elle doit être exportée.

### Démonstration :

```bash
prenom=Marie
bash
echo $prenom       # Rien ne s’affiche

exit
export prenom=Marie
bash
echo $prenom       # Affiche : Marie
```



## **QCM 4 : Rendre une variable globale**

**Question** :  
Quelle commande permet de rendre la variable `universite` visible dans tous les processus enfants du shell courant ?

a) `set universite=UdeM`  
b) `declare universite=UdeM`  
c) `universite=UdeM export`  
d) `export universite=UdeM`

**Bonne réponse** : **d**

### Justification :  
Seule la commande `export` permet de rendre une variable accessible dans les **processus enfants**. L’ordre est correct : `export NOM=valeur`.



## **QCM 5 : Suppression d'une variable**

**Question** :  
Quelle commande permet de **supprimer une variable** du shell courant ?

a) `delete variable`  
b) `export variable=undefined`  
c) `unset variable`  
d) `remove variable`

**Bonne réponse** : **c**

### Justification :  
La commande `unset` est la commande standard pour supprimer une variable, qu'elle soit locale ou exportée.

### Démonstration :

```bash
prenom=Sara
unset prenom
echo $prenom        # Rien ne s'affiche
```



## **QCM 6 : Exploration de l’environnement**

**Question** :  
La commande `env | grep LANG` permet de :

a) Supprimer les variables contenant le mot `LANG`  
b) Rechercher et afficher les variables contenant le mot `LANG`  
c) Modifier les variables de langue  
d) Lister les variables locales

**Bonne réponse** : **b**

### Justification :  
La commande `env` liste toutes les variables d’environnement. En la combinant avec `grep`, on peut filtrer l’affichage selon un mot-clé.

### Démonstration :

```bash
env | grep LANG
# Exemple de sortie : LANG=fr_CA.UTF-8
```



## **QCM 7 : Commandes de visualisation**

**Question** :  
Quelle commande permet de **voir toutes les variables d’environnement** uniquement ?

a) `printenv`  
b) `set`  
c) `alias`  
d) `ls /etc/environment`

**Bonne réponse** : **a**

### Justification :  
`printenv` affiche uniquement les variables d’environnement.  
`set` affiche toutes les variables locales **et** d’environnement **et** fonctions définies.



## **QCM 8 : Variable persistante**

**Question** :  
Pour rendre une variable d’environnement **permanente**, que faut-il faire ?

a) L’ajouter dans un script Bash exécuté à chaque démarrage  
b) L’écrire dans `/etc/shadow`  
c) Utiliser `unset` au démarrage  
d) Exécuter `bash` avec l’option `--keep-vars`

**Bonne réponse** : **a**

### Justification :  
Les variables d’environnement permanentes doivent être ajoutées dans un fichier lu automatiquement à l’ouverture d’un terminal : `.bashrc`, `.profile`, ou `/etc/environment`.



## **QCM 9 : Variable système `PATH`**

**Question** :  
Quel est le rôle de la variable `PATH` dans un système Linux ?

a) Elle indique le répertoire personnel de l’utilisateur  
b) Elle contient les options du terminal courant  
c) Elle définit les chemins où chercher les exécutables  
d) Elle stocke les préférences linguistiques

**Bonne réponse** : **c**

### Justification :  
`PATH` est une variable cruciale : elle indique où chercher les programmes lorsqu’une commande est tapée dans le terminal.

### Démonstration :

```bash
echo $PATH
# Affiche une liste de chemins séparés par des `:`
```



## **QCM 10 : Effet du `export`**

**Question** :  
Après avoir écrit `export VAR=test`, que se passe-t-il ?

a) La variable est supprimée  
b) La variable est uniquement accessible dans le shell actuel  
c) La variable est accessible dans les shells enfants  
d) La variable est enregistrée définitivement

**Bonne réponse** : **c**

### Justification :  
`export` rend une variable visible par tous les processus enfants du shell actuel, mais **pas de manière permanente**. Pour la persistance, il faut modifier `.bashrc`.




# 9 - **Tableau récapitulatif des commandes à retenir**

| Action                                 | Commande                              |
|----------------------------------------|----------------------------------------|
| Déclarer une variable locale           | `nom=valeur`                           |
| Rendre une variable globale            | `export nom`                           |
| Voir les variables locales             | `set`                                  |
| Voir les variables d’environnement     | `env` ou `printenv`                    |
| Supprimer une variable                 | `unset nom`                            |
| Définir une variable permanente        | Modifier `~/.bashrc` + `source`        |




# 11 - Correction Exercice 1



### Étapes de réalisation

#### 1. Création de la variable `ville` et affectation de la valeur `Montreal`

```bash
ville=Montreal
```

#### 2. Affichage du contenu de la variable

```bash
echo $ville
```

**Résultat attendu :**

```
Montreal
```

#### 3. Lancement d’un nouveau shell avec la commande `bash`

```bash
bash
```

#### 4. Tentative d’affichage à nouveau de la variable

```bash
echo $ville
```

**Résultat obtenu :**  
Rien ne s’affiche, la variable n’existe plus dans ce nouveau shell.



## Réponses aux questions
### 1. La variable est-elle encore disponible dans le nouveau shell ?
**Réponse :** Non, la variable `ville` n’est plus disponible dans le nouveau shell.
### 2. Pourquoi ne l’est-elle plus ?
**Réponse :** La variable a été définie dans le shell initial, et elle n’a pas été exportée. En Bash, une variable déclarée sans `export` est **locale au shell courant**. Lorsqu’un nouveau shell est lancé (dans ce cas via la commande `bash`), il s’agit d’un **sous-shell**, qui ne connaît que les variables **exportées** du shell parent. Les variables locales ne sont pas transmises automatiquement.
## Explication complémentaire

Si l’on souhaite rendre une variable accessible dans les sous-shells, il faut utiliser la commande `export` :

```bash
export ville=Montreal
```

Ainsi, après avoir lancé un nouveau shell avec `bash`, la commande `echo $ville` donnera :

```
Montreal
```

## Conclusion
- Les variables locales ne sont visibles que dans le shell où elles sont définies. Pour qu’une variable soit accessible dans les sous-shells, elle doit être exportée avec la commande `export`.



<br/>

# 12 - Correction Exercice 2 - De la variable locale à la variable d’environnement  

**Objectif : Comprendre le rôle de la commande `export`.**



### Étapes de réalisation

#### 1. Création d’une variable appelée `universite` avec la valeur `UdeM`

```bash
universite=UdeM
```

#### 2. Transformation de cette variable en variable d’environnement (globale)

```bash
export universite
```

> À noter : on peut également faire directement :  
> `export universite=UdeM`

#### 3. Lancement d’un nouveau shell avec la commande `bash`

```bash
bash
```

#### 4. Affichage de la variable dans le nouveau shell

```bash
echo $universite
```

**Résultat attendu :**

```
UdeM
```



## Réponses aux questions

### 1. Quelle est la différence entre une variable locale et une variable d’environnement ?

**Réponse :**  
Une **variable locale** est visible uniquement dans le shell dans lequel elle a été déclarée. Elle n’est pas transmise aux processus enfants.  
Une **variable d’environnement** (ou variable exportée) est une variable qui est transmise aux processus enfants du shell (par exemple, un sous-shell lancé avec `bash`).

Autrement dit :

- **Variable locale** : valable uniquement dans le shell courant.
- **Variable d’environnement** : valable dans le shell courant **et** tous les sous-shells lancés à partir de celui-ci.



### 2. Pourquoi `export` est-il nécessaire ici ?

**Réponse :**  
La commande `export` est nécessaire pour transformer une variable locale en **variable d’environnement**, c’est-à-dire une variable qui sera **héritée par tous les sous-processus** (sous-shells, scripts, etc.).

Sans `export`, la variable ne serait visible que dans le shell courant et disparaîtrait dès qu’un nouveau shell est ouvert.


## Conclusion

La commande `export` joue un rôle fondamental pour rendre une variable **persistante dans les sous-shells**. Elle permet de passer d’une portée **locale** à une portée **globale** dans le contexte de l’environnement du shell. Cela est essentiel pour les scripts, les configurations système et le passage de paramètres à des programmes.


<br/>
<br/>





# Exercice 3 : Exploration des variables existantes  

**Objectif : Observer les variables d’environnement déjà présentes dans le système.**



### Étapes de réalisation

#### 1. Afficher toutes les variables d’environnement avec la commande `env`

```bash
env
```

> Cette commande affiche toutes les **variables d’environnement** actuellement exportées dans le shell.



#### 2. Filtrer les variables liées à la langue avec la commande :

```bash
env | grep LANG
```

**Exemple de sortie possible :**

```
LANG=fr_CA.UTF-8
```

> La variable `LANG` indique la **langue** et la **configuration régionale** (locale) utilisée par le système.



#### 3. Afficher les valeurs des variables spécifiques suivantes

```bash
echo $HOME
echo $USER
echo $PWD
echo $SHELL
```

**Exemples de résultats :**

- `$HOME` → `/home/utilisateur`  
- `$USER` → `utilisateur`  
- `$PWD` → `/home/utilisateur` (répertoire courant)  
- `$SHELL` → `/bin/bash` (ou autre selon le shell utilisé)



## Réponses aux questions

### 1. Quelle est la différence entre `env` et `set` ?

**Réponse :**

- `env` affiche **uniquement les variables d’environnement**, c’est-à-dire celles qui sont exportées et disponibles pour les sous-processus.  
- `set` affiche **toutes les variables** du shell, y compris :
  - les **variables locales**
  - les **fonctions définies**
  - les **variables d’environnement**
  - les **paramètres spéciaux**

> En résumé :  
> `env` → variables d’environnement exportées  
> `set` → tout le contenu du shell (environnement + variables locales + fonctions)



### 2. À quoi servent les variables affichées ?

**Réponse :**

- **`HOME`** : répertoire personnel de l’utilisateur. C’est là que le shell se place par défaut au démarrage.
- **`USER`** : identifiant de l’utilisateur en cours. Utile dans les scripts et les authentifications.
- **`PWD`** : répertoire courant (Print Working Directory). Il reflète l’endroit où l’on se trouve dans l’arborescence du système de fichiers.
- **`SHELL`** : chemin absolu vers l’interpréteur de commandes utilisé (par exemple `/bin/bash`). Cela permet de savoir quel shell est utilisé.



## Conclusion

Cet exercice montre comment consulter l’état actuel de l’environnement du shell. Il permet de distinguer les **variables locales** des **variables d’environnement**, et de comprendre à quoi servent les variables système essentielles. Cela constitue une base importante pour l’écriture de scripts ou la configuration d’un environnement utilisateur.

