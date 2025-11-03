# 🧩 Task 2 — Git Branching & Merge Workflow

## 🎯 Objective

Validate version control practices using **Git branching**, **feature development**, and **merge workflow**.

---

## 🧠 Overview

In this task, we demonstrate a clean **Git workflow** by:

* Creating a Flask web application
* Managing multiple branches for different features
* Merging all work back into a development branch (`dev`)

This ensures a well-structured version control process aligned with real-world development practices.

---

## 📁 Project Structure

```
Task2/
├── app.py              # Main Flask application
├── requirements.txt    # Python dependencies
└── templates/
    ├── index.html      # Home page
    ├── login.html      # Login page
    └── signup.html     # Sign up page
```

---

## ⚙️ Step 1 — Initialize the Git Repository

Open your terminal and navigate to the project folder:

```bash
cd D:\Assignments\DevOpsLab3\ESE\Task2
git init
```

Add all project files and commit the base version:

```bash
git add .
git commit -m "Initial commit with basic project structure"
```

---

## 🌿 Step 2 — Create the Development Branch

Create and switch to the `dev` branch:

```bash
git checkout -b dev
```

---

## 🌱 Step 3 — Create Feature Branches

Now, create two feature branches for login and signup:

```bash
git checkout -b feature/login
git checkout dev
git checkout -b feature/signup
```

You should now have the following branches:

* `main`
* `dev`
* `feature/login`
* `feature/signup`

---

## 💻 Step 4 — Make Changes in Feature Branches

You can now work independently on each feature branch.

### ✳️ Example — `feature/login`

In this branch, edit **`templates/login.html`** to enhance the login form or add form fields.

Then commit your changes:

```bash
git add .
git commit -m "Added login page enhancements"
```

---

### ✳️ Example — `feature/signup`

In this branch, edit **`templates/signup.html`** to add validation or new design elements.

Then commit your changes:

```bash
git add .
git commit -m "Added form validation to signup page"
```

---

## 🔁 Step 5 — Merge Feature Branches into `dev`

Once both feature branches are complete, merge them into the `dev` branch.

```bash
git checkout dev
git merge feature/login
git merge feature/signup
```

If there are no conflicts, you’ll see a message like:

```
Merge made by the 'recursive' strategy.
```

If using GitHub, you can alternatively create **Pull Requests (PRs)** for each merge.

---

## 📜 Step 6 — Verify Merge Workflow

To confirm successful integration, check your branch history:

```bash
git log --oneline --graph --all
```

✅ You should see all commits from `feature/login` and `feature/signup` merged into `dev`.

---

## 🧾 Outcome

* Properly structured **Git branching model**
* Independent feature development on separate branches
* Clean integration into `dev` through merge or pull requests
* Version control workflow aligned with **CI/CD best practices**

## 🚀 Summary

| Step  | Description                                  |
| ----- | -------------------------------------------- |
| **1** | Initialize Git repo                          |
| **2** | Create `dev` and feature branches            |
| **3** | Make independent changes in feature branches |
| **4** | Merge branches into `dev`                    |
| **5** | Verify clean workflow                        |
