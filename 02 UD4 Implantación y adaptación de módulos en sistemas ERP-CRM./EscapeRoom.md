
#odoo #docker

# EscapeRoom (parametrización de Odoo)

Tarea UD4. Implantación y adaptación de módulos en sistemas ERP-CRM.

Actualmente están muy extendidos los **EscapeRoom** y están extendidos en muchas ciudades. 

Un **EscapeRoom** es un curso intensivo pensado para formar al alumnado en las competencias necesarias para empezar a trabajar inmediatamente después. Estos cursos, cuentan con **temarios concentrados** y se enfocan en las **cuestiones prácticas del día a día.** 

<aside>
💡 Es una forma rápida de **especialización.**

</aside>

Queremos utilizar **Odoo** para gestionar una empresa que se dedique a realizar estos cursos. Para ello vamos a necesitar lo siguiente:

- Partir de una BDD limpia con el nombre que decidas sin datos de prueba.
- En nuestro ERP tendremos instalados los siguientes módulos: Ventas, Compras e Inventario.
- En la empresa tienen distintos departamentos que van a utilizar el sistema:
    - **Administradores:** podrán acceder a todas las tareas propias del sistema.
    - **Consultores:** podrán consultar la información sobre los **bootcamps, expertos, tecnologías, alumnos…**
    - **Creadores:** podrán añadir y modificar información en el sistema.
- Crea los objetos que estimes oportunos para la gestión de la empresa. Como mínimo: bootcamps, expertos, tecnologías, alumnos… Deberás completar dichos objetos con los campos que estimes oportunos.
- Establece las relaciones que consideres oportunas entre ellos y con el resto de objetos de Odoo.
- Crea las vistas correspondientes para dichos objetos: **tree, form y kanban.** Deben cumplir los siguientes requisitos:
    - En la vista formulario se debe poder cargar una imagen para los objetos.
    - En la vista formulario deben aparecer varias pestañas (page).
    - En la vista kanban deben aparecer distintos campos y la imagen.
- Deberás añadir los objetos como elementos de menús donde estimes conveniente. Por ejemplo, los bootcamps aparecerán en los mismos menús en los que aparezca el objeto producto (módulos: inventario, ventas y compras).
- Crea un informe imprimible en HTML para Bootcamps y para expertos.

Deberás subir al aula virtual lo siguiente como entrega:

- Un pdf con las capturas correspondientes de toda la funcionalidad.
- La copia de seguridad de la BDD exportada para poder probar tu aplicación.