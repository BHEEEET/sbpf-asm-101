# Assembly 101

This document provides a collection of commands and techniques for working with Solana BPF (SBPF) shared object (`.so`) programs.
It covers installation, logging, debugging, disassembly, and basic hex editing. The goal is to give a practical reference for analyzing and modifying compiled Solana programs during development and testing.

---

## Table of Contents

1. [Installation](#installation)
2. [Logging](#logging)
3. [Debugging](#debugging)

   * [Disassemble entire `.so` file](#disassemble-entire-so-file)
   * [Dump raw data of all sections](#dump-raw-data-of-all-sections)
   * [Disassemble `.text` section](#disassemble-text-section)
   * [Read raw section data](#read-raw-section-data)
   * [Read section headers with addresses](#read-section-headers-with-addresses)
   * [Read section data with llvm-objdump](#read-section-data-with-llvm-objdump)
4. [Hex Editing](#hex-editing)

---

## Installation

Install the Solana BPF tooling from source:

```bash
cargo install --git https://github.com/blueshift-gg/sbpf.git
```

---

## Logging

Enable program logging with:

```bash
RUST_LOG=debug sbpf test
```

Disable logging with:

```bash
RUST_LOG=off sbpf test
```

---

## Debugging

### Disassemble entire `.so` file

Dump ELF headers, sections, and program information:

```bash
readelf -W -a deploy/hello_world.so
```

### Dump raw data of all sections

View the raw byte contents of each section:

```bash
objdump -s deploy/hello_world.so | less
```

### Disassemble `.text` section

Disassemble executable instructions in the `.text` section for the BPF target:

```bash
llvm-objdump -d --arch=bpf deploy/hello_world.so
```

### Read raw section data

Inspect specific section data in hexadecimal form:

```bash
readelf -x .text deploy/hello_world.so
readelf -x .rodata deploy/hello_world.so
```

### Read section headers with addresses

Display section headers, offsets, and virtual memory addresses:

```bash
readelf -W -S deploy/hello_world.so
```

### Read section data with llvm-objdump

Dump section contents using LLVM’s objdump:

```bash
llvm-objdump -s --section=.rodata --arch=bpf deploy/hello_world.so
```

---

## Hex Editing

### View hex data

Display the binary file in hexadecimal:

```bash
xxd hello_world.so
```

### Dump hex data to a file

Export a hex dump to an editable file:

```bash
xxd hello_world.so > dump.hex
```

Edit `dump.hex` in a text or hex editor to modify specific bytes.

### Convert back to binary

Reconstruct the modified binary from the edited hex dump:

```bash
xxd -r dump.hex > hello_world_patched.so
```

### ezBPF CLI
Easy sBPF disassembler:

Install:
```
cargo install --git https://github.com/deanmlittle/ezbpf
```

Disassemble command:
```bash
ezbpf hello_world.so

# or dump to json

ezbpf hello_world.so > dump.json
```
