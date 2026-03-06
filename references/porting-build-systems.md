# Porting Build Systems to Xmake

This guide covers porting existing projects from CMake, Make, Autotools, Premake, and other build systems to xmake.

## Quick Start: TryBuild

Before fully porting, test if xmake can build the project without modifications:

```bash
# Auto-detect build system
xmake

# Explicitly specify
xmake f --trybuild=cmake
xmake f --trybuild=make
xmake f --trybuild=autotools
xmake f --trybuild=meson
xmake f --trybuild=ninja
xmake f --trybuild=scons
xmake f --trybuild=msbuild
xmake f --trybuild=xbazel
```

This automatically detects CMakeLists.txt, Makefile, configure, etc.

---

## Porting CMake to Xmake

### Basic Target Mapping

| CMake | Xmake |
|-------|-------|
| `add_executable()` | `set_kind("binary")` |
| `add_library(STATIC)` | `set_kind("static")` |
| `add_library(SHARED)` | `set_kind("shared")` |
| `add_library(MODULE)` | `set_kind("shared")` + custom rule |
| `add_subdirectory()` | `add_subdirs()` |
| `include()` | `include()` |

### Example Port

**CMake:**
```cmake
cmake_minimum_required(VERSION 3.15)
project(MyApp)

add_executable(myapp main.cpp utils.cpp)
target_link_libraries(myapp PRIVATE pthread)
target_compile_options(myapp PRIVATE -Wall -O2)
target_compile_definitions(myapp PRIVATE VERSION=1.0)
```

**Xmake:**
```lua
target("myapp")
    set_kind("binary")
    add_files("main.cpp", "utils.cpp")
    add_links("pthread")
    add_cflags("-Wall", "-O2")
    add_defines("VERSION=1.0")
```

### Porting Dependencies

**CMake:**
```cmake
find_package(ZLIB REQUIRED)
find_package(OpenSSL 1.1)
find_package(Boost REQUIRED)

target_link_libraries(myapp ZLIB::ZLIB OpenSSL::SSL Boost::filesystem)
```

**Xmake:**
```lua
add_requires("zlib", "openssl", "boost")

target("myapp")
    add_files("src/*.cpp")
    add_packages("zlib", "openssl", "boost")
```

### Porting Options

**CMake:**
```cmake
option(BUILD_TESTS "Build tests" ON)
option(ENABLE_FEATURE_X "Enable feature X" OFF)

if(BUILD_TESTS)
    add_subdirectory(tests)
endif()
```

**Xmake:**
```lua
option("tests")
    set_default(true)
    set_description("Build tests")

option("feature_x")
    set_default(false)
    set_description("Enable feature X")

target("myapp")
    add_files("src/*.cpp")
    add_options("feature_x")

if has_config("tests") then
    add_subdirs("tests")
end
```

### Porting Conditional Compilation

**CMake:**
```cmake
if(WIN32)
    target_sources(myapp PRIVATE win32.cpp)
elseif(UNIX)
    target_sources(myapp PRIVATE unix.cpp)
endif()

if(CMAKE_BUILD_TYPE STREQUAL "Debug")
    target_compile_definitions(myapp PRIVATE DEBUG=1)
endif()
```

**Xmake:**
```lua
if is_plat("windows") then
    add_files("win32.cpp")
elseif is_plat("linux", "macosx") then
    add_files("unix.cpp")
end

if is_mode("debug") then
    add_defines("DEBUG=1")
end
```

### Porting Generator Expressions

**CMake:**
```cmake
target_include_directories(myapp PRIVATE 
    $<BUILD_INTERFACE:${CMAKE_SOURCE_DIR}/include>
    $<INSTALL_INTERFACE:include>
)
```

**Xmake:**
```lua
target("myapp")
    add_includedirs("include")
```

### Porting Custom Commands

