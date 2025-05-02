# **Cours : Gestion des Services et Planification des Tâches sous Linux**



# **Partie 1 – Introduction aux Démons et Services**

### 1.1 Qu’est-ce qu’un démon ?

### 1.2 Démons vs Services : quelle différence ?

# **Partie 2 – Introduction à systemd**

### 2.1 Qu’est-ce que systemd ?

### 2.2 Commandes essentielles avec `systemctl`

### 2.3 Journaux avec `journalctl`


# **Partie 3 – Créer et Gérer ses Propres Services**

### 3.1 Fichiers `.service` personnalisés


### 3.2 Redémarrage automatique et gestion des erreurs


### 3.3 Analyse de performance



# **Partie 4 – Planification avec cron**

### 4.1 Syntaxe de base d’un `cron job`



### 4.2 Exemples pratiques avec correction



### 4.3 Contrôle d’accès à `cron`




# **Partie 5 – Tâches ponctuelles avec at**

### 5.1 Introduction à `at`



### 5.2 Exemples pratiques avec correction




# **Partie 6 – Sauvegardes Avancées et Planifiées**

### 6.1 Sauvegarde de fichiers avec `rsync`



### 6.2 Exercice : rsync limité à 200KB/s de 19h à 7h





# **Partie 7 – Logs et Rotation**

### 7.1 Centralisation des logs avec `rsyslog`


### 7.2 Rotation avec `logrotate`



### 7.3 Exercice : Surveillance journalière des logs





# **Partie 8 – Quiz et Analyse**

### 8.1 Analyse de tâches cron complexes

### 8.2 Quiz final


<br/>
<br/>


<br/>
<br/>

# **Partie 1 – Introduction aux Démons et Services**



## **1.1 Définition : Qu’est-ce qu’un démon ?**

Un **démon** (ou *daemon* en anglais) est un **programme qui tourne en arrière-plan**, sans interface graphique, et qui **travaille silencieusement** pour offrir des **services essentiels** au système ou aux utilisateurs.

###  Exemples simples :

* `sshd` : permet les connexions à distance via SSH
* `httpd` : serveur web Apache
* `crond` : planifie l'exécution de tâches
* `systemd` : orchestre tous les autres services au démarrage

---

## **1.2 Définition Technique : Nature et Fonction d’un Démon**

Un **démon** est un **processus système indépendant**, lancé :

* soit **au démarrage du système**
* soit **manuellement par l’administrateur**
* et qui **ne s’arrête pas** tant qu’il n’est pas explicitement stoppé.

Il est :

* détaché de tout terminal ou session utilisateur (`tty`)
* souvent créé par **le processus init (PID 1)**, désormais **systemd** dans la majorité des distributions modernes.

---

## **1.3 Comment les démons sont-ils créés ?**

### 3 étapes :

1. **Lancement initial** via un fichier de configuration ou script
2. **Détachement** du terminal courant
3. **Exécution en continu**, souvent en boucle infinie, jusqu'à arrêt ou crash

Sur les systèmes modernes, **systemd** s’occupe de cette gestion automatiquement via des unités `.service`.

---

## **1.4 Les noms de démons finissent par `d`**

C’est une **convention de nommage** :

* `sshd` → Secure Shell Daemon
* `ftpd` → FTP Daemon
* `systemd` → System Daemon
* `rsyslogd` → Remote SYSLOG Daemon

---

## **1.5 Liste de démons courants dans Linux**

| Démon      | Fonction principale                              |
| ---------- | ------------------------------------------------ |
| `sshd`     | Permet les connexions SSH                        |
| `crond`    | Gère les tâches planifiées                       |
| `httpd`    | Fournit un service web (Apache)                  |
| `systemd`  | Contrôle les services et le démarrage du système |
| `rsyslogd` | Gère la journalisation système                   |
| `cupsd`    | Gère l'impression                                |
| `ntpd`     | Synchronise l'heure avec des serveurs NTP        |

---

## **1.6 Démons vs Services**

