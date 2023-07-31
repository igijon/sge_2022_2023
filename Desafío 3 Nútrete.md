#odoo #docker 

# Desafío 3: Nútrete

Se quiere programar un modulo de Odoo para la compañía ********************Nútrete.******************** Este módulo se encargará de la gestión de la información para una clínica de dietistas y nutricionistas. Para ello, en nuestro sistema, tendremos **como mínimo** los siguientes datos:

![Untitled](11%20📈%20SGE%202022-2023/Desafío%203%20Nútrete/Untitled.png)

- **Clientes**: registrarán los datos habituales como datos personales, dni, foto, historial, motivo de consulta (cambiar de hábitos alimenticios, aprender a comer sano, aumentar de peso, asesoramiento en embarazo o lactancia, mejora de rendimiento deportivo, aprender a leer etiquetado de alimentos….)
- **Dietistas**: registrarán los datos habituales como datos personales, dni, foto, especialidad (dieta vegetariana, paleo, detox, hipocalórica, proteica… investiga un poquito)…
- **Nutricionistas**: al igual que los dietistas registrarán los datos personales, dni, foto, especialidad (nutrición deportiva, pediátrica, clínica…)
- **Dieta**: registrará el cliente al que va dirigida, el nutricionista y/o dietista que ha trabajado en dicha dieta y un listado de revisiones. Una dieta sólo podrá ser creada, modificada o borrada por un nutricionista y/o dietista y consultada por ellos.
- **Revisiones**: registrará fecha, hora, dieta, peso, comentarios del paciente, tipo de evolución según los objetivos conseguidos.
- ********************Talleres/charlas:******************** algunos nutricionistas y/o dietistas impartirán talleres a los clientes que así lo deseen sobre diversos temas fijando una fecha y hora en el calendario para ello. Dichos talleres se realizarán telemáticamente y se adjuntará el link correspondiente además de los datos que consideres oportunos.

Tendrás que:

- Analizar los modelos necesarios. Debes tener en cuenta que puedes tener más modelos o completar el módulo con la información que creas necesaria.
- Deberás realizar las relaciones correspondientes entre ellos.
- Deberás pensar qué campos querrás mostrar que sean calculados.
- Deberás implementar los campos que se cargarán con valores por defecto.
- Debes tener en cuenta las restricciones de formato y unicidad de algunos campos.
- Deberás generar los datos de demo con un software (Python, Bash, Java, Kotlin…) que tú implementes para generar los XML correspondientes con las relaciones establecidas en todos los modelos.
- Deberás optimizar las vistas tree, form y kanban de los distintos modelos.
- Deberás realizar las vistas correspondientes de búsqueda, estableciendo filtros de las tres maneras posibles y su correspondiente vista de calendario cuando corresponda.

<aside>
<img src="https://www.notion.so/icons/fire_orange.svg" alt="https://www.notion.so/icons/fire_orange.svg" width="40px" /> No te olvides de:

- Crear tu repositorio y añadir al profesor como colaborador.
- Crear las historias de usuario en github (compárteme el link del proyecto) teniendo en cuenta dos sprints:
    - Primer sprint: 9 de febrero.
    - Segundo sprint: 16 de febrero.
- Establece correctamente una rama por cada  historia de usuario, la rama dev y por último la rama main.

</aside>