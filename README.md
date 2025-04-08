### Objectif du Projet
Créer un pipeline dans Azure Data Factory qui copie les données depuis un fichier 
CSV (dans Azure Blob Storage) vers une table dans Azure SQL Database.

## 🧱 Étapes du projet :

# ✅ Étape 1 – Créer le service Azure Data Factory :

Aller dans le portail Azure.
Cliquer sur "Create a Resource", chercher Data Factory.
Remplir les champs :
  Nom de la Data Factory
  Subscription
  Resource Group
  Région
Lancer la création.
Une fois déployée, cliquer sur "Author & Monitor" pour ouvrir l’interface de création de pipelines.


# ✅ Étape 2 – Préparer les ressources de stockage et base de données :

Uploader le fichier cars.csv dans un container Azure Blob Storage.
Créer une table Cars dans Azure SQL Database avec cette structure :
 
  CREATE TABLE Cars (
  Make NVARCHAR(50),
  Model NVARCHAR(50),
  Type NVARCHAR(50),
  Origin NVARCHAR(50),
  DriveTrain NVARCHAR(50),
  Length FLOAT
);

S’assurer que le firewall du serveur SQL autorise les connexions de services Azure (sinon le pipeline échouera).



# ✅ Étape 3 – Créer les Linked Services :

Les connexions vers les sources/destinations de données.
Aller dans l’onglet Manage > Linked services.
Créer un Linked Service pour :
  Azure Blob Storage (avec account key ou SAS token)
  Azure SQL Database (avec login/mot de passe SQL)


# ✅ Étape 4 – Créer les Datasets : 
Représentation des données sources et cibles.
1- Dataset source (CSV dans Blob Storage):
Type : Azure Blob Storage > DelimitedText
Sélectionner le fichier cars.csv
Cocher "First row as header"
Lier au Linked Service Blob Storage

2- Dataset cible (table dans Azure SQL):
Type : Azure SQL Database
Sélectionner la table Cars
Lier au Linked Service SQL


# ✅ Étape 5 – Créer la pipeline :

Aller dans l’onglet Author > Pipelines > "New pipeline".
Donner un nom à la pipeline : ex. CopyCarsPipeline.


# ✅ Étape 6 – Ajouter une Copy Data Activity :
L’élément central du pipeline.

Faire glisser une Copy Data activity dans le canevas.
Dans les propriétés :
  Source : sélectionner le dataset Blob (CSV)
  Sink : sélectionner le dataset SQL
Dans l’onglet Mapping :
  Vérifier que les colonnes sont bien mappées automatiquement (sinon le faire manuellement).


# ✅ Étape 7 – Exécuter le pipeline :
Cliquer sur Debug pour tester immédiatement.
Aller dans l’onglet Monitor pour voir l’état d’exécution.
Vérifier que l’activité est marquée comme Success.

# ✅ Étape 8 – Vérifier les données dans la base SQL :
Ouvrir le portail SQL ou utiliser Azure Query Editor.
Lancer : SELECT * FROM Cars;
Vérifier que les lignes du fichier cars.csv ont bien été insérées dans la table.




## 🧠 Résultat :
✔ Le pipeline fonctionne. À chaque exécution :
  Il lit le fichier cars.csv dans Azure Blob.
  Il copie les données dans la table Cars de Azure SQL DB.
  Le tout sans écrire une seule ligne de code, via l’interface graphique d’ADF.





















