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

---

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




## **2. Préparation d’un répertoire partagé**

**Tâche :** Créez l’arborescence suivante :

```
/home/shared_data/
├── file1.txt
├── file2.txt
└── dossier1/
    ├── script1.sh
    └── script2.sh
```

**Contenu suggéré des scripts (script1.sh, script2.sh)** :
```bash
#!/bin/bash
echo "Bonjour depuis le script!"
```

---

## **3. Attribution des permissions**

**Tâche :** Attribuez les permissions suivantes :

| Fichier / Dossier                      | Propriétaire     | Permissions |
|----------------------------------------|------------------|-------------|
| file1.txt                              | Albert           | lecture/écriture pour lui seulement |
| file2.txt                              | Bernard          | lecture/écriture pour Bernard |
| dossier1/script1.sh                    | Bernard          | exécutable uniquement par Bernard |
| dossier1/script2.sh                    | Claude           | modifiable mais pas exécutable |
| shared_data/                           | Danielle         | contrôle total sur tout le dossier |

**Résultat attendu :** Utilisez `ls -l` pour prouver chaque configuration.  
Incluez les captures d’écran des commandes et de leurs effets.

---

## **4. Table de tests d’accès**

Pour chaque utilisateur, testez les actions suivantes **avec `su nom_utilisateur -c 'commande'`** :  

| Action                                     | Utilisateur | Résultat Attendu |
|--------------------------------------------|-------------|------------------|
| Lire `file1.txt`                           | Albert      | ✅                |
| Écrire dans `file2.txt`                    | Albert      | ❌ (Permission refusée) |
| Modifier `file2.txt`                       | Bernard     | ✅                |
| Exécuter `script1.sh`                      | Bernard     | ✅                |
| Modifier `script2.sh`                      | Claude      | ✅                |
| Exécuter `script1.sh`                      | Claude      | ❌                |
| Lire/écrire partout                        | Danielle    | ✅ (tous les accès) |

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
