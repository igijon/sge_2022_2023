#postgresql #odoo 
# UD3. Organización, consulta y tratamiento de la información en sistemas ERP-CRM.

# Configuración inicial

- Es importante seleccionar correctamente el país porque se aplicarán características propias de la legislación de dicho país en cuanto a gestión empresarial.
- Vamos a seleccionar una BDD con datos demo porque es para un entorno de desarrollo y así introduce algunos usuarios, algunos productos… para poder trabajar.
- Si queremos crear o gestionar otra BDD:

[http://localhost:8069/web/database/selector](http://localhost:8069/web/database/selector)

![Untitled](11%20📈%20SGE%202022-2023/01%20UD3%20Organización,%20consulta%20y%20tratamiento%20de%20la%20información/Untitled.png)

- Configuramos la compañía:

![Untitled](11%20📈%20SGE%202022-2023/01%20UD3%20Organización,%20consulta%20y%20tratamiento%20de%20la%20información/Untitled%201.png)

- Debemos configurar la compañía en primer lugar, porque a la hora de añadir distintos componentes o módulos toma los datos de la compañía activa incluso en la BDD.
- Creación y configuración de usuarios:

![Untitled](11%20📈%20SGE%202022-2023/01%20UD3%20Organización,%20consulta%20y%20tratamiento%20de%20la%20información/Untitled%202.png)

- Podemos instalar y actualizar nuestros módulos en la sección de Aplicaciones.
- Por defecto, instala unos módulos básicos que son el módulo web y poco más.
- Podemos instalar todo lo necesario de una vez si pulsamos el modo de vista en árbol:

![Untitled](11%20📈%20SGE%202022-2023/01%20UD3%20Organización,%20consulta%20y%20tratamiento%20de%20la%20información/Untitled%203.png)

# Herramieta de gestión de la BDD. Uso de psql.

Vamos a trabajar con una BDD PostgreSQL. Tenemos instalado además un cliente de BDD en el docker de postgreSQL en consola que es psql.

```bash
docker-compose run --rm db psql -h db -U odoo prueba
```

![Untitled](11%20📈%20SGE%202022-2023/01%20UD3%20Organización,%20consulta%20y%20tratamiento%20de%20la%20información/Untitled%204.png)

<aside>
💡 Debemos estar en el directorio donde tengamos docker-compose.yaml

</aside>

**Lista de roles que tenemos**

![Untitled](11%20📈%20SGE%202022-2023/01%20UD3%20Organización,%20consulta%20y%20tratamiento%20de%20la%20información/Untitled%205.png)

**Para crear una BDD:**

![Untitled](11%20📈%20SGE%202022-2023/01%20UD3%20Organización,%20consulta%20y%20tratamiento%20de%20la%20información/Untitled%206.png)

**Para ver las tablas de la BDD tenemos:**

![Untitled](11%20📈%20SGE%202022-2023/01%20UD3%20Organización,%20consulta%20y%20tratamiento%20de%20la%20información/Untitled%207.png)

![Untitled](11%20📈%20SGE%202022-2023/01%20UD3%20Organización,%20consulta%20y%20tratamiento%20de%20la%20información/Untitled%208.png)

Como no tenemos tablas, no hace nada.

Si queremos ver las BDD que tenemos, podemos hacer:

![Untitled](11%20📈%20SGE%202022-2023/01%20UD3%20Organización,%20consulta%20y%20tratamiento%20de%20la%20información/Untitled%209.png)

Si nos fijamos, nos lista las BDD creadas, las que acabamos de crear por el usuario odoo y las que se crearon desde el ERP.

**Para conectar con una BDD concreta:**

![Untitled](11%20📈%20SGE%202022-2023/01%20UD3%20Organización,%20consulta%20y%20tratamiento%20de%20la%20información/Untitled%2010.png)

Ahora, podemos utilizar SQL directamente:

![Untitled](11%20📈%20SGE%202022-2023/01%20UD3%20Organización,%20consulta%20y%20tratamiento%20de%20la%20información/Untitled%2011.png)

Si ahora listamos las tablas:

![Untitled](11%20📈%20SGE%202022-2023/01%20UD3%20Organización,%20consulta%20y%20tratamiento%20de%20la%20información/Untitled%2012.png)

Vamos a probar un insert:

![Untitled](11%20📈%20SGE%202022-2023/01%20UD3%20Organización,%20consulta%20y%20tratamiento%20de%20la%20información/Untitled%2013.png)

Si consultamos:

![Untitled](11%20📈%20SGE%202022-2023/01%20UD3%20Organización,%20consulta%20y%20tratamiento%20de%20la%20información/Untitled%2014.png)

Este cliente es un cliente completo por consola. Para obtener ayuda sobre los comandos:

![Untitled](11%20📈%20SGE%202022-2023/01%20UD3%20Organización,%20consulta%20y%20tratamiento%20de%20la%20información/Untitled%2015.png)

Para **salir**:

![Untitled](11%20📈%20SGE%202022-2023/01%20UD3%20Organización,%20consulta%20y%20tratamiento%20de%20la%20información/Untitled%2016.png)

# Estructura de la BDD.

Una vez conectados, podemos ver también la BDD creada para cada una de las empresas que tengamos en Odoo (creamos una BDD para cada empresa)

Si exploramos las tablas correspondientes, podremos ver que para una nueva empresa o BDD creada tendremos aproximadamente 100 tablas creadas en el esquema o más. Por el contrario, veremos que según vayamos instalando módulos, el número de tablas se multiplicará por 3 ó 4.

Por defecto, cada una de las tablas está nombrada según al módulo al que pertenece. De esta manera, veremos que todas las tablas pertenecientes al módulo de stock empezarán por **stock_.**

A partir de ahora vamos a tener activado el modo desarrollador: **Ajustes (Opciones generales) ⇒ Activar modo desarrollador** (primera opción) dentro le la sección Herramientas de desarrollo.

![Untitled](11%20📈%20SGE%202022-2023/01%20UD3%20Organización,%20consulta%20y%20tratamiento%20de%20la%20información/Untitled%2017.png)

# Organización y consulta de la información.

La BDD de un sistema ERP es de gran envergadura. 

Almacena las tablas con los datos de la aplicación, vistas de las diferentes tablas y otros elementos como funciones o disparadores que realizan operaciones sobre los datos. Por ello, debido a esta gran cantidad de información almacenada, se hace necesaria una organización entre sus componentes.

Lo que se hace es establecer una serie de normativas o nomenclatura para organizar la información que los desarrolladores deben seguir a la hora de modificar el código fuente o esquema de la BDD.

Por ejemplo, incluir el prefijo en los componentes de la BDD para saber a qué módulo pertenece, o establecer una serie de campos dentro de una tabla como obligatorios, para poder asegurar el correcto funcionamiento de la aplicación.

En los sistemas de planificación empresarial desarrollados en un lenguaje orientado a objetos, cualquier dato es accesible a través de los objetos. Por ejemplo, en Odoo tenemos un objeto **res.partner** para acceder a los datos concernientes a los colaboradores o socios, un objeto **account.invoice** para los datos de las facturas, etc. Como ves, ambos van precedidos de un prefijo que indica el módulo al cual pertenecen.

Habitualmente, la información de esos objetos se guarda en tablas con el mismo nombre:

![Untitled](11%20📈%20SGE%202022-2023/01%20UD3%20Organización,%20consulta%20y%20tratamiento%20de%20la%20información/Untitled%2018.png)

![Untitled](11%20📈%20SGE%202022-2023/01%20UD3%20Organización,%20consulta%20y%20tratamiento%20de%20la%20información/Untitled%2019.png)

![Untitled](11%20📈%20SGE%202022-2023/01%20UD3%20Organización,%20consulta%20y%20tratamiento%20de%20la%20información/Untitled%2020.png)

Podemos acceder a los modelos desde **Ajustes**, en modo desarrollador. Como ejemplo, puedes ver que el modelo Usuarios (res.users), se almacena en BDD en una tabla con el mismo nombre **res_users.**

![Untitled](11%20📈%20SGE%202022-2023/01%20UD3%20Organización,%20consulta%20y%20tratamiento%20de%20la%20información/Untitled%2021.png)

# Tablas y vistas de la BDD

Como hemos explicado, un objeto guarda todo elemento que forma parte de la aplicación.
Como elementos de la aplicación tenemos la propia base de datos (tablas, disparadores o
acciones sobre esas tablas, etc), los formularios, los informes, las ventanas o los
asistentes, por ejemplo.
En Odoo (con el modo desarrollador activado) se accede a los objetos a través de la opción `Ajustes/Técnico/Estructura de la base de datos/Modelos`. En esta opción podemos
encontrar toda la información que maneja la aplicación.
Dentro de los objetos distinguimos las tablas. Una tabla es una estructura de datos
organizada en filas y columnas, de manera que cada columna es un campo (o atributo) y
cada fila un registro. Por ejemplo, en Odoo en la tabla res.users cada registro se
corresponde con un usuario, y para cada usuario se guardan una serie de atributos como
nombre, login o password.
En ocasiones, la base de datos está formada por tantas tablas y objetos que se vuelve
compleja y difícil de manejar. En esos casos, interesa que algunos grupos o perfiles de
usuarios tengan una vista parcial de esos datos. Para estos casos se utilizan las vistas.
Una vista es básicamente una "tabla virtual" a la que se puede acceder como si fuera una
tabla del esquema, pero que realmente no lo es. Tienen la misma estructura que las tablas: filas y columnas o campos, y se puede acceder a ellas de la misma forma, a través de consultas de acceso a datos como veremos posteriormente.

# Consultas de acceso a datos.

Las consultas de acceso a datos nos permiten acceder a la información que
guardan las tablas y vistas de la base de datos.
Las consultas de acceso a datos sirven para indicar al sistema de gestión de la
base de datos que devuelva un extracto de la información en forma de un conjunto
de registros.
Los pasos para crear una consulta son:

- Seleccionar las tablas o vistas sobres las que va a actuar la consulta.
- Establecer la relación entre las tablas y vistas, en caso de que la aplicación no la
proporcione.
- Seleccionar los campos a mostrar en la consulta.
- Ejecutar la consulta.

Las consultas pueden actuar sobre una o varias tablas o vistas, y se pueden
guardar para ser utilizadas posteriormente. En ocasiones la aplicación permite
realizar consultas de acceso a datos, o bien podemos conectarnos directamente al
sistema gestor de base de datos.
Las consultas de acceso a datos se pueden construir escribiendo el código en el
lenguaje de consulta utilizado, como por ejemplo SQL, o bien mediante asistentes
y constructores gráficos si se trata de consultas poco complejas.

# Cálculos: pedidos, albaranes, facturas, asientos predefinidos, trazabilidad…

Entre los procesos a realizar por un sistema de planificación empresarial destacan los siguientes:

**Contabilidad**. Incluye los procesos donde se reflejan las operaciones económicas realizadas por la empresa, la determinación de los costes de la empresa y los presupuestos del ejercicio fiscal. Se proporcionan asientos predefinidos para la introducción rápida de asientos sin necesidad de tener conocimientos de contabilidad.

**Operaciones de compra:**Crear una orden de compra o pedido de compra.Recibir los bienes.Controlar la factura de compra.Registrar el pago al proveedor.

**Operaciones de venta:**Crear una orden de venta o pedido de venta y recibir la conformidad del cliente.Preparar los bienes a enviar al cliente y realizar el albarán y la entrega.Realizar la factura de venta.Registrar el cobro al cliente o pago del cliente.

**Trazabilidad**: se llama así al proceso de la entrada del producto hasta la salida del mismo.Es posible que por las necesidades de la empresa no sea necesario utilizar todos los procesos del ERP, por ejemplo, puede ser usado sólo como un CRM, o sólo como un programa de contabilidad, pero su verdadera potencia se alcanza con la integración de todas sus funciones.

# Utilización de los asistentes

A la hora de realizar cualquier proceso, en una aplicación, es común el uso de
asistentes para ayudarnos en la tarea. Los asistentes son programas que
incorpora la aplicación para facilitar la realización de determinadas tareas.

Un asistente o wizard nos permitirá ejecutar una tarea concreta que hayamos
programado sobre uno o varios objetos del ERP. Generalmente los asistentes son
pantallas flotantes al estilo "Siguiente, Siguiente, Aceptar" donde se va
introduciendo la información necesaria para la realización de una tarea concreta.

Los asistentes normalmente aparecen en menús o en la parte lateral de los
formularios. Algunos de ellos son:

Asistente de configuración.
Asistente para crear cuentas contables a partir de una plantilla.
Asistente para enviar un correo electrónico o un mensaje a uno o varios clientes.
Asistente para la realización de tareas masivas, como por ejemplo la facturación
de todos los albaranes que se encuentren en estado de facturar.
...

Los asistentes de configuración son un caso particular de asistentes que nos
permiten realizar las tareas de configuración de la aplicación, y que se utilizan en
las tareas iniciales de instalación.
Los asistentes deben proporcionar métodos de volver atrás o volver a ejecutarlos
si hemos errado la introducción de algún parámetro.
Por ejemplo, en Odoo, en Ajustes (como el modo desarrollador activado) podemos
entrar en "Asistente de configuración" y editar el estado ‘Hecho’, de manera que
podamos volver a ejecutar el asistente para introducir de nuevo los parámetros de
configuración que nos interesen en dicho asistente.

![Untitled](11%20📈%20SGE%202022-2023/01%20UD3%20Organización,%20consulta%20y%20tratamiento%20de%20la%20información/Untitled%2022.png)

# Procedimientos almacenados en el servidor.

Dentro del tratamiento que le damos a la información, a veces es necesario que se
lleven a cabo determinadas tareas de forma automática, en respuesta a algo que
pasa en la aplicación.
Por ejemplo, podemos querer enviar un correo electrónico al cliente de manera
automática, cada vez que nos hace un pedido, o bien registrar en la ficha del
cliente el pedido realizado. Ambas acciones: enviar el correo o registrar el pedido
en el histórico del cliente, son acciones que se desencadenan automáticamente
por el sistema, y la acción que las desencadena es la realización de un pedido por
parte de un cliente.
Para hacer esto en la mayoría de los sistemas podemos utilizar:

**Procedimientos almacenados de servidor**. Un procedimiento almacenado es un programa, procedimiento o función almacenado en una base de datos y listo para ser usado. Pueden ser ejecutados directamente por el usuario o bien cuando se cumpla una determinada condición a través de disparadores.

**Eventos de servidor**. Un evento de servidor consiste en detectar que pasa algo en la aplicación y hacer que el sistema responda a este suceso de forma automática. Los eventos de servidor son creados a nivel de objetos y no de base de datos, como ocurre en los procedimientos. Podemos definir los eventos de servidor a través de los menús de la aplicación, no siendo necesario introducirnos en la base de datos para programar la acción deseada.Un procedimiento almacenado en PostgreSQL se puede escribir en múltiples lenguajes de programación. Para definir un procedimiento en pgAdmin utilizamos la siguiente sintaxis:

![Untitled](11%20📈%20SGE%202022-2023/01%20UD3%20Organización,%20consulta%20y%20tratamiento%20de%20la%20información/Untitled%2023.png)

En el código podemos escribir cualquier instrucción del lenguaje SQL para el
manejo de base de datos. Los elementos entre corchetes son opcionales, no es
necesario ponerlos si no los vamos a utilizar. El procedimiento se ejecuta con una
instrucción SELECT:
SELECT nombre_funcion([argumentos]);

# Extracción de datos en sistemas ERP-CRM y almacenes de datos.

El proceso de extracción de datos podemos definirlo como la operación de sacar datos de una aplicación para ser tratados en otra aplicación. La extracción de datos puede realizarse utilizando diferentes sistemas. 

Un uso muy común es utilizar herramientas ofimáticas que se conectan a la aplicación ERP, para obtener información de la base de datos y volcarla en la aplicación ofimática como un procesador de textos, una hoja de cálculo, etc. 

Existen procesos más complicados y potentes para extraer información. Son los llamados procesos de Business Intelligence. Este tipo de soluciones debe realizar tres tareas: transformar y combinar los datos para extraer la información, convertirla en potentes indicadores y mostrarla en distintos formatos gráficos. Según el origen de los datos y el tipo de información que queramos obtener, se pueden utilizar: 

**Consultas e Informes**. Se usan cuando todos los datos están en una sola base de datos, y se extraen a partir de una consulta SQL. La aplicación facilita informes y consultas predefinidos, aunque como hemos visto también se pueden generar consultas e informes personalizados utilizando la propia aplicación, incluso herramientas externas como JasperReports.

**Almacenes de datos**. La extracción de datos se hace desde diferentes sistemas y distintas bases de datos, creando almacenes de datos con el objetivo de homogeneizar e integrar la información. 

**Cubos multidimensionales**. Un cubo n-dimensional es un conjunto de datos multidimensionales organizados en ejes y celdas, que maneja la información de una base de datos relacional. También existen bases de datos multidimensionales, como contraposición a la operación de guardar los datos en bases de datos relacionales y luego manejarlos con cubos. 

En ocasiones el proceso de extracción y manipulación de la información no se realiza en tiempo real debido al gran volumen de información que hay que manejar, para evitar una disminución en la velocidad de respuesta a la hora de presentar los datos. Esto quiere decir que primero se extrae la información y luego es manipulada, lo cual significa que puede haber una leve diferencia entre la información manipulada y el verdadero contenido de la base de datos.

# Bibliografía

- Wiki de José Castillo: https://castilloinformatica.es/wiki/index.php?title=Odoo 

- Canal José Castillo: https://www.youtube.com/channel/UCHgxX1MDRj6wIZabKQbZINQ 

- Canal de SGE: https://www.youtube.com/channel/UC8gl7Ap_GZVbsKjri2GChkg 

- Institut Obert de Catalunya: https://ioc.xtec.cat/materials/FP/Recursos/fp_dam_m10_/ web/fp_dam_m10_htmlindex/material_pdf.html 

- https://educacionadistancia.juntadeandalucia.es/aulavirtual/ 

- Wikipedia: https://es.wikipedia.org/wiki/Wikipedia:Portada 

- Documentación instalación de Odoo 14: https://www.odoo.com/documentation/14.0/es/ administration/install/install.html 

- Manual de instalación de Odoo de José Castillo: https://castilloinformatica.es/wiki/ index.php?title=Instal%C2%B7lar_Odoo