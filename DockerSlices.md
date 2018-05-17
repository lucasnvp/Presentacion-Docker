title: Docker
author:
  name: Lucas Visser
  twitter: lucasnvp
  url: https://fiqus.com/
output: dockerslides.html
controls: true

--

# Docker

## "Quiero dockerizar todos los ambientes posibles que use Fiqus"

--

### Porque usar Docker?

Nos sirve para aislar el entorno de desarrollo, test y producción; Tanto como en sistemas operativos como versiones de frameworks y lenguajes.

--

### Vamos por un ejemplo? 

* `docker container run -ti debian bash`

Que hace -ti? ti = terminal interactiva. Lo utilizamos para poder ingresar al contenedor.

--

### Comandos utiles

* `docker container ps`

* `docker container inspect nombreDelContainer`

* `docker container start/stop/kill`

* `docker container attach`

* `docker container logs`

* `docker container cp`

--

### Volume

Son el lugar donde los contenedores persisten la data.

Para crear un volume:

* `docker volume create nombreDelVolume`

* `docker volume ls`

* `docker volume inspect nombreDelVolume`

--

### Dockerfile

Todo muy bonito, pero si quiero automatizar todo un poco mas?

--

### Ejemplo de Dockerfile

```bash
# Imagen base
FROM haskell:8
# Variable de la carpeta
ENV APP_DIR=/app
# Creo la carpeta
RUN mkdir ${APP_DIR}
# Carpeta en la cual se va a iniciar
WORKDIR ${APP_DIR}
# Agrego todos los archivos a esa carpeta
ADD . ${APP_DIR}
```

--

### Docker Compose

Que pasa si mi proyecto tiene que usar más de un contenedor?

```bash
version: "2"

services:
  db:
    extends:
      file: base.yml
      service: db

  web:
    extends:
      file: base.yml
      service: web
    build:
      args:
        - DJANGO_ENV=dev
    command: python manage.py runserver 0.0.0.0:8000
    volumes:
      - ./webapp:/webapp
    ports:
      - "8000:8000"
    depends_on:
      - db

volumes:
  db_data:
    external: true
```

--

# Preguntas??