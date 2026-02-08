1) Creamos un folder 
   ### mkdir azstor

2) Entramos en la carpeta 
   ### cd azstor

3) Creamos un proyecto de consola
   ### dotnet new console


4) Instalamos estos 2 paquetes

    ### dotnet add package Azure.Storage.Blobs
    ### dotnet add package Azure.Identity

5) Creamos un subfolder
    ### mkdir data

6) Corremos esto, para que abra el archivo
    ### code Program.cs

7) Reemplazamos su contenido por esto

        using Azure.Storage.Blobs;
        using Azure.Storage.Blobs.Models;
        using Azure.Identity;

        Console.WriteLine("Azure Blob Storage exercise\n");

        // Create a DefaultAzureCredentialOptions object to configure the DefaultAzureCredential
        DefaultAzureCredentialOptions options = new()
        {
            ExcludeEnvironmentCredential = true,
            ExcludeManagedIdentityCredential = true
        };

        // Run the examples asynchronously, wait for the results before proceeding
        await ProcessAsync();

        Console.WriteLine("\nPress enter to exit the sample application.");
        Console.ReadLine();

        async Task ProcessAsync()
        {
            // CREATE A BLOB STORAGE CLIENT
            


            // CREATE A CONTAINER
            


            // CREATE A LOCAL FILE FOR UPLOAD TO BLOB STORAGE
            


            // UPLOAD THE FILE TO BLOB STORAGE
            


            // LIST BLOBS IN THE CONTAINER
            


            // DOWNLOAD THE BLOB TO A LOCAL FILE
            

        }



8) Debajo de // CREATE A BLOB STORAGE CLIENT 
    vamos a pegar lo siguiente

      string connectionString = 
        "DefaultEndpointsProtocol=https;AccountName=storageacct8173;AccountKey=9Fixii5KTT X8ekDSRhodvD/sur/cOg90rRnj+bDIdu0nAkML+AStdGCNqg==;EndpointSuffix=core.windows.net"; // Replace with your storage account name
 
        BlobServiceClient blobServiceClient = new BlobServiceClient(connectionString);
    
    ###  Necesitamos cambiar YOUR_ACCOUNT_NAME

    Para eso entramos al portal de azure

    ### En la pantalla principal de Overview
    ### Seleccionamos Resource Groups
            Luego seleccionamos el recurso para este ejercicio 
        En este caso es myResourceGroupABN2026

    ### Haz clic en el nombre de la cuenta de almacenamiento, en este caso, storageacct8173.

    ### Dentro de la cuenta de almacenamiento, en el menú lateral izquierdo, busca la opción Access keys o Claves de acceso

    ### Buscamos del lado izquierdo
        Security + Networking

    ### Y hacemos click en Access Keys
    
        Copia el connection string que necesites para tu aplicación o uso.

    ### Pegamos la informacion en el connection String quedando esa parte de la siguiente forma


    // Copy the connection string from the portal in the variable below.
    string storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=msaabnall;AccountKey=eSYF7mxrdYG7XfeaVbTJ6Q3uwHsQ==;EndpointSuffix=core.windows.net";


9) Guardamos cambios

    En la parte de Container pegamos esto

    string containerName = "wtblob" + Guid.NewGuid().ToString();

    // Create the container and return a container client object
    Console.WriteLine("Creating container: " + containerName);
    BlobContainerClient containerClient = 
        await blobServiceClient.CreateBlobContainerAsync(containerName);

    // Check if the container was created successfully
    if (containerClient != null)
    {
        Console.WriteLine("Container created successfully, press 'Enter' to continue.");
        Console.ReadLine();
    }
    else
    {
        Console.WriteLine("Failed to create the container, exiting program.");
        return;
    }

10) En BlogStorage pegamos lo siguiente


    // Create a local file in the ./data/ directory for uploading and downloading
    Console.WriteLine("Creating a local file for upload to Blob storage...");
    string localPath = "./data/";
    string fileName = "wtfile" + Guid.NewGuid().ToString() + ".txt";
    string localFilePath = Path.Combine(localPath, fileName);

    // Write text to the file
    await File.WriteAllTextAsync(localFilePath, "Hello, World!");
    Console.WriteLine("Local file created, press 'Enter' to continue.");
    Console.ReadLine();

