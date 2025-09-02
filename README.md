# 🛍️ RFM Segmentation for Retail Customer Insights | Python

**Author:** Bùi Xuân Bảo Duy (Kelvin)  
**Date:** July 2025  
**Tools Used:** Python  

## 🗂️ Table of Contents
[1️⃣ Context]  
[2️⃣ Dataset Description & Data Structure]  
[3️⃣ Data Cleaning & EDA]  
[4️⃣ RFM Analysis]  
[5️⃣ Visualization]  
[6️⃣ Insights & Recommendations]  

---

## 1️⃣ Context

📘 **Main Context**  
- **SuperStore** is a global retail company with a large and diverse customer base.  
- The company is facing **financial challenges**, requiring **data-driven decision-making** across departments.  
- The **Marketing Department** plans strategic **Christmas and New Year campaigns** to strengthen customer relationships and drive growth cost-effectively.  
- The **customer dataset** is too large for **manual segmentation** this year.  
- To ensure **efficient resource allocation** and **campaign planning**, the **Data Analytics & Operations team** proposes building an **RFM-based segmentation workflow in Python** to support Marketing's strategy while aligning with the company's overall **operational goals**.  

👥 **Who is this project for?**  
- Data Analytics & Operations Team  
- Marketing & Sales Teams  
- Decision-makers & stakeholders  

🎯 **Objective**  
To segment customers using RFM analysis to identify high-value groups, uncover behavioral patterns, and provide actionable insights that support customer retention and targeted marketing—especially under financial constraints.

🛠️ **Tools Used**  
- Excel: For initial data storage  
- Google Colab (Python): For data cleaning, preprocessing, RFM analysis, segmentation, and visualization  

---

## 2️⃣ Dataset Description & Data Structure (DD & DS)

### 📊 Data Description  
- **Size:** 541,909 rows × 8 columns  
- **Sheets:** E-commerce retail, Segmentation  
- **Format:** .xlsx  
- **Time range:** 01/12/2010 – 09/12/2011  
- **Business type:** UK-based, non-store online retailer specializing in unique all-occasion gifts  

### 🗃️ E-commerce Retail Schema

| Column Name   | Data Type        | Description                                                     |
|---------------|------------------|-----------------------------------------------------------------|
| InvoiceNo      | object           | Invoice number, uniquely identifies each transaction. Starts with 'C' if cancelled. |
| StockCode      | object           | Product code, uniquely assigned to each item.                   |
| Description    | object           | Name of the product.                                            |
| Quantity       | int64            | Number of items purchased per transaction.                      |
| InvoiceDate    | datetime64[ns]   | Date and time when the transaction occurred.                    |
| UnitPrice      | float64          | Price per product unit in sterling.                             |
| CustomerID     | float64          | Unique identifier for each customer.                            |
| Country        | object           | Name of the country where the customer resides.                  |

### 🏷️ Segmentation & RFM Score  

| Segment               | RFM Score                                                                                                                        |
|-----------------------|----------------------------------------------------------------------------------------------------------------------------------|
| Champions             | 555, 554, 544, 545, 454, 455, 445                                                                                                |
| Loyal                 | 543, 444, 435, 355, 354, 345, 344, 335                                                                                            |
| Potential Loyalist     | 553, 551, 552, 541, 542, 533, 532, 531, 452, 451, 442, 441, 431, 453, 433, 432, 423, 353, 352, 351, 342, 341, 333, 323             |
| New Customers          | 512, 511, 422, 421, 412, 411, 311                                                                                                |
| Promising              | 525, 524, 523, 522, 521, 515, 514, 513, 425, 424, 413, 414, 415, 315, 314, 313                                                    |
| Need Attention         | 535, 534, 443, 434, 343, 334, 325, 324                                                                                            |
| About To Sleep         | 331, 321, 312, 221, 213, 231, 241, 251                                                                                            |
| At Risk                | 255, 254, 245, 244, 253, 252, 243, 242, 235, 234, 225, 224, 153, 152, 145, 143, 142, 135, 134, 133, 125, 124                       |
| Cannot Lose Them       | 155, 154, 144, 214, 215, 115, 114, 113                                                                                            |
| Hibernating customers  | 332, 322, 233, 232, 223, 222, 132, 123, 122, 212, 211                                                                             |
| Lost customers         | 111, 112, 121, 131, 141, 151                                                                                                     |

