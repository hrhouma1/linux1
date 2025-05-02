

# Service VS Démon

| Critère                         | Démon                                         | Service (systemd)                                           |
| ------------------------------- | --------------------------------------------- | ----------------------------------------------------------- |
| Définition                      | Programme qui tourne en arrière-plan          | Unité gérée par `systemd` pour lancer et gérer un programme |
| Lancement                       | Automatique ou manuel (souvent au démarrage)  | Géré avec `systemctl` (`start`, `stop`, `enable`, etc.)     |
| Tourne en arrière-plan ?        | Oui                                           | Pas toujours (ex : un script court peut être un service)    |
| Géré par `systemctl` ?          | Non, sauf si intégré comme service            | Oui                                                         |
| Nécessite un fichier `.service` | Non                                           | Oui (ex : `/etc/systemd/system/mon_service.service`)        |
| Peut exister sans `systemd`     | Oui (anciens systèmes, scripts manuels)       | Non (service = unité systemd)                               |
| Exemples                        | `sshd`, `cron`, script infini en arrière-plan | `nginx.service`, `docker.service`, `mon_script.service`     |

### À retenir :

* Un **démon** est un **programme**.
* Un **service** est une **façon de gérer ce programme avec `systemd`**.


