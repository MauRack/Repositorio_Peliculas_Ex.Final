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
    <title>Buscador de Películas</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f4f4f4;
            margin: 0;
            padding: 20px;
        }
        .movie {
            border: 1px solid #ccc;
            background: #fff;
            padding: 15px;
            margin-bottom: 10px;
        }
    </style>
</head>
<body>
    <h1>Buscador de Películas</h1>
    <div id="results">
        <!-- Resultados dinámicos aquí -->
    </div>

    <script>
        async function buscarPeliculas(query) {
            const response = await fetch(`https://api.example.com/search?q=${query}`);
            const data = await response.json();
            const resultsDiv = document.getElementById('results');
            resultsDiv.innerHTML = '';

            data.results.forEach(movie => {
                const movieDiv = document.createElement('div');
                movieDiv.classList.add('movie');
                movieDiv.innerHTML = `
                    <h2>${movie.title}</h2>
                    <p>${movie.overview}</p>
                `;
                resultsDiv.appendChild(movieDiv);
            });
        }

        // Ejemplo: Llamar a la función con un término de búsqueda
        buscarPeliculas('Matrix');
    </script>
</body>
</html>
```

Este archivo permite mostrar dinámicamente la información de las películas buscadas mediante una API.