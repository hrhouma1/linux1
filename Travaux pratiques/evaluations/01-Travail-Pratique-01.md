# **TP 1 – Gestion des Permissions et Personnalisation d’Environnement Linux**

# **Objectif général :**
Comprendre, appliquer et tester les **permissions Unix** (`chmod`, `chown`, `su`) dans un environnement multi-utilisateurs.  
Apprendre à personnaliser un environnement utilisateur avec `.bashrc`.



# **Durée :** 1 semaine  
## **Livrable :**  
- Un **rapport PDF ou Word** avec captures d’écran, explications et réflexions.  
- Scripts ou commandes utilisées (dans le rapport et dans un fichier `.sh` si applicable).

# **Contexte**
Vous êtes administrateur d’un serveur Linux utilisé par 4 collègues : **Albert, Bernard, Claude et Danielle**.  
Vous devez :
- Créer les utilisateurs : **Albert, Bernard, Claude et Danielle**. 
- Préparer un espace de travail commun.
- Attribuer des permissions spécifiques.
- Tester les actions attendues.
- Ajouter une touche personnelle à leur environnement avec des messages de bienvenue animés.


#  **Étapes du TP**










<br/>
<br/>

# **PARTIE 1 – Donner les droits administrateur à l’utilisateur `eleve`**



### **Contexte :**  
Dans ce TP, vous travaillez principalement avec l’utilisateur `eleve`. Il est possible que cet utilisateur ait déjà été créé lors de l’installation d’Ubuntu 22.04. Si ce n’est pas le cas, vous devez le créer manuellement.



### **Tâche :**

Créer l’utilisateur **eleve** avec le mot de passe **eleve**  
(**si ce n’est pas déjà fait lors de l’installation de Ubuntu 22.04**).  
Puis, ajoutez cet utilisateur au groupe **sudo** afin qu’il puisse exécuter des commandes administratives.



### **Commandes à compléter :**

```bash
Commande 1.1 : ___________________________________________   # Créer l’utilisateur eleve avec le mot de passe eleve (si nécessaire)
Commande 1.2 : ___________________________________________   # Ajouter l’utilisateur eleve au groupe sudo
Commande 1.3 : ___________________________________________   # Ouvrir le fichier sudoers avec l’outil sécurisé
Commande 1.4 : ___________________________________________   # Vérifier ou ajouter la ligne autorisant eleve à utiliser sudo
Commande 1.5 : ___________________________________________   # Tester une commande avec sudo (ex : sudo ls /root)
```



### **Résultat attendu :**

- L’utilisateur `eleve` existe et peut se connecter avec son mot de passe
- Il peut exécuter des commandes avec `sudo` (mot de passe requis)
- Aucun message d’erreur à l’exécution d’une commande `sudo`


### **Captures d’écran exigées :**

1. Création de l’utilisateur `eleve` (si nécessaire)
2. Ajout au groupe sudo
3. Contenu du fichier sudoers modifié ou vérifié
4. Test réussi d’une commande avec `sudo`






<br/>
<br/>

# **PARTIE 2 – Création des utilisateurs et modification de leurs mots de passe**



### **Contexte :**  
Vous êtes maintenant connecté en tant que `eleve` ou `root`, qui disposent des droits `sudo`. Vous devez :  
- Créer quatre utilisateurs : `albert`, `bernard`, `claude`, `danielle`  
- Définir un mot de passe initial lors de la création (notez-le)  
- Vérifier que leurs répertoires personnels sont créés  
- Tester la connexion avec `su -`  
- Modifier leurs mots de passe **à partir de votre compte `eleve`**



### **Commandes à compléter :**

```bash
Commande 2.1 : ___________________________________________   # Créer l’utilisateur albert (indiquez le mot de passe utilisé)
Commande 2.2 : ___________________________________________   # Créer l’utilisateur bernard (mot de passe à noter)
Commande 2.3 : ___________________________________________   # Créer l’utilisateur claude (mot de passe à noter)
Commande 2.4 : ___________________________________________   # Créer l’utilisateur danielle (mot de passe à noter)
Commande 2.5 : ___________________________________________   # Afficher les répertoires dans /home pour vérifier les créations
Commande 2.6 : ___________________________________________   # Se connecter avec su - albert, puis revenir à eleve
Commande 2.7 : ___________________________________________   # Modifier le mot de passe de l’utilisateur albert à partir de eleve
Commande 2.8 : ___________________________________________   # Modifier le mot de passe de l’utilisateur bernard à partir de eleve
Commande 2.9 : ___________________________________________   # Modifier le mot de passe de l’utilisateur claude à partir de eleve
Commande 2.10 : __________________________________________  # Modifier le mot de passe de l’utilisateur danielle à partir de eleve
```



