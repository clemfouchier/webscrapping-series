Webscrapping Séries : un projet en briques

EN RESUME

CONTEXTE
Pourquoi ce projet ?
Depuis ma reconversion en 2024, j'ai réalisé 4 projets personnels, tous avec des jeux de données issus de Kaggle.

Si ces projets m'ont permis de solidifier mes bases de Python, SQL et PowerBI, et d'améliorer mes compétences en statistiques (A/B testing), il m'ont tous laissé un goût d'inachevé, car peu reproductibles dans un environnement professionnel. 

Cette fois, j'ai voulu me passer de Kaggle (et de Conda!), aller chercher mes propres données via une API ou du webscrapping, gérer mon environnement de code de manière professionnelle avec git et uv puis générer un rapport Power BI versionné, clair et attractif. En clair, gérer tout le cycle de vie des données de la collecte à l'analyse.

En parrallèle, dans un contexte professionnel, j'interviens sur des projets liés à l'expérience utilisateurs, avec de nombreux retours sous forme de verbatims. Pour que ce projet ait un impact immédiat pour mes clients, j'ai donc choisi un sujet qui me permet de comprendre la satisfaction des utilisateurs et d'explorer le NLP. Un choix qui a déjà eu un impact concret, car j'ai développé une solution d'analyse sémantique pour ce client suite à mes recherches.

Mon choix s'est donc porté sur une analyse comparative des notes et des commentaires de deux séries dans un univers médiéval / médiaval-fantastique, deux genres que j'affectionne particulièrement en tant que passionné d'histoire. Je souhaite recueillir les avis et les notes sur plusieurs plateformes pour proposer une notation unique par série et pour centraliser les commentaires.

L'idée finale est de mettre à disposition cette donnée à de potentiels décideurs pour comprendre facilement pourquoi les utilisateurs ont plus apprécié une série plutôt qu'une autre, quels mots et thématiques ressortent parmi ceux qui ont aimé ou pas. Ce qui serait utile par exemple pour des producteurs, pour pouvoir décider s'ils financent ou pas des prochaines saisons, et sur quelles thématiques ils pourraient l'orienter.

Ma volonté est d'en faire un projet à brique, c'est-à-dire continuer à travailler régulièrement pour l'améliorer au fil de l'eau, ajouter de nouvelles fonctionnalités, prendre le temps d'apprendre et de me tromper sans crainte, explorer des nouvelles thématiques data etc.

Il s'agit donc de la meilleure manière de capitaliser sur mes connaissances et de monter en compétences hors du cadre professionnel.

MON AMBITION
Gérer toute le cycle de vie de la donnée dans ce projet :
- Avoir un code propre, bien documenté, et organisé sous forme de scripts.
- Automatiser les transformations et les documenter avec DBT.
- Générer une pipeline hebdomadaire de données et la stocker dans une BDD - éventuellement sur un cloud.
- Aller chercher les données de plusieurs séries dans la limite du raisonnable (une dizaine ?).
- Les mettres à disposition dans PowerBI, et pouvoir choisir les séries que l'on souhaite comparer.
- Pouvoir analyser les notes et commentaires récueillis par genre, par acteur, par nationalité...
- Explorer les différentes manières de traiter les données textuelles pour générer de la valeur.

MES LIGNES DIRECTRICES DANS L'UTILISATION DE L'IA :
- PAS DE VIBE CODING (meilleur moyen de ne pas apprendre)
- Avant de demander à un LLM, réfléchir par moi-même à un minimum de 2 hypothèses.
- Challenger les propositions, et comprendre les raisons derrière chaque choix.
- Demander des cas génériques anonymisés à reprendre par soi-même.
- Ne pas injecter les données dans le LLM.

En résumé, j'utilise l'IA comme un mentor, avec exigence.

METHODOLOGE
- Préparation de l'environnement avec uv et github
- Scrapping d'Allociné et Imdb avec BeautifulSoup et Selenium
- Fusion des données et transformation des données avec pandas
- Analyse exploratoire des données avec pandas et seaborn
- Preprocessing avec pandas puis analyse sémantique avec NLKT et scikit-learn
- Intégration à PowerBI et construction du rapport

ETAPE PAR ETAPE
0. Préparation de l'environnement
1. Scrapping de deux plateformes différentes
2. Fusion et transformation des données
3. Analyse exploratoire
4. Analyse Sémantique
5. Intégration à PowerBI

0. PREPARATION DE L'ENVIRONNEMENT 
J'ai commencé à utiliser les environnements virtuels via venv. J'ai ensuite essayé la librairie uv, qui est la solution la plus simple pour initialiser les projets et gérer ses environnements virtuels et les dépendances (versions des librairies utilisées).

Documentation ici : https://docs.astral.sh/uv/

L'organisation des dossiers suit un cheminement logique entre le stockage de la donnée, le scrapping, le nettoyage/analyse puis l'analyse sémantique.

Voici l'arborescence de manière simplifiée :
.venv
0_Data
    1_Raw
       data_allocine.csv
       data_imdb.csv 
    2_Clean
        Traitement intermédiaire
            data_clean.csv
            data_preprocessed.csv
        bigrams.csv                     --> Table longue de bigrams (analyse sémantique)
        data_preprocessed_final.csv     --> Commentaires, notes, et métadonnées
        info_series.csv                 
