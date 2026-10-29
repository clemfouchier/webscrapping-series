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

Ma volonté est d'en faire un projet à briques, c'est-à-dire continuer à travailler régulièrement pour l'améliorer au fil de l'eau, ajouter de nouvelles fonctionnalités, prendre le temps d'apprendre et de me tromper sans crainte, explorer des nouvelles thématiques data etc.

Il s'agit donc de la meilleure manière de capitaliser sur mes connaissances et de monter en compétences hors du cadre professionnel.

MON AMBITION
Gérer toute le cycle de vie de la donnée dans ce projet :
- Avoir un code propre, bien documenté, et organisé sous forme de scripts.
- Automatiser les transformations et les documenter avec DBT.
- Générer une pipeline hebdomadaire de données et la stocker dans une BDD - éventuellement sur un cloud.
- Aller chercher les données de plusieurs séries dans la limite du raisonnable (probablement dizaine).
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
4. Analyse sémantique
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

i. La récupération des commentaires

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

ii. La récupération des informations sur les séries

Grâce au 'allocine_id', je peux récupérer les informations suivantes via une fonction python appelée 'info_series' :
- le titre
- l'url de l'affiche
- le genre
- les réalisateurs
- les acteurs
- la nationalité

Quand il y a plusieurs occurrences par donnée (ex : plusieurs acteurs), une fonction 'extract_text' permet de tous les concaténer avec le séparateur '|'.

Ces données sont enregistrées dans le fichier 0_Data/2_Clean/info_series.csv

b. Scrapping d'imdb avec Selenium (plus possible aujourd'hui)

J'ai d'abord essayé de scrapper les données d'imdb avec BeautifulSoup, comme pour allociné.
Mais je suis tombé sur une surpise : il n'était pas possible car imdb repérait que la requête émanait d'un bot.

C'est la librairie Selenium qui m'a donnée la solution. La librairie ouvre un "vrai" navigateur, et simule la navigation humaine. 

Il faut donc simuler les temps de pause, les clics sur des boutons (ex : accepter les cookies qui apparaissent), naviguer sur la page 'avis', scroller, et même cliquer sur le bouton 'plus de commentaires' jusqu'à que tous les commentaires apparaissent pour pouvoir récupérer les données des commentaires, la note et la date.

Comme pour le scrapping d'allocine, je crée des dataframes par série, que je concatène.

Ensuite, j'effectue les transformations suivantes :
- supprimer les lignes où il n'y a pas de notes.
- concaténer le titre de l'avis et l'avis (sur imdb, chaque avis a un titre)
- dans la récupération des dates, bien écrire les mois en anglais pour pouvoir convertir en dates
- changements de type de données

Ce fichier est enregistré ici :  0_Data/1_Raw/data_imdb.csv

2. FUSION ET TRANSFORMATION DES DONNEES

Dans le notebook 2_Nettoyage_Analyse/3_fusion_nettoyage.ipynb, j'importe les deux fichiers data_allocine.csv et data_imdb.csv.

Ici, je concatène les fichiers, et je réinitialise l'index.

Voici ensuite les transformations que j'effectue :
- Création d'une colonne note, qui remet toutes les notes à la même échelle de 10 (x2 pour les notes d'Allociné, sur 5). Les colonnes 'notesAllocine' et 'notesImdb' sont ensuite supprimées.
- Conservation du nom de la plateforme pour Allociné et Imdb.
- Transformation de la colonne date en format date.
- Ajouter les 'allocine_id', soit l'id unique par série sur toutes les lignes (en particulier celles qui viennent de la plateforme Imdb).
- Ajouter le review_id, soit l'identifiant unique par commentaire, sur toutes les lignes (en particulier celles qui viennent de la plateforme Imdb).
- Transformation du type de données lorsque nécessaire.

Le fichier est exporté à 2_Clean/Traitement_intermediaire/data_clean.csv

3. ANALYSE EXPLORATOIRE

Lors de cette phase, je prends le temps d'analyser les données, leur distribution, le lien entre les variables, et je tire les premiers enseignements.

J'ouvre le fichier 'data_clean.csv' de la dernière phase. Voici les principaux enseignements que l'on peut tirer :

