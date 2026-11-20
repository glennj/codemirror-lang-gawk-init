# CodeMirror 6 GNU AWK (gawk) Support

`codemirror-lang-gawk` is a CodeMirror 6 language plugin providing full syntax highlighting, autocompletion, diagnostic linting, and custom themes for both standard **POSIX AWK** and **GNU AWK (gawk)**.

---

## 🚀 How to Run the Demo

### Prerequisites
* [Node.js](https://nodejs.org/) (v16+ recommended)
* `npm`

### Running the Demo Locally
1. **Install Dependencies**:
   ```bash
   npm install
   ```

2. **Start Development Server**:
   ```bash
   npm run dev
   ```
   Open your browser to `http://localhost:3000/` (or the URL printed in the terminal) to launch the interactive live editor demo.

3. **Build Library for Production**:
   ```bash
   npm run build
   ```

4. **Preview Production Build**:
   ```bash
   npm run preview
   ```

---

## ✨ CodeMirror 6 Features Implemented

* **Stream Parser Engine (`gawkLanguage` / `gawkStreamParser`)**:
  Custom tokenizer built for CodeMirror 6 supporting standard AWK and GNU AWK extensions (including directives, special pattern blocks, regex literals, field references, and hex/octal numbers).

* **Syntax Highlighting & Bundled Themes (`gawkHighlighting`, `gawkDarkTheme`, `gawkLightTheme`)**:
  Token tags mapped to `@lezer/highlight` with custom dark and light editor themes tailored for AWK development.

* **Context-Aware Autocompletion (`gawkCompletionSource`)**:
  * **GNU Directives**: `@include`, `@load`, `@namespace`.
  * **Built-in Functions**: Comprehensive completion list with function signatures, types, and detailed documentation tooltips (e.g., `gensub`, `patsplit`, `strftime`, `typeof`, `asort`).
  * **Built-in Variables**: Standard and GNU-specific special variables (`FS`, `OFS`, `NR`, `NF`, `ARGIND`, `FPAT`, `ERRNO`, `SYMTAB`, etc.).
  * **Snippets**: Quick insertions for `BEGIN`, `END`, `function`, `for (in)`, `for (i=1..n)`, `@include`, `@namespace`, and `@load`.
  * **Document Symbol Scanner**: Dynamically collects user-defined function names and variable identifiers from the active document.

* **Live Diagnostic Linter (`gawkLinter`)**:
  Real-time syntax checking highlighting code issues directly in the editor:
  * Unterminated string literals (`"text...`)
  * Unclosed regular expression literals (`/pattern...`)
  * Unmatched braces (`{}`) and parentheses (`()`)
  * Unknown `@` directives or missing directive file/namespace strings

---

## 🛠️ AWK and GNU AWK (gawk) Features Implemented

### 1. Standard POSIX AWK Core
* **Pattern Blocks**: `BEGIN`, `END`
* **Control Keywords**: `if`, `else`, `while`, `do`, `for`, `in`, `break`, `continue`, `next`, `exit`, `return`, `delete`, `function`, `func`
* **Built-in Variables**: `ARGC`, `ARGV`, `CONVFMT`, `ENVIRON`, `FILENAME`, `FNR`, `FS`, `NF`, `NR`, `OFMT`, `OFS`, `ORS`, `RLENGTH`, `RS`, `RSTART`, `SUBSEP`
* **Built-in Functions**:
  * *Math*: `atan2`, `cos`, `exp`, `int`, `log`, `rand`, `sin`, `sqrt`, `srand`
  * *String*: `sub`, `gsub`, `index`, `length`, `match`, `split`, `sprintf`, `substr`, `tolower`, `toupper`
  * *I/O*: `print`, `printf`, `getline`, `close`, `fflush`, `system`
* **Field References**: `$0`, `$1`, `$2`, ..., `$NF`
* **Regular Expressions**: `/pattern/` literals and binary regex match operators (`~`, `!~`)

### 2. GNU AWK (gawk) Specific Extensions
* **Directives**:
  * `@include "filename.awk"` — modular code inclusion
  * `@load "extension"` — dynamic C extension module loading
  * `@namespace "name"` — symbol scoping and namespace isolation
* **Pattern Blocks**:
  * `BEGINFILE` — executed before reading each new input file
  * `ENDFILE` — executed after completing each input file
* **Built-in Variables**:
  * `ARGIND` — index of current file in `ARGV`
  * `ERRNO` — detailed system error string for failed I/O operations
  * `FPAT` — regular expression defining field matching patterns (useful for CSV parsing)
  * `FIELDWIDTHS` — space-separated column widths for fixed-width data
  * `RT` — actual input text matching the record separator `RS`
  * `SYMTAB` — global symbol table array mapping variable names to values
  * `FUNCTAB` — array of all defined functions
  * `PROCINFO` — process information and platform metadata array
  * `BINMODE`, `LINT`, `PREC`, `ROUNDMODE`, `STRTAB`, `TEXTDOMAIN`
* **Built-in Functions**:
  * *String & Array Extensions*: `gensub`, `patsplit`, `asort`, `asorti`, `strtonum`
  * *Bitwise Operations*: `and`, `or`, `xor`, `compl`, `lshift`, `rshift`
  * *Time Functions*: `systime`, `strftime`, `mktime`
  * *Type Checking*: `typeof`, `isarray`
  * *Internationalization (i18n)*: `bindtextdomain`, `dcgettext`, `dcngettext`
* **Numeric Formats**: Hexadecimal (`0x1F`) and octal (`0755`) numeric constants.

---

## 📦 Usage & Code Example

```typescript
import { EditorView } from '@codemirror/view';
import { basicSetup } from 'codemirror';
import { gawk, gawkDarkTheme } from 'codemirror-lang-gawk';

const editor = new EditorView({
  doc: `@include "helpers.awk"
@namespace "analytics"

BEGIN {
    FS = ","
    print "Processing environment:", ENVIRON["USER"]
}

BEGINFILE {
    print "Starting file #" ARGIND ":", FILENAME
}

$1 ~ /"GET|"POST/ {
    # Extract match with GNU AWK gensub
    clean_url = gensub(/"(GET|POST)\s+([^ ]+).*/, "\\2", "g", $0)
    patsplit(clean_url, tokens, /[a-zA-Z0-9_]+/)
    requests[$1]++
}

END {
    print "Analysis complete."
}`,
  extensions: [
    basicSetup,
    gawk({ lint: true, autocomplete: true }),
    gawkDarkTheme
  ],
  parent: document.getElementById('editor')!
});
```

---

## 📜 License

[MIT](LICENSE)

