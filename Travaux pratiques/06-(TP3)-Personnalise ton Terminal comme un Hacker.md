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

*Essayez les commandes suivantes :*

```bash
whoami
sudo apt update
sudo install curl
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
> ```bash
> sed -i '/figlet "Bienvenue !"/d' ~/.bashrc
> ```
> ```bash
> sed -i '/toilet -f mono12 -F metal "Hello Hacker!"/d' ~/.bashrc
> ```
> Cette commande :
> - cherche la ligne contenant exactement `toilet -f mono12 -F metal "Hello Hacker!"`
> - et la **supprime** du fichier `.bashrc` (`-i` modifie le fichier directement).
> Ensuite, pour recharger ton .bashrc sans redémarrer :
> ```bash
> source ~/.bashrc
> ```






<br/>


# Étape 3 – Change la couleur du prompt



🔴 **ATTENTION, ATTENTION, ATTENTION** 🔴  
⚠️ **Avant tout**, **vérifiez toujours quel utilisateur est actif** ❗  
👤 Vous allez modifier des fichiers critiques comme `.bashrc`, donc **assurez-vous de ne pas agir sur le mauvais utilisateur** ⚠️


## 3.1. Changer la couleur du prompt

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


*Félicitations !, tu as ajouté cette ligne à ton `~/.bashrc` :*

```bash
echo 'export PS1="\[\e[1;36m\][\u@\h \W]\$\[\e[0m\] "' >> ~/.bashrc
```

Elle modifie l’apparence de ton **invite de commande (prompt)** en couleur cyan.



## 3.2. Pour **annuler** cette modification, utilise :

```bash
sed -i '/export PS1="\\\[\\e\[1;36m\\\]\\\[\\u@\\h \\W\\\]\\\\\$\\\[\\e\[0m\\\] "/d' ~/.bashrc
```

> Cette commande supprime précisément la ligne ajoutée. Les `\` doivent être **échappés** deux fois dans `sed`.



## 3.3.  Recharge ensuite ton `.bashrc` :

```bash
source ~/.bashrc
```

## 3.4.  Recharge ensuite ton `.bashrc` :

### Si Tu veux revenir à l’invite par défaut sans redémarrer ? Tu peux aussi exécuter directement :

```bash
export PS1='\u@\h:\w\$ '
```

> Cela remet l'invite à une forme plus classique comme : `user@host:/chemin$`






## 3.5.  Problème remarqué (changement temporaire) :


- Malheureusement, si j'exécute su , je reviens au prompt personnalisé.


> La commande  
> ```bash  
> export PS1='\u@\h:\w\$ '  
> ```  
> **change le prompt temporairement**, **seulement pour la session en cours**.



### 3.5.1. **Pourquoi ?**
Parce que :
- Cette commande modifie la variable `PS1` **dans le shell actif uniquement**.
- Dès que tu fermes ton terminal ou ouvres une nouvelle session, **le `.bashrc` est relu**, et si tu n’as pas défini `PS1` dans ce fichier, le système remet ce qu’il a comme valeur par défaut (parfois issue de `/etc/bash.bashrc` ou `/etc/profile` selon ta distro).



### 3.5.2. **Solution permanente**

Si nous voudrons que ce prompt simple soit **appliqué à chaque ouverture de terminal**, tu dois **l’ajouter dans ton `.bashrc`** **à la fin** (et non pas en utilisant `echo >>`, pour éviter les doublons).

Voici comment faire proprement :

```bash
# Ouvre le .bashrc dans un éditeur
nano ~/.bashrc
```

1. Va à la fin du fichier.
2. Ajoute cette ligne :

```bash
export PS1='\u@\h:\w\$ '
```

3. Sauvegarde (Ctrl + O), puis quitte (Ctrl + X).
4. Recharge ton fichier :

```bash
source ~/.bashrc
```



### 3.5.3. Résultat :

Tu auras un prompt simple comme :

```bash
su
superfun@debian:~$
```
… à chaque fois que tu ouvres un terminal.

```bash
su
```

### 3.5.4. Une autre méthode pour les paresseux avec sed sans changer manuellement (optionnel):

*Je vous propose une autre méthode optionnelle mais plus rapide qui montre  **comment ajouter ou remplacer proprement** le prompt `PS1='\u@\h:\w\$ '` de façon **permanente** dans le fichier `~/.bashrc` en utilisant `sed`.*



#### 3.5.4.1. **Supprimer toutes les personnalisations PS1 existantes**

```bash
sed -i '/^export PS1=/d' ~/.bashrc
```

> Cette commande :
- supprime toutes les lignes qui commencent par `export PS1=`
- évite les conflits avec d'anciennes personnalisations



#### 3.5.4.2. **Ajouter un nouveau `PS1` simple à la fin du fichier**

```bash
echo 'export PS1="\u@\h:\w\$ "' >> ~/.bashrc
```



#### 3.5.4.3. **Recharger le fichier pour appliquer le nouveau prompt immédiatement**

```bash
source ~/.bashrc
```



#### 3.5.4.4.  Résumé complet en une ligne (si tu veux faire tout d’un coup) :

```bash
sed -i '/^export PS1=/d' ~/.bashrc && echo 'export PS1="\u@\h:\w\$ "' >> ~/.bashrc && source ~/.bashrc
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


