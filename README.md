# 🛍️ RFM-Based Customer Segmentation for Retail Company Insights | Python

**Author:** Bùi Xuân Bảo Duy (Kelvin)  
**Date:** July 2025  
**Tools Used:** Python  

## 🗂️ Table of Contents
1️⃣ [Context](#context)  
2️⃣ [Dataset Description & Data Structure (DD & DS)](#dataset-description--data-structure-dd--ds)  
3️⃣ [Data Cleaning & EDA](#data-cleaning--eda)  
4️⃣ [RFM Analysis](#rfm-analysis)  
5️⃣ [Visualization](#visualization)  
6️⃣ [Insights & Recommendations](#insights--recommendations)

---

## 1️⃣ Context

📘 **Main Context**  
- **SuperStore** is a global retail company with a large and diverse customer base.  
- The company is facing **financial challenges**, requiring **data-driven decision-making** across departments.  
- The **Marketing Department** plans strategic **Christmas and New Year campaigns** to strengthen customer relationships and drive growth cost-effectively.  
- The **customer dataset** is too large for **manual segmentation** this year.  
- To ensure **efficient resource allocation** and **campaign planning**, the **Data Analytics & Operations team** proposes building an **RFM-based segmentation workflow in Python** to support Marketing's strategy while aligning with the company's overall **operational goals**.  

👥 **Target Audience**  
- Data Analytics & Operations Team  
- Marketing & Sales Teams  
- Decision-makers & stakeholders  

🎯 **Objective**  
To segment customers using RFM analysis to identify high-value groups, uncover behavioral patterns, and provide actionable insights that support customer retention and targeted marketing—especially under financial constraints.

🛠️ **Tools Used**  
- Excel: For initial data storage  
- Google Colab (Python): For data cleaning, preprocessing, RFM analysis, segmentation, and visualization, using packages such as pandas, numpy, matplotlib, seaborn, etc.

---

## 2️⃣ Dataset Description & Data Structure (DD & DS)

### 📊 Data Description  
- **Size:** 541,909 rows × 8 columns  
- **Sheets:** E-commerce retail, Segmentation  
- **Format:** .xlsx  
- **Time range:** 01/12/2010 – 09/12/2011  

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

### What’s the RFM Model?

RFM model: a segmentation technique used in marketing and customer relationship management.  
Objective: to segment and understand customer behavior, which aligns with the customer segmentation application rule-based segmentation method by mapping specific RFM score combinations to meaningful customer segments.  

**Recency (R)**  
How recently a customer made a purchase. The more recent the purchase, the higher the score.  

**Frequency (F)**  
How often a customer makes a purchase over a specific period. Higher frequency means a higher score.  

**Monetary Value (M)**  
How much money a customer spends over a given period. Higher spending results in a higher score.  

### Reference Table
| Dimension   | 5                  | 4               | 3                   | 2             | 1                  |
|-------------|--------------------|-----------------|---------------------|---------------|--------------------|
| **Recency** | Very recently      | Fairly recently | Moderately recently | Long time ago | Very long time ago |
| **Frequency** | Very frequent    | Frequent        | Moderate            | Infrequent    | Hardly ever        |
| **Monetary**  | Very high spending | High spending   | Moderate spending   | Low spending  | Very low spending  |


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
### 📦 3.1. Import Libraries  

**Python code:**  
```python
# import libraries
import pandas as pd  
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
```

### 📁 3.2. Read Excel file  

**Python code:**
```python
# Read Excel file
df =pd.read_excel('/content/ecommerce retail.xlsx')
```

### 🔎 3.3. Explore data  

- Checked **data types & structure** → dataset includes both object and numeric columns, with a large number of records.  
- **Summary statistics** (min, max, mean, etc.) helped identify potential outliers.  
- Reviewed **sample records** (head) to understand the data format.  

### 🕵️ 3.4. Checking null values  

- Null values in **Description** → acceptable.  
- Null values in **CustomerID** → not acceptable, since segmentation requires valid customer identifiers.  
- Action: rows with missing **CustomerID** need to be dropped.  

### 🧹 3.5. Drop null values  

- After removing rows with null **CustomerID** and converting to integer, the dataset was reduced to **406,829 rows**.  
- This cleaned dataset will be used for the RFM analysis.

### ⏳ 3.6. Standardize date format  
Since the current date is set to 31/12/2011, it must be explicitly defined for further calculations  

**Python code:**  
```python  
# Standardize date format  
df['InvoiceDate'] = pd.to_datetime(df['InvoiceDate'])  
current_date = pd.to_datetime('31/12/2011', format='%d/%m/%Y')  
```

### 🧾 3.7. Handle duplicated values  

There are two types of duplicate records in the dataset:  

- **Fully duplicated rows** (all values in `InvoiceNo`, `StockCode`, `InvoiceDate`, `CustomerID`, `Quantity` are the same) → **remove**.  
- **Partially duplicated rows** (same `InvoiceNo`, `StockCode`, `InvoiceDate`, `CustomerID` but different `Quantity`) → **sum up** the quantities and consolidate into single entries.  

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

### 💰 3.8. TotalPrice & IsCancelled  
Next, we'll generate TotalPrice & IsCancelled to df, where:  
- **TotalPrice** = Quantity * UnitPrice  
- **IsCancelled** = InvoiceNo that starts with "C"

**Python code:**  
```python  
# Generate TotalPrice & IsCancelled
df['TotalPrice'] = df['Quantity'] * df['UnitPrice']
df['IsCancelled'] = df['InvoiceNo'].astype(str).str.startswith('C')
```

### 🔍 3.9. Check if UnitPrice < 0  
To ensure data validity, we checked for negative `UnitPrice` values, as sales transactions cannot have negative prices. No such records were found.  

### 🛑 3.10. Check if IsCancelled = False & Quantity <= 0  
We also verified that all non-cancelled transactions (`IsCancelled = False`) have positive quantities. No invalid records were detected.  

---

## 4️⃣ RFM Analysis  
### Calculate RFM  
Next, we’ll calculate Recency, Frequency & Monetary, where:  
- Recency: The number of days between the current date and the customer's most recent purchase (excluding cancelled orders).  
- Frequency: The total number of unique purchase transactions (InvoiceNo), excluding cancelled orders.  
- Monetary: The total monetary value of all transactions.  
All group by CustomerID.

**Python code:**  
```python
# Group by CustomerID to calculate R,F,M  
new_df = df.groupby('CustomerID').agg(  
    Recency=('InvoiceDate', lambda x: (current_date - x[~df.loc[x.index, 'IsCancelled']].max()).days),  
    Frequency=('InvoiceNo', lambda x: x[~x.astype(str).str.startswith('C')].nunique()),  
    Monetary=('TotalPrice', 'sum')  
).reset_index()  
```

### Handle unusual values  
- There will be cases where Recency returns NaN, we have to exclude them out of the model
- Moreover, cases where Monetary equals 0 indicate fully canceled orders—effectively meaning no purchase was made. These should be excluded from the RFM model  
- new_df now:  

![Image](https://github.com/kelvinduybui/RFM-Segmentation-for-Retail-Customer-Insights-Python/blob/main/Images/new_df.head.png?raw=true)

### Calculate RFM Score
Next, we’ll calculate R_score, F_score & M_score, where:
- R_score: Based on the recency rank (descending), with lower recency getting higher scores.
- F_score and M_score: Based on the rank of frequency and monetary (ascending), with higher values getting higher scores.
Each score is assigned into 5 quantiles (1 to 5), & combined to form the RFM_score

**Python code:**  
```python  
new_df['R_score'] = pd.qcut(new_df['Recency'].rank(method='first', ascending=False), 5, labels=[1, 2, 3, 4, 5])
new_df['F_score'] = pd.qcut(new_df['Frequency'].rank(method='first'), 5, labels=[1, 2, 3, 4, 5])
new_df['M_score'] = pd.qcut(new_df['Monetary'].rank(method='first'), 5, labels=[1, 2, 3, 4, 5])
new_df['RFM_score'] = new_df['R_score'].astype(str) + new_df['F_score'].astype(str) + new_df['M_score'].astype(str)
```

### RFM Segmentation  

**Python code:**  
```python  
def assign_segment(rfm_score):  
    if rfm_score in ['555', '554', '544', '545', '454', '455', '445']:  
        return 'Champions'
    elif rfm_score in ['543', '444', '435', '355', '354', '345', '344', '335']:
        return 'Loyal'
    elif rfm_score in ['553','551','552','541','542','533','532','531','452','451','442','441','431','453','433','432','423','353','352','351','342','341','333','323']:
        return 'Potential Loyalist'
    elif rfm_score in ['512', '511', '422', '421', '412', '411', '311']:
        return 'New Customers'
    elif rfm_score in ['525', '524', '523', '522', '521', '515', '514', '513', '425', '424', '413', '414', '415', '315', '314','313']:
        return 'Promising'
    elif rfm_score in ['535', '534', '443', '434', '343', '334', '325','324']:
        return 'Need Attention'
    elif rfm_score in ['331', '321', '312', '221', '213', '231', '241', '251']:
        return 'About To Sleep'
    elif rfm_score in ['255', '254', '245', '244', '253', '252', '243', '242', '235', '234', '225', '224', '153', '152', '145', '143', '142', '135', '134', '133', '125', '124']:
        return 'At Risk'
    elif rfm_score in ['155', '154', '144', '214', '215', '115', '114',  '113']:
        return 'Cannot Lose Them'
    elif rfm_score in ['332', '322', '233', '232', '223', '222', '132', '123', '122', '212', '211']:
        return 'Hibernating customers'
    elif rfm_score in ['111', '112', '121', '131', '141', '151']:
        return 'Lost customers'
    else:
        return 'Others'

new_df['Segment'] = new_df['RFM_score'].apply(assign_segment)
```

### New_df  
After doing things, new_df will look like this:  

![Image](https://github.com/kelvinduybui/RFM-Segmentation-for-Retail-Customer-Insights-Python/blob/main/Images/new_df.head.png?raw=true)  

After that, we do some basic calculations for further analysis  
- Total_Customers: Count of unique CustomerIDs.  
- Total_Recency: Sum of Recency scores.  
- Total_Orders: Sum of Frequency values.  
- Total_Sales: Sum of Monetary values.

**Python code:**  
```python
df_segment = new_df.groupby('Segment').agg(
    Total_Customers=('CustomerID', 'nunique'),
    Total_Recency=('Recency', 'sum'),
    Total_Orders=('Frequency', 'sum'),
    Total_Sales=('Monetary', 'sum'),
).reset_index()

df_segment
```

**Result:**  
![Image](https://github.com/kelvinduybui/RFM-Segmentation-for-Retail-Customer-Insights-Python/blob/main/Images/df_segment.png?raw=true)


---

## 5️⃣ Visualization  
### Segment visualization  
![Image](https://github.com/kelvinduybui/RFM-Segmentation-for-Retail-Customer-Insights-Python/blob/main/Images/Customer%20count.png?raw=true)  

#### Key Takeaway:  
**Champions** and **Hibernating Customers** are the **largest** groups, while **Cannot Lose Them** and **Promising** are the **smallest**, showing a strong engaged base but also many inactive customers, highlighting both retention and reactivation opportunities.  

![Image](https://github.com/kelvinduybui/RFM-Segmentation-for-Retail-Customer-Insights-Python/blob/main/Images/RFM%20by%20segment.png?raw=true)  

#### Key Takeaway:  
The clear differences across segments highlight the potential for creating more granular **sub-segmentation** to better tailor customer strategies.  

👉 To simplify the problem, we’ll group 11 segments into 4 groups, based on the similarities from the above distributions, 4 groups are:  
- **The Elite Customers:** Champions, Loyal  
- **The Potential Stars:** Promising, New Customers, Potential Loyalist, Need Attention  
- **The Fading Ones:** At Risk, About to Sleep, Hibernating  
- **The Silent Crowd:** Cannot Lose Them, Lost Customers

### Segments to groups  

**Python code:**  
```python
def assign_group(segment):
    if segment in ['Champions', 'Loyal']:
        return 'The Elite Customers'
    elif segment in ['Promising', 'New Customers', 'Potential Loyalist', 'Need Attention']:
        return 'The Potential Stars'
    elif segment in ['At Risk', 'About To Sleep', 'Hibernating customers']:
        return 'The Fading Ones'
    elif segment in ['Cannot Lose Them', 'Lost customers']:
        return 'The Silent Crowd'
    else:
        return 'Others'

new_df['Group'] = new_df['Segment'].apply(assign_group)
```

### Groups visualization  

![Image](https://github.com/kelvinduybui/RFM-Segmentation-for-Retail-Customer-Insights-Python/blob/main/Images/Customer%20distri%20by%20Griup.png?raw=true)  

#### Key Takeaway:  
The customer groups rank from largest to smallest as **The Fading Ones (32.3%), The Elite Customers (29.2%), The Potential Stars (25.1%), and The Silent Crowd (13.4%)**.  

### Number of customers over time visualization  

![Image](https://github.com/kelvinduybui/RFM-Segmentation-for-Retail-Customer-Insights-Python/blob/main/Images/Unique%20customer%20per%20group%20per%20month.png?raw=true)  

#### Key Takeaway:  
**The Elite Customers** remain the largest and most stable group, while **The Potential Stars** surged late in 2011 before dropping. **The Fading Ones** declined, and **The Silent Crowd** nearly disappeared.  

### RFM over time visualization

![Image](https://github.com/kelvinduybui/RFM-Segmentation-for-Retail-Customer-Insights-Python/blob/main/Images/RFM%20by%20month.png?raw=true)  

#### Key Takeaway:  
The chart shows that **R_score** **improved** sharply in the last months of 2011, reaching its peak in December (this is expected since Recency is calculated based on the cut-off date, so purchases closer to Dec-2011 naturally lead to higher R_scores), while **F_score** and **M_score** stayed relatively **stable** with slight declines before recovering at year-end.

### Customers per Country visualization  

![Image](https://github.com/kelvinduybui/RFM-Segmentation-for-Retail-Customer-Insights-Python/blob/main/Images/Customer%20by%20country.png?raw=true)  

#### Key Takeaway:  
As it can be seen from the chart, the **UK outperforms the others**, so we **exclude** the UK to have clearer insights.  

### Customers per Country visualization (without UK)  
![Image](https://github.com/kelvinduybui/RFM-Segmentation-for-Retail-Customer-Insights-Python/blob/main/Images/Customer%20by%20country%20(no%20UK).png?raw=true)

#### Key Takeaway:  
The chart shows that a certain number of customers come from some specific **European countries**, while most other countries contribute only a small share. This indicates that customers are mainly concentrated in a few major markets.  

---

## 6️⃣ Insights & Recommendations  
### Group insights  
- **The Fading Ones** (32.3%): These customers likely haven’t interacted recently or frequently, suggesting potential churn risk. They may require re-engagement strategies.  
- **The Elite Customers** (29.2%): The second-largest group. These are high-value, loyal customers who should be prioritized with loyalty programs and exclusive offers.  
- **The Potential Stars** (25.1%): Promising customers who may not have reached elite status yet but are trending positively. They are good candidates for upselling & nurturing.  
- **The Silent Crowd** (13.4%): The smallest segment, likely consisting of low-value customers. They might not be worth a heavy investment unless reactivation is cost-effective.  

### Group recommendation  
**1. The Elite Customers**  
- Focus on **retaining** them since they spend a lot and have interacted recently — they are the core revenue stream right now.  
- **Action**: Offer VIP perks, personalized care, loyalty programs, or fast customer support.  

**2. The Potential Stars**  
- **Nurture** this group because it behaves similarly to the Elite but hasn't reached the same value yet — easier to convert than acquiring new customers.  
- **Action**: Give small gifts on first purchase, offer bundle deals, and apply gentle upselling.  

**3. The Fading Ones**  
- **Selectively reactivate** them due to the large volume, but not all are worth marketing to avoid overspending.  
- **Action**: Run winback campaigns targeting the top 10–20% highest-value customers in this segment based on Monetary and Frequency.  
  
**4. Silent Crowd**  
- **Temporarily ignore** this group as it has minimal financial impact in the short term.  
- **Action**: Do not allocate marketing budget to this group when resources are limited.

### RFM insights & recommendation  
#### Insights:  
- **The Elite Customers** and **Potential Stars** grew steadily throughout the year, becoming key segments, but both **dropped sharply** in December 2011.  
- **Frequency** and **Monetary** scores were **relatively flat**, with a dip around August and a slight recovery by December 2011.  

#### Recommendation:  
- Investigate the **sharp decline** in December 2011 — Was there a change in promotions, stock issues, or technical problems during a critical holiday period?  
- Understand why **customer numbers fell during the holiday season**, a time when customer traffic is typically expected to rise — consider reviewing campaign timing, product relevance, or external market factors.  

### Countries insights & recommendation
#### Insights:  
- The **United Kingdom** dominates the customer base, with a high number of **Elite Customers** and **Potential Stars** → remains the core revenue driver.  
- France, Germany, and Belgium show promising volumes of valuable customers → potential secondary markets for focused efforts.  
- Other countries have small, mixed customer groups without clear strength → currently low-return markets.  

#### Recommendation:  
- **Prioritize retention** efforts in the **UK**, targeting **Elite** and **Potential customers** via low-cost tactics like personalized emails or loyalty perks.  
- Run low-budget reactivation campaigns in **France & Germany**, using newsletters, limited-time offers, or remarketing popups to gauge responsiveness.  
- **Pause** or **minimize marketing** in low-impact countries, especially where customer value is unclear → conserve budget for high-ROI areas.


---
