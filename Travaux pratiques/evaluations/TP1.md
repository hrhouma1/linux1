# 🛡️ **TP – Gestion des Permissions et Personnalisation d’Environnement Linux**

## 🎯 **Objectif général :**
Comprendre, appliquer et tester les **permissions Unix** (`chmod`, `chown`, `su`) dans un environnement multi-utilisateurs.  
Apprendre à personnaliser un environnement utilisateur avec `.bashrc`.

---

## ⏳ **Durée :** 1 semaine  
## 🧪 **Livrable :**  
- Un **rapport PDF ou Word** avec captures d’écran, explications et réflexions.  
- Scripts ou commandes utilisées (dans le rapport et dans un fichier `.sh` si applicable).

---

## 📦 **Contexte**
Vous êtes administrateur d’un serveur Linux utilisé par 4 collègues : **Albert, Bernard, Claude et Danielle**.  
Vous devez :
- Créer les utilisateurs.
- Préparer un espace de travail commun.
- Attribuer des permissions spécifiques.
- Tester les actions attendues.
- Ajouter une touche personnelle à leur environnement avec des messages de bienvenue animés.

---

# 🧭 **Étapes du TP**

---

## **1. Création des utilisateurs**

**Tâche :** Créez les utilisateurs suivants :  
`albert`, `bernard`, `claude`, `danielle`  

**Résultat attendu :**
```bash
/home/albert
/home/bernard
/home/claude
/home/danielle
```

**Indices :** utilisez `adduser` (ou `useradd` avec options).  
Vous devez vérifier que chaque utilisateur peut se connecter (commande `su`).

---

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