> Pour annuler :
> ```bash
> sed -i "/alias matrix='cmatrix'/d" ~/.bashrc # Supprimer la ligne alias matrix du .bashrc
> source ~/.bashrc # Recharger le .bashrc
> su
>```


*Vous pouvez le faire manuellement aussi :*

> Pour annuler :
> ```bash
> # Ouvrir le fichier .bashrc en édition
> nano ~/.bashrc
> # Supprimer manuellement la ligne : alias matrix='cmatrix'
> # Enregistrer et quitter (Ctrl + X, puis Y, puis Entrée)
> # Recharger le .bashrc
> source ~/.bashrc
> ```


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


<br/>

---
# Annexe 1 - script complet de réinitialisation du `.bashrc`
----


*Ce script supprime proprement les personnalisations que nous avons ajoutées, notamment :*

- `figlet "Bienvenue !"`
- `toilet -f mono12 -F metal "Hello Hacker!"`
- `export PS1=...` (couleur cyan)
- Puis recharge le `.bashrc`



### Annexe 1.1 - **Script : reset_bashrc.sh**

```bash
#!/bin/bash

echo "🔧 Réinitialisation du fichier ~/.bashrc..."

# Supprimer la ligne figlet
sed -i '/figlet "Bienvenue !"/d' ~/.bashrc

# Supprimer la ligne toilet
sed -i '/toilet -f mono12 -F metal "Hello Hacker!"/d' ~/.bashrc

# Supprimer les modifications de PS1 couleur cyan
sed -i '/export PS1="\\\[\\e\[1;36m\\\]\\\[\\u@\\h \\W\\\]\\\\\$\\\[\\e\[0m\\\] "/d' ~/.bashrc

# Optionnel : supprimer toute autre ligne PS1 personnalisée
sed -i '/^export PS1=/d' ~/.bashrc

# Recharger le bashrc
echo "🔁 Rechargement du .bashrc..."
source ~/.bashrc

echo "✅ Réinitialisation terminée. Ouvre un nouveau terminal pour confirmer que tout est revenu à la normale."
```



### Annexe 1.2 - **Comment l’utiliser :**

1. Ouvre un terminal.
2. Crée le fichier :

```bash
nano reset_bashrc.sh
```

3. Colle le script ci-dessus.
4. Sauvegarde (Ctrl + O, Entrée), puis quitte (Ctrl + X).
5. Donne les droits d'exécution :

```bash
chmod +x reset_bashrc.sh
```

6. Lance le script :

```bash
./reset_bashrc.sh
```






<br/> 

---
# Annexe 2 - Réinitialiser et Nettoyer Son .bashrc : Supprimer les Messages Personnalisés et Restaurer un Prompt Sobre
----



###  2.1. **Objectif du script :**
- Sauvegarder automatiquement l'ancien `.bashrc`
- Supprimer toutes les personnalisations suivantes :
  - `figlet "Bienvenue !"`
  - `toilet -f mono12 -F metal "Hello Hacker!"`
  - toutes les lignes `export PS1=...`
