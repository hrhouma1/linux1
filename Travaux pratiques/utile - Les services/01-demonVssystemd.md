## **Exercice 1 pratique : Différence entre un démon classique et un service systemd**

### Objectif :

Comprendre concrètement la différence entre :

* Un **démon lancé manuellement** (par exemple via `&` en arrière-plan)
* Un **service supervisé par systemd**, géré via `systemctl`



# 1 - Prérequis (python)


Normalement, sur **Ubuntu 22.04**, Python 3 est **installé par défaut**. Tu peux donc exécuter le TP sans problème.

### Vérification rapide :

```bash
python3 --version
```

Tu devrais obtenir une sortie comme :

```
Python 3.10.12
```

Si pour une raison exceptionnelle Python 3 n'est pas disponible (rare mais possible sur un système minimal), installe-le avec :

```bash
sudo apt update
sudo apt install python3 -y
```



<br/>
<br/>

# 2 - Manipulations

### 1. Créer un faux démon manuellement

Créer un petit script Python simulant un démon :

```bash
nano /usr/local/bin/mon_daemon.py
```

Contenu du fichier :

```python
#!/usr/bin/env python3
import time

with open("/tmp/daemon.log", "a") as f:
    while True:
        f.write("Daemon actif...\n")
        f.flush()
        time.sleep(5)
```

Rendre le script exécutable :

```bash
chmod +x /usr/local/bin/mon_daemon.py
```

Lancer le script comme un démon classique (en arrière-plan) :

```bash
/usr/local/bin/mon_daemon.py &
```

Vérifier qu’il tourne :

```bash
ps aux | grep mon_daemon
tail -f /tmp/daemon.log
```

---

### 2. Tuer le démon avec `kill -9`

Récupérer le PID du processus avec `ps`, puis :

```bash
kill -9 <PID>
```

Résultat attendu :

* Le processus est définitivement arrêté.
* Le fichier `/tmp/daemon.log` ne s’actualise plus.

---

### 3. Créer le même script en tant que service systemd

Créer un fichier de configuration systemd :

```bash
sudo nano /etc/systemd/system/mon_daemon.service
```

Contenu du fichier :

```ini
[Unit]
Description=Mon démon supervisé

[Service]
ExecStart=/usr/local/bin/mon_daemon.py
Restart=always

[Install]
WantedBy=multi-user.target
```

Recharger systemd et activer le service :

```bash
sudo systemctl daemon-reexec
sudo systemctl daemon-reload
sudo systemctl enable --now mon_daemon
```

---

### 4. Tuer le processus du service avec `kill -9`

Récupérer le PID :

```bash
ps aux | grep mon_daemon
kill -9 <PID>
```

Résultat attendu :

* Le service est immédiatement redémarré par `systemd`.
* Le PID change (processus relancé).
* Le fichier `/tmp/daemon.log` continue de s’écrire automatiquement.

---

### Conclusion pédagogique

| Critère                 | Démon classique (manuel) | Service systemd        |
| ----------------------- | ------------------------ | ---------------------- |
| Supervision             | Aucune                   | systemd                |
| Redémarrage automatique | Non                      | Oui (`Restart=always`) |
| Commandes de gestion    | `kill`, `ps`             | `systemctl`            |
| Résilience              | Faible                   | Forte                  |




<br/>
<br/>

# 3 -  Récapitulatif minimal du TP pour Ubuntu 22.04 (Python déjà disponible) :

1. Crée le script Python :

```bash
sudo nano /usr/local/bin/mon_daemon.py
```

2. Copie-colle :

```python
#!/usr/bin/env python3
import time

with open("/tmp/daemon.log", "a") as f:
    while True:
        f.write("Daemon actif...\n")
        f.flush()
        time.sleep(5)
```

3. Rends le script exécutable :

```bash
sudo chmod +x /usr/local/bin/mon_daemon.py
```

4. Lance-le en arrière-plan pour tester :

```bash
/usr/local/bin/mon_daemon.py &
```

5. Ou crée directement le service `systemd` :

```bash
sudo nano /etc/systemd/system/mon_daemon.service
```

6. Ajoute :

```ini
[Unit]
Description=Mon démon supervisé

[Service]
ExecStart=/usr/local/bin/mon_daemon.py
Restart=always

[Install]
WantedBy=multi-user.target
```

7. Active et démarre le service :

```bash
sudo systemctl daemon-reexec
sudo systemctl daemon-reload
sudo systemctl enable --now mon_daemon
```

8. Vérifie qu’il résiste au `kill -9` :

```bash
ps aux | grep mon_daemon
kill -9 <PID>
```

