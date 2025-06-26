## 🖥️ 1. Launch EC2 Instance
- Launch an **Ubuntu EC2 instance**
- Name it: `EKS-Main`
---

## ☸️ 2. Create EKS Cluster
- Use the AWS Console or CLI to create an EKS Cluster.
- **Wait 15–20 minutes** for the cluster to become active.
- Ensure the worker nodes are created and joined to the cluster.

---

## 🛢️ 3. Create RDS (MariaDB) Database
- Go to RDS Console → Create Database.
- Select **MariaDB** as the engine.
- Make sure the DB is created **in the same VPC** as your EKS cluster.


---

## 🔌 4. Connect to Worker Node and Setup DB
- SSH into one of the **EKS worker nodes**.
- Install MariaDB client:


* Connect to the RDS database:

  ```bash
  mysql -h <RDS-ENDPOINT> -P <PORT> -u <USERNAME> -p
  ```
* Inside MariaDB prompt, create database and tables:

---

## 🔐 5. Update Security Group for EKS Cluster

* Go to **EKS cluster's Security Group**.
* **Remove existing inbound rules.**
* **Add new inbound rule**:

  * Type: All Traffic
  * Source: 0.0.0.0/0 

---

## 📥 6. Clone the Project on EC2

SSH into your `EKS-Main` instance and run:

```bash
git clone https://github.com/abhipraydhoble/Project-StudentApp.git
cd Project-StudentApp/Backend
```

---

## ⚙️ 7. Configure Backend

* Edit the `context.xml` file:

  ```bash
  vim src/main/webapp/META-INF/context.xml
  ```
* Update it with:

  * `RDS Endpoint`
  * `Port`
  * `Database Name`
  * `Username`
  * `Password`

---

## 🐳 8. Build and Push Backend Docker Image

```bash
docker build -t <your-dockerhub-username>/studentapp-backend .
docker push <your-dockerhub-username>/studentapp-backend
```

---

## 📦 9. Update Backend Deployment

* Edit `deployment.yaml` in Backend directory:

  ```bash
  vim deployment.yaml
  ```

* Change the image name to:

  ```
  <your-dockerhub-username>/studentapp-backend
  ```

* Deploy to EKS:

  ```bash
  kubectl apply -f .
  ```

* Check service:

  ```bash
  kubectl get svc
  ```

---

## 🔁 10. Repeat Security Group Update

> ⚠️ **Reminder**: Again, update EKS security group to allow **All Traffic** and remove previous rules.

---

## 🌐 11. Access Backend

* Copy the **EXTERNAL-IP** of backend LoadBalancer.
* Access: `http://<svc-lb-link>:8080/student`

---

## 🎨 12. Configure Frontend

* Go to the `Frontend` directory:

  ```bash
  cd ../Frontend
  ```
* Edit `index.html`:

  ```bash
  vim index.html
  ```
* At **line 118**, paste the backend link (from above) for the student endpoint.

---

## 🐳 13. Build and Push Frontend Docker Image

```bash
docker build -t <your-dockerhub-username>/studentapp-frontend .
docker push <your-dockerhub-username>/studentapp-frontend
```

---

## 📦 14. Update Frontend Deployment

* Edit `deployment.yaml` in Frontend directory:

  ```bash
  vim deployment.yaml
  ```

* Change image to:

  ```
  <your-dockerhub-username>/studentapp-frontend
  ```

* Deploy to EKS:

  ```bash
  kubectl apply -f .
  ```

* Check service:

  ```bash
  kubectl get svc
  ```

---

## 🌐 15. Access Full StudentApp

* Copy **frontend LoadBalancer Link**
* Visit: `a1b2c3d4e5.elb.amazonaws.com`
* 🎉 You will see your **StudentApp** page!

---