- Ajouter un prompt standard simple
- Recharger `.bashrc`



### 2.2. **Script : `reset_bashrc.sh`**

```bash
#!/bin/bash

# Sauvegarde du .bashrc actuel
cp ~/.bashrc ~/.bashrc.bak_$(date +%Y%m%d_%H%M%S)
echo "📦 Sauvegarde de ~/.bashrc créée : ~/.bashrc.bak_$(date +%Y%m%d_%H%M%S)"

# Suppression des personnalisations
sed -i '/figlet "Bienvenue !"/d' ~/.bashrc
sed -i '/toilet -f mono12 -F metal "Hello Hacker!"/d' ~/.bashrc
sed -i '/^export PS1=/d' ~/.bashrc

# Ajout d'un prompt sobre et permanent
echo 'export PS1="\u@\h:\w\$ "' >> ~/.bashrc
echo "✅ Prompt simple ajouté."

# Recharge immédiat du fichier bashrc
echo "🔁 Rechargement de ~/.bashrc..."
source ~/.bashrc

echo "✅ Réinitialisation terminée. Le prompt est maintenant sobre et les messages d'accueil ont été retirés."
```



### 2.3. **Instructions pour l’utiliser :**

1. Crée le script :

```bash
nano reset_bashrc.sh
```

2. Colle le contenu ci-dessus.
3. Enregistre (`Ctrl + O`, puis `Entrée`), quitte (`Ctrl + X`).
4. Rends-le exécutable :

```bash
chmod +x reset_bashrc.sh
```

5. Exécute-le :

```bash
./reset_bashrc.sh
```


---
# Annexe 3 - Sauvegarde automatique complète du `.bashrc` avant modification  (`~/.bashrc.bak`)
----


## 3.1. Fichier : `reset_bashrc.sh`

### Objectifs du script :
- Sauvegarder automatiquement le fichier `.bashrc` avec horodatage
- Supprimer toutes les lignes liées à :
  - `figlet "Bienvenue !"`
  - `toilet -f mono12 -F metal "Hello Hacker!"`
  - Toute ligne personnalisant le `PS1`
- Ajouter un prompt standard permanent (`\u@\h:\w\$`)
- Recharger le fichier `.bashrc` immédiatement
- Guider l’utilisateur pas à pas dans un style professionnel et pédagogique



### 3.2. Script à coller :

```bash
#!/bin/bash

# ========================================
# Script de réinitialisation du fichier .bashrc
# Auteur : Professeur Linux
# Objectif : Nettoyer les personnalisations et restaurer un prompt sobre
# ========================================

# Définir l’horodatage pour la sauvegarde
TIMESTAMP=$(date +%Y%m%d_%H%M%S)
BACKUP_FILE="$HOME/.bashrc.bak_$TIMESTAMP"

# Sauvegarde du fichier .bashrc
cp ~/.bashrc "$BACKUP_FILE"
echo "Sauvegarde du fichier .bashrc effectuée : $BACKUP_FILE"

# Suppression des personnalisations connues
sed -i '/figlet "Bienvenue !"/d' ~/.bashrc
sed -i '/toilet -f mono12 -F metal "Hello Hacker!"/d' ~/.bashrc
sed -i '/^export PS1=/d' ~/.bashrc

# Ajout d’un prompt standard et propre
echo 'export PS1="\u@\h:\w\$ "' >> ~/.bashrc
echo "Prompt sobre ajouté à la fin du .bashrc."

# Rechargement du fichier pour appliquer les changements immédiatement
echo "Rechargement du fichier ~/.bashrc..."
source ~/.bashrc

# Message final
echo "Réinitialisation terminée avec succès."
echo "Votre terminal est maintenant propre et fonctionnel avec un prompt standard."
```



### 3.3. Instructions d’utilisation :

1. Crée le fichier :
```bash
nano reset_bashrc.sh
```

2. Colle le contenu du script ci-dessus.
3. Enregistre avec `Ctrl + O`, puis quitte avec `Ctrl + X`.
4. Rends le fichier exécutable :
```bash
chmod +x reset_bashrc.sh
```

