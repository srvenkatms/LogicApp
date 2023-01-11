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


