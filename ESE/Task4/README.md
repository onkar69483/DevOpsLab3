## 🧠 Overview

In this task, we:

* Scanned a Docker image for known security vulnerabilities.
* Identified affected system libraries and packages.
* Proposed remediation steps to secure the image.
* Documented findings in this report.

---

## 🧰 STEP 1 — Install Trivy (Open-Source Security Scanner)

Trivy is a lightweight vulnerability scanner for containers and file systems.

### 🔧 Installation (Windows via Chocolatey)

```bash
choco install trivy
```

If you’re on Linux or macOS, follow official installation docs:
👉 [https://aquasecurity.github.io/trivy/v0.54.1/getting-started/installation/](https://aquasecurity.github.io/trivy/v0.54.1/getting-started/installation/)

Verify installation:

```bash
trivy --version
```

---

## ⚙️ STEP 2 — Build Your Docker Image

Navigate to your **Task4** folder and build your existing app image.

Example:

```bash
cd D:\Assignments\DevOpsLab3\ESE\Task4
docker build -t secure-app .
```

Check that the image exists:

```bash
docker images
```

---

## 🔍 STEP 3 — Scan the Docker Image

Run a Trivy scan on the image:

```bash
trivy image secure-app
```

Trivy will analyze:

* The base operating system packages (Debian, Alpine, etc.)
* Installed Python or Node packages
* Configuration issues

You’ll see a vulnerability summary like this:

```
secure-app (debian 13.1)
========================
Total: 52 (LOW: 51, MEDIUM: 1, HIGH: 0, CRITICAL: 0)
```

---

## 📊 STEP 4 — Analyze Findings

From the scan:

* **Low severity:** Minor library vulnerabilities
* **Medium severity:** One vulnerability in system packages
* **No High or Critical vulnerabilities** ✅
* Most issues came from core libraries like:

  * `glibc`
  * `coreutils`
  * `bash`
  * `util-linux`

These are part of the **Debian base image**, not your Flask app.

---

## 🧹 STEP 5 — Apply Fixes

To harden the image and reduce vulnerabilities:

### ✅ **Fix 1 — Use a smaller base image**

Replace:

```dockerfile
FROM debian:13.1
```

with:

```dockerfile
FROM python:3.9-slim
```

or even better:

```dockerfile
FROM python:3.9-alpine
```

This drastically reduces OS-level vulnerabilities.

---

### ✅ **Fix 2 — Update system packages**

Add this before copying your app:

```dockerfile
RUN apt-get update && apt-get upgrade -y && apt-get clean
```

This ensures all Debian security patches are installed.

---

### ✅ **Fix 3 — Optimize pip install**

Use:

```dockerfile
RUN pip install --no-cache-dir -r requirements.txt
```

to remove cache layers and keep the image clean.

---

### ✅ **Fix 4 — Rebuild and Rescan**

After making changes:

```bash
docker build -t secure-app-fixed .
trivy image secure-app-fixed
```

You should see a significantly lower vulnerability count.

---

## 📁 Recommended Folder Structure

```
Task4/
├── Dockerfile
├── app.py
├── requirements.txt
├── README.md
└── Jenkinsfile (optional)
```

---

## 🧾 Outcome

✅ Performed Docker image security scan
✅ Identified 52 vulnerabilities (Low/Medium only)
✅ No High/Critical CVEs found
✅ Proposed effective security fixes
✅ Demonstrated awareness of **container security best practices**

