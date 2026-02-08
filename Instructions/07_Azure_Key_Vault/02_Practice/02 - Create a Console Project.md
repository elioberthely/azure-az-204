1) Creamos un folder 
   ### mkdir appconfig

2) Entramos en la carpeta 
   ### cd appconfig

3) Creamos un proyecto de consola
   ### dotnet new console

4) Instalamos estos 2 paquetes

    ### dotnet add package Azure.Identity
    ### dotnet add package Microsoft.Extensions.Configuration.AzureAppConfiguration

5) Editamos el archivo Program.cs

    ### code Program.cs



6) Pegamos este contenido

        ```csharp
        
        using Microsoft.Extensions.Configuration;
        using Microsoft.Extensions.Configuration.AzureAppConfiguration;
        using Azure.Identity;

        // Set the Azure App Configuration endpoint, replace YOUR_APP_CONFIGURATION_NAME
        // with the name of your actual App Configuration service

        string endpoint = "https://YOUR_APP_CONFIGURATION_NAME.azconfig.io"; 

        // Configure which authentication methods to use
        // DefaultAzureCredential tries multiple auth methods automatically
        DefaultAzureCredentialOptions credentialOptions = new()
        {
            ExcludeEnvironmentCredential = true,
            ExcludeManagedIdentityCredential = true
        };

        // Create a configuration builder to combine multiple config sources
        var builder = new ConfigurationBuilder();

        // Add Azure App Configuration as a source
        // This connects to Azure and loads configuration values
        builder.AddAzureAppConfiguration(options =>
        {
            
            options.Connect(new Uri(endpoint), new DefaultAzureCredential(credentialOptions));
        });

        // Build the final configuration object
        try
        {
            var config = builder.Build();
            
            // Retrieve a configuration value by key name
            Console.WriteLine(config["Dev:conStr"]);
        }
        catch (Exception ex)
        {
            Console.WriteLine($"Error connecting to Azure App Configuration: {ex.Message}");
        }

        ```

7) Editamos el endpoint

      ```csharp

      string endpoint = "https://appconfigname15335.azconfig.io"; 
    
      ```

8)   ### az login

   ### az login

    Nos aparece el prompt y seleccionamos nuestra cuenta de correo

    Si nos aparece este error

    Authentication failed against tenant d39d348d-12b6-42d6-89be-5713c7bf86e9 'Directorio predeterminado': (pii). Status: Response_Status.Status_InteractionRequired, Error code: 3399614467, Tag: 558133255
    If you need to access subscriptions in the following tenants, please use `az login --tenant TENANT_ID`.
    d39d348d-12b6-42d6-89be-5713c7bf86e9 'Directorio predeterminado'
    No subscriptions found for x@hotmail.com.

9) Si falla corremos este 

    az login --use-device-code

    esto nos muestra este mensaje

    ### To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code ABKGEISIS to authenticate.
 
    Hacemos click a este link e ingresamos el codigo que nos muestra osea ABKGEISIS
    
    Damos aceptar listo

10) El mensaje devuelve

    connectionString

    Que es el valor que el punto 23 de la documentacion le pusimos

    az appconfig kv set --name $appConfigName \
        --key Dev:conStr \
        --value connectionString \
        --yes

    ✅ Autenticación correcta
    ✅ Permisos correctos
    ✅ App Configuration funcionando
    ✅ Tu app leyendo desde Azure

11) Las credenciales

    🔐 Azure CLI (az login)

    Cuando ejecutas:

    az login

    Son las que provee la cuenta de azure y permite obtener ese resultado de lo contrario no se mostraria

    