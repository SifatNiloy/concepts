# Level 3: Automated Testing & Linting (Guard Rails)

Before deploying any Docker container, we want our pipeline to catch bugs **automatically** before any human ever tests it.

---

## 🛡️ The 3 Layers of Defense in CI

1. **Linting (`eslint`):** Catches syntax mistakes, unused variables, and bad practices.
2. **Type Checking (`tsc --noEmit`):** Catches TypeScript type errors.
3. **Automated Unit Tests (`jest` or `vitest`):** Verifies that your functions return the expected outputs.

If **any single one of these fails**, GitHub immediately:
* Marks the commit with a red ❌.
* Stops the pipeline (preventing broken code from building or deploying).
* Sends an email notification showing the exact line of code that failed.

---

## 🎯 Hands-On: Adding a Test to Next.js

In your `my-nextjs-docker-app` (or any Node app), let's create a simple function and test it:

### Step 1: Create a utility function
Create `utils/math.ts`:
```typescript
export function add(a: number, b: number): number {
  return a + b;
}
```

### Step 2: Install Jest or Vitest for testing
In your terminal:
```powershell
npm install -D vitest
```

Add a test script to your `package.json`:
```json
"scripts": {
  "dev": "next dev",
  "build": "next build",
  "start": "next start",
  "lint": "next lint",
  "test": "vitest run"
}
```

### Step 3: Write a test file
Create `utils/math.test.ts`:
```typescript
import { expect, test } from 'vitest';
import { add } from './math';

test('adds 1 + 2 to equal 3', () => {
  expect(add(1, 2)).toBe(3);
});
```

Test it locally first:
```powershell
npm test
```
*(You should see `✓ math.test.ts (1 test) Passed`)*

---

## 🤖 The Automated CI Workflow

Now let GitHub run this test automatically on every push!

Create `.github/workflows/ci.yml`:

```yaml
name: Continuous Integration (CI)

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  verify:
    runs-on: ubuntu-latest

    steps:
      - name: 1. Checkout code
        uses: actions/checkout@v4

      - name: 2. Set up Node 20
        uses: actions/setup-node@v4
        with:
          node-version: 20
          cache: 'npm' # Caches node_modules for super fast subsequent runs!

      - name: 3. Install dependencies
        run: npm ci

      - name: 4. Check for Lint errors
        run: npm run lint

      - name: 5. Run Unit Tests
        run: npm test

      - name: 6. Build production bundle
        run: npm run build
```

---

## ⚡ The Power of Caching (`cache: 'npm'`)

Notice this line in the workflow:
```yaml
cache: 'npm'
```
Without caching, every run downloads 200MB of npm dependencies from the internet, taking 1-2 minutes.  
With caching, GitHub saves your `package-lock.json` dependencies after the first run. The next run finishes in **under 15 seconds**!

---

## ✅ Level 3 Checklist

- [ ] I understand how CI acts as a quality gate before deploying.
- [ ] I can write a unit test with Vitest/Jest.
- [ ] I know how to run tests and linters in GitHub Actions.
- [ ] I know how `cache: 'npm'` speeds up builds dramatically.

👉 Ready for the big step? Open [level-4-real-world.md](file:///d:/CODES/concepts/2-cicd-learning-guide/level-4-real-world.md) to automatically build and push your Docker image to Docker Hub!
