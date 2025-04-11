
# Cours Linux — Comprendre les Permissions avec `ls -l`

## Objectif du cours

Ce cours a pour but d'expliquer de manière claire et visuelle le système de permissions sous Linux, en utilisant la commande `ls -l`. Vous apprendrez à lire et interpréter les autorisations liées aux fichiers et dossiers.



## Présentation générale des permissions

La commande suivante permet d’afficher les détails des fichiers et répertoires :

```bash
ls -l
```

Elle retourne une liste avec des lignes comme :

```
-rwxr-xr--
```

Chaque ligne vous donne des informations précieuses sur les droits d’accès. Voici un tableau ASCII pédagogique et clair pour expliquer les permissions affichées par la commande `ls -l` en Linux. Ce tableau distingue les fichiers et dossiers, et explique les différentes combinaisons de permissions (lecture, écriture, exécution) pour l'utilisateur, le groupe, et les autres.



```
+---------------------------------------------------------------+
| Syntaxe       | Type    | Lecture | Écriture | Exécution | Description                 |
+--------------+---------+---------+----------+-----------+-----------------------------+
| -r--r--r--    | fichier |   Oui   |   Non    |    Non    | Fichier en lecture seule     |
| -rw-r--r--    | fichier |   Oui   |   Oui    |    Non    | Fichier modifiable par le    |
|              |         |         |          |           | propriétaire uniquement      |
| -rwxr-xr-x    | fichier |   Oui   |   Oui    |   Oui    | Script ou binaire exécutable |
| drwxr-xr-x    | dossier |   Oui   |   Oui    |   Oui    | Dossier accessible (cd)      |
| drwx------    | dossier |   Oui   |   Oui    |   Oui    | Dossier privé (proprio seul) |
| -rw-------    | fichier |   Oui   |   Oui    |    Non    | Fichier privé du propriétaire|
| -rwx------    | fichier |   Oui   |   Oui    |   Oui    | Fichier exécutable privé     |
+--------------+---------+---------+----------+-----------+-----------------------------+
```



## Explication pédagogique

Chaque ligne affichée par `ls -l` commence par une série de 10 caractères :

```
-rwxr-xr--
```

Décomposons-les en trois parties distinctes.



### 1. Premier caractère : type de fichier

| Caractère | Signification        |
|-----------|----------------------|
| `-`       | Fichier normal       |
| `d`       | Dossier              |
| `l`       | Lien symbolique      |



### 2. Les 9 caractères suivants : les permissions

Ces caractères sont divisés en trois groupes :

| Plage de caractères | Représente        | Exemple (`-rwxr-xr--`) |
|---------------------|-------------------|-------------------------|
| Positions 2 à 4     | Propriétaire      | `rwx`                   |
| Positions 5 à 7     | Groupe            | `r-x`                   |
| Positions 8 à 10    | Autres (public)   | `r--`                   |

---

### 3. Signification des lettres

| Lettre | Signification           |
|--------|--------------------------|
| `r`    | Lecture (Read)           |
| `w`    | Écriture (Write)         |
| `x`    | Exécution (Execute)      |
| `-`    | Permission absente       |



## Exemple illustré

Voici une interprétation visuelle d’une permission typique :

```
-rw-r--r--
││ │  │  │
││ │  │  └── Autres : lecture seule
││ │  └──── Groupe : lecture seule
││ └─────── Propriétaire : lecture + écriture
│└───────── Type : fichier normal
└────────── Résumé : Fichier modifiable par le propriétaire uniquement
```



## Exercices de test

Créez un fichier et appliquez des permissions différentes :

```bash
touch exemple.txt
chmod 644 exemple.txt
ls -l exemple.txt
```

Autres exemples utiles :

```bash
chmod 755 script.sh
chmod 700 secret.txt
```



## À retenir

- Les permissions sont fondamentales pour la sécurité du système.
- Les 10 caractères visibles dans `ls -l` résument les droits d'accès.
- La commande `chmod` modifie les permissions.
- La commande `chown` permet de changer le propriétaire ou le groupe.







<br/>
<br/>


# Travaux Pratiques — Manipulation des permissions sous Linux

## Objectif du TP

Ce TP a pour but de **manipuler les permissions sur les fichiers et dossiers**, et de **comprendre l’impact réel** des commandes `chmod`, `chown` et `ls -l` sur un système Linux.



## Prérequis

- Avoir un terminal Linux ou un environnement en ligne (VM, WSL, etc.).
- Accès à un compte utilisateur non-root (mais possibilité d'utiliser `sudo` si nécessaire).



## Étape 1 — Créer un environnement de test

```bash
mkdir ~/tp_permissions
cd ~/tp_permissions

touch fichier1.txt
mkdir dossier1
```

Vérifiez les permissions initiales :

```bash
ls -l
```



## Étape 2 — Tester différentes permissions sur un fichier

1. Donnez la lecture seule au fichier :

```bash
chmod 444 fichier1.txt
ls -l fichier1.txt
```

Essayez de le modifier :

```bash
echo "test" > fichier1.txt  # Devrait échouer
```

2. Donnez l’écriture seule :

```bash
chmod 222 fichier1.txt
ls -l fichier1.txt
cat fichier1.txt  # Devrait échouer
```

3. Donnez les droits complets :

```bash
chmod 777 fichier1.txt
ls -l fichier1.txt
```

Observez et notez les effets.



## Étape 3 — Créer un fichier exécutable

```bash
echo -e '#!/bin/bash\necho "Bonjour depuis script.sh"' > script.sh
chmod 755 script.sh
ls -l script.sh
./script.sh
```

Testez ensuite :

```bash
chmod -x script.sh
./script.sh  # Devrait échouer
```



## Étape 4 — Manipuler les permissions d’un dossier

```bash
mkdir secret
touch secret/fichier_secret.txt
chmod 700 secret
ls -ld secret
```

Essayez avec un autre utilisateur (ou créez un faux utilisateur si possible).



## Étape 5 — Modifier le propriétaire et le groupe

```bash
sudo chown $USER:$USER fichier1.txt
ls -l fichier1.txt
```

Changez temporairement de propriétaire :

```bash
sudo useradd temporaire
sudo chown temporaire:temporaire fichier1.txt
ls -l fichier1.txt
```

Revenez au propriétaire initial :

```bash
sudo chown $USER:$USER fichier1.txt
```



## Étape 6 — Nettoyage

```bash
cd ~
rm -r ~/tp_permissions
sudo userdel -r temporaire
```



## Questions de réflexion

1. Quelles permissions sont nécessaires pour qu’un utilisateur puisse :
   - Lire un fichier ?
   - Modifier un fichier ?
   - Lister le contenu d’un dossier ?
   - Entrer dans un dossier ?
   - Exécuter un script ?

2. Quelle différence existe-t-il entre :
   - `chmod 755 fichier.sh`  
   - `chmod 700 fichier.sh`  
   - `chmod 777 fichier.sh` ?

3. Pourquoi est-il dangereux de mettre `777` à un fichier ou dossier sensible ?

