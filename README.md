# E-Commerce Customer Insights & Predictive Churn Analysis

## Project Overview

This project analyzes customer transaction history from an e-commerce platform to identify customer segments, predict churn risk, and provide actionable recommendations for targeted marketing strategies. Using machine learning techniques including K-Means clustering and Logistic Regression, the analysis achieves 83% accuracy in predicting customer churn with 96% precision.

## Business Objective

The primary goal is to help the marketing team:
- Identify distinct customer segments based on purchasing behavior
- Predict which customers are likely to stop buying (churn)
- Provide data-driven recommendations for customer retention strategies
- Optimize marketing spend by targeting high-risk customers

## Dataset

- **Source**: Online Retail Dataset (2009-2011)
- **Records**: 1,067,371 transactions (1,033,036 after deduplication)
- **Customers**: 4,372 unique customers
- **Features**: Invoice, StockCode, Description, Quantity, InvoiceDate, Price, Customer ID, Country

## Methodology

### 1. Data Preprocessing
- Combined two years of transaction data (2009-2010 and 2010-2011)
- Removed 34,335 duplicate transactions to prevent inflated metrics
- Handled 235,151 records with missing Customer IDs
- Filtered out returns and canceled orders (invoices starting with 'C')
- Created engineered features: Sales, Hour, Month, and temporal patterns

### 2. Exploratory Data Analysis

#### Revenue Analysis by Geography
- **United Kingdom**: \$14.4M (dominant market - 86% of total revenue)
- **Ireland (EIRE)**: \$616K
- **Netherlands**: \$554K
- **Germany**: \$425K
- **France**: $349K

**Insight**: UK dominance suggests strong domestic presence with opportunities for international expansion.

#### Temporal Patterns

**Peak Sales Hours**:
- **12:00 PM**: \$2.69M (highest transaction volume)
- **1:00 PM**: \$2.35M
- **10:00 AM**: $2.32M

**Highest Average Transaction Value**:
- **7:00 AM**: $71.93 per transaction

**Peak Sales Months**:
- **November**: \$2.32M (holiday shopping)
- **October**: \$2.07M
- **September**: $1.78M

**Business Recommendation**: Early morning hours (7:00 AM) attract high-value customers—likely wholesalers or serious gift buyers—presenting opportunities for premium/wholesale promotions during these hours.

### 3. RFM Analysis & Customer Segmentation

Implemented **Recency, Frequency, Monetary (RFM)** analysis to create customer profiles:

- **Recency**: Days since last purchase (snapshot date: max date + 1 day)
- **Frequency**: Number of unique purchases (invoices)
- **Monetary**: Total spending amount

#### K-Means Clustering Results

Using the Elbow Method, **3 optimal clusters** were identified with a **Silhouette Score of 0.58**, indicating well-defined customer segments:

| Cluster | Label | Recency (days) | Frequency (orders) | Monetary (\$) | Characteristics |
|---------|-------|----------------|-------------------|--------------|-----------------|
| **0** | Moderate Customers | 66.3 | 7.6 | \$3,135 | Regular customers with steady spending |
| **1** | Churn-Risk Customers | 462.2 | 2.2 | \$746 | Inactive customers, low engagement |
| **2** | VIP Customers | 23.1 | 143.1 | $173,124 | High-value, frequent, recent buyers |

**Cluster Insights**:

**Cluster 2 - VIP Customers** (Top Priority):
- Recent, frequent, high-spending customers
- Represent the company's most valuable segment
- **Recommended Actions**: Loyalty rewards, early product access, premium offers, dedicated account management

**Cluster 0 - Moderate Customers** (Growth Opportunity):
- Largest customer segment
- Steady but not exceptional spending
- **Recommended Actions**: Upselling opportunities, personalized recommendations, targeted promotions

**Cluster 1 - Churn-Risk Customers** (Retention Focus):
- Long time since last purchase (462 days average)
- Low transaction frequency and monetary value
- **Recommended Actions**: Re-engagement campaigns, win-back emails, special discounts

### 4. Churn Prediction Model

#### Feature Engineering
Created additional predictive features:
- **is_churned**: Binary label (1 if Recency > 90 days, 0 otherwise)
- **Avg_Order_Value**: Monetary / Frequency
- **Tenure**: Days since first purchase

**Critical Design Decision**: Excluded 'Recency' from the model to avoid data leakage, forcing the model to learn from actual behavioral patterns rather than simply checking the answer.

#### Model Performance

**Algorithm**: Logistic Regression  
**Train-Test Split**: 80-20  
**Overall Accuracy**: 83%

| Metric | Class 0 (Active) | Class 1 (Churned) |
|--------|------------------|-------------------|
| Precision | 76% | **96%** |
| Recall | 96% | 71% |
| F1-Score | 85% | 81% |

#### Confusion Matrix Analysis (Test Set: 1,177 customers)

