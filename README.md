# 🏰 Séries médiévales : analyse comparative des avis

Pipeline data de bout en bout : scraping d'Allociné et d'IMDb, analyse sémantique des avis, rapport interactif Power BI. 

Objectif : aider un décideur (ex : un producteur) à comprendre pourquoi une série plaît plus qu'une autre.

![Aperçu de la page comparatif du Power BI](docs/img/PowerBI_comparatif.png)

![Aperçu de la page analyse du Power BI](docs/img/PowerBI_analyse.png)

## Analyse
A Knight of the Seven Kingdoms (ma série préférée de 2026 !) :
- Positif : l'évolution des personnages principaux 
- Négatif : les 2 premiers épisodes, et la durée des épisodes (30 minutes)
- Recommandation : continuer dans cette voie !

King & Conqueror :
- Positif : similarité avec l'univers de Game of Thrones (notamment les acteurs)  
- Négatif : manque de véracité historique, en particulier le premier épisode
- Recommandation : se rapprocher davantage de l'histoire (riche) de Guillaume le Conquérant

## Stack
Python · pandas · BeautifulSoup · Selenium · NLTK · scikit-learn · Power BI · uv · Git

## Pipeline
[schéma : Collecte → Nettoyage → NLP → Power BI]

![Schéma de la pipeline](docs/img/pipeline.png)

## Limites
- Biais de sélection : seuls les spectateurs engagés laissent un avis
- 84 % des avis viennent d'IMDb (notation plus généreuse qu'Allociné)
- Scraping d'IMDb plus possible aujourd'hui via Selenium : empêche la reproductibilité
- NLP sur les commentaires en anglais, car plus nombreux

## Roadmap
- [ ] BERTopic pour générer des thématiques
- [ ] Scripts Python à la place des notebooks
- [ ] Base SQL + dbt
- [ ] Pipeline hebdomadaire
- [ ] Récupération de 5 à 10 séries
- [ ] Sélection dynamique des deux séries à comparer

## Ma démarche avec l'IA : un mentor
- Pas de vibe coding
- Code générique à adapter manuellement
- Pas de données injectées dans le LLM 
- Réflexion préalable à mes propres solutions

## Contact
Clément — Data Analyst · [LinkedIn](https://www.linkedin.com/in/cl%C3%A9ment-fouchier-20206a152/)