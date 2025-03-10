**Flask : Présentation et Utilisations** 🚀

---

### **1. Qu'est-ce que Flask ?**

Flask est un micro-framework web écrit en Python. "Micro" ne signifie pas qu'il est limité en fonctionnalités, mais plutôt qu'il est léger et minimaliste, laissant à l'utilisateur la liberté d'ajouter uniquement les extensions nécessaires à son projet.

📌 **Caractéristiques principales :**
- **Léger et flexible** : parfait pour les API REST ou les petites applications.
- **Facile à apprendre** : syntaxe simple et lisible.
- **Extensible** : supporte des extensions comme SQLAlchemy (ORM), Flask-JWT-Extended (authentification JWT), et bien d'autres.
- **Compatible avec Jinja2** : moteur de templates puissant.
- **Bonne documentation** et large communauté.

---

### **2. Installation de Flask**

```bash
pip install Flask
```

---

### **3. Premier projet Flask**

**Structure du projet :**
```
my_flask_app/
|-- app.py
```

**app.py :**
```python
from flask import Flask

app = Flask(__name__)

@app.route('/')
def home():
    return "Hello, Flask!"

if __name__ == '__main__':
    app.run(debug=True)
```

**Lancer l’application :**
```bash
python app.py
```

Accède à l'URL **http://127.0.0.1:5000/** et tu verras "Hello, Flask!". 🚀

---

### **4. Le pattern Application Factory**

Le pattern Application Factory est une bonne pratique dans Flask. Il consiste à créer une fonction qui configure et retourne une instance de l'application Flask.

📌 **Avantages :**
- **Modularité** : Facile de créer des versions différentes de l'application (développement, test, production).
- **Testabilité** : On peut créer une nouvelle instance propre pour chaque test.
- **Organisation propre** : Configuration, routes et extensions sont mieux organisées.

**Structure modulaire :**
```
my_flask_app/
|-- app/
|   |-- __init__.py           # Application factory
|   |-- models.py             # SQLAlchemy models
|   |-- routes.py             # Routes de l'application
|   |-- config.py             # Configuration
|-- run.py                    # Point d'entrée de l'application
```

**Exemple de configuration (app/config.py)**
```python
class Config:
    SECRET_KEY = 'supersecretkey'
    SQLALCHEMY_DATABASE_URI = 'sqlite:///users.db'
    SQLALCHEMY_TRACK_MODIFICATIONS = False

class DevelopmentConfig(Config):
    DEBUG = True

class ProductionConfig(Config):
    DEBUG = False
```

**Application Factory (app/__init__.py)**
```python
from flask import Flask
from flask_sqlalchemy import SQLAlchemy
from flask_jwt_extended import JWTManager

db = SQLAlchemy()
jwt = JWTManager()

def create_app(config_class='app.config.DevelopmentConfig'):
    app = Flask(__name__)
    app.config.from_object(config_class)

    db.init_app(app)
    jwt.init_app(app)

    from app.routes import main
    app.register_blueprint(main)

    with app.app_context():
        db.create_all()

    return app
```

**Point d'entrée (run.py)**
```python
from app import create_app

app = create_app()

if __name__ == '__main__':
    app.run()
```

---

### **5. Commandes pour tester**

**Lancer l'application :**
```bash
python run.py
```

**Créer un utilisateur :**
```bash
curl -X POST http://127.0.0.1:5000/register \
-H "Content-Type: application/json" \
-d '{"username": "alice", "email": "alice@example.com"}'
```

**Se connecter et récupérer un token :**
```bash
curl -X POST http://127.0.0.1:5000/login \
-H "Content-Type: application/json" \
-d '{"username": "alice"}'
```

**Accéder à une route protégée :**
```bash
curl -H "Authorization: Bearer <your_token_here>" \
http://127.0.0.1:5000/protected
```

---

### **6. Conclusion**

Cette documentation présente les bases de Flask, ainsi qu'une structure modulaire avancée avec l'application factory. Cela permet d'obtenir une application plus claire, extensible et facile à maintenir.

Prochaine étape : Ajouter des tests unitaires ou des validations avec Flask-WTF ou Marshmallow ? Dis-moi ! 🚀

