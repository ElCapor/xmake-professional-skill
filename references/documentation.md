# Documentation Generation

Xmake provides built-in support for generating documentation using Doxygen, Pandoc, and custom documentation rules.

## Doxygen Integration

### Basic Usage

First, ensure Doxygen is installed:

```bash
# Linux
sudo apt install doxygen

# macOS
brew install doxygen

# Windows
choco install doxygen
```

Then run:

```bash
xmake doxygen
```

### Output Directory

Specify custom output directory:

```bash
xmake doxygen -o /tmp/output project/src
```

### Configuration

Xmake automatically detects Doxyfile in the project root. You can also specify:

```bash
xmake doxygen Doxyfile.custom
```

---

## Pandoc Integration

### Using Custom Rules

Create a rule to convert Markdown to HTML:

```lua
rule("markdown")
    set_extensions(".md", ".markdown")
    on_build_file(function (target, sourcefile, opt)
        local output = target:targetdir() .. "/" .. path.basename(sourcefile) .. ".html"
        os.vrunv('pandoc', {
            "-s", 
            "-f", "markdown", 
            "-t", "html", 
            "-o", output, 
            sourcefile
        })
    end)

target("docs")
    set_kind("object")
    add_rules("markdown")
    add_files("docs/*.md")
```

### Pandoc Options

Common Pandoc options:

| Option | Description |
|--------|------------|
| `-s` | Standalone document |
| `-f format` | Input format |
| `-t format` | Output format |
| `-o file` | Output file |
| `--toc` | Generate table of contents |
| `--css=style.css` | Custom CSS |

### Complete Example

```lua
rule("pandoc.html")
    set_extensions(".md")
    on_build_file(function (target, sourcefile, opt)
        import("core.project.depend")
        
        local output = path.join(target:targetdir(), path.basename(sourcefile) .. ".html")
        
        depend.on_changed(function ()
            os.vrunv('pandoc', {
                "-s",
                "--toc",
                "-c", "style.css",
                "-f", "markdown",
                "-t", "html",
                "-o", output,
                sourcefile
            })
        end, {files = sourcefile})
    end)

rule("pandoc.pdf")
    set_extensions(".md")
    on_build_file(function (target, sourcefile, opt)
        local output = path.join(target:targetdir(), path.basename(sourcefile) .. ".pdf")
        os.vrunv('pandoc', {
            "-s",
            "-o", output,
            sourcefile
        })
    end)

target("docs")
    set_kind("object")
    add_files("docs/*.md", {rules = "pandoc.html"})
```

---

## Sphinx Documentation

### Using with Make

```lua
task("docs")
    on_run(function()
        os.cd("docs")
        os.execute("sphinx-build -b html . _build/html")
    end)
task_end()
```

### Using Custom Rule

```lua
rule("sphinx")
    on_build_files(function (target, batch, opt)
        os.cd("docs")
        os.execute("sphinx-build -b html . build/html")
    end)

target("docs")
    add_rules("sphinx")
```

---

## Custom Documentation Generation

### Complete Documentation Target

```lua
-- Documentation directory
local DOC_DIR = "docs"

-- Generate HTML from Markdown
rule("doc.markdown")
    set_extensions(".md")
    on_build_file(function(target, sourcefile, opt)
        import("core.project.depend")
        
        local output = path.join("build/docs", path.basename(sourcefile) .. ".html")
        
        depend.on_changed(function()
            os.vrunv('pandoc', {
                "-s",
                "-f", "markdown",
                "-t", "html",
                "--css=doc.css",
                "-o", output,
                sourcefile
            })
        end, {files = sourcefile})
    end)

-- Generate API documentation from headers
rule("doc.api")
    on_build_files(function(target, batch, opt)
        os.execute("doxygen Doxyfile.api")
    end)

target("documentation")
    set_kind("phony")
    add_rules("doc.markdown")
    add_files("docs/*.md")
    add_deps("api_docs")
    after_build(function(target)
        os.cp("build/docs/*.html", "dist/docs/")
    end)

target("api_docs")
    set_kind("phony")
    add_rules("doc.api")
```

---

## Documentation Installation

### Installing Docs with Project

```lua
target("myapp")
    set_kind("binary")
    add_files("src/*.cpp")
    add_install_files("docs/html", "docs")

-- Or as separate target
target("docs")
    set_kind("phony")
    add_install_files("build/docs", "docs", {prefixdir = ""})
```

---

## CI/CD Documentation

### GitHub Actions for Docs

```yaml
name: Documentation

on:
  push:
    branches: [main]
    paths: ['docs/**']

jobs:
  docs:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      - name: Install xmake
        uses: xmake-io/setup-xmake@v1
      
      - name: Generate Docs
        run: xmake doxygen
      
      - name: Deploy
        uses: actions/upload-artifact@v3
        with:
          name: documentation
          path: docs/html/
```

---

## Best Practices

### 1. Separate Documentation Target

```lua
-- Don't mix docs with main targets
target("myapp")
    set_kind("binary")
    add_files("src/*.cpp")

target("docs")
    set_kind("phony")
    set_default(false)
    -- doc generation
```

### 2. Use Dependencies

```lua
target("myapp")
    add_deps("docs")  -- Build docs before app if needed
```

### 3. Incremental Generation

```lua
rule("doc")
    on_build_file(function(target, sourcefile, opt)
        import("core.project.depend")
        
        local output = path.join(target:targetdir(), path.basename(sourcefile) .. ".html")
        
        depend.on_changed(function()
            -- Generate only if changed
            os.execute("pandoc " .. sourcefile .. " -o " .. output)
        end, {files = sourcefile})
    end)
```

### 4. Multiple Output Formats

```lua
rule("doc.html")
    set_extensions(".md")
    on_build_file(function(target, sourcefile)
        os.vrunv('pandoc', {"-s", "-t", "html", "-o", 
            path.join(target:targetdir(), path.basename(sourcefile) .. ".html"),
            sourcefile})
    end)

rule("doc.pdf")
    set_extensions(".md") 
    on_build_file(function(target, sourcefile)
        os.vrunv('pandoc', {"-s", "-o",
            path.join(target:targetdir(), path.basename(sourcefile) .. ".pdf"),
            sourcefile})
    end)

target("docs-html")
    set_kind("phony")
    add_files("docs/*.md", {rules = "doc.html"})

target("docs-pdf")
    set_kind("phony")
    add_files("docs/*.md", {rules = "doc.pdf"})
```

---

## Task-Based Documentation

### Generate Docs Task

```lua
task("docs")
    set_menu {
        usage = "xmake docs [options]",
        description = "Generate documentation",
        options = {
            {'o', "output", "kv", "docs/build", "Output directory."},
            {'f', "format", "kv", "html", "Output format (html, pdf, tex)."}
        }
    }
    on_run(function()
        import("core.base.option")
        
        local output = option.get("output")
        local format = option.get("format")
        
        if format == "html" then
            os.execute("pandoc docs/*.md -s -t html -o " .. output .. "/docs.html")
        elseif format == "pdf" then
            os.execute("pandoc docs/*.md -s -o " .. output .. "/docs.pdf")
        end
    end)
task_end()
```

Run with:

```bash
xmake docs -o ./dist -f html
```
