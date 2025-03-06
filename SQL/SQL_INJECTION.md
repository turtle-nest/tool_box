### Cours Complet sur l'Injection SQL (SQL Injection)

#### Qu'est-ce que l'injection SQL ?

L'injection SQL (ou *SQL injection* en anglais) est une technique d'attaque utilisée par des attaquants pour exploiter des vulnérabilités dans une application web en insérant ou modifiant des requêtes SQL dans des champs d'entrée. L'objectif principal est de manipuler la base de données sous-jacente, de lire ou de modifier des informations sensibles, d'exécuter des commandes malveillantes ou de provoquer des comportements non souhaités sur l'application.

#### Comment l'injection SQL fonctionne-t-elle ?

Les applications web utilisent souvent des bases de données SQL pour stocker et récupérer des informations. Une application web qui ne protège pas correctement les champs d'entrée peut permettre à un attaquant de manipuler ces requêtes SQL en injectant des instructions SQL malveillantes dans les champs de formulaire, les URL, ou d'autres points d'entrée.

Voici un exemple basique de ce à quoi peut ressembler une vulnérabilité d'injection SQL :

1. L'application prend un champ d'entrée utilisateur (par exemple, un formulaire de connexion).
2. L'input utilisateur est utilisé pour construire une requête SQL qui récupère les informations nécessaires.
   
   Exemple de requête vulnérable :
   ```sql
   SELECT * FROM users WHERE username = '$username' AND password = '$password';
   ```
   
   Si l'utilisateur entre des valeurs comme suit :
   - `username = 'admin'`
   - `password = 'password123'`
   
   La requête générée serait :
   ```sql
   SELECT * FROM users WHERE username = 'admin' AND password = 'password123';
   ```

   Cependant, si l'attaquant entre un input malveillant tel que :
   - `username = 'admin' OR 1=1 --`
   - `password = 'anything'`
   
   La requête deviendrait :
   ```sql
   SELECT * FROM users WHERE username = 'admin' OR 1=1 -- AND password = 'anything';
   ```
   Ici, `1=1` est toujours vrai, ce qui contourne l'authentification et permet à l'attaquant d'accéder à l'application sans connaître le mot de passe.

#### Types d'injections SQL

Il existe plusieurs types d'injections SQL, dont voici les principaux :

1. **Injection SQL Basique :**
   Cela consiste à manipuler des requêtes SQL via des entrées utilisateur pour en modifier le comportement.
   - Exemple : `SELECT * FROM users WHERE username = 'admin' OR 1=1;`

2. **Injection d'Autorisation (ou d'authentification) :**
   Cela permet à un attaquant de contourner les mécanismes d'authentification d'une application en manipulant les requêtes de connexion.
   
3. **Injection de Union :**
   L'attaquant utilise l'instruction `UNION` pour combiner plusieurs requêtes SELECT en une seule. Cela peut permettre d'extraire des données d'autres tables que celles auxquelles l'utilisateur est normalement autorisé à accéder.
   - Exemple :
     ```sql
     SELECT name, password FROM users WHERE username = 'admin' UNION SELECT email, credit_card_number FROM customers;
     ```

4. **Injection d'Erreur (Error-based SQL Injection) :**
   Cette méthode repose sur l'exploitation des messages d'erreur retournés par la base de données. Si la base de données retourne des messages d'erreur détaillés (par exemple, la structure des tables ou des informations sur les colonnes), l'attaquant peut exploiter cette information pour découvrir des détails sensibles.
   
5. **Injection Blind :**
   L'injection SQL "blind" (aveugle) est utilisée lorsqu'un attaquant ne peut pas voir les messages d'erreur. Cependant, il peut observer les changements dans le comportement de l'application pour en déduire des informations. Par exemple, l'attaquant peut poser une série de questions booléennes, comme :
   ```sql
   SELECT * FROM users WHERE username = 'admin' AND 1=1;
   SELECT * FROM users WHERE username = 'admin' AND 1=2;
   ```

6. **Injection de temps (Time-based Blind Injection) :**
   Dans ce cas, l'attaquant envoie une requête SQL qui provoque un délai dans la réponse de la base de données, et l'attaquant déduit des informations en fonction de la durée de la réponse.

#### Prévention des attaques par injection SQL

1. **Utiliser des requêtes préparées (Prepared Statements) :**
   Les requêtes préparées sont une méthode sûre pour interagir avec la base de données. Elles séparent la logique SQL de l'entrée utilisateur, ce qui empêche l'injection SQL.
   Exemple avec MySQL en PHP :
   ```php
   $stmt = $mysqli->prepare("SELECT * FROM users WHERE username = ? AND password = ?");
   $stmt->bind_param("ss", $username, $password);
   $stmt->execute();
   ```

2. **Utiliser des ORM (Object-Relational Mapping) :**
   Les ORM, comme SQLAlchemy pour Python ou Hibernate pour Java, sont conçus pour éviter les injections SQL en utilisant des API qui échappent automatiquement les données et construisent des requêtes sécurisées.

3. **Valider et nettoyer les entrées utilisateurs :**
   Une autre méthode consiste à valider les entrées des utilisateurs et à ne permettre que des valeurs spécifiques, comme des chiffres ou des chaînes de texte limitées. Cela permet de réduire les risques d'injection.

4. **Éviter les messages d'erreur détaillés :**
   Ne laissez pas la base de données ou l'application afficher des messages d'erreur détaillés qui pourraient aider un attaquant à comprendre la structure de la base de données ou des requêtes.

5. **Principe du moindre privilège :**
   L'utilisateur de la base de données doit avoir uniquement les privilèges nécessaires pour exécuter ses tâches. Un utilisateur avec des privilèges limités limite l'impact d'une attaque SQL.

6. **Utiliser des outils de détection d'injection SQL :**
   Il existe plusieurs outils de sécurité, comme OWASP ZAP ou SQLmap, qui peuvent détecter les vulnérabilités d'injection SQL dans une application.

#### Conclusion

L'injection SQL est une vulnérabilité dangereuse qui peut avoir des conséquences graves sur la sécurité des applications web. Il est essentiel pour les développeurs de prendre des mesures proactives pour éviter ce type d'attaque, notamment en utilisant des requêtes préparées, en validant et en nettoyant les entrées, et en appliquant des principes de sécurité tels que le moindre privilège.

La prévention de l'injection SQL nécessite une combinaison de bonnes pratiques de codage, de tests de sécurité et de configuration correcte des bases de données et des applications.