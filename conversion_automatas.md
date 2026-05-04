# Conversión entre Modelos Formales  
## ER → AFD y AFD → Gramática  

---

## Integrantes

- Jean Paul Ortiz  
- Iván Daniel Naranjo  
- María Andrea Avendaño  
- Alexandra Puerta  

---

##  Introducción

¿Alguna vez te preguntaste cómo sabe una computadora si una contraseña 
tiene el formato correcto, o si un correo electrónico está bien escrito? 
Por dentro, usa exactamente lo que veremos a continuación.

En este documento se presentan dos procesos fundamentales dentro de la teoría de autómatas:

- Conversión de **Expresión Regular (ER) a Autómata Finito Determinista (AFD)** -es decir, pasar de una "receta" a una "máquina".
- Conversión de **Autómata Finito Determinista (AFD) a Gramática Regular** -es decir, pasar de la "máquina" a un "manual de instrucciones escrito".

Estos procesos permiten demostrar la equivalencia entre diferentes representaciones de los lenguajes regulares.

---

#  Parte 1: Conversión de ER → AFD

##  Expresión Regular

(a|b)*abb

¿Qué significa esto?

Significa que estamos buscando cualquier palabra que esté formada por letras "a" y "b" (en cualquier orden y cantidad, gracias a la primera parte (a|b)*), pero que obligatoriamente debe terminar con la secuencia exacta "abb".

---

##  Paso 1: ER → AFND (Método de Thompson)

Imagina que la Expresión Regular es como un manual de instrucciones para armar un mueble complejo. Si intentas armarlo todo de golpe y a la perfección, lo más probable es que te confundas. Las computadoras sienten lo mismo.

Por eso, usamos la "Construcción de Thompson" para crear un primer prototipo o esqueleto, conocido como Autómata Finito No Determinista (AFND). En esta etapa, en lugar de encajar todo a la perfección, unimos las distintas partes de nuestra fórmula usando "puentes mágicos" que se pueden cruzar sin gastar ninguna letra. A estos puentes los llamamos transiciones ε (épsilon).

Piensa en las transiciones ε como si fueran cinta adhesiva temporal: nos permiten pegar rápidamente todas las partes de la expresión regular para ver la forma general del recorrido, aunque el resultado sea un mapa con múltiples caminos que tendremos que limpiar más adelante.

---

![ER inicial](imagenes/1.png)
---

## Paso 2: AFND → AFD (Construcción de subconjuntos)
Siguiendo con la idea del paso anterior, ya tenemos nuestro "esqueleto" unido con cinta adhesiva (las transiciones ε). El problema es que es inestable e impredecible. Si le damos este borrador a un programa de computadora, se va a confundir, porque en un mismo punto podría tener varias opciones distintas para una misma letra, o incluso moverse sin recibir ninguna letra.

Para solucionar esto, usamos la "Construcción de subconjuntos". Lo que hacemos es tomar todos esos caminos confusos y agruparlos en bloques sólidos o "súper estados" (A, B, C, D). Es como quitar toda la cinta adhesiva y poner tornillos definitivos: ahora construimos un mecanismo donde, si estás en un punto y recibes una pieza (una letra 'a' o 'b'), solo hay un único camino posible a seguir.

### Tabla de transición

| Estado | a | b |
|--------|---|---|
| A      | B | A |
| B      | B | C |
| C      | B | D |
| D      | B | A |

--- 

## AFD Final

Un autómata limpio, determinista y seguro. No importa qué palabra intentes procesar, la máquina fluirá por este mapa de forma automática y siempre sabrá si la palabra termina correctamente en la meta (D) o no.

![AFD final](imagenes/2.png)

### Probemos con una palabra real

Veamos si la palabra **"aabb"** es aceptada. Para ser válida 
debe terminar en "abb":

| Letra leída | Estábamos en | Vamos a |
|-------------|--------------|---------|
| a           | A            | B       |
| a           | B            | B       |
| b           | B            | C       |
| b           | C            | D √     |

Llegamos a D, que es el estado de aceptación. La palabra **"aabb"** es válida.

Ahora probemos **"ab"** , que NO debería ser aceptada:

| Letra leída | Estábamos en | Vamos a |
|-------------|--------------|---------|
| a           | A            | B       |
| b           | B            | C       |

Terminamos en C, no en D. Por esto la palabra **"ab"** es rechazada.
--- 

## Conclusiones
- Se requiere un paso intermedio (AFND)
- Se eliminan transiciones ε
- Se obtiene un autómata determinista equivalente

---

#  Parte 2: Conversión de AFD → Gramática

