# Document de Priorité de Chargement des Fichiers : `.bash_profile`, `.profile`, `.bashrc`

<br/>
<br/>


# Partie 1



Lorsque vous utilisez un système Linux, vous interagissez souvent avec un terminal pour exécuter des commandes. Ce terminal est piloté par un **interpréteur de commandes**, souvent Bash (`/bin/bash`). À chaque ouverture de terminal ou connexion à la machine, **Linux charge automatiquement certains fichiers de configuration** pour préparer votre environnement de travail.

Parmi les fichiers les plus importants à connaître se trouvent :

- `.bash_profile`
- `.profile`
- `.bashrc`

Ces fichiers permettent, par exemple, de :

- Définir des **variables d’environnement**
- Ajouter des **alias**
- Modifier l’**invite de commande (prompt)**
- Exécuter des scripts à l’ouverture d’un terminal

Mais **tous ne sont pas lus de la même façon**, et surtout **pas dans le même ordre**. Comprendre **quand et lequel de ces fichiers est exécuté** est crucial pour diagnostiquer des comportements inattendus ou configurer correctement son environnement.



## 1.1. Pourquoi est-ce important ?

Imaginons que vous ajoutiez un alias ou une variable dans `.profile`, mais que rien ne se passe lors de votre connexion. Ou encore, vous modifiez `.bashrc`, mais cela ne change rien quand vous vous connectez en SSH.

Sans comprendre la **logique de chargement** de ces fichiers, il est difficile de configurer un environnement cohérent.



## 1.2. Objectif de ce pragraphe

Ce cours vous guide pas à pas pour :

- Comprendre **la différence entre un shell de login et un shell non-login**
- Savoir **quel fichier est lu dans quel contexte**
- Apprendre à **tester et vérifier le comportement** de votre configuration
- Appliquer les **bonnes pratiques** pour un environnement propre et cohérent



💡 **À la fin de cette leçon, vous saurez exactement :**

- Quand `.bashrc`, `.bash_profile` ou `.profile` est utilisé
- Pourquoi certains fichiers semblent "ignorés"
- Comment organiser vos fichiers pour qu'ils soient toujours pris en compte



## 1.2. Objectif de ce pragraphe

Sous Linux, à chaque ouverture de terminal ou connexion à une session, le système lit automatiquement certains fichiers de configuration afin de :

- Préparer votre environnement de travail
- Charger des variables (`PATH`, `EDITOR`, etc.)
- Exécuter des scripts ou définir des alias
- Modifier le comportement du shell (`PS1`, couleurs, etc.)

Mais **tous les fichiers ne sont pas toujours lus**, et **cela dépend du type de shell** que vous utilisez.



##  1.3. Quand chaque fichier est-il chargé ?

Voici un tableau synthétique :

| Action de l'utilisateur                  | Type de shell      | Fichiers lus                                 | Fichiers ignorés                            |
|------------------------------------------|---------------------|-----------------------------------------------|---------------------------------------------|
| Ouverture d’un terminal (`Ctrl+Alt+T`)   | Non-login shell     | ✅ `.bashrc`                                  | ❌ `.bash_profile`, `.profile`               |
| Connexion SSH (`ssh user@host`)          | Login shell         | ✅ `.bash_profile` → ou `.profile` si l'autre n'existe pas | ❌ `.bashrc`                                 |
| `sudo -s`                                | Non-login shell     | ✅ `.bashrc` (de l’utilisateur courant)        | ❌ `.bash_profile`, `.profile`               |
| `sudo -i`                                | Login shell (root)  | ✅ `/root/.bash_profile`                      | ❌ `/root/.profile`, `/root/.bashrc` (si non inclus manuellement) |
| `su -` ou `su -l`                        | Login shell         | ✅ `.bash_profile` → ou `.profile`            | ❌ `.bashrc`                                 |
| `bash` lancé manuellement                | Non-login shell     | ✅ `.bashrc`                                  | ❌ `.bash_profile`, `.profile`               |



##  1.4. Résumé simple

- **.bash_profile** → Chargé **seulement dans un shell de login**
- **.profile** → Lu **si .bash_profile est absent**
- **.bashrc** → Chargé **dans tous les shells interactifs non-login** (ex: terminal graphique)



##  1.5. Bonnes pratiques