### **Résultat attendu :**

- Les répertoires des utilisateurs apparaissent dans `/home`
- `su - albert` permet de se connecter à sa session
- Tous les mots de passe ont été modifiés avec succès depuis `eleve`



### **Captures d’écran exigées :**

1. Sortie de la commande `ls /home`
2. Connexion réussie à la session de `albert` puis retour
3. Modifications des mots de passe des 4 utilisateurs




<br/>
<br/>

# **PARTIE 3 – Ajout de Danielle et Claude à sudoers**


### **Contexte :**  
Vous êtes connecté avec l’utilisateur `eleve` (ou `root`) qui possède les droits `sudo`.  
Vous devez désormais accorder les **droits administrateurs** à **Danielle** et **Claude**, en les ajoutant au fichier `sudoers`.

Cela leur permettra d’exécuter des commandes critiques avec `sudo`, comme des mises à jour système, des modifications de comptes utilisateurs, ou encore la gestion de fichiers système.



### **Objectifs :**
- Ajouter **claude** et **danielle** au fichier `sudoers`
- Vérifier qu’ils peuvent tous deux exécuter des commandes avec `sudo`





### **Commandes à compléter :**

```bash
Commande 3.1 : ___________________________________________   # Ouvrir le fichier sudoers avec visudo
Commande 3.2 : ___________________________________________   # Ajouter une ligne autorisant claude à exécuter des commandes sudo
Commande 3.3 : ___________________________________________   # Ajouter une ligne autorisant danielle à exécuter des commandes sudo
Commande 3.4 : ___________________________________________   # Vérifier que claude appartient au groupe sudo OU est listé dans /etc/sudoers
Commande 3.5 : ___________________________________________   # Vérifier que danielle appartient au groupe sudo OU est listée dans /etc/sudoers
Commande 3.6 : ___________________________________________   # Se connecter avec l’utilisateur claude
Commande 3.7 : ___________________________________________   # Exécuter une commande avec sudo (ex : sudo whoami) pour tester
Commande 3.8 : ___________________________________________   # Se connecter avec l’utilisateur danielle
Commande 3.9 : ___________________________________________   # Exécuter une commande avec sudo pour tester
```



### **Résultat attendu :**

- Claude et Danielle figurent dans le fichier `/etc/sudoers`, ou font partie d’un groupe (ex. `sudo`) leur accordant les droits d’administration.
- En testant une commande avec `sudo`, ils sont invités à entrer leur mot de passe, puis la commande s'exécute correctement.
- Aucun message d’erreur lié à des permissions insuffisantes ne doit apparaître.



### **Captures d’écran exigées :**

1. Affichage des lignes ajoutées dans le fichier `sudoers`
2. Exécution réussie d'une commande avec `sudo` pour Claude
3. Exécution réussie d'une commande avec `sudo` pour Danielle


<br/>
<br/>

# **PARTIE 4 – Préparation d’un répertoire partagé**



### **Contexte :**  
Vous devez créer un répertoire partagé situé dans `/home/shared_data/`, avec une structure précise de fichiers et sous-dossiers.  
Deux scripts `.sh` doivent également être créés avec un message affiché lors de l’exécution.

### **Utilisateur attendu pour cette partie :**
Cette partie doit être réalisée **en tant que root** ou **en tant qu'utilisateur `eleve` avec les droits `sudo`**.

Utilisez donc la commande `sudo` devant chaque commande si vous êtes connecté avec `eleve`.



### **Objectifs :**
- Créer l’arborescence complète dans `/home/shared_data`
- Rédiger le contenu de deux scripts simples
- Vérifier la bonne création des fichiers et dossiers



### **Structure attendue :**

