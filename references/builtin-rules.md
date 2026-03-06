# Builtin Rules

Xmake provides built-in rules to simplify common build scenarios.

## Build Mode Rules

### mode.debug

```lua
add_rules("mode.debug")
```

Equivalent to:
```lua
if is_mode("debug") then
    set_symbols("debug")
    set_optimize("none")
end
```

Switch with: `xmake f -m debug`

### mode.release

```lua
add_rules("mode.release")
```

Equivalent to:
```lua
if is_mode("release") then
    set_symbols("hidden")
    set_optimize("fastest")
    set_strip("all")
end
```

Switch with: `xmake f -m release`

### mode.releasedbg

```lua
add_rules("mode.releasedbg")
```

Generates release binary + separate debug symbols (.pdb, .dSYM, .sym).

### mode.minsizerel

```lua
add_rules("mode.minsizerel")
```

Optimizes for size over speed.

### mode.check

```lua
add_rules("mode.check")
```

Memory detection mode with AddressSanitizer.

### mode.profile

```lua
add_rules("mode.profile")
```

Performance profiling with -pg flag.

### mode.coverage

```lua
add_rules("mode.coverage")
```

Code coverage analysis.

### mode.valgrind

```lua
add_rules("mode.valgrind")
```

Valgrind memory analysis support.

### mode.asan

```lua
add_rules("mode.asan")
```

AddressSanitizer support.

### mode.tsan

```lua
add_rules("mode.tsan")
```

ThreadSanitizer support.

### mode.lsan

```lua
add_rules("mode.lsan")
```

LeakSanitizer support.

### mode.ubsan

```lua
add_rules("mode.ubsan")
```

UndefinedBehaviorSanitizer support.

## Qt Rules

### qt.static

Static Qt library:
```lua
target("qtlib")
    add_rules("qt.static")
    add_files("src/*.cpp")
    add_frameworks("QtNetwork", "QtGui")
```

### qt.shared

Shared Qt library:
```lua
target("qtlib")
    add_rules("qt.shared")
    add_files("src/*.cpp")
```

### qt.console

Qt console application:
```lua
target("qtconsole")
    add_rules("qt.console")
    add_files("src/*.cpp")
```

### qt.quickapp

Qt Quick/QML application:
```lua
target("qtquick")
    add_rules("qt.quickapp")
    add_files("src/*.cpp")
    add_files("src/qml.qrc")
```

### qt.widgetapp

Qt Widgets application:
```lua
target("qtwidgets")
    add_rules("qt.widgetapp")
    add_files("src/*.cpp")
    add_files("src/mainwindow.ui")
    add_files("src/mainwindow.h")  -- Q_OBJECT macro
```

## Xcode Rules (iOS/macOS)

### xcode.bundle

```lua
target("mybundle")
    add_rules("xcode.bundle")
    add_files("src/*.m")
    add_files("src/Info.plist")
```

### xcode.framework

```lua
target("myframework")
    add_rules("xcode.framework")
    add_files("src/*.m")
    add_files("src/Info.plist")
```

### xcode.application

```lua
target("myapp")
    add_rules("xcode.application")
    add_files("src/*.m", "src/**.storyboard", "src/*.xcassets")
    add_files("src/Info.plist")
```

## Android Rules

### android.native_app

Android native application:
```lua
target("android_app")
    set_kind("binary")
    add_files("src/main.cpp", "src/android_native_app_glue.c")
    add_syslinks("log")
    add_rules("android.native_app", {
        android_sdk_version = "35",
        android_manifest = "android/AndroidManifest.xml",
        package_name = "com.example.myapp"
    })
```

## WDK Rules

See `references/wdk-development.md` for Windows Driver Kit rules:
- `wdk.driver`
- `wdk.static`
- `wdk.shared`
- `wdk.binary`
- `wdk.env.kmdf`
- `wdk.env.umdf`
- `wdk.env.wdm`

## Unity Build Rule

### utils.unity_build

```lua
add_rules("utils.unity_build")

target("myapp")
    add_files("src/*.c", {unity = true})
```

## Other Rules

### bin2c

Binary to C header conversion:
```lua
add_files("src/*.png", {rules = "utils.bin2c"})
```

### embed.keil

Keil embedded development:
```lua
add_files("src/*.c", {rules = "embed.keil"})
```
