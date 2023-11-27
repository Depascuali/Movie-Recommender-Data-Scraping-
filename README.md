# Recomendador-de-Peliculas


### Introduction

The goal of this code is to have some fun with Web Scraping on a real-life scenario. Ever found yourself wanting to watch a movie but totally clueless about what to pick? Well, by scraping Rotten Tomatoes Website, we can grab a random movie based on our favorite streaming service and genre. It's a simple but entertaining exercise using Beautiful Soup and HTML scraping.

### Installing libraries

```python

import requests
import random
from bs4 import BeautifulSoup
```

### Generate the input data and the URL to scrape
This way, we obtain the user's desired platform and genre input, which will modify the URL to be scraped, ensuring we always get movies with the specified characteristics.

```python

Plataforma = input("Plataforma (e.g., netflix, amazon_prime, disney_plus, max_us, apple_tv_us): ")
Genero = input("Genero (e.g., action, adventure, animation, anime, biography, comedy, crime, documentary, drama, fantasy, history, horror, musical, sci_fi, sports, war): ")
url = f'https://www.rottentomatoes.com/browse/movies_at_home/affiliates:{Plataforma}~genres:{Genero}~sort:popular'
```

### We retrieve all the movie names in the web along with their class.

```python
source_action = requests.get(url)
soup_action = BeautifulSoup(source_action.text,"html.parser")

movies = soup_action.find_all('span',{"class":'p--small'})
```

### We create a loop to allocate each name into the created list.
After creating 'best_movies' list, we use the loop to store each movie name in this list.

```python
best_movies = []

for movie in movies:

    best_movies.append(movie.text)
```

### We also create a base URL that will be later used to provide the generated movie URL to the user.

```python
url_base = "https://www.rottentomatoes.com/m/"
```

### We exclude the first 4 movies from the list and generate a random movie using the 'random' library.
The reason we exclude the first 4 movies is that, regardless of the chosen category or platform, 4 default movies are displayed that do not contribute to the analysis.

```python
movies_excluded = best_movies[4:]

pelicula_aleatoria = random.choice(movies_excluded).strip()
```

### We generate the URL of the movie.
The reason we replace spaces with underscores is because this is how Rotten Tomatoes considers spaces in the URL. The final URL is composed of the site's base URL and the movie name with the mentioned replacement.

```python
peli_url = pelicula_aleatoria.replace(" ","_")


url_final = url_base + peli_url
```

### We create the print statement where the user will get a random movie and its corresponding URL.

```python
print(f"Su película es: \033[1;91m{pelicula_aleatoria}\033[0m, puedes ver mas aqui:",url_final)
```

### For example...
Su película es: Snowpiercer, puedes ver mas aqui: https://www.rottentomatoes.com/m/Snowpiercer

In case the user, after analyzing the movie through the URL, is not convinced, they can rerun the code to get another movie.
