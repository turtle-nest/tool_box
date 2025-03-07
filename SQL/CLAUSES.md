# SQL : Les clauses `ON UPDATE` et `ON DELETE` (avec explications simples et exemples)**

En SQL, les clauses `ON UPDATE` et `ON DELETE` sont utilisées dans le cadre des contraintes de clé étrangère (`FOREIGN KEY`). Elles définissent le comportement à adopter lorsqu’une ligne référencée dans une table par une clé étrangère est modifiée ou supprimée. Ces clauses sont essentielles pour garantir l’intégrité des données dans des bases de données relationnelles.

---

## 1. **Contexte : Clé étrangère et relation entre tables**

Avant d’aborder ces clauses, rappelons rapidement le concept de **clé étrangère** :

- Une clé étrangère est une colonne (ou un ensemble de colonnes) dans une table qui fait référence à la **clé primaire** d'une autre table.
- Elle établit un **lien** entre deux tables afin de garantir la **cohérence** des données.

**Exemple de tables :**

```sql
CREATE TABLE Departments (
    id INT PRIMARY KEY,
    name VARCHAR(50)
);

CREATE TABLE Employees (
    id INT PRIMARY KEY,
    name VARCHAR(50),
    department_id INT,
    FOREIGN KEY (department_id) REFERENCES Departments(id)
);
```

Ici, la colonne `department_id` dans la table `Employees` est une **clé étrangère** qui fait référence à la colonne `id` de la table `Departments`.

---

## 2. **Clause `ON DELETE`**

La clause `ON DELETE` définit ce qui se passe lorsqu’une ligne référencée dans la table parent (ici `Departments`) est supprimée.

**Les options disponibles :**

- **`ON DELETE CASCADE`** : supprime automatiquement les lignes de la table enfant lorsque la ligne référencée dans la table parent est supprimée.
- **`ON DELETE SET NULL`** : remplace la clé étrangère par `NULL` dans la table enfant si la ligne parent est supprimée.
- **`ON DELETE RESTRICT`** : empêche la suppression de la ligne parent si des lignes dans la table enfant la référencent.
- **`ON DELETE NO ACTION`** : comportement par défaut, identique à `RESTRICT` dans la plupart des SGBD (empêche la suppression si des lignes sont liées).

**Exemple avec `CASCADE` :**

```sql
CREATE TABLE Employees (
    id INT PRIMARY KEY,
    name VARCHAR(50),
    department_id INT,
    FOREIGN KEY (department_id) REFERENCES Departments(id)
    ON DELETE CASCADE
);
```

**Comportement :**

```sql
-- Ajout de données
INSERT INTO Departments VALUES (1, 'HR');
INSERT INTO Employees VALUES (101, 'Alice', 1);

-- Suppression d'un département
DELETE FROM Departments WHERE id = 1;

-- Résultat : La ligne d'Alice est aussi supprimée automatiquement
SELECT * FROM Employees;
-- Table vide
```

---

## 3. **Clause `ON UPDATE`**

La clause `ON UPDATE` définit ce qui se passe lorsqu’une clé primaire référencée dans la table parent est **modifiée**.

**Les options disponibles :**

- **`ON UPDATE CASCADE`** : met à jour automatiquement les clés étrangères dans la table enfant lorsque la clé primaire est modifiée dans la table parent.
- **`ON UPDATE SET NULL`** : remplace la clé étrangère par `NULL` si la clé primaire de la table parent est modifiée.
- **`ON UPDATE RESTRICT`** : empêche la modification de la clé primaire si des lignes dans la table enfant la référencent.
- **`ON UPDATE NO ACTION`** : comportement par défaut, identique à `RESTRICT` dans la plupart des SGBD.

**Exemple avec `CASCADE` :**

```sql
CREATE TABLE Employees (
    id INT PRIMARY KEY,
    name VARCHAR(50),
    department_id INT,
    FOREIGN KEY (department_id) REFERENCES Departments(id)
    ON UPDATE CASCADE
);
```

**Comportement :**

```sql
-- Ajout de données
INSERT INTO Departments VALUES (1, 'HR');
INSERT INTO Employees VALUES (101, 'Alice', 1);

-- Mise à jour d'un département
UPDATE Departments SET id = 2 WHERE id = 1;

-- Résultat : La clé étrangère est mise à jour automatiquement
SELECT * FROM Employees;
-- Résultat : Alice a maintenant department_id = 2
```

---

## 4. **Combiner `ON UPDATE` et `ON DELETE`**

On peut utiliser ces deux clauses ensemble :

```sql
CREATE TABLE Employees (
    id INT PRIMARY KEY,
    name VARCHAR(50),
    department_id INT,
    FOREIGN KEY (department_id) REFERENCES Departments(id)
    ON DELETE CASCADE
    ON UPDATE CASCADE
);
```

---

## 5. **Résumé des comportements**

| Option              | `ON DELETE`                 | `ON UPDATE`                 |
|---------------------|----------------------------|----------------------------|
| **CASCADE**          | Supprime aussi les enfants | Met à jour les enfants      |
| **SET NULL**         | Met à `NULL` la clé étrangère | Met à `NULL` la clé étrangère |
| **RESTRICT**        | Empêche la suppression     | Empêche la mise à jour      |
| **NO ACTION**       | Identique à `RESTRICT`     | Identique à `RESTRICT`     |

---

**Cas pratiques :**
- **`CASCADE`** : lorsque la suppression ou modification d'un parent doit entraîner les mêmes changements dans la table enfant (ex : suppression d'un département -> suppression des employés liés).
- **`SET NULL`** : lorsque l'association avec le parent peut être perdue, mais l’enfant peut rester sans être lié à aucun parent.
- **`RESTRICT`** ou **`NO ACTION`** : pour éviter toute suppression ou modification si des dépendances existent.

---

En maîtrisant ces deux clauses, tu assures l'intégrité et la cohérence des relations entre les tables de ta base de données SQL ! 🚀