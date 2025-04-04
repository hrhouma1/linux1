Voici la **version améliorée** de ton script de simulation de terminal "piraté", avec une **variante sonore** ajoutée grâce à `mpg123`. Cette version :

- Installe `mpg123` automatiquement si nécessaire
- Joue un son dramatique ou perturbant au lancement du terminal (tu peux utiliser un `.mp3` de ton choix)
- Combine texte, effets, couleurs, délais, et son pour **faire vraiment peur**
- Est toujours **100 % inoffensive**

---

## 📁 Script final : `simulate_hacked_terminal_with_sound.sh`

```bash
#!/bin/bash

# ======================================================
# TP Linux : Simulation extrême de piratage terminal
# Auteur : Professeur Linux
# Variante : avec effet sonore (mpg123)
# Objectif : Simulation réaliste et pédagogique
# ======================================================

BASHRC="$HOME/.bashrc"
BACKUP="$HOME/.bashrc.bak_hacked_$(date +%Y%m%d_%H%M%S)"

# Sauvegarde du fichier .bashrc
cp "$BASHRC" "$BACKUP"
echo "Sauvegarde du fichier .bashrc : $BACKUP"

# Vérifie ou installe mpg123
if ! command -v mpg123 &>/dev/null; then
    echo "Installation de mpg123..."
    sudo apt update && sudo apt install -y mpg123
fi

# Chemin par défaut d’un fichier .mp3 effrayant (ou dramatique)
DEFAULT_MP3="$HOME/hack_warning.mp3"

# Téléchargement d’un fichier audio dramatique si non trouvé
if [ ! -f "$DEFAULT_MP3" ]; then
    echo "Téléchargement d’un son dramatique..."
    wget -q -O "$DEFAULT_MP3" https://www.soundjay.com/misc/sounds/bell-ringing-01.mp3
fi

# Ajout de la simulation dans .bashrc
cat << EOF >> "$BASHRC"

# ====== DÉBUT DE LA SIMULATION DE PIRATAGE ======
clear
(sleep 1 && mpg123 -q "$DEFAULT_MP3") &

echo -e "\e[41m\e[30m██████████████  SYSTEM BREACH DETECTED  ██████████████\e[0m"
sleep 1
echo -e "\e[1;31m[CRITICAL] Kernel corrupted - /lib/core.so\e[0m"
sleep 1
echo -e "\e[1;33m[TRACE] Remote shell opened - 45.133.1.67:443\e[0m"
sleep 1
echo -e "\e[1;31m[ALERT] Root access escalated. Sudo override bypassed.\e[0m"
sleep 1
echo -e "\e[1;31m[FATAL] Injecting payload to /etc/shadow and /boot/grub...\e[0m"
sleep 1
echo -e "\e[1;31m[!!!] Running: rm -rf / --no-preserve-root\e[0m"
sleep 1
echo -e "\e[5;31mSYSTEM DESTRUCTION IN PROGRESS...\e[0m"
sleep 1
clear
figlet -f mono12 "System Hacked"
sleep 2
echo ""
echo -e "\e[1;31mFichiers supprimés : /home/*, /var/*, /etc/*\e[0m"
echo -e "\e[1;31mSauvegarde effacée, réseau détourné.\e[0m"
sleep 1
echo ""
echo -e "\e[1;32m... Heureusement, ce n'était qu'une simulation pédagogique.\e[0m"
echo -e "\e[1;36mApprends à protéger ton .bashrc 😉\e[0m"
echo ""

# ====== FIN DE LA SIMULATION ======
EOF

echo "Simulation avec son ajoutée dans ~/.bashrc"
echo "Effet visible au prochain lancement de terminal (ou avec : source ~/.bashrc)"
```

---

## ✅ Instructions d’utilisation :

1. Crée le script :
```bash
nano simulate_hacked_terminal_with_sound.sh
```

2. Colle le script ci-dessus.

3. Rends-le exécutable :
```bash
chmod +x simulate_hacked_terminal_with_sound.sh
```

4. Exécute-le :
```bash
./simulate_hacked_terminal_with_sound.sh
```

5. **Ferme puis rouvre ton terminal**, ou exécute :
```bash
source ~/.bashrc
```

---

## 🔁 Pour désactiver la simulation :

Tu peux restaurer le `.bashrc` propre avec :
```bash
mv ~/.bashrc.bak_hacked_* ~/.bashrc
source ~/.bashrc
```

---

Souhaite-tu que je :
- Intègre ce script dans ton **TP Thinkific HTML Bootstrap** ?
- Prépare un **script menu interactif** pour activer / désactiver la simulation ?
- T’ajoute d’autres variantes encore plus immersives (clignotements, sons multiples, commandes zombies) ?
