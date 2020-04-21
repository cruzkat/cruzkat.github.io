---
layout: post
title: : Creating a Recommender System for Trails

----

*an overview of my NLP project creating a recommender engine from reviews.

-----

## Project Scope

For this project, I used Natural Language Processing to look at reviews for 1,000 hiking trails from AllTrails.com. My goal was to create a recommender system for trails that recommends based on trail similarity, rather than just geographical proximity (like AllTrails current recommendations). 

## Data Cleaning

I scraped the trail review data using Selenium. I got the text reviews, mileage, elevation gain, and difficulty rating for each hike. I then filtered the reviews to only look at ones longer than 3 characters. After concatenating all reviews of a trail, I was ready to start text pre-processing.

### Text Pre-Processing

In order to prepare the text for topic modeling, I needed to reduce the amount of words in my corpus. I first removed all punctuation and changed the text to lowercase. Using NLTK and Spacy libraries, I then removed stopwords and lemmatized the words.

## Topic Modeling

Topic modeling is quite subjective. Fortunately for me, as a frequent hiker, I have domain knowledge. I tried different methods of topic modeling including LSA and NMF combined with TF-IDF and CountVectorizer. Ultimately, I went with the method that had topics and output that made the most sense.

That ended up to be using Count Vectorizer and Latent Dirichlet Allocation (LDA).

My next step that I played around with were the amount of topics. I tried variations of 3 to 6 topics. I also did several iterations where I continued to add custom stopwords - words like trail, hike, good, and day were very common. These could be added in custom stopwords or caught with the *min-df* and *max-df* parameters in the vectorizers.

## Clustering

After topic modeling, I attempte to take the document-topic matrix and use that for clustering. I tried KMeans, UMAP, and TSNE for visualizing these clusters. I found that the clustered topics didn't make as much sense. In future work, I would like to continue working with the combination of clustering and topic modeling. This way I can have trails that are similar enough to be in the same cluster, but come from different topics.

## Recommender System

For my final output, I wanted to be able to recommend trails based on similiarity. I used cosine similarity on the document-topic matrix to calculate recommended trails. A way that I improved the trail recommender was by adding additional features from my web scraping. I added elevation gain, miles, and hiking difficulty rating. Since my document-topic matrix was only in the range of 0 to 1, I needed to standardize these new features. Without standardization, my topics are pretty much completely null because of how much larger the added features are.

I did see further improvement after standardization. I set the function to pull the 10 most similar trails.



