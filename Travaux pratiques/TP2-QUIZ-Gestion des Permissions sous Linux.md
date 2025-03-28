# **Quiz : Gestion des Permissions sous Linux**  

---
# **1. Questions à Choix Multiples (QCM)**  
---

**1) Quelle commande permet de modifier le propriétaire d'un fichier ou d'un répertoire ?**  
- A) `chmod`  
- B) `chown`  
- C) `ls -l`  
- D) `usermod`  



**2) Quelle est la différence principale entre les commandes `useradd` et `adduser` ?**  
- A) `useradd` est interactif, tandis que `adduser` est non-interactif.  
- B) `adduser` est un script convivial qui crée un répertoire personnel par défaut, contrairement à `useradd` qui nécessite l'option `-m`.  
- C) `adduser` est utilisé uniquement pour supprimer des utilisateurs.  
- D) `useradd` est uniquement disponible sur Ubuntu.  



**3) Quelle commande permet de vérifier les permissions d'un fichier ou d'un répertoire ?**  
- A) `ls -a`  
- B) `chown`  
- C) `ls -l`  
- D) `chmod`  



**4) Si un fichier a les permissions `644`, quelles actions sont possibles pour le propriétaire, le groupe, et les autres utilisateurs ?**  
- A) Lecture et écriture pour tous.  
- B) Lecture, écriture et exécution pour le propriétaire uniquement.  
- C) Lecture et écriture pour le propriétaire, lecture seule pour les autres.  
- D) Lecture, écriture et exécution pour tout le monde.  


**5) Pourquoi l'utilisation de `chmod -R 777 /home/shared_data` peut-elle être risquée ?**  
- A) Cela bloque l'accès à tous les utilisateurs.  
- B) Cela permet à tous les utilisateurs de lire, écrire, et exécuter tous les fichiers sans restriction.  
- C) Cela désactive toutes les permissions d'exécution.  
- D) Cela supprime tous les fichiers dans le répertoire.  



**6) Quelle commande permet d'attribuer des permissions spécifiques à un fichier ?**  
- A) `usermod`  
- B) `chmod`  
- C) `adduser`  
- D) `passwd`  

**7) Pourquoi est-il important de vérifier les permissions d'un fichier après l'utilisation de `chown` ?**  
- A) Pour vérifier si le fichier a été supprimé.  
- B) Pour s'assurer que l'utilisateur a les autorisations nécessaires pour lire, écrire ou exécuter le fichier.  
- C) Pour vérifier la connexion réseau.  
- D) Pour modifier le nom du fichier.  



**8) Quel est l'effet de la commande suivante ?**  
```bash
chmod 751 /home/shared_data/dossier1/script1.sh
```  
- A) Autoriser l'exécution pour tous les utilisateurs, mais l'écriture uniquement pour le propriétaire.  
- B) Interdire l'accès complet au fichier.  
- C) Autoriser la lecture, l'écriture et l'exécution pour le propriétaire, la lecture et l'exécution pour le groupe, et l'exécution seule pour les autres.  
- D) Accorder tous les droits à tout le monde.  



---
# **2. Questions Ouvertes**  
---


**9) Expliquez pourquoi la commande `chown` est importante dans la gestion des permissions sous Linux. Décrivez également un scénario où son utilisation est nécessaire.**  
*Votre réponse :*  



**10) Quels sont les risques potentiels de définir les permissions d'un répertoire partagé avec la commande `chmod -R 777` ? Proposez une solution alternative plus sécurisée.**  
*Votre réponse :*  



**11) Décrivez le processus complet pour créer un utilisateur, lui attribuer un répertoire personnel, et configurer correctement ses permissions d'accès à un dossier partagé.**  
*Votre réponse :*  



**12) Pourquoi est-il important de vérifier les permissions des fichiers et des répertoires après avoir utilisé `chown` et `chmod` ?**  
*Votre réponse :*  



**13) Expliquez l'importance de l'utilisation d'un shell valide lors de la création d'un utilisateur. Comment vérifier si un utilisateur a un shell valide ?**  
*Votre réponse :*  



---
# **3. Questions de Réflexion**  
---


**14) Un utilisateur nommé `claude` tente d'exécuter le fichier `script1.sh` dans le dossier `/home/shared_data/dossier1`, mais reçoit un message d'erreur indiquant qu'il n'a pas l'autorisation d'exécuter le fichier. Quelle pourrait être la cause du problème ? Comment le résoudre ?**  
*Votre réponse :*  



**15) Un administrateur a configuré un dossier partagé avec `chmod -R 777 /home/shared_data`. Quelles sont les implications de cette configuration en termes de sécurité ? Comment l'administrateur pourrait-il configurer ce dossier de manière plus sûre tout en permettant un accès collaboratif ?**  
*Votre réponse :*  



**16) En quoi consiste le `sticky bit` et comment peut-il être utilisé pour améliorer la sécurité d'un répertoire partagé ?**  
*Votre réponse :*  



**17) Quelles sont les bonnes pratiques pour limiter les permissions d'exécution sur des fichiers critiques ? Donnez un exemple concret.**  
*Votre réponse :*  


---
# **4. Questions Pratiques (Commandes à Tester)**  
---


**18) Créez un utilisateur nommé `luc` et configurez son répertoire personnel. Attribuez-lui l'accès en lecture et en écriture au dossier `/home/shared_data`, mais pas l'exécution.**  
*Votre réponse (commandes) :*  



**19) Modifiez les permissions de `file1.txt` pour que seul son propriétaire puisse le lire et l’écrire, tandis que les autres utilisateurs ne doivent pas pouvoir l’accéder.**  
*Votre réponse (commandes) :*  



**20) Assurez-vous que tous les utilisateurs du groupe `collaborateurs` puissent lire et écrire dans le répertoire `/home/shared_data`, mais qu’ils ne puissent pas exécuter de fichiers ou de scripts présents dans ce répertoire.**  
*Votre réponse (commandes) :*  

