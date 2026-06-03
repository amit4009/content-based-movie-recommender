# Content-Based Movie Recommender

## Overview

This project builds a content-based movie recommendation system using TMDB movie metadata. The recommender suggests movies similar to a selected input movie by comparing metadata-based tags such as overview, genres, keywords, cast, and director.

This is primarily a content-based recommender. It does not use collaborative filtering, user ratings, watch history, or real user-item interaction data.

## Business Problem

Streaming and content platforms need recommendation systems that help users discover relevant titles quickly. When user interaction data is limited or unavailable, content-based filtering can provide a practical starting point by recommending items with similar metadata and themes.

This project demonstrates how movie metadata can be transformed into numerical features and used to generate similar-movie recommendations.

## Dataset

* **Source:** TMDB 5000 Movie Metadata Dataset from Kaggle
* **Files Used:** `tmdb_5000_movies.csv`, `tmdb_5000_credits.csv`
* **Movies Processed:** 4,806
* **Recommendation Type:** Content-based filtering
* **Input Features:** Overview, genres, keywords, cast, crew/director

The movie metadata and credits datasets are merged on movie title to combine descriptive information with cast and crew details.

## Methodology

* Loaded TMDB movie metadata and credits datasets
* Merged movie and credits data on title
* Selected relevant fields: movie ID, title, overview, genres, keywords, cast, and crew
* Removed missing records before feature processing
* Extracted genre and keyword names from JSON-like fields
* Extracted top cast members and director information
* Removed spaces from multi-word names to preserve entity identity
* Combined overview, genres, keywords, cast, and director into a single `tags` feature
* Converted tags into a numerical count matrix using `CountVectorizer`
* Limited the vocabulary to 5,000 features and removed English stop words
* Converted the vector matrix into sparse format for efficient similarity calculation
* Computed pairwise cosine similarity across movies
* Built a recommendation function that returns the top 5 most similar movies for a selected title

## Recommendation Approach

The recommender uses text/tag similarity rather than user behavior.

```text
Movie metadata → tags → CountVectorizer → cosine similarity → top 5 similar movies
```

`CountVectorizer` converts movie tags into token-count vectors. Cosine similarity then compares those vectors to identify movies with similar metadata patterns.

## Example Recommendation

For the movie:

```text
Gandhi
```

The system recommends:

```text
Gandhi, My Father
The Wind That Shakes the Barley
A Passage to India
Guiana 1838
Ramanujan
```

similarly for 'Spider man -3', the system suggests:
```text
Spider Man-2
Spider Man
The Amazing Spider Man-2
The Amazing Spider Man
Arachnophobia
```
![alt text](movie_recomendation-1.png)



These results show that the recommender can surface movies with related historical, biographical, or thematic elements.

## User History Extension

The notebook also includes a simple user-history function that aggregates recommendations from multiple input movies.

Example input:

```text
Avatar
Spectre
```

The function collects similar movies for each title and returns a de-duplicated recommendation list.

This is a basic aggregation approach, not a learned personalization model.

## Business Impact

This project demonstrates how content metadata can support movie discovery when user behavior data is unavailable. The approach can help create a baseline recommendation layer for cold-start situations, similar-title browsing, and content discovery workflows.

## Tech Stack

Python, Pandas, NumPy, Scikit-learn, CountVectorizer, Cosine Similarity, SciPy Sparse Matrix, Pickle, Jupyter Notebook

## Repository Structure

```text
content-based-movie-recommender/
├── Recomendation_system.ipynb
├── tmdb_5000_movies.csv
├── tmdb_5000_credits.csv
├── movie_list.pkl
├── similarity_sparse.pkl
└── README.md
```

## How to Run

```bash
git clone https://github.com/amit4009/content-based-movie-recommender.git
cd content-based-movie-recommender
jupyter notebook
```

Then open:

```text
Recomendation_system.ipynb
```

## Limitations

* The recommender does not use real user ratings, clicks, watch history, or interaction data.
* The system is content-based only and does not implement collaborative filtering.
* Recommendations depend heavily on the quality of metadata tags.
* Popularity, recency, user preferences, and diversity constraints are not included.
* The current system returns similar movies, not ranked personalized recommendations.
* Evaluation is qualitative; no offline recommendation metrics such as Precision@K, Recall@K, MAP, or NDCG are calculated.

## Future Improvements

* Add TF-IDF vectorization and compare it against CountVectorizer
* Add collaborative filtering using user-rating data
* Add hybrid recommendations combining metadata and user interactions
* Add offline ranking metrics such as Precision@K, Recall@K, MAP, and NDCG
* Add diversity and popularity-aware ranking logic
* Add a Flask or Streamlit interface for interactive recommendations
* Add poster images and TMDB API integration for better user experience
