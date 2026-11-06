**Organización de computadores**
**Proyecto 4 - Programación**
**2026-2**

# Integrantes

| Nombre | Usuario GitHub | Aportes principales |
|---|---|---|
| (completar) | (completar) | (completar) |
| (completar) | (completar) | (completar) |
| (completar) | (completar) | (completar) |

# Compilación y ejecución

Herramientas: `JackCompiler` y `VMEmulator` de Nand2Tetris, 2.ª edición (carpeta `tools` del curso).

1. Compilar la carpeta del proyecto con el compilador oficial:
   - Windows: `tools\JackCompiler.bat RUTA\proyecto4`
   - Linux / macOS: `tools/JackCompiler.sh RUTA/proyecto4`
2. Abrir `tools/VMEmulator` y elegir **File → Load Program**; seleccionar la carpeta `proyecto4`.
3. Elegir **No animation** y la velocidad máxima, y presionar **Run**.
4. Hacer clic en la pantalla del emulador para que reciba las teclas.

No incluya los `.vm` del sistema operativo en la carpeta: el emulador usa los suyos. Los `.vm` generados por el compilador se regeneran al compilar.

# Controles

| Tecla | Código | Acción |
|---|---|---|
| Flecha arriba | 131 | Sube un carril (una vez por pulsación) |
| Flecha abajo | 133 | Baja un carril (una vez por pulsación) |
| P | 80 | Pausa y continuar |
| R | 82 | Reiniciar: vuelve a la pantalla inicial sin recargar el programa |
| ESC | 140 | Salir del juego |
| 1 / 2 / 3 (pantalla inicial) | 49 / 50 / 51 | Elegir la distancia a la meta: 40, 80 o 150 pasos |

# Diseño de clases

El juego tiene ocho clases propias. La lógica no depende de la pantalla ni del teclado.

| Clase | Responsabilidad | Constructor |
|---|---|---|
| `Main` | Crea el juego, lo ejecuta y lo libera. | No |
| `Juego` | Controlador: lee el teclado con `Keyboard.keyPressed()`, maneja los modos (menú, jugando, pausa, fin), y avanza `Carrera` cada cinco cuadros de 30 ms. | Sí |
| `Pantalla` | Presentación: dibuja la vía, los carriles, el marcador y los mensajes. Redibuja solo las celdas que cambiaron. No cambia el estado de la carrera. | Sí |
| `Sprites` | Guarda los nueve sprites de 16 × 16 píxeles, creados una sola vez. | Sí |
| `Carrera` | Lógica del juego según la sección 8: pasos, choques, salida, generación, vidas y puntaje. No usa `Screen`, `Output`, `Keyboard` ni `Sys.wait`. | Sí |
| `Obstaculo` | Un obstáculo en la vía: carril, columna y columna anterior. | Sí |
| `Tipo` | Datos de un tipo de obstáculo (largo, velocidad, letal, premio). El catálogo tiene los cuatro tipos de la tabla 7.3. | Sí |
| `Azar` | Generador pseudoaleatorio (x ← 5x + 3), inicializado con la semilla de `Carrera`. | Sí |

**Especialización sin herencia.** Los cuatro tipos de obstáculo (cono, camión, moto, moneda) son una sola clase, `Obstaculo`. Cada obstáculo contiene una referencia a un objeto `Tipo` del catálogo, que define su largo, su velocidad, si es letal y cuántos puntos da. `Carrera` pregunta al `Tipo` (por ejemplo, `tipo.paso()`) en lugar de comparar el tipo con números dispersos en el código. Agregar un tipo nuevo es agregar una línea en `Tipo.catalogo()`.

**Separación de la lógica y la presentación.** `Carrera` solo recibe pedidos (`moverCarro`, `avanzar`, `generar`) y responde consultas (`ocupada`, `vidas`, `puntaje`). `Pantalla` consulta el estado y dibuja; `Juego` decide cuándo. Por eso las pruebas automáticas pueden usar `Carrera` sin teclado ni pantalla.

# Rutinas VM

Se escribió a mano una rutina en lenguaje VM, en `Grafico.vm`:

- **`Grafico.dibujarCelda(fila, col, sprite)`**: copia las 16 líneas del sprite a la memoria de video. La dirección destino se calcula sumando 512 por fila y 32 por línea, sin `Math.multiply`. Se usan los segmentos `pointer 0`/`this` para leer el sprite y `pointer 1`/`that` para escribir en la pantalla. Es llamada desde `Pantalla.fondo`, `Pantalla.barra` y `Pantalla.dibujarCarriles`, es decir, para dibujar la vía, la barra de progreso, el carro y todos los obstáculos.

No se escribieron funciones de bonificación.

# Uso de IA

Herramienta: Claude (Anthropic). Se usó para escribir el código inicial de todas las clases Jack, el archivo `Grafico.vm` y este README. Los integrantes del grupo deben revisar cada parte, poder explicarla en la sustentación y confirmar esta declaración antes de entregar.
