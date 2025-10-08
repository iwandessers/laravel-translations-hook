# Laravel Translations Hook

A Git pre-commit hook that automatically syncs translation keys across all language files in your Laravel project using Claude Code.

## Installation

### Option 1: Using pre-commit framework (Recommended)

1. Install [pre-commit](https://pre-commit.com/):

```bash
pip install pre-commit
# or
brew install pre-commit
```

2. Add to your `.pre-commit-config.yaml`:

```yaml
repos:
  - repo: https://github.com/iwandessers/laravel-translations-hook
    rev: v1.0.0  # Use the latest release tag
    hooks:
      - id: laravel-translations
```

3. Install the hook:

```bash
pre-commit install
```

4. Install Claude Code CLI (if not already installed):

```bash
npm install -g @anthropic-ai/claude-code
```

### Option 2: Manual installation

1. Copy the hook script to your Laravel project:

```bash
# Navigate to your Laravel project
cd your-laravel-project

# Copy the translate-all-modified-keys script to git hooks directory
cp /path/to/translate-all-modified-keys .git/hooks/pre-commit
```

2. Make it executable:

```bash
chmod +x .git/hooks/pre-commit
```

3. Install Claude Code CLI (if not already installed):

```bash
npm install -g @anthropic-ai/claude-code
```

## Compatibility

- **macOS**: Fully supported (BSD sed compatible)
- **Linux**: Fully supported (not tested!)
- **Windows**: Use Git Bash or WSL (not tested!)

## How It Works

The hook performs these steps:

1. **Detects Translation Changes**: Checks if any staged files are in the `lang/` or `resources/lang/` directories
2. **Lists Affected Languages**: Identifies all language directories in your project
3. **Calls Claude Code**: Sends a detailed prompt asking Claude to:
   - Analyze the git diff for translation changes
   - Compare all language files for consistency
   - Add missing translation keys to other languages
   - Automatically translate English values to the target language
4. **Auto-stages Changes**: Automatically stages the synced translation files
5. **Proceeds with Commit**: Allows the commit to continue if sync is successful

## Customization Options

### Permission prompts
By default, the hook uses `--dangerously-skip-permissions` to avoid blocking on permission prompts during the commit. Remove this flag if you want to review file operations:

```bash
# Change this line in the script (line 137):
claude --dangerously-skip-permissions -p "$PROMPT"

# To this:
claude -p "$PROMPT"
```

### Skip the hook
If you need to commit without running the hook:

```bash
git commit --no-verify
```

## Example Workflow

When you modify `lang/en/messages.php` and commit:

1. Hook detects the change
2. Claude Code analyzes the diff
3. Syncs the new keys to `lang/es/messages.php`, `lang/fr/messages.php`, etc.
4. Automatically translates the values to Spanish, French, etc.
5. Auto-stages and commits everything together
