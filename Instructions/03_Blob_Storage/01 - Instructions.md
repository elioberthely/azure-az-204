1) Entramos al portal

    https://portal.azure.com/

2) En la parte superior seleccionamos el primer icono   [>_]
        De las opciones que nos aparecen seleccionamos
   ###  Bash

3) Nos aparece una ventana que dice
        Getting Started
   ###  Seleccionamos No storage account required

        Nos aparece abajo Suscription
   ###  Seleccionamos Pay As You Go

        Procedemos a dar click en Apply


4)  En la parte inferior aparece la linea de comando corriendose en 
    ### NEGRO Y AMARILLO de lo contrario esta mal

5)  Nos aparece en esa consola una serie de botones

    Switch to Powershell   Restart  Manage Files  New Session     Editor      Web preview     Setting Help

    ### Damos click en Setting
    ### Seleccionamos Go to Clasic Version

6)  Esperemos a que se cargue todo
    Nos aparece de nuevo elio [~]$


7)  Creamos un resource group

    ### az group create --location eastus2  --name myResourceGroupABN2026

8)  Nos debe devolver esta instruccion

                {
            "id": "/subscriptions/5030b2c6-f741-4e78-80c0-e395c2d2d0d0/resourceGroups/myResourceGroupABN2026",
            "location": "eastus2",
            "managedBy": null,
            "name": "myResourceGroupABN2026",
            "properties": {
                "provisioningState": "Succeeded"
            },
            "tags": null,
            "type": "Microsoft.Resources/resourceGroups"
            }


9) Creamos estas 3 variables - sustituimos los nombres por los que creamos

    resourceGroup=myResourceGroupABN2026
    location=eastus
    accountName=storageacct$RANDOM

7) Corremos este comando

az storage account create --name $accountName --resource-group $resourceGroup --location $location  --sku Standard_LRS 

###   Las diagonales solo son para que sean mas legibles

az storage account create --name $accountName \
    --resource-group $resourceGroup \
    --location $location \
    --sku Standard_LRS 


8) Damos enter y esto nos regresa una respuesta

{
  "accessTier": "Hot",
  "accountMigrationInProgress": null,
  "allowBlobPublicAccess": false,
  "allowCrossTenantReplication": false,
  "allowSharedKeyAccess": null,
  "allowedCopyScope": null,
  "azureFilesIdentityBasedAuthentication": null,
  "blobRestoreStatus": null,
  "creationTime": "2026-01-30T02:16:38.725454+00:00",
  "customDomain": null,
  "defaultToOAuthAuthentication": null,
  "dnsEndpointType": null,
  "dualStackEndpointPreference": null,
  "enableExtendedGroups": null,
  "enableHttpsTrafficOnly": true,
  "enableNfsV3": null,
  "encryption": {
    "encryptionIdentity": null,
    "keySource": "Microsoft.Storage",
    "keyVaultProperties": null,
    "requireInfrastructureEncryption": null,
    "services": {
      "blob": {
        "enabled": true,
        "keyType": "Account",
        "lastEnabledTime": "2026-01-30T02:16:39.084820+00:00"
      },
      "file": {
        "enabled": true,
        "keyType": "Account",
        "lastEnabledTime": "2026-01-30T02:16:39.084820+00:00"
      },
      "queue": null,
      "table": null
    }
  },
  "extendedLocation": null,
  "failoverInProgress": null,
  "geoPriorityReplicationStatus": null,
  "geoReplicationStats": null,
  "id": "/subscriptions/5030b2c6-f741-4e78-80c0-e395c2d2d0d0/resourceGroups/myResourceGroupABN2026/providers/Microsoft.Storage/storageAccounts/storageacct8173",
  "identity": null,
  "immutableStorageWithVersioning": null,
  "isHnsEnabled": null,
  "isLocalUserEnabled": null,
  "isSftpEnabled": null,
  "isSkuConversionBlocked": null,
  "keyCreationTime": {
    "key1": "2026-01-30T02:16:39.084820+00:00",
    "key2": "2026-01-30T02:16:39.084820+00:00"
  },
  "keyPolicy": null,
  "kind": "StorageV2",
  "largeFileSharesState": null,
  "lastGeoFailoverTime": null,
  "location": "eastus",
  "minimumTlsVersion": "TLS1_0",
  "name": "storageacct8173",
  "networkRuleSet": {
    "bypass": "AzureServices",
    "defaultAction": "Allow",
    "ipRules": [],
    "ipv6Rules": [],
    "resourceAccessRules": null,
    "virtualNetworkRules": []
  },
  "placement": null,
  "primaryEndpoints": {
    "blob": "https://storageacct8173.blob.core.windows.net/",
    "dfs": "https://storageacct8173.dfs.core.windows.net/",
    "file": "https://storageacct8173.file.core.windows.net/",
    "internetEndpoints": null,
    "ipv6Endpoints": null,
    "microsoftEndpoints": null,
    "queue": "https://storageacct8173.queue.core.windows.net/",
    "table": "https://storageacct8173.table.core.windows.net/",
    "web": "https://storageacct8173.z13.web.core.windows.net/"
  },
  "primaryLocation": "eastus",
  "privateEndpointConnections": [],
  "provisioningState": "Succeeded",
  "publicNetworkAccess": null,
  "resourceGroup": "myResourceGroupABN2026",
  "routingPreference": null,
  "sasPolicy": null,
  "secondaryEndpoints": null,
  "secondaryLocation": null,
  "sku": {
    "name": "Standard_LRS",
    "tier": "Standard"
  },
  "statusOfPrimary": "available",
  "statusOfSecondary": null,
  "storageAccountSkuConversionStatus": null,
  "tags": {},
  "type": "Microsoft.Storage/storageAccounts",
  "zones": null
}


