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

### **4. Routes et paramètres**  

**Route avec paramètre dynamique :**  
```python
@app.route('/user/<username>')
def greet_user(username):
    return f"Hello, {username}!"
```

Accède à **http://127.0.0.1:5000/user/Alice** ➡️ "Hello, Alice!"  

---

### **5. Méthodes HTTP (GET, POST, PUT, DELETE)**  

```python
from flask import request, jsonify

@app.route('/user', methods=['POST'])
def create_user():
    data = request.json
    return jsonify({"message": f"User {data['name']} created!"}), 201
```

Requête POST avec JSON :  
```bash
curl -X POST http://127.0.0.1:5000/user \
-H "Content-Type: application/json" \
-d '{"name": "Alice"}'
```

---

### **6. Templates HTML avec Jinja2**  

**Structure :**  
```
my_flask_app/
|-- app.py
|-- templates/
    |-- home.html
```

**home.html :**  
```html
<!DOCTYPE html>
<html>
<head>
    <title>Flask App</title>
</head>
<body>
    <h1>Welcome, {{ username }}!</h1>
</body>
</html>
```

**app.py :**  
```python
from flask import render_template

@app.route('/welcome/<username>')
def welcome(username):
    return render_template('home.html', username=username)
```

---

### **7. Flask avec SQLAlchemy (Base de données)**  

**Installation :**  
```bash
pip install Flask-SQLAlchemy
```

**app.py :**  
```python
from flask_sqlalchemy import SQLAlchemy

app.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:///users.db'
db = SQLAlchemy(app)

class User(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    name = db.Column(db.String(80), nullable=False)

db.create_all()
```

---

### **8. Authentification avec JWT (Flask-JWT-Extended)**  

**Installation :**  
```bash
pip install Flask-JWT-Extended
```

**app.py :**  
```python
from flask_jwt_extended import JWTManager, create_access_token, jwt_required, get_jwt_identity

app.config['JWT_SECRET_KEY'] = 'supersecretkey'
jwt = JWTManager(app)

@app.route('/login', methods=['POST'])
def login():
    username = request.json.get('username')
    access_token = create_access_token(identity=username)
    return jsonify(access_token=access_token)

@app.route('/protected', methods=['GET'])
@jwt_required()
def protected():
    current_user = get_jwt_identity()
    return jsonify(logged_in_as=current_user)
```

---

### **9. Conclusion**  

Flask est un framework ultra flexible et adapté pour :  
- Construire des **API RESTful**.  
- Créer des **interfaces web** avec des templates HTML.  
- Gérer l’**authentification** avec JWT.  
- Interagir avec des bases de données via **SQLAlchemy**.  

Tu veux qu’on aille plus loin avec un projet complet Flask (comme une API CRUD avec JWT et SQLAlchemy) ? Dis-moi ! 🚀  