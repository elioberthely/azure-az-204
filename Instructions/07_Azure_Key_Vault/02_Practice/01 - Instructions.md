
<details> 
    <summary>In this exercise, you create an Azure App Configuration resource, store configuration settings using Azure CLI, and build a .NET console application that uses the ConfigurationBuilder to retrieve configuration values. You learn how to organize settings with hierarchical keys and authenticate your application to access cloud-based configuration data.</summary>

Tasks performed in this exercise:

1) Create an Azure App Configuration resource
2) Store connection string configuration information
3) Create a .NET console app to retrieve the configuration information
4) Clean up resources
</details>


### Create an Azure App Configuration resource and add configuration information

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

    ### az group create  --name myResourceGroupKeyVaultSecond --location eastus

8) Nos debe devolver esta instruccion

    {
    "id": "/subscriptions/5030b2c6-f741-4e78/resourceGroups/myResourceGroupKeyVault",
    "location": "eastus",
    "managedBy": null,
    "name": "myResourceGroupKeyVaultSecond",
    "properties": {
        "provisioningState": "Succeeded"
    },
    "tags": null,
    "type": "Microsoft.Resources/resourceGroups"
    }

9) Creamos estas 3 variables - sustituimos los nombres por los que creamos

    resourceGroup=myResourceGroupKeyVaultSecond
    location=eastus

    ### el nombre tiene que ser solo minusculas 
    appConfigName=appconfigname$RANDOM

10) Corroboramos el valor de la variable keyVaultName
    echo $resourceGroup
    echo $location
    echo $appConfigName

11) Ejecuta el siguiente comando para asegurarte de que el proveedor Microsoft.AppConfiguration esté registrado para tu suscripción.

    ### az provider register --namespace Microsoft.AppConfiguration

12) Ejecutamos este comando y vemos el status, debe aparecer Registered

    az provider show --namespace Microsoft.AppConfiguration --query "registrationState"

13) Una vez que ya diga "Registered" seguimos con el siguiente paso

14) Creamos un Azure App Configuration resource.

    az keyvault create --name $keyVaultName --resource-group $resourceGroup --location $location

    ###    az appconfig create --location $location \
    --name $appConfigName \
    --resource-group $resourceGroup \
    --sku Free


15) Esto nos debe regresar una respuesta

        {
        "createMode": null,
        "creationDate": "2026-02-08T12:00:40+00:00",
        "dataPlaneProxy": {
            "authenticationMode": "Local",
            "privateLinkDelegation": "Disabled"
        },
        "defaultKeyValueRevisionRetentionPeriodInSeconds": 604800,
        "disableLocalAuth": false,
        "enablePurgeProtection": false,
        "encryption": {
            "keyVaultProperties": null
        },
        "endpoint": "https://appconfigname15335.azconfig.io",
        "id": "/subscriptions/5030b2c6-f741-4e78/resourceGroups/myresourcegroupkeyvaultsecond/providers/Microsoft.AppConfiguration/configurationStores/appconfigname15335",
        "identity": null,
        "location": "eastus",
        "name": "appconfigname15335",
        "privateEndpointConnections": null,
        "provisioningState": "Succeeded",
        "publicNetworkAccess": null,
        "resourceGroup": "myresourcegroupkeyvaultsecond",
        "sku": {
            "name": "free"
        },
        "softDeleteRetentionInDays": 0,
        "systemData": {
            "createdAt": "2026-02-08T12:00:40+00:00",
            "createdBy": "x@hotmail.com",
            "createdByType": "User",
            "lastModifiedAt": "2026-02-08T12:00:40+00:00",
            "lastModifiedBy": "x@hotmail.com",
            "lastModifiedByType": "User"
        },
        "tags": {},
        "type": "Microsoft.AppConfiguration/configurationStores"
        }

