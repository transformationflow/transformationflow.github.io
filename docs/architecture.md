---
hide:
  - navigation
---
# Data Architecture
Now that we have clearly defined **data** as a store of information, which itself has location, structure and content, and whose context is described by metadata, we will dig deeper into the practicalities of how data moves from point of collection to downstream use-cases.

## Data Collection
- optimised for application use-case
- e.g. mobile devices of users
- SaaS capture this in internal databases
- realm of the **software engineer**

## Data Storage
- aggregated from collection channels
- could be a set of files, packets of data etc.
- could be a storage bucket, database, dropbox, server etc.
- typically arrives from what is termed a **pipeline**
- realm of the **data engineer**

## Data Analytics Engineering
- optimised for end use-case e.g. dashboards, machine learning
- inbound profiling and monitoring  
- cleaning and transformation of data
- optimising transformation deployment
- outbound orchestration
- realm of the **analytics engineer**

# Data Analytics
- Ah-hoc data exploration
- Report and dashboard development
- Uses traditional Business Intelligence tools or potentially notebooks
- realm of the **data analyst**

# Data Science
- Machine learning model development
- Predictive models
- Fancy stuff
- realm of the **data scientist**