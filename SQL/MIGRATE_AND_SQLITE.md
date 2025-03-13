### 🚀 **Fiche Résumé : Manipulations Flask-Migrate & SQLite**  

Cette fiche résume les commandes essentielles pour **gérer la base de données** avec **Flask-Migrate** et **SQLite**.  

---

## **1️⃣ Initialiser et gérer Flask-Migrate**
Flask-Migrate est une extension qui permet de gérer les migrations de bases de données avec SQLAlchemy.

### **🔹 Initialisation (à faire une seule fois)**
⚠️ **À exécuter dans le dossier racine du projet**  
```bash
pip install flask-migrate  # Installe Flask-Migrate si ce n'est pas déjà fait
export FLASK_APP=app       # Définit l'application Flask (remplacer par le bon nom si nécessaire)
flask db init              # Crée le dossier 'migrations/'
```
- 📌 `flask db init` est à exécuter **une seule fois** au début du projet.

---

### **🔹 Créer et appliquer une migration**
Une migration est nécessaire après toute modification du modèle SQLAlchemy.

```bash
flask db migrate -m "Description des changements"
flask db upgrade
```
- `flask db migrate -m "Ajout des colonnes"` : Crée une migration en fonction des changements des modèles.
- `flask db upgrade` : Applique la migration et met à jour la base.

---

### **🔹 Annuler une migration**
```bash
flask db downgrade
```
- ⏪ Revient à l’état précédent de la base de données.

---

### **🔹 Recréer la base de données depuis zéro (si nécessaire)**
⚠️ **Attention : Cette commande supprime toutes les données !**
```bash
rm mydb.db  # Supprime la base SQLite
flask db upgrade  # Recrée la base avec la structure correcte
```

---

## **2️⃣ Manipulations SQLite**
SQLite est un moteur de base de données léger intégré à Python.

### **🔹 Accéder à la base de données**
```bash
sqlite3 mydb.db
```
Une fois dans la console SQLite, vous pouvez exécuter des commandes SQL.

---

### **🔹 Vérifier les tables**
```sql
.tables
```
- 📌 Affiche toutes les tables existantes.

---

### **🔹 Vérifier la structure d'une table**
```sql
PRAGMA table_info(users);
```
- 📌 Affiche les colonnes de la table `users`.

**Exemple de sortie :**
```
0|id|INTEGER|1||1
1|first_name|VARCHAR(50)|1||0
2|last_name|VARCHAR(50)|1||0
3|email|VARCHAR(120)|1||0
4|password|VARCHAR(128)|1||0
5|is_admin|BOOLEAN|0||0
```

---

### **🔹 Insérer des données**
```sql
INSERT INTO users (first_name, last_name, email, password, is_admin) 
VALUES ('John', 'Doe', 'john.doe@example.com', 'hashed_password', 0);
```

---

### **🔹 Lire les données**
```sql
SELECT * FROM users;
```

---

### **🔹 Supprimer une table**
⚠️ **Attention : Supprime toutes les données !**
```sql
DROP TABLE users;
```

---

### **🔹 Quitter SQLite**
```sql
.exit
```

---

## **3️⃣ Déboguer les erreurs courantes**
### ❌ **Flask ne trouve pas l'application (`Could not import 'app'`)**
🔹 Solution :
```bash
export FLASK_APP=app
```

---

### ❌ **Pas de commande `flask db` (`No such command 'db'`)**
🔹 Solution :
```bash
pip install flask-migrate
flask db init  # Si c'est la première migration
```

---

### ❌ **Colonne manquante (`sqlite3.OperationalError: no such column`)**
🔹 Solution :
```bash
flask db migrate -m "Ajout des colonnes manquantes"
flask db upgrade
```

---

### **📌 Récapitulatif des commandes les plus utilisées**
| Objectif | Commande |
|----------|----------|
| **Initialiser Flask-Migrate** | `flask db init` |
| **Créer une migration** | `flask db migrate -m "Description"` |
| **Appliquer une migration** | `flask db upgrade` |
| **Revenir en arrière** | `flask db downgrade` |
| **Supprimer et recréer la base** | `rm mydb.db && flask db upgrade` |
| **Ouvrir SQLite** | `sqlite3 mydb.db` |
| **Lister les tables** | `.tables` |
| **Vérifier les colonnes d’une table** | `PRAGMA table_info(nom_table);` |
| **Lire les données d'une table** | `SELECT * FROM nom_table;` |
| **Insérer des données** | `INSERT INTO nom_table (col1, col2) VALUES ('val1', 'val2');` |
| **Quitter SQLite** | `.exit` |

---

### 🎯 **Conclusion**
Avec ces commandes, tu peux **gérer** les bases de données en Flask avec SQLAlchemy et SQLite, **corriger** les erreurs courantes et **vérifier** les modifications.

✅ **Tu peux maintenant exécuter les commandes pas à pas pour t’assurer que tout fonctionne !** 🚀