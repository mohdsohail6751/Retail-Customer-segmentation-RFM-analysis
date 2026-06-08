# 🛒 Retail Customer Segmentation & RFM Analysis

![Python](https://img.shields.io/badge/Python-3.11-blue?logo=python)
![Power BI](https://img.shields.io/badge/PowerBI-Dashboard-yellow?logo=powerbi)
![Status](https://img.shields.io/badge/Status-Complete-brightgreen)
![Dataset](https://img.shields.io/badge/Dataset-UCI%20Online%20Retail-orange)

> A complete end-to-end data science project that segments retail customers using **RFM Analysis** and **K-Means Clustering**, with an interactive **Power BI Dashboard**.

---

## 📌 Project Overview

Customer segmentation helps businesses target the right customers with the right message at the right time. This project uses the **UCI Online Retail dataset** (541,909 transactions) to:

- Calculate **Recency, Frequency, Monetary (RFM)** metrics per customer
- Score and segment customers into **9 business segments**
- Apply **K-Means Clustering** to find data-driven groups
- Visualize everything in a **3-page Power BI Dashboard**

---

## 📁 Project Structure

```
RFM-Customer-Segmentation/
│
├── 📓 RFM_Customer_Segmentation.ipynb   ← Main Jupyter Notebook
├── 📊 rfm_results.csv                   ← Exported RFM scores per customer
├── 📈 RFM_Dashboard.pbix                ← Power BI Dashboard file
├── 📄 RFM_Project_Report.docx           ← Full project report (Word)
├── 📽️ RFM_Presentation.pptx             ← Project presentation slides
│
├── plots/
│   ├── rfm_distributions.png            ← R, F, M histograms
│   ├── segment_distribution.png         ← Pie + bar chart of segments
│   ├── rfm_heatmap.png                  ← RFM score heatmap by segment
│   ├── elbow_silhouette.png             ← Elbow method + silhouette scores
│   ├── snake_plot.png                   ← Cluster comparison snake plot
│   └── monthly_revenue.png             ← Revenue trend over time
│
└── README.md
```

---

## 📦 Dataset

| Attribute | Value |
|---|---|
| **Source** | [UCI Machine Learning Repository](https://archive.ics.uci.edu/dataset/352/online+retail) |
| **Also on** | [Kaggle](https://www.kaggle.com/datasets/jihyeseo/online-retail-data-set-from-uci-ml-repo) |
| **Rows** | 541,909 |
| **Unique Customers** | ~4,338 |
| **Date Range** | Dec 2010 – Dec 2011 |
| **Country** | United Kingdom (+ 37 others) |

### Columns
| Column | Description |
|---|---|
| `InvoiceNo` | Transaction ID (prefix 'C' = cancellation) |
| `StockCode` | Product code |
| `Description` | Product name |
| `Quantity` | Units purchased |
| `InvoiceDate` | Transaction date & time |
| `UnitPrice` | Price per unit (£) |
| `CustomerID` | Unique customer identifier |
| `Country` | Customer's country |

---

## 🧹 Data Cleaning Steps

1. Dropped rows with missing `CustomerID`
2. Removed duplicate rows
3. Removed cancelled orders (InvoiceNo starting with `'C'`)
4. Removed rows where `Quantity <= 0` or `UnitPrice <= 0`
5. Created `TotalPrice = Quantity × UnitPrice`

---

## 📐 RFM Analysis

| Metric | Formula | Meaning |
|---|---|---|
| **Recency** | Days since last purchase | Lower = more recent = better |
| **Frequency** | Count of unique invoices | Higher = more loyal |
| **Monetary** | Sum of TotalPrice | Higher = more valuable |

Each metric is scored **1–5** using quantile bins (`pd.qcut`):
- Recency is **reversed** (lower days → higher score)
- Combined score string e.g. `"555"` = Champion

---

## 🏷️ Customer Segments

| Segment | Description | Action |
|---|---|---|
| **Champions** | Best customers — recent, frequent, high spend | Reward & upsell |
| **Loyal Customers** | Regular buyers with good scores | Loyalty programs |
| **Potential Loyalists** | Recent but infrequent | Nurture & engage |
| **New Customers** | Very recent, first-time buyers | Onboarding offers |
| **About To Sleep** | Declining activity | Re-engagement |
| **At Risk** | Once loyal, now inactive | Win-back campaigns |
| **Cannot Lose Them** | High value but going cold | Urgent outreach |
| **Hibernating** | Low R, F, M scores | Strong discounts |
| **Lost Customers** | Lowest scores, long inactive | Last-chance email |

---

## 🤖 K-Means Clustering

- **Preprocessing:** Log transformation + StandardScaler
- **Method:** `scipy.cluster.vq` (Windows-compatible, avoids OpenBLAS issues)
- **Optimal K:** 4 (determined via Elbow method)

| Cluster | Label | Profile |
|---|---|---|
| 0 | VIP / Champions | Low recency, high frequency, high monetary |
| 1 | Lost / Inactive | High recency, low frequency, low monetary |
| 2 | Loyal / Regular | Medium scores across all metrics |
| 3 | At Risk / Sleeping | Medium-high recency, low frequency |

---

## 📊 Power BI Dashboard

3-page interactive dashboard built from `rfm_results.csv`:

- **Page 1 — Overview:** KPI cards, pie chart, bar chart by segment
- **Page 2 — RFM Analysis:** Clustered bar, matrix table, scatter plot
- **Page 3 — Cluster Analysis:** Donut chart, bar chart, scatter plot

---

## 🛠️ Tech Stack

| Tool | Purpose |
|---|---|
| Python 3.11 | Core language |
| Pandas, NumPy | Data processing |
| Matplotlib, Seaborn | Static visualization |
| Plotly | Interactive 3D scatter |
| SciPy | K-Means clustering |
| Scikit-learn | Data scaling |
| Jupyter Notebook | Development |
| Power BI Desktop | Dashboard |

---

## 🚀 How to Run

### 1. Clone the repository
```bash
git clone https://github.com/yourusername/RFM-Customer-Segmentation.git
cd RFM-Customer-Segmentation
```

### 2. Install dependencies
```bash
pip install pandas numpy matplotlib seaborn scikit-learn plotly scipy openpyxl
```

### 3. Download the dataset
Download from [Kaggle](https://www.kaggle.com/datasets/jihyeseo/online-retail-data-set-from-uci-ml-repo) and place `Online Retail.xlsx` in the project folder.

### 4. Run the notebook
```bash
jupyter notebook RFM_Customer_Segmentation.ipynb
```

### 5. Open the dashboard
Open `RFM_Dashboard.pbix` in **Power BI Desktop** and load your `rfm_results.csv`.

---

## ⚠️ Known Issue (Windows)

If you get `AttributeError: 'NoneType' object has no attribute 'split'` with scikit-learn KMeans:

**Fix:** Use `scipy.cluster.vq` instead:
```python
from scipy.cluster.vq import kmeans, vq
centroids, _ = kmeans(rfm_scaled_arr, 4, iter=100)
rfm['Cluster'], _ = vq(rfm_scaled_arr, centroids)
```

---

## 📈 Key Findings

- **Champions** represent the highest revenue customers — prioritize retention
- **37%+ of customers** are Lost or Hibernating — re-engagement campaigns needed
- **Cannot Lose Them** segment: historically high-value but recently inactive — urgent action required
- **K=4 clusters** cleanly separated VIP, Regular, At-Risk, and Lost customers

---

## 📄 License

This project is for educational purposes. Dataset is publicly available from UCI ML Repository.

---

## 🙋 Author

**Mohd. Sohail**
Data Science Project — Retail Customer Segmentation & RFM Analysis
