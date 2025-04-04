
<br/>
<br/>

# 1  - Ajout du proxy

```bash
export http_proxy="http://10.1.0.5:8080"
export https_proxy="http://10.1.0.5:8080"
export ftp_proxy="http://10.1.0.5:8080"
```

<br/>
<br/>


# 2 - ❌ Erreurs à corriger :
- Il y a des espaces après `export` et avant les noms des variables : **à éviter**.
- Les adresses proxy ne sont **pas entre guillemets** : c’est recommandé, surtout si tu veux éviter des erreurs d’interprétation par le shell.
- Les lignes commencent par `===>` : ce n’est **pas valide dans un shell Linux**.

<br/>
<br/>


# 3 - Pour appliquer ça de manière permanente :
Ajoute-les à la fin de ton fichier `~/.bashrc` (ou `~/.bash_profile` selon ta distro) :

```bash
echo 'export http_proxy="http://10.1.0.5:8080"' >> ~/.bashrc
echo 'export https_proxy="http://10.1.0.5:8080"' >> ~/.bashrc
echo 'export ftp_proxy="http://10.1.0.5:8080"' >> ~/.bashrc
source ~/.bashrc
```






<br/>
<br/>


# 4 - L’INVERSE  



##  D’abord je teste :

```bash
echo $http_proxy
echo $https_proxy
echo $ftp_proxy
# ou bien
env | grep proxy
```


##  L’inverse de ceci :

```bash
export http_proxy=10.1.0.5:8080
export https_proxy=10.1.0.5:8080
export ftp_proxy=10.1.0.5:8080
env | grep proxy
```



## 🚫 Est ceci !!!!!!

```bash
unset http_proxy
unset https_proxy
unset ftp_proxy
env | grep proxy
```

