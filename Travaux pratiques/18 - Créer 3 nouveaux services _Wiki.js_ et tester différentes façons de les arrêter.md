# Exercice annexe : « Créer 3 nouveaux services *Wiki.js* et tester différentes façons de les arrêter »

### Objectif 

1. **Maitriser la création d’instances supplémentaires à partir d’un template systemd (`wikijs@.service`).**
2. **Comparer plusieurs commandes de “mise à l’arrêt” et repérer celles qui fonctionnent correctement (et pourquoi).**



# Création de trois nouvelles instances

*(on part du même gabarit `/etc/systemd/system/wikijs@.service` déjà en place)*

| Environnement | Compte Linux   | Dossier                | Port | Base MariaDB   |
| ------------- | -------------- | ---------------------- | ---- | -------------- |
| stage         | `wikijs_stage` | `/var/www/stage.site1` | 3003 | `wikidb_stage` |
| qa            | `wikijs_qa`    | `/var/www/qa.site1`    | 3004 | `wikidb_qa`    |
| demo          | `wikijs_demo`  | `/var/www/demo.site1`  | 3005 | `wikidb_demo`  |

> **Astuce** : faites exactement comme pour `dev/test/prod`, en changeant seulement :

> • le nom d’utilisateur `wikijs_stage`…

> • le dossier `/var/www/stage.site1`…

> • la base/port dans `config.yml`.

Une fois les dossiers/configs prêts :

```bash
sudo systemctl enable --now wikijs@stage
sudo systemctl enable --now wikijs@qa
sudo systemctl enable --now wikijs@demo
```

Contrôle :

```bash
sudo systemctl status wikijs@stage --no-pager
sudo systemctl status wikijs@qa    --no-pager
sudo systemctl status wikijs@demo  --no-pager
```














<br/>

# Annexe 1

```bash
# 1. Afficher tous les processus Node avec l’unité systemd (si elle existe)
ps -eo pid,user,unit,cmd | grep -w node

# 2. Version compacte : PID + ligne de commande
pgrep -a node

# 3. Boucle : pour chaque PID Node, montrer les ports qu’il écoute
for pid in $(pgrep node); do lsof -Pan -p "$pid" -i; done

# 4. Afficher directement tous les sockets Node ouverts
ss -tlnp | grep node

# 5. Pour un PID précis (REMPLACEZ 1234 par le vrai PID) :
systemctl status --pid=1234
cat /proc/1234/cgroup | grep system.slice

# 6. Arrêter proprement l’instance systemd (REMPLACEZ wikijs@qa)
sudo systemctl stop wikijs@qa

# 7. Plan B si aucun service systemd (toujours remplacer 1234)
kill -15 1234        # arrêt doux
kill -9  1234        # arrêt brutal (ultime recours)

# 8. Vérifier qu’il ne reste plus de Node en écoute
ss -tlnp | grep node
```


<br/>

# Annexe 2



## Tenter plusieurs commandes d’arrêt

```bash
# Commande A – arrêt correct par systemd
sudo systemctl stop wikijs@stage

# Commande B – erreur logique : désactive pour plus tard, ne stoppe pas
sudo systemctl disable wikijs@qa

# Commande C – ⚠️ très risquée : tue tous les processus "wikijs"
sudo killall wikijs

# Commande D – arrêt indirect via le port de l’instance demo (3005)
sudo fuser -k 3005/tcp

# Commande E – arrêt forcé de qa (port 3004) via lsof + kill -9
sudo lsof -ti:3004 | xargs sudo kill -9

# Commande F – reload ≠ stop : ne coupe pas le service
sudo systemctl reload wikijs@demo
```



## Relancer et analyser

### Réponses correctes uniquement :

```bash
sudo systemctl stop wikijs@stage
sudo fuser -k 3005/tcp
sudo lsof -ti:3004 | xargs sudo kill -9
```

### Vérification de l’arrêt :

