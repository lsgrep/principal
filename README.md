# Principal

Automated codebase improvement framework using AI-powered code analysis. Principal acts as your automated "principal engineer," making focused, atomic improvements to your codebase through iterative AI-driven analysis.

## Overview

Principal provides shell-based orchestration scripts that leverage AI models (Claude Code or Codex) to systematically improve codebases. Each iteration makes ONE focused improvement following a strict priority hierarchy:

**Security > Correctness > Performance > Maintainability**

## Features

- **Atomic Improvements**: One focused change per iteration with clear commit history
- **Smart Stalling Detection**: Automatically stops when no more improvements can be made
- **Focus Areas**: Target specific aspects of your codebase (e.g., "error handling", "type safety")
- **Git Integration**: Automatic commits with detailed context
- **Two AI Backends**: Support for both Claude Code and Codex

## Requirements

- Bash (POSIX-compliant shell)
- Git
- One of:
  - [Claude Code CLI](https://github.com/anthropics/claude-code) (`claude`)
  - [Codex CLI](https://github.com/openai/codex) (`codex`)

## Installation

```bash
# Clone the repository
git clone https://github.com/lsgrep/principal.git

# Make scripts executable
chmod +x principal/principal-claude-code
chmod +x principal/principal-codex

# Optionally, add to PATH
export PATH="$PATH:$(pwd)/principal"
```

## Usage

```bash
# Basic usage (5 iterations, general improvements)
./principal-claude-code

# Focus on a specific area
./principal-claude-code "error handling"

# Custom iteration count
./principal-claude-code -n 10 "type safety"

# Verbose output
./principal-claude-code -v "security"

# Dry run (preview without execution)
./principal-claude-code -d "performance"

# Add custom context to the prompt
./principal-claude-code -p "Prioritize immutability" "refactoring"

# Using Codex instead of Claude Code
./principal-codex "documentation"
```

## Options

| Flag | Long | Description | Default |
|------|------|-------------|---------|
| `-n` | `--max-iters` | Maximum iterations | 5 |
| `-s` | `--stall-limit` | Stop after N no-change rounds | 3 |
| `-p` | `--prompt` | Custom prompt appendage | - |
| `-d` | `--dry-run` | Preview mode without execution | false |
| `-v` | `--verbose` | Detailed output | false |
| `-h` | `--help` | Display help | - |

## How It Works

```
1. Validate git repository
        ↓
2. Construct base prompt with rules and focus area
        ↓
3. Iterative improvement loop:
   ├── Execute AI tool with prompt
   ├── Check for file changes
   ├── If changes: create atomic commit, reset stall counter
   └── If no changes: increment stall counter
        ↓
4. Stop when max iterations reached OR stall limit exceeded
        ↓
5. Display session summary
```

### Commit Format

```
chore(FOCUS_AREA): principal #ITERATION_NUMBER

Files: file1.ts file2.ts
Iteration: 1/5
```

## Examples

### Improve Error Handling
```bash
./principal-claude-code -n 10 "error handling"
```

### Security-Focused Pass
```bash
./principal-claude-code -n 15 -s 5 "security"
```

### Performance Optimization
```bash
./principal-claude-code -v "performance" -p "Focus on reducing memory allocations"
```

### General Code Quality
```bash
./principal-claude-code -n 20 -s 3
```

## CI/CD Integration

Principal can be integrated into CI/CD pipelines for periodic code quality improvements:

```yaml
# Example GitHub Actions workflow
- name: Run Principal
  run: |
    ./principal-claude-code -n 5 "type safety"
    git push origin HEAD
```

## Built-in Rules

The AI is instructed to:

1. Make ONE focused improvement per iteration
2. Follow priority: security > correctness > performance > maintainability
3. Make reasonable decisions without asking questions
4. Prefer small changes over large refactors
5. Skip cosmetic-only changes
6. Exit early if no improvements remain

## License

MIT

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.
