# **TP Corrigé  Les Services -  Sauvegardes et Planification**



## **Contexte du TP**

Vous êtes administrateur d’un serveur Linux de production. Plusieurs services critiques tournent, des sauvegardes doivent être automatisées, les journaux doivent être surveillés et archivés. Votre tâche est de tout automatiser proprement, avec rotation, supervision et documentation.



## **Étape 1 – Création du service systemd `monitor`**

### 1.1 Script de supervision à surveiller

```bash
sudo nano /usr/local/bin/monitor.py
```

Contenu :

```python
#!/usr/bin/env python3
import time
while True:
    with open("/var/log/monitoring.log", "a") as f:
        f.write("Monitor actif : " + time.ctime() + "\n")
    time.sleep(60)
```

```bash
sudo chmod +x /usr/local/bin/monitor.py
```

### 1.2 Unité systemd

```bash
sudo nano /etc/systemd/system/monitor.service
```

Contenu :

```ini
[Unit]
Description=Service de supervision monitor.py
After=network.target

[Service]
ExecStart=/usr/local/bin/monitor.py
Restart=always
RestartSec=5

[Install]
WantedBy=multi-user.target
```

### 1.3 Activation et démarrage

```bash
sudo systemctl daemon-reload
sudo systemctl enable monitor
sudo systemctl start monitor
```

---

## **Étape 2 – Script de sauvegarde `rsync`**

### 2.1 Création du script

```bash
sudo nano /usr/local/bin/sync_monitor.sh
```

Contenu :

```bash
#!/bin/bash
rsync -av --delete --bwlimit=250 /opt/monitoring/ /mnt/monitor_backup/ >> /var/log/sync.log 2>&1
```

```bash
sudo chmod +x /usr/local/bin/sync_monitor.sh
```

### 2.2 Planification dans crontab

```bash
crontab -e
```

Ajouter :

```
0 1 * * * /usr/local/bin/sync_monitor.sh
```

---

## **Étape 3 – Rotation des logs `monitoring.log`**

### 3.1 Création du fichier `logrotate`

```bash
sudo nano /etc/logrotate.d/monitoring
```

Contenu :

```bash
/var/log/monitoring.log {
    rotate 5
    compress
    missingok
    notifempty
    daily
    postrotate
        systemctl restart monitor
    endscript
}
```

### 3.2 Test

```bash
sudo logrotate -f /etc/logrotate.conf
ls -lh /var/log/monitoring*
```

---

## **Étape 4 – Envoi quotidien des logs par mail**

### 4.1 Ajout dans crontab

```bash
crontab -e
```

Ajouter :

```
0 6 * * * tail -n 100 /var/log/syslog | mail -s "Rapport journalier du serveur" admin@example.com
```

Assurez-vous que `mailutils` est installé :

```bash
sudo apt install mailutils
```

---

## **Étape 5 – Vérification et rendu**

### Vérifiez :

* `systemctl status monitor`
* `journalctl -u monitor`
* Les fichiers :

```bash
ls -lh /var/log/monitoring*
ls -lh /mnt/monitor_backup/
ls -lh /var/log/sync.log
```

### Capturez :

* La structure du dossier `/etc/systemd/system/`
* La sortie de `logrotate -f`
* Le contenu du journal `/var/log/monitoring.log` après redémarrage