11) En la parte de Upload the File to Blob Storage


        // Get a reference to the blob and upload the file
        BlobClient blobClient = containerClient.GetBlobClient(fileName);

        Console.WriteLine("Uploading to Blob storage as blob:\n\t {0}", blobClient.Uri);

        // Open the file and upload its data
        using (FileStream uploadFileStream = File.OpenRead(localFilePath))
        {
            await blobClient.UploadAsync(uploadFileStream);
            uploadFileStream.Close();
        }

        // Verify if the file was uploaded successfully
        bool blobExists = await blobClient.ExistsAsync();
        if (blobExists)
        {
            Console.WriteLine("File uploaded successfully, press 'Enter' to continue.");
            Console.ReadLine();
        }
        else
        {
            Console.WriteLine("File upload failed, exiting program..");
            return;
        }


12) En Download pegamos esto

        // Adds the string "DOWNLOADED" before the .txt extension so it doesn't 
        // overwrite the original file

        string downloadFilePath = localFilePath.Replace(".txt", "DOWNLOADED.txt");

        Console.WriteLine("Downloading blob to: {0}", downloadFilePath);

        // Download the blob's contents and save it to a file
        BlobDownloadInfo download = await blobClient.DownloadAsync();

        using (FileStream downloadFileStream = File.OpenWrite(downloadFilePath))
        {
            await download.Content.CopyToAsync(downloadFileStream);
        }

        Console.WriteLine("Blob downloaded successfully to: {0}", downloadFilePath);



13) Luego abrimos Azure Cloud Shell (Bash) en las opciones donde se encuentra la terminal

    ### az login

    Nos aparece el prompt y seleccionamos nuestra cuenta de correo

    Si nos aparece este error

    Authentication failed against tenant d39d348d-12b6-42d6-89be-5713c7bf86e9 'Directorio predeterminado': (pii). Status: Response_Status.Status_InteractionRequired, Error code: 3399614467, Tag: 558133255
    If you need to access subscriptions in the following tenants, please use `az login --tenant TENANT_ID`.
    d39d348d-12b6-42d6-89be-5713c7bf86e9 'Directorio predeterminado'
    No subscriptions found for x@hotmail.com.

14) Si falla corremos este 

    az login --use-device-code

    esto nos muestra este mensaje

    ### To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code ABKGEISIS to authenticate.
 
    Hacemos click a este link e ingresamos el codigo que nos muestra osea ABKGEISIS
    
    Damos aceptar listo
    
15) dotnet run
Azure Blob Storage exercise

Creating container: wtblob243d2867-fdda-9fac-3b28f1291
Container created successfully, press 'Enter' to continue.

16) Regresamos al portal de Azure

    Buscamos    Pay-As-You-Go | Resource groups > myResourceGroup > storageacct8173


    Ya aqui del lado izquierdo buscamos
    ###    Data Storage
    ###    Seleccionamos Container

17) Ahi nos aparece un container 
    wtblob243d2867-fdda-4d13-9fac-3b28f20

18) Nos regresamos a la terminal
    ### Damos un enter para que suba el archivo

19) Nos regresamos al portal ya vemos que esta el archivo que creamos

    ### Nos creo este archivo txt

