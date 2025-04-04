## **1. Questions à Choix Multiples (QCM)**  

---

### **1) Quelle commande permet de modifier le propriétaire d'un fichier ou d'un répertoire ?**  
**Réponse : B) `chown`**  

---

#### **Explication :**  
La commande `chown` (abréviation de "Change Owner") est utilisée pour modifier le propriétaire et/ou le groupe associé à un fichier ou un répertoire. Elle est essentielle pour s'assurer que seuls les utilisateurs autorisés peuvent accéder, modifier ou exécuter un fichier.  

---

#### **Syntaxe Générale :**  
```bash
chown [nouveau_propriétaire]:[nouveau_groupe] fichier_ou_dossier
```
- Si seul le propriétaire est spécifié :  
  ```bash
  chown albert fichier.txt
  ```
  Cela attribue le fichier `fichier.txt` à l'utilisateur `albert`.

- Si propriétaire et groupe sont spécifiés :  
  ```bash
  chown albert:collaborateurs fichier.txt
  ```
  Cela attribue le fichier `fichier.txt` à l'utilisateur `albert` et au groupe `collaborateurs`.

- Pour modifier les permissions de manière récursive sur un dossier et son contenu :  
  ```bash
  chown -R albert:collaborateurs /home/shared_data
  ```

---

#### **Scénario d'Utilisation :**  
Un administrateur crée un fichier `report.txt` qui appartient par défaut à l'utilisateur `root`. Pour permettre à un utilisateur nommé `bernard` de modifier ce fichier, l'administrateur doit transférer la propriété avec :  
```bash
chown bernard report.txt
```
---

---

### **2) Quelle est la différence principale entre les commandes `useradd` et `adduser` ?**  
**Réponse : B) `adduser` est un script convivial qui crée un répertoire personnel par défaut, contrairement à `useradd` qui nécessite l'option `-m`.**  

---

#### **Explication :**  
- **`useradd` :**  
  - Outil bas-niveau utilisé pour ajouter un utilisateur au système.  
  - Par défaut, il n'inclut pas de répertoire personnel.  
  - Pour créer un répertoire personnel, il faut spécifier l’option `-m` :  
    ```bash
    useradd -m albert
    ```
  - Nécessite l'utilisation de `passwd` pour définir un mot de passe.  

- **`adduser` :**  
  - Script convivial qui appelle `useradd` en arrière-plan mais fournit une interface interactive.  
  - Crée automatiquement un répertoire personnel et demande la définition d’un mot de passe.  
  - Configure également des fichiers de configuration par défaut (ex: `.bashrc`, `.profile`).

---

#### **Scénario d'Utilisation :**  
Un administrateur souhaite créer un utilisateur nommé `luc` avec un répertoire personnel.  
Avec `useradd` :  
```bash
useradd -m luc
passwd luc
```
Avec `adduser` :  
```bash
adduser luc
```
L'approche `adduser` est plus pratique car elle demande directement la création du mot de passe et s'assure que le répertoire personnel est configuré correctement.

---

---

### **3) Quelle commande permet de vérifier les permissions d'un fichier ou d'un répertoire ?**  
**Réponse : C) `ls -l`**  

---

#### **Explication :**  
La commande `ls -l` affiche les détails d'un fichier ou d'un répertoire, incluant ses permissions, le propriétaire, le groupe, la taille, et la date de dernière modification.  

---

#### **Syntaxe Générale :**  
```bash
ls -l chemin_du_fichier_ou_du_dossier
```
---

#### **Exemple :**  
```bash
ls -l /home/shared_data/file1.txt
```
Produit une sortie similaire à :  
```
-rw-r--r-- 1 albert albert 4096 Mar 28 09:00 file1.txt
```
- `-rw-r--r--` : Indique les permissions du fichier.  
  - `rw-` : Le propriétaire (`albert`) peut lire et écrire.  
  - `r--` : Le groupe (`albert`) peut lire.  
  - `r--` : Les autres utilisateurs peuvent lire.  

