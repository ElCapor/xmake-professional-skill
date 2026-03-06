---
name: xmake-pro
description: Expert-level skill for using the xmake build system. Use this skill whenever the user wants to build, compile, or manage C/C++ projects with xmake, including cross-compilation, Android NDK development, Windows Driver Kit (WDK) development, WebAssembly (WASM) compilation, package management, custom build rules, testing, or integrating with cmake/make/autotools projects. This skill provides comprehensive knowledge of xmake's Lua-based configuration, advanced features, and best practices for projects of any complexity.
---

# Xmake Pro Skill

This skill enables expert-level usage of xmake, a modern C/C++ build system. Use this skill for any task involving xmake configuration, compilation, or project management.

## Quick Reference

### Essential Commands

```bash
# Configuration
xmake f -p <platform>                    # Configure for platform (linux, windows, android, wasm, etc.)
xmake f -p android --ndk=/path/to/ndk     # Android NDK configuration
xmake f -p wasm                          # WebAssembly compilation
xmake f -p cross --sdk=/path/to/toolchain # Cross-compilation

# Building
xmake                                     # Build all targets
xmake -r                                 # Rebuild (force)
xmake -j8                                # Parallel build with 8 jobs
xmake -t clean                           # Clean build artifacts

# Testing
xmake test                               # Run unit tests

# Package Management
xrepo install <package>                  # Install a package
xrepo search <package>                   # Search for packages
```

### Basic xmake.lua Structure

```lua
-- Project configuration
add_requires("package1", "package2")

-- Target definition
target("myapp")
    set_kind("binary")
    add_files("src/*.c")
    add_packages("package1")
```

## Project Complexity Assessment

When starting a new project, evaluate its complexity to select appropriate features:

### Simple Projects (Single target, few dependencies)
- Use basic `add_files()` and `add_options()`
- No complex directory structure needed

### Medium Projects (Multiple targets, internal modules)
- Use `add_subdirs()` for modular structure
- Use `add_requires()` + `add_packages()` for external dependencies
- Consider custom options with `option()`

### Complex Projects (Multi-platform, many dependencies)
- Use all of the above plus:
- Custom rules (`add_rules()`)
- Package configurations with `add_requireconfs()`
- Platform-specific code with `is_plat()`, `is_arch()`
- Pre/post build commands
- Job graph optimization for parallel builds
- Integration with external build systems

## Project Structure

See `references/project-structure.md` for detailed guidance on organizing xmake projects, including:
- Using `add_subdirs()` for modular builds
- Target visibility and inheritance
- Include patterns and best practices

## Package Management

See `references/package-management.md` for comprehensive coverage of:
- Declaring dependencies with `add_requires()`
- Binding packages to targets with `add_packages()`
- Version specifications and constraints
- Optional and system packages
- Package configurations
- Multi-repository support

## Cross-Compilation

See `references/cross-compilation.md` for detailed coverage of:
- Automatic toolchain detection
- Manual toolchain configuration
- Custom cross-platform builds
- Android NDK integration

## Android Development

See `references/android-development.md` for:
- NDK toolchain setup
- STL configuration (llvm, gnustl)
- SDK integration
- Multi-architecture builds (arm64-v8a, armeabi-v7a)

## Windows Driver Kit (WDK)

See `references/wdk-development.md` for:
- KMDF/UMDF/WDM driver development
- Driver signing
- TraceWPP and MANIFEST tools
- Code signing configuration

## WebAssembly (WASM)

See `references/wasm-development.md` for:
- Emscripten toolchain setup
- WASI support
- Browser-based WASM compilation

## Custom Rules and Policies

See `references/rules-policies.md` for:
- Creating custom build rules
- Built-in rules (wdk, qt, etc.)
- Pre/post build commands
- Policy configuration

## Testing

See `references/testing.md` for:
- Unit test integration
- Test target configuration
- Running specific test groups
- CI/CD integration

## Integration with External Build Systems

See `references/external-builds.md` for:
- Building cmake projects from xmake
- Building make/autotools projects
- Using xrepo.cmake in CMake projects
- Mixed xmake/cmake builds

## Porting Build Systems

