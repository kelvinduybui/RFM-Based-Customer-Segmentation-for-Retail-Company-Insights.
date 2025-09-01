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

#### 🗃️ E-commerce Retail Schema

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

#### 🏷️ Segmentation & RFM Score  

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
