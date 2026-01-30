Below is a **complete hands-on lab plan for GitHub Actions**, mapped **section by section**, with **real tasks**, **expected outcome**, and **what you learn**.
This is designed so you can actually *build CI/CD*, not just read YAML.

You can do all labs in **one GitHub repo**, or split into multiple repos.

---

# 🧪 GitHub Actions – Hands-On Labs (Section-wise)

---

## 1️⃣ Introduction & Basics — *First Workflow*

### 🧪 Lab 1: Hello GitHub Actions

**Task**

* Create a new GitHub repo
* Add `.github/workflows/hello.yml`
* Print “Hello GitHub Actions”

**What you do**

* Learn workflow structure
* Run first pipeline

**Outcome**

* Workflow runs on every push
* Green checkmark ✅

Solution: [1_hello_github_actions.yml](./yml_files/1_hello.yml)

---

## 2️⃣ Workflow Structure — *Understand YAML*

### 🧪 Lab 2: Workflow Anatomy

**Task**

* Rename workflow & job
* Add multiple steps
* Use comments

**What you do**

* Modify job name
* Add step descriptions

**Outcome**

* Clear, readable pipeline

Solution: [2_workflow_anatomy.yml](./yml_files/2_multiple_steps.yml)

---

## 3️⃣ Triggers — *Control Execution*

### 🧪 Lab 3: Event Triggers

**Task**

* Trigger on:

  * push to `main`
  * pull request
  * manual trigger

**What you do**

* Use `on: push`, `pull_request`, `workflow_dispatch`

**Outcome**

* You control when workflows run

Solution: [3_event_triggers.yml](./yml_files/3_event_triggers.yaml)

---

## 4️⃣ Runners — *Execution Environment*

### 🧪 Lab 4: Runner Types

**Task**

* Run workflow on:

  * ubuntu-latest
  * windows-latest

**What you do**

* Change `runs-on`
* Print OS info

**Outcome**

* Understand runner environments

Solution: [4_runner_types.yml](./yml_files/4_runner_types.yml)
---

## 5️⃣ Jobs & Steps — *Parallel & Dependent Jobs*

### 🧪 Lab 5: Multi-Job Workflow

**Task**

* Create:

  * build job
  * test job (depends on build)

**What you do**

* Use `needs`
* Observe parallel execution

**Outcome**

* Job dependency understanding

Solution: [5_parallel_dependent_jobs.yml](./yml_files/5_parallel_dependent_jobs.yml)

Explanation: [5_parallel_dependent_jobs_explanation.md](./5_parallel_dependent_jobs.md)

---

## 6️⃣ Actions — *Reuse Marketplace Actions*

### 🧪 Lab 6: Using Official Actions

**Task**

* Checkout repo
* Setup Python
* Run script

**Actions used**

* `actions/checkout`
* `actions/setup-python`

**Outcome**

* Understand action reuse

---

## 7️⃣ Environment Variables & Secrets

### 🧪 Lab 7: Secrets & ENV

**Task**

* Define env variable
* Create GitHub secret
* Print masked value

**What you do**

* Use `${{ secrets.MY_SECRET }}`

**Outcome**

* Secure data handling

Solution: [7_secrets_env.yml](./yml_files/7_env_variables_and_secrets.yml)

Explanation: [7_secrets_env_explanation.md](./7_env_variables_and_secrets.md)

---

## 8️⃣ Expressions & Conditions

### 🧪 Lab 8: Conditional Steps

**Task**

* Run step only on `main` branch
* Use step output in another step

**What you do**

* Use `if:` and outputs

**Outcome**

* Dynamic workflow logic

Solution: [8_conditional_steps.yml](./yml_files/8_conditionals.yml)

Explanation: [8_conditional_steps_explanation.md](./8_conditional_steps_explanation.md)




---

## 9️⃣ Matrix Builds

### 🧪 Lab 9: Matrix Strategy

**Task**

* Test Python versions:

  * 3.9
  * 3.10
  * 3.11

**What you do**

* Use `strategy.matrix`

**Outcome**

* Multi-version testing

Solution: [9_matrix.yml](./yml_files/9_matrix.yml)

Explanation: [9_matrix_explanation.md](./9_matrix_explanation.md)


---

## 🔟 Artifacts & Caching

### 🧪 Lab 10: Artifacts & Cache

**Task**

* Upload test report
* Cache pip dependencies

**What you do**

* Use `upload-artifact`
* Use `cache`

**Outcome**

* Faster pipelines

---

## 1️⃣1️⃣ Testing & Quality

### 🧪 Lab 11: CI for Python App

**Task**

* Run pytest
* Generate coverage report

**What you do**

* Fail build if test fails

**Outcome**

* Real CI pipeline

---

## 1️⃣2️⃣ Deployment Workflows

### 🧪 Lab 12: Environment-Based Deployment

**Task**

* Create `dev` and `prod` environments
* Add manual approval for prod

**What you do**

* Use `environment:` keyword

**Outcome**

* Controlled deployments

---

## 1️⃣3️⃣ Docker & Containers

### 🧪 Lab 13: Docker Build & Push

**Task**

* Build Docker image
* Push to Docker Hub / GHCR

**What you do**

* Use `docker build`
* Login using secrets

**Outcome**

* Container CI/CD

---

## 1️⃣4️⃣ Cloud Integration (Azure / AWS / GCP)

### 🧪 Lab 14: Azure Deployment (OIDC)

**Task**

* Login to Azure using OIDC
* Deploy resource or run CLI

**What you do**

* Use `azure/login`
* Avoid secrets

**Outcome**

* Secure cloud auth

---

## 1️⃣5️⃣ Security & Best Practices

### 🧪 Lab 15: Secure Workflow

**Task**

* Restrict permissions
* Scan for secrets

**What you do**

* Set `permissions`
* Use Dependabot

**Outcome**

* Production-ready security

---

## 1️⃣6️⃣ Reusable Workflows

### 🧪 Lab 16: Reusable CI Workflow

**Task**

* Create reusable CI workflow
* Call it from another workflow

**What you do**

* Use `workflow_call`

**Outcome**

* DRY pipelines

---

## 1️⃣7️⃣ Debugging & Monitoring

### 🧪 Lab 17: Debug Failures

**Task**

* Intentionally fail workflow
* Enable debug logs

**What you do**

* Use `ACTIONS_STEP_DEBUG`

**Outcome**

* Faster troubleshooting

---

## 1️⃣8️⃣ Real-World Project (Capstone)

### 🧪 Lab 18: Full DevOps Pipeline

**Task**

* CI → Docker → Registry → Cloud/K8s

**Pipeline**

1. Code push
2. Tests
3. Docker build
4. Image push
5. Deploy

**Outcome**

* End-to-end CI/CD

---

## 📁 Suggested Repo Structure

```
.github/workflows/
  01-hello.yml
  02-triggers.yml
  03-matrix.yml
  04-docker.yml
  05-deploy.yml
app/
  main.py
  test_app.py
Dockerfile
```

---

## 🎯 What You’ll Be Able To Do After This

✅ Write production CI/CD pipelines
✅ Use GitHub Actions professionally
✅ Deploy to cloud securely
✅ Clear DevOps interviews

---

Happy Learning! 🚀