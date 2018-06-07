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
      
The code to conduct the analysis presented in this section can be found in  
[eda_for_customers.ipynb](/notebooks/eda_for_customers.ipynb)

<details>
<summary><h2>Customer Location</h1></summary>
<p>
      
The distribution of customer's in the sample per state is shown below:

![customer_state_distribution](Capstone_2/report/eda/customers_per_state.png?raw=True "")

Majority of the customers in the dataset are located in `CA`, `CO`, `NY`, `UT`, `WA` and `TX`. Three of those states are ranked as the top 4 in the US Census Bureau's population ranking <sup><a href = https://en.wikipedia.org/wiki/List_of_U.S._states_and_territories_by_population>[3]</a></sup>. It is interesting, however, to note that so many customers are also located in fairly low population states (`CO`,`UT`).

</p>
</details>


<details>
<summary><h2>Product Type</h1></summary>
<p>
      
The overall distribution of customer sales per `product_type` is shown below:

![customer_pt_distribution](/report/eda/customer_distribution.png?raw=true "")

The distribution shows that products from type `B` are the most expensive as very few customers have purchased from that category, but it has generated the most sales. Conversely, `A` and `F` products appear to be cheaper items due to the discrepancy from number of customers who have purhcased it and the sales generated.

</p>
</details>

<details>
<summary><h2>Seasonality</h1></summary>
<p>

The distribution of orders and sales per fiscal quarter is shown below:

![qtr_distribution](/report/eda/quarter_distribution.jpg?raw=True "")

As expected, the states with the most customers have the highest orders and sales per quarter. Although, both `CO` and `UT` customers outperform `NY` every quarter despite a smaller customer pool -- which may indicate that `CO` and `UT` customers are more valuable than `NY` customers.

It is also interesting to note is that `Q2_orders` total is significantly higher for `NY` than every other state by a wide margin. This did not directly translate to higher sales in the plot for `Q2_sales`. The discrepancy in `Q2_orders` and `Q2_sales` can be attributed to `NY` customers buying less expensive items.

</p>
</details>

<details>
<summary><h2>Feature Correlation</h1></summary>
<p>
      
A heatmap of Pearson correlation coefficients calculated for the dataset is shown below:

![pt_heatmap](/report/eda/heatmap.png?raw=True "")

The feature set consists of the 21 categories of `product_type`, amount of sales per fiscal quarter (e.g. `Q1_sales`), and amount of orders per fiscal quarter (e.g., `Q2_orders`). Majority of the features have positive, but weak linear correlation to each other. There are some instances of high correlation between `product_type`s (e.g. `F` and `G`) which can be interpreted as `product_type`s that customers often buy together. 

There are also several examples of higher correlation coefficients between `product_type` and quarterly orders/sales (e.g., `F` and `Q3_orders`) which indicate that customers who buy in Q3 often buy items from `F`.

Fairly strong correlation is aso observed among quarterly sales. The highest correlation coefficients among all features can be found between `Q4_sales` and `Q1_sales`. This could possibly be due to customers buying gifts for the holiday season in `Q4` and having to make gift exchanges in `Q1` --  and exchange orders would count as a new sale.

A bubble chart showing the relationship between `product_type`, `state`, number of customers, and sales is shown below

![bubbles](/report/eda/product_type_state_color.png?raw=True "")

The color of the markers represent the number of unique customers that have purchased given a `product_type` and `state`. The size of the markers represent the average of the sales made by those same customers. 

The bubble chart shows that the most popular `product_type`s -- based on number of customers who have purchased them -- are `A`,`F`,`K`,`Q`, and `T` for most locations. The most sales are generated from `product_type` `E` have across all almost states. 

Specific `product_type`s are much popular in certain states than others. It appears that customers from military addresses (`AE`) have generated the  most sales for `D` type products. While customers in Guam (`GU`) have spent the most money on `C`, `D` and `B`. Products from `H` and `K` are quite popular among customers in the Northern Mariana Islands and Virgin Islands (`VI`) customers are fond of `Q` items.

A closer look at the state with the most customers (`CA`) shows that products from `K` have been bought from the most customers and also generated the most total sales. 

![CA](/report/eda/product_type_per_state/product_type_hist_for_CA.png?raw=True "")