---

---

### **4) Si un fichier a les permissions `644`, quelles actions sont possibles pour le propriétaire, le groupe, et les autres utilisateurs ?**  
**Réponse : C) Lecture et écriture pour le propriétaire, lecture seule pour les autres.**  

---

#### **Explication :**  
Les permissions `644` se décomposent comme suit :  
- **Propriétaire (6) :** Lecture (`4`) + Écriture (`2`) = `rw-`  
- **Groupe (4) :** Lecture seule = `r--`  
- **Autres (4) :** Lecture seule = `r--`  

---

#### **Scénario d’Utilisation :**  
Si un fichier nommé `document.txt` appartient à `bernard` avec les permissions `644`, alors :  
- **Bernard (propriétaire)** peut lire et modifier le fichier.  
- **Les autres membres du groupe** peuvent seulement lire le fichier.  
- **Tous les autres utilisateurs** peuvent aussi lire le fichier.  

---

---

### **5) Pourquoi l'utilisation de `chmod -R 777 /home/shared_data` peut-elle être risquée ?**  
**Réponse : B) Cela permet à tous les utilisateurs de lire, écrire, et exécuter tous les fichiers sans restriction.**  

---

#### **Explication :**  
Les permissions `777` signifient :  
- **Lecture, écriture, et exécution pour le propriétaire, le groupe, et les autres utilisateurs.**  
- C'est dangereux car tout utilisateur peut potentiellement :  
  - Modifier ou supprimer des fichiers critiques.  
  - Exécuter des fichiers malveillants.  
  - Modifier la structure des répertoires.  

---

#### **Solution Alternative :**  
Pour autoriser uniquement l'accès en lecture et écriture au groupe `collaborateurs`, mais interdire l’exécution :  
```bash
chmod -R 664 /home/shared_data
chown -R :collaborateurs /home/shared_data
```

---

Je vais continuer avec les questions restantes. Laissez-moi avancer sur les réponses détaillées.

### **6) Quelle commande permet d'attribuer des permissions spécifiques à un fichier ?**  
**Réponse : B) `chmod`**  

---

#### **Explication :**  
La commande `chmod` (Change Mode) est utilisée pour modifier les permissions d'un fichier ou d'un répertoire. Elle permet de définir des droits d'accès pour le propriétaire, le groupe et les autres utilisateurs.  

---

#### **Syntaxe Générale :**  
```bash
chmod [options] mode fichier_ou_dossier
```
---

#### **Exemples :**  
- **Définir les permissions `644` sur un fichier :**  
  ```bash
  chmod 644 file1.txt
  ```
  Cela signifie que le propriétaire a les droits de lecture et d'écriture (`rw-`), tandis que le groupe et les autres utilisateurs peuvent uniquement lire (`r--`).

- **Autoriser l’exécution pour tous les utilisateurs (`751`) :**  
  ```bash
  chmod 751 script.sh
  ```
  Cela donne :  
  - **Propriétaire :** Lecture, écriture, exécution (`rwx`).  
  - **Groupe :** Lecture, exécution (`r-x`).  
  - **Autres :** Exécution seulement (`--x`).  

- **Modifier les permissions de manière récursive (`-R`) :**  
  ```bash
  chmod -R 755 /home/shared_data
  ```
  Cela affecte tous les fichiers et dossiers à l'intérieur de `/home/shared_data`.

---

---

### **7) Pourquoi est-il important de vérifier les permissions d'un fichier après l'utilisation de `chown` ?**  
**Réponse : B) Pour s'assurer que l'utilisateur a les autorisations nécessaires pour lire, écrire ou exécuter le fichier.**  

---

#### **Explication :**  
L'utilisation de `chown` permet de changer le propriétaire d'un fichier, mais elle n'affecte pas directement les permissions associées au fichier.  

---

