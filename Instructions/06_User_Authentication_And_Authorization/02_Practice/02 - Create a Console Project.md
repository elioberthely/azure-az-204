1) Creamos un folder 
   ### mkdir graphapp

2) Entramos en la carpeta 
   ### cd graphapp

3) Creamos un proyecto de consola
   ### dotnet new console


4) Instalamos estos 2 paquetes

    ### dotnet add package Azure.Identity
    ### dotnet add package Microsoft.Graph
    ### dotnet add package dotenv.net


    
5) Creamos un archivo llamado .env en la raiz, sustituimos los datos por la informacion de Azure

    CLIENT_ID="YOUR_CLIENT_ID"
    TENANT_ID="YOUR_TENANT_ID"

6) Lo modificamos asi,tomando estos datos de App registration

     Application (client) ID    

     Directory (tenant) ID

7) Modificamos el Program.cs con esto

        using Microsoft.Graph;
        using Azure.Identity;
        using dotenv.net;

        // Load environment variables from .env file (if present)
        DotEnv.Load();
        var envVars = DotEnv.Read();

        // Read Azure AD app registration values from environment
        string clientId = envVars["CLIENT_ID"];
        string tenantId = envVars["TENANT_ID"];

        // Validate that required environment variables are set
        if (string.IsNullOrEmpty(clientId) || string.IsNullOrEmpty(tenantId))
        {
            Console.WriteLine("Please set CLIENT_ID and TENANT_ID environment variables.");
            return;
        }

        // ADD CODE TO DEFINE SCOPE AND CONFIGURE AUTHENTICATION



        // ADD CODE TO CREATE GRAPH CLIENT AND RETRIEVE USER PROFILE



8) Agregamos esto a Define Scope And Configure Authentication

        // Define the Microsoft Graph permission scopes required by this app
        var scopes = new[] { "User.Read" };

        // Configure interactive browser authentication for the user
        var options = new InteractiveBrowserCredentialOptions
        {
            ClientId = clientId, // Azure AD app client ID
            TenantId = tenantId, // Azure AD tenant ID
            RedirectUri = new Uri("http://localhost") // Redirect URI for auth flow
        };
        var credential = new InteractiveBrowserCredential(options);

9) Agregamos esto a Create Graph Client and Retrieve User Profile

        // Create a Microsoft Graph client using the credential
        var graphClient = new GraphServiceClient(credential);

        // Retrieve and display the user's profile information
        Console.WriteLine("Retrieving user profile...");
        await GetUserProfile(graphClient);

        // Function to get and print the signed-in user's profile
        async Task GetUserProfile(GraphServiceClient graphClient)
        {
            try
            {
                // Call Microsoft Graph /me endpoint to get user info
                var me = await graphClient.Me.GetAsync();
                Console.WriteLine($"Display Name: {me?.DisplayName}");
                Console.WriteLine($"Principal Name: {me?.UserPrincipalName}");
                Console.WriteLine($"User Id: {me?.Id}");
            }
            catch (Exception ex)
            {
                // Print any errors encountered during the call
                Console.WriteLine($"Error retrieving profile: {ex.Message}");
            }
        }

10) Corremos el comando dotnet run

11) Nos abre el navegador web y aceptamos los permisos

12) La pagina web, nos regresa esto

        Authentication complete.
        You can return to the application. Please close this browser tab.

        For your security: Do not share the contents of this page, the address bar, or take screenshots.

13) Esto nos regresa los datos del perfil

<!-- Retrieving user profile...
Display Name: Juan Perez
Principal Name: juanperez_hotmail.com#EXT#@juanperezhotmail.onmicrosoft.com
User Id: 399b9ae7-71-4202-ae8b080
 -->
