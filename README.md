# Repositorio_Peliculas_Ex.Final
Este es un repositorio de películas que puedes disfrutar en tu tiempo libre junto con tu familia y/o amigos
## Propósito:
Nuestro trabajo tiene como propósito reunir de distintas bases de datos la mayor cantidad de películas posibles y mostrar en qué plataformas se podrían visualizar.
## API’s a usar:
### Open Movie Database



import requests
api_key = "538bda4b"
movie_title = input( "ingrese la pelicula o serie ")  # Título de la película
api_url = f"http://www.omdbapi.com/?apikey={api_key}&t={movie_title}"
response = requests.get(api_url)

if response.status_code == 200:
    movie_data = response.json()
    if movie_data.get("Response") == "True":
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

### Trak.tv
### The Movie Database


