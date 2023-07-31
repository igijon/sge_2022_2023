#docker #docker-compose #odoo #postgresql
# 6.1. Configuración inicial. Primeros pasos

Arrancamos Docker Compose tal y como lo teníamos configurado (Ver unidad 2).

Instalamos los módulos CRM, Ventas, Compras, Facturación.

Ahora que vamos a programar necesitamos saber dónde se va a almacenar el .log, para ello lo tenemos que establecer en el fichero .conf (antes de arrancar Docker compose para que coja la configuración).

`logfile = /var/log/odoo/odoo-server.log`

<aside>
💡 Tenemos la opción de mapearlo al igual que hacemos con el fichero .conf y los addons.

</aside>

```docker
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
      - ./log:/var/log/odoo
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

<aside>
💡 Con una búsqueda por WARN podemos ver los warnings.

</aside>

![Untitled](300%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%201%20Configuración%20inicial%20Primeros%20pasos/Untitled.png)

En el fichero .conf, podemos ver entre otras cosas como sabemos: usuario, usuario de la BDD y contraseña.

Tenemos un servicio web y lo que vemos en el navegador es un cliente web que es una aplicación en JS muy completa. Si abrimos las herramientas de desarrollo en el navegador, en la pestaña de red y refrescamos:

![Untitled](300%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%201%20Configuración%20inicial%20Primeros%20pasos/Untitled%201.png)

Lo primero que demanda es un web/ que envía una página html que básicamente inicia el cliente web:
Luego comienza a descargar css y js del servidor.
Utiliza paradigmas de programación moderna y configura un SPA.
El servicio Odoo se amplía mediante módulos que están en la ruta indicada en el .conf: /opt/odoo/odoo/addons (nosotros lo tenemos mapeado a nuestro directorio)

Tenemos un directorio para cada módulo. El módulo base y el web serían unos de ellos.
Si quiero hacer un módulo para Odoo, bastaría con crear un directorio y hacerlo ahí directamente pero hay un problema, ahí están los módulos oficiales y es mejor tener los módulos provisionales o los módulos en desarrollo en un directorio diferente.

<aside>
💡 Como estamos usando Docker, está preparado el entorno para desarrollo. En el fichero .conf tenemos la siguiente configuración:
`addons_path = /mnt/extra-addons`
A su vez, en docker-compose.yaml tenemos mapeado dicho directorio de la siguiente manera:
`./addons:/mnt/extra-addons`

</aside>

Para crear un módulo nuevo de ejemplo, vamos a abrir el terminal del contenedor de odoo y ejecutamos el siguiente comando dentro del directorio de módulos:

![Untitled](300%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%201%20Configuración%20inicial%20Primeros%20pasos/Untitled%202.png)

Tenemos que comprobar que lo vemos desde el directorio mapeado:

![Untitled](300%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%201%20Configuración%20inicial%20Primeros%20pasos/Untitled%203.png)

![Untitled](300%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%201%20Configuración%20inicial%20Primeros%20pasos/Untitled%204.png)

Vamos a ver si podemos instalarlo desde la aplicación (aunque aún no tiene funcionalidad). Este módulo se ha creado simplemente con el esqueleto de un módulo base para Odoo.

Para instalarlo tenemos que tener **********el modo de desarrollador activado en ajustes.**********

![Untitled](300%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%201%20Configuración%20inicial%20Primeros%20pasos/Untitled%205.png)

![Untitled](300%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%201%20Configuración%20inicial%20Primeros%20pasos/Untitled%206.png)

Ya podemos instalarlo. Además, podemos actualizarlo cada vez que cambiemos o añadamos funcionalidad. Desde ******************Aplicaciones, Actualizaciones…******************

Instalamos el módulo pero aún no ocurre nada porque no hemos codificado nada para dicho módulo.

![Untitled](300%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%201%20Configuración%20inicial%20Primeros%20pasos/Untitled%207.png)

<aside>
💡 Añadimos control de versiones.
El link de Github donde puedes ver los distintos commits se encuentra:

</aside>

[GitHub - igijon/odoo_python_school_2023: Ejemplo básico de módulo en python para Odoo.](https://github.com/igijon/odoo_python_school_2023)