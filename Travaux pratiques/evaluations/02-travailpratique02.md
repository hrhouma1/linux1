# TP 2 – Gestion de plusieurs sites web avec un reverse proxy (proxy inverse) dans un environnement Linux

<br/>


<h1 id="tb"> Table des matières </h1>

* [Contexte](#contexte)
* [Objectifs de l’épreuve](#objectifs)
* [Consignes générales](#consignes)
* [Partie 1 – Installation et configuration de Wiki.js](#partie1)
* [Partie 2 – Configuration du reverse proxy NGINX](#partie2)
* [Partie 3 – Validation, dépannage et analyse](#partie3)
* [Partie 4 – Question d’analyse architecturale](#partie4)
* [Grille d’évaluation](#grille)



<h1 id="contexte">CONTEXTE</h1>

Vous êtes administrateur système dans une entreprise de services numériques. Votre mandat consiste à configurer un environnement local hébergeant **trois instances indépendantes de Wiki.js**, chacune représentant un environnement classique de développement logiciel :

* Environnement de développement (`dev.site1.local`)
* Environnement de test (`test.site1.local`)
* Environnement de production (`prod.site1.local`)

Chaque instance de Wiki.js devra être installée **manuellement sur une seule machine Ubuntu Server 22.04 LTS**, connectée à sa propre base de données MariaDB et exposée via un **reverse proxy NGINX**.
[Retour à la table des matières](#tb)

<br/>


<h1 id="objectifs">OBJECTIFS DE L’ÉPREUVE</h2>

* Maîtriser l’installation manuelle d’un moteur de wiki basé sur Node.js.
* Gérer correctement des bases de données SQL distinctes.
* Automatiser le démarrage des services via `systemd`.
* Configurer un reverse proxy HTTP avec des sous-domaines internes.
* Diagnostiquer des erreurs de déploiement fréquentes.
* Appliquer les meilleures pratiques en matière d’isolation de service.


[Retour à la table des matières](#tb)

<br/>


<h1 id="consignes">CONSIGNES GÉNÉRALES</h1>

* Vous travaillez **sur une seule machine virtuelle Ubuntu 22.04**.
* Toutes les instances doivent être **indépendantes**, sans ports en conflit.
* Aucun serveur ne doit être exposé directement : **seul le proxy NGINX écoute sur le port 80**.
* Les adresses `*.site1.local` doivent fonctionner via `/etc/hosts`.
* Vous devez suivre une démarche professionnelle, avec structure, clarté et rigueur.

[Retour à la table des matières](#tb)

<br/>


<h1 id="partie1">PARTIE 1 – INSTALLATION ET CONFIGURATION DE WIKI.JS (40 POINTS)</h1>

1. Créez trois utilisateurs Linux dédiés : `wikijs_dev`, `wikijs_test`, `wikijs_prod`.
2. Créez trois bases de données distinctes sous MariaDB :

   * `wikidb_dev`, `wikidb_test`, `wikidb_prod`
3. Installez manuellement Wiki.js dans :

   * `/var/www/dev.site1`
   * `/var/www/test.site1`
   * `/var/www/prod.site1`
4. Modifiez le fichier `config.yml` de chaque instance :

   * Port différent (`3000`, `3001`, `3002`)
   * `bindIP: 127.0.0.1`
   * Informations de connexion correctes à la base
5. Créez et activez les services systemd :

   * `wikijs-dev.service`, `wikijs-test.service`, `wikijs-prod.service`

[Retour à la table des matières](#tb)

<br/>


<h1 id="partie2">PARTIE 2 – CONFIGURATION DU REVERSE PROXY NGINX (30 POINTS)</h1>

6. Configurez NGINX pour servir les sous-domaines suivants :

| Nom d’hôte       | Port interne | Chemin local        |
| ---------------- | ------------ | ------------------- |
| dev.site1.local  | 3000         | /var/www/dev.site1  |
| test.site1.local | 3001         | /var/www/test.site1 |
| prod.site1.local | 3002         | /var/www/prod.site1 |

7. Modifiez `/etc/hosts` pour faire pointer ces domaines vers `127.0.0.1`.
8. Testez l’accès aux sites avec un navigateur ou `curl`.

[Retour à la table des matières](#tb)

<br/>



<h1 id="partie3">PARTIE 3 – VALIDATION, DÉPANNAGE ET ANALYSE (30 POINTS)</h1>

9. Corrigez les erreurs suivantes si elles apparaissent :

   * Mauvaise indentation YAML
   * Problèmes de connexion MariaDB
   * Port déjà occupé
   * Droits de fichiers insuffisants

10. Justifiez les décisions suivantes :

* Pourquoi utiliser des utilisateurs différents pour chaque Wiki.js ?
* Pourquoi préférer un reverse proxy plutôt que l’exposition directe ?
* Quels sont les risques de sécurité si les ports sont exposés sans NGINX ?

11. Proposez une méthode pour sécuriser l’installation avec HTTPS :

* Utilisation de `certbot`
* Configuration du `bindIP`
* Rôle du DNS public

[Retour à la table des matières](#tb)

<br/>

<h1 id="partie4">PARTIE 4 – QUESTION D’ANALYSE ARCHITECTURALE (10 POINTS)</h1>

#### Énoncé

Deux approches sont possibles pour exposer plusieurs environnements :

**Approche A** :
`sous-domaines`
→ `http://dev.site1.local`, `http://test.site1.local`, `http://prod.site1.local`

**Approche B** :
`chemins internes`
→ `http://site1.local/dev`, `http://site1.local/test`, `http://site1.local/prod`

#### Question

1. Comparez les deux approches :

   a) Implications techniques principales (4 points)
   b) Cas d’usage où l’approche B est tolérable, et quand elle est déconseillée (3 points)
   c) Quelle approche recommandez-vous et pourquoi ? (3 points)

[Retour à la table des matières](#tb)

<br/>

<h1 id="grille">GRILLE D’ÉVALUATION</h1>

| Critères                             | Points |
| ------------------------------------ | ------ |
| Wiki.js (x3) installé et fonctionnel | 15     |
| Bases de données distinctes          | 10     |
| Configuration `config.yml` correcte  | 10     |
| Services `systemd` fonctionnels      | 5      |
| NGINX reverse proxy configuré        | 15     |
| `/etc/hosts` bien édité              | 5      |
| Diagnostic et résolution des erreurs | 10     |
| Justification technique              | 10     |
| Proposition HTTPS argumentée         | 10     |
| **Total**                            | **90** |

| Analyse de l’architecture            | Points |
| ------------------------------------ | ------ |
| Présentation des différences         | 2      |
| Cas d’usage pour chaque approche     | 2      |
| Analyse des risques et compatibilité | 2      |
| Recommandation claire et motivée     | 4      |
| **Sous-total**                       | **10** |

