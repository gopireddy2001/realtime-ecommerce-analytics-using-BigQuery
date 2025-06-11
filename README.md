# 🛍 Real-Time E-Commerce Analytics with BigQuery, Python & Looker Studio

Track and visualize real-time e-commerce performance using a cloud-based pipeline with:

* 📊 **Live BigQuery Data Ingestion** via Python & Colab
* 📈 **Interactive Looker Studio Dashboards**
* ☁️ Google Cloud-native setup (no local infra needed)

---

## 🚀 Features

* Simulates real-time e-commerce orders using Python and Faker
* Streams data into Google BigQuery in near real-time
* Visualizes insights in Looker Studio:

  * Total Revenue, Orders
  * Sales Trends
  * Category Breakdown
  * Stock Alerts

---

## 🧰 Tech Stack

| Tool            | Purpose                          |
| --------------- | -------------------------------- |
| Python (Colab)  | Simulate and push real-time data |
| Google BigQuery | Cloud data warehouse             |
| Looker Studio   | Real-time visual dashboards      |
| Faker Library   | Generate fake product/order data |

---

## 📦 Folder Structure

```bash
.
├── colab_script.ipynb         # Python script that generates and inserts fake orders
├── queries/                   # SQL queries for insights
│   └── category_revenue.sql
├── looker_design_guide.md     # Tips to create the dashboard layout
├── screenshots/               # (Optional) Add images of your Looker dashboard
└── README.md
```

---

## ⚙️ Setup Instructions

### 🔑 Step 1: Enable Google Cloud Services

* Go to [Google Cloud Console](https://console.cloud.google.com)
* Create a project (e.g., `realtimeecommerceanalytics`)
* Enable **BigQuery API**
* Create dataset: `ecommerce_data`
* Create table: `orders`

Schema example:

```bash
order_id: STRING  
timestamp: TIMESTAMP  
product_id: STRING  
category: STRING  
price: FLOAT  
quantity: INTEGER  
location: STRING  
stock_remaining: INTEGER
```

---

### 💻 Step 2: Run Python Script on Google Colab

```python
# In colab_script.ipynb
from google.cloud import bigquery
from faker import Faker
import random
import pandas as pd
from datetime import datetime

# Configure your BigQuery client and table ID
client = bigquery.Client()
table_id = "your-project-id.ecommerce_data.orders"

# Function to simulate an order
def generate_fake_order():
    fake = Faker()
    return {
        "order_id": fake.uuid4(),
        "timestamp": datetime.utcnow().isoformat(),
        "product_id": str(random.randint(1000, 9999)),
        "category": random.choice(["Electronics", "Fitness", "Clothing", "Home"]),
        "price": round(random.uniform(10.0, 500.0), 2),
        "quantity": random.randint(1, 5),
        "location": fake.city(),
        "stock_remaining": random.randint(0, 50)
    }

# Insert into BigQuery
def insert_order_into_bq(order):
    errors = client.insert_rows_json(table_id, [order])
    if errors == []:
        print("✅ Order inserted")
    else:
        print("❌ Errors:", errors)

# Stream 1 order every few seconds
for _ in range(5):
    insert_order_into_bq(generate_fake_order())
```

🔒 **Note:** Use a service account key if running outside Google Colab.

---

### 📊 Step 3: Build Looker Studio Dashboard

1. Go to [Looker Studio](https://lookerstudio.google.com/)
2. Create → Report → Connect to BigQuery
3. Select your table: `ecommerce_data.orders`
4. Add visualizations:

   * **Line Chart** → Sales Over Time
   * **Bar Chart** → Quantity by Category
   * **Pie Chart** → Revenue by Location
   * **Table** → Products with Low Stock (`stock_remaining < 10`)
5. Add filters: Date range, Category dropdown, etc.
6. (Optional) Create calculated field: `Revenue = price * quantity`

📌 See `looker_design_guide.md` for design inspiration.

---

## 📸 Example Dashboard

---

## 📈 Sample SQL Query

```sql
SELECT 
  category,
  COUNT(order_id) AS total_orders,
  ROUND(SUM(price * quantity), 2) AS total_revenue
FROM `your-project-id.ecommerce_data.orders`
GROUP BY category
ORDER BY total_revenue DESC
```

---

## 🧠 Insights You Can Extract

* 🔍 Top-selling product categories
* 📉 Products running out of stock
* 🕐 Peak sales hours/days
* 💰 Average price per category

---

## 📣 Share Your Project

This makes a great portfolio project!
You can also https://www.linkedin.com/in/gurukavyagopireddy/ using the highlights in this repo.

---

