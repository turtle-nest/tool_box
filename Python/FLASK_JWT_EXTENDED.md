# Implémenter l'authentification JWT avec `flask-jwt-extended`

Les JSON Web Tokens (JWT) sont une méthode standard pour représenter des informations de manière sécurisée entre deux parties. Dans le contexte des applications web, ils sont couramment utilisés pour authentifier les utilisateurs et sécuriser les API. Ce tutoriel vous guidera à travers l'implémentation de l'authentification JWT dans une application Flask en utilisant la bibliothèque `flask-jwt-extended`.

## Prérequis

Avant de commencer, assurez-vous d'avoir installé les bibliothèques nécessaires. Vous pouvez les installer en utilisant `pip` :

```bash
pip install Flask flask-jwt-extended
```


## Configuration de l'application Flask

Commencez par importer les modules nécessaires et configurez l'application Flask avec une clé secrète pour signer les tokens JWT :

```python
from flask import Flask, jsonify, request
from flask_jwt_extended import JWTManager, create_access_token, jwt_required, get_jwt_identity

app = Flask(__name__)

# Clé secrète pour signer les tokens JWT
app.config['JWT_SECRET_KEY'] = 'votre_clé_secrète_ici'
jwt = JWTManager(app)
```


## Création des routes d'inscription et de connexion

Pour gérer l'authentification des utilisateurs, créez des routes pour l'inscription et la connexion. Dans un environnement réel, vous devriez stocker les informations des utilisateurs dans une base de données sécurisée et hacher les mots de passe. Pour simplifier, nous utiliserons un dictionnaire en mémoire :

```python
# Simuler une base de données d'utilisateurs
users_db = {}

@app.route('/register', methods=['POST'])
def register():
    username = request.json.get('username')
    password = request.json.get('password')

    if username in users_db:
        return jsonify({'message': 'Utilisateur déjà existant'}), 409

    # Enregistrer l'utilisateur (dans un vrai cas, hachez le mot de passe)
    users_db[username] = password
    return jsonify({'message': 'Utilisateur créé avec succès'}), 201

@app.route('/login', methods=['POST'])
def login():
    username = request.json.get('username')
    password = request.json.get('password')

    if users_db.get(username) != password:
        return jsonify({'message': 'Nom d\'utilisateur ou mot de passe incorrect'}), 401

    # Créer un token d'accès JWT
    access_token = create_access_token(identity=username)
    return jsonify(access_token=access_token), 200
```


## Protection des routes avec JWT

Pour sécuriser certaines routes de votre application, utilisez le décorateur `@jwt_required()` fourni par `flask-jwt-extended`. Cela garantit que seules les requêtes avec un token JWT valide peuvent accéder à ces routes :

```python
@app.route('/protected', methods=['GET'])
@jwt_required()
def protected():
    # Obtenir l'identité de l'utilisateur à partir du token JWT
    current_user = get_jwt_identity()
    return jsonify(logged_in_as=current_user), 200
```


## Gestion de la révocation des tokens (liste noire)

Pour améliorer la sécurité, il est essentiel de pouvoir révoquer des tokens, par exemple lors de la déconnexion d'un utilisateur. Une approche courante consiste à maintenir une liste noire des tokens révoqués. Voici comment vous pouvez implémenter cela en utilisant une structure en mémoire :

```python
from flask_jwt_extended import JWTManager, create_access_token, jwt_required, get_jwt_identity, get_jwt

# Initialiser la liste noire
blacklist = set()

# Configurer la fonction de vérification de la liste noire
@jwt.token_in_blocklist_loader
def check_if_token_revoked(jwt_header, jwt_payload):
    jti = jwt_payload['jti']
    return jti in blacklist

@app.route('/logout', methods=['DELETE'])
@jwt_required()
def logout():
    jti = get_jwt()['jti']
    blacklist.add(jti)
    return jsonify({"message": "Déconnecté avec succès"}), 200
```


Dans cet exemple, chaque token possède un identifiant unique (jti). Lors de la déconnexion, cet identifiant est ajouté à la liste noire. Pour chaque requête protégée, la fonction `check_if_token_revoked` vérifie si le jti du token est dans la liste noire, empêchant ainsi l'accès si le token a été révoqué.

**Remarque :** Dans une application de production, il est recommandé d'utiliser une base de données ou un système de cache (comme Redis) pour stocker la liste noire, afin de garantir la persistance et l'efficacité.

## Exécution de l'application

Pour exécuter l'application Flask, ajoutez le bloc de code suivant à la fin de votre script :

```python
if __name__ == '__main__':
    app.run(debug=True)
```


## Considérations de sécurité

Lors de l'implémentation de l'authentification JWT, gardez à l'esprit les points suivants :

- **Stockage sécurisé des tokens côté client :** Assurez-vous que les tokens sont stockés de manière sécurisée pour éviter les attaques XSS et CSRF.
- **Utilisation de clés secrètes robustes :** Utilisez des clés secrètes complexes et difficiles à deviner pour signer vos tokens.
- **Gestion de l'expiration des tokens :** Définissez une durée de vie appropriée pour vos tokens et implémentez des mécanismes de rafraîchissement si nécessaire.

Pour une liste complète des bonnes pratiques et des considérations de sécurité liées aux JWT, consultez la [cheat sheet de l'OWASP sur les JSON Web Tokens](https://cheatsheetseries.owasp.org/cheatsheets/JSON_Web_Token_for_Java_Cheat_Sheet.html).

## Conclusion

En suivant ce tutoriel, vous avez appris à implémenter l'authentification JWT dans une application Flask en utilisant `flask-jwt-extended`. Cette méthode offre une solution sécurisée et sans état pour gérer l'authentification des utilisateurs dans vos applications web. 