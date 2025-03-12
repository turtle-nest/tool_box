```markdown
# Supprimer une branche Git (localement et à distance)

Dans ce document, nous aborderons deux scénarios :  
1. **La suppression d’une branche localement**  
2. **La suppression d’une branche à distance**  

---

## 1. Supprimer une branche localement

### A. Vérifier la branche courante

Avant de supprimer une branche en local, il est préférable de vérifier la branche sur laquelle vous vous trouvez. Vous ne pouvez pas supprimer la branche sur laquelle vous êtes actuellement.

```bash
git branch
```

Cette commande liste toutes les branches locales.  
Assurez-vous de **ne pas** être sur la branche que vous souhaitez supprimer.  
Pour changer de branche :

```bash
git checkout <nom-de-branche>
```

### B. Supprimer la branche locale

Pour supprimer une branche localement, utilisez :

```bash
git branch -d <nom-de-branche>
```

- L’option `-d` (pour *--delete*) supprime uniquement la branche si tous les commits de celle-ci ont été fusionnés (*merged*) dans une autre branche.  
- Si vous voulez forcer la suppression (même si la branche n’a pas été fusionnée), utilisez :

```bash
git branch -D <nom-de-branche>
```

> **Attention :** La suppression forcée peut vous faire perdre définitivement des travaux s’ils ne sont pas fusionnés ou enregistrés ailleurs.

---

## 2. Supprimer une branche à distance

### A. Vérifier les branches distantes

Pour lister toutes les branches distantes :

```bash
git branch -r
```

### B. Supprimer la branche distante

Pour supprimer la branche dans votre dépôt distant (par exemple **origin**), vous pouvez utiliser :

#### Méthode moderne (recommandée)

```bash
git push origin --delete <nom-de-branche>
```

#### Ancienne syntaxe (toujours possible)

```bash
git push origin :<nom-de-branche>
```

La différence est purement syntaxique :  
- `git push origin --delete <nom-de-branche>` est plus explicite.  
- `git push origin :<nom-de-branche>` revient à pousser une branche vide vers la branche distante, provoquant sa suppression.

---

## 3. Bonnes pratiques et précautions

1. **Assurez-vous que la branche n’est plus nécessaire** : Vérifiez si tous les changements ont été fusionnés (merge) ou intégrés ailleurs.
2. **Conservez un historique si nécessaire** : Avant de supprimer, vous pouvez créer un tag ou un branchement temporaire si vous pensez en avoir besoin pour y revenir plus tard.
3. **Travailler à plusieurs ?** : Si vous collaborez sur un projet, communiquez avec les autres membres de l’équipe avant de supprimer une branche, afin d’éviter toute perte de travail.

---

## 4. Récapitulatif rapide

- **Supprimer en local** :  
  ```bash
  # Branchement sur une autre branche, puis:
  git branch -d <nom-de-branche>
  # ou forcé
  git branch -D <nom-de-branche>
  ```

- **Supprimer à distance** :  
  ```bash
  git push origin --delete <nom-de-branche>
  # ou ancienne syntaxe
  git push origin :<nom-de-branche>
  ```

---

> **Note** : Assurez-vous toujours d’effectuer un `git pull` et de consulter l’historique (`git log`, `git status`, etc.) avant de procéder à la suppression d’une branche, pour éviter toute perte de travail.
```