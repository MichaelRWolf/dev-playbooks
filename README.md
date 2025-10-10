# dev-playbooks

Reusable playbooks, recipes, and setup guides for development workflows across multiple projects.

## Purpose

This repository contains step-by-step guides and configuration templates for:

- Git workflows and hooks
- CI/CD pipelines
- Coding standards and linters
- Development tool setup
- Best practices and patterns

These playbooks are designed to be copied and adapted for use across different repositories.

## Setup

After cloning this repository, install the pre-commit hooks:

```bash
# Install pre-commit framework (if not already installed)
pip install pre-commit

# Install the git hooks
pre-commit install

# (Optional) Test the hooks on all files
pre-commit run --all-files
```

The pre-commit hooks will automatically:

- Fix markdown formatting issues using markdownlint

Config file location search order:

1. `.markdownlint.json` in project (version controlled, everyone uses same rules)
2. `~/.markdownlint.json` (fallback, not used if project config exists)
3. Markdownlint defaults (fallback)

## Structure

- `git/` - Git hooks, workflows, and configuration guides
- `ci/` - CI/CD pipeline templates and setup guides
- `coding-standards/` - Code formatting, linting, and style guides

## Usage

Each playbook includes:

- AI assistant prompts (copy/paste into Cursor or other AI tools)
- Manual step-by-step instructions
- Configuration file examples
- Troubleshooting guides

## License

MIT License - See [LICENSE](LICENSE) file for details.
