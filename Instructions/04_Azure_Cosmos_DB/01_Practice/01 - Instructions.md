Create an Azure Cosmos DB account
Add a database and a container
Add data to your database

1) Entramos al portal

    https://portal.azure.com/

2) En la ventana de inicio

    ###  Hacemos click en el primer Icono de + Create a Resource

3) En el buscador buscamos 
    ###  Azure Cosmos DB
    ###  Seleccionamos el primero de la lista Azure Cosmos DB

4) Eso nos manda a otra pantalla que dice

    ###  Azure Cosbos DB

    Subscription            Plan
    Pay-As-You-Go           Azure Cosmos DB

    ###  Hacemos click en Create


5) Nos aparecen 2 opciones

    Azure Cosmos DB for NoSQL                
    Azure Cosmos DB's core, or native API for working with documents. Supports fast, flexible development with familiar SQL query language and client libraries for .NET, JavaScript, Python, and Java.

    Azure DocumentDB (with MongoDB compatibility)
    Fully managed database service for apps written for MongoDB. Recommended if you have existing MongoDB workloads that you plan to migrate to Azure Cosmos DB.

    ### Hacemos click en la primera en Azure Cosmos DB For NoSQL

6)  Nos aparece una pantalla con muchas opciones

    ###    Workload Type
           Seleccionamos Learning ya que es solo para pruebas 
               Learning


    ###    Subscription        
            Pay-As-You-Go

    ###     Resource Group
            Creamos uno nuevo que se llame az204-cosmos-rg2026
 
    ###     Account Name
            Aqui le asignamos el nombre de lo que vendria siendo el nombre este proyecto
               proyecto-cosmosdb-2026
   
    ###     Availability Zones
                Disable

    ###     Location
                (US) West US 2


    ###     Capacity mode

        ✅ Resumen rápido
        Modo	                 Cómo se cobra	    Escalado	            Ideal para
        Provisioned Throughput	 Por hora, fijo	    Manual o autoscale	 Tráfico constante o predecible
        Serverless	             Solo por consumo	Automático	         Tráfico intermitente o impredeci

        Seleccionamos Serverless

    ###     Select Review + create.

7)  Nos aparece una pantalla con este mensaje

    ###     Your deployment is complete

    Damos click en go to resource

8)  Nos aparecen en la pantalla una serie de botones

    ###     + Add Container     Refresh     Move        Open in VS Code     Data Explorer   Enable geo-redundancy       Delete      Feedback

    ###     Seleccionamos 
            Add Container

9)  Esto nos manda a otra pantalla

    ###     Aqui nos aparece del lado izquierdo un Boton + New y que despliega 2 opciones

            + New Container
            + New Database

            Concepto        	Equivale a                      	Contiene
            Database	        Base de datos tradicional	        Uno o varios contenedores
            Container	        Tabla o colección de documentos	    Los datos (documentos JSON)

    ###     Creamos una base de datos primero y le ponemos
                    ToDoList


 10)         Luego creamos un contenedor
    ###      Seleccionamos Use Existing
                    El dropdown nos despliega una opcion ToDoList (que fue la que creamos anteriormente
    ###      ToDoList

    ###      En container id 
                    Escribimos Items
    ###      En Partition key: Ingresamos /category. Los ejemplos usados en este demo, usan esa partition key


11) Esto nos crea estos elementos

    Home
    ToDoList
        >   Items

12) En la parte superior nos aparece New Item
    ###         Hacemos click en New Item

    Agregamos este ejemplo le damos Save

       {
            "id": "1",
            "category": "personal",
            "name": "groceries",
            "description": "Pick up apples and strawberries.",
            "isComplete": false
       }

13) Creamos otro ejemplo con otro id

       {
            "id": "2",
            "category": "personal",
            "name": "groceries",
            "description": "Pick up bread",
            "isComplete": true
       }

14) Aqui termina el ejercicio 