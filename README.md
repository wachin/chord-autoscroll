# Chord Autoscroll

**A text editor with auto-scroll and chord transposition for guitarists.**

> **¿Hablas español?** También está disponible de una versión en español de este documento: [README_ES.md](README_ES.md)

Chord Autoscroll is an application written in Python and PyQt6 designed for guitarists and musicians who need to manage song files in text format (.txt) with lyrics and chords. It allows reading songs with automatic scrolling and instantly transposing chords to adapt them to the singer's voice or the instrument's tuning.

---

## Features

- **Adjustable auto-scroll**: Automatic text scrolling with configurable speed in real time.
- **Chord transposition**: Transposes musical chords from -7 to +7 semitones, with option to use sharps or flats.
- **Multiple tabs**: Open and edit several songs simultaneously.
- **Spell checking**: Highlights misspelled words with red wavy underline (supports Spanish and English).
- **Find and replace**: Text search with exact match option.
- **Drag and drop**: Open .txt files by dragging them directly into the window.
- **Persistent configuration**: Automatically saves font, speed, accidentals preference, and last used path.
- **Encoding detection**: Automatically identifies file encoding (UTF-8, ISO-8859-1, Windows-1252, etc.).
- **Keyboard shortcuts**: Quick access to the most used functions.
- **Spanish interface**: Complete translation of Qt dialogs.

---

## Requirements

- Python 3.x
- PyQt6
- Linux operating system (tested on Debian 12, MX Linux 23, antiX 23)

---

## Installation

### 1. Install dependencies

Run the following command in the terminal:

```bash
sudo apt-get install python3 python3-pyqt6 python3-all-dev \
    qt6-translations-l10n fonts-noto-mono \
    python3-chardet python3-enchant
```

### 2. Run the program

Navigate to the project folder and run:

```bash
python3 chord_autoscroll.py
```

You can also use the included launcher:

```bash
./Launcher.sh
```

---

## Usage

### Opening songs

There are two ways to load text files with chords:

- **Drag and drop**: Drag a `.txt` file into the program window.
- **From the menu**: Click **File > Open** and select the file.

### Transposing chords

Click the **"Transpose"** button (bottom right corner) to open a semitone menu. Select a value between -7 and +7 to transpose all chords in the current song.

