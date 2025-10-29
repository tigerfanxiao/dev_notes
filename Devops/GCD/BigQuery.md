
```python
from google.cloud import bigquery

# Create a "Client" object
client = bigquery.Client()
# Construct a reference to the "hacker_news" dataset
dataset_ref = client.dataset("hacker_news", project="bigquery-public-data")

# API request - fetch the dataset
dataset = client.get_dataset(dataset_ref)
# Construct a reference to the "full" table
table_ref = dataset_ref.table("full")
# API request - fetch the table
table = client.get_table(table_ref)

```

![[Pasted image 20241013113854.png]]


```python
# Print information on all the columns in the "full" table in the "hacker_news" dataset
table.schema

# Preview the first five lines of the "full" table
client.list_rows(table, max_results=5).to_dataframe()

# Preview the first five entries in the "by" column of the "full" table
client.list_rows(table, selected_fields=table.schema[:1], max_results=5).to_dataframe()
```