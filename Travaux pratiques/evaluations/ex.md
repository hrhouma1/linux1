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



