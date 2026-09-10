# Chord Autoscroll

**Un editor de texto con auto-scroll y transposición de acordes para guitarristas.**

Chord Autoscroll es una aplicación escrita en Python y PyQt6 diseñada para guitarristas y músicos que necesitan gestionar archivos de canciones en formato texto (.txt) con letras y acordes. Permite leer canciones con desplazamiento automático y transponer acordes al instante para adaptarlos a la voz del cantante o a la afinación del instrumento.

---

## Características

- **Auto-scroll ajustable**: Desplazamiento automático del texto con velocidad configurable en tiempo real.
- **Transposición de acordes**: Transpone acordes musicales de -7 a +7 semitonos, con opción de usar sostenidos o bemoles.
- **Múltiples pestañas**: Abre y edita varias canciones simultáneamente.
- **Verificación ortográfica**: Resalta palabras mal escritas con subrayado rojo ondulado (soporte para español e inglés).
- **Buscar y reemplazar**: Búsqueda de texto con opción de coincidencia exacta.
- **Arrastrar y soltar**: Abre archivos .txt arrastrándolos directamente a la ventana.
- **Configuración persistente**: Guarda automáticamente la fuente, velocidad, preferencia de accidentes y última ruta usada.
- **Detección de codificación**: Identifica automáticamente la codificación de archivos (UTF-8, ISO-8859-1, Windows-1252, etc.).
- **Atajos de teclado**: Acceso rápido a las funciones más usadas.
- **Interfaz en español**: Traducción completa de los diálogos de Qt.

---

## Requisitos

- Python 3.x
- PyQt6
- Sistema operativo Linux (probado en Debian 12, MX Linux 23, antiX 23)

---

## Instalación

### 1. Instalar dependencias

Ejecuta el siguiente comando en la terminal:

```bash
sudo apt-get install python3 python3-pyqt6 python3-all-dev \
    qt6-translations-l10n fonts-noto-mono \
    python3-chardet python3-enchant
```

### 2. Ejecutar el programa

Navega a la carpeta del proyecto y ejecuta:

```bash
python3 chord_autoscroll.py
```

También puedes usar el lanzador incluido:

```bash
./Launcher.sh
```

---

## Uso

### Abrir canciones

Existen dos maneras de cargar archivos de texto con acordes:

- **Arrastrar y soltar**: Arrastra un archivo `.txt` hacia la ventana del programa.
- **Desde el menú**: Haz clic en **Archivo > Abrir** y selecciona el archivo.

### Transponer acordes

Haz clic en el botón **"Transponer"** (esquina inferior derecha) para abrir un menú de semitonos. Selecciona un valor entre -7 y +7 para transponer todos los acordes de la canción actual.