```
/home/shared_data/
├── file1.txt
├── file2.txt
└── dossier1/
    ├── script1.sh
    └── script2.sh
```



### **Contenu attendu pour `script1.sh` :**

```bash
#!/bin/bash
echo "Bonjour depuis le script 1!"
```

### **Contenu attendu pour `script2.sh` :**

```bash
#!/bin/bash
echo "Bonjour depuis le script 2!"
```



### **Commandes à compléter :**

```bash
Commande 4.1 : ___________________________________________   # Créer le dossier /home/shared_data
Commande 4.2 : ___________________________________________   # Créer le fichier file1.txt
Commande 4.3 : ___________________________________________   # Créer le fichier file2.txt
Commande 4.4 : ___________________________________________   # Créer le sous-dossier dossier1
Commande 4.5 : ___________________________________________   # Créer le fichier script1.sh dans dossier1
Commande 4.6 : ___________________________________________   # Créer le fichier script2.sh dans dossier1
Commande 4.7 : ___________________________________________   # Écrire le contenu dans script1.sh
Commande 4.8 : ___________________________________________   # Écrire le contenu dans script2.sh
Commande 4.9 : ___________________________________________   # Vérifier toute la structure avec ls -R ou tree
```



### **Résultat attendu :**

- Le répertoire `/home/shared_data/` contient bien deux fichiers texte et un sous-dossier `dossier1/`.
- Le sous-dossier contient deux scripts `.sh` avec un contenu correct.
- La commande `ls -R` (ou `tree`) reflète exactement la structure attendue.


### **Captures d’écran exigées :**

1. Sortie de la commande `ls -R /home/shared_data` ou `tree /home/shared_data`
2. Affichage du contenu de `script1.sh` et `script2.sh` avec `cat`




## **PARTIE 5 – Attribution des permissions**

---

### **Contexte :**  
Vous avez créé une structure de fichiers partagée dans `/home/shared_data`.  
Vous devez maintenant attribuer des propriétaires et des permissions précises à chaque élément pour garantir un accès contrôlé.

---

### **Utilisateur attendu :**  
Cette partie doit être réalisée **en tant que root** ou **avec l’utilisateur `eleve` disposant des droits `sudo`**.  
Toutes les commandes de cette section doivent donc être exécutées avec les privilèges nécessaires.

---

### **Objectifs :**
Attribuer les **propriétaires** et les **permissions** suivantes :

| Fichier / Dossier                      | Propriétaire     | Permissions attendues                         |
|----------------------------------------|------------------|-----------------------------------------------|
| `/home/shared_data/file1.txt`          | albert           | Lecture/écriture pour lui seulement           |
| `/home/shared_data/file2.txt`          | bernard          | Lecture/écriture pour lui uniquement          |
| `/home/shared_data/dossier1/script1.sh`| bernard          | Exécutable uniquement par Bernard             |
| `/home/shared_data/dossier1/script2.sh`| claude           | Modifiable (lecture/écriture) mais non exécutable |
| `/home/shared_data/`                  | danielle         | Contrôle total sur tout le dossier (accès complet) |



### **Commandes à compléter :**

```bash
Commande 5.1 : ___________________________________________   # Changer le propriétaire de file1.txt à albert
Commande 5.2 : ___________________________________________   # Appliquer les permissions lecture/écriture pour albert uniquement
Commande 5.3 : ___________________________________________   # Changer le propriétaire de file2.txt à bernard
Commande 5.4 : ___________________________________________   # Appliquer les permissions lecture/écriture pour bernard uniquement
Commande 5.5 : ___________________________________________   # Changer le propriétaire de script1.sh à bernard
Commande 5.6 : ___________________________________________   # Appliquer les permissions exécution seule pour le propriétaire
Commande 5.7 : ___________________________________________   # Changer le propriétaire de script2.sh à claude
Commande 5.8 : ___________________________________________   # Appliquer les permissions lecture/écriture sans exécution
Commande 5.9 : ___________________________________________   # Changer récursivement le propriétaire du dossier shared_data à danielle
Commande 5.10 : __________________________________________  # Donner à danielle tous les droits sur tous les fichiers du dossier
Commande 5.11 : __________________________________________  # Vérifier les permissions avec ls -l /home/shared_data et sous-dossiers
```



