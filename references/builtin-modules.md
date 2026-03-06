# Builtin Lua Modules

Xmake provides extended builtin Lua modules for scripting.

## io Module

Extended I/O operations beyond Lua's standard library.

### io.open

```lua
local file = io.open("file.txt", "r")
local data = file:read("*all")
file:close()
```

With encoding support:

```lua
local file = io.open("file.txt", "r", {encoding = "utf8"})
local file = io.open("file.txt", "r", {encoding = "utf16le"})
local file = io.open("file.txt", "r", {encoding = "binary"})
```

### File Object Methods

```lua
local file = io.open("file.txt", "r")
local size = file:size()           -- Get file size
local path = file:path()           -- Get absolute path
local line = file:read("L")        -- Read line with newline
for line in file:lines() do       -- Iterate lines
    print(line)
end
file:close()
```

### Writing

```lua
local file = io.open("file.txt", "w")
file:write("hello")                    -- Basic write
file:writef("hello %s", "world")       -- Formatted write
file:print("hello $(buildir)")          -- Print with variables
file:printf("hello %s\n", "world")      -- Formatted print
file:close()
```

### io.readfile / io.writefile

```lua
local data = io.readfile("file.txt")
io.writefile("output.txt", "content")
```

### io.exists

```lua
if io.exists("file.txt") then
    print("File exists")
end
```

### io.mkdir / io.rmdir

```lua
io.mkdir("newdir")
io.rmdir("olddir")
```

### io.cp / io.mv

```lua
io.cp("src.txt", "dst.txt")
io.mv("old.txt", "new.txt")
```

## os Module

Operating system utilities.

### os.getenv / os.setenv

```lua
local path = os.getenv("PATH")
os.setenv("MY_VAR", "value")
```

### os.execute / os.exec

```lua
os.execute("ls -la")
os.exec("ls -la")
```

### os.run / os.vrun

```lua
os.run("echo hello")
os.vrun("echo %s", "hello")
```

### os.iorun / os.iorunv

```lua
local output = os.iorun("ls")
local output = os.iorunv("ls", {"-la"})
```

### os.mkdir / os.rmdir

```lua
os.mkdir("dir")
os.rmdir("dir")
```

### os.cp / os.mv / os.rm

```lua
os.cp("src", "dst")
os.mv("old", "new")
os.rm("file")
```

### os.exit

```lua
os.exit(0)
os.exit(1, true)  -- with cleanup
```

### os.date / os.time

```lua
local date = os.date()
local time = os.time()
```

### os.rootdir / os.projectdir

```lua
local root = os.rootdir()
local proj = os.projectdir()
```

## path Module

Path manipulation utilities.

### path.join

```lua
local full = path.join("dir", "subdir", "file.txt")
```

### path.basename / path.extname / path.dirname

```lua
local name = path.basename("/dir/file.txt")     -- "file.txt"
local ext = path.extname("/dir/file.txt")       -- ".txt"
local dir = path.dirname("/dir/file.txt")        -- "/dir"
```

### path.is_absolute

```lua
if path.is_absolute("/home/user") then
    -- absolute path
end
```

### path.translate

```lua
local win = path.translate("/home/user")  -- on Windows: "C:\home\user"
```

## string Module

Extended string operations.

### string.iterate

```lua
for char in string.utf8("hello") do
    print(char)
end
```

### string.startswith / string.endswith

```lua
if string.startswith("hello world", "hello") then
    -- starts with
end
```

### string.join

```lua
local result = string.join({"a", "b", "c"}, ",")
```

## table Module

Extended table operations.

### table.join

```lua
local merged = table.join({1, 2}, {3, 4})
```

### table.keys / table.values

```lua
local keys = table.keys({a = 1, b = 2})
local values = table.values({a = 1, b = 2})
```

## print / printf / cprint

### print

```lua
print("Hello")
print("Value:", value)
```

### printf

```lua
printf("Hello %s", "World")
```

### cprint (colored)

```lua
cprint("%{red}Error%{reset}: %s", "message")
```

## format / vformat

### format

```lua
local s = format("Hello %s", "World")
```

### vformat

```lua
local s = vformat("Hello $(var)")
```

## import Module

Import external modules.

```lua
import("core.base.task")
import("async.runjobs")
import("package.tools.cmake")
```

## coroutine Module

Coroutine support (built into Lua).

```lua
local co = coroutine.create(function()
    print("in coroutine")
end)
coroutine.resume(co)
```

## hash Module

Hashing utilities.

### hash.md5 / hash.sha1 / hash.sha256

```lua
local md5 = hash.md5("data")
local sha256 = hash.sha256("data")
```

## raise Module

Exception handling.

```lua
raise("Error message")
```

## try-catch-finally

```lua
try(function()
    -- try block
catch(function(e)
    -- catch block
finally(function()
    -- finally block
end)
```

## ipairs / pairs

Standard Lua iteration.

```lua
for i, v in ipairs(arr) do end
for k, v in pairs(table) do end
```

## inherit Module

Inheritance utilities.

```lua
local subclass = inherit(superclass)
```
