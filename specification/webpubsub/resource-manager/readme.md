# WebPubSub

> see https://aka.ms/autorest

This is the AutoRest configuration file for WebPubSub.



---
## Getting Started
To build the SDK for WebPubSub, simply [Install AutoRest](https://aka.ms/autorest/install) and in this folder, run:

> `autorest`

To see additional help and options, run:

> `autorest --help`
---

## Configuration



### Basic Information
These are the global settings for the WebPubSub API.

``` yaml
openapi-type: arm
tag: package-2025-01-01-preview
```

### Suppression

``` yaml
directive:
  - suppress: EnumInsteadOfBoolean
    where:
    - $.definitions.NameAvailability.properties.nameAvailable
    - $.definitions.Dimension.properties.toBeExportedForShoebox
    - $.definitions.Operation.properties.isDataAction
    - $.definitions.WebPubSubTlsSettings.properties.clientCertEnabled
    - $.definitions.WebPubSubProperties.properties.disableLocalAuth
    - $.definitions.WebPubSubProperties.properties.disableAadAuth
    reason:  The boolean properties 'nameAvailable' and 'isDataAction' is standard property defined by Azure API spec. 'toBeExportedForShoebox' by Geneva metrics team. Keep 'clientCertEnabled' bool to be consistent with SignalR and not break existing customers. 'disableLocalAuth' and 'disableAadAuth' by Identity team.
  - suppress: TrackedResourceListByImmediateParent
    reason: Another list APIs naming approach is used over the specs
  - suppress: AvoidNestedProperties
    where:
    - $.definitions.ShareablePrivateLinkResourceType.properties.properties
    - $.definitions.WebPubSubFeature.properties.properties
    reason:  The 'properties' is a user-defined dictionary, cannot be flattened.
  - suppress: PutInOperationName
    where:
    - $.paths["/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.SignalRService/webPubSub/{resourceName}/privateEndpointConnections/{privateEndpointConnectionName}"].put.operationId
    reason:  It's indeed an UPDATE operation, but PUT is required per NRP requirement.
  - suppress: InvalidSkuModel
    where:
    - $.definitions.Sku
    reason: It's required by spec of enumerating SKUs for an existing resource
  - suppress: ParametersOrder
    where:
    - $.paths["/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.SignalRService/webPubSub/{resourceName}/hubs/{hubName}"].get
    - $.paths["/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.SignalRService/webPubSub/{resourceName}/hubs/{hubName}"].put
    - $.paths["/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.SignalRService/webPubSub/{resourceName}/hubs/{hubName}"].delete
    - $.paths["/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.SignalRService/webPubSub/{resourceName}/privateEndpointConnections/{privateEndpointConnectionName}"].get
    - $.paths["/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.SignalRService/webPubSub/{resourceName}/privateEndpointConnections/{privateEndpointConnectionName}"].put
    - $.paths["/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.SignalRService/webPubSub/{resourceName}/privateEndpointConnections/{privateEndpointConnectionName}"].delete
    - $.paths["/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.SignalRService/webPubSub/{resourceName}/sharedPrivateLinkResources/{sharedPrivateLinkResourceName}"].get
    - $.paths["/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.SignalRService/webPubSub/{resourceName}/sharedPrivateLinkResources/{sharedPrivateLinkResourceName}"].put
    - $.paths["/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.SignalRService/webPubSub/{resourceName}/sharedPrivateLinkResources/{sharedPrivateLinkResourceName}"].delete
    reason: It can introduce a breaking change when updating parameter order, since Web PubSub service has already shipped public versions.
```

### Tag: package-2021-10-01

These settings apply only when `--tag=package-2021-10-01` is specified on the command line.

``` yaml $(tag) == 'package-2021-10-01'
input-file:
- Microsoft.SignalRService/stable/2021-10-01/webpubsub.json
```

### Tag: package-2021-09-01-preview

These settings apply only when `--tag=package-2021-09-01-preview` is specified on the command line.

``` yaml $(tag) == 'package-2021-09-01-preview'
input-file:
- Microsoft.SignalRService/preview/2021-09-01-preview/webpubsub.json
```

### Tag: package-2021-06-01-preview

These settings apply only when `--tag=package-2021-06-01-preview` is specified on the command line.

``` yaml $(tag) == 'package-2021-06-01-preview'
input-file:
- Microsoft.SignalRService/preview/2021-06-01-preview/webpubsub.json
```

### Tag: package-2021-04-01-preview

These settings apply only when `--tag=package-2021-04-01-preview` is specified on the command line.

``` yaml $(tag) == 'package-2021-04-01-preview'
input-file:
- Microsoft.SignalRService/preview/2021-04-01-preview/webpubsub.json
```

### Tag: package-2022-08-01-preview

These settings apply only when `--tag=package-2022-08-01-preview` is specified on the command line.

```yaml $(tag) == 'package-2022-08-01-preview'
input-file:
- Microsoft.SignalRService/preview/2022-08-01-preview/webpubsub.json
```

### Tag: package-2023-02-01

These settings apply only when `--tag=package-2023-02-01` is specified on the command line.

```yaml $(tag) == 'package-2023-02-01'
input-file:
- Microsoft.SignalRService/stable/2023-02-01/webpubsub.json
```

### Tag: package-2023-03-01-preview

These settings apply only when `--tag=package-2023-03-01-preview` is specified on the command line.

```yaml $(tag) == 'package-2023-03-01-preview'
input-file:
- Microsoft.SignalRService/preview/2023-03-01-preview/webpubsub.json
```

### Tag: package-2023-06-01-preview

These settings apply only when `--tag=package-2023-06-01-preview` is specified on the command line.

```yaml $(tag) == 'package-2023-06-01-preview'
input-file:
- Microsoft.SignalRService/preview/2023-06-01-preview/webpubsub.json
```

### Tag: package-2023-08-01-preview

These settings apply only when `--tag=package-2023-08-01-preview` is specified on the command line.

```yaml $(tag) == 'package-2023-08-01-preview'
input-file:
- Microsoft.SignalRService/preview/2023-08-01-preview/webpubsub.json
```

### Tag: package-2024-01-01-preview

These settings apply only when `--tag=package-2024-01-01-preview` is specified on the command line.

```yaml $(tag) == 'package-2024-01-01-preview'
input-file:
- Microsoft.SignalRService/preview/2024-01-01-preview/webpubsub.json
```

### Tag: package-2024-03-01

These settings apply only when `--tag=package-2024-03-01` is specified on the command line.

```yaml $(tag) == 'package-2024-03-01'
input-file:
- Microsoft.SignalRService/stable/2024-03-01/webpubsub.json
```

### Tag: package-2024-04-01-preview

These settings apply only when `--tag=package-2024-04-01-preview` is specified on the command line.

```yaml $(tag) == 'package-2024-04-01-preview'
input-file:
- Microsoft.SignalRService/preview/2024-04-01-preview/webpubsub.json
```

### Tag: package-2024-08-01-preview

These settings apply only when `--tag=package-2024-08-01-preview` is specified on the command line.

```yaml $(tag) == 'package-2024-08-01-preview'
input-file:
- Microsoft.SignalRService/preview/2024-08-01-preview/webpubsub.json
```

### Tag: package-2024-10-01-preview

These settings apply only when `--tag=package-2024-10-01-preview` is specified on the command line.

```yaml $(tag) == 'package-2024-10-01-preview'
input-file:
- Microsoft.SignalRService/preview/2024-10-01-preview/webpubsub.json
```

### Tag: package-2025-01-01-preview

These settings apply only when `--tag=package-2025-01-01-preview` is specified on the command line.

```yaml $(tag) == 'package-2025-01-01-preview'
input-file:
- Microsoft.SignalRService/preview/2025-01-01-preview/webpubsub.json
```

---

# Code Generation


## Swagger to SDK

This section describes what SDK should be generated by the automatic system.
This is not used by Autorest itself.

``` yaml $(swagger-to-sdk)
swagger-to-sdk:
  - repo: azure-sdk-for-net
  - repo: azure-sdk-for-python
  - repo: azure-sdk-for-java
  - repo: azure-sdk-for-node
  - repo: azure-sdk-for-js
  - repo: azure-sdk-for-go
  - repo: azure-sdk-for-ruby
    after_scripts:
      - bundle install && rake arm:regen_all_profiles['azure_mgmt_webpubsub']
  - repo: azure-resource-manager-schemas
  - repo: azure-powershell
```

## Python

See configuration in [readme.python.md](./readme.python.md)

## Go

See configuration in [readme.go.md](./readme.go.md)

## Java

See configuration in [readme.java.md](./readme.java.md)



