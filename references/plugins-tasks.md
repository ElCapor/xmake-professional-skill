# Plugins and Tasks

Xmake supports custom tasks and plugins for extending functionality.

## Tasks

### Defining a Task

```lua
task("hello")
    on_run(function ()
        print("Hello xmake!")
    end)
```

### Running a Task

Tasks can be called from other tasks or target scripts:

```lua
target("myapp")
    after_build(function (target)
        import("core.project.task")
        task.run("hello")
    end)
```

### Task with Menu

Make a task executable from command line:

```lua
task("echo")
    on_run(function ()
        import("core.base.option")
        cprint("${bright}%s", option.get("message"))
    end)
    set_menu {
        usage = "xmake echo [options]",
        description = "Echo the given message!",
        options = {
            {'m', "message", "kv", nil, "Set message to echo."}
        }
    }
```

Run with:

```bash
xmake echo -m "Hello World"
```

## Task Menu Options

### Boolean Option

```lua
{'v', "verbose", "k", nil, "Enable verbose output."}
```

- First char: short option
- Second: long option
- `k`: key-only boolean
- `nil`: default value

### Key-Value Option

```lua
{'o', "output", "kv", "a.out", "Set output file."}
```

### Choice Option

```lua
{'m', "mode", "kv", "debug", "Set mode.", "debug", "release"}
```

## Builtin Plugins

### Project Generation

```bash
# Generate Makefile
xmake project -k makefile

# Generate CMakeLists.txt
xmake project -k cmakelists

# Generate Ninja build file
xmake project -k ninja

# Generate Xcode project
xmake project -k xcode

# Generate Visual Studio project
xmake project -k vsxmake
xmake project -k vsxmake2019
xmake project -k vsxmake2022
```

### Compilation Database

```bash
# Generate compile_commands.json
xmake project -k compile_commands
```

### Auto-update VS Project

```lua
add_rules("plugin.vsxmake.autoupdate")
target("test")
    add_files("src/*.cpp")
```

## Custom Plugins

### Creating a Plugin

```lua
task("myplugin")
    on_run(function ()
        import("core.base.option")
        
        local verbose = option.get("verbose")
        local output = option.get("output")
        
        print("Running my plugin!")
    end)
    set_menu {
        usage = "xmake myplugin [options]",
        description = "My custom plugin",
        options = {
            {'v', "verbose", "k", nil, "Verbose output."},
            {'o', "output", "kv", "output.txt", "Output file."}
        }
    }
task_end()
```

### Running Plugin

```bash
xmake myplugin -v -o result.txt
```

## Task Dependencies

### Run After Another Task

```lua
task("task2")
    after("task1")
    on_run(function ()
        print("Task 2 runs after Task 1")
    end)
```

### Run Before Another Task

```lua
task("task1")
    before("task2")
    on_run(function ()
        print("Task 1 runs before Task 2")
    end)
```

## Macro Tasks

### Creating a Macro

```lua
task("build_release")
    on_run(function ()
        xmake("f", "-m", "release")
        xmake()
    end)
task_end()
```

### Running Macro

```bash
xmake m build_release
```

## Event Tasks

### On Config Event

```lua
task("on_config_task")
    on_config(function ()
        print("Configuration complete!")
    end)
```

### On Build Events

```lua
task("build_task")
    before_build(function ()
        print("Before build!")
    end)
    after_build(function ()
        print("After build!")
    end)
```

## Examples

### Complete Plugin Example

```lua
-- Define the plugin
task("package")
    on_run(function ()
        import("core.base.option")
        import("core.project.project")
        
        local target_name = option.get("target")
        local output_dir = option.get("output") or "."
        
        print("Packaging target: %s", target_name)
        
        -- Package logic here
    end)
    
    set_menu {
        usage = "xmake package [options]",
        description = "Package the target for distribution",
        options = {
            {'t', "target", "kv", nil, "Target name to package."},
            {'o', "output", "kv", ".", "Output directory."},
            {'v', "verbose", "k", nil, "Verbose output."}
        }
    }
task_end()
```

### Usage

```bash
xmake package -t myapp -o ./dist -v
```

## Best Practices

1. Use meaningful task names
2. Document menu options clearly
3. Use `task_end()` to close task definitions
4. Import modules inside `on_run` for better performance
5. Use event hooks for build integration
