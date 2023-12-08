

### Goal of the Project
1. Practice advanced SQL skills including joining, grouping and manipulating.
2. Practice using Azure Databricks.

### Step 1: Find the Database
* After searching, I found I am interested in this Kaggle Database about game recommendations on Steam.
I like the topic; it has multiple tables so I can practice joining the tables.
* Anton Kozyriev. (2023). <i>Game Recommendations on Steam</i> [Data set]. Kaggle. https://doi.org/10.34740/KAGGLE/DS/2871694


### Step 2: Open a Databricks and Load the Data
1. Create a new Azure Databricks Service and assigned it to the default Resource group.
2. Create a Computing Cluster for the Databricks.
* Note: The first cluster I created failed to start because I created a multi-node cluster. Currently, you can only use Azure Student subscription to create a Single node cluster which will have one Driver node with 4 cores.
* Now with an organization subscription, I can create a multi-node cluster. But remember to configure a brief automated shutdown time to minimize credit expenses.
3. I tried to upload the files but it is too large, so I uploaded to DBFS, the Databricks File System.


### Step 3: Create Tables from the File Using Notebook
1. Create a dataframe from the csv file.
```python
# File location and type
file_location = "/FileStore/tables/recommendations.csv"
file_type = "csv"

# CSV options
infer_schema = "false"
first_row_is_header = "true"
delimiter = ","

# The applied options are for CSV files. For other file types, these will be ignored.
df = spark.read.format(file_type) \
  .option("inferSchema", infer_schema) \
  .option("header", first_row_is_header) \
  .option("sep", delimiter) \
  .load(file_location)

display(df)
```
<img width="963" alt="Screenshot 2023-10-10 at 8 36 36 PM" src="https://github.com/nogibjj/lisa-mini-project6/assets/46847817/27581573-66ea-4d40-914b-8b6cb133da95">

2. Create a Temp table in Python and Query the created table in SQL.
```python
# Create a view or table

temp_table_name = "recommendations_csv"

df.createOrReplaceTempView(temp_table_name)
```

```SQL
%sql

/* Query the created temp table in a SQL cell */

select * from `recommendations_csv`
```

3. Create a permanent table, so it can be reused next time or by different users.

```python
permanent_table_name = "recommendations"

df.write.format("parquet").saveAsTable(permanent_table_name)
```
4. Created tables for all csv file and checked the tables at the Catalog Explorer
<img width="1141" alt="Screenshot 2023-10-10 at 9 27 44 PM" src="https://github.com/nogibjj/lisa-mini-project6/assets/46847817/c3696750-1f24-4d26-9779-3a26b9a8cc9a">


### Step 4: Advanced Query on the Table
1. Write SQL query to find the user who reviewed most of the game, and list all of the game he/she has reviewd, and the review she gave to the games. Get the number of games she reviewed and the money she spent on the game
```SQL
SELECT
  u.user_id,
  u.products,
  u.reviews,
  COUNT(g.app_id) AS num_games_reviewed,
  SUM(g.price_original) AS money_spent_on_games
FROM default.users u
LEFT JOIN default.games g ON u.products LIKE CONCAT('%', g.app_id, '%')
GROUP BY u.user_id, u.products, u.reviews
ORDER BY num_games_reviewed DESC
LIMIT 1;
```


2. Write SQL query to filter and get all of the games that has the best review, and sort them by price, also list if they are available on MacOs. 

```SQL
SELECT
  app_id,
  title,
  CAST(price_original AS DECIMAL(10,2)) AS price_original,
  mac,
  user_reviews
FROM default.games
WHERE user_reviews = 'overwhelmingly positive'
ORDER BY price_original ASC;
```

* Note: The table is too large, with 10,000 rows, it took several minutes to run the query.

* Note: You can override the primary language by specifying the language magic command % at the beginning of a cell. For example: `%python` `%SQL%` `%md`

### Things to Explore
* Explore **Global init scripts** feature at Admin Account: Global init scripts run on all cluster nodes launched in your workspace. They can help you to enforce consistent cluster configurations across your workspace in a safe, visible, and secure manner.
* I have issue managing my account and enable Databricks Assistent. Need to debug it.

## CLI Demo

### Create Table


### List Tables

### Put Item



### Get Item



### Update Item


### Query Items


### Scan Items


### Delete Item


### Delete Table



### Upload Records via JSON