9) Escribimos este comando

    ### echo $accountName
    esto nos regresa el storageacct


10) Asigna un rol a tu nombre de usuario de Microsoft Entra
Para permitir que tu aplicación cree recursos y elementos, asigna tu usuario de Microsoft Entra al rol Storage Blob Data Owner. Realiza los siguientes pasos en Cloud Shell.

    userPrincipal=$(az rest --method GET --url https://graph.microsoft.com/v1.0/me \
    --headers 'Content-Type=application/json' \
    --query userPrincipalName --output tsv)

   ### SI EL COMANDO NO CORRE PRESIONA Ctrl + C , intenta de nuevo

11) Una vez que se haya corrido el comando 
   ### echo $userPrincipal
        debe devolver e_hotmail.com#EXT#@ehotmail.onmicrosoft.com

12) Corremos el siguiente comando
    Ese comando solo obtiene el ID del Storage Account

    resourceID=$(az storage account show --name $accountName \
    --resource-group $resourceGroup \
    --query id --output tsv)

13) Corremos el comando 
   ### echo $resourceID

    Esto nos debe devolver algo asi

    /subscriptions/5030b2c6-f741-4e78-80c0-e395c2d2d0d0/resourceGroups/myResourceGroupABN2026/providers/Microsoft.Storage/storageAccounts/storageacct8173

14) Ejecuta el siguiente comando para crear y asignar el rol Storage Blob Data Owner.
    Este rol te otorga permisos para administrar contenedores y elementos.

    Corremos este comando 

    az role assignment create --assignee $userPrincipal \
    --role "Storage Blob Data Owner" \
    --scope $resourceID

15) Esto nos devuelve

            {
            "condition": null,
            "conditionVersion": null,
            "createdBy": null,
            "createdOn": "2026-01-30T03:16:11.319844+00:00",
            "delegatedManagedIdentityResourceId": null,
            "description": null,
            "id": "/subscriptions/5030b2c6-f741-4e78-80c0-e395c2d2d0d0/resourceGroups/myResourceGroupABN2026/providers/Microsoft.Storage/storageAccounts/storageacct8173/providers/Microsoft.Authorization/roleAssignments/ffa3ef75-4703-40de-af3c-974ff1156601",
            "name": "ffa3ef75-4703-40de-af3c-974ff1156601",
            "principalId": "399b9ae7-75d1-4602-afba-ae882336b080",
            "principalType": "User",
            "resourceGroup": "myResourceGroupABN2026",
            "roleDefinitionId": "/subscriptions/5030b2c6-f741-4e78-80c0-e395c2d2d0d0/providers/Microsoft.Authorization/roleDefinitions/b7e6dc6d-f1e8-4753-8033-0f276bb0955b",
            "scope": "/subscriptions/5030b2c6-f741-4e78-80c0-e395c2d2d0d0/resourceGroups/myResourceGroupABN2026/providers/Microsoft.Storage/storageAccounts/storageacct8173",
            "type": "Microsoft.Authorization/roleAssignments",
            "updatedBy": "399b9ae7-75d1-4602-afba-ae882336b080",
            "updatedOn": "2026-01-30T03:16:11.697844+00:00"
            }

16)