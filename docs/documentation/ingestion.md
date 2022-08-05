Moving data around consistently, reliably and at scale is a complex problem to solve. However there are many potential solutions, especially when working with BigQuery.  The objective here is to land your data consistently into BigQuery in a low-cost (or ideally zero-cost), reliable and low maintenance manner.

# Data Ingestion Options

=== "Native Services"
    Using native services should be the preference here, whether it is direct integration with a Google product,
    direct connection from an external supplier or by leveraging a Data Transfer into BigQuery or Google Cloud Storage.  

=== "External Tables"
    External tables can be a very low-maintenance solution to data ingestion requirements, as data can be directly queried from Google Cloud Storage or Amazon S3 buckets without requiring loading of the data.  Files with the schema included (i.e. Parquet or Avro) are preferable to CSV or JSON files.  

=== "3rd Party Services" 
    3rd Party Services are essential when you need to integrate data sources without native connectors. We recommend that you pick a single service, and one where your costs do not scale with the volume of data ingested.  Currently we recommend:

    - [Airbyte](https://airbyte.com/): If you are comfortable deploying and maintaining your own instance on a virtual machine.
    - [Airbyte Cloud](https://cloud.airbyte.io/) or Dataddo](https://www.dataddo.com/): If you are not.

    Both are extensible, however Dataddo will build new connectors for you at no cost.  Airbyte will require more technical input (predominantly Python/Java), but has a friendly, active and fast-growing community. Dataddo is written in Go.

=== "Custom Development" 
    This is almost always a bad idea, even if you do have the technical capabilities and resources.  Continue down this path at your peril... then use a 3rd party specialist or platform to do this complex job reliably.