* **Un démon est un processus**, un service est une **abstraction logique**.
* Un **service** peut être **un démon**, mais pas toujours (ex : `tmp.mount` n'est pas un processus mais une unité).

| Élément   | Démon                           | Service                  |
| --------- | ------------------------------- | ------------------------ |
| Nature    | Processus en arrière-plan       | Unité gérée par systemd  |
| Visible ? | Non, sauf via `ps`, `systemctl` | Visible avec `systemctl` |
| Exemple   | `sshd`                          | `sshd.service`           |

---

## **1.7 Contrôle des démons**

Sous **systemd**, tous les démons sont contrôlés via **`systemctl`**, avec les commandes suivantes :

| Action             | Commande                      |
| ------------------ | ----------------------------- |
| Démarrer           | `sudo systemctl start sshd`   |
| Arrêter            | `sudo systemctl stop sshd`    |
| Redémarrer         | `sudo systemctl restart sshd` |
| Voir le statut     | `sudo systemctl status sshd`  |
| Activer au boot    | `sudo systemctl enable sshd`  |
| Désactiver au boot | `sudo systemctl disable sshd` |

---

## **1.8 En Résumé – Points Clés à Retenir**

* Un **démon** est un **processus sans interface**, qui **fournit un service**.
* Il est souvent **démarré au boot**, géré par **systemd**.
* Pour **voir, activer, désactiver ou relancer** un démon, on utilise `systemctl`.
* Tous les services système modernes sont désormais **des unités gérées par systemd**.












<br/>
<br/>

# **Partie 2 – Introduction à systemd et systemctl**

---

## **2.1 Qu’est-ce que systemd ?**

**systemd** est le système d’initialisation par défaut de la majorité des distributions Linux modernes (Ubuntu, Fedora, Debian, CentOS, etc.).
Il remplace l'ancien système SysVinit et sert à :

* Initialiser le système au démarrage (montage des disques, démarrage des services, etc.)
* Gérer tous les services système de manière centralisée
* Offrir des outils intégrés tels que `systemctl`, `journalctl`, `systemd-analyze`, `systemd-nspawn`, etc.

### Avantages de systemd :

* Démarrage plus rapide grâce à une initialisation **parallèle**
* Redémarrage automatique des services en cas de défaillance
* Gestion avancée des dépendances entre services
* Centralisation de la journalisation

---

## **2.2 Le rôle de `systemctl`**

`systemctl` est l’outil principal en ligne de commande pour interagir avec `systemd`.

Il permet de :

* Démarrer, arrêter ou redémarrer un service
* Activer ou désactiver un service au démarrage
* Obtenir le statut d’un service
* Afficher l’ensemble des services actifs ou inactifs

---

## **2.3 Commandes fondamentales avec `systemctl`**

| Action                             | Commande                                |
| ---------------------------------- | --------------------------------------- |
| Démarrer un service                | `sudo systemctl start nom_du_service`   |
| Arrêter un service                 | `sudo systemctl stop nom_du_service`    |
| Redémarrer un service              | `sudo systemctl restart nom_du_service` |
| Recharger la configuration         | `sudo systemctl reload nom_du_service`  |
| Vérifier le statut                 | `systemctl status nom_du_service`       |
| Activer un service au démarrage    | `sudo systemctl enable nom_du_service`  |
| Désactiver un service au démarrage | `sudo systemctl disable nom_du_service` |

Remplacez `nom_du_service` par le nom réel du service concerné (ex : `sshd`, `nginx`, `apache2`, etc.).

---

## **2.4 Visualisation des services**

Afficher tous les services actifs :

```bash
systemctl list-units --type=service --state=running
```

Afficher tous les services (actifs ou non) :

```bash
systemctl list-units --type=service
```

---

## **2.5 Utilisation de `journalctl` pour consulter les logs**

`systemd` intègre un système de logs centralisé : `journald`.
Vous pouvez consulter les journaux avec la commande `journalctl`.

| Objectif                              | Commande                          |
| ------------------------------------- | --------------------------------- |
| Voir tous les logs                    | `journalctl`                      |
| Voir les logs d’un service spécifique | `journalctl -u nom_du_service`    |
| Voir les logs depuis le dernier boot  | `journalctl -b`                   |
| Suivre les logs en temps réel         | `journalctl -f`                   |
| Voir les logs de la dernière heure    | `journalctl --since "1 hour ago"` |

---

## **2.6 Les unités (`units`) dans systemd**

Une unité est un objet que systemd peut gérer. Chaque unité correspond à un fichier avec une extension spécifique.

| Type d’unité | Extension  | Exemple             |
| ------------ | ---------- | ------------------- |
| Service      | `.service` | `nginx.service`     |
| Montage      | `.mount`   | `home.mount`        |
| Socket       | `.socket`  | `cups.socket`       |
| Cible        | `.target`  | `multi-user.target` |
| Minuterie    | `.timer`   | `logrotate.timer`   |

---

## **2.7 Les cibles (`targets`) de systemd**

Une cible représente un état global du système, équivalent aux anciens niveaux d’exécution (runlevels).

| Nom de la cible     | Description                                     |
| ------------------- | ----------------------------------------------- |
| `rescue.target`     | Mode de récupération (accès root uniquement)    |
| `multi-user.target` | Mode multi-utilisateur sans interface graphique |
| `graphical.target`  | Mode avec interface graphique                   |
| `default.target`    | Cible par défaut lors du démarrage              |

Changer la cible courante :

```bash
sudo systemctl isolate multi-user.target
```

---

## **2.8 Analyse des performances de démarrage**

Mesurer le temps total de démarrage :

```bash
systemd-analyze
```

Lister les services classés par temps de chargement :

```bash
systemd-analyze blame
```

Voir la chaîne des dépendances critiques :

```bash
systemd-analyze critical-chain
```

---

## **2.9 Gérer les redémarrages automatiques**

Pour forcer le redémarrage automatique d’un service après un échec, modifiez ou créez une unité `.service` avec :

```ini
[Service]
ExecStart=/chemin/vers/script
Restart=always
RestartSec=5
```

Cela redémarre le service 5 secondes après chaque échec.

---

## **2.10 Résumé**

* `systemd` est le gestionnaire d’unités moderne des systèmes Linux
* `systemctl` est l’interface en ligne de commande pour interagir avec `systemd`
* Les services, cibles, sockets, et autres ressources sont appelés **unités**
* `journalctl` permet de consulter les logs système
* Les outils comme `systemd-analyze` permettent d’analyser les performances au démarrage




<br/>
<br/>


# **Partie 3 – Créer et Gérer ses Propres Services avec systemd**

---

## **3.1 Objectif de cette partie**

Comprendre comment :

* Créer une unité de service personnalisée
* Intégrer un script système dans systemd
* Tester, activer, désactiver et relancer ce service

---

## **3.2 Structure d’un fichier `.service`**

Les fichiers de service se trouvent généralement dans :

* `/etc/systemd/system/` (unités personnalisées et persistantes)
* `/lib/systemd/system/` (unités installées par les paquets)

### Exemple de fichier `webapp.service` :

```ini
[Unit]
Description=Service personnalisé pour lancer une application web
After=network.target

[Service]
ExecStart=/usr/local/bin/webapp
Restart=always
RestartSec=5

[Install]
WantedBy=multi-user.target
```

### Explication des sections :

* `[Unit]` : dépendances du service, description
* `[Service]` : commande à exécuter, politique de redémarrage
* `[Install]` : cible d’activation (ici, niveau multi-utilisateur)

---

## **3.3 Étapes de création d’un service systemd personnalisé**

### Étape 1 – Écrire ou localiser le script à exécuter

Exemple :

```bash
sudo nano /usr/local/bin/webapp
```

Contenu du script :

```bash
#!/bin/bash
while true; do
  echo "Application en cours d'exécution" >> /var/log/webapp.log
  sleep 30
done
```

Rendre le script exécutable :

```bash
sudo chmod +x /usr/local/bin/webapp
```

---

### Étape 2 – Créer l’unité de service

```bash
sudo nano /etc/systemd/system/webapp.service
```

Copiez le contenu suivant :

```ini
[Unit]
Description=Web Application Custom Service
After=network.target

[Service]
ExecStart=/usr/local/bin/webapp
Restart=always
RestartSec=10

[Install]
WantedBy=multi-user.target
```

---

### Étape 3 – Activer et démarrer le service

Recharger systemd pour prendre en compte le nouveau fichier :

```bash
sudo systemctl daemon-reexec
```

Ou plus fréquemment :

```bash
sudo systemctl daemon-reload
```

Activer le service au démarrage :

```bash
sudo systemctl enable webapp
```

Démarrer le service :

```bash
sudo systemctl start webapp
```

---

### Étape 4 – Vérifier le bon fonctionnement

Afficher l’état du service :

```bash
systemctl status webapp
```

Vérifier les journaux :

```bash
journalctl -u webapp
```

---

## **3.4 Modifier un service déjà en place**

Si vous modifiez le fichier `.service` :

1. Sauvegardez
2. Rechargez la configuration :

```bash
sudo systemctl daemon-reload
```

3. Redémarrez le service :

```bash
sudo systemctl restart webapp
```

---

## **3.5 Supprimer un service personnalisé**

Désactiver le service :

```bash
sudo systemctl disable webapp
```

Arrêter le service :

```bash
sudo systemctl stop webapp
```

Supprimer le fichier d’unité :

```bash
sudo rm /etc/systemd/system/webapp.service
```

Recharger systemd :

```bash
sudo systemctl daemon-reload
```

---

## **3.6 Cas pratique : Redémarrage automatique d’un script planté**

Dans le bloc `[Service]`, utilisez :

```ini
Restart=always
RestartSec=5
```

Cela permet :

* Un redémarrage automatique après échec
* Une attente de 5 secondes entre chaque redémarrage

---

## **3.7 Cas pratique : Service temporaire non persistant**

Créer un service que l’on souhaite **exécuter uniquement une fois**, sans l’enregistrer pour les démarrages futurs :

```bash
sudo systemctl start /etc/systemd/system/webapp.service
```

Sans `enable`, le service ne sera pas lancé au prochain redémarrage.

---

## **3.8 Résumé**

* Les services personnalisés systemd sont définis dans des fichiers `.service`
* Ils permettent d’intégrer n’importe quel script au système
* L’utilisation des commandes `daemon-reload`, `enable`, `start`, `status` est essentielle
* Un bon service inclut une politique de redémarrage robuste
* La structure `[Unit]`, `[Service]`, `[Install]` est obligatoire














<br/>
<br/>

# **Partie 4 – Planification de Tâches avec cron**

---

## **4.1 Introduction**

`cron` est un outil qui permet de programmer **des tâches répétitives** sous Linux.
Le **démon `crond`** s’exécute en arrière-plan et lit les fichiers `crontab` pour exécuter les commandes à une date/heure précises.

---

## **4.2 Structure d’une tâche cron**

```bash
*     *     *     *     *   commande à exécuter
-     -     -     -     -
|     |     |     |     |
|     |     |     |     +----- jour de la semaine (0 à 6, dimanche = 0)
|     |     |     +----------- mois (1 à 12)
|     |     +----------------- jour du mois (1 à 31)
|     +----------------------- heure (0 à 23)
+---------------------------- minute (0 à 59)
```

---

## **4.3 Astuce pour s’entraîner rapidement**

Modifier la crontab pour l’utilisateur courant :

```bash
crontab -e
```

Afficher les tâches cron existantes :

```bash
crontab -l
```

---

## **4.4 Exercice 1 – Sauvegarde de /etc tous les dimanches à 3h**

### Objectif :

Créer un script qui archive `/etc` dans `/var/backups`.

### Étapes :

1. Créer un dossier d'exercice :

```bash
mkdir ~/cron_tp && cd ~/cron_tp
```

2. Créer un fichier `backup.sh` :

```bash
nano backup.sh
```

Contenu :

```bash
#!/bin/bash
tar -czf /var/backups/etc-backup-$(date +%Y%m%d).tar.gz /etc
```

3. Rendre le script exécutable :

```bash
chmod +x backup.sh
```

4. Créer le dossier de destination si besoin :

```bash
sudo mkdir -p /var/backups
```

5. Ajouter à la crontab :

```bash
crontab -e
```

Ajouter la ligne suivante :

```
0 3 * * 0 /home/utilisateur/cron_tp/backup.sh
```

Remplacez `utilisateur` par votre nom d’utilisateur Linux.

---

## **4.5 Exercice 2 – Amuse-toi avec du texte**

### Objectif :

Écrire « Bonjour ! Nous sommes le \[date] » toutes les 5 minutes dans un fichier.

### Commande dans la crontab :

```bash
*/5 * * * * echo "Bonjour ! Nous sommes le $(date)" >> /tmp/bonjour.txt
```

Vérifiez ensuite :

```bash
tail /tmp/bonjour.txt
```

---

## **4.6 Exercice 3 – Créer une page web dynamique qui change toutes les minutes**

### Objectif :

Télécharger différentes pages web toutes les minutes et les afficher dans le dossier web.

### Étapes :

1. Créer un fichier `mypage.sh` :

```bash
nano ~/cron_tp/mypage.sh
```

Contenu :

```bash
#!/bin/bash
urls=("www.yahoo.com" "www.cmaisonneuve.qc.ca" "www.crosemont.qc.ca" "www.netflix.com")

index_file="/home/$USER/index.txt"
backup_dir="/var/www/html"

index=0
if [[ -f "$index_file" ]]; then
  read -r index < "$index_file"
fi

url=${urls[$((index % ${#urls[@]}))]}
wget -qO "$backup_dir/index.html" --timeout=10 "$url"

next_index=$(( (index + 1) % ${#urls[@]} ))
echo "$next_index" > "$index_file"
```

2. Rendre le script exécutable :

```bash
chmod +x ~/cron_tp/mypage.sh
```

3. Lancer la tâche toutes les minutes :

```bash
crontab -e
```

Ajouter :

```
* * * * * /home/utilisateur/cron_tp/mypage.sh
```

4. Tester en accédant à :

```
http://localhost/index.html
```

Chaque minute, la page changera automatiquement.

---

## **4.7 Exercice 4 – Observer les processus et les utilisateurs**

### Objectif :

Observer qui est connecté et quels processus tournent entre 9h et 17h.

### Ajouter à la crontab :

```bash
# Toutes les heures entre 9h et 17h : processus
0 9-17 * * 1-5 ps aux >> /var/log/processus.txt

# Toutes les 5 minutes entre 9h et 17h : utilisateurs connectés
*/5 9-17 * * 1-5 who >> /var/log/qui.txt
```

Visualiser ensuite :

```bash
tail /var/log/processus.txt
tail /var/log/qui.txt
```

---

## **4.8 Astuce : tester ses tâches cron en direct**

Pour s’assurer que tout fonctionne :

1. Mettre une tâche chaque minute :

```bash
* * * * * echo "Tâche test à $(date)" >> ~/cron.log
```

2. Lancer :

```bash
tail -f ~/cron.log
```

Attendez une minute : la ligne s’affichera automatiquement.

---

## **4.9 Résumé**

* `cron` permet d’exécuter des scripts à des moments précis, automatiquement.
* Un script cron doit être **exécutable** et spécifier un **chemin absolu**.
* Vous pouvez observer les résultats avec `tail`, ou créer des fichiers `.log` pour vérifier vos essais.
* Ce système permet de faire **des sauvegardes**, des **tests automatisés**, ou de **mettre à jour dynamiquement des services**.






<br/>
<br/>



# **Partie 5 – Tâches Ponctuelles avec `at`**

---

## **5.1 Qu’est-ce que `at` ?**

Le programme `at` permet de programmer **une tâche unique** à exécuter **plus tard**, **une seule fois** (contrairement à `cron`, qui est répétitif).

C’est parfait pour :

* Planifier un **redémarrage** du système
* Lancer un **script de traitement différé**
* Automatiser une **opération sensible** à un moment précis

---

## **5.2 Vérification et installation du service `atd`**

Avant toute chose, assurez-vous que le démon `atd` est actif :

```bash
sudo systemctl start atd
sudo systemctl enable atd
sudo systemctl status atd
```

Si `at` n’est pas installé :

```bash
sudo apt install at   # Debian/Ubuntu
sudo yum install at   # CentOS/RHEL
```

---

## **5.3 Syntaxe de base**

```bash
at <heure ou moment>
```

Exemples :

```bash
at now + 1 minute
at 17:00
at 09:30 tomorrow
at now + 2 days
```

Une fois dans l’éditeur `at>`, tapez la commande à exécuter, puis terminez par `Ctrl + D`.

---

## **5.4 Exercice 1 – Planifier un message différé**

### Objectif :

Afficher un message sur votre terminal dans **1 minute**.

1. Lancez la commande :

```bash
at now + 1 minute
```

2. Dans le prompt `at>`, tapez :

```bash
echo "Ceci est un message planifié avec at"
```

3. Terminez avec :

```bash
Ctrl + D
```

4. Attendez une minute et vérifiez que le message s’affiche.

---

## **5.5 Exercice 2 – Script différé sur un autre terminal**

### Objectif :

Afficher un message sur un autre terminal (ex: `/dev/pts/0`).

1. Créez un script `traitement.sh` :

```bash
nano ~/cron_tp/traitement.sh
```

Contenu :

```bash
#!/bin/bash
echo "Message différé sur le terminal /dev/pts/0" > /dev/pts/0
```

2. Rendre le script exécutable :

```bash
chmod +x ~/cron_tp/traitement.sh
```

3. Planifier le script :

```bash
at now + 2 minutes -f ~/cron_tp/traitement.sh
```

Ouvrez le terminal `pts/0` en parallèle et observez.

---

## **5.6 Gestion des tâches `at`**

| Action                               | Commande              |
| ------------------------------------ | --------------------- |
| Voir les tâches planifiées           | `atq` ou `at -l`      |
| Supprimer une tâche (ID = 5 par ex.) | `atrm 5` ou `at -d 5` |
| Voir le contenu de la tâche ID 5     | `at -c 5`             |

---

## **5.7 Contrôle d’accès à `at`**

| Fichier         | Fonction                                                             |
| --------------- | -------------------------------------------------------------------- |
| `/etc/at.allow` | Seuls les utilisateurs listés ici peuvent utiliser `at`              |
| `/etc/at.deny`  | Tous les utilisateurs **sauf** ceux listés ici peuvent utiliser `at` |

Priorité :

* Si `at.allow` existe, **seuls** les utilisateurs dans ce fichier ont le droit.
* Sinon, `at.deny` s’applique.

### Exemple : interdire `toto`

```bash
echo toto >> /etc/at.deny
```

---

## **5.8 Exercice 3 – Planifier un redémarrage vendredi à 17h**

### Objectif :

Automatiser un redémarrage à une date précise.

1. Tapez :

```bash
at 17:00 Fri
```

2. Dans le prompt `at>`, entrez :

```bash
sudo /sbin/shutdown -r now
```

3. Terminez avec `Ctrl + D`

Note : Pour tester sans redémarrage, utilisez `echo "Redémarrage prévu"` à la place.

---

## **5.9 Exercice 4 – Script automatique dans 2 jours**

1. Créez un script simple :

```bash
echo "echo Traitement terminé dans 2 jours" > ~/cron_tp/futur.sh
chmod +x ~/cron_tp/futur.sh
```

2. Planifiez-le :

```bash
at now + 2 days -f ~/cron_tp/futur.sh
```

---

## **5.10 Résumé**

* `at` est utile pour des **tâches ponctuelles**, uniques, différées.
* Il est complémentaire de `cron`, qui est cyclique.
* Vous pouvez observer **immédiatement** l’effet avec `echo`, ou construire des **scripts différés**.



<br/>
<br/>



# **Partie 6 – Sauvegardes Avancées et Planifiées avec `rsync`**

---

## **6.1 Pourquoi utiliser `rsync` ?**

`rsync` est un outil de synchronisation puissant qui permet de copier des fichiers :

* localement ou entre deux machines
* de manière **optimisée** (il ne copie que les fichiers modifiés)
* avec **compression, filtrage, sauvegarde et journalisation**

Il est idéal pour :

* des **sauvegardes régulières**
* des **migrations de serveurs**
* des **copies différentielles rapides**

---

## **6.2 Syntaxe de base**

```bash
rsync [options] source destination
```

Exemple simple :

```bash
rsync -av /home/user/documents /mnt/backup/
```

Options fréquentes :

* `-a` : archive (préserve les permissions, liens, dates)
* `-v` : verbose (affiche ce qui est copié)
* `--delete` : supprime les fichiers absents de la source
* `--bwlimit=KBps` : limite la bande passante utilisée

---

## **6.3 Exercice 1 – Sauvegarde locale avec limitation de bande passante**

### Objectif :

Sauvegarder le dossier `/home` dans `/mnt/backup` tous les soirs à 20h, **sans saturer la bande passante**.

### Étapes :

1. Créer le script :

```bash
mkdir -p ~/cron_tp
nano ~/cron_tp/sync_home.sh
```

Contenu :

```bash
#!/bin/bash
rsync -av --delete --bwlimit=200 /home/ /mnt/backup/
```

2. Rendre le script exécutable :

```bash
chmod +x ~/cron_tp/sync_home.sh
```

3. Ajouter dans `crontab` :

```bash
crontab -e
```

Ajouter :

```
0 20 * * * /home/utilisateur/cron_tp/sync_home.sh
```

(Remplacez `utilisateur` par votre nom réel)

---

## **6.4 Exercice 2 – Synchronisation à distance**

### Objectif :

Synchroniser `/var/www` vers un autre serveur via SSH.

1. Commande directe (à tester manuellement avant automatisation) :

```bash
rsync -avz -e ssh /var/www user@remote_host:/backup/www
```

2. Script automatisé :

```bash
nano ~/cron_tp/rsync_remote.sh
```

Contenu :

```bash
#!/bin/bash
rsync -az -e ssh /var/www user@192.168.0.100:/backup/www
```

3. Crontab (exécution quotidienne à 01h) :

```
0 1 * * * /home/utilisateur/cron_tp/rsync_remote.sh
```

Pré-requis :

* accès SSH sans mot de passe (clé publique/privée configurée)
* droits en écriture sur `/backup/www` sur la machine distante

---

## **6.5 Exercice 3 – Sauvegarde uniquement la nuit**

### Objectif :

Limiter l’exécution de `rsync` aux heures non ouvrables (19h à 7h).
Arrêter tout processus `rsync` en journée.

### Étapes :

1. Script de lancement `rsync` :

```bash
crontab -e
```

Ajouter :

```
0 19 * * * /home/utilisateur/cron_tp/sync_home.sh
```

2. Script d’arrêt de sécurité :

```
0 7 * * * pkill rsync
```

Cela garantit que :

* `rsync` commence à 19h
* S’il est encore actif à 7h, il est tué

---

## **6.6 Exercice 4 – Sauvegarde avec journalisation**

1. Script :

```bash
nano ~/cron_tp/rsync_logged.sh
```

Contenu :

```bash
#!/bin/bash
date >> /var/log/rsync_backup.log
rsync -av --delete /home /mnt/backup >> /var/log/rsync_backup.log 2>&1
```

2. Planification quotidienne :

```bash
0 22 * * * /home/utilisateur/cron_tp/rsync_logged.sh
```

3. Lecture du journal :

```bash
tail -n 20 /var/log/rsync_backup.log
```

---

## **6.7 Résumé**

* `rsync` est un outil **rapide**, **intelligent**, et **contrôlable**
* Il peut être limité en bande passante avec `--bwlimit`
* Il peut être automatisé avec `cron`
* Il peut être **restreint à certaines plages horaires**
* Il permet une **journalisation complète** pour audit et suivi




<br/>
<br/>






# **Partie 7 – Gestion des Logs et Rotation avec `logrotate`**

---

## **7.1 Pourquoi utiliser `logrotate` ?**

Les journaux systèmes (dans `/var/log/`) **augmentent continuellement** avec le temps :

* Ils peuvent **saturer le disque**
* Deviennent **lourds à consulter**
* Peuvent **réduire les performances** si mal gérés

**logrotate** permet de :

* Faire une **rotation automatique des logs**
* Supprimer ou compresser les anciens fichiers
* Conserver un historique limité dans le temps
* Exécuter une commande après la rotation (ex : redémarrer un service)

---

## **7.2 Fonctionnement général**

`logrotate` est généralement exécuté **quotidiennement par `cron`**, via :

```bash
/etc/cron.daily/logrotate
```

Il lit la **configuration globale** :

```bash
/etc/logrotate.conf
```

Et les **configurations spécifiques** :

```bash
/etc/logrotate.d/
```

---

## **7.3 Syntaxe d’un fichier de configuration**

### Exemple :

```bash
/var/log/webapp.log {
    weekly
    rotate 4
    compress
    missingok
    notifempty
    postrotate
        systemctl restart webapp
    endscript
}
```

### Explications :

* `weekly` : rotation chaque semaine
* `rotate 4` : conserver 4 anciens fichiers
* `compress` : compresser les anciens logs
* `missingok` : ne pas afficher d’erreur si le fichier est manquant
* `notifempty` : ne pas tourner le fichier s’il est vide
* `postrotate` : exécuter une commande après la rotation

---

## **7.4 Exercice 1 – Rotation d’un journal personnalisé**

### Objectif :

Faire la rotation de `/var/log/rsync_backup.log` chaque semaine, garder 4 versions compressées.

1. Créer le fichier `/etc/logrotate.d/rsync_backup` :

```bash
sudo nano /etc/logrotate.d/rsync_backup
```

2. Contenu :

```bash
/var/log/rsync_backup.log {
    weekly
    rotate 4
    compress
    missingok
    notifempty
}
```

3. Tester la configuration :

```bash
sudo logrotate /etc/logrotate.conf --debug
```

4. Forcer la rotation pour test :

```bash
sudo logrotate /etc/logrotate.conf --force
```

5. Vérifier :

```bash
ls -lh /var/log/rsync_backup*
```

---

## **7.5 Exercice 2 – Rotation avec redémarrage de service**

### Objectif :

Faire la rotation de `/var/log/webapp.log` et redémarrer le service `webapp`.

1. Créer le fichier :

```bash
sudo nano /etc/logrotate.d/webapp
```

2. Contenu :

```bash
/var/log/webapp.log {
    weekly
    rotate 2
    compress
    missingok
    notifempty
    postrotate
        systemctl restart webapp
    endscript
}
```

3. Forcer une rotation pour tester :

```bash
sudo logrotate -f /etc/logrotate.conf
```

---

## **7.6 Exercice 3 – Rotation et envoi par courriel (simulation)**

### Objectif :

Envoyer les 100 dernières lignes du fichier `/var/log/syslog` chaque nuit à minuit.

1. Ajouter dans la crontab :

```bash
crontab -e
```

Contenu :

```
0 0 * * * tail -n 100 /var/log/syslog | mail -s "Derniers logs système" votreadresse@email.com
```

2. Pour tester l'envoi localement :

```bash
mail
```

Prérequis : le système doit avoir un **agent de mail installé** (`mailutils`, `sendmail`, `postfix`...).

---

## **7.7 Exercice 4 – Rotation personnalisée d’un fichier simulé**

1. Créez un faux journal :

```bash
echo "ligne test" >> /tmp/test.log
```

2. Créez le fichier `/etc/logrotate.d/testlog` :

```bash
sudo nano /etc/logrotate.d/testlog
```

Contenu :

```bash
/tmp/test.log {
    daily
    rotate 3
    compress
    missingok
    notifempty
}
```

3. Forcez la rotation :

```bash
sudo logrotate -f /etc/logrotate.conf
```

4. Observez les fichiers :

```bash
ls -lh /tmp/test.log*
```

---

## **7.8 Résumé**

* `logrotate` gère la taille, la fréquence, la conservation et la compression des logs
* Il s’intègre parfaitement avec `cron` (via `/etc/cron.daily/logrotate`)
* Il peut redémarrer des services ou envoyer des mails
* Les fichiers de configuration personnalisés vont dans `/etc/logrotate.d/`





# **Partie 8 – Quiz Final et Évaluation Pratique**

---

## **8.1 Objectif pédagogique**

Valider que l’apprenant sait :

* Créer, activer et surveiller un service systemd personnalisé
* Planifier des tâches répétitives et ponctuelles avec `cron` et `at`
* Synchroniser des dossiers avec `rsync` et limiter la bande passante
* Gérer la rotation des journaux avec `logrotate`
* Utiliser les bons outils de surveillance (`systemctl`, `journalctl`, `logrotate`, etc.)

---

## **8.2 Partie A – Lecture et analyse de `crontab` (5 points)**

Expliquez **ce que fait chaque ligne** suivante :

1. `0 3 * * 0 /usr/local/bin/backup.sh`
2. `*/10 9-17 * * 1-5 who >> /var/log/connected.log`
3. `@reboot systemctl restart nginx`
4. `0 19 * * * /home/user/sync.sh`
5. `0 7 * * * pkill rsync`

---

## **8.3 Partie B – Création de service systemd (15 points)**

Vous devez créer un service appelé `weather_logger` qui :

* Exécute un script `/usr/local/bin/weather.sh`
* Écrit la température dans `/var/log/weather.log` toutes les 5 minutes (via cron)
* Le service doit se relancer automatiquement en cas d’échec
* Fournir le fichier `weather_logger.service`
* Activer et démarrer le service
* Vérifier son statut et ses logs

---

## **8.4 Partie C – Planification différée avec `at` (5 points)**

1. Créez un script `/home/user/alert.sh` qui envoie un message "Test de sécurité terminé" dans un fichier `/tmp/alerte.log`.
2. Programmez ce script pour qu’il s’exécute dans **2 heures** exactement à partir de maintenant.

---

## **8.5 Partie D – Synchronisation `rsync` (10 points)**

Créez un script `sync_project.sh` qui :

* Sauvegarde le dossier `/home/user/projects/` vers `/mnt/sauvegardes/`
* Supprime les fichiers obsolètes (`--delete`)
* Limite la bande passante à 150 KB/s
* Enregistre les actions dans `/var/log/sync.log`

Planifiez ce script pour qu’il s’exécute **tous les soirs à 22h**.

---

## **8.6 Partie E – Rotation de logs avec `logrotate` (10 points)**

1. Créez un fichier de configuration `/etc/logrotate.d/weather` pour le fichier `/var/log/weather.log`
2. Exigences :

   * Rotation hebdomadaire
   * 3 fichiers conservés
   * Compression activée
   * Rotation même si vide
   * Redémarrage du service `weather_logger` après la rotation

---

## **8.7 Partie F – Étude de cas finale : scénario complet (15 points)**

**Contexte :** Vous gérez un serveur Linux sur lequel tourne une application personnalisée (`/usr/local/bin/monitor.py`).

Objectifs :

1. Créer un service `monitor.service` qui exécute ce script au démarrage du système.
2. Planifier une sauvegarde du répertoire `/opt/monitoring` tous les jours à 1h dans `/mnt/monitor_backup` avec `rsync`, limitée à 250 KB/s.
3. Créer un fichier `/etc/logrotate.d/monitoring.log` pour `/var/log/monitoring.log`, avec rotation tous les 2 jours, conservation de 5 logs compressés.
4. Envoyer les 100 dernières lignes de `/var/log/syslog` par mail chaque matin à 6h à `admin@example.com`.

---

## **8.8 Barème (sur 60 points)**

| Partie                      | Points |
| --------------------------- | ------ |
| A – Analyse de crontab      | 5      |
| B – Service systemd         | 15     |
| C – Planification avec `at` | 5      |
| D – Script rsync            | 10     |
| E – logrotate personnalisé  | 10     |
| F – Cas complet             | 15     |

---

## **8.9 Consignes finales**

* Tous les scripts doivent être exécutable (`chmod +x`)
* Les chemins doivent être **absolus**
* Vous devez montrer la sortie de :

  * `systemctl status`
  * `journalctl -u`
  * `ls -lh` dans les répertoires de logs et sauvegardes


