# Cours : Comprendre `.bashrc`, `.profile`, `.bash_profile` et leur comportement avec sudo

# 1. Pourquoi ces fichiers existent-ils ?

Quand un utilisateur ouvre un terminal ou se connecte à une session, Linux charge des **fichiers de configuration shell** pour définir :

- Les **variables d'environnement** (ex: `$PATH`, `$HOME`)
- Les **alias**
- Les **fonctions shell**
- L'apparence du **prompt**
- Des **scripts à exécuter automatiquement**



# 2. Les fichiers de configuration possibles

| Fichier             | Chargé par                  | Type de shell         | Emplacement      |
|---------------------|-----------------------------|------------------------|------------------|
| `.bashrc`           | Bash interactif non-login   | Terminal courant       | Dans `~/` (home utilisateur) |
| `.profile`          | Shell de login              | Bash **ou** autre shell | Dans `~/` |
| `.bash_profile`     | Shell de login (si présent) | Bash uniquement        | Dans `~/` |
| `/etc/bash.bashrc`  | Pour tous les utilisateurs  | Bash interactif        | Global (système) |
| `/etc/profile`      | Pour tous les utilisateurs  | Shell de login         | Global (système) |

> ✅ Si `.bash_profile` existe, `.profile` n'est **pas** lu.



# 3. Comportement selon les types de session

| Méthode d'accès                  | Fichiers chargés                                |
|----------------------------------|--------------------------------------------------|
| Terminal graphique (GNOME, etc.)| `.profile` ou `.bash_profile`                   |
| Terminal simple (`bash`)        | `.bashrc`                                       |
| Connexion SSH                   | `.bash_profile` puis `.bashrc` (si bash)        |
| `sudo -s`                       | Charge `.bashrc` **de l'utilisateur courant**   |
| `sudo -i`                       | Charge `.profile` ou `.bash_profile` **du root**|



# 4. Illustration : Utilisateur normal vs root

### Exemple utilisateur normal : `/home/etudiant`

```bash
/home/etudiant/.bashrc
/home/etudiant/.profile
```

### Exemple root : `/root`

```bash
/root/.bashrc
/root/.profile
/root/.bash_profile
```



# 5. Que fait `sudo -s` exactement ?

```bash
sudo -s
```

- Devient root **avec l’environnement** de l’utilisateur actuel.
- Ne change pas `$HOME`, ni `$PATH`, ni le prompt.
- **Charge `.bashrc` de l’utilisateur courant**, PAS celui de root.



# 6. Que fait `sudo -i` exactement ?

```bash
sudo -i
```

- Devient root **en login complet**.
- Change `$HOME` vers `/root`
- Charge `.bash_profile`, `.profile` et `.bashrc` **du root**.
- C’est comme si vous aviez tapé `su -`.



# 7. Exemples concrets

### Créez un alias personnalisé :

Dans `/home/etudiant/.bashrc` :

```bash
alias bonjour='echo Bonjour $USER'
```

Dans `/root/.bashrc` :

```bash
alias bonjour='echo Bonjour, je suis le ROOT !'
```

### Test :

```bash
# En tant qu'utilisateur
bonjour
# => Bonjour etudiant

# Avec sudo -s (en root mais environnement utilisateur)
sudo -s
bonjour
# => Bonjour etudiant

# Avec sudo -i (en root complet)
sudo -i
bonjour
# => Bonjour, je suis le ROOT !
```



# 8. Résumé visuel

| Commande      | UID actif | HOME utilisé  | Fichier lu               | Environnement chargé        |
|---------------|-----------|----------------|---------------------------|-----------------------------|
| `bash`        | etudiant  | /home/etudiant | `.bashrc`                 | utilisateur                 |
| `sudo -s`     | root      | /home/etudiant | `.bashrc` (de etudiant)   | environnement utilisateur   |
| `sudo -i`     | root      | /root          | `.bash_profile`, `.bashrc`| environnement root          |
| `su`          | root      | /root          | Aucune config chargée     | très basique                |
| `su -`        | root      | /root          | `.bash_profile`, etc.     | environnement root complet  |



# 9. Recommandations 

- Utilisez `.bashrc` pour des **alias**, **couleurs**, **PS1**, etc.
- Utilisez `.profile` ou `.bash_profile` pour des **export de variables**.
- Ne mélangez pas les deux : `.bashrc` = interactif / `.profile` = login
- Testez avec `echo $HOME` ou `echo $USER` pour voir le contexte actuel.



# 10. Pour aller plus loin

```bash
# Savoir quel fichier est lu :
echo $0            # Donne le type de shell
echo $HOME         # Affiche le home utilisé
whoami             # Affiche l’utilisateur effectif
```

