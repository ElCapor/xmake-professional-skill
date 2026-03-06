# API Reference Overview

This section covers all xmake API references organized by category.

## Description APIs

### Global Interfaces

Used at root scope to configure targets, options, packages, and project settings.

Key APIs:
- `add_requires()` - Declare package dependencies
- `add_rules()` - Add build rules
- `add_subdirs()` - Include subdirectories
- `add_includedirs()` - Add include directories
- `add_defines()` - Add preprocessor defines
- `add_files()` - Add source files
- `add_links()` - Add linker libraries
- `target()` - Define build targets
- `option()` - Define configuration options

### Target APIs

Used within target scope to configure specific targets.

Key APIs:
- `set_kind()` - Set target type (binary, static, shared)
- `add_packages()` - Bind packages to target
- `on_load()` - Target load hook
- `on_build()` - Target build hook
- `on_install()` - Target install hook

See `references/target-apis.md` for complete target API reference.

### Option APIs

Used to define configurable options:

```lua
option("myoption")
    set_default(false)
    set_description("Enable my feature")
    add_links("mylib")
```

## Builtin Rules

See `references/builtin-rules.md` for complete builtin rules reference including:
- `mode.debug` - Debug build mode
- `mode.release` - Release build mode
- `qt.widgetapp` - Qt widget application
- `qt.quickapp` - Qt Quick application
- `android.native_app` - Android native application
- `wdk.driver` - Windows driver

## Builtin Policies

See `references/builtin-policies.md` for policy configuration:

```lua
add_policies("build.optimization.lto")  -- Link-time optimization
add_policies("build.sanitizer.address")  -- Address sanitizer
add_policies("package.precompiled")  -- Precompiled packages
```

## Builtin Variables

See `references/builtin-variables.md` for builtin variable reference:
- `$(buildir)` - Build directory
- `$(targetdir)` - Target output directory
- `$(installdir)` - Install directory
- `$(scriptdir)` - Script directory
- `$(projectdir)` - Project directory

## Specification

See `references/specification.md` for complete xmake.lua syntax specification.

## Conditions

See `references/conditions.md` for condition expressions:

```lua
if is_plat("windows") then
    -- windows specific
end

if is_arch("arm64") then
    -- arm64 specific
end
```
