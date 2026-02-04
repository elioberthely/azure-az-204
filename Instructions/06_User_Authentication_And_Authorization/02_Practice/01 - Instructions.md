
<details> 
    <summary>In this exercise, you create a .NET app to authenticate with Microsoft Entra ID and request an access token, then call the Microsoft Graph API to retrieve and display your user profile information. You learn how to configure permissions and interact with Microsoft Graph from your application.</summary>

Tasks performed in this exercise:

1) Register an application with the Microsoft identity platform
2) Create a .NET console application that implements interactive authentication, and uses the GraphServiceClient class to retrieve user profile information.
</details>




1) Entramos al portal

    https://portal.azure.com/

2) En la pagina principal, en el cuadro de busquedas.
   ### Buscamos App Registrations y damos click

3) Damos click a + New Registration

        Field	                        Value

        Name	                        Enter myGraphApplication

        Supported account types	        Seleccionamos   

                                        1) Accounts in this organizational directory only (Directorio predeterminado only - Single tenant)
        Redirect URI (optional)	        Select Public client/native (mobile & desktop) 
                                        e ingresamos http://localhost en el cuadro de texto adjunto
    
    #### Esto es importante agregarlo http://localhost
    
        Damos click en                  Registrar


4) Nos genera este registro    

        Display name:              myGraphApplication
        Application (client) ID:   6c6ae34a-99e7-e8aa66187d6a
        Object ID:                 ab804f4f-a3c3-345ef55c440c
        Directory (tenant) ID:     d39d348d-89be-5713c7bf86e9

