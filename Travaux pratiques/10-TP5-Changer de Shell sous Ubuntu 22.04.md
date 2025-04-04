# Changer de Shell sous Ubuntu 22.04 (Pratique)


# Étape 1 : Vérifier les shells disponibles

Avant de changer, voyons quels shells sont installés sur le système :

```bash
cat /etc/shells
```

Cela retourne généralement une liste comme :

```
/bin/sh
/bin/bash
/usr/bin/zsh
/bin/dash
```



# Étape 2 : Vérifier le shell actuel

```bash
echo $SHELL
```

> Cela affichera par exemple : `/bin/bash` ou `/usr/bin/zsh`


# Étape 3 : Changer de shell avec `chsh`

###  Syntaxe :

```bash
chsh -s <chemin_du_nouveau_shell>
```

###  Exemple : passer à `zsh`

```bash
chsh -s /usr/bin/zsh
```

> ⚠️ Il faut entrer le chemin complet (trouvé dans `/etc/shells`).


# Étape 4 : Se déconnecter / reconnecter

Le changement ne prend **effet qu’à la prochaine session**.  
Tu dois donc **te déconnecter** puis **te reconnecter** pour voir le nouveau shell.



## Astuce : Vérification finale

Après reconnexion, tape :

```bash
echo $SHELL
```

Cela doit afficher `/usr/bin/zsh` ou le shell que tu as choisi.



## ❓ Et si tu veux tester un shell sans le changer définitivement ?

Tu peux juste le lancer temporairement :

```bash
zsh
```

Ou revenir au shell précédent :

```bash
exit
```



## 🚫 Revenir à bash

Si tu veux revenir à Bash :

```bash
chsh -s /bin/bash
```


> 🧠 **Note pédagogique** : changer de shell peut être utile pour personnaliser son environnement (Zsh avec Oh My Zsh, Fish, etc.). Mais attention à bien connaître les commandes de base du nouveau shell avant de l'adopter.