20) Quedando el archivo final de la siguiente manera


            using Azure.Storage.Blobs;
            using Azure.Storage.Blobs.Models;
            using Azure.Identity;

            Console.WriteLine("Azure Blob Storage exercise\n");

            // Create a DefaultAzureCredentialOptions object to configure the DefaultAzureCredential
            DefaultAzureCredentialOptions options = new()
            {
                ExcludeEnvironmentCredential = true,
                ExcludeManagedIdentityCredential = true
            };

            // Run the examples asynchronously, wait for the results before proceeding
            await ProcessAsync();

            Console.WriteLine("\nPress enter to exit the sample application.");
            Console.ReadLine();

            async Task ProcessAsync()
            {
                // CREATE A BLOB STORAGE CLIENT
                // Create a credential using DefaultAzureCredential with configured options
                    string connectionString = 
                    "DefaultEndpointsProtocol=https;AccountName=storageacct8173;AccountKey=9FiCdZs9Qii5KTTGlTbXMLviX8ekDSRhodvD/sur/cOg90rRnj+bDIdu0nAkML+AStdGCNqg==;EndpointSuffix=core.windows.net"; // Replace with your storage account name
            
                    BlobServiceClient blobServiceClient = new BlobServiceClient(connectionString);
                

                    // CREATE A CONTAINER
                    // Create a unique name for the container
                    string containerName = "wtblob" + Guid.NewGuid().ToString();

                    // Create the container and return a container client object
                    Console.WriteLine("Creating container: " + containerName);
                    BlobContainerClient containerClient = 
                        await blobServiceClient.CreateBlobContainerAsync(containerName);

                    // Check if the container was created successfully
                    if (containerClient != null)
                    {
                        Console.WriteLine("Container created successfully, press 'Enter' to continue.");
                        Console.ReadLine();
                    }
                    else
                    {
                        Console.WriteLine("Failed to create the container, exiting program.");
                        return;
                    }
            
                    // CREATE A LOCAL FILE FOR UPLOAD TO BLOB STORAGE
                    // Create a local file in the ./data/ directory for uploading and downloading
                    Console.WriteLine("Creating a local file for upload to Blob storage...");
                    string localPath = "./data/";
                    string fileName = "wtfile" + Guid.NewGuid().ToString() + ".txt";
                    string localFilePath = Path.Combine(localPath, fileName);

                    // Write text to the file
                    await File.WriteAllTextAsync(localFilePath, "Hello, World!");
                    Console.WriteLine("Local file created, press 'Enter' to continue.");
                    Console.ReadLine();


                // UPLOAD THE FILE TO BLOB STORAGE
                // Get a reference to the blob and upload the file
                    BlobClient blobClient = containerClient.GetBlobClient(fileName);

                    Console.WriteLine("Uploading to Blob storage as blob:\n\t {0}", blobClient.Uri);

                    // Open the file and upload its data
                    using (FileStream uploadFileStream = File.OpenRead(localFilePath))
                    {
                        await blobClient.UploadAsync(uploadFileStream);
                        uploadFileStream.Close();
                    }

                    // Verify if the file was uploaded successfully
                    bool blobExists = await blobClient.ExistsAsync();
                    if (blobExists)
                    {
                        Console.WriteLine("File uploaded successfully, press 'Enter' to continue.");
                        Console.ReadLine();
                    }
                    else
                    {
                        Console.WriteLine("File upload failed, exiting program..");
                        return;
                    }




                    // LIST BLOBS IN THE CONTAINER
                    Console.WriteLine("Listing blobs in container...");
                    await foreach (BlobItem blobItem in containerClient.GetBlobsAsync())
                    {
                        Console.WriteLine("\t" + blobItem.Name);
                    }

                    Console.WriteLine("Press 'Enter' to continue.");
                    Console.ReadLine();


                    // DOWNLOAD THE BLOB TO A LOCAL FILE
                    // Adds the string "DOWNLOADED" before the .txt extension so it doesn't 
                    // overwrite the original file

                    string downloadFilePath = localFilePath.Replace(".txt", "DOWNLOADED.txt");

                    Console.WriteLine("Downloading blob to: {0}", downloadFilePath);

                    // Download the blob's contents and save it to a file
                    BlobDownloadInfo download = await blobClient.DownloadAsync();

                    using (FileStream downloadFileStream = File.OpenWrite(downloadFilePath))
                    {
                        await download.Content.CopyToAsync(downloadFileStream);
                    }

                    Console.WriteLine("Blob downloaded successfully to: {0}", downloadFilePath);

            }