## Descripción
---
Este proceso permite convertir un Autómata Finito Determinista (AFD) en una **Gramática Regular (Tipo 3)**.


Ahora vamos a hacer el proceso inverso. Imagina que el autómata (el AFD) es una máquina física y perfecta que ya funciona. Nuestro objetivo ahora es escribir un manual de instrucciones para que cualquier persona, sin tener la máquina, pueda fabricar las mismas palabras paso a paso. A este manual lo llamamos Gramática Regular (Tipo 3).


## Idea clave

- Cada estado del AFD → un **no terminal**
- Cada transición → una **producción**
- Cada estado final → producción con **ε**



Para traducir el mapa de la máquina a un manual escrito, solo necesitamos aplicar tres reglas sencillas de nuestra "fábrica":

1.Cada círculo (Estado) → Un trabajador (Variable o No Terminal): Representado por letras mayúsculas. Ellos son los encargados de poner una letra nueva y pasar el trabajo al siguiente.

2.Cada flecha (Transición) → Una regla de ensamblaje (Producción): Nos dice qué letra pone el trabajador y a quién le pasa la palabra incompleta después.

3.Cada meta (Estado final) → El sello de "Terminado" (ε): Una regla especial que significa que la palabra ya está lista para salir de la fábrica.

---

## AFD dado
Vamos a trabajar con esta máquina de dos estados:

![AFD dado](imagenes/3.png)
---

## Tabla de transición

| Estado | a | b |
|--------|---|---|
| S      | A | S |
| A      | A | S |



Si miramos la máquina, funciona de esta manera:

| Si el turno lo tiene... | y pone la pieza 'a', el turno pasa a... | y pone la pieza 'b', el turno pasa a... |
|--------|---|---|
| **S** (Trabajador inicial) | A | S (Se queda con el turno) |
| **A** (Trabajador final) | A (Se queda con el turno) | S |

---


## Paso 1: Asignar variables

Cada estado del AFD se convierte en una variable (no terminal):

- S → estado inicial  
- A → estado  

A cada círculo de nuestra máquina le asignamos un "trabajador" (letras mayúsculas):

- **S → Nuestro trabajador inicial.** Él siempre pone la primera pieza.
- **A → Nuestro segundo trabajador.**

---

---

## Paso 2: Crear producciones

Se crean producciones a partir de las transiciones del AFD:

- δ(S, a) = A → S → aA  
- δ(S, b) = S → S → bS  
- δ(A, a) = A → A → aA  
- δ(A, b) = S → A → bS  

Ahora convertimos las flechas del mapa en instrucciones escritas. *La lógica es: El trabajador actual fabrica una pieza (minúscula) y le pasa el turno al siguiente trabajador (Mayúscula).*

- Si el trabajador **S** pone una **'a'**, le pasa el turno a **A**. → Esto se escribe: **S → aA**
- Si el trabajador **S** pone una **'b'**, él mismo se queda el turno. → Esto se escribe: **S → bS**
- Si el trabajador **A** pone una **'a'**, él mismo se queda el turno. → Esto se escribe: **A → aA**
- Si el trabajador **A** pone una **'b'**, le pasa el turno de vuelta a **S**. → Esto se escribe: **A → bS**

---

---

## Paso 3: Estado final

Como el estado **A** es de aceptación, se agrega:

A → ε

Nuestra máquina dice que el estado **A** tiene doble círculo, lo que significa que es un punto de aceptación o "meta".
Para decirle a nuestro manual que cuando la palabra esté en manos del trabajador **A** ya podemos darla por terminada y enviarla al cliente, agregamos esta regla mágica de finalización (donde **ε** o "épsilon" significa simplemente "vacío" o "terminar"):

- **A → ε**

---

---

## Gramática final

S → aA | bS

A → aA | bS | ε


Así queda nuestra Gramática Regular:

**S → aA | bS** *(Traducción humana: El trabajador S puede poner una 'a' y pasarle el trabajo a A, O poner una 'b' y quedarse trabajando).*

**A → aA | bS | ε** *(Traducción humana: El trabajador A puede poner una 'a' y quedarse trabajando, O poner una 'b' y devolverle el trabajo a S, O poner su sello de terminado [ε] y acabar la palabra).*

---


## Conclusiones

- El proceso es directo y sistemático  
- Cada estado se convierte en una variable  
- Cada transición se convierte en producción  
- Los estados finales generan ε  
- Se mantiene el mismo lenguaje del AFD  

---

## Errores comunes

- Olvidar agregar ε en estados finales  
- No incluir todas las transiciones  
- Confundir estados con símbolos  
- Escribir mal las producciones 