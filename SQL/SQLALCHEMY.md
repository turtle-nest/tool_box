# SQLAlchemy

SQLAlchemy est une bibliothèque Python open-source qui fournit un ensemble d'outils SQL et un mapper relationnel objet (ORM) pour interagir avec des bases de données relationnelles. Elle permet aux développeurs de travailler avec des bases de données en utilisant des objets Python, offrant ainsi un accès aux données à la fois efficace et flexible. citeturn0search25

L'API déclarative de SQLAlchemy simplifie la définition des modèles en combinant la définition des tables et le mapping des classes en une seule étape. Cette approche permet de définir des classes Python qui sont directement liées aux tables de la base de données, rendant le code plus lisible et maintenable.

**1. Configuration de base avec l'API déclarative**

Pour commencer à utiliser l'API déclarative, il est nécessaire de créer une classe de base à partir de laquelle toutes les classes mappées hériteront. Cette base est généralement créée en utilisant la fonction `declarative_base` :


```python
from sqlalchemy.ext.declarative import declarative_base

Base = declarative_base()
```


Toutes les classes mappées dériveront de cette classe `Base`. Chaque classe représente une table dans la base de données, et chaque attribut de la classe correspond à une colonne de la table.

**2. Définition d'une classe mappée**

Une fois la classe de base définie, vous pouvez créer des classes qui représentent les tables de votre base de données. Par exemple :


```python
from sqlalchemy import Column, Integer, String

class Utilisateur(Base):
    __tablename__ = 'utilisateurs'
    id = Column(Integer, primary_key=True)
    nom = Column(String)
    email = Column(String)
```


Dans cet exemple, la classe `Utilisateur` est mappée à la table `utilisateurs`. Les attributs `id`, `nom` et `email` correspondent aux colonnes de la table.

**3. Création d'une session et interaction avec la base de données**

Pour interagir avec la base de données, il est nécessaire de créer une session :


```python
from sqlalchemy import create_engine
from sqlalchemy.orm import sessionmaker

# Création du moteur
engine = create_engine('sqlite:///example.db')

# Création de la session
Session = sessionmaker(bind=engine)
session = Session()
```


La session permet d'ajouter, de supprimer et de requêter des objets dans la base de données.

**4. Ajout d'un nouvel enregistrement**

Pour ajouter un nouvel utilisateur à la base de données :


```python
nouvel_utilisateur = Utilisateur(nom='Jean Dupont', email='jean.dupont@example.com')
session.add(nouvel_utilisateur)
session.commit()
```


Cette opération ajoute l'utilisateur et enregistre les modifications dans la base de données.

**5. Requête des enregistrements**

Pour récupérer tous les utilisateurs :


```python
utilisateurs = session.query(Utilisateur).all()
for utilisateur in utilisateurs:
    print(utilisateur.nom, utilisateur.email)
```


Cette requête récupère tous les enregistrements de la table `utilisateurs` et affiche le nom et l'email de chaque utilisateur.

**6. Mise à jour d'un enregistrement**

Pour mettre à jour l'email d'un utilisateur spécifique :


```python
utilisateur = session.query(Utilisateur).filter_by(nom='Jean Dupont').first()
utilisateur.email = 'nouvel.email@example.com'
session.commit()
```


Cette opération modifie l'email de l'utilisateur et enregistre les modifications.

**7. Suppression d'un enregistrement**

Pour supprimer un utilisateur :


```python
utilisateur = session.query(Utilisateur).filter_by(nom='Jean Dupont').first()
session.delete(utilisateur)
session.commit()
```


Cette opération supprime l'utilisateur de la base de données.

En utilisant l'API déclarative de SQLAlchemy, il est possible de gérer efficacement les interactions avec la base de données en utilisant des classes et des objets Python, rendant le code plus intuitif et maintenable.