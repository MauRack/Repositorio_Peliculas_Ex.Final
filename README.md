# Repositorio_Peliculas_Ex.Final
Este es un repositorio de películas que puedes disfrutar en tu tiempo libre junto con tu familia y/o amigos
## Propósito:
Nuestro trabajo tiene como propósito reunir de distintas bases de datos la mayor cantidad de películas posibles y mostrar en qué plataformas se podrían visualizar.
## API’s a usar:
### Open Movie Database


# Películas y Series con OMDb API

Este proyecto es un script en Python que permite obtener información sobre películas o series a través de la [OMDb API](http://www.omdbapi.com/).

## Código

```python
import requests

# Configuración de la clave de la API y la URL
api_key = "538bda4b"
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

### Trak.tv
### The Movie Database