**CMake:**
```cmake
add_custom_command(
    OUTPUT generated.cpp
    COMMAND generatortool --input=input.txt --output=generated.cpp
    DEPENDS input.txt
)

add_executable(myapp main.cpp generated.cpp)
```

**Xmake:**
```lua
target("generator")
    set_kind("binary")
    add_files("generator.cpp")

target("myapp")
    set_kind("binary")
    add_files("main.cpp")
    add_deps("generator")
    after_build(function(target)
        os.execute("generatortool --input=input.txt --output=generated.cpp")
    end)
    add_files("generated.cpp")
```

Or use custom rules:
```lua
rule("gen")
    on_build_file(function(target, sourcefile)
        local output = target:targetdir() .. "/" .. path.basename(sourcefile) .. ".cpp"
        os.execute("generatortool --input=" .. sourcefile .. " --output=" .. output)
    end)

target("myapp")
    add_files("input.txt", {rules = "gen"})
```

### Porting Install Rules

**CMake:**
```cmake
install(TARGETS myapp DESTINATION bin)
install(FILES header.h DESTINATION include)
```

**Xmake:**
```lua
target("myapp")
    add_install_files("bin", "myapp")
    add_install_files("include/header.h", "include")
```

---

## Porting Make to Xmake

### Basic Port

**Make:**
```makefile
CC = gcc
CFLAGS = -Wall -O2 -Iinclude
LDFLAGS = -lpthread

SRCS = main.c utils.c config.c
OBJS = $(SRCS:.c=.o)

myapp: $(OBJS)
    $(CC) -o $@ $^ $(LDFLAGS)

%.o: %.c
    $(CC) $(CFLAGS) -c -o $@ $<

clean:
    rm -f $(OBJS) myapp
```

**Xmake:**
```lua
target("myapp")
    set_kind("binary")
    add_files("*.c")
    add_includedirs("include")
    add_cflags("-Wall", "-O2")
    add_links("pthread")
```

### Porting Phony Targets

**Make:**
```makefile
.PHONY: all clean test install

all: myapp

test: myapp
    ./myapp --test

install: myapp
    install -D myapp /usr/local/bin/

clean:
    rm -f *.o myapp
```

**Xmake:**
```lua
target("myapp")
    set_kind("binary")
    add_files("*.c")

-- Test target
target("test")
    set_kind("binary")
    set_default(false)
    add_files("test/*.c")
    after_build(function()
        os.execute("./test_runner")
    end)
```

### Porting Pattern Rules

**Make:**
```makefile
%.o: %.c
    $(CC) $(CFLAGS) -c -o $@ $<
```

**Xmake:**
Xmake handles this automatically, but you can customize:
```lua
-- Custom compilation
on_build_file(function(target, sourcefile, opt)
    local objfile = target:objectfile(sourcefile)
    local cmd = string.format("%s -c %s -o %s", 
        target:get("cc") or "gcc",
        sourcefile, 
        objfile)
    os.execute(cmd)
end)
```

---

## Porting Autotools to Xmake

### Basic Port

**configure.ac / configure:**
```bash
./configure --prefix=/usr/local --enable-shared --disable-static
make
make install
```

**Xmake:**
```lua
add_rules("mode.release")

target("mylib")
    set_kind("shared")
    add_files("src/*.c")

-- Installation
add_installdirs("bin", "lib", "include")
```

### Porting configure Options

**Autotools:**
```bash
./configure --enable-feature --with-somelib --prefix=/opt
```

**Xmake:**
```lua
option("feature")
    set_default(true)
    set_description("Enable feature")

option("with_somelib")
    set_description("Path to somelib")
    set_default("/usr/local")

target("myapp")
    add_files("src/*.c")
    if has_config("feature") then
        add_defines("ENABLE_FEATURE")
    end
    if has_config("with_somelib") then
        add_includedirs(get_config("with_somelib") .. "/include")
    end
```

### Porting AC_SEARCH_LIBS

**Autotools:**
```bash
AC_SEARCH_LIBS([sqrt], [m], [], [AC_MSG_ERROR([libm not found])])
```

