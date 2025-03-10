# Environenemnt virtuel python

Pour activer un environnement virtuel Python sur macOS (avec la puce Apple Silicon M3), voici les étapes :

1. **Ouvrir le terminal** : Lance le terminal sur ton Mac (tu peux le trouver via Spotlight ou dans Applications > Utilitaires).

2. **Créer un environnement virtuel** (si ce n’est pas déjà fait) :  
   Place-toi dans le dossier de ton projet :  
   ```bash
   cd /chemin/vers/ton/projet
   ```  
   Puis crée l'environnement virtuel (remplace "venv" par le nom que tu veux) :  
   ```bash
   python3 -m venv venv
   ```

3. **Activer l’environnement virtuel** :  
   Sur macOS (et Linux), utilise cette commande :  
   ```bash
   source venv/bin/activate
   ```

4. **Vérifier l’activation** :  
   Si tout va bien, tu devrais voir le nom de l'environnement (par exemple `venv`) s'afficher au début de la ligne de commande, comme ceci :  
   ```bash
   (venv) nom-de-ton-mac:ton-projet utilisateur$
   ```

5. **Installer des dépendances** (si besoin) :  
   Une fois l'environnement activé, tu peux installer des bibliothèques comme d'habitude avec `pip` :  
   ```bash
   pip install flask
   ```

6. **Désactiver l’environnement virtuel** :  
   Quand tu as fini, tu peux le désactiver avec :  
   ```bash
   deactivate
   ```

Dis-moi si tu rencontres des erreurs ou si tu veux aller plus loin avec les environnements virtuels ! 🚀🐍