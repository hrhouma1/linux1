# EXAMEN – SERVICES `systemd` ET GESTION DES PROCESSUS SOUS UBUNTU 22.04

- *Nom de l’étudiant* : \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_ 
- *Groupe :* \_\_\_\_\_\_\_\_\_\_\_\_

<br/>

### PARTIE 1 – INVESTIGATION SUR UN SERVICE `systemd` DÉJÀ ACTIF (PORT 3000)


#### 1. PRÉREQUIS

Avant de commencer cette section, assurez-vous que les conditions suivantes sont remplies :

* Un serveur **Wiki.js** configuré pour s’exécuter sur le port 3000.
* Un serveur **NGINX** actif.

**Commandes de préparation :**

```bash
sudo apt update
sudo apt install nginx
sudo systemctl start nginx
sudo systemctl enable nginx
sudo systemctl status nginx
```

<br/>

## 2. CONTEXTE DU SCÉNARIO

Monsieur Éric, administrateur système, tente de déployer l’application **Wiki.js**. L’installation semble correcte, mais l’application ne répond pas comme prévu sur le port 3000. Vous devez mener une enquête détaillée pour identifier et résoudre définitivement ce problème.

<br/>

## 3. ÉTAPES DE L’INVESTIGATION

### Étape 1 – Vérification du port utilisé

Vérification initiale du port 3000 :

```bash
lsof -i :3000
```


Résultat :

```
COMMAND   PID   USER     FD   TYPE DEVICE SIZE/OFF NODE NAME
node     9744   wikijs   22u  IPv4  59242      0t0  TCP *:3000 (LISTEN)
```


*Question 1* :

> À quoi sert la commande `lsof` ? Comparez-la à la commande `fuser` : ont-elles les mêmes usages ? En quoi diffèrent-elles dans la gestion des fichiers ou des ports ouverts sous Linux ?



*Réponse 1 :*

....................................................................................


### Étape 2 – Tentative d’arrêt manuel

Tentative d'arrêt du processus :

```bash
kill -15 9744
kill -9 9744
```




<br/>

*Question 2* :

> Le processus se relance-t-il automatiquement après avoir été arrêté manuellement ?
> **Expliquez pourquoi, en vous appuyant sur les mécanismes de `systemd` :**


*Réponse 2 :*

```
....................................................................................
....................................................................................
....................................................................................
```

<br/>

*Question 3* :

> Quelle est la différence entre un **daemon** (ou processus en arrière-plan) classique et un **service `systemd`** ?
> Donnez un exemple concret pour illustrer chaque cas.

*Réponse 3* :

```
....................................................................................
....................................................................................
....................................................................................
```

<br/>

### Étape 3 – Identification du service responsable

Identification du service `systemd` :

```bash
systemctl list-units --type=service --state=running | grep -i wiki
ps aux | grep node
ls -l /etc/systemd/system/ | grep wiki
```

Service identifié :

```
wikijs.service
```

<br/>



*Question 4* :

> Vous avez identifié le service `wikijs.service` à l’aide des commandes suivantes :
>
> ```bash
> systemctl list-units --type=service --state=running | grep -i wiki
> ps aux | grep node
> ls -l /etc/systemd/system/ | grep wiki
> ```
>
> **Expliquez en quoi ces commandes permettent de confirmer l’existence d’un service `systemd` actif.**
> Justifiez pourquoi ces trois approches sont complémentaires.


*Réponse 4* :


```
....................................................................................
....................................................................................
....................................................................................
....................................................................................
```




<br/>

### Étape 4 – Vérification du lien entre service et port

Confirmation finale du lien :

```bash
sudo lsof -i -P -n | grep 3000
ps -p <PID> -o pid,cmd
```

<br/>



*Question 5* :

> Expliquez comment les commandes ci-dessus permettent d'établir un lien direct entre un **PID donné**, le **port 3000** et le **service `systemd`** identifié précédemment.
> Pourquoi est-ce une étape indispensable avant d'arrêter ou désactiver un service ?

*Réponse 5* :

```
....................................................................................
....................................................................................
....................................................................................
....................................................................................
```




<br/>

### Étape 5 – Désactivation définitive du service

Commandes finales :

```bash
sudo systemctl stop wikijs.service
sudo systemctl disable wikijs.service
```

Vérification de la libération du port :

```bash
lsof -i :3000
```



**Question 6** :

