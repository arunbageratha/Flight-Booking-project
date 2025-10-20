# ✈️ Flight Booking CI/CD Pipeline (GitHub Actions)

This **GitHub Actions** workflow automates the **Continuous Integration and Continuous Deployment (CI/CD)** process for the **Flight Booking project** across **development (dev)** and **production (prod)** environments in **Google Cloud Platform (GCP)**.

---

## 🚀 Workflow Trigger

The pipeline triggers automatically on every **push** to either branch:

- **`dev`** → Deploys to **Airflow DEV** environment  
- **`main`** → Deploys to **Airflow PROD** environment  

---

## ⚙️ Key Functions

### **1. Code Checkout**
Retrieves the latest repository code using  
`actions/checkout@v3`.

---

### **2. GCP Authentication & Setup**
- Authenticates securely to GCP using the service account key stored in GitHub Secrets (`GCP_SA_KEY`).  
- Initializes the **Google Cloud SDK** with the project ID (`GCP_PROJECT_ID`).

---

### **3. Airflow Variable Management**
- Uploads the `variables.json` file to the **Cloud Composer GCS bucket** corresponding to each environment:
  - **DEV** → `variables/dev/variables.json`
  - **PROD** → `variables/prod/variables.json`
- Imports the variables into the **Airflow environment** using the command:  
  ```bash
  gcloud composer environments run <env-name> --location us-central1 variables import -- /home/airflow/gcs/data/<env>/variables.json
