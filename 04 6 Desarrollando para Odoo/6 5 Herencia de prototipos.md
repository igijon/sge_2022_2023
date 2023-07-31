
#odoo #python #postgresql #docker 

# 6.5. Herencia de prototipos

La herencia por prototipo es la más parecida a la programación tradicional. El objeto nuevo se almacena en otra tabla distinta que contiene las del objeto original más las nuevas.

![Untitled](300%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%205%20Herencia%20de%20prototipos/Untitled.png)

La herencia por prototipo en Odoo no es la solución ideal si sólo queremos ampliar cosas de Odoo porque Odoo ya está hecho y normalmente se extienden cosas.

La herencia por prototipo se trata de una clase nueva. Vamos a hacer un ejemplo. (`models.py`)

```python
class task(models.Model):
    _name='school.task'
    _description='base class for tasks'
    name = fields.Char()
    qualification = fields.Float()

class individual_task(models.Model):
    _name='school.individual_task'
    _description='one student task'
    _inherit = 'school.task'
    
    student = fields.Many2one('res.partner', ondelete='cascade')

class groupal_task(models.Model):
    _name = 'school.groupal_task'
    _description = 'many student task'
    _inherit = 'school.task'

    student = fields.Many2many('res.partner')
```

En este caso la clase task hace de prototipo.

Tengo dos clases que hacen cosas parecidas y se pueden desarrollar de forma parecida. Por ejemplo la clase task para ser el prototipo y luego dos clases más `individual_task` y `groupal_task` que son las que realmente se utilizarán.

Añadimos los permisos en `ir.model.access.csv`

```python
id,name,model_id:id,group_id:id,perm_read,perm_write,perm_create,perm_unlink
access_school_classroom,school.classroom,model_school_classroom,base.group_user,1,1,1,1
access_school_teacher,school.teacher,model_school_teacher,base.group_user,1,1,1,1
access_school_seminar,school.seminar,model_school_seminar,base.group_user,1,1,1,1
access_school_task,school.task,model_school_task,base.group_user,1,1,1,1
access_school_task,school.task,model_school_task,base.group_user,1,1,1,1
access_school_individual_task,school.individual_task,model_school_individual_task,base.group_user,1,1,1,1
access_school_groupal_task,school.groupal_task,model_school_groupal_task,base.group_user,1,1,1,1
```

Añadimos también las relaciones en el modelo `student`(`models.py`)

```python
class student(models.Model):
    _name = 'res.partner'
    _inherit = 'res.partner' #herencia de clase

    #_description = 'school.student'

    #name = fields.Char(string="Nombre", readonly=False, required=True, help="Este es el nombre")
    birth_year = fields.Integer()
    dni = fields.Char(string="DNI")
    password = fields.Char(default=lambda p: secrets.token_urlsafe(12))
    description = fields.Text()
    inscription_date = fields.Datetime(default=lambda d: fields.Datetime().now())
    last_login = fields.Datetime()
    is_student = fields.Boolean()
    level = fields.Selection([('1','1'), ('2','2')])
    #photo = fields.Binary()
    photo = fields.Image(max_widtth=200, max_height=200)
    # Clave ajena a la clave primaria de classroom. Se guarda en BDD
    classroom = fields.Many2one("school.classroom", domain="[('level', '=', level)]", ondelete="set null", help="Clase a la que pertenece")
    #Campo relacionado no almacenado en BDD
    teachers = fields.Many2many('school.teacher', related='classroom.teachers', readonly=True)
    state = fields.Selection([('1', 'Matriculado'), ('2', 'Estudiante'), ('3', 'Ex-estudiante')], default="2")
    
    individual_tasks = fields.One2many('school.individual_task', 'student')
    groupal_task = fields.Many2many('school.groupal_task')
```

Y finalmente en la vista formulario de student (`views.xml`), añado en el notebook del formulario (el widget `many2many_tags` permite que se puedan ver como tags los nombres de los estudiantes):

```xml
<notebook>
	<page name="teachers_page" string="Teachers"> 
	  <field name="teachers">
	    <tree>
	      <field name="name"/>
        <field name="topic"/>
      </tree>
    </field>
   </page>
   <page name="individual_task_page" string="Individual Tasks">
	   <field name="individual_tasks">
	     <tree>
		     <field name="name"/>
	       <field name="qualification"/>
	     </tree>
     </field>
   </page>
   <page name="groupal_task_page" string="Groupal Tasks">
	   <field name="groupal_tasks">
	     <tree>
	       <field name="name"/>
         <field name="qualification"/>
         <field name="student" widget="many2many_tags"/>
       </tree>
     </field>
   </page>
 </notebook>
```

![Untitled](300%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%205%20Herencia%20de%20prototipos/Untitled%201.png)

Existen en este tipo de herencia por prototipo tablas diferentes.

![Untitled](300%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%205%20Herencia%20de%20prototipos/Untitled%202.png)

<aside>
💥 Problema: si desde el estudiante quiero crear una tarea de cualquier tipo, no me asigna el estudiante desde el cual estoy creando la tarea al mostrarlo en el formulario.

</aside>

![Untitled](300%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%205%20Herencia%20de%20prototipos/Untitled%203.png)

Para solucionar esto debemos hacer la vista formulario y no mostrar el campo porque dicho campo se calcula automáticamente a partir del estudiante.

Creamos un fichero `tasks.xml`

```xml
<odoo>
    <data>

        <record model="ir.ui.view" id="school.individual_task_form">
            <field name="name">individual task form</field>
            <field name="model">school.individual_task</field>
            <field name="arch" type="xml">
                <form>
                    <group>
                        <field name="name"/>
                        <field name="qualification"/>
                    </group>
                </form>
            </field>
        </record>
    
        <record model="ir.actions.act_window" id="school.action_task_window">
            <field name="name">school task window</field>
            <field name="res_model">school.task</field>
            <field name="view_mode">tree,form</field>
        </record>

        <menuitem
            id="school.menu_task"
            name="task"
            parent="school.menu_1"
            action="school.action_task_window"/>
    </data>
</odoo>
```

Lo añadimos al `__**manifest__**.py`

```python
...
# always loaded
    'data': [
        'security/ir.model.access.csv',
        'views/views.xml',
        'views/tasks.xml',
        'views/templates.xml',
    ],
...
```

Y lo calculará Odoo.

# Groupal tasks pasando el estudiante por contexto

Queremos que el estudiante se pase por contexto al crear una tarea grupal. Recordemos que es una relación `many2many` por lo tanto tenemos que hacer lo siguiente:

En el fichero `views.xml`, en el formulario de estudiante: `school.student_form`, vamos a tener lo siguiente:

![Untitled](300%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%205%20Herencia%20de%20prototipos/Untitled%204.png)

Después, en el modelo, haremos: (`models.py`)

![Untitled](300%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%205%20Herencia%20de%20prototipos/Untitled%205.png)

De esta manera, al crear una tarea grupal aparecerá nuestro estudiante y además, si tenemos en `tasks.xml`:

![Untitled](300%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%205%20Herencia%20de%20prototipos/Untitled%206.png)

Sólo aparecerán los estudiantes para ser seleccionados.

![Untitled](300%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%205%20Herencia%20de%20prototipos/Untitled%207.png)

![Untitled](300%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%205%20Herencia%20de%20prototipos/Untitled%208.png)