5. Exécute le script :
```bash
./reset_bashrc.sh
```

### 3.4. Version améliorée -  Nouveau script : `reset_bashrc.sh` (avec sélection interactive du prompt)

```bash
#!/bin/bash

# ========================================
# Script de réinitialisation du fichier .bashrc
# Auteur : Professeur Linux
# Objectif : Nettoyer les personnalisations et restaurer un prompt sobre ou personnalisé
# ========================================

# Couleurs pour l'affichage
VERT="\e[32m"
JAUNE="\e[33m"
BLEU="\e[34m"
ROUGE="\e[31m"
NORMAL="\e[0m"

# Définir l’horodatage pour la sauvegarde
TIMESTAMP=$(date +%Y%m%d_%H%M%S)
BACKUP_FILE="$HOME/.bashrc.bak_$TIMESTAMP"

# Sauvegarde du fichier .bashrc
cp ~/.bashrc "$BACKUP_FILE"
echo -e "${JAUNE}Sauvegarde du fichier .bashrc effectuée : $BACKUP_FILE${NORMAL}"

# Suppression des personnalisations connues
sed -i '/figlet "Bienvenue !"/d' ~/.bashrc
sed -i '/toilet -f mono12 -F metal "Hello Hacker!"/d' ~/.bashrc
sed -i '/^export PS1=/d' ~/.bashrc

# Choix du prompt
echo ""
echo -e "${BLEU}Choisissez un style de prompt à appliquer :${NORMAL}"
echo "1. Sobre        → \u@\h:\w\$"
echo "2. Coloré       → [user@host dir] (cyan)"
echo "3. Geek         → Heure + chemin (multiligne)"
echo "4. Minimaliste  → \$"
echo "5. Rouge vif    → \u@\h:\w (rouge)"
echo ""

read -p "Votre choix [1-5] : " choix_prompt

# Définition du prompt choisi
case $choix_prompt in
    1) PS1_FINAL='export PS1="\u@\h:\w\$ "' ;;
    2) PS1_FINAL='export PS1="\[\e[1;36m\][\u@\h \W]\$\[\e[0m\] "' ;;
    3) PS1_FINAL='export PS1="\[\e[0;32m\]\A \[\e[1;34m\]\w\n\u@\h \$ \[\e[0m\]"' ;;
    4) PS1_FINAL='export PS1="\$ "' ;;
    5) PS1_FINAL='export PS1="\[\e[1;31m\]\u@\h:\w \$\[\e[0m\]"' ;;
    *) echo -e "${ROUGE}Choix invalide. Le prompt sobre par défaut sera appliqué.${NORMAL}"
       PS1_FINAL='export PS1="\u@\h:\w\$ "' ;;
esac

# Ajout du nouveau prompt dans .bashrc
echo "$PS1_FINAL" >> ~/.bashrc
echo -e "${VERT}Prompt personnalisé appliqué.${NORMAL}"

# Rechargement du .bashrc
echo -e "${JAUNE}Rechargement du fichier ~/.bashrc...${NORMAL}"
source ~/.bashrc

# Message final
echo -e "${VERT}Réinitialisation complète. Votre nouveau prompt est en place.${NORMAL}"
```



#### 3.4.1. Résumé des fonctionnalités ajoutées :
- Sauvegarde automatique horodatée
- Nettoyage ciblé des personnalisations (`figlet`, `toilet`, `PS1`)
- Menu interactif avec **5 styles de prompt**
- Application immédiate (`source ~/.bashrc`)
- Messages pédagogiques et clairs



#### 3.4.2. Utilisation :

Même étapes que précédemment :
1. Crée le fichier :
```bash
nano reset_bashrc.sh
```
2. Colle le script ci-dessus.
3. Enregistre (Ctrl + O) puis quitte (Ctrl + X).
4. Rends le script exécutable :
```bash
chmod +x reset_bashrc.sh
```
5. Lance-le :
```bash
./reset_bashrc.sh
```



<br/>

---
# Annexe 4 - Script Linux ultra complet et professionnel (optionnel, je ne vous encourage pas de le faire pour le moment)
----

