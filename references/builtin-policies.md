# Builtin Policies

Policies control xmake's build behavior. Use `set_policy()` to modify defaults.

## Getting All Policies

```bash
xmake l core.project.policy.policies
```

## Configuration

### In xmake.lua

```lua
-- Global (affects all targets)
set_policy("policy.name", true)

-- Target-specific
target("myapp")
    set_policy("policy.name", true)
```

### Via Command Line

```bash
xmake f --policies=policy1,policy2
xmake f --policies=policy:n  # disable
```

## Build Policies

### build.across_targets_in_parallel

Parallel build between targets (enabled by default).

```lua
set_policy("build.across_targets_in_parallel", false)  -- disable
```

### build.fence

Restrict parallel compilation for specific targets.

```lua
target("codegen")
    set_policy("build.fence", true)  -- compile first
```

### build.merge_archive

Merge dependent static libraries into parent.

```lua
target("app")
    add_deps("lib1", "lib2")
    set_policy("build.merge_archive", true)
```

### build.ccache

Enable build cache (enabled by default).

```lua
set_policy("build.ccache", false)  -- disable
```

Or via command line:

```bash
xmake f --ccache=n
```

### build.optimization.lto

Link-Time Optimization.

```lua
set_policy("build.optimization.lto", true)
```

### build.distcc

Distributed compilation.

```lua
set_policy("build.distcc", true)
```

### build.distcc.remote_only

Only use remote for distributed compilation.

```lua
set_policy("build.distcc.remote_only", true)
```

### build.sanitizer.address

AddressSanitizer.

```lua
set_policy("build.sanitizer.address", true)
```

### build.sanitizer.undefined

UndefinedBehaviorSanitizer.

```lua
set_policy("build.sanitizer.undefined", true)
```

### build.sanitizer.thread

ThreadSanitizer.

```lua
set_policy("build.sanitizer.thread", true)
```

### build.sanitizer.memory

MemorySanitizer.

```lua
set_policy("build.sanitizer.memory", true)
```

## Check Policies

### check.auto_ignore_flags

Auto-detect and ignore unsupported flags (enabled by default).

```lua
set_policy("check.auto_ignore_flags", false)  -- disable, force all flags
```

### check.auto_map_flags

Auto-map flags between compilers (enabled by default).

```lua
set_policy("check.auto_map_flags", false)  -- disable flag mapping
```

### check.target_package_licenses

License compatibility check (enabled by default).

```lua
set_policy("check.target_package_licenses", false)  -- disable check
```

## Package Policies

### package.precompiled

Use precompiled packages.

```lua
set_policy("package.precompiled", true)
```

### package.fetch_only

Only fetch packages, don't build.

```lua
set_policy("package.fetch_only", true)
```

### package.install_only

Only install, don't link.

```lua
set_policy("package.install_only", true)
```

### package.system

Prefer system packages.

```lua
set_policy("package.system", true)
```

### package.strict

Strict dependency version compatibility.

```lua
set_policy("package.strict", true)
```

### package.fetch_precompiled

Fetch precompiled binaries.

```lua
set_policy("package.fetch_precompiled", false)
```

## Run Policies

### run.autobuild

Auto-build dependencies before running.

```lua
set_policy("run.autobuild", false)
```

## Preprocessor Policies

### preprocessor.uniquify

Uniquify macro expansions.

```lua
set_policy("preprocessor.uniquify", true)
```

## Network Policies

### network.connect_timeout

Set connection timeout.

```lua
set_policy("network.connect_timeout", 30)
```
