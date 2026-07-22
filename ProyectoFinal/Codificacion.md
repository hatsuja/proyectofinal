<div align="center">
    
# Codificación y Validación del programa

</div>

La codificación del programa se realizó en el lenguaje de programación C

## Código en C

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <time.h>

#ifdef _WIN32
    #include <windows.h>
#endif

// Estructuras
typedef struct {
    char nombre[50];
    char guia[100];
    char series[30];
} Ejercicio;

typedef struct {
    char nombre[50];
    float peso;
    float alturaCm;
    int opcionNivel;
    float factorCalorias;
    float imc;
} Usuario;

#define TIPO_VOLUMEN 0
#define TIPO_RECOMPOSICION 1
#define TIPO_DEFICIT 2
// --- PROTOTIPOS DE FUNCIONES ---
void limpiarPantalla();
void mostrarFraseMotivacional();
void registrarUsuario(Usuario *user);
void mostrarPlanPersonalizado(const Usuario *user);
void configurarRutina(int opcionNivel, Ejercicio rutinaAsignada[], int *tiempoMinutos);
int iniciarCronometro(int segundosTotales, const char *nombreMensaje);
int configurarTiempoManual();
void ejecutarRutinaPredefinida(int cantidadEjercicios, const Ejercicio rutinaAsignada[], int tiempoMinutosRutina, float factorCalorias, int *contadorRutinas);
void ejecutarCronometroLibre(float factorCalorias, int *contadorRutinas);
void mostrarReporteFinal(const char *nombreUsuario, int contadorRutinas);
// --- FUNCIONES AUXILIARES ---
void limpiarPantalla() {
    #ifdef _WIN32
        system("cls");
    #else
        system("clear");
    #endif
}

void mostrarFraseMotivacional() {
    char frases[4][100] = {
        "El unico entrenamiento malo es aquel que no ocurrio.",
        "Tu cuerpo puede soportar casi cualquier cosa.",
        "No tienes que ser extremo, solo consistente.",
        "Pequenos progresos diarios dan grandes resultados."
    };
    srand(time(NULL));
    int indice = rand() % 4;
    printf("\n======================================================\n");
    printf(" MOTIVACION DEL DIA:\n");
    printf("   \"%s\"\n", frases[indice]);
    printf("======================================================\n\n");
}
// --- MODULOS DE CONFIGURACION Y DATOS ---
void registrarUsuario(Usuario *user) {
    limpiarPantalla();
    printf("--- BIENVENIDO A FITLIFE APP --- \n");
    printf("Por favor, ingresa tu nombre: ");
    scanf(" %[^\n]", user->nombre);
    mostrarFraseMotivacional();
    do {
        printf("Por favor, ingresa tu peso en kg (20 a 300): ");
        scanf("%f", &user->peso);
        printf("Por favor, ingresa tu altura en cm (50 a 250): ");
        scanf("%f", &user->alturaCm);
        if (user->peso < 20.0 || user->peso > 300.0 || user->alturaCm < 50.0 || user->alturaCm > 250.0) {
            printf("\n Error: Valores ilogicos ingresados.\n\n");
        }
    } while (user->peso < 20.0 || user->peso > 300.0 || user->alturaCm < 50.0 || user->alturaCm > 250.0);
    float alturaM = user->alturaCm / 100.0;
    user->imc = user->peso / (alturaM * alturaM);
    printf("\nSELECCIONA TU NIVEL DE DIFICULTAD:\n");
    printf("1. Principiante (Factor: 5.0 kcal/min)\n");
    printf("2. Normal (Factor: 7.5 kcal/min)\n");
    printf("3. Experto (Factor: 10.0 kcal/min)\n");
    do {
        printf("Elige una opcion (1-3): ");
        scanf("%d", &user->opcionNivel);
    } while (user->opcionNivel < 1 || user->opcionNivel > 3);
    if (user->opcionNivel == 1) user->factorCalorias = 5.0;
    else if (user->opcionNivel == 2) user->factorCalorias = 7.5;
    else user->factorCalorias = 10.0;
}

