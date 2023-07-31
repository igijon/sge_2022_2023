
#odoo #python #postgresql #docker 

# 6.8. Wizard

Nosotros en una vista tree podemos crear un banner, por ejemplo:

![Untitled](300%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%208%20Wizard/Untitled.png)

Para crear esto de momento, tendremos que acceder al fichero de controladores: `controllers/controllers.py`

Un controlador, es una respuesta a una petición web asociada a una ruta.

Tendremos un controlador con este aspecto:

![Untitled](300%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%208%20Wizard/Untitled%201.png)

Creamos una hoja de estilos para que se genere con estos estilos:

![Untitled](300%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%208%20Wizard/Untitled%202.png)

Esta hoja de estilos la almacenamos en la ruta que indica el controlador. Lo podéis ver en el repositorio.

Finalmente, para que funcione, en la vista tenemos que hacer:

![Untitled](300%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%208%20Wizard/Untitled%203.png)

Creamos el modelo course:

![Untitled](300%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%208%20Wizard/Untitled%204.png)

Lo relacionamos en la clase también:

![Untitled](300%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%208%20Wizard/Untitled%205.png)

Hacemos las vistas para este nuevo modelo:

![Untitled](300%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%208%20Wizard/Untitled%206.png)

y las añadimos al manifest:

![Untitled](300%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%208%20Wizard/Untitled%207.png)

No olvidemos añadir los permisos:

![Untitled](300%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%208%20Wizard/Untitled%208.png)

Vamos a crear un wizard que es el resultado de una acción. Se puede tener como resultado de un botón o como resultado de una acción.

En este caso arrancará al pulsar Crear curso.

Los wizards se fundamentan en el modelo. 

Vamos a hacer el modelo del wizard, después haremos la vista y el action.

![Untitled](300%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%208%20Wizard/Untitled%209.png)

Hacemos la vista:

![Untitled](300%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%208%20Wizard/Untitled%2010.png)

Hacemos el action:

![Untitled](300%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%208%20Wizard/Untitled%2011.png)

Nos fijamos que en el controlador tenemos:

![Untitled](300%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%208%20Wizard/Untitled%2012.png)

Tenemos que dar los permisos correspondientes y ya se lanza el wizard:

![Untitled](300%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%208%20Wizard/Untitled%2013.png)

![Untitled](300%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%208%20Wizard/Untitled%2014.png)

Ahora queremos mejorar que classroom y students si se crean y después se descarta el curso no se queden creados, para ello deben ser modelos transitorios también:

![Untitled](300%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%208%20Wizard/Untitled%2015.png)

![Untitled](300%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%208%20Wizard/Untitled%2016.png)

![Untitled](300%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%208%20Wizard/Untitled%2017.png)

Ahora vamos a crear las acciones para agregar una clase y un estudiante de forma más cómoda.

![Untitled](300%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%208%20Wizard/Untitled%2018.png)

[Add classroom y add student en el wizard · igijon/odoo_python_school_2023@71ed459](https://github.com/igijon/odoo_python_school_2023/commit/71ed459bcb5c911c52557519740fd36858a56d64)

Queremos que sea completamente funcional y se pueda crear el curso completo.

[Almacenamiento del curso al pulsar el botón create · igijon/odoo_python_school_2023@34a891f](https://github.com/igijon/odoo_python_school_2023/commit/34a891f090b85f8850b5a2dde6afe471ed30b91c)