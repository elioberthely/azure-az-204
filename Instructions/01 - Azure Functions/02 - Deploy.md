

1) Del Lado Izquierdo seleccionamos el Simbolo de A, en la columna de Activity Bar

2) Buscamos Resourcer (Remote)
    Seleccionamos Sign in to Azure
    Nos logeamos

3) En caso que nos aparezca el siguiente mensaje


        Please sign in to a specific tenant (directory) to continue

        Sign in to Tenant (Directory)

        View Accounts & Tenants

###             Damos click a Sign in to Tenant (Directory)

4) Nos aparece del lado izquierdo
        Pay-As-You-Go


5) Alado de Resources esta invisible unos botones, ponemos el cursor a la derecha se despliegan 4 iconos
        + Seleccionamos este (Create Resource)

###     Seleccionamos Create Function App in Azure...

6) nos aparece esta leyenda
        Enter a name for the new function app. (Press 'Enter' to confirm or 'Escape' to cancel)

###     Ponemos un nombre unico en este caso Myfunctionappabn 
                empezando con mayuscula

        Seleccionamos la region WestUS
          Select .NET 8.0 Isolated
          Select resource authentication type:  Secrets

7) Una vez que se crea
        Hacemos del lado izquierdo, en Pay-As-You-Go
        Buscamos Function App
        nos debe aparecer la funcion ebmyfuctionappabn



###     Deploy the Project to Azure

1) Buscamos del lado izquierdo  Pay-As-You-Go
        Seleccionamos Function App
        Seleccionamos la funcion que creamos
        Damos boton derecho - Deploy to Function App

2) Aparece en la terminal de Azure
        Deploy to app "ebmyfuctionappabn"

3) Al finalizar aparece una ventana que dice view output. Eso significa que se creo correctamente

4) Damos click en Output, en la parte inferior, y ahi aparece el log donde apunta a la direccion
        https://ebmyfunctionappabn.azurewebsites.net/api/httpexample


5) Una vez desplegado en produccion

6) HAcemos una modificacion en local
        Cambiamos Welcome to Azure Functions!

### Welcome to Azure Functions ABN

7) Hacemos un deploy nuevo y debe aparecer este cambio







