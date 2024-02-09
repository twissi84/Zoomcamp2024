> If you're looking for Airflow videos from the 2022 edition,
> check the [2022 cohort folder](../cohorts/2022/week_2_data_ingestion/). <br>
> If you're looking for Prefect videos from the 2023 edition,
> check the [2023 cohort folder](../cohorts/2023/week_2_data_ingestion/).

# Week 2: Workflow Orchestration

Welcome to Week 2 of the Data Engineering Zoomcamp! 🚀😤 This week, we'll be covering workflow orchestration with Mage.

Mage is an open-source, hybrid framework for transforming and integrating data. ✨

This week, you'll learn how to use the Mage platform to author and share _magical_ data pipelines. This will all be covered in the course, but if you'd like to learn a bit more about Mage, check out our docs [here](https://docs.mage.ai/introduction/overview). 

* [2.2.1 - 📯 Intro to Orchestration](#221----intro-to-orchestration)
* [2.2.2 - 🧙‍♂️ Intro to Mage](#222---%EF%B8%8F-intro-to-mage)
* [2.2.3 - 🐘 ETL: API to Postgres](#223----etl-api-to-postgres)
* [2.2.4 - 🤓 ETL: API to GCS](#224----etl-api-to-gcs)
* [2.2.5 - 🔍 ETL: GCS to BigQuery](#225----etl-gcs-to-bigquery)
* [2.2.6 - 👨‍💻 Parameterized Execution](#226----parameterized-execution)
* [2.2.7 - 🤖 Deployment (Optional)](#227----deployment-optional)
* [2.2.8 - 🧱 Advanced Blocks (Optional)](#228----advanced-blocks-optional)
* [2.2.9 - 🗒️ Homework](#229---%EF%B8%8F-homework)
* [2.2.10 - 👣 Next Steps](#2210----next-steps)

## 📕 Course Resources

### 2.2.1 - 📯 Intro to Orchestration

In this section, we'll cover the basics of workflow orchestration. We'll discuss what it is, why it's important, and how it can be used to build data pipelines.

Videos
- 2.2.1a - [What is Orchestration?](https://youtu.be/Li8-MWHhTbo)

Resources
- [Slides](https://docs.google.com/presentation/d/17zSxG5Z-tidmgY-9l7Al1cPmz4Slh4VPK6o2sryFYvw/)

### 2.2.2 - 🧙‍♂️ Intro to Mage

In this section, we'll introduce the Mage platform. We'll cover what makes Mage different from other orchestrators, the fundamental concepts behind Mage, and how to get started. To cap it off, we'll spin Mage up via Docker 🐳 and run a simple pipeline.

Videos
- 2.2.2a - [What is Mage?](https://youtu.be/AicKRcK3pa4)
- 2.2.2b - [Configuring Mage](https://youtu.be/2SV-av3L3-k)
- 2.2.2c - [A Simple Pipeline](https://youtu.be/stI-gg4QBnI)

Resources
- [Getting Started Repo](https://github.com/mage-ai/mage-zoomcamp)
- [Slides](https://docs.google.com/presentation/d/1y_5p3sxr6Xh1RqE6N8o2280gUzAdiic2hPhYUUD6l88/)

### 2.2.3 - 🐘 ETL: API to Postgres

Hooray! Mage is up and running. Now, let's build a _real_ pipeline. In this section, we'll build a simple ETL pipeline that loads data from an API into a Postgres database. Our database will be built using Docker— it will be running locally, but it's the same as if it were running in the cloud.

Videos
- 2.2.3a - [Configuring Postgres](https://youtu.be/pmhI-ezd3BE)
- 2.2.3b - [Writing an ETL Pipeline](https://youtu.be/Maidfe7oKLs)

Resources
- [Taxi Dataset](https://github.com/DataTalksClub/nyc-tlc-data/releases/download/yellow/yellow_tripdata_2021-01.csv.gz)
- [Sample loading block](https://github.com/mage-ai/mage-zoomcamp/blob/solutions/magic-zoomcamp/data_loaders/load_nyc_taxi_data.py)


### 2.2.4 - 🤓 ETL: API to GCS

Ok, so we've written data _locally_ to a database, but what about the cloud? In this tutorial, we'll walk through the process of using Mage to extract, transform, and load data from an API to Google Cloud Storage (GCS). 

We'll cover both writing _partitioned_ and _unpartitioned_ data to GCS and discuss _why_ you might want to do one over the other. Many data teams start with extracting data from a source and writing it to a data lake _before_ loading it to a structured data source, like a database.

Videos
- 2.2.4a - [Configuring GCP](https://youtu.be/00LP360iYvE)
- 2.2.4b - [Writing an ETL Pipeline](https://youtu.be/w0XmcASRUnc)

Resources
- [DTC Zoomcamp GCP Setup](../week_1_basics_n_setup/1_terraform_gcp/2_gcp_overview.md)

### 2.2.5 - 🔍 ETL: GCS to BigQuery

Now that we've written data to GCS, let's load it into BigQuery. In this section, we'll walk through the process of using Mage to load our data from GCS to BigQuery. This closely mirrors a very common data engineering workflow: loading data from a data lake into a data warehouse.

Videos
- 2.2.5a - [Writing an ETL Pipeline](https://youtu.be/JKp_uzM-XsM)

### 2.2.6 - 👨‍💻 Parameterized Execution

By now you're familiar with building pipelines, but what about adding parameters? In this video, we'll discuss some built-in runtime variables that exist in Mage and show you how to define your own! We'll also cover how to use these variables to parameterize your pipelines. Finally, we'll talk about what it means to *backfill* a pipeline and how to do it in Mage.

Videos
- 2.2.6a - [Parameterized Execution](https://youtu.be/H0hWjWxB-rg)
- 2.2.6b - [Backfills](https://youtu.be/ZoeC6Ag5gQc)

Resources
- [Mage Variables Overview](https://docs.mage.ai/development/variables/overview)
- [Mage Runtime Variables](https://docs.mage.ai/getting-started/runtime-variable)

### 2.2.7 - 🤖 Deployment (Optional)

In this section, we'll cover deploying Mage using Terraform and Google Cloud. This section is optional— it's not *necessary* to learn Mage, but it might be helpful if you're interested in creating a fully deployed project. If you're using Mage in your final project, you'll need to deploy it to the cloud.

Videos
- 2.2.7a - [Deployment Prerequisites](https://youtu.be/zAwAX5sxqsg)
- 2.2.7b - [Google Cloud Permissions](https://youtu.be/O_H7DCmq2rA)
- 2.2.7c - [Deploying to Google Cloud - Part 1](https://youtu.be/9A872B5hb_0)
- 2.2.7d - [Deploying to Google Cloud - Part 2](https://youtu.be/0YExsb2HgLI)

Resources
- [Installing Terraform](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli)
- [Installing `gcloud` CLI](https://cloud.google.com/sdk/docs/install)
- [Mage Terraform Templates](https://github.com/mage-ai/mage-ai-terraform-templates)

Additional Mage Guides
- [Terraform](https://docs.mage.ai/production/deploying-to-cloud/using-terraform)
- [Deploying to GCP with Terraform](https://docs.mage.ai/production/deploying-to-cloud/gcp/setup)

### 2.2.8 - 🗒️ Homework 

We've prepared a short exercise to test you on what you've learned this week. You can find the homework [here](../cohorts/2024/02-workflow-orchestration/homework.md). This follows closely from the contents of the course and shouldn't take more than an hour or two to complete. 😄

### 2.2.9 - 👣 Next Steps

Congratulations! You've completed Week 2 of the Data Engineering Zoomcamp. We hope you've enjoyed learning about Mage and that you're excited to use it in your final project. If you have any questions, feel free to reach out to us on Slack. Be sure to check out our "Next Steps" video for some inspiration for the rest of your journey 😄.

Videos
- 2.2.9a - [Next Steps](https://youtu.be/uUtj7N0TleQ)

Resources
- [Slides](https://docs.google.com/presentation/d/1yN-e22VNwezmPfKrZkgXQVrX5owDb285I2HxHWgmAEQ/edit#slide=id.g262fb0d2905_0_12)

### 📑 Additional Resources

- [Mage Docs](https://docs.mage.ai/)
- [Mage Guides](https://docs.mage.ai/guides)
- [Mage Slack](https://www.mage.ai/chat)


# Community notes

Did you take notes? You can share them here:

## 2024 notes

* Add your notes above this line
HOMEWORK SUBMISSION 2024:
Copied of the word without the diagram:
Homework Module 2 Mage:
Diagram:
only visible in Word
 
Code load_api_data
import io
import pandas as pd
import requests
if 'data_loader' not in globals():
    from mage_ai.data_preparation.decorators import data_loader
if 'test' not in globals():
    from mage_ai.data_preparation.decorators import test

@data_loader
def load_data_from_api(*args, **kwargs):
    
    urls = ['https://github.com/DataTalksClub/nyc-tlc-data/releases/download/green/green_tripdata_2020-10.csv.gz', 'https://github.com/DataTalksClub/nyc-tlc-data/releases/download/green/green_tripdata_2020-11.csv.gz','https://github.com/DataTalksClub/nyc-tlc-data/releases/download/green/green_tripdata_2020-12.csv.gz']

    taxi_dtypes = {
        'VendorID': pd.Int64Dtype(),
        'passenger_count': pd.Int64Dtype(),
        'trip_distance': float,
        'RatecodeID': pd.Int64Dtype(),
        'store_and_fwd_flag': str,
        'PULocationID': pd.Int64Dtype(),
        'DOLocationID': pd.Int64Dtype(),
        'payment_type': pd.Int64Dtype(),
        'fare_amount': float,
        'extra': float,
        'mta_tax': float,
        'tip_amount': float,
        'tolls_amount': float,
        'improvement_surcharge': float,
        'total_amount': float,
        'congestion_surcharge': float 
    }
    

    parse_dates = ['lpep_pickup_datetime', 'lpep_dropoff_datetime']
    
    dfs = [pd.read_csv(url, sep=",", compression="gzip", dtype=taxi_dtypes,parse_dates=parse_dates) for url in urls]    
    
    data = pd.concat(dfs, ignore_index=True)

    return data

@test
def test_output(output, *args) -> None:
    """
    Template code for testing the output of the block.
    """
    assert output is not None, 'The output is undefined'


code transform_datas_needed:
import pandas as pd

if 'transformer' not in globals():
    from mage_ai.data_preparation.decorators import transformer
if 'test' not in globals():
    from mage_ai.data_preparation.decorators import test

@transformer
def transform(data, *args, **kwargs):

    data['lpep_pickup_date'] = pd.to_datetime(data['lpep_pickup_datetime']).dt.date
    #data['monat'] = data['lpep_pickup_datetime'].dt.month
    data['monats'] = pd.to_datetime(data['lpep_pickup_datetime']).dt.month

    print("Rows im Oktober:",data['monats'].isin([1]).sum())
    print("Rows im Oktober:",data['monats'].isin([2]).sum())
    print("Rows im Oktober:",data['monats'].isin([3]).sum())
    print("Rows im Oktober:",data['monats'].isin([4]).sum())
    print("Rows im Oktober:",data['monats'].isin([5]).sum())
    print("Rows im Oktober:",data['monats'].isin([6]).sum())
    print("Rows im Oktober:",data['monats'].isin([7]).sum())
    print("Rows im Oktober:",data['monats'].isin([8]).sum())
    print("Rows im Oktober:",data['monats'].isin([9]).sum())
    print("Rows im Oktober:",data['monats'].isin([10]).sum())
    print("Rows im Oktober:",data['monats'].isin([11]).sum())
    print("Rows im Oktober:",data['monats'].isin([12]).sum())

    print(data['monats'].unique())
    return data[data['monats']>=10]

@test
def test_output(output, *args) -> None:
    """
    Template code for testing the output of the block.
    """
    assert output is not None, 'The output is undefined'

code transform_passengers
if 'transformer' not in globals():
    from mage_ai.data_preparation.decorators import transformer
if 'test' not in globals():
    from mage_ai.data_preparation.decorators import test

@transformer
def transform(data, *args, **kwargs):
    # Specify your transformation logic here
    print("Rows with 0 Passengers:",data['passenger_count'].isin([0]).sum())

    return data[data['passenger_count']>0]

@test
def test_output(output, *args) -> None:
    """
    Template code for testing the output of the block.
    """
    assert output is not None, 'The output is undefined'


code transform_trip_distance
if 'transformer' not in globals():
    from mage_ai.data_preparation.decorators import transformer
if 'test' not in globals():
    from mage_ai.data_preparation.decorators import test

@transformer
def transform(data, *args, **kwargs):

    print(data['VendorID'].unique())
    print("Rows with 0 Trip Distance:",data['trip_distance'].isin([0]).sum())

    return data[data['trip_distance']>0]
   

@test
def test_output(output, *args) -> None:
    """
    Template code for testing the output of the block.
    """
    assert output is not None, 'The output is undefined'


code taxi_to_gcp_partionend:

import pyarrow as pa
import pyarrow.parquet as pq
import os

if 'data_exporter' not in globals():
    from mage_ai.data_preparation.decorators import data_exporter

os.environ['GOOGLE_APPLICATION_CREDENTIALS'] ="/home/src/tobi-mage.json"

bucket_name = 'mage-zoomcamp-tobi-wissen'
project_id = 'tobi-mage-zoom-321'
table_name = "nyc_green_taxi_data"
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



code write_taxi_to_bigquery
Select * from {{df_1}}