Puedes cambiar entre sostenidos (#) y bemoles (b) desde **Herramientas > Usar Sostenidos**.

### Control de auto-scroll

- Haz clic en **"Iniciar"** para comenzar el desplazamiento automático.
- Haz clic en **"Detener"** para pausarlo.
- Ajusta la velocidad con el deslizador inferior.
- Puedes cambiar la velocidad máxima desde **Herramientas > Cambiar Velocidad Máxima**.

### Buscar y reemplazar

Presiona **Ctrl+F** o ve a **Editar > Buscar y Reemplazar** para abrir el diálogo de búsqueda. Puedes activar la coincidencia exacta para distinguir entre mayúsculas y minúsculas.

### Cambiar fuente

Ve a **Formato > Fuente** para seleccionar la familia y tamaño de fuente.

### Cambiar idioma del corrector

Ve a **Herramientas > Idioma del Corrector** para alternar entre español e inglés.

---

## Atajos de teclado

| Atajo | Acción |
|-------|--------|
| `Ctrl+O` | Abrir archivo |
| `Ctrl+S` | Guardar archivo |
| `Ctrl+F` | Buscar y reemplazar |
| `Ctrl++` | Aumentar tamaño de fuente |
| `Ctrl+-` | Disminuir tamaño de fuente |

---

## Archivos de ejemplo

La carpeta `Ejemplos/` incluye canciones de muestra en formato texto con acordes:

- A quien iré - Luis Enrique Espinosa (C)
- A quien iré - Luis Enrique Espinosa (D)
- Canta al Señor - Vertical (C#)
- De tal manera - Abel Zabala (A#)
- El Espíritu de Dios - Hector Pinilla (E)
- La niña de tus ojos - Daniel Calveti (A)
- La niña de tus ojos - Daniel Calveti (C)
- No hay lugar más alto - Miel San Marcos (A)
- Renuévame - Marcos Witt (C)
- Renuévame - Marcos Witt (D)
- Sumergeme - Jesús A.R (A#)

---

## Formato de acordes soportado

El programa reconoce acordes en el siguiente formato:

```
C, Cm, Cmaj7, Cdim, Caug, Csus4, Cadd9
C#, Db, D, Dm, D7, Dsus2
E, Em, E7, Emaj7
F, Fm, F#m, Gb
G, G7, Gsus4, G/B
A, Am, A7, A#m, Bb
B, Bm, B7
```

Los acordes deben estar en líneas donde la mayoría de las palabras sean acordes para que la transposición funcione correctamente.

---

## Configuración

La configuración se guarda automáticamente en un archivo JSON (`chord_autoscroll.json`) e incluye:

- Familia y tamaño de fuente
- Velocidad de desplazamiento y posición del deslizador
- Velocidad máxima de desplazamiento
- Preferencia de sostenidos/bemoles
- Última ruta de archivo abierto

---

## Dependencias

| Paquete | Descripción |
|---------|-------------|
| `python3-pyqt6` | Framework gráfico para la interfaz de usuario |
| `python3-chardet` | Detección automática de codificación de archivos |
| `python3-enchant` | Verificación ortográfica (corrector) |
| `fonts-noto-mono` | Fuente monoespaciada para visualización de acordes |
| `qt6-translations-l10n` | Archivos de traducción de Qt para la interfaz en español |
| `python3-all-dev` | Archivos de desarrollo necesarios para compilar `enchant` |

---

## Detalle técnico: Uso de dependencias

A continuación se explica cómo se utiliza cada paquete en el código:

### 1. PyQt6 (`python3-pyqt6`)

Es el framework principal que provee toda la interfaz gráfica. Se usa en tres módulos:

**QtGui - Componentes de interfaz:**
```python
from PyQt6.QtGui import (QFont, QAction, QActionGroup, QTextCursor,
                          QShortcut, QKeySequence, QTextCharFormat, QColor,
                          QSyntaxHighlighter, QRegularExpression)
```

**QtWidgets - Ventanas y controles:**
```python
from PyQt6.QtWidgets import (QApplication, QMainWindow, QTextEdit, QVBoxLayout,
                              QHBoxLayout, QWidget, QPushButton, QLabel, QSlider,
                              QFileDialog, QMenu, QMessageBox, QInputDialog,
                              QTabWidget, QDialog, QLineEdit, QCheckBox, QGridLayout)
```

**QtCore - Funcionalidades principales:**
```python
from PyQt6.QtCore import Qt, QTimer, QTranslator, QLocale, QLibraryInfo
```

**Ejemplo de uso - Timer para auto-scroll:**
```python
# Inicia un temporizador que llama a scroll_text cada cierto intervalo
self.scroll_timer = QTimer()
self.scroll_timer.timeout.connect(self.scroll_text)
self.scroll_timer.start(self.scroll_speed)
```

**Ejemplo de uso - Atajos de teclado:**
```python
# Crea un atajo de teclado para buscar (Ctrl+F)
find_shortcut = QShortcut(QKeySequence("Ctrl+F"), self)
find_shortcut.activated.connect(self.show_find_replace_dialog)
```

---

### 2. Chardet (`python3-chardet`)

Se utiliza para detectar automáticamente la codificación de archivos de texto al abrirlos. Esto permite manejar archivos en UTF-8, ISO-8859-1, Windows-1252, etc.

**Uso en el código (línea 749):**
```python
def open_dropped_file(self, file_path):
    if os.path.exists(file_path) and file_path.lower().endswith('.txt'):
        with open(file_path, 'rb') as file:
            raw_data = file.read()
            # Detecta la codificación del archivo
            detected = chardet.detect(raw_data)
            encoding = detected['encoding'] or 'utf-8'
```

**¿Por qué es necesario?** Sin `chardet`, al abrir un archivo con codificación diferente a UTF-8 (por ejemplo, un archivo creado en Windows con Windows-1252), el texto podría mostrar caracteres incorrectos (como � en lugar de acentos).

---

### 3. Enchant (`python3-enchant`)

Se utiliza para la verificación ortográfica en tiempo real. Resalta palabras mal escritas con un subrayado rojo ondulado.

**Uso en el código (líneas 31-63):**
```python
import enchant

class SpellChecker(QSyntaxHighlighter):
    def __init__(self, parent=None):
        super().__init__(parent)
        self.spell_dict = None
        self.current_language = 'es'
        self.load_dictionary()

    def load_dictionary(self):
        try:
            # Carga el diccionario español
            self.spell_dict = enchant.Dict(self.current_language)
        except enchant.errors.DictNotFoundError:
            # Si no encuentra español, intenta con inglés
            try:
                self.spell_dict = enchant.Dict("en_US")
                self.current_language = 'en_US'
            except:
                self.spell_dict = None

    def highlightBlock(self, text):
        if not self.spell_dict:
            return
        # Patrón para encontrar palabras (solo letras)
        word_pattern = QRegularExpression(r'\b[a-zA-ZáéíóúüñÁÉÍÓÚÜÑ]+\b')
        iterator = word_pattern.globalMatch(text)

        while iterator.hasNext():
            match = iterator.next()
            word = match.captured(0)
            # Si la palabra no está en el diccionario, la resalta
            if not self.spell_dict.check(word):
                self.setFormat(match.capturedStart(), match.capturedLength(),
                               self.misspelled_format)
```

**¿Por qué es necesario?** `enchant` es un backend de verificación ortográfica que soporta múltiples diccionarios. Sin él, no se podría ofrecer la función de resaltar errores ortográficos en las letras de las canciones.

---

### 4. Qt Translations (`qt6-translations-l10n`)

Proporciona los archivos de traducción de Qt al español. No se importa directamente en Python, sino que `QTranslator` busca los archivos `.qm` en las rutas del sistema.

**Uso en el código (líneas 311-320):**
```python
def __init__(self):
    super().__init__()
    self.translator = QTranslator()
    # Obtiene la ruta donde Qt guarda las traducciones
    translations_path = QLibraryInfo.path(QLibraryInfo.LibraryPath.TranslationsPath)

    # Carga la traducción al español de Qt
    if self.translator.load("qtbase_es", translations_path):
        QApplication.installTranslator(self.translator)
```

**¿Por qué es necesario?** Sin este paquete, los diálogos nativos de Qt (como "Abrir archivo", "Guardar como", botones "Aceptar/Cancelar") aparecerían en inglés. Con el paquete instalado, estos elementos se muestran automáticamente en español.

---

### 5. Noto Mono (`fonts-noto-mono`)

Es la fuente predeterminada para mostrar el texto. No se importa en Python, sino que se especifica por nombre en la configuración.

**Uso en el código (línea 1038):**
```python
def load_config(self):
    if os.path.exists(self.config_file):
        with open(self.config_file, 'r') as f:
            self.config = json.load(f)
    else:
        self.config = {
            'max_speed': 100,
            'font_family': 'Noto Mono',  # Fuente predeterminada
            'font_size': 10,
            'last_opened_path': '',
            'use_sharps': True
        }
```

**¿Por qué es necesario?** Una fuente monoespaciada es esencial para alinear correctamente los acordes con las letras de las canciones. Los acordes deben quedar exactamente encima de la sílaba donde se tocan, y esto solo es posible con fuentes de ancho fijo.

---

### 6. Python All Dev (`python3-all-dev`)

Es un paquete de desarrollo que proporciona las cabeceras de Python necesarias para compilar extensiones en C/C++. No se usa directamente en el código, pero es un requisito de compilación para `python3-enchant`.

**¿Por qué es necesario?** El paquete `python3-enchant` tiene dependencias que requieren compilación en C. Sin `python3-all-dev`, la instalación de `enchant` fallaría con errores de compilación.

---

## Licencia

Este proyecto está licenciado bajo la licencia GPL-3.0. Consulta el archivo [LICENSE](LICENSE) para más detalles.

---

Que Dios les bendiga.
