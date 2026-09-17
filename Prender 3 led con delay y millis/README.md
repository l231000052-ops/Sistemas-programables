# Comparación entre delay() y millis() para el Control de Múltiples LEDs

Práctica sobre paradigmas de ejecución en Arduino: control de tres LEDs con distintos intervalos de parpadeo (500 ms, 1000 ms y 1500 ms), comparando el uso de `delay()` frente a `millis()`, e incorporando una tarea adicional de impresión periódica por el puerto serie en la versión no bloqueante.

## Descripción

Este proyecto compara dos formas de manejar el tiempo en Arduino: la Parte 1 usa `delay()` para controlar tres LEDs, bloqueando por completo la ejecución del programa mientras transcurre cada pausa; la Parte 2 reimplementa el mismo control con `millis()`, logrando que cada LED parpadee de forma independiente y simultánea, además de agregar una tarea extra (mensaje "Hola Mundo" cada 3000 ms por el monitor serie) que corre en paralelo sin afectar el parpadeo.

## Objetivos de aprendizaje

Comparar de forma práctica el uso de `delay()` frente a `millis()` para el control de temporización de múltiples LEDs, evidenciando cómo la temporización no bloqueante permite ejecutar varias tareas de forma concurrente dentro de un mismo `loop()`.

## Herramientas y material utilizado

* Arduino UNO R4 WiFi
* Arduino IDE / Tinkercad
* Protoboard
* LEDs: rojo, amarillo y verde
* Resistencias limitadoras de corriente
* Cables de conexión (jumpers)

## Diagrama del circuito

![Diagrama del circuito](Diagrama/Diagrama%20delay()%20y%20millis().png)

## Montaje físico

![Montaje físico](Diagrama/Armado.jpeg)

## Código

* [Parte 1 - delay()](https://github.com/l231000052-ops/Sistemas-programables/blob/main/Paradigmas%20de%20Ejecucion/Parte1_delay.ino)
* [Parte 2 - millis()](https://github.com/l231000052-ops/Sistemas-programables/blob/main/Paradigmas%20de%20Ejecucion/Parte2_millis.ino)

## Reporte

[Reporte.pdf](Reporte/Reporte.pdf)

## Resultados

Con `delay()`, los LEDs se encendían y apagaban en secuencia estricta, con un ciclo total mucho más largo (6 segundos) que la suma de sus intervalos nominales, y no fue posible agregar la tarea del monitor serie sin alterar aún más los tiempos. Con `millis()`, los tres LEDs parpadearon de forma simultánea respetando sus intervalos individuales (500, 1000 y 1500 ms), y el mensaje "Hola Mundo" se imprimió cada 3 segundos por el monitor serie sin afectar el comportamiento de los LEDs, confirmando la capacidad de la temporización no bloqueante para ejecutar múltiples tareas en paralelo.

## Video del funcionamiento

* [Ver video con delay()](https://www.youtube.com/watch?v=uhysPlJfVlU)
* [Ver video con millis()](https://youtu.be/SooWojiCWjc)

## Conclusiones

Esta práctica dejó clara la diferencia entre bloquear el procesador con `delay()` y liberar su tiempo con `millis()`. Comparar ambas versiones del código permitió entender por qué `millis()` es indispensable cuando un sistema necesita atender varias tareas o eventos externos al mismo tiempo, y sentó las bases para prácticas posteriores como el semáforo con solicitud peatonal.

