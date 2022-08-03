BigQuery [Remote Functions](https://cloud.google.com/bigquery/docs/reference/standard-sql/remote-functions) are the mechanisms by which we can extend BigQuery functionality beyond the normal boundaries of a typical SQL-based data environment.  They are Google Cloud Functions which - when structured, deployed, authorized and triggered correctly - enable SQL queries to communicate to _any_ API in the outside world.  

This enables Transformation Flows to not just augment the data as the individual partitions move through the flow, but also to trigger external processes and actions for e.g. monitoring and observability or external file processing, all within a simple `SELECT` statement.

# Function Actions
Remote Functions are currently in private beta and have different authentication requirements depending on the specific use-case.  They operate in a variety of different ways, either:

- triggering an external action (e.g. BigQuery to Slack)
- returning a value to BigQuery from an external API (without authentication)
- returning a value to BigQuery from an external API (with authentication)
- accessing files on a GCS bucket and use an authenticated external API to process the files and return value to BigQuery

We use Google Secrets Manager to manage secrets, and the deployment process depends on whether the function needs access to your internal assets or API keys.

# Function Responses
Remote Functions return responses in JSON, which require decoding in BigQuery.  This process is built into the user-facing functions, with each function returning data in the most relevant structure.

# Existing Remote Functions
The following functions are developed and currently in a private beta testing phase, with docs in progress.  Note that GCS is Google Cloud Storage, which interoperates natively with BigQuery and other Google APIs.

- **BigQuery (Text-based Query Reponse) -> Slack Notification**
- **BigQuery (Text/Icon-based Query Reponse) -> Slack Notification**
- **GCS (Bucket Inventory) -> BigQuery**
- **GCS (PDF Documents) -> Google Document AI Parser -> BigQuery**
- **Google Data Transfer Service (Scheduled Queries) -> BigQuery**
- **GCS (Assets & Permissions) -> BigQuery**
- **Google Workspace (Google Groups Members) -> BigQuery**

# Remote Function Roadmap
The following functions are in an earlier stage of testing, but are on the development roadmap.  We are currently developing the process for additional feature requests, however we currently prioritise Google APIs due to the interoperability with other Google Cloud resources and the standardised authentication mechanisms. 

- **GCS (Image Files) -> Google Cloud Vision AI -> BigQuery (Image Annotations)**
- **GCS (Video Files) -> Google Cloud Video AI -> BigQuery (Video Annotations)**
- **GCS (Text Files) -> Google Cloud Natural Language AI -> BigQuery (Text Annotations)**
- **GCS (Text Files) -> Google Translation AI -> BigQuery (Text Translations)**
- **GCS (JSON Files) -> GCS (JSONL Files) -> BigQuery (External Table)**