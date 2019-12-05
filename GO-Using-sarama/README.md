export AZURE_RESOURCE_GROUP=newtestrg1

export EVENT_HUBS_NAMESPACE=newtestnamespace2

export EVENT_HUB_NAME=newtesthub

export EVENTHUBS_CONNECTION_STRING="Endpoint=sb://newtestnamespace2.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=Gnu6YryKOUAboseOBDG+pVP47jnk8zBIUUCwhTQYv9c="

export EVENTHUBS_BROKER=$EVENT_HUBS_NAMESPACE.servicebus.windows.net:9093

export EVENTHUBS_TOPIC=$EVENT_HUB_NAME

export EVENTHUBS_CONSUMER_GROUPID=newtestcg4


go run producer.go

go run consumer.go
