# Symbole &

Le symbole `&` dans un terminal Linux (comme sur Ubuntu 22.04) est utilisé pour **exécuter une commande en arrière-plan**.

### Explication :

Quand tu exécutes une commande comme :

```bash
/usr/local/bin/mon_daemon.py &
```

Tu demandes à **lancer le script** mais **sans bloquer le terminal**. Cela signifie que :

* Le script tourne en tâche de fond.
* Tu peux continuer à utiliser le terminal pour d'autres commandes.

---

### Exemple :

```bash
sleep 60 &
```

Cette commande attend 60 secondes, mais elle est exécutée en arrière-plan. Tu verras un retour comme :

```
[1] 12345
```

* `[1]` est le **numéro du job** (interne au terminal)
* `12345` est le **PID** (Process ID) du processus lancé

---

### Sans `&` :

Si tu fais simplement :

```bash
/usr/local/bin/mon_daemon.py
```

Le terminal sera **bloqué** tant que le script tourne, car il exécute une boucle infinie (`while True`). Tu devrais utiliser `Ctrl+C` pour l’interrompre manuellement.

---

### À retenir :

| Commande     | Comportement                                       |
| ------------ | -------------------------------------------------- |
| `commande`   | S'exécute au **premier plan** (bloque le terminal) |
| `commande &` | S'exécute en **arrière-plan** (libère le terminal) |