You can switch between sharps (#) and flats (b) from **Tools > Use Sharps**.

### Auto-scroll control

- Click **"Start"** to begin automatic scrolling.
- Click **"Stop"** to pause it.
- Adjust the speed with the bottom slider.
- You can change the maximum speed from **Tools > Change Maximum Speed**.

### Find and replace

Press **Ctrl+F** or go to **Edit > Find and Replace** to open the search dialog. You can enable exact match to distinguish between uppercase and lowercase.

### Change font

Go to **Format > Font** to select the font family and size.

### Change checker language

Go to **Tools > Checker Language** to switch between Spanish and English.

---

## Keyboard shortcuts

| Shortcut | Action |
|----------|--------|
| `Ctrl+O` | Open file |
| `Ctrl+S` | Save file |
| `Ctrl+F` | Find and replace |
| `Ctrl++` | Increase font size |
| `Ctrl+-` | Decrease font size |

---

## Example files

The `Ejemplos/` folder includes sample songs in text format with chords:

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

## Supported chord format

The program recognizes chords in the following format:

```
C, Cm, Cmaj7, Cdim, Caug, Csus4, Cadd9
C#, Db, D, Dm, D7, Dsus2
E, Em, E7, Emaj7
F, Fm, F#m, Gb
G, G7, Gsus4, G/B
A, Am, A7, A#m, Bb
B, Bm, B7
```

Chords must be on lines where most words are chords for transposition to work correctly.

---

## Configuration

Configuration is automatically saved in a JSON file (`chord_autoscroll.json`) and includes:

- Font family and size
- Scroll speed and slider position
- Maximum scroll speed
- Sharps/flats preference
- Last opened file path

---

## Dependencies

| Package | Description |
|---------|-------------|
| `python3-pyqt6` | Graphical framework for the user interface |
| `python3-chardet` | Automatic file encoding detection |
| `python3-enchant` | Spell checking (checker) |
| `fonts-noto-mono` | Monospaced font for chord display |
| `qt6-translations-l10n` | Qt translation files for Spanish interface |
| `python3-all-dev` | Development files needed to compile `enchant` |

---

## Technical detail: Usage of dependencies

Below is an explanation of how each package is used in the code:

### 1. PyQt6 (`python3-pyqt6`)

It is the main framework that provides the entire graphical interface. It is used in three modules:

**QtGui - Interface components:**
```python
from PyQt6.QtGui import (QFont, QAction, QActionGroup, QTextCursor,
                          QShortcut, QKeySequence, QTextCharFormat, QColor,
                          QSyntaxHighlighter, QRegularExpression)
```

**QtWidgets - Windows and controls:**
```python
from PyQt6.QtWidgets import (QApplication, QMainWindow, QTextEdit, QVBoxLayout,
                              QHBoxLayout, QWidget, QPushButton, QLabel, QSlider,
                              QFileDialog, QMenu, QMessageBox, QInputDialog,
                              QTabWidget, QDialog, QLineEdit, QCheckBox, QGridLayout)
```

**QtCore - Core functionalities:**
```python
from PyQt6.QtCore import Qt, QTimer, QTranslator, QLocale, QLibraryInfo
```

**Usage example - Timer for auto-scroll:**
```python
# Starts a timer that calls scroll_text at a given interval
self.scroll_timer = QTimer()
self.scroll_timer.timeout.connect(self.scroll_text)
self.scroll_timer.start(self.scroll_speed)
```

**Usage example - Keyboard shortcuts:**
```python
# Creates a keyboard shortcut for search (Ctrl+F)
find_shortcut = QShortcut(QKeySequence("Ctrl+F"), self)
find_shortcut.activated.connect(self.show_find_replace_dialog)
```

---

### 2. Chardet (`python3-chardet`)

It is used to automatically detect the encoding of text files when opening them. This allows handling files in UTF-8, ISO-8859-1, Windows-1252, etc.

**Usage in code (line 749):**
```python
def open_dropped_file(self, file_path):
    if os.path.exists(file_path) and file_path.lower().endswith('.txt'):
        with open(file_path, 'rb') as file:
            raw_data = file.read()
            # Detects file encoding
            detected = chardet.detect(raw_data)
            encoding = detected['encoding'] or 'utf-8'
```

**Why is it necessary?** Without `chardet`, opening a file with encoding different from UTF-8 (for example, a file created in Windows with Windows-1252), the text could display incorrect characters (like � instead of accents).

---

### 3. Enchant (`python3-enchant`)

It is used for real-time spell checking. Highlights misspelled words with a red wavy underline.

**Usage in code (lines 31-63):**
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
            # Loads Spanish dictionary
            self.spell_dict = enchant.Dict(self.current_language)
        except enchant.errors.DictNotFoundError:
            # If Spanish not found, tries English
            try:
                self.spell_dict = enchant.Dict("en_US")
                self.current_language = 'en_US'
            except:
                self.spell_dict = None

    def highlightBlock(self, text):
        if not self.spell_dict:
            return
        # Pattern to find words (letters only)
        word_pattern = QRegularExpression(r'\b[a-zA-ZáéíóúüñÁÉÍÓÚÜÑ]+\b')
        iterator = word_pattern.globalMatch(text)

        while iterator.hasNext():
            match = iterator.next()
            word = match.captured(0)
            # If word is not in dictionary, highlight it
            if not self.spell_dict.check(word):
                self.setFormat(match.capturedStart(), match.capturedLength(),
                               self.misspelled_format)
```

**Why is it necessary?** `enchant` is a spell checking backend that supports multiple dictionaries. Without it, the function to highlight spelling errors in song lyrics could not be offered.

---

### 4. Qt Translations (`qt6-translations-l10n`)

Provides Qt translation files to Spanish. It is not directly imported in Python, but `QTranslator` looks for `.qm` files in system paths.

**Usage in code (lines 311-320):**
```python
def __init__(self):
    super().__init__()
    self.translator = QTranslator()
    # Gets the path where Qt stores translations
    translations_path = QLibraryInfo.path(QLibraryInfo.LibraryPath.TranslationsPath)

    # Loads Qt Spanish translation
    if self.translator.load("qtbase_es", translations_path):
        QApplication.installTranslator(self.translator)
```

**Why is it necessary?** Without this package, native Qt dialogs (like "Open file", "Save as", "OK/Cancel" buttons) would appear in English. With the package installed, these elements are automatically displayed in Spanish.

---

### 5. Noto Mono (`fonts-noto-mono`)

It is the default font for displaying text. It is not imported in Python, but specified by name in the configuration.

**Usage in code (line 1038):**
```python
def load_config(self):
    if os.path.exists(self.config_file):
        with open(self.config_file, 'r') as f:
            self.config = json.load(f)
    else:
        self.config = {
            'max_speed': 100,
            'font_family': 'Noto Mono',  # Default font
            'font_size': 10,
            'last_opened_path': '',
            'use_sharps': True
        }
```

**Why is it necessary?** A monospaced font is essential to correctly align chords with song lyrics. Chords must be placed exactly above the syllable where they are played, and this is only possible with fixed-width fonts.

---

### 6. Python All Dev (`python3-all-dev`)

It is a development package that provides Python headers needed to compile C/C++ extensions. It is not used directly in code, but is a compilation requirement for `python3-enchant`.

**Why is it necessary?** The `python3-enchant` package has dependencies that require C compilation. Without `python3-all-dev`, the installation of `enchant` would fail with compilation errors.

---

## License

This project is licensed under the GPL-3.0 license. See the [LICENSE](LICENSE) file for more details.

---

God bless you.
