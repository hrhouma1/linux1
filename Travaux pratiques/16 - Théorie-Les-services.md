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




