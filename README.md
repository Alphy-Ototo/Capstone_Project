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

Data was securely accessed through Google Drive and processed in Python via Google Colab.

#### Exploratory Data Analysis (EDA)

Conducted in-depth EDA on customer, sales, GA, materials, visit-plan, operating hours, and cutoff-time datasets
Created visualizations and extracted insights for segmentation, operational patterns, device behavior, and event-level interactions

The EDA focused on four key areas:
1. Customer Segmentation Insights
    - 6,300+ unique customers across 44 sales offices
    - Top channels: Restaurants, Convenience, Attractions, Hotels
    - 95% of customers use 48-hour shipping
    - Most common delivery modes: OFS, Sideload, Rapid Delivery
2. Behavioral Insights from Google Analytics
   - Over 3 million valid GA events after cleaning
   - The majority of interactions come from desktop devices, suggesting further analysis into checkout for phones and tablets
   - Heavy browsing activity was noted, but low checkout conversion
   - Engagement types analyzed:- view_item_list
                               - user_engagement
                               - add_to_cart
                               - remove_from_cart
                               - begin_checkout
                               - purchase 
3. Operational Insights
   - Most customers are scheduled for visits on a weekly or 4-week cycle
   - Cutoff times cluster around 3:00–4:00 PM
   - Earlier cutoff times correlate with shorter shipping windows
   - Sales Representatives drive the majority of orders, but MyCoke360 orders show an upward trend
4. Product Catalog Insights
   - 1,200+ SKUs
   - Dominant beverage categories:
   - Core Sparkling
   - Energy Drinks
   - Sports Drinks
   - High packaging diversity (20+ pack types)

#### Modeling 

- Model of choice was Logistic regression
- A leakage-safe predictive modeling pipeline was developed

Target Definition

made_a_purchase = 1 if the customer completed a purchase in the given session window.

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

#### Final Recommendations & Next Steps
- Target customers above the optimal probability threshold for outreach through email, calls, or text,  maximizing revenue recovery.  
- Focus on user-experience improvements on the most common drop-off points identified in GA events.    
- Consider A/B testing different outreach strategies for customers at varying risk levels. 

#### Challenges Encountered 

- Large Data Volume & Complexity: Very large visit-plan files (13M+ rows) slowed processing
- Data Leakage Risks: - GA purchase-success events unintentionally leaked outcome labels
                      - Some operational flags and timestamps represented future information.
- Merging Multiple Operational Tables
- Different date formats across sources
- Heavy memory usage during merges
- Very high Data imbalance

These challenges improved my understanding of real-world data engineering constraints.

 #### Key Take-aways
 
This project strengthened my skills in:
- Real-world machine learning
- Handling data leakage in machine learning
- Data engineering & cleaning
- Handling very large datasets
- Using predictive models in real time and using them to derive actionable insights

