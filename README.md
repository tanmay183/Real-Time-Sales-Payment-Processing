# 🌐 **Real-Time-Sales-Payment-Processing** 🚀

---
![Architecture](https://github.com/user-attachments/assets/9d6ed287-1c9d-43d4-9928-ffe38e86adfb)

## 📌 **Table of Contents**
1. 🎯 **Introduction**
2. 🛠️ **Tech Stack**
3. 🏗️ **System Architecture**
4. 🔄 **Implementation Process**
   - 📱 **Initial Setup Steps**
   - 🌍 **Data Simulation**
   - 📰 **Pub/Sub Topics Setup**
   - 🏛️ **Cassandra Setup**
   - ⚙️ **Data Processing**
   - 🚨 **Error Handling & DLQ**
   - 🚀 **Execution Order**
5. 📂 **Scripts & Configurations**

---

## 🎯 **1. Introduction**

🔹 The e-commerce industry thrives on **real-time data processing**. This project showcases a **Real-Time Data Streaming & Ingestion Pipeline** that simulates **order and payment transactions**, processes them on the fly, and stores structured results in **Cassandra NoSQL**.

🔹 Key Technologies: **Google Cloud Pub/Sub, Python, Cassandra, Docker**.

### ✅ **Prerequisites**
💡 Before running this project, ensure you have:
✔ **Google Cloud Account** (Set up Pub/Sub topics & authentication)  
✔ **Python Environment** (`pip install -r requirements.txt`)  
✔ **Docker & Docker Compose** (For containerized Cassandra)  
✔ **Cassandra Database Initialized** (Schema setup provided)  
✔ **Basic Understanding of System Architecture**

---

## 🛠️ **2. Tech Stack**

| 🔹 Technology | 🏆 Purpose |
|--------------|------------|
| ☁ **Google Cloud Pub/Sub** | Real-time messaging system |
| 🐍 **Python** | Data production, consumption & processing |
| 📂 **Apache Cassandra** | NoSQL database for fast data retrieval |
| 🐳 **Docker** | Containerization for seamless deployment |

---

## 🏗️ **3. System Architecture**

⚡ The system follows a **real-time streaming architecture** with:
- **Producers** 📤 sending `order_data` & `payment_data`.
- **Google Cloud Pub/Sub** 📱 acting as a messaging bus.
- **Consumers** 📥 processing and storing data in **Cassandra**.
- **Dead Letter Queue (DLQ)** 🚨 handling unmatched payments.

🔄 **Flow:**
1️⃣ **Producers generate** order & payment data  
2️⃣ **Pub/Sub Topics** handle real-time messaging  
3️⃣ **Consumers process & store** data in Cassandra  
4️⃣ **DLQ captures errors** for unmatched transactions  

---

## 📱 **4. Implementation Process**

### 🔹 **Step 1: Initial Setup Steps**

1️⃣ **Google Cloud SDK Setup:** Ensure Google Cloud SDK is installed for `gcloud` CLI access.  
2️⃣ **Create Pub/Sub Topics:** Create topics named `orders_data`, `payments_data`, and `dlq_payments_data` with default subscribers.  
3️⃣ **Authenticate GCP Account:** Run:
```sh
gcloud auth application-default login
```
   This opens a browser where you select your registered GCP email. Once authenticated, the default credentials file is created at:
   ```sh
   /Users/shashankmishra/.config/gcloud/application_default_credentials.json
   ```
4️⃣ **IAM & Admin Setup:** Create a new service account and assign **Pub/Sub Producer** & **Pub/Sub Subscriber** roles.  
5️⃣ **Generate Service Account Keys:** Download the JSON key file, then replace its content in:
   ```sh
   /Users/shashankmishra/.config/gcloud/application_default_credentials.json
   ```

### 📰 **Step 2: Pub/Sub Topics Setup**
- `orders_data` – Order-related events 🛍️  
- `payments_data` – Payment transactions 💸  

### 🏛 **Step 3: Cassandra Setup**
#### 🛠 **Python Package Installation**
```sh
pip3 install google-cloud-pubsub cassandra-driver
```

#### 🚢 **Launch Cassandra Using Docker**
```sh
docker-compose -f docker-compose-cassandra.yml up -d
```
✅ **Once Cassandra starts, access it via:**
```sh
docker exec -it <container_id> cqlsh
```

#### 📂 **Create Cassandra Keyspace & Table**
```sql
CREATE KEYSPACE IF NOT EXISTS ecom_store 
WITH REPLICATION = { 'class' : 'SimpleStrategy', 'replication_factor' : 1 };

CREATE TABLE ecom_store.orders_payments_facts (
    order_id BIGINT PRIMARY KEY,
    customer_id BIGINT,
    item TEXT,
    quantity BIGINT,
    price DOUBLE,
    shipping_address TEXT,
    order_status TEXT,
    creation_date TEXT,
    payment_id BIGINT,
    payment_method TEXT,
    card_last_four TEXT,
    payment_status TEXT,
    payment_datetime TEXT
);
```

---

## 🚀 **5. Execution Order**
1️⃣ **Start Cassandra** 🏛️:
```sh
docker-compose up
```
2️⃣ **Run Producers** 🛠:
```sh
python orders_producer.py
python payments_producer.py
```
3️⃣ **Run Consumers** 📱:
```sh
python order_consumer.py
python payment_consumer.py
```
4️⃣ **Handle DLQ Records** ⚠️:
```sh
python dlq_handler.py
```

---

## 📂 **6. Scripts & Configurations**

| 🏷️ File | 💡 Description |
|--------|--------------|
| `orders_producer.py` | Generates & sends **mock order data** to Pub/Sub |
| `payments_producer.py` | Generates & sends **mock payment data** to Pub/Sub |
| `order_consumer.py` | Listens & processes **orders_data** messages |
| `payment_consumer.py` | Listens & processes **payments_data**, updates DB & handles DLQ |

---

## 🎉 **Conclusion**
This project **empowers real-time e-commerce operations** by ensuring **fast, reliable, and scalable data processing** using **Google Cloud Pub/Sub, Apache Cassandra, Python, and Docker**.

🚀 **Ready to revolutionize real-time data ingestion? Let’s deploy!** 🚀

