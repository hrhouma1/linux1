# <h1 id="wikijs-postgres">Installer Wiki.js sur Ubuntu 22.04 LTS avec PostgreSQL</h1>

# <h2 id="prerequis">Prérequis</h2>

* Ubuntu 22.04 LTS ([télécharger l’ISO](https://releases.ubuntu.com/22.04/ubuntu-22.04-live-server-amd64.iso))
* Accès sudo
* Connexion Internet

---

## <h2 id="git">1. Installation de Git</h2>

```bash
sudo apt update
sudo apt install git -y
git --version
```

---

## <h2 id="nodejs">2. Installation de Node.js (LTS)</h2>

```bash
curl -fsSL https://deb.nodesource.com/setup_lts.x | sudo -E bash -
sudo apt install -y nodejs
node -v && npm -v
```

---

## <h2 id="postgres">3. Installation et configuration de PostgreSQL</h2>

```bash
sudo apt install postgresql postgresql-contrib -y
sudo systemctl start postgresql
sudo systemctl enable postgresql
```

### Création de la base de données et de l’utilisateur :

```bash
sudo -u postgres psql
```

Dans l’invite PostgreSQL :

```sql
CREATE DATABASE wikidb;
CREATE USER wikidb_user WITH PASSWORD 'StrongPassword123!';
GRANT ALL PRIVILEGES ON DATABASE wikidb TO wikidb_user;
\q
```

---

## <h2 id="wikijs-user">4. Création d’un utilisateur système pour Wiki.js</h2>

```bash
sudo adduser --system --group wikijs
sudo mkdir -p /var/www/wikijs
sudo chown -R wikijs:wikijs /var/www/wikijs
sudo usermod -s /bin/bash wikijs
```

---

## <h2 id="install-wikijs">5. Installation de Wiki.js</h2>

```bash
sudo su - wikijs
cd /var/www/wikijs
wget https://github.com/Requarks/wiki/releases/latest/download/wiki-js.tar.gz
tar xzf wiki-js.tar.gz
rm wiki-js.tar.gz
cp config.sample.yml config.yml
exit
```

---

## <h2 id="config-postgres">6. Configuration de Wiki.js pour PostgreSQL</h2>

Éditez `/var/www/wikijs/config.yml` :

```bash
sudo nano /var/www/wikijs/config.yml
```

### Remplacez la section `db:` par :

```yaml
db:
  type: postgres
  host: localhost
  port: 5432
  user: wikidb_user
  pass: StrongPassword123!
  db: wikidb
  ssl: false
```

Assurez-vous que les sections suivantes soient **commentées ou supprimées** si inutilisées :

```yaml
#  schema: public      # COMMENTEZ si non utilisé
#  storage: path/to/database.sqlite  # COMMENTEZ si non utilisé
#  sslOptions:         # COMMENTEZ si inutilisé
#    auto: true
#    rejectUnauthorized: false
```

Aussi, pour que Wiki.js soit accessible depuis l’extérieur :

```yaml
bindIP: 0.0.0.0
port: 3000
```

---

## <h2 id="systemd">7. Création du service systemd</h2>

### Ajoutez aux sudoers et définissez un mot de passe pour wikijs :

```bash
sudo passwd wikijs
sudo usermod -aG sudo wikijs
```

### Créez le service :

```bash
sudo nano /etc/systemd/system/wikijs.service
```

Contenu :

```ini
[Unit]
Description=Wiki.js
After=network.target

[Service]
Type=simple
ExecStart=/usr/bin/node server
Restart=always
User=wikijs
Environment=NODE_ENV=production
WorkingDirectory=/var/www/wikijs

[Install]
WantedBy=multi-user.target
```

---

## <h2 id="demarrage">8. Démarrage et vérification</h2>

```bash
sudo systemctl daemon-reload
sudo systemctl start wikijs
sudo systemctl enable wikijs
systemctl status wikijs
```

---

## <h2 id="troubleshooting">9. Dépannage courant</h2>

Si vous voyez des erreurs de type :

* `bad indentation of a mapping entry`
* `Unable to read configuration file!`

Revérifiez que vous avez bien commenté ou supprimé les blocs suivants dans `config.yml` :

```yaml
#  sslOptions:
#    auto: true

#  schema: public

#  storage: path/to/database.sqlite
```

Puis redémarrez :

```bash
sudo systemctl restart wikijs
```

---

## <h2 id="acces">10. Accès à l’interface Wiki.js</h2>

Dans votre navigateur :
`http://<IP_de_votre_serveur>:3000`

---

## <h2 id="https">11. Passage en HTTPS (optionnel)</h2>

```bash
sudo apt update
sudo apt install certbot python3-certbot-nginx -y
sudo ufw allow 'Nginx Full'
sudo certbot --nginx
sudo certbot renew --dry-run
```



# <h1 id="fin">Félicitations !</h1>

Wiki.js est maintenant opérationnel avec PostgreSQL sur Ubuntu 22.04.


<br/>
<br/>

# Annexe 1  - Explorer PostgreSQL

## <h1 id="connexion">1. Connexion à PostgreSQL</h1>

```bash
sudo -u postgres psql
```

Ou, si tu veux te connecter directement à une base spécifique avec un utilisateur :

```bash
psql -U wikidb_user -d wikidb -h localhost
```



## <h1 id="navigation">2. Commandes de navigation dans `psql`</h1>

| Commande       | Description                       |
| -------------- | --------------------------------- |
| `\l`           | Liste toutes les bases de données |
| `\c nom_base`  | Se connecter à une base           |
| `\dt`          | Liste les tables                  |
| `\d nom_table` | Affiche la structure d'une table  |
| `\du`          | Liste les utilisateurs            |
| `\dn`          | Liste les schémas                 |
| `\df`          | Liste les fonctions               |
| `\x`           | Active le mode d'affichage étendu |
| `\q`           | Quitter `psql`                    |



## <h1 id="requetes">3. Exemples de requêtes utiles</h1>

### Tables, colonnes et index :

```sql
-- Lister toutes les tables d’un schéma
SELECT tablename FROM pg_tables WHERE schemaname = 'public';

-- Voir les colonnes d'une table
SELECT column_name, data_type FROM information_schema.columns WHERE table_name = 'users';

-- Voir les index
SELECT * FROM pg_indexes WHERE tablename = 'users';
```



### Espaces et tailles :

```sql
-- Taille d'une base
SELECT pg_size_pretty(pg_database_size('wikidb'));

-- Taille d’une table
SELECT pg_size_pretty(pg_total_relation_size('users'));
```



### Activité et sessions :

```sql
-- Voir les connexions actives
SELECT * FROM pg_stat_activity;

-- Voir les verrous en cours
SELECT * FROM pg_locks;
```



### Schéma courant :

```sql
-- Voir le schéma par défaut de l’utilisateur connecté
SHOW search_path;
```



## <h1 id="export">4. Export et dump</h1>

```bash
# Dump complet de la base
pg_dump -U wikidb_user -d wikidb -F c -f wikidb.dump

# Export en SQL lisible
pg_dump -U wikidb_user -d wikidb -F p -f wikidb.sql
```



## <h1 id="nettoyage">5. Nettoyage et vérification</h1>

```sql
-- Trouver les tables vides
SELECT schemaname, tablename
FROM pg_tables
WHERE schemaname = 'public'
AND NOT EXISTS (SELECT 1 FROM information_schema.columns WHERE table_name = tablename);

-- Vérifier les contraintes
SELECT * FROM information_schema.table_constraints WHERE table_schema = 'public';
```



<br/>
<br/>



# Annexe 2 - Optionnel - Explorer  les bases de PostgreSQL




# <h1 id="exercice-postgresql"> Exercice : Maîtriser les bases de PostgreSQL (via terminal)</h1>



## <h2 id="objectif"> Objectif</h2>

Créer une base de données complète nommée `coursdb`, avec un utilisateur dédié, deux tables (`etudiants`, `programmes`), relations, insertions, requêtes SQL, et gestion de droits.



## <h2 id="etape1">Étape 1 – Créer la base et l’utilisateur</h2>

### En tant que superutilisateur PostgreSQL :

```bash
sudo -u postgres psql
```

### Dans `psql` :

```sql
-- Créer une base de données
CREATE DATABASE coursdb;

-- Créer un utilisateur
CREATE USER cours_user WITH PASSWORD 'cours123';

-- Donner tous les droits sur la base
GRANT ALL PRIVILEGES ON DATABASE coursdb TO cours_user;

-- Quitter
\q
```



## <h2 id="etape2"> Étape 2 – Se connecter à la base</h2>

```bash
psql -U cours_user -d coursdb -h localhost
```


## <h2 id="etape3"> Étape 3 – Créer les tables</h2>

```sql
-- Créer la table programmes
CREATE TABLE programmes (
    id SERIAL PRIMARY KEY,
    nom TEXT NOT NULL,
    niveau TEXT NOT NULL
);

-- Créer la table etudiants avec relation vers programmes
CREATE TABLE etudiants (
    id SERIAL PRIMARY KEY,
    nom TEXT NOT NULL,
    age INTEGER CHECK (age > 0),
    programme_id INTEGER REFERENCES programmes(id) ON DELETE CASCADE
);
```



## <h2 id="etape4"> Étape 4 – Insérer des données</h2>

```sql
-- Insérer des programmes
INSERT INTO programmes (nom, niveau) VALUES
('Informatique', 'DEC'),
('Administration', 'AEC');

-- Insérer des étudiants
INSERT INTO etudiants (nom, age, programme_id) VALUES
('Alice', 21, 1),
('Bob', 24, 1),
('Caroline', 20, 2);
```



## <h2 id="etape5"> Étape 5 – Faire des requêtes</h2>

```sql
-- Tous les étudiants
SELECT * FROM etudiants;

-- Nom des étudiants avec leur programme
SELECT e.nom AS etudiant, p.nom AS programme
FROM etudiants e
JOIN programmes p ON e.programme_id = p.id;

-- Compter les étudiants par programme
SELECT p.nom, COUNT(e.id) AS total
FROM programmes p
LEFT JOIN etudiants e ON p.id = e.programme_id
GROUP BY p.nom;
```



## <h2 id="etape6">🛠 Étape 6 – Modifier ou supprimer</h2>

```sql
-- Modifier un étudiant
UPDATE etudiants SET age = 22 WHERE nom = 'Alice';

-- Supprimer un étudiant
DELETE FROM etudiants WHERE nom = 'Bob';

-- Supprimer un programme (provoque suppression en cascade)
DELETE FROM programmes WHERE nom = 'Administration';
```



## <h2 id="etape7"> Étape 7 – Gestion des utilisateurs</h2>

```sql
-- Connecté en tant que postgres
sudo -u postgres psql

-- Créer un lecteur seul
CREATE USER lecteur WITH PASSWORD 'readonly123';

-- Connexion à la base
\c coursdb

-- Donner accès en lecture seulement
GRANT CONNECT ON DATABASE coursdb TO lecteur;
GRANT USAGE ON SCHEMA public TO lecteur;
GRANT SELECT ON ALL TABLES IN SCHEMA public TO lecteur;

-- Appliquer automatiquement aux futures tables
ALTER DEFAULT PRIVILEGES IN SCHEMA public
GRANT SELECT ON TABLES TO lecteur;

\q
```



## <h2 id="bonus"> Bonus – Vérification</h2>

```sql
-- Voir les utilisateurs
\du

-- Lister les tables et voir leurs droits
\dt+
```


