# 🛵 Blinkit Sales & Delivery Analytics

End-to-end analytics project on a **10 million+ row Blinkit order dataset**, covering data cleaning, feature engineering, and exploratory data analysis in **Python (Pandas, NumPy, Matplotlib, Seaborn)**, with insights visualized in an interactive **Power BI dashboard**.

---

## 📌 Project Overview

Blinkit generates massive volumes of order-level data every day — spanning sales, discounts, delivery performance, and customer feedback. This project simulates a real-world analyst workflow: taking a raw, messy 10M-row dataset and turning it into clean, analysis-ready data and business-ready dashboards.

The project answers questions like:
- Which cities and product categories drive the most sales?
- How effective are discounts, and how do they relate to order quantity?
- Which delivery partner is fastest, and how does delivery time affect customer ratings?
- What are customers' preferred payment methods?

---

## 🧰 Tech Stack

| Tool | Purpose |
|---|---|
| **Python** (Pandas, NumPy) | Data cleaning & feature engineering |
| **Matplotlib / Seaborn** | Exploratory data analysis & visualizations |
| **Power BI** | Interactive dashboard for business stakeholders |
| **Jupyter Notebook** | Analysis workflow |

---

## 📂 Dataset

- **Source file:** `blinkit_10M.csv`
- **Scale:** ~10 million order records across **62 stores** in **6 cities** (Mumbai, Delhi, Bengaluru, Hyderabad, Pune, Chennai)
- **Raw columns include:** `order_id`, `store_id`, `customer_city`, `order_date`, `product_brand`, `product_category`, `mrp`, `selling_price`, `discount_pct`, `quantity`, `customer_rating`, `delivery_time_min`, `delivery_partner`, `payment_method`

> ⚠️ Note: Due to file size, the raw dataset is not hosted directly in this repo — see [Kaggle: Blinkit Sales Dataset](https://www.kaggle.com) or your original data source, and place it in the project root as `blinkit_10M.csv` before running the notebook.

---

## 🧹 Data Cleaning & Feature Engineering

All cleaning logic lives in `Blinkit_Python_codes.ipynb`. Key problems solved:

### 1. Missing values in `customer_city`
Instead of dropping or guessing, the city code was **extracted from `store_id`** (e.g. `STORE-MUM-01` → `MUM`) and mapped to full city names via a dictionary. This recovered 100% of the missing city data without any assumptions.

### 2. Delivery time categorization
`delivery_time_min` was bucketed into a new `Delivery_Category` column:
- **Fast** → ≤ 30 min
- **Moderate** → 31–60 min
- **Slow** → > 60 min

### 3. Missing customer ratings
Rather than imputing a fake rating, missing values were labeled **`"Rating not given"`** — preserving the signal that a customer chose not to rate their order (useful for future churn/experience analysis).

### 4. Date feature extraction
`order_date` was converted to `datetime` and split into `orderYear`, `orderMonth`, `orderMonth_name`, `orderDay`, `orderDay_name`, and `orderQuarter` to enable time-based trend analysis.

### 5. Fixing broken discount logic
The original `discount_pct` column didn't reconcile with `mrp` vs `selling_price`, so it was **dropped and rebuilt from scratch**:
- `Total_Discount` = `mrp - selling_price` (when selling price is lower)
- `Total_Hike` = `selling_price - mrp` (when selling price is higher)
- `Price_Category` = `Discounted` / `Hiked`

### 6. Quantity segmentation
`quantity` was bucketed into a `Quantity_Category` column:
- **Single** (1) · **Multiple** (2–5) · **Bulk** (>5)

Final cleaned dataset is exported as **`Blinkit_Final_data.csv`**.

---

## 📊 Key Insights

**Overall performance**
- **10M** total orders · **₹2Bn** total sales · **26M** units sold · **62** stores
- Average Selling Price: **₹156** · Average Order Value: **₹160**
- **98.5%** of items were sold at a discount vs. only 1.5% at a price hike — total discount given: **₹367M**

**Sales by geography**
- Mumbai leads (**₹226M**), followed closely by Delhi (**₹218M**) and Bengaluru (**₹200M**)
- Hyderabad, Pune, and Chennai form the next tier (₹150–160M each)

**Product performance**
- **Personal Care** (₹501M) and **Beverages** (₹470M) are the top revenue categories, followed by Household and Snacks
- By volume, **Beverages (10.1M units)** and **Snacks (8.8M units)** sell the most — high frequency, lower ticket size
- Top brands by sales: **Dove, Pepsi, Red Bull, Sprite, Himalaya**
- A minor data quality issue was observed: duplicate/near-duplicate category labels (e.g. `Snacks` vs `snack`, `Beverages` vs `Beverage`) suggesting inconsistent data entry upstream — flagged as a cleanup opportunity

**Payments**
- **UPI dominates at 47%**, followed by Card (30%) and Cash (22%) — reflecting India's digital payments shift

**Delivery performance**
- **97%+ of orders are delivered "Fast"** (≤30 min), validating Blinkit's quick-commerce promise
- All three delivery partners (Blinkit, Shadowfax, Zomato) perform almost identically (~13.7–13.8 min average)
- City-wise delivery time varies more than partner-wise: **Pune (15.4 min)** and **Hyderabad (15.0 min)** are slowest, **Chennai (12.0 min)** and **Delhi (12.7 min)** are fastest
- Blinkit handles the highest order volume among partners (4.0M), ahead of Shadowfax (3.5M) and Zomato (2.5M)

**Sales trend**
- Monthly sales are fairly stable (~131–136M/month) except a sharp dip in **February** (~122M), worth investigating for seasonality or operational issues

---

## 📈 Dashboard

The Power BI dashboard (`Blinkit_data_Vis.pdf`) is organized into 4 pages:
1. **Executive Summary** — KPIs, monthly trend, city-wise sales, payment method split, category/brand performance
2. **Pricing Analysis** — MRP vs. Selling Price, discount vs. hike distribution, average selling price & quantity by category
3. **Discount & Quantity Behavior** — Discount impact on order quantity, category-wise discount totals
4. **Delivery Performance** — Delivery time by partner/city, delivery category split, rating vs. delivery time correlation

---

## 📁 Repository Structure

```
Blinkit-Sales-Delivery-Analytics/
│
├── Blinkit_Python_codes.ipynb     # Data cleaning, feature engineering & EDA
├── Blinkit_Final_data.csv         # Cleaned, analysis-ready dataset (output)
├── Blinkit_data_Vis.pdf           # Power BI dashboard export (4 pages)
└── README.md                      # Project documentation
```

---

## 🚀 How to Run

1. Clone this repository
   ```bash
   git clone https://github.com/satyamsatyam1215-cmd/Blinkit-Sales-Delivery-Analytics.git
   cd Blinkit-Sales-Delivery-Analytics
   ```
2. Place the raw `blinkit_10M.csv` dataset in the project root
3. Install dependencies
   ```bash
   pip install pandas numpy matplotlib seaborn jupyter
   ```
4. Run the notebook
   ```bash
   jupyter notebook Blinkit_Python_codes.ipynb
   ```

---

## 🙋 About Me

**Satyam** — B.Com student (University of Mumbai) building a career in Data Analytics.
Skills: Advanced Excel · SQL · Python (Pandas, NumPy) · Power BI

📫 Feel free to connect on [LinkedIn](#) or check out my other projects!
