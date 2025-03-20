# Sentiment Analysis on movie reviews
Projet pour le cours de Machine Learning for NLP

Pour l'instant fait:
- données regroupées en deux sets : train et test, à partir du dossier 'aclImdb_v1.tar.gz' fourni par les auteurs du papier
- recodage des notes associées aux critiques en plusieurs labels
- test de scraping des résumés de films ('descriptions') pour quelques urls.
  2 problèmes : 1) les descriptions obtenues ne sont pas complètes, alors qu'elles le sont sur les pages imdb des films
                2) elles semblent courtes, il faut peut-être aller chercher plutôt la rubrique 'histoire/plot' sur la page du film (résumé plus long dispo)



IDEES à creuser et liens utiles en vrac :
https://snap.stanford.edu/data/web-Movies.html
https://github.com/kbastani/sentiment-analysis-movie-reviews
https://www.cs.cornell.edu/people/pabo/movie-review-data/

scraping allociné : 
https://datacorner.fr/sentiment-analysis/
https://github.com/ibmw/allocine-dataset-scraper
https://github.com/erwan29880/analyse-de-sentiments/blob/main/train.ipynb


scraping imdb :
https://github.com/borhenDH/IMDB-web-scraping/blob/main/IMDB%20web%20scraping/Projet_Python1_2.ipynb

modèles sur hugging face :
- binaire et trained sur amazon reviews : https://huggingface.co/AnkitAI/reviews-roberta-base-sentiment-analysis

inférence sur les films, à partir de films qui vont sortir => prédire le score des reviews ?

pour le modèle en positif/négatif, mettre plus de poids quand l'erreur est entre positif/negatif qu'entre positif et très positif ?
