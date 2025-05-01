df = pd.read_csv("/content/IMDB_top_1000.csv")
df = df[ ['Title', 'Genre' ]] .dropna()
def recommend_movies(movie_title):
movie_title = movie_title. lower()
matched movie = None
for title in df[ 'Title']:
if movie_title in title.lower():
matched movie = title
break
if not matched movie:
return "Movie not found. Try another title."
movie_genre = df[df['Title' ] == matched_movie] [ 'Genre' ] . values [0]
similar_movies =
for index, row in df.iterrows():
if row[ 'Title'] != matched_movie: # Skip the movie itself
if any(genre in row[ 'Genre'] for genre in movie_genre.split(', ')):
similar_movies.append(row['Title'])
top_5 = similar_movies[:5]
result = f"Matched movie: {matched_movie}\n"
result += "Top similar movies: \n"
for i, title in enumerate(top_5, 1):
result += f"{i}. {title}\n"
return result
movie = input("Enter a movie you like: ")
print(recommend_movies(movie))
