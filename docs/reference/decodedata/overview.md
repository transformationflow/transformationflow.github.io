# Introduction
The Google Analytics 4 export to BigQuery has a schema which is very difficult to either query in SQL or analyse in connected tools.  Decode Data dynamically remodels this data into a foundational structure which is optimised for querying and analysis.

# Quickstart
The function used for deployment depends on your particular use case.  The following questions need to be answered in order to determine the specific deployment function.

Question | Description
--- | ---
App or Web | Is this a GA4 property for a website or app? This impacts the standard events, parameters and properties included in the decoded schema.
Single or Multi-Property | Do you want to deploy this onto a single property or multiple properties in the same region? Note that separate regions require separate deployments.
Data Profile | Do you want to extract the standard set of events, or the custom set of events observed in the actual data?
Data Integration Required | Do you need to align the output schemas to enable downstream integration of the data?

Invocation syntax and options are detailed in the [deployment function reference documentation](resources/deployment.md).

## Website Analytics

| App or Web | Single or Multi-Property | Data Profile | Data Integration Required | Deployment Function
| :-: | :-: | :-: | :-: | ---
| Web | Single | Standard | :material-close: | `GA4_STANDARD_WEB`
| Web | Single | Standard | :material-check: | `GA4_STANDARD_WEB`
| Web | Multi | Standard | :material-close: | `GA4_STANDARD_WEB`
| Web | Multi | Standard | :material-check: | `GA4_STANDARD_WEB`
| Web | Single | Custom | :material-close: | `GA4_CUSTOM_WEB`
| Web | Single | Custom | :material-check: | `GA4_CUSTOM_WEB_INTEGRATED`
| Web | Multi | Custom | :material-close: | `GA4_CUSTOM_WEB`
| Web | Multi | Custom | :material-check: | `GA4_CUSTOM_WEB_INTEGRATED`

## App Analytics

| App or Web | Single or Multi-Property | Data Profile | Data Integration Required | Deployment Function
| :-: | :-: | :-: | :-: | ---
| App | Single | Standard | :material-close: | `GA4_STANDARD_APP`
| App | Single | Standard | :material-check: | `GA4_STANDARD_APP`
| App | Multi | Standard | :material-close: | `GA4_STANDARD_APP`
| App | Multi | Standard | :material-check: | `GA4_STANDARD_APP`
| App | Single | Custom | :material-close: | `GA4_CUSTOM_APP`
| App | Single | Custom | :material-check: | `GA4_CUSTOM_APP_INTEGRATED`
| App | Multi | Custom | :material-close: | `GA4_CUSTOM_APP`
| App | Multi | Custom | :material-check: | `GA4_CUSTOM_APP_INTEGRATED`