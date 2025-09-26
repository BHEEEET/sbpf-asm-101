Got it — here’s your notes reorganized with a **table of contents** and clean sectioning:

---

# Assembly 101

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

```bash
cargo install --git https://github.com/blueshift-gg/sbpf.git
```

---

## Logging

Enable logging:

```bash
RUST_LOG=debug sbpf test
```

Disable logging:

```bash
RUST_LOG=off sbpf test
```

---

## Debugging

### Disassemble entire `.so` file

```bash
readelf -W -a deploy/hello_world.so
```

### Dump raw data of all sections

```bash
objdump -s deploy/hello_world.so | less
```

### Disassemble `.text` section

```bash
llvm-objdump -d --arch=bpf deploy/hello_world.so
```

### Read raw section data

```bash
readelf -x .text deploy/hello_world.so
readelf -x .rodata deploy/hello_world.so
```

### Read section headers with addresses

```bash
readelf -W -S deploy/hello_world.so
```

### Read section data with llvm-objdump

```bash
llvm-objdump -s --section=.rodata --arch=bpf deploy/hello_world.so
```

---

## Hex Editing

### View hex data

```bash
xxd hello_world.so
```

### Dump hex data to a file

```bash
xxd hello_world.so > dump.hex
```

Edit `dump.hex` in any hex editor to modify bytes.

### Convert back to binary

```bash
xxd -r dump.hex > hello_world_patched.so
```