# **Simple App with ArgoCD** 🚀  

Aplikasi sederhana yang **dideploy menggunakan ArgoCD** langsung dari **GitHub** tanpa perlu build image sendiri.  

## **🛠 Teknologi yang Digunakan**  
✅ **ArgoCD** - GitOps untuk deployment otomatis.  
✅ **Kubernetes** - Menjalankan aplikasi dalam cluster.  
✅ **HashiCorp HTTP Echo** - Image siap pakai untuk testing.  

---

## **📂 Struktur Folder**  
```
simple-app/
│── k8s/
│   ├── deployment.yaml     # Deployment aplikasi
│   ├── service.yaml        # Service aplikasi
│── argocd-config/
│   ├── argocd-app.yaml     # ArgoCD Application 
│── .gitignore
│── README.md
```

---

## **🚀 Cara Deploy dengan ArgoCD**  

### **1️⃣ Tambahkan Repository ke ArgoCD**  
#### 🔹 **Via ArgoCD UI**  
1. **Login ke ArgoCD** (`http://<ARGOCD_SERVER>/`)  
2. Masuk ke **Settings → Repositories**  
3. Tambahkan repo GitHub:  
   - **URL**: `https://github.com/username/simple-app.git`  
   - Jika **private**, gunakan **username & token GitHub**  

#### 🔹 **Via `argocd CLI`**  
```sh
argocd repo add https://github.com/username/simple-app.git
```

Jika repo **private**, gunakan token GitHub:  
```sh
argocd repo add https://github.com/username/simple-app.git   --username <your-username>   --password <your-github-token>
```

---

### **2️⃣ Deploy Aplikasi dengan ArgoCD**  
Jalankan perintah berikut untuk deploy aplikasi:  

```sh
kubectl apply -f argocd-config/argocd-app.yaml
```

Cek status aplikasi:  

```sh
argocd app sync simple-app
```

---

## **📜 File s Kubernetes**  

### **🔹 Deployment (`k8s/deployment.yaml`)**  
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: simple-app
  labels:
    app: simple-app
spec:
  replicas: 2
  selector:
    matchLabels:
      app: simple-app
  template:
    metadata:
      labels:
        app: simple-app
    spec:
      containers:
        - name: simple-app
          image: hashicorp/http-echo:0.2.3  # Image siap pakai
          args:
            - "-text=Hello from ArgoCD!"
          ports:
            - containerPort: 5678
```

### **🔹 Service (`k8s/service.yaml`)**  
```yaml
apiVersion: v1
kind: Service
metadata:
  name: simple-app-service
spec:
  selector:
    app: simple-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 5678
  type: LoadBalancer
```

---

## **📌 Hasil Akhir**  
🎯 **Aplikasi akan otomatis terdeploy & terupdate** dari GitHub ke Kubernetes melalui ArgoCD! 🚀  

---

## **📚 Referensi**  
- [ArgoCD Official Docs](https://argo-cd.readthedocs.io/en/stable/)  
- [Kubernetes Official Docs](https://kubernetes.io/docs/)  
