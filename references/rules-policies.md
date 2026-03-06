# Custom Rules and Policies

Xmake allows creating custom build rules for handling specialized file types and build scenarios.

## Creating Custom Rules

### Basic Syntax

```lua
rule("rulename")
    set_extensions(".ext1", ".ext2")
    on_build_file(function (target, sourcefile, opt)
        -- build logic
    end)
```

### Example: Markdown to HTML

```lua
rule("markdown")
    set_extensions(".md", ".markdown")
    on_build_file(function (target, sourcefile, opt)
        import("core.project.depend")
        
        os.mkdir(target:targetdir())
        
        local targetfile = path.join(target:targetdir(), path.basename(sourcefile) .. ".html")
        
        depend.on_changed(function ()
            os.vrunv('pandoc', {"-s", "-f", "markdown", "-t", "html", "-o", targetfile, sourcefile})
        end, {files = sourcefile})
    end)

target("test")
    set_kind("object")
    add_rules("markdown")
    add_files("src/*.md")
```

## Applying Rules

### Method 1: Using add_rules()

```lua
target("test")
    set_kind("binary")
    add_rules("markdown")
    add_files("src/*.md")
```

### Method 2: Specifying in add_files

```lua
target("test")
    set_kind("binary")
    add_files("src/*.md", {rules = "markdown"})
```

Rules in `add_files` have higher priority than `add_rules()`.

## Rule Lifecycle

Rules support the complete build lifecycle:

- **on_load**: When rule is loaded
- **on_config**: After configuration
- **before_build**: Before building
- **on_build**: During build
- **after_build**: After build

## Built-in Rules

### Mode Rules

```lua
add_rules("mode.debug", "mode.release")
add_rules("mode.profile")
add_rules("mode.check")
```

### WDK Rules

See `references/wdk-development.md`

### Qt Rules

```lua
add_rules("qt.widgetapp")
add_rules("qt.quickapp")
```

### Android Rules

```lua
add_rules("android.native_app")
```

## Policies

### Setting Policies

```lua
add_policies("policy.name", value)
```

### Common Policies

```lua
-- Build policy
add_policies("build.cache", true)

-- Package policy
add_policies("package.install_only", true)
```

## Pre/Post Build Commands

### Using Target Hooks

```lua
target("myapp")
    before_build(function (target)
        print("Before building")
    end)
    after_build(function (target)
        print("After building")
        os.cp("output.bin", "/dest/")
    end)
```

### Using on_build

```lua
target("myapp")
    on_build(function (target)
        -- custom build commands
    end)
```

## Best Practices

1. Use custom rules for non-standard file types
2. Use `depend.on_changed()` for incremental builds
3. Use built-in rules when available (wdk, qt, android)
4. Use policies for global build behavior settings
5. Use before/after hooks for pre/post build operations
