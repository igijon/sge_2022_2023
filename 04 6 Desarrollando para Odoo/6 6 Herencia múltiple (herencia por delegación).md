
#odoo #python #postgresql #docker 

# 6.6. Herencia múltiple (herencia por delegación).

![Untitled](11%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%206%20Herencia%20múltiple%20(herencia%20por%20delegación)/Untitled.png)

Al poner `_inherits` establecemos que podemos heredar de varios y heredaríamos todas las propiedades. Tenemos que añadir un diccionario con todos los modelos de los que hereda.

`task_id` es un atributo many2one que se genera automáticamente en la tabla de tareas individuales y hace referencia a la tabla de la cuál hereda.

Cuando creamos una `individual_task`, se generará una `task` en la tabla correspondiente y otra en individual_task que hace referencia a la individual_task correspondiente mediante el campo `task_id`.

![Untitled](11%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%206%20Herencia%20múltiple%20(herencia%20por%20delegación)/Untitled%201.png)

Podemos tener todas las tareas listadas:

![Untitled](11%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%206%20Herencia%20múltiple%20(herencia%20por%20delegación)/Untitled%202.png)

![Untitled](11%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%206%20Herencia%20múltiple%20(herencia%20por%20delegación)/Untitled%203.png)

![Untitled](11%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%206%20Herencia%20múltiple%20(herencia%20por%20delegación)/Untitled%204.png)

Esta vista sólo nos informará de las cosas comunes.

Otro ejemplo de este tipo de herencia es en el caso de los productos. Cuando creamos un producto se crea un elemento en `product.product` pero también se crea un elemento de `product.template`. Es una herencia múltiple respecto de product.template y a partir de esta plantilla podríamos crear otros productos.