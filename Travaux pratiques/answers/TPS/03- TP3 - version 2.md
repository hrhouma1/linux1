
Parfait. Voici **le script ultime de simulation de système compromis**, conçu pour **faire vraiment peur**, **impressionner fortement les étudiants**, tout en **étant 100% sans danger**.

---

## 💡 Objectif
Créer un effet **visuel extrême**, très réaliste, qui donne l’illusion que :
- Le système est en train de s’autodétruire
- Des données sont volées
- Des fichiers sont corrompus
- Un accès distant est établi

**⚠️ Sans rien modifier pour de vrai.** C’est purement **visuel et temporaire**, intégré dans `.bashrc` comme une **expérience pédagogique sur la sécurité et les fichiers d’environnement**.

---

## 📁 Fichier : `simulate_hacked_terminal.sh`

```bash
#!/bin/bash

# =============================================
# TP Linux : Simulation avancée de piratage terminal
# Nom : simulate_hacked_terminal.sh
# Auteur : Professeur Linux
# Objectif : Créer un effet terrifiant mais inoffensif pour sensibiliser à la sécurité
# =============================================

BASHRC="$HOME/.bashrc"
BACKUP="$HOME/.bashrc.bak_hacked_$(date +%Y%m%d_%H%M%S)"

# Sauvegarde de sécurité
cp "$BASHRC" "$BACKUP"
echo "Sauvegarde de ~/.bashrc enregistrée : $BACKUP"

# Ajout de la simulation de piratage au démarrage du terminal
cat << 'EOF' >> "$BASHRC"

# ====== DÉBUT DE LA SIMULATION ======
clear
sleep 0.5

echo -e "\e[41m\e[30m███████  SYSTEM ALERT  ███████\e[0m"
sleep 0.5
echo -e "\e[1;31m[CRITICAL ERROR] Kernel panic in module /usr/lib/syscore\e[0m"
sleep 1
echo -e "\e[1;33m[WARNING] Connection to 185.23.54.10 established (root@yourhost)...\e[0m"
sleep 1
echo -e "\e[1;31m[ERROR] Unauthorized access detected. SUDO privileges escalated.\e[0m"
sleep 1
echo -e "\e[1;37m> Initializing payload... injecting into /etc/shadow\e[0m"
sleep 0.7
echo -e "\e[1;31m> Overwriting critical boot sector...\e[0m"
sleep 1
echo -e "\e[1;31m> Encrypting home directory...\e[0m"
sleep 1
echo -e "\e[1;31m> Sending data to external host...\e[0m"
sleep 1
echo -e "\e[1;31m> Running rm -rf / --no-preserve-root\e[0m"
sleep 1
echo -e "\e[1;30m█████████████████████████████████████████████████████\e[0m"
sleep 1
echo -e "\e[5;31mSYSTEM FAILURE\e[0m"
sleep 2
clear
figlet -f mono12 "You got hacked."
sleep 2
echo ""
echo -e "\e[1;37mRelax. This is just a simulation.\e[0m"
echo -e "\e[1;32mTP terminé. Ton .bashrc vient de t’enseigner une bonne leçon de sécurité.\e[0m"
echo ""

# ====== FIN DE LA SIMULATION ======
EOF

echo "Simulation visuelle ajoutée dans ~/.bashrc"
echo "Elle s’exécutera à chaque ouverture de terminal (jusqu'à suppression)"
```

---

## ✅ Utilisation :

1. Crée le fichier :
```bash
nano simulate_hacked_terminal.sh
```

2. Colle le script ci-dessus.

3. Rends-le exécutable :
```bash
chmod +x simulate_hacked_terminal.sh
```

4. Lance-le :
```bash
./simulate_hacked_terminal.sh
```

5. **Ferme puis rouvre le terminal**, ou exécute :
```bash
source ~/.bashrc
```

---

## 🔧 Désactivation (restauration) :

Si un(e) étudiant(e) panique, voici comment désactiver :

```bash
mv ~/.bashrc.bak_hacked_* ~/.bashrc
source ~/.bashrc
```

---

## 🔬 Intérêt pédagogique :

- Montrer que **tout ce qui s’exécute au démarrage du terminal peut être modifié**
- Sensibiliser à la **protection des fichiers d’environnement**
- Démontrer un **"social engineering visuel"**
- Amener une discussion sur les **scripts malveillants**

---

Souhaite-tu aussi que je :
- Intègre ça comme **Étape Bonus du TP Thinkific** avec capture d’écran ?
- Ajoute une **fonction pour l’activer ou désactiver dynamiquement depuis un menu ?**
- Te crée une **version HTML Bootstrap documentée** de ce TP ?

Je peux aussi ajouter une **variante avec son (si mpg123 est installé)** pour amplifier l’effet.
