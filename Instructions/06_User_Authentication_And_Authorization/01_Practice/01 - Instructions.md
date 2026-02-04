
<details> 
    <summary>Implement interactive authentication with MSAL.NET
In this exercise, you register an application in Microsoft Entra ID, then create a .NET console application that uses MSAL.NET to perform interactive authentication and acquire an access token for Microsoft Graph. You learn how to configure authentication scopes, handle user consent, and see how tokens are cached for subsequent runs.</summary>

Tasks performed in this exercise:

1) Register an application with the Microsoft identity platform
2) Create a .NET console app that implements the PublicClientApplicationBuilder class to configure authentication.
3) Acquire a token interactively using the user.read Microsoft Graph permission.
</details>



1) Entramos al portal

    https://portal.azure.com/

2) En la pagina principal, en el cuadro de busquedas.
   ### Buscamos App Registrations y damos click

3) Damos click a + New Registration

        Field	                        Value

        Name	                        Enter myMsalApplication

        Supported account types	        Seleccionamos   

                                        1) Accounts in this organizational directory only (Directorio predeterminado only - Single tenant)
        Redirect URI (optional)	        Select Public client/native (mobile & desktop) 
                                        e ingresamos http://localhost en el cuadro de texto adjunto
       
    #### Esto es importante agregarlo http://localhost

        Damos click en                  Registrar


4) Nos genera este registro    

        Display name:              myMsalApplication
        Application (client) ID:   6c6ae34a-99e7-e8aa66187d6a
        Object ID:                 ab804f4f-a3c3-345ef55c440c
        Directory (tenant) ID:     d39d348d-89be-5713c7bf86e9

