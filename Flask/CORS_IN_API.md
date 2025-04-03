# 🧠 I. Qu'est-ce que CORS ?

### ➤ Définition :
CORS (Cross-Origin Resource Sharing) est un **mécanisme de sécurité** mis en place par les navigateurs pour **contrôler les requêtes HTTP effectuées entre deux origines différentes**.

### ➤ Une "origine", c’est quoi ?
Une origine est définie par le **triplet : protocole + domaine + port**.
- Exemple 1 : `http://localhost:5000`
- Exemple 2 : `https://api.monsite.com`

Si une page web chargée depuis `http://localhost:3000` essaie de faire une requête vers `http://localhost:5000`, cela **déclenche une vérification CORS**, car ce sont **deux origines différentes** (port différent ici).

---

## 🔐 II. Pourquoi CORS est-il important ?

CORS protège les utilisateurs contre des attaques du type **Cross-Site Request Forgery (CSRF)** ou autres abus de scripts malveillants exécutés depuis d'autres sites.

Par défaut, le navigateur **bloque** les requêtes AJAX (XHR, fetch) vers une API externe **si l’API ne donne pas explicitement l’autorisation** via les **en-têtes HTTP**.

---

## ⚠️ III. Symptômes d’un problème CORS

- Requête bloquée par le navigateur
- Message dans la console :
  ```
  Access to fetch at 'http://localhost:5000/data' from origin 'http://localhost:3000' has been blocked by CORS policy
  ```

---

## 🛠 IV. Activer CORS dans une API Flask

### 1. Installation de Flask-CORS

```bash
pip install -U flask-cors
```

### 2. Importation

Dans votre fichier principal `app.py` :

```python
from flask import Flask
from flask_cors import CORS
```

### 3. Activation de CORS

#### a. **Activer globalement :**

```python
app = Flask(__name__)
CORS(app)
```

Cela autorise **toutes les origines** à accéder à toutes les routes — à **n’utiliser que pour du développement** ou avec prudence en production.

#### b. **Limiter à une origine spécifique :**

```python
CORS(app, origins=["http://localhost:3000"])
```

#### c. **Limiter par route :**

```python
from flask_cors import cross_origin

@app.route("/api/data")
@cross_origin(origins="http://localhost:3000")
def get_data():
    return {"message": "Hello"}
```

---

## 🧾 V. En-têtes HTTP ajoutés par Flask-CORS

Une fois CORS activé, Flask ajoutera automatiquement ces en-têtes HTTP dans ses réponses :

```http
Access-Control-Allow-Origin: http://localhost:3000
Access-Control-Allow-Methods: GET, POST, ...
Access-Control-Allow-Headers: Content-Type
```

---

## 📦 VI. Requêtes préalables (Preflight Requests)

Quand une requête est "sensible" (ex : méthode PUT, DELETE, ou en-têtes personnalisés), le navigateur **envoie d'abord une requête OPTIONS** appelée "preflight".

Flask-CORS gère automatiquement ces requêtes si activé.

---

## 🧪 VII. Tester CORS

- Utilise un **frontend React, Vue ou Angular** qui appelle l’API Flask hébergée sur un autre port.
- Teste via un navigateur (Postman **ne déclenche pas les erreurs CORS**).

---

## 🛑 VIII. Résolution des erreurs fréquentes

| Problème | Cause probable | Solution |
|---------|----------------|----------|
| `Blocked by CORS policy` | CORS pas activé ou mal configuré | Utiliser `CORS(app)` ou `@cross_origin` |
| Preflight échoue (code 405) | Flask ne gère pas les OPTIONS | Vérifier que `CORS()` est bien appliqué |
| `Access-Control-Allow-Origin` manquant | Origine non autorisée | Spécifier `origins=[...]` correctement |
| Erreur 500 côté API | Mauvais endpoint, erreur logique | Regarder les logs Flask |

---

## 🔐 IX. Conseils pour la production

- **Ne pas autoriser toutes les origines** (`*`) en production.
- **Limiter** les origines aux domaines connus.
- Pensez à **activer les options de sécurité comme HTTPS**, rate limiting, etc.

---

## 📌 X. Exemple complet

```python
#!/usr/bin/python3
from flask import Flask, jsonify
from flask_cors import CORS

app = Flask(__name__)
CORS(app, origins=["http://localhost:3000"])

@app.route('/api/hello')
def hello():
    return jsonify(message="Hello from Flask API!")

if __name__ == '__main__':
    app.run(port=5000, debug=True)
```

---

## ✅ En résumé

- CORS protège les utilisateurs d’accès non autorisés entre origines.
- Flask ne gère pas CORS nativement → on utilise Flask-CORS.
- Toujours tester via un navigateur.
- En prod, **limiter l’accès aux origines connues** pour éviter les failles de sécurité.

---
