# Xmake Professional Skill

A comprehensive skill for Claude Code, OpenCode, and other AI assistants that teaches them how to use the xmake build system professionally.

## What is this?

This skill enables AI agents to understand and use xmake for building C/C++ projects. It covers:

- **Basic to advanced xmake usage** - From simple projects to complex multi-platform builds
- **Package management** - Using xmake's built-in package system
- **Cross-compilation** - Android NDK, iOS, WebAssembly, Linux, Windows
- **Windows Driver Kit (WDK)** - Driver development
- **Custom rules and policies** - Tailoring the build process
- **Testing** - Unit test integration
- **CI/CD** - GitHub Actions, GitLab CI, and more
- **Multi-language support** - CUDA, Rust, Swift, Go, and other languages
- **IDE integration** - VSCode, CLion, Vim, and more

## Installation

### Claude Code

```bash
claude auth
claude skill install https://github.com/ElCapor/xmake-professional-skill
```

Or manually clone and link:

```bash
git clone https://github.com/ElCapor/xmake-professional-skill ~/.claude/skills/xmake-pro
```

### OpenCode

```bash
opencode skill install https://github.com/ElCapor/xmake-professional-skill
```

Or manually:

```bash
git clone https://github.com/ElCapor/xmake-professional-skill ~/.opencode/skills/xmake-pro
```

### Generic Skill System

Clone the repository to your skills directory:

```bash
git clone https://github.com/ElCapor/xmake-professional-skill /path/to/your/skills/xmake-pro
```

## Usage

Once installed, simply mention xmake or ask about building C/C++ projects:

- "How do I configure xmake for Android?"
- "Build my project with xmake"
- "Add OpenSSL as a dependency with xmake"
- "Set up CI/CD for my xmake project"

The skill will automatically provide expert guidance based on the documentation.

## Documentation Structure

```
xmake-pro/
├── SKILL.md                    # Main skill entry point
└── references/                  # Detailed reference guides
    ├── package-management.md   # Dependencies
    ├── cross-compilation.md   # Cross-platform builds
    ├── android-development.md # Android NDK
    ├── wdk-development.md    # Windows drivers
    ├── wasm-development.md   # WebAssembly
    ├── testing.md            # Unit tests
    ├── ci-cd.md              # CI/CD pipelines
    ├── target-apis.md        # Target API reference
    ├── builtin-rules.md       # Builtin rules
    ├── builtin-policies.md    # Build policies
    ├── builtin-variables.md  # Variables
    ├── builtin-modules.md    # Lua modules
    └── ...                   # And more!
```

## Version

- **xmake**: 3.0.7+
- **Skill Version**: 1.0.0

## License

MIT License - Feel free to use and contribute!

## Links

- [xmake Official Documentation](https://xmake.io)
- [xmake GitHub](https://github.com/xmake-io/xmake)
- [Report Issues](https://github.com/ElCapor/xmake-professional-skill/issues)
