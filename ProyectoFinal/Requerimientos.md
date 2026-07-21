<div align="center">

# Programa y Análisis del Programa

</div>

---

### A. Módulo de Gestión de Datos Corporales 
* `datosUsuario();`

El sistema debe permitir al usuario ingresar sus datos personales básicos, específicamente el peso y la altura además deberá rechazar valores ilógicos por ejemplo peso menor a 20 kg o mayor a 300 kg, y a su vez altura menor a 50 cm o mayor a 250 cm mediante una estructura de control que solicite reingresar el dato. Los datos deberán almacenarse en variables (float) para asegurar que el cálculo del IMC no sufra errores de redondeo.

---

### B. Módulo de Nutrición Predictiva
* `planDieta();`

El sistema debe generar y mostrar un plan de comidas o dieta sugerida, incluso deberá clasificar el IMC en categorías como peso bajo, normal, sobrepeso, obesidad, adaptando la recomendación nutricional de forma específica para cada rango, por otro lado el plan nutricional deberá incluir sugerencias de hidratación básica por ejemplo "beber 2 litros de agua al día" como parte de la recomendación estándar, independientemente del plan elegido.

---

### C. Módulo de Entrenamiento Personalizado
* `entrenamiento();`

El sistema debe mostrar un menú de rutinas de ejercicio y permitir al usuario elegir entre tres niveles de intensidad predefinidos como principiante, normal o experto estructura de datos de las rutinas debe separar el nombre del ejercicio, la descripción técnica como la guía de movimiento y el volumen de entrenamiento como series o repeticiones, permitiendo una lectura clara para el usuario.

---

### D. Módulo de Cronómetro, Control de Tiempo y Calcular calorías
* `configurarTiempoManual();`
* `inciarRutinaCronometro();`
* `calcularCalorias();`

El sistema debe reconocer si el usuario quiere una rutina libre e iniciar un cronómetro personalizable el cual debe llegar a cero, y llamar a la función de cálculo de calorías para esa rutina libre además no puede aceptar valores ilógicos en la configuración del cronometro ya que debe ser valores positivivos.

Si el usuario decide iniciar la rutina el sistema debe reconocer el nivel de entrenamiento posteriormente seleccionando para ejecutar acciones automáticas como sumar "1" al contador de rutinas que el usuario a realizado y llamar a la función de cálculo de calorías cuando el usuario haya decidido parar la rutina de entrenamiento.

El sistema deberá diferenciar entre el estado "En curso" y "Finalizado", garantizando que el cronómetro solo registre datos (calorías y rutinas) si el temporizador llega a cero por completo.

La visualización del tiempo deberá ser en formato `MM:SS`.

Para el calculo de las calorías el sistema deberá aplicar un factor multiplicador de calorías (kcal/min) que sea proporcional a la dificultad del nivel que el usuario haya seleccionado:
* **Principiante:** 5.0 kcal/min
* **Normal:** 7.5 kcal/min
* **Experto:** 10.0 kcal/min

---

### E. Módulo de Cierre y Reporte Final
* `resumenDiario();`

El sistema debe detectar la opción de "Salir" del menú principal para interrumpir el programa y, antes de cerrarse, el sistema deberá generar un reporte del esfuerzo realizado frente a un objetivo base, notificando al usuario si ha completado al menos una sesión de entrenamiento durante su jornada.


  
[📂 Ir al Índice Principal 📂](https://github.com/hatsuja/proyectofinal)



