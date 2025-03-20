# Sentiment Analysis on movie reviews
Projet pour le cours de Machine Learning for NLP

Pour l'instant fait:
- données regroupées en deux sets : train et test, à partir du dossier 'aclImdb_v1.tar.gz' fourni par les auteurs du papier
- recodage des notes associées aux critiques en plusieurs labels
- test de scraping des résumés de films ('descriptions') pour quelques urls.
  2 problèmes : 1) les descriptions obtenues ne sont pas complètes, alors qu'elles le sont sur les pages imdb des films
                2) elles semblent courtes, il faut peut-être aller chercher plutôt la rubrique 'histoire/plot' sur la page du film (résumé plus long dispo)

