---
title: File Structure and How to Start
date: 2025-07-16
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
using './main.bicep'

param location = 'northeurope'
param name = 'integrations-dev'
```

This is now already looking a lot better and we can rest easier knowing that each environment is deployed the same way.

# Separation of Resources and Configurations
At some point you might start to question if deploying all the resources in one file is the best way to go. You might have added a storage account to the existing bicep deployment files and started to think that maybe it's a bit unnecessary to have all the resources in one file.

```Bicep
param location string
param resourceGroupName string
param storageAccountName string

resource resourceGroupDeployment 'Microsoft.Resources/resourceGroups@2025-04-01' = {
  location: location
  name: resourceGroupName
}

resource storageAccountDeployment 'Microsoft.Storage/storageAccounts@2025-01-01' = {
  location: location
  name: storageAccountName
}
```
```Bicep
using './main.bicep'

param location = 'northeurope'
param resourceGroupName = 'integrations-dev'
param storageAccountName = 'intstoragedev'
```

In this case you can just separate the files into their own folders to keep them separated and name them accordingly to easily keep track on what is where.

├─ ResourceGroup
│  ├─ resourceGroup.bicep
│  ├─ resourceGroup.dev.bicepparam
│  ├─ resourceGroup.test.bicepparam
│  └─ resourceGroup.prod.bicepparam
└─ StorageAccount
   ├─ storageAccount.bicep
   ├─ storageAccount.dev.bicepparam
   ├─ storageAccount.test.bicepparam
   └─ storageAccount.prod.bicepparam

Maybe you also have some high level settings in one resource and you only want to modify the configuration of it instead. This will become more evident in solutions like Logic Apps Standard and Functions where you can have one solution that you would want to deploy without changing the high level settings. For these components I would recommend just simply separating the bicep deployment files into base resources that are propably deployed only once and then configuration files which use an existing resource.

Now you should have the resources neatly in different files and you only deploy the ones that are changed, this also helps with keeping track on what has been deployed and when. It should also improve performance since Azure is not checking the template for changes.

├─ ResourceGroup
│  ├─ resourceGroup.bicep
│  ├─ resourceGroup.dev.bicepparam
│  ├─ resourceGroup.test.bicepparam
│  └─ resourceGroup.prod.bicepparam
└─ LogicApps
   ├─ Infrastructure
   │  ├─ logicApp.bicep
   │  ├─ logicApp.dev.bicepparam
   │  ├─ logicApp.test.bicepparam
   │  └─ logicApp.prod.bicepparam
   └─ AppSettings
      ├─ appSetting.bicep
      ├─ appSetting.dev.bicepparam
      ├─ appSetting.test.bicepparam
      └─ appSetting.prod.bicepparam

# Modules
Next up on the list is the discussion on modules, if you have a larger environment where some things might be repeating multiple times, like the deployment of a storage account. If you want to update or make changes to it, it quickly becomes a hassle to deal with and is prone to mistakes due to having to manually update many different files. Luckily there is a fairly easy answer to this one and that is modules.

Instead of having a Storage Account resource in your bicep file every time, you can create a single resource file, and refer to it from anothe bicep file when needed. This is a fairly straightforward change and all you need to do is replace resouce with module and then use a file path to it. Do note that the parameter names that you use in the module file will also be reflected when referring to it.

```Bicep
param location string
param storageAccountName string

module storageAccountDeployment '../storageAccount.bicep' = {
  location: location
  storageAccountName: storageAccountName
}
```

Now you can easily make changes to resources and have it take effect on the rest of the solution. The modules do not need a separate parameter file, because it will provide those from the original location and pass it through to the module file.

├─ StorageAccount
│   ├─ storageAccount.bicep
│   ├─ storageAccount.dev.bicepparam
│   ├─ storageAccount.test.bicepparam
│   └─ storageAccount.prod.bicepparam
└─ Modules
   └─ StorageAccount
      └─ storageAccount.bicep

> Documentation: https://learn.microsoft.com/en-us/azure/azure-resource-manager/bicep/modules

# Versioning
So, what happens if you want to make changes to the module file that could break solutions and you don't have the time to update everything or you are working in a team? The easy answer is versioning. This is simply adding a folder in front of the Modules side and adding a new one whenever it seems appropriate, usually when there are changes to the parameters. Then it can be adopted into use individually instead of breaking solutions right away by just changing the file path to the new version.

├─ StorageAccount
│   ├─ storageAccount.bicep
│   ├─ storageAccount.dev.bicepparam
│   ├─ storageAccount.test.bicepparam
│   └─ storageAccount.prod.bicepparam
└─ Modules
   └─ StorageAccount
      ├─ v1
      │  └─ storageAccount.bicep
      └─ v2
         └─ storageAccount.bicep

# What should be in a single deployment file?


# Importing
Importing common variables and steps

> More on this in here: https://learn.microsoft.com/en-us/azure/azure-resource-manager/bicep/parameter-files?tabs=Bicep

# TLDR

- Pipelines, import and steps
- Stages and UI elements