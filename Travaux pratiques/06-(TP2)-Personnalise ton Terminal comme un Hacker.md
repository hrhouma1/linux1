# TP Linux — "Personnalise ton Terminal comme un Hacker !"


# Objectifs pédagogiques

- Modifier l’apparence du terminal (couleurs, messages, prompt)
- Découvrir les fichiers `.bashrc`, `.profile` et leurs rôles
- Créer un message d’accueil stylisé (ASCII Art, couleurs)
- Bonus : animations ASCII, sons, alias rigolos
- Manipuler les permissions et comprendre les effets sur les fichiers de configuration


<br/>

# Étape 1 – Crée un utilisateur fun et connecte-toi

```bash
sudo adduser superfun
su - superfun
```

*Malheureusement, cela ne fonctionne pas, car l'utilisateur superfun ne fait pas partie de la liste des sudoers.*

Il faut ajouter l'utilisateur `superfun` à la liste des **sudoers** (donc lui donner les droits d'administration), juste après la création de l'utilisateur :


## Étape 1.1 – Donne les droits sudo à ton utilisateur fun

```bash
# Ajoute superfun au groupe sudo (sur Debian/Ubuntu)
sudo usermod -aG sudo superfun
```

*Cela ajoute `superfun` au groupe `sudo`, ce qui lui permet d'exécuter des commandes avec les privilèges d'administrateur.*



## Étape 1.2 – Pour vérifier :

Connecte-toi en tant que `superfun` :

```bash
su - superfun
```

Puis teste une commande avec `sudo` :

```bash
sudo ls /root
```

*Si tout fonctionne, il te demande le mot de passe de `superfun`, puis exécute la commande.*



<br/>

# Étape 2 – Affiche un message de bienvenue en ASCII Art

### 2.1. Installe `figlet` et `toilet` :

```bash
sudo apt update
sudo apt install figlet toilet -y
```

### 2.2. Ajoute un message d’accueil dans `.bashrc` :

```bash
echo 'figlet "Bienvenue !"' >> ~/.bashrc
```

Ou encore plus stylisé avec `toilet` :

```bash
echo 'toilet -f mono12 -F metal "Hello Hacker!"' >> ~/.bashrc
```



### 2.3. Déconnexion et reconnexion :




Déconnecte-toi puis reconnecte-toi pour voir le résultat !

```bash
exit
su
```


Reconnecte-toi en tant que `superfun` :

```bash
su - superfun
```

<br/>



### 2.4. Supprimer le message d'accueil du `.bashrc` (À faire plus tard)

Avant de supprimer le message d'accueil, il est **très important de savoir** sous quel utilisateur vous devez effectuer l'opération.

> **Question essentielle : dois-je le faire en tant que `root` ou en tant que `superfun` ?**  
> ⚠️ C’est une décision très importante : si vous utilisez le mauvais utilisateur, **la suppression ne fonctionnera pas** !



#### 2.4.1. Vérifier l'utilisateur courant

Voici deux façons de vérifier quel utilisateur est actif :

```bash
su
whoami
exit
```

ou

```bash
su - superfun
whoami
exit
```

> La commande `whoami` vous dira si vous êtes bien connecté en tant que `superfun` ou `root`.



#### 2.4.2. Suppression des lignes ajoutées dans le `.bashrc`

Pour supprimer les messages d’accueil du terminal (affichés à chaque ouverture), exécutez les commandes suivantes **en tant que l'utilisateur concerné** (probablement `superfun`) :

```bash
sed -i '/figlet "Bienvenue !"/d' ~/.bashrc
```

```bash
sed -i '/toilet -f mono12 -F metal "Hello Hacker!"/d' ~/.bashrc
```

> Ces commandes :
- recherchent les lignes exactes mentionnées
- les suppriment du fichier `.bashrc` (`-i` pour modifier le fichier en place)



#### 2.4.3. 🔄 Recharger le fichier `.bashrc`

Après la suppression, rechargez le fichier pour appliquer les changements immédiatement :

```bash
source ~/.bashrc
```





#### 2.4.4. Résumé

> Pour **annuler** ou **supprimer** la ligne ci-haut que tu as ajoutée dans ton `~/.bashrc`, exécutez la commande suivante :

```bash
sed -i '/figlet "Bienvenue !"/d' ~/.bashrc
```

```bash
sed -i '/toilet -f mono12 -F metal "Hello Hacker!"/d' ~/.bashrc
```

> Cette commande :
- cherche la ligne contenant exactement `toilet -f mono12 -F metal "Hello Hacker!"`
- et la **supprime** du fichier `.bashrc` (`-i` modifie le fichier directement).

Ensuite, pour recharger ton .bashrc sans redémarrer :

```bash
source ~/.bashrc
```








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

<br/>

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


<br/>

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


<br/>

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


<br/>

# Étape 7 – Questions de réflexion

1. Quelle est la différence entre `.bashrc`, `.bash_profile` et `.profile` ?
2. Pourquoi est-il déconseillé de mettre trop de commandes lourdes dans `.bashrc` ?
3. Que se passe-t-il si `.bashrc` contient une erreur de syntaxe ?
4. Que signifie le `\[\e[1;36m\]` utilisé dans le `PS1` ?
5. Quelle commande permet de réinitialiser `.bashrc` à sa version par défaut ?



## ✅ À faire valider

- Le terminal affiche un message personnalisé
- Le prompt est coloré
- Des alias rigolos sont définis
- Un effet visuel (ASCII / matrix / cowsay) fonctionne
- Le fichier `.bashrc` est protégé
- Les questions sont complétées dans un fichier `questions.txt`
