# Commandes MySQL les plus utilisées depuis le terminal :  

---

### 🔗 **Connexion à MySQL**  
**Se connecter à MySQL (en tant que root)** :  
```bash
mysql -u root -p
```
*`-u` : nom d'utilisateur*  
*`-p` : demande le mot de passe*  

**Se connecter à une base de données spécifique** :  
```bash
mysql -u root -p nom_de_la_base
```

**Quitter MySQL** :  
```bash
exit;
```

---

### 🗃️ **Gestion des bases de données**  
**Afficher les bases de données** :  
```sql
SHOW DATABASES;
```

**Créer une base de données** :  
```sql
CREATE DATABASE nom_de_la_base;
```

**Supprimer une base de données** :  
```sql
DROP DATABASE nom_de_la_base;
```

**Utiliser une base de données** :  
```sql
USE nom_de_la_base;
```

---

### 📝 **Gestion des tables**  
**Afficher les tables** :  
```sql
SHOW TABLES;
```

**Créer une table** :  
```sql
CREATE TABLE utilisateurs (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nom VARCHAR(50),
    email VARCHAR(100),
    date_creation TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

**Afficher la structure d'une table** :  
```sql
DESCRIBE utilisateurs;
```
ou  
```sql
SHOW COLUMNS FROM utilisateurs;
```

**Supprimer une table** :  
```sql
DROP TABLE utilisateurs;
```

**Vider une table (sans la supprimer)** :  
```sql
TRUNCATE TABLE utilisateurs;
```

---

### 🛠️ **Manipulation des données**  
**Insérer des données** :  
```sql
INSERT INTO utilisateurs (nom, email) VALUES ('Alice', 'alice@example.com');
```

**Afficher les données** :  
```sql
SELECT * FROM utilisateurs;
```

**Filtrer les résultats** :  
```sql
SELECT * FROM utilisateurs WHERE nom = 'Alice';
```

**Mettre à jour des données** :  
```sql
UPDATE utilisateurs SET email = 'alice@newdomain.com' WHERE nom = 'Alice';
```

**Supprimer des données** :  
```sql
DELETE FROM utilisateurs WHERE nom = 'Alice';
```

---

### 🔍 **Requêtes avancées**  
**Trier les résultats** :  
```sql
SELECT * FROM utilisateurs ORDER BY nom ASC;
```

**Limiter le nombre de résultats** :  
```sql
SELECT * FROM utilisateurs LIMIT 5;
```

**Rechercher une valeur partielle (LIKE)** :  
```sql
SELECT * FROM utilisateurs WHERE email LIKE '%example.com';
```

**Jointure entre deux tables** :  
```sql
SELECT u.nom, c.nom_commande
FROM utilisateurs u
JOIN commandes c ON u.id = c.utilisateur_id;
```

---

### 🧑‍💻 **Gestion des utilisateurs MySQL**  
**Créer un utilisateur** :  
```sql
CREATE USER 'nouvel_utilisateur'@'localhost' IDENTIFIED BY 'mot_de_passe';
```

**Donner des permissions** :  
```sql
GRANT ALL PRIVILEGES ON nom_de_la_base.* TO 'nouvel_utilisateur'@'localhost';
```

**Appliquer les changements de permissions** :  
```sql
FLUSH PRIVILEGES;
```

**Supprimer un utilisateur** :  
```sql
DROP USER 'nouvel_utilisateur'@'localhost';
```

---

### 🧰 **Import et export de bases de données**  
**Exporter une base de données** :  
```bash
mysqldump -u root -p nom_de_la_base > sauvegarde.sql
```

**Importer une base de données** :  
```bash
mysql -u root -p nom_de_la_base < sauvegarde.sql
```

---

### 📝 **Commandes diverses**  
**Vérifier la version de MySQL** :  
```bash
mysql --version
```

**Compter le nombre de lignes dans une table** :  
```sql
SELECT COUNT(*) FROM utilisateurs;
```

**Changer le mot de passe d'un utilisateur** :  
```sql
ALTER USER 'root'@'localhost' IDENTIFIED BY 'nouveau_mot_de_passe';
```

**Afficher les utilisateurs MySQL** :  
```sql
SELECT User, Host FROM mysql.user;
```

---
