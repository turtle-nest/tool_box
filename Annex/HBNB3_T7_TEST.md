# Tester les opérations CRUD sur les Amenities (amenities)

### Tester les opérations CRUD sur les Users (users)

#### a) Créer un utilisateur (POST)  
**URL** : `http://127.0.0.1:5000/users/`  
**Méthode** : `POST`  
**Headers** :  
```
Content-Type: application/json
```
**Body (JSON)** :
```json
{
    "first_name": "John",
    "last_name": "Doe",
    "email": "john.doe@example.com",
    "password": "securepassword"
}
```
**Réponse attendue (201 Created)** :
```json
{
    "id": 1,
    "first_name": "John",
    "last_name": "Doe",
    "email": "john.doe@example.com"
}
```

---

#### b) Lire la liste des utilisateurs (GET)  
**URL** : `http://127.0.0.1:5000/users/`  
**Méthode** : `GET`  
**Réponse attendue (200 OK)** :
```json
[
    {
        "id": 1,
        "first_name": "John",
        "last_name": "Doe",
        "email": "john.doe@example.com"
    }
]
```

---

#### c) Lire un utilisateur spécifique (GET)  
**URL** : `http://127.0.0.1:5000/users/1`  
**Méthode** : `GET`  
**Réponse attendue (200 OK)** :
```json
{
    "id": 1,
    "first_name": "John",
    "last_name": "Doe",
    "email": "john.doe@example.com"
}
```

---

#### d) Mettre à jour un utilisateur (PUT)  
**URL** : `http://127.0.0.1:5000/users/1`  
**Méthode** : `PUT`  
**Headers** :  
```
Content-Type: application/json
```
**Body (JSON)** :
```json
{
    "first_name": "Jonathan",
    "last_name": "Doe",
    "email": "jonathan.doe@example.com"
}
```
**Réponse attendue (200 OK)** :
```json
{
    "message": "User updated successfully"
}
```

---

#### e) Supprimer un utilisateur (DELETE)  
**URL** : `http://127.0.0.1:5000/users/1`  
**Méthode** : `DELETE`  
**Réponse attendue (200 OK)** :
```json
{
    "message": "User deleted successfully"
}
```
---

Non, la ligne :

```python
is_admin = db.Column(db.Boolean, default=False)
```

ne change pas les tests CRUD que tu avais définis précédemment pour les **Users**, **Amenities**, **Places** et **Reviews**. Cependant, voici quelques points à vérifier :

### **Impact sur les Tests CRUD**
1. **Création d'un utilisateur (POST)**  
   - Si l’attribut `is_admin` est défini avec une valeur par défaut `False`, alors si tu ne l’inclus pas explicitement dans le `body` de la requête POST, il prendra cette valeur automatiquement.
   - Si tu veux créer un admin, il faudra ajouter `"is_admin": true` dans le `body`.

   **Exemple de test modifié (optionnel) pour un admin :**
   ```json
   {
       "first_name": "Admin",
       "last_name": "User",
       "email": "admin@example.com",
       "password": "securepassword",
       "is_admin": true
   }
   ```

2. **Lecture d’un utilisateur (GET)**  
   - L’ajout de `is_admin` à la base de données signifie que si tu veux afficher cet attribut dans la réponse JSON, il faut l'ajouter à la méthode `to_safe_dict()`.

   **Modifie `to_safe_dict()` ainsi :**
   ```python
   def to_safe_dict(self):
       """Return a dictionary without the password field"""
       return {
           'id': self.id,
           'first_name': self.first_name,
           'last_name': self.last_name,
           'email': self.email,
           'is_admin': self.is_admin  # Ajout de cet attribut
       }
   ```

   **Réponse attendue après GET :**
   ```json
   {
       "id": 1,
       "first_name": "John",
       "last_name": "Doe",
       "email": "john.doe@example.com",
       "is_admin": false
   }
   ```

3. **Mise à jour d’un utilisateur (PUT)**  
   - Tu peux maintenant modifier cet attribut si besoin :
   ```json
   {
       "is_admin": true
   }
   ```
   **Réponse attendue après PUT :**
   ```json
   {
       "message": "User updated successfully"
   }
   ```

4. **Suppression d’un utilisateur (DELETE)**  
   - Aucun changement requis ici.


🔍 **Vérifie tes tests Postman pour voir si l’attribut `is_admin` est bien géré comme attendu !** 🚀
---


#### a) Créer un amenity (POST)  
**URL** : `http://127.0.0.1:5000/amenities/`  
**Méthode** : `POST`  
**Headers** :  
```
Content-Type: application/json
```
**Body (JSON)** :
```json
{
    "name": "Swimming Pool"
}
```
**Réponse attendue (201 Created)** :
```json
{
    "id": 1,
    "name": "Swimming Pool"
}
```

#### b) Lire la liste des amenities (GET)  
**URL** : `http://127.0.0.1:5000/amenities/`  
**Méthode** : `GET`  
**Réponse attendue (200 OK)** :
```json
[
    {
        "id": 1,
        "name": "Swimming Pool"
    }
]
```

