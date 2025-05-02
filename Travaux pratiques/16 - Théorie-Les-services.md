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



## **1.1 Définition Vulgarisée : Qu’est-ce qu’un démon ?**

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

