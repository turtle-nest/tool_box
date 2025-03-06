Bien sûr ! Voici la version mise à jour avec l'ajout de l'annulation du dernier commit :

---

# Guide de Manipulation de Git et des Commits

Ce guide fournit une liste de commandes Git utiles pour la gestion des versions de votre projet. Git est un outil de contrôle de version distribué qui permet de suivre les modifications de votre code et de collaborer avec d'autres développeurs.

## Initialiser un dépôt Git

Avant de commencer à utiliser Git, il est nécessaire d'initialiser un dépôt Git dans votre projet.

```bash
git init
```

Cette commande crée un nouveau dépôt Git local dans le répertoire courant.

## Ajouter des fichiers au suivi de Git

Pour ajouter des fichiers à Git afin qu'ils soient suivis et enregistrés dans l'historique des versions, utilisez :

```bash
git add <file>
```

Pour ajouter tous les fichiers modifiés (ou nouveaux fichiers) dans le répertoire courant :

```bash
git add .
```

## Créer un commit

Un commit est un enregistrement des modifications. Après avoir ajouté vos fichiers avec `git add`, vous pouvez créer un commit avec :

```bash
git commit -m "Message du commit"
```

**Exemple** :

```bash
git commit -m "Ajout de la fonctionnalité de connexion utilisateur"
```

## Afficher l'état du dépôt

Pour vérifier l'état de votre dépôt, les fichiers modifiés, ou ceux qui sont prêts pour le commit :

```bash
git status
```

## Voir l'historique des commits

Pour afficher l'historique des commits dans votre dépôt :

```bash
git log
```

Vous pouvez également utiliser des options comme `--oneline` pour afficher un résumé des commits dans une ligne :

```bash
git log --oneline
```

## Revenir à un commit précédent

Si vous souhaitez annuler les modifications depuis un commit précédent, utilisez :

```bash
git checkout <commit_hash>
```

Vous pouvez aussi revenir à un commit spécifique sans changer votre historique de commits :

```bash
git reset --hard <commit_hash>
```

## Annuler le dernier commit

Si vous souhaitez annuler le dernier commit (sans supprimer les modifications locales), utilisez :

```bash
git reset --soft HEAD~1
```

Cette commande annule le dernier commit, mais garde les modifications dans votre espace de travail, vous permettant de les modifier et de refaire un commit si nécessaire.

Si vous souhaitez annuler le dernier commit **et** les modifications dans votre espace de travail, utilisez :

```bash
git reset --hard HEAD~1
```

Cela supprimera à la fois le dernier commit et toutes les modifications non validées.

## Gérer des branches

Git permet de travailler sur différentes branches. Pour créer une nouvelle branche et basculer dessus :

```bash
git checkout -b <branch_name>
```

Pour basculer sur une branche existante :

```bash
git checkout <branch_name>
```

## Fusionner des branches (Merge)

Pour fusionner une branche dans votre branche actuelle, vous devez d'abord vous assurer que vous êtes sur la branche cible (par exemple, `main` ou `develop`), puis :

```bash
git merge <branch_name>
```

## Pousser des commits vers un dépôt distant

Pour pousser vos commits vers un dépôt distant (par exemple, GitHub), utilisez :

```bash
git push origin <branch_name>
```

**Exemple** :

```bash
git push origin main
```

## Récupérer les changements depuis un dépôt distant

Pour récupérer les changements d'un dépôt distant et les fusionner avec votre branche locale :

```bash
git pull origin <branch_name>
```

**Exemple** :

```bash
git pull origin main
```

## Cloner un dépôt distant

Pour cloner un dépôt Git distant (par exemple, un projet GitHub) :

```bash
git clone <repository_url>
```

**Exemple** :

```bash
git clone https://github.com/utilisateur/mon-projet.git
```

## Résoudre les conflits de fusion

Lors de la fusion de branches, des conflits peuvent survenir si Git ne parvient pas à fusionner automatiquement les fichiers. Dans ce cas :

1. Git vous indiquera les fichiers en conflit.
2. Vous devez ouvrir ces fichiers, résoudre les conflits (en choisissant ou modifiant le code) et enregistrer les changements.
3. Après avoir résolu les conflits, ajoutez à nouveau les fichiers avec `git add` et validez avec un commit.

## Revertir un commit

Si vous voulez annuler un commit spécifique (sans effacer les changements dans les fichiers), utilisez la commande suivante :

```bash
git revert <commit_hash>
```

Cela crée un nouveau commit qui inverse les modifications du commit spécifié.

## Annuler des modifications locales

Si vous avez modifié un fichier, mais souhaitez annuler ces changements avant de les commettre, utilisez :

```bash
git checkout -- <file>
```

Cela rétablit le fichier à sa dernière version validée (commitée).

## Nettoyer les fichiers non suivis

Si vous avez des fichiers non suivis (non ajoutés avec `git add`), vous pouvez les supprimer en utilisant :

```bash
git clean -f
```

### Commandes utiles supplémentaires

- **Afficher les différences** : `git diff` – Affiche les différences entre les fichiers modifiés et la dernière version commitée.
- **Supprimer une branche locale** : `git branch -d <branch_name>` – Supprime une branche locale après la fusion.
- **Vérifier la configuration de Git** : `git config --list` – Affiche la configuration de Git, comme le nom et l'email.

## Conclusion

Ce guide couvre les commandes Git de base nécessaires pour gérer les versions de votre projet. Utilisez-les pour suivre vos modifications, collaborer avec d'autres développeurs et maintenir un historique propre de votre code. Pour en savoir plus, consultez la documentation officielle de Git [ici](https://git-scm.com/doc).

---

J'ai ajouté les sections pour annuler le dernier commit en précisant les commandes `git reset --soft HEAD~1` et `git reset --hard HEAD~1`. Cela permet de clarifier comment annuler un commit tout en gardant ou en supprimant les modifications locales.