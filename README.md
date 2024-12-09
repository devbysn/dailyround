### README: Kubernetes Ingress with Rate Limiting and Custom Logging

---

#### **1. Prerequisites**
- Install the following:
  - [Minikube](https://minikube.sigs.k8s.io/docs/start/)
  - [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/)
  - [Helm](https://helm.sh/docs/intro/install/)

---

#### **2. Setup Instructions**

1. **Start Minikube:**
   ```bash
   minikube start
   ```

2. **Deploy Web Applications:**
   - Apply `app1.yaml`:
     ```bash
     kubectl apply -f app1.yaml
     ```
   - Apply `app2.yaml`:
     ```bash
     kubectl apply -f app2.yaml
     ```

3. **Install NGINX Ingress:**
   ```bash
   `kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/cloud/deploy.yaml
``

4. **Apply Ingress Configuration:**
   ```bash
   kubectl apply -f ingress.yaml
   ```

5. **Get Minikube IP:**
   ```bash
   minikube ip
   ```
   Use this IP to access the endpoints.

---

#### **3. Endpoints**
- **App2 (Version 2):** `http://<minikube-ip>/v2`
- **App1 (Version 1):** `http://<minikube-ip>/v1`
- **Any Other Route:** Returns `404`

---

#### **4. Logs**
To view logs with custom header `X-Client-Id`:
```bash
kubectl logs -l app.kubernetes.io/name=ingress-nginx -n ingress-nginx
```

---

#### **5. Rate Limiting**
- **Limit:** 5 requests/min per IP + `X-Client-Id`
- **Exceeding Requests:** Returns HTTP `429` (Too Many Requests).
