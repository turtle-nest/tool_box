**SQLite : Présentation et Tutoriel Complet**  

---

### **1. Présentation de SQLite**  
**Qu’est-ce que SQLite ?**  
SQLite est un moteur de base de données relationnelle léger, rapide et autonome. Contrairement aux bases de données comme MySQL ou PostgreSQL, SQLite ne fonctionne pas en tant que service : il stocke les données dans un simple fichier. Cela le rend particulièrement adapté aux applications embarquées, aux tests et aux petits projets.  

**Caractéristiques principales :**  
- **Base de données sans serveur** : pas besoin de service en arrière-plan.  
- **Portable** : les bases sont des fichiers autonomes, faciles à transférer.  
- **Zéro configuration** : pas besoin d’installation complexe.  
- **Conformité ACID** : transactions sûres et fiables.  
- **Langage SQL standard** : supporte la plupart des commandes SQL classiques.  

**Cas d’usage :**  
- Applications mobiles (Android/iOS)  
- Développement embarqué (IoT)  
- Stockage local pour des outils ou scripts Python  
- Tests rapides de bases de données relationnelles  

---

### **2. Installation de SQLite**  
**Sur macOS/Linux :**  
SQLite est souvent préinstallé. Pour vérifier :  
```bash
sqlite3 --version
```
Sinon, pour l’installer :  
```bash
# Sur macOS avec Homebrew
brew install sqlite

# Sur Ubuntu/Debian
sudo apt update && sudo apt install sqlite3
```

**Sur Windows :**  
1. Télécharge SQLite depuis [sqlite.org/download.html](https://sqlite.org/download.html).  
2. Ajoute le dossier contenant `sqlite3.exe` à la variable d’environnement `PATH`.  

---

### **3. Premiers pas avec SQLite**  
**Créer une base de données :**  
```bash
sqlite3 mydatabase.db
```

**Créer une table :**  
```sql
CREATE TABLE users (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    username TEXT NOT NULL,
    email TEXT UNIQUE NOT NULL,
    age INTEGER
);
```

**Insérer des données :**  
```sql
INSERT INTO users (username, email, age) VALUES
('alice', 'alice@example.com', 25),
('bob', 'bob@example.com', 30);
```

**Afficher les données :**  
```sql
SELECT * FROM users;
```

---

### **4. Commandes SQLite essentielles**  
**Structure de la table :**  
```sql
PRAGMA table_info(users);
```

**Modifier une table :**  
Ajouter une colonne :  
```sql
ALTER TABLE users ADD COLUMN phone TEXT;
```

**Mettre à jour des données :**  
```sql
UPDATE users SET age = 26 WHERE username = 'alice';
```

**Supprimer des données :**  
```sql
DELETE FROM users WHERE username = 'bob';
```

**Supprimer une table :**  
```sql
DROP TABLE users;
```

---

### **5. Requêtes SQL avancées**  
**Filtrer des résultats :**  
```sql
SELECT * FROM users WHERE age > 25;
```

**Trier les résultats :**  
```sql
SELECT * FROM users ORDER BY age DESC;
```

**Jointures entre tables :**  
```sql
CREATE TABLE orders (
    id INTEGER PRIMARY KEY,
    user_id INTEGER,
    product TEXT,
    FOREIGN KEY(user_id) REFERENCES users(id)
);

SELECT users.username, orders.product
FROM users
JOIN orders ON users.id = orders.user_id;
```

**Regroupement et agrégats :**  
```sql
SELECT age, COUNT(*) AS total FROM users GROUP BY age;
```

---

### **6. Transactions et sécurité**  
**Démarrer une transaction :**  
```sql
BEGIN TRANSACTION;
```

**Valider les changements :**  
```sql
COMMIT;
```

**Annuler les changements :**  
```sql
ROLLBACK;
```

---

### **7. Utiliser SQLite avec Python**  
**Connexion à la base de données :**  
```python
import sqlite3

conn = sqlite3.connect('mydatabase.db')
cur = conn.cursor()
```

**Créer une table :**  
```python
cur.execute('''CREATE TABLE IF NOT EXISTS users (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    username TEXT NOT NULL,
    email TEXT UNIQUE NOT NULL
)''')
conn.commit()
```

**Insérer des données :**  
```python
cur.execute("INSERT INTO users (username, email) VALUES (?, ?)", ("charlie", "charlie@example.com"))
conn.commit()
```

**Récupérer des données :**  
```python
cur.execute("SELECT * FROM users")
rows = cur.fetchall()
for row in rows:
    print(row)
```

**Fermer la connexion :**  
```python
conn.close()
```

---

### **8. Bonnes pratiques**  
- **Utilisez des index** sur les colonnes souvent utilisées dans les `WHERE`.  
- **Validez les entrées utilisateurs** pour éviter les injections SQL.  
- **Effectuez des sauvegardes régulières** du fichier `.db`.  
- **Utilisez des transactions** pour garantir l'intégrité des données.  

---

**9. Ressources supplémentaires**  
- [Documentation officielle SQLite](https://sqlite.org/docs.html)  
- [Outil en ligne SQLite Browser](https://sqlitebrowser.org/)  
- [Tutoriel SQLite avec Python](https://docs.python.org/3/library/sqlite3.html)  

---

Tu veux qu’on construise une base de données ensemble ou approfondir un aspect particulier ? 🚀  