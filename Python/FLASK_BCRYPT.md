Flask-Bcrypt est une extension Flask qui fournit des utilitaires de hachage bcrypt pour votre application. Étant donné la puissance croissante du matériel moderne, comme les GPU, les algorithmes de hachage traditionnels tels que MD5 et SHA1 sont devenus plus vulnérables aux attaques. Bcrypt est conçu pour être lent intentionnellement, offrant ainsi une sécurité accrue pour des données sensibles comme les mots de passe. 

### Installation

Pour installer Flask-Bcrypt, utilisez l'une des commandes suivantes :

```bash
pip install flask-bcrypt
```


Assurez-vous que les en-têtes de développement Python sont installés, car ils sont nécessaires pour la dépendance `py-bcrypt`. Sur les systèmes basés sur Debian, recherchez le paquet `python-dev`, et sur les systèmes basés sur RedHat, le paquet `python-devel`. 

### Utilisation

Après l'installation, importez la classe `Bcrypt` et passez-lui l'objet Flask :

```python
from flask import Flask
from flask_bcrypt import Bcrypt

app = Flask(__name__)
bcrypt = Bcrypt(app)
```


Les deux principales méthodes fournies par Flask-Bcrypt sont `generate_password_hash` et `check_password_hash`. Voici comment les utiliser :

```python
# Hachage d'un mot de passe
pw_hash = bcrypt.generate_password_hash('mot_de_passe').decode('utf-8')

# Vérification du mot de passe
bcrypt.check_password_hash(pw_hash, 'mot_de_passe')  # Retourne True
```


Il est important de noter que dans Python 3, vous devez décoder le hachage généré en utilisant `.decode('utf-8')`. 

### Exemple complet

Voici un exemple complet d'application Flask intégrant Flask-Bcrypt pour le hachage des mots de passe :

```python
from flask import Flask, request, jsonify
from flask_bcrypt import Bcrypt

app = Flask(__name__)
bcrypt = Bcrypt(app)

# Simuler une base de données
users = {}

@app.route('/register', methods=['POST'])
def register():
    username = request.json.get('username')
    password = request.json.get('password')
    if username in users:
        return jsonify({'message': 'Utilisateur déjà existant'}), 400
    pw_hash = bcrypt.generate_password_hash(password).decode('utf-8')
    users[username] = pw_hash
    return jsonify({'message': 'Utilisateur créé avec succès'}), 201

@app.route('/login', methods=['POST'])
def login():
    username = request.json.get('username')
    password = request.json.get('password')
    pw_hash = users.get(username)
    if pw_hash and bcrypt.check_password_hash(pw_hash, password):
        return jsonify({'message': 'Connexion réussie'}), 200
    return jsonify({'message': 'Nom d\'utilisateur ou mot de passe incorrect'}), 400

if __name__ == '__main__':
    app.run(debug=True)
```


Dans cet exemple, nous avons deux routes : `/register` pour enregistrer un nouvel utilisateur avec un mot de passe haché, et `/login` pour vérifier les informations d'identification de l'utilisateur.

### Bonnes pratiques

- **Configurer les tours de hachage** : Vous pouvez définir la complexité du hachage en configurant la valeur `BCRYPT_LOG_ROUNDS` dans les configurations de votre application Flask. Par défaut, cette valeur est fixée à 12. 

- **Gestion des mots de passe longs** : Par défaut, l'algorithme bcrypt a une longueur maximale de mot de passe de 72 octets et ignore les octets au-delà de cette limite. Une solution consiste à hacher le mot de passe donné en utilisant un hachage cryptographique (tel que sha256), à prendre son hexdigest pour éviter les problèmes d'octets NULL, puis à hacher le résultat avec bcrypt. Si la valeur de configuration `BCRYPT_HANDLE_LONG_PASSWORDS` est définie sur True, cette solution sera activée. Attention : n'activez pas cette option sur un projet qui utilise déjà Flask-Bcrypt, sinon vous risquez de compromettre la vérification des mots de passe. 

Pour une démonstration pratique de l'utilisation de Flask-Bcrypt, vous pouvez consulter cette vidéo :

videoAuthentication and Authorization with Flask-Bcryptturn0search3

En suivant ce tutoriel, vous devriez être en mesure d'intégrer efficacement Flask-Bcrypt dans votre application Flask pour assurer une gestion sécurisée des mots de passe. 