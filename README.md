# Overview 

Welcome to a comprehensive analysis of retail sales data using advanced Python techniques, where insights are derived through customer segmentation, market basket analysis, and event-driven sales spike visualization. All exploratory work is done in Jupyter Notebook with pandas, matplotlib, and seaborn, focusing on uncovering how business events and festivals drive significant sales spikes, identifying frequently paired products, and segmenting customers for targeted strategies—all interpreted through clear, actionable graphs.

## Features
- Data Cleaning: Handles missing values, duplicates, and outliers.

- Exploratory Data Analysis (EDA): Presents key trends, patterns, and insights with visualizations.

- Market Basket Analysis: Identifies product associations and frequently bought-together items.

- RFM Analysis: Segments customers based on Recency, Frequency, and Monetary values.

- Interactive Visualizations: Clear, interpretable charts for business decision-making.

## Tech Used

- Language: Python

- Platform: VScode, Jupyter Notebook

- Libraries: Pandas, NumPy, Matplotlib, Seaborn, Scikit-learn

# Questions
Below are the questions I want to answer in my project:

1. How are customers categorized into different RFM segments, and what patterns are observed in the plots?

2. How can customers be grouped into distinct segments using K-means clustering?

3. Which products are most frequently bought together, as seen in the association rule graphs?

4. What are the strongest product association rules discovered from transaction data?

5. What are the key business events and festivals that trigger the highest spikes in retail sales?

# The Analysis 
Here's how i approched to this questions :

## 1. How are customers categorized into different RFM segments, and what patterns are observed in the plots?

In this analysis, I calculated the Recency, Frequency, and Monetary (RFM) values for each customer based on their purchase history, then assigned RFM scores to categorize customers into distinct segments such as top customers, loyal customers, and at-risk customers. Using visualizations, I explored the distribution of customers across these segments, revealing patterns like which groups are the largest, where high-value buyers cluster, and which segments require attention for retention or reactivation strategies.

View my notebook for details :[1_RFM_Analysis](C:\Users\lenovo\Documents\Retail_project\1_RFM_Analysis.ipynb)

```python 
centre_circle = plt.Circle((0, 0), 0.60, fc='white')
fig = plt.gcf()
fig.gca().add_artist(centre_circle)

plt.title('Customer Segments (RFM Donut Chart)')
plt.tight_layout()
plt.show()
```
### Result

![Customer_segmentation](Images\customer_segmentation.png)
*Donut chart showing percentage of each segments.*

### Insights

- The largest segment is Regular customers, making up nearly half (45.5%) of the customer base, indicating most buyers shop steadily but without exceptionally high frequency or value.

- At Risk (25.3%) and High Spenders (19.6%) comprise significant portions, suggesting there are both valuable and potentially slipping customers needing targeted action for retention or engagement.

- Very few customers fall into Frequent Champions (1.2%) and Hibernating (7.7%), highlighting a small group of highly engaged buyers and a small subset that rarely interacts, ideal for reactivation campaigns.

### Solutions 

- Offer loyalty programs and personalized incentives to encourage regular customers to purchase more frequently and move into higher-value segments.

- Implement win-back campaigns and special deals to re-engage at-risk and hibernating customers, aiming to reduce churn and reactivate dormant buyers.

- Reward high spenders and frequent champions with VIP benefits or referral incentives to maintain their loyalty and turn them into brand promoters.

## 2. How can customers be grouped into distinct segments using K-means clustering?

K-means clustering was used to segment customers into groups with similar purchase frequency and spending, making it easier to identify and target distinct customer types for tailored marketing strategies.

View my notebook for details : [2_Kmean_segmentation](2_Kmean_segmentation.ipynb)

``` python
optimal_k = 4
kmeans = KMeans(n_clusters=optimal_k, random_state=42)
rfm['Cluster'] = kmeans.fit_predict(X_scaled)

plt.figure(figsize=(8,6))
sns.scatterplot(x=rfm['Recency'], y=rfm['Monetary'], hue=rfm['Cluster'], palette='Set1', alpha=0.7)
plt.show()
```
### Result 
![Cluster_segmentation](Images\Cluster_segmentation.png)

