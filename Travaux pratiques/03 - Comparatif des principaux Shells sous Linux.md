# Comparatif des principaux Shells sous Linux


# 1. `sh` – Bourne Shell

### Chemin : `/bin/sh`

- Shell historique, développé dans les années 1970.
- Très simple et minimaliste.
- Peu adapté à une utilisation interactive moderne.
- Utilisé principalement pour écrire des scripts très portables sur divers systèmes Unix.

Points forts :
- Compatibilité maximale entre systèmes Unix.
- Léger et stable.

Limitations :
- Peu de fonctionnalités avancées (pas de complétion, pas de coloration syntaxique).

Utilisation typique :
- Scripts système très basiques.
- Exécution dans des environnements minimalistes.



# 2. `bash` – Bourne Again SHell

### Chemin : `/bin/bash`

- Le shell par défaut sur la majorité des distributions Linux pendant de nombreuses années.
- Riche en fonctionnalités interactives et de scripting.
- Supporte les structures de contrôle complexes (`if`, `for`, `while`, etc.).
- Dispose de l'historique des commandes, de la complétion automatique, de l'édition de ligne de commande.

Points forts :
- Facile à apprendre.
- Puissant pour l’automatisation et l’interactivité.
- Très répandu et bien documenté.

Limitations :
- Moins rapide que `dash` pour les scripts très simples.

Utilisation typique :
- Shell par défaut des utilisateurs.
- Écriture de scripts Bash complexes.



# 3. `dash` – Debian Almquist Shell

### Chemin : `/bin/dash`

- Shell minimaliste et très rapide.
- Utilisé comme shell par défaut pour l'exécution des scripts système dans Ubuntu (notamment `/bin/sh` est un lien vers `dash` sur Ubuntu).
- Ne dispose pas des fonctions interactives comme la complétion ou l’historique.

Points forts :
- Très rapide à l'exécution.
- Parfait pour les scripts systèmes simples.

Limitations :
- Mauvais pour une utilisation interactive.
- Moins compatible avec les scripts `bash` utilisant des extensions spécifiques.

Utilisation typique :
- Scripts de démarrage système.
- Environnements embarqués ou légers.



# 4. `zsh` – Z Shell

### Chemin : `/usr/bin/zsh`

- Shell moderne avec de nombreuses fonctionnalités avancées.
- Très personnalisable, utilisé souvent avec "Oh My Zsh".
- Dispose d’une complétion intelligente, d’un correcteur de commande, d’un historique puissant, etc.

Points forts :
- Très riche pour l’interactivité.
- Interface personnalisable.
- Favori des utilisateurs avancés et des développeurs.

Limitations :
- Légèrement plus complexe à prendre en main.
- Moins répandu que Bash par défaut (mais très utilisé dans des environnements modernes comme macOS).

Utilisation typique :
- Utilisation interactive avancée.
- Environnements de développement personnalisés.



# 5. `fish` – Friendly Interactive Shell

### Chemin : `/usr/bin/fish`

- Conçu pour être simple et agréable à utiliser.
- Syntaxe plus lisible que Bash.
- Coloration syntaxique intégrée, aide contextuelle, complétion automatique intuitive.

Points forts :
- Très convivial pour les nouveaux utilisateurs.
- Interface moderne et interactive.

Limitations :
- Incompatible avec la syntaxe Bash (scripts).
- Moins standardisé dans les environnements Linux traditionnels.

Utilisation typique :
- Utilisation interactive par les débutants ou les développeurs recherchant un environnement agréable.
- Pas recommandé pour écrire des scripts système.



# Résumé comparatif

| Shell  | Interactif | Scripting | Vitesse | Facilité | Cas d’usage typique                |
|--------|------------|-----------|---------|----------|------------------------------------|
| `sh`   | Non        | Oui       | Élevée  | Basique  | Scripts système portables          |
| `bash` | Oui        | Oui       | Moyenne | Facile   | Shell par défaut, scripting Bash   |
| `dash` | Non        | Oui       | Très élevée | Basique | Démarrage système, exécution rapide |
| `zsh`  | Oui        | Oui       | Moyenne | Avancée  | Shell personnalisé, développeurs   |
| `fish` | Oui        | Non*      | Moyenne | Très facile | Environnement interactif moderne   |

(*) La syntaxe de `fish` diffère de celle de `bash` : les scripts ne sont pas compatibles.

