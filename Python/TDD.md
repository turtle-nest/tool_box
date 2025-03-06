# Cours Complet sur le Test-Driven Development (TDD) en Python

Le **Test-Driven Development (TDD)** est une approche de développement logiciel où les tests sont écrits avant le code de production. Le but est de garantir que le code fonctionne comme prévu tout en réduisant les risques de régressions. Cette méthode s'inscrit dans une démarche d'amélioration continue du code et de sa qualité.

#### 1. **Qu’est-ce qu’un test interactif ?**

Un **test interactif** est un moyen rapide de tester une fonction ou un module en utilisant directement un interpréteur interactif (comme Python dans un terminal). Cela permet d'exécuter des bouts de code de manière simple sans avoir à configurer un fichier de test complet.

##### Exemple d’utilisation d’un test interactif :

```python
>>> def addition(a, b):
...     return a + b
...
>>> addition(2, 3)
5
```

Les tests interactifs sont utiles pour des validations rapides, mais ne remplacent pas des tests formels automatisés. Ils ne sont pas reproductibles et ne garantissent pas une couverture complète des cas d’usage.

#### 2. **Pourquoi les tests sont-ils importants ?**

Les tests ont plusieurs avantages cruciaux dans le développement :

- **Prévention des régressions** : Ils garantissent que les modifications du code n'introduisent pas de nouveaux bugs.
- **Qualité du code** : Ils encouragent un code bien conçu, avec une anticipation des cas d'erreur et des comportements limites.
- **Documentation vivante** : Les tests automatisés servent de documentation de l'utilisation du code.
- **Confiance** : Ils permettent de déployer du code avec assurance, même dans des projets complexes.

#### 3. **Comment écrire des Docstrings pour créer des tests ?**

Les **Docstrings** sont des chaînes de documentation attachées aux fonctions, classes ou modules pour décrire leur fonctionnement. Elles peuvent aussi inclure des exemples de tests interactifs. Cela permet d’avoir une documentation vivante qui sert aussi de test.

##### Exemple de Docstring avec des tests interactifs :

```python
def multiplication(a, b):
    """
    Multiplie deux nombres.

    Exemple :
    >>> multiplication(3, 4)
    12
    >>> multiplication(-2, 5)
    -10
    """
    return a * b
```

Pour exécuter les tests contenus dans les Docstrings, on utilise le module `doctest` de Python :

```bash
python -m doctest -v votre_fichier.py
```

#### 4. **Comment écrire de la documentation pour chaque module et fonction ?**

La documentation joue un rôle essentiel dans le code. Elle doit être claire et précise pour permettre à d'autres développeurs de comprendre le fonctionnement d'un module ou d'une fonction.

##### 1. **Module :**
Un module doit commencer par une **Docstring** qui décrit son objectif global.

```python
"""
Ce module contient des fonctions mathématiques de base :
- addition
- soustraction
- multiplication
"""
```

##### 2. **Fonction :**
Chaque fonction doit avoir une **Docstring** qui décrit :
- Ce qu’elle fait.
- Les arguments attendus (et leurs types).
- Ce qu’elle retourne.
- Les exceptions potentielles.
- Des exemples d’utilisation.

##### Exemple de Docstring pour une fonction de division :

```python
def division(a, b):
    """
    Divise un nombre par un autre.

    Arguments :
    a (int, float) : Le dividende
    b (int, float) : Le diviseur (doit être non nul)

    Retourne :
    float : Le résultat de la division.

    Exceptions :
    ValueError : Si b est égal à 0.

    Exemple :
    >>> division(10, 2)
    5.0
    >>> division(5, 0)
    Traceback (most recent call last):
    ...
    ValueError: Le diviseur ne peut pas être 0.
    """
    if b == 0:
        raise ValueError("Le diviseur ne peut pas être 0.")
    return a / b
```

#### 5. **Options de base pour créer des tests avec `unittest`**

Le module `unittest` de Python est un outil puissant pour écrire et exécuter des tests unitaires. Voici quelques concepts de base pour créer des tests automatisés.

##### 1. **Créer une classe de test** :
Une classe de test hérite de `unittest.TestCase` et contient des méthodes de test.

##### Exemple de classe de test :

```python
import unittest

class TestMathFunctions(unittest.TestCase):
    def test_addition(self):
        self.assertEqual(addition(2, 3), 5)

    def test_division(self):
        with self.assertRaises(ValueError):
            division(10, 0)

if __name__ == "__main__":
    unittest.main()
```

##### 2. **Méthodes standards** :
- `setUp()` : Prépare l'environnement avant chaque test (par exemple, initialiser des objets nécessaires).
- `tearDown()` : Nettoie après chaque test.

##### Exécution des tests :
```bash
python -m unittest fichier_test.py
```

#### 6. **Comment trouver les cas limites (edge cases) ?**

Les **cas limites** sont des situations où une fonction pourrait échouer ou ne pas se comporter comme prévu. Identifier ces cas est essentiel pour tester la robustesse de votre code.

##### Comment identifier les cas limites ?
- **Analyse des entrées** : Tester des valeurs extrêmes comme des nombres négatifs, zéro, des très grands nombres, ou des chaînes vides.
- **Scénarios spéciaux** : Tester des situations avec des entrées non valides, des listes vides ou des dictionnaires sans clés.

##### Exemple de fonction avec un cas limite pour vérifier si un mot est un palindrome :

```python
def is_palindrome(word):
    """
    Vérifie si un mot est un palindrome.

    Cas limites :
    - Chaîne vide
    - Chaîne avec des espaces ou caractères spéciaux
    """
    word = ''.join(c.lower() for c in word if c.isalnum())
    return word == word[::-1]
```

Les cas limites à tester pourraient être :
- Des mots vides.
- Des mots contenant des espaces ou des caractères spéciaux.
- Des mots avec des majuscules et minuscules mélangées.

#### 7. **Les différentes approches de tests dans TDD**

Dans TDD, vous suivez souvent un cycle appelé **Red-Green-Refactor** :
1. **Red** : Écrire un test qui échoue.
2. **Green** : Faire en sorte que le test réussisse en écrivant du code.
3. **Refactor** : Améliorer le code tout en s'assurant que les tests réussissent toujours.

### Conclusion

Le Test-Driven Development est une méthode très efficace pour garantir la qualité du code. Grâce à l'utilisation des tests automatisés, vous pouvez améliorer la robustesse de votre logiciel, éviter les régressions et faciliter sa maintenance. Les Docstrings et le module `unittest` sont des outils essentiels pour écrire des tests clairs et compréhensibles, et les cas limites permettent de tester les scénarios extrêmes qui pourraient faire échouer votre code.