*Je vous propose un **script Linux ultra complet et professionnel mais très avancé**, intégrant plus de **25 options**. L'objectif est de vous donner une idée sur ce que nous pouvons faire avec les scripts shell*

- Gestion complète du `.bashrc`
- Personnalisation du terminal
- Gestion d'utilisateurs
- Messages ASCII, sons, alias, prompt, protection, etc.
- Réinitialisation et sauvegardes
- Ton propre de professeur expérimenté, sans emojis



## Fichier : `linux_hacker_terminal_master.sh`

```bash
#!/bin/bash

# ===============================================
# TP Linux - Personnalise ton terminal comme un Hacker Pro
# Auteur : Professeur Linux (50 ans d'expérience)
# Objectif : Gestion utilisateur, personnalisation .bashrc, sécurité et fun
# ===============================================

# COULEURS POUR L'AFFICHAGE
VERT="\e[32m"
ROUGE="\e[31m"
JAUNE="\e[33m"
BLEU="\e[34m"
NORMAL="\e[0m"

# STYLES DE PROMPT
PROMPT1='export PS1="\u@\h:\w\$ "'
PROMPT2='export PS1="\[\e[1;36m\][\u@\h \W]\$\[\e[0m\] "'
PROMPT3='export PS1="\[\e[0;32m\]\A \[\e[1;34m\]\w\n\u@\h \$ \[\e[0m\]"'
PROMPT4='export PS1="\$ "'
PROMPT5='export PS1="\[\e[35m\](\d \t) \u@\h:\w \$\[\e[0m\]"'
PROMPT6='export PS1="\[\e[1;33m\][\u@\h: \W]>>\[\e[0m\] "'
PROMPT7='export PS1="\[\e[1;31m\]\u@\h:\w \$\[\e[0m\]"'

# INTRODUCTION
clear
echo -e "${BLEU}Bienvenue dans le TP de personnalisation du terminal.${NORMAL}"
echo -e "Ce programme vous accompagne pas à pas pour transformer votre terminal en environnement puissant, esthétique et sécurisé."
echo ""

# VÉRIFICATION DE L'UTILISATEUR COURANT
if [ "$(id -u)" -eq 0 ]; then
    echo -e "Vous êtes connecté en tant que ${VERT}root${NORMAL}."
else
    echo -e "Vous êtes connecté en tant que ${JAUNE}utilisateur standard${NORMAL} : $(whoami)"
fi

# CRÉATION D’UN UTILISATEUR
read -p "Souhaitez-vous créer un nouvel utilisateur ? (o/n) : " rep
if [[ "$rep" =~ ^[Oo]$ ]]; then
    read -p "Nom du nouvel utilisateur : " new_user
    if id "$new_user" &>/dev/null; then
        echo "L'utilisateur existe déjà."
    else
        useradd -m "$new_user"
        passwd "$new_user"
        echo "Utilisateur créé avec succès."
    fi

    read -p "Souhaitez-vous l'ajouter au groupe sudo ? (o/n) : " rep_sudo
    if [[ "$rep_sudo" =~ ^[Oo]$ ]]; then
        usermod -aG sudo "$new_user"
        echo "Utilisateur ajouté au groupe sudo."
    fi
fi

# INSTALLATION DES OUTILS SI NÉCESSAIRE
apt update
apt install -y figlet toilet cmatrix fortune cowsay mpg123

# MENU PRINCIPAL
while true; do
    echo -e "\n${BLEU}===== MENU PRINCIPAL =====${NORMAL}"
    echo "1. Ajouter message d’accueil avec figlet"
    echo "2. Ajouter message avec toilet (style metal)"
    echo "3. Modifier le prompt (7 styles disponibles)"
    echo "4. Ajouter alias utiles et fun (matrix, ll, gs...)"
    echo "5. Ajouter écran de démarrage personnalisé"
    echo "6. Ajouter un son au démarrage"
    echo "7. Protéger le .bashrc (lecture seule)"
    echo "8. Supprimer tous les éléments personnalisés"
    echo "9. Réinitialiser le .bashrc proprement"
    echo "10. Afficher l’utilisateur courant"
    echo "11. Sauvegarder le .bashrc actuel"
    echo "12. Restaurer prompt simple temporairement"
    echo "13. Afficher chemin exact du .bashrc"
    echo "14. Créer un alias temporaire (session uniquement)"
    echo "15. Ajouter date/heure dynamique dans prompt"
    echo "16. Ajouter affichage automatique de fortune"
    echo "17. Lister tous les alias actuels"
    echo "18. Supprimer tous les alias personnalisés"
    echo "19. Tester cmatrix directement"
    echo "20. Supprimer la protection de .bashrc"
    echo "21. Quitter"
    echo -n "Choisissez une option [1-21] : "
    read choix

    case $choix in
        1) echo 'figlet "Bienvenue !"' >> ~/.bashrc && echo "Message figlet ajouté." ;;
        2) echo 'toilet -f mono12 -F metal "Hello Hacker!"' >> ~/.bashrc && echo "Message toilet ajouté." ;;
        3)
            echo "Choisissez un style de prompt :"
            echo "1. Sobre"
            echo "2. Cyan [user@host dir]"
            echo "3. Geek (heure + chemin)"
            echo "4. Minimaliste"
            echo "5. Avec date/heure complète"
            echo "6. Jaune encadré"
            echo "7. Rouge standard"
            read -p "Votre choix [1-7] : " pstyle
            sed -i '/^export PS1=/d' ~/.bashrc
            case $pstyle in
                1) echo "$PROMPT1" >> ~/.bashrc ;;
                2) echo "$PROMPT2" >> ~/.bashrc ;;
                3) echo "$PROMPT3" >> ~/.bashrc ;;
                4) echo "$PROMPT4" >> ~/.bashrc ;;
                5) echo "$PROMPT5" >> ~/.bashrc ;;
                6) echo "$PROMPT6" >> ~/.bashrc ;;
                7) echo "$PROMPT7" >> ~/.bashrc ;;
                *) echo "Choix invalide." ;;
            esac
            echo "Prompt modifié."
            ;;
        4)
            echo "alias matrix='cmatrix'" >> ~/.bashrc
            echo "alias ll='ls -lah'" >> ~/.bashrc
            echo "alias gs='git status'" >> ~/.bashrc
            echo "Alias ajoutés."
            ;;
        5)
            cat <<EOT >> ~/.bashrc

echo "Bienvenue \$USER - Terminal lancé le \$(date)"
fortune | cowsay
EOT
            echo "Écran personnalisé ajouté."
            ;;
        6)
            read -p "Chemin du fichier mp3 : " mp3
            echo "mpg123 \"$mp3\" &" >> ~/.bashrc
            echo "Son ajouté au démarrage."
            ;;
        7)
            chmod 400 ~/.bashrc
            echo ".bashrc protégé en lecture seule."
            ;;
        8)
            sed -i '/figlet/d;/toilet/d;/fortune/d;/cowsay/d;/^export PS1=/d;/matrix/d;/mpg123/d;/Bienvenue \$USER/d;/alias ll/d;/alias gs/d' ~/.bashrc
            echo "Éléments personnalisés supprimés."
            ;;
        9)
            cp ~/.bashrc ~/.bashrc.bak_$(date +%Y%m%d_%H%M%S)
            sed -i '/figlet/d;/toilet/d;/fortune/d;/cowsay/d;/^export PS1=/d;/matrix/d;/mpg123/d;/alias/d' ~/.bashrc
            echo "$PROMPT1" >> ~/.bashrc
            echo ".bashrc réinitialisé."
            ;;
        10) whoami ;;
        11)
            cp ~/.bashrc ~/.bashrc.bak_$(date +%Y%m%d_%H%M%S)
            echo "Sauvegarde créée."
            ;;
        12) eval "$PROMPT1" && echo "Prompt temporairement défini." ;;
        13) echo "Chemin du .bashrc : $HOME/.bashrc" ;;
        14)
            read -p "Nom de l'alias : " aname
            read -p "Commande de l'alias : " acmd
            alias $aname="$acmd"
            echo "Alias temporaire '$aname' ajouté."
            ;;
        15)
            echo 'export PS1="\[\e[0;32m\]\d \t \[\e[0m\]\u@\h:\w\$ "' >> ~/.bashrc
            echo "Prompt avec date/heure ajouté."
            ;;
        16)
            echo 'fortune | cowsay' >> ~/.bashrc
            echo "Fortune ajouté au démarrage."
            ;;
        17) alias ;;
        18) sed -i '/^alias/d' ~/.bashrc && echo "Alias supprimés." ;;
        19) cmatrix ;;
        20) chmod 644 ~/.bashrc && echo "Protection supprimée." ;;
        21) echo "Fin du script. À bientôt." ; exit 0 ;;
        *) echo "Choix invalide." ;;
    esac

    read -p "Appuyez sur Entrée pour continuer..."
done
```