Pour garantir un comportement cohérent dans tous les cas, on recommande **d’inclure `.bashrc` dans `.bash_profile`**, comme ceci :

```bash
# Dans ~/.bash_profile
if [ -f ~/.bashrc ]; then
  . ~/.bashrc
fi
```

Ainsi, même dans un shell de login, `.bashrc` sera exécuté également.




# Partie 2

## 2.1 - Objectif

Comprendre **l'ordre dans lequel Linux charge les fichiers de configuration shell** (`.bash_profile`, `.profile`, `.bashrc`) pour un utilisateur donné, et savoir **qui est prioritaire sur qui**.



## 2.1. Règle d'or : **.bash_profile a priorité sur .profile**

> 🔁 Autrement dit :  
> **Si `.bash_profile` existe, `.profile` est ignoré**.



## 2.2. Priorité de chargement selon le type de shell

### 🟡 2.2.1. Shell de **login** (connexion, `ssh`, `su -`, `sudo -i`) :

Linux cherche dans cet ordre :

1. `.bash_profile` ✅
2. `.bash_login` ❌ (rarement utilisé)
3. `.profile` ✅ **(uniquement si les deux premiers n’existent pas)**

> 📌 Donc : **Si `.bash_profile` est présent, `.profile` ne sera jamais lu.**



### 🟢 2.2.2. Shell **interactif non-login** (ex: terminal GNOME, `bash`) :

Linux exécute seulement :

- `.bashrc`

> 🔎 C’est pour ça qu’on ne voit pas les exports définis dans `.bash_profile` quand on ouvre juste un terminal.


<br/>
<br/>

# 2.3. Exemples de vraie vie

### 2.3.1. Exemple 1 : `.bash_profile` existe

- Tu ouvres une session SSH → `.bash_profile` est lu
- `.profile` est **ignoré**
- Tu ouvres un terminal → `.bashrc` est lu

### 2.3.2. Exemple 2 : `.bash_profile` n’existe PAS

- Tu ouvres une session SSH → `.profile` est lu
- Tu ouvres un terminal → `.bashrc` est lu


<br/>
<br/>

# 2.3.3. Résumé des priorités

| Type de shell               | Fichier chargé en priorité                | Fichier ignoré                   |
|-----------------------------|-------------------------------------------|----------------------------------|
| Login shell (`ssh`, `su -`) | `.bash_profile` → `.bash_login` → `.profile` | Les suivants sont ignorés si un est trouvé |
| Terminal (`bash`)           | `.bashrc`                                 | `.bash_profile` et `.profile` ne sont pas lus |


<br/>
<br/>

# 2.4. Astuce recommandée pour tous

Dans `.bash_profile`, ajoute toujours :

```bash
# Pour que bashrc soit exécuté même lors d'un login shell
if [ -f ~/.bashrc ]; then
  . ~/.bashrc
fi
```

 Cela permet d'avoir **un comportement cohérent** : `.bashrc` sera toujours exécuté, même après une connexion SSH ou un `sudo -i`.



<br/>
<br/>

# 2.5. Expérience rapide à faire

```bash
# Créez ou modifiez vos fichiers
echo "LU : .bash_profile" >> ~/.bash_profile
echo "LU : .profile" >> ~/.profile
echo "LU : .bashrc" >> ~/.bashrc

# Test 1 : ouvrir un terminal graphique
# Résultat attendu : "LU : .bashrc"

# Test 2 : se connecter avec ssh ou sudo -i
# Résultat attendu : "LU : .bash_profile"
# -> "LU : .profile" n'apparaît pas

# Test 3 : supprimer .bash_profile, puis refaire sudo -i
# Résultat attendu : "LU : .profile"
```


<br/>
<br/>

# 2.6. Conclusion 

- `.bash_profile` bloque `.profile`
- `.bashrc` est toujours indépendant, il ne se déclenche que dans un terminal
- Pour les shells de login, `.bash_profile` est prioritaire











<br/>
<br/>

# Pratique 1 : Comprendre `.bashrc`, `.bash_profile`, `.profile` par la pratique

<br/>

## Pratique 1.1. Objectif

- Comprendre ce qu’est un **shell de login** et un **shell non-login**
- Expérimenter le comportement réel de `.bashrc`, `.bash_profile` et `.profile`
- Observer quel fichier est utilisé selon le type d’ouverture (terminal, SSH, sudo, etc.)