### **Résultat attendu :**

- Chaque fichier a le propriétaire assigné correctement.
- Les permissions (`r`, `w`, `x`) sont appliquées exactement comme demandé.
- La commande `ls -l` sur chaque fichier doit montrer ces droits.



### **Captures d’écran exigées :**

1. Sortie de la commande `ls -l /home/shared_data/`
2. Sortie de la commande `ls -l /home/shared_data/dossier1/`
3. Toute autre preuve utile que vous jugez pertinente







<br/>
<br/>




## **PARTIE 6 – Table de tests d’accès par utilisateur**

---

### **Contexte :**  
Les permissions ont été attribuées. Il est maintenant temps de **tester les accès** de chaque utilisateur sur les fichiers et scripts du répertoire `/home/shared_data`.

Pour chaque test, vous devez vous connecter **temporairement** avec un utilisateur via la commande :

```bash
su -c 'commande à exécuter' nom_utilisateur
```



### **Objectifs :**
- Vérifier que les droits d'accès fonctionnent comme prévu
- Documenter les résultats observés pour chaque test
- Identifier les succès ou les erreurs de permission


### **Commandes à compléter :**


Donc pour chaque utilisateur, testez les actions suivantes **avec `su nom_utilisateur -c 'commande'`** :  


```bash
Commande 6.1 : ___________________________________________   # Albert tente de lire file1.txt
Commande 6.2 : ___________________________________________   # Albert tente d’écrire dans file1.txt
Commande 6.3 : ___________________________________________   # Albert tente d’écrire dans file2.txt
Commande 6.4 : ___________________________________________   # Bernard écrit dans file2.txt
Commande 6.5 : ___________________________________________   # Bernard exécute script1.sh
Commande 6.6 : ___________________________________________   # Claude modifie script2.sh (ex : ajoute une ligne)
Commande 6.7 : ___________________________________________   # Claude tente d’exécuter script1.sh
Commande 6.8 : ___________________________________________   # Danielle modifie file1.txt
Commande 6.9 : ___________________________________________   # Danielle modifie script2.sh
```


- Est-ce que vous confirmer l'exactitude des informations dans cette table ?

### **Résultat attendu pour chaque commande :**

| **Test**                                            | **Résultat Attendu**       |
|-----------------------------------------------------|-----------------------------|
| Albert lit file1.txt                                | Succès                      |
| Albert écrit dans file1.txt                         | Succès                      |
| Albert écrit dans file2.txt                         | Échec (permission refusée)  |
| Bernard écrit dans file2.txt                        | Succès                      |
| Bernard exécute script1.sh                          | Succès                      |
| Claude modifie script2.sh                           | Succès                      |
| Claude exécute script1.sh                           | Échec (permission refusée)  |
| Danielle modifie file1.txt                          | Succès                      |
| Danielle modifie script2.sh                         | Succès                      |




### **Captures d’écran exigées :**

- Exécution de **chaque test** avec `su -c`
- Messages de succès ou d'erreur affichés par le système
- (Par exemple : `Permission denied` ou affichage du contenu généré)







<br/>
<br/>


## **PARTIE 7 – Personnalisation de l’environnement `.bashrc`**



### **Contexte :**  
Chaque utilisateur de votre système doit avoir un **message personnalisé** ou un **effet visuel** qui s’affiche lorsqu’il ouvre une session (connexion avec `su - nom_utilisateur`).  
Cela se fait en modifiant le fichier **`.bashrc`** présent dans le répertoire personnel de chaque utilisateur.

Vous allez modifier ce fichier pour chacun des 4 utilisateurs créés.



### **Objectifs :**
- Ajouter du contenu personnalisé dans le `.bashrc` de chaque utilisateur
- Utiliser au moins une des commandes suivantes :
  - `figlet` (affichage ASCII stylisé)
  - `cowsay` (message dans une bulle avec vache)
  - `fortune` (citation aléatoire)
  - `echo` (message simple)
- Vérifier que cela fonctionne dès la connexion à la session



### **Commandes à compléter :**

