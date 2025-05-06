# 📊 Warehouse & Retail Sales Power BI Dashboard

This Power BI dashboard provides a comprehensive overview of warehouse and retail sales activity across multiple years (2017–2020). It visualizes key business metrics such as sales quantities, returns, supplier performance, product distribution, and transfer patterns using interactive visuals and calculated insights.

---

## 📌 Dashboard Highlights 

| Section | Description |
|--------|-------------|
| **🔹 KPI Cards** | Display key performance indicators:<br>• **Retail Sales QTY**, **Warehouse Sales QTY** — formatted in **K / M / B** using DAX<br>• **Returned QTY** — filters negative warehouse sales<br>• **Number of Transfers** — counts only valid transfers<br>• **Transfer Sales Ratio** — derived metric showing transfer proportion |
| **📅 Year-wise Filter** | Four interactive year buttons (2017–2020) allow seamless filtering across all visuals |
| **📈 Retail vs Warehouse Sales (Trend Line Chart)** | Shows monthly comparison of retail and warehouse sales to uncover seasonal demand, returns, and distribution fluctuations |
| **📊 Type-wise Distribution (Bar Chart)** | Visualizes sales by product categories (BEER, WINE, LIQUOR, etc.) with values displayed using custom units (M, K) |
| **📋 Supplier Performance (Top 10 Lists)** | Split into:<br>• **Top 10 Suppliers by Sales**<br>• **Top 10 Suppliers by Returns** — helps identify underperformers |
| **📉 Transfers History** | Monthly line chart showing transfer volume; aids in recognizing warehouse activity spikes or logistical inconsistencies |
| **🎨 Styling** | Clean, professional visual theme with:<br>• Gray background for contrast<br>• Branded colored tiles (blue, green, red)<br>• Consistent label formatting and borders |

---

## 🧠 DAX & Power Query Logic 

### 1. 🔢 Formatted Sales Values (K / M / B)
Used across KPI cards and visuals for readability. Prevents cluttered numbers while maintaining sortability via a hidden numerical column.

```dax
Formatted_Total_Sales = 
VAR _ts = [Total_Warehouse_Sales_Concerns]
RETURN 
    SWITCH(
        TRUE(),
        _ts >= 1e9, FORMAT(_ts / 1e9, "0.00") & " B",
        _ts >= 1e6, FORMAT(_ts / 1e6, "0.00") & " M",
        _ts >= 1e3, FORMAT(_ts / 1e3, "0.00") & " K",
        FORMAT(_ts, "0")
    )
````

> **Sort Fix**: Use `[Total_Warehouse_Sales_Concerns]` in the backend for correct numerical sorting when this field is displayed as a label.

---

### 2. 🚫 Returned QTY (Negative Sales Filter)

Calculates only those warehouse sales that represent product returns.

```dax
Total_Warehouse_Sales_Concerns = 
CALCULATE(
    SUM('W&R Sales Data'[OG_warehouse_sales]),
    'W&R Sales Data'[OG_warehouse_sales] < 0
)
```

---

### 3. ✅ Positive Warehouse Sales (Only Sales)

Filters out returns to ensure only valid, complete sales are counted.

```dax
Total_Warehouse_Sales_Positive = 
CALCULATE(
    SUM('W&R Sales Data'[OG_warehouse_sales]),
    'W&R Sales Data'[OG_warehouse_sales] > 0
)
```

---

### 4. 🔄 Number of Transfers

Counts the number of rows that have valid retail transfer quantities.

```dax
Quantity_of_Transfers = 
CALCULATE(
    COUNTROWS('W&R Sales Data'),
    'W&R Sales Data'[OG_retail_transfers] > 0
)
```

---

### 5. 📊 Transfer Sales Ratio

A derived metric to show what portion of warehouse activity is composed of transfers.

```dax
Transfer_Sales_Ratio = 
DIVIDE([Quantity_of_Transfers], [Total_Warehouse_Sales_Positive], 0)
```

---

### 6. 📅 Power Query Month Name Mapping

Replaces numeric month IDs with full names for visual clarity on trend charts.

```m
= Table.AddColumn(#"PreviousStep", "Month Name", each 
    if [Month] = 1 then "January" else 
    if [Month] = 2 then "February" else 
    if [Month] = 3 then "March" else 
    if [Month] = 4 then "April" else 
    if [Month] = 5 then "May" else 
    if [Month] = 6 then "June" else 
    if [Month] = 7 then "July" else 
    if [Month] = 8 then "August" else 
    if [Month] = 9 then "September" else 
    if [Month] = 10 then "October" else 
    if [Month] = 11 then "November" else 
    "December"
)
```

---

## 🧾 Dataset Overview

| Column                | Description                   |
| --------------------- | ----------------------------- |
| `OG_warehouse_sales`  | Quantity sold via warehouses  |
| `OG_retail_sales`     | Quantity sold via retail      |
| `OG_retail_transfers` | Quantity of transferred goods |
| `Month`               | Numerical month (1–12)        |
| `Year`                | Calendar year                 |
| `Product_Type`        | BEER, WINE, LIQUOR, etc.      |
| `Supplier`            | Company/brand name            |

---

## 📂 File Contents

| File             | Description                              |
| ---------------- | ---------------------------------------- |
| `README.md`      | This documentation file                  |
| `images/`        | Contains screenshots of the dashboard UI |

---

## 📸 Dashboard Preview

![image](https://github.com/user-attachments/assets/80b87348-4cf6-4443-afa6-f9a00509e217)


---


## 🔖 Tags

`#PowerBI` `#DataAnalysis` `#DAX` `#RetailSales` `#WarehouseManagement` `#DashboardDesign` `#SQL` `#BusinessIntelligence`

```