See `references/porting-build-systems.md` for:
- Quick TryBuild for existing projects
- Porting CMake to xmake (targets, dependencies, options, install)
- Porting Make to xmake (targets, phony, pattern rules)
- Porting Autotools to xmake (configure options, AC_SEARCH_LIBS)
- Porting Premake to xmake (filters, configurations)
- Porting build tasks (custom targets, scripts)
- Advanced porting (FetchContent, find_package, generator expressions)
- Cross-compilation porting
- Testing porting (CTest, Google Test)
- Mixed build systems and xrepo.cmake usage
- Complete porting checklist

## Advanced Features

See `references/advanced-features.md` for:
- Job graph optimization (`async.jobgraph`)
- Parallel build configuration
- Custom defines and flags
- Preprocessor handling
- Lua scripting in xmake
- Toolchain customization

## Plugins and Tasks

See `references/plugins-tasks.md` for:
- Defining custom tasks
- Task menu configuration
- Builtin plugins (project generation, compile_commands)
- Creating custom plugins
- Task dependencies and macros
- Event-based tasks

## CI/CD Integration

See `references/ci-cd.md` for:
- GitHub Actions workflows
- GitLab CI pipelines
- CircleCI configuration
- Azure Pipelines
- Caching strategies
- Cross-platform builds

## Target APIs

See `references/target-apis.md` for:
- Target information APIs (name, kind, targetfile, targetdir)
- Detection APIs (has_cfuncs, has_cflags, check_sizeof)
- Platform/architecture checks (is_plat, is_arch)
- Lifecycle hooks (on_load, on_build, on_install)
- Source and object file management

## Lua Best Practices

See `references/lua-best-practices.md` for:
- Root scope vs target scope
- Declaring helper functions
- Modularizing xmake.lua with include() and add_subdirs()
- Using import() for modules
- Table-driven configuration
- Common patterns and anti-patterns

## Advanced Build Features

See `references/advanced-build.md` for:
- Build cache acceleration
- Distributed compilation
- Remote compilation
- Unity build
- LTO optimization
- Sanitizers (ASan, USan, TSan, MSan)
- Performance tuning

## Graphics & Framework Development

See `references/graphics-frameworks.md` for:
- OpenGL development
- Vulkan programs
- SDL2 applications
- ImGui integration
- Raylib games
- Metal (macOS/iOS)
- Qt applications
- Terminal UI (NCurses)

## Multi-Language Support

See `references/multi-language.md` for:
- CUDA development
- Rust integration
- Swift support
- Go projects
- D language
- Fortran
- Nim
- Zig
- Vala
- Pascal
- Lex/Yacc
- Protocol Buffers
- OpenMP

## IDE Integration

See `references/ide-integration.md` for:
- VSCode integration
- IntelliJ IDEA / CLion
- Vim/Neovim
- Emacs
- Visual Studio
- Zed Editor
- Language Server Protocol (LSP)

## API Reference

See `references/api-reference.md` for:
- Global interfaces
- Target APIs
- Option APIs
- Builtin rules
- Builtin policies
- Builtin variables
- Specification

## Builtin Modules

See `references/builtin-modules.md` for:
- io - File I/O operations
- os - Operating system utilities
- path - Path manipulation
- string - String operations
- table - Table operations
- print/printf/cprint - Output functions
- import - Module importing
- coroutine - Coroutine support
- hash - Hashing utilities
- raise - Exception handling
- try-catch-finally - Error handling

## Builtin Rules

See `references/builtin-rules.md` for:
- Build mode rules (debug, release, profile, coverage)
- Qt rules (widgetapp, quickapp, console)
- Xcode rules (bundle, framework, application)
- Android rules (native_app)
- WDK rules
- Unity build

## Builtin Policies

See `references/builtin-policies.md` for:
- Build policies (parallel, ccache, LTO, sanitizers)
- Check policies (auto_ignore_flags, auto_map_flags)
- Package policies (precompiled, fetch_only)
- Network policies

## Builtin Variables

See `references/builtin-variables.md` for:
- Directory variables (builddir, projectdir, scriptdir)
- Environment variables ($(env VAR), $(shell cmd))
- Custom variables
- Registry access on Windows

## Basic Commands

See `references/basic-commands.md` for:
- Build commands (build, rebuild, clean)
- Run commands
- Test commands
- Install/uninstall
- Package commands
- Project creation

## Complete TOC Mapping

See `references/toc-mapping.md` for:
- Complete mapping of all xmake documentation sections
- References to specific guides for each topic
- Untitled section derivations from URLs