```bash
# Vérifier qu’aucun service n’est encore actif
sudo systemctl status wikijs@stage
sudo systemctl status wikijs@qa
sudo systemctl status wikijs@demo

# Voir si des ports sont toujours écoutés
sudo ss -tlnp | grep node
```



## Prolongement (facultatif)

```bash
# 1. Redémarrer proprement toutes les instances
sudo systemctl restart wikijs@{stage,qa,demo}
```

```bash
# 2. Revoir tous les ports Node.js actuellement actifs
sudo lsof -iTCP -sTCP:LISTEN | grep node
```

```bash
# 3. Vérifier que tous les ports sont bien libérés après un arrêt
sudo systemctl stop wikijs@{stage,qa,demo}
sudo ss -tlnp | egrep '3003|3004|3005'
```

<br/>

# Annexe 3


<h1 id="voir-ports">🔍 1. Voir tous les ports actifs et les processus liés</h1>

| Commande                                            | Commentaire / But                                                         |
| --------------------------------------------------- | ------------------------------------------------------------------------- |
| `sudo ss -tuln`                                     | Affiche les ports ouverts TCP/UDP (écoute), sans résolution DNS (rapide). |
| `sudo lsof -i -P -n`                                | Affiche tous les ports et les programmes qui les utilisent.               |
| `sudo lsof -i -P -n \| grep node`                   | Filtre pour ne voir que les processus Node.js.                            |
| `sudo netstat -tulnp`                               | Ancienne commande équivalente à `ss`, affiche aussi les PID.              |
| `sudo apt install net-tools`                        | Installe `netstat` (nécessaire sur Ubuntu moderne).                       |
| `ps aux \| grep node`                               | Liste tous les processus contenant "node".                                |
| `sudo lsof -i :3000`                                | Affiche le processus qui écoute spécifiquement sur le port 3000.          |
| `sudo apt install nmap`                             | Installe `nmap`, un scanner de ports.                                     |
| `sudo nmap -sT -O localhost`                        | Scanne les ports ouverts en TCP sur la machine locale.                    |
| `docker ps --format "table {{.Names}}\t{{.Ports}}"` | Liste les conteneurs Docker avec leurs ports exposés.                     |



<h1 id="systemd">🛑 2. Gestion des services Node.js avec systemd</h1>

| Commande                                              | Commentaire / But                                     |
| ----------------------------------------------------- | ----------------------------------------------------- |
| `systemctl \| grep node`                              | Cherche les services contenant "node".                |
| `systemctl list-units --type=service`                 | Liste tous les services en cours.                     |
| `ls /etc/systemd/system/`                             | Liste les fichiers `.service` installés manuellement. |
| `sudo systemctl stop nom-du-service`                  | Arrête proprement un service (ex: `wikijs.service`).  |
| `sudo systemctl disable nom-du-service`               | Empêche le service de démarrer au prochain boot.      |
| `sudo rm /etc/systemd/system/nom-du-service.service`  | Supprime le fichier de service.                       |
| `sudo systemctl daemon-reexec`                        | Recharge complètement `systemd`.                      |
| `sudo systemctl daemon-reload`                        | Recharge les fichiers `.service`.                     |
| `systemctl list-units --type=service \| grep -i node` | Liste les services Node.js (insensible à la casse).   |
| `systemctl list-units --type=service \| grep 3100`    | Cherche les services associés au port 3100.           |
| `sudo systemctl reset-failed`                         | Réinitialise l’état des services en échec.            |



<h1 id="kill">⚠️ 3. Arrêt manuel d’un processus (si pas un service)</h1>

| Commande                                        | Commentaire / But                                       |
| ----------------------------------------------- | ------------------------------------------------------- |
| `sudo kill 1064738`                             | Tente de tuer le processus (PID 1064738) proprement.    |
| `sudo kill -9 1064738`                          | Tuer le processus de force (SIGKILL).                   |
| `ps -fp 1072723`                                | Affiche le détail du processus 1072723 (format étendu). |
| `sudo ls -l /proc/1072723/exe`                  | Montre le binaire réel exécuté par le processus.        |
| `sudo cat /proc/1072723/cmdline \| tr '\0' ' '` | Affiche la commande exacte qui a lancé le processus.    |



