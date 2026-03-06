# Multi-Language Development

Xmake supports building projects in various programming languages beyond C/C++.

## CUDA

### Basic CUDA Setup

```lua
add_requires("cuda")

target("cuda_app")
    set_kind("binary")
    add_files("src/*.cu", "src/*.cpp")
    add_packages("cuda")
```

### CUDA Architecture

```lua
target("cuda_app")
    add_cugtids("7.0")  -- GPU architecture
    add_cupts("7.0")
```

## Rust

### Rust Integration

```lua
add_requires("rust")

target("rust_app")
    set_kind("binary")
    add_rustfiles("src/*.rs")
    add_packages("rust")
```

## Swift

### Swift on Apple Platforms

```lua
target("swift_app")
    set_kind("binary")
    add_swiftfiles("src/*.swift")
    add_frameworks("Foundation", "AppKit")
```

## Go

### Go Integration

```lua
target("go_app")
    set_kind("binary")
    add_gofiles("src/*.go")
```

## D Language

### D Language Support

```lua
add_requires("dlang")

target("d_app")
    set_kind("binary")
    add_dfiles("src/*.d")
```

## Fortran

### Fortran Support

```lua
add_requires("fortran")

target("fortran_app")
    set_kind("binary")
    add_fortranfiles("src/*.f90")
```

## Objective-C

### Objective-C on Apple

```lua
target("objc_app")
    set_kind("binary")
    add_objcfiles("src/*.m")
    add_frameworks("Foundation", "AppKit")
```

## Nim

### Nim Support

```lua
add_requires("nim")

target("nim_app")
    set_kind("binary")
    add_nimfiles("src/*.nim")
```

## Zig

### Zig Integration

```lua
add_requires("zig")

target("zig_app")
    set_kind("binary")
    add_zigfiles("src/*.zig")
```

## Vala

### Vala Support

```lua
add_requires("vala")

target("vala_app")
    set_kind("binary")
    add_valafiles("src/*.vala")
```

## Pascal

### FreePascal

```lua
add_requires("fpc")

target("pascal_app")
    set_kind("binary")
    add_pascalfiles("src/*.pas")
```

## Lex/Yacc

### Lex and Yacc Files

```lua
target("parser")
    set_kind("binary")
    add_lexfiles("src/*.l")
    add_yaccfiles("src/*.y")
```

## ASN.1

### ASN.1 Encoding

```lua
add_requires("asn1c")

target("asn1_app("binary")
   ")
    set_kind add_asn1files("src/*.asn1")
```

## Protocol Buffers

### Protobuf Support

```lua
add_requires("protobuf")

target("proto_app")
    set_kind("binary")
    add_protobuffiles("src/*.proto")
    add_packages("protobuf")
```

## OpenMP

### OpenMP Support

```lua
target("openmp_app")
    set_kind("binary")
    add_files("src/*.cpp")
    add_cxflags("-fopenmp")
    add_ldflags("-fopenmp")
```

## Cosmocc

### Cosmocc Compiler

```lua
toolchain("cosmocc")
    set_kind("toolchain")
    set_sdkdir("/path/to/cosmocc")
    
target("cosmos_app")
    set_kind("binary")
    add_files("src/*.c")
    add_toolchains("cosmocc")
```

## Best Practices

1. Use appropriate toolchain for each language
2. Handle linking carefully cross-language
3. Use package managers for language-specific dependencies
4. Consider language runtime requirements
