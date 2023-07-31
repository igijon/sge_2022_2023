
#odoo #python #postgresql #docker
# 6.4. Principios de herencia en Odoo. Herencia de clases en Odoo

Normalmente en Odoo adaptamos cosas o funcionalidades de cosas ya hechas,  por eso en cuanto a herencia hay diferencias motivadas por la forma de programar en Odoo. 

Por ejemplo, normalmente no utilizamos clientes para crear nuevos tipos de clientes, ampliamos el modelo, añadimos funcionalidades…

[Chapter 13: Inheritance - Odoo 16.0 documentation](https://www.odoo.com/documentation/16.0/developer/howtos/rdtraining/13_inheritance.html)

![Untitled](300%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%204%20Principios%20de%20herencia%20en%20Odoo/Untitled.png)

El modelo que más se ajusta a lo que conocemos en Java o Python es el denominado ******************************************herencia por prototipos****************************************** (marcada en gris más oscuro) y es en la que a partir del modelo original, en base a la herencia, se genera otro nuevo independiente. En Odoo si se modifica el original, también se modificaría el nuevo y se almacenarían en tablas diferentes (hablamos de modelos en Odoo).

<aside>
<img src="https://www.notion.so/icons/fire_yellow.svg" alt="https://www.notion.so/icons/fire_yellow.svg" width="40px" /> Por ejemplo, partir de un producto, tendré otros tipos de productos con cosas diferentes pero por ejemplo en la tabla de ventas no aparecerán los nuevos porque las ventas referencian a la tabla de productos. Esta herencia se puede utilizar en Python pero no es la más utilizada en Odoo.

</aside>

La herencia más utilizada es la llamada ************************************herencia de clases************************************. Al heredar, ampliamos el modelo existente (no hablamos de clases y objetos, estamos hablando de modelos de Odoo). El modelo integra los datos del objeto y las relaciones del ORM entre otras cosas. Cuando heredemos de esta forma, ampliamos el modelo añadiendo nuevos atributos, sobreescribiendo otros si hace falta pero se almacenará en la misma tabla.

También tenemos un último tipo que es la **********************************************************herencia por delegación.********************************************************** En este caso se genera un nuevo modelo que tiene una referencia al objeto original más las cosas nuevas. Tiene una relación con el original, dos tablas distintas pero en la tabla nueva sólo tiene los atributos nuevos y la original los atributos que teníamos.

# Herencia en el modelo.

Vamos a hacer que los estudiantes sean clientes de Odoo. Habría que ampliar la clase cliente. Para saber qué modelo es el que queremos ampliar… 

![Untitled](300%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%204%20Principios%20de%20herencia%20en%20Odoo/Untitled%201.png)

Vemos que es `res.partner`. Es el modelo que puede ser cliente, usuario, empresa… todos ellos son elementos de `res.partner`.

Nuestro modelo es independiente de todo pero ahora vamos a cambiar algunas cosas.

<aside>
💥 ********************NOTA IMPORTANTE:******************** Estos cambios van a afectar a la coherencia de los modelos (BDD) por lo que para no dejar inconsistente el sistema, deberás desinstalar el módulo ************school************ para que se eliminen las relaciones y tablas correspondientes antes de cualquier modificación de este tipo.

</aside>

<aside>
<img src="https://www.notion.so/icons/fire_yellow.svg" alt="https://www.notion.so/icons/fire_yellow.svg" width="40px" /> Inicialmente vamos a hacer **herencia de clase**. 
En name indicamos el modelo del que heredamos (el que vamos a ampliar). 
Ya no es necesaria la descripción y el name también lo quitamos porque ya está en el modelo. El resto de campos en principio no están en el modelo. Si estuviesen, se sobreescribirían.
Tenemos que indicar además que se produce herencia, por tanto utilizamos la palabra **`_inherit` y como queremos aplicar **********************************herencia de clase********************************** deben coincidir el modelo y el padre.
En la herencia de clases, no haría falta especificar el `_name` pero por cuestiones de claridad lo dejamos.

</aside>

```python
class student(models.Model):
    _name = 'res.partner'
    _inherit = 'res.partner'
    #_description = 'school.student'

    #name = fields.Char(string="Nombre", readonly=False, required=True, help="Este es el nombre")
    birth_year = fields.Integer()
    dni = fields.Char(string="DNI")
    password = fields.Char(default=lambda p: secrets.token_urlsafe(12))
    description = fields.Text()
    inscription_date = fields.Datetime(default=lambda d: fields.Datetime().now())
    last_login = fields.Datetime()
...
```

<aside>
💥 Tenemos que tener en cuenta que hay muchas referencias a `school.student` que habría que cambiar. Así que buscamos y reemplazamos por `res.partner` (en la clase `classroom`)
También tenemos que hacer la sustitución en la vista.

</aside>

```python
class classroom(models.Model):
    _name = 'school.classroom'
    _description = 'Las clases'

    name = fields.Char() # Todos los modelos deben tener un field name

    level = fields.Selection([('1','1'), ('2','2')])
    # El primero es el valor que se guarda en BDD y el segundo el texto que se muestra

    # Esto es una consulta, no se guarda en BDD
    # Se declara como un field pero no se guarda porque es simplemente una
    # consulta a partir del many2one que sí se guarda en BDD
    students = fields.One2many(string="Alumnos", comodel_name="res.partner", inverse_name='classroom')
    # comodel_name es el modelo con el que establecemos la relación. Una clase tiene
    # muchos estudiantes, el modelo serían los estudiantes
    # inverse_name sería la clave ajena de la clase con la que relacionamos.
    # En este caso una clase tiene varios estudiantes y el campo que será la clave 
    # ajena del modelo student es classroom
    teachers = fields.Many2many(comodel_name='school.teacher',
                                relation='teachers_classroom',
                                column1='classroom_id',
                                column2='teacher_id')

...
```

```xml
<odoo>
  <data>
    <record model="ir.ui.view" id="school.student_list">
      <field name="name">school student list</field>
      <field name="model">res.partner</field>
      <field name="arch" type="xml">
        <tree decoration-info="birth_year>1990" decoration-warning="birth_year&lt;1980">
          <field name="name"/>
          <field name="birth_year" sum="Total"/>
          <field name="password" />
        </tree>
      </field>
    </record>

    <record model="ir.ui.view" id="school.teacher_list">
      <field name="name">school.teacher list</field>
      <field name="model">school.teacher</field>
      <field name="arch" type="xml">
        <tree>
          <field name="name"/>
          <field name="topic"/>
          <field name="phone" />
        </tree>
      </field>
    </record>

    <record model="ir.ui.view" id="school.student_form">
      <field name="name">school student form</field>
      <field name="model">res.partner</field>
      <field name="arch" type="xml">
        <form>
          <header>
            <field name="state" widget="statusbar"></field>
          </header>
          <sheet>
            <div class="oe_button_box">
                <button type="object" class="oe_stat_button" icon="fa-pencil-square-o"  name="regenerate_password">
                  <div class="o_form_field o_stat_info">
                    <span class="o_stat_value">
                      <!-- <field name="password" string="Password"/> -->
                      <!-- En el ejemplo va a mostrar sólo una parte porque tiene unas dimensiones concretas
                      -->
                    </span>
                    <span class="o_stat_text">Password</span>
                  </div>
                </button>
              </div>
            <field name="photo" widget="image"/>
            <group>
              <group>
                <separator string="Personal Data"></separator>
                <field name="name"/>
                <field name="birth_year"/>
                <field name="password"/>
                <field name="dni"/>
                <field name="description"/>
              </group>
              <group states="2">
                <separator string="Inscription Data"></separator>
                <field name="inscription_date"/>
                <field name="last_login"/>
                <field name="is_student"/>
                <field 
                  name="level"
                  attrs="{'invisible':[('is_student','=',False)]}"/>
                <field 
                  name="classroom" 
                  domain="[('level', '=', level)]"
                  attrs="{'invisible':[('is_student','=',False)],
                          'required':[('is_student','=', True)]}"/> 
              </group>
            </group>
            <notebook>
              <page name="teachers_page" string="Teachers"> 
                <field name="teachers">
                  <tree>
                    <field name="name"/>
                    <field name="topic"/>
                  </tree>
                </field>
              </page>
            </notebook>
          </sheet>
        </form>
      </field>
    </record>
  
    <record id="school.student_kanban" model="ir.ui.view">
      <field name="name">school student kanban</field>
      <field name="model">res.partner</field>
      <field name="arch" type="xml">
        <kanban 
          default_group_by="classroom"
          quick_create_view="school.quick_create_student_form">
          <field name="id"></field>
          <field name="classroom"></field>
          <templates>
            <t t-name="kanban-box">
              <div t-attf-class="oe_kanban_color_{{kanban_getcolor(record.classroom.raw_value)}} oe_kanban_global_click o_kanban_record_has_image_fill">
                <a type="open">
                  <img width="150" style="padding:5px" class="oe_kanban_image"
                    t-att-src="kanban_image('res.partner', 'photo', record.id.raw_value)"
                    alt="student image"/>
                </a>
                <div t-attf-class="oe_kanban_content">
                  <h4>
                    <a type="edit">
                      <field name="name"></field>
                    </a>
                  </h4>
                  <ul>
                    <li>Classroom:
                      <field name="classroom"></field>
                    </li>
                  </ul>
                </div>
              </div>
            </t>
          </templates>
        </kanban>
      </field>
    </record>

    <record model="ir.ui.view" id="school.quick_create_student_form">
      <field name="name">school student form</field>
      <field name="model">res.partner</field>
      <field name="arch" type="xml">
        <form>
          <field name="name"/>
          <field name="birth_year"/>
          <field name="classroom"/>
          <field name="level"/>
        </form>
      </field>
    </record>
   
    <record model="ir.ui.view" id="school.student_search">
      <field name="name">school student search</field>
      <field name="model">res.partner</field>
      <field name="arch" type="xml">
        <search>
          <field name="name"/>
          <field name="dni" />
          <field name="birth_year" string="Min year" filter_domain="[('birth_year','>=',self)]"/>
          <filter name="adult" string="Adult students" domain="[('birth_year','&lt;=','2005')]"></filter> 
          <filter name="group_by_classroom" string="Group by classroom" context="{'group_by': 'classroom'}"></filter> 
        </search>
      </field>
    </record>

    <!-- Vista calendar -->
    <record model="ir.ui.view" id="school.seminar_calendar">
      <field name="name">school.seminar search</field>
      <field name="model">school.seminar</field>
      <field name="arch" type="xml">
        <calendar 
          string="Seminar calendar" 
          date_start="date"
          date_stop="finish"
          color="classroom"> <!--El color no lo elige, lo calcula en función del identificador-->
          <field name="name"/>
        </calendar>
      </field>
    </record>

    <!-- actions opening views on models -->

    <record model="ir.actions.act_window" id="school.action_student_window">
      <field name="name">school student window</field>
      <field name="res_model">res.partner</field>
      <field name="view_mode">tree,form,kanban</field>
    </record>

    <record model="ir.actions.act_window" id="school.action_classroom_window">
      <field name="name">school classroom window</field>
      <field name="res_model">school.classroom</field>
      <field name="view_mode">tree,form</field>
    </record>

    <record model="ir.actions.act_window" id="school.action_teacher_window">
      <field name="name">school teacher window</field>
      <field name="res_model">school.teacher</field>
      <field name="view_mode">tree,form</field>
    </record>
    
    <record model="ir.actions.act_window" id="school.action_seminar_window">
      <field name="name">school seminar window</field>
      <field name="res_model">school.seminar</field>
      <field name="view_mode">tree,form,calendar</field>
    </record>

    <!-- Top menu item -->

    <menuitem name="school" id="school.menu_root"/>
    <!-- menu categories -->

    <menuitem name="Management" id="school.menu_1" parent="school.menu_root"/>

    <!-- actions -->

    <menuitem name="Students" id="school.menu_1_student_list" parent="school.menu_1"
              action="school.action_student_window"/>
    <menuitem name="Classrooms" id="school.menu_1_classroom_list" parent="school.menu_1"
              action="school.action_classroom_window"/>
    <menuitem name="Teachers" id="school.menu_1_teacher_list" parent="school.menu_1"
              action="school.action_teacher_window"/>
    <menuitem name="Seminars" id="school.menu_1_seminar_list" parent="school.menu_1"
              action="school.action_seminar_window"/>
  </data>
</odoo>
```

<aside>
💥 En ir.model.access.csv eliminamos la línea correspondiente a `school.student`.

</aside>

```xml
id,name,model_id:id,group_id:id,perm_read,perm_write,perm_create,perm_unlink
access_school_classroom,school.classroom,model_school_classroom,base.group_user,1,1,1,1
access_school_teacher,school.teacher,model_school_teacher,base.group_user,1,1,1,1
```

<aside>
💥 En los datos de demo hay que cambiar también el modelo!!!

</aside>

```xml
<odoo>
    <data>
        <record id='student44474991D' model='res.partner'>
            <field name='name'>Reade Swedeland</field>
            <field name='dni'>44474991D</field>
            <field name='birth_year'>1997
            </field>
            <field name="classroom" ref="classroomDAM1"></field>
        </record>
        <record id='student94932356Y' model='res.partner'>
            <field name='name'>Anjela Pearse</field>
            <field name='dni'>94932356Y</field>
...
```

<aside>
💥 ****NOTA:**** Si inicialmente no has desinstalado el módulo school, al actualizar dará fallo porque se generan inconsistencias en BDD. Podemos intentar solucionarlo borrando la tabla inicial `school.student`. Podemos hacerlo manualmente desde postgresql aunque no es garantía de éxito.

</aside>

![Untitled](300%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%204%20Principios%20de%20herencia%20en%20Odoo/Untitled%202.png)

![Untitled](300%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%204%20Principios%20de%20herencia%20en%20Odoo/Untitled%203.png)

![Untitled](300%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%204%20Principios%20de%20herencia%20en%20Odoo/Untitled%204.png)

<aside>
💥 ********************NOTA IMPORTANTE:******************** Si aún borrando el modelo desde la BDD sigue fallando es porque se ha generado una inconsistencia y deberemos eliminar el volumen correspondiente y volver a generarlo para restaurar la instalación de Odoo en Docker. **Lo mejor en este caso es desinstalar el módulo school antes de generar cambios tan agresivos en los modelos.**

</aside>

Una vez instalado de nuevo, en el modelo res.partner podemos ver que están los campos añadidos y cuando entramos en ******************************school/students****************************** vemos que al pinchar en cualquier de ellos aparecen las vistas de **************cliente**************:

![Untitled](300%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%204%20Principios%20de%20herencia%20en%20Odoo/Untitled%205.png)

![Untitled](300%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%204%20Principios%20de%20herencia%20en%20Odoo/Untitled%206.png)

<aside>
💥 Faltaría hacer la herencia en la vista, por eso no se muestran los datos.

</aside>

# Herencia de vistas.

Debemos tener en cuenta qué vistas hay que ampliar siguiendo el concepto que hemos hecho antes con los modelos.

Los clientes están compartidos en todo, en proveedores… si la modificamos puede fallar en algunos jobs por lo que vamos a hacer que los clientes (**********************res.partner**********************) en cualquier job cojan las vistas de odoo menos en nuestro módulo school que queremos que coja las nuestras.

Vamos a modificar la vista formulario, para ello vamos a entrar en la vista formulario de un cliente que sea una persona en Odoo y analizar lo que tenemos ahí.

Además del action que teníamos, añadimos el siguiente para que coja nuestras vistas en lugar de las de Odoo para ****************res.partner.****************

```xml
<record model="ir.actions.act_window" id="school.action_student_window">
      <field name="name">school student window</field>
      <field name="res_model">res.partner</field>
      <field name="view_mode">tree,form,kanban</field>
    </record>

    <record model="ir.actions.act_window.view" id="school.action_view_student_form">
      <field eval="2" name="sequence"/>
      <field name="view_mode">form</field>
      <field name="view_id" ref="school.student_form"/>
      <field name="act_window_id" ref="school.action_student_window"/>
    </record>

    <record model="ir.actions.act_window.view" id="school.action_view_student_kanban">
      <field eval="3" name="sequence"/>
      <field name="view_mode">kanban</field>
      <field name="view_id" ref="school.student_kanban"/>
      <field name="act_window_id" ref="school.action_student_window"/>
    </record>

    <record model="ir.actions.act_window.view" id="school.action_view_student_tree">
      <field eval="1" name="sequence"/>
      <field name="view_mode">tree</field>
      <field name="view_id" ref="school.student_list"/>
      <field name="act_window_id" ref="school.action_student_window"/>
    </record>
```

De esta forma quedan enlazados y vemos que para nuestros estudiantes coge nuestras vistas y para los clientes del resto de módulos coge las que están establecidas por Odoo.

![Untitled](300%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%204%20Principios%20de%20herencia%20en%20Odoo/Untitled%207.png)

![Untitled](300%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%204%20Principios%20de%20herencia%20en%20Odoo/Untitled%208.png)

Para ver las prioridades con las que se cargan unas vistas u otras, podemos entrar en la vista kanban de cliente, por ejemplo:

![Untitled](300%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%204%20Principios%20de%20herencia%20en%20Odoo/Untitled%209.png)

Vemos que tiene un número de secuencia 16. Puedo cambiar las secuencias estableciendo para el nuestro otra secuencia con esto y de este modo el nuestro tendría menos prioridad que el propio de Odoo en el caso de tener algún problema de prioridades (sobre todo en versiones antiguas de Odoo).

```xml
<field name="priority">17</field>
```

Ahora vamos a adaptar las cosas a las que ya tenía Odoo, por ejemplo la foto que si miramos en el form de Odoo se corresponde con el field `image_1920`. Lo cambio en el form y en el kanban.

![Untitled](300%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%204%20Principios%20de%20herencia%20en%20Odoo/Untitled%2010.png)

<aside>
💥 En los datos de demo en lugar de photo tendremos que establecer el campo `image_1920` con la imagen en base64 y después veremos cómo limitar el tamaño de la foto.

</aside>

<aside>
💥 Tenemos algunos clientes que no son alumnos y no queremos que aparezcan ahí porque aparecen todos los res.partner. Serán alumnos los que tienen marcado `is_student`.

</aside>

Vamos a hacer que la vista tree por defecto cargue una vista search.

Hacemos un nuevo filtro en la vista search:

```xml
<record model="ir.ui.view" id="school.student_search">
      <field name="name">school student search</field>
      <field name="model">res.partner</field>
      <field name="arch" type="xml">
        <search>
          <field name="name"/>
          <field name="dni" />
          <field name="birth_year" string="Min year" filter_domain="[('birth_year','>=',self)]"/>
          <filter name="adult" string="Adult students" domain="[('birth_year','&lt;=','2005')]"></filter> 
          <filter name="student" string="Is Student" domain="[('is_student', '=', True)]" />
          <filter name="group_by_classroom" string="Group by classroom" context="{'group_by': 'classroom'}"></filter> 
        </search>
      </field>
    </record>
```

Y lo añadimos al action.

```xml
<record model="ir.actions.act_window" id="school.action_student_window">
      <field name="name">school student window</field>
      <field name="res_model">res.partner</field>
      <field name="view_mode">tree,form,kanban</field>
      <field name="search_view_id" ref="school.student_search"/>
    </record>
```

![Untitled](300%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%204%20Principios%20de%20herencia%20en%20Odoo/Untitled%2011.png)

Pero queremos que nos aparezca por defecto.

Podríamos hacerlo con un domain en el action aunque sería muy agresivo porque se aplica directamente como filtro en la BDD, pero sería así:

```xml
<record model="ir.actions.act_window" id="school.action_student_window">
      <field name="name">school student window</field>
      <field name="res_model">res.partner</field>
      <field name="view_mode">tree,form,kanban</field>
      <field name="search_view_id" ref="school.student_search"/>
      <field name="domain">[('is_student','=',True)]</field>
    </record>
```

![Untitled](300%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%204%20Principios%20de%20herencia%20en%20Odoo/Untitled%2012.png)

Ahora vamos a probar a hacerlo de otra manera. Utilizando el field `context`que es un field especial que le pasa la información al cliente web (no formando parte de la consulta de BDD como antes). En este caso, el filtro se hace en el cliente y se extrae en el cliente.

La vista lo que entiende es JavaScript.

```xml
<record model="ir.actions.act_window" id="school.action_student_window">
	<field name="name">school student window</field>
  <field name="res_model">res.partner</field>
  <field name="view_mode">tree,form,kanban</field>
  <field name="search_view_id" ref="school.student_search"/>
  <!-- <field name="domain">[('is_student','=',True)]</field> -->
  <field name="domain"></field> <!--Debemos eliminar lo establecido en la ejecución anterior si actualizamos el módulo-->
  <field name="context">{'search_default_student':1}</field>
</record>
```

![Untitled](300%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%204%20Principios%20de%20herencia%20en%20Odoo/Untitled%2013.png)

Aparece el filtro Is Student por defecto.

Si además quiero que cada vez que yo creo un estudiante, se quede marcada esa opción por defecto, se lo puedo indicar también en el action.

```xml
<record model="ir.actions.act_window" id="school.action_student_window">
      <field name="name">school student window</field>
      <field name="res_model">res.partner</field>
      <field name="view_mode">tree,form,kanban</field>
      <field name="search_view_id" ref="school.student_search"/>
      <!-- <field name="domain">[('is_student','=',True)]</field> -->
      <field name="domain"></field> <!--Debemos eliminar lo establecido en la ejecución anterior si actualizamos el módulo-->
      <field name="context">{'search_default_student':1, 'default_is_student':True}</field>
    </record>
```

![Untitled](300%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%204%20Principios%20de%20herencia%20en%20Odoo/Untitled%2014.png)

Si quisiésemos modificar la vista de `res.partner` añadiendo los datos de student (teniendo en cuenta que de este modo aparecerá en los students y también en los clientes de cualquier job haríamos lo siguiente:

```xml
<record model="ir.ui.view" id="school.student_partner_form">
	<field name="name">school student form inherit</field>
  <field name="model">res.partner</field>
  <field name="inherit_id" ref="base.view_partner_form"></field>
  <field name="arch" type="xml">
	  <notebook position="inside">
	    <page name="student" string="Student">
	      <group>
	        <field name="birth_year"/>
          <field name="password"/>
          <field name="dni"/>
          <field name="description"/>
          <field name="inscription_date"/>
          <field name="last_login"/>
          <field name="is_student"/>
          <field 
	          name="level"
            attrs="{'invisible':[('is_student','=',False)]}"/>
          <field 
	          name="classroom" 
            domain="[('level', '=', level)]"
            attrs="{'invisible':[('is_student','=',False)],
                  'required':[('is_student','=', True)]}"/>
         </group>
        </page>
       </notebook>
     </field>
   </record>
```

No olvidemos comentar las vistas que establecimos en los actions:

```xml
<record model="ir.actions.act_window" id="school.action_student_window">
      <field name="name">school student window</field>
      <field name="res_model">res.partner</field>
      <field name="view_mode">tree,form,kanban</field>
      <field name="search_view_id" ref="school.student_search"/>
      <!-- <field name="domain">[('is_student','=',True)]</field> -->
      <field name="domain"></field> <!--Debemos eliminar lo establecido en la ejecución anterior si actualizamos el módulo-->
      <field name="context">{'search_default_student':1, 'default_is_student':True}</field>
    </record>

    <!-- <record model="ir.actions.act_window.view" id="school.action_view_student_form">
      <field eval="2" name="sequence"/>
      <field name="view_mode">form</field>
      <field name="view_id" ref="school.student_form"/>
      <field name="act_window_id" ref="school.action_student_window"/>
    </record>

    <record model="ir.actions.act_window.view" id="school.action_view_student_kanban">
      <field eval="3" name="sequence"/>
      <field name="view_mode">kanban</field>
      <field name="view_id" ref="school.student_kanban"/>
      <field name="act_window_id" ref="school.action_student_window"/>
    </record>

    <record model="ir.actions.act_window.view" id="school.action_view_student_tree">
      <field eval="1" name="sequence"/>
      <field name="view_mode">tree</field>
      <field name="view_id" ref="school.student_list"/>
      <field name="act_window_id" ref="school.action_student_window"/>
    </record> -->
```

De esta manera cuando visualizamos los estudiantes vemos:

![Untitled](300%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%204%20Principios%20de%20herencia%20en%20Odoo/Untitled%2015.png)

Pero si vamos a clientes en el módulo Ventas obtenemos:

![Untitled](300%20📈%20SGE%202022-2023/04%206%20Desarrollando%20para%20Odoo/6%204%20Principios%20de%20herencia%20en%20Odoo/Untitled%2016.png)

De momento lo dejaremos como lo teníamos antes, cargando las vistas nuestras para los estudiantes.