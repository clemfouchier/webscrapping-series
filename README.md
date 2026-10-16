Webscrapping Séries : un projet en briques

EN RESUME

CONTEXTE
Pourquoi ce projet ?
Depuis ma reconversion en 2024, j'ai réalisé 4 projets personnels, tous avec des jeux de données issus de Kaggle.

Si ces projets m'ont permis de solidifier mes bases de Python, SQL et PowerBI, et d'améliorer mes compétences en statistiques (A/B testing), il m'ont tous laissé un goût d'inachevé, car peu reproductibles dans un environnement professionnel. 

Cette fois, j'ai voulu me passer de Kaggle (et de Conda!), aller chercher mes propres données via une API ou du webscrapping, gérer mon environnement de code de manière professionnelle avec git et uv puis générer un rapport Power BI versionné, clair et attractif. En clair, gérer tout le cycle de vie des données de la collecte à l'analyse.

En parrallèle, dans un contexte professionnel, j'interviens sur des projets liés à l'expérience utilisateurs, avec de nombreux retours sous forme de verbatims. Pour que ce projet ait un impact immédiat pour mes clients, j'ai donc choisi un sujet qui me permet de comprendre la satisfaction des utilisateurs et d'explorer le NLP. Un choix qui a déjà eu un impact concret, car j'ai développé une solution d'analyse sémantique pour ce client suite à mes recherches.

Mon choix s'est donc porté sur une analyse comparative des notes et des commentaires de deux séries dans un univers médiéval / médiaval-fantastique, deux genres que j'affectionne particulièrement en tant que passionné d'histoire. Je souhaite recueillir les avis et les notes sur plusieurs plateformes pour proposer une notation unique par série et pour centraliser les commentaires.

L'idée finale est de mettre à disposition cette donnée à de potentiels décideurs pour comprendre facilement pourquoi les utilisateurs ont plus apprécié une série plutôt que l'autre, quels mots et thématiques ressortent parmi ceux qui ont aimé ou pas. Ce qui serait utile par exemple pour des producteurs, pour pouvoir décider s'ils financent ou pas des prochaines saisons, et sur quelles thématiques ils pourraient l'orienter.

Mon ambition est d'en faire un projet à brique, c'est-à-dire continuer à travailler régulièrement pour l'améliorer au fil de l'eau, ajouter de nouvelles fonctionnalités, prendre le temps d'apprendre et de me tromper sans crainte, explorer des nouvelles thématiques data etc.

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
- Avant de demander, réfléchir par moi-même à un minimum de 2 hypothèses.
- Challenger les propositions, et comprendre les raisons derrière chaque choix.
- Demander des cas génériques anonymisés à reprendre par soi-même.
- Ne pas injecter les données.

En résumé, j'utilise l'IA comme un mentor, avec exigence.

METHODOLOGE
- Préparation de l'environnement avec uv et github
- Scrapping d'Allociné et Imdb avec BeautifulSoup et Selenium
- Fusion des données et transformation des données avec pandas
- Analyse exploratoire des données avec pandas et seaborn
- Analyse sémantique avec NLKT et scikit-learn
- Intégration à PowerBI

ETAPE PAR ETAPE
0. Préparation de l'environnement
1. Scrapping de deux plateformes différentes
2. Fusion et transformation des données
3. Analyse exploratoire
4. Analyse Sémantique
5. Intégration à PowerBI

CONCLUSION
Projet hautement formateur.
Nombreuses possibilités d'évolution.

EVOLUTIONS POSSIBLES
En amont :
- Organiser le projet sous formes de scripts python vs les notebooks
- Générer une BDD SQL
- Insérer dans un environnement cloud
- Automatiser une pipeline de données

Analyse :
- Aller chercher les données de plus de séries (en restant raisonable)
- Dans Power BI, pouvoir choisir les deux séries à comparer.
- Tester de nouvelles techniques d'analyse sémantique / NLP (Deeplearning, thématiques...).
- Créer une page qui analyse les indicateurs selon les informations dont nous disposons sur les séries (ex : un genre particulier, un acteur particulier, une nationalité... avec possibilité de croiser)