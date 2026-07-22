<div align="center">

# FitLife

</div>

---

# Análisis del Programa

## Problemática central 

Lo que resuelve FitLife App es la falta de accesibilidad, personalización y constancia en el seguimiento de la salud y el acondicionamiento físico diario, Muchas personas desean llevar un estilo de vida saludable, pero se enfrentan a tres barreras principales que esta aplicación aborda directamente:

* No todos los usuarios tienen el mismo peso, altura o nivel de condición física. Entrenar sin una guía personalizada o con ejercicios genéricos suele llevar a la frustración, al abandono o incluso a lesiones.

* Existen muchas calculadoras de IMC o tablas nutricionales en internet, pero están aisladas y el usuario necesita un sistema que conecte sus datos corporales (IMC), su nivel de experiencia, un plan nutricional acorde y una rutina de ejercicios en un solo lugar.

* Entrenar como un cronómetr o sin un refuerzo positivo constante hace que sea muy fácil perder la noción del tiempo o la motivación para terminar una sesión.

## ¿Cómo soluciona la app esta problemática?

* Valida los datos corporales del usuario y calcula su IMC para determinar automáticamente su perfil y un factor de calorías adecuado.

* Ofrece planes específicos según las necesidades reales del usuario, eliminando las adivinanzas sobre qué comer.

* Utiliza un cronómetro interactivo multiplataforma que guía ejercicio por ejercicio, calcula las calorías quemadas al finalizar y registra las rachas de rutinas completadas para fomentar la constancia diaria.

# Requerimientos del sistema

## Gestión de Datos Corporales
* **Función asociada:** `registrarUsuario()`

El sistema debe permitir al usuario ingresar sus datos personales básicos (nombre, peso y altura). Cuenta con una estructura de control `do-while` que rechaza estrictamente valores ilógicos mediante validaciones específicas: un peso menor o igual a 0 o mayor a 300 kg, y una altura menor a 50.0 cm o mayor a 250 cm, solicitando el reingreso inmediato de los datos en caso de error. Los valores numéricos se almacenan en variables de tipo `float` dentro de una estructura (`Usuario`) para asegurar que el cálculo del IMC y las operaciones derivadas mantengan precisión y eviten errores de redondeo.

---

## Plan de Nutrición Predictiva
* **Función asociada:** `mostrarPlanPersonalizado()`

El sistema debe calcular automáticamente el Índice de Masa Corporal (IMC) del usuario y utilizar un arreglo bidimensional estático de cadenas (`menusNutricionales[3][3]`) para clasificar el estado corporal en tres rangos específicos mediante estructuras condicionales (`if / else if / else`): Bajo peso ($IMC < 18.5$), Peso normal ($18.5 \le IMC \le 24.9$) y Sobrepeso ($IMC \ge 25.0$). A partir de esta clasificación, despliega un plan nutricional adaptado que incluye desayuno, almuerzo y cena, además de una recomendación estándar e independiente de hidratación fijada de 2 a 3 litros de agua al día.

---

## Entrenamiento Personalizado
* **Función asociada:** `configurarRutina()`

El sistema debe desplegar un menú de niveles de dificultad (1. Principiante, 2. Normal, 3. Experto) con una estructura `do-while` que restringe la selección únicamente a opciones válidas entre 1 y 3. Según la opción elegida, la función configura un arreglo de estructuras (`Ejercicio`) que separa de forma estructurada el nombre del ejercicio, la guía técnica de movimiento y el volumen de entrenamiento expresado en series y repeticiones, garantizando una lectura clara para el usuario.

---

## Cronómetro, Control de Tiempo y Cálculo de Calorías
* **Funciones asociadas:** `configurarTiempoManual()`, `iniciarCronometro()`, `calcularCalorias()`, `ejecutarRutinaPredefinida()`, `ejecutarCronometroLibre()`

El sistema permite al usuario elegir entre dos modalidades de entrenamiento mediante un menú interactivo:

* **Rutina predefinida:** Ejecuta de forma secuencial los ejercicios asignados para su nivel, controlando el tiempo por ejercicio y validando la confirmación del usuario (s/n).
* **Cronómetro libre independiente (`configurarTiempoManual`):** Funciona como un cronómetro libre e independiente donde el usuario configura manualmente el tiempo deseado. El sistema restringe los límites de entrada mediante un `do-while`, aceptando únicamente valores enteros mayores a 0 y con un límite máximo de 30 minutos.

### Control de tiempo y registro:
* El sistema debe procesar los segundos totales mediante una cuenta regresiva que actualiza la visualización en formato `MM:SS` en tiempo real y realiza pausas por segundo adaptadas al sistema operativo (Windows o sistemas POSIX).
* El sistema evalúa el estado de finalización del cronómetro: si el usuario interrumpe la ejecución (por ejemplo, con `Ctrl+C`), la función retorna un estado diferente de 1, interpretándose como no finalizado y evitando sumar datos falsos. El cronómetro solo incrementa el `contadorRutinas` y ejecuta la función `calcularCalorias()` si el temporizador llega a cero por completo.

### Cálculo de calorías:
Para la estimación del gasto energético, el sistema aplica un factor multiplicador en kcal/min estrictamente proporcional al nivel de dificultad seleccionado por el usuario:
* **Principiante:** 5.0 kcal/min
* **Normal:** 7.5 kcal/min
* **Experto:** 10.0 kcal/min

---

## Cierre y Reporte Final
* **Función asociada:** `mostrarReporteFinal()`

El sistema gestiona un ciclo principal controlado por un menú que permite al usuario repetir actividades (`opcionMenuPrincipal == 1`) o finalizar el programa. Al detectar la opción de salida, el flujo interrumpe el ciclo e invoca la función de cierre para generar un reporte final de la jornada. Este reporte evalúa el esfuerzo contrastado con un objetivo base, validando mediante una estructura condicional si el `contadorRutinas` es mayor o igual a 1 para notificar con un mensaje personalizado el cumplimiento de al menos una sesión completada durante el día.

# Diseño del programa


Estructura de desglose de las funciones

  <img width="1269" height="353" alt="image" src="https://github.com/user-attachments/assets/6d55075b-10ba-4264-a7f8-4bf90139b234" />

[📂 Ir al Índice Principal 📂](https://github.com/hatsuja/proyectofinal)



