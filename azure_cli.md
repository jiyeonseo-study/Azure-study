# Azure CLI on MacOS 

## Install 
```
$ brew update && brew install azure-cli
```

## Login first!
```
$ az login 
```

## Bot Service create, update 
```
$ az bot publish --name "my-bot-name" --resource-group "my-resource-group" // 처음 배포 경우 
$ az bot update --name "my-bot-name" --resource-group "my-resource-group" // 이미 만들어진 앱인 경우 
```

## More resources 
- https://docs.microsoft.com/en-us/cli/azure/install-azure-cli-macos?view=azure-cli-latest
