This is a **clean, hands-on example** that demonstrates **both parallel jobs and dependent jobs**—perfect for the **Jobs & Steps lab**.

---

## 🧪 Lab: Jobs & Steps — Parallel & Dependent Jobs

### 🎯 Goal

* Run **two jobs in parallel**
* Run **one job only after both succeed**

---

## 📁 Example Use Case

Imagine a simple CI pipeline:

1. **Lint code** (job A)
2. **Run tests** (job B) → runs in parallel with lint
3. **Build app** (job C) → runs only if A & B succeed

---

## ✅ Complete Working Example

```yaml
name: Parallel and Dependent Jobs Demo

on:
  push:
    branches:
      - main
  workflow_dispatch:

jobs:
  lint:
    name: Lint Job
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Run lint
        run: echo "Linting completed successfully"

  test:
    name: Test Job
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Run tests
        run: echo "All tests passed successfully"

  build:
    name: Build Job (Depends on Lint & Test)
    runs-on: ubuntu-latest
    needs:
      - lint
      - test
    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Build application
        run: echo "Build completed successfully"
```

---

## 🔍 What This Demonstrates

### 1️⃣ Parallel Jobs

```yaml
lint:
test:
```

* `lint` and `test` **start at the same time**
* No `needs` → GitHub runs them in parallel

---

### 2️⃣ Dependent Job

```yaml
build:
  needs:
    - lint
    - test
```

* `build` waits for **both** jobs
* If **either fails**, `build` is skipped ❌

---

### 3️⃣ Jobs vs Steps (Key Concept)

| Level | Runs where       | Purpose                |
| ----- | ---------------- | ---------------------- |
| Jobs  | Separate runners | Parallelism, isolation |
| Steps | Same runner      | Sequential commands    |

---

## 🧪 How to See This Clearly in GitHub UI

1. Push to `main` or click **Run workflow**
2. Open **Actions → Workflow run**
3. You’ll see:

   * `lint` & `test` side-by-side
   * `build` waiting
4. Once both succeed → `build` starts

---

## 💥 Try This Experiment (Very Important)

Change one step to fail:

```yaml
run: exit 1
```

Result:

* `lint` or `test` ❌
* `build` → **skipped**
* This proves `needs` behavior

---

## 🧠 Best Practices (Production Tip)

* Keep jobs **small and focused**
* Use parallel jobs to **reduce total runtime**
* Use `needs` only where ordering is required

---