> Expliquez la différence entre les commandes `systemctl stop` et `systemctl disable`.
> Pourquoi est-il nécessaire d’exécuter **les deux** pour garantir que le service ne redémarrera plus, même après un redémarrage de la machine ?
> Pourquoi la commande kill -9 est-elle insuffisante dans ce cas ?

**Réponse 6** :

```
....................................................................................
....................................................................................
....................................................................................
....................................................................................
```



<br/>

**Question 7** :

**7.1. Pourquoi le processus lié au port 3000 redémarrait-il automatiquement après un `kill -9` ?**

Réponse :
....................................................................................
....................................................................................

**7.2. Quelle commande permet d’identifier rapidement les services `systemd` actifs contenant un mot-clé ?**

Réponse :
....................................................................................

**7.3. Quelle méthode vous a permis de confirmer le lien entre le port et le fichier `wikijs.service` ?**

Réponse :
....................................................................................

**7.4. Expliquez pourquoi il est recommandé d’utiliser `systemctl stop` et `disable` plutôt que `kill -9`.**

Réponse :
....................................................................................
....................................................................................

**7.5. Proposez un script simple pour automatiser cette recherche :**

```bash
# Script :
....................................................................................  
....................................................................................  
```

<br/>

**Question 8 (Bonus)** :

Pour approfondir, testez et expliquez ces outils :

* `systemd-cgls`
* `ss -lptn`
* `fuser -k 3000/tcp`
* `journalctl -u wikijs.service`



<br/>
<br/>

# PARTIE 2 – DÉTECTION ET NEUTRALISATION D’UN SERVICE MALVEILLANT (`systemd`)



## 1. CONSIGNES GÉNÉRALES IMPORTANTES

* Réalisation sous Ubuntu 22.04.
* Le service malveillant `logservice.service` est actif dès le démarrage.
* Remettre un rapport final précis.
* Aucun redémarrage ou arrêt complet du système autorisé.

<br/>

## 2. CONTEXTE TECHNIQUE

Un service malveillant génère en continu des fichiers logs dans `/var/log/malicious`, risquant de saturer le système. Ce service redémarre automatiquement en cas d’arrêt (`Restart=always`).

Fichier suspect :

```
/etc/systemd/system/logservice.service
```

<br/>

## 3. OBJECTIF 

* Identifier un service anormal.
* Arrêter et désactiver définitivement un service `systemd`.
* Nettoyer les fichiers générés.
* Fournir un rapport technique précis.

<br/>


### 3.1. Créer le script malveillant

```bash
sudo mkdir -p /opt/malicious
sudo nano /opt/malicious/logwriter.sh
```

**Contenu du script :**

```bash
#!/bin/bash

mkdir -p /var/log/malicious

while true; do
  echo "$(date) - LOG ENTRY" >> /var/log/malicious/attack.log
  sleep 0.1
done
```

### 3.2. Rendre le script exécutable

```bash
sudo chmod +x /opt/malicious/logwriter.sh
```



### 3.3. Créer le service `systemd` malveillant

```bash
sudo nano /etc/systemd/system/logservice.service
```

**Contenu du fichier `logservice.service` :**

```ini
[Unit]
Description=Service Malveillant – Générateur de Logs
After=network.target

[Service]
ExecStart=/opt/malicious/logwriter.sh
Restart=always
RestartSec=1

[Install]
WantedBy=multi-user.target
```



### 3.4. Activer et démarrer le service

```bash
sudo systemctl daemon-reload
sudo systemctl enable logservice
sudo systemctl start logservice
```



### 3.5. Vérifier le comportement

```bash
sudo systemctl status logservice
ls -lh /var/log/malicious/
```

- Vous verrez le fichier grandir très rapidement.
- Ce service simule un **comportement malveillant de type "log flooding"**, typique d’un binaire installé discrètement et persistant au démarrage. Il vous pousse à : 

* Identifier un fichier qui enfle dangereusement (`du`, `lsof`, etc.)
* Corréler PID ↔ service `systemd`
* Essayer d’arrêter avec `kill -9` (échec)
* Comprendre `Restart=always`
* Appliquer la séquence correcte :

  * `systemctl stop`
  * `systemctl disable`
  * `rm`
  * `daemon-reload`
  * Nettoyage




## 4. ÉTAPES À RÉALISER

### Étape 1 – Identification du problème

Commandes à utiliser :

```bash
du -sh /var/log/*
ps aux | grep logservice
lsof | grep malicious
systemctl status logservice
```