```bash
Commande 7.1 : ___________________________________________   # Ajouter un message personnalisé dans le .bashrc d’albert
Commande 7.2 : ___________________________________________   # Ajouter un message personnalisé dans le .bashrc de bernard
Commande 7.3 : ___________________________________________   # Ajouter un message personnalisé dans le .bashrc de claude
Commande 7.4 : ___________________________________________   # Ajouter un message personnalisé dans le .bashrc de danielle
Commande 7.5 : ___________________________________________   # Tester l'affichage du message pour albert
Commande 7.6 : ___________________________________________   # Tester l'affichage du message pour bernard
Commande 7.7 : ___________________________________________   # Tester l'affichage du message pour claude
Commande 7.8 : ___________________________________________   # Tester l'affichage du message pour danielle
```



### **Résultat attendu :**

- Lorsqu’un utilisateur se connecte avec `su - nom`, un message s'affiche automatiquement (ASCII, citation, message, ou effet visuel).
- Les messages sont différents d’un utilisateur à l’autre.
- Les effets sont visibles directement dans le terminal après la connexion.



### **Captures d’écran exigées :**

1. Affichage du message `.bashrc` lors de la connexion pour chaque utilisateur
2. Contenu de chaque fichier `.bashrc` modifié (`cat /home/nom/.bashrc` ou `nano`)



### **Exemple autorisé (pour Albert uniquement) :**

```bash
echo 'figlet "Bienvenue Albert!"' >> /home/albert/.bashrc
```

Pour les autres, **vous devez inventer quelque chose d’unique**.







<br/>
<br/>
<br/>




## **PARTIE 8 – Questions de réflexion finales**



### **Contexte :**  
Vous avez maintenant :
- Créé et configuré plusieurs utilisateurs
- Géré des fichiers et des permissions spécifiques
- Modifié des environnements utilisateur via `.bashrc`
- Vérifié les accès et exécuté des scripts selon les droits

Il est temps de prendre du recul et de réfléchir aux **impacts, risques et bonnes pratiques** liés à ce que vous avez mis en place.



### **Instructions :**

Répondez **sérieusement** aux questions suivantes. Vous devez **argumenter vos réponses**. Un copier-coller de commande ne suffit pas. Utilisez des exemples lorsque pertinent.

**Votre réponse peut être écrite directement sous chaque question dans le rapport à remettre.**



### **Questions à compléter :**

<br/>

**1. Quelle est l'importance de bien configurer les permissions sur un système Linux multi-utilisateur ?**  
(Expliquez ce qui pourrait arriver si tous les fichiers avaient des permissions 777.)

Réponse :  
...............................................................................................................................  
...............................................................................................................................  
...............................................................................................................................

<br/>

**2. Quelle est la différence entre les permissions d'exécution (`x`) et les permissions de lecture (`r`) ? Donnez un exemple concret.**

Réponse :  
...............................................................................................................................  
...............................................................................................................................  
...............................................................................................................................

<br/>

**3. Quelle commande permet de changer les permissions ? Quelle commande permet de changer le propriétaire ? Donnez un exemple concret d’utilisation correcte de chaque.**

Réponse :  
...............................................................................................................................  
...............................................................................................................................  
...............................................................................................................................

<br/>

**4. Quelle est la différence entre l’appartenance à un groupe (`sudo`) et le fait d’être explicitement listé dans le fichier `/etc/sudoers` ?**

Réponse :  
...............................................................................................................................  
...............................................................................................................................  
...............................................................................................................................

<br/>

**5. Pourquoi est-il déconseillé de donner des droits `sudo` à tous les utilisateurs d’un système ? Quels seraient les risques ?**

Réponse :  
...............................................................................................................................  
...............................................................................................................................  
...............................................................................................................................

<br/>

**6. Que se passerait-il si un utilisateur pouvait modifier un script qui est exécuté par un administrateur ? Donnez un exemple de faille.**

Réponse :  
...............................................................................................................................  
...............................................................................................................................  
...............................................................................................................................

<br/>

**7. Quelle est l’utilité de personnaliser le `.bashrc` ? Peut-on en faire un outil d’ingénierie sociale ou de sécurité ?**

Réponse :  
...............................................................................................................................  
...............................................................................................................................  
...............................................................................................................................




<br/>
<br/>
<br/>




---
# **PARTIE 9**
---

### **Contexte :**  
Vous allez mener une série de tests pour comprendre :

