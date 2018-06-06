<details>
<summary><h1>The Client</h1></summary>
<p>
      
The boom of e-commerce has resulted in online retailers servicing millions of customers. Retaining customers so that they continue purchasing on their site requires specialized marketing techniques. It is not feasible, however, to use marketing techniques specific to each individual customer. If the customer pool can be segmented into smaller groups, then the time required to develop effective marketing techniques can be reduced.

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
      
</p>
</details>
      

<details>
<summary><h1>Exploratory Data Analysis</h1></summary>
<p>
    
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

![pre_conn_md]

[pre_conn_md]:https://github.com/wsjk/Capstone_2/blob/master/report/pre_conn_md.png
