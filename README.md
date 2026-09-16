# CampusEats Task Tracker


**Course:** SE3090 – Software Engineering Frameworks  
**Lab Practical:** 08 – Git, Collaborative Development, CI/CD, Security & Code Quality  
**Author:** Mummullage B.U.T  
**IT Number:** IT24102699

---

## Table of Contents

- [Overview](#overview)
- [Learning Outcomes](#learning-outcomes)
- [Practical Agenda](#practical-agenda)
- [Task 01 — Repository Setup and Initial Push](#task-01--repository-setup-and-initial-push)
- [Task 02 — Feature Branch and Conventional Commits](#task-02--feature-branch-and-conventional-commits)
- [Task 03 — Pull Request and Code Review](#task-03--pull-request-and-code-review)
- [Task 04 — GitHub Issues and Collaborative Workflow](#task-04--github-issues-and-collaborative-workflow)
- [Task 05 — CI/CD Pipeline using GitHub Actions](#task-05--cicd-pipeline-using-github-actions)
- [Task 06 — Code Quality and Security Review](#task-06--code-quality-and-security-review)
- [Quick Test — Knowledge Check](#quick-test--knowledge-check)
- [Submission Instructions](#submission-instructions)

---

## Overview

This repository was built as part of SE3090 Lab Practical 08. The scenario simulates being part of a fast-moving engineering team at **CampusEats** — a platform for campus food ordering — preparing a new release of the task-tracking service.

The practical covers setting up a GitHub repository, working with feature branches, opening and reviewing pull requests, creating GitHub Issues, automating checks with GitHub Actions, and addressing security and code quality concerns.

---

## Learning Outcomes

| Code | Outcome |
|------|---------|
| LO1  | Use Git and GitHub for branch-based feature development, pull requests, and code reviews. |
| LO2  | Resolve merge conflicts cleanly using standard Git command-line tools. |
| LO3  | Configure a basic CI/CD pipeline using GitHub Actions to automate build and test checks. |
| LO4  | Apply security best practices, including secret scanning awareness and basic code quality checks. |

---

## Practical Agenda

| Task | Title | Duration | Learning Outcomes |
|------|-------|----------|-------------------|
| Task 01 | Repository Setup & Initial Push | 15 min | LO1 |
| Task 02 | Feature Branch & Conventional Commits | 15 min | LO1 |
| Task 03 | Pull Request & Code Review | 15 min | LO1 |
| Task 04 | GitHub Issues & Collaborative Workflow | 12 min | LO3 |
| Task 05 | CI/CD Pipeline using GitHub Actions | 22 min | LO3, LO4 |
| Task 06 | Code Quality & Security Review | 13 min | LO3, LO4 |
| Quick Test | Knowledge Check | 8 min | LO1–LO4 |
| **Total** | | **100 min** | *(20 minutes buffer for submission)* |

---

## Task 01 — Repository Setup and Initial Push

**Objective:** Create a public GitHub repository and push an initial commit to the `main` branch.

**Steps performed:**

```bash
git clone https://github.com/umandathathsarani/campuseats-task-tracker.git
cd campuseats-task-tracker
mkdir src
echo "// CampusEats task list" > src/tasks.js
git remote -v
git status
```

**Expected Output:** A public GitHub repository with the `main` branch containing `README.md` and one initial commit.

---

## Task 02 — Feature Branch and Conventional Commits

**Objective:** Demonstrate branch-based development using the GitHub Flow model and Conventional Commits naming.

Real engineering teams never commit directly to `main`. Every change lives on a feature branch.

**Steps performed:**

```bash
git switch -c feature/add-task-list
git add src/tasks.js
git commit -m "feat: add initial CampusEats task list"
git push -u origin feature/add-task-list
```

**Source file — `src/tasks.js`:**

```javascript
// CampusEats task list
const tasks = [
  "Design the menu screen",
  "Build the orders API",
  "Add user login",
];
console.log(`CampusEats has ${tasks.length} open tasks`);
```

**Conventional Commits Reference:**

| Prefix | Purpose |
|--------|---------|
| `feat:` | A new feature |
| `fix:` | A bug fix |
| `docs:` | Documentation changes |
| `chore:` | Maintenance tasks (build, CI, dependencies) |
| `refactor:` | Code rewrite without feature or bug change |

**Branching Strategy Note:**

We follow the **GitHub Flow** branching strategy. The `main` branch is considered the single source of truth and must always remain in a stable, deployable state. Instead of committing directly to `main`, developers create short-lived, descriptively named feature branches for any new features, bug fixes, or updates. Once the work on a feature branch is complete, a Pull Request (PR) is opened to propose the changes. The PR serves as a dedicated space for peer code review, discussion, and automated checks. Only after the code has been reviewed, approved, and has passed all automated tests is the feature branch merged back into `main`. This strategy ensures high code quality, clean collaboration, and prevents broken code from reaching production.

**Expected Output:** A pushed branch `feature/add-task-list` visible on GitHub with at least one conventional commit.

---

## Task 03 — Pull Request and Code Review

**Objective:** Open a Pull Request and conduct a code review before merging into `main`.

Pull Requests (PRs) let team members review code before it reaches `main`.

**Steps performed:**

1. Navigated to the GitHub repository and clicked **Compare & pull request** for the `feature/add-task-list` branch.
2. Set base to `main` and compare to `feature/add-task-list`.
3. Added a clear title (`feat: add initial CampusEats task list`) and a short description.
4. Submitted a review comment under the **Files changed** tab.
5. Merged the PR using **Confirm Merge** and deleted the branch.
6. Pulled the merged changes back locally:

```bash
git switch main
git pull origin main
```

**Expected Output:** A merged Pull Request on GitHub with a description, at least one review comment, and the local `main` updated.

---

## Task 04 — GitHub Issues and Collaborative Workflow

**Objective:** Use GitHub Issues to plan and track upcoming development work.

Teams plan and track work with Issues — small, labelled units of work that anyone can pick up. Issues link naturally to branches, commits, and pull requests.

**Issues created:**

| # | Title | Label |
|---|-------|-------|
| 1 | Add due dates to tasks | `enhancement` |
| 2 | Fix typo in README | `bug` |
| 3 | Add CI workflow | `enhancement` |

**Linking Issues to Work:**

By including a phrase like `Closes #1` or `Fixes #2` in a commit message or Pull Request description, you create an automatic link between your work and the tracked issue. When that commit or PR is successfully merged into the `main` branch, GitHub will automatically close the linked issue. This keeps the project's task board organized and ensures issues are closed exactly when the fix goes live, without needing manual updates.

**Expected Output:** At least three labelled issues on the repository.

---

## Task 05 — CI/CD Pipeline using GitHub Actions

**Objective:** Configure an automated CI pipeline using GitHub Actions that triggers on every push and pull request.

Continuous Integration means every push is automatically checked. The pipeline follows the lifecycle: `Commit -> Build -> Test -> Package -> Deploy -> Monitor`. This task automates the first stages.

**Steps performed:**

```bash
git switch -c chore/add-ci
git add .github/workflows/ci.yml
git commit -m "chore: add GitHub Actions CI workflow"
git push -u origin chore/add-ci
```

**Workflow file — `.github/workflows/ci.yml`:**

```yaml
name: CI

on:
  push:
    branches: [ "main", "feature/**", "chore/**" ]
  pull_request:
    branches: [ "main" ]

jobs:
  build-and-check:
    runs-on: ubuntu-latest
    steps:
      - name: Check out the repository
        uses: actions/checkout@v4

      - name: List repository files
        run: ls -la

      - name: Basic project check
        run: |
          echo "Running CI for CampusEats Task Tracker"
          test -f README.md && echo "README found"
```

**How CI protects the codebase:** A failing CI check blocks a pull request from being merged. This acts as an automated safety net, ensuring that only verified, passing code can reach the `main` branch.

**Expected Output:** A successful (green) GitHub Actions run triggered by the push/PR, and the passing check shown on the pull request.

---

## Task 06 — Code Quality and Security Review

**Objective:** Identify code quality issues and commit a cleaner, more secure version.

### Code Quality Issues Identified

The following problems were found in the original code:

```javascript
// BEFORE — what is wrong here?
function calc(a, b, t) {
  var x = a * b;
  if (t == "vip") { x = x - x * 0.1 }
  console.log("API_KEY=sk_live_9f8a7b6c5d"); // !!
  return x
}
```

| # | Issue | Explanation |
|---|-------|-------------|
| 1 | **Hardcoded Secret** | The API key `sk_live_9f8a7b6c5d` is committed directly into source code — a critical security vulnerability. |
| 2 | **Unclear Naming** | Variables `a`, `b`, `t`, `x` and function name `calc` give no context about what the code does. |
| 3 | **Magic Number** | The discount value `0.1` is unexplained. It should be a named constant like `VIP_DISCOUNT`. |
| 4 | **`var` instead of `const`** | `var` has function-level scope and is considered outdated; `const` or `let` should be used instead. |
| 5 | **Loose equality (`==`)** | Using `==` instead of strict equality `===` can produce unexpected type coercion bugs. |
| 6 | **No input validation** | Negative prices or quantities are not rejected, which could cause incorrect totals. |

### Improved Code

```javascript
// AFTER — clear names, no magic numbers, no secrets
const VIP_DISCOUNT = 0.1;

function calculateTotal(price, quantity, customerType) {
  if (price < 0 || quantity < 0) {
    throw new Error("price and quantity must be >= 0");
  }
  const subtotal = price * quantity;
  return customerType === "vip"
    ? subtotal * (1 - VIP_DISCOUNT)
    : subtotal;
}
// the API key comes from an environment variable,
// e.g. process.env.API_KEY — never hard-coded
```

### Dependency Check Note

Dependabot alerts were enabled in the GitHub **Security and quality** tab. Since this repository has no external dependencies (`package.json` is not present), Dependabot is active but currently reports zero vulnerable dependencies. In a production project, `npm audit` would be run regularly to identify and fix known-vulnerable packages.

### Reflection on Secure Development

During this lab, I learned that secrets like API keys or database passwords should never be committed to source control, as doing so compromises them permanently. Instead, they should be stored securely in environment variables or a secrets manager. I also saw how regular code review and automated CI pipelines act as critical safety nets to catch poor coding practices, magic numbers, and missing input validations before they reach the main branch. Finally, enabling automated dependency checks (like Dependabot) is essential to detect and fix vulnerable open-source packages before they become an exploitable weakness in the system.

---

## Quick Test — Knowledge Check

### Multiple Choice

**Q1.** Which Git command creates a new branch AND switches to it in one step?

- (a) `git branch new && git checkout new`
- (b) `git checkout new`
- (c) `git switch -c new`
- (d) `git branch -m new`

**Q2.** A pull request (PR) is primarily used to:

- (a) propose changes and have them reviewed before merging
- (b) download code from a remote repository
- (c) create a new branch
- (d) delete a remote branch

**Q3.** The main purpose of CI/CD is to:

- (a) manage GitHub Issues
- (b) write better commit messages
- (c) create feature branches automatically
- (d) automatically build, test, and deploy changes

**Q4.** GitHub Actions workflow files must be stored in:

- (a) the root of the repository as `.yml` files
- (b) `.github/workflows/` as `.yml` files
- (c) the `src/` folder
- (d) a `ci/` directory at the root

**Q5.** Which of the following is a secure development practice?

- (a) commit API keys directly to the repository
- (b) share passwords over Slack
- (c) store secrets in environment variables and never commit them
- (d) use the same password for all services

---

### Git Command-Based

**Q6.** Your team asks you to stage all changed files and commit with the message `fix: correct total`. Write the two Git commands:

```
Answer:
```

---

### CI/CD

**Q7.** List three stages of a typical CI/CD pipeline in the correct order:

```
Answer:
```

---

### Security Scenario

**Q8.** A teammate accidentally pushes a commit containing a live API key to a public repository.  
Name the risk this creates and list three actions the team should take immediately:

```
Risk:

Action 1:

Action 2:

Action 3:
```

---

## Submission Instructions

The submission includes:

- `IT24102699_Lab08.docx` — Word document with all screenshots, notes, and Quick Test answers.
- This repository: [campuseats-task-tracker](https://github.com/umandathathsarani/campuseats-task-tracker)

> **The Golden Security Rule:** Never commit secrets (API keys, passwords, tokens) to Git.
> If a secret is ever pushed, treat it as **compromised** — remove it, rotate (regenerate) the key,
> and move it to an environment variable or secrets store. Deleting it in a later commit is not
> enough; it remains in the Git history.