- Les notes données, et donc les moyennes, sont basées ici uniquement sur les personnes qui ont mis un avis. Ce sont donc des spectateurs plus engagés, avec une conviction.
- 84% des avis proviennent de la plateforme Imdb. Cette dernière note plus favorablement qu'Allocine, plateforme française.
- En février 2026, pour la série 'A knight of the Sevent Kingdoms', 60% des notes données sont des 10. Deux hypothèses :
    1. L'engouement : les épisodes les mieux notés de la série sont sortis en février. Les fans de Game of Thrones ont pu regarder dès la sortie et donner une très bonne note.
    2. Une opération pour améliorer la note, avec de nombreux avis donnés, a eu lieu.
- Ayant moi-même vu la série, je pense que l'engouement explique ces notes. Mais, par soucis d'apprentissage, j'ai choisi de donner un poids de 80/100 à ces notes (uniquement valable lors de cette phase).

Voici donc les moyennes pour chaque série avec les intervalles de confiance :
A Knight of the Seven Kingdoms : 8,11 [7.96 - 8.26] / 10
King & Conqueror : 4,38 [3.98 - 4.79] / 10

Par soucis d'apprentissage, j'ai également calculé le skew (asymétrie) et le kurtosis (applatissement), qui servent à analyser statistiquement la forme de la distribution.

Nous verrons par la suite comment l'analyse sémantique nous aide à expliquer un tel écart de notation.

4. ANALYSE SEMANTIQUE

J'ai réalisé l'analyse sémantique en deux partie.

La première partie est le preprocessing (3_Analyse_Semantique/5_preprocessing.ipynb). Elle consiste à :
- Créer des colonnes qui regroupent certaines notes en catégories (3)
- Identifier des signaux dans les verbatims, appelées métadonnées (points d'exclamations et mots écrits en lettre CAPITALES)
- Nettoyer les mots inutiles et la ponctuation.
- Lemmatiser, soit regrouper les mots qui ont une même racine (ex : trouver, trouve --> 1 seul mot).

Cette phase permet de tirer quelques enseignements :
- La présence de mots en LETTRES CAPITALES semble corrélé positivement avec les notes positives. Surtout lorsqu'il y en a au moins 50 dans un même avis.
- Les points d'exclamation (!!) intensifient les avis : on les trouve davantage où il y a des notes positives et négatives. Lorsqu'il y en a plus de 15, c'est un signe que la note est positive.
- A l'inverse, les points d'interrogration (??) semblent davantage présent pour mettre en cause la série, surtout lorsqu'il y en a beaucoup (10 ou plus)

Le fichier est enregistré ici : /0_Data/2_Clean/Traitement_intermediaire/data_preprocessed.csv

La deuxième partie est consacrée à l'exploration des méthodes d'anayse sémantique : 3_Analyse_Semantique/5_analyse_semantique.ipynb

J'ai essayé les solutions suivantes :
- Bag of Words : 1 mot = 1 occurence
- TF-IDF : même principe, mais pondère les mots en fonction de leur rareté dans le corpus.
- Bigrams : décompte les combinaisons de 2 mots (ex : très bien)
- ANNFIN : Donne un score émotionnel aux mots, et donc un score de sentiment à chaque commentaire
- Latent Ditriech Allocation (LDA) : trouve les catégories. 

J'ai choisi la méthode qui pondère le poids des mots (TF-IDF) et de n'afficher que les bigrams.

IMAGE BIGRAMS.

A date, je n'ai pas retenu le Latent Ditriech Allocation, car je me suis concentré sur des solutions simples, faciles à comprendre et à utiliser. Mais à l'avenir, je pourrai davantage explorer cette possibilité, tout comme le DeepLearning (notamment BERTopic).

Voici l'analyse que je tire des bigrams :
A Knight of the Seven Kingdoms :
- Les verbatims positifs mettent en avant les personnages principaux, et leur développement tout au long de la série.
- Au contraire, les verbatims négatifs parlent également des personnages principaux, des deux premiers épisodes, du format d'épisodes courts (30 minutes), et du budget qui semble faible pour une série de l'univers de Game of Thrones.

Mes recommandations pour la prochaine saison : continuer sur cette lancée en se concentrant sur le développement des deux personnages. 

King & Conqueror :
- Les commentaires positifs parlent également de Game of Throne : les spectateurs sont probablement attirés par la ressemblence avec l'univers. Les acteurs Nikolaj Coster Waldau, qui joue Jamie Lannister, et James Norton, présent dans House od the Dragon, sont mis en avant. Ils apprécient l'aspect historique de la série, qu'ils jugent précis, presque à la manière d'un documentaire.
- Au contraire, les commentaires négatifs remettent fortement en cause la véricité historique de la série. Le premier épisode revient particulièrement dans les commentaires.

Mes recommandations pour la prochaine saison : essayer de se rapprocher davantage des faits historiques. Si des simplifications doivent être effectuées, que ce soit fait en cohérence avec les personnages historiques.

A ce stade, deux fichiers sont enregistrés :
- '/0_Data/2_Clean/data_preprocessed_final.csv', soit le même fichier qu'avant, mais en ajoutant le score de sentiment par commentaire (via l'algorithme ANNFIN)
- '/0_Data/2_Clean/bigrams.csv'

