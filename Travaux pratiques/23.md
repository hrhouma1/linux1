

# **But de l’attaque**

> **Créer une backdoor persistante** sur un système Linux.
> À chaque démarrage, un **reverse shell Bash** est automatiquement lancé vers l’attaquant, sans interaction de la victime.

---

# **OPTION 1 – Avec une seule machine (Ubuntu 22.04)**

## **Résumé**

* Le service malveillant se connecte à **127.0.0.1:4444** (localhost)
* L’attaquant **écoute le port 4444** localement avec `nc`
* Un shell Bash s’ouvre localement dans le terminal de l’attaquant

## **Schéma ASCII**

```
+---------------------------+
|  Ubuntu 22.04 Unique      |
|--------------------------|
| Terminal 1 :             |
| nc -lvnp 4444            |
|                          |
| Terminal 2 :             |
| systemd lance            |
| bash -> 127.0.0.1:4444   |
+--------------------------+
```

## **Comportement**

* Le shell Bash se connecte **à lui-même**
* Cela prouve que le service `systemd` est actif et fonctionne
* C’est **local et sans risque réseau**, idéal pour un TP débutant

---

# **OPTION 2 – Avec deux machines**

## **Configuration**

| Rôle      | OS           | IP            |
| --------- | ------------ | ------------- |
| Attaquant | Kali Linux   | 192.168.56.10 |
| Victime   | Ubuntu 22.04 | 192.168.56.11 |

### **Réseau** : `Host-Only` ou `Internal Network`

## **Schéma ASCII**

```
+---------------------------+          +---------------------------+
|  Machine Ubuntu 22.04     |          |  Machine Kali Linux       |
|--------------------------|          |--------------------------|
| reverse.sh :             |          | nc -lvnp 4444             |
| bash -> 192.168.56.10:4444| -------> | écoute le shell Bash      |
|                          |          | reçoit accès à la victime |
+--------------------------+          +--------------------------+
```

## **Comportement**

* Le service `systemd` lance le reverse shell vers Kali
* Kali reçoit un accès Bash à distance
* Le shell revient **automatiquement à chaque redémarrage**

---

# **Détails de l’attaque**

1. L’attaquant **place un script Bash** dans `/opt/.dossier/`
2. Il crée un **fichier `.service`** dans `/etc/systemd/system/`
3. Il active le service avec :

   ```bash
   systemctl enable nom_du_service
   systemctl start nom_du_service
   ```
4. Le script Bash **établit une connexion TCP sortante** vers le port 4444
5. L’attaquant écoute (`nc -lvnp 4444`) et **prend le contrôle** via Bash
6. Le service redémarre **à chaque boot**, même si la victime ne fait rien

---

# **Ce que l’attaquant peut faire une fois connecté**

Dans le shell reçu :

* `whoami` → l’utilisateur courant
* `uname -a` → voir le système
* `cat /etc/passwd` → lire les comptes
* `cat ~/.bash_history` → voir les commandes utilisées
* `apt install` → installer d'autres outils
* `scp`, `curl`, `wget` → exfiltrer des fichiers
* Créer d'autres backdoors, modifier `sudoers`, etc.

---

# **Conclusion**

> L’attaque consiste à **enregistrer un script malveillant comme un service `systemd` légitime**, pour qu’il soit lancé **à chaque démarrage**, et fournisse un **shell distant ou local** à l’attaquant.

C’est une forme **de persistance avancée**, car elle utilise :

* un mécanisme système natif (`systemd`)
* peu de fichiers visibles
* un comportement automatique
* aucune interface graphique
