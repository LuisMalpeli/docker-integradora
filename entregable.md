# Fundamentos y usos prácticos de Docker

## Entregable trabajo integrador: [Su nombre y apellido]


## Parte 1 - Conteinerizar una Aplicación


### Creando la imágen

- Ejecute el comando correspondiente para buildear la imágen. Elija un nombre de imágen y un tag acorde. 

    ```bash
    # docker build -t docker-integradora .
    ```
- Muestre cuánto espacio ocupa la imaǵen una vez creada.

    ```bash
    # docker image ls
    ```
- ¿Puede hacer algo para optimizar o mejorar la imágen?. Describa qué modificaciones puede hacer para optimizar la imágen.

_(Describa que modificaciones podría hacer para mejorar la imágen de ser posible)_

Se podria utilizar una imagen base mas ligera y descargar las herramientas especificas que se vayan a utilizar.


### Correr la aplicación

Una vez creada la imágen, debería ser capaz de correr la aplicación.

- Ejecute un comando para poder correr la aplicación.
    ```bash
    # docker run -d -p 8080:3000 docker-integradora
    ```
- Explique el comando y cada parámetro enviado.

    _(Realice una explicación de los parámetros aquí)_

    El parametro (-d) me permite correr el contenedor sin ocupar la ventana de la terminal.
    El parametro (-p) me permite especificar que puerto expuesto del contenedor es accesible por la maquina host. El puerto 8080 de la maquina host accede al puerto 3000 del contenedor.

- Muestre una captura de pantalla o un copy-paste del contenedor corriendo.

    _(Inserte aquí la captura de pantalla o los la salida de la shell con el contenedor corriendo)_
    ![Screenshot](./imgs/docker%20run%20terminal.png)

- Adjunte una captura de pantalla con la aplicación funcionando con la URL utilizada para acceder.

    ![Screenshot](./imgs/docker%20run%208080.png.png)


## Parte 2 - Actualizar aplicación (imágen)

### 1. Actualizar el código fuente

- Ejecutemos los comando necesarios para que la aplicación tome los cambios. Realice un etiquetado (tag) coherente respecto a los cambios en la imágen
    
    ```bash
    # docker tag docker-integradora docker-integradora:v2
    # docker build -t docker-integradora:v2 .
    # docker run -d -p 8080:3000 docker-integradora:v2
    ```

- Mostrar captura de pantalla con la app corriendo con las modificaciones realizadas.

    ![Screenshot](./imgs/docker%20tag%20v2.png.png)

> La actualizaciones realizadas, dejan a la primera versión obsoleta

### 2. Elimine el contenedor e imágen anterior

- Elimine la imágen y el contenedor hecho en el punto anterior

    ```bash
    # docker image rm docker-integradora:latest
    # docker container rm b4ba7c6a6c9e
    ```

- Liste las imágenes y contenedores para ver que ya no existen.

    ```bash
    # docker container ls
    # docker image ls
    ```


## Parte 3 - Compartir app

Para compartir la imágen de la aplicación usaremos la registry de [DockerHub](https://hub.docker.com/).

> [!TIP]
> Repase lo realizado en el [Laboratorio 2.3](https://github.com/kity-linuxero/docker_410_practicas/blob/main/labs/02-conceptos-basicos/23-images-push.md#3-subimos-a-la-registry).


- Escriba los comandos necesarios para que sea posible subir la imaǵen correctamente.

    ```bash
    # docker image tag docker-integradora:v2 malpeliLuis/docker-integradora:latest
    # docker login
    # docker push docker.io/malpeliluis/docker-integradora:latest
    ```

- Comparta la URL de DockerHub para que pueda ser posible probar y descargar su imágen.

    [Actualice el link](https://hub.docker.com/r/malpeliluis/docker-integradora)

- Agregue un _overview_ para el repositorio de Dockerhub con instrucciones para correr la imágen y todo lo que considere necesario para que un tercero pueda ejecutar la imágen.

> [!TIP]
> Utilice el formato [markdown](https://docs.github.com/es/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax) para darle formato al overview.


## Parte 4 - Persistencia de datos

Los datos en esta APP se guardan en un archivo `/etc/todos/todo.db`.

- Escriba los comandos utilizados para realizar lo solicitado con la explicación correspondiente.

    ```bash
    # docker volume create data
    # docker run -d -p 8080:3000 --mount type=bind,target="$(pwd)/todo.db",target="/etc/todos/todo.db" docker-integradora:v2
    # docker run -d -p 8080:3000 -v data:/etc/todos/todo.db docker-integradora:v2
    ```

- Decida que tipo de persistencia es la adecuada para la app.

Utilizo volumenes 

> [!TIP]
> Repase [volúmenes y persistencia](https://docker.idepba.com.ar/clase4.html#/volumenes) de datos.


## Parte 5 - Aplicaciones multicontainer


- [Crear una red](https://docker.idepba.com.ar/clase4.html#/network_create) para conexión entre los contenedores que servirá también para conectar a la aplicación.

    ```bash
    # docker network create red-integradora
    ```
- [Crear un nuevo volumen](https://docker.idepba.com.ar/clase4.html#/volume_create) para persistir los datos de la base MySQL. El path donde se almacenan los datos en el contenedor MySQL es `/var/lib/mysql`.
    
    ```bash
    # docker volume create data
    ```
- Iniciar el contenedor de la aplicación utilizando el comando `docker run` enviando las variables de entornos necesarias para la conexión con la base de datos.

    ```bash
    # docker run --name mysql -v data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=my-secret-pw,MYSQL_DATABASE=todos -d mysql:8.0

    ```

> [!TIP]
> Set environments variables (-e, --env) [Docker Docs](https://docs.docker.com/reference/cli/docker/container/run/#env).




## Parte 6 - Utilizando Docker Compose

En la carpeta raíz del proyecto, cree un archivo de docker compose `compose.yml` o `docker-compose.yml`. Adicionalmente pégue el contenido del archivo `compose` en este lugar:

```compose
# Copie aquí el contenido del archivo compose.
```

> [!IMPORTANT]  
> El instructor debe ejecutar el comando `docker compose up` y la aplicación debe descargarse y ejecutarse correctamente.

----


<p align="center">
  <img src="./imgs/logos.footer.gray.webp">
</p>




