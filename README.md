# Serverless 
- 
# IoT Hub 
- example my driving : https://azure.microsoft.com/en-us/campaigns/mydriving/

# Stream Analytics 

# Event Hubs

# Azure Functions 
## Scinario 
- Webhook
- Timer 
## output 
- Azure Event Hubs
- Azure Queue Storage 
  - 
- Azure Blob Storage 

## serverless webapp tutorial 
link : https://docs.microsoft.com/en-us/azure/functions/tutorial-static-website-serverless-api-with-database 

### 1) create a resource group
```
$ az group create -n first-serverless-app -l westcentralus
{
  "id": "/resourceGroups/first-serverless-app",
  "location": "westcentralus",
  "managedBy": null,
  "name": "first-serverless-app",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null
}
```

### 2) create a storage account 
```
az storage account create -n {storageaccountname} -g first-serverless-app --kind StorageV2 -l westcentralus --https-only true --sku Standard_LRS
```
Storage account name must be between 3 and 24 characters in length and use numbers and lower-case letters only.

### 3) go to [azure portal](https://portal.azure.com) -> storage account -> Static website (preview) -> Enabled 
- document name : index.html 
