# 🎵 Digio Music Manager

**Digio Music Manager** es una aplicación de escritorio desarrollada en **Python + Tkinter** para organizar una biblioteca musical, crear playlists y transferir canciones a reproductores MP3/MP4 que se conectan al computador como una unidad USB.

El proyecto nació para volver a utilizar un **Digio antiguo de aproximadamente 2 GB**, pero también incluye un perfil de exportación para un segundo reproductor con **10 GB de capacidad de referencia** y soporte anunciado de formatos como FLAC.

> **Versión:** 1.2 · **Sistema principal:** Windows · **Idioma:** Español  
> Proyecto independiente, no oficial ni afiliado a Digio u otros fabricantes.

## ✨ Características

- **Importación de música por carpetas**, con búsqueda de archivos de audio en subcarpetas.
- **Creación y edición de playlists**: agregar canciones, eliminarlas de una lista, renombrar listas y modificar el orden de reproducción.
- **Dos perfiles de dispositivo** con configuraciones de exportación distintas.
- **Opciones de calidad MP3**: 128, 192, 256 y 320 kbps.
- **Conversión opcional a MP3 o FLAC** mediante FFmpeg, cuando el formato de origen lo requiere.
- **Copia de archivos originales sin recomprimir**, cuando el formato seleccionado lo permite.
- **Exportación de cada playlist a una carpeta**, con archivos numerados (`001_`, `002_`, etc.) y un archivo auxiliar `.m3u`.
- **Estimación del espacio necesario** y consulta del espacio libre real en la unidad de destino.
- **Transferencia con barra de progreso**, cancelación y resumen de archivos copiados, omitidos o con errores.
- **Persistencia local** de carpetas importadas, playlists y preferencias entre sesiones.
- Accesos a **plataformas de descarga gratuita autorizada** desde la propia interfaz.

La aplicación **no borra los archivos musicales originales**, no formatea unidades y evita sobrescribir canciones existentes durante la exportación.

## 🖥️ Requisitos

| Componente | Requisito |
| --- | --- |
| Sistema operativo | Windows (uso principal previsto) |
| Python | **3.9 o posterior** para ejecutar el código fuente |
| Interfaz gráfica | Tkinter, incluido habitualmente con Python en Windows |
| FFmpeg | **Opcional**; necesario para convertir formatos de audio |
| Conexión USB | Para transferir música a un reproductor conectado como almacenamiento |

No se necesitan paquetes adicionales de `pip` para ejecutar la interfaz desde Python.

## 🚀 Instalación y ejecución

1. Descarga o clona el repositorio.
2. Abre una terminal en la carpeta donde está `digio_music_manager.py`.
3. Ejecuta:

   ```powershell
   py digio_music_manager.py
   ```

   También puedes utilizar `python digio_music_manager.py` si ese es el comando disponible en tu equipo.

Si descargaste el paquete que incluye `ABRIR_DIGIO.bat`, puedes utilizarlo en Windows para iniciar la aplicación.

### FFmpeg para conversiones

