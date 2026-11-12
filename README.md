# 🚀 Mars Photos

Aplicación Android desarrollada con **Kotlin** y **Jetpack Compose** que consume un servicio web REST para obtener fotografías de la superficie de Marte. Es el proyecto práctico del curso **Android Basics with Compose** de Google Developers, y demuestra cómo conectar una app a internet siguiendo una arquitectura **MVVM** con corrutinas.

> **Referencia:** [Get data from the internet – Android Developers Codelab](https://developer.android.com/codelabs/basic-android-kotlin-compose-getting-data-internet)

---

## 📑 Tabla de contenidos

- [Estado del proyecto](#-estado-del-proyecto)
- [Características](#-características)
- [Arquitectura](#-arquitectura)
- [Tecnologías](#-tecnologías)
- [Estructura del proyecto](#-estructura-del-proyecto)
- [Requisitos](#-requisitos)
- [Instalación y ejecución](#-instalación-y-ejecución)
- [API utilizada](#-api-utilizada)
- [Roadmap](#-roadmap)
- [Créditos y licencia](#-créditos-y-licencia)

---

## 📌 Estado del proyecto

El proyecto se encuentra en la fase de **conexión con la red** del codelab: la app realiza la petición HTTP, deserializa el JSON y muestra en pantalla el resultado, por ejemplo: `Success: N Mars photos retrieved`.

La visualización de las imágenes en una cuadrícula (con **Coil**, ya incluido como dependencia) está planificada en el [Roadmap](#-roadmap).

## ✨ Características

- Petición HTTP `GET` asíncrona con **Retrofit** y **OkHttp**.
- Deserialización de JSON con **kotlinx.serialization**.
- Gestión de tres estados de interfaz: **Cargando**, **Éxito** y **Error**.
- Manejo de errores de red (`IOException`) y de servidor (`HttpException`).
- Interfaz 100 % declarativa con **Jetpack Compose** y **Material 3**.
- Previews de Compose para cada pantalla de estado.

## 🏗️ Arquitectura

La app sigue el patrón **MVVM** con flujo de datos unidireccional:

```
┌──────────────┐   estado    ┌───────────────┐   suspend fun   ┌────────────────┐
│  HomeScreen  │ ◄────────── │ MarsViewModel │ ───────────────►│  MarsApiService │
│   (Compose)  │             │  (UiState)    │ ◄───────────────│   (Retrofit)    │
└──────────────┘             └───────────────┘   List<MarsPhoto>└────────────────┘
```

| Capa | Archivo | Responsabilidad |
|------|---------|-----------------|
| **Modelo** | `model/MarsPhoto.kt` | Data class serializable con `id` e `imgSrc` (`img_src` en el JSON). |
| **Red** | `network/MarsApiService.kt` | Configura Retrofit, define `GET photos` y expone el singleton `MarsApi`. |
| **ViewModel** | `ui/screens/MarsViewModel.kt` | Lanza la petición en `viewModelScope` y publica un `MarsUiState` (`Loading`, `Success`, `Error`). |
| **Vista** | `ui/screens/HomeScreen.kt` | Renderiza `LoadingScreen`, `ResultScreen` o `ErrorScreen` según el estado. |
| **Tema** | `ui/theme/` | `Color.kt`, `Shape.kt`, `Theme.kt` y `Type.kt` (Material Design 3). |

## 🛠️ Tecnologías

| Categoría | Herramienta |
|-----------|-------------|
| Lenguaje | [Kotlin](https://kotlinlang.org/) |
| UI | [Jetpack Compose](https://developer.android.com/jetpack/compose) + Material 3 |
| Arquitectura | MVVM, `ViewModel` de Jetpack Lifecycle |
| Red | [Retrofit 2.9.0](https://square.github.io/retrofit/) + [OkHttp 4.11.0](https://square.github.io/okhttp/) |
| Serialización | [kotlinx.serialization 1.6.0](https://github.com/Kotlin/kotlinx.serialization) |
| Asincronía | Corrutinas de Kotlin (`viewModelScope`) |
| Imágenes | [Coil 2.4.0](https://coil-kt.github.io/coil/) (incluida, pendiente de uso) |
| Pruebas | JUnit, Espresso, Compose UI Test |

## 📂 Estructura del proyecto

```text
Marsphotos/
├── app/
│   ├── src/
│   │   ├── main/
│   │   │   ├── java/com/example/marsphotos/
│   │   │   │   ├── model/
│   │   │   │   │   └── MarsPhoto.kt
│   │   │   │   ├── network/
│   │   │   │   │   └── MarsApiService.kt
│   │   │   │   ├── ui/
│   │   │   │   │   ├── MarsPhotosApp.kt
│   │   │   │   │   ├── screens/
│   │   │   │   │   │   ├── HomeScreen.kt
│   │   │   │   │   │   └── MarsViewModel.kt
│   │   │   │   │   └── theme/
│   │   │   │   │       ├── Color.kt
│   │   │   │   │       ├── Shape.kt
│   │   │   │   │       ├── Theme.kt
│   │   │   │   │       └── Type.kt
│   │   │   │   └── MainActivity.kt
│   │   │   ├── res/                  # Drawables, strings, temas, íconos
│   │   │   └── AndroidManifest.xml   # Incluye el permiso INTERNET
│   │   ├── androidTest/              # Pruebas instrumentadas
│   │   └── test/                     # Pruebas unitarias
│   ├── build.gradle.kts
│   └── proguard-rules.pro
├── gradle/                           # Wrapper y catálogo de versiones
├── build.gradle.kts
├── settings.gradle.kts
└── gradlew / gradlew.bat
```

## 📋 Requisitos

- **Android Studio** (versión reciente, se recomienda Hedgehog o superior)
- **JDK 17** o superior (el que incluye Android Studio es suficiente)
- **Android SDK 34**
- Dispositivo físico o emulador con **Android 7.0 (API 24)** o superior
- **Conexión a internet** (necesaria para obtener los datos)

## ▶️ Instalación y ejecución

1. **Clona el repositorio**

   ```bash
   git clone https://github.com/JordyMacias/App-MarsPhotos.git
   cd App-MarsPhotos
   ```

2. **Abre el proyecto** en Android Studio: `File > Open` y selecciona la carpeta clonada.

3. **Sincroniza Gradle** y espera a que se descarguen las dependencias.

4. **Ejecuta la app** (`Run ▶`) en un emulador o dispositivo físico.

También puedes compilar desde la terminal:

```bash
./gradlew assembleDebug        # Genera el APK de depuración
./gradlew test                 # Pruebas unitarias
./gradlew connectedAndroidTest # Pruebas instrumentadas (requiere dispositivo)
```

## 🌐 API utilizada

La app consume el servidor de ejemplo creado por Google para el codelab:

```
GET https://android-kotlin-fun-mars-server.appspot.com/photos
```

Respuesta (fragmento):

```json
[
  { "id": "424906", "img_src": "https://mars.jpl.nasa.gov/msl-raw-images/..." }
]
```

> ℹ️ Es un servicio de práctica que sirve imágenes provenientes de los rovers de la NASA; no es la API oficial de la NASA.

## 🗺️ Roadmap

- [ ] Mostrar las imágenes en una cuadrícula (`LazyVerticalGrid`) con **Coil** (`AsyncImage`).
- [ ] Botón **Reintentar** en la pantalla de error (el string `retry` ya existe).
- [ ] Capa de **repositorio** con inyección manual de dependencias.
- [ ] Pruebas unitarias del `ViewModel` y de la capa de red.
- [ ] Activar minificación (`isMinifyEnabled`) en la variante *release*.

## 👤 Créditos y licencia

- Proyecto basado en el codelab de [Android Developers](https://developer.android.com/courses/android-basics-compose/course), © The Android Open Source Project, bajo licencia [Apache 2.0](https://www.apache.org/licenses/LICENSE-2.0).
- Adaptado y mantenido por **[Jordy Macías](https://github.com/JordyMacias)**.
