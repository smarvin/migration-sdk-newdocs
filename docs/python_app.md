---
title: Writing a Python Application
layout: docs
---

The Migration SDK has a Python wrapper that utilizes the .Net binaries. You can choose to use that or write their own implementation. Input required for this application is essentially the same as that of the .Net application.

## Required elements
The main elements required for a basic migration are listed in the following sections.

### Configuration

#### Source
* `ServerURL`
* `SiteContentUrl`
* `AccessTokenName`
* `AccessToken`

### Destination
* `SiteContentUrl`
* `AccessTokenName`
* `AccessToken`
* `ServerUrl`

### Migration plan

## Example code

The following is some Python code to get started with. The code assumes you already have the tableau_migration module installed.

```
Unset

import configparser         # configuration parser
from tableau_migration.migration_engine import PyMigrationPlanBuilder
from tableau_migration.migration_engine_migrators import PyMigrator
import helper               # Helper code. Must be imported after tableau_migration modules

if __name__ == '__main__':
    planBuilder = PyMigrationPlanBuilder()
    migration = PyMigrator()

    config = configparser.ConfigParser()
    config.read('config.DEV.ini')

    # Build the plan
    planBuilder = planBuilder \
                .from_source_tableau_server(config['SOURCE']['URL'], config['SOURCE']['SITE_CONTENT_URL'], config['SOURCE']['ACCESS_TOKEN_NAME'], config['SOURCE']['ACCESS_TOKEN']) \
                .to_destination_tableau_cloud(config['DESTINATION']['SITE_CONTENT_URL'], config['DESTINATION']['ACCESS_TOKEN_NAME'], config['DESTINATION']['ACCESS_TOKEN'], config['DESTINATION']['URL'],) \
                .for_server_to_cloud() \
                .with_tableau_id_authentication_type()        

    helper.AddUserTransformer(planBuilder)

    plan = planBuilder.build()

    # Run the migration 
    result = migration.execute(plan)

    print("All done")
```
