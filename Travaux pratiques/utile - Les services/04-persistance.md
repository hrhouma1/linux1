**la persistance** est ce qui différencie vraiment un **service** bien géré d’un simple **démon lancé à la main** ?



| Comportement après redémarrage du système | Démon lancé manuellement (ex: `mon_script.py &`) | Service `systemd` (`mon_script.service`)         |
| ----------------------------------------- | ------------------------------------------------ | ------------------------------------------------ |
| Est relancé automatiquement au boot       | ❌ Non                                            | ✅ Oui (si `enabled` avec `systemctl enable`)     |
| Supervision (restarts automatiques)       | ❌ Non                                            | ✅ Oui (avec `Restart=always` dans le `.service`) |
| Logging centralisé (`journalctl`)         | ❌ Non                                            | ✅ Oui                                            |
| Peut être arrêté proprement               | ❌ Souvent non (nécessite `kill`)                 | ✅ Oui (`systemctl stop mon_script`)              |
| Gestion d'état (actif/inactif/échec)      | ❌ Non                                            | ✅ Oui (`systemctl status`)                       |

---

### Résumé :

* Un **démon lancé avec `&`** est **temporaire** : il disparaît au redémarrage, et il n’est pas supervisé.
* Un **service `systemd`** est **persistant** : il peut démarrer au boot, redémarrer automatiquement en cas d’échec, être contrôlé proprement, et il laisse des traces dans les journaux système.

