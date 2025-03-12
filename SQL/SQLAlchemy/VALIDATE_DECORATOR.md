Dans SQLAlchemy, la validation des attributs des entités peut être effectuée de plusieurs manières, notamment à l'aide de décorateurs comme `validates()`, qui permettent de valider les entrées avant leur enregistrement dans la base de données.

Voici les différentes méthodes de validation spécifiques que vous pouvez utiliser avec SQLAlchemy, comme indiqué dans les liens que vous avez partagés :

### 1. **Le décorateur `validates()`**
   Le décorateur `validates()` permet de définir des méthodes de validation qui seront appelées chaque fois qu'un attribut d'une instance d'un objet mappé est affecté. Cela vous permet de valider des données à la volée, avant leur enregistrement dans la base de données.

   Exemple d’utilisation du décorateur `validates()` :

   ```python
   from sqlalchemy.orm import validates

   class User(Base):
       __tablename__ = 'users'
       id = Column(Integer, primary_key=True)
       name = Column(String)
       age = Column(Integer)

       @validates('age')
       def validate_age(self, key, age):
           if age < 18:
               raise ValueError("Age must be 18 or older.")
           return age

       @validates('name')
       def validate_name(self, key, name):
           if len(name) < 3:
               raise ValueError("Name must be at least 3 characters long.")
           return name
   ```

   Dans cet exemple, chaque fois que l'attribut `age` ou `name` est modifié, les méthodes `validate_age()` et `validate_name()` sont appelées respectivement pour vérifier que l'âge est supérieur ou égal à 18 et que le nom a au moins 3 caractères.

### 2. **Les descripteurs et les hybrides**
   Les descripteurs et les hybrides sont utilisés pour effectuer des transformations plus complexes sur les attributs ou pour définir des calculs et des validations à la volée sur les propriétés des objets.

   - **Les descripteurs** sont des objets qui permettent de contrôler l'accès à un attribut d'une classe, ce qui peut être utile pour implémenter des logiques de validation ou de transformation des données avant de les stocker dans la base.
   
   - **Les hybrides** permettent de définir des propriétés calculées, qui peuvent être des expressions complexes ou des conditions, et d'effectuer des validations ou des calculs avant d'interagir avec la base de données.

   Exemple avec un hybride :

   ```python
   from sqlalchemy.ext.hybrid import hybrid_property
   from sqlalchemy.orm import validates

   class Product(Base):
       __tablename__ = 'products'
       id = Column(Integer, primary_key=True)
       _price = Column("price", Integer)

       @hybrid_property
       def price(self):
           return self._price

       @price.setter
       def price(self, value):
           if value < 0:
               raise ValueError("Price must be non-negative.")
           self._price = value
   ```

   Dans cet exemple, un hybride est utilisé pour gérer l'attribut `price` de manière contrôlée, en empêchant la mise à jour avec des valeurs négatives.

### 3. **Validations spécifiques avec des méthodes de classe**
   Vous pouvez également définir des méthodes de classe personnalisées pour valider plus finement certains aspects des objets ou des relations. Cela peut inclure la validation des attributs en fonction de conditions plus complexes ou de la logique métier.

En résumé, les types de validateurs que vous pouvez utiliser dans SQLAlchemy incluent :
- **Le décorateur `validates()`** pour valider des attributs individuels.
- **Les descripteurs et les hybrides** pour effectuer des validations et des transformations complexes.
- **Les méthodes de classe** pour implémenter des validations spécifiques à la logique métier.

Ces méthodes peuvent être combinées pour garantir que les données dans la base de données sont valides avant qu'elles ne soient insérées ou mises à jour.