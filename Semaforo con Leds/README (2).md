# 🚦 Semáforo Vehicular y Peatonal con Máquina de Estados Finitos (FSM)

Proyecto de control secuencial para **Arduino UNO R4 WiFi** que implementa un semáforo vehicular (rojo/amarillo/verde) con cruce peatonal (rojo/verde) mediante una **máquina de estados finitos**, temporización **no bloqueante** con `millis()` y un pulsador con **antirrebote (debounce) por software**.

## 📋 Descripción

A diferencia de una implementación básica con `delay()`, este proyecto resuelve toda la temporización con `millis()`, de modo que el microcontrolador nunca se bloquea y puede atender el pulsador peatonal en cada iteración del `loop()`. El comportamiento se modela con un `enum class` (`EstadoSemaforo`), lo que evita fugas de nombres al espacio global y deja explícitas las transiciones del sistema.

## 🛠️ Herramientas y material utilizado

- Arduino UNO R4 WiFi
- Arduino IDE
- Protoboard
- LEDs: rojo, amarillo y verde (vehicular) y rojo/verde (peatonal)
- Resistencias limitadoras de corriente
- Pulsador (botón) para la solicitud peatonal
- Cables de conexión (jumpers)

## 🔌 Conexión de pines

| Pin Arduino | Componente               |
|:-----------:|--------------------------|
| D2          | LED Vehicular ROJO       |
| D3          | LED Vehicular AMARILLO   |
| D4          | LED Vehicular VERDE      |
| D5          | LED Peatonal ROJO        |
| D6          | LED Peatonal VERDE       |
| D7          | Pulsador (con `INPUT_PULLUP`) |

> Cada LED debe conectarse con su resistencia limitadora de corriente en serie. El pulsador usa la resistencia pull-up interna del Arduino (`INPUT_PULLUP`), por lo que no necesita resistencia externa.

## 🧠 Máquina de estados

El sistema tiene 4 estados:

```
VEH_VERDE ──(8 s)──▶ VEH_AMARILLO ──(2 s)──▶ VEH_ROJO ─┬─(sin solicitud, 6 s)──▶ VEH_VERDE
                                                         └─(con solicitud peatonal)──▶ PEA_VERDE ──(5 s)──▶ VEH_VERDE
```

| Estado         | Duración | LEDs encendidos                  |
|----------------|:--------:|-----------------------------------|
| `VEH_VERDE`    | 8000 ms  | Vehicular verde + Peatonal rojo   |
| `VEH_AMARILLO` | 2000 ms  | Vehicular amarillo + Peatonal rojo|
| `VEH_ROJO`     | 6000 ms* | Vehicular rojo + Peatonal rojo    |
| `PEA_VERDE`    | 5000 ms  | Vehicular rojo + Peatonal verde   |

\* `VEH_ROJO` termina antes de los 6 s si hay una solicitud peatonal pendiente.

### Reglas de seguridad y prioridad

1. El botón **solo arma** la solicitud peatonal (`solicitudPeatonal = true`) si el vehicular está en **verde o amarillo**.
2. Si se presiona el botón en cualquier otro estado, la pulsación se **ignora**.
3. En cuanto el vehicular llega a rojo, si hay una solicitud pendiente, se atiende **de inmediato** (no espera los 6 s completos).
4. Si nadie solicita el cruce, el ciclo vehicular continúa **de forma autónoma**.

## ⏱️ Temporización no bloqueante

Todo el código evita `delay()`. Cada estado guarda su `millis()` de entrada (`inicioEstado`) y compara el tiempo transcurrido contra su duración objetivo en cada vuelta del `loop()`, permitiendo leer el pulsador sin interrupciones.

## 🔘 Antirrebote del pulsador

La función `botonPresionado()` separa dos roles:

- `ultimaLecturaCruda`: última lectura física del pin, usada solo para reiniciar el temporizador de rebote.
- `estadoEstable`: último valor confirmado como estable durante `T_DEBOUNCE` (50 ms), usado para detectar el flanco de bajada real.

Esto evita activaciones múltiples o falsas por rebote mecánico del botón.

## 📷 Circuito

**Diagrama esquemático (Tinkercad):**

Arduino UNO conectado a una protoboard con 5 LEDs (vehicular: rojo/amarillo/verde, peatonal: rojo/verde) y un pulsador.

**Circuito armado (foto real):**

Implementación física sobre protoboard con Arduino UNO R4 WiFi.

> Las imágenes del diagrama y del circuito armado se encuentran en la carpeta del proyecto (`Diagrama_semaforo.png`, `Armado.jpeg`).

## 🚀 Cómo usarlo

1. Abre el archivo `.ino` en el **Arduino IDE**.
2. Selecciona la placa **Arduino UNO R4 WiFi** y el puerto correspondiente en *Herramientas*.
3. Arma el circuito según la tabla de pines.
4. Sube el código a la placa (*Subir* / `Ctrl+U`).
5. Observa el ciclo vehicular automático y presiona el botón durante verde o amarillo para solicitar el cruce peatonal.

## 📁 Estructura sugerida del repositorio

```
├── semaforo_fsm.ino        # Código fuente
├── Diagrama_semaforo.png   # Diagrama esquemático (Tinkercad)
├── Armado.jpeg             # Foto del circuito armado
└── README.md                # Este archivo
```

## 👥 Integrantes

- Javier
- Gabriel
- Rosa

## 📄 Licencia

Proyecto académico de uso educativo.
