# AR-A3C
Aprendizaje Reforzado - Método Ventaja Actor-Crítico Asincrono (A3C)

### Introduccion ###

Entonces, ¿qué es A3C? El grupo DeepMind de Google lanzó el algoritmo A3C en 2016, y esencialmente superó a DQN. Fue más rápido, más simple, más robusto y logró puntajes mucho mejores en las tareas estándar de Deep RL. Además de todo eso, podría funcionar tanto en espacios de acción continuos como discretos. A3C se convirtió en el algoritmo Deep RL de referencia para nuevos problemas desafiantes con estados complejos y espacios de acción.
  
Método Ventaja Actor-Crítico Asincrono es bastante difícil de implementar. Comencemos por desglosar el nombre y luego la mecánica detrás del algoritmo en sí.
  
*<strong>Asíncrono</strong>: El algoritmo es un algoritmo asíncrono en el que varios agentes trabajadores se entrenan en paralelo, cada uno con su entorno. Esto permite que nuestro algoritmo se capacite más rápido a medida que más trabajadores se capaciten en paralelo y obtenga una experiencia de capacitación más diversa ya que la experiencia de cada trabajador es independiente.  
  
*<strong>Ventaja</strong>: La ventaja es una métrica para juzgar qué tan buenas fueron sus acciones y cómo resultaron. Esto permite que el algoritmo se centre en dónde faltaban las predicciones de la red. Intuitivamente, esto nos permitirá medir la ventaja de tomar acción, siguiendo la política π en el intervalo de tiempo dado.
  
*<strong>Actor-Crítico</strong>: El aspecto Actor-Crítico del algoritmo utiliza una arquitectura que comparte capas entre la política y la función de valor.

### ¿Cómo funciona A3C? ###

En un alto nivel, el algoritmo A3C utiliza un esquema de actualización asincrónica que opera en pasos de experiencia de duración fija en un entorno continuo y pasos de experiencia de duración por lotes en un entorno episódico. Utilizará estos segmentos para calcular los estimadores de las recompensas y la función de ventaja. Cada trabajador realiza el siguiente ciclo de flujo de trabajo:

1. Obtener los parámetros de la red global.  
2. Interactuar con el entorno siguiendo la política local durante n número de pasos.  
3. Calcular el valor y la pérdida de la politica.  
4. Obtener los gradientes de pérdidas.  
5. Actualizar la red global.  
6. Repetir.  

Con estos pasos de capacitación, esperamos ver una aceleración lineal con la cantidad de agentes. Sin embargo, la cantidad de agentes que nuestra máquina puede admitir está limitada por la cantidad de núcleos de CPU disponibles. Además, A3C incluso puede escalar a más de una máquina, y algunas investigaciones más recientes (como IMPALA) admiten escalarlo aún más.

La diferencia clave con A2C es la parte asíncrona. A3C consta de múltiples agentes independientes (redes) con sus propios pesos, que interactúan con una copia diferente del entorno en paralelo. Así, pueden explorar una mayor parte del espacio de estado-accion en mucho menos tiempo.

<p align="center">
  <img src="https://user-images.githubusercontent.com/95035101/198832284-cdb38864-fee8-4637-aa74-c9e3641bbd1c.png">
</p>

### Implementaciones del mundo real de A3C ###

A3C es un algoritmo de aprendizaje automático efectivo porque utiliza múltiples agentes para explorar el entorno, posiblemente explorando de manera más eficiente el espacio de estado (especialmente para ayudar al agente a aprender). Además, A3C utiliza una red compartida entre agentes, lo que permite que cada agente se beneficie de las experiencias de los demás en la red.

A3C se ha empleado en algunas implementaciones de inteligencia artificial reconocibles. Como ilustración, se puede implementar para desarrollar un algoritmo que podría identificar objetos en imágenes con mayor precisión. Otra aplicación involucró el uso de A3C para permitir que los robots naveguen a través de entornos con técnicas para evitar la gestión de obstáculos. También tiene aplicaciones en toda la industria financiera, especialmente en ingeniería financiera: A3C podría realizar análisis predictivos en la fijación de precios.

Algunos otros ejemplos del mundo real de A3C en inteligencia artificial incluyen el reconocimiento facial y la visión por computadora. El reconocimiento facial se usa para identificar ciertas personas u objetos en imágenes o videos digitales, y la visión por computadora usa algoritmos que permiten a las computadoras interpretar los detalles de una imagen, incluida la iluminación, la pose y las texturas.

### Ventajas ###

*Este algoritmo es más rápido y más robusto que los algoritmos estándar de aprendizaje por refuerzo.  
*Funciona mejor que las otras técnicas de aprendizaje por refuerzo debido a la diversificación del conocimiento como se explicó anteriormente.  
*Se puede utilizar tanto en espacios de acción discretos como continuos.  

### Desventajas ###

El principal inconveniente de la asincronía es que algunos agentes jugarán con una versión anterior de los parámetros. Por supuesto, la actualización puede no ocurrir de forma asíncrona sino al mismo tiempo. En ese caso, tenemos una versión mejorada de A2C con múltiples agentes en lugar de uno. A2C esperará a que todos los agentes terminen su segmento y luego actualizará los pesos de la red global y restablecerá a todos los agentes.

Pero. Siempre hay un pero. Algunos argumentan que no es necesario tener muchos agentes si son sincrónicos, ya que esencialmente no son diferentes en absoluto. Y estoy de acuerdo. De hecho, lo que hacemos es crear múltiples versiones del entorno y solo dos redes.

La primera red (generalmente conocida como modelo de pasos) interactúa con todos los entornos durante n pasos de tiempo en paralelo y genera un lote de experiencias. Con esa experiencia, entrenamos la segunda red (modelo de tren) y actualizamos el modelo de pasos con los nuevos pesos. Y repetimos el proceso.

<p align="center">
  <img src="https://user-images.githubusercontent.com/95035101/198839812-a46ad1f8-4f7d-4dc5-822c-70c2ae1beff3.jpg">
</p>

