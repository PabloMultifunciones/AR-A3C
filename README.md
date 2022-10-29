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

Con estos pasos de capacitación, esperamos ver una aceleración lineal con la cantidad de agentes. Sin embargo, la cantidad de agentes que nuestra máquina puede admitir está limitada por la cantidad de núcleos de CPU disponibles. Además, A3C incluso puede escalar a más de una máquina, y algunas investigaciones más recientes (como IMPALA) admiten escalarlo aún más. ¡Mira el enlace para obtener información más detallada!

### Implementacion ###

Necesitamos hacer que nuestros agentes trabajen en paralelo. Primero definamos qué tipo de modelo usaremos. El agente maestro tendrá la red global y cada agente trabajador local tendrá una copia de esta red en su proceso.

<p align="center">
  <img src="https://user-images.githubusercontent.com/95035101/198832284-cdb38864-fee8-4637-aa74-c9e3641bbd1c.png">
</p>
