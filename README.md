# Azure-Event-Hub

1) Azure Event Hub is PAAS(managed service), whereas when you run Kafka on Azure it's an IAAS solution(manage your own Linux VM-s, more control and more integration) .
 
 
 Setup 
 
 1) $ az login
 
 {
    "cloudName": "AzureCloud",
    "id": "00dc7dcb-c8b1-4a68-b1c7-91e72c68e329",
    "isDefault": true,
    "name": "Free Trial",
    "state": "Enabled",
    "tenantId": "ea755644-93f6-43cb-8a39-b662312bb2a5",
    "user": {
      "name": "jumbadshital6@gmail.com",
      "type": "user"
    }
 }
 

2) $ export AZURE_RESOURCE_GROUP=azureResourceGroup

3) $ az group create --name $AZURE_RESOURCE_GROUP --location "West US 2"

{
  "id": "/subscriptions/00dc7dcb-c8b1-4a68-b1c7-91e72c68e329/resourceGroups/azureResourceGroup",
  "location": "westus2",
  "managedBy": null,
  "name": "azureResourceGroup",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null,
  "type": "Microsoft.Resources/resourceGroups"
}

4) export EVENT_HUBS_NAMESPACE=newEventHubsNamespace1

5) az eventhubs namespace create --name $EVENT_HUBS_NAMESPACE --resource-group $AZURE_RESOURCE_GROUP --location "West US 2" --enable-kafka true --enable-auto-inflate false

{
  "createdAt": "2019-12-02T06:04:54.953000+00:00",
  "id": "/subscriptions/00dc7dcb-c8b1-4a68-b1c7-91e72c68e329/resourceGroups/azureResourceGroup/providers/Microsoft.EventHub/namespaces/newEventHubsNamespace1",
  "isAutoInflateEnabled": false,
  "kafkaEnabled": true,
  "location": "West US 2",
  "maximumThroughputUnits": 0,
  "metricId": "00dc7dcb-c8b1-4a68-b1c7-91e72c68e329:neweventhubsnamespace1",
  "name": "newEventHubsNamespace1",
  "provisioningState": "Succeeded",
  "resourceGroup": "azureResourceGroup",
  "serviceBusEndpoint": "https://newEventHubsNamespace1.servicebus.windows.net:443/",
  "sku": {
    "capacity": 1,
    "name": "Standard",
    "tier": "Standard"
  },
  "tags": {},
  "type": "Microsoft.EventHub/Namespaces",
  "updatedAt": "2019-12-02T06:06:16.647000+00:00"
}

6) export EVENT_HUB_NAME=newEventHubName1

7) az eventhubs eventhub create --name $EVENT_HUB_NAME --resource-group $AZURE_RESOURCE_GROUP --namespace-name $EVENT_HUBS_NAMESPACE --partition-count 10

{
  "captureDescription": null,
  "createdAt": "2019-12-02T06:10:10.267000+00:00",
  "id": "/subscriptions/00dc7dcb-c8b1-4a68-b1c7-91e72c68e329/resourceGroups/azureResourceGroup/providers/Microsoft.EventHub/namespaces/newEventHubsNamespace1/eventhubs/newEventHubName1",
  "location": "West US 2",
  "messageRetentionInDays": 7,
  "name": "newEventHubName1",
  "partitionCount": 10,
  "partitionIds": [
    "0",
    "1",
    "2",
    "3",
    "4",
    "5",
    "6",
    "7",
    "8",
    "9"
  ],
  "resourceGroup": "azureResourceGroup",
  "status": "Active",
  "type": "Microsoft.EventHub/Namespaces/EventHubs",
  "updatedAt": "2019-12-02T06:10:10.490000+00:00"
}

8) az eventhubs namespace authorization-rule list --resource-group $AZURE_RESOURCE_GROUP --namespace-name $EVENT_HUBS_NAMESPACE

[
  {
    "id": "/subscriptions/00dc7dcb-c8b1-4a68-b1c7-91e72c68e329/resourceGroups/azureResourceGroup/providers/Microsoft.EventHub/namespaces/newEventHubsNamespace1/AuthorizationRules/RootManageSharedAccessKey",
    "location": "West US 2",
    "name": "RootManageSharedAccessKey",
    "resourceGroup": "azureResourceGroup",
    "rights": [
      "Listen",
      "Manage",
      "Send"
    ],
    "type": "Microsoft.EventHub/Namespaces/AuthorizationRules"
  }
]

9) $ export EVENT_HUB_AUTH_RULE_NAME=RootManageSharedAccessKey

10) az eventhubs namespace authorization-rule keys list --resource-group $AZURE_RESOURCE_GROUP --namespace-name $EVENT_HUBS_NAMESPACE --name $EVENT_HUB_AUTH_RULE_NAME

{
  "aliasPrimaryConnectionString": null,
  "aliasSecondaryConnectionString": null,
  "keyName": "RootManageSharedAccessKey",
  "primaryConnectionString": "Endpoint=sb://neweventhubsnamespace1.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=e5IrqLLDozsfJRTflKh1XNoEIT+DgFRVvjaDkMdyXXg=",
  "primaryKey": "e5IrqLLDozsfJRTflKh1XNoEIT+DgFRVvjaDkMdyXXg=",
  "secondaryConnectionString": "Endpoint=sb://neweventhubsnamespace1.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=9unPMPj9meZTgYRh62ry3cZyVu9v2AkLLiygbDNgI7E=",
  "secondaryKey": "9unPMPj9meZTgYRh62ry3cZyVu9v2AkLLiygbDNgI7E="
}

11) export EVENTHUBS_CONNECTION_STRING="
Endpoint=sb://neweventhubsnamespace1.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=e5IrqLLDozsfJRTflKh1XNoEIT+DgFRVvjaDkMdyXXg="

12) export EVENT_HUBS_NAMESPACE=newEventHubsNamespace1

13) export EVENTHUBS_BROKER=$EVENT_HUBS_NAMESPACE.servicebus.windows.net:9093

14) export EVENTHUBS_TOPIC=newEventHubName1

 





 
 
 
 
 References :
 https://blogs.msdn.microsoft.com/opensourcemsft/2015/08/08/choosing-between-azure-event-hub-and-kafka-what-you-need-to-know/
 
 https://dev.to/azure/how-to-quickly-test-connectivity-to-your-azure-event-hubs-for-kafka-cluster-without-writing-any-code-4la1