#### **Raison Principale :**  
- Après avoir transféré la propriété d’un fichier, l'utilisateur cible pourrait ne pas avoir les permissions nécessaires pour y accéder.  
- Par exemple, si le fichier a des permissions `444` (`r--r--r--`), même si l'utilisateur est désormais propriétaire, il ne pourra pas modifier le fichier.  

---

#### **Scénario d'Utilisation :**  
Un administrateur modifie le propriétaire d’un fichier `config.txt` pour un utilisateur `bernard` :  
```bash
chown bernard config.txt
```
---

Si `config.txt` a les permissions `444`, `bernard` pourra lire le fichier mais pas l'écrire. Il faudra donc vérifier et ajuster les permissions si nécessaire :  
```bash
chmod 644 config.txt  # Donne les droits d'écriture au propriétaire
```
---

---

### **8) Quel est l'effet de la commande suivante ?**  
```bash
chmod 751 /home/shared_data/dossier1/script1.sh
```  
**Réponse : C) Autoriser la lecture, l'écriture et l'exécution pour le propriétaire, la lecture et l'exécution pour le groupe, et l'exécution seule pour les autres.**  

---

#### **Explication :**  
La commande `chmod 751` attribue des permissions spécifiques :  

| Catégorie | Valeur Numérique | Signification  |
|-----------|------------------|----------------|
| Propriétaire   | 7 (4+2+1) | Lecture, Écriture, Exécution (`rwx`) |
| Groupe        | 5 (4+0+1) | Lecture, Exécution (`r-x`) |
| Autres        | 1 (0+0+1) | Exécution seulement (`--x`) |

---

#### **Interprétation :**  
- Le **propriétaire** peut lire, écrire et exécuter le script (`rwx`).  
- Les **membres du groupe** peuvent lire et exécuter, mais pas modifier (`r-x`).  
- **Tous les autres utilisateurs** peuvent uniquement exécuter (`--x`).  

---

#### **Scénario d’Utilisation :**  
Un script contenant des informations sensibles doit être protégé contre toute modification par les utilisateurs non autorisés, tout en étant exécutable par tous :  
```bash
chmod 751 /home/shared_data/dossier1/script1.sh
```
---

---

## **2. Questions Ouvertes**  

---

### **9) Expliquez pourquoi la commande `chown` est importante dans la gestion des permissions sous Linux. Décrivez également un scénario où son utilisation est nécessaire.**  

---

#### **Importance de `chown` :**  
La commande `chown` est essentielle pour la gestion des permissions car elle permet de définir quel utilisateur ou groupe possède un fichier ou un répertoire. La propriété d’un fichier détermine les droits d’accès et de modification qu’un utilisateur peut avoir.  

---

#### **Scénario :**  
Supposons qu’un administrateur crée un dossier partagé `/home/shared_data` et souhaite que `albert` ait le contrôle complet sur ce dossier.  
- **Création du dossier :**  
  ```bash
  mkdir /home/shared_data
  ```
- **Définition du propriétaire (`albert`) et du groupe (`collaborateurs`) :**  
  ```bash
  chown albert:collaborateurs /home/shared_data
  ```
- **Vérification :**  
  ```bash
  ls -ld /home/shared_data
  ```
  Sortie attendue :  
  ```
  drwxr-xr-x 2 albert collaborateurs 4096 Mar 28 11:00 /home/shared_data
  ```

---

---

### **10) Quels sont les risques potentiels de définir les permissions d'un répertoire partagé avec la commande `chmod -R 777` ? Proposez une solution alternative plus sécurisée.**  

---

