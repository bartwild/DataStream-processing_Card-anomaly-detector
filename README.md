# 💳 Real-Time Card Transaction Anomaly Detector

This project is a **stream processing system** for detecting **anomalous behavior in card transactions** using Apache Flink, Kafka, Redis, and Docker. It processes transactions in real time and raises alerts based on custom rule-based anomaly detectors.

> 🚀 Built entirely in **Python**, containerized using **Docker Compose**, and includes a fully interactive **visualization module** with real-time map rendering.

---

## 👨‍💻 Authors

- Dawid Bartosiak  
- Zuzanna Godek

---

## 🧠 System Overview

The system detects the following **10 types of card transaction anomalies**:

1. **TransactionTenTimesTheAverage** – Transaction 10× higher than historical average  
2. **NegativeTransaction** – Transaction with a negative amount  
3. **LimitExceeded** – Exceeds available card limit  
4. **VeryHighValue** – Transaction exceeds 10,000 PLN  
5. **ImpossibleTravel** – Movement speed between transactions > 900 km/h  
6. **RapidTransactions** – Multiple transactions <10s apart  
7. **PinAvoidance** – Series of transactions just below 100 PLN  
8. **ManyTransactionsNoPin** – ≥5 transactions without PIN in a window  
9. **DormantCardActivity** – Sudden activity after long dormancy  
10. **MultiCardDistance** – Different cards of same user used far apart in short time  

---

## ⚙️ Architecture & Components
+-------------+ +-----------+ +---------+ +--------------+
| Redis DB | <---> | Generator | ---> | Kafka | ---> | Apache Flink |
+-------------+ +-----------+ +---------+ +--------------+
|
v
+---------------+
| Visualizer UI |
+---------------+

### 🗃 Redis
- Stores card data: `card_id`, `user_id`, location, statistical parameters, limits.

### 🧾 Generator
- Produces synthetic transactions (normal + anomalies).
- Publishes to Kafka topic.
- Pulls baseline stats from Redis.

### ⚡ Apache Flink
- Detects anomalies in **streaming mode**.
- Implements detectors as modular Python classes.
- Uses **Sliding** and **Tumbling windows** per card or per user.
- Outputs alerts to Kafka.

### 📊 Visualizer
- 4 display modes: by user, by card, by type of anomaly.
- Real-time transaction map.
- Built with interactive stream merging and live updates.

---

## 🐳 Dockerized Stack

Everything is containerized via `docker-compose`:

- Apache Kafka + Kafka UI
- Apache Flink (TaskManager + JobManager)
- Redis + Redis Commander
- Generator + Visualizer (Python)
- Custom Flink image with Python & Kafka connectors

---

## 🖥️ Run the System

### 🛠 Setup (first time)
```bash
./scripts/t_setup.sh
```
### ▶️ Launch the system
```bash
./scripts/t_run.sh
```
### 🛠 Debug containers (optional)
```bash
./scripts/debug.sh
```