Si quieres convertir canciones desde formatos como FLAC, WAV, M4A o WMA a MP3, o generar archivos FLAC, instala [FFmpeg](https://ffmpeg.org/download.html) y comprueba que el comando `ffmpeg` esté disponible en el **PATH** del sistema:

```powershell
ffmpeg -version
```

Sin FFmpeg podrás seguir creando playlists y copiando archivos que **no necesitan conversión**. Las conversiones pendientes se omitirán y la aplicación te avisará.

## 🎧 Perfiles y calidad de audio

| Perfil | Capacidad de referencia | Formatos de salida ofrecidos | Recomendación |
| --- | --- | --- | --- |
| **Digio NX5858** | Aprox. 2 GB | MP3 | MP3 de 128–192 kbps |
| **Reproductor blanco** | 10 GB de referencia | MP3, FLAC u original compatible | MP3 de 256–320 kbps; probar FLAC antes de transferir grandes colecciones |

**Importante:** las capacidades son referencias, no comprobaciones automáticas del hardware. El programa consulta el **espacio libre real** de la unidad elegida. La compatibilidad efectiva con FLAC, `.m3u` y otros formatos depende del firmware de cada reproductor y todavía requiere pruebas en los dispositivos físicos.

**La calidad de origen importa:** convertir un MP3 de 128 kbps a 320 kbps o FLAC **no recupera** la información de audio perdida. Por defecto, un MP3 existente se copia sin volver a comprimir; la recompresión se puede activar de forma explícita.

## 📂 Cómo usarlo

1. **Biblioteca y playlists:** pulsa **Agregar carpeta** para importar música. También puedes crear una playlist directamente desde una carpeta.
2. Crea una playlist con **Nueva** y agrega las canciones seleccionadas desde la biblioteca.
3. Organiza el orden con los botones **↑** y **↓**.
4. **Dispositivo y calidad:** selecciona el perfil, la unidad USB de destino, el formato de salida y la calidad MP3.
5. Elige si quieres guardar las listas dentro de `MUSIC` o directamente en la raíz de la unidad.
6. Revisa la estimación de espacio y pulsa **Copiar playlists al MP4**.
7. Una vez terminada la transferencia, **expulsa la unidad de forma segura** desde Windows.

### Ejemplo de estructura exportada

```text
REPRODUCTOR (E:)\
└── MUSIC\
    ├── Rock\
    │   ├── 001_Cancion_A_abc123.mp3
    │   └── 002_Cancion_B_def456.mp3
    ├── Electronica\
    │   └── 001_Cancion_C_ghi789.mp3
    ├── DIGIO_Rock.m3u
    └── DIGIO_Electronica.m3u
```

Los nombres mostrados son **ilustrativos**. La aplicación numera los archivos para facilitar la reproducción en el orden esperado incluso cuando el dispositivo no reconoce listas `.m3u`.

> **Nota:** cambiar o eliminar una playlist dentro del programa **no borra automáticamente** una exportación anterior del dispositivo. No se implementa sincronización bidireccional.

## 💾 Dónde se guardan las playlists

En Windows, la configuración y las listas se guardan en:

```text
%APPDATA%\DigioMusicManager\biblioteca.json
```

Este archivo contiene **rutas a las canciones originales**, no copias de los archivos de audio. Si mueves o eliminas los archivos de origen, tendrás que volver a importarlos o actualizar tus playlists.

## 📦 Crear un ejecutable `.exe` (onefile + windowed)

Puedes distribuir el programa como **un único ejecutable sin ventana de consola** mediante PyInstaller.

**1. Instala PyInstaller:**

```powershell
py -m pip install --upgrade pyinstaller
```

**2. Compila desde la carpeta del script:**

```powershell
py -m PyInstaller --onefile --windowed --clean --name "DigioMusicManager" digio_music_manager.py
```

**3. Encuentra el ejecutable:**

```text
dist\DigioMusicManager.exe
```

El `.exe` incluye el intérprete de Python y la interfaz, por lo que el usuario final **no necesita instalar Python**. **FFmpeg no queda incluido automáticamente** en esta compilación: para utilizar conversiones, el computador donde se ejecute debe disponer de `ffmpeg` en su PATH o debes preparar un empaquetado adicional.

Opcionalmente, puedes incorporar un ícono `.ico` agregando `--icon="digio.ico"` al comando de compilación.

## 🎶 Música gratuita y uso autorizado

La pestaña **Descargas gratuitas** abre los sitios de:

- [Bandcamp](https://bandcamp.com)
- [SoundCloud](https://soundcloud.com)
- [Jamendo Music](https://www.jamendo.com)
- [Free Music Archive](https://freemusicarchive.org)

La descarga depende de las opciones ofrecidas por cada artista, obra y plataforma. **Digio Music Manager no descarga canciones directamente**, no elude DRM ni extrae contenido protegido de servicios de streaming. Descarga únicamente los archivos que tengas derecho a obtener y utilizar.

## ⚠️ Limitaciones conocidas

- El reconocimiento de playlists `.m3u` depende del firmware; las carpetas con canciones numeradas funcionan como alternativa de organización.
- La conversión depende de FFmpeg instalado y detectable desde el PATH.
- El modo **Original** del segundo perfil se limita a las extensiones consideradas compatibles por la configuración de la aplicación; esto **no certifica** su reproducción real.
- La estimación del espacio de archivos convertidos es aproximada; depende de la duración y compresión de cada canción.
- La aplicación no descarga música, no administra bibliotecas de Spotify o YouTube Music y no sincroniza eliminaciones con el reproductor.
- Los sistemas de archivos distintos de FAT/FAT16/FAT32 generan una advertencia de compatibilidad; **la aplicación no formatea discos**.
- Antes de transferir mucha música, se recomienda probar unos pocos archivos directamente en el reproductor.

## 🛠️ Tecnologías

- Python 3.9+
- Tkinter / ttk
- Biblioteca estándar de Python
- FFmpeg (opcional, para conversiones)
- PyInstaller (opcional, para empaquetado en Windows)

## 📄 Licencia

**Pendiente de definir.** Antes de distribuir o aceptar contribuciones, incorpora al repositorio un archivo `LICENSE` con la licencia que decidas utilizar. Subir código a GitHub no concede automáticamente permisos de reutilización a terceros.
