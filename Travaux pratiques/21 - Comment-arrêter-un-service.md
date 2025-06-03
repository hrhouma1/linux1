# Arréter un service


# Prérequis:

- Avoir un site wikijs en cours d'exécution (Regardez le document suivant : 18 - Mini projet-1 Installer Wikijs - version Postgres.md)
- Avoir nginx

```bash
apt update
apt install nginx
sytemctl start nginx
systemctl enable nginx
systemctl status nginx
```
  
## Contexte

Monsieur Éric, administrateur système, tente d’installer et de lancer une application nommée **Wiki.js** sur un serveur Ubuntu. L’application est censée écouter sur le port 3000, mais elle ne fonctionne pas. Après plusieurs essais infructueux, Éric entreprend une série d’investigations pour identifier et résoudre le problème.



## Étape 1 – Vérification du port utilisé

Éric commence par vérifier si le port 3000 est déjà utilisé par un autre service. Il utilise la commande suivante :

```bash
lsof -i :3000
```

Résultat obtenu :

```
COMMAND   PID   USER     FD   TYPE DEVICE SIZE/OFF NODE NAME
node     9744   wikijs   22u  IPv4  59242      0t0  TCP *:3000 (LISTEN)
```

Un processus `node` est en écoute sur le port 3000. Il relève le PID : 9744.



## Étape 2 – Tentative de libération du port avec `kill`

Éric tente d'arrêter le processus manuellement :

```bash
kill -15 9744
kill -9 9744
```

Mais après relance de la commande `lsof -i :3000`, un nouveau PID est affiché :

```
COMMAND   PID   USER     FD   TYPE DEVICE SIZE/OFF NODE NAME
node     9842   wikijs   22u  IPv4  59243      0t0  TCP *:3000 (LISTEN)
```

Conclusion : le processus est automatiquement redémarré. Une autre entité (probablement un service) le relance.



## Étape 3 – Recherche du service systemd

Éric suspecte un service systemd. Il cherche donc à identifier son nom exact.

Commandes utilisées :

```bash
systemctl list-units --type=service
systemctl list-units --type=service --state=exited
systemctl list-units --type=service --state==running
systemctl list-units --type=service --state=running | grep -i wiki
```

Ou encore

```bash
ls -l --sort=time /etc/systemd/system/
```

