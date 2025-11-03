# 🧩 Task 3 — Kubernetes Deployment (Mini-Infra Challenge)

## 🎯 Objective

Deploy and scale a **custom Nginx web server** using Kubernetes to understand **deployment**, **service exposure**, **ConfigMaps**, and **scaling**.

---

## 🧰 STEP 1 — Install Minikube

Minikube lets you run a **local Kubernetes cluster** on your PC.

### 🔧 Installation (Windows - PowerShell as Administrator)

If you have [Chocolatey](https://chocolatey.org/install):

```bash
choco install minikube
```

Or install manually from the official docs:
👉 [https://minikube.sigs.k8s.io/docs/start/](https://minikube.sigs.k8s.io/docs/start/)

---

## ⚙️ STEP 2 — Start Minikube

Open **PowerShell** (not admin) and run:

```bash
minikube start
```

This will:

* Create a **local Kubernetes cluster**
* Start one node (the control plane)
* Automatically configure `kubectl` to talk to your cluster

---

## 🧠 STEP 3 — Verify Cluster Setup

Check that your cluster is active:

```bash
kubectl get nodes
```

Expected output:

```
NAME       STATUS   ROLES           AGE   VERSION
minikube   Ready    control-plane   2m    v1.33.0
```

---

## 📝 STEP 4 — Create Project Files

Inside your `Task3` folder, create two files:

### 🗂️ `index.html`

This will be your custom webpage content.

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Welcome to My Kubernetes Nginx!</title>
  </head>
  <body style="font-family: Arial; text-align: center; margin-top: 50px;">
    <h1>🚀 Hello from Kubernetes!</h1>
    <p>This custom page is served by Nginx running in a Kubernetes Pod.</p>
  </body>
</html>
```

---

### 🧾 `deployment.yaml`

This file defines your **Nginx Deployment** and **NodePort Service**.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
        - name: nginx
          image: nginx:latest
          ports:
            - containerPort: 80
          volumeMounts:
            - name: html
              mountPath: /usr/share/nginx/html
      volumes:
        - name: html
          configMap:
            name: mywebpage
---
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  type: NodePort
  selector:
    app: nginx
  ports:
    - port: 80
      targetPort: 80
      nodePort: 30007
```

---

## 📦 STEP 5 — Create ConfigMap for the Webpage

Run this to create a ConfigMap from your `index.html`:

```bash
kubectl create configmap mywebpage --from-file=index.html
```

Verify:

```bash
kubectl get configmaps
```

Expected output:

```
NAME               DATA   AGE
kube-root-ca.crt   1      5m
mywebpage          1      10s
```

---

## 🚀 STEP 6 — Deploy to Cluster

Apply your configuration:

```bash
kubectl apply -f deployment.yaml
```

Check if your pods and service are running:

```bash
kubectl get pods
kubectl get svc
```

You should see:

```
NAME                              READY   STATUS    RESTARTS   AGE
nginx-deployment-xxxxx            1/1     Running   0          10s

NAME            TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
kubernetes      ClusterIP   10.96.0.1       <none>        443/TCP        2m
nginx-service   NodePort    10.109.210.70   <none>        80:30007/TCP   20s
```

---

## ⚖️ STEP 7 — Scale the Deployment

Increase the replicas to 2:

```bash
kubectl scale deployment nginx-deployment --replicas=2
```

Check again:

```bash
kubectl get pods
```

Expected output:

```
NAME                              READY   STATUS    RESTARTS   AGE
nginx-deployment-xxxxxx           1/1     Running   0          1m
nginx-deployment-yyyyyy           1/1     Running   0          5s
```

---

## 🌐 STEP 8 — Access the Application

To access the running Nginx site:

```bash
minikube service nginx-service
```

This command:

* Starts a **tunnel** (because you’re on Windows + Docker)
* Displays the URL (like `http://127.0.0.1:57276`)
* Automatically opens your browser

You’ll now see your custom `index.html` page served from the Nginx pod 🎉

---

## 🧹 STEP 9 — Clean Up (Optional)

To delete resources:

```bash
kubectl delete -f deployment.yaml
kubectl delete configmap mywebpage
```

Stop the cluster:

```bash
minikube stop
```

Delete the cluster completely:

```bash
minikube delete
```

---

## 🧾 Outcome

✅ Working **custom Nginx deployment**
✅ Exposed using a **NodePort service**
✅ Served a **custom `index.html` page** via ConfigMap
✅ Scaled replicas to **2 pods**
✅ Verified in browser via `minikube service`