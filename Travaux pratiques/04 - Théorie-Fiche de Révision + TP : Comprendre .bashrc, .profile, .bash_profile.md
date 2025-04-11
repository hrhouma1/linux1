# Fiche de Révision + TP : Comprendre `.bashrc`, `.profile`, `.bash_profile` 



# Objectif pédagogique

- Comprendre quand Linux lit les fichiers `.bashrc`, `.profile`, `.bash_profile`
- Savoir les tester avec des exemples concrets
- Savoir la différence entre une **connexion** et une **ouverture de terminal**
- Comprendre ce que fait `sudo -s` et `sudo -i`
- Retenir ce qu’il faut modifier selon ce qu’on veut faire



# Partie 1 : Comprendre les fichiers

| Fichier            | Quand est-il utilisé ?                          | Ce qu’il configure            |
|--------------------|--------------------------------------------------|-------------------------------|
| `.bashrc`          | Quand on **ouvre un terminal** (Ctrl+Alt+T)     | Le prompt, les alias, les couleurs |
| `.bash_profile`    | Quand on **se connecte** (SSH, `sudo -i`, etc.) | Variables globales, PATH, etc. |
| `.profile`         | Pareil que `.bash_profile`, mais pour tous les shells (sh, dash...) | À la connexion |



# Partie 2 : Expériences guidées (TP simple)

 **But** : Écrire du texte dans chaque fichier pour voir **quand** il est utilisé.



###  Étape 1 : Préparer les tests

**Connectez-vous avec votre utilisateur (par exemple : `etudiant`)**  
Puis tapez ceci :

```bash
echo 'JE SUIS DANS .bashrc' >> ~/.bashrc
echo 'JE SUIS DANS .bash_profile' >> ~/.bash_profile
```

>  Ces lignes vont écrire une phrase dans vos fichiers, pour qu’on les voie ensuite.



###  Étape 2 : Fermer et rouvrir un terminal

Fermez complètement votre terminal et rouvrez-le.

👉 **Attendu** : Vous verrez s’afficher `JE SUIS DANS .bashrc`  
✅ Cela prouve que `.bashrc` est lu à l’ouverture d’un terminal **normal**.



###  Étape 3 : Se connecter avec sudo -i

Tapez ensuite :

```bash
sudo -i
```

👉 **Attendu** : Vous ne verrez PAS la phrase, sauf si vous modifiez le `.bash_profile` de root !

```bash
echo 'JE SUIS DANS LE .bash_profile DU ROOT' >> /root/.bash_profile
```

Puis refaites :

```bash
sudo -i
```

✅ Vous verrez alors la phrase ! → Preuve que `.bash_profile` est lu quand on fait `sudo -i`.



###  Étape 4 : Tester `sudo -s`

Tapez :

```bash
sudo -s
```

👉 **Attendu** : Vous verrez `JE SUIS DANS .bashrc` (celui de votre utilisateur, pas celui de root)

✅ Cela montre que `sudo -s` garde l’environnement **de l’utilisateur courant**.



##  Partie 3 : Résumé visuel pour vos mémoires

| Action                      | Fichier utilisé        | Chargé par           | Exemple |
|-----------------------------|------------------------|-----------------------|---------|
| Ouvrir un terminal normal   | `.bashrc`              | Terminal graphique    | Ctrl+Alt+T |
| Se connecter en SSH         | `.bash_profile`        | Shell de login        | ssh user@ip |
| `sudo -i`                   | `/root/.bash_profile`  | Shell root complet    | sudo -i |
| `sudo -s`                   | `.bashrc` de l’utilisateur | Shell simple root | sudo -s |



##  Partie 4 : Ce qu’on peut mettre dans ces fichiers

### Exemple dans `.bashrc` :
```bash
alias ll='ls -la'
export PATH="$PATH:/opt/monprogramme"
```

### Exemple dans `.bash_profile` :
```bash
export LANG=fr_FR.UTF-8
```



##  Partie 5 : Nettoyer les fichiers après le TP

```bash
# Nettoyer les fichiers test
sed -i '/JE SUIS DANS/d' ~/.bashrc
sed -i '/JE SUIS DANS/d' ~/.bash_profile
sudo sed -i '/JE SUIS DANS/d' /root/.bash_profile
```


## ✅ Partie 6 : À retenir par cœur

🟢 `.bashrc` = **Terminal normal**  
🟢 `.bash_profile` = **Connexion (SSH, sudo -i)**  
🟢 `sudo -s` = root avec environnement **de l’utilisateur courant**  
🟢 `sudo -i` = root avec **environnement root complet**



## ✍️ Exercice 

1. Créez un fichier `.bashrc` avec un message personnalisé.
2. Créez un fichier `.bash_profile` avec un message différent.
3. Testez `sudo -s` et `sudo -i` pour voir les différences.
4. Prenez 3 captures d’écran : 
   - Une avec l’ouverture d’un terminal
   - Une avec `sudo -s`
   - Une avec `sudo -i`
5. Remplissez ce tableau :

| Action        | Fichier utilisé      | Message affiché |
|---------------|----------------------|------------------|
| Terminal      |                      |                  |
| sudo -s       |                      |                  |
| sudo -i       |                      |                  |

