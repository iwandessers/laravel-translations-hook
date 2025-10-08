# Laravel Translations Hook

A Git pre-commit hook that automatically syncs translation keys across all language files in your Laravel project using Claude Code.

## Installation Steps

### 1. Save the hook file

```bash
# Navigate to your Laravel project
cd your-laravel-project

# Create or edit the pre-commit hook
vi .git/hooks/pre-commit
```

Paste the script from the artifact above.

### 2. Make it executable

```bash
chmod +x .git/hooks/pre-commit
```

### 3. Install Claude Code CLI (if not already installed)

```bash
npm install -g @anthropic-ai/claude-code
```

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

### Manual approval mode
Remove the `--yes` flag from the claude command if you want to review changes before they're applied:

```bash
claude code "$PROMPT"  # Remove --yes for manual approval
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
