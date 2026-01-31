

1) F1 - Abre Command Palette
    Azure Functions: Create New Project
        Select .NET 8.0 Isolated
        Select HTTP trigger
        Provide a function name
            Debe empezar con Mayuscula para respetar la convencion de C#
        Provide a namespace    
               Ebazfunctionnamespace.Function
        Authorization level
               Anonymous


2) Esto crea este codigo

using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Azure.Functions.Worker;
using Microsoft.Extensions.Logging;

namespace My.Function;

public class HttpExample
{
    private readonly ILogger<HttpExample> _logger;

    public HttpExample(ILogger<HttpExample> logger)
    {
        _logger = logger;
    }

    [Function("HttpExample")]
    public IActionResult Run([HttpTrigger(AuthorizationLevel.Anonymous, "get", "post")] HttpRequest req)
    {
        _logger.LogInformation("C# HTTP trigger function processed a request.");
        return new OkObjectResult("Welcome to Azure Functions!");
    }
}

3) Ctrl . 
    Abrir terminal

4) Presionar F5
    esto corre la aplicacion

5) Va a aparecer una ventana que dice

    #:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:

        Visual Studio Code
    --------------------------------------------------------------------------------------------------------------------

        Azure Functions requieres a storage account to be configured

        Connect Azure Storage Account       Use Local Emulator      Skip for now        Cancel
    #:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:

    Seleccionamos Skip for now


6) Esto nos genera un resultado en la consola un url en este caso local con un puerto, tiene la ruta

    #:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:

    Executing task: func host start
    HttpExample: [GET,POST] http://localhost:7071/api/HttpExample

    #:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:#:

7) Abrimos el link

8) Debemos ver este mensaje 

    Welcome to Azure Functions!




9) Del Lado Izquierdo seleccionamos el Simbolo de A, en la columna de Activity Bar

10) Buscamos Workspace (Local Project)
    Seleccionamos la carpeta
    
    
    ### Se despliega Functions
    ### Sobre la funcion damos boton derecho 
            Damos click en Ejecute Function Now
                Nos aparece en la barra superior (Command Palette) : {"name" : "Azure"} 
    ###            Damos enter

            

11) Se ejecuta un toast que nos muestra esto

Executed function "HttpExample". Response: "Welcome to Azure Functions!"

