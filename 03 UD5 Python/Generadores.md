# Generadores

#python 


[python_bases/11_Generadores at main · igijon/python_bases](https://github.com/igijon/python_bases/tree/main/11_Generadores)

# Propuesto

Implementa un generador turnos en una farmacia.

Tenemos tres áreas de atención:

- Óptica
- Farmacia
- Cosmética

Inicialmente solicitaremos al usuario el área y mostraremos un turno según el área. La nomenclatura según las áreas (utiliza generadores para esto):

- Óptica: O-X
- Farmacia: F-X
- Cosmética: C-X

El mensaje en el que comunicamos el número al cliente debe tener el texto adicional “Su turno es: xxx. Espere a ser atendido”. Utiliza decoradores para esto.

Divide tu programa en varios módulos, al menos dos:

- [numeros.py](http://numeros.py) donde se implementen generadores y decoradores
- [principal.py](http://principal.py) donde se implementen los métodos de gestión principal

Utiliza unittest para el desarrollo de los casos unitarios que consideres necesarios.