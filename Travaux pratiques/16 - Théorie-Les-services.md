# **Cours : Gestion des Services et Planification des Tâches sous Linux**



## **Partie 1 – Introduction aux Démons et Services**

### 1.1 Qu’est-ce qu’un démon ?

* Définition simple
* Définition technique
* Exemples typiques de démons : `sshd`, `httpd`, `crond`, `rsyslogd`
* Comment les démons sont créés au démarrage

### 1.2 Démons vs Services : quelle différence ?

* Point commun : processus en arrière-plan
* Distinction au niveau de la gestion et de l’activation



## **Partie 2 – Introduction à systemd**

### 2.1 Qu’est-ce que systemd ?

* Remplacement de SysV Init
* Fonctionnalités principales
* Concepts clés : unités, cibles, journaux

### 2.2 Commandes essentielles avec `systemctl`

* `start`, `stop`, `restart`, `status`, `enable`, `disable`
* Gestion des services au démarrage
* Liste des services actifs : `systemctl list-units --type=service`

### 2.3 Journaux avec `journalctl`

* Lire les logs d’un service
* Filtrage par date, service, boot, etc.

---

## **Partie 3 – Créer et Gérer ses Propres Services**

### 3.1 Fichiers `.service` personnalisés

* Structure d’un fichier unit (`[Unit]`, `[Service]`, `[Install]`)
* Exemple complet avec `/usr/local/bin/webapp`
* Activation au démarrage

### 3.2 Redémarrage automatique et gestion des erreurs

* `Restart=always`, `RestartSec`, etc.

### 3.3 Analyse de performance

* Utilisation de `systemd-analyze`, `blame`, `critical-chain`



## **Partie 4 – Planification avec cron**

### 4.1 Syntaxe de base d’un `cron job`

* Champs de la syntaxe (`* * * * *`)
* Signification de chaque champ
* Répétitions, intervalles, jours spécifiques

### 4.2 Exemples pratiques avec correction

* **Exercice 1** : Sauvegarde de `/etc` tous les dimanches à 3h
* **Exercice 2** : Sauvegarde PostgreSQL quotidienne à 23h30
* **Exercice 3** : Vérification et redémarrage automatique de `nginx`
* **Exercice 4** : Téléchargement automatique avec `wget`
* **Exercice 5** : Rotation de pages HTML toutes les minutes
* **Exercice 6** : Script `mypage.sh` avec index.txt dynamique

### 4.3 Contrôle d’accès à `cron`

* Fichiers `cron.allow` et `cron.deny`



## **Partie 5 – Tâches ponctuelles avec at**

### 5.1 Introduction à `at`

* Utilisation de base : `at now + 1 minute`
* Liste des jobs (`atq`), suppression (`atrm`), contenu (`at -c`)
* Exécution différée d’un script

### 5.2 Exemples pratiques avec correction

* **Exercice 9** : Script différé `traitement.sh`
* Contrôle d’accès avec `at.allow` et `at.deny`


## **Partie 6 – Sauvegardes Avancées et Planifiées**

### 6.1 Sauvegarde de fichiers avec `rsync`

* Options : `-a`, `--bwlimit`, `--delete`
* Optimisation de la bande passante
* Programmation avec `cron`

### 6.2 Exercice : rsync limité à 200KB/s de 19h à 7h

* Script de synchronisation
* Planification avec `cron` (lancement/arrêt automatique)



## **Partie 7 – Logs et Rotation**

### 7.1 Centralisation des logs avec `rsyslog`

* Modification du fichier `/etc/rsyslog.conf`
* Définition d’un fichier unique `/var/log/syslog.log`

### 7.2 Rotation avec `logrotate`

* Création du fichier `/etc/logrotate.d/syslog`
* Options : `weekly`, `rotate`, `compress`, `notifempty`

### 7.3 Exercice : Surveillance journalière des logs

* Envoi par courriel des 100 dernières lignes
* Commande `tail | mail`



## **Partie 8 – Quiz et Analyse**

### 8.1 Analyse de tâches cron complexes

* Explication des lignes de crontab (Exercice 7)

### 8.2 Quiz final

* Scénarios à corriger (syntaxe erronée, accès interdit, conflits de port, etc.)


<br/>
<br/>


<br/>
<br/>

# **Partie 1 – Introduction aux Démons et Services**



## **1.1 Définition Vulgarisée : Qu’est-ce qu’un démon ?**

Un **démon** (ou *daemon* en anglais) est un **programme qui tourne en arrière-plan**, sans interface graphique, et qui **travaille silencieusement** pour offrir des **services essentiels** au système ou aux utilisateurs.

### ✦ Exemples simples :

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



