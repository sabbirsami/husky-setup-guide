# Husky Setup Guide for Next.js Frontend & Express TypeScript Backend

This guide provides step-by-step instructions to set up Husky with pre-commit hooks, commit message validation, and code quality checks for both frontend and backend projects.

## Table of Contents

- [Frontend Setup (Next.js)](#frontend-setup-nextjs)
- [Backend Setup (Express TypeScript)](#backend-setup-express-typescript)
- [Testing the Setup](#testing-the-setup)
- [Troubleshooting](#troubleshooting)

---

## Frontend Setup (Next.js)

### 1. Install Required Dependencies

```bash
# Navigate to your frontend project
cd your-frontend-project

# Install Husky and related packages
npm install --save-dev husky @commitlint/cli @commitlint/config-conventional lint-staged prettier prettier-plugin-tailwindcss
```

### 2. Initialize Husky

```bash
# Initialize Husky
npx husky init
```

### 3. Update package.json Scripts

Add the following scripts to your `package.json`:

```json
{
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start",
    "lint": "eslint",
    "lint:fix": "eslint --fix",
    "format": "prettier --write .",
    "prepare": "husky",
    "type-check": "tsc --pretty --noEmit"
  },
  "lint-staged": {
    "*.{ts,tsx,js,jsx}": ["eslint --fix", "prettier --write"]
  }
}
```

### 4. Create Commitlint Configuration

Create `.commitlintrc.ts` in your project root:

```typescript
import type { UserConfig } from '@commitlint/types';

const commitConfig: UserConfig = {
  extends: ['@commitlint/config-conventional'],
};

module.exports = commitConfig;
```

### 5. Create Prettier Configuration

Create `.prettierrc` in your project root:

```json
{
  "semi": true,
  "trailingComma": "es5",
  "singleQuote": true,
  "printWidth": 80,
  "tabWidth": 2,
  "useTabs": false,
  "plugins": ["prettier-plugin-tailwindcss"]
}
```

**Note:** The `prettier-plugin-tailwindcss` plugin automatically sorts Tailwind CSS classes according to the recommended class order. Make sure you have a `tailwind.config.js` file in your project root for the plugin to work properly.

### 6. Configure Pre-commit Hook

Replace the content of `.husky/pre-commit` with:

```bash
#!/usr/bin/env sh
. "$(dirname -- "$0")/_/husky.sh"

echo '🏗️👷 Styling, testing and building your project before committing'

# Check staged files
npx lint-staged ||
(
    echo '🤢🤮🤢🤮 Its FOKING RAW - Your styling looks disgusting. 🤢🤮🤢🤮
            Staged files Check Failed. Run npm run format, add changes and try commit again.';
    false;
)

# Check Prettier standards
npm run format ||
(
    echo '🤢🤮🤢🤮 Its FOKING RAW - Your styling looks disgusting. 🤢🤮🤢🤮
            Prettier Check Failed. Run npm run format, add changes and try commit again.';
    false;
)

# Check ESLint Standards
npm run lint ||
(
    echo '😤🏀👋😤 Get that weak s**t out of here! 😤🏀👋😤
            ESLint Check Failed. Make the required changes listed above, add changes and try to commit again.'
    false;
)

# Check tsconfig standards
npm run type-check ||
(
    echo '🤡😂❌🤡 Failed Type check. 🤡😂❌🤡
            Are you seriously trying to write TypeScript like JavaScript?'
    false;
)

echo '🤔🤔🤔🤔... Alright... Code looks good to me... trying to build now. 🤔🤔🤔🤔'

echo '✅✅✅✅ You win this time... I am committing this now. ✅✅✅✅'
```

### 7. Configure Commit Message Hook

Create `.husky/commit-msg` with:

```bash
#!/usr/bin/env sh
. "$(dirname -- "$0")/_/husky.sh"

npx commitlint --config .commitlintrc.ts --edit $1 ||
(
    echo '❌ Commit message format is incorrect. Please follow the conventional commit format.';
    false;
)
```

### 8. Make Hook Files Executable

```bash
chmod +x .husky/pre-commit .husky/commit-msg
```

---

## Backend Setup (Express TypeScript)

### 1. Install Required Dependencies

```bash
# Navigate to your backend project
cd your-backend-project

# Install Husky and related packages
npm install --save-dev husky @commitlint/cli @commitlint/config-conventional lint-staged prettier
npm i prettier-plugin-tailwindcss
```

### 2. Initialize Husky

```bash
# Initialize Husky
npx husky init
```

### 3. Update package.json Scripts

Add the following scripts to your `package.json`:

```json
{
  "scripts": {
    "dev": "nodemon src/server.ts",
    "build": "tsc",
    "start": "node dist/server.js",
    "lint": "eslint src/**/*.ts",
    "lint:fix": "eslint src/**/*.ts --fix",
    "format": "prettier --write src/**/*.ts",
    "prepare": "husky",
    "type-check": "tsc --noEmit"
  },
  "lint-staged": {
    "src/**/*.{ts,js}": ["eslint --fix", "prettier --write"]
  }
}
```

### 4. Create Commitlint Configuration

Create `.commitlintrc.ts` in your project root:

```typescript
import type { UserConfig } from '@commitlint/types';

const commitConfig: UserConfig = {
  extends: ['@commitlint/config-conventional'],
};

module.exports = commitConfig;
```

### 5. Create Prettier Configuration

Create `.prettierrc` in your project root:

```json
{
  "semi": true,
  "trailingComma": "es5",
  "singleQuote": true,
  "printWidth": 80,
  "tabWidth": 2,
  "useTabs": false
}
```

### 6. Configure Pre-commit Hook

Replace the content of `.husky/pre-commit` with:

```bash
#!/usr/bin/env sh
. "$(dirname -- "$0")/_/husky.sh"

echo '🏗️👷 Styling, testing and building your project before committing'

# Check staged files
npx lint-staged ||
(
    echo '🤢🤮🤢🤮 Its FOKING RAW - Your styling looks disgusting. 🤢🤮🤢🤮
            Staged files Check Failed. Run npm run format, add changes and try commit again.';
    false;
)

# Check Prettier standards
npm run format ||
(
    echo '🤢🤮🤢🤮 Its FOKING RAW - Your styling looks disgusting. 🤢🤮🤢🤮
            Prettier Check Failed. Run npm run format, add changes and try commit again.';
    false;
)

# Check ESLint Standards
npm run lint ||
(
    echo '😤🏀👋😤 Get that weak s**t out of here! 😤🏀👋😤
            ESLint Check Failed. Make the required changes listed above, add changes and try to commit again.'
    false;
)

# Check tsconfig standards
npm run type-check ||
(
    echo '🤡😂❌🤡 Failed Type check. 🤡😂❌🤡
            Are you seriously trying to write TypeScript like JavaScript?'
    false;
)

echo '🤔🤔🤔🤔... Alright... Code looks good to me... trying to build now. 🤔🤔🤔🤔'

echo '✅✅✅✅ You win this time... I am committing this now. ✅✅✅✅'
```

### 7. Configure Commit Message Hook

Create `.husky/commit-msg` with:

```bash
#!/usr/bin/env sh
. "$(dirname -- "$0")/_/husky.sh"

npx commitlint --config .commitlintrc.ts --edit $1 ||
(
    echo '❌ Commit message format is incorrect. Please follow the conventional commit format.';
    false;
)
```

### 8. Make Hook Files Executable

```bash
chmod +x .husky/pre-commit .husky/commit-msg
```

---

## Testing the Setup

### Test Pre-commit Hooks

1. Make some changes to your code
2. Stage the changes:
   ```bash
   git add .
   ```
3. Try to commit:
   ```bash
   git commit -m "test: testing husky setup"
   ```

### Test Commit Message Validation

Try committing with an invalid message:

```bash
git commit -m "invalid message"
```

This should fail. Use conventional commit format:

```bash
git commit -m "feat: add new feature"
git commit -m "fix: resolve bug in authentication"
git commit -m "docs: update README"
```

### Valid Commit Message Formats

- `feat:` - New features
- `fix:` - Bug fixes
- `docs:` - Documentation changes
- `style:` - Code style changes (formatting, etc.)
- `refactor:` - Code refactoring
- `test:` - Adding or updating tests
- `chore:` - Maintenance tasks

---

## Troubleshooting

### Common Issues

1. **Husky hooks not running:**

   ```bash
   # Reinstall husky
   npm uninstall husky
   npm install --save-dev husky
   npx husky init
   ```

2. **Permission denied errors:**

   ```bash
   chmod +x .husky/pre-commit .husky/commit-msg
   ```

3. **ESLint or Prettier errors:**

   ```bash
   # Fix linting issues
   npm run lint:fix

   # Format code
   npm run format
   ```

4. **TypeScript errors:**
   ```bash
   # Check types
   npm run type-check
   ```

### Bypassing Hooks (Emergency Only)

```bash
# Skip pre-commit hooks (NOT RECOMMENDED)
git commit -m "emergency fix" --no-verify

# Skip commit message validation (NOT RECOMMENDED)
git commit -m "emergency fix" --no-verify
```

### Updating Husky

```bash
# Update to latest version
npm update husky

# Reinstall hooks
npx husky init
```

### Prettier Plugin Tailwindcss Issues

**Issue: Tailwind classes not being sorted**

1. **Check if prettier-plugin-tailwindcss is installed:**
   ```bash
   npm list prettier-plugin-tailwindcss
   ```

2. **Ensure the plugin is in your .prettierrc:**
   ```json
   {
     "plugins": ["prettier-plugin-tailwindcss"]
   }
   ```

3. **Make sure you have tailwind.config.js in your project root:**
   ```bash
   # If missing, create it
   npx tailwindcss init
   ```

4. **Test the plugin manually:**
   ```bash
   # Format a specific file
   npx prettier --write "src/components/YourComponent.tsx"
   
   # Check if classes are being sorted
   echo '<div className="text-red-500 bg-blue-200 p-4 m-2"></div>' | npx prettier --parser html --stdin-filepath test.html
   ```

5. **Clear Prettier cache:**
   ```bash
   npx prettier --cache-location=node_modules/.cache/prettier/.prettier-cache --write .
   ```

**Issue: Plugin conflicts with other Prettier plugins**

Make sure prettier-plugin-tailwindcss is the **last** plugin in your plugins array:
```json
{
  "plugins": ["@trivago/prettier-plugin-sort-imports", "prettier-plugin-tailwindcss"]
}
```

---

## Quick Setup Commands Summary

### Frontend (Next.js)

```bash
cd your-frontend-project
npm install --save-dev husky @commitlint/cli @commitlint/config-conventional lint-staged prettier
npx husky init
npm i prettier-plugin-tailwindcss
# Then follow configuration steps above
```

### Backend (Express TypeScript)

```bash
cd your-frontend-project
npm install --save-dev husky @commitlint/cli @commitlint/config-conventional lint-staged prettier prettier-plugin-tailwindcss
npx husky init
# Then follow configuration steps above
```

---

**Note:** This setup ensures code quality, consistent formatting, and conventional commit messages across your entire project. The hooks will run automatically before each commit, preventing bad code from entering your repository.
