# Repositorio_Peliculas_Ex.Final
Este es un repositorio de películas que puedes disfrutar en tu tiempo libre junto con tu familia y/o amigos
## Propósito:
Nuestro trabajo tiene como propósito reunir de distintas bases de datos la mayor cantidad de películas posibles y mostrar en qué plataformas se podrían visualizar.
## API’s a usar:

# Películas y Series con OMDb API

Este proyecto es un script en Python que permite obtener información sobre películas o series a través de la [OMDb API](http://www.omdbapi.com/).

## Código

```python
import requests

# Configuración de la clave de la API y la URL
api_key = "538bda4b" # El usuario debe registrarse en la página e ingresar su clave API
movie_title = input("Ingrese la película o serie: ")  # Título de la película
api_url = f"http://www.omdbapi.com/?apikey={api_key}&t={movie_title}"

# Realizar la solicitud GET a la API
response = requests.get(api_url)

# Verificar si la solicitud fue exitosa
if response.status_code == 200:
    movie_data = response.json()
    
    # Verificar si la película fue encontrada
    if movie_data.get("Response") == "True":
        # Mostrar información de la película
        print("Título:", movie_data.get("Title"))
        print("Año:", movie_data.get("Year"))
        print("Género:", movie_data.get("Genre"))
        print("Director:", movie_data.get("Director"))
        print("Actores:", movie_data.get("Actors"))
        print("Trama:", movie_data.get("Plot"))
        print("Poster URL:", movie_data.get("Poster"))
    else:
        print("Error:", movie_data.get("Error"))
else:
    print("Error en la solicitud:", response.status_code)
```
El API devuelve el título, año, género, director, actores, trama y un poster de la película


#  Películas y Series con TMDB API
Este script en Python utiliza la API de **TMDB** (The Movie Database) para buscar información sobre películas o series, como su título, sinopsis, calificación, fecha de estreno y las plataformas de streaming disponibles en una región específica. A continuación, el código completo:

```python
import requests

# Configuración
api_key = "f6d6d0150dc01c08d2895f0689aba2c3"  # Reemplaza con tu clave de TMDB
base_url = "https://api.themoviedb.org/3"

def buscar_pelicula(movie_name, region="ES"):
    """
    Busca una película en TMDB por su nombre y muestra detalles como
    título, sinopsis, calificación y fecha de estreno. También llama
    a la función obtener_plataformas para listar las plataformas de streaming.
    """
    # Paso 1: Buscar la película y obtener su ID
    search_url = f"{base_url}/search/movie"
    search_params = {
        "api_key": api_key,
        "query": movie_name,
        "language": "es-ES"
    }

    search_response = requests.get(search_url, params=search_params)
    if search_response.status_code == 200:
        search_data = search_response.json()
        results = search_data.get("results", [])
        if results:
            # Obtener el primer resultado
            movie = results[0]
            movie_id = movie["id"]
            print(f"Película encontrada: {movie['title']} (ID: {movie_id})")
            print(f"Fecha de estreno: {movie.get('release_date', 'No disponible')}")
            print(f"Sinopsis: {movie.get('overview', 'No disponible')}")
            print(f"Calificación: {movie.get('vote_average', 'No disponible')} / 10")
            
            # Paso 2: Obtener plataformas de streaming
            obtener_plataformas(movie_id, region)
        else:
            print("No se encontraron resultados para la película.")
    else:
        print("Error en la solicitud de búsqueda:", search_response.status_code)

def obtener_plataformas(movie_id, region="ES"):
    """
    Obtiene y muestra las plataformas de streaming, compra o alquiler
    disponibles para una película en una región específica.
    """
    # Endpoint para obtener plataformas
    providers_url = f"{base_url}/movie/{movie_id}/watch/providers"
    providers_response = requests.get(providers_url, params={"api_key": api_key})
    if providers_response.status_code == 200:
        providers_data = providers_response.json()
        results = providers_data.get("results", {}).get(region, {})
        
        flatrate = results.get("flatrate", [])
        buy = results.get("buy", [])
        rent = results.get("rent", [])
        
        if flatrate or buy or rent:
            print("\n--- Plataformas disponibles ---")
            if flatrate:
                print("Disponible para streaming en:")
                for provider in flatrate:
                    print(f"- {provider['provider_name']}")
            if buy:
                print("\nDisponible para compra en:")
                for provider in buy:
                    print(f"- {provider['provider_name']}")
            if rent:
                print("\nDisponible para alquiler en:")
                for provider in rent:
                    print(f"- {provider['provider_name']}")
        else:
            print("\nNo se encontró información de plataformas para esta película.")
    else:
        print("Error al obtener plataformas:", providers_response.status_code)

# Entrada del usuario
movie_name = input("Ingrese el nombre de la película o serie: ")
region = "ES"  # Región predeterminada: España
buscar_pelicula(movie_name, region)
```
El API devuelve el título, fecha de estreno, sinopsis, calificación y plataformas de disponibilidad


# Trakt API Movie/Series Info

Este script de Python permite obtener información detallada sobre películas y series utilizando la API de Trakt. El código solicita al usuario el nombre de una película o serie y muestra detalles como el título, año de lanzamiento, sinopsis y puntuación.

## Funcionamiento

1. El script solicita al usuario que ingrese el nombre de una película o serie.
2. Luego, reemplaza los espacios por guiones y convierte el nombre a minúsculas para ajustarse al formato requerido por la API de Trakt.
3. El script realiza una solicitud HTTP a la API de Trakt usando el nombre de la película/serie y obtiene la información detallada.
4. Finalmente, imprime el título, año de lanzamiento, sinopsis y puntuación de la película/serie.

## Código

```python
import requests

# Configura tu API key
CLIENT_ID = 'f1e01b686947368b3f63b6ccb47e16f3ccec02633762ba04ba3b7a8033df45e9'

# Solicitar el nombre de la película o serie
MOVIE_NAME = input('Escribe el nombre de la película o serie: ').strip()

# Reemplazar espacios por guiones para coincidir con el formato requerido por la API
MOVIE_ID = MOVIE_NAME.replace(" ", "-").lower()

# Endpoint de la API
url = f'https://api.trakt.tv/movies/{MOVIE_ID}?extended=full'

# Encabezados requeridos
headers = {
    'Content-Type': 'application/json',
    'trakt-api-version': '2',
    'trakt-api-key': CLIENT_ID
}

# Realizar la petición
response = requests.get(url, headers=headers)

# Procesar la respuesta
if response.status_code == 200:
    data = response.json()
    print("\nTítulo:", data['title'])
    print("Año:", data['year'])
    print("Sinopsis:", data['overview'])
    print("Puntuación:", data['rating'])
else:
    print("Error:", response.status_code, response.text)
```
Finalmente, el API devuelve el título, año, sinopsis y puntuación dada a la película

# Integración con HTML

Además de las APIs mencionadas, el proyecto incluye una página HTML interactiva para mostrar resultados en un entorno visual y amigable.

## Código

```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Información de Películas y Series</title>
    <link href="https://fonts.googleapis.com/css2?family=Lora:wght@400;700&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Lora', serif;
            background-color: #121212;
            color: #fff;
            margin: 0;
            padding: 0;
        }

        .header {
            background-color: #181818;
            padding: 20px;
            text-align: center;
        }

        .header h1 {
            color: #fff;
            font-size: 36px;
        }

        .search-bar {
            display: flex;
            justify-content: center;
            margin-top: 30px;
        }

        .search-bar input {
            width: 50%;
            padding: 10px;
            font-size: 16px;
            margin-right: 10px;
            border: none;
            border-radius: 5px;
        }

        .search-bar button {
            padding: 10px 20px;
            font-size: 16px;
            background-color: #007BFF;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
        }

        .search-bar button:hover {
            background-color: #0056b3;
        }

        .content {
            margin-top: 30px;
            display: flex;
            flex-wrap: wrap;
            justify-content: center;
        }

        .movie-container {
            width: 250px;
            margin: 15px;
            background-color: #222;
            border-radius: 8px;
            overflow: hidden;
            color: #fff;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
        }

        .movie-container img {
            width: 100%;
            height: 350px;
            object-fit: cover;
        }

        .movie-details {
            padding: 15px;
        }

        .movie-title {
            font-size: 18px;
            font-weight: bold;
            margin-bottom: 10px;
        }

        .rating {
            color: #FFD700;
        }

        .genres, .platforms {
            font-size: 14px;
            color: #ccc;
        }

        .movie-details p {
            font-size: 14px;
            color: #aaa;
        }
    </style>
</head>
<body>
    <div class="header">
        <h1>🎬 Información de Películas y Series</h1>
    </div>

    <div class="search-bar">
        <input id="movie-input" type="text" placeholder="Buscar película o serie...">
        <button onclick="searchMovies()">Buscar</button>
    </div>

    <div class="content" id="content">
    </div>

    <script>
        const apiKeyTMDb = 'f6d6d0150dc01c08d2895f0689aba2c3';
        const apiKeyOMDB = '538bda4b';
        const CLIENT_ID_Trakt = 'f1e01b686947368b3f63b6ccb47e16f3ccec02633762ba04ba3b7a8033df45e9';

        async function searchMovies() {
            const movieName = document.getElementById('movie-input').value.trim();
            if (!movieName) {
                alert('Por favor, ingresa el nombre de una película o serie.');
                return;
            }

            const resultsTMDb = searchTMDb(movieName);
            const resultsOMDB = searchOMDB(movieName);
            const resultsTrakt = searchTrakt(movieName);

            Promise.all([resultsTMDb, resultsOMDB, resultsTrakt]).then(([tmdbResults, omdbResults, traktResults]) => {
                displayAllMoviesAndSeries([...tmdbResults, ...omdbResults, ...traktResults]);
            });
        }

        async function searchTMDb(movieName) {
            const url = `https://api.themoviedb.org/3/search/multi?api_key=${apiKeyTMDb}&query=${encodeURIComponent(movieName)}&language=es-ES`;
            try {
                const response = await fetch(url);
                const data = await response.json();
                const detailedResults = await Promise.all(data.results.map(async item => {
                    if (item.id && item.media_type === 'movie') {
                        const detailsUrl = `https://api.themoviedb.org/3/movie/${item.id}?api_key=${apiKeyTMDb}&language=es-ES&append_to_response=watch/providers`;
                        const detailsResponse = await fetch(detailsUrl);
                        const detailsData = await detailsResponse.json();
                        const platforms = detailsData["watch/providers"]?.results?.ES?.flatrate?.map(provider => provider.provider_name).join(', ') || 'No disponible';
                        return { ...item, platforms };
                    }
                    return item;
                }));
                return detailedResults;
            } catch (error) {
                console.error('Error al buscar en TMDb:', error);
                return [];
            }
        }

        async function searchOMDB(movieName) {
            const url = `http://www.omdbapi.com/?apikey=${apiKeyOMDB}&t=${encodeURIComponent(movieName)}&language=es`;
            try {
                const response = await fetch(url);
                const data = await response.json();
                return data.Response === "True" ? [data] : [];
            } catch (error) {
                console.error('Error al buscar en OMDB:', error);
                return [];
            }
        }

        async function searchTrakt(movieName) {
            const url = `https://api.trakt.tv/search/movie?query=${encodeURIComponent(movieName)}&type=movie&extended=full`;
            const headers = {
                'Content-Type': 'application/json',
                'trakt-api-version': '2',
                'trakt-api-key': CLIENT_ID_Trakt
            };

            try {
                const response = await fetch(url, { headers });
                const data = await response.json();
                return data || [];
            } catch (error) {
                console.error('Error al buscar en Trakt.tv:', error);
                return [];
            }
        }

        function displayAllMoviesAndSeries(results) {
            const contentContainer = document.getElementById('content');
            contentContainer.innerHTML = '';

            if (!results || results.length === 0) {
                contentContainer.innerHTML = '<p>No se encontraron resultados.</p>';
                return;
            }

            results.forEach(item => {
                const genres = item.Genre || (item.genre_ids ? item.genre_ids.map(id => genreMap[id]).join(', ') : 'No disponible');
                const platforms = item.platforms || 'No disponible';

                const itemElement = `
                    <div class="movie-container">
                        <img src="${item.Poster || (item.poster_path ? 'https://image.tmdb.org/t/p/w500' + item.poster_path : 'https://via.placeholder.com/200x300')}" alt="Poster">
                        <div class="movie-details">
                            <div>
                                <div class="movie-title">${item.Title || item.title || item.name} (${item.Year || 'N/A'})</div>
                                <div class="rating">⭐ Calificación: ${item.imdbRating || item.vote_average || 'No disponible'}/10</div>
                                <div class="genres"><strong>Géneros:</strong> ${genres}</div>
                                <p><strong>ID:</strong> ${item.imdbID || item.id || 'No disponible'}</p>
                                <p>${item.Plot || item.overview || 'Sin sinopsis disponible'}</p>
                            </div>
                            <div class="platforms"><strong>Plataformas:</strong> ${platforms}</div>
                        </div>
                    </div>
                `;
                contentContainer.innerHTML += itemElement;
            });
        }

        const genreMap = {
            28: 'Acción',
            12: 'Aventura',
            16: 'Animación',
            35: 'Comedia',
            80: 'Crimen',
            99: 'Documental',
            18: 'Drama',
            10751: 'Familiar',
            14: 'Fantasía',
            36: 'Historia',
            27: 'Terror',
            10402: 'Música',
            9648: 'Misterio',
            10749: 'Romance',
            878: 'Ciencia Ficción',
            10770: 'Película de TV',
            53: 'Suspenso',
            10752: 'Bélica',
            37: 'Western'
        };
    </script>
</body>
</html>
```

Este archivo permite mostrar dinámicamente la información de las películas buscadas mediante una API.
