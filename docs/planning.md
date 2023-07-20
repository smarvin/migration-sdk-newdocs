---
title: Planning Your Migration
description: Some considerations before you begin using the Tableau Migration SDK.
layout: docs
---

## Differences between Tableau Server and Tableau Cloud
While Tableau Server and Tableau Cloud are remarkably similar, there are some differences between the two products that impact how you migrate. We will call those differences out within specific steps in “How to Migrate,” but here are some helpful resources to help you understand how those differences may impact the migration plan you design.

<!--- Link to stuffLicensing, high level differences, specific examples
Version differences - link to Release Navigator --->

>There are some differences in Tableau Server and Tableau Cloud that can stop some customers from migrating to Tableau Cloud. These recourses are not designed to be conclusive for a complete platform evaluation. Please work with your account team or a Tableau Partner to understand if a migration to Tableau Cloud is not right for you at this time.

## What this Migration SDK does not do
Need to create a list of un-supported content types and point to manual processes of re-creation and third-parties who provide offerings. Include in this list aspects of the migration (pre-environment screens, matching of content, verification of migration, etc)

## What to do in Cloud before using the Tableau Migration SDK

Need to include the prerequisites before opening the SDK. 
* Get a Site
* Choose your preferred region
* Set up Tableau Bridge
* Configure Site Settings
* Determine security model (Authentication and Authorization)
* etc

## In-Flight Transformations
To account for these differences, and to simplify the migration process, many migration steps will transform content as it migrates from Tableau Server to Tableau Cloud. 

Sentence on what transformers are and define standard and default transformers and how to recognize them.

## API Throttling
Tableau Cloud has published Site Capacity limits to ensure all customers have a consistent performance experience. This includes limits on how many API calls a User can make on a specific Tableau Cloud Site within an hour. The Migration SDK has built in throttling limits to help you plan your migration in a way that navigates these limits. However, in cases where your migration plan exceeds the published hourly limits, the SDK will batch your migration over multiple hours to avoid errors. 