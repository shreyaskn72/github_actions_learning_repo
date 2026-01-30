Absolutely 👍
**Lab 9 (Matrix Builds)** is a *big unlock* in GitHub Actions, so I’ll give you a **clean, real-world example** with a **strong explanation and mental model**.

---

# 🧪 Lab 9: Matrix Builds

## 🎯 Scenario (Very Common in Real CI)

You maintain a Python app that must work on:

* Multiple **Python versions**
* Multiple **operating systems**

Instead of writing separate jobs, you use a **matrix**.

---

## 🧠 What You’ll Learn in This Lab

* What a matrix is
* How parallelism works
* `include` / `exclude`
* Matrix context usage
* How failures affect matrix runs

---

## ✅ Complete Working Example

```yaml
name: Matrix Build Demo

on:
  push:
    branches:
      - master
  workflow_dispatch:

jobs:
  test:
    name: Test on ${{ matrix.os }} with Python ${{ matrix.python }}
    runs-on: ${{ matrix.os }}

    strategy:
      fail-fast: false
      matrix:
        os: [ubuntu-latest, windows-latest]
        python: ["3.9", "3.10", "3.11"]
        exclude:
          - os: windows-latest
            python: "3.9"

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Set up Python
        uses: actions/setup-python@v5
        with:
          python-version: ${{ matrix.python }}

      - name: Show environment info
        run: |
          python --version
          echo "Running on OS: ${{ runner.os }}"

      - name: Run tests
        run: echo "Tests passed 🎉"
```

---

## 🔍 What This Matrix Creates (Important)

### Matrix Definition

```yaml
os: [ubuntu-latest, windows-latest]
python: ["3.9", "3.10", "3.11"]
```

➡ Cartesian product = **2 × 3 = 6 jobs**

### Exclusion

```yaml
exclude:
  - os: windows-latest
    python: "3.9"
```

➡ Removes 1 job
➡ Final total = **5 jobs**

---

## 🧪 Visual Mental Model

| OS      | Python | Job        |
| ------- | ------ | ---------- |
| Ubuntu  | 3.9    | ✅          |
| Ubuntu  | 3.10   | ✅          |
| Ubuntu  | 3.11   | ✅          |
| Windows | 3.9    | ❌ excluded |
| Windows | 3.10   | ✅          |
| Windows | 3.11   | ✅          |

---

## 🧠 Key Concepts Explained

---

### 1️⃣ `matrix` Context

You can access values like:

```yaml
${{ matrix.os }}
${{ matrix.python }}
```

Used in:

* Job name
* `runs-on`
* Steps
* Conditions

---

### 2️⃣ Parallel Execution

Each matrix combination:

* Runs as a **separate job**
* On a **separate runner**
* In **parallel** 🚀

---

### 3️⃣ `fail-fast`

```yaml
fail-fast: false
```

| Setting          | Behavior              |
| ---------------- | --------------------- |
| `true` (default) | Stop all if one fails |
| `false`          | Let all finish        |

✔ Best for CI test visibility

---

### 4️⃣ Matrix Job Naming

```yaml
name: Test on ${{ matrix.os }} with Python ${{ matrix.python }}
```

✔ Makes GitHub UI readable
✔ Essential for large matrices

---

## 🧪 Try This Experiment (Very Important)

Change this step:

```yaml
run: exit 1
```

Result:

* One matrix job ❌
* Others continue (because `fail-fast: false`)
* You can see **which OS + version failed**

---

## ⚠️ Common Mistakes (Avoid These)

❌ Using matrix values outside matrix job
❌ Forgetting `runs-on: ${{ matrix.os }}`
❌ Not naming jobs (hard to debug)

---

## 🔥 Optional Advanced Add-On

### Include a Special Case

```yaml
include:
  - os: ubuntu-latest
    python: "3.12"
    experimental: true
```

Then use:

```yaml
if: matrix.experimental != true
```

---

## 🚀 Where Matrix Builds Are Used

* Language version testing
* OS compatibility
* Multi-region builds
* Microservices CI

---