#### **Risques Potentiels :**  
L’utilisation de `chmod -R 777` donne **tous les droits (lecture, écriture, exécution)** à **tout le monde**. Cela signifie que :  
- **Modification illégitime :** Tout utilisateur peut modifier ou supprimer des fichiers importants.  
- **Exécution de scripts malveillants :** Tout utilisateur peut exécuter des scripts potentiellement dangereux.  
- **Perte de données :** Des utilisateurs non autorisés peuvent accidentellement ou intentionnellement modifier ou détruire des données.  
- **Faille de sécurité :** Toute personne ayant accès au système peut exploiter ces permissions pour compromettre l’intégrité ou la disponibilité des données.  

---

#### **Solution Alternative :**  
- Créer un groupe spécifique (`collaborateurs`) pour les utilisateurs ayant besoin d'accéder au répertoire.  
- Attribuer ce groupe au dossier partagé :  
  ```bash
  chown -R albert:collaborateurs /home/shared_data
  ```
- Limiter les permissions en lecture et écriture pour le groupe :  
  ```bash
  chmod -R 770 /home/shared_data
  ```
  - **7 (Propriétaire)** : Lecture, écriture, exécution.  
  - **7 (Groupe)** : Lecture, écriture, exécution.  
  - **0 (Autres)** : Aucune permission.  

- Pour les fichiers devant être uniquement consultés :  
  ```bash
  chmod -R 750 /home/shared_data
  ```
---

Je vais continuer immédiatement avec les questions restantes.

### **11) Décrivez le processus complet pour créer un utilisateur, lui attribuer un répertoire personnel, et configurer correctement ses permissions d'accès à un dossier partagé.**  

---

#### **Étape 1 : Création de l'utilisateur avec son répertoire personnel**  
La commande `adduser` est préférée pour sa simplicité et son processus interactif qui inclut la création d’un répertoire personnel.  
```bash
adduser luc
```
Cela crée :  
- Un utilisateur `luc` dans le fichier `/etc/passwd`.  
- Un répertoire personnel `/home/luc`.  
- Les fichiers de configuration par défaut (`.bashrc`, `.profile`, etc.).  
- Un mot de passe est demandé immédiatement après l'exécution de la commande.  

---

#### **Étape 2 : Création du groupe et configuration des permissions**  
Pour une meilleure gestion des permissions, un groupe dédié est créé.  
```bash
groupadd collaborateurs
```
---

#### **Étape 3 : Ajout de l’utilisateur au groupe `collaborateurs`**  
```bash
usermod -aG collaborateurs luc
```
Cette commande ajoute `luc` au groupe `collaborateurs` sans supprimer ses autres appartenances de groupe.  

---

#### **Étape 4 : Création d’un dossier partagé**  
Un dossier partagé `/home/shared_data` est créé pour que plusieurs utilisateurs puissent y accéder.  
```bash
mkdir /home/shared_data
```
---

#### **Étape 5 : Configuration des propriétaires et permissions**  
1. Définir `albert` comme propriétaire du dossier et `collaborateurs` comme groupe :  
```bash
chown albert:collaborateurs /home/shared_data
```
2. Définir les permissions pour permettre uniquement au groupe de lire, écrire, et exécuter :  
```bash
chmod 770 /home/shared_data
```
Cela signifie :  
- **Propriétaire (`albert`) :** Lecture, écriture, exécution (`rwx`).  
- **Groupe (`collaborateurs`) :** Lecture, écriture, exécution (`rwx`).  
- **Autres utilisateurs :** Aucun accès (`---`).  

---

#### **Étape 6 : Vérification des permissions**  
```bash
ls -ld /home/shared_data
```
Sortie attendue :  
```
drwxrwx--- 2 albert collaborateurs 4096 Mar 28 12:00 /home/shared_data
```
---

#### **Étape 7 : Tester l'accès en tant que `luc`**  
Se connecter comme utilisateur `luc` et essayer de lire, écrire dans `/home/shared_data` :  
```bash
su luc
echo "Test d'accès" > /home/shared_data/test.txt
```
Si aucune erreur n’apparaît, l’utilisateur a bien accès au dossier partagé.  

---

---

