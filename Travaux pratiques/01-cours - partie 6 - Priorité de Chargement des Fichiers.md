# Document de Priorité de Chargement des Fichiers : `.bash_profile`, `.profile`, `.bashrc`


# 1 - Objectif

Comprendre **l'ordre dans lequel Linux charge les fichiers de configuration shell** (`.bash_profile`, `.profile`, `.bashrc`) pour un utilisateur donné, et savoir **qui est prioritaire sur qui**.



## 1.1. Règle d'or : **.bash_profile a priorité sur .profile**

> 🔁 Autrement dit :  
> **Si `.bash_profile` existe, `.profile` est ignoré**.



## 1.2. Priorité de chargement selon le type de shell

### 🟡 1.2.1. Shell de **login** (connexion, `ssh`, `su -`, `sudo -i`) :

Linux cherche dans cet ordre :

1. `.bash_profile` ✅
2. `.bash_login` ❌ (rarement utilisé)
3. `.profile` ✅ **(uniquement si les deux premiers n’existent pas)**

> 📌 Donc : **Si `.bash_profile` est présent, `.profile` ne sera jamais lu.**



### 🟢 1.2.2. Shell **interactif non-login** (ex: terminal GNOME, `bash`) :

Linux exécute seulement :

- `.bashrc`

> 🔎 C’est pour ça qu’on ne voit pas les exports définis dans `.bash_profile` quand on ouvre juste un terminal.



# 2. Exemples de vraie vie

### 2.1. Exemple 1 : `.bash_profile` existe

- Tu ouvres une session SSH → `.bash_profile` est lu
- `.profile` est **ignoré**
- Tu ouvres un terminal → `.bashrc` est lu

### 2.2. Exemple 2 : `.bash_profile` n’existe PAS

- Tu ouvres une session SSH → `.profile` est lu
- Tu ouvres un terminal → `.bashrc` est lu


# 3. Résumé des priorités

| Type de shell               | Fichier chargé en priorité                | Fichier ignoré                   |
|-----------------------------|-------------------------------------------|----------------------------------|
| Login shell (`ssh`, `su -`) | `.bash_profile` → `.bash_login` → `.profile` | Les suivants sont ignorés si un est trouvé |
| Terminal (`bash`)           | `.bashrc`                                 | `.bash_profile` et `.profile` ne sont pas lus |



# 4. Astuce recommandée pour tous

Dans `.bash_profile`, ajoute toujours :

```bash
# Pour que bashrc soit exécuté même lors d'un login shell
if [ -f ~/.bashrc ]; then
  . ~/.bashrc
fi
```

 Cela permet d'avoir **un comportement cohérent** : `.bashrc` sera toujours exécuté, même après une connexion SSH ou un `sudo -i`.



# 5. Expérience rapide à faire

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



# 6. Conclusion 

- `.bash_profile` bloque `.profile`
- `.bashrc` est toujours indépendant, il ne se déclenche que dans un terminal
- Pour les shells de login, `.bash_profile` est prioritaire