## Instructions

1. Enregistre ce script dans un fichier :
```bash
nano linux_hacker_terminal_master.sh
```

2. Colle tout le contenu ci-dessus.
3. Rends-le exécutable :
```bash
chmod +x linux_hacker_terminal_master.sh
```

4. Lance-le :
```bash
./linux_hacker_terminal_master.sh
```







<br/>
<br/>




---
# Annexe 5 - Message d'accueil simple en arabe
----

### 5.1. Message simple en arabe

Ajoute cette ligne à la fin de ton fichier `.bashrc` :

```bash
echo "مرحباً بك في جهازك، يا بطل لينكس!"
```



### 5.2. Avec `figlet` (ASCII Art)

Malheureusement, `figlet` ne supporte pas bien l'arabe (car c’est du texte bidirectionnel et avec ligatures).  
Mais tu peux créer un style hybride en ASCII en arabe/latin :

```bash
echo 'figlet "مرحباً بك"' >> ~/.bashrc
```

> Cela affichera le texte en ASCII, mais mal formé pour l’arabe. Ce n’est donc **pas recommandé**.



### 5.3. Meilleure alternative : utiliser `echo -e` avec couleurs

```bash
echo -e "\e[1;33mمرحباً بك في عالم لينكس!\e[0m" >> ~/.bashrc
```