1_Scrapping
    1_scrapping_allocine.ipynb
    2_scrapping_imdb.ipynb
2_Nettoyage_Analyse
    3_fusion_nettoyage.ipynb
    4_analyse_exploratoire.ipynb
3_Analyse_Semantique
    5_preprocessing.ipynb
    6_analyse_semantique.ipynb
Analyse_series.pbip                     --> Fichier PowerBI
requirements.txt                        --> Librairies utilisées et dépendances.

1. SCRAPPING DES PLATEFORMES

a. Allocine avec Beautiful Soup

La récupération sur la page commentaire

A date, le scrapping de la série d'allocine se réalise au travers de la librairie Beautiful Soup.

Chaque série peut être identifiée dans Allocine au travers du 'allocine_id'. Par exemple, dans cette page : 'https://www.allocine.fr/series/ficheserie-28230/critiques/', l'id est "28230".

Pour les deux séries, je récupère les données suivantes sur l'ensemble des pages 'avis' de chaque série :
- la note
- les commentaires
- les dates

A cette étape, je réalise les transformations suivantes : 
- la note : la transformation de la virgule en point pour convertir la donnée en nombre décimal.
- les commentaires : supprimer les caractères spéciaux (\x00-\x1f\x7f\xa0\uf000-\uffff)
- les dates : supprimer 'Publié le' pour pouvoir le convertir la donnée en format date.

Je créée un dataframe par série, puis je les concatène.

Une fois que les données sont concaténées, je réalise de nouveaux des transformations :
- Changement de type de données
- Pour la date, je traduis tous les noms français en anglais pour pouvoir convertir au format date
- Je créée un 'review_id' par commentaire, qui reprend le 'allocine_id', et l'index de la ligne. Par exemple, la première ligne pour la série 'A Knight of the Seven Kingdoms' est 282301 ("28230" & "1").

Ces données sont enregistrées dans le dossier 0_Data/1_Raw/data_allocine.csv

La récupération des informations sur les séries

Grâce au 'allocine_id', je peux récupérer les informations suivantes via une fonction python appelée 'info_series' :
- le titre
- l'url de l'affiche (en passant par les méta-informations)
- le genre
- les réalisateurs
- les acteurs
- la nationalité

Quand il y a plusieurs occurrences par donnée (ex : plusieurs acteurs), une fonction 'extract_text' permet de tous les concaténer avec le séparateur '|'.

Ces données sont enregistrées dans le fichier 0_Data/2_Clean/info_series.csv

b. Scrapping d'imdb avec Selenium (plus possible aujourd'hui)

J'ai d'abord essayé de scrapper les données d'imdb avec BeautifulSoup, comme pour allociné.
Mais je suis tombé sur une surpise : il n'était pas possible car imdb repérait que la requête émanait d'un bot.

C'est la librairie selenium qui m'a donné la solution. La librairie ouvre un "vrai" navigateur, et simule la navigation humaine. 

Il faut donc simuler les temps de pause, les clics sur des boutons (ex : accepter les cookies qui apparaissent), naviguer sur la page 'avis', scroller, et même cliquer sur le bouton 'plus de commentaires' jusqu'à que tous les commentaires apparaissent pour pouvoir récupérer les données des commentaires, la note et la date.

Comme pour le scrapping d'allocine, je crée des dataframes par série, que je concatène.

Ensuite, j'effectue les transformations suivantes :
- supprimer les lignes où il n'y a pas de notes.
- concaténer le titre de l'avis et l'avis (sur imdb, chaque avis a un titre)
- dans la récupération des dates, bien écrire les mois en anglais pour pouvoir convertir en dates
- changements de type de données

Ce fichier est enregistré ici :  0_Data/1_Raw/data_imdb.csv

2. FUSION ET TRANSFORMATION DES DONNEES


CONCLUSION
Entre le début du projet et aujourd'hui, je sens qu'un cap majeur a été franchi dans ma manière d'appréhender le code, et plus globalement les projets data. 

Je suis aujourd'hui capable de débuter un projet data proprement en créant l'environnement virtuel associé, d'aller récupérer les données par moi-même selon différentes sources, de les assembler, les transformer, concevoir une solution d'analyse sémantique simple, et d'intégrer tout ce travail dans un rapport PowerBI conçu pour être attractif et facile à utiliser.

Mais ce projet ne s'arrête pas aujourd'hui, les possibilité d'évolution sont nombreuses :
- Organiser le projet sous formes de scripts python (au lieu des notebooks, pratiques pour explorer les données)
- Générer une BDD SQL
- Insérer les données dans un environnement cloud
- Automatiser la pipeline de données
- Aller chercher les données de plus de séries (en restant raisonable)
- Dans Power BI, pouvoir choisir les deux séries à comparer.
- Tester de nouvelles techniques d'analyse sémantique / NLP (Deeplearning, thématiques...).
- Créer une page qui analyse les indicateurs selon les informations dont nous disposons sur les séries (ex : un genre particulier, un acteur particulier, une nationalité... avec possibilité de croiser)

Je continue à avancer quotidiennement sur ce projet pour le rendre un peu mieu chaque jour avec régularité - la meilleure façon d'avancer.