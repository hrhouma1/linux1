
---
# **1. Rappel à la Gestion des Permissions**  
---

La gestion des permissions est un aspect essentiel de l'administration des systèmes Linux. Elle permet de contrôler qui peut accéder, modifier ou exécuter des fichiers et des répertoires. Une mauvaise configuration des permissions peut entraîner des failles de sécurité majeures, compromettant ainsi la confidentialité, l'intégrité et la disponibilité des données.

Dans cette section, nous allons explorer les différentes étapes nécessaires pour créer des utilisateurs, configurer des fichiers et des répertoires partagés, et appliquer des permissions adaptées pour garantir un accès contrôlé. Nous verrons également comment vérifier ces permissions pour nous assurer qu'elles sont correctement appliquées.  

Les exercices présentés permettront de comprendre comment les commandes `chown`, `chmod`, et `su` peuvent être utilisées pour gérer efficacement les accès, ainsi que les risques potentiels associés à une mauvaise configuration.  

Enfin, nous discuterons des meilleures pratiques pour sécuriser les fichiers critiques, tout en garantissant un accès approprié aux utilisateurs légitimes.  




---
# 2.**Gestion des Permissions**
---

## **2.1. Connexion en tant que Superutilisateur**
```bash
su
# Entrez le mot de passe du superutilisateur
```

## **2.2. Création des Utilisateurs**
```bash
# Assurez-vous que les utilisateurs existent ou créez-les
adduser albert
adduser bernard
adduser claude
adduser danielle
```

## **2.3. Création du Dossier et des Fichiers**
```bash
# Créer le dossier shared_data et les sous-dossiers/fichiers nécessaires
mkdir -p /home/shared_data
mkdir -p /home/shared_data/dossier1

touch /home/shared_data/file1.txt
touch /home/shared_data/file2.txt
touch /home/shared_data/dossier1/script1.sh
touch /home/shared_data/dossier1/script2.sh
```

## **2.4. Attribution des Propriétaires et des Permissions**
```bash
# Configuration initiale des permissions et propriétaires
chown albert:albert /home/shared_data/file1.txt
chmod 644 /home/shared_data/file1.txt
chown albert:albert /home/shared_data/file2.txt
chmod 444 /home/shared_data/file2.txt

chown bernard:bernard /home/shared_data/file2.txt
chmod 664 /home/shared_data/file2.txt
chown bernard:bernard /home/shared_data/dossier1/script1.sh
chmod 751 /home/shared_data/dossier1/script1.sh

chown -R claude:claude /home/shared_data/dossier1
chmod 766 /home/shared_data/dossier1
chmod 666 /home/shared_data/dossier1/script1.sh
chmod 666 /home/shared_data/dossier1/script2.sh

chown -R danielle:danielle /home/shared_data
chmod -R 777 /home/shared_data
```

## **2.5. Vérification des Répertoires Home des Utilisateurs**
```bash
# Vérifiez que les répertoires des utilisateurs sont correctement configurés
ls -l /home
```

## **2.6. Tableau de Tests de Permission et Vérification**
Voici un tableau qui décrit les actions que chaque utilisateur essaiera de réaliser et les résultats attendus basés sur les permissions définies :

| **Action/Test**                                            | **Utilisateur** | **Commande**                                           | **Résultat Attendu**                      |
|------------------------------------------------------------|-----------------|--------------------------------------------------------|-------------------------------------------|
| Lire `file1.txt`                                           | Albert          | `cat /home/shared_data/file1.txt`                      | Succès                                    |
| Écrire dans `file1.txt`                                    | Albert          | `echo "Test" >> /home/shared_data/file1.txt`           | Succès                                    |
| Lire `file2.txt`                                           | Albert          | `cat /home/shared_data/file2.txt`                      | Succès                                    |
| Écrire dans `file2.txt`                                    | Albert          | `echo "Test" >> /home/shared_data/file2.txt`           | Échec (Permission refusée)                |
| Lire et écrire dans `file2.txt`                            | Bernard         | `echo "Bernard" >> /home/shared_data/file2.txt`        | Succès                                    |
| Exécuter `script1.sh` dans `dossier1`                      | Bernard         | `bash /home/shared_data/dossier1/script1.sh`           | Succès                                    |
| Lire et écrire dans `dossier1`                             | Claude          | `echo "Claude" >> /home/shared_data/dossier1/test.txt` | Succès                                    |
| Exécuter `script1.sh` dans `dossier1`                      | Claude          | `bash /home/shared_data/dossier1/script1.sh`           | Échec (Permission refusée)                |
| Exécuter toutes les actions sur tous les fichiers/dossiers | Danielle        | `echo "Danielle" >> /home/shared_data/file1.txt`       | Succès (accès total)                      |

