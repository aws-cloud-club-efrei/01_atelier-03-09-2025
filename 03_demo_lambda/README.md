# ⚡ Demo Lambda - Traitement automatique d'images

**Suite de la 01_demo_s3** - Cette fonction Lambda complète l'application Nike Store en ajoutant un traitement automatique des images uploadées.

## 🔗 Lien avec 01_demo_s3

Cette démo étend l'application Flask Nike Store (`01_demo_s3`) en ajoutant une couche de traitement automatique :

- L'utilisateur upload une image via l'interface web Nike Store
- La fonction Lambda se déclenche automatiquement
- Elle crée une copie horodatée dans le dossier `analyse/`

## 📋 Prérequis

- Application `01_demo_s3` déployée et fonctionnelle
- Bucket S3 configuré (ex: `e-commerce-bucket-acc-efrei`)
- Compte AWS avec accès à Lambda et S3

## 🛠️ Étapes de déploiement

### 1. Créer la fonction Lambda

1. **Aller dans AWS Lambda Console**

   - Connectez-vous à AWS Console
   - Recherchez "Lambda" dans les servi
   - Coiquez sur "Créer une fonction"

2. **Configuration de base**
   - **Nom de la fonction :** `e-commerce-lambda-function-acc-efrei`
   - **Runtime :** Python 3.9 ou plus récent
   - Cliquez sur "Créer une fonction"

### 2. Ajouter le code

1. **Dans la section "Code"**
   - Supprimez le code par défaut
   - Copiez-collez le contenu du fichier `lambda_function.py`
   - Cliquez sur "Deploy" pour sauvegarder

### 3. Configurer le trigger S3

1. **Ajouter un déclencheur**
   - Cliquez sur "Ajouter un déclencheur"
   - **Source :** S3
   - **Bucket :** Sélectionnez votre bucket `e-commerce-bucket-acc-efrei`
   - **Type d'événement :** Tous les événements de création d'objets
   - **Préfixe :** Laissez vide (pour traiter tous les fichiers)
   - **Suffixe :** Laissez vide
   - Cliquez sur "Ajouter"

### 4. Permissions

1. **Rôle d'exécution automatique**
   - AWS crée automatiquement un rôle avec les permissions de base
   - Laissez AWS gérer les permissions nécessaires
   - Le rôle créé contient déjà les permissions S3 requises pour cette fonction

## 🧪 Test de la fonction

### 1. Lancer l'application Nike Store (01_demo_s3)

```bash
cd ../01_demo_s3
python app.py
```

- Ouvrez http://127.0.0.1:3000
- Allez dans la section "Administration S3" du site Nike Store

### 2. Uploader une image

1. **Via l'interface web Nike Store**

   - Sélectionnez une image
   - Cliquez sur "Upload"
   - L'image sera uploadée dans votre bucket S3

2. **Déclenchement automatique**
   - La fonction Lambda se déclenche automatiquement
   - Elle crée une copie horodatée dans `analyse/`

### 3. Vérifier les logs

1. **Aller dans AWS Lambda Console**

   - Sélectionnez votre fonction `e-commerce-lambda-function-acc-efrei`
   - Cliquez sur l'onglet "Surveillance"
   - Cliquez sur "Afficher les journaux dans CloudWatch"

2. **Logs attendus**
   ```
   📁 Nouveau fichier détecté: nocta.png
   🔄 Copie: nocta.png → analyse/nocta_2025-08-11_18h30m45s.png
   ✅ Copie créée avec timestamp: analyse/nocta_2025-08-11_18h30m45s.png
   ```

### 4. Vérifier le résultat dans S3

1. **Aller dans AWS S3 Console**
   - Ouvrez votre bucket `e-commerce-bucket-acc-efrei`
   - Vérifiez la présence de l'image originale à la racine
   - Vérifiez la création du dossier `analyse/`
   - Dans `analyse/`, trouvez l'image avec le bon format de nommage

## 📊 Format de nommage

Les images copiées suivent ce format :

```
nom-original_YYYY-MM-DD_HHhMMmSSs.extension
```

**Exemple :**

- **Original :** `nocta.png`
- **Copie :** `nocta_2025-08-11_18h30m45s.png`

## 🔧 Dépannage

### Fonction ne se déclenche pas

- Vérifiez que le trigger S3 est bien configuré
- Vérifiez les permissions du rôle Lambda
- Uploadez une image via l'interface Nike Store (pas directement dans S3)

### Erreurs dans les logs

- Vérifiez les permissions S3 du rôle Lambda
- Assurez-vous que le bucket existe
- Vérifiez que le nom du bucket correspond à celui de 01_demo_s3

### Pas de dossier `analyse/` créé

- Vérifiez les logs CloudWatch pour voir les erreurs
- Assurez-vous que la fonction a les permissions d'écriture S3
- Testez avec une image Nike depuis l'interface web
