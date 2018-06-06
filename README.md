<details>
<summary><h1>The Client</h1></summary>
<p>
      
The boom of e-commerce has resulted in online retailers servicing billions of customers <sup><a href = https://www.statista.com/statistics/251666/number-of-digital-buyers-worldwide/>[1]</a></sup>. Furthermore, it is also estimated that 96% of Americans shop online <sup><a href = http://www.cpcstrategy.com/blog/2017/05/ecommerce-statistics-infographic/>[2]</a></sup>. With so many options in the realm of online shopping, a major goal in e-commerce is customer retention. A common approach to achieving this is through specialized marketing techniques. It is not feasible, however, to develop marketing techniques specific to each individual customer. If the customer pool can be segmented into smaller groups, then the time required to develop effective marketing techniques can be reduced -- which is why it is important for companies to truly understand their customers.

</p>
</details>

<details>
<summary><h1>The Problem</h1></summary>
<p>
  
Online retailers have enormous customer pools and customers often share similar traits. There may be, however, a better way to identify customers than simply grouping them by their location. Customers are a source of revenue and it would be more advantageous to segment customers based on metrics that are associated with direct profit. 

How can we identify the *best* customers? 

</p>
</details>

<details>
<summary><h1>The Data</h1></summary>
<p>
Customer data has been obtained from an online retailer. The dataset is provided in the form of several tables obtained from a relational database. The primary key is the `customer_id`. The dataset only contains customer data prior to connection to an account manager. The following features are provided in the data:

* Total sales per customer
* Total orders per customer
* Total sales per Product Types per customer
* Total orders per Product Types per customer
* Total sales per Fiscal Quarter per customer
* Total orders per Fiscal Quarter per customer
* Customer shipping and billing zip code
* Cohort for each account manger

Per the request of the data provider, sensitive information regarding actual numbers for sales and amount of customers were scrubbed. Only the relative amounts are allowed to be presented. The product type categories were replaced with dummy names. 

Customer sales does exist from other sources (e.g. Kaggle, UCI Machine Learning Repository), but they do not come from companies that use a business model that utilizes personal account managers.

</p>
</details>

<details>
<summary><h1>Cleaning The Data</h1></summary>
<p>
      
The raw data is provided via several small tables. Each individual table is pivoted so that each row represents a record for a single customer. The pivoted tables are then merged together into a combined dataset. 

A sample of the raw data for customer purchases per `product_type` is shown below:

| CUSTOMER_ID | PRODUCT_TYPE | SALES |
|-------------|-----------|---------|
| cid1 | A | 799.9 |
| cid1 | C | 531.87 |
| cid1 | D | 375.96 |
| cid2  | B | 1062.31 |
| cid2  | E | 892.72 |
| cid2  | F | 366.22 |
| cid2  | G | 360.64 |
| cid2  | D | 239.9 |
| cid3  | B | 4256.53 |
| cid3  | C | 2612.06 |
| cid3  | H | 1235.27 |

The table after pivoting:

| CUSTOMER_ID | A |   C   | D | G | B | E | F | H |
|-------------|-------|---------|----------|----------------------|---------|-------|-------------|--------------|
|     cid1    | 799.9 |     0   |  375.96  |           0          |    0    |   0   |     0       |      0       |
|     cid2    |   0   |  139.95 |  239.9   |         360.64       | 1062.31 | 892.72|   366.22    |      0       |
|     cid3    | 799.9 | 2612.06 |  375.96  |           0          | 4256.53 |   0   |     0       |  1235.27     |

There are a total of 21 distinct product types. If a customer did not make any purchases for a given `product_type`, a `0` value is used. The same process is applied to the other raw data tables. The pivoted tables are joined together on the `customer_id` key to create a dataset formatted for analysis.

Furthermore, the data was pivoted so that each row represented information on a single customer. Using the `pivot_table` method in `pandas` resulted in numerous `null` values and they were replaced with `0`. The data was further reduced to only customers with a billing address located in the US. 

The customer locations data was filtered to only include customers located in the US. The data contains a state abbreviations column that had to be cleaned to deal with situations like:
* inconsistencies in upper and lower case abbrevations
* full state name used instead of abbrevations
</p>
</details>
      
<details>
<summary><h1>Exploratory Data Analysis</h1></summary>
<p>

<details>
<summary><h2>Customer Location</h1></summary>
<p>
      
The distribution of customer's in the sample per state is shown below:

![customer_state_distribution]

Majority of the customers in the dataset are located in `CA`, `CO`, `NY`, `UT`, `WA` and `TX`. Three of those states are ranked as the top 4 in the US Census Bureau's population ranking <sup><a href = https://en.wikipedia.org/wiki/List_of_U.S._states_and_territories_by_population>[3]</a></sup>. It is interesting, however, to note that so many customers are also located in fairly low population states (`CO`,`UT`).

</p>
</details>


<details>
<summary><h2>Product Type</h1></summary>
<p>
      
The overall distribution of customer sales per `product_type` is shown below:

![customer_pt_distribution]

The distribution shows that products from type `B` are the most expensive as very few customers have purchased from that category, but it has generated the most sales. Conversely, `A` and `F` products appear to be cheaper items due to the discrepancy from number of customers who have purhcased it and the sales generated.

</p>
</details>

<details>
<summary><h2>Seasonality</h1></summary>
<p>

The distribution of orders and sales per fiscal quarter is shown below:

![qtr_distribution]



</p>
</details>


A heatmap of Pearson correlation coefficients calculated for a set of features is shown below:

![pt_heatmap]

The feature set consists of the 21 categories of `product_type`, amount of sales per fiscal quarter (e.g. `Q1_sales`), and amount of orders per fiscal quarter (e.g., `Q2_orders).




</p>
</details>



<details>
<summary><h1>Customer Segmentation</h1></summary>
<p>
    
</p>
</details>

<details>
<summary><h1>Network Analysis</h1></summary>
<p>
    
</p>
</details>

<details>
<summary><h1>Recommender System</h1></summary>
<p>
    
</p>
</details>

[customer_state_distribution]:https://github.com/wsjk/Capstone_2/tree/master/report/eda/customers_per_state.png
[customer_pt_distribution]:https://github.com/wsjk/Capstone_2/tree/master/report/eda/customer_distribution.png
[qtr_distribution]:https://github.com/wsjk/Capstone_2/tree/master/report/eda/quarter_distribution.png
[pt_heatmap]:https://github.com/wsjk/Capstone_2/tree/master/report/eda/heatmap.png
