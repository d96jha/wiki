An **ELF (Executable and Linkable Format)** file is the standard binary format for executables, object files, shared libraries, and core dumps on Linux and other Unix-like systems.

It is composed of several well-defined parts, each serving a specific purpose during **compilation**, **linking**, **loading**, and **execution**.

---

## 🧱 Major Parts of an ELF File

### 🗂️ 1. **ELF Header**

* The **first part** of the file.
* Describes the overall file layout and type.

Includes:

* File type (`executable`, `relocatable`, `shared`)
* Target architecture (e.g., x86\_64, ARM)
* Entry point address
* Offsets to the **Program Header Table** and **Section Header Table**

🔍 Command to view:

```bash
readelf -h yourfile
```

---

### 🗃️ 2. **Program Header Table (PHT)**

* Describes the segments used during **program loading**.
* Only present in **executable and shared objects**, not in object files (`.o`).

Each **program header** (or segment) tells the OS:

* What parts of the file to map into memory
* Whether it’s code (`.text`), data (`.data`), or dynamic linking info

🔍 View with:

```bash
readelf -l yourfile
```

---

### 📦 3. **Section Header Table (SHT)**

* Describes the file's **sections**, used by **linkers** and **debuggers**, not the OS loader.
* Present in **all ELF files**, especially relocatable ones.

Each section has:

* A name (`.text`, `.data`, `.bss`, `.symtab`, `.debug_info`, etc.)
* Type (`code`, `data`, `symbol table`, `relocation info`)
* Flags (`allocatable`, `writable`, `executable`)

🔍 View with:

```bash
readelf -S yourfile
```

---

### 📁 4. **Sections** (e.g., `.text`, `.data`, `.rodata`, etc.)

These are the actual **containers of data**. Some common ones include:

| Section        | Purpose                                      |
| -------------- | -------------------------------------------- |
| `.text`        | Compiled machine code                        |
| `.data`        | Initialized global/static variables          |
| `.bss`         | Uninitialized data (allocated at runtime)    |
| `.rodata`      | Read-only data (e.g., strings, constants)    |
| `.symtab`      | Full symbol table (link-time)                |
| `.strtab`      | String table (names for symbols)             |
| `.rela.text`   | Relocation entries for `.text`               |
| `.dynamic`     | Dynamic linking information                  |
| `.dynsym`      | Symbol table for dynamic linker              |
| `.got`, `.plt` | For dynamic symbol resolution                |
| `.debug_*`     | DWARF debugging info (if compiled with `-g`) |

---

### 🔗 5. **String Tables**

Sections that store names used in other parts of the ELF:

* `.strtab`: for symbol names
* `.shstrtab`: for section names
* `.dynstr`: for dynamic symbols

---

### 🧮 6. **Symbol Tables**

* `.symtab`: All symbols (functions, globals) used for linking.
* `.dynsym`: Dynamic symbols used at runtime (for shared libraries).

Each symbol entry includes:

* Name
* Type (function, object)
* Scope (local/global)
* Section
* Address

---

### 📐 7. **Relocation Entries**

* `.rela.*` or `.rel.*` sections describe how to fix addresses during linking or loading.
* Used in **relocatable** and **shared** objects.

---

### 🔧 8. **Dynamic Section (`.dynamic`)**

* Contains information used by the **dynamic linker**, such as:

  * Needed shared libraries
  * Symbol hashes
  * Init/fini function addresses

🔍 View with:

```bash
readelf -d yourfile
```

---

### 🧵 9. **.init / .fini Sections**

* Optional sections executed before (`.init`) and after (`.fini`) `main()`.
* Used for static initialization/destructors in C++.

---

## 🧭 ELF File Layout Summary

### In Executables or Shared Objects:

```text
[ ELF Header ]
[ Program Header Table ]
[ Segments (e.g., .text, .data, .bss) ]
[ Section Header Table ]
```

### In Relocatable Object Files (`.o`):

```text
[ ELF Header ]
[ Section Header Table ]
[ Sections (e.g., .text, .data, .symtab, .rel.text) ]
```

---

## 🔍 Tools to Explore ELF Files

| Tool       | Description                                   |
| ---------- | --------------------------------------------- |
| `readelf`  | Low-level ELF inspector                       |
| `objdump`  | Disassembler + ELF analyzer                   |
| `nm`       | Lists symbols                                 |
| `strings`  | Extracts printable strings                    |
| `file`     | Tells you if it's ELF, architecture, etc.     |
| `elfutils` | Advanced ELF/DWARF tools (`eu-readelf`, etc.) |

---

> **My note**:
>
> The elf container format is fairly simple: this is just a high level
> overview:
> * a better description can of course be found in
    [wikipedia](https://en.wikipedia.org/wiki/Executable_and_Linkable_Format)
> * see also the ELF [Specification](https://refspecs.linuxbase.org/elf/elf.pdf)

---

> **My note**:
>
> alone the Elf format is not very useful, it is interpreted in the
> context of a machine constant:
> * the [Arm ABI](https://github.com/ARM-software/abi-aa) defines two
    machine constants:
>   - `EM_ARM` (40) for 32-bit Arm
>   - `EM_AARCH64` (184) for 64-bit Arm

> **My note**:
>
> see the [llvm::ELF](https://llvm.org/doxygen/namespacellvm_1_1ELF.html)
> namespace for an example implementation.
> * it has an anonymous `enum` that defines machine constants
>   (all values has the prefix `EM_`)
