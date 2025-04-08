# 📊 Azure Data Factory – ETL de fichier CSV vers Azure SQL Database

Ce projet montre comment configurer un pipeline ETL simple avec **Azure Data Factory (ADF)** pour transférer des données depuis un fichier CSV stocké dans **Azure Blob Storage** vers une table dans **Azure SQL Database**, le tout via l'interface graphique d'Azure sans écrire de code.

---

## 🧭 Objectif

Créer un pipeline dans ADF qui :
- Extrait des données à partir d’un fichier CSV dans Azure Blob Storage
- Les charge dans une table relationnelle `Cars` dans Azure SQL
- Le tout en utilisant **Linked Services**, **Datasets** et une **Copy Activity**

---

## 📌 Vue d’ensemble du pipeline

![Diagramme complet du pipeline](screenshots/Capture d'écran 2025-04-08 155448.png)

Ce diagramme illustre l’architecture globale du pipeline :
- Source : Blob Storage contenant `Cars.csv`
- Activité de copie orchestrée par ADF
- Destination : table `Cars` dans Azure SQL

---

## 🧱 Étapes du projet

### 1. Création du service Azure Data Factory

- Crée une ressource ADF depuis le portail Azure
- Remplis les champs : nom, groupe de ressources, région
- Clique sur **Author & Monitor** pour accéder à l'interface graphique

---

### 2. Préparation des données sources et cibles

- Téléverse le fichier `cars.csv` dans un conteneur Azure Blob Storage
- Crée la table cible dans Azure SQL Database avec ce script :
sql
CREATE TABLE Cars (
  Make NVARCHAR(50),
  Model NVARCHAR(50),
  Type NVARCHAR(50),
  Origin NVARCHAR(50),
  DriveTrain NVARCHAR(50),
  Length FLOAT
);


### 3. 🔗 Création des Linked Services

Les **Linked Services** sont des connexions sécurisées vers les sources et destinations de données.

- 🔐 Crée un Linked Service vers **Azure Blob Storage**
  - Méthode d’authentification : Account Key ou SAS Token
- 🔐 Crée un Linked Service vers **Azure SQL Database**
  - Méthode d’authentification : SQL Login (utilisateur + mot de passe)
  - Assure-toi que le firewall du serveur SQL autorise les services Azure

---

### 4. 📦 Définition des Datasets

Les **datasets** représentent les données manipulées dans le pipeline.

- Dataset source :
  - Type : Azure Blob Storage – DelimitedText (CSV)
  - Fichier : `cars.csv`
  - Option cochée : *First row as header*
- Dataset destination :
  - Type : Azure SQL Database
  - Table cible : `Cars`

---

### 5. 🛠️ Création du pipeline et configuration de la Copy Activity

- Crée un **pipeline** (nommé par ex. `CopyCarsPipeline`)
- Ajoute une **Copy Data Activity**
- Sélectionne :
  - Le dataset source (`cars.csv`)
  - Le dataset cible (`Cars`)
- Va dans l’onglet **Mapping** pour vérifier que les colonnes sont bien mappées (automatiquement ou manuellement)

📷 **Exemple avancé : plusieurs activités de copie dans un pipeline**

![Exemple avancé avec plusieurs Copy Activities](screenshots/Capture d'écran 2025-04-08 155519.png)

---

### 6. ▶️ Exécution et suivi du pipeline

- Lance un **Debug** dans l’interface d’ADF
- Ouvre l’onglet **Monitor** pour :
  - Suivre l’état d’exécution
  - Vérifier que l’activité est bien en *Success*
  - Obtenir le nombre de lignes transférées

---

### 7. 🧾 Vérification des données dans Azure SQL Database

- Connecte-toi à ton Azure SQL Database
- Exécute la requête suivante pour voir les données importées :
sql
SELECT * FROM Cars;

![Résultat d'importation dans SQL](screenshots/Capture d'écran 2025-04-08 155642.png)



### 🧠 Résultat attendu

📷 **Diagramme illustrant l’architecture complète du pipeline :**

![Pipeline complet : CSV → SQL](screenshots/Capture d'écran 2025-04-08 155448.png)

Ce projet aboutit à un pipeline fonctionnel dans Azure Data Factory capable de :

- Lire un fichier CSV (`cars.csv`) stocké dans Azure Blob Storage
- Copier automatiquement les données dans une table (`Cars`) d’une base de données Azure SQL
- Gérer l’ensemble du processus sans avoir besoin d’écrire une seule ligne de code

Ce pipeline peut ensuite être :
- Exécuté à la demande
- Programmé pour s'exécuter automatiquement
- Étendu pour d'autres types de données ou destinations

---

### 🚀 Améliorations possibles

Voici quelques idées pour enrichir ce projet de base :

- 🔄 **Automatisation** : ajouter des triggers pour exécuter le pipeline à des intervalles réguliers (quotidien, horaire...) ou à l'arrivée d'un nouveau fichier
- 🧪 **Ajout de transformations** : intégrer des **Mapping Data Flows** pour transformer les données pendant leur transit
- 📁 **Multi-fichiers** : configurer le pipeline pour traiter plusieurs fichiers CSV à la fois via des wildcards ou un paramétrage dynamique
- 🧩 **Modularité** : paramétrer le pipeline pour le rendre réutilisable avec différents noms de fichiers, schémas ou destinations

---

### 🙏 Crédit

Ce tutoriel est **inspiré** du travail de la chaîne YouTube :

🎥 [Adam Marczak – Azure for Everyone](https://www.youtube.com/@AdamMarczak)  
📺 Vidéo originale : [Azure Data Factory Tutorial – ETL Made Easy](https://www.youtube.com/watch?v=EpDkxTHAhOs)

L'objectif ici est pédagogique : reproduire et adapter le projet à des fins d’apprentissage.

---