## **2.7. Réalisation des Tests**
```bash
# Exécuter les tests pour vérifier les permissions
su albert -c 'cat /home/shared_data/file1.txt'
su albert -c 'echo "Test" >> /home/shared_data/file2.txt'
su bernard -c 'echo "Bernard" >> /home/shared_data/file2.txt'
su bernard -c 'bash /home/shared_data/dossier1/script1.sh'
su claude -c 'echo "Claude" >> /home/shared_data/dossier1/test.txt'
su claude -c 'bash /home/shared_data/dossier1/script1.sh'
su danielle -c 'echo "Danielle" >> /home/shared_data/file1.txt'
```










## **2.8. Exercice - Ajouter des Permissions d’Exécution avec `chmod` (notation symbolique)


Dans cet exercice, vous allez explorer les permissions d’exécution et la manière de les accorder aux utilisateurs, aux groupes et à tous les autres en utilisant la notation symbolique de `chmod`.

### Objectifs pédagogiques

- Comprendre la notation symbolique (`u`, `g`, `o`, `a`) avec `chmod`
- Ajouter dynamiquement la permission d'exécution sans modifier les autres droits
- Vérifier les effets avec `ls -l`



### Étapes à suivre

#### 2.8.1. Création d’un fichier script simple
```bash
touch mon_script.sh
echo -e "#!/bin/bash\necho 'Script lancé !'" > mon_script.sh
```

#### 2.8.2. Affichage des permissions actuelles
```bash
ls -l mon_script.sh
```
Vérifiez que le fichier n’est pas exécutable.

#### 2.8.3. Ajout de la permission d'exécution pour l’utilisateur (propriétaire)
```bash
chmod u+x mon_script.sh
ls -l mon_script.sh
```
Ensuite, essayez d’exécuter le script avec :
```bash
./mon_script.sh
```

#### 2.8.4. Ajout de la permission d'exécution pour le groupe
```bash
chmod g+x mon_script.sh
ls -l mon_script.sh
```

#### 2.8.5. Ajout de la permission d'exécution pour tous les utilisateurs (a = all)
```bash
chmod a+x mon_script.sh
ls -l mon_script.sh
```

#### 2.8.6. Suppression de toutes les permissions d’exécution
```bash
chmod a-x mon_script.sh
ls -l mon_script.sh
```



##### 2.8.7 Résumé des commandes utilisées

| Commande               | Action                                                       |
|------------------------|--------------------------------------------------------------|
| `chmod u+x fichier`    | Ajoute la permission d'exécution pour l'utilisateur          |
| `chmod g+x fichier`    | Ajoute la permission d'exécution pour le groupe              |
| `chmod o+x fichier`    | Ajoute la permission d'exécution pour les autres utilisateurs |
| `chmod a+x fichier`    | Ajoute la permission d'exécution pour tout le monde          |
| `chmod a-x fichier`    | Retire la permission d'exécution pour tout le monde          |



#### 2.8.8. Questions de réflexion

1. Pourquoi utilise-t-on parfois `u+x` au lieu de `chmod 744` ?
2. Est-il possible d'ajouter une permission sans toucher aux autres ? Expliquez.
3. Quelle est la différence entre `chmod a+x` et `chmod 755` ?






