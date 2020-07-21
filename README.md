# Using Github Action to Deploy to Azure App Service
We can take a look at here how to package [asp.net core app](https://dotnet.microsoft.com/learn/aspnet/what-is-aspnet-core) run it locally and move it all the way up to [azure app service](https://azure.microsoft.com/en-us/services/app-service/#product-overview) with [GitHub actions](https://docs.github.com/en/actions). 

#### Creating a dotnet application.
The first step is to create web application which we want to deploy to azure app service.
What I done here is to create a aspnet web application using a default template
for that I need to have a [.NET SDK](https://dotnet.microsoft.com/) install locally where you are building aspnet application.
```
donet new webapp
``` 

Run this command to find dotnet version
```
dotnet --version
```

#### Creating a secret which GitHub needed for deploying to Azure APP Service
This step is mandatory because I need to grant a access to GitHub to be able to find that I m a authenticated user for azure and so GitHub need some kind of RBAC (Role Base Access Control) to be able to perform that action successfully inside an azure. Go to `Setting` page inside a GitHub find a `secrets` which happens to have existed in the left side navigation area and create a new `secret` which hold the RBAC information.

Next step How do I get a secret which I needed to have inside a GitHub, SignIn to [AZURE PORTAL](https://portal.azure.com/). Run a [cloud shell](https://docs.microsoft.com/en-us/azure/cloud-shell/overview) inside an azure.

Run this command to know how many [resource groups](https://docs.microsoft.com/en-us/azure/azure-resource-manager/management/manage-resource-groups-portal) do you have inside an azure.
```
az group list
```

This step is required to delicate a control to a specific resource group or if you doesn't have one create a new [resource group](https://docs.microsoft.com/en-us/azure/azure-resource-manager/management/manage-resource-groups-portal) you can also create a resource group from [azure cli](https://docs.microsoft.com/en-us/azure/azure-resource-manager/management/manage-resources-cli) if you have it [install](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest) locally. Copy the id from this command `az group list` and delicate a control to a specific resource group you want to.

#### Creating a RBAC for GitHub Actions

In this command create a [service principal azure](https://docs.microsoft.com/en-us/powershell/azure/create-azure-service-principal-azureps?view=azps-4.4.0) give it a name `svcgithubaction` and created a [RBAC](https://docs.microsoft.com/en-us/azure/role-basetiondemd-access-control/overview) give it role for `contributor` then finally in the scopes area past a resource group id which you want to delicate a control.

```
az ad sp create-for-rbac -n svcgithubaction --role contributor --scopes "resource group Id" --sdk-auth 
```
Copy the output of this command and paste them inside a secret you have created earlier inside a GitHub.


