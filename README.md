# Asistente de voz para Windows

Asistente de escritorio desarrollado en Python para controlar tareas habituales del sistema mediante comandos de voz en español. El proyecto combina reconocimiento y síntesis de voz con automatización de aplicaciones, ventanas, multimedia y funciones de productividad.

## Funciones principales

- Reconocimiento de voz en español mediante `SpeechRecognition`.
- Respuesta por voz con `pyttsx3`.
- Consulta de la hora.
- Búsquedas en Google.
- Reproducción de música en YouTube.
- Apertura y cierre de aplicaciones.
- Control de volumen del sistema.
- Movimiento de ventanas entre monitores.
- Maximización y cierre de ventanas.
- Pausa y reanudación de reproducción multimedia.
- Capturas de pantalla.
- Cambio de dispositivo de salida de audio.
- Lectura en voz alta de PDF y archivos de texto.
- Creación de notas mediante dictado.
- Recordatorios y temporizadores.
- Apertura de Google Calendar.
- Modo de dictado para escribir en la aplicación activa.
- Atajos personalizados para tareas como compilar, ejecutar o gestionar pestañas.
- Limpieza básica de papelera y archivos temporales.
- Modo silencio para evitar respuestas cuando no se desea interacción.

## Compatibilidad

El proyecto está orientado principalmente a **Windows**.

Varias funciones dependen directamente de características del sistema operativo, como:

- control de audio mediante `pycaw`;
- apertura de aplicaciones instaladas en Windows;
- automatización de ventanas;
- limpieza de la papelera;
- ejecución de comandos de PowerShell;
- uso de atajos de teclado del sistema.

Algunas rutas de aplicaciones se configuran manualmente en `abrir_apps.py`, por lo que deben adaptarse al equipo donde se ejecute el asistente.

## Estructura del proyecto

```text
asistente/
├── asistente.py
├── abrir_apps.py
├── cerrar_apps.py
├── mover_ventanas.py
├── Sistema_y_Multimedia.py
├── productividad.py
├── volumen.py
├── asistente.spec
└── .gitignore
```

### Módulos

#### `asistente.py`

Punto de entrada del programa. Gestiona:

- escucha del micrófono;
- reconocimiento de comandos;
- síntesis de voz;
- búsqueda web;
- reproducción multimedia;
- selección y ejecución de las distintas acciones.

#### `abrir_apps.py`

Contiene un diccionario de aplicaciones conocidas y sus rutas locales para poder abrirlas mediante comandos de voz.

Antes de utilizar esta función es necesario adaptar esas rutas a las aplicaciones instaladas en cada equipo.

#### `cerrar_apps.py`

Busca procesos mediante `psutil` y permite cerrarlos a partir del nombre indicado por voz.

#### `mover_ventanas.py`

Gestiona ventanas y configuraciones multimonitor:

- búsqueda de ventanas por título;
- movimiento entre monitores;
- activación;
- maximización.

#### `Sistema_y_Multimedia.py`

Incluye funciones relacionadas con el sistema y contenido multimedia:

- selección del dispositivo de audio;
- lectura de PDF o texto;
- creación de notas mediante voz.

#### `productividad.py`

Agrupa herramientas de productividad:

- recordatorios;
- temporizadores;
- Google Calendar;
- dictado de texto;
- atajos personalizados;
- limpieza de archivos temporales y papelera.

#### `volumen.py`

Controla el volumen maestro de Windows mediante `pycaw`.

## Instalación

Se recomienda utilizar un entorno virtual:

```bash
python -m venv .venv
```

En Windows:

```powershell
.venv\Scripts\Activate.ps1
```

Instala las dependencias declaradas en `requirements.txt`:

```bash
pip install -r requirements.txt
```

> La instalación de PyAudio puede variar según la versión de Python y Windows utilizada.

## Configuración

### 1. Rutas de aplicaciones

Edita el diccionario `apps_disponibles` de `abrir_apps.py` para que apunte a las rutas reales de las aplicaciones instaladas en tu equipo.

Ejemplo:

```python
apps_disponibles = {
    "code": r"C:\ruta\a\Code.exe",
    "word": r"C:\ruta\a\WINWORD.exe",
}
```

### 2. Cambio de dispositivo de audio

La función de cambio de salida de audio ejecuta un comando de PowerShell que utiliza los cmdlets `Get-AudioDevice` y `Set-AudioDevice`.

Para utilizarla, el equipo debe disponer del módulo de PowerShell correspondiente.

### 3. Micrófono

Asegúrate de que Windows permite a Python acceder al micrófono y de que existe un dispositivo de entrada configurado correctamente.

## Ejecución

Desde la carpeta del proyecto:

```bash
python asistente.py
```

Al iniciarse, el asistente saluda y comienza a escuchar comandos.

## Ejemplos de comandos

Algunos comandos reconocidos por la implementación actual son:

```text
hora
busca arquitectura de computadores
reproduce una canción
abre spotify
cuéntame un chiste
abrir code
cierra chrome
sube el volumen
baja el volumen
mueve chrome al monitor dos
pantalla completa
captura de pantalla
cambiar salida de audio
leer archivo
toma nota
recuérdame comprar pan a las 18:00
calendario
temporizador de 5 minutos
escribir
compilar
ejecutar
actualiza
nueva pestaña
cierra pestaña
hacer limpieza
silencio
habla
salir
```

El reconocimiento se realiza en español de España mediante `recognize_google(..., language="es-ES")`.

## Modo silencio

El comando:

```text
silencio
```

activa un modo en el que el asistente evita responder verbalmente a comandos no reconocidos.

Para volver al modo normal:

```text
habla
```

## Empaquetado con PyInstaller

El repositorio conserva `asistente.spec`, que permite generar un ejecutable con PyInstaller.

Ejemplo:

```bash
pip install pyinstaller
pyinstaller asistente.spec
```

Las carpetas generadas `build/` y `dist/` no se versionan.

## Limitaciones actuales

- Las rutas de algunas aplicaciones están configuradas manualmente y deben adaptarse a cada equipo.
- El reconocimiento de voz de Google requiere conexión a Internet.
- Varias acciones dependen específicamente de Windows.
- El sistema de comandos se basa actualmente en coincidencias de frases y palabras, no en interpretación semántica avanzada.
- Algunas acciones pueden depender de la aplicación que tenga el foco en ese momento.
- Las tareas de automatización deben utilizarse con precaución, especialmente el cierre de procesos y la limpieza de archivos temporales.

## Objetivo del proyecto

Este proyecto sirve como experimentación práctica con reconocimiento de voz, automatización del escritorio y control del sistema desde Python, integrando diferentes bibliotecas en una única interfaz basada en comandos hablados.
