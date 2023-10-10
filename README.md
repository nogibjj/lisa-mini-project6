

### Goal of the Project
1. Practice advanced SQL skills including joining, grouping and manipulating.
2. Practice using Azure Databricks.

### Step 1: Find the Database
 @misc{anton kozyriev_2023,
	title={Game Recommendations on Steam},
	url={https://www.kaggle.com/ds/2871694},
	DOI={10.34740/KAGGLE/DS/2871694},
	publisher={Kaggle},
	author={Anton Kozyriev},
	year={2023}
}
After searching, I found I am interested in this Kaggle Database about game recommendations on Steam.
I like the topic; it has multiple tables so I can practice joining the tables.


### Step 2: Open a Databricks and Load the Data
1. I created a new Azure Databricks Service and assigned it to the default Resource group.
2. I created a Computing Cluster for the Databricks.
3. I tried to upload the files but it is too large, so I uploaded to DBFS, the Databricks File System.

### Step 3: Create Tables from the File


















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

