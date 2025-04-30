# AI-movie-Recommender
import seaborn as sns
import pandas as pd

df = pd.read_csv("/content/drive/MyDrive/IMDB_top_1000.csv")
df
import pandas as pd
import random

# Load the movie data
df = pd.read_csv("/content/drive/MyDrive/IMDB_top_1000.csv")

# Clean the movie titles by removing rank and year
df['Cleaned_Title'] = df['Title'].str.replace(r'\d+\.\s*', '', regex=True)  # Remove rank number
df['Cleaned_Title'] = df['Cleaned_Title'].str.replace(r'\(\d{4}\)', '', regex=True)  # Remove year in parentheses
df['Cleaned_Title'] = df['Cleaned_Title'].str.strip()  # Remove extra spaces

# Function to recommend 5 shuffled movies in the same genre
def recommend_movies(movie_title):
    movie_title = movie_title.strip()

    # Find the genre of the movie
    matched_movie = df[df['Cleaned_Title'].str.lower() == movie_title.lower()]
    if matched_movie.empty:
        return "❌ Movie not found. Please try a different title."

    # Get the genre of the matched movie
    movie_genre = matched_movie.iloc[0]['Genre']

    # Filter movies with the same genre (excluding the matched movie)
    similar_movies = df[(df['Genre'].str.contains(movie_genre)) & (df['Cleaned_Title'].str.lower() != movie_title.lower())]

    # Check if there are enough movies in the same genre
    if similar_movies.shape[0] < 5:
        shuffled_movies = similar_movies['Title'].tolist()  # Use all available movies if fewer than 5
    else:
        # Shuffle the similar movies and pick top 5
        shuffled_movies = similar_movies.sample(n=5, random_state=42)['Title'].tolist()

    if not shuffled_movies:
        return "❌ No similar movies found in the same genre."

    # Return the result
    result = f"✅ Matched movie: {movie_title}\n🎬 Recommended movies in the same genre:\n"
    for i, title in enumerate(shuffled_movies, 1):
        result += f"{i}. {title}\n"

    return result

# Get the movie input from user
movie = input("Enter a movie you like: ")
print(recommend_movies(movie))

