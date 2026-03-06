# Target APIs

Target APIs are used in target scripts (on_load, on_build, etc.) to access and manipulate target properties.

## Basic Target Information

### target:name()

Get the target name:

```lua
on_load(function (target)
    print("Target name: " .. target:name())
end)
```

### target:kind()

Get target kind (binary, static, shared):

```lua
on_load(function (target)
    print("Kind: " .. target:kind())
end)
```

### target:get(key)

Get target configuration by key:

```lua
on_load(function (target)
    local links = target:get("links")
    local defines = target:get("defines")
    local includedirs = target:get("includedirs")
end)
```

### target:set(key, value)

Set target configuration:

```lua
on_load(function (target)
    target:set("links", "pthread")
    target:set("defines", "MY_DEFINE")
end)
```

### target:add(key, value)

Add to target configuration:

```lua
on_load(function (target)
    target:add("links", "pthread")
    target:add("defines", "EXTRA_DEF")
end)
```

## File Paths

### target:targetfile()

Get the output file path:

```lua
on_build(function (target)
    local output = target:targetfile()
    print("Building: " .. output)
end)

after_build(function (target)
    os.cp(target:targetfile(), "/tmp/")
end)
```

### target:targetdir()

Get the output directory:

```lua
on_build_file(function (target, sourcefile)
    local dir = target:targetdir()
    local out = path.join(dir, path.basename(sourcefile) .. ".o")
end)
```

### target:scriptdir()

Get the target's xmake.lua directory:

```lua
on_load(function (target)
    local dir = target:scriptdir()
end)
```

### target:basename()

Get the base name of target:

```lua
on_load(function (target)
    print(target:basename())
end)
```

### target:filename()

Get the filename of target:

```lua
on_load(function (target)
    print(target:filename())
end)
```

## Symbol and Install

### target:symbolfile()

Get symbol file path:

```lua
on_build(function (target)
    local sym = target:symbolfile()
end)
```

### target:installdir()

Get install directory:

```lua
on_install(function (target)
    local dir = target:installdir()
end)
```

## Object Files

### target:objectfile(sourcefile)

Get object file path for a source file:

```lua
on_build_file(function (target, sourcefile)
    local obj = target:objectfile(sourcefile)
end)
```

### target:objectfiles()

Get all object files:

```lua
on_load(function (target)
    local objs = target:objectfiles()
end)
```

## Source Files

### target:sourcebatches()

Get source batches:

```lua
on_load(function (target)
    local batches = target:sourcebatches()
end)
```

### target:headerfiles()

Get header files:

```lua
on_load(function (target)
    local headers = target:headerfiles()
end)
```

## Detection APIs

### target:has_cfuncs(funcname, opts)

Check if C function exists:

```lua
on_load(function (target)
    if target:has_cfuncs("foo", {includes = "foo.h"}) then
        target:add("defines", "HAVE_FOO")
    end
end)
```

### target:has_cxxfuncs(funcname, opts)

Check if C++ function exists:

```lua
target:has_cxxfuncs("std::cout", {includes = "iostream"})
```

### target:has_ctypes(typename, opts)

Check if C type exists:

```lua
target:has_ctypes("foo_t", {includes = "foo.h"})
```

### target:has_cflags(flags)

Check if C flags are supported:

```lua
if target:has_cflags("-Wall") then
    target:add("cflags", "-Wall")
end
```

### target:has_cxxflags(flags)

Check if C++ flags are supported:

```lua
target:has_cxxflags("-std=c++17")
```

### target:has_cincludes(header)

Check if header can be included:

```lua
target:has_cincludes("stdio.h")
```

### target:has_features(feature)

Check if compiler feature is available:

```lua
target:has_features("cxx_constexpr")
```

### target:check_csnippets(code)

Check if C code compiles:

```lua
target:check_csnippets([[
    int foo() { return 0; }
]])
```

### target:check_sizeof(type)

Get size of type:

```lua
local size = target:check_sizeof("long")
```

## Platform/Architecture

### target:is_plat(...)

Check if target platform matches:

```lua
on_load(function (target)
    if target:is_plat("windows") then
        target:add("links", "ws2_32")
    end
end)
```

### target:is_arch(...)

Check if target architecture matches:

```lua
on_load(function (target)
    if target:is_arch("arm64-v8a") then
        target:add("defines", "ARM64")
    end
end)
```

### target:is_arch64()

Check if target is 64-bit architecture:

```lua
if target:is_arch64() then
    target:add("defines", "ARCH_64")
end
```

## Lifecycle Hooks

### on_load

Called when target is loaded:

```lua
target("myapp")
    on_load(function (target)
        target:add("links", "pthread")
    end)
```

### on_config

Called after configuration:

```lua
on_config(function (target)
    -- post-config logic
end)
```

### before_build / after_build

Before/after build:

```lua
before_build(function (target)
    print("Building " .. target:name())
end)

after_build(function (target)
    os.cp(target:targetfile(), "/output/")
end)
```

### on_build

Called during build:

```lua
on_build(function (target)
    -- custom build logic
end)
```

### on_build_file

Called for each source file:

```lua
on_build_file(function (target, sourcefile, opt)
    print("Building " .. sourcefile)
end)
```

### on_install / on_uninstall

During install/uninstall:

```lua
on_install(function (target)
    os.cp(target:targetfile(), "$(installdir)/bin/")
end)

on_uninstall(function (target)
    os.rm("$(installdir)/bin/" .. target:filename())
end)
```

### on_run

When running target:

```lua
on_run(function (target)
    os.exec(target:targetfile())
end)
```
