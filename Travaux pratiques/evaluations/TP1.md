# 🛡️ **TP – Gestion des Permissions et Personnalisation d’Environnement Linux**

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

---

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

---

### **Résultat attendu :**

- Chaque fichier a le propriétaire assigné correctement.
- Les permissions (`r`, `w`, `x`) sont appliquées exactement comme demandé.
- La commande `ls -l` sur chaque fichier doit montrer ces droits.

---

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

---

### **Objectifs :**
- Vérifier que les droits d'accès fonctionnent comme prévu
- Documenter les résultats observés pour chaque test
- Identifier les succès ou les erreurs de permission

---

### **Commandes à compléter :**

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


Donc pour chaque utilisateur, testez les actions suivantes **avec `su nom_utilisateur -c 'commande'`** :  
- Est-ce que vous confirmer l'exactitude des informations dans cette table ?

| Action                                     | Utilisateur | Résultat Attendu |
|--------------------------------------------|-------------|------------------|
| Lire `file1.txt`                           | Albert      | ✅                |
| Écrire dans `file2.txt`                    | Albert      | ❌ (Permission refusée) |
| Modifier `file2.txt`                       | Bernard     | ✅                |
| Exécuter `script1.sh`                      | Bernard     | ✅                |
| Modifier `script2.sh`                      | Claude      | ✅                |
| Exécuter `script1.sh`                      | Claude      | ❌                |
| Lire/écrire partout                        | Danielle    | ✅ (tous les accès) |


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














---

## **5. 🎨 Personnalisation `.bashrc` – Créativité obligatoire !**

**Tâche :** Créez pour chaque utilisateur un `.bashrc` qui s’affiche à la connexion avec un message personnalisé en ASCII art et une touche humoristique.

**Outils possibles** :
- `figlet`
- `fortune`
- `cowsay`

👉 **Seul Albert est fait pour vous en exemple :**
```bash
# Pour Albert
echo 'figlet "Bienvenue Albert!"' >> /home/albert/.bashrc
```

**Résultat attendu :**
Quand on fait :
```bash
su - albert
```

On voit :
```
 _____ _                        _       
| __  | |_ ___ ___ ___ ___ ___| |_ ___ 
| __ -|   | .'|  _| . | . | -_|  _|_ -|
|_____|_|_|__,|___|  _|  _|___|_| |___|
                 |_| |_|              
```

### 🔍 Votre mission :
- Inventez un message et un comportement différent pour chaque utilisateur.  
- Claude pourrait avoir une date du jour.
- Bernard un proverbe (avec `fortune`).
- Danielle un logo de son rôle.

---

## **6. Questions de réflexion à inclure dans le rapport**

1. Quelle différence entre `chmod a+x` et `chmod 755` ?
2. Pourquoi restreindre les droits d’exécution à certains utilisateurs ?
3. Que se passe-t-il si un fichier a les droits `777` dans un environnement sensible ?
4. À quoi sert le `sticky bit` dans un répertoire comme `/tmp` ?
5. Comment éviter qu’un utilisateur devienne accidentellement propriétaire d’un fichier critique ?

---

## ✅ Évaluation (20 points)

| Critère                                      | Points |
|---------------------------------------------|--------|
| Création des utilisateurs et arborescence   | 4      |
| Permissions bien appliquées                 | 4      |
| Tests fonctionnels réalisés avec captures   | 4      |
| `.bashrc` personnalisés et originaux        | 4      |
| Qualité du rapport et réponses aux questions| 4      |
