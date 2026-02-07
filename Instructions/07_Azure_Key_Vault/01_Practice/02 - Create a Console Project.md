1) Creamos un folder 
   ### mkdir keyvault

2) Entramos en la carpeta 
   ### cd keyvault

3) Creamos un proyecto de consola
   ### dotnet new console


4) Instalamos estos 2 paquetes

    ### dotnet add package Azure.Identity
    ### dotnet add package Azure.Security.KeyVault.Secrets


5) Editamos el archivo Program.cs

    code Program.cs

6) Reemplazamos el codigo con esta estructura


    using Azure.Identity;
    using Azure.Security.KeyVault.Secrets;

    // Replace YOUR-KEYVAULT-NAME with your actual Key Vault name
    string KeyVaultUrl = "https://YOUR-KEYVAULT-NAME.vault.azure.net/";


    // ADD CODE TO CREATE A CLIENT



    // ADD CODE TO CREATE A MENU SYSTEM



    // ADD CODE TO CREATE A SECRET



    // ADD CODE TO LIST SECRETS



7) Establecemos la parte de 
    ### ADD CODE TO CREATE A CLIENT


    DefaultAzureCredentialOptions options = new()
{
    ExcludeEnvironmentCredential = true,
    ExcludeManagedIdentityCredential = true
};

// Create the Key Vault client using the URL and authentication credentials
var client = new SecretClient(new Uri(KeyVaultUrl), new DefaultAzureCredential(options));


8) Establecemos la parte de 
   ### ADD CODE TO CREATE A MENU SYSTEM

    // Main application loop - continues until user types 'quit'
    while (true)
    {
        // Display menu options to the user
        Console.Clear();
        Console.WriteLine("\nPlease select an option:");
        Console.WriteLine("1. Create a new secret");
        Console.WriteLine("2. List all secrets");
        Console.WriteLine("Type 'quit' to exit");
        Console.Write("Enter your choice: ");

        // Read user input and convert to lowercase for easier comparison
        string? input = Console.ReadLine()?.Trim().ToLower();
        
        // Check if user wants to exit the application
        if (input == "quit")
        {
            Console.WriteLine("Goodbye!");
            break;
        }

        // Process the user's menu selection
        switch (input)
        {
            case "1":
                // Call the method to create a new secret
                await CreateSecretAsync(client);
                break;
            case "2":
                // Call the method to list all existing secrets
                await ListSecretsAsync(client);
                break;
            default:
                // Handle invalid input
                Console.WriteLine("Invalid option. Please enter 1, 2, or 'quit'.");
                break;
        }
    }

9) En la parte ADD CODE TO CREATE A SECRET

    async Task CreateSecretAsync(SecretClient client)
    {
        try
        {
            Console.Clear();
            Console.WriteLine("\nCreating a new secret...");
            
            // Get the secret name from user input
            Console.Write("Enter secret name: ");
            string? secretName = Console.ReadLine()?.Trim();

            // Validate that the secret name is not empty
            if (string.IsNullOrEmpty(secretName))
            {
                Console.WriteLine("Secret name cannot be empty.");
                return;
            }
            
            // Get the secret value from user input
            Console.Write("Enter secret value: ");
            string? secretValue = Console.ReadLine()?.Trim();

            // Validate that the secret value is not empty
            if (string.IsNullOrEmpty(secretValue))
            {
                Console.WriteLine("Secret value cannot be empty.");
                return;
            }

            // Create a new KeyVaultSecret object with the provided name and value
            var secret = new KeyVaultSecret(secretName, secretValue);
            
            // Store the secret in Azure Key Vault
            await client.SetSecretAsync(secret);

            Console.WriteLine($"Secret '{secretName}' created successfully!");
            Console.WriteLine("Press Enter to continue...");
            Console.ReadLine();
        }
        catch (Exception ex)
        {
            // Handle any errors that occur during secret creation
            Console.WriteLine($"Error creating secret: {ex.Message}");
        }
    }


10) Localizamos // ADD CODE TO LIST SECRETS 


async Task ListSecretsAsync(SecretClient client)
{
    try
    {
        Console.Clear();
        Console.WriteLine("Listing all secrets in the Key Vault...");
        Console.WriteLine("----------------------------------------");

        // Get an async enumerable of all secret properties in the Key Vault
        var secretProperties = client.GetPropertiesOfSecretsAsync();
        bool hasSecrets = false;

        // Iterate through each secret property to retrieve full secret details
        await foreach (var secretProperty in secretProperties)
        {
            hasSecrets = true;
            try
            {
                // Retrieve the actual secret value and metadata using the secret name
                var secret = await client.GetSecretAsync(secretProperty.Name);
                
                // Display the secret information to the console
                Console.WriteLine($"Name: {secret.Value.Name}");
                Console.WriteLine($"Value: {secret.Value.Value}");
                Console.WriteLine($"Created: {secret.Value.Properties.CreatedOn}");
                Console.WriteLine("----------------------------------------");
            }
            catch (Exception ex)
            {
                // Handle errors for individual secrets (e.g., access denied, secret not found)
                Console.WriteLine($"Error retrieving secret '{secretProperty.Name}': {ex.Message}");
                Console.WriteLine("----------------------------------------");
            }
        }

        // Inform user if no secrets were found in the Key Vault
        if (!hasSecrets)
        {
            Console.WriteLine("No secrets found in the Key Vault.");
        }
    }
    catch (Exception ex)
    {
        // Handle general errors that occur during the listing operation
        Console.WriteLine($"Error listing secrets: {ex.Message}");
    
    }
    Console.WriteLine("Press Enter to continue...");
    Console.ReadLine();
}

11) Cambiamos el valor de Azure Value

    string KeyVaultUrl = "https://YOUR-KEYVAULT-NAME.vault.azure.net/";


12) Corremos el comando 

    ### az login


      Nos aparece el prompt y seleccionamos nuestra cuenta de correo

    Si nos aparece este error

    Authentication failed against tenant d39d348d-12b6-42d6-89be-5713c7bf86e9 'Directorio predeterminado': (pii). Status: Response_Status.Status_InteractionRequired, Error code: 3399614467, Tag: 558133255
    If you need to access subscriptions in the following tenants, please use `az login --tenant TENANT_ID`.
    d39d348d-12b6-42d6-89be-5713c7bf86e9 'Directorio predeterminado'
    No subscriptions found for x@hotmail.com.

13) Si falla corremos esto

     az login --use-device-code


13) Agregamos opcion 1 

    Luego interactuamos la opcion 2


    Tenant: Directorio predeterminado
    Subscription: Pay-As-You-Go (4e78-c0-e395c2d0)
    Listing all secrets in the Key Vault...
    ----------------------------------------
    Name: Test
    Value: TestValue
    Created: 2/7/2026 6:56:31 PM +00:00
    ----------------------------------------
    Name: MySecret
    Value: My secret value
    Created: 2/7/2026 5:47:57 PM +00:00
    ----------------------------------------
    Press Enter to continue...