### **12) Pourquoi est-il important de vérifier les permissions des fichiers et des répertoires après avoir utilisé `chown` et `chmod` ?**  

---

#### **Raisons Principales :**  
1. **Vérification de la Compatibilité des Permissions :**  
   - Lorsqu'un propriétaire change avec `chown`, l'utilisateur nouvellement attribué peut ne pas avoir les permissions suffisantes pour lire, écrire ou exécuter le fichier.  
   - Exemple : Si un fichier a des permissions `444` (`r--r--r--`), un changement de propriétaire via `chown` n'autorisera pas l’écriture par ce nouvel utilisateur.  

2. **Sécurité :**  
   - Les modifications apportées avec `chmod` peuvent exposer des fichiers critiques à des utilisateurs non autorisés.  
   - Des permissions trop permissives (`777`) peuvent conduire à une compromission du système.  

3. **Intégrité des données :**  
   - Si les permissions sont incorrectes, un utilisateur peut accidentellement modifier ou supprimer des fichiers importants.  

---

#### **Vérification des permissions :**  
Après avoir utilisé `chown` ou `chmod`, toujours vérifier les permissions actuelles avec :  
```bash
ls -l fichier_ou_dossier
```
---

---

### **13) Expliquez l'importance de l'utilisation d'un shell valide lors de la création d'un utilisateur. Comment vérifier si un utilisateur a un shell valide ?**  

---

#### **Importance d’un Shell Valide :**  
1. **Connexion Interactive :**  
   - Si un utilisateur n’a pas de shell valide, il ne pourra pas ouvrir de session interactive via SSH ou sur le terminal local.  
   - Un shell valide est généralement `/bin/bash`, `/bin/sh`, etc.  

2. **Exécution des Commandes :**  
   - Un utilisateur sans shell valide ne pourra pas exécuter de commandes à distance ou interagir avec le système de manière standard.  

3. **Sécurité :**  
   - Si un compte utilisateur est uniquement destiné à des tâches spécifiques (ex : serveur FTP), définir un shell invalide (`/usr/sbin/nologin`) empêche l’utilisateur de se connecter au système.  

---

#### **Comment vérifier si un utilisateur a un shell valide ?**  
Utiliser la commande suivante :  
```bash
grep nom_utilisateur /etc/passwd
```
---

#### **Exemple :**  
```bash
grep luc /etc/passwd
```
Sortie possible :  
```
luc:x:1002:1002::/home/luc:/bin/bash
```
Ici, le shell est `/bin/bash`, ce qui est valide.  

---

#### **Vérifier tous les utilisateurs avec un shell invalide :**  
```bash
grep -E 'nologin|false' /etc/passwd
```
---

---

## **3. Questions de Réflexion**  

---

### **14) Un utilisateur nommé `claude` tente d'exécuter le fichier `script1.sh` dans le dossier `/home/shared_data/dossier1`, mais reçoit un message d'erreur indiquant qu'il n'a pas l'autorisation d'exécuter le fichier. Quelle pourrait être la cause du problème ? Comment le résoudre ?**  

---

#### **Cause du problème :**  
Le fichier `script1.sh` n’a pas les permissions d’exécution pour `claude`.  

---

#### **Vérification des permissions actuelles :**  
```bash
ls -l /home/shared_data/dossier1/script1.sh
```
Sortie possible :  
```
-rw-r--r-- 1 bernard collaborateurs 4096 Mar 28 13:00 script1.sh
```
---

#### **Problème :**  
Le fichier a uniquement des permissions de lecture (`r--`) pour les autres utilisateurs (`claude` fait partie de cette catégorie).  

---

#### **Solution :**  
Donner la permission d’exécution au fichier :  
```bash
chmod +x /home/shared_data/dossier1/script1.sh
```
---

#### **Vérification :**  
```bash
ls -l /home/shared_data/dossier1/script1.sh
```
Sortie attendue :  
```
-rwxr--r-- 1 bernard collaborateurs 4096 Mar 28 13:00 script1.sh
```
---

