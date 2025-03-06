# Cours Complet sur l'utilisation de `unittest` en Python

Le module `unittest` de Python est un framework de test unitaire intégré, permettant d'écrire des tests automatisés pour vérifier le comportement de votre code. L'objectif principal des tests unitaires est de valider que chaque composant (fonction, méthode ou classe) fonctionne comme prévu dans tous les cas, y compris les cas limites et les erreurs possibles.

#### 1. **Introduction au module `unittest`**

Le module `unittest` est un framework qui suit le style de test **xUnit**, un ensemble de pratiques populaires pour écrire des tests dans divers langages de programmation. Il fournit une structure pour organiser et exécuter des tests, ainsi que des outils pour vérifier que le code fonctionne correctement.

#### 2. **Principaux concepts de `unittest`**

Voici les éléments clés du module `unittest` :

- **TestCase** : La classe de base pour écrire un test. Elle contient des méthodes de test, que l’on exécute pour vérifier la validité du code testé.
- **Assertions** : Des méthodes qui vérifient si le résultat du test correspond aux attentes.
- **Test suite** : Un ensemble de tests regroupés.
- **Test runner** : Un outil pour exécuter les tests et afficher les résultats.

#### 3. **Écrire un Test avec `unittest`**

##### 3.1. **Créer une classe de test**

Chaque test unitaire doit être contenu dans une classe qui hérite de `unittest.TestCase`. Cette classe contiendra des méthodes qui testent des comportements spécifiques de votre code.

##### Exemple de base :

```python
import unittest

def addition(a, b):
    return a + b

class TestMathFunctions(unittest.TestCase):

    def test_addition(self):
        result = addition(2, 3)
        self.assertEqual(result, 5)

if __name__ == "__main__":
    unittest.main()
```

- La classe `TestMathFunctions` hérite de `unittest.TestCase`.
- La méthode `test_addition` vérifie que la fonction `addition(2, 3)` renvoie bien `5` avec l'assertion `self.assertEqual`.

##### 3.2. **Assertions dans `unittest`**

Les assertions sont des méthodes qui permettent de vérifier si un test réussit ou échoue.

Quelques assertions courantes :

- `assertEqual(a, b)` : Vérifie que `a == b`.
- `assertNotEqual(a, b)` : Vérifie que `a != b`.
- `assertTrue(x)` : Vérifie que `x` est vrai.
- `assertFalse(x)` : Vérifie que `x` est faux.
- `assertIsNone(x)` : Vérifie que `x` est `None`.
- `assertIsNotNone(x)` : Vérifie que `x` n'est pas `None`.
- `assertRaises(Exception)` : Vérifie qu’une exception spécifique est levée.

##### Exemple d'assertions multiples :

```python
class TestMathFunctions(unittest.TestCase):

    def test_addition(self):
        self.assertEqual(addition(2, 3), 5)

    def test_negative_addition(self):
        self.assertEqual(addition(-2, -3), -5)

    def test_addition_with_zero(self):
        self.assertEqual(addition(2, 0), 2)

    def test_division_by_zero(self):
        with self.assertRaises(ZeroDivisionError):
            1 / 0
```

#### 4. **Utilisation des méthodes `setUp()` et `tearDown()`**

Les méthodes `setUp()` et `tearDown()` sont utilisées pour préparer et nettoyer le contexte de vos tests. Ces méthodes sont appelées avant et après chaque méthode de test.

- **`setUp()`** : Prépare l'environnement avant l'exécution de chaque test (par exemple, initialiser des objets ou des variables).
- **`tearDown()`** : Nettoie après chaque test (par exemple, fermer des fichiers ou des connexions réseau).

##### Exemple :

```python
import unittest

class TestMathFunctions(unittest.TestCase):

    def setUp(self):
        self.a = 5
        self.b = 3

    def tearDown(self):
        # Actions de nettoyage après chaque test
        pass

    def test_addition(self):
        self.assertEqual(self.a + self.b, 8)

    def test_subtraction(self):
        self.assertEqual(self.a - self.b, 2)
```

#### 5. **Créer une suite de tests (Test Suite)**

Une **suite de tests** est un groupe de tests que l’on peut exécuter ensemble. Cela permet de regrouper plusieurs classes de tests ou méthodes pour les exécuter en une seule fois.

##### Exemple de création d'une suite de tests :

```python
def suite():
    suite = unittest.TestSuite()
    suite.addTest(TestMathFunctions('test_addition'))
    suite.addTest(TestMathFunctions('test_subtraction'))
    return suite

if __name__ == "__main__":
    runner = unittest.TextTestRunner()
    runner.run(suite())
```

#### 6. **Exécution des tests**

Les tests peuvent être exécutés de différentes manières :

- **Via la ligne de commande** : Utilisez la commande suivante pour exécuter un fichier de test Python contenant des tests.
  
  ```bash
  python -m unittest votre_fichier.py
  ```

- **Dans le code** : Vous pouvez exécuter les tests depuis le code en utilisant `unittest.main()`.

  ```python
  if __name__ == "__main__":
      unittest.main()
  ```

- **Avec un Test Runner** : Vous pouvez utiliser un runner pour exécuter des suites de tests. Un runner génère des rapports sur les tests passés ou échoués.

#### 7. **Exemple complet avec assertions et `setUp()`**

Voici un exemple complet d'utilisation de `unittest` avec plusieurs tests, assertions, et une préparation d'environnement avec `setUp()`.

```python
import unittest

def addition(a, b):
    return a + b

def division(a, b):
    if b == 0:
        raise ValueError("Diviseur ne peut pas être 0.")
    return a / b

class TestMathFunctions(unittest.TestCase):

    def setUp(self):
        """Préparation de l'environnement avant chaque test"""
        self.a = 10
        self.b = 2

    def tearDown(self):
        """Nettoyage après chaque test"""
        pass

    def test_addition(self):
        """Test de la fonction d'addition"""
        self.assertEqual(addition(self.a, self.b), 12)

    def test_division(self):
        """Test de la fonction de division"""
        self.assertEqual(division(self.a, self.b), 5.0)

    def test_division_by_zero(self):
        """Vérifie que la division par zéro lève une exception"""
        with self.assertRaises(ValueError):
            division(self.a, 0)

if __name__ == "__main__":
    unittest.main()
```

#### 8. **Test de méthodes d'objets**

Si vous travaillez avec des classes et des objets, vous pouvez tester des méthodes spécifiques de ces objets. Voici un exemple avec une classe `Calculator` :

```python
class Calculator:
    def add(self, a, b):
        return a + b

    def subtract(self, a, b):
        return a - b

class TestCalculator(unittest.TestCase):

    def setUp(self):
        self.calc = Calculator()

    def test_add(self):
        self.assertEqual(self.calc.add(2, 3), 5)

    def test_subtract(self):
        self.assertEqual(self.calc.subtract(5, 3), 2)

if __name__ == "__main__":
    unittest.main()
```

#### 9. **Conclusion**

Le module `unittest` est un outil puissant et flexible pour automatiser les tests dans Python. Grâce à des fonctionnalités comme les assertions, les méthodes `setUp()` et `tearDown()`, et la gestion des suites de tests, il permet de garantir la qualité du code et d'éviter les régressions. Les tests unitaires vous offrent une plus grande confiance dans la stabilité de votre code tout en facilitant sa maintenance à long terme.