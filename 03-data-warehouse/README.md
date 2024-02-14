## Data Warehouse and BigQuery

- [Slides](https://docs.google.com/presentation/d/1a3ZoBAXFk8-EhUsd7rAZd-5p_HpltkzSeujjRGB2TAI/edit?usp=sharing)  
- [Big Query basic SQL](big_query.sql)


### Data Warehouse

- [Data Warehouse and BigQuery](https://youtu.be/jrHljAoD6nM)

### Partitoning and clustering

- [Partioning and Clustering](https://youtu.be/jrHljAoD6nM?t=726)  
- [Partioning vs Clustering](https://youtu.be/-CqXf7vhhDs)  

### Best practices

- [BigQuery Best Practices](https://youtu.be/k81mLJVX08w)  

### Internals of BigQuery

- [Internals of Big Query](https://youtu.be/eduHi1inM4s)  

### Advanced

#### ML
[BigQuery Machine Learning](https://youtu.be/B-WtpB0PuG4)  
[SQL for ML in BigQuery](big_query_ml.sql)

**Important links**
- [BigQuery ML Tutorials](https://cloud.google.com/bigquery-ml/docs/tutorials)
- [BigQuery ML Reference Parameter](https://cloud.google.com/bigquery-ml/docs/analytics-reference-patterns)
- [Hyper Parameter tuning](https://cloud.google.com/bigquery-ml/docs/reference/standard-sql/bigqueryml-syntax-create-glm)
- [Feature preprocessing](https://cloud.google.com/bigquery-ml/docs/reference/standard-sql/bigqueryml-syntax-preprocess-overview)

##### Deploying ML model

- [BigQuery Machine Learning Deployment](https://youtu.be/BjARzEWaznU)  
- [Steps to extract and deploy model with docker](extract_model.md)  



### Homework

* [Homework](../cohorts/2023/week_3_data_warehouse/homework.md)
* Docomentation of the homework:
import io
import pandas as pd
import requests
if 'data_loader' not in globals():
    from mage_ai.data_preparation.decorators import data_loader
if 'test' not in globals():
    from mage_ai.data_preparation.decorators import test


@data_loader
def load_data_from_api(*args, **kwargs):
    
    urls = [
'https://d37ci6vzurychx.cloudfront.net/trip-data/green_tripdata_2022-01.parquet','https://d37ci6vzurychx.cloudfront.net/trip-data/green_tripdata_2022-02.parquet',
'https://d37ci6vzurychx.cloudfront.net/trip-data/green_tripdata_2022-03.parquet','https://d37ci6vzurychx.cloudfront.net/trip-data/green_tripdata_2022-04.parquet',
'https://d37ci6vzurychx.cloudfront.net/trip-data/green_tripdata_2022-05.parquet','https://d37ci6vzurychx.cloudfront.net/trip-data/green_tripdata_2022-06.parquet',
'https://d37ci6vzurychx.cloudfront.net/trip-data/green_tripdata_2022-07.parquet','https://d37ci6vzurychx.cloudfront.net/trip-data/green_tripdata_2022-08.parquet',
'https://d37ci6vzurychx.cloudfront.net/trip-data/green_tripdata_2022-09.parquet','https://d37ci6vzurychx.cloudfront.net/trip-data/green_tripdata_2022-10.parquet',
'https://d37ci6vzurychx.cloudfront.net/trip-data/green_tripdata_2022-11.parquet','https://d37ci6vzurychx.cloudfront.net/trip-data/green_tripdata_2022-12.parquet'

]


    
    dfs = [pd.read_parquet(url) for url in urls]    
    
    data = pd.concat(dfs, ignore_index=True)

    data['lpep_pickup_datetime'] = pd.to_datetime(data['lpep_pickup_datetime']).dt.date

    data['lpep_dropoff_datetime'] = pd.to_datetime(data['lpep_dropoff_datetime']).dt.date

    return data

@test
def test_output(output, *args) -> None:
    """
    Template code for testing the output of the block.
    """
    assert output is not None, 'The output is undefined'

Import to GC Bucket:
from mage_ai.settings.repo import get_repo_path
from mage_ai.io.config import ConfigFileLoader
from mage_ai.io.google_cloud_storage import GoogleCloudStorage
from pandas import DataFrame
from os import path

if 'data_exporter' not in globals():
    from mage_ai.data_preparation.decorators import data_exporter


@data_exporter
def export_data_to_google_cloud_storage(df: DataFrame, **kwargs) -> None:
    """
    Template for exporting data to a Google Cloud Storage bucket.
    Specify your configuration settings in 'io_config.yaml'.

    Docs: https://docs.mage.ai/design/data-loading#googlecloudstorage
    """
    config_path = path.join(get_repo_path(), 'io_config.yaml')
    config_profile = 'default'

    bucket_name = 'mage-zoomcamp-tobi-wissen'
    object_key = 'nyc_green_taxi_homework.parquet'

    GoogleCloudStorage.with_config(ConfigFileLoader(config_path, config_profile)).export(
        df,
        bucket_name,
        object_key,
    )
Partionend input to GC Bucket:
import pyarrow as pa
import pyarrow.parquet as pq
import os

if 'data_exporter' not in globals():
    from mage_ai.data_preparation.decorators import data_exporter

os.environ['GOOGLE_APPLICATION_CREDENTIALS'] ="/home/src/tobi-mage.json"

bucket_name = 'mage-zoomcamp-tobi-wissen'
project_id = 'tobi-mage-zoom-321'
table_name = "nyc_green_taxi_data_homework"
root_path = f'{bucket_name}/{table_name}'

@data_exporter
def export_data(data, *args, **kwargs):
    data['lpep_pickup_date'] = data['lpep_pickup_datetime'].dt.date

    table = pa.Table.from_pandas(data)
    gcs = pa.fs.GcsFileSystem()

    pq.write_to_dataset(
        table,
        root_path=root_path,
        partition_cols = ['lpep_pickup_date'],
        filesystem = gcs
    )
    
Questions in BigQuery:
#Create External Table for the Homework. Both from the partionized dataset and without
CREATE EXTERNAL TABLE `earnest-goal-410521.ny_taxi.green_tripdata_2022_partionized`
  OPTIONS (
    format ="Parquet",
    uris = ['gs://mage-zoomcamp-tobi-wissen/nyc_green_taxi_data_homework/lpep_pickup_date=*.parquet']
    );

CREATE OR REPLACE EXTERNAL TABLE `ny_taxi.green_tripdata_2022`
OPTIONS (
  format = 'Parquet',
  uris = ['gs://mage-zoomcamp-tobi-wissen/nyc_green_taxi_homework.parquet']
);

#Question 1 what is the count for the dataset?
SELECT count(*) FROM `ny_taxi.green_tripdata_2022`;

#Question 2 Write a query to count the distinct number of PULocationIDs for the entire dataset on both the tables.What is the estimated amount of data that will be read when this query is executed on the External Table and the Table? First create a table from the external table

create or replace table ny_taxi.local_green_tripdata_2022 as Select * from `ny_taxi.green_tripdata_2022`;

SELECT COUNT(DISTINCT(PULocationID)) FROM `ny_taxi.green_tripdata_2022`;

SELECT COUNT(DISTINCT(PULocationID)) FROM `ny_taxi.local_green_tripdata_2022`;

#Question 3: How many records have a fare_amount of 0?

select count(fare_amount)
from `ny_taxi.green_tripdata_2022`
where fare_amount =0;

#Question 4: Partionend table
create or replace table ny_taxi.local_green_tripdata_2022_partionend
PARTITION BY DATE(lpep_pickup_datetime) as( Select * from `ny_taxi.green_tripdata_2022`);

#Question 5: Write a query to retrieve the distinct PULocationID between lpep_pickup_datetime 06/01/2022 and 06/30/2022 (inclusive)

#Use the materialized table you created earlier in your from clause and note the estimated bytes. Now change the table in the from clause to the partitioned table you created for question 4 and note the estimated bytes processed. What are these values? 

select * from `ny_taxi.local_green_tripdata_2022`LIMIT 10

select distinct (PULocationID)
from `ny_taxi.local_green_tripdata_2022`
where lpep_pickup_datetime >='2022-06-01' and lpep_pickup_datetime <='2022-06-30'
group by PULocationID;

---------------------------------------------------------------------------

HOMEWORK OF THE WORKSHOP:
Question 1:
sum of the value
def square_root_generator(limit):
    n = 1
    while n <= limit:
        yield n ** 0.5
        n += 1

# Example usage:
limit = 5
generator = square_root_generator(limit)
z=0
for sqrt_value in generator:
    z=sqrt_value+z
    print(sqrt_value)
    print("Summiert",z)

Question 2:
I changed limit to 13 and used the last value

Question 3:
def people_1():
    for i in range(1, 6):
        yield {"ID": i, "Name": f"Person_{i}", f"Age": 25 + i, "City": "City_A"}

alter=0
for person in people_1():
    print(person)
    alter=person.get("Age")+alter
    print(alter)


def people_2():
    for i in range(3, 9):
        yield {"ID": i, "Name": f"Person_{i}", f"Age": 30 + i, "City": "City_B", "Occupation": f"Job_{i}"}
for person in people_2():
    print(person)
    alter=person.get("Age")+alter
    print(alter)

Question 4:




## Community notes

Did you take notes? You can share them here.

* [Notes by Alvaro Navas](https://github.com/ziritrion/dataeng-zoomcamp/blob/main/notes/3_data_warehouse.md)
* [Isaac Kargar's blog post](https://kargarisaac.github.io/blog/data%20engineering/jupyter/2022/01/30/data-engineering-w3.html)
* [Marcos Torregrosa's blog post](https://www.n4gash.com/2023/data-engineering-zoomcamp-semana-3/) 
* [Notes by Victor Padilha](https://github.com/padilha/de-zoomcamp/tree/master/week3)
* [Notes from Xia He-Bleinagel](https://xiahe-bleinagel.com/2023/02/week-3-data-engineering-zoomcamp-notes-data-warehouse-and-bigquery/)
* [Bigger picture summary on Data Lakes, Data Warehouses, and tooling](https://medium.com/@verazabeida/zoomcamp-week-4-b8bde661bf98), by Vera
* [Notes by froukje](https://github.com/froukje/de-zoomcamp/blob/main/week_3_data_warehouse/notes/notes_week_03.md)
* [Notes by Alain Boisvert](https://github.com/boisalai/de-zoomcamp-2023/blob/main/week3.md)
* [Notes from Vincenzo Galante](https://binchentso.notion.site/Data-Talks-Club-Data-Engineering-Zoomcamp-8699af8e7ff94ec49e6f9bdec8eb69fd)
* Add your notes here (above this line)
