[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/L4KUMW_3)
# 📝 Laboratorio 1: Layout de Calculadora con Jetpack Compose

## 👤 Datos del Estudiante

**Completa la siguiente información antes de comenzar:**

- **Nombre completo**: _____________________________
- **Carrera**: _____________________________
- **Fecha de entrega**: _____________________________

---

## Objetivo

El objetivo de este laboratorio es que los estudiantes creen un layout completo de una calculadora usando Jetpack Compose, incluyendo:
- Una pantalla de texto que muestre los números ingresados.
- Una cuadrícula de botones con los dígitos y operadores.
- Manejo básico de estado con remember y mutableStateOf.

## Introducción

### ¿Qué es Kotlin?
Kotlin es un lenguaje de programación moderno, conciso y expresivo desarrollado por JetBrains. Es 100% interoperable con Java y es el lenguaje preferido por Google para el desarrollo de aplicaciones Android. Kotlin reduce significativamente la cantidad de código repetitivo y ofrece características avanzadas como null safety y funciones de extensión.

### Declaración de Variables en Kotlin
En Kotlin existen dos formas principales de declarar variables:
- **`val`**: Para valores inmutables (constantes). Una vez asignado, no se puede cambiar.
  ```kotlin
  val nombre = "Pablo"  // No se puede reasignar
  ```
- **`var`**: Para variables mutables. Se pueden reasignar después de la declaración inicial.
  ```kotlin
  var edad = 25  // Se puede reasignar
  edad = 26      // Válido
  ```

### Declaración de Funciones en Kotlin
En Kotlin, las funciones se declaran usando la palabra clave `fun`. La sintaxis básica es:

```kotlin
fun nombreFuncion(parametro: Tipo): TipoRetorno {
    // cuerpo de la función
    return valor
}
```

Ejemplos prácticos:
- **Función simple sin parámetros:**
  ```kotlin
  fun saludar(): String {
      return "¡Hola mundo!"
  }
  ```
- **Función con parámetros:**
  ```kotlin
  fun sumar(a: Int, b: Int): Int {
      return a + b
  }
  ```
- **Función de expresión (más concisa):**
  ```kotlin
  fun multiplicar(a: Int, b: Int) = a * b
  ```
- **Función sin valor de retorno (Unit):**
  ```kotlin
  fun mostrarMensaje(mensaje: String) {
      println(mensaje)
  }
  ```

### ¿Qué es Jetpack Compose?
Jetpack Compose es el toolkit moderno de Android para crear interfaces de usuario nativas. Utiliza un paradigma declarativo donde describes cómo debe verse la UI en lugar de cómo construirla paso a paso. Con Compose, escribes funciones que describen tu interfaz y el framework se encarga de actualizarla cuando cambia el estado.

Características principales:
- **Declarativo**: Describes qué quieres mostrar, no cómo crearlo
- **Composable**: Las UI se construyen combinando funciones más pequeñas
- **Reactividad**: La UI se actualiza automáticamente cuando cambia el estado

## Instrucciones

### 🏗️ Estructura del Proyecto Android

Antes de comenzar, es importante entender la estructura básica de un proyecto Android:

```
Calculator/
├── app/
│   ├── build.gradle.kts          # Configuración de dependencias de la app
│   ├── src/
│   │   ├── main/
│   │   │   ├── AndroidManifest.xml      # Configuración de la aplicación
│   │   │   ├── java/ec/edu/uisek/calculator/
│   │   │   │   ├── MainActivity.kt      # Actividad principal
│   │   │   │   └── ui/theme/           # Archivos de tema y estilo
│   │   │   └── res/                    # Recursos (layouts, strings, colores)
│   │   ├── test/                       # Tests unitarios
│   │   └── androidTest/                # Tests de integración
│   └── proguard-rules.pro
├── build.gradle.kts               # Configuración del proyecto
└── gradle/                        # Archivos de Gradle
```

**Archivos clave para este laboratorio:**
- **`MainActivity.kt`**: Punto de entrada de la aplicación donde configuraremos Compose
- **`ui/theme/`**: Contiene los archivos de tema (colores, tipografía, formas)
- **`build.gradle.kts`**: Contiene las dependencias de Jetpack Compose

