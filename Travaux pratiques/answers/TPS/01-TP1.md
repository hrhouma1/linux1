# **Réponses au Quiz 1 - Gestion des Utilisateurs sous Linux**  

---
# **1. Questions à Choix Multiples (QCM)**  
---

**1) Quelle commande permet de supprimer un utilisateur ainsi que son répertoire personnel ?**  
**Réponse : C) `userdel -r`**  
- La commande `userdel -r` supprime un utilisateur du système ainsi que son répertoire personnel (ex : `/home/nom_utilisateur`).  
- Cette option est utile lorsque vous souhaitez supprimer définitivement un utilisateur sans laisser de fichiers orphelins sur le système.  
- En cas d’erreur, il est possible de recréer l’utilisateur, mais ses fichiers seront perdus si aucune sauvegarde n’a été réalisée.  



**2) Quelle est la différence principale entre `adduser` et `useradd` ?**  
**Réponse : B) `adduser` est interactif et crée un répertoire personnel par défaut, alors que `useradd` est moins interactif et nécessite l'option `-m`.**  
- `adduser` est un script qui simplifie le processus de création d'un utilisateur en fournissant une interface interactive.  
- Par défaut, `adduser` crée un répertoire personnel, initialise les fichiers de configuration de l’utilisateur, et demande de définir un mot de passe.  
- `useradd` est une commande de bas-niveau qui ne crée pas de répertoire personnel par défaut. Pour cela, l’option `-m` doit être spécifiée (`useradd -m nom_utilisateur`).  
- Si l’option `-m` est omise, l’utilisateur sera ajouté au fichier `/etc/passwd` mais sans dossier `/home/nom_utilisateur`, ce qui peut poser des problèmes lors de la connexion.  



**3) Quelle commande permet de vérifier la liste des utilisateurs existants sur le système ?**  
**Réponse : B) `cat /etc/passwd`**  
- Le fichier `/etc/passwd` contient la liste de tous les utilisateurs enregistrés sur le système.  
- Chaque ligne du fichier représente un utilisateur et suit un format spécifique :  
  ```
  nom_utilisateur:x:UID:GID:commentaire:chemin_du_repertoire:shell
  ```
- Pour filtrer un utilisateur spécifique, on peut utiliser :  
  ```bash
  cat /etc/passwd | grep nom_utilisateur
  ```
- Les utilisateurs créés récemment doivent apparaître dans ce fichier s'ils ont été ajoutés correctement.  



**4) Pourquoi est-il nécessaire de redémarrer le système après l'ajout de nouveaux utilisateurs ?**  
**Réponse : B) Pour appliquer les changements et afficher les utilisateurs dans les options de connexion.**  
- Le redémarrage du système permet au gestionnaire de connexion (par exemple, `GDM`, `LightDM`, etc.) de rafraîchir la liste des utilisateurs disponibles.  
- Cela est particulièrement important si un nouvel utilisateur a été ajouté manuellement avec `useradd` ou si le fichier `/etc/passwd` a été modifié directement.  
- Le redémarrage permet aussi de vérifier que l'utilisateur est bien configuré avec un répertoire personnel, un shell valide, et les permissions adéquates.  



---
# **2. Questions Ouvertes**  
---

**5) Expliquez pourquoi l'utilisation de la commande `adduser` est préférable à `useradd` dans la plupart des cas.**  
**Réponse :**  
L'utilisation de `adduser` est recommandée car elle fournit une interface interactive qui simplifie le processus de création d'un utilisateur. Contrairement à `useradd`, qui est une commande bas-niveau, `adduser` :  
- Crée automatiquement un répertoire personnel (`/home/nom_utilisateur`).  
- Initialise les fichiers de configuration par défaut (comme `.bashrc`, `.profile`, etc.).  
- Demande immédiatement de définir un mot de passe pour l'utilisateur.  
- Assigne automatiquement un groupe primaire correspondant au nom de l'utilisateur.  
- Fournit des messages d’erreur clairs et des indications lorsque des étapes sont manquantes.  
`useradd` est préférable uniquement lorsque vous souhaitez un contrôle manuel total sur les paramètres de l'utilisateur.  



**6) Que faire si un utilisateur créé n'apparaît pas dans l'écran de connexion après le redémarrage ?**  
**Réponse :**  
Si un utilisateur n'apparaît pas après redémarrage :  
1. **Vérifier l'existence de l'utilisateur dans `/etc/passwd` :**  
   ```bash
   cat /etc/passwd | grep nom_utilisateur
   ```  
2. **Vérifier l'existence du répertoire personnel dans `/home` :**  
   ```bash
   ls /home
   ```  
3. **Vérifier les permissions :**  
   ```bash
   ls -ld /home/nom_utilisateur
   ```  
   Les permissions doivent indiquer que l'utilisateur est propriétaire de son répertoire.  
4. **Vérifier la validité du shell :**  
   ```bash
   grep nom_utilisateur /etc/passwd
   ```  
   Un shell valide doit être spécifié (par exemple `/bin/bash`).  
5. **Vérifier les configurations du gestionnaire de connexion :**  
   S’assurer qu’aucune configuration particulière ne masque l’utilisateur.  
6. **Vérifier si l'utilisateur est désactivé :**  
   ```bash
   sudo passwd -S nom_utilisateur
   ```  
   Si l’état est `L (locked)`, l’utilisateur est bloqué. Utiliser `passwd -u nom_utilisateur` pour le débloquer.  



**7) Décrivez brièvement les étapes nécessaires pour supprimer des utilisateurs existants et en créer de nouveaux sous Linux.**  
**Réponse :**  
1. Passer en mode superutilisateur :  
   ```bash
   su
   ```  
2. Supprimer les utilisateurs existants :  
   ```bash
   userdel -r nom_utilisateur
   ```  
3. Ajouter de nouveaux utilisateurs :  
   ```bash
   adduser nom_utilisateur
   ```  
4. Vérifier la création des répertoires personnels :  
   ```bash
   ls /home
   ```  
5. Redémarrer le système :  
   ```bash
   reboot
   ```  



---
# **3. Question de Résolution de Problème**  
---

**8) Après avoir ajouté un utilisateur avec `useradd`, celui-ci ne peut pas se connecter. Quelles vérifications feriez-vous pour diagnostiquer et corriger le problème ?**  
**Réponse :**  
1. Vérifier la présence de l’utilisateur dans `/etc/passwd`.  
2. Créer un répertoire personnel si absent :  
   ```bash
   sudo useradd -m nom_utilisateur
   ```  
3. Définir un mot de passe pour l'utilisateur :  
   ```bash
   passwd nom_utilisateur
   ```  
4. Vérifier si le compte est verrouillé :  
   ```bash
   passwd -S nom_utilisateur
   ```  
   Déverrouiller si nécessaire :  
   ```bash
   passwd -u nom_utilisateur
   ```  
5. Vérifier le shell par défaut et le modifier si nécessaire :  
   ```bash
   sudo usermod -s /bin/bash nom_utilisateur
   ```  
6. Vérifier les permissions du répertoire personnel.  
7. Redémarrer le système pour appliquer les modifications.  

