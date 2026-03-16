# template

> Replace this with your project description.

## Development Environment Setup

### Prerequisites

- macOS or Linux
- [direnv](https://direnv.net/) — loads `.env` into your shell automatically
- [Node.js](https://nodejs.org/) — required for `markdownlint-cli2` pre-commit hook

### 1. Install uv

[uv](https://docs.astral.sh/uv/) is the package and project manager for this project.

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

Or via the VS Code task: **Terminal → Run Task → Install uv**.

### 2. Install dependencies

```bash
uv sync --all-extras
```

This creates a `.venv` virtualenv and installs all dependencies including dev tools.
Run via VS Code task: **Terminal → Run Task → uv sync (all extras)**.

### 3. Allow direnv

```bash
direnv allow
```

This enables automatic loading of `.env` whenever you `cd` into the project directory.
Run via VS Code task: **Terminal → Run Task → direnv allow**.

### 4. Register pre-commit hooks

```bash
uv run pre-commit install
```

This installs git hooks that run automatically on every `git commit`.
Run via VS Code task: **Terminal → Run Task → pre-commit install**.

### One-shot setup

Run all setup steps except `uv` installation in sequence via
**Terminal → Run Task → Setup dev environment**.

### 5. VS Code extensions

When you open this project in VS Code it will automatically prompt you to install
the recommended extensions defined in `.vscode/extensions.json`.

To install them all from the CLI without the prompt:

```bash
cat .vscode/extensions.json \
  | grep '"' | grep -v '//' | grep -v '{' | grep -v '}' \
  | tr -d '",' | xargs -I{} code --install-extension {}
```

---

## Pre-commit hooks

| Hook | What it does |
| --- | --- |
| `gitleaks` | Scans for secrets / credentials accidentally committed |
| `ruff-check` | Lints Python code and auto-fixes safe issues |
| `ruff-format` | Formats Python code |
| `basedpyright` | Type-checks Python code |
| `markdownlint-cli2` | Lints Markdown files |

To run all hooks manually against all files:

```bash
uv run pre-commit run --all-files
```

## Running tests

```bash
uv run pytest
```

## Linting and formatting

```bash
uv run poe ruff          # format + lint Python
uv run poe markdownlint  # lint Markdown via npx markdownlint-cli2
uv run poe pymarkdown    # lint Markdown via pymarkdown
```