</p>
</details>


</p>
</details>


<details>
<summary><h1>Customer Segmentation</h1></summary>
<p>
      
The code to conduct the unsupervised learning and clustering analysis presented in this section can be found in  
[clustering.ipynb](/notebooks/clustering.ipynb)

<details>
<summary><h2>Processing Data</h2></summary>
<p>
      
An unsupervised learning approach is used to conduct customer segmentation. The goal is to use clustering algorithms to be able to segment customers into informative groups.

The dataset is kept to continuous numerical features: the `product_type`s, quarterly sales and orders. The data is scaled using the `StandardScaler()` method from `sci-kit learn` library. After scaling the data, Principal Components Analysis (PCA) is applied to the dataset. The resulting `explained_variance_ratio` from PCA is:

|    PCA0   |    PCA1   |    PCA2    |    PCA3    |    PCA4    |    PCA5    |    PCA6    |    PCA7    |    PCA8    |    PCA9    |    PCA10   |    PCA11   |    PCA12   |    PCA13   |    PCA14   |    PCA15   |    PCA16   |    PCA17  |    PCA18   |    PCA19   |    PCA20   |    PCA21   |    PCA22   |    PCA23   |    PCA24   |    PCA25   |    PCA26   |    PCA27   | PCA28 |
|-----------|-----------|------------|------------|------------|------------|------------|------------|------------|------------|------------|------------|------------|------------|------------|------------|------------|-----------|------------|------------|------------|------------|------------|------------|------------|------------|------------|------------|-------|
| 0.2284605 | 0.2893555 | 0.34382091 | 0.39476022 | 0.43490012 | 0.47336997 | 0.50902827 | 0.54393445 | 0.57858124 | 0.61295025 | 0.64680443 | 0.68018976 | 0.71259298 | 0.74354075 | 0.77250861 | 0.79802764 | 0.82239615 | 0.8442303 | 0.86501432 | 0.88359106 | 0.90179775 | 0.91936953 | 0.93643075 | 0.95107138 | 0.96453712 | 0.97538785 | 0.98470915 | 0.99278416 |  1.0  |

There are 29 features in the dataset and after conducting PCA, it takes 21 principial components to explain at least 90% of the variance in the data.

</p>
</details>


<details>
<summary><h2>KMeans Clustering</h2></summary>
<p>
      
For the first attempt at clustering, `MiniBatchKMeans` algorithm is used from the `sci-kit learn` library due to limited time and resources. From `sci-kit learn` documentation <sup><a href = http://scikit-learn.org/stable/modules/clustering.html#mini-batch-kmeans>[3]</a></sup>:
      
      The MiniBatchKMeans is a variant of the KMeans algorithm which uses mini-batches to reduce the computation time, while still attempting to optimise the same objective function. Mini-batches are subsets of the input data, randomly sampled in each training iteration. These mini-batches drastically reduce the amount of computation required to converge to a local solution. 

The dataset was processed using PCA with 21 components prior to applying `MiniBatchKMeans`. The number of clusters was set to `6` for the clustering analysis.

Plots of the predicted labels of the dataset from `MiniBatchKMeans` are provided below. The 2-D plots show pairs of principcal components in sequential order.

![kmeans](/report/clustering/KMEANS.jpg?raw=true "")

</p>
</details>


<details>
<summary><h2>DBSCAN</h2></summary>
<p>
      
`DBSCAN` algorithm from `sci-kit learn` is evaluated on its ability to cluster customers. Compared to `KMeans`, one of the advantages of using `DBSCAN` is that the number of clusters does not have to be pre-defined. Furthermore, `DBSCAN` performs better for data that may not conform to globular chunks<sup><a href = http://scikit-learn.org/stable/modules/generated/sklearn.cluster.DBSCAN.html>[4]</a></sup>:

      DBSCAN is a density based algorithm – it assumes clusters for dense regions. It is also the first actual clustering algorithm we’ve looked at: it doesn’t require that every point be assigned to a cluster and hence doesn’t partition the data, but instead extracts the ‘dense’ clusters and leaves sparse background classified as ‘noise’.

![dbscan](/report/clustering/DBSCAN.jpg?raw=true "")

</p>
</details>


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


