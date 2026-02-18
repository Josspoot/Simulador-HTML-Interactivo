# Simulador de Asignación de Memoria: Best Fit vs Worst Fit
https://josspoot.github.io/Simulador-HTML-Interactivo/

Este proyecto es una herramienta interactiva web diseñada para visualizar y comparar dos de los algoritmos más comunes de administración de memoria en Sistemas Operativos, el simulador permite observar en tiempo real cómo se fragmenta la memoria y cómo cada estrategia decide ubicar los procesos.

---

## 1. Introducción
El simulador presenta una comparativa visual entre las estrategias de **Mejor Ajuste (Best Fit)** y **Peor Ajuste (Worst Fit)**. A través de una interfaz sencilla, el usuario puede generar bloques de memoria de tamaños aleatorios y asignar procesos para analizar el comportamiento de cada algoritmo frente a la fragmentación externa.

---

## 2. Instrucciones de Uso

### ¿Qué debe hacer el usuario?
1. **Configurar la memoria:** Ingrese el número de bloques deseados (mínimo 3, máximo 10) y haga clic en **Generar**.
2. **Definir procesos:** Ingrese el tamaño del proceso en kilobytes (KB) en el campo correspondiente.
3. **Asignar:** Presione el botón **Asignar** para enviar el proceso a ambos algoritmos simultáneamente.

### ¿Qué se observa en pantalla?
* **Bloques Verdes:** Representan el espacio libre disponible.
* **Bloques Rojos:** Representan el espacio ocupado por un proceso (identificados como P1, P2, etc.).
* **División de Bloques:** Cuando un proceso entra en un bloque, este se divide en la parte ocupada y el residuo libre.

### Interpretación de resultados
Al asignar varios procesos, notarás que un algoritmo puede agotar sus bloques grandes más rápido que el otro, o dejar espacios pequeños que ya no pueden ser utilizados por nuevos procesos.

---

## 3. Explicación de los Algoritmos

El simulador evalúa cada bloque $b$ disponible para un proceso de tamaño $s$ bajo las siguientes reglas:

### Best Fit (Mejor Ajuste)
Busca el bloque libre donde el desperdicio sea el mínimo posible. La lógica es encontrar el bloque con tamaño $b$ tal que:
* $b \ge s$
* $(b - s)$ sea el valor mínimo de toda la lista.

**Ventaja:** Preserva los bloques de memoria más grandes para procesos futuros que realmente los necesiten.
**Desventaja:** Suele crear fragmentos libres muy pequeños (residuos de tamaño $1/n$) que terminan siendo inútiles.

### Worst Fit (Peor Ajuste)
Busca el bloque libre más grande disponible en la memoria. La lógica es encontrar el bloque con tamaño $b$ tal que:
* $b = \max(lista\_de\_bloques)$
* $b \ge s$

**Ventaja:** Al usar el bloque más grande, el residuo generado ($b - s$) suele ser lo suficientemente grande para albergar otro proceso.
**Desventaja:** Agota rápidamente los bloques de gran tamaño, lo que puede impedir la carga de procesos pesados más adelante.

---

## 4. Reflexión y Análisis
La eficiencia de la gestión de memoria no es absoluta. En este simulador se puede comprobar que:
1. **Best Fit** aprovecha mejor los espacios pequeños pero sufre de una fragmentación externa más agresiva.
2. **Worst Fit** mantiene la utilidad de los espacios sobrantes por más tiempo, pero es ineficiente si llegan procesos que requieren casi la totalidad de la memoria disponible.

Este ejercicio demuestra que la elección del algoritmo depende de la carga de trabajo esperada en el sistema operativo.

---

## Tecnologías Utilizadas
* **HTML:** Estructura de la página.
* **CSS:** Estilizado visual y diferenciación de estados de memoria.
* **JavaScript (Vanilla):** Lógica de los algoritmos de búsqueda y manipulación del DOM.

---
**Clausula uso de IA**
* nombre: Josue Poot
* IA utilizada: gemini
* motivo: entender el Best Fit y Worst Fit
* promt: ayudame a entender como hacer el java scipt de el worst fit y best fit
* respuesta(Raw):

Entiendo, Joss. Entrarle a la lógica de punteros y búsqueda en arreglos puede ser confuso al principio. La clave está en cómo recorremos la "lista" de bloques de memoria para encontrar el lugar ideal.

Aquí te explico la lógica de los dos algoritmos paso a paso.

---

## 1. El concepto base: `forEach`

En ambos algoritmos, usamos `mem.forEach((b, i) => { ... })`. Esto significa: "Revisa cada bloque `b` de la memoria, y dime su índice `i`".

Para que un proceso de tamaño `size` quepa, el bloque debe cumplir dos condiciones iniciales:

1. `b.free === true` (Que el bloque esté vacío).
2. `b.size >= size` (Que el proceso quepa físicamente).

---

## 2. Lógica de Best Fit (El "Apretado")

El objetivo es encontrar el bloque que deje el **menor espacio sobrante**.

```javascript
function bestFit(mem, size) {
  let idx = -1; // No hemos encontrado nada aún
  let min = Infinity; // Empezamos con un tamaño "infinito" para poder comparar

  mem.forEach((b, i) => {
    // Si está libre Y cabe Y además es MÁS PEQUEÑO que el que encontramos antes
    if (b.free && b.size >= size && b.size < min) {
      min = b.size; // Este es nuestro nuevo "mejor candidato"
      idx = i;      // Guardamos su posición
    }
  });
  return idx; // Al final, regresamos el bloque más ajustado
}

```

**Metáfora:** Es como si intentaras meter un libro en una mochila; buscas la mochila más pequeña donde el libro apenas quepa para no desperdiciar espacio en las mochilas grandes.

---

## 3. Lógica de Worst Fit (El "Holgado")

Aquí buscamos lo contrario: el bloque **más grande de todos**.

```javascript
function worstFit(mem, size) {
  let idx = -1;
  let max = -1; // Empezamos en -1 para que cualquier bloque sea más grande

  mem.forEach((b, i) => {
    // Si está libre Y cabe Y además es MÁS GRANDE que el máximo anterior
    if (b.free && b.size >= size && b.size > max) {
      max = b.size; // Este es el bloque más grande encontrado hasta ahora
      idx = i;      // Guardamos su posición
    }
  });
  return idx;
}

```

**Metáfora:** Aquí buscas la mochila más grande que tengas, para que después de meter el libro, aún te quede mucho espacio libre para guardar otras cosas.

---

## 4. La función `allocateBlock` (La división)

Esta es la parte más interesante de tu código. Cuando ya elegimos un bloque (`index`), no solo lo llenamos, sino que lo **partimos**.

1. **Calculamos el resto:** `let rest = mem[index].size - size;`
2. **Ocupamos el bloque:** Marcamos el bloque original como `free: false` y le ponemos el nombre del proceso (P1, P2...).
3. **Creamos un nuevo bloque:** Si sobró espacio (`rest > 0`), usamos `mem.splice(index + 1, 0, ...)` para insertar un **nuevo bloque libre** justo después del que acabamos de ocupar.

---

### Ejemplo rápido:

Tienes un bloque de **500 KB** y tu proceso es de **200 KB**.

* **Antes:** `[ {size: 500, free: true} ]`
* **Después:** `[ {size: 200, free: false, name: "P1"}, {size: 300, free: true} ]`

Como puedes ver, el bloque de 500 se "rompió" en uno de 200 ocupado y uno de 300 libre.

¿Te queda clara la diferencia entre usar `min = b.size` vs `max = b.size`, o te gustaría que hiciéramos un ejercicio rápido con valores numéricos?