---
# 3 - Annexe 01 - useradd vs adduser 
---

## 3.1. Création d'Utilisateurs avec `useradd` et `adduser`

La gestion des utilisateurs est une tâche fondamentale en administration système sous Linux. Deux commandes principales sont couramment utilisées pour créer des utilisateurs : `useradd` et `adduser`. Bien qu'elles aient des noms similaires, elles fonctionnent différemment.

## 3.2. `useradd` : La Commande Basique

La commande `useradd` est un utilitaire standard disponible sur toutes les distributions Linux. Elle permet de créer un nouvel utilisateur sans interaction humaine, ce qui la rend idéale pour les scripts d'automatisation. Toutefois, `useradd` ne demande pas de mot de passe ni d'informations supplémentaires par défaut. Voici un exemple de son utilisation :

```bash
# Créer un utilisateur sans mot de passe
useradd -m albert
```

Après avoir créé l'utilisateur, vous devez définir un mot de passe manuellement avec la commande `passwd` :

```bash
# Ajouter un mot de passe pour l'utilisateur
passwd albert
```

## 3.3. `adduser` : La Version Conviviale

Sur certaines distributions Linux, comme Debian et Ubuntu, `adduser` est une commande plus conviviale, qui est en réalité un script Perl enveloppant `useradd`. Cette commande guide l'administrateur à travers un processus interactif où il peut configurer plusieurs aspects du compte utilisateur, y compris le mot de passe, le nom complet, et d'autres informations facultatives.

Voici comment utiliser `adduser` :

```bash
# Créer un utilisateur avec un processus interactif
adduser albert
```

Le système vous posera une série de questions :

1. **Mot de passe** : Vous serez invité à entrer et confirmer le mot de passe de l'utilisateur.
2. **Nom complet** : Vous pouvez fournir un nom complet pour l'utilisateur (optionnel).
3. **Numéro de chambre, Téléphone de travail, Téléphone personnel, Autres informations** : Ces champs sont facultatifs et peuvent être laissés vides en appuyant sur `Entrée`.
4. **Confirmation** : Vous devrez confirmer les informations avant que l'utilisateur ne soit créé.

## 3.4. Quand Utiliser `useradd` ou `adduser` ?

- **`useradd`** : Utilisez cette commande lorsque vous avez besoin de créer des utilisateurs via un script ou lorsque vous ne souhaitez pas être invité à entrer des informations supplémentaires.
- **`adduser`** : Utilisez cette commande lorsque vous préférez un processus interactif et souhaitez configurer plusieurs détails du compte utilisateur en une seule étape.

---
# 4. Conclusion
---

La différence entre `useradd` et `adduser` est principalement liée à l'interaction et à la facilité d'utilisation. Les administrateurs qui préfèrent un contrôle total et moins d'interaction opteront pour `useradd`, tandis que ceux qui apprécient un processus guidé trouveront `adduser` plus adapté. Selon vos besoins spécifiques, vous pouvez choisir l'une ou l'autre de ces commandes pour gérer efficacement les utilisateurs sur votre système Linux.

---
# 5. Annexe 01 - mot de passe
---

Lorsque vous utilisez la commande `useradd`, par défaut, elle ne demande pas de créer un mot de passe pour l'utilisateur. Pour ajouter un mot de passe immédiatement après avoir créé un utilisateur, vous pouvez utiliser la commande `passwd`. Voici comment procéder :

```bash
# Créer les utilisateurs
useradd -m pierre
useradd -m marie
useradd -m luc
useradd -m sophie

# Ajouter un mot de passe pour chaque utilisateur
passwd pierre
passwd marie
passwd luc
passwd sophie
```

Avec chaque commande `passwd`, le système vous demandera d'entrer un mot de passe pour l'utilisateur spécifié. Assurez-vous que le mot de passe est conforme aux règles de sécurité du système (par exemple, longueur minimale, complexité, etc.).

