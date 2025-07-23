# 🧩 Kustomize Demo Example: Dev + Logging

This repository demonstrates how to structure a multi-environment **Kustomize** setup using **overlays**, without relying on components.

## 🔧 Features

- ✅ Base Nginx Deployment
- ✅ Dev overlay (replicas, labels, annotations)
- ✅ Logging overlay (adds config for logging)
- ✅ ConfigMap & Secret injection
- ✅ Uses modern Kustomize syntax (v5+)

---

## 🗂 Project Structure

kustomize-demo/
|-----base
    |--kustomization.yaml
    |--deployment.yaml
|-----overlays
    |---dev/
      |--kustomization.yaml
      |--dep-patch.yaml 
    |---logging/
      |--log-config.yaml
      |--kustomization.yaml 

# 📦 Requirements

### ✅1-kubectl

### ✅2-Kustomize (v5.x recommended)

#### ✅3-A running Kubernetes cluster (e.g., minikube, kind, or cloud)


## 🚀 Getting Started

### Apply Full Setup (Dev + Logging):
```bash
kubectl apply -k overlays/dev-logging
```

### Apply Individual Overlays:
```bash
kubectl apply -k overlays/dev
kubectl apply -k overlays/logging
```

---

## 🔁 How to Use Key Kustomize Features

### 🔢 1. Change Replicas
Use the `replicas` field in `kustomization.yaml`:
```yaml
replicas:
  - name: myapp
    count: 4
```
✅ This tells Kustomize to patch the deployment named `myapp` and set replicas to 4.

---

### 🏷️ 2. Add Labels
```yaml
commonLabels:
  env: dev
  tier: frontend
```
✅ This adds these labels to **all resources** in the overlay.

---

### 📝 3. Add Annotations
```yaml
commonAnnotations:
  maintained-by: dev-team
  purpose: test-deployment
```
✅ Annotations are helpful for tracking, documentation, and tools like Prometheus.

---

### 🔐 4. Inject ConfigMaps and Secrets
```yaml
configMapGenerator:
  - name: app-config
    literals:
      - app-mode=development
      - log-level=debug

secretGenerator:
  - name: app-secret
    literals:
      - db-user=admin
      - db-pass=supersecret

generatorOptions:
  disableNameSuffixHash: true
```
✅ This creates a `ConfigMap` and `Secret` that can be injected into pods using `envFrom` or `volumes`.
✅ `disableNameSuffixHash` prevents Kustomize from appending a random hash to the name.

---

### ✂️ 5. Prefix / Suffix Resource Names
```yaml
namePrefix: dev-
nameSuffix: -v1
```
✅ This results in resources like `dev-myapp-v1`.
✅ Useful for multi-tenant clusters or avoiding name collisions.

---


