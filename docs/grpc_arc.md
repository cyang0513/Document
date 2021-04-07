# Host ASP.NET Core Grpc service via Azure Container Registry

Recently I've been looking into running ASP.NET Core Grpc service on Azure. Sadly Azure App Service does not support hosting Grpc service [at this moment](https://feedback.azure.com/forums/169385-web-apps/suggestions/40585333-grpc-support-in-azure-app-service) and Azure VM is not what I'm looking for neither. So the option I have is to containerized the Grpc service and host it via Azure Container Registry and run it with Container Instances.

## Manage SSL Certificate
We want to manage the server side SSL certificate for Grpc service in a central place, so Azure Key Vault is an idea choice. Make sure you have the **pfx** format certificate and upload it to your key vault certificates. Note that once it's uploaded, it will generate a Secret Identifier and a Key Identifier for the same certificate version.

![](https://github.com/adam-p/markdown-here/raw/master/src/common/images/icon48.png)



## Configure Kestrel
It's more convenient to configure Kestrel in code than in appsettings.json. Especially if you want to take advantage of Azure App Configuration and Azure Key Vault.