![image](https://github.com/user-attachments/assets/c93e94bf-226c-4291-b1a1-aa608c288cb7)

Mais aucun service ne montre directement une correspondance avec le port 3000.

Il poursuit avec des recherches croisées :

```bash
sudo ss | grep -i wiki
sudo ss | grep -i 3000
ps aux | grep node
systemctl | grep node
```

Une hypothèse se confirme : un service appelé `wikijs.service` semble être responsable du processus.



## Étape 4 – Vérification de la corrélation entre processus, port et service

Éric établit manuellement le lien entre le processus `node`, le port 3000 et le service en question :

```bash
lsof -i :3000
sudo lsof -i -P -n | grep node
sudo lsof -i -P -n | grep wiki
```

Il retrouve le fichier de service :

```bash
ls -l /etc/systemd/system/ | grep wiki
```

Fichier identifié : `wikijs.service`.



## Étape 5 – Arrêt définitif du service et libération du port

Éric stoppe proprement le service et désactive son redémarrage automatique :

```bash
sudo systemctl stop wikijs.service
sudo systemctl disable wikijs.service
```

Vérification finale :

```bash
lsof -i :3000
```

Le port 3000 est maintenant libéré.


## Étape bonus – Scan complet des ports ouverts (facultatif)

Pour s'assurer de l’état global du serveur, Éric utilise l’outil `nmap` :

```bash
sudo apt install nmap
sudo nmap -sT -O localhost
```



# Questions

1. Pourquoi le port 3000 restait-il bloqué malgré l’usage de `kill` ?
2. Quelle méthode permet d’identifier le nom du service qui utilise un port spécifique ?
3. Quelle commande pourrait-on utiliser pour rechercher tous les services contenant le mot `wiki` dans leur nom ou leur description ?
4. Proposez un script bash permettant d’automatiser cette investigation.



# Annexe 1 : Méthodes plus avancées


La méthode décrite (recherche manuelle à partir du port avec `lsof`, `ps`, `systemctl`, etc.) est **fiable mais artisanale**. Il existe des **méthodes plus avancées et automatisées**, utiles pour les environnements complexes, comme les serveurs de production ou les clusters. Voici un aperçu des alternatives plus avancées :



##  Méthodes plus avancées pour identifier le service derrière un port

### 1. **`systemd-cgls` ou `systemd-cgtop`**

Ces outils permettent de visualiser l'arborescence des services et processus contrôlés par systemd.

```bash
systemd-cgls
```

ou encore 

```bash
systemd-cgls
systemd-cgls | grep -i wiki
systemd-cgls | grep -i postgres
systemd-cgls | grep -i nginx
systemd-cgls /system.slice/wikijs.service
ls /sys/fs/cgroup/system.slice/ | grep wiki
ps -ef | grep wiki
```

Tu peux ensuite chercher le PID du processus, et voir à quel service il est rattaché.



### 2. **`systemctl status` combiné à `--property`**

Tu peux interroger tous les services et demander à voir leur `MainPID`, puis croiser avec le port :

```bash
systemctl show --all --property=MainPID --no-pager
```

Puis :

```bash
sudo lsof -i :3000
```

Et tu compares les `MainPID` au PID qui utilise le port.



### 3. **Utiliser `fuser` pour directement tuer le processus par port**

```bash
sudo fuser -k 3000/tcp
```

Cela tue directement le processus associé au port. Mais attention : cette méthode **ne désactive pas le service**, donc il peut redémarrer. Elle est utile dans un script temporaire.



### 4. **Avec `ss --processes`**

```bash
sudo ss -lptn 'sport = :3000'
```

Cela montre directement :

* le port,
* le protocole,
* le processus,
* **et parfois le nom du service** si bien configuré.



### 5. **Utiliser `netstat` avec `--program` (si disponible)**

```bash
sudo netstat -tulnp | grep 3000
```

Permet d’identifier directement le programme utilisant un port. Tu peux croiser avec `systemctl` :

```bash
ps -p <PID> -o pid,cmd
```



### 6. **Via journaux `systemd`**

Si le service a bien démarré par systemd, tu peux faire :

```bash
journalctl -u wikijs.service
```

Cela te montre l’historique d’exécution, y compris le port écouté.



## Bonus : Automatisation avec un script

Un script plus avancé pourrait :

1. Chercher le port avec `ss` ou `lsof`
2. Extraire le PID
3. Associer le PID à un service via `systemctl`
4. Afficher le nom du service automatiquement


<br/>
<br/>

# Annexe 2 - Automatiser l'identification du service systemd responsable d'un port donné. 




###  Script Bash : Identifier le service systemd derrière un port

```bash
#!/bin/bash

# Demande à l'utilisateur d'entrer le numéro de port
read -p "Entrez le numéro de port (ex: 3000) : " PORT

# Étape 1 : Obtenir le PID du processus qui écoute sur ce port
PID=$(sudo lsof -i :$PORT -sTCP:LISTEN -t)

# Vérification si un PID a été trouvé
if [ -z "$PID" ]; then
    echo "Aucun processus ne semble écouter sur le port $PORT."
    exit 1
fi

echo "Processus détecté sur le port $PORT : PID = $PID"

# Étape 2 : Afficher les informations sur le processus
echo "Commande liée à ce PID :"
ps -p $PID -o pid,cmd

# Étape 3 : Chercher le service systemd auquel appartient ce PID
echo ""
echo "Recherche du service systemd correspondant..."
SERVICE=$(systemctl | grep $PID | awk '{print $1}')

if [ -z "$SERVICE" ]; then
    echo "Aucun service systemd trouvé automatiquement pour le PID $PID."
    echo "Vérification manuelle possible avec :"
    echo "  systemctl status <nom_du_service>"
    echo "ou recherche dans /etc/systemd/system/"
else
    echo "Service systemd détecté : $SERVICE"
    echo ""
    echo "Statut du service :"
    systemctl status $SERVICE
fi
```



###  Utilisation :

1. Enregistre ce script dans un fichier, par exemple `recherche_service_port.sh`
2. Rends-le exécutable :

```bash
chmod +x recherche_service_port.sh
```

3. Exécute-le :

```bash
./recherche_service_port.sh
```



###  Ce que vous avez appris :
* Comment lier un port à un processus (`lsof`)
* Comment remonter au service via `systemctl` et `grep`
* Comment automatiser un diagnostic réseau en Bash

