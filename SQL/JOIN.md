# Les JOINS en SQL : Guide complet

Les JOINS en SQL permettent de combiner les données de plusieurs tables basées sur une ou plusieurs colonnes communes. Ils sont essentiels pour exploiter la puissance des bases de données relationnelles.

### 1. Les différents types de JOINS

#### a) INNER JOIN
L'INNER JOIN ne renvoie que les lignes qui ont des correspondances dans les deux tables.

Exemple :
```sql
SELECT utilisateurs.nom, commandes.produit
FROM utilisateurs
INNER JOIN commandes
ON utilisateurs.id = commandes.utilisateur_id;
```

#### b) LEFT JOIN
Le LEFT JOIN renvoie toutes les lignes de la table de gauche et les correspondances de la table de droite. Si aucune correspondance n'est trouvée, les valeurs sont NULL.

Exemple :
```sql
SELECT utilisateurs.nom, commandes.produit
FROM utilisateurs
LEFT JOIN commandes
ON utilisateurs.id = commandes.utilisateur_id;
```

#### c) RIGHT JOIN
Le RIGHT JOIN est l'inverse du LEFT JOIN : il renvoie toutes les lignes de la table de droite et les correspondances de la table de gauche.

Exemple :
```sql
SELECT utilisateurs.nom, commandes.produit
FROM utilisateurs
RIGHT JOIN commandes
ON utilisateurs.id = commandes.utilisateur_id;
```

#### d) FULL OUTER JOIN
Le FULL OUTER JOIN renvoie toutes les lignes des deux tables, avec des valeurs NULL si aucune correspondance n'est trouvée. Attention : MySQL ne supporte pas directement le FULL OUTER JOIN.

Solution avec UNION :
```sql
SELECT utilisateurs.nom, commandes.produit
FROM utilisateurs
LEFT JOIN commandes
ON utilisateurs.id = commandes.utilisateur_id
UNION
SELECT utilisateurs.nom, commandes.produit
FROM utilisateurs
RIGHT JOIN commandes
ON utilisateurs.id = commandes.utilisateur_id;
```

### 2. Utilisation avancée des JOINS

#### a) JOIN avec plusieurs conditions
```sql
SELECT utilisateurs.nom, commandes.produit
FROM utilisateurs
JOIN commandes
ON utilisateurs.id = commandes.utilisateur_id AND commandes.date >= '2024-01-01';
```

#### b) JOIN avec des alias
```sql
SELECT u.nom, c.produit
FROM utilisateurs AS u
JOIN commandes AS c
ON u.id = c.utilisateur_id;
```

#### c) JOIN de plus de deux tables
```sql
SELECT u.nom, c.produit, p.prix
FROM utilisateurs AS u
JOIN commandes AS c
ON u.id = c.utilisateur_id
JOIN produits AS p
ON c.produit_id = p.id;
```

### 3. Conclusion
Les JOINS sont des outils puissants pour manipuler et analyser des bases de données relationnelles. Bien comprendre leurs différences et leur utilisation avancée vous permettra d'exploiter pleinement MySQL et SQL.

