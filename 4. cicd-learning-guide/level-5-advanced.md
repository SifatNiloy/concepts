# Level 5: Secrets, Environments & Matrix Builds

In production apps, you have sensitive API keys (Stripe, Database URLs, AWS keys) and multiple branches (`main`, `staging`). You must manage them cleanly and securely.

---

## 🔐 1. GitHub Repository Secrets

Never commit `.env` or `.env.local` to Git! Instead, use **GitHub Secrets**:

### How to add a Secret in GitHub:
1. Go to your GitHub repository in your browser.
2. Click **Settings** $\rightarrow$ **Secrets and variables** $\rightarrow$ **Actions**.
3. Click **New repository secret**.
4. Name: `DATABASE_URL`
5. Value: `postgresql://user:password@db.myhost.com:5432/mydb`

### How to use it inside your YAML file:
```yaml
steps:
  - name: Run Database Migrations
    env:
      DATABASE_URL: ${{ secrets.DATABASE_URL }}
      STRIPE_KEY: ${{ secrets.STRIPE_SECRET_KEY }}
    run: npm run db:migrate
```

GitHub automatically redacts secrets in the logs: if your script prints `DATABASE_URL`, GitHub replaces the password with `***` so nobody can steal it from build logs!

---

## 🔀 2. Matrix Builds (Testing on Multiple Environments)

What if you want to make sure your code runs on **Node 18, Node 20, AND Node 22**?  
Or on both **Ubuntu and Windows**?

Instead of writing 3 different workflows, use a **Matrix**:

```yaml
jobs:
  test-matrix:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        node-version: [18, 20, 22]

    steps:
      - uses: actions/checkout@v4
      - name: Use Node.js ${{ matrix.node-version }}
        uses: actions/setup-node@v4
        with:
          node-version: ${{ matrix.node-version }}
      - run: npm ci
      - run: npm test
```

GitHub will automatically spin up **3 runners simultaneously**, testing your app across all 3 Node versions in parallel!

---

## 🛡️ 3. Branch Protection Rules

In professional software development, nobody is allowed to push directly to `main`.

### The Standard Industry Workflow:
1. You create a feature branch: `git checkout -b feature/login-page`
2. You make your commits and push: `git push origin feature/login-page`
3. You open a **Pull Request (PR)** on GitHub.
4. Your GitHub Actions CI workflow triggers automatically on the PR.
5. If tests fail $\rightarrow$ GitHub **blocks the Merge button**!
6. Only when CI passes (green checkmark ✅) can the code be merged into `main`.

---

## ✅ Level 5 Checklist

- [ ] I know how to add secrets in GitHub Repo Settings.
- [ ] I know how to inject secrets into steps using `${{ secrets.MY_SECRET }}`.
- [ ] I understand how Matrix builds let you test multiple versions in parallel.
- [ ] I understand why branch protection rules prevent broken code on `main`.