16) Asigna un rol a tu nombre de usuario de Microsoft Entra
Para permitir que tu aplicación cree recursos y elementos, asigna tu usuario de Microsoft Entra al rol Storage Blob Data Owner. Realiza los siguientes pasos en Cloud Shell.

    userPrincipal=$(az rest --method GET --url https://graph.microsoft.com/v1.0/me \
    --headers 'Content-Type=application/json' \
    --query userPrincipalName --output tsv)


17) Una vez que se haya corrido el comando 
   ### echo $userPrincipal
        debe devolver e_hotmail.com#EXT#@ehotmail.onmicrosoft.com

18) Ejecuta el siguiente comando para obtener el ID del recurso de tu servicio App Configuration.
    El ID del recurso establece el alcance para la asignación del rol.

    resourceID=$(az appconfig show \
    --resource-group $resourceGroup \
    --name $appConfigName \
    --query id --output tsv)


19) Corremos el comando 
   ### echo $resourceID

    Esto nos debe devolver algo asi

    /subscriptions/5032c6-f741-80c0-e3952d0d0/resourceGroups/myresourcegroupkeyvaultsecond/providers/Microsoft.AppConfiguration/configurationStores/appconfigname15335

20) ### Ejecuta el siguiente comando para crear y asignar el rol App Configuration Data Reader.
       Este rol te otorga permisos para administrar contenedores y elementos.

        az role assignment create --assignee $userPrincipal \
    --role "App Configuration Data Reader" \
    --scope $resourceID

21) Nos devuelve este resultado

        {
            "condition": null,
            "conditionVersion": null,
            "createdBy": null,
            "createdOn": "2026-02-08T12:11:36.495060+00:00",
            "delegatedManagedIdentityResourceId": null,
            "description": null,
            "id": "/subscriptions/503c6-f741-4e78-80c0-2d2d0d0/resourceGroups/myresourcegroupkeyvaultsecond/providers/Microsoft.AppConfiguration/configurationStores/appconfigname15335/providers/Microsoft.Authorization/roleAssignments/65-846a-4a59-b237-792653c8220b",
            "name": "6dd-846a-b237-792653c8220b",
            "principalId": "399b9ae7-75d1-ba-ae882336b080",
            "principalType": "User",
            "resourceGroup": "myresourcegroupkeyvaultsecond",
            "roleDefinitionId": "/subscriptions/5032c6-f741-4e78-8d0d0/providers/Microsoft.Authorization/roleDefinitions/516239f1-63e1-a4de-a74fa071",
            "scope": "/subscriptions/5030b2c6-f741-4e78-2d2d0d0/resourceGroups/myresourcegroupkeyvaultsecond/providers/Microsoft.AppConfiguration/configurationStores/appconfigname15335",
            "type": "Microsoft.Authorization/roleAssignments",
            "updatedBy": "399b9ae7-4602-afba-ae836b080",
            "updatedOn": "2026-02-08T12:11:36.906809+00:00"
        }


22) Agregar información de configuración con Azure CLI

    En Azure App Configuration, una clave como Dev:conStr es una clave jerárquica o con espacio de nombres. Los dos puntos (:) actúan como un delimitador que crea una jerarquía lógica, donde:

    Dev representa el espacio de nombres o prefijo del entorno (indica que esta configuración es para el entorno de Desarrollo).

    conStr representa el nombre de la configuración.

    Esta estructura jerárquica permite organizar los valores de configuración por entorno, funcionalidad o componente de la aplicación, lo que facilita la administración y recuperación de configuraciones relacionadas.

    Ejecuta el siguiente comando para almacenar la cadena de conexión de marcador de posición.
 


23) Corremos el siguiente comando

    az appconfig kv set --name $appConfigName \
    --key Dev:conStr \
    --value connectionString \
    --yes


24) Esto nos regresa esta respuesta.
    ### This command returns some JSON. The last line contains the value in plain text.

        {
        "contentType": "",
        "etag": "ZygAcALGqoA0p5X8RXf2Hy8uTig1zYF5SHo-gs",
        "key": "Dev:conStr",
        "label": null,
        "lastModified": "2026-02-08T12:25:19+00:00",
        "locked": false,
        "tags": {},
        "value": "connectionString"
        }

