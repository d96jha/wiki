When dealing with ELF (Executable and Linkable Format) binaries that include **DWARF (Debugging With Attributed Record Formats)** debug information, there's a lot going on behind the scenes.

Let’s break down the **main parts of DWARF debug information** inside an ELF file and what each part does.

---

## 🧩 Overview: ELF + DWARF

* **ELF** is the container format (used in Linux).
* **DWARF** is the format used inside the ELF to store debug information.

When you compile with `-g` (e.g., `gcc -g`), DWARF sections are added to the ELF binary to enable debugging.

---

## 🧱 Key DWARF Sections Inside an ELF File

| Section Name                          | Description                                                               |
| ------------------------------------- | ------------------------------------------------------------------------- |
| `.debug_info`                         | The **core debug info**: functions, variables, types, line mappings.      |
| `.debug_abbrev`                       | Defines **abbreviations** used in `.debug_info`. Compresses metadata.     |
| `.debug_line`                         | Maps **machine code to source lines** (line tables).                      |
| `.debug_str`                          | Contains **debug strings** (names, paths, etc.) used in `.debug_info`.    |
| `.debug_loc` / `.debug_loclists`      | Location lists – describe **where variables live** during execution.      |
| `.debug_ranges` / `.debug_rnglists`   | Describe address ranges where code is **valid** for a symbol or function. |
| `.debug_pubnames` / `.debug_pubtypes` | Indexes for **public** symbols (deprecated in DWARF5).                    |
| `.debug_aranges`                      | Maps **addresses to compilation units**.                                  |
| `.debug_frame` / `.eh_frame`          | **Call frame information** for stack unwinding (used in backtraces).      |
| `.debug_macinfo` / `.debug_macro`     | Information about **preprocessor macros**.                                |
| `.debug_types`                        | Type information split into separate units (used in split DWARF).         |
| `.debug_cu_index`, `.debug_tu_index`  | Indexes used in **split DWARF** (DWZ or Fission).                         |
| `.gdb_index`                          | GDB-specific index for faster symbol lookup.                              |

---

## 🎯 Key Concepts in DWARF

* **Compilation Unit (CU):**
  Each `.c` or `.cpp` file compiled contributes a **CU**, stored in `.debug_info`.

* **Debugging Information Entry (DIE):**
  Tree-like structures that represent **functions, variables, structs**, etc.

* **Attributes and Abbreviations:**
  Each DIE has **attributes** like name, type, line number. `.debug_abbrev` defines how to interpret these attributes.

---

## 🔍 Viewing DWARF Sections

### Use `readelf`:

```bash
readelf --debug-dump=info your_binary
```

Or to list all debug sections:

```bash
readelf -S your_binary | grep debug
```

### Use `objdump`:

```bash
objdump --dwarf=info your_binary
```

### Use `dwarfdump` (if installed):

```bash
dwarfdump your_binary
```

---

## 🧪 Example: What Happens When You Compile with `-g`

Given this code:

```c
int main() {
    int x = 42;
    return x;
}
```

And compile:

```bash
gcc -g -o test test.c
```

DWARF will store:

* In `.debug_info`: A DIE for `main`, and a nested DIE for `x`.
* In `.debug_line`: Info saying `int x = 42;` corresponds to address `0x00401234`.
* In `.debug_loc`: Describes where `x` lives in memory or register at different program points.

---

## 🧠 Bonus: `strip` and `objcopy`

You can remove or extract debug info:

* Strip all debug info:

  ```bash
  strip --strip-debug binary
  ```

* Extract debug info to separate file:

  ```bash
  objcopy --only-keep-debug binary binary.debug
  objcopy --strip-debug binary
  objcopy --add-gnu-debuglink=binary.debug binary
  ```

This enables **separate debug symbols** for packaging or symbol servers.

---

## 📚 Summary Table of Common DWARF Sections

| Section Name    | Purpose                                          |
| --------------- | ------------------------------------------------ |
| `.debug_info`   | Core tree of debug info (functions, vars, types) |
| `.debug_abbrev` | Compact schema/format for `.debug_info`          |
| `.debug_line`   | Source line to address mapping                   |
| `.debug_str`    | Strings used by debug info                       |
| `.debug_loc`    | Variable locations (memory/register)             |
| `.debug_ranges` | Code ranges for scopes or inlined functions      |
| `.debug_frame`  | Stack unwinding info                             |
| `.debug_macro`  | Macro definitions and uses                       |
| `.gdb_index`    | Fast symbol index for GDB                        |

---

> **My note**:
>
> DWARF more complicated than ELF:
> * a better description can of course be found in
    [wikipedia](https://en.wikipedia.org/wiki/DWARF)
> * see also the [DWARF Version 5 Standard](https://dwarfstd.org/doc/DWARF5.pdf)
    and the [website](http://dwarfstd.org)

---
