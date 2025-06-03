# Arréter un service


# Prérequis:

- Avoir un site wikijs en cours d'exécution
  
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
ls -l --sort=time /etc/systemd/system/
```

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



## Questions pour les apprenants

1. Pourquoi le port 3000 restait-il bloqué malgré l’usage de `kill` ?
2. Quelle méthode permet d’identifier le nom du service qui utilise un port spécifique ?
3. Quelle commande pourrait-on utiliser pour rechercher tous les services contenant le mot `wiki` dans leur nom ou leur description ?
4. Proposez un script bash permettant d’automatiser cette investigation.

