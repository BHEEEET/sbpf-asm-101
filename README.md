# Assembly 101

install:
```
cargo install --git https://github.com/blueshift-gg/sbpf.git
```

## Logging
To see logging of the programs enable it by running:
```
RUST_LOG=debug sbpf test
```

turn off:
```
RUST_LOG=off sbpf test
```

## Debugging

Dissasemble entire `.so` file:
```
readelf -a assembly101/hello_world/deploy/hello_world.so 
```

Raw data all sections
```
objdump -s assembly101/hello_world/deploy/hello_world.so | less
```

Disassamble `.text`section from the `.so`file:
```
llvm-objdump -d --arch=bpf assembly101/hello_world/deploy/hello_world.so 
```

raw section data
```
readelf -x .text   assembly101/hello_world/deploy/hello_world.so

readelf -x .rodata  deploy/hello_world.so
```

```
readelf -S assembly101/hello_world/deploy/hello_world.so
```

Reading section data using `llvm-objdump`
```
llvm-objdump -s --section=.rodata --arch=bpf assembly101/hello_world/deploy/hello_world.so
```