void mostrarPlanPersonalizado(const Usuario *user) {
    const char *menusNutricionales[3][3] = {
        // [0] TIPO_VOLUMEN (Bajo peso)
        {
            "Batido de leche entera, platano, avena y mantequilla de mani.", // Desayuno
            "Carne de res o pollo, arroz blanco y pure de camote.",          // Almuerzo
            "Pescado o pollo con papas al horno y aguacate."                 // Cena
        },
        // [1] TIPO_RECOMPOSICION (Peso normal)
        {
            "Tortilla de claras de huevo con espinacas y avena en agua.",
            "Pechuga de pollo a la plancha, arroz integral y ensalada verde.",
            "Filete de pescado al horno con brocoli al vapor."
        },
        // [2] TIPO_DEFICIT (Sobrepeso)
        {
            "Huevos revueltos con tomate, cebolla y te verde (sin azucar).",
            "Pechuga de pavo a la plancha con una porcion grande de ensalada mixta.",
            "Atun en agua con lechuga, pepino y un chorrito de limon."
        }
    };
    // Arreglos auxiliares para los titulos y etiquetas
    const char *titulosPlanes[3] = {
        "PLAN DE VOLUMEN (Enfoque: Subir masa muscular magra)",
        "PLAN DE RECOMPOSICION (Enfoque: Mantener peso y tonificar)",
        "PLAN DE DEFICIT CALORICO (Enfoque: Perder grasa corporal)"
    };
    const char *etiquetasComidas[3] = {"Desayuno", "Almuerzo", "Cena"};
        // INICIO DE LA INTERFAZ
    limpiarPantalla();
    printf("========================================================\n");
    printf("             TU PLAN PERSONALIZADO FITLIFE              \n");
    printf("========================================================\n");
    printf("Usuario: %s\n", user->nombre);
    printf("Tu IMC calculado es: %.1f\n\n", user->imc);
    printf(" PLAN NUTRICIONAL SUGERIDO:\n");
    int indicePlan; // Guardará el número del plan que le toca al usuario
    // ---------------------------------------------------------
    // AMPLIAR LA LÓGICA DE SALUD (if / else if / else)
    // ---------------------------------------------------------
    if (user->imc < 18.5) {
        indicePlan = TIPO_VOLUMEN;           // Bajo peso
    } 
    else if (user->imc >= 18.5 && user->imc <= 24.9) {
        indicePlan = TIPO_RECOMPOSICION;     // Peso normal
    } 
    else {
        indicePlan = TIPO_DEFICIT;           // Sobrepeso (IMC >= 25.0)
    }
    printf("-> %s\n", titulosPlanes[indicePlan]);
    // Un bucle for recorre el arreglo multidimensional para imprimir las comidas
    for (int i = 0; i < 3; i++) {
        printf("   * %s: %s\n", etiquetasComidas[i], menusNutricionales[indicePlan][i]);
    }
    printf("\n RECOMENDACION DE HIDRATACION ESTANDAR:\n");
    printf("-> Beber al menos 2 a 3 litros de agua al dia.\n");
    printf("--------------------------------------------------------\n");
}

