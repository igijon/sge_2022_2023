#odoo #docker #postgresql 

# Odoo

[odoo - Official Image | Docker Hub](https://hub.docker.com/_/odoo)

# PostgreSQL

Iniciamos un server de postgreSQL que es el SGBD que usa Odoo.

```bash
docker run -d --restart="always" -e POSTGRES_USER=odoo -e POSTGRES_PASSWORD=odoo -e POSTGRES_DB=postgres --name db postgres:13
```

# Odoo

Para iniciar una instancia de Odoo que enlace con ese contenedor con PostgreSQL podemos hacer:

```bash
docker run -p 8069:8069 --name odoo --link db:db -t odoo
```

Para parar y arrancar el contenedor de odoo tenemos:

```jsx
docker start -a odoo
docker stop odoo
```

![Untitled](300%20📈%20SGE%202022-2023/00%20UD2%20Montar%20Odoo%20con%20Docker/Odoo/Untitled.png)

# Persistencia

Cuando se crea el contenedor de Odoo de la manera anterior, el almacén de archivos de Odoo se crea dentro del contenedor. Si se elimina, se pierde el almacén de archivos. La mejor forma de solventar esto es usar volúmenes.

```bash
docker run -d -v odoo-db:/var/lib/postgresql/data -e POSTGRES_USER=odoo -e POSTGRES_PASSWORD=odoo -e POSTGRES_DB=postgres --name db postgres:13
```

Con este comando, el volumen llamado odoo-db persistirá incluso si se elimina el contenedor y se puede reutilizar emitiendo el mismo comando.

La ruta /var/lib/postgresql/data utilizada como punto de montaje del volumen coincide con el directorio donde se conservan las bases de datos.

Para el caso de Odoo:

```bash
docker run -v odoo-data:/var/lib/odoo -d -p 8069:8069 --name odoo --link db:db -t odoo
```

La ruta /var/lib/odoo utilizada como punto de montaje del volumen debe coincidir con el directorio data_dir en el archivo de configuración de Odoo.

![Untitled](300%20📈%20SGE%202022-2023/00%20UD2%20Montar%20Odoo%20con%20Docker/Odoo/Untitled%201.png)

Vamos a probar a crear una BDD de prueba, con datos demo.

Una vez hecho esto, instalaremos algunos módulos: Inventario, Ventas, CRM, Facturación…

Después… si probamos a parar el contenedor y volverlo a activar… volvemos al estado que teníamos, es decir, no hemos perdido los datos.

![Untitled](300%20📈%20SGE%202022-2023/00%20UD2%20Montar%20Odoo%20con%20Docker/Odoo/Untitled%202.png)

# Ejecutar Odoo con una configuración personalizada

La configuración por defecto se encuentra en la instalación de Odoo en `/etc/odoo/odoo.conf`. Puede ser sobreescrita al iniciar utilizando volúmenes distintos.

El archivo plantilla de configuración puedes encontrarlo aquí:

[docker/odoo.conf at master · odoo/docker](https://github.com/odoo/docker/blob/master/15.0/odoo.conf)

```bash
docker run -v odoo-data:/var/lib/odoo -v /Users/inma/Documents/config_odoo:/etc/odoo -d -p 8069:8069 --name odoo --link db:db -t odoo
```

En este caso, cargará el fichero `odoo.conf` que se encuentre en la ruta `/Users/inma/Documents/config_odoo` porque estoy diciendo que lo mapee con el directorio `/etc/odoo` del contenedor, por lo tanto leerá de mi directorio ese fichero de configuración.

![Untitled](300%20📈%20SGE%202022-2023/00%20UD2%20Montar%20Odoo%20con%20Docker/Odoo/Untitled%203.png)

# Montando addons personalizados

Cuando comencemos a desarrollar, lo haremos en un directorio de paquetes que será nuestro directorio de desarrollo… no sérá el directorio normal, por lo que en el fichero de configuración indicaremos el directorio que tiene que cargar para los addons que estemos programando. Haremos lo mismo que en el caso anterior, lo mapearemos con el directorio local del anfitrión en el que estemos programando y levantaremos el contenedor con este comando:

```bash
docker run -v odoo-data:/var/lib/odoo -v /Users/inma/Documents/config_odoo:/etc/odoo -v /path/to/addons:/mnt/extra-addons -d -p 8069:8069 --name odoo --link db:db -t odoo
```

# Ejecutar múltiples instancias de Odoo

Cuando estemos desarrollando y haciendo pruebas, puede interesarnos levantar varias instancias simultáneas de Odoo para ver cambios… podemos hacerlo así:

```bash
docker run -p 8070:8069 --name odoo2 --link db:db -t odoo
docker run -p 8071:8069 --name odoo3 --link db:db -t odoo
```

# Docker Compose

Docker Compose es una herramienta para definir y ejecutar aplicaciones de Docker de varios contenedores. Orquesta contenedores!

En Compose, se utiliza un archivo YAML para configurar los servicios de la aplicación. Después, con un solo comando, se crean e inician todos los servicios de la configuración.

En un directorio crearemos un fichero `docker-compose.yaml`:

```yaml
version: '3.1'
services:
  web:
    image: odoo:15.0
    depends_on:
      - db
    ports:
      - "8069:8069"
    volumes:
      - odoo-web-data:/var/lib/odoo
      - ./config:/etc/odoo
      - ./addons:/mnt/extra-addons
    environment:
      - HOST=db
      - USER=odoo
      - PASSWORD=myodoo
  db:
    image: postgres:13
    environment:
      - POSTGRES_DB=postgres
      - POSTGRES_PASSWORD=myodoo
      - POSTGRES_USER=odoo
      - PGDATA=/var/lib/postgresql/data/pgdata
    volumes:
      - odoo-db-data:/var/lib/postgresql/data/pgdata

volumes:
  odoo-web-data:
  odoo-db-data:
```

En el mismo directorio tendremos dos directorios:

- `config`: donde tendremos el fichero `odoo.conf` que queremos cargar para arrancar odoo.
- `addons`: donde tendremos los distintos módulos que desarrollaremos para odoo.

![Untitled](300%20📈%20SGE%202022-2023/00%20UD2%20Montar%20Odoo%20con%20Docker/Odoo/Untitled%204.png)

![Untitled](300%20📈%20SGE%202022-2023/00%20UD2%20Montar%20Odoo%20con%20Docker/Odoo/Untitled%205.png)