---

## 3️⃣ Data Cleaning & EDA  
### 📦 Import Libraries  

**Python code:**  
```python
# import libraries
import pandas as pd  
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
```

### 📁 Read Excel file  

**Python code:**
```python
# Read Excel file
df =pd.read_excel('/content/ecommerce retail.xlsx')
```

### 🔎 Explore data  

**Python code:**  
```python  
df.info()  
```

**Result:**  
![Image]  

**Python code:**  
```python  
df.describe()  
```

**Result:**  
![Image]  

**Python code:**  
```python  
df.head()  
```

**Result:**  
![Image]  

### Checking null values  

**Python code:**  
```python  
# Checking null values  
null_values_per_column = df.isnull().sum()  
print(null_values_per_column)  
```

**Result:**  
![Image]  

Null values in Description are acceptable, but nulls in CustomerID are not, as customer segmentation requires valid customer identifiers  
=> We drop null values in CustomerID  

### Drop null values  
**Python code:**  
```python  
# Drop null values  
df = df.dropna(subset=['CustomerID'])  
df['CustomerID'] = df['CustomerID'].astype(int)  
df.count()
```

**Result:**  
![Image]  

As we can see, the number of rows has been dropped to 406,829  

### Standardize date format  
Since the current date is set to 31/12/2011, it must be explicitly defined for further calculations  

**Python code:**  
```python  
# Standardize date format  
df['InvoiceDate'] = pd.to_datetime(df['InvoiceDate'])  
current_date = pd.to_datetime('31/12/2011', format='%d/%m/%Y')  
```

### Handle duplicated values  

There are two types of duplicate records in the dataset:  

- **Fully duplicated rows**: All values in the columns `InvoiceNo`, `StockCode`, `InvoiceDate`, `CustomerID`, and `Quantity` are exactly the same.  

- **Partially duplicated rows**: The columns `InvoiceNo`, `StockCode`, `InvoiceDate`, and `CustomerID` are the same, but the `Quantity` values differ.  

Data cleaning steps:  
1. Remove all fully duplicated rows.  
2. For partially duplicated rows (same `InvoiceNo`, `StockCode`, `InvoiceDate`, `CustomerID` but different `Quantity`), sum up the quantities to consolidate them into single entries.  

**Python code:**  
```python  
# Drop duplicate rows
df = df.drop_duplicates(subset=['InvoiceNo', 'StockCode', 'InvoiceDate', 'CustomerID', 'Quantity'])
# Sum duplicated InvoiceID but different quantity
df = df.groupby(['InvoiceNo', 'StockCode', 'InvoiceDate', 'CustomerID'], as_index=False).agg({
'Quantity': 'sum',
'Description': 'first',
'UnitPrice': 'first',
'Country': 'first'
})
df = df[['InvoiceNo', 'StockCode', 'Description', 'Quantity', 'InvoiceDate', 'UnitPrice', 'CustomerID', 'Country']] 
```

### TotalPrice & IsCancelled
Next, we'll generate TotalPrice & IsCancelled to df, where:  
- TotalPrice = Quantity * UnitPrice  
- IsCancelled = InvoiceNo that starts with "C"

**Python code:**  
```python  
# Generate TotalPrice & IsCancelled
df['TotalPrice'] = df['Quantity'] * df['UnitPrice']
df['IsCancelled'] = df['InvoiceNo'].astype(str).str.startswith('C')
```

### Check if UnitPrice <0
To ensure data quality, we'll check for any records where UnitPrice <0  

**Python code:**  
```python
# Check if UnitPrice <0  
df[df['UnitPrice']<0]
```

**Result:**  
![Image]  
As a result, no such cases found, we’ll move to the next step  

### Check if IsCancelled = False & Quantity <= 0  
To further ensure data quality, we'll check for any records where IsCancelled = False & Quantity <= 0  

**Python code:**  
```python
# Check if IsCancelled = False & Quantity <= 0
df[(df['IsCancelled'] == False) & (df['Quantity'] <= 0)]
```

**Result:**  
![Image]  

As a result, no such cases found, we’ll move to the next phase
