# Docker 3 - Almacenamiento en Docker

> Javier González Díaz

## Ejemplo 2: Contenedor mariadb con almacenamiento persistente

Si estudiamos la documentación de la imagen mariadb en Docker Hub, nos indica que podemos
crear un contenedor con información persistente de mariadb, de la siguiente forma

```bash
$ docker run --name some-mariadb -v /home/$USER/datadir:/var/lib/mysql -e
MYSQL_ROOT_PASSWORD=my-secret-pw -d mariadb
```

![](Tarea.assets/Captura desde 2024-01-26 10-20-26.png)

Es decir, se va a crear un directorio /home/usuario/datadir en el host, donde se va a guardar la
información de la base de datos. Si tenemos que crear de nuevo el contenedor indicaremos ese
directorio como bind mount y volveremos a tener accesible la información

```bash
$ cd datadir
$ ls
```

![Captura desde 2024-01-26 10-08-19](Tarea.assets/Captura desde 2024-01-26 10-08-19.png)

```bash
$ docker exec -it some-mariadb bash -c 'mariadb -u root -p$MYSQL_ROOT_PASSWORD'
```

![Captura desde 2024-01-26 10-11-55](Tarea.assets/Captura desde 2024-01-26 10-11-55.png)

```bash
$ docker rm -f some-mariadb
```

![Captura desde 2024-01-26 10-14-00](Tarea.assets/Captura desde 2024-01-26 10-14-00.png)

```bash
$ docker run --name some-mariadb -v /home/$USER/datadir:/var/lib/mysql -e
MYSQL_ROOT_PASSWORD=my-secret-pw -d mariadb
```

![Captura desde 2024-01-26 10-20-26](Tarea.assets/Captura desde 2024-01-26 10-20-26.png)



## Ejercicios

### Volúmenes

1.Crea un volumen docker que se llame **miweb** .

```bash
$ docker volume create miweb
```

![image-20240205094208321](./assets/image-20240205094208321.png)

2.Crea un contenedor desde la imagen php:7.4-apache donde montes en el directorio /var/www/html (que sabemos que es el DocumentRoot del servidor que nos ofrece esa imagen) el volumen docker que has creado.

```bash
$ docker run -d -p 8080:80 -v miweb:/var/www/html php:7.4-apache
```

![image-20240205094509754](./assets/image-20240205094509754.png)

3.Utiliza el comando docker cp para copiar un fichero index.html en el directorio /var/www/html .

```bash
$ docker cp index.html gallant_hawking:/var/www/html/
```

![image-20240205094901561](./assets/image-20240205094901561.png)

4.Accede al contenedor desde el navegador para ver la información ofrecida por el fichero index.html .

![image-20240205095018555](./assets/image-20240205095018555.png)

5.Borra el contenedor

```bash
$ docker rm gallant_hawking -f
```

![image-20240205095053446](./assets/image-20240205095053446.png)

6.Crea un nuevo contenedor y monta el mismo volumen como en el ejercicio anterior.

```bash
docker run -d -p 8080:80 --name contenedor2 -v miweb:/var/www/html php:7.4-apache
```

![image-20240205095304123](./assets/image-20240205095304123.png)

7.Accede al contenedor desde el navegador para ver la información ofrecida por el fichero index.html . ¿Seguía existiendo ese fichero?

![image-20240205095354552](./assets/image-20240205095354552.png)

### Bind Mount

1. Crea un directorio en tu host y dentro crea un fichero index.html .

   ```bash
   $ mkdir mi_directorio
   $ cd mi_directorio
   $ touch index.html
   ```

   ![Captura desde 2024-02-02 09-27-08](Tarea.assets/Captura desde 2024-02-02 09-27-08.png)

2. Crea un contenedor desde la imagen php:7.4-apache donde montes en el directorio

/var/www/html el directorio que has creado por medio de bind mount .	

```bash
$ docker run -d -p 8080:80 --name mi_contenedor -v $(pwd)/mi_directorio:/var/www/html php:7.4-apache
```

![Captura desde 2024-02-02 09-32-57](Tarea.assets/Captura desde 2024-02-02 09-32-57.png)

3. Accede al contenedor desde el navegador para ver la información ofrecida por el fichero

index.html .

![Captura desde 2024-02-02 09-36-39](Tarea.assets/Captura desde 2024-02-02 09-36-39.png)

4. Modifica el contenido del fichero index.html en tu host y comprueba que al refrescar la

página ofrecida por el contenedor, el contenido ha cambiado.

![Captura desde 2024-02-02 09-38-08](Tarea.assets/Captura desde 2024-02-02 09-38-08.png)

5. Borra el contenedor

```bash
$ docker stop mi_contenedor
$ docker rm mi_contenedor
```

![Captura desde 2024-02-02 09-40-30](Tarea.assets/Captura desde 2024-02-02 09-40-30.png)

6. Crea un nuevo contenedor y monta el mismo directorio como en el ejercicio anterior.

```bash
$ docker run -d -p 8080:80 --name mi_contenedor -v $(pwd)/mi_directorio:/var/www/html php:7.4-apache
```

![Captura desde 2024-02-02 09-32-57](Tarea.assets/Captura desde 2024-02-02 09-32-57-1706863293820-6.png)

7. Accede al contenedor desde el navegador para ver la información ofrecida por el fichero

index.html . ¿Se sigue viendo el mismo contenido?

Sí