#### c) Lire un amenity spécifique (GET)  
**URL** : `http://127.0.0.1:5000/amenities/1`  
**Méthode** : `GET`  
**Réponse attendue (200 OK)** :
```json
{
    "id": 1,
    "name": "Swimming Pool"
}
```

#### d) Mettre à jour un amenity (PUT)  
**URL** : `http://127.0.0.1:5000/amenities/1`  
**Méthode** : `PUT`  
**Headers** :  
```
Content-Type: application/json
```
**Body (JSON)** :
```json
{
    "name": "Gym"
}
```
**Réponse attendue (200 OK)** :
```json
{
    "message": "Amenity updated successfully"
}
```

#### e) Supprimer un amenity (DELETE)  
**URL** : `http://127.0.0.1:5000/amenities/1`  
**Méthode** : `DELETE`  
**Réponse attendue (200 OK)** :
```json
{
    "message": "Amenity deleted successfully"
}
```

---

### Tester les opérations CRUD sur les Places (places)

#### a) Créer un place (POST)  
**URL** : `http://127.0.0.1:5000/places/`  
**Méthode** : `POST`  
**Headers** :  
```
Content-Type: application/json
```
**Body (JSON)** :
```json
{
    "title": "Cozy Apartment",
    "description": "A nice place to stay",
    "price": 100.5,
    "latitude": 48.8566,
    "longitude": 2.3522
}
```
**Réponse attendue (201 Created)** :
```json
{
    "id": 1,
    "title": "Cozy Apartment",
    "description": "A nice place to stay",
    "price": 100.5,
    "latitude": 48.8566,
    "longitude": 2.3522
}
```

#### b) Lire la liste des places (GET)  
**URL** : `http://127.0.0.1:5000/places/`  
**Méthode** : `GET`  
**Réponse attendue (200 OK)** :
```json
[
    {
        "id": 1,
        "title": "Cozy Apartment",
        "description": "A nice place to stay",
        "price": 100.5,
        "latitude": 48.8566,
        "longitude": 2.3522
    }
]
```

#### c) Lire un place spécifique (GET)  
**URL** : `http://127.0.0.1:5000/places/1`  
**Méthode** : `GET`  
**Réponse attendue (200 OK)** :
```json
{
    "id": 1,
    "title": "Cozy Apartment",
    "description": "A nice place to stay",
    "price": 100.5,
    "latitude": 48.8566,
    "longitude": 2.3522
}
```

#### d) Mettre à jour un place (PUT)  
**URL** : `http://127.0.0.1:5000/places/1`  
**Méthode** : `PUT`  
**Headers** :  
```
Content-Type: application/json
```
**Body (JSON)** :
```json
{
    "title": "Luxury Apartment",
    "description": "A luxurious place in Paris",
    "price": 250.0
}
```
**Réponse attendue (200 OK)** :
```json
{
    "message": "Place updated successfully"
}
```

#### e) Supprimer un place (DELETE)  
**URL** : `http://127.0.0.1:5000/places/1`  
**Méthode** : `DELETE`  
**Réponse attendue (200 OK)** :
```json
{
    "message": "Place deleted successfully"
}
```

---

### Tester les opérations CRUD sur les Reviews (reviews)

#### a) Créer un review (POST)  
**URL** : `http://127.0.0.1:5000/reviews/`  
**Méthode** : `POST`  
**Headers** :  
```
Content-Type: application/json
```
**Body (JSON)** :
```json
{
    "text": "Great place!",
    "rating": 5
}
```
**Réponse attendue (201 Created)** :
```json
{
    "id": 1,
    "text": "Great place!",
    "rating": 5
}
```

#### b) Lire la liste des reviews (GET)  
**URL** : `http://127.0.0.1:5000/reviews/`  
**Méthode** : `GET`  
**Réponse attendue (200 OK)** :
```json
[
    {
        "id": 1,
        "text": "Great place!",
        "rating": 5
    }
]
```

#### c) Lire un review spécifique (GET)  
**URL** : `http://127.0.0.1:5000/reviews/1`  
**Méthode** : `GET`  
**Réponse attendue (200 OK)** :
```json
{
    "id": 1,
    "text": "Great place!",
    "rating": 5
}
```

#### d) Mettre à jour un review (PUT)  
**URL** : `http://127.0.0.1:5000/reviews/1`  
**Méthode** : `PUT`  
**Headers** :  
```
Content-Type: application/json
```
**Body (JSON)** :
```json
{
    "text": "Amazing experience!",
    "rating": 4
}
```
**Réponse attendue (200 OK)** :
```json
{
    "message": "Review updated successfully"
}
```

#### e) Supprimer un review (DELETE)  
**URL** : `http://127.0.0.1:5000/reviews/1`  
**Méthode** : `DELETE`  
**Réponse attendue (200 OK)** :
```json
{
    "message": "Review deleted successfully"
}
```

---

Après ces tests, tu auras validé toutes les opérations CRUD pour **Amenities**, **Places** et **Reviews** avec **Postman** ! 🚀