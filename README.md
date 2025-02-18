# 🛒 Demand Forecasting for Walmart Sales & Inventory Optimization  
**🚀 Predicting Product Demand to Optimize Inventory & Reduce Overstock**  

## 📍 Introduction: Why This Problem Matters  
Imagine you're managing inventory for one of the largest retailers in the world—**Walmart**.  

You wake up to find that your store is **out of stock** for best-selling items. Customers leave frustrated, and revenue is lost.  

On the other hand, in another store, you've ordered **way too much** of a product that **isn’t selling**. Shelves are overflowing, and soon you'll need to **mark down prices, cutting into profit margins**.  

❌ **Overstock leads to revenue loss.**  
❌ **Stockouts lead to frustrated customers.**  
❌ **Both cost businesses millions of dollars.**  

💡 **What if we could predict demand before it happens?**  
This project **uses machine learning** to develop **forecasting models** that predict **weekly product demand trends** for Walmart stores, helping **optimize inventory planning and revenue management**.  

---

## 📊 Business Problem  
Retail businesses face the **same challenge**:  
📌 **If you overstock, you lose money. If you understock, you lose customers.**  

For **Walmart, the stakes are even higher**. Sales are **not uniform** across all stores—**seasonality, promotions, location, and customer preferences impact demand differently**.  

Historically, retailers have relied on:  
❌ **Moving averages** that don’t capture demand spikes.  
❌ **Gut feeling** that leads to inconsistent inventory planning.  
❌ **Static forecasting models** that don’t adjust dynamically.  

That’s where **machine learning** comes in—providing **dynamic, real-time forecasting** based on historical **trends, seasonality, and demand fluctuations**.

---

## 🔍 How I Built the Forecasting Model  
Before making predictions, I performed **Exploratory Data Analysis (EDA)** to understand historical sales trends.  

### 📂 Dataset Overview  
- **Source:** [Kaggle Retail Sales Data](https://www.kaggle.com/datasets/aslanahmedov/walmart-sales-forecast)  
- **Size:** **400,000+ weekly sales transactions**  
- **Timeframe:** **2010–2012**  

### 📌 Features Used for Forecasting  
- 🏪 **Store ID** → Unique identifier for each store.  
- 📦 **Product ID** → Unique identifier for each item.  
- 📊 **Weekly Sales** → Revenue per product, per week.  
- 📅 **Date** → Time-series index.  
- 🎉 **Holiday Indicator** → Whether a sale happened during a holiday period.  

---

## 📊 Key EDA Findings: What I Discovered  

Before diving into forecasting, I conducted a **deep exploratory data analysis (EDA)** to better understand sales patterns, seasonal fluctuations, and anomalies in the dataset. The goal was to uncover **hidden trends** that could influence the accuracy of the forecasting model.  

---

## 🔍 **Insights from Data Analysis**  

### **1️⃣ Holiday Sales Spikes: Major Demand Fluctuations**  
📈 **Sales during holiday weeks skyrocketed compared to non-holiday periods.**  

- **Black Friday saw the most significant spike** in sales across all stores and departments, sometimes increasing revenue by over **50% compared to regular weeks**.  
- **Christmas and Thanksgiving also exhibited large demand surges**, particularly for electronics, apparel, and seasonal goods.  
- **Non-holiday weeks were relatively stable**, with fluctuations mainly driven by store size and type.  

💡 **Business Insight:**  
- Retailers must **strategically stock inventory before holidays** to avoid stockouts and lost revenue.  
- Simple moving averages **fail** to capture these holiday effects, requiring a **seasonal forecasting model like SARIMA**.  

---

### **2️⃣ Category-Based Demand: Some Products Are Predictable, Others Are Not**  
📦 **Not all product categories follow the same demand pattern.**  

- **Grocery items exhibited stable demand year-round** → Consumers buy food at a **consistent rate**, so fluctuations were minimal.  
- **Electronics & apparel had strong seasonality** → Sales **peaked around the holidays** but were much lower in off-season months.  
- **Home goods & furniture had irregular sales patterns**, with occasional spikes unrelated to holidays (possibly due to promotions).  

💡 **Business Insight:**  
- **One-size-fits-all forecasting won’t work**—certain products require a **seasonal model**, while others may need a **trend-based model**.  
- **Inventory planning should be tailored** to category-specific sales trends.  

📌 **Visualizations Used:**  
✔ **Treemap of product categories** → Highlighted which product types had the highest total sales.  
✔ **Stacked bar charts comparing sales trends across categories** → Showed how demand shifted during peak seasons.  

---

### **3️⃣ Sales Data Was Already Stationary (ADF Test Results)**  
✅ **Before applying forecasting models, I needed to check if the data was stationary.**  

- Using the **Augmented Dickey-Fuller (ADF) test**, I found that the **p-value was below 0.05**, indicating that the dataset was already **stationary**.  
- This means that **differencing (D or d) was not required** in the SARIMA model, reducing unnecessary complexity.  

📌 **ADF Test Output:**  
```plaintext
ADF Statistic: -5.908
p-value: 2.67e-07
Critical Values:
   1%: -3.507
   5%: -2.895
  10%: -2.584



## 🧠 Forecasting Models: Which One Performed Best?  
I tested **three forecasting models** to find the best fit for predicting Walmart's weekly sales.

### **1️⃣ ARIMA (AutoRegressive Integrated Moving Average)**  
✅ Works well for **short-term forecasting**, but struggles with **seasonality**.  
📉 **MAE: 1.9M, RMSE: 2.1M**  

---

### **2️⃣ Prophet (Facebook Prophet)**  
✅ Designed for **business time-series forecasting**.  
✅ Captured **seasonality** better than ARIMA.  
📉 **MAE: 8.5M, RMSE: 10.7M**  

---

### **3️⃣ SARIMA (Seasonal ARIMA) – The Best Model for Walmart**  
📌 **Why SARIMA?**  
- Unlike ARIMA, **SARIMA explicitly models seasonality**, which is **critical for retail sales.**  
- Since **ADF test showed stationarity**, I set **D=0, d=0** to prevent unnecessary complexity.  

📌 **Final SARIMA Model Specification:**  
```python
order=(1,0,1)   # ARIMA Parameters
seasonal_order=(1,0,1,52)   # Seasonal Parameters
