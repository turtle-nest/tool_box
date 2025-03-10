# Implémenter des points de terminaison d'accès administrateur

Pour implémenter des points de terminaison réservés aux administrateurs dans une application Flask, vous pouvez utiliser la bibliothèque `flask-jwt-extended` pour gérer l'authentification par JSON Web Tokens (JWT). Cette approche vous permet de définir des rôles d'utilisateur et de sécuriser des routes spécifiques en fonction de ces rôles.

## Prérequis

Assurez-vous d'avoir installé Flask et `flask-jwt-extended` :

```bash
pip install Flask flask-jwt-extended
```


## Configuration de l'application Flask

Commencez par configurer votre application Flask et initialiser `flask-jwt-extended` :

```python
from flask import Flask, jsonify, request
from flask_jwt_extended import JWTManager, create_access_token, jwt_required, get_jwt

app = Flask(__name__)
app.config['JWT_SECRET_KEY'] = 'votre_clé_secrète_ici'
jwt = JWTManager(app)
```


## Gestion des utilisateurs et des rôles

Pour cet exemple, nous utiliserons un dictionnaire en mémoire pour stocker les utilisateurs et leurs rôles. Dans une application réelle, une base de données serait recommandée.

```python
# Simuler une base de données d'utilisateurs avec rôles
users_db = {
    'admin': {'password': 'adminpass', 'roles': ['admin']},
    'user1': {'password': 'user1pass', 'roles': ['user']}
}
```


## Routes d'inscription et de connexion

Implémentez les routes pour l'inscription et la connexion des utilisateurs :

```python
@app.route('/register', methods=['POST'])
def register():
    username = request.json.get('username')
    password = request.json.get('password')
    roles = request.json.get('roles', [])

    if not username or not password:
        return jsonify({'message': 'Nom d\'utilisateur et mot de passe requis'}), 400

    if username in users_db:
        return jsonify({'message': 'Utilisateur déjà existant'}), 409

    # Enregistrer l'utilisateur
    users_db[username] = {'password': password, 'roles': roles}
    return jsonify({'message': 'Utilisateur créé avec succès'}), 201

@app.route('/login', methods=['POST'])
def login():
    username = request.json.get('username')
    password = request.json.get('password')

    if not username or not password:
        return jsonify({'message': 'Nom d\'utilisateur et mot de passe requis'}), 400

    user = users_db.get(username)
    if not user or user['password'] != password:
        return jsonify({'message': 'Nom d\'utilisateur ou mot de passe incorrect'}), 401

    # Créer un token d'accès JWT avec les rôles de l'utilisateur
    access_token = create_access_token(identity=username, additional_claims={'roles': user['roles']})
    return jsonify(access_token=access_token), 200
```


## Création d'un décorateur pour les rôles

Pour restreindre l'accès aux routes en fonction des rôles, créez un décorateur personnalisé :

```python
from functools import wraps

def role_required(required_role):
    def decorator(fn):
        @wraps(fn)
        @jwt_required()
        def wrapper(*args, **kwargs):
            claims = get_jwt()
            roles = claims.get('roles', [])
            if required_role not in roles:
                return jsonify({'message': 'Accès interdit'}), 403
            return fn(*args, **kwargs)
        return wrapper
    return decorator
```


## Création d'un point de terminaison protégé pour les administrateurs

Utilisez le décorateur `role_required` pour protéger les routes réservées aux administrateurs :

```python
@app.route('/admin', methods=['GET'])
@role_required('admin')
def admin_endpoint():
    return jsonify({'message': 'Bienvenue, administrateur !'}), 200
```


## Exécution de l'application

Ajoutez le bloc suivant pour exécuter l'application :

```python
if __name__ == '__main__':
    app.run(debug=True)
```


## Test des points de terminaison avec `curl`

Vous pouvez tester vos points de terminaison à l'aide de `curl` :

1. **Inscription d'un nouvel utilisateur avec rôle administrateur :**

```bash
curl -X POST -H "Content-Type: application/json" -d '{"username": "admin", "password": "adminpass", "roles": ["admin"]}' http://127.0.0.1:5000/register
```


2. **Connexion pour obtenir un token JWT :**

```bash
curl -X POST -H "Content-Type: application/json" -d '{"username": "admin", "password": "adminpass"}' http://127.0.0.1:5000/login
```


Cette commande retournera un token JWT que vous pourrez utiliser pour accéder aux routes protégées.

3. **Accès au point de terminaison administrateur :**

```bash
curl -H "Authorization: Bearer VOTRE_TOKEN_ICI" http://127.0.0.1:5000/admin
```


Remplacez `VOTRE_TOKEN_ICI` par le token obtenu lors de la connexion.

En suivant ces étapes, vous pouvez implémenter des points de terminaison sécurisés réservés aux administrateurs dans votre application Flask en utilisant `flask-jwt-extended`. L'utilisation de `curl` facilite les tests de ces points de terminaison en ligne de commande. 