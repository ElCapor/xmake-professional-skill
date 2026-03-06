# Integration with External Build Systems

Xmake can integrate with cmake, make, autotools, and other build systems.

## TryBuild Mode

Build projects maintained by other build systems directly from xmake:

```bash
$ xmake f --trybuild=cmake
$ xmake f --trybuild=autotools
$ xmake f --trybuild=make
$ xmake f --trybuild=meson
$ xmake f --trybuild=ninja
$ xmake f --trybuild=msbuild
$ xmake f --trybuild=xcodebuild
```

### With Cross-Compilation

```bash
$ xmake f -p android --trybuild=autotools --ndk=/path/to/ndk
$ xmake f -p iphoneos --trybuild=cmake
$ xmake f -p mingw --trybuild=cmake --mingw=/path/to/mingw
$ xmake f -p cross --trybuild=cmake --sdk=/path/to/toolchain
```

### Passing Extra Configuration

```bash
$ xmake f --trybuild=autotools --tryconfigs="--enable-shared=no"
```

## Building CMake Projects from Xmake

### Using package.tools.cmake

Integrate a CMake-based library as a package:

```lua
add_rules("mode.debug", "mode.release")

package("foo")
    add_deps("cmake")
    set_sourcedir(path.join(os.scriptdir(), "foo"))
    on_install(function (package)
        local configs = {}
        table.insert(configs, "-DCMAKE_BUILD_TYPE=" .. (package:debug() and "Debug" or "Release"))
        table.insert(configs, "-DBUILD_SHARED_LIBS=" .. (package:config("shared") and "ON" or "OFF"))
        import("package.tools.cmake").install(package, configs)
    end)
    on_test(function (package)
        assert(package:has_cfuncs("add", {includes = "foo.h"}))
    end)
package_end()

add_requires("foo")

target("demo")
    set_kind("binary")
    add_files("src/main.c")
    add_packages("foo")
```

### Using Autotools

```lua
package("pcre2")
    set_sourcedir(path.join(os.scriptdir(), "3rd/pcre2"))
    on_install(function (package)
        import("package.tools.autotools").install(package, {"--shared"})
    end)
package_end()
```

## Using Xrepo.cmake in CMake Projects

Use xmake packages in CMake projects via xrepo.cmake:

```cmake
# Download xrepo.cmake
include(FetchContent)
FetchContent_Declare(xrepo_cmake GIT_REPOSITORY https://github.com/xmake-io/xrepo-cmake GIT_TAG main)
FetchContent_MakeAvailable(xrepo_cmake)

# Use xrepo_package to get a package
xrepo_package(zlib)
xrepo_package(openssl)
```

### Example

```cmake
# Download xrepo.cmake if not exists
if(NOT EXISTS "${CMAKE_BINARY_DIR}/xrepo.cmake")
    file(DOWNLOAD "https://raw.githubusercontent.com/xmake-io/xrepo-cmake/main/xrepo.cmake"
        "${CMAKE_BINARY_DIR}/xrepo.cmake")
endif()

include(${CMAKE_BINARY_DIR}/xrepo.cmake)

# Install and use zlib
xrepo_package(zlib)
target_link_libraries(myapp PRIVATE ZLIB::ZLIB)
```

## Finding CMake Packages in Xmake

Use CMake's find_package within xmake:

```bash
xmake l find_package cmake::ZLIB
xmake l find_package cmake::LibXml2
```

## Mixed Xmake/CMake Projects

Generate CMakeLists.txt from xmake:

```bash
$ xmake project -k cmakelists
```

## Building from Xrepo

Build packages using xrepo:

```bash
$ xrepo install -p android zlib
$ xrepo install -p android --ndk=/path/to/ndk zlib
```

## Best Practices

1. Use `--trybuild=` for quick integration of external projects
2. Use `package.tools.cmake` for full integration as a dependency
3. Use xrepo.cmake to use xmake packages in CMake projects
4. Set appropriate cross-compilation parameters when building for other platforms