<h1 id="pm2">🔁 4. Détection et arrêt des processus gérés par PM2</h1>

| Commande         | Commentaire / But                                |
| ---------------- | ------------------------------------------------ |
| `which pm2`      | Vérifie si `pm2` est installé.                   |
| `pm2 list`       | Liste les applications gérées par PM2.           |
| `pm2 stop all`   | Arrête toutes les applications PM2.              |
| `pm2 delete all` | Supprime toutes les références aux applications. |
| `pm2 unstartup`  | Désactive le démarrage auto de PM2 au boot.      |



<h1 id="suppression">🧹 5. Suppression complète d’un service systemd</h1>

| Commande                                           | Commentaire / But                                       |
| -------------------------------------------------- | ------------------------------------------------------- |
| `ls /etc/systemd/system/ \| grep -i wiki`          | Filtre les fichiers `.service` liés à "wiki".           |
| `sudo systemctl stop wikijs.service`               | Stoppe le service avant suppression.                    |
| `sudo systemctl disable wikijs.service`            | Désactive le service.                                   |
| `sudo rm /etc/systemd/system/wikijs.service`       | Supprime le fichier du service.                         |
| `sudo systemctl daemon-reload`                     | Recharge la config systemd.                             |
| `sudo systemctl reset-failed`                      | Réinitialise les erreurs systemd.                       |
| `systemctl list-units --type=service \| grep wiki` | Vérifie si le service a bien disparu.                   |
| `systemctl status wikijs.service`                  | Vérifie si le service existe encore.                    |
| `ls -l /etc/systemd/system/`                       | Liste complète pour vérifier manuellement.              |
| `grep -r ExecStart /etc/systemd/system/`           | Cherche les lignes de démarrage dans tous les services. |



<br/>

# Annexe 4 - ports, systemd, kill, PM2, suppression





###  Liste exhaustive des commandes 

```bash
sudo ss -tuln
sudo lsof -i -P -n
sudo lsof -i -P -n | grep node
sudo netstat -tulnp
sudo apt install net-tools
ps aux | grep node
sudo lsof -i :3000
sudo apt install nmap
sudo nmap -sT -O localhost
docker ps --format "table {{.Names}}\t{{.Ports}}"
systemctl | grep node
systemctl list-units --type=service
ls /etc/systemd/system/
sudo systemctl stop nom-du-service
sudo systemctl stop wikijs.service
sudo systemctl disable nom-du-service
sudo rm /etc/systemd/system/nom-du-service.service
sudo systemctl daemon-reexec
sudo systemctl daemon-reload
systemctl | grep 3100
systemctl | grep node
sudo systemctl stop wikijs.service
sudo kill 1064738
sudo kill -9 1064738
sudo ss -tuln | grep 3100
sudo lsof -i :3100
sudo systemctl list-units --type=service | grep -i node
ps aux | grep node
ps -fp 1072723
which pm2 && pm2 list
pm2 stop all
pm2 delete all
pm2 unstartup
sudo ls -l /proc/1072723/exe
sudo cat /proc/1072723/cmdline | tr '\0' ' '
ls /etc/systemd/system/ | grep -i wiki
ls /lib/systemd/system/ | grep -i wiki
grep -r 3100 /etc/systemd/system/
sudo systemctl stop nom.service
sudo systemctl disable nom.service
sudo systemctl daemon-reload
sudo systemctl reset-failed
ls /etc/systemd/system/
ls /etc/systemd/system/ | grep -i wiki
sudo systemctl stop wikijs.service
sudo systemctl disable wikijs.service
sudo rm /etc/systemd/system/wikijs.service
sudo systemctl daemon-reload
sudo systemctl reset-failed
systemctl list-units --type=service | grep wiki
systemctl status wikijs.service
ls -l /etc/systemd/system/
grep -r ExecStart /etc/systemd/system/
```





