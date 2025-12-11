## Swire Coca-Cola Cart Abandonment Analysis
### MSBA Capstone 

The goal of this project is to understand customer behavior on Swire Coca-Cola’s MyCoke360 ordering platform and develop a predictive model to identify which customers are most likely to abandon their carts, enabling targeted outreach to recover lost revenue from cart abandonment. 

#### Business Problem

Swire Coca-Cola is experiencing cases of customers failing to complete their purchases and abandoning their carts, which potentially  leads to lost revenue and operational inefficiencies, and may signal deeper issues such as pricing concerns or customer dissatisfaction.
Understanding why abandonment occurs and predicting which customers are likely to abandon their carts allows Swire to implement more effective recovery and engagement strategies.

This project aims to:
- Understand behavioral and operational factors contributing to abandonment
- Engineer features from Google Analytics, operations, product, and customer data
- Build a calibrated predictive model to estimate purchase likelihood
- Design a profit-optimized threshold for decision-making

#### Data Sources 

The analysis integrates several operational datasets, including:
- Customer Profiles: Sales office, channel descriptions, distribution mode
- Order Logs: Created dates, order quantities, sales channels
- Google Analytics: Event-level browsing and interaction logs
- Operating Hours & Frequencies: Customer-specific delivery/ordering schedules
- Cutoff Times: Daily operational cutoffs for each plant
- Product Catalog: Beverage categories, pack types, sizes, brands

#### Exploratory Data Analysis (EDA)

Conducted in-depth EDA on customer, sales, GA, materials, visit-plan, operating hours, and cutoff-time datasets
Created visualizations and extracted insights for segmentation, operational patterns, device behavior, and event-level interactions

The EDA focused on four key areas:
1. Customer Segmentation Insights
   
3. Operational Insights
  
4. Product Catalog Insights
  
#### Modeling 

- Model of choice was Logistic regression
- A leakage-safe predictive modeling pipeline was developed

#### Leakage Handling

Removed all columns containing future or outcome-dependent information, such as:
- purchase_successful
- order_complete_flag
- EVENT_PAGE_NAME containing “success.”
- transaction_id, payment_success

Also, filtering out event rows corresponding to completed purchases.
This ensures the model does not peek at information unavailable at prediction time.

#### Model Performance

- Cross-Validation ROC-AUC: **0.93**
- Time-Based Holdout ROC-AUC: **0.883**
- Accuracy: **81%**
- Brier Score: **0.122** (better calibrated than baseline 0.276)
- Threshold: **0.50**
- The confusion matrix and classification report indicate strong performance on both positive and negative classes.

#### Profit-Based Threshold Optimization

While typical models optimize accuracy or ROC-AUC, businesses care about profit.

Using the following assumptions:
- Revenue per recovered purchase = $50
- Cost per contact = $1

We performed a threshold sweep to calculate:
- TP/FP/FN/TN
- Cost of outreach
- Revenue gained
- Net profit per 1,000 customers

The optimal threshold identified maximizes net profit, not accuracy.
This approach leads to more actionable insights for Swire’s operations team.

#### Group Solution 

Our analysis allowed us to determine an optimal probability threshold of 0.50, which effectively distinguishes customers at high risk of cart abandonment from those likely to complete their orders. Customers whose predicted abandonment probability exceeds this threshold were identified as strong candidates for targeted outreach, enabling the business to focus recovery efforts where the expected return is highest. Based on these insights, we also recommended implementing A/B tests across email, phone, and text outreach strategies, as well as across different risk tiers, to quantify which communication method generates the greatest lift in recovered revenue. This solution ensures that operational resources are deployed strategically, improving conversion efficiency and maximizing overall revenue recovery from abandoned carts.

#### The Business Value of the Solution

This solution provides direct business value by enabling Swire Coca-Cola to target customers with a high risk of cart abandonment (above the optimal probability threshold), then do targeted outreach, such as emails, calls, or text reminders, to complete their purchase. By focusing efforts on high-probability customers, the model maximizes revenue recovery while minimizing unnecessary outreach costs. This ensures a more efficient use of sales resources and delivers a measurable financial impact by converting abandoned carts into completed orders.

### My Contribution
My primary contributions to this project centered on conducting the exploratory data analysis (EDA) and leading the full modeling workflow. I examined customer behavior patterns, engineered meaningful features, and carefully addressed data leakage to ensure the model only learned from information available at prediction time. I evaluated multiple models and ultimately developed the probability thresholding framework that determined which customers should be targeted for outreach. I then translated the modeling results into a clear, actionable business solution. This work formed the foundation for the team’s final recommendation on maximizing revenue recovery from cart abandonment.

#### Challenges Encountered 

- Large Data Volume & Complexity: Large data sets slowed processing
- Data Leakage Risks: GA purchase-success events unintentionally leaked outcome labels, and some operational flags and timestamps also had data leakage
- Merging Multiple Operational Tables
- Different date formats across sources
- Heavy memory usage during merges
- Very high Data imbalance

These challenges improved my understanding of real-world data engineering constraints.

 #### Key Take-aways
 
This project strengthened my ability to translate complex data into clear business decisions. I learned how to connect customer behavior patterns to measurable revenue opportunities, design features that reflect real operational constraints, and evaluate model output in a way that supports business goals. My skills in working with large datasets also improved. Overall, this project reinforced my technical ability to use data to drive meaningful business decisions within a real business context.