Je vais continuer immédiatement avec les questions suivantes.

### **15) Un administrateur a configuré un dossier partagé avec `chmod -R 777 /home/shared_data`. Quelles sont les implications de cette configuration en termes de sécurité ? Comment l'administrateur pourrait-il configurer ce dossier de manière plus sûre tout en permettant un accès collaboratif ?**  

---

#### **Implications en termes de sécurité :**  
La permission `777` signifie :  
- Lecture (`r`), écriture (`w`), et exécution (`x`) pour **le propriétaire, le groupe, et tous les autres utilisateurs**.  
- N'importe quel utilisateur du système peut :  
  - Lire les fichiers (`r`), même s'ils contiennent des informations confidentielles.  
  - Modifier les fichiers (`w`), ce qui permet de détruire ou d'altérer des données.  
  - Exécuter des fichiers (`x`), ce qui est particulièrement dangereux pour les scripts pouvant contenir des commandes sensibles.  
- Risques majeurs :  
  - Exposition des fichiers à la suppression ou à la modification par des utilisateurs non autorisés.  
  - Possibilité d’introduire des logiciels malveillants par n'importe quel utilisateur.  
  - Failles de sécurité exploitables par des attaquants pour compromettre le système.  

---

#### **Exemple de risque :**  
Si un utilisateur malveillant dépose un script `malware.sh` dans `/home/shared_data` avec un contenu destructeur, il pourra l'exécuter sans restriction.  

---

#### **Solution Alternative Plus Sécurisée :**  

1. **Créer un groupe dédié aux utilisateurs autorisés :**  
```bash
groupadd collaborateurs
```

---

2. **Attribuer le dossier au groupe `collaborateurs` :**  
```bash
chown -R albert:collaborateurs /home/shared_data
```

---

3. **Définir les permissions de manière appropriée :**  
- Permettre seulement au propriétaire et au groupe d'accéder au dossier, tout en empêchant les autres utilisateurs d'accéder au contenu.  
```bash
chmod -R 770 /home/shared_data
```
---

4. **Ajouter les utilisateurs autorisés au groupe `collaborateurs` :**  
```bash
usermod -aG collaborateurs luc
usermod -aG collaborateurs bernard
```
---

5. **Vérifier les permissions du dossier :**  
```bash
ls -ld /home/shared_data
```
Sortie attendue :  
```
drwxrwx--- 2 albert collaborateurs 4096 Mar 28 14:00 /home/shared_data
```

---

6. **Utilisation du Sticky Bit (optionnelle) :**  
Pour protéger les fichiers d'un dossier partagé contre la suppression par des utilisateurs non propriétaires :  
```bash
chmod +t /home/shared_data
```
---

---

### **16) En quoi consiste le `sticky bit` et comment peut-il être utilisé pour améliorer la sécurité d'un répertoire partagé ?**  

---

#### **Définition du Sticky Bit :**  
Le `sticky bit` est un indicateur spécial qui peut être appliqué à un répertoire pour empêcher les utilisateurs d'autres groupes de supprimer ou de renommer des fichiers appartenant à d'autres utilisateurs dans ce répertoire.  

---

#### **Fonctionnement :**  
- Même si un répertoire est accessible en écriture pour plusieurs utilisateurs, seul le propriétaire d'un fichier peut le supprimer ou le renommer.  
- Cela est particulièrement utile pour les répertoires publics où plusieurs utilisateurs sont censés collaborer sans risque de suppression accidentelle ou malveillante.  

---

#### **Exemple de Configuration :**  
1. **Créer un répertoire partagé :**  
```bash
mkdir /home/shared_data
```
---

2. **Définir les permissions pour un usage collaboratif :**  
```bash
chmod 1777 /home/shared_data
```
- **1** indique que le `sticky bit` est activé.  
- Le reste (`777`) permet l'accès total en lecture, écriture, et exécution pour tout le monde, mais **seul le propriétaire d'un fichier peut le supprimer**.  

