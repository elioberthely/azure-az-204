
<details> 
    <summary>In this exercise, you create an Azure Key Vault, store secrets using the Azure CLI, and build a .NET console application that can create and retrieve secrets from the key vault. You learn how to configure authentication, manage secrets programmatically, and clean up resources when finished.</summary>

Tasks performed in this exercise:

1) Create Azure Key Vault resources
2) Store a secret in a key vault using Azure CLI
3) Create a .NET console app to create and retrieve secrets
4) Clean up resources
</details>


### Create Azure Key Vault resources and add a secret

1) Entramos al portal

    https://portal.azure.com/

2) En la parte superior seleccionamos el primer icono   [>_]
        De las opciones que nos aparecen seleccionamos
   ###  Bash

<details>
<summary>Detalles</summary>

3) Nos aparece una ventana que dice **Getting Started**  
   - Seleccionamos **No storage account required**  
   - Nos aparece abajo **Subscription**  
     - Seleccionamos **Pay As You Go**  
   - Procedemos a dar click en **Apply**

4) En la parte inferior aparece la línea de comando corriéndose en **NEGRO Y AMARILLO**  
   - De lo contrario, está mal.  
   - Nos aparece en esa consola una serie de botones:  
     `Switch to Powershell   Restart   Manage Files   New Session   Editor   Web preview   Setting Help`  
   - Damos click en **Setting**  
   - Seleccionamos **Go to Classic Version**

6) Esperemos a que se cargue todo  
   - Nos aparece de nuevo el prompt: `elio [~]$`
</details>  


7)  Creamos un resource group

    ### az group create  --name myResourceGroupKeyVault --location eastus2 

8) Nos debe devolver esta instruccion

    {
    "id": "/subscriptions/5030b2c6-f741-4e78/resourceGroups/myResourceGroupKeyVault",
    "location": "eastus2",
    "managedBy": null,
    "name": "myResourceGroupKeyVault",
    "properties": {
        "provisioningState": "Succeeded"
    },
    "tags": null,
    "type": "Microsoft.Resources/resourceGroups"
    }

9) Creamos estas 3 variables - sustituimos los nombres por los que creamos

    resourceGroup=myResourceGroupKeyVault
    location=eastus2

    ### el nombre tiene que ser solo minusculas 
    keyVaultName=mykeynaultname$RANDOM

10) Corroboramos el valor de la variable keyVaultName

    echo $keyVaultName

11) Creamos un Azure Key Vault Resource

    az keyvault create --name $keyVaultName --resource-group $resourceGroup --location $location

    ###    az keyvault create --name $keyVaultName --resource-group $resourceGroup --location $location

12) Si Marca error al crear esto verificamos 


    ### QUE EL KEYVAULT ESTE DISPONIBLE
    ###   az keyvault check-name --name $keyVaultName
    
    {
        "message": null,
        "nameAvailable": true,
        "reason": null
    }

    ### Si sigue fallando corremos este comando 
    ---   az keyvault create --name $keyVaultName --resource-group $resourceGroup --location $location --enable-rbac-authorization true --debug

    ### Si marca este error 

    (MissingSubscriptionRegistration)
    The subscription is not registered to use namespace 'Microsoft.KeyVault'

    👉 Tu suscripción NO tiene registrado el provider de Key Vault.
        Esto pasa muchísimo en:

        suscripciones nuevas

        Pay-As-You-Go

        Cloud Shell recién usado

        Azure no crea Key Vaults hasta que el provider esté registrado.

    Corremos este comando y esperamos hasta que ya haya sido registrado

    az provider show \
        --namespace Microsoft.KeyVault \
        --query registrationState

        Si devuelve Registering esperamos hasta que haya completado

    En el momento que diga Registered corremos el comando de nuevo

13) Corremos este comando y si nos regresa valores se ejecuto bien 

    az keyvault show \
  --name $keyVaultName \
  --resource-group $resourceGroup

14) Esto nos deberia devolver algo asi 

    {
        "id": "/subscriptions/5030b2c6-f741-e395c2d2d0d0/resourceGroups/myResourceGroupKeyVault/providers/Microsoft.KeyVault/vaults/mykeynaultname15166",
        "location": "eastus2",
        "name": "mykeynaultname15166",
        "properties": {
            "accessPolicies": [],
            "createMode": null,
            "enablePurgeProtection": null,
            "enableRbacAuthorization": true,
            "enableSoftDelete": true,
            "enabledForDeployment": false,
            "enabledForDiskEncryption": false,
            "enabledForTemplateDeployment": false,
            "hsmPoolResourceId": null,
            "networkAcls": null,
            "privateEndpointConnections": null,
            "provisioningState": "Succeeded",
            "publicNetworkAccess": "Enabled",
            "sku": {
            "family": "A",
            "name": "standard"
            },
            "softDeleteRetentionInDays": 90,
            "tenantId": "d39d348d-12b6-5713c7bf86e9",
            "vaultUri": "https://mykeynaultname15166.vault.azure.net/"
        },
        "resourceGroup": "myResourceGroupKeyVault",
        "systemData": {
            "createdAt": "2026-02-07T16:51:24.241000+00:00",
            "createdBy": "x@hotmail.com",
            "createdByType": "User",
            "lastModifiedAt": "2026-02-07T16:51:24.241000+00:00",
            "lastModifiedBy": "x@hotmail.com",
            "lastModifiedByType": "User"
        },
        "tags": {},
        "type": "Microsoft.KeyVault/vaults"
        }


