### 📌 **Afficher les tables avec Flask Shell**

Si vous utilisez **Flask** avec SQLAlchemy et que vous souhaitez voir les tables de votre base de données dans le shell interactif, voici un guide détaillé.

---

## **1️⃣ Ouvrir le shell Flask**
Lancez votre terminal dans votre projet Flask et exécutez :

```bash
flask shell
```

Cela ouvrira un shell Python interactif avec le contexte de votre application.

---

## **2️⃣ Importer l’instance de la base de données**
Une fois dans le shell, importez votre base de données (`db`) et vos modèles :

```python
from votre_application import db
from votre_application.models import *  # Importer tous les modèles
```

🔹 **Remplacez** `votre_application` par le nom réel de votre module Flask.

---

## **3️⃣ Lister toutes les tables**
Pour voir toutes les tables présentes dans la base de données :

```python
db.engine.table_names()
```

⚠️ **SQLAlchemy 2.0** : Si vous utilisez SQLAlchemy 2.0, utilisez plutôt :

```python
db.inspect(db.engine).get_table_names()
```

---

## **4️⃣ Voir la structure d’une table**
Si vous souhaitez voir la structure d’une table spécifique, utilisez :

```python
from sqlalchemy import inspect
inspector = inspect(db.engine)
columns = inspector.get_columns('nom_de_votre_table')
for column in columns:
    print(column['name'], column['type'])
```

🔹 **Remplacez** `'nom_de_votre_table'` par le nom de la table dont vous voulez voir la structure.

---

## **5️⃣ Afficher les données d’une table**
Pour afficher les enregistrements d’un modèle :

```python
VotreModèle.query.all()
```

Par exemple, si vous avez un modèle `User` :

```python
User.query.all()
```

Pour afficher les 5 premiers utilisateurs avec un affichage plus lisible :

```python
for user in User.query.limit(5).all():
    print(user.id, user.username, user.email)
```

---

## **6️⃣ Vérifier la définition d’une table**
Si vous souhaitez voir la définition SQLAlchemy d’un modèle :

```python
print(User.__table__)
```

Cela affichera la structure du modèle `User`.

---

### 🎯 **Résumé des commandes utiles**
| Action | Commande |
|--------|---------|
| Ouvrir le shell Flask | `flask shell` |
| Lister les tables | `db.engine.table_names()` (SQLAlchemy <2.0) <br> `db.inspect(db.engine).get_table_names()` (SQLAlchemy 2.0) |
| Voir les colonnes d'une table | `inspector.get_columns('nom_table')` |
| Afficher toutes les entrées d’un modèle | `VotreModèle.query.all()` |
| Afficher les 5 premiers enregistrements | `VotreModèle.query.limit(5).all()` |
| Voir la définition d’un modèle | `print(VotreModèle.__table__)` |

---

Avec ces commandes, vous pouvez facilement explorer votre base de données avec Flask et SQLAlchemy. 🚀  
Besoin d’aide pour un cas particulier ? N’hésite pas à demander ! 😊