void configurarRutina(int opcionNivel, Ejercicio rutinaAsignada[], int *tiempoMinutos) {
    if (opcionNivel == 1) {
        *tiempoMinutos = 1;
        strcpy(rutinaAsignada[0].nombre, "Sentadillas libres"); 
        strcpy(rutinaAsignada[0].guia, "Baja la cadera como si te sentaras en una silla."); 
        strcpy(rutinaAsignada[0].series, "3 series x 10 rep");
        
        strcpy(rutinaAsignada[1].nombre, "Flexiones con rodillas"); 
        strcpy(rutinaAsignada[1].guia, "Apoya rodillas en el suelo y baja el pecho."); 
        strcpy(rutinaAsignada[1].series, "3 series x 8 rep");
        
        strcpy(rutinaAsignada[2].nombre, "Plancha Abdominal"); 
        strcpy(rutinaAsignada[2].guia, "Apoya antebrazos y manten el cuerpo alineado."); 
        strcpy(rutinaAsignada[2].series, "3 series x 20 seg");
    } else if (opcionNivel == 2) {
        *tiempoMinutos = 1;
        strcpy(rutinaAsignada[0].nombre, "Sentadillas con salto"); 
        strcpy(rutinaAsignada[0].guia, "Haz una sentadilla clasica y salta."); 
        strcpy(rutinaAsignada[0].series, "4 series x 12 rep");
        
        strcpy(rutinaAsignada[1].nombre, "Flexiones de pecho"); 
        strcpy(rutinaAsignada[1].guia, "Cuerpo estirado, baja el pecho al suelo."); 
        strcpy(rutinaAsignada[1].series, "4 series x 12 rep");
        
        strcpy(rutinaAsignada[2].nombre, "Zancadas / Lunges"); 
        strcpy(rutinaAsignada[2].guia, "Da un paso adelante bajando la rodilla."); 
        strcpy(rutinaAsignada[2].series, "4 series x 10 por pierna");
    } else {
        *tiempoMinutos = 1;
        strcpy(rutinaAsignada[0].nombre, "Burpees"); 
        strcpy(rutinaAsignada[0].guia, "Flexion, encoge piernas y salto explosivo."); 
        strcpy(rutinaAsignada[0].series, "4 series x 15 rep");
        
        strcpy(rutinaAsignada[1].nombre, "Flexiones diamante"); 
        strcpy(rutinaAsignada[1].guia, "Manos juntas para triceps."); 
        strcpy(rutinaAsignada[1].series, "4 series x 12 rep");
        
        strcpy(rutinaAsignada[2].nombre, "Sentadillas bulgaras"); 
        strcpy(rutinaAsignada[2].guia, "Un pie atras en una silla y baja."); 
        strcpy(rutinaAsignada[2].series, "4 series x 12 por pierna");
    }
}
// --- MODULOS DE CRONOMETROS Y CALORIAS ---
int iniciarCronometro(int segundosTotales, const char *nombreMensaje) {
    int tiempoRestante = segundosTotales;
    printf("\n %s\n", nombreMensaje);
    printf("Presiona Ctrl+C en tu teclado si deseas salir antes.\n\n");
    while (tiempoRestante > 0) {
        int minutos = tiempoRestante / 60;
        int segundos = tiempoRestante % 60;
        printf("\rTiempo restante: %02d:%02d ", minutos, segundos);
        fflush(stdout);
        #ifdef _WIN32
            Sleep(1000);
        #else
            struct timespec ts = {1, 0};
            nanosleep(&ts, NULL);
        #endif
        tiempoRestante--;
    }
    printf("\n\n Transcurso finalizado con exito.\n");
    return 1;
}

int configurarTiempoManual() {
    int minutos = 0;
    do {
        printf("\n CONFIGURACION DE CRONOMETRO LIBRE:\n");
        printf("Ingresa los minutos que deseas cronometrar (Max 30 min): ");
        scanf("%d", &minutos);
        if (minutos <= 0 || minutos > 30) {
            printf("\n Error: El tiempo debe ser mayor a 0 y no pasar de 30 minutos.\n");
        }
    } while (minutos <= 0 || minutos > 30);  
    return minutos;
}

float calcularCalorias(int minutos, float factorCalorias) {
    return minutos * factorCalorias;
}

// --- MODULOS DE EJECUCION DE ENTRENAMIENTO ---

void ejecutarRutinaPredefinida(int cantidadEjercicios, const Ejercicio rutinaAsignada[], int tiempoMinutosRutina, float factorCalorias, int *contadorRutinas) {
    char comenzar;
    printf("\nDeseas iniciar la rutina predefinida ahora? (s/n): ");
    scanf(" %c", &comenzar);
    if (comenzar == 's' || comenzar == 'S') {
        int minutosPorEjercicio = tiempoMinutosRutina / cantidadEjercicios;
        if (minutosPorEjercicio < 1) minutosPorEjercicio = 1; 
        int segundosPorEjercicio = minutosPorEjercicio * 60;
        int rutinaCompletaExitosa = 1;
        for (int i = 0; i < cantidadEjercicios; i++) {
            printf("\n--------------------------------------------------\n");
            printf("Iniciando ejercicio %d de %d:\n", i + 1, cantidadEjercicios);
            printf("• %s (%s)\n", rutinaAsignada[i].nombre, rutinaAsignada[i].series);
            printf("• Guia: %s\n", rutinaAsignada[i].guia);
            char mensaje[100];
            snprintf(mensaje, sizeof(mensaje), "EJERCICIO EN CURSO: %s", rutinaAsignada[i].nombre);
            int resultado = iniciarCronometro(segundosPorEjercicio, mensaje);
            if (resultado != 1) {
                rutinaCompletaExitosa = 0;
                break;
            }
        }
        if (rutinaCompletaExitosa == 1) {
            (*contadorRutinas)++;
            float totalCalorias = calcularCalorias(tiempoMinutosRutina, factorCalorias);
            printf("\n--- REPORTE DE ENTRENAMIENTO ---\n");
            printf("Calorias quemadas: %.1f kcal\n", totalCalorias);
            printf("Rutinas completadas registradas en total: %d\n", *contadorRutinas);
        }
    } else {
        printf("\nPlan guardado para luego.\n");
    }
}

