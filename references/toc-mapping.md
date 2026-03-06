# Complete Table of Contents Mapping

This document maps all xmake documentation sections from the official TOC to skill references.

## Getting Started

| Section | Skill Reference |
|---------|----------------|
| Quick Start | `references/basic-commands.md` (create-project, build, run) |
| Introduction | Main SKILL.md |
| Create a project | `references/basic-commands.md` |
| How to install xmake | Main SKILL.md |

## Basic Commands

| Section | Reference |
|---------|-----------|
| Build Configuration | `references/basic-commands.md` |
| Build Targets | `references/basic-commands.md` |
| Clean Targets | `references/basic-commands.md` |
| Run targets | `references/basic-commands.md` |
| Run tests | `references/testing.md` |
| Install and Uninstall | `references/basic-commands.md` |
| Cross Compilation | `references/cross-compilation.md` |
| Switch Toolchains | `references/cross-compilation.md` |
| Pack Programs | `references/basic-commands.md` |

## Project Configuration

| Section | Reference |
|---------|-----------|
| Configure Targets | `references/project-structure.md` |
| Define options | `references/project-structure.md` |
| Syntax description | `references/project-structure.md` |
| Multi-level Directories | `references/project-structure.md` |
| Namespace Isolation | `references/project-structure.md` |
| Custom Rule | `references/rules-policies.md` |
| Custom Rules | `references/rules-policies.md` |
| Custom Toolchain | `references/cross-compilation.md` |
| Plugin and Task | `references/advanced-features.md` |
| Custom Description Scope API | `references/lua-best-practices.md` |

## Package Management

| Section | Reference |
|---------|-----------|
| Add Packages | `references/package-management.md` |
| Package Dependencies | `references/package-management.md` |
| Using Official Packages | `references/package-management.md` |
| Using Local Packages | `references/package-management.md` |
| Using System Packages | `references/package-management.md` |
| Using Source Code Packages | `references/package-management.md` |
| Package Distribution | `references/package-management.md` |
| Repository Management | `references/package-management.md` |
| Using packages in CMake | `references/external-builds.md` |
| Network Optimization | `references/package-management.md` |
| Distribute Private Libraries | `references/package-management.md` |
| Package target | `references/package-management.md` |
| Xrepo CLI | `references/package-management.md` |

## Platform Guides

| Section | Reference |
|---------|-----------|
| Android Application | `references/android-development.md` |
| iOS Application | `references/ios-development.md` (create this) |
| Mac Application | `references/macos-development.md` (create this) |
| Linux Framebuffer | `references/linux-framebuffer.md` (create this) |
| Windows Driver Kit | `references/wdk-development.md` |
| WebAssembly | `references/wasm-development.md` |

## Graphics & UI Frameworks

| Section | Reference |
|---------|-----------|
| OpenGL Program | `references/graphics.md` (create this) |
| Vulkan Program | `references/graphics.md` |
| SDL2 Program | `references/graphics.md` |
| ImGui Program | `references/graphics.md` |
| Raylib Program | `references/graphics.md` |
| Metal Application | `references/graphics.md` |
| Audio Programs | `references/graphics.md` |
| Terminal TUI Programs | `references/graphics.md` |

## Examples by Language

| Section | Reference |
|---------|-----------|
| Basic Examples | `references/examples-basic.md` |
| C++ Modules | `references/cxx-modules.md` |
| ASN.1 | `references/crypto-encryption.md` |
| CUDA | `references/cuda.md` |
| Protobuf | `references/protobuf.md` |
| OpenMP | `references/openmp.md` |
| Fortran | `references/fortran.md` |
| Rust | `references/rust.md` |
| Swift | `references/swift.md` |
| Go | `references/golang.md` |
| Zig | `references/zig.md` |
| Nim | `references/nim.md` |
| D | `references/dlang.md` |
| Vala | `references/vala.md` |
| Pascal | `references/pascal.md` |
| Lex/Yacc | `references/lex-yacc.md` |

## Embedded & Cross-Platform

| Section | Reference |
|---------|-----------|
| Linux Driver Module | `references/linux-driver.md` |
| Linux BPF | `references/linux-bpf.md` |
| Keil C51 | `references/embedded-keil.md` |
| Keil MDK | `references/embedded-keil.md` |
| Merge Static Libraries | `references/advanced-features.md` |
| Bin2c/Bin2obj | `references/advanced-features.md` |
| Unity Build | `references/advanced-features.md` |

## API Reference

### Global Interfaces

| Section | Reference |
|---------|-----------|
| Global Interfaces | `references/api-reference.md` |
| Project Targets | `references/target-apis.md` |
| Configuration Option | `references/project-structure.md` |
| Package Dependencies | `references/package-management.md` |
| Helper Interfaces | `references/api-reference.md` |

### Builtin Modules

| Section | Reference |
|---------|-----------|
| io | `references/builtin-io.md` |
| os | `references/builtin-os.md` |
| path | `references/builtin-path.md` |
| string | `references/builtin-string.md` |
| table | `references/builtin-table.md` |
| print/printf | `references/builtin-print.md` |
| format/vformat | `references/builtin-format.md` |
| import | `references/builtin-import.md` |
| coroutine | `references/builtin-coroutine.md` |
| hash | `references/builtin-hash.md` |
| raise | `references/builtin-exception.md` |
| try-catch-finally | `references/builtin-exception.md` |
| ipairs/pairs | `references/builtin-iteration.md` |
| inherit | `references/builtin-inherit.md` |

