# Transformation Flow Methodology

## Introduction
Just as there is no single way to write SQL code, there is no single way to think about and write data transformations.  However there are some guiding principles which support development of transformations which are clean, readable, and are as simple to maintain, monitor and update as possible.

## Logical Definition vs. Deployment
The dividing line between these two activities is one which is frequently blurred, and one which it is critical to understand when planning data transformations.  

In its essence, the logical definition of a data transformation is the abstract sequence of actions through which each row of ingested data needs to be transformed, and the deployment approach is how this is practically implemented.

Consider two end-to-end data pipelines with _exactly_ the same data transformation requirements.






Just as the decoupling of storage and compute in modern cloud data warehouses has 





Depending on the scale and type of data that you are processing, the deployment aproach will vary massively in order to manage performance and cost.  

### Logical Flow
In its purest form, The logical data transformation ...



### Deployment
Scheduled Queries

DBT/Dataform, 



### Considerations

On one end of the spectrum, a small dataset containing a few hundred thousand rows and appending a few thousand rows a day will not require a complex transformation deployment strategy.

#### Size
#### Update Frequency (Velocity)
#### Query Frequency




## Modular Thinking

## Cost Optimistation


## Practical Approach



### 1. Data Profiling
### 2. Data Type Correction


### 3. Computation

## SQL Guide
There are many excellent SQL style guides available, all of which are opinionated by definition and many of which are inconsistent with each other.  To indent or not to indent?  Upper or lower case keywords?  Multi-line or sing line statements? Subqueries or Common Table Expressions?  Actually that last one's easy (nearly always Common Table Expressions), but for these and many other questions there are different schools of thought on how you should structure and format your SQL code.  

Since Transformation Flow functions automate generation of more verbose or complex SQL queries, the SQL style does need to be explicitly defined and should be clear in all of the worked examples and code snippets.  As such it - and any human-written SQL code - adheres consistently to a few conventions:
- Keywords (e.g. SELECT, FROM, JOIN) always in upper case 
- Column names in snake case (e.g. `customer_id`, `ingest_timestamp`)
- Keywords on new lines where it enhances readability
- Common Table Expressions used in all queries for modularity
- One Common Table Expression performs only one action type
- Common Table Expression names in snake case, explaining the specific action (e.g. `filter_for_most_recent_record`) 
- Avoid nested subqueries at (nearly) all costs

However, in reality there are times when the SQL developer needs to use his or her judgement in how to structure and format code.  In some circumstances it might be clearer to put a `CASE` statement on multiple lines, but in other cases a single line could result in more readable code.

So, in homage to the Zen of Python, we propose a subset of these guidelines to outline our approach to SQL.  Futher discussion of each of these points, along with some guidance around interpretation is included in the [link] section.

### Start with the data

### Should You Ever `SELECT * `?
The first SQL query you will ever write will probably start with `SELECT * FROM...`, which means 'SELECT every column from...' but is it right to use this?  You will see many guides which explicitly and unequivocally state that this is **bad practice** and should never be done!  In reality, the situation is a little more nuanced.

#### The Case For `SELECT *`
The first reason you'll need to `SELECT *` is simple: understanding your data.  You might have some documentation or general feel for the structure and content of the data you're working with, but you need to start with a `SELECT *` in order to bring the full dataset into your flow (potentially with a with a `WHERE` clause to limit the data returned).  Let's be clear, understanding your data is not simply selecting the data and giving the first 200 lines a cursory glance in table format.  This might be acceptable for a relatively small dataset which fits onto a screen or two, but you can draw some very wrong conclusions from visually inspecting a small sample from a large dataset so beware!

In some SQL dialects (including BigQuery) you can add an `EXCEPT ()` statement to exclude specific columns which you excplicity do not want, potentially to avoid duplicate column names or to exclude columns which are known to be uninformative, not useful or simply not used.

The benefit to this approach is that by not explicitly naming every column you want, if new columns appear in the data then they will automatically be included in your downstream flow.  Your query will also be more concise and therefore quicker to write, saving you - the SQL developer/author/magician - valuable time. 

#### The Case Against `SELECT *`
However you may _not_ want new columns to automatically appear in your dataset or downstream flow!  By explicitly naming the columns you need then your query will be more verbose (not a problem when it's just a small set of columns, but when you have hundreds can quickly become extremely annoying and time-consuming), but it will also be more robust.  Imagine a scenario where a new column is introduced into the inbound data which conflicts with an important column name used in your downstream flow.  

In some SQL dialects, this would cause the query to fail - which could have significant implications for downstream consumers of data.  In BigQuery the query would not fail, but the second introduction of the column name would be aliased with a numerical suffix and any calculations, joins or aggregations which were intended to operate on this field would instead be operating on the new column.  This new column - the one which magically appeared in the inbound data thanks to our use of `SELECT *` - could potentially have completely different contents, meaning and/or a different data type to the original column.  It's hard to say which is worse.  In the first case there would be disruption but also indication of a problem: an error, but not passed silently.  In the second case there might be no disruption, but the data could be silently polluted and the downstream actions and decisions compromised.

Either is undesirable, and since predictability is more important than flexibility then it appears you should beware `SELECT *` statements if you want to build robust data transformation flows.

#### Conclusion
As with all complex questions reduced to a binary choice, the answer is clear and consistent: it depends!  However there is a hybrid approach which gains you the best of both worlds - building a transformation flow which: 

- Automatically recognises new columns
- Does not require the SQL developer to manually define every single column required
- Does not potentially result in naming collisions between new and existing columns
- Can potentially highlight mismatches between column counts and column names 

This is implemented by composing the flow from both SQL queries and `txflow` functions, composed as follows:
- Initial Common Table Expression (CTE) as a `SELECT *` query
- Subsequent CTE generated by function at time of flow build (`txflow.transform.select_all_explicit`), resulting in explicit naming of each existing field in auto-generated verbose query

The transformation flow monitoring console then displays column counts and column names through each flow stage, highlighting the new column and enabling inclusion via rebuilding the transformation flow.  This can then be tested before deployment to ensure there are neither downstream unwelcome surprises or undetected, silent errors.