**Question 9** :

Expliquez ce que fait chacune de ces commandes et en quoi elle permet d’identifier un comportement anormal lié au service logservice.
Votre réponse doit mentionner les éléments techniques observables (taille, processus, fichiers ouverts, statut, etc.).

Réponse :

```bash

....................................................................................  
....................................................................................  
```

<br/>






### Étape 2 – Tentatives d’arrêt

* **Commande 1** (arrêt brutal du processus) :

```bash
sudo kill -9 <PID>
```

* **Commande 2** (arrêt via `systemd`) :

```bash
sudo systemctl stop logservice
```

<br/>

**Question 10** :

> Les deux commandes ci-dessus visent à interrompre l’exécution du service malveillant.
>
> * Laquelle est réellement efficace pour stopper durablement le service ?
> * Pourquoi la commande `kill -9` est-elle souvent insuffisante dans le cas d’un service géré par `systemd` ?
> * Justifiez votre réponse en lien avec les paramètres du fichier `.service`.

**Réponse attendue :**

```bash
....................................................................................
....................................................................................
....................................................................................
....................................................................................
```




<br/>







### Étape 3 – Désactivation complète du service

* Désactivation du service au démarrage :

```bash
sudo systemctl disable logservice
```

* Suppression du fichier de définition :

```bash
sudo rm /etc/systemd/system/logservice.service
```

* Rechargement de la configuration `systemd` :

```bash
sudo systemctl daemon-reload
```

* Suppression des fichiers générés :

```bash
sudo rm -rf /var/log/malicious
```

<br/>

**Question 11** :

> Expliquez précisément le rôle de chacune des commandes ci-dessus.
>
> Pourquoi faut-il exécuter les **trois premières commandes dans cet ordre** pour désactiver totalement un service `systemd` ?
>
> Pourquoi la commande `daemon-reload` est-elle indispensable dans ce processus ?
>
> Et enfin, quel est le **risque si la suppression du dossier `/var/log/malicious` est oubliée** ?

**Réponse :**

```bash
....................................................................................
....................................................................................
....................................................................................
....................................................................................
```











<br/>





### Étape 4 – Validation finale

* Vérification de l’arrêt du service :

```bash
sudo systemctl status logservice
```

* Vérification de la suppression du répertoire :

```bash
ls -la /var/log/malicious
```

<br/>

**Question 12** :

> Expliquez ce que permettent de vérifier ces deux commandes.
>
> Que devriez-vous observer dans leur sortie respective si **le service a bien été neutralisé** et si **les fichiers malveillants ont bien été supprimés** ?


**Réponse attendue** :

```bash
....................................................................................
....................................................................................
....................................................................................
....................................................................................
```










<br/>

**Question 13** :

### 13.1. Identification

1. Quelle commande a détecté le répertoire problématique ?

Réponse :
....................................................................................

2. Quel était le nom exact du processus malveillant ?

Réponse :
....................................................................................

3. Quelle commande a permis d'obtenir son PID ?

Réponse :
....................................................................................

### 13.2. Arrêt du processus

1. Commande utilisée pour un arrêt brutal ?

Réponse :
....................................................................................

2. Le processus est-il revenu automatiquement ?

Réponse :
....................................................................................

3. Commande efficace pour arrêter définitivement le service ?

Réponse :
....................................................................................

### 13.3. Désactivation et nettoyage

1. Commande utilisée pour supprimer définitivement le service ?

Réponse :
....................................................................................

2. Commande utilisée pour supprimer les fichiers générés ?

Réponse :
....................................................................................

### 13.4. Questions de compréhension

1. Pourquoi `kill -9` est-il insuffisant contre certains services ?

Réponse :
....................................................................................

2. Que signifie `Restart=always` dans un fichier `.service` ?

Réponse :
....................................................................................

3. Commande pour recharger la configuration des services `systemd` ?

Réponse :
....................................................................................

<hr/>

## BONUS (FACULTATIF)

Créer un service `hello.service` écrivant la date actuelle toutes les 5 secondes dans `/tmp/bonjour.log` :

* Script : `/opt/bonjour.sh`
* Service : `/etc/systemd/system/hello.service`

Insérez votre solution dans le rapport final si terminé.

<br/>

# LIVRABLE FINAL ATTENDU :

Contenu requis :

* Toutes les commandes utilisées en annexe.
* Observations détaillées.
* Réponses complètes aux questions.
* Solution bonus si réalisée.