> Ce message s’affichera en jaune (code `\e[1;33m`) à chaque ouverture de terminal.



### 5.4. Message complet en arabe et français

Tu peux aussi combiner deux messages :

```bash
echo -e "\e[1;32mBienvenue - مرحباً بك، $(whoami) !\e[0m" >> ~/.bashrc
```



### 5.5. Résultat attendu

Lorsque tu ouvriras un nouveau terminal ou que tu taperas :

```bash
source ~/.bashrc
```

Tu verras :

```text
Bienvenue - مرحباً بك، superfun !
```

en vert.














# Annexe 6 - Message d'accueil simple en arabe avec l'heure
----

### 6.1. Message simple en arabe

Ajoute cette ligne à la fin de ton fichier `.bashrc` :

```bash
echo "مرحباً بك في جهازك، يا بطل لينكس!"
```



### 6.2. Avec `figlet` (ASCII Art)

Malheureusement, `figlet` ne supporte pas bien l'arabe (car c’est du texte bidirectionnel et avec ligatures).  
Mais tu peux créer un style hybride en ASCII en arabe/latin :

```bash
echo 'figlet "مرحباً بك"' >> ~/.bashrc
```

> Cela affichera le texte en ASCII, mais mal formé pour l’arabe. Ce n’est donc **pas recommandé**.



### 6.3. Meilleure alternative : utiliser `echo -e` avec couleurs

```bash
echo -e "\e[1;33mمرحباً بك في عالم لينكس!\e[0m" >> ~/.bashrc
```

> Ce message s’affichera en jaune à chaque ouverture de terminal.


### 6.4. Message complet en arabe avec l'utilisateur et l'heure

Tu peux ajouter un message plus personnalisé, incluant le nom de l’utilisateur et l’heure actuelle en arabe :

```bash
echo -e "\e[1;32mمرحباً بك يا $(whoami)!\e[0m" >> ~/.bashrc
echo -e "\e[1;34mالوقت الحالي هو: $(LC_TIME=ar_AE.UTF-8 date +\"%A %d %B %Y - %H:%M:%S\")\e[0m" >> ~/.bashrc
```