---

#### **Vérification :**  
```bash
ls -ld /home/shared_data
```
Sortie attendue :  
```
drwxrwxrwt 2 albert collaborateurs 4096 Mar 28 14:30 /home/shared_data
```
Le `t` à la fin indique que le `sticky bit` est activé.  

---

#### **Application Pratique :**  
Si plusieurs utilisateurs doivent accéder au dossier `/home/shared_data`, chacun pourra :  
- Créer ses propres fichiers.  
- Modifier ses propres fichiers.  
- Lire les fichiers des autres.  
- **Ne pas supprimer les fichiers appartenant à d'autres utilisateurs.**  

---

---

### **17) Quelles sont les bonnes pratiques pour limiter les permissions d'exécution sur des fichiers critiques ? Donnez un exemple concret.**  

---

#### **Bonnes Pratiques :**  

1. **Éviter les permissions trop permissives (`777` ou `755`) sur des fichiers sensibles.**  
2. **Attribuer les permissions en fonction des utilisateurs ou des groupes qui doivent y avoir accès.**  
3. **Utiliser `chmod` pour restreindre l’accès aux fichiers exécutables :**  
   - Un fichier qui ne doit pas être exécuté par des utilisateurs spécifiques doit avoir les permissions `640` (lecture, écriture pour le propriétaire et lecture pour le groupe uniquement).  
4. **Définir un propriétaire approprié avec `chown`.**  
5. **Utiliser des ACLs (Access Control Lists) pour affiner le contrôle d’accès si nécessaire.**  
6. **Désactiver l’exécution pour les utilisateurs non autorisés.**  

---

#### **Exemple Concret :**  
Supposons qu’un fichier script critique `backup.sh` doit être utilisé uniquement par l’administrateur `albert` :  

1. **Créer le script :**  
```bash
touch /home/albert/backup.sh
```

2. **Définir le propriétaire comme `albert` et les permissions `700` :**  
```bash
chown albert:albert /home/albert/backup.sh
chmod 700 /home/albert/backup.sh
```

---

#### **Vérification :**  
```bash
ls -l /home/albert/backup.sh
```
Sortie attendue :  
```
-rwx------ 1 albert albert 4096 Mar 28 15:00 /home/albert/backup.sh
```

---

#### **Explication :**  
- **Propriétaire (`albert`) :** Lecture, écriture, exécution (`rwx`).  
- **Groupe et autres utilisateurs :** Aucun accès (`---`).  

---

Je vais maintenant répondre aux questions pratiques.

## **4. Questions Pratiques (Commandes à Tester)**  

---

### **18) Créez un utilisateur nommé `luc` et configurez son répertoire personnel. Attribuez-lui l'accès en lecture et en écriture au dossier `/home/shared_data`, mais pas l'exécution.**  

---

#### **Étape 1 : Création de l’utilisateur `luc` avec son répertoire personnel**  
```bash
adduser luc
```
Cela va :  
- Créer l’utilisateur `luc`.  
- Créer un répertoire personnel `/home/luc`.  
- Demander un mot de passe pour l’utilisateur.  

---

#### **Étape 2 : Création du dossier `/home/shared_data` si ce n’est pas déjà fait**  
```bash
mkdir /home/shared_data
```
---

#### **Étape 3 : Création d’un groupe dédié aux utilisateurs autorisés**  
```bash
groupadd collaborateurs
```
---

#### **Étape 4 : Ajouter `luc` au groupe `collaborateurs`**  
```bash
usermod -aG collaborateurs luc
```
---

#### **Étape 5 : Définir le groupe propriétaire du dossier `/home/shared_data`**  
```bash
chown -R albert:collaborateurs /home/shared_data
```
---

