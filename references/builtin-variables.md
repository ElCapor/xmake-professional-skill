# Builtin Variables

Xmake provides `$(varname)` syntax for built-in variables.

## Usage

```lua
add_cxflags("-I$(builddir)")
add_files("$(projectdir)/src/*.c")
```

## Directory Variables

### $(os)

Current build platform OS (e.g., ios, linux, windows).

### $(host)

Native host OS (macosx, linux, windows).

### $(plat)

Target platform.

### $(arch)

Target architecture.

### $(mode)

Build mode (debug, release).

### $(tmpdir)

Temporary directory.

### $(curdir)

Current working directory.

### $(builddir)

Build output directory (default: `./build`).

### $(scriptdir)

Current xmake.lua directory.

### $(projectdir)

Project root directory (from `xmake -P`).

### $(globaldir)

Global config directory (default: `~/.config`).

### $(configdir)

Project config directory.

### $(programdir)

Xmake installation directory.

## Environment Variables

### $(env VAR)

Get environment variable:

```lua
add_includedirs("$(env PROGRAMFILES)/OpenSSL/inc")
```

### $(shell cmd)

Execute shell command:

```lua
add_ldflags("$(shell pkg-config --libs sqlite3)")
```

## Registry (Windows)

### $(reg path;name)

Read Windows registry:

```lua
local value = os.getenv("VS_PATH")  -- via env
```

## Custom Variables

### From Command Line

```bash
xmake f --myvar=value
```

```lua
add_defines("MYVAR=$(myvar)")
```

## Common Patterns

```lua
target("myapp")
    add_files("$(projectdir)/src/*.c")
    add_includedirs("$(builddir)/generated")
    add_defines("BUILD_DIR=\"$(builddir)\"")
    on_run(function(target)
        os.cp("$(scriptdir)/config.json", "$(builddir)/")
    end)
```
