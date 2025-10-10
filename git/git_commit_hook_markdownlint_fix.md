# Setup Guide: Markdownlint Pre-Commit Hook

Use this prompt with an AI assistant or follow manually to set up markdownlint-fix pre-commit hooks in any repository.

---

## Prompt for AI Assistant

```text
Set up markdownlint-fix pre-commit hook in this repository:

1. Install pre-commit framework if not already installed:
   - Check if pre-commit is installed
   - If not, provide command: pip install pre-commit

2. Check for existing changes and stash if needed:
   - Run: git status --short
   - If there are any changes (modified, staged, or untracked):
     - Run: git stash push -u -m "Stashing before markdownlint setup"
     - Inform user that changes were stashed
     - User can restore with: git stash pop
   - If no changes, continue without stashing
   - This isolates markdownlint setup changes from development work

3. Create or update .pre-commit-config.yaml with:
   - repo: https://github.com/igorshubovych/markdownlint-cli
   - rev: v0.39.0 (or latest)
   - hook id: markdownlint-fix

4. Create .markdownlint.json in project root with ONLY this content:
   {
     "MD013": false
   }
   
   CRITICAL: Do NOT add any other rules to suppress errors!
   - MD013 is disabled because it can't be auto-fixed (line length)
   - ALL other markdownlint errors MUST cause the commit to FAIL
   - User will manually fix any remaining errors
   - NEVER hide or suppress errors by adding config rules

5. Install the git hooks:
   - Run: pre-commit install

6. Ask the user what they want to do:
   - "Run hook on staged files only?"
   - "Run hook on all files?"
   - "Skip running hook for now?"
   - Based on their choice:
     - Staged: pre-commit run
     - All files: pre-commit run --all-files
     - Skip: do nothing
   - If it FAILS with errors, that's EXPECTED
   - Show the errors to the user
   - User will fix them manually
   - Do NOT add config rules to suppress the errors

7. Update README.md in project root:
   - Add "Setup" section if not present
   - Document pre-commit installation steps
   - Document what the hooks do (auto-fix markdown)
   - Explain that commits will FAIL if unfixable errors exist
   - Document config file search order:
     1. .markdownlint.json in project (version controlled)
     2. ~/.markdownlint.json (fallback)
     3. Markdownlint defaults (fallback)

8. Show git status to display modified files
   - User will decide what/when to commit
   - Do NOT stage or commit files automatically
```

---

## Manual Steps

### 1. Install Pre-Commit Framework

```bash
# Check if already installed
pre-commit --version

# Install if needed
pip install pre-commit
```

### 2. Check for Existing Changes and Stash if Needed

```bash
# Check if there are any changes
git status --short

# If there are changes (modified, staged, or untracked), stash them
git stash push -u -m "Stashing before markdownlint setup"

# Note: You can restore later with: git stash pop
```

This isolates the markdownlint setup changes from any existing development work.

If there are no changes, skip the stash and continue to the next step.

### 3. Create .pre-commit-config.yaml

Create file in project root:

```yaml
repos:
  - repo: https://github.com/igorshubovych/markdownlint-cli
    rev: v0.39.0
    hooks:
      - id: markdownlint-fix
```

### 4. Create .markdownlint.json

Create file in project root with **ONLY** this content:

```json
{
  "MD013": false
}
```

**CRITICAL:** This config should ONLY contain MD013 disabled.

- MD013 (line-length) is disabled because it can't be auto-fixed
- **DO NOT add other rules to suppress errors**
- All other markdownlint errors should cause commits to **FAIL**
- You must manually fix remaining errors - don't hide them

### 5. Install Git Hooks

```bash
pre-commit install
```

This creates `.git/hooks/pre-commit` that runs automatically on every commit.

### 6. Test the Setup (Optional)

Choose one based on your needs:

```bash
# Option 1: Run on staged files only
pre-commit run

# Option 2: Run on all files in repo
pre-commit run --all-files

# Option 3: Skip testing (hook will run automatically on next commit)
# Just proceed without running the hook now
```

If the hook fails with errors, **that's expected and good** - fix the issues manually.

### 7. Update README.md

Add this section to your project's README.md:

```markdown
## Setup

After cloning this repository, install the pre-commit hooks:

\```bash
# Install pre-commit framework (if not already installed)
pip install pre-commit

# Install the git hooks
pre-commit install

# (Optional) Test the hooks on all files
pre-commit run --all-files
\```

The pre-commit hooks will automatically:

- Fix markdown formatting issues using markdownlint

Config file location search order:

1. `.markdownlint.json` in project (version controlled, everyone uses same rules)
2. `~/.markdownlint.json` (fallback, not used if project config exists)
3. Markdownlint defaults (fallback)

```

---

## Why This Setup?

**markdownlint-fix vs markdownlint:**

- `markdownlint-fix` auto-fixes issues (better for commit hooks)
- `markdownlint` only reports issues (better for CI checks)

**Philosophy: Fail on errors, don't hide them:**

- Commits SHOULD FAIL if there are unfixable markdown errors
- Suppressing errors by adding config rules is **EVIL**
- Only MD013 (line-length) is disabled - it can't be auto-fixed
- All other errors require manual fixes by you
- Errors are feedback, not obstacles to be hidden

**Project-level config:**

- Version controlled `.markdownlint.json` ensures consistency
- Everyone uses same rules, no "works on my machine" issues
- No dependency on individual home directory configs
- Config should ONLY contain MD013: false - nothing else

**Pre-commit framework:**

- Industry standard, widely used
- Auto-manages hook installation
- Easier than custom bash scripts
- Works for anyone who clones the repo

---

## Troubleshooting

**Hook not running:**

```bash
# Verify hook is installed
ls -la .git/hooks/pre-commit

# Reinstall if needed
pre-commit install
```

**Config not found:**

```bash
# Verify config file exists
ls -la .markdownlint.json

# Test config is being used
pre-commit run --all-files --verbose
```

**Commit fails with markdownlint errors:**

- This is EXPECTED and GOOD behavior
- Read the error messages carefully
- Fix the markdown issues manually
- DO NOT add config rules to suppress the errors
- Only MD013 should be disabled - all others must be fixed
- Run `pre-commit run --all-files` to verify your fixes

**MD013 line-length warnings:**

- These are informational only
- Can't be auto-fixed (would require text rewrapping)
- Disabled in our config to reduce noise
- This is the ONLY rule that should be disabled

---

## Related Files

- `.pre-commit-config.yaml` - Pre-commit framework configuration
- `.markdownlint.json` - Markdownlint rules configuration
- `.git/hooks/pre-commit` - Auto-generated hook (not version controlled)

---

## Document History

Last updated: 2025-10-10