#### **Étape 6 : Définir les permissions adéquates sur le dossier**  
On veut permettre l’accès en lecture et écriture, mais pas l’exécution.  
```bash
chmod 660 /home/shared_data
```
---

#### **Étape 7 : Vérification des permissions**  
```bash
ls -ld /home/shared_data
```
Sortie attendue :  
```
-rw-rw---- 1 albert collaborateurs 4096 Mar 28 15:30 /home/shared_data
```
---

#### **Explication :**  
- **Propriétaire (`albert`) :** Lecture et écriture (`rw-`).  
- **Groupe (`collaborateurs`) :** Lecture et écriture (`rw-`).  
- **Autres utilisateurs :** Aucun accès (`---`).  
- `luc`, en tant que membre du groupe `collaborateurs`, peut lire et modifier les fichiers dans `/home/shared_data`, mais ne peut pas exécuter de scripts.  

---

---

### **19) Modifiez les permissions de `file1.txt` pour que seul son propriétaire puisse le lire et l’écrire, tandis que les autres utilisateurs ne doivent pas pouvoir l’accéder.**  

---

#### **Étape 1 : Création du fichier `file1.txt` (si non existant)**  
```bash
touch /home/shared_data/file1.txt
```
---

#### **Étape 2 : Attribution d’un propriétaire au fichier**  
```bash
chown luc:luc /home/shared_data/file1.txt
```
---

#### **Étape 3 : Définition des permissions strictes (`600`)**  
```bash
chmod 600 /home/shared_data/file1.txt
```
---

#### **Étape 4 : Vérification des permissions**  
```bash
ls -l /home/shared_data/file1.txt
```
Sortie attendue :  
```
-rw------- 1 luc luc 4096 Mar 28 15:45 file1.txt
```
---

#### **Explication :**  
- **Propriétaire (`luc`) :** Lecture et écriture (`rw-`).  
- **Groupe (`luc`) :** Aucun accès (`---`).  
- **Autres utilisateurs :** Aucun accès (`---`).  

---

---

### **20) Assurez-vous que tous les utilisateurs du groupe `collaborateurs` puissent lire et écrire dans le répertoire `/home/shared_data`, mais qu’ils ne puissent pas exécuter de fichiers ou de scripts présents dans ce répertoire.**  

---

#### **Étape 1 : Création du groupe `collaborateurs` (si non existant)**  
```bash
groupadd collaborateurs
```
---

#### **Étape 2 : Création du dossier partagé `/home/shared_data` (si non existant)**  
```bash
mkdir /home/shared_data
```
---

#### **Étape 3 : Définition du propriétaire du dossier et du groupe associé**  
```bash
chown -R albert:collaborateurs /home/shared_data
```
---

#### **Étape 4 : Configuration des permissions sur le dossier**  
```bash
chmod 660 /home/shared_data
```
---

#### **Étape 5 : Désactiver l’exécution des fichiers à l’intérieur du dossier**  
Les fichiers et scripts ne doivent pas être exécutables par le groupe `collaborateurs`.  
```bash
chmod -R 660 /home/shared_data/*
```
---

#### **Étape 6 : Ajouter les utilisateurs au groupe `collaborateurs`**  
```bash
usermod -aG collaborateurs luc
usermod -aG collaborateurs bernard
```
---

#### **Étape 7 : Vérification des permissions**  
```bash
ls -ld /home/shared_data
```
Sortie attendue :  
```
drw-rw---- 2 albert collaborateurs 4096 Mar 28 16:00 /home/shared_data
```
---

#### **Explication :**  
- **Propriétaire (`albert`) :** Lecture, écriture, exécution (`rwx`).  
- **Groupe (`collaborateurs`) :** Lecture et écriture uniquement (`rw-`).  
- **Autres utilisateurs :** Aucun accès (`---`).  
- Les membres du groupe `collaborateurs` peuvent lire et écrire dans ce répertoire mais **ne peuvent pas exécuter de fichiers ou de scripts**.  