Cette dernière consiste en un fichier csv qui compte, pour chaque couple review_id x bigram, le nombre d'occurences du bigram. Il s'agit donc d'un table de faits particulièrement longue.

5. INTEGRATION A POWER BI

A date, j'intègre 3 fichiers, qui deviennent 3 tables.
- info_series.csv devient la table de dimension dim_Info_series : la table rescence les informations sur les séries (genre, réalisateur...)
- data_preprocessed_final.csv devient la table de dimension dim_Comments : la table donne les commentaires, la note et les métadonnées.
- bigrams.csv devient fact_Bigrams : c'est la table longue qui répertorie le décompte des bigrams par review_id.

Dans Power Query, je garde la source dans un dossier 'Src', et je réalise les transformations dans un dossier 'Transformés'.

Voici les transformations que je réalise à ce stage (bien légères) :
- Pour toutes les tables : modification des types
- Pour la table commentaires : création d'une colonne notes_3_categories avec Positif, Neutre et Négatif

Voici le modèle :
SCHEMA MODELE.

Voici la première page 'Comparatif' :

SCHEMA PREMIERE PAGE.

Elle permet de comparer en un coup d'oeil les bigrams qui ressortent le plus, la distribution des notes et du score de sentiment.

Voici la seconde page 'Analyse par série" :

SCHEMA DEUXIEME PAGE.

Elle offre une vue globale de l'appréciation d'une série et permet de filtrer facilement les commentaires selon les bigrams qui ressortent, la note ou le score de sentiment.

CONCLUSION
Entre le début du projet et aujourd'hui, je sens qu'un cap majeur a été franchi dans ma manière d'appréhender le code, et plus globalement les projets data. 

Je suis aujourd'hui capable de débuter un projet data proprement en créant l'environnement virtuel associé, d'aller récupérer les données par moi-même selon différentes sources, de les assembler, les transformer, concevoir une solution d'analyse sémantique simple, et d'intégrer tout ce travail dans un rapport Power BI conçu pour être attractif et facile à utiliser.

Mais ce projet ne s'arrête pas aujourd'hui, les possibilité d'évolution sont nombreuses :
- Organiser le projet sous formes de scripts python (au lieu des notebooks, pratiques pour explorer les données)
- Générer une BDD SQL
- Insérer les données dans un environnement cloud
- Automatiser la pipeline de données
- Aller chercher les données de plus de séries (en restant raisonable, 5 à 10 maximum)
- Dans Power BI, pouvoir choisir les deux séries à comparer.
- Tester de nouvelles techniques d'analyse sémantique / NLP (BERTopic).
- Créer une page qui analyse les indicateurs selon les informations dont nous disposons sur les séries (ex : un genre particulier, un acteur particulier, une nationalité... avec possibilité de croiser)
- Créer une page qui analyse les données selon les métadonnées (ex : nombre de mots en lettre capitale, de points d'exclamation, d'interrogation...)

Je continue à avancer quotidiennement sur ce projet pour le rendre un peu meilleur chaque jour avec régularité.