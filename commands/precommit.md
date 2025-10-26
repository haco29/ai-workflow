# Enhanced Precommit Code Check

## Overview

Comprehensive pre-commit workflow that runs all quality checks automatically. This enhanced version includes traditional CI checks plus additional compliance validations using inline scripts that leverage existing cursor rules.

## What it does

### Core Quality Checks (via `mise ci`)

- **TypeScript Compilation** - `pnpm type-check`
- **ESLint Code Quality** - `pnpm lint`
- **Prettier Formatting** - `pnpm format:check`
- **Test Suite** - `pnpm test`

### Enhanced Checks (via `mise run precommit-enhanced`)

- **React Development Patterns** - Validates compliance with cursor rules for React/TypeScript best practices
- **Smart Commit Message Generation** - Analyzes staged changes and generates a comprehensive commit message

## Usage

Choose the appropriate command based on your needs:

### Basic Quality Checks

```bash
mise run ci
```

Runs traditional quality checks (TypeScript, ESLint, Prettier, Tests).

### Enhanced Quality Checks

```bash
mise run precommit-enhanced
```

Runs all quality checks plus React development pattern compliance, and generates a smart commit message based on your staged changes.

### Individual Checks

```bash
mise run type-check       # TypeScript compilation
mise run lint            # ESLint checks
mise run format:check    # Prettier validation
mise run test            # Test suite
mise run pattern-check   # React patterns compliance
```

## Why Inline Scripts?

Instead of creating separate script files, this approach uses **inline bash scripts** because:

- **Simplicity**: No additional files to maintain
- **Self-contained**: Everything is defined in `mise.toml`
- **Cursor Rules Integration**: Leverages existing `.cursor/rules/react-development.mdc` patterns
- **Git Integration**: Uses `git diff --cached` for accurate staged changes detection
- **No Dependencies**: Uses only bash commands

## Implementation Benefits

- **Pattern Validation**: References cursor rules for React/TypeScript best practices
- **Fail-fast**: Stops on first error to provide immediate feedback
- **Cross-platform**: Works on any system with bash

## Expected Behavior

### Pattern Check Logic

The pattern check will:

- ✅ **Pass** when `.cursor/rules/react-development.mdc` exists
- ⚠️ **Warn** when the patterns file is not found

This validates that React development standards are available for reference.

### Commit Message Generation

The enhanced precommit command will:

- ✅ **Analyze staged changes** using `git diff --cached --stat` and `git diff --cached --name-only`
- ✅ **Detect change patterns** (new files, modifications, deletions, test updates)
- ✅ **Generate semantic commit messages** following conventional commit format
- ✅ **Include impact summaries** with file counts, component changes, and quality metrics
- ✅ **Provide copy-ready output** that you can use directly with `git commit -m`

The generated commit message will include:

- **Semantic prefix** (feat, fix, refactor, test, docs, etc.)
- **Concise description** summarizing the main changes
- **Detailed body** with bullet points of specific improvements
- **Impact metrics** showing files changed, tests updated, etc.
- **Quality confirmation** that all checks passed

## Available Pattern Checks

When the React development patterns file is found, the following checks are available:

- **Component naming conventions** - PascalCase for components, camelCase for utilities
- **Props interface definitions** - Proper TypeScript interfaces for component props
- **State management patterns** - React Query for server state, useState for local state
- **Accessibility standards (WCAG 2.1 AA)** - Semantic HTML, ARIA attributes, keyboard navigation
- **Performance optimization patterns** - Memoization, component separation, lazy loading
- **E2E testing data-testid patterns** - Consistent test ID naming for reliable testing
- **TypeScript best practices** - Generic components, proper type definitions
- **Error handling patterns** - Error boundaries, accessible error messages

## Testing the Implementation

You can test the implementation with these commands:

```bash
# Test individual components
mise run type-check
mise run lint
mise run format:check
mise run test
mise run pattern-check

# Test complete workflows
mise run ci                    # Basic quality checks
mise run precommit-enhanced   # Enhanced quality checks
```

All checks should pass without errors, and the pattern check should display the available pattern validations when the cursor rules file is present.

## Example Commit Message Output

When you run `mise run precommit-enhanced`, you'll get a generated commit message like this:

```
🎯 SUGGESTED COMMIT MESSAGE:
=====================================

feat: enhance trackables page UX with improved search and test coverage

✨ Key improvements:
- Replace basic input with SearchInput component (dark variant, debouncing)
- Add comprehensive test IDs for E2E testing automation
- Remove duplicate headers for cleaner visual hierarchy
- Update component tests for new structure

🔧 Files changed:
- 5 component files modified
- 1 test file updated
- 2 new documentation files

📊 Quality metrics:
- All precommit checks ✅ (TypeScript, ESLint, Prettier, Tests)
- 410 tests passing
- Zero linting/type errors

=====================================

Copy the above message and use: git commit -m "..."
```

This gives you a professional, comprehensive commit message that you can copy and paste directly into your `git commit` command.