### 1️⃣ Crear un Composable para la pantalla de entrada

1. Dentro de tu proyecto de Android Studio, crea un nuevo archivo Kotlin llamado:
    ```
    CalculatorScreen.kt
    ```
2. Define un Composable llamado CalculatorScreen:
    ```kotlin
    @Composable
    fun CalculatorScreen() {
        // Estado para almacenar el texto de la pantalla
        var inputText by remember { mutableStateOf("") }

        Column(
            modifier = Modifier
                .fillMaxSize()
                .background(Color.Black)
                .padding(16.dp),
            verticalArrangement = Arrangement.Top
        ) {
            // Pantalla de entrada (TextField)
            TextField(
                value = inputText,
                onValueChange = { inputText = it },
                modifier = Modifier
                    .fillMaxWidth()
                    .padding(bottom = 16.dp),
                textStyle = LocalTextStyle.current.copy(
                    fontSize = 36.sp,
                    textAlign = TextAlign.End,
                    color = Color.White
                ),
                colors = TextFieldDefaults.colors(
                    focusedContainerColor = Color.Transparent,
                    unfocusedContainerColor = Color.Transparent,
                    focusedIndicatorColor = Color.Transparent,
                    unfocusedIndicatorColor = Color.Transparent,
                    cursorColor = Color.White
                ),
                singleLine = true
            )

            // Aquí colocaremos la cuadrícula de botones
            CalculatorGrid { label ->
                inputText += label
            }
        }
    }
    ```

### 2️⃣ Crear un Composable para la cuadrícula de botones

1.	Crea un Composable llamado CalculatorGrid:
    ```kotlin
    @Composable
    fun CalculatorGrid(onButtonClick: (String) -> Unit) {
        val buttons = listOf(
            "7", "8", "9", "÷",
            "4", "5", "6", "×",
            "1", "2", "3", "−",
            "0", ".", "=", "+"
        )

        LazyVerticalGrid(
            columns = GridCells.Fixed(4),
            modifier = Modifier
                .fillMaxWidth()
                .padding(8.dp),
            horizontalArrangement = Arrangement.spacedBy(8.dp),
            verticalArrangement = Arrangement.spacedBy(8.dp)
        ) {
            items(buttons) { label ->
                CalculatorButton(label = label) {
                    onButtonClick(label)
                }
            }
        }
    }
    ```
2.	Crea un Composable auxiliar para cada botón:
    ```kotlin
    @Composable
    fun CalculatorButton(label: String, onClick: () -> Unit) {
        Box(
            modifier = Modifier
                .aspectRatio(1f)
                .background(Color.DarkGray, shape = MaterialTheme.shapes.medium)
                .clickable { onClick() },
            contentAlignment = Alignment.Center
        ) {
            Text(
                text = label,
                color = Color.White,
                fontSize = 24.sp,
                fontWeight = FontWeight.Bold,
                textAlign = TextAlign.Center
            )
        }
    }
    ```

### 3️⃣ Agregar el Composable a la pantalla principal

En tu MainActivity, llama a CalculatorScreen() dentro de setContent:
```kotlin
class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            CalculatorScreen()
        }
    }
}
```

### 4️⃣ Puntos clave que deben revisar los estudiantes
- Uso de dp para dimensiones y sp para texto.
- Uso de remember { mutableStateOf("") } para mantener el estado del TextField.
- Uso de it en lambdas como parámetro implícito.
- Creación de un layout con Column y LazyVerticalGrid para organizar pantalla + botones.

### 5️⃣ Objetivo del ejercicio
Al finalizar, cada estudiante debería poder:
- Ver un TextField en la parte superior que refleja el texto ingresado.
- Ver una cuadrícula de 4×4 botones debajo del TextField.
- Interactuar con los botones y ver cómo se actualiza la pantalla.


### 6️⃣ Datos del docente

Para cualquier inquietud de este ejercicio puedes contactar al docente

- UISEK - Google Chat: pablo.perez@uisek.edu.ec
- PUCE - Microsoft Teams: paperez@puce.edu.ec