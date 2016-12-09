<!-- $theme: gaia -->

# M07. Desenvolupament web en entorn servidor
## UF2. ???
Autor: Sergi Coll

---

## Patró MVC

**MVC** són les sigles de:

**model-vista-controlador**
***model-view-controler***

que es consisteix en un patró d'arquitectura del software.

---

## Arquitectura de software

* L'**aquitectura de software**, a semblança dels plànols d'un edifici o construcció, defineix la forma com s'***oganitzen***, ***interactuen*** i es ***relacionen*** entre sí les parts del programari. 
---

## MVC

* permite “no mezclar lenguajes de programación en el mismo
código”.

---

## MVC
**MVC** devideix les aplicacions en 3 nivells d'abstracció:
* **Modelo**: representa la lógica de negocios. 
  * Es el encargado de accesar de forma directa a los datos actuando como “intermediario” con la base de datos. 
 * **Vista**: es la encargada de mostrar la información al usuario de forma gráfica.
  * **Controlador**: es el intermediario entre la vista y el modelo. Es quien controla las interacciones del usuario solicitando los datos al modelo y entregándolos a la vista para que ésta, lo presente al usuario.

---

## MVC: Funcionament

1. El **usuario** realiza una petición.
2. El **controlador** captura el evento (*event handler*). 
3. Hace la llamada al modelo/modelos correspondientes efectuando las modificaciones pertinentes sobre el modelo
4. El **modelo** será el encargado de interactuar con la base de datos i retornará esta información al controlador.
5. El **controlador** recibe la información y la envía a la vista.
6. La **vista**, los entregará al usuario de forma “humanamente legible”.

---

## MVC

Imatge

---

<!-- *template: invert -->

# Arrays

---
