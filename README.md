# 5CLOUD-data-platform-for-real-time-event-analysis

📌 Introduction

Ce projet a pour objectif de construire une plateforme de données temps réel capable de collecter, ingérer, traiter et visualiser des données provenant d’une source ouverte.
Nous avons utilisé les services Microsoft Azure (Synapse Analytics, Data Lake) ainsi que des outils locaux (Python, Jupyter Notebook) pour simuler le fonctionnement complet d’une architecture cloud moderne.

La source choisie est l’API gratuite Open-Meteo, qui fournit des données météorologiques actualisées en continu.
Le pipeline mis en place permet de récupérer les observations météo (température, vent), de les stocker dans Azure, de les transformer via Synapse, puis de les visualiser localement.

⸻

🎯 Objectifs du projet

Le projet devait permettre de :
	•	Collecter des données météorologiques en temps réel via une API publique.
	•	Stocker automatiquement ces données dans un stockage cloud Azure (Data Lake Gen2).
	•	Traiter et transformer les données à l’aide d’Azure Synapse Analytics.
	•	Produire des visualisations analytiques depuis un notebook Python.
	•	Simuler un pipeline de données complet, similaire à une architecture utilisée en entreprise.

⸻

🛠️ Architecture & Environnement Azure

Un compte Azure for Students a été utilisé pour créer les ressources suivantes :
	•	Synapse Analytics Workspace : syn-etgrealtime
	•	Azure Data Lake Storage Gen2 : stetgrealtimeleo01
	•	Conteneur de stockage : data/openmeteo
	•	Connexion entre Synapse et le Data Lake pour lecture SQL via OPENROWSET.

Les fichiers JSON météo collectés sont placés dans le dossier :
data/openmeteo/

⸻

📥 Étape 1 – Collecte des données (API Open-Meteo)

Nous avons sélectionné l’API Open-Meteo comme source de données.
L’API permet de récupérer :
	•	la température en °C
	•	la vitesse du vent en km/h
	•	l’horodatage des mesures

En pratique, un script Python local interrogeait périodiquement l’API et enregistrait un fichier JSON sous la forme :

meteo_YYYYMMDD_HHMM.json

Chaque fichier simule une ingestion horaire, comme si Azure Data Factory effectuait un pipeline automatique.

Ces fichiers ont ensuite été envoyés manuellement dans Azure Data Lake, reproduisant le résultat d’un pipeline automatisé tout en évitant les coûts Azure.

⸻

☁️ Étape 2 – Ingestion dans Azure Data Lake

Le Data Lake comporte un dossier openmeteo dans lequel tous les fichiers JSON ont été copiés.

Azure Synapse a été relié à ce stockage grâce à une linked service, ce qui permet :
	•	la lecture directe des fichiers JSON dans SQL
	•	le traitement à la volée sans besoin de base SQL dédiée

Chaque fichier JSON représente un échantillon horaire, permettant ainsi une analyse réelle de séries temporelles.

⸻

🧮 Étape 3 – Traitement et transformation via Azure Synapse Analytics

L’ingestion terminée, la transformation a été effectuée grâce au moteur SQL Serverless de Synapse.

Les actions réalisées :
	1.	Création d’une source de données externe pointant vers le Data Lake.
	2.	Définition d’un format de fichier JSON.
	3.	Lecture dynamique des fichiers JSON grâce à OPENROWSET.
	4.	Extraction des champs utiles :
	•	obs_time
	•	temperature
	•	windspeed

Une vue logique nommée dbo.vw_openmeteo a été créée pour exposer ces données nettoyées et uniformisées.

Synapse a ensuite permis de générer un graphique intégré afin de valider la cohérence des données avant analyse.

⸻

📊 Étape 4 – Visualisation locale avec Jupyter Notebook

Les fichiers JSON ont été téléchargés localement dans un dossier :

Meteo_json/

À partir d’un notebook Python, nous avons :
	•	chargé tous les fichiers JSON
	•	converti les données en DataFrame (pandas)
	•	nettoyé les séries temporelles
	•	généré 3 graphiques via matplotlib :

Figure 7 – Évolution de la température

Titre sous l’image :
Évolution horaire de la température mesurée à Paris.

Figure 8 – Variation de la vitesse du vent

Titre sous l’image :
Variation de la vitesse du vent sur la même période.

Figure 9 – Corrélation température / vent

Titre sous l’image :
Représentation combinée température et vitesse du vent.

Ces figures servent de dashboard analytique local.
Elles reproduisent l’étape finale qui aurait été réalisée dans Power BI dans un scénario 100% cloud.

⸻

📌 Conclusion

Ce projet démontre la construction d’une plateforme de données en temps réel basée sur Azure, tout en utilisant des alternatives locales compatibles avec les contraintes étudiantes.

Nous avons réussi à :
	•	Collecter des données météorologiques réelles.
	•	Simuler une ingestion automatisée vers un Data Lake Azure.
	•	Traiter dynamiquement les données via Synapse SQL Serverless.
	•	Produire des visualisations analytiques permettant d’interpréter les tendances.

Cette architecture reproduit fidèlement le fonctionnement d’un pipeline professionnel tout en restant simple, économique et pédagogique.
