# LogicApp
az ad sp create-for-rbac --name "arm-template-deployment" --role contributor --scopes /subscriptions/{subscription-id} --sdk-auth
{
  "clientId": "*************31a48dd102e8",
  "clientSecret": "************LrrKvTVT35bp4",
  "subscriptionId": "*******************26-f893a7e5e5ac",
  "tenantId": "********************0812966a1b7",
  "activeDirectoryEndpointUrl": "https://login.microsoftonline.com",
  "resourceManagerEndpointUrl": "https://management.azure.com/",
  "activeDirectoryGraphResourceId": "https://graph.windows.net/",
  "sqlManagementEndpointUrl": "https://management.core.windows.net:8443/",
  "galleryEndpointUrl": "https://gallery.azure.com/",
  "managementEndpointUrl": "https://management.core.windows.net/"
}

Add above output to GH Repo as Repositiory secrets

![image](https://user-images.githubusercontent.com/66653353/211693725-406cbf0a-a937-4075-affb-a881cd00be17.png)

Create Actions Yaml for deployment
#Name of the GitHub Action
name: Deploy Logic App

#Set the action on which the workflow will trigger
on:
 push:
   branches:
     - master

jobs:
  validate-and-deploy:
   runs-on: ubuntu-latest
   env:
     ENV_RESOURCEGROUP: logicappcicd
     ENV_RESOURCEGROUPLOCATION: East US

   steps:

    #Checkout the repository
    - name: Checkout Repository
      uses: actions/checkout@master

    #Use the azure provided action to log on to azure using service pricipal
    - name: Login to Azure
      uses: azure/login@v1
      with:
          creds: ${{secrets.AZURE_CREDENTIALS}}
    
    # Create the resource group
    - name: Create Resource Group
      run: az group create -n ${{env.ENV_RESOURCEGROUP}} -l ${{env.ENV_RESOURCEGROUPLOCATION}}

    #Validate the  ARM template
    - name: Validate ARM template
      run: |
        az deployment group validate -g ${{env.ENV_RESOURCEGROUP}} --mode Incremental --template-file ./src/Deployment/azure-logicapp-demo.deploy.json --parameters ./src/Deployment/azure-logicapp-demo.deploy.parameters.json
    
    #Deploy the ARM Template
    - name: Deploy Logic App
      run: |
       echo "::set-env name=logicappurl::$(az deployment group create -g az-logicapp-githubactions-demo-rg --template-file ./src/Deployment/azure-logicapp-demo.deploy.json --parameters ./src/Deployment/azure-logicapp-demo.deploy.parameters.json --query 'properties.outputs.logicAppUrl.value' -o tsv)"
      shell: bash   
      
    # Log Out From Azure 
    - name: Logout
      run: az logout
