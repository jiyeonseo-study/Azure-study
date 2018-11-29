
# serverless webapp tutorial 
link : https://docs.microsoft.com/en-us/azure/functions/tutorial-static-website-serverless-api-with-database 

## 1. Create Web app 
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
az storage account create -n <storage account name> -g first-serverless-app --kind StorageV2 -l westcentralus --https-only true --sku Standard_LRS
```
Storage account name must be between 3 and 24 characters in length and use numbers and lower-case letters only.

### 3) go to [azure portal](https://portal.azure.com) -> storage account -> Static website (preview) -> Enabled 
- document name : index.html 

### 4) git clone sample page and generate it 
```
cd ~
git clone https://github.com/Azure-Samples/functions-first-serverless-web-application
cd ~/functions-first-serverless-web-application/www
npm install
npm run generate
cd dist
az storage blob upload-batch -s . -d \$web --account-name <storage account name>
```

you created a resource group named first-serverless-app containing a Storage account. A blob container named `$web` in the Storage account stores the static content for your web application and makes the content available publicly

## 2. Blob storage
### 1) Create a blob storage container
```
az storage container create -n images --account-name <storage account name> --public-access blob
```

### 2) Create an Azure Function app
an HTTP request or when a blob is created in a storage container
```
az functionapp create -n <function app name> -g first-serverless-app -s <storage account name> -c westcentralus
{
  "created": true
}
```

### 3) Configure the function app
```
az functionapp config appsettings set --name <function app name> -g first-serverless-app --settings FUNCTIONS_EXTENSION_VERSION=~1
```
