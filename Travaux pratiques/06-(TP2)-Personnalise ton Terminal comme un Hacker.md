04-(TP2)-Gestion-des-permissions.md
# TP Linux — "Personnalise ton Terminal comme un Hacker !"


# Objectifs pédagogiques

- Modifier l’apparence du terminal (couleurs, messages, prompt)
- Découvrir les fichiers `.bashrc`, `.profile` et leurs rôles
- Créer un message d’accueil stylisé (ASCII Art, couleurs)
- Bonus : animations ASCII, sons, alias rigolos
- Manipuler les permissions et comprendre les effets sur les fichiers de configuration



# Étape 1 – Crée un utilisateur fun et connecte-toi

```bash
sudo adduser superfun
su - superfun
```



# Étape 2 – Affiche un message de bienvenue en ASCII Art

### Installe `figlet` et `toilet` :

```bash
sudo apt update
sudo apt install figlet toilet -y
```

### Ajoute un message d’accueil dans `.bashrc` :

```bash
echo 'figlet "Bienvenue !"' >> ~/.bashrc
```

Ou encore plus stylisé avec `toilet` :

```bash
echo 'toilet -f mono12 -F metal "Hello Hacker!"' >> ~/.bashrc
```

Déconnecte-toi puis reconnecte-toi pour voir le résultat !



# Étape 3 – Change la couleur du prompt

Modifie la variable `PS1` dans `.bashrc` :

```bash
echo 'export PS1="\[\e[1;36m\][\u@\h \W]\$\[\e[0m\] "' >> ~/.bashrc
```

> 🔵 Cela met ton prompt en cyan avec le nom d’utilisateur et le répertoire courant.

Recharge :

```bash
source ~/.bashrc
```



# Étape 4 – Crée un alias rigolo

Ajoute dans `.bashrc` :

```bash
echo "alias matrix='cmatrix'" >> ~/.bashrc
```

Installe `cmatrix` (si dispo) :

```bash
sudo apt install cmatrix -y
```

Puis teste :

```bash
matrix
```



# Étape 5 – Crée un mini écran de démarrage personnalisé

Édite `.bashrc` et ajoute :

```bash
echo ""
echo "Bienvenue, $USER ! Nous sommes le $(date)"
echo ""
figlet "Amuse-toi !"
fortune | cowsay
```

Installe les programmes :

```bash
sudo apt install fortune cowsay -y
```



# Étape 6 – Protection contre la modification par d'autres utilisateurs

Rends `.bashrc` en lecture seule :

```bash
chmod 400 ~/.bashrc
```

Essaie ensuite de le modifier… Résultat ?

---

## 💡 Bonus – Ajoute une musique au démarrage

(🎵 Requiert `mpg123` et un fichier `.mp3` à disposition)

```bash
sudo apt install mpg123 -y
echo 'mpg123 ~/musique.mp3 &' >> ~/.bashrc
```

---

## 🔍 Étape 7 – Questions de réflexion

1. Quelle est la différence entre `.bashrc`, `.bash_profile` et `.profile` ?
2. Pourquoi est-il déconseillé de mettre trop de commandes lourdes dans `.bashrc` ?
3. Que se passe-t-il si `.bashrc` contient une erreur de syntaxe ?
4. Que signifie le `\[\e[1;36m\]` utilisé dans le `PS1` ?
5. Quelle commande permet de réinitialiser `.bashrc` à sa version par défaut ?

---

## ✅ À faire valider

- Le terminal affiche un message personnalisé
- Le prompt est coloré
- Des alias rigolos sont définis
- Un effet visuel (ASCII / matrix / cowsay) fonctionne
- Le fichier `.bashrc` est protégé
- Les questions sont complétées dans un fichier `questions.txt`
