
*"Personnalise ton Terminal comme un Hacker !"* est un **script pédagogique Linux qui donne l’impression que le système est compromis** ("system hacked"), **sans danger réel**


# 1 -  Objectif pédagogique :

Faire croire (visuellement) à une compromission du terminal pour :
- Initier à la modification du `.bashrc`
- Comprendre les effets des scripts de démarrage
- Sensibiliser à la sécurité et aux comportements à éviter



# 2 - Nom du script : `prank_hacked_terminal.sh`

```bash
#!/bin/bash

# =============================================
# Script pédagogique de simulation de piratage
# Nom : prank_hacked_terminal.sh
# Auteur : Professeur Linux
# But : Simuler un terminal compromis pour apprentissage .bashrc
# Aucune action dangereuse réelle
# =============================================

# Chemin du fichier bashrc
BASHRC="$HOME/.bashrc"

# Sauvegarde automatique du .bashrc
BACKUP="$HOME/.bashrc.bak_hacked_$(date +%Y%m%d_%H%M%S)"
cp "$BASHRC" "$BACKUP"
echo "Fichier .bashrc sauvegardé : $BACKUP"

# Ajout du faux message de piratage
cat << 'EOF' >> "$BASHRC"

# === Simulation d'un système compromis ===
clear
echo -e "\e[41m\e[1;37mALERTE : SYSTÈME COMPROMIS !\e[0m"
sleep 1
echo -e "\e[31mTentative de restauration du système...\e[0m"
sleep 1
echo -e "\e[33m[!] Fichiers système non détectés : /etc/passwd...\e[0m"
sleep 1
echo -e "\e[31m[!] Connexion root interceptée...\e[0m"
sleep 1
echo -e "\e[35mVotre machine est sous contrôle.\e[0m"
sleep 2
figlet -f mono12 "System Hacked"
sleep 1
echo ""
echo -e "\e[1;31mDonnées en cours de transmission...\e[0m"
sleep 2
echo -e "\e[1;37m...juste une blague pédagogique. :) \e[0m"
echo ""

EOF

echo "Code de simulation ajouté à ~/.bashrc"
echo "L'effet sera visible au prochain lancement de terminal ou connexion."

```



##  Rappel de sécurité :
- Ce script **ne modifie rien de dangereux**.
- Il ajoute uniquement un **effet visuel simulé** lors de l’ouverture du terminal.
- Il est **réversible** grâce à la sauvegarde automatique (`.bashrc.bak_hacked_YYYYMMDD_HHMMSS`).



##  Instructions :

1. Crée le script :
```bash
nano prank_hacked_terminal.sh
```

2. Colle le contenu ci-dessus.
3. Rends-le exécutable :
```bash
chmod +x prank_hacked_terminal.sh
```

4. Exécute-le :
```bash
./prank_hacked_terminal.sh
```

5. Ouvre un **nouveau terminal** pour voir l’effet.



# 4 - Pour annuler la blague :

Reviens à ton `.bashrc` original :

```bash
mv ~/.bashrc.bak_hacked_* ~/.bashrc
source ~/.bashrc
```
