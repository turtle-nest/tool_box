# Mapper une entité utilisateur avec SQLAlchemy dans Flask

Le mappage d'une entité utilisateur avec SQLAlchemy dans une application Flask consiste à créer une représentation de la table des utilisateurs dans la base de données via un modèle Python. Voici une synthèse des étapes essentielles pour mettre cela en place.

### 1. Installation des bibliothèques nécessaires

```bash
pip install Flask flask_sqlalchemy flask_bcrypt
```

### 2. Configuration de Flask et SQLAlchemy

Créez une application Flask et configurez la connexion à la base de données :

```python
from flask import Flask
from flask_sqlalchemy import SQLAlchemy

app = Flask(__name__)
app.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:///users.db'
app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = False

db = SQLAlchemy(app)
```

### 3. Création du modèle utilisateur

Le modèle utilisateur est une classe Python qui mappe la table SQL :

```python
from flask_bcrypt import Bcrypt

bcrypt = Bcrypt(app)

class User(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    username = db.Column(db.String(150), unique=True, nullable=False)
    email = db.Column(db.String(150), unique=True, nullable=False)
    password_hash = db.Column(db.String(128), nullable=False)

    def __init__(self, username, email, password):
        self.username = username
        self.email = email
        self.password_hash = bcrypt.generate_password_hash(password).decode('utf-8')
```

Explication :
- `id` : clé primaire unique.
- `username` et `email` : colonnes uniques et obligatoires.
- `password_hash` : mot de passe sécurisé, haché avec Flask-Bcrypt.

### 4. Création de la base de données

Avant d'ajouter des utilisateurs, créez la base de données et la table :

```python
with app.app_context():
    db.create_all()
```

### 5. Ajout d'un utilisateur

Pour insérer un utilisateur dans la base de données :

```python
new_user = User(username='john', email='john@example.com', password='password123')
db.session.add(new_user)
db.session.commit()
```

### 6. Lecture des utilisateurs

Pour récupérer tous les utilisateurs :

```python
users = User.query.all()
for user in users:
    print(user.username, user.email)
```

### 7. Mise à jour d'un utilisateur

```python
user = User.query.get(1)
user.username = 'john_doe'
db.session.commit()
```

### 8. Suppression d'un utilisateur

```python
user = User.query.get(1)
db.session.delete(user)
db.session.commit()
```

En suivant ces étapes, vous avez mappé l'entité utilisateur à un modèle SQLAlchemy et configuré les opérations CRUD pour interagir avec la base de données.

