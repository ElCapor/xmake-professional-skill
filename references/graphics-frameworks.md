# Graphics & Framework Development

This reference covers graphics programming and UI frameworks supported by xmake.

## OpenGL

### Basic OpenGL Program

```lua
target("opengl_app")
    set_kind("binary")
    add_files("src/*.cpp")
    add_links("gl", "glu", "glut")
```

### With GLEW

```lua
add_requires("glew", "glut")

target("opengl_app")
    set_kind("binary")
    add_files("src/*.cpp")
    add_packages("glew", "glut")
```

## Vulkan

### Basic Vulkan Setup

```lua
add_requires("vulkan-sdk")

target("vulkan_app")
    set_kind("binary")
    add_files("src/*.cpp")
    add_packages("vulkan-sdk")
    add_defines("VK_USE_PLATFORM_WIN32_KHR")
```

## SDL2

### Basic SDL2 Program

```lua
add_requires("sdl2")

target("sdl_app")
    set_kind("binary")
    add_files("src/*.cpp")
    add_packages("sdl2", "sdl2_image", "sdl2_mixer")
```

### SDL2 with Image/Mixer

```lua
add_requires("sdl2", "sdl2_image", "sdl2_mixer", "sdl2_ttf")

target("sdl_game")
    set_kind("binary")
    add_files("src/*.cpp")
    add_packages("sdl2", "sdl2_image", "sdl2_mixer", "sdl2_ttf")
```

## ImGui

### ImGui with SDL2

```lua
add_requires("imgui", "sdl2")

target("imgui_app")
    set_kind("binary")
    add_files("src/*.cpp")
    add_packages("imgui", "sdl2")
```

### ImGui with OpenGL

```lua
add_requires("imgui", "glew")

target("imgui_gl")
    set_kind("binary")
    add_files("src/*.cpp")
    add_packages("imgui", "glew")
```

## Raylib

### Basic Raylib Program

```lua
add_requires("raylib")

target("raylib_game")
    set_kind("binary")
    add_files("src/*.cpp")
    add_packages("raylib")
```

## Metal (macOS/iOS)

### Metal Framework

```lua
target("metal_app")
    set_kind("binary")
    add_files("src/*.cpp", "src/*.metal")
    add_frameworks("Metal", "MetalKit")
```

## Qt

### Qt Widget Application

```lua
add_requires("qt5core", {optional = true})

target("qt_app")
    set_kind("binary")
    add_files("src/*.cpp")
    add_rules("qt.widgetapp")
    add_frameworks("QtCore", "QtGui", "QtWidgets")
```

### Qt Quick Application

```lua
target("qt_quick")
    set_kind("binary")
    add_files("src/*.cpp")
    add_rules("qt.quickapp")
```

## Terminal UI (TUI)

### NCurses

```lua
add_requires("ncurses")

target("tui_app")
    set_kind("binary")
    add_files("src/*.cpp")
    add_packages("ncurses")
    addlinks("ncurses", "panel", "form")
```

## Audio

### Using SDL2_mixer

```lua
add_requires("sdl2_mixer")

target("audio_app")
    set_kind("binary")
    add_files("src/*.cpp")
    add_packages("sdl2", "sdl2_mixer")
```

## Best Practices

1. Use package managers for framework dependencies
2. Platform-specific code via `is_plat()` and `is_arch()`
3. Use frameworks on Apple platforms
4. Use pkg-config on Linux
