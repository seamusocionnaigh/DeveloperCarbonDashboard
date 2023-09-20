# The Salesforce Developer Carbon Dashboard

This dashboard uses Event Monitoring and CRM Analytics to illustrate the estimated carbon emissions associated with an org’s usage of Salesforce App Server CPU time via specific features and products. The dashboard does not calculate the entire carbon emissions footprint of an org. The carbon emission coefficient for a given instance is subject to change based on several factors including the carbon intensity of the electrical grid the instance relies on and the App Server SKUs serving that instance.
Information in this dashboard on carbon emissions is intended for informational purposes and should not be used in regulatory or financial statements. Salesforce maintains [net zero residual emissions](https://www.salesforce.com/company/sustainability/faq/), seeking to reduce emissions in line with a [1.5°C future](https://news.mit.edu/2023/explained-climate-benchmark-rising-temperatures-0827) while compensating for remaining emissions by procuring high-quality renewable energy and carbon credits. Learn more about Salesforce’s commitment to sustainability [here](https://www.salesforce.com/company/sustainability/).


![image](https://github.com/seamusocionnaigh/DeveloperCarbonDashboard/assets/20658634/cd93735b-3c01-4eb3-ac6b-6265f4837e2d)


## Methodology:
Carbon per mms of Core App Server time is derived from a PUE-adjusted carbon coefficient per data center for both AWS and Salesforce first party data centers. This figure is calculated as the emissions factor in grams per megawatt hour multiplied by the PUE per datacenter. The PUE-adjusted carbon coefficient is then scaled against each App Server SKU types’ daily megawatt hour and divided by the available processing seconds, in millions, for that specific App Server SKU to get to the estimated carbon per million milliseconds of compute time.
These per-SKU, per-data center carbon coefficients are then combined with data on the App Server SKU types that serve each Salesforce instance to produce a single carbon coefficient per instance. Data on the App Server SKU types comprising Salesforce instances are current as of current as of June 2023.

## Licence Requirements

This CRM Analytics templated app requires Event Monitoring and CRM Analytics Growth / Plus.  Growth licences are preferred in order to store the large number of event logs.  **This app will not work with either Event Monitoring or CRM Analytics licences on their own**.

## Installation Instructions

**Note:** The app has a dependency on:
*  The latest version of the Event Monitoring analytics app (Version 58.0) with the Append beta feature enabled
*  only one Event Monitoring analytics app created in the target org.
    *  Multiple or subsequent Event Monitoring analytics apps creates metadata components with incremental names (e.g. ApexExecution2). Currently the Developer Carbon Dashboard will not use these subsequent apps and components.  You may have to delete all existing Event Monitoring analytics apps and recreate a single app using v58.0 with the Append beta feature enabled.

### Prerequisite: Setting Up The Log Ingestion process:

Easy Option: [CRM Analytics Templated App](https://trailhead.salesforce.com/content/learn/modules/event_monitoring_analytics)

* (If the "Event Monitoring Analytics" CRM Analytics app is already running with the below-listed log types, you can skip to Package Deployment)
* Select a sandbox or production org
* [Ensure Event Monitoring Analytics is enabled](https://help.salesforce.com/s/articleView?id=sf.bi_app_event_monitor_enable_select_PSL.htm&type=5)
* [Assign permissions to users](https://help.salesforce.com/s/articleView?id=bi_app_event_monitor_create_permsets.htm&type=5&language=en_US)
* [Create the analytics app](https://help.salesforce.com/s/articleView?language=en_US&type=5&id=sf.bi_app_admin_wave_create.htm)
    * *Hard Requirement*: **must** include following log types:
      * ApexSoapWithUsers
      * ApexRestApiWithUsers
      * RestApiWithUsers
      * APIWithUsers
      * BulkApiWithUsers
      * DashboardWithUsers
      * ReportWithUsers
      * URIWithUsers
      * VisualforceRequestWithUsers

Harder Option: [Set up a custom process to push Event Logs into CRM Analytics](https://www.salesforcehacker.com/2015/01/simple-script-for-loading-event.html)

### Deploying Developer Carbon Dashboard

There are two options:

* Deploy CRM Analytics Template Bundle via sfdx source or mdapi deployments
  * Deploy **only** the WaveTemplateBundle metadata type
  * In Analytics Studio:
    * Create -> App -> Developer Carbon Dashboard -> Select existing Event Monitoring app -> name app ("Developer Carbon Dashboard") -> Create
* Deploy CRM Analytics assets via sfdx source or mdapi deployments
  * Deploy all metadata types in package.xml **except** WaveTemplateBundle
  * Upload misc/Instance Carbon Per CPU Reference.csv to Developer Carbon Dashboard app via UI
  * Run Carbon Emissions and Carbon Emissions Apex recipes
  * Edit Carbon Emissions dashboard
    * Edit image placeholder
    * Upload all .png files in misc folder

## Solution Overview

![Developer Carbon Dashboard ERD](https://github.com/seamusocionnaigh/DeveloperCarbonDashboard/assets/20658634/f0228f16-bda5-4113-a5e1-99051d5be7cb)