- Comment fonctionnent les variables locales et exportées
- Quelle est leur visibilité entre utilisateurs
- Comment rendre une variable **persistante** pour un utilisateur spécifique
- Comment **partager une variable** (ou pas) entre plusieurs utilisateurs



### **Règles à suivre :**

- Travaillez à chaque fois avec un **utilisateur différent** (Albert, Bernard, Claude, Danielle)
- Ne modifiez **que leur propre environnement**
- Ne trichez pas en copiant un `.bashrc` d’un utilisateur à un autre — chaque utilisateur doit être traité **indépendamment**



### **Objectifs :**
- Définir des variables pour chaque utilisateur
- Tester leur visibilité dans un sous-shell
- Vérifier qu’un autre utilisateur **ne peut pas voir** la variable
- Ajouter une variable permanente via le `.bashrc` de chaque utilisateur



## **Scénario 1 – Variables locales et exportées**

### **Étapes à suivre pour chaque utilisateur :**

(Commencez par Albert)

```bash
Commande B1.1 : ___________________________________________   # Se connecter avec su - albert
Commande B1.2 : ___________________________________________   # Créer une variable locale nom="Albert"
Commande B1.3 : ___________________________________________   # Afficher la variable
Commande B1.4 : ___________________________________________   # Lancer un sous-shell avec bash
Commande B1.5 : ___________________________________________   # Vérifier si la variable est encore visible
Commande B1.6 : ___________________________________________   # Exporter la variable nom
Commande B1.7 : ___________________________________________   # Lancer un nouveau shell
Commande B1.8 : ___________________________________________   # Vérifier que la variable est visible dans ce sous-shell
Commande B1.9 : ___________________________________________   # Quitter et repasser à eleve
```

Répétez ces étapes pour Bernard (`nom="Bernard"`), Claude (`nom="Claude"`) et Danielle (`nom="Danielle"`).



### **Résultat attendu :**

- Les variables **non exportées** disparaissent dans un sous-shell
- Les variables **exportées** persistent dans un sous-shell
- Chaque utilisateur **ne peut accéder qu’à ses propres variables**



## **Scénario 2 – Variables permanentes via .bashrc**

### **Étapes à suivre (exemple avec Claude) :**

```bash
Commande B2.1 : ___________________________________________   # Se connecter avec su - claude
Commande B2.2 : ___________________________________________   # Ouvrir le fichier ~/.bashrc
Commande B2.3 : ___________________________________________   # Ajouter export PROJET_CLAUDE="Projet Système"
Commande B2.4 : ___________________________________________   # Recharger ~/.bashrc avec source
Commande B2.5 : ___________________________________________   # Vérifier que la variable est bien définie
Commande B2.6 : ___________________________________________   # Fermer la session et se reconnecter pour tester la persistance
Commande B2.7 : ___________________________________________   # Refaire echo $PROJET_CLAUDE
```

Répétez le même processus avec :

- `PROJET_ALBERT="TP Sécurité"` pour Albert  
- `PROJET_BERNARD="TP Scripts"` pour Bernard  
- `PROJET_DANIELLE="TP Admin"` pour Danielle



### **Résultat attendu :**

- Chaque utilisateur possède une variable **permanente** visible dès la connexion
- Ces variables **ne sont visibles que pour leur utilisateur**
- La commande `echo $PROJET_X` retourne la bonne valeur après redémarrage



## **Questions de réflexion à rédiger dans votre rapport :**

<br/>

**1. Quelle différence constatez-vous entre une variable exportée dans le terminal et une variable ajoutée dans `.bashrc` ?**  
...............................................................................................................................

**2. Est-il possible pour Bernard d'accéder à une variable définie par Albert ? Pourquoi ?**  
...............................................................................................................................

**3. Quelle méthode utiliseriez-vous si vous vouliez partager une même variable entre tous les utilisateurs ? (Piste : `/etc/profile`)**  
...............................................................................................................................

**4. Peut-on supprimer une variable exportée ? Comment ?**  
...............................................................................................................................

**5. À quoi faut-il faire attention lorsque l’on modifie `.bashrc` ? Donnez un exemple d'erreur potentielle.**  
...............................................................................................................................









