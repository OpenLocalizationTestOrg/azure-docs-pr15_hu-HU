<properties
    pageTitle="Első lépések a táblázat tárhely és a Visual Studio csatlakoztatott szolgáltatások (ASP.NET) |} Microsoft Azure"
    description="Hogyan kezdjek hozzá az Azure táblatárolóhoz ASP.NET projektben, a Visual Studióban, miután részletesen a tárterület a Visual Studio segítségével a csatlakoztatott szolgáltatások"
    services="storage"
    documentationCenter=""
    authors="TomArcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="storage"
    ms.workload="web"
    ms.tgt_pltfrm="vs-getting-started"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/18/2016"
    ms.author="tarcher"/>

# <a name="get-started-with-table-storage-and-visual-studio-connected-services-aspnet"></a>Első lépések a táblázat tárhely és a Visual Studio csatlakoztatott szolgáltatások (ASP.NET)

[AZURE.INCLUDE [storage-try-azure-tools-tables](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>– Áttekintés
Ez a cikk ismerteti, hogyan kezdjek hozzá az Azure táblatárolóhoz a Visual Studióban, után a korábban létrehozott vagy ASP.NET projektben Azure tároló fiók hivatkozott a **Csatlakoztatott szolgáltatások hozzáadása** a Visual Studio párbeszédpanel segítségével. Ez a cikk bemutatja, hogyan általános feladatok elvégzéséhez az Azure-táblázatok, beleértve a létrehozás és töröl egy táblát, valamint táblázat szervezetek használata. A minták írt C\# kód és a [Microsoft Azure tároló ügyfél tár a .NET rendszerhez](https://msdn.microsoft.com/library/azure/dn261237.aspx). Azure használatával kapcsolatos további általános tudnivalókat a táblázat tárhely, lásd: az [első lépések az Azure táblatárolóhoz .NET használatával](storage-dotnet-how-to-use-tables.md).

Azure táblatárolóhoz lehetővé teszi, hogy strukturált adatok nagy mennyiségű tárolhatja. A szolgáltatás nem egy NoSQL adattárhoz, hogy elfogadja a hitelesített hívásait belüli és kívüli Azure a felhőben. Azure táblák ideálisak strukturált, nem relációs adatok tárolására szolgáló.


## <a name="access-tables-in-code"></a>Access-tábláit a kódot.

1. Ellenőrizze, hogy a névtér nyilatkozatokat tetején látható a C# fájlt tartalmazza e **használata** kimutatásokban.

         using Microsoft.Azure;
         using Microsoft.WindowsAzure.Storage;
         using Microsoft.WindowsAzure.Storage.Auth;
         using Microsoft.WindowsAzure.Storage.Table;

2. Egy **CloudStorageAccount** objektum, amely a tárhely fiókadatok beolvasása. Az alábbi kód használatával beszerzése a a tárhely kapcsolati karakterlánc és a tárhely fiók adatait a Azure szolgáltatás beállításai.

         CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));

    **Megjegyzés:** - összes elé a kódot a fenti kód használata az alábbi példák.

3. A táblázat objektumok a tárterület-fiókjában hivatkozni szeretne egy **CloudTableClient** objektum kaphat.  

        // Create the table client.
        CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

4. Kap egy **CloudTable** hivatkozás objektum egy adott tábla és a személyek hivatkozást.

        // Get a reference to a table named "peopleTable"
        CloudTable table = tableClient.GetTableReference("peopleTable");

## <a name="create-a-table-in-code"></a>Táblázat létrehozása a kódot.

Az Azure táblázatot létrehozni, csak adja hozzá hívást kezdeményez **CreateIfNotExistsAsync()** az előző kódot.

    // Create the CloudTable if it does not exist
    await table.CreateIfNotExistsAsync();

## <a name="add-an-entity-to-a-table"></a>Egy személy hozzáadása táblához

Egy személy hozzáadása egy táblához, amely meghatározza a szervezet tulajdonságainak osztály hozzon létre. A következő kódrészlet egy személy osztály első Vevőnév használja, mint a sor billentyűt, és a vezetéknevet partíciót kulcsként **CustomerEntity** nevű határozza meg.

    public class CustomerEntity : TableEntity
    {
        public CustomerEntity(string lastName, string firstName)
        {
            this.PartitionKey = lastName;
            this.RowKey = firstName;
        }

        public CustomerEntity() { }

        public string Email { get; set; }

        public string PhoneNumber { get; set; }
    }