## Pratique 1.2.  Rappel ultra simplifié

| Situation (la vraie vie)                  | Ce que Linux voit            | Quel fichier est lu             |
|-------------------------------------------|-------------------------------|----------------------------------|
| Tu ouvres un terminal (Ctrl+Alt+T)        | Shell **non-login**          | `.bashrc` uniquement ✅           |
| Tu te connectes avec `ssh`                | Shell **de login**           | `.bash_profile` ou `.profile` ✅ |
| Tu fais `sudo -i`                         | Shell **de login root**      | `/root/.bash_profile` ✅         |
| Tu fais `sudo -s`                         | Shell **non-login root**     | Ton `.bashrc` est utilisé ✅     |



## Pratique 1.3. Préparation des fichiers de test

Tape ces commandes pour injecter une phrase dans chaque fichier :

```bash
echo "LU : .bashrc" >> ~/.bashrc
echo "LU : .bash_profile" >> ~/.bash_profile
echo "LU : .profile" >> ~/.profile
```

 Ces phrases s’afficheront si le fichier est lu.



## Pratique 1.4. Tester avec un terminal normal

### Étapes :

1. Ferme tous tes terminaux.
2. Ouvre un nouveau terminal (ex: Ctrl+Alt+T ou via menu).
3. Observe.

### Résultat attendu :

```
LU : .bashrc
```

**Explication** : seul `.bashrc` est exécuté dans un terminal normal.



## Pratique 1.5. Tester avec sudo -i

### Étapes :

1. Tape la commande :
```bash
sudo -i
```

2. Regarde ce qui s’affiche.

### Résultat attendu :

```
LU : .bash_profile
```

**Explication** : sudo -i ouvre un **shell de login root**, donc lit `.bash_profile`.  
`.profile` est ignoré car `.bash_profile` existe.



## Pratique 1.6. Supprimer `.bash_profile` et retester

### Étapes :

1. Supprime `.bash_profile` :
```bash
rm ~/.bash_profile
```

2. Tape ensuite :
```bash
sudo -i
```

3. Regarde ce qui s’affiche.

### Résultat attendu :

```
LU : .profile
```

 **Explication** : comme `.bash_profile` n’existe plus, Linux lit `.profile`.



## Pratique 1.7.  Tester avec sudo -s

```bash
sudo -s
```

### Résultat attendu :

```
LU : .bashrc
```

**Explication** : sudo -s garde ton environnement utilisateur. C’est comme si tu restais dans ton terminal → `.bashrc` est exécuté.



## Pratique 1.8.  Nettoyage des fichiers après le TP

```bash
sed -i '/LU :/d' ~/.bashrc
sed -i '/LU :/d' ~/.bash_profile
sed -i '/LU :/d' ~/.profile
```



<br/>
<br/>

## 📎 ANNEXE – Définition vulgarisée des shells

<br/>
<br/>

### 🟢 Shell de login = Connexion à une session

- Tu te **connectes** à une machine (via SSH ou `sudo -i` ou `su -`)
- Linux te dit : “Bienvenue, je te charge ton environnement complet”
- Il lit : `.bash_profile`, ou `.profile` si le premier n’existe pas

🧠 Exemple : Tu arrives à l’école → on t’ouvre les portes, on te donne ton sac, ton emploi du temps


<br/>
<br/>

### 🔵 Shell non-login = Tu ouvres juste un terminal

- Tu es **déjà connecté**, tu ouvres juste un **terminal** dans ton bureau
- Linux te dit : “OK, tu es déjà loggé, je te donne juste ton terminal”
- Il lit : `.bashrc`

🧠 Exemple : Tu ouvres ton ordinateur déjà allumé → pas besoin de refaire toute la session, juste ton bureau


<br/>
<br/>

# Résumé final

| Action              | Shell de login ? | Fichier lu            | Remarques |
|---------------------|------------------|------------------------|-----------|
| Terminal (Ctrl+Alt+T)| ❌ Non-login      | `.bashrc`              | Pas de session rechargée |
| `ssh user@host`     | ✅ Oui            | `.bash_profile` ou `.profile` | Chargement complet |
| `sudo -s`           | ❌ Non-login      | `.bashrc` de l’utilisateur | Environnement utilisateur |
| `sudo -i`           | ✅ Oui            | `.bash_profile` de root | Changement complet vers root |



