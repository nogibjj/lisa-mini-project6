

### Goal of the Project
1. Practice advanced SQL skills including joining, grouping and manipulating.
2. Practice using Azure Databricks.

### Step 1: Find the Database
* After searching, I found I am interested in this Kaggle Database about game recommendations on Steam.
I like the topic; it has multiple tables so I can practice joining the tables.
* Anton Kozyriev. (2023). <i>Game Recommendations on Steam</i> [Data set]. Kaggle. https://doi.org/10.34740/KAGGLE/DS/2871694


### Step 2: Open a Databricks and Load the Data
1. I created a new Azure Databricks Service and assigned it to the default Resource group.
2. I created a Computing Cluster for the Databricks.
3. I tried to upload the files but it is too large, so I uploaded to DBFS, the Databricks File System.
* Note: The first cluster I created failed to start because I created a multi-node cluster. Currently, you can only use Azure Student subscription to create a Single node cluster which will have one Driver node with 4 cores.


### Step 3: Create Tables from the File
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


















# DynamoDB Demos

## CLI Demo

### Create Table

```bash
aws dynamodb create-table \
    --table-name customers \
    --attribute-definitions \
        AttributeName=customer_id,AttributeType=N \
    --key-schema \
        AttributeName=customer_id,KeyType=HASH \
    --provisioned-throughput \
        ReadCapacityUnits=5,WriteCapacityUnits=5
```

### List Tables

```bash
aws dynamodb list-tables
```

### Put Item

```bash
aws dynamodb put-item \
    --table-name customers \
    --item \
        '{"customer_id": {"N": "1"}, "name": {"S": "John Doe"}}'
```

### Get Item

```bash
aws dynamodb get-item \
    --table-name customers \
    --key \
        '{"customer_id": {"N": "1"}}'
```

### Update Item

```bash
aws dynamodb update-item \
    --table-name customers \
    --key \
        '{"customer_id": {"N": "1"}}' \
    --update-expression \
        "SET #name = :name" \
    --expression-attribute-names \
        '{"#name": "name"}' \
    --expression-attribute-values \
        '{":name": {"S": "Jane Doe"}}'
```

### Query Items

```bash
aws dynamodb query \
    --table-name customers \
    --key-condition-expression \
        "customer_id = :customer_id" \
    --expression-attribute-values \
        '{":customer_id": {"N": "1"}}'
```

### Scan Items

```bash
aws dynamodb scan \
    --table-name customers
```

### Delete Item

```bash
aws dynamodb delete-item \
    --table-name customers \
    --key \
        '{"customer_id": {"N": "1"}}'
```

### Delete Table

```bash
aws dynamodb delete-table \
    --table-name customers
```


### Upload Records via JSON

batch write json to dynamo
```
aws dynamodb 

```


```json
// records.json

[
  {
    "customer_id": {"N": "1"}, 
    "name": {"S": "John Doe"}
  },
  {
    "customer_id": {"N": "2"},
    "name": {"S": "Jane Smith"} 
  },
  {
    "customer_id": {"N": "3"},
    "name": {"S": "Bob Johnson"}
  },
  {
    "customer_id": {"N": "4"},
    "name": {"S": "Sarah Davies"}
  },
  {
    "customer_id": {"N": "5"},
    "name": {"S": "Mike Wong"}
  }
]
```




## References

