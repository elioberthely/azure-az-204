1) Creamos un folder 
   ### mkdir authapp

2) Entramos en la carpeta 
   ### cd authapp

3) Creamos un proyecto de consola
   ### dotnet new console


4) Instalamos estos 2 paquetes

    ### dotnet add package Microsoft.Identity.Client
    ### dotnet add package dotenv.net


5) Creamos un archivo llamado .env en la raiz, sustituimos los datos por la informacion de Azure

    CLIENT_ID="YOUR_CLIENT_ID"
    TENANT_ID="YOUR_TENANT_ID

6) Lo modificamos asi,tomando estos datos de App registration

     Application (client) ID

     Directory (tenant) ID

7) Modificamos el Program.cs con esto

        ```csharp

        using Microsoft.Identity.Client;
        using dotenv.net;

        // Load environment variables from .env file
        DotEnv.Load();
        var envVars = DotEnv.Read();

        // Retrieve Azure AD Application ID and tenant ID from environment variables
        string _clientId = envVars["CLIENT_ID"];
        string _tenantId = envVars["TENANT_ID"];

        // ADD CODE TO DEFINE SCOPES AND CREATE CLIENT



        // ADD CODE TO ACQUIRE AN ACCESS TOKEN

        ```

8) En Scope pegamos esto

      ```csharp
        // ADD CODE TO DEFINE SCOPES AND CREATE CLIENT
        // Define the scopes required for authentication
           string[] _scopes = { "User.Read" };
            
            
        // Build the MSAL public client application with authority and redirect URI
        var app = PublicClientApplicationBuilder.Create(_clientId)
            .WithAuthority(AzureCloudInstance.AzurePublic, _tenantId)
            .WithDefaultRedirectUri()
            .Build();

        ```

9) en Adquire Access Token esto

        ```csharp
        // ADD CODE TO ACQUIRE AN ACCESS TOKEN
        // Attempt to acquire an access token silently or interactively
        AuthenticationResult result;
        try
        {
            // Try to acquire token silently from cache for the first available account
            var accounts = await app.GetAccountsAsync();
            result = await app.AcquireTokenSilent(_scopes, accounts.FirstOrDefault())
                        .ExecuteAsync();
        }
        catch (MsalUiRequiredException)
        {
            // If silent token acquisition fails, prompt the user interactively
            result = await app.AcquireTokenInteractive(_scopes)
                        .ExecuteAsync();
        }

        // Output the acquired access token to the console
        Console.WriteLine($"Access Token:\n{result.AccessToken}");
        
        ```

10) Corremos el comando dotnet run

11) Nos abre el navegador web y aceptamos los permisos

12) La pagina web, nos regresa esto

        Authentication complete.
        You can return to the application. Please close this browser tab.

        For your security: Do not share the contents of this page, the address bar, or take screenshots.


13) La consola nos regresa el token

<!-- Access T:
eyJ0eXAiOiJKV1QiLCJub25jZSI6IkdZb2RabktlU0hvS29DQmYwZXlJSHgxb2s4cXhOcDhpY3ozbVkxcHgydk0iLCJhbGciOiJSUzI1NiIsIng1dCI6IlBjWDk4R1g0MjBUMVg2c0JEa3poUW1xZ3dNVSIsImtpZCI6IlBjWDk4R1g0MjBUMVg2c0JEa3poUW1xZ3dNVSJ9.BGtB5JIpJ8GRocov8AFk8BQsgfaPRjL7z-Z14fX3oV4HR8k7SDUgFFtfdfsyNdNbRAgbQEI9WBZvltcdQQzc_eTHb6z-d48yVFxzKoZ6xAxT3cUtPfDgjjtOwgTx-g2X99ir5p6gfKfX2XANBz9tBXyjcEXE07u6F78cy6l2dRKNGXS8T0dDUHjNeNjBjHUssUzyz4lClI2YWA8qFvgANYt9v1L9P9ERUSBxwDnn5iRtFb2xfhHOW6BB9n_b6lYayBwNHAd2YG3reJ7qq656pQSYVmkVtjFLIAh3X7vGGqfTwtnuEZVOebuTU_Nau2Z51JdK9eBzP_R35BVOtH7ZSQ -->


14) Inicia la aplicación por segunda vez y observa que ya no recibes la notificación de Permisos solicitados. El permiso que concediste anteriormente se ha almacenado en caché.
Nota: Si tienes varias cuentas y con algunas configuraciones de cuenta, es posible que vuelvas a ver la notificación.