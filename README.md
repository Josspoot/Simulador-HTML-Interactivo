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
