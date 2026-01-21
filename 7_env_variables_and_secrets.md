Here’s a **clear, minimal, hands-on example** for **7️⃣ Environment Variables & Secrets**, exactly at *lab level* (easy to run, easy to understand).

---

## 🧪 Lab 7: Environment Variables & Secrets

### 🎯 Goal

* Use **environment variables** at different levels
* Use a **GitHub Secret**
* See how secrets are **masked in logs**

---

## 🔐 Pre-Lab Setup (Required)

1. Go to your GitHub repository
2. **Settings → Secrets and variables → Actions**
3. Click **New repository secret**
4. Add:

   * **Name:** `DB_PASSWORD`
   * **Value:** `super-secret-password`

---

## ✅ Complete Working Example

```yaml
name: Environment Variables and Secrets Demo

on:
  workflow_dispatch:
  push:
    branches:
      - main

# Workflow-level environment variable
env:
  APP_NAME: demo-app

jobs:
  env_secrets_job:
    runs-on: ubuntu-latest

    # Job-level environment variable
    env:
      ENVIRONMENT: dev

    steps:
      - name: Show workflow and job env vars
        run: |
          echo "App name: $APP_NAME"
          echo "Environment: $ENVIRONMENT"

      - name: Use step-level env variable
        env:
          STEP_MESSAGE: "Hello from step-level env"
        run: echo "$STEP_MESSAGE"

      - name: Use GitHub secret safely
        env:
          DB_PASSWORD: ${{ secrets.DB_PASSWORD }}
        run: |
          echo "Connecting to database..."
          echo "DB password is: $DB_PASSWORD"
```

---

## 🔍 What You Will Observe

### ✅ Environment Variable Scope

| Level    | Defined Where    | Accessible In    |
| -------- | ---------------- | ---------------- |
| Workflow | Top-level `env:` | All jobs & steps |
| Job      | Under job        | Only that job    |
| Step     | Inside step      | Only that step   |

---

### 🔐 Secret Masking (Very Important)

Even though we echo the secret:

```bash
echo "DB password is: $DB_PASSWORD"
```

GitHub logs will show:

```
DB password is: ***
```

✔ Secret is **never exposed**
✔ Automatically masked by GitHub

---

## 🧠 Key Rules to Remember

### ❗ Rule 1: Never echo secrets in real pipelines

This lab does it **only for learning**.

### ❗ Rule 2: Always pass secrets via `env`

✔ Correct:

```yaml
env:
  TOKEN: ${{ secrets.MY_TOKEN }}
```

❌ Avoid:

```yaml
run: echo ${{ secrets.MY_TOKEN }}
```

---

## 🧪 Optional Challenge (Try This)

1. Add another secret: `API_KEY`
2. Print both secrets in one step
3. Confirm both are masked
4. Remove step-level env and see failure

---
## 🎉 Congratulations!