**Xmake:**
```lua
target("myapp")
    add_files("src/*.c")
    if target:has_ctypes("double sqrt(double)", {includes = "math.h"}) then
        add_links("m")
    else
        raise("libm not found")
    end
```

---

## Porting Premake to Xmake

### Basic Port

**Premake:**
```lua
workspace "MyApp"
    configurations { "Debug", "Release" }
    location "build"

project "MyApp"
    kind "ConsoleApp"
    language "C++"
    cppdialect "C++17"
    files { "src/**.cpp" }
    links { "pthread" }
    
    filter "configurations:Release"
        optimize "On"
```

**Xmake:**
```lua
add_rules("mode.debug", "mode.release")

target("MyApp")
    set_kind("binary")
    set_languages("c++17")
    add_files("src/**.cpp")
    add_links("pthread")
```

### Porting Filter/Config

**Premake:**
```lua
filter { "configurations:Debug" }
    defines { "DEBUG" }
    symbols "On"

filter { "configurations:Release" }
    defines { "NDEBUG" }
    optimize "On"

filter { "system:windows" }
    links { "ws2_32" }
    
filter { "system:linux" }
    links { "pthread" }
```

**Xmake:**
```lua
if is_mode("debug") then
    add_defines("DEBUG")
elseif is_mode("release") then
    add_defines("NDEBUG")
end

if is_plat("windows") then
    add_links("ws2_32")
elseif is_plat("linux") then
    add_links("pthread")
end
```

---

## Porting Build Tasks

### Porting Custom Make Targets

**Make:**
```makefile
.PHONY: doc lint format

doc:
    doxygen Doxyfile

lint:
    clang-tidy src/*.cpp

format:
    clang-format -i src/*.cpp
```

**Xmake:**
```lua
task("doc")
    on_run(function()
        os.execute("doxygen Doxyfile")
    end)
task_end()

task("lint")
    on_run(function()
        for _, f in ipairs(os.files("src/*.cpp")) do
            os.execute("clang-tidy " .. f)
        end
    end)
task_end()

task("format")
    on_run(function()
        os.execute("clang-format -i src/*.cpp")
    end)
task_end()
```

Run with:
```bash
xmake doc
xmake lint
xmake format
```

### Porting Complex Build Scripts

**Make:**
```makefile
build-release:
    mkdir -p build/release
    cd build/release
    cmake -DCMAKE_BUILD_TYPE=Release ../..
    make -j4

.PHONY: build-release
```

**Xmake:**
```lua
task("build-release")
    on_run(function()
        os.mkdir("build/release")
        os.cd("build/release")
        os.execute("cmake -DCMAKE_BUILD_TYPE=Release ../..")
        os.execute("cmake --build . -j4")
    end)
task_end()
```

---

## Advanced Porting

### Porting FetchContent/ExternalProject

**CMake:**
```cmake
include(FetchContent)
FetchContent_Declare(
    googletest
    GIT_REPOSITORY https://github.com/google/googletest.git
    GIT_TAG release-1.12.1
)
FetchContent_MakeAvailable(googletest)
```

**Xmake:**
```lua
add_requires("gtest")

target("mytest")
    set_kind("binary")
    add_files("test/*.cpp")
    add_packages("gtest")
```

Or for non-xmake packages:
```lua
package("mydep")
    add_deps("cmake")
    set_sourcedir(path.join(os.scriptdir(), "deps/mydep"))
    on_install(function(package)
        import("package.tools.cmake").install(package, {
            "-DCMAKE_BUILD_TYPE=Release",
            "-DBUILD_SHARED_LIBS=OFF"
        })
    end)
package_end()

add_requires("mydep")
```

### Porting find_package