### Extension Modules

| Section | Reference |
|---------|-----------|
| core.base.* | `references/core-base.md` |
| core.tool.* | `references/core-tool.md` |
| core.project.* | `references/core-project.md` |
| core.language.* | `references/core-language.md` |
| core.cache.* | `references/core-cache.md` |
| core.ui.* | `references/core-ui.md` |
| async.jobgraph | `references/advanced-features.md` |
| async.runjobs | `references/advanced-features.md` |
| lib.detect | `references/lib-detect.md` |
| net.http | `references/net-http.md` |
| net.ping | `references/net-ping.md` |
| utils.archive | `references/utils-archive.md` |
| utils.binary | `references/utils-binary.md` |
| utils.platform | `references/utils-platform.md` |
| privilege.sudo | `references/privilege-sudo.md` |
| devel.git | `references/devel-git.md` |
| cli.amalgamate | `references/cli-amalgamate.md` |
| cli.iconv | `references/cli-iconv.md` |
| lz4 | `references/compress-lz4.md` |

### Option Instance

| Section | Reference |
|---------|-----------|
| Option Instance | `references/option-instance.md` |
| Package Instance | `references/package-instance.md` |
| Target Instance | `references/target-apis.md` |

## Builtin Rules & Policies

| Section | Reference |
|---------|-----------|
| Builtin Rules | `references/builtin-rules.md` |
| Builtin Policies | `references/builtin-policies.md` |
| Builtin Variables | `references/builtin-variables.md` |
| Specification | `references/specification.md` |
| Conditions | `references/conditions.md` |

## Advanced Features

| Section | Reference |
|---------|-----------|
| Auto-scan Source Build | `references/autoscan.md` |
| Automatic Code Generation | `references/advanced-features.md` |
| Distributed Compilation | `references/distributed-compilation.md` |
| Build Cache Acceleration | `references/build-cache.md` |
| Remote Compilation | `references/remote-compilation.md` |
| Performance Optimization | `references/performance.md` |
| Environment Variables | `references/environment-variables.md` |
| Unity Build Acceleration | `references/unity-build.md` |
| Custom Module | `references/lua-best-practices.md` |
| Native Modules | `references/native-modules.md` |

## IDE Integration

| Section | Reference |
|---------|-----------|
| IDE Integration Plugins | `references/ide-integration.md` |
| Builtin Plugins | `references/builtin-plugins.md` |
| Plugin Development | `references/plugin-development.md` |
| Theme Style | `references/theme-style.md` |

## Best Practices

| Section | Reference |
|---------|-----------|
| FAQ | `references/faq.md` |
| Configuration Optimization | `references/configuration-optimization.md` |
| AI Q&A Optimization | `references/ai-qa-optimization.md` |
| Performance | `references/performance.md` |

## Version History (Changelogs)

| Section | Reference |
|---------|-----------|
| xmake v3.0.7 | External link |
| xmake v3.0.6 | External link |
| xmake v3.0.5 | External link |
| xmake v2.9.1 | External link |
| xmake v2.8.x series | External link |
| xmake v2.7.x series | External link |
| xmake v2.6.x series | External link |
| xmake v2.5.x series | External link |

## Untitled Sections (Derived from URLs)

| URL Path | Derived Name |
|----------|--------------|
| /about/contact.md | Contact Information |
| /about/sponsor.md | Sponsors |
| /about/team.md | Team |
| /about/who_is_using_xmake.md | Who is Using Xmake |
| /examples/bindings/lua-module.md | Lua Module Binding |
| /examples/bindings/nodejs-module.md | Node.js Module Binding |
| /examples/bindings/python-module.md | Python Module Binding |
| /examples/bindings/swig.md | SWIG Bindings |
| /examples/cpp/asn1.md | ASN.1 Development |
| /examples/cpp/cppfront.md | C++Front Development |
| /examples/cpp/cosmocc.md | Cosmocc Toolchain |
| /examples/cpp/graphics/qt.md | Qt Application |
| /examples/cpp/graphics/mfc.md | MFC Application |
| /examples/cpp/graphics/winsdk.md | Windows SDK Graphics |
| /examples/cpp/linux-bpf.md | Linux BPF Programs |
| /examples/cpp/merge-static-libraries.md | Merge Static Libraries |
| /examples/cpp/openmp.md | OpenMP Support |
| /examples/cpp/linux-driver-module.md | Linux Driver Module |
| /examples/cpp/protobuf.md | Protocol Buffers |
| /examples/cpp/wasm.md | WebAssembly |
| /examples/cpp/wdk.md | Windows Driver Kit |
| /examples/embed/keil-c51.md | Keil C51 |
| /examples/embed/keil-mdk.md | Keil MDK |
| /examples/other-languages/dlang.md | D Language |
| /examples/other-languages/cuda.md | CUDA |
| /examples/other-languages/fortran.md | Fortran |
| /examples/other-languages/golang.md | Go |
| /examples/other-languages/lex-yacc.md | Lex/Yacc |
| /examples/other-languages/nim.md | Nim |
| /examples/other-languages/pascal.md | Pascal |
| /examples/other-languages/objc.md | Objective-C |
| /examples/other-languages/rust.md | Rust |
| /examples/other-languages/swift.md | Swift |
| /examples/other-languages/vala.md | Vala |
| /examples/other-languages/zig.md | Zig |