Személyek érintő táblázat műveletek vannak kész használata a **CloudTable** objektum meg a korábban létrehozott "Kód táblák az Access." A **TableOperation** objektum helyén kell elvégezni a műveletet. A következő kódot példa bemutatja, hogyan hozhat létre egy **CloudTable** objektum és egy **CustomerEntity** objektum. A művelet előkészítése, egy **TableOperation** létrejön az ügyfél entitás beszúrása a táblázatba. Végezetül CloudTable.ExecuteAsync hívja fel a művelet végrehajtása.

    // Create a new customer entity.
    CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
    customer1.Email = "Walter@contoso.com";
    customer1.PhoneNumber = "425-555-0101";

    // Create the TableOperation that inserts the customer entity.
    TableOperation insertOperation = TableOperation.Insert(customer1);

    // Execute the insert operation.
    await peopleTable.ExecuteAsync(insertOperation);

## <a name="insert-a-batch-of-entities"></a>A köteg szervezetek beszúrása

Több entitás táblába az egyetlen írási művelet beszúrhat. A következő példa két személy objektumot ("Kovács" és "Zsolt Kovács") hoz létre, ezek a Beszúrás módszerrel **TableBatchOperation** -objektum, és elindítja a műveletet **CloudTable.ExecuteBatchAsync**hívja fel.

    // Create the batch operation.
    TableBatchOperation batchOperation = new TableBatchOperation();

    // Create a customer entity and add it to the table.
    CustomerEntity customer1 = new CustomerEntity("Smith", "Jeff");
    customer1.Email = "Jeff@contoso.com";
    customer1.PhoneNumber = "425-555-0104";

    // Create another customer entity and add it to the table.
    CustomerEntity customer2 = new CustomerEntity("Smith", "Ben");
    customer2.Email = "Ben@contoso.com";
    customer2.PhoneNumber = "425-555-0102";

    // Add both customer entities to the batch insert operation.
    batchOperation.Insert(customer1);
    batchOperation.Insert(customer2);

    // Execute the batch operation.
    await peopleTable.ExecuteBatchAsync(batchOperation);

## <a name="get-all-of-the-entities-in-a-partition"></a>Felvenni az összes a személyek felvétele
A lekérdezés az összes partíciót vállalkozások táblázat, használja a **TableQuery** objektumot. A következő példa adja meg a személyek hol "Kovács"-e a partíciót kulcs szűrőt. Ebben a példában a mezőket a lekérdezés eredményében a konzolhoz minden személy nyomtatja.

    // Construct the query operation for all customer entities where PartitionKey="Smith".
    TableQuery<CustomerEntity> query = new TableQuery<CustomerEntity>().Where(TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Smith"));

    // Print the fields for each customer.
    TableContinuationToken token = null;
    do
    {
        TableQuerySegment<CustomerEntity>
        resultSegment = await peopleTable.ExecuteQuerySegmentedAsync(query, token);
        token = resultSegment.ContinuationToken;

        foreach (CustomerEntity entity in resultSegment.Results)
        {
        Console.WriteLine("{0}, {1}\t{2}\t{3}", entity.PartitionKey, entity.RowKey,
        entity.Email, entity.PhoneNumber);
        }
        } while (token != null);

        return View();


## <a name="get-a-single-entity"></a>Egyetlen egységet beszerzése
Egy adott személy kéréséhez lekérdezés is írhat. A következő kódrészlet egy **TableOperation** objektum "Zsolt Kovács" nevű ügyfél megadásához használja. Ez a módszer csak egy szervezet adja eredményül, hanem egy gyűjtemény és a visszaadott érték a **TableResult.Result** egy **CustomerEntity** objektumot. Ez leggyorsabban egy entitás beolvasni a táblázat szolgáltatásból való partíciót vagy a sor megadása a lekérdezés.

    // Create a retrieve operation that takes a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute the retrieve operation.
    TableResult retrievedResult = await peopleTable.ExecuteAsync(retrieveOperation);
    
    // Print the phone number of the result.
    if (retrievedResult.Result != null)
        Console.WriteLine(((CustomerEntity)retrievedResult.Result).PhoneNumber);
    else
       Console.WriteLine("The phone number could not be retrieved.");

## <a name="delete-an-entity"></a>Entitás törlése
Miután megtalálta entitás törölheti. A következő kódot: "Zsolt Kovács" nevű ügyfél entitás keres, és ha úgy találja meg, hogy törlése.

    // Create a retrieve operation that expects a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute the operation.
    TableResult retrievedResult = peopleTable.Execute(retrieveOperation);

    // Assign the result to a CustomerEntity object.
    CustomerEntity deleteEntity = (CustomerEntity)retrievedResult.Result;

    // Create the Delete TableOperation and then execute it.
    if (deleteEntity != null)
    {
       TableOperation deleteOperation = TableOperation.Delete(deleteEntity);

       // Execute the operation.
       await peopleTable.ExecuteAsync(deleteOperation);

       Console.WriteLine("Entity deleted.");
    }

    else
       Console.WriteLine("Couldn't delete the entity.");

## <a name="next-steps"></a>Következő lépések

[AZURE.INCLUDE [vs-storage-dotnet-tables-next-steps](../../includes/vs-storage-dotnet-tables-next-steps.md)]