15) Asigna un rol a tu nombre de usuario de Microsoft Entra
Para permitir que tu aplicación cree recursos y elementos, asigna tu usuario de Microsoft Entra al rol Storage Blob Data Owner. Realiza los siguientes pasos en Cloud Shell.

    userPrincipal=$(az rest --method GET --url https://graph.microsoft.com/v1.0/me \
    --headers 'Content-Type=application/json' \
    --query userPrincipalName --output tsv)


16) Una vez que se haya corrido el comando 
   ### echo $userPrincipal
        debe devolver e_hotmail.com#EXT#@ehotmail.onmicrosoft.com

17) Corremos el siguiente comando
    Ese comando solo obtiene el ID del Storage Account

    resourceID=$(az storage account show --name $accountName \
    --resource-group $resourceGroup \
    --query id --output tsv)


18) Corremos el comando 
   ### echo $resourceID

    Esto nos debe devolver algo asi

    /subscriptions/5030b2c6-f741-4e78-e395c2d2d0d0/resourceGroups/myResourceGroup/providers/Microsoft.Storage/storageAccounts/storageacct8173


19) ### Ejecuta el siguiente comando para crear y asignar el rol Asignar Key Values.
       Este rol te otorga permisos para administrar contenedores y elementos.

        az role assignment create --assignee $userPrincipal \
                --role "Key Vault Secrets Officer" \
                --scope $resourceID

20) Nos devuelve este resultado

        {
        "condition": null,
        "conditionVersion": null,
        "createdBy": null,
        "createdOn": "2026-02-07T17:35:50.866733+00:00",
        "delegatedManagedIdentityResourceId": null,
        "description": null,
        "id": "/subscriptions/5030b2c6-f741-4e78-80c0/resourceGroups/myResourceGroupKeyVault/providers/Microsoft.KeyVault/vaults/mykeynaultname15166/providers/Microsoft.Authorization/roleAssignments/080bf381-7d502e957e42",
        "name": "080bf381-11d5-49d6-9d31-7d502e957e42",
        "principalId": "399b9ae7-afba-ae882336b080",
        "principalType": "User",
        "resourceGroup": "myResourceGroupKeyVault",
        "roleDefinitionId": "/subscriptions/5030b2c6-f741-4e78-80c0/providers/Microsoft.Authorization/roleDefinitions/b86a8fe4-4948-aee5-eccb2c155cd7",
        "scope": "/subscriptions/5030b2c6-f741-e395c2d2d0d0/resourceGroups/myResourceGroupKeyVault/providers/Microsoft.KeyVault/vaults/mykeynaultname15166",
        "type": "Microsoft.Authorization/roleAssignments",
        "updatedBy": "399b9ae7-75d1-4602-afba",
        "updatedOn": "2026-02-07T17:35:51.151748+00:00"
        }


21) Ejecute el siguiente comando para crear y asignar el rol de Oficial de Secretos de Key Vault.

        az keyvault secret set --vault-name $keyVaultName \
            --name "MySecret" --value "My secret value"


22) Esto nos debe devolver esto

       {
            "attributes": {
                "created": "2026-02-07T17:40:41+00:00",
                "enabled": true,
                "expires": null,
                "notBefore": null,
                "recoverableDays": 90,
                "recoveryLevel": "Recoverable+Purgeable",
                "updated": "2026-02-07T17:40:41+00:00"
            },
            "contentType": null,
            "id": "https://mykeynaultname15166.vault.azure.net/secrets/MySecret/a245271317dc4d6ea4b5120340bbcf56",
            "kid": null,
            "managed": null,
            "name": "MySecret",
            "tags": {
                "file-encoding": "utf-8"
            },
            "value": "My secret value"
            }


23) Agregar y recuperar un secreto con Azure CLI
    Ejecute el siguiente comando para crear un secreto.

        az keyvault secret set --vault-name $keyVaultName \
            --name "MySecret" --value "My secret value"


24) Esto nos debe devolver esto

        {
            "attributes": {
                "created": "2026-02-07T17:47:57+00:00",
                "enabled": true,
                "expires": null,
                "notBefore": null,
                "recoverableDays": 90,
                "recoveryLevel": "Recoverable+Purgeable",
                "updated": "2026-02-07T17:47:57+00:00"
            },
                "contentType": null,
                "id": "https://mykeynaultname15166.vault.azure.net/secrets/MySecret/534a7ce066c4ba5dc4cba",
                "kid": null,
                "managed": null,
                "name": "MySecret",
                "tags": {
                    "file-encoding": "utf-8"
            },
               "value": "My secret value"
            }

25) ya que lo corrimos si queremos saber ponemos este comando

    ### az keyvault secret show --name "MySecret" --vault-name $keyVaultName