<br/>
<br/>
<br/>

## **PARTIE 10**



### **Contexte :**  
Vous allez tester si une variable définie (locale ou exportée) par **Albert** est accessible par les autres utilisateurs (**Bernard, Claude, Danielle**).  
Cela vous permettra de comprendre les **limites de portée des variables** dans un système multi-utilisateur Linux.



### **Objectifs :**
- Définir une variable exportée avec un utilisateur
- Se connecter avec un autre utilisateur
- Tester la portée de la variable
- Comprendre pourquoi elle n’est pas visible
- Trouver **la bonne méthode** pour la rendre visible **pour tous**



### **Étapes à suivre :**

<br/>

```bash
Commande B1.1 : ___________________________________________   # Se connecter avec su - albert
Commande B1.2 : ___________________________________________   # Créer une variable ville=Montreal
Commande B1.3 : ___________________________________________   # Exporter la variable ville
Commande B1.4 : ___________________________________________   # Vérifier qu'elle est bien exportée (echo $ville)
Commande B1.5 : ___________________________________________   # Quitter la session albert (exit)
Commande B1.6 : ___________________________________________   # Se connecter avec su - bernard
Commande B1.7 : ___________________________________________   # Tenter d’afficher la variable $ville
```



### **Résultat attendu :**

- La variable `ville` **existe uniquement pour Albert**, même si elle a été exportée
- Pour Bernard (ou tout autre utilisateur), la variable **n’est pas visible**


### **Question à rédiger dans votre rapport :**

<br/>

**1. Pourquoi la variable exportée par Albert n’est-elle pas visible par Bernard ?**  
...............................................................................................................................

**2. Comment faire pour qu’une variable soit accessible pour tous les utilisateurs automatiquement ?**  
(Piste : où faudrait-il écrire `export ville=Montreal` pour qu’elle soit chargée pour tous les utilisateurs ?)  
...............................................................................................................................



### **Extension facultative (bonus avancé – 2 points supplémentaires)**

Trouvez une manière de **définir une variable commune à tous les utilisateurs**, **sans avoir à la redéclarer pour chacun**.  
Indiquez le **fichier système** à modifier et faites une démonstration avec Albert, Bernard ou Danielle.

### Est-ce que ce tableau est correcte ?


## **Tableau récapitulatif – Portée des variables dans un système Linux**

| **Emplacement / Méthode**            | **Visible pour l’utilisateur courant ?** | **Visible dans un sous-shell ?** | **Visible pour les autres utilisateurs ?** | **Persiste après fermeture de session ?** |
|--------------------------------------|------------------------------------------|----------------------------------|---------------------------------------------|--------------------------------------------|
| `ville=Montreal`                     | ✅                                        | ❌                                | ❌                                           | ❌                                          |
| `export ville=Montreal`             | ✅                                        | ✅                                | ❌                                           | ❌                                          |
| Ajout dans `~/.bashrc`              | ✅                                        | ✅                                | ❌                                           | ✅ (pour cet utilisateur uniquement)       |
| Ajout dans `/etc/profile` ou `/etc/environment` | ✅ pour tous                          | ✅ pour tous                      | ✅                                           | ✅                                          |



## **Conclusion à insérer dans le rapport :**

- Une variable définie dans le terminal est **locale** au shell courant.
- Une variable exportée est **héritable par les sous-shells**, mais **jamais** par d'autres utilisateurs.
- Pour rendre une variable **permanente**, il faut la placer dans un fichier comme `.bashrc`.
- Pour qu’elle soit **accessible à tous les utilisateurs**, il faut l’inscrire dans un fichier **système partagé** comme `/etc/profile`, **avec précaution** (droits root nécessaires).



<br/>
<br/>
<br/>

## **PARTIE Bonus**


## Questions de réflexion à inclure dans le rapport**

1. Quelle différence entre `chmod a+x` et `chmod 755` ?
2. Pourquoi restreindre les droits d’exécution à certains utilisateurs ?
3. Que se passe-t-il si un fichier a les droits `777` dans un environnement sensible ?
4. À quoi sert le `sticky bit` dans un répertoire comme `/tmp` ?
5. Comment éviter qu’un utilisateur devienne accidentellement propriétaire d’un fichier critique ?


