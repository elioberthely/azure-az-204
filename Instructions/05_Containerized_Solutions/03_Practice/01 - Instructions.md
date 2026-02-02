Deploy a container to Azure Container Apps with the Azure CLI
In this exercise, you deploy a containerized application to Azure Container Apps using Azure CLI. You learn how to create a container app environment, deploy your container, and verify that your application is running in Azure.

Tasks performed in this exercise:

Create resources in Azure
Create an Azure Container Apps environment
Deploy a container app to the environment


1) Create a resource group and prepare the Azure environment

) Entramos al portal

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

7) Crea un grupo de recursos para los recursos necesarios en este ejercicio. Reemplaza myResourceGroup con el nombre que quieras usar para el grupo de recursos. 

az group create --location eastus --name myResourceGroupABN2026

<details>
    <summary>8) Nos debe devolver una respuesta su creacion </summary>
{
  "id": "/subscriptions/5030b2c6-f741-4e78/resourceGroups/myResourceGroupABN2026",
  "location": "eastus",
  "managedBy": null,
  "name": "myResourceGroupABN2026",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null,
  "type": "Microsoft.Resources/resourceGroups"
}
</details>

8) Ejecuta el siguiente comando para asegurarte de que tienes instalada la versión más reciente de la extensión **Azure Container Apps** para la CLI.


9) Nos devolvio este valor

    No stable version of 'containerapp' to install. Preview versions allowed.
The installed extension 'containerapp' is in preview.

10) Registrar namespaces
    Hay dos namespaces que deben registrarse para Azure Container Apps, y te aseguras de que estén registrados en los siguientes pasos. Cada registro puede tardar algunos minutos en completarse si aún no están configurados en tu suscripción.

    ### Registrar el namespace Microsoft.App.

    az provider register --namespace Microsoft.App


    ### Registrar el proveedor Microsoft.OperationalInsights para el espacio de trabajo de Azure Monitor Log Analytics si no lo has usado antes.

    az provider register --namespace Microsoft.OperationalInsights

11) Crear un entorno de Azure Container Apps

    Un entorno en Azure Container Apps crea un límite seguro alrededor de un grupo de aplicaciones en contenedor. Las aplicaciones en contenedor desplegadas en el mismo entorno se implementan en la misma red virtual y escriben registros en el mismo espacio de trabajo de Log Analytics.

    Crea un entorno con el comando az containerapp env create. Reemplaza myResourceGroup y myLocation con los valores que usaste 
    anteriormente. La operación tarda unos minutos en completarse.

    az containerapp env create \
        --name my-container-env \
        --resource-group myResourceGroup \
        --location myLocation

    ### az containerapp env create --name my-container-env --resource-group myResourceGroupABN2026 --location eastus


<details>
    <summary>12) Nos debe devolver una respuesta su creacion </summary>
{
 "id": "/subscriptions/5030b2c6-f741-4e78-80c0-e395c2d2d0d0/resourceGroups/myResourceGroupABN2026/providers/Microsoft.App/managedEnvironments/my-container-env",
  "location": "East US",
  "name": "my-container-env",
  "properties": {
    "appInsightsConfiguration": null,
    "appLogsConfiguration": {
      "destination": "log-analytics",
      "logAnalyticsConfiguration": {
        "customerId": "49111334-923b-84ba4a8d865c",
        "dynamicJsonColumns": false,
        "sharedKey": null
      }
    },
    "availabilityZones": null,
    "customDomainConfiguration": {
      "certificateKeyVaultProperties": null,
      "certificatePassword": null,
      "certificateValue": null,
      "customDomainVerificationId": "AA20FDA27E11826BF883540E7DF9DD7C630CB12704BB88C",
      "dnsSuffix": null,
      "expirationDate": null,
      "subjectName": null,
      "thumbprint": null
    },
    "daprAIConnectionString": null,
    "daprAIInstrumentationKey": null,
    "daprConfiguration": {
      "version": "1.13.6-msft.6"
    },
    "defaultDomain": "reddune-afcf372f.eastus.azurecontainerapps.io",
    "diskEncryptionConfiguration": null,
    "environmentMode": null,
    "eventStreamEndpoint": "https://eastus.azurecontainerapps.dev/subscriptions/5030b2c6-c2d2d0d0/resourceGroups/myResourceGroupABN2026/managedEnvironments/my-container-env/eventstream",
    "infrastructureResourceGroup": null,
    "ingressConfiguration": null,
    "kedaConfiguration": {
      "version": "2.17.2"
    },
    "openTelemetryConfiguration": null,
    "peerAuthentication": {
      "mtls": {
        "enabled": false
      }
    },
    "peerTrafficConfiguration": {
      "encryption": {
        "enabled": false
      }
    },
    "provisioningState": "Succeeded",
    "publicNetworkAccess": "Enabled",
    "staticIp": "20.185.96.2",
    "vnetConfiguration": null,
    "workloadProfiles": [
      {
        "enableFips": false,
        "name": "Consumption",
        "workloadProfileType": "Consumption"
      }
    ],
    "zoneRedundant": false
  },
  "resourceGroup": "myResourceGroupABN2026",
  "systemData": {
    "createdAt": "2026-02-02T09:02:00.2665664",
    "createdBy": "x@hotmail.com",
    "createdByType": "User",
    "lastModifiedAt": "2026-02-02T09:02:00.2665664",
    "lastModifiedBy": "x@hotmail.com",
    "lastModifiedByType": "User"
  },
  "type": "Microsoft.App/managedEnvironments"
}
</details>


13) Desplegar una aplicación en contenedor en el entorno

    Después de que el entorno de aplicaciones en contenedor termine de desplegarse, puedes desplegar una imagen de contenedor en tu entorno.

    Despliega una imagen de contenedor de aplicación de ejemplo con el comando containerapp create. Reemplaza myResourceGroup con el valor que usaste anteriormente.

    az containerapp create \
    --name my-container-app \
    --resource-group myResourceGroup \
    --environment my-container-env \
    --image mcr.microsoft.com/azuredocs/containerapps-helloworld:latest \
    --target-port 80 \
    --ingress 'external' \
    --query properties.configuration.ingress.fqdn


    ### az containerapp create --name my-container-app --resource-group myResourceGroupABN2026 --environment my-container-env --image mcr.microsoft.com/azuredocs/containerapps-helloworld:latest --target-port 80 --ingress 'external' --query properties.configuration.ingress.fqdn


    Esto regresa esta respuesta

    The behavior of this command has been altered by the following extension: containerapp

    Container app created. Access your app at https://my-container-app.reddune-afcf372f.eastus.azurecontainerapps.io/


14) Abrimos el link (crea una pagina web)

    Your Azure Container Apps app is live

    To Learn more, follow our docs here

15) Al establecer --ingress en external, haces que la aplicación en contenedor esté disponible para solicitudes públicas. El comando devuelve un enlace para acceder a tu aplicación.

    Aplicación en contenedor creada. Accede a tu aplicación en <url>

    Para verificar el despliegue, selecciona la URL que devolvió el comando az containerapp create para confirmar que la aplicación en contenedor se está ejecutando.

