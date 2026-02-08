

<details> 
    <summary>In this exercise, you create an Azure API Management instance, import an OpenAPI specification backend API, configure the API settings including the web service URL and subscription requirements, and test the API operations to verify they work correctly.</summary>

Tasks performed in this exercise:

1) Create an Azure API Management (APIM) instance
2) Import an API
3) Configure the backend settings
4) Test the API
</details>


### Import and configure an API with Azure API Management

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

    ### az group create  --name myResourceGroupAPIManagement --location eastus 

8) Nos debe devolver esta instruccion

    {
        "id": "/subscriptions/5030b6-f741-4e78-80c0-e395d0d0/resourceGroups/myResourceGroupAPIManagement",
        "location": "eastus",
        "managedBy": null,
        "name": "myResourceGroupAPIManagement",
        "properties": {
            "provisioningState": "Succeeded"
        },
        "tags": null,
        "type": "Microsoft.Resources/resourceGroups"
    }

9) Creamos estas 4 variables - sustituimos los nombres por los que creamos
    
    myLocation=eastus
    myEmail=elioberthely@hotmail.com
    myResourceGroup=myResourceGroupAPIManagement

    ### el nombre tiene que ser solo minusculas 
    myApiName=import-apim-$RANDOM

10) Corroboramos el valor de las variables

    echo $myLocation
    echo $myEmail
    echo $myResourceGroup
    echo $myApiName


11) Creamos un Azure Key Vault Resource

    az apim create -n $myApiName \
    --location $myLocation \
    --publisher-email $myEmail  \
    --resource-group $myResourceGroup \
    --publisher-name Import-API-Exercise \
    --sku-name Consumption

12) Si nos aparece este error 

        ### Resource provider 'Microsoft.ApiManagement' used by this operation is not registered. We are registering for you.
        
        Corremos este comando

        az provider show --namespace Microsoft.ApiManagement --query "registrationState"

        Nos dice el estado

        Esperamos unos 2 minutos y corremos de nuevo hasta que diga Registered

        Corremos de nuevo el comando

            az apim create -n $myApiName \
        --location $myLocation \
        --publisher-email $myEmail  \
        --resource-group $myResourceGroup \
        --publisher-name Import-API-Exercise \
        --sku-name Consumption
    
13) Entramos a Azure Portal
        https://portal.azure.com/


14) Buscamos API Management services.

15) Actualizamos nos debe aparecer el recurso online

    import-apim-7694    Online  Consumption mtv1    API Management service      East US     myResourceGroupAPIManagement    Pay-As-You-Go

16) Le damos click a import-apim-7694 este nos manda a otra pantalla

17) Del lado izquierdo seleccionamos APIs

    Aparece un acordeon con varias opciones

        APIs
        MCP Servers
        Products
        ...
        OAuth 2.0 + OpenID Connect

18) Seleccionamos APIs

    Ya en el segundo nivel de APIs, buscamos 

    Create from definition
    Open API le damos click y nos aparece una ventana

    ## Seleccionamos en la parte superior Full

---
   ### OpenAPI Specification	https://petstore.swagger.io/v2/swagger.json	
    
    References the service implementing the API, requests are forwarded to this address. Most of the necessary information in the form is automatically populated after you enter this value.

    URL Scheme                  HTTPS

    Le damos click al boton Create
---

19) Actualizamos, vamos a ver debajo un API dice Swagger PetStore

19) Probar la API


    Selecciona Test en la barra de menú. Esto mostrará todas las operaciones disponibles en la API.

20) Busca y selecciona la operación Find Pets by id

21) En la sección Template parameters, ingresa available como valor en el campo status.

22) Selecciona Send. Es posible que necesites desplazarte hacia abajo en la página para ver la respuesta HTTP.

23) El backend responderá con 200 OK y algunos datos.


---
1️⃣ Lo que ya hiciste

1) Consumiste un API existente: https://petstore.swagger.io/v2/swagger.json.

2) Ese API ya está creado y expone operaciones como: Find Pets by status, Add Pet, Delete Pet, etc.

3) Lo que hiciste en Azure fue importar ese API a tu instancia de API Management (APIM).

2️⃣ ¿Qué significa “Import and Configure API”?

### Cuando haces Import and Configure API en Azure:

### Importa la definición del API:

### Esa definición está en formato Swagger / OpenAPI (swagger.json).

Azure lee este archivo y sabe qué rutas, métodos HTTP y parámetros existen.

Básicamente te dice: “Estas son las operaciones que este API tiene y cómo llamarlas”.

Crea un contenedor en Azure APIM para ese API:

No estás copiando la lógica del API.

Lo que haces es exponer ese API existente a través de Azure, como si pusieras un “proxy” delante del API real.

Permite configuración adicional:

Puedes definir políticas, como autenticación, límite de solicitudes (rate limiting), transformación de respuestas, caching, etc.

También puedes organizar el API, cambiar nombres de operaciones, agrupar endpoints, etc.

Te permite probar y documentar:

La sección de Test que usaste es parte de APIM.

Puedes invocar el API sin escribir código, ver las respuestas, probar distintos parámetros, etc.

🔑 Clave

### No estás creando un API nuevo: solo estás consumiendo uno existente y poniéndolo detrás de APIM.

### APIM actúa como puerta de entrada (gateway). Esto te da control sobre un API de terceros o interno: seguridad, monitoreo, métricas, transformación, etc.

### En otras palabras, Import and Configure API hace esto:

### "Toma un API existente y lo registra dentro de Azure APIM para que puedas gestionarlo, probarlo y agregarle políticas sin cambiar el API original."
---



