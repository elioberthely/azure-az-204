
<details> 
    <summary>In this exercise, you deploy and run a container in Azure Container Instances (ACI) using Azure CLI. You learn how to create a container group, specify container settings, and verify that your containerized application is running in the cloud.
 </summary>

Tasks performed in this exercise:
1) Create Azure Container Instance resources in Azure
2) Create and deploy a container
3) Verify the container is running
</details>


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

7) az group create --location eastus --name myResourceGroupABN
<details>
    <summary>8) Nos debe devolver una respuesta su creacion </summary>
{
  "id": "/subscriptions/5030b2c6-f741-4e78-80c0-e30d0/resourceGroups/myResourceGroupABN",
  "location": "eastus",
  "managedBy": null,
  "name": "myResourceGroupABN",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null,
  "type": "Microsoft.Resources/resourceGroups"
}
</details>



8) Crear y desplegar un contenedor
    Creas un contenedor proporcionando un nombre, una imagen de Docker y un grupo de recursos de Azure al comando az container create. Expones el contenedor a Internet especificando una etiqueta de nombre DNS.

    Ejecuta el siguiente comando para crear un nombre DNS que se usará para exponer tu contenedor a Internet. Tu nombre DNS debe ser único; ejecuta este comando desde Cloud Shell para crear una variable que contenga un nombre único.

    DNS_NAME_LABEL=aci-example-$RANDOM

9) Ejecuta el siguiente comando para crear una instancia de contenedor. Reemplaza **myResourceGroup** y **myLocation** con los valores que usaste anteriormente. La operación tarda unos minutos en completarse.

    az container create --resource-group myResourceGroup \
        --name mycontainer \
        --image mcr.microsoft.com/azuredocs/aci-helloworld \
        --ports 80 \
        --dns-name-label $DNS_NAME_LABEL --location myLocation \
        --os-type Linux \
        --cpu 1 \
        --memory 1.5 

az container create --resource-group myResourceGroupABN --name mycontainer --image mcr.microsoft.com/azuredocs/aci-helloworld --ports 80 --dns-name-label $DNS_NAME_LABEL --location eastus --os-type Linux --cpu 1 --memory 1.5


<details>
    <summary>9) Nos debe devolver una respuesta su creacion </summary>
{
  "confidentialComputeProperties": null,
  "containerGroupProfile": null,
  "containers": [
    {
      "command": null,
      "configMap": {
        "keyValuePairs": {}
      },
      "environmentVariables": [],
      "image": "mcr.microsoft.com/azuredocs/aci-helloworld",
      "instanceView": {
        "currentState": {
          "detailStatus": "",
          "exitCode": null,
          "finishTime": null,
          "startTime": "2026-02-02T08:47:02.661000+00:00",
          "state": "Running"
        },
        "events": [
          {
            "count": 1,
            "firstTimestamp": "2026-02-02T08:46:49+00:00",
            "lastTimestamp": "2026-02-02T08:46:49+00:00",
            "message": "pulling image \"mcr.microsoft.com/azuredocs/aci-helloworld@sha256:b9cec4d6b50c6b628e5dca972cf132d38ed8f5bc955bb179c3\"",
            "name": "Pulling",
            "type": "Normal"
          },
          {
            "count": 1,
            "firstTimestamp": "2026-02-02T08:46:53+00:00",
            "lastTimestamp": "2026-02-02T08:46:53+00:00",
            "message": "Successfully pulled image \"mcr.microsoft.com/azuredocs/aci-helloworld@sha256:b9cec4d6b50c6bf25e3f5dca972cf132d38ed8f5bc955bb179c3\"",
            "name": "Pulled",
            "type": "Normal"
          },
          {
            "count": 1,
            "firstTimestamp": "2026-02-02T08:47:02+00:00",
            "lastTimestamp": "2026-02-02T08:47:02+00:00",
            "message": "Started container",
            "name": "Started",
            "type": "Normal"
          }
        ],
        "previousState": null,
        "restartCount": 0
      },
      "livenessProbe": null,
      "name": "mycontainer",
      "ports": [
        {
          "port": 80,
          "protocol": "TCP"
        }
      ],
      "readinessProbe": null,
      "resources": {
        "limits": null,
        "requests": {
          "cpu": 1.0,
          "gpu": null,
          "memoryInGb": 1.5
        }
      },
      "securityContext": null,
      "volumeMounts": null
    }
  ],
  "diagnostics": null,
  "dnsConfig": null,
  "encryptionProperties": null,
  "extensions": null,
  "id": "/subscriptions/5030b2c6-f741-e395c2d2d0d0/resourceGroups/myResourceGroupABN/providers/Microsoft.ContainerInstance/containerGroups/mycontainer",
  "identity": null,
  "imageRegistryCredentials": null,
  "initContainers": [],
  "instanceView": {
    "events": [],
    "state": "Running"
  },
  "ipAddress": {
    "autoGeneratedDomainNameLabelScope": "Unsecure",
    "dnsNameLabel": "aci-example-11321",
    "fqdn": "aci-example-11321.eastus.azurecontainer.io",
    "ip": "134.33.242.10",
    "ports": [
      {
        "port": 80,
        "protocol": "TCP"
      }
    ],
    "type": "Public"
  },
  "isCreatedFromStandbyPool": false,
  "location": "eastus",
  "name": "mycontainer",
  "osType": "Linux",
  "priority": null,
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroupABN",
  "restartPolicy": null,
  "sku": "Standard",
  "standbyPoolProfile": null,
  "subnetIds": null,
  "tags": {},
  "type": "Microsoft.ContainerInstance/containerGroups",
  "volumes": null,
  "zones": null
}
</details>

10) En el comando anterior, $DNS_NAME_LABEL especifica tu nombre DNS. El nombre de la imagen, mcr.microsoft.com/azuredocs/aci-helloworld, hace referencia a una imagen de Docker que ejecuta una aplicación web básica de Node.js.

 

11) **Verificar que el contenedor se esté ejecutando**
    Puedes comprobar el estado de creación del contenedor con el comando `az container show`.

    Ejecuta el siguiente comando para verificar el estado de aprovisionamiento del contenedor que creaste. Reemplaza **myResourceGroup** con el valor que usaste anteriormente.

az container show --resource-group myResourceGroup --name mycontainer --query "{FQDN:ipAddress.fqdn,ProvisioningState:provisioningState}" --out table

12) Nos devuelve esta respuesta

FQDN                                        ProvisioningState
------------------------------------------  -------------------
aci-example-11321.eastus.azurecontainer.io  Succeeded

