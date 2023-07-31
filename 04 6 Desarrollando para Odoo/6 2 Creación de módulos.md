#odoo #postgresql #python #docker 

# 6.2. Creación de módulos

- Odoo se basa en un fw OpenObject
- Permite una aplicación de tipo RAD (Raid Application Development).
- Capa ORM (Object Relational Mapping). Desarrollamos la aplicación y
automáticamente se mapea en la BDD aislándonos del modelo relacional completo.
- Se utiliza la arquitectura MVC (Modelo vista controlador).
- Permite realizar flujos de trabajo.
- Tiene un diseñador de informes.
- Facilita la traducción ya que tiene un módulo de traducción…

# MVC

![Untitled](11%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%202%20Creación%20de%20módulos/Untitled.png)

- El modelo son las clases diseñadas en Python.
- Las vistas están en el cliente web y están definidas como ficheros XML.
- El controlador son los métodos definidos también en ficheros Python y permiten
manipular los modelos.

# BDD

- No hay un diseño explícito de la base de datos. Depende de los módulos que se
van instalando.
- Es el resultado del mapeo del diseño de clases de ODOO en el SGBD
PostgreSQL.
- No tenemos fácil acceso al diseño entidad relación o diagrama del
modelo relacional.
- No obstante podemos conocer los modelos más importantes.

<aside>
💡 Siempre con el modo de desarrollador activo en Odoo.

</aside>

Si nos situamos en el módulo ************Ventas************. Si seleccionamos un prespuesto y nos situamos con el ratón encima, podemos obtener la información de desarrollo:

![Untitled](11%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%202%20Creación%20de%20módulos/Untitled%201.png)

Podemos ver que el modelo es ********************sale.order******************** y tu tabla en BDD sería **********************sale_order,********************** como vimos en su momento.

<aside>
💡 Si quiero obtener más información puedo acceder a ****************************Ajustes/Técnico/Estructura de la BDD/Modelos.****************************

</aside>

Cada modelo se corresponde a 1 o más tablas en BDD.

Las clases y los atributos ****************siempre**************** se escriben con minúscula. Si escribimos un “.”, esto representa cierta jerarquía, por ejemplo, si tenemos ******************res.partner,****************** implica que **************partner************** forma parte de ******res****** (el módulo base sería res). La jerarquía sería ********************************módulo.modelo.******************************** En BDD el “.” se sustituye por “-”.

<aside>
💡 No se recomienda tocar la BDD mediante SQL directamente desde el terminal de postgresql, sino desde el ORM.

</aside>

# Estructura de los módulos de Odoo.

Los módulos de Odoo están compuestos por distintos ficheros.

![Untitled](11%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%202%20Creación%20de%20módulos/Untitled%202.png)

El fichero `__**manifest__**.py` define la estructura básica (__**openerp**__.py en versiones anteriores), hay XML que definen el contenido de la BDD, otros XML orientados a definir la forma que van a tener las vistas y otros XML para los reports (informes) y algunos ficheros Python que nos servirán para definir modelos y controladores y los wizards (asistentes).

# Creación de un módulo nuevo manualmente

- Añadir un directorio en el **************addons-path**************
- Crear el fichero `__init__.py`  para la importación de ficheros.
- Crear el fichero `__odoo__.py`  para definir el módulo.
- Crear los archivos Python que contienen los objetos.
- Crear los xml que contienen los datos de módulos para vistas, entradas de menús o datos de demo.
- Opcionalmente, crear informes.
- Actualmente, también podemos crear módulos con el comando `scaffold` tal y como hicimos en el apartado anterior.

# Fichero `__**init__.py**`

- Archivo de importación de Python
- Inicialmente tiene este contenido:

![Untitled](11%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%202%20Creación%20de%20módulos/Untitled%203.png)

- `controllers` es el directorio donde localizaremos los controladores y `models` es el directorio donde localizaremos los modelos.
- Dentro de models y controllers también hay un __**init__**.py con la información de importación correspondiente…

# Fichero `__**manifest__**.py`

![Untitled](11%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%202%20Creación%20de%20módulos/Untitled%204.png)

Contiene un diccionario que será interpretado cuando arrancamos en el menú de Odoo, la parte de aplicaciones:

![Untitled](11%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%202%20Creación%20de%20módulos/Untitled%205.png)

![Untitled](11%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%202%20Creación%20de%20módulos/Untitled%206.png)

- De este fichero hay que destacar las secciones: ******************************************depends, data y demo.******************************************
    - ************************************depends:************************************ indica los módulos que deben estar instalados para la instalación de este módulo. Si hace falta, se instalarán dichas dependencias.
    - ************data:************ indica los archivos xml que son necesarios para cargar el módulo.
    - ************demo:************ igual que data pero cuando está seleccionada la casilla demo al crear la BDD en Odoo.
- Los archivos python que nos interesan de momento son los que están dentro de **************models.************** De momento aparece comentado, pero si descomentamos, nos queda un modelo funcional.

![Untitled](11%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%202%20Creación%20de%20módulos/Untitled%207.png)

- Si hacemos lo mismo con `views/view.xml`, vemos código comentado que si activamos se convierten en vistas básicas pero funcionales.

![Untitled](11%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%202%20Creación%20de%20módulos/Untitled%208.png)

# Modelos.