### Insights

- The plot reveals clear segmentation, separating high-value and active customers from less engaged groups.

- Most customers cluster in areas of low monetary value and high recency, showing many are infrequent or inactive shoppers.

- Distinct, smaller clusters with recent, higher spending highlight priority targets for personalized retention or reward strategies.

# 3 & 4. Which products are most frequently bought together, and what are the strongest product association rules?

This section reveals which products are commonly bought together and highlights the strongest item associations, providing clear insights for cross-selling and bundle promotions.It reveals the most frequently paired products and the strongest purchase patterns, guiding cross-selling and product bundling strategies.

```python 
# Plot top association rules for key product categories.
categories = ['Clothing', 'Electronics', 'Furniture']
for cat in categories:
    plot_association_rules(df, cat)  # Shows top 10 rules for each category
```
For detail code prefer my notebook: [Basket_analysis](3_Basket_analysis.ipynb)

# Result

![Clothing_category](Images\Association_rule_clothing.png)
*Graph shows the commanly bought cloths, business owners can now make there strategy based on this.*

### Insights

- In the Clothing category, accessories like handkerchiefs and apparel such as kurtis, t-shirts, and leggings show strong co-purchase patterns, indicating these items are often bought together and could be bundled for promotions.

- Within Electronics, phones frequently lead to purchases of accessories and printers, suggesting targeted offers on peripherals or multi-item discounts can boost sales.

- For Furniture, chairs are strongly associated with tables and bookcases, highlighting cross-selling potential by combining these essential items in packages or showroom arrangements.

- Across all categories, high lift values in the top associations confirm that these grouped items are bought together much more often than by chance, providing actionable insights for product placement and marketing strategies.

# 5. What are the key business events and festivals that trigger the highest spikes in retail sales?

I cleaned and grouped sales data by date, identified the top 10 peaks, labeled them with key festivals or events, and used a bar chart to highlight when major sales spikes occur.

See my notebook for details: [4_Highest_spikes](4_Highest_spikes.ipynb)

```pyhton
 top10 = sales_per_day.nlargest(10, 'Quantity').copy()
spike_dates = top10['Order Date'].tolist()
results = []

for spike_date in spike_dates:
    date_sales = df[df['Order Date'] == spike_date].groupby('State')['Quantity'].sum().reset_index()
    date_sales = date_sales.sort_values('Quantity', ascending=False)
    if len(date_sales):
        top_state = date_sales.iloc[0]['State']
        qty = date_sales.iloc[0]['Quantity']
        results.append({'Date': spike_date.strftime('%d-%b-%Y'), 'State': top_state, 'Quantity': qty})

results_df = pd.DataFrame(results)
print(results_df)
```

# Result
![Highest_spikes](Images\Spikes.png)
*Shows what exactly triggered sales*

# Insights

- The biggest sales spikes occur during Bank Holidays, indicating these days drive exceptional shopping activity.

- Pre-Monsoon, Summer, and Winter Sales consistently generate high purchase volumes, showing strong consumer response to seasonal promotions.

- Festivals like Diwali, Makar Sankranti, and event periods such as Independence Day Eve are major triggers for increased sales, likely due to celebratory shopping and gifting traditions.

- Back-to-School and specific market events also contribute to notable sales peaks, highlighting key times for targeted campaigns and inventory planning.

# What i learned:
Through this retail analytics project, I learned how to perform customer segmentation using RFM analysis and K-means clustering, identify frequent product pairings with association rule mining, and analyze sales spikes triggered by business events and festivals. I strengthened my skills in data cleaning, feature engineering, and practical use of visualization tools to convert raw data into business insights.

# Challenges I faced:
I encountered challenges in merging and preprocessing messy datasets, determining the best clustering parameters, and tuning thresholds for association rules to get meaningful results. Interpreting clusters and rules in a business context, and labeling seasonal spikes accurately, also required attention to detail and domain understanding.

# Conclusion:
This project demonstrated how advanced analytics can uncover actionable insights in retail business—enabling smarter segmentation, optimized marketing, and inventory planning. Despite data and modeling challenges, the approach offers repeatable methods for driving sales and customer value in real-world scenarios.