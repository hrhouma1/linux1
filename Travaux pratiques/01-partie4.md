#  Vulgarisation : `.bashrc`, `.profile`, `.bash_profile` avec des exemples concrets


#  1- Objectif : 

*comprendre "quand" et "pourquoi" Linux lit ces fichiers*

Ces fichiers sont comme des **scripts d’accueil** que ton terminal exécute **quand tu ouvres une session** ou **un terminal**.  
Chacun a un rôle **spécifique** selon le **contexte**.



# 2 - Analogie simple : la boîte d’entrée

Imagine ton système comme une entreprise. Quand tu arrives au boulot :

- Tu passes **à l’accueil principal** le matin → on t’enregistre et on te donne un café.
- Ensuite, tu vas dans **ton bureau** → ton ordinateur, ton décor, tes réglages personnels se chargent.

→ En **Linux** :
- L’accueil = **Shell de login** → charge `.profile` ou `.bash_profile`
- Ton bureau = **Terminal** → charge `.bashrc`



# 3 - Que sont ces fichiers, vraiment ?

| Fichier            | Quand est-il utilisé ?                                                  | Exemple concret |
|--------------------|--------------------------------------------------------------------------|-----------------|
| `.bash_profile`    | Quand tu **te connectes à Linux** (login via SSH, console, ou `su -`)    | Tu te connectes au serveur via `ssh` |
| `.profile`         | Pareil, mais pour tous les shells (non bash aussi, comme `sh`, `dash`)   | Tu te connectes sur Ubuntu avec un shell par défaut |
| `.bashrc`          | Quand tu **ouvres un terminal normal** dans ton interface graphique      | Tu ouvres GNOME Terminal sur Ubuntu |
| `/etc/profile`     | Chargé au tout début pour **tous les utilisateurs**                     | Règles communes à tous |
| `/etc/bash.bashrc` | Chargé pour tous les utilisateurs, quand ils ouvrent un terminal Bash    | Alias pour tous |



# 4 - Exemple de la vraie vie : imagine que tu écris dans ces fichiers

## 4.1. Tu ajoutes ceci dans `/home/etudiant/.bashrc`

```bash
echo "Bienvenue dans le terminal"
```

## 4.2. Tu ajoutes ceci dans `/home/etudiant/.bash_profile`

```bash
echo "Bienvenue à la connexion (login shell)"
```



## 4.3. Tu fais ces tests :

#### 4.3.1. Tu ouvres juste un terminal normal (Ctrl+Alt+T) :
- Tu verras : `Bienvenue dans le terminal`
- Car `.bashrc` est exécuté ✅
- Mais `.bash_profile` **n’est pas** exécuté ❌

#### 4.3.2. Tu te connectes en SSH :
```bash
ssh etudiant@serveur
```
- Tu verras : `Bienvenue à la connexion (login shell)`
- Car `.bash_profile` est exécuté ✅
- Mais `.bashrc` **pas tout de suite** ❌ sauf si tu le rappelles dedans

#### 4.3.3. Tu fais :
```bash
sudo -i
```
- Tu es **root**
- Tu verras le message de `/root/.bash_profile`

#### 4.4.4. Tu fais :
```bash
sudo -s
```
- Tu es **root**
- Tu verras le message de **ton** `.bashrc` (pas celui de root)



# 5. Pourquoi c’est parfois confus ?

Parce que certains fichiers s’appellent presque pareil et sont lus **dans des cas différents**.

- `.bash_profile` = Quand tu ouvres une **vraie session (login shell)** → ex: ssh, `su -`, `sudo -i`
- `.bashrc` = Quand tu ouvres un **terminal normal** → ex: GNOME Terminal, `bash`

> **Astuce de pro** : Mets toujours ça dans ton `.bash_profile` pour qu’il exécute aussi `.bashrc` :

```bash
if [ -f ~/.bashrc ]; then
   . ~/.bashrc
fi
```



# 6. Encore plus d’exemples pratiques

### 6.1. Tu veux colorer ton terminal à chaque ouverture ?
→ Mets les couleurs dans `.bashrc`

### 2. Tu veux que certaines variables soient définies **à la connexion SSH** ?
→ Mets-les dans `.bash_profile` ou `.profile`

### 6.3. Tu veux que tous les utilisateurs aient un alias commun ?
→ Ajoute dans `/etc/bash.bashrc`



# 7. Résumé simplifié (tableau clair)

| Action                         | Fichier exécuté                  | Exemple réel                        |
|-------------------------------|----------------------------------|-------------------------------------|
| Ouvrir un terminal graphique  | `.bashrc`                        | Ctrl+Alt+T sur Ubuntu               |
| Se connecter en SSH           | `.bash_profile` puis `.bashrc`  | `ssh etudiant@ip`                   |
| Faire `sudo -s`               | `.bashrc` **de l’utilisateur**  | sudo root sans changement d’env     |
| Faire `sudo -i`               | `.bash_profile` **du root**     | comme une vraie connexion root      |
| Faire `su`                    | aucun fichier (basique)         | se connecter root sans `-`          |
| Faire `su -`                  | `.bash_profile` du root         | se connecter root avec login shell  |



# 8. Bonus : commandes pour voir ce qui est lu

```bash
# Savoir quel shell tu utilises
echo $0

# Voir ton HOME (important pour savoir où chercher .bashrc)
echo $HOME

# Qui est connecté réellement ?
whoami

# Vérifier si .bashrc a été lu
cat ~/.bashrc
```


# 9. À retenir 

- `.bashrc` = **quand tu ouvres un terminal normal**
- `.bash_profile` = **quand tu te connectes à une session**
- `sudo -s` = shell root avec ton environnement → lit ton `.bashrc`
- `sudo -i` = shell root complet → lit les fichiers de `root`