- En Odoo, toda la información se basa en el concepto de modelos. Todos los recursos en Odoo son modelos: facturas, socios…
- Los metadatos, también son modelos: menús, acciones, informes,…
- El nombre de los modelos es jerárquico como dijimos:
    - account.transfer ⇒ una transferencia monetaria
    - account.invoice ⇒ una factura
    - account.invoice.line ⇒ una línea de factura
- En general, la primera palabra será la correspondiente al módulo.
- Los modelos se declaran en python como una subclase de `models.Model`
- El ORM de Odoo es construido sobre PostgreSQL pero es peligroso manipular directamente la BDD. El ORM facilita la gestión de los datos.

# Ficheros XML

Nos permiten declarar:

- Datos de demo
- Vistas
- Informes

# Creación del primer modelo

![Untitled](11%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%202%20Creación%20de%20módulos/Untitled%209.png)

- Un field es un campo relacionado en el ORM. Definimos por ejemplo `name = fields.Char()` porque queremos que se guarde en el modelo, en la BDD.
- Para entrar como root y reiniciar el servicio de odoo.

![Untitled](11%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%202%20Creación%20de%20módulos/Untitled%2010.png)

![Untitled](11%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%202%20Creación%20de%20módulos/Untitled%2011.png)

<aside>
💡 También podríamos reiniciar el contenedor.

</aside>

- Con la modificación que hemos hecho, aparentemente no hay ningún cambio salvo uno. Si entramos en Odoo, en la pestaña ******************************************************************************************Ajustes/Técnico/Estructura de la BDD/Modelos****************************************************************************************** y buscamos, podemos ver nuestro modelo creado:

<aside>
💡 No podemos olvidar actualizar el módulo en la instalación de Odoo.

</aside>

![Untitled](11%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%202%20Creación%20de%20módulos/Untitled%2012.png)

![Untitled](11%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%202%20Creación%20de%20módulos/Untitled%2013.png)

- Tenemos que destacar el campo ******id****** que es autoincrementable y la PK.
- De momento, este modelo no es accesible desde ningún sitio ⇒ Vamos a crear una vista editando `/views/views.xml`

![Untitled](11%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%202%20Creación%20de%20módulos/Untitled%2014.png)

- `record` implica que será un registro de la BDD. Este registro necesita saber dónde se van a guardar las cosas.
- Este registro se almacena en la tabla `ir.ui.view`, va a tener un id único `school.sudent_list` y va a tener un nombre (que no es lo más importante), un modelo, que en este caso sería `school.student`, un archivo xml con un campo que mostrará la vista que en este caso será el que tenemos creado en el .py del modelo student, que es `name`.
- Si reiniciamos el servicio y actualizamos el módulo podemos ver en **********************************************Ajustes/Técnico/Interfaz de usuario/Vistas.**********************************************

![Untitled](11%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%202%20Creación%20de%20módulos/Untitled%2015.png)

![Untitled](11%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%202%20Creación%20de%20módulos/Untitled%2016.png)

- Podemos ver que el contenido del fichero coincide con el del field arch del xml.
- Esta vista aún es inaccesible porque no hay ningún menú que permita acceder a ella.
- Para relacionarlo y ver una lista de estudiantes de momento, voy a descomentar en el fichero `views.xml` el action tal y como vemos a continuación:

![Untitled](11%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%202%20Creación%20de%20módulos/Untitled%2017.png)

- Esto es un ************action************. Un action relaciona un menú, un botón, con una acción que se desencadena en el cliente y se convierte en una petición al servidor. En este caso este action se transformará en una petición JS demandando al servidor las vistas tree y form del modelo student y el servidor retornará la vista creada tree y una vista form generada automáticamente para el modelo student ya que no tenemos una programada por el momento.
- Para que el action funcione, el cliente debe tener dicho menú. Buscamos la parte de menú al final, la descomentamos y la codificamos en el mismo `views.xml`.

![Untitled](11%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%202%20Creación%20de%20módulos/Untitled%2018.png)

- Tenemos que tener en cuenta que el action del menú ítem: `school.student_list` ****************************tiene que coincidir con el id del action que hemos creado antes: `school.action_student_window`.
- Si guardamos y reiniciamos, tampoco funcionará porque a partir de la versión 11, falta un detalle obligatorio de seguridad.
- Editamos el fichero `ir.model.access.csv`

![Untitled](11%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%202%20Creación%20de%20módulos/Untitled%2019.png)

- Hemos actualizado los datos con los permisos para módulo y modelo.
- Para cada modelo tendremos que crear una línea y en función de los grupos de usuarios que puedan tener permisos, tendrá restricciones.
- Falta una única cosa en `__**manifest__**.py`: descomentar la línea relacionada con este fichero de seguridad.

![Untitled](11%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%202%20Creación%20de%20módulos/Untitled%2020.png)

- Ahora al reiniciar y actualizar deberíamos ver el menú y las vistas correspondientes.

![Untitled](11%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%202%20Creación%20de%20módulos/Untitled%2021.png)

![Untitled](11%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%202%20Creación%20de%20módulos/Untitled%2022.png)

![Untitled](11%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%202%20Creación%20de%20módulos/Untitled%2023.png)

- Podría crear estudiantes, porque aunque la única vista que tengo es la de lista y no he creado ningún formulario por ejemplo, coge la vista por defecto.

![Untitled](11%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%202%20Creación%20de%20módulos/Untitled%2024.png)

- De este modo tendríamos creado un módulo básico con funcionalidad básica a partir de un modelo básico con vistas básicas, permisos…