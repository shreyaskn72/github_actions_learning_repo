Let’s make **Lab 10 fully runnable end-to-end** so you can literally copy → push → watch it work.

Below I’ll give you:

1. `app.py`
2. `test_app.py`
3. `requirements.txt`
4. **Full workflow**
5. **Deep explanation of how everything connects**

---

# 📁 Project Structure

```
.
├── app.py
├── test_app.py
├── requirements.txt
└── .github/workflows/lab10-artifacts-cache.yml
```

---

## 1️⃣ `app.py` (Simple Application)

```python
def add(a, b):
    return a + b


def subtract(a, b):
    return a - b
```

Why this?

* Simple logic
* Easy to test
* Focus stays on GitHub Actions, not app complexity

---

## 2️⃣ `test_app.py` (Pytest Tests)

```python
from app import add, subtract


def test_add():
    assert add(2, 3) == 5


def test_subtract():
    assert subtract(5, 3) == 2
```

Why this?

* Uses `pytest`
* Two clear passing tests
* If logic breaks → pipeline fails

---

## 3️⃣ `requirements.txt`

```text
pytest==8.0.0
```

Why pin versions?

* Reproducible builds
* Stable cache behavior
* Professional CI practice

---

## 4️⃣ Full GitHub Actions Workflow

```yaml
name: Artifacts and Caching Demo

on:
  push:
    branches:
      - main
  workflow_dispatch:

jobs:
  test:
    runs-on: ubuntu-latest

    steps:
      # 1️⃣ Fetch repository code
      - name: Checkout code
        uses: actions/checkout@v4

      # 2️⃣ Install Python runtime
      - name: Set up Python
        uses: actions/setup-python@v5
        with:
          python-version: "3.11"

      # 3️⃣ Cache pip dependencies (speed optimization)
      - name: Cache pip dependencies
        uses: actions/cache@v4
        with:
          path: ~/.cache/pip
          key: ${{ runner.os }}-pip-${{ hashFiles('requirements.txt') }}
          restore-keys: |
            ${{ runner.os }}-pip-

      # 4️⃣ Install dependencies
      - name: Install dependencies
        run: pip install -r requirements.txt

      # 5️⃣ Run tests and generate artifact
      - name: Run tests and generate report
        run: |
          mkdir -p reports
          pytest -q > reports/test-report.txt

      # 6️⃣ Upload test report as artifact
      - name: Upload test report
        uses: actions/upload-artifact@v4
        with:
          name: test-report
          path: reports/
```

---

# 🔍 FULL WORKFLOW EXPLANATION (Step-by-Step)

---

## 🔹 Step 1: Checkout Code

```yaml
uses: actions/checkout@v4
```

✔ Pulls your repo into the runner
✔ Required for tests and installs

---

## 🔹 Step 2: Setup Python

```yaml
uses: actions/setup-python@v5
```

✔ Installs Python 3.11
✔ Ensures consistent runtime

---

## 🔹 Step 3: Cache Dependencies (🔥 Critical)

```yaml
uses: actions/cache@v4
```

### What’s cached?

```yaml
path: ~/.cache/pip
```

✔ Where pip stores downloaded packages

---

### Cache Key Logic

```yaml
key: ${{ runner.os }}-pip-${{ hashFiles('requirements.txt') }}
```

✔ New cache when dependencies change
✔ Separate cache per OS

---

### Restore Keys

```yaml
restore-keys:
  ${{ runner.os }}-pip-
```

✔ Fallback cache if exact key not found

---

## 🔹 Step 4: Install Dependencies

```bash
pip install -r requirements.txt
```

* First run → slow (cache miss)
* Next runs → fast (cache hit)

---

## 🔹 Step 5: Run Tests & Generate Report

```bash
pytest -q > reports/test-report.txt
```

✔ Runs tests
✔ Saves output to file
✔ Ensures artifact exists even on success

---

## 🔹 Step 6: Upload Artifact

```yaml
uses: actions/upload-artifact@v4
```

✔ Stores test report
✔ Downloadable from GitHub UI
✔ Available for 90 days

---

## 🧠 Cache vs Artifact (Burn This In)

| Feature      | Cache       | Artifact |
| ------------ | ----------- | -------- |
| Purpose      | Speed       | Output   |
| Between runs | ✅           | ❌        |
| Downloadable | ❌           | ✅        |
| Expires      | Usage-based | 90 days  |

---

## 🧪 How to Validate This Lab

1. Push code to `main`
2. Open **Actions tab**
3. Observe:

   * First run: cache miss
   * Second run: cache hit
4. Download `test-report` artifact
5. See pytest output inside ZIP

---

## ⚠️ Try Breaking It (Learning Trick)

Change `add()`:

```python
return a - b
```

Result:

* Tests ❌
* Artifact still uploaded
* CI fails as expected

---