Si vous voulez automatiser la création de l'utilisateur avec un mot de passe, vous pouvez utiliser la commande suivante, mais attention à la sécurité puisque le mot de passe sera visible dans l'historique des commandes :

```bash
echo "motdepasse" | passwd --stdin nom_utilisateur
```

Notez que l'option `--stdin` fonctionne sur certaines distributions Linux, comme Red Hat ou CentOS. Si cette option n'est pas disponible, vous devrez utiliser la méthode précédente pour définir les mots de passe manuellement.

---
# Annexe 02 - changer le mot de passe
---

- Pour changer le mot de passe de l'utilisateur `luc`, vous pouvez utiliser la commande suivante :

```bash
passwd luc
```

Après avoir exécuté cette commande, le système vous demandera de saisir un nouveau mot de passe pour l'utilisateur `luc`, puis de le confirmer en le saisissant une deuxième fois.

Voici comment cela se déroule :

1. **Entrez le nouveau mot de passe** : Tapez le nouveau mot de passe pour `luc`.
2. **Confirmez le mot de passe** : Retapez le même mot de passe pour confirmation.

Une fois ces étapes terminées, le mot de passe de l'utilisateur `luc` sera mis à jour.

---
# Annexe 03 -  **Questions de Réflexion**
---

### **Objectif**: 
- Mieux comprendre la gestion des permissions dans Linux et l'impact de ces configurations sur la sécurité globale du système.


1. **Expliquez l'importance des permissions dans un système Linux. Comment les permissions incorrectes peuvent-elles affecter la sécurité d'un système ?**
   - Cette question vise à vous amener à réfléchir sur les implications de la sécurité et la gestion des accès dans un environnement Linux.

2. **Quelles différences y a-t-il entre les permissions de lecture, écriture et exécution ? Pourquoi est-il crucial de configurer correctement ces permissions dans un environnement multi-utilisateur ?**
   - Cette question est conçue pour vous aider à comprendre et à expliquer l'importance de chaque type de permission dans la sécurité et la fonctionnalité d'un système.

3. **Quel est le rôle du fichier sudoers dans la gestion des permissions système ? Comment une mauvaise configuration de ce fichier peut-elle compromettre un système Linux ?**
   - Cette question vous encourage à explorer le rôle critique du fichier sudoers dans la gestion des privilèges d'administration et les risques associés à sa mauvaise gestion.

4. **Pourquoi est-il important de limiter les permissions d'exécution sur certains fichiers ou scripts ? Donnez un exemple où cette limitation pourrait prévenir une faille de sécurité.**
   - Cette question vous permet de discuter de l'importance de la restriction des permissions d'exécution et de réfléchir à des scénarios où de telles restrictions sont essentielles à la sécurité.

5. **Comment les commandes `chmod` et `chown` influencent-elles la sécurité des fichiers et dossiers sur un serveur Linux ? Quelles sont les bonnes pratiques pour leur utilisation ?**
   - Cette question vous demande de décrire comment ces commandes affectent la sécurité des fichiers et des dossiers et de réfléchir aux meilleures pratiques pour leur utilisation efficace et sûre.

6. **Examinez un scénario où un utilisateur a accidentellement obtenu des permissions excessives sur des fichiers critiques. Quelles pourraient être les conséquences et comment ce problème pourrait-il être résolu ?**
   - Cette question vise à vous faire réfléchir aux impacts potentiels de la surattribution de permissions et aux mesures correctives possibles.

7. **Qu'est-ce que le "sticky bit" et comment peut-il être utilisé pour améliorer la sécurité des répertoires partagés ?**
   - Cette question vous encourage à explorer le concept du sticky bit, son utilité et son application dans la sécurisation des environnements où plusieurs utilisateurs accèdent aux mêmes répertoires.

8. **Discutez de l'importance de la séparation des droits d'utilisateur et d'administrateur sur un système Linux. Comment cela influence-t-il la sécurité générale du système ?**
   - Cette question vous permet de réfléchir à l'importance de la séparation des privilèges dans la prévention des abus de droits et des erreurs accidentelles.

