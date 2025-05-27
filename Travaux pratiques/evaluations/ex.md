# Exercice annexe : « Créer 3 nouveaux services *Wiki.js* et tester différentes façons de les arrêter »

### Objectif pédagogique

1. **Maitriser la création d’instances supplémentaires à partir d’un template systemd (`wikijs@.service`).**
2. **Comparer plusieurs commandes de “mise à l’arrêt” et repérer celles qui fonctionnent correctement (et pourquoi).**



# 1️⃣ Création de trois nouvelles instances

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



# 2️⃣ Consigne : « Arrêtez proprement les 3 services créés »

On vous propose **six** commandes possibles ; **trois** seulement sont correctes pour arrêter *uniquement* l’instance ciblée ; les autres sont inefficaces ou dangereuses.

> **Question :** *lesquelles choisissez-vous et pourquoi ?*

| Candidat | Commande à exécuter                 | Analyse attendue                                                                                                                |                                                                                    |
| -------- | ----------------------------------- | ------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------- |
| **A**    | `sudo systemctl stop wikijs@stage`  | ✔️ Bonne pratique : systemd gère le PID et les dépendances.                                                                     |                                                                                    |
| **B**    | `sudo systemctl disable wikijs@qa`  | ❌ Ne stoppe rien immédiatement : désactive seulement au prochain boot.                                                          |                                                                                    |
| **C**    | `sudo killall wikijs`               | ❌ Tue **toutes** les instances ; perte de contrôle, pas sélectif.                                                               |                                                                                    |
| **D**    | `sudo fuser -k 3005/tcp`            | ✔️ Valide : tue le processus écoutant sur le port **3005** (demo) ; fonctionne si on connaît le port, mais pas orienté service. |                                                                                    |
| **E**    | \`sudo lsof -ti:3004                | xargs sudo kill -9\`                                                                                                            | ✔️ Valide pour *qa* (port 3004) ; force‐kill PID → brutal, pas de journal systemd. |
| **F**    | `sudo systemctl reload wikijs@demo` | ❌ Reload ≠ Stop : envoie SIGHUP, le service reste actif.                                                                        |                                                                                    |



# 3️⃣ Correction/commentaires à fournir aux étudiants

1. **Réponse attendue** : A ✅, D ✅, E ✅
2. **Pourquoi ?**

| Commande                      | Effet réel                                       | Raisons                                                        |
| ----------------------------- | ------------------------------------------------ | -------------------------------------------------------------- |
| `systemctl stop wikijs@stage` | demande à systemd d’envoyer `SIGTERM` au bon PID | Respecte la journalisation ; relance facile.                   |
| `fuser -k 3005/tcp`           | tue le processus dont **le port** est 3005       | Utile quand le service n’est pas connu mais que le port l’est. |
| `lsof -ti:3004 …`             | même logique que *fuser* mais via lsof           | Variante équivalente, plus verbeuse.                           |
| Autres                        | **Incorrects** pour un *stop immédiat et ciblé*  | —                                                              |

> **Discussion** : insister sur la préférence à donner à `systemctl stop <instance>` qui …
> • enregistre l’évènement dans `journalctl`,
> • respecte les dépendances,
> • peut déclencher des *ExecStopPost* si définis.



# 4️⃣ Prolongement (facultatif)

1. **Relancer** les 3 instances en une ligne :

   ```bash
   sudo systemctl restart wikijs@{stage,qa,demo}
   ```

2. **Lister** tous les sockets ouverts par Wiki.js :

   ```bash
   sudo lsof -iTCP -sTCP:LISTEN | grep node
   ```

3. **Vérifier** qu’aucun port 3003-3005 n’est à nouveau ouvert après l’arrêt :

   ```bash
   sudo ss -tlnp | egrep '3003|3004|3005'
   ```




