# 📌 **Explication complète de `git fetch`**

#### ✅ **Définition**
`git fetch` est une commande Git qui permet de **récupérer** les mises à jour d'un dépôt distant sans les fusionner automatiquement avec ta branche locale.

Contrairement à `git pull`, qui récupère **et fusionne** directement les modifications, `git fetch` te permet de voir ce qui a changé avant d'appliquer ces modifications.

---

### 🎯 **Pourquoi utiliser `git fetch` ?**
- Vérifier les changements distants sans modifier ton travail en cours.
- Éviter les conflits directs avec ton code local.
- Comparer les différences entre ta branche locale et la version distante avant de fusionner.

---

### 🔥 **Comment utiliser `git fetch` ?**
#### 🔹 **1. Récupérer toutes les branches mises à jour sur `origin`**
```bash
git fetch origin
```
Cela mettra à jour les références locales des branches distantes sans modifier tes fichiers locaux.

---

#### 🔹 **2. Voir quelles branches ont changé**
Après un `fetch`, tu peux voir les différences entre ta branche locale et la branche distante :
```bash
git status
```
ou encore :
```bash
git log origin/main --oneline
```
Cela te montre les derniers commits de `main` sur le dépôt distant.

---

#### 🔹 **3. Mettre à jour une branche locale avec `fetch` + `merge`**
Si tu veux récupérer les modifications d’une branche distante sans `git pull`, utilise :
```bash
git fetch origin
git merge origin/main
```
Cela revient à `git pull`, mais avec plus de contrôle.

---

#### 🔹 **4. Mettre à jour une branche locale avec `fetch` + `rebase`**
Si tu veux garder un historique propre :
```bash
git fetch origin
git rebase origin/main
```
Cela rejouera tes commits au-dessus des derniers changements de `main`.

---

### 🛠 **Cas d'utilisation de `git fetch`**
| Cas | Commande |
|------|-----------|
| Vérifier les mises à jour distantes sans impacter ta branche locale | `git fetch origin` |
| Comparer ta branche locale avec la distante | `git log main..origin/main --oneline` |
| Mettre à jour une branche locale sans `pull` | `git fetch origin && git merge origin/main` |
| Rebaser ta branche locale sur la version distante | `git fetch origin && git rebase origin/main` |

---

### 🚀 **Différence entre `git fetch` et `git pull`**
| Commande | Action |
|----------|--------|
| `git fetch` | Récupère les mises à jour distantes **sans modifier** ta branche locale |
| `git pull`  | Récupère **et fusionne** automatiquement les modifications distantes avec ta branche locale |

📌 **En résumé** :  
- `git fetch` est plus sûr et donne plus de contrôle.
- `git pull` est plus rapide mais peut provoquer des conflits inattendus.

**Bonne pratique** : Toujours faire un `git fetch` avant un `git pull` pour voir ce qui a changé avant d'appliquer des mises à jour ! 🚀