**CMake:**
```cmake
find_package(OpenSSL 1.1 REQUIRED)
find_package(Threads REQUIRED)
pkg_check_modules(GTK3 REQUIRED gtk+-3.0)

target_link_libraries(myapp 
    OpenSSL::SSL 
    OpenSSL::Crypto
    Threads::Threads
    GTK3::gtk
)
```

**Xmake:**
```lua
-- Use xmake packages
add_requires("openssl", "gtk3")

-- Or find system packages
find_package("openssl")
find_package("threads")

target("myapp")
    add_files("src/*.cpp")
    add_packages("openssl", "gtk3")
```

### Porting Generator Expressions

**CMake:**
```cmake
target_include_directories(myapp PRIVATE
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
    $<INSTALL_INTERFACE:$<INSTALL_PREFIX>/include>
)
```

**Xmake:**
```lua
-- Simple: just use relative paths
add_includedirs("include")

-- For installation
add_install_files("include/*.h", "include")
```

---

## Cross-Compilation Porting

### Porting CMake Toolchain Files

**CMake:**
```bash
cmake -DCMAKE_TOOLCHAIN_FILE=../toolchain.cmake ..
```

**Xmake:**
```bash
xmake f -p android --ndk=/path/to/ndk -a arm64-v8a
xmake
```

### Porting Autotools Cross-Compilation

**Autotools:**
```bash
./configure --host=aarch64-linux-gnu --prefix=/opt
make
```

**Xmake:**
```bash
xmake f -p cross --sdk=/path/to/toolchain --cross=aarch64-linux-gnu
xmake
```

---

## Porting Testing

### Porting CTest

**CMake:**
```cmake
enable_testing()
add_test(NAME MyTest COMMAND mytest)
```

**Xmake:**
```lua
target("mytest")
    set_kind("binary")
    add_files("test/*.cpp")
    add_tests()
```

Run:
```bash
xmake test
```

### Porting Google Test

**CMake:**
```cmake
add_executable(mytest test.cpp)
target_link_libraries(mytest gtest gtest_main)
include(GoogleTest)
gtest_discover_tests(mytest)
```

**Xmake:**
```lua
add_requires("gtest")

target("mytest")
    set_kind("binary")
    add_files("test/*.cpp")
    add_packages("gtest")
    add_tests()
```

---

## Mixed Build Systems

### Using Both CMake and Xmake

Keep CMakeLists.txt for external users, add xmake.lua for xmake users:

```bash
# Build with xmake
xmake

# Or use xmake to invoke cmake
xmake f --trybuild=cmake
xmake
```

### Xmake as Package Manager for CMake

Use xrepo.cmake in your CMake project:

```cmake
include(FetchContent)
FetchContent_Declare(xrepo_cmake GIT_REPOSITORY https://github.com/xmake-io/xrepo-cmake GIT_TAG main)
FetchContent_MakeAvailable(xrepo_cmake)

xrepo_package(zlib)
xrepo_package(openssl)

target_link_libraries(myapp ZLIB::ZLIB OpenSSL::SSL)
```

---

## Porting Checklist

| Category | Legacy System | Xmake Equivalent |
|----------|---------------|------------------|
| Executable | `add_executable()` | `set_kind("binary")` |
| Static Lib | `add_library(STATIC)` | `set_kind("static")` |
| Shared Lib | `add_library(SHARED)` | `set_kind("shared")` |
| Include Dir | `target_include_directories()` | `add_includedirs()` |
| Links | `target_link_libraries()` | `add_links()` / `add_packages()` |
| Defines | `target_compile_definitions()` | `add_defines()` |
| Flags | `target_compile_options()` | `add_cflags()` |
| Subdirs | `add_subdirectory()` | `add_subdirs()` |
| Options | `option()` / `cmake_option()` | `option()` |
| Find Package | `find_package()` | `add_requires()` / `find_package()` |
| Test | `add_test()` / `gtest_discover_tests()` | `add_tests()` |
| Install | `install()` | `add_install_files()` |
| Custom Cmd | `add_custom_command()` | `rule()` / `on_build()` |
