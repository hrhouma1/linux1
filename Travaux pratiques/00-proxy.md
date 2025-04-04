
<br/>
<br/>

# 1  - Ajout du proxy

```bash
export http_proxy=http://10.1.0.5:8080
export https_proxy=http://10.1.0.5:8080
export ftp_proxy=http://10.1.0.5:8080
echo $http_proxy
echo $https_proxy
echo $ftp_proxy
env | grep proxy 
```

<br/>
<br/>




<br/>
<br/>


# 2 - Pour appliquer ça de manière permanente :
Ajoute-les à la fin de ton fichier `~/.bashrc` (ou `~/.bash_profile` selon ta distro) :

```bash
echo 'export http_proxy=http://10.1.0.5:8080' >> ~/.bashrc
echo 'export https_proxy=http://10.1.0.5:8080' >> ~/.bashrc
echo 'export ftp_proxy=http://10.1.0.5:8080' >> ~/.bashrc
source ~/.bashrc
```






<br/>
<br/>


# 3 - L’INVERSE  



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

