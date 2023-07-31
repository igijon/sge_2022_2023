#odoo #docker 

# Desafío 4: Gestiónate

Vamos a desarrollar un módulo de Odoo para la compañía **************************Gestiónate.************************** Este módulo se encargará de la gestión de la información relacionada con proyectos de desarrollo de software y seguimiento de tareas.

Para nuestro sistema, tendremos **************************como mínimo************************** que gestionar la siguiente información:

![Untitled](11%20📈%20SGE%202022-2023/Desafío%204%20Gestiónate/Untitled.png)

- ******************************Tecnologías:****************************** este modelo guardará información sobre una tecnología concreta, descripción, logo de la tecnología…
- **********************Clientes:********************** almacenaremos información relacionada con los datos personales, empresa…
- **Empleados:** almacenaremos información relacionada con los datos personales y tendremos distintos tipos:
    - **********************************Desarrolladores:********************************** además de lo anterior tendremos que almacenar información referente a las tecnologías que manejan y nivel de conocimiento en cada una, categoría…
    - ****************************Scrum master:**************************** es un empleado desarrollador que además de formar parte del equipo y puede establecer reuniones con el product owner.
    - **************************Product owner:************************** es un empleado (no desarrollador) que puede establecer reuniones con el scrum master y con el cliente.
- ********************Proyecto:******************** almacenaremos información descriptiva del proyecto, clientes, empleados asociados al proyecto. También almacenaremos información de:
    - ******************Sprints:****************** En cada sprint se tendrán almacenadas **tareas** relacionadas con el proyecto. Los sprints tendrán una duración temporal (típicamente de 2 semanas)
    - ********************Plannings:******************** Asociados a cada proyecto tendremos varios días de planing (de retro, demo y planning del siguiente sprint) en la que asisten todos los miembros del proyecto. En esta planning tendremos información de los requisitos que se tendrán que desglosar en tareas y la fecha de celebración de la planning.
- ****************Tareas:**************** de una tarea almacenaremos: id, descripción de la tarea, tamaño de la tarea (en tallas de camiseta), prioridad de la tarea, sprint asociado y desarrollador asignado. También almacenaremos la fecha de comienzo y finalización de dicha tarea y el estado en el que se encuentra: To-do, in-progress, done.

Tendrás que:

- Analizar los modelos necesarios. Debes tener en cuenta que puedes tener más modelos o completar el módulo con la información que creas necesaria. Deberás aplicar los conocimientos que tienes sobre **********************************herencia en Odoo.**********************************
- Deberás realizar las relaciones correspondientes entre ellos.
- Deberás analizar qué campos quieres que sean calculados o por defecto y la unicidad o restricciones de formato que consideres.
- Deberás optimizar las vistas tree, form y kanban de los distintos modelos.
- Deberás establecer las vistas calendar para los modelos que consideres oportuno.
- Deberás hacer uso de onchange para detectar los cambios de los campos que consideres y mostrar los mensajes oportunos o restricciones correspondientes.
- Deberás programar un wizard para la creación de objetos del modelo que decidas.

<aside>
<img src="https://www.notion.so/icons/fire_orange.svg" alt="https://www.notion.so/icons/fire_orange.svg" width="40px" /> No te olvides de:

- Crear tu repositorio y añadir al profesor como colaborador.
- Crear las historias de usuario en github (compárteme el link del proyecto) teniendo en cuenta dos sprints:
    - Primer sprint: 2 de marzo.
    - Segundo sprint: 8 de marzo.
- Establece correctamente una rama por cada  historia de usuario, la rama dev y por último la rama main.
</aside>