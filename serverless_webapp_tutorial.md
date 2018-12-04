
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

### 4) Code the Function app 
- add HTTP Triger Function app with C# or Javascript (Authorization level	: Annonymous) 
- C# code : https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/GetUploadUrl/run.csx 
- Javascript code : https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/javascript/GetUploadUrl/index.js 

### 5) Add an environment variable for the storage connection string
Bash variable 'STORAGE_CONNECTION_STRING' 
```
export STORAGE_CONNECTION_STRING=$(az storage account show-connection-string -n <storage account name> -g first-serverless-app --query "connectionString" --output tsv)
```
Application setting name 'AZURE_STORAGE_CONNECTION_STRING'
```
az functionapp config appsettings set -n <function app name> -g first-serverless-app --settings AZURE_STORAGE_CONNECTION_STRING=$STORAGE_CONNECTION_STRING -o table
```
### 6) Configure CORS(cross-origin resource sharing)

### 7) Configure CORS in the Storage account
```
az storage cors add --methods OPTIONS PUT --origins '*' --exposed-headers '*' --allowed-headers '*' --services b --account-name <storage account name>
```

### 8) Modify the web app to upload images
```
cd ~/functions-first-serverless-web-application/www/dist

export FUNCTION_APP_URL="https://"$(az functionapp show -n <function app name> -g first-serverless-app --query "defaultHostName" --output tsv)

echo $FUNCTION_APP_URL // for checking 

echo "window.apiBaseUrl = '$FUNCTION_APP_URL'" > settings.js

cat settings.js // to confirm 

export BLOB_BASE_URL=$(az storage account show -n <storage account name> -g first-serverless-app --query primaryEndpoints.blob -o tsv | sed 's/\/$//')

echo $BLOB_BASE_URL

echo "window.blobBaseUrl = '$BLOB_BASE_URL'" >> settings.js

cat settings.js // It should be 2, apiBaseUrl and blobBaseUrl

az storage blob upload -c \$web --account-name <storage account name> -f settings.js -n settings.js // Upload the file to Blob storage.
```

### 9) Test
upload test image on the static web site 
```
az storage blob list --account-name <storage account name> -c images -o table

az storage blob delete-batch -s images --account-name <storage account name> // to delete 
```

## Resize images with Azure Functions
### 1) Create a blob storage container
container named `thumnails`
```
az storage container create -n thumbnails --account-name <storage account name> --public-access blob
```

### 2) Creating new Function with Blob Trigger template named 'ResizeImage'
- path : images/{name} 
- Integration 
- function app 
  - project.json : https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/ResizeImage/project.json 
  - run.csx : https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/ResizeImage/run.csx
  
### 3) Test
- check uploaded image 
```
az storage blob list --account-name <storage account name> -c images -o table
```
- check resized thumnail
```
az storage blob list --account-name <storage account name> -c thumbnails -o table
```
- get thumbnail URL 
```
az storage blob url --account-name <storage account name> -c thumbnails -n <filename> --output tsv
```
- delete all images 
```
az storage blob delete-batch -s images --account-name <storage account name>
az storage blob delete-batch -s thumbnails --account-name <storage account name>
```

## Store image metadata with Azure Cosmos DB
### 1) create Cosmos DB 
```
az cosmosdb create -g first-serverless-app -n <cosmos db account name>
```
This command was not working in my Azure Portal. created cosmos db via Portal UI.  

create db named `imagesdb` in cosmos db account
```
az cosmosdb database create -g first-serverless-app -n <cosmos db account name> --db-name imagesdb
```

create a new collection named images in the database with a throughput of 400 request units (RUs).
```
az cosmosdb collection create -g first-serverless-app -n <cosmos db account name> --db-name imagesdb --collection-name images --throughput 400
```

### 2) Save a document to Cosmos DB when a thumbnail is created
- change integration configuration and code for the function "ResizeImage` 

### 3) Create a function to list images from Cosmos DB
- template : HttpTrigger 
- Authorization level	 : Anonymous
- (Integration) Input : Cosmos DB 
- c# : https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/GetImages/run.csx 
- javascript : https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/javascript/GetImages/index.js 
