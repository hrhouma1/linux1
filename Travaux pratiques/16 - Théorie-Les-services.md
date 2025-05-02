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