### 6.5. Explication

- `$(whoami)` affiche le nom de l’utilisateur actuel.
- `date +...` affiche la date et l’heure.
- `LC_TIME=ar_AE.UTF-8` force l’affichage en arabe (ex : الاثنين 08 أبريل 2025 - 14:33:10).
- Les couleurs sont :
  - Vert pour le message principal (`\e[1;32m`)
  - Bleu pour la date (`\e[1;34m`)
- `\e[0m` réinitialise la couleur.



### 6.6. Résultat attendu

Quand tu taperas :

```bash
source ~/.bashrc
```

Ou que tu ouvriras un nouveau terminal, tu verras :

```text

مرحباً بك يا superfun!
الوقت الحالي هو: الثلاثاء 09 أبريل 2025 - 14:35:42
```





<br/>
<br/>

----
# Annexe 7 – Message d’accueil artistique avec `fortune`, `cowsay` et affichage en arabe
----


### 7.1. Objectif

Créer un message d’accueil original et artistique en arabe, incluant :

- Un message de bienvenue en couleur
- L’heure et la date actuelles en arabe
- Un encadré `cowsay` avec un proverbe ou une phrase amusante
- Optionnel : utilisation de `fortune` avec phrases traduites en arabe



### 7.2. Installer les programmes nécessaires

```bash
sudo apt update
sudo apt install fortune cowsay -y
```



### 7.3. Ajouter ce bloc dans le fichier `.bashrc`

Ajoute les lignes suivantes à la **fin de ton fichier `~/.bashrc`** :

```bash
echo -e "\e[1;33m============================\e[0m"
echo -e "\e[1;32mمرحباً بك يا $(whoami)!\e[0m"
echo -e "\e[1;34mالوقت الحالي هو: $(LC_TIME=ar_AE.UTF-8 date +\"%A %d %B %Y - %H:%M:%S\")\e[0m"
echo -e "\e[1;33m============================\e[0m"
echo ""
echo " $(cowsay -f tux 'أهلاً بك في محطة لينكس الخاصة بك. كن مبدعاً واستمتع!')"
```



### 7.4. Explication

- Encadrement stylisé en jaune (`===`)
- Message en vert : nom de l'utilisateur
- Heure et date en arabe : locale `ar_AE.UTF-8`
- `cowsay` affiche un message fun en arabe, dans une bulle texte avec un pingouin (`-f tux`)
- Pas besoin de `fortune` si tu ajoutes tes propres phrases



### 7.5. Exemple de sortie dans le terminal

```text
============================
مرحباً بك يا superfun!
الوقت الحالي هو: الثلاثاء 09 أبريل 2025 - 14:35:42
============================

 -------------------------------
أهلاً بك في محطة لينكس الخاصة بك. كن مبدعاً واستمتع!
           \   
            \  
             (o< 
             //\
             V_/_  
```



### 7.6. Option : Créer tes propres fortunes en arabe

1. Crée un fichier texte avec des proverbes arabes :

```bash
nano ~/.fortunes_ar.txt
```

Exemple de contenu :

```
من جد وجد، ومن زرع حصد.
الوقت كالسيف إن لم تقطعه قطعك.
التكرار يعلم الشطار.
%
```

> Utilise `%` pour séparer chaque phrase.

2. Compile-le avec `strfile` :

```bash
sudo apt install fortune-mod -y
strfile ~/.fortunes_ar.txt
```

3. Utilise-le dans `.bashrc` :

```bash
fortune ~/.fortunes_ar.txt | cowsay -f tux
```



### 7.7. Résultat personnalisé

Tu peux remplacer cette ligne dans `.bashrc` :

```bash
echo " $(cowsay -f tux 'أهلاً بك في محطة لينكس الخاصة بك. كن مبدعاً واستمتع!')"
```

par :

```bash
fortune ~/.fortunes_ar.txt | cowsay -f tux
```

Ainsi, à chaque nouvelle session, un **proverbe arabe différent** s’affichera dans ton terminal !