| Actual / Predicted | Active | Churned |
|-------------------|--------|---------|
| **Active** | 548 ✓ | 20 (False Alarm) |
| **Churned** | 177 (Missed) | 432 ✓ (Caught) |

**Key Takeaways**:
- **96% Precision for Churn**: When the model flags a customer as likely to churn, it's correct 96% of the time
- **Only 20 False Alarms**: Minimal waste on unnecessary retention offers
- **432 Churners Identified**: Successfully caught 71% of at-risk customers
- **177 Missed Cases**: Room for improvement by incorporating additional data

### 5. Feature Importance Analysis

| Feature | Coefficient | Impact on Churn |
|---------|-------------|-----------------|
| **Cluster** | +3.31 | Strongest positive predictor—certain segments are much more likely to churn |
| **Tenure** | +0.61 | Longer-tenured customers with low frequency are at higher risk |
| **Avg_Order_Value** | -0.10 | Slight protective effect |
| **Monetary** | -0.58 | Higher total spending reduces churn probability |
| **Frequency** | **-2.20** | **Most important protective factor**—frequent visits dramatically reduce churn risk |

**Critical Insight**: **Frequency is the #1 driver of customer loyalty**. Every additional unique visit significantly reduces churn probability. Habits are stronger than one-off high-value purchases.

## Strategic Recommendations

### 1. **Automated Intervention System**
- Implement real-time churn prediction scoring
- Trigger automatic retention offers for high-confidence flags (96% precision)
- Recommended actions: Personalized emails, loyalty rewards, exclusive discounts

### 2. **Frequency-Driven Marketing**
- **Priority**: Increase visit frequency over transaction size
- Moving a customer from 2 to 5 visits has a larger impact than doubling order value
- Implement strategies: subscription models, frequent buyer programs, limited-time offers

### 3. **Cluster-Specific Campaigns**

**VIP Customers (Cluster 2)**:
- White-glove service and exclusive perks
- Early access to new products
- Personalized shopping experiences

**Moderate Customers (Cluster 0)**:
- Cross-sell and upsell opportunities
- Product recommendations based on purchase history
- Gamification to increase frequency

**Churn-Risk Customers (Cluster 1)**:
- Aggressive win-back campaigns
- "We miss you" emails with time-limited offers
- Re-engagement surveys to understand departure reasons

### 4. **Low-Risk Aggressive Testing**
- With only 1.7% false alarm rate, the business can confidently offer expensive retention incentives
- No fear of "cannibalizing" revenue from loyal customers

### 5. **Future Model Enhancements**
To capture the 29% of churners currently missed:
- Incorporate customer service interaction data
- Add product category preferences
- Include website engagement metrics (click-through rates, browsing behavior)
- Consider demographic data if available

## Technologies Used

- **Python 3.x**
- **pandas**: Data manipulation and analysis
- **NumPy**: Numerical computing
- **Matplotlib & Seaborn**: Data visualization
- **scikit-learn**: Machine learning
  - K-Means Clustering
  - Logistic Regression
  - StandardScaler
  - Train-test split
  - Model evaluation metrics

## Project Structure

```
├── online_retail.xlsx                    # Source data (2009-2011)
├── online_retail_full.parquet            # Processed dataset
├── E-Commerce_Churn_Analysis.ipynb       # Main analysis notebook
├── README.md                             # Project documentation
└── requirements.txt                      # Python dependencies
```

## Key Results Summary

| Metric | Value |
|--------|-------|
| Total Customers Analyzed | 4,372 |
| Customer Segments Identified | 3 |
| Churn Prediction Accuracy | 83% |
| Churn Prediction Precision | 96% |
| At-Risk Customers Identified | 432 |
| False Positive Rate | 1.7% |
| Top Revenue Country | UK (\$14.4M) |
| Peak Sales Hour | 12:00 PM |
| Highest AOV Hour | 7:00 AM ($71.93) |
| Silhouette Score | 0.58 |

## Business Impact

This analysis enables the marketing team to:
1. **Reduce customer churn** by 71% through early intervention
2. **Optimize marketing spend** with 96% confidence in targeting
3. **Increase customer lifetime value** by focusing on frequency-driven strategies
4. **Segment customers effectively** for personalized marketing campaigns
5. **Identify high-value hours** for premium product promotions

## How to Run

1. Clone the repository
```bash
git clone https://github.com/MichaelOkere30/ecommerce-churn-analysis.git
cd ecommerce-churn-analysis
```

2. Install dependencies
```bash
pip install -r requirements.txt
```

3. Open and run the Jupyter notebook
```bash
jupyter notebook E-Commerce_Churn_Analysis.ipynb
```

## Author

**Okere Ezra Val**  
Aspiring Data Analyst | Data Analytics Enthusiast

## License

This project is licensed under the MIT License - see the LICENSE file for details.

## Acknowledgments

- Dataset: UCI Machine Learning Repository - Online Retail Dataset
- Inspired by real-world e-commerce analytics challenges
