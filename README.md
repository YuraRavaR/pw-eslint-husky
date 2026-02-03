# Playwright + ESLint + Husky Testing Framework

A professional end-to-end testing framework built with **Playwright**, featuring automated code quality enforcement through **ESLint**, **Prettier**, and **Husky** pre-commit hooks.

---

## 📚 Table of Contents

- [Project Overview](#project-overview)
- [Tech Stack & Libraries](#tech-stack--libraries)
- [Project Structure](#project-structure)
- [File Descriptions](#file-descriptions)
- [Getting Started](#getting-started)
- [Workflow Guide](#workflow-guide)
- [Git Hooks & Code Quality](#git-hooks--code-quality)
- [CI/CD Pipeline](#cicd-pipeline)
- [Best Practices](#best-practices)

---

## 🎯 Project Overview

This project provides a robust foundation for browser automation testing with built-in code quality tools that automatically format and lint your code before each commit. It ensures consistent code style across the team and catches potential issues early.

### Key Features

- ✅ **Playwright** for cross-browser E2E testing
- ✅ **TypeScript** for type-safe test development
- ✅ **ESLint** with Playwright-specific rules
- ✅ **Prettier** for consistent code formatting
- ✅ **Husky + lint-staged** for pre-commit automation
- ✅ **GitHub Actions** for CI/CD pipeline

---

## 📦 Tech Stack & Libraries

### Core Testing

| Library            | Version | Purpose                                           |
| ------------------ | ------- | ------------------------------------------------- |
| `@playwright/test` | ^1.58.1 | End-to-end testing framework for web applications |

### TypeScript Support

| Library             | Version | Purpose                                 |
| ------------------- | ------- | --------------------------------------- |
| `typescript`        | ^5.9.3  | Adds static typing to JavaScript        |
| `typescript-eslint` | ^8.54.0 | ESLint parser and plugin for TypeScript |
| `@types/node`       | ^25.2.0 | TypeScript definitions for Node.js      |

### Code Quality & Formatting

| Library                    | Version | Purpose                                                           |
| -------------------------- | ------- | ----------------------------------------------------------------- |
| `eslint`                   | ^9.39.2 | JavaScript/TypeScript linter for identifying problematic patterns |
| `@eslint/js`               | ^9.39.2 | ESLint's recommended JavaScript rules                             |
| `eslint-plugin-playwright` | ^2.5.1  | Playwright-specific linting rules                                 |
| `eslint-plugin-prettier`   | ^5.5.5  | Runs Prettier as an ESLint rule                                   |
| `eslint-config-prettier`   | ^10.1.8 | Disables ESLint rules that conflict with Prettier                 |
| `prettier`                 | ^3.8.1  | Opinionated code formatter                                        |
| `globals`                  | ^17.3.0 | Global identifiers for various JavaScript environments            |

### Git Hooks & Automation

| Library       | Version | Purpose                                         |
| ------------- | ------- | ----------------------------------------------- |
| `husky`       | ^9.1.7  | Git hooks manager - runs scripts before commits |
| `lint-staged` | ^16.2.7 | Runs linters only on staged (changed) files     |

### Utilities

| Library | Version | Purpose                                             |
| ------- | ------- | --------------------------------------------------- |
| `jiti`  | ^2.6.1  | Runtime TypeScript and ESM support for config files |

---

## 📁 Project Structure

```
pw-eslint-husky/
├── .github/
│   ├── workflows/
│   │   └── playwright.yml      # CI/CD pipeline configuration
│   └── pull_request_template.md # PR review checklist
├── .husky/
│   └── pre-commit              # Pre-commit hook script
├── tests/
│   └── example.spec.ts         # Example Playwright test
├── .prettierrc.json            # Prettier code formatting configuration
├── eslint.config.mts           # ESLint configuration (flat config)
├── playwright.config.ts        # Playwright test configuration
├── tsconfig.json               # TypeScript compiler options
├── package.json                # Project dependencies and scripts
├── package-lock.json           # Locked dependency versions
└── .gitignore                  # Git ignore patterns
```

---

## 📄 File Descriptions

### `package.json`

**Purpose:** Project manifest containing dependencies, scripts, and lint-staged configuration.

**Key Sections:**

- `type: "module"` - Enables ES modules (import/export syntax)
- `scripts.prepare` - Runs `husky` on `npm install` to set up git hooks
- `devDependencies` - All development tools and testing libraries
- `lint-staged` - Configuration for running tools on staged files:
  - TypeScript/JavaScript files: Format with Prettier, then fix with ESLint
  - JSON/Markdown files: Format with Prettier only

---

### `eslint.config.mts`

**Purpose:** ESLint configuration using the new flat config format.

**Configuration Breakdown:**

1. **Base Configs:**
   - `js.configs.recommended` - JavaScript best practices
   - `tseslint.configs.recommended` - TypeScript best practices
   - `prettierConfig` - Disables ESLint rules that conflict with Prettier

2. **Ignored Paths:**
   - `node_modules/`, `playwright-report/`, `test-results/`, `dist/`, `.git/`

3. **TypeScript Rules:**
   - `prettier/prettier: warn` - Shows Prettier issues as ESLint warnings
   - `no-unused-vars: warn` - Warns on unused variables (ignores `_` prefixed)
   - `no-explicit-any: warn` - Discourages `any` type usage
   - `no-non-null-assertion: warn` - Warns on `!` operator

4. **Playwright-Specific Rules (for test files):**
   - `expect-expect: warn` - Tests should contain assertions
   - `no-focused-test: error` - Prevents `.only()` from being committed
   - `no-skipped-test: warn` - Warns about skipped tests
   - `no-wait-for-timeout: warn` - Discourages hard waits
   - `prefer-web-first-assertions: warn` - Encourages `toBeVisible()` over manual checks
   - `no-networkidle: error` - Prevents unreliable networkidle waits

---

### `.prettierrc.json`

**Purpose:** Prettier code formatting configuration.

**Key Settings:**

- `printWidth: 120` - Maximum line length before wrapping
- `tabWidth: 2` - Number of spaces per indentation level
- `useTabs: false` - Use spaces instead of tabs
- `semi: true` - Add semicolons at the end of statements
- `singleQuote: true` - Use single quotes instead of double quotes
- `trailingComma: "all"` - Add trailing commas wherever possible
- `bracketSpacing: true` - Add spaces inside object braces `{ foo: bar }`
- `arrowParens: "avoid"` - Omit parentheses when possible in arrow functions `x => x`
- `endOfLine: "lf"` - Use Unix-style line endings
  **Special Overrides:**

- JSON files use 4-space indentation for better readability

This configuration ensures consistent code formatting across the entire project and integrates with ESLint through `eslint-plugin-prettier`.

---

### `playwright.config.ts`

**Purpose:** Configures Playwright test runner behavior.

**Key Options:**

| Option          | Value                     | Description                                 |
| --------------- | ------------------------- | ------------------------------------------- |
| `testDir`       | `./tests`                 | Directory containing test files             |
| `fullyParallel` | `true`                    | Run tests in parallel for faster execution  |
| `forbidOnly`    | `true` (on CI)            | Fails if `.only()` is left in code          |
| `retries`       | `2` (CI) / `0` (local)    | Retry failed tests on CI                    |
| `workers`       | `1` (CI) / `auto` (local) | Parallel workers                            |
| `reporter`      | `html`                    | Generates HTML test reports                 |
| `trace`         | `on-first-retry`          | Captures trace on first retry for debugging |

**Browser Projects:**

- Chromium (Desktop Chrome)
- Firefox (Desktop Firefox)
- WebKit (Desktop Safari)

---

### `tsconfig.json`

**Purpose:** TypeScript compiler configuration.

**Key Options:**

| Option                     | Value      | Purpose                              |
| -------------------------- | ---------- | ------------------------------------ |
| `target`                   | `ES2022`   | Compile to modern JavaScript         |
| `module`                   | `NodeNext` | Use Node.js module resolution        |
| `strict`                   | `true`     | Enable all strict type checking      |
| `noUncheckedIndexedAccess` | `true`     | Adds `undefined` to index signatures |
| `verbatimModuleSyntax`     | `true`     | Enforces explicit `type` imports     |
| `resolveJsonModule`        | `true`     | Allows importing JSON files          |

---

### `tests/example.spec.ts`

**Purpose:** Sample Playwright test demonstrating basic testing patterns.

**Tests Included:**

1. **`has title`** - Navigates to playwright.dev and verifies page title
2. **`get started link`** - Clicks navigation link and verifies heading

**Key Patterns Used:**

- `test()` - Test function wrapper
- `expect()` - Assertion library
- `page.goto()` - Navigation
- `page.getByRole()` - User-facing locator (recommended)
- `toHaveTitle()` / `toBeVisible()` - Web-first assertions

---

### `.husky/pre-commit`

**Purpose:** Git hook that runs before each commit.

**Behavior:**

- Executes `npx lint-staged`
- lint-staged runs Prettier and ESLint only on staged files
- If linting fails, commit is blocked

---

### `.github/workflows/playwright.yml`

**Purpose:** GitHub Actions CI/CD pipeline.

**Workflow Steps:**

1. **Trigger:** On push/PR to `main` or `master` branches
2. **Checkout:** Clone repository
3. **Setup Node.js:** Install LTS version
4. **Install Dependencies:** `npm ci` (clean install)
5. **Install Browsers:** `npx playwright install --with-deps`
6. **Run Tests:** `npx playwright test`
7. **Upload Report:** Save HTML report as artifact (30 days retention)

---

### `.github/pull_request_template.md`

**Purpose:** Standardized PR checklist for code reviews.

**Checklist Items:**

- ✓ Unique selectors
- ✓ User-facing locators
- ✓ No debug code left
- ✓ Tests are independent
- ✓ Web-first assertions used
- ✓ Code passes linting

---

## 🚀 Getting Started

### Prerequisites

- Node.js (LTS version recommended)
- npm (comes with Node.js)
- Git

### Installation

```bash
# 1. Clone the repository
git clone <repository-url>
cd pw-eslint-husky

# 2. Install dependencies (also sets up Husky hooks via "prepare" script)
npm install

# 3. Configure Git hooks path
git config core.hooksPath .husky

# 4. Install Playwright browsers
npx playwright install
```

### Verify Installation

```bash
# Run example tests
npx playwright test

# View test report
npx playwright show-report

# Verify Git hooks are configured
git config --get core.hooksPath
# Should output: .husky
```

### ⚠️ Important: Setting Up Husky After Clone

After cloning the repository, each developer must configure Git to use Husky hooks:

```bash
# This tells Git to look for hooks in .husky/ directory
git config core.hooksPath .husky
```

**Why is this needed?**

- Git hooks are not tracked in the repository for security reasons
- The `prepare` script in package.json runs on `npm install`, but it doesn't always configure the hooks path
- Without this configuration, pre-commit checks won't run

**To verify it's working:**

```bash
# Check the configuration
git config --get core.hooksPath

# Try committing a file with .only() - it should be blocked
```

---

## 🔄 Workflow Guide

### Step 1: Create a New Test File

Create your test in the `tests/` directory:

```typescript
// tests/my-feature.spec.ts
import { test, expect } from '@playwright/test';

test.describe('My Feature', () => {
  test('should do something', async ({ page }) => {
    await page.goto('https://example.com');
    await expect(page.getByRole('heading')).toBeVisible();
  });
});
```

### Step 2: Run Tests Locally

```bash
# Run all tests
npx playwright test

# Run specific test file
npx playwright test tests/my-feature.spec.ts

# Run tests in headed mode (see browser)
npx playwright test --headed

# Run tests in UI mode (interactive)
npx playwright test --ui

# Run tests in specific browser
npx playwright test --project=chromium

# Debug a test
npx playwright test --debug
```

### Step 3: View Test Results

```bash
# Open HTML report
npx playwright show-report

# View trace (after failed test with trace)
npx playwright show-trace test-results/<test-name>/trace.zip
```

### Step 4: Stage and Commit Changes

```bash
# Stage your changes
git add .

# Commit (pre-commit hook runs automatically)
git commit -m "feat: add my feature tests"
```

**What happens on commit:**

1. **Husky** triggers the pre-commit hook
2. **lint-staged** identifies staged files
3. **Prettier** formats the code
4. **ESLint** checks for issues and auto-fixes what it can
5. If all passes → commit succeeds
6. If errors remain → commit is blocked, fix and retry

### Step 5: Push and Create PR

```bash
# Push to remote
git push origin feature/my-feature

# Create Pull Request on GitHub
# → CI pipeline runs automatically
# → Use PR template checklist for review
```

---

## 🔒 Git Hooks & Code Quality

### How Pre-commit Works

```
┌─────────────────┐
│   git commit    │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  Husky Hook     │
│  (pre-commit)   │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  lint-staged    │
│ (staged files)  │
└────────┬────────┘
         │
    ┌────┴────┐
    ▼         ▼
┌───────┐ ┌────────┐
│Prettier│ │ ESLint │
│--write │ │ --fix  │
└───┬───┘ └────┬───┘
    │         │
    └────┬────┘
         │
         ▼
┌─────────────────┐
│ Pass? → Commit  │
│ Fail? → Blocked │
└─────────────────┘
```

### Manual Linting Commands

```bash
# Run ESLint on all files
npx eslint .

# Run ESLint with auto-fix
npx eslint . --fix

# Run Prettier check
npx prettier --check .

# Run Prettier fix
npx prettier --write .
```

---

## ⚙️ CI/CD Pipeline

### Workflow Diagram

```
┌──────────────────────────────────────────────────────┐
│                  GitHub Actions                       │
├──────────────────────────────────────────────────────┤
│                                                       │
│  Trigger: Push/PR to main/master                     │
│                                                       │
│  ┌─────────────────────────────────────────────┐     │
│  │ 1. Checkout code                             │     │
│  │ 2. Setup Node.js (LTS)                       │     │
│  │ 3. npm ci (install dependencies)             │     │
│  │ 4. Install Playwright browsers               │     │
│  │ 5. Run: npx playwright test                  │     │
│  │ 6. Upload playwright-report/ as artifact     │     │
│  └─────────────────────────────────────────────┘     │
│                                                       │
│  Timeout: 60 minutes                                  │
│  Report retention: 30 days                            │
│                                                       │
└──────────────────────────────────────────────────────┘
```

### Viewing CI Results

1. Go to **Actions** tab in GitHub repository
2. Click on the workflow run
3. Download **playwright-report** artifact
4. Extract and open `index.html` in browser

---

## ✨ Best Practices

### Writing Tests

1. **Use user-facing locators:**

   ```typescript
   // ✅ Good
   page.getByRole('button', { name: 'Submit' });
   page.getByText('Welcome');
   page.getByLabel('Email');

   // ❌ Avoid
   page.locator('.btn-primary');
   page.locator('#submit-btn');
   ```

2. **Use web-first assertions:**

   ```typescript
   // ✅ Good - auto-retrying
   await expect(page.getByRole('heading')).toBeVisible();

   // ❌ Avoid - no auto-retry
   const heading = await page.locator('h1').isVisible();
   expect(heading).toBe(true);
   ```

3. **Avoid hard waits:**

   ```typescript
   // ✅ Good
   await page.getByRole('button').click();
   await expect(page.getByText('Success')).toBeVisible();

   // ❌ Avoid
   await page.waitForTimeout(2000);
   ```

4. **Keep tests independent:**
   - Each test should set up its own state
   - Tests should not depend on other tests' execution order

### Code Style

- Use descriptive test names
- Follow the Page Object Model for complex tests
- Remove all debug code (`console.log`, `page.pause()`) before committing
- Run tests locally before pushing

---

## 📝 License

ISC

---

## 🤝 Contributing

1. Create a feature branch
2. Write tests following project conventions
3. Ensure all tests pass locally
4. Submit PR using the template
5. Address review feedback
