# 🧩 Task 1 — Docker + Jenkins Mini Pipeline

## 🎯 Objective

Demonstrate a simple **CI/CD pipeline** using **Jenkins** and **Docker** that automatically builds and runs a containerized web app.

---

## 🧠 Overview

In this task, we:

1. Created a simple Flask web app returning *“Hello DevOps”*.
2. Wrote a **Dockerfile** to containerize the app.
3. Configured a **Jenkins pipeline** that:

   * Pulls code from GitHub
   * Builds the Docker image
   * Runs the container automatically

---

## 📁 Folder Structure

```
Task1/
├── app.py
├── requirements.txt
├── Dockerfile
├── Jenkinsfile
```

---

## 🧱 STEP 1 — Create the Web App

**File:** `app.py`

```python
from flask import Flask

app = Flask(__name__)

@app.route('/')
def home():
    return "Hello DevOps!"

if __name__ == '__main__':
    app.run(host='0.0.0.0', debug=True, port=5000)
```

**File:** `requirements.txt`

```
flask
```

---

## 🐳 STEP 2 — Create the Dockerfile

**File:** `Dockerfile`

```dockerfile
FROM python:3.9-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install -r requirements.txt

COPY . .

EXPOSE 5000

CMD ["python", "app.py"]
```

This Dockerfile:

* Uses a lightweight Python 3.9 base image
* Installs Flask
* Copies the source code
* Exposes port `5000`
* Starts the app using `app.py`

---

## ⚙️ STEP 3 — Build & Test Docker Container (Optional Local Test)

Before automating with Jenkins, you can test manually:

```bash
docker build -t hello-devops .
docker run -d -p 5000:5000 hello-devops
```

Open in browser:
👉 [http://localhost:5000](http://localhost:5000)
You should see:
**Hello DevOps!**

---

## 🧩 STEP 4 — Jenkins Pipeline Setup

### ✅ Prerequisites

* Jenkins installed and running
* Docker installed and accessible to Jenkins
* GitHub repo connected

---

### 🧾 Jenkinsfile

**File:** `Jenkinsfile`

```groovy
pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/onkar69483/DevOpsLab3.git', branch: 'main'
            }
        }

        stage('Build Docker Image') {
            steps {
                dir('ESE/Task1') {
                    bat 'docker build -t hello-devops .'
                }
            }
        }

        stage('Run Container') {
            steps {
                dir('ESE/Task1') {
                    bat 'docker rm -f hello-devops || true'
                    bat 'docker run -d -p 5000:5000 --name hello-devops hello-devops'
                }
            }
        }
    }
}
```

---

### 🔍 Pipeline Explanation

| Stage                  | Description                                                                   |
| ---------------------- | ----------------------------------------------------------------------------- |
| **Checkout**           | Pulls source code from GitHub repository.                                     |
| **Build Docker Image** | Builds a Docker image named `hello-devops` from the `Dockerfile`.             |
| **Run Container**      | Removes any old container (if exists) and starts a new one mapping port 5000. |

---

## 🧠 STEP 5 — Run Jenkins Pipeline

1. Open Jenkins → *New Item* → *Pipeline*
2. Add pipeline script from SCM (Git) and specify your repo URL.
3. Click **Build Now**.
4. Jenkins will automatically:

   * Clone the repo
   * Build the Docker image
   * Run the container

---

## 🌐 STEP 6 — Verify the Output

After the build succeeds, open in your browser:
👉 [http://localhost:5000](http://localhost:5000)

You should see:
**Hello DevOps!**

---

## 📸 STEP 7 — Screenshots to Capture (for submission)

* ✅ Jenkins build success screen
* ✅ Docker container running logs
* ✅ Browser output showing “Hello DevOps”

---

## 🧹 STEP 8 — Cleanup (Optional)

To stop and remove the container:

```bash
docker rm -f hello-devops
```

To remove the image:

```bash
docker rmi hello-devops
```

---

## 🧾 Outcome

✅ Simple Flask web app running inside a Docker container
✅ Jenkins pipeline successfully automates build and run steps
✅ Verified web output via `localhost:5000`