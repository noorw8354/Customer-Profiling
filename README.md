# 📊 Customer Profiling & Engagement Scoring (PES)

This project builds a **Customer Profiling System** using **Salesforce Veeva CRM data**.  
It extracts **call activity**, **email interactions**, and **FSFA message engagement** to calculate a unified **Physician Engagement Score (PES)** at **Account, Country, Quarter, and Representative** level.

The workflow uses **PySpark for querying**, **Pandas for transformations**, and **Matplotlib/Seaborn for visualization**.

---

## 🚀 Project Workflow Overview

### **1. Data Extraction (PySpark SQL)**  
The project pulls 3 major datasets:

- **FSFA Calls (`fsfa_call`)**
- **Sent Emails (`fsfa_sentemail`)**
- **Sent FSFA Messages (`fsfa_sent_message`)**

Filtering includes:
- Submitted & non-deleted records  
- Country filtering via 2-digit MarketCode  
- Dates >= 2024-01-01  
- Delivered status for emails  

---

## 🔧 2. Data Processing (Pandas)

### **Call Data Transformations**
- Convert call timestamps  
- Generate rolling activity windows:
  - Last **30 days**
  - Last **60 days**
  - Last **90 days**
- Convert boolean to numeric for fast aggregation  
- Aggregate by:

```
Account, Quarter, Country → total_calls, calls_l30, calls_l60, calls_l90, calls_with_sample
```

---

### **Email Data Transformations**
- Convert open/click metrics to integers  
- Aggregate by:

```
Account, Quarter, Country → total_emails, opened_emails, clicked_emails
```

- Compute **Email CTR**

---

### **FSFA Message Transformations**
- Convert click values  
- Aggregate by:

```
Account, Quarter, Country → total_messages, clicked_messages
```

- Compute **Message CTR**

---

## 🧩 3. Final Dataset Construction

Datasets are merged using:

```
df_call  ←→  df_email  ←→ df_msg
```

Extra enrichments include:

- Mapping Country → Market  
- Randomly generating recommendations (placeholder)  
- Filling missing values  

---

## ⭐ 4. PES (Physician Engagement Score)

A composite metric was created:

```
PES =
( total_calls * 0.6 ) +
( total_emails * 0.2 ) +
( opened_emails * 0.3 ) +
( clicked_emails * 0.5 ) +
( total_messages * 0.2 ) +
( clicked_messages * 0.5 )
```

This score represents a **holistic engagement measure** across calls, emails, and messaging touchpoints.

---

## 👤 5. Rep Mapping

Using the earliest call record for each account:

```
Account → Owner → Representative Name
```

This enables **rep-level profiling**.

---

## 📈 6. Pivoted Summary KPIs

### **Country-level PES**
```
Pivot → index = [Market, Country], columns = quarter
```

### **Rep-level PES**
```
Pivot → index = [Market, Country, Name], columns = quarter
```

---

## 🔥 7. Visualizations (Heatmaps)

### **Country-level PES heatmap**
- Static heatmap for all countries

### **Rep-level PES heatmap**
- Dynamically resized per country  
- Visualizes quarter-wise PES by rep  

---

## 📦 Technology Stack

| Component | Tools Used |
|----------|------------|
| Data Extraction | PySpark SQL |
| Data Processing | Pandas, NumPy |
| Scoring Engine | Python |
| Visualization | Matplotlib, Seaborn |
| Database | Veeva CRM (fsfa tables) |

---

## 📁 Project Purpose

This project is designed to:

- Build a **unified profiling dataset** for healthcare professionals  
- Evaluate **rep effectiveness**  
- Score accounts using **multi-channel engagement metrics**  
- Help Sales & Marketing teams take **data-driven decisions**  
- Provide insights into **Quarter-wise engagement trends**  

---

## 📌 Future Enhancements

- Replace dummy recommendation model with ML-based prioritization  
- Include segmentation (A/B/C customers)  
- Integrate with Power BI dashboards  
- Build automated PySpark → Delta pipelines  

---

## 🙌 Author

**Noor Wali**  
Expert Data Analyst — Middle East & Africa  
Haleon  
