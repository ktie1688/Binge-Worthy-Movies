# Binge Worthy Movie Recommendations
## Context
In the age of streaming, viewers have access to thousands of movies and TV shows, making the search for the next binge-worthy show an exhausting task. Recommender systems can eliminate that search by using algorithms to filter through those titles and return a list of recommendations based on a viewer's preferences. 
## Objective
To learn about what makes recommender systems tick, I will use content and collaborative filtering (CF) to recommend movie titles based on a show the viewer just watched.
## Tools/Resources
* Data sourced from MovieLens at https://grouplens.org/datasets/movielens/.
* Pandas, NumPy, Matplotlib, Seaborn, and scikit-learn.
## Methodology
For simplicity, I focused on creating an algorithm that accounts for the rating and genre attributes. My key features were:

**movies.csv**

movieId - the unique identifier indexing that movie.

title - the name of the movie followed by the year of release.

genres - the genre categories that a specific title falls under.

**ratings.csv**

userId - the unique identifier indexing a specific MovieLens user that provided a rating for a movie.

rating - the rating level given to a movie from a scale of 1 to 5.

First, I created a utility matrix that would act as a map for storing and retrieving different movies and their rating data. There were 4 mapping dictionaries that would each play a key role in identifying a user and the specific rating they submitted and identifying the movie that received that user's rating. 

Testing the utility matrix, I was able to learn that the most active user rated 2,698 movies! Users also typically submitted just under 100 ratings and most movies received less than 50 ratings.

<img width="919" alt="Screenshot 2024-04-01 at 9 02 40 AM" src="https://github.com/ktie1688/Binge-Worthy-Movie-Titles/assets/116592410/f4feb2c2-04a2-40d6-9670-1e0777d6354b">
<img width="485" alt="Screenshot 2024-04-01 at 9 03 04 AM" src="https://github.com/ktie1688/Binge-Worthy-Movie-Titles/assets/116592410/4ac92198-9a0b-4860-92d4-7b14660d21dc">

To make the recommender system actionable, I used k-Nearest Neighbors (kNN) to arrange the utility matrix based on the cosine similarity scores of its elements. Cosine similarity is a popular method of measuring similarity between context words in Natural Language Processing (NLP). I created a Python function that takes most similar user interaction vectors of movie i to recommend k movies. 

<img width="816" alt="Screenshot 2024-04-01 at 8 43 52 AM" src="https://github.com/ktie1688/Binge-Worthy-Movie-Titles/assets/116592410/d1e1711a-3691-44eb-a10e-1ecc30e118d1">

For testing, I inputted Toy Story (1995) and was recommended: 

<img width="1021" alt="Screenshot 2024-04-01 at 8 44 54 AM" src="https://github.com/ktie1688/Binge-Worthy-Movie-Titles/assets/116592410/32e7d1c1-228c-4127-b10d-96c862691c0b">

**Interestingly, these are all family friendly movies from the '90s.** *Not a bad start.*

However, because CF heavily relies on user ratings to even work, I needed to address what's known as the cold start problem. When there is not enough data in the utility matrix, no recommendations can be matched back to a specific movie, so to address this I added content-based filtering to gather that missing data with movie genres.

I took a similar approach of grouping the genres and passing through a cosine similarity function that would return a matrix of similar movies based on genre. Here, I discovered that for 9,500+ movies, there were 35 unique genre categories.

<img width="906" alt="Screenshot 2024-04-01 at 9 15 37 AM" src="https://github.com/ktie1688/Binge-Worthy-Movies/assets/116592410/40fc73d1-2d04-4f50-b7ef-db89e69da321">

Next, I decided to create a movie finder function that would allow me to specify a movie and then generate a user-specific number of recommendations. The problem with the current dataset is that the movie title strings are followed by dates (i.e. 'Toy Story (1995)' or 'Borrowers, The (1997)'), meaning a user would need to type that whole string out. For quality of life, I used the Python package fuzzywuzzy, which is uses a string matching algorithm that finds the most similar string inputted. 

Finally, testing the recommender system with Toy Story (1995), I was recommended:

<img width="1021" alt="Screenshot 2024-04-01 at 8 41 57 AM" src="https://github.com/ktie1688/Binge-Worthy-Movie-Titles/assets/116592410/a587c3ed-417f-48df-a5a1-fdded5e139f6">

## Conclusions & Findings
**Finished result is a working recommender system that takes a user specified movie title and number of recommendations parameter to return a list of similar movies they might like.** Further improvements can be made like adjusting parameters based on evaluation metrics (RMSE, Precision @K, Recall @K) or enriching the utility matrix with more movie and rating data. Performance can also be more efficient by implementing a compressed recommender system, which I shortly explored by using matrix factorization to identify any latent features and applying scikit-learn's Truncated SVD for dimension reduction.

Some key takeaways from this recommender project are that reliability is part and parcel with quality of recommendation. Without enough data, CF won't work unless complemented by initial data collection from content or even demographic filtering. Performance and resource management can become a matter of concern especially if working with large and computationally complex matrices, so optimization is also worth continued exploration. Finally, more attributes such as movie description, cast, crew, and time data could help further enhance recommendation quality and provide for more binge worthy results!
