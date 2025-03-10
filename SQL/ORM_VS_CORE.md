**SQLAlchemy : Core vs ORM — Présentation complète**  

---

**1. Introduction à SQLAlchemy**  
SQLAlchemy est une bibliothèque Python puissante pour interagir avec des bases de données relationnelles comme MySQL, PostgreSQL ou SQLite. Elle offre deux approches principales :  

- **SQLAlchemy Core** : une approche impérative, proche du SQL, basée sur des expressions et des tables.  
- **SQLAlchemy ORM (Object Relational Mapper)** : une approche orientée objet qui mappe des classes Python sur des tables SQL.  

Ces deux approches utilisent la même base d'outils, mais elles diffèrent dans leur style et leur niveau d'abstraction.  

---

**2. SQLAlchemy Core** : approche déclarative et proche du SQL  

**a) Caractéristiques principales :**  
- Requêtes SQL expressives via Python.  
- Flexible et performant, avec un contrôle total sur les requêtes.  
- Approche adaptée pour des scripts SQL complexes ou des projets avec des besoins précis en optimisation.  

**b) Exemple avec SQLAlchemy Core :**  

**Installation :**  
```bash
pip install sqlalchemy
```

**Connexion à la base de données :**  
```python
from sqlalchemy import create_engine, MetaData, Table, Column, Integer, String

engine = create_engine('sqlite:///example.db')
metadata = MetaData()

users = Table('users', metadata,
    Column('id', Integer, primary_key=True),
    Column('username', String, nullable=False),
    Column('email', String, unique=True)
)

metadata.create_all(engine)
```

**Insertion de données :**  
```python
from sqlalchemy import insert

with engine.connect() as connection:
    stmt = insert(users).values(username='alice', email='alice@example.com')
    connection.execute(stmt)
    connection.commit()
```

**Requête de sélection :**  
```python
from sqlalchemy import select

with engine.connect() as connection:
    stmt = select(users)
    result = connection.execute(stmt)
    for row in result:
        print(row)
```

---

**3. SQLAlchemy ORM** : approche orientée objet  

**a) Caractéristiques principales :**  
- Utilisation de classes Python pour représenter des tables.  
- Relations entre objets simplifiées (one-to-many, many-to-many).  
- Plus haut niveau d’abstraction, plus proche de la POO.  

**b) Exemple avec SQLAlchemy ORM :**  

**Déclaration d’un modèle ORM :**  
```python
from sqlalchemy.orm import declarative_base, sessionmaker
from sqlalchemy import Column, Integer, String

Base = declarative_base()

class User(Base):
    __tablename__ = 'users'
    
    id = Column(Integer, primary_key=True)
    username = Column(String, nullable=False)
    email = Column(String, unique=True)

engine = create_engine('sqlite:///example.db')
Base.metadata.create_all(engine)
```

**Création d’une session :**  
```python
Session = sessionmaker(bind=engine)
session = Session()
```

**Insertion de données :**  
```python
new_user = User(username='bob', email='bob@example.com')
session.add(new_user)
session.commit()
```

**Requête avec ORM :**  
```python
users = session.query(User).all()
for user in users:
    print(user.username, user.email)
```

---

**4. Différences principales entre SQLAlchemy Core et ORM**  

| Caractéristique         | SQLAlchemy Core         | SQLAlchemy ORM          |  
|------------------------|------------------------|------------------------|  
| Style                   | Procédural, proche du SQL | Orienté objet          |  
| Niveau d’abstraction    | Bas, contrôle direct   | Élevé, abstrait les requêtes SQL |  
| Performances           | Plus rapide pour des requêtes simples | Peut être moins performant pour des cas complexes |  
| Complexité des relations | Plus manuel            | Relations simplifiées  |  
| Flexibilité             | Très flexible          | Plus structuré         |  

---

**5. Quand utiliser quoi ?**  

- **Utiliser SQLAlchemy Core si :**  
  - Vous avez besoin d’un contrôle fin sur les requêtes SQL.  
  - Vous manipulez des bases de données très complexes.  
  - Vous privilégiez les performances sur de gros volumes de données.  

- **Utiliser SQLAlchemy ORM si :**  
  - Vous travaillez avec Python de manière orientée objet.  
  - Vous gérez beaucoup de relations entre tables.  
  - Vous souhaitez simplifier la maintenance et l’évolution du code.  

---

**6. Ressources complémentaires**  

- [Documentation SQLAlchemy](https://www.sqlalchemy.org/)  
- [Core Tutorial](https://docs.sqlalchemy.org/en/20/tutorial/index.html)  
- [ORM Tutorial](https://docs.sqlalchemy.org/en/20/orm/tutorial.html)  

---

Tu veux qu’on fasse un projet complet avec SQLAlchemy, genre une API Flask avec une base SQLite ou MySQL ? 🚀  