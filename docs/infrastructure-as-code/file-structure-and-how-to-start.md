---
title: File Structure and How to Start
---
# Intro
Once you start on your journey of putting your solutions into IaC (Infrastructure as Code) you quite easily start to question which way is the correct way to do things. Let's start things off simple and assume you are the only one working on the project for now and are starting from scratch.

If you are already not familiar with how Bicep works for Azure solutions, check out the page detailing the structure and how it works.

# First Step
The first step would be to create a single file which does everything you need, for an example:
```Bicep
resource resourceGroupDeployment 'Microsoft.Resources/resourceGroups@2025-04-01' = {
  location: northeurope
  name: integrations-dev
}

resource resourceGroupDeployment 'Microsoft.Resources/resourceGroups@2025-04-01' = {
  location: northeurope
  name: integrations-test
}

resource resourceGroupDeployment 'Microsoft.Resources/resourceGroups@2025-04-01' = {
  location: northeurope
  name: integrations-prod
}
```

Now this has all the things you need. The problem that arises from this one is that this deployment will deploy all the resources into all the environments every time. Not ideal if you want a more finely tuned control over the environments. To resolve this issue you can simply create three different files, one for each environment.

You might easily come across another problem later on when you have multiple resources across these files and manually adding to each one will make it more difficult to keep them organized. One of the main things about having a test environment is making sure that the production environment works, so what you need is the assurance that you put the same resources to test and prod.

# Parameters and Environments
Next step from this one would be to go back to having a single file for the resources using parameters, and another file per environment to handle all the changes.
```Bicep
param location string
param name string

resource resourceGroupDeployment 'Microsoft.Resources/resourceGroups@2025-04-01' = {
  location: location
  name: name
}
```
```Bicep
using 'main.bicep'

param location = 'northeurope'
param name = 'integrations-dev'
```

This is now already looking a lot better and we can rest easier knowing that each environment is deployed the same way. At some point you might start to question if deploying all the resources in one file is the best way to go. Maybe you have some high level settings in one resource and you only want to modify the configuration of it instead.

# Separation of Resources and Configurations
Resource deployment and configuration

# Modules
One resource type for all of your needs, logic is there is one template file for one resource type that is then used in other files. This allows you to make changes in one place and have it reflected in all other places.

# Versioning
v1 and v2 folders when you are working as a team, just like all other versioning, this comes into play when you have multiple places where changes would need to be implemented, or someone else is also using your repository as a source. If you have som breaking changes done to the solution, make a new version. Then it can be adopted into use individually instead of breaking solutions right away.

# Importing
Importing common variables and steps

> More on this in here: https://learn.microsoft.com/en-us/azure/azure-resource-manager/bicep/parameter-files?tabs=Bicep

# TLDR

- Pipelines, import and steps
- Stages and UI elements