void ejecutarCronometroLibre(float factorCalorias, int *contadorRutinas) {
    int minutosLibres = configurarTiempoManual();
    char comenzarLibre;
    printf("\nDeseas iniciar el cronometro libre ahora? (s/n): ");
    scanf(" %c", &comenzarLibre);
    if (comenzarLibre == 's' || comenzarLibre == 'S') {
        int estadoLibre = iniciarCronometro(minutosLibres * 60, "CRONOMETRO LIBRE EN CURSO...");
        
        if (estadoLibre == 1) {
            (*contadorRutinas)++;
            float totalCaloriasLibre = calcularCalorias(minutosLibres, factorCalorias);
            printf("\n--- REPORTE DE CRONOMETRO LIBRE ---\n");
            printf("Calorias quemadas: %.1f kcal\n", totalCaloriasLibre);
            printf("Rutinas completadas registradas en total: %d\n", *contadorRutinas);
        }
    } else {
        printf("\nCronometro libre cancelado.\n");
    }
}

void mostrarReporteFinal(const char *nombreUsuario, int contadorRutinas) {
    printf("\n======================================================\n");
    printf("REPORTE FINAL DE LA JORNADA:\n");
    printf("• Rutinas completadas: %d\n", contadorRutinas);
    if (contadorRutinas >= 1) {
        printf("¡Objetivo cumplido! Has completado al menos una sesion de entrenamiento hoy. ¡Excelente trabajo, %s!\n", nombreUsuario);
    } else {
        printf("No registraste ninguna sesion completa de entrenamiento hoy. ¡Animo para la proxima, %s!\n", nombreUsuario);
    }
    printf("======================================================\n");
    printf("¡Gracias por usar FitLife App! ¡Hasta luego!\n");
}
// --- PROGRAMA PRINCIPAL ---
int main() {
    #ifdef _WIN32
        SetConsoleOutputCP(65001); // UTF-8 para Windows
    #endif
    Usuario user;
    int contadorRutinas = 0;
    int opcionMenuPrincipal = 0;
    int cantidadEjercicios = 3;
    Ejercicio rutinaAsignada[3];
    int tiempoMinutosRutina = 0;
    registrarUsuario(&user);
    mostrarPlanPersonalizado(&user);
    configurarRutina(user.opcionNivel, rutinaAsignada, &tiempoMinutosRutina);
    do {
        printf("\n======================================================\n");
        printf(" RUTINA ASIGNADA PARA TU NIVEL (%d min totales)\n\n", tiempoMinutosRutina);
        for (int i = 0; i < cantidadEjercicios; i++) {
            printf("%d. %s (%s)\n", i + 1, rutinaAsignada[i].nombre, rutinaAsignada[i].series);
            printf("   Guia: %s\n\n", rutinaAsignada[i].guia);
        }
        printf("======================================================\n");
        int tipoEntrenamiento = 0;
        printf("Como deseas entrenar hoy?\n");
        printf("1. Usar la rutina predefinida asignada\n");
        printf("2. Usar cronometro libre independiente (Max. 30 min)\n");
        printf("Elige una opcion (1-2): ");
        scanf("%d", &tipoEntrenamiento);
        if (tipoEntrenamiento == 1) {
            ejecutarRutinaPredefinida(cantidadEjercicios, rutinaAsignada, tiempoMinutosRutina, user.factorCalorias, &contadorRutinas);
        } else {
            ejecutarCronometroLibre(user.factorCalorias, &contadorRutinas);
        }
        printf("\n======================================================\n");
        printf("Que deseas hacer ahora?\n");
        printf("1. Realizar otra actividad (Continuar en la app)\n");
        printf("2. Salir del programa\n");
        printf("Elige una opcion (1-2): ");
        scanf("%d", &opcionMenuPrincipal);
    } while (opcionMenuPrincipal == 1); 
    mostrarReporteFinal(user.nombre, contadorRutinas); 
    return 0;
}
```

## Validación y pruebas de escritorio

[📂 Ir al Índice Principal 📂](https://github.com/hatsuja/proyectofinal)




