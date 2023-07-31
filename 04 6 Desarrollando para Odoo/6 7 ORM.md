
#odoo #python #postgresql #docker 

# 6.7. ORM

<aside>
💥 **ORM**: object relational mapping.

</aside>

El objetivo del ORM es simplificar (o evitar) el diseño de BDD. 

El ORM en Odoo es implícito. Cada declaración de clases implica el mapeo en BDD.

Para ver el ORM vamos a arrancar Odoo en modo shell:

![Untitled](300%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%207%20ORM/Untitled.png)

`self` es el usuario en el que hemos entrado. 

![Untitled](300%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%207%20ORM/Untitled%201.png)

Así puedo acceder a los fields del singleton que referencia a los usuarios.

![Untitled](300%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%207%20ORM/Untitled%202.png)

Así puedo acceder a todos los usuarios.

![Untitled](300%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%207%20ORM/Untitled%203.png)

![Untitled](300%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%207%20ORM/Untitled%204.png)

`env` es el environment y si recupero res.partner, asociado a ese string se encuentra el modelo res.partner.  `Env` es una variable, o un objeto y a través de él podemos acceder a todas las tablas de la BDD. Para buscar tendría que hacer search:

![Untitled](300%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%207%20ORM/Untitled%205.png)

Aquí aparecen todos los contactos de Odoo.

Puedo buscar por un criterio:

![Untitled](300%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%207%20ORM/Untitled%206.png)

![Untitled](300%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%207%20ORM/Untitled%207.png)

![Untitled](300%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%207%20ORM/Untitled%208.png)

![Untitled](300%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%207%20ORM/Untitled%209.png)

Operaciones básicas: unión, intersección, resta….

![Untitled](300%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%207%20ORM/Untitled%2010.png)

La programación funcional es compatible con la estructurada pero es diferente y se programa de forma distinta. Python tiene la función filter, map, iterables que son típicos de la programación funcional.

![Untitled](300%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%207%20ORM/Untitled%2011.png)

![Untitled](300%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%207%20ORM/Untitled%2012.png)

![Untitled](300%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%207%20ORM/Untitled%2013.png)

Si hay cosas repetidas `mapped` no las duplica.

![Untitled](300%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%207%20ORM/Untitled%2014.png)

![Untitled](300%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%207%20ORM/Untitled%2015.png)

![Untitled](300%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%207%20ORM/Untitled%2016.png)

![Untitled](300%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%207%20ORM/Untitled%2017.png)

Mapea el contenido como deseemos establecerlo.

<aside>
💥 Otra cosa importante es el contexto.

</aside>

![Untitled](300%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%207%20ORM/Untitled%2018.png)

Es un diccionario de cosas. Lo utilizamos para enviar información entre elementos que no son 100% compatibles. El cliente y servidor se comunican con JSON por ejemplo. Context nos permite pasar información de cambios de protocolo… y otras cosas. Una función puede cambiar el contexto y todas las que se creen a partir de ahí tendrán ese contexto. Es una forma de pasar información cuando no hay otra forma de enviarse.

Por ejemplo, pasábamos el active_id entre formularios pero para eso debo estar en un formulario.

![Untitled](300%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%207%20ORM/Untitled%2019.png)

![Untitled](300%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%207%20ORM/Untitled%2020.png)

No tengo formulario activo porque no estoy en ningún formulario.

![Untitled](300%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%207%20ORM/Untitled%2021.png)

![Untitled](300%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%207%20ORM/Untitled%2022.png)

`search` actúa sobre el modelo, no sobre la búsqueda anterior.

![Untitled](300%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%207%20ORM/Untitled%2023.png)

Así sí porque he unido los criterios de búsqueda y se están mostrando los que cumplen las dos condiciones.

Si quiero que se cumpla una u otra condición tenemos que poner `|` la tubería.

![Untitled](300%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%207%20ORM/Untitled%2024.png)

Es notación polaca inversa.

![Untitled](300%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%207%20ORM/Untitled%2025.png)

![Untitled](300%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%207%20ORM/Untitled%2026.png)

A partir del segundo resultado.

![Untitled](300%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%207%20ORM/Untitled%2027.png)

![Untitled](Untitled%2028.png)

Creando elementos desde la consola:

![Untitled](Untitled%2029.png)

![Untitled](Untitled%2030.png)

![Untitled](Untitled%2031.png)

Para crear un elemento Many2Many, modificamos la función que calculaba el estudiante por defecto en las groupal_task:

![Untitled](Untitled%2032.png)

![Untitled](Untitled%2033.png)

<aside>
💥 Volviendo a crear el estudiante si hemos reiniciado la instancia porque las cosas que hacemos en la terminal son cosas de prueba que no se guardan en la BDD.

</aside>

![Untitled](Untitled%2034.png)

Si vemos las tareas grupales de s1 estarán vacías de momento

![Untitled](Untitled%2035.png)

Una forma de añadir tareas sería pero esto sustituye el valor del atributo.

![Untitled](Untitled%2036.png)

Otra forma sería:

![Untitled](Untitled%2037.png)

Con esta sintaxis, se crea un nuevo registro y se vincula.

También puedo borrarlas y desvincularlas:

![Untitled](Untitled%2038.png)

Estas son las cosas que puedo hacer:

- (0,_,{’field’:value}): Crea un nuevo registro y lo vincula
- (1,id,{’field’:value}): Actualiza los valores en un registro ya vinculado
- (2,id,_): Desvincula y elimina el registro
- (3,id,_): Desvincula pero no elimina el registro de la relación
- (4,id,_): Vincula un registro ya existente
- (5,_,_): Desvincula pero no elimina todos los registros vinculados
- (6,_,[ids]): Reemplaza la lista de registros vinculados

Si queremos vincular el 3 que ya está creado que es el g2:

![Untitled](Untitled%2039.png)

![Untitled](Untitled%2040.png)

Esto último es similar a lo que hicimos al inicio de sustituir los valores de groupal_tasks.

![Untitled](Untitled%2041.png)

Puedo ver el id de un formulario en su tabla correspondiente con `ref`

![Untitled](Untitled%2042.png)

![Untitled](Untitled%2043.png)

También podemos eliminarlo

![Untitled](Untitled%2044.png)

Para salir de la terminal

![Untitled](Untitled%2045.png)

# Onchanges

Es un decorador que pondremos en una función para hacer que se ejecute cuando un campo cambia de valor.

![Untitled](Untitled%2046.png)

Este decorador, detecta un cambio en el campo correspondiente y ejecuta la función. También informa a la vista de que los cambios se deben producir y el servidor ejecutará esta función y devolverá el diccionario con la información del warning programada.

![Untitled](Untitled%2047.png)

Podemos usar una estética nueva:

![Untitled](Untitled%2048.png)

![Untitled](Untitled%2049.png)

Vamos a hacer otras modificaciones relacionadas con la interacción del usuario.

Cuando el usuario selecciona, por ejemplo, el nivel, queremos que se carguen las clases asociadas y eso lo vamos a programar ahora en el `onchange`

Vamos a eliminar las restricciones `domain` que teníamos en el formulario de school en el campo classroom y también en el modelo asociado al campo y vamos a añadir el onchange:

![Untitled](Untitled%2050.png)

Podemos añadir en las vistas que sólo se muestre classroom si level ha sido seleccionado:

![Untitled](Untitled%2051.png)

OnChange es la mejor forma de hacer los filtros y avisos de warning.