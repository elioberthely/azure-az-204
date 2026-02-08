1) Creamos un folder 
   ### mkdir cosmosdb

2) Entramos en la carpeta 
   ### cd cosmosdb

3) Creamos un proyecto de consola
   ### dotnet new console


4) Instalamos estos 3 paquetes
    ### dotnet add package Microsoft.Azure.Cosmos --version 3.*
    ### dotnet add package Newtonsoft.Json --version 13.*
    ### dotnet add package dotenv.net

5) Ejecuta el siguiente comando para crear el archivo .env que contendrá los secretos y luego ábrelo en el editor de código.
code
   ###  touch .env
   ###  code .env

   Una vez que hayamos corrido esto nos abre el archivo

6) Agregue el siguiente código al archivo .env. 
   ### Reemplace YOUR_DOCUMENT_ENDPOINT y YOUR_ACCOUNT_KEY con los valores que anotó anteriormente.

   Quedando algo asi

   DOCUMENT_ENDPOINT=https://cosmosexercise10126.documents.azure.com:443/
   ACCOUNT_KEY=ayRVHv7tcDnoSIKDZh2fkjqi8g8xsjkSCecHqN9OtcJHSKWHC2MorACDbetFcmg==


7) Corremos esto, para que abra el archivo
    ### code Program.cs



8) Sustituimos por esto

        ```csharp

        using Microsoft.Azure.Cosmos;
        using dotenv.net;

        string databaseName = "myDatabase"; // Name of the database to create or use
        string containerName = "myContainer"; // Name of the container to create or use

        // Load environment variables from .env file
        DotEnv.Load();
        var envVars = DotEnv.Read();
        string cosmosDbAccountUrl = envVars["DOCUMENT_ENDPOINT"];
        string accountKey = envVars["ACCOUNT_KEY"];

        if (string.IsNullOrEmpty(cosmosDbAccountUrl) || string.IsNullOrEmpty(accountKey))
        {
            Console.WriteLine("Please set the DOCUMENT_ENDPOINT and ACCOUNT_KEY environment variables.");
            return;
        }

        // CREATE THE COSMOS DB CLIENT USING THE ACCOUNT URL AND KEY


        try
        {
            // CREATE A DATABASE IF IT DOESN'T ALREADY EXIST


            // CREATE A CONTAINER WITH A SPECIFIED PARTITION KEY


            // DEFINE A TYPED ITEM (PRODUCT) TO ADD TO THE CONTAINER


            // ADD THE ITEM TO THE CONTAINER


        }
        catch (CosmosException ex)
        {
            // Handle Cosmos DB-specific exceptions
            // Log the status code and error message for debugging
            Console.WriteLine($"Cosmos DB Error: {ex.StatusCode} - {ex.Message}");
        }
        catch (Exception ex)
        {
            // Handle general exceptions
            // Log the error message for debugging
            Console.WriteLine($"Error: {ex.Message}");
        }

        // This class represents a product in the Cosmos DB container
        public class Product
        {
            public string? id { get; set; }
            public string? name { get; set; }
            public string? description { get; set; }
        }

        ```


9) Agregamos debajo de 

    ###    // CREATE THE COSMOS DB CLIENT USING THE ACCOUNT URL AND KEY

    ```csharp
    
    CosmosClient client = new(
        accountEndpoint: cosmosDbAccountUrl,
        authKeyOrResourceToken: accountKey
    );

    ```

10) Agregamos debajo de

    ### // CREATE A DATABASE IF IT DOESN'T ALREADY EXIST

    ```csharp
    
    Database database = await client.CreateDatabaseIfNotExistsAsync(databaseName);
    Console.WriteLine($"Created or retrieved database: {database.Id}");
    
    ```

11) Agregamos debajo de
    ### // CREATE A CONTAINER WITH A SPECIFIED PARTITION KEY

    ```csharp
    
    Container container = await database.CreateContainerIfNotExistsAsync(
        id: containerName,
        partitionKeyPath: "/id"
    );
    Console.WriteLine($"Created or retrieved container: {container.Id}");

    ```

12) Agregamos debajo de

    ### // DEFINE A TYPED ITEM (PRODUCT) TO ADD TO THE CONTAINER

    ```csharp
    
    Product newItem = new Product
    {
        id = Guid.NewGuid().ToString(), // Generate a unique ID for the product
        name = "Sample Item",
        description = "This is a sample item in my Azure Cosmos DB exercise."
    };

    ```

13) 
    ### // ADD THE ITEM TO THE CONTAINER

    ```csharp
    
    ItemResponse<Product> createResponse = await container.CreateItemAsync(
        item: newItem,
        partitionKey: new PartitionKey(newItem.id)
    );

    Console.WriteLine($"Created item with ID: {createResponse.Resource.id}");
    Console.WriteLine($"Request charge: {createResponse.RequestCharge} RUs");

    ```

14) Buildeamos la aplicacion

    ### dotnet build

15) Debemos obtener una respuesta sin error

    cosmosdb -> C:\AZ-204-2026\04_Azure_Cosmos_DB\cosmosdb\bin\Debug\net8.0\cosmosdb.dll

    Build succeeded.
        0 Warning(s)
        0 Error(s)

    Time Elapsed 00:00:11.44


16) Corremos la aplicacion

    ###    dot net run


17) Nos crea estos mensajes en consola

    Created or retrieved database: myDatabase
    Created or retrieved container: myContainer
    Created item with ID: 60dd3f04-6e13-4539-86f2-1fbefb8c230d
    Request charge: 6.29 RUs

18) Nos vamos al portal de azure

    Buscamos en resource group
    ### myResourceGroupCosmosDB

19) Ahi nos aparece

    ###    cosmosexercise10126 que es de tipo Azure Cosmos DB Account

20) Del lado izquierdo seleccionamos 

    ### myDatabase
        
    un nivel mas a dentro
    ### myContainer

21) Seleccionamos Items

    nos muestra un registro que agregamos previamente en la aplicacion



    {
    "id": "60dd3f04-6e13-4539-86f2-1fbefb8c230d",
    "name": "Sample Item",
    "description": "This is a sample item in my Azure Cosmos DB exercise.",
    "_rid": "-uYWAOhEQ7AA==",
    "_self": "dbs/-uYWAA==/coll /docs/-uYWAOhEQ7wBAAAAAAAAAA==/",
    "_etag": "\"260fabe5-0000-0200df5bb0000\"",
    "_attachments": "attachments/",
    "_ts": 1769862587
}


22) Limpiamos los recursos