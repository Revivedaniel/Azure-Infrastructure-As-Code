# Free Tier MERN Environment
This IAC environment sets up commonly needed infrastucture for a MERN stack web app.
<br/>
This type of environment is good for Dev/Hobby projects

There are 3 ways to deploy this IAC. Azure Bicep, Azure Resource Manager(ARM) Template, and Terraform. 
Each folder contains all the files you need to deploy the environment.

## Resources in this Environment
* Static Web App (For a React frontend)
* App Service (For the Express Backend)
* App Service Plan (This controls the compute resources such as OS, region, and price tier)
* Azure Cosmos DB for MongoDB account (This is a great NoSQL DB solution that makes it simple to migrate MERN projects that use MongoDB and Mongoose ODM to Azure maintained DBs)
* Application Insights (To track analytics and other metrics for your express backend)
#### NOTE: Application Insights is only free to a certain extent. Once you breach a certain threshold of insights captured, you will begin accruing cost. 
For more information, read these docs [Application Insights - Is it free?](https://learn.microsoft.com/en-us/azure/azure-monitor/faq#is-it-free-)
#### NOTE: CosmosDB for MongoDb will have the enableFreeTier set to true, only one free tier CosmosDB resource is allowed per subscription.
#### If you receive a deployment error regarding this, set enableFreeTier: false.
#### Serverless is nearly free at dev/hobby levels of reads and writes so I would not worry too much about using serverless without the free tier.
for more information, read these docs: [Azure Cosmos DB free tier](https://learn.microsoft.com/en-us/azure/cosmos-db/free-tier)

## Parameters
### DatabaseName
This is the name given to the CosmosDB account. 
This parameter has to satisfy the following: 
* Be completely unique (These accounts create domain records for the URI so these must be unique enough to have not been used by someone else)
* Must be Lowercase letters, numbers, and hyphens.
* Start with lowercase letter or number.
* Character limit: 3-44
* Value must be a string ""

### StaticWebAppName
This is the name given to the Static Web App (you frontend React App)<br/>
This parameter is pretty open and just has a max character limit of 40

### location
This is the location where all the resources will be created.<br/>
It is best practive to choose the location nearest your end users.<br/>
In this case, I would use the location nearest to you: [Azure Geographies](https://azure.microsoft.com/en-us/explore/global-infrastructure/geographies/#geographies)

## Tags
Tags are a great tool to keep track of cost across multiple resources, projects, and cost centers.<br/>
These templates do not include tags but you can easily add them by following the setps below:

### ARM Template
You can add a tags property to the resources like below:
```json
"tags": {
    "TagName1": "TagValue1",
    "TagName2": "TagValue2"
},
```
The end result might look like this:
```json
{
    "type": "Microsoft.Storage/storageAccounts",
    "apiVersion": "2021-04-01",
    "name": "[concat('storage', uniqueString(resourceGroup().id))]",
    "location": "[parameters('location')]",
    "sku": {
      "name": "Standard_LRS"
    },
    "kind": "Storage",
    "tags": {
      "TagName1": "TagValue1",
      "TagName2": "TagValue2"
    },
    "properties": {}
}
```

### Bicep
You can add a tags property to the resources like below:
```txt
tags: {
  TagName1: 'TagValue1'
  TagName2: 'TagValue2'
}
```
```txt
resource stgAccount 'Microsoft.Storage/storageAccounts@2021-04-01' = {
  name: 'storage${uniqueString(resourceGroup().id)}'
  location: location
  sku: {
    name: 'Standard_LRS'
  }
  kind: 'Storage'
  tags: {
    TagName1: 'TagValue1'
    TagName2: 'TagValue2'
  }
}
```

### Terraform
You can add a tags property to the resources like below:
```tf
tags = {
  TagName1 = "TagValue1"
  TagName2 = "TagValue2"
}
```
```tf
resource "azurerm_storage_account" "example" {
  name                     = "storageaccountname"
  resource_group_name      = azurerm_resource_group.example.name
  location                 = azurerm_resource_group.example.location
  account_tier             = "Standard"
  account_replication_type = "GRS"

  tags = {
    TagName1 = "TagValue1"
    TagName2 = "TagValue2"
  }
}
```