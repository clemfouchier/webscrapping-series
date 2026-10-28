# 🏰 Séries médiévales : analyse comparative des notes et des avis

Pipeline data de bout en bout : scraping d'Allociné et d'IMDb (Beautiful Soup, Selenium), transformations avec pandas, analyse sémantique des avis (NLTK, scikit-learn), rapport interactif Power BI appliqué à la comparaison de deux séries médiévales. Environnement géré avec uv et git.

Objectif métier : aider un décideur (ex : un producteur) à comprendre pourquoi une série plaît plus qu'une autre.

Objectif personnel : développement continu de compétences techniques transférables à des environnements data professionnels.

Statut : projet en cours, dernière mise à jour le 28 octobre 2026.

![Aperçu de la page comparatif du Power BI](docs/img/PowerBI_comparatif.png)

![Aperçu de la page analyse du Power BI](docs/img/PowerBI_analyse.png)

## Analyse
A Knight of the Seven Kingdoms (ma série préférée de 2026 !) :
- Positif : l'évolution des personnages principaux 
- Négatif : les 2 premiers épisodes, et la durée des épisodes (30 minutes)
- Recommandation : continuer sur la lancée des derniers épisodes !

King & Conqueror :
- Positif : similarité avec l'univers de Game of Thrones (notamment les acteurs)  
- Négatif : manque de véracité historique, en particulier le premier épisode
- Recommandation : se rapprocher davantage de la riche histoire de Guillaume le Conquérant

## Stack
Python · pandas · BeautifulSoup · Selenium · NLTK · scikit-learn · Power BI · uv · Git

## Pipeline

![Schéma de la pipeline](docs/img/pipeline.png)

Le projet suit 5 grandes étapes :
1. **Collecte** : récupération des commentaires, notes, dates et métadonnées des deux séries sur Allociné et IMDb
2. **Concaténation et nettoyage** : harmonisation des notes, formats et identifiants
3. **Analyse exploratoire** : distributions des notes, différences entre les plateformes et enseignements statistiques
4. **Analyse sémantique** : prétraitement des commentaires, extractions des bigrams via la méthode TF-IDF.
5. **Restitution** : modélisation des données et création d'un rapport PowerBI interactif.

Concernant le NLP, le choix du couple TF-IDF - bigrams a été adopté pour privilégier l'interprétabilité et la simplicité dans une optique d'automatisation de la pipeline.

## Limites
- Seules les notes des spectateurs qui laissent un commentaire sont retenues, traduisant un engagement fort
- 84 % des avis viennent d'IMDb (notation plus généreuse qu'Allociné)
- Scraping d'IMDb plus possible aujourd'hui via Selenium : empêche la reproductibilité
- NLP sur les commentaires en anglais, car plus nombreux

## Prochains développements : ma feuille de route
- [ ] BERTopic pour générer des thématiques
- [ ] Scripts Python à la place des notebooks
- [ ] Base SQL + dbt
- [ ] Pipeline hebdomadaire
- [ ] Récupération de 5 à 10 séries
- [ ] Sélection dynamique des deux séries à comparer
- [ ] Intégration à un environnement cloud

## Ma démarche avec l'IA : un mentor et pas un exécutant
- Pas de vibe coding
- Code générique à adapter manuellement
- Pas de données injectées dans le LLM 
- Réflexion préalable à mes propres solutions

## Contact
Clément — Data Analyst... et passionné d'Histoire · [LinkedIn](https://www.linkedin.com/in/cl%C3%A9ment-fouchier-20206a152/)