### Tester les opérations CRUD sur les Amenities (amenities)

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