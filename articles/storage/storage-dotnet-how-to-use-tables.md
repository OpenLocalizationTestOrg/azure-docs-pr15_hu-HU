<properties
    pageTitle="Első lépések az Azure táblatárolóhoz .NET segítségével |} Microsoft Azure"
    description="Strukturált adatokat tárolja a felhőben Azure táblatárolóhoz, egy NoSQL adattár használatával."
    services="storage"
    documentationCenter=".net"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/18/2016"
    ms.author="tamram"/>


# <a name="get-started-with-azure-table-storage-using-net"></a>Azure táblatárolóhoz .NET használata – első lépések

[AZURE.INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-tables](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>– Áttekintés

Azure táblatárolóhoz a szolgáltatás, amely strukturált NoSQL adatokat tárolja a felhőben. Táblatároló a schemaless Design egy kulcsot/attribútum áruházból. Mivel táblatároló schemaless, akkor is alkalmassá teheti az adatok, az alkalmazás evolve igényeihez egyszerűen. Hozzáférés az adatokhoz gyors és az alkalmazás az összes különféle költséghatékony. A művelet rendszerint költség jelentősen kisebb, mint a hagyományos SQL-hasonló mennyiségű adattal táblatároló.

Rugalmas adatkészleteket, például a webalkalmazások, címjegyzékek, eszköz adatait és más típusú metaadatokat a szolgáltatáshoz felhasználói adatok tárolására táblatároló is használhatja. Tetszőleges számú személyek is tárolnia egy táblában, és egy tárterület-fiókot is tartalmazhatnak tetszőleges számú táblázatok, a tárhely fiók kapacitás határértékén felfelé.

### <a name="about-this-tutorial"></a>Ebben az oktatóanyagban kapcsolatban

Ebből az oktatóanyagból megtudhatja, hogy miként néhány gyakori alkalmazási területek használatával Azure táblatárolóhoz, létrehozása és töröl egy táblát, és beszúrása, módosítása, törlése és táblázat-adatok lekérdezése a .NET kódírás.

**Becsült időt vesz igénybe:** 45 perc

**Prerequisities:**

- [Microsoft Visual Studio](https://www.visualstudio.com/en-us/visual-studio-homepage-vs.aspx)
- [Azure tároló ügyfél tár a .NET rendszerhez](https://www.nuget.org/packages/WindowsAzure.Storage/)
- [A .NET rendszerhez Azure Konfigurációkezelő](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/)
- [Azure tárterület-fiók](storage-create-storage-account.md#create-a-storage-account)

[AZURE.INCLUDE [storage-dotnet-client-library-version-include](../../includes/storage-dotnet-client-library-version-include.md)]

### <a name="more-samples"></a>További minták

További példákat táblatároló használ olvassa el a [Azure Táblatárolóhoz a .NET – első lépések](https://azure.microsoft.com/documentation/samples/storage-table-dotnet-getting-started/)című témakört. A minta alkalmazás letöltése és indítsa el, vagy keresse meg a GitHub kódot.


[AZURE.INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

[AZURE.INCLUDE [storage-development-environment-include](../../includes/storage-development-environment-include.md)]

### <a name="add-namespace-declarations"></a>Névtér deklarációs hozzáadása

Adja hozzá a következő `using` kimutatások tetején a `program.cs` fájl:

    using Microsoft.Azure; // Namespace for CloudConfigurationManager
    using Microsoft.WindowsAzure.Storage; // Namespace for CloudStorageAccount
    using Microsoft.WindowsAzure.Storage.Table; // Namespace for Table storage types

### <a name="parse-the-connection-string"></a>A kapcsolati karakterlánc elemzése

[AZURE.INCLUDE [storage-cloud-configuration-manager-include](../../includes/storage-cloud-configuration-manager-include.md)]

### <a name="create-the-table-service-client"></a>A táblázat szolgáltatás ügyfél létrehozása

A **CloudTableClient** osztály beolvasni a táblázatok és táblatárolóban lévő személyek teszi lehetővé. Az alábbiakban az egyik módja a szolgáltatási ügyfelet létrehozása:

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

Most már készen áll, amely beolvassa az adatokat, és adatot ír táblatárolóhoz kódírás.

## <a name="create-a-table"></a>Táblázat létrehozása

Ez a példa bemutatja, hogyan hozzon létre egy táblázatot, ha még nem létezik:

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Retrieve a reference to the table.
    CloudTable table = tableClient.GetTableReference("people");

    // Create the table if it doesn't exist.
    table.CreateIfNotExists();

## <a name="add-an-entity-to-a-table"></a>Egy személy hozzáadása táblához

Személyek hozzárendelése a C\# **TableEntity**származó objektumok egy egyéni osztály használatával. Egy személy hozzáadása egy táblához, hozzon létre, amely meghatározza a szervezet tulajdonságainak osztály. A következő kódot határozza meg, hogy olyan személy osztály, amely a sor billentyűt, és a vezetéknevet partíciót kulcsként a vevő első nevét használja. Közös egy személy partíciót és a sor kulcs egyedileg azonosító a szervezet a táblázatban. Személyek ugyanazzal a partíciót kulccsal gyorsabb, mint a másik partíciót billentyűkkel lekérdezhető, de használata különböző partíciót billentyűk lehetővé teszi a párhuzamos műveletek nagyobb méretezhetőség.  Minden olyan tulajdonságot, amely a táblázat szolgáltatásban kell tárolni, a tulajdonság kell a nyilvános tulajdonság, amelyek mindkét közzététele támogatott típusú `get` és `set`.
Emellett a személy típusú *kell* jelenítik meg a paraméterek nélküli konstruktornak.

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

Táblázat, amely magában foglalja a szervezetek műveleteket a "Táblázat létrehozása" szakaszban korábban létrehozott **CloudTable** objektum keresztül. Végrehajtandó művelet egy **TableOperation** objektum jelöli.  Az alábbi példa mutatja az **CloudTable** objektumra, majd a **CustomerEntity** objektum létrehozását.  A művelet előkészítése, egy **TableOperation** objektum beszúrásához az ügyfél entitás a táblázatba jön létre.  Végezetül **CloudTable.Execute**hívja fel a művelet végrehajtása.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable object that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Create a new customer entity.
    CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
    customer1.Email = "Walter@contoso.com";
    customer1.PhoneNumber = "425-555-0101";

    // Create the TableOperation object that inserts the customer entity.
    TableOperation insertOperation = TableOperation.Insert(customer1);

    // Execute the insert operation.
    table.Execute(insertOperation);

## <a name="insert-a-batch-of-entities"></a>A köteg szervezetek beszúrása

Az írást egy táblába beszúrhat egy köteg szervezetek. Néhány egyéb megjegyzések köteg műveletek:

-  Frissítések végezhet törli, és szúrja be az azonos kötegben művelet.
-  Egy egyetlen köteg művelet legfeljebb 100 személyek is tartalmazhat.
-  Egyetlen köteg művelet összes entitás ugyanazt a partíciót kulcsot kell rendelkeznie.
-  Ajánlatos egy lekérdezés egy köteg művelet végrehajtása, amíg a köteg az egyetlen művelet kell lennie.

<!-- -->
A következő példa két személy objektumot hoz létre, és hozzáadja **TableBatchOperation** látni szeretné a **beszúrása** módszerrel. Ezután **CloudTable.Execute** neve végrehajtani a műveletet.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable object that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

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
    table.ExecuteBatch(batchOperation);

## <a name="retrieve-all-entities-in-a-partition"></a>Az összes partíciót vállalkozások beolvasása

A lekérdezés minden entitás partíciót a táblázat, használja a **TableQuery** objektum.
A következő példa adja meg a személyek hol "Kovács"-e a partíciót kulcs szűrőt. Ebben a példában a mezőket a lekérdezés eredményében a konzolhoz minden személy nyomtatja.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable object that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Construct the query operation for all customer entities where PartitionKey="Smith".
    TableQuery<CustomerEntity> query = new TableQuery<CustomerEntity>().Where(TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Smith"));

    // Print the fields for each customer.
    foreach (CustomerEntity entity in table.ExecuteQuery(query))
    {
        Console.WriteLine("{0}, {1}\t{2}\t{3}", entity.PartitionKey, entity.RowKey,
            entity.Email, entity.PhoneNumber);
    }

## <a name="retrieve-a-range-of-entities-in-a-partition"></a>Személyek felvétele egy cellatartomány beolvasása

Ha nem szeretné a lekérdezés partíciót az összes entitás, megadhatja a tartomány a partíciót kulcs szűrő sor kulcs szűrővel kombinálva. A következő példa két szűrő használatával egyben az összes entitás partíciót "Kovács", ahol a sor kulcs (Utónév) verziónál az ábécé "E" betűvel kezdődik, és majd kinyomtatja a lekérdezés eredményét.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable object that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Create the table query.
    TableQuery<CustomerEntity> rangeQuery = new TableQuery<CustomerEntity>().Where(
        TableQuery.CombineFilters(
            TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Smith"),
            TableOperators.And,
            TableQuery.GenerateFilterCondition("RowKey", QueryComparisons.LessThan, "E")));

    // Loop through the results, displaying information about the entity.
    foreach (CustomerEntity entity in table.ExecuteQuery(rangeQuery))
    {
        Console.WriteLine("{0}, {1}\t{2}\t{3}", entity.PartitionKey, entity.RowKey,
            entity.Email, entity.PhoneNumber);
    }

## <a name="retrieve-a-single-entity"></a>Egyetlen egységet beolvasása

Egy adott személy beolvasásához lekérdezés is írhat. A következő kódot **TableOperation** segítségével adja meg az ügyfél "Kovács Zsolt".
Ez a módszer ad eredményül, csak egy egyed, hanem egy gyűjtemény és a visszaadott érték a **TableResult.Result** egy **CustomerEntity** objektumot.
Ez leggyorsabban egy entitás beolvasni a táblázat szolgáltatásból való partíciót vagy a sor megadása a lekérdezés.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable object that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Create a retrieve operation that takes a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute the retrieve operation.
    TableResult retrievedResult = table.Execute(retrieveOperation);

    // Print the phone number of the result.
    if (retrievedResult.Result != null)
       Console.WriteLine(((CustomerEntity)retrievedResult.Result).PhoneNumber);
    else
       Console.WriteLine("The phone number could not be retrieved.");

## <a name="replace-an-entity"></a>Egy személy cseréje

Entitás frissítéséhez megtalálja a táblázat szolgáltatásból, entitás objektumok módosíthatók, és mentse a változtatásokat az a tábla-szolgáltatásba. A következő kódrészlet egy meglévő vevő telefonszám változik. Helyett hívásához **beszúrása**, a kód **cseréje**használja. A szervezet teljesen cserélendő a kiszolgálón ennek hatására, kivéve, ha a szervezet a kiszolgálón megváltozott a beolvasás, amely esetben a művelet sikertelen lesz óta.  Ezt a hibát is megakadályozhatja, hogy véletlenül a frissítés és a lekérés között egy másik összetevő az alkalmazás által elvégzett módosítás felülírja az alkalmazást.  A megfelelő kezelését, ezt a hibát, hogy a szervezet ismételt lekéréséhez, végezze el a módosításokat (Ha még érvényes), majd végrehajtása egy másik **helyére** .  A következő szakaszban kell követnie Ez a működési módjának felülbírálására.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable object that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Create a retrieve operation that takes a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute the operation.
    TableResult retrievedResult = table.Execute(retrieveOperation);

    // Assign the result to a CustomerEntity object.
    CustomerEntity updateEntity = (CustomerEntity)retrievedResult.Result;

    if (updateEntity != null)
    {
       // Change the phone number.
       updateEntity.PhoneNumber = "425-555-0105";

       // Create the Replace TableOperation.
       TableOperation updateOperation = TableOperation.Replace(updateEntity);

       // Execute the operation.
       table.Execute(updateOperation);

       Console.WriteLine("Entity updated.");
    }

    else
       Console.WriteLine("Entity could not be retrieved.");

## <a name="insert-or-replace-an-entity"></a>Beszúrás – vagy – csere entitás

Műveletek **cseréje** meghiúsul, ha a szervezet óta lehívni a kiszolgálóról megváltozott.  Ezenkívül a szervezet kell lekérni a kiszolgálóról, először annak érdekében, hogy a **Csere** művelet sikeres.
Egyes esetekben azonban nem tudja, hogy ha a szervezet a kiszolgálón található, és az aktuális benne tárolt értékei nem számít. A frissítés célszerű felülírni összes.  Ehhez használja egy **InsertOrReplace** művelet.  Ez a művelet szúrja be a személy, ha nem létezik, vagy ha igen, függetlenül attól, amikor az utolsó frissítés elvégezték váltja fel.  A következő példában kódot az ügyfél entitás Zsolt Kovács a program továbbra is, de majd mentésekor a kiszolgálón keresztül **InsertOrReplace**.  A szervezet között a lekérés és frissítés művelet frissítéseiről felülíródnak.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable object that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Create a retrieve operation that takes a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute the operation.
    TableResult retrievedResult = table.Execute(retrieveOperation);

    // Assign the result to a CustomerEntity object.
    CustomerEntity updateEntity = (CustomerEntity)retrievedResult.Result;

    if (updateEntity != null)
    {
       // Change the phone number.
       updateEntity.PhoneNumber = "425-555-1234";

       // Create the InsertOrReplace TableOperation.
       TableOperation insertOrReplaceOperation = TableOperation.InsertOrReplace(updateEntity);

       // Execute the operation.
       table.Execute(insertOrReplaceOperation);

       Console.WriteLine("Entity was updated.");
    }

    else
       Console.WriteLine("Entity could not be retrieved.");

## <a name="query-a-subset-of-entity-properties"></a>A lekérdezés csak egy részhalmazát entitás tulajdonságai

A lekérdezés néhány tulajdonságok beolvasható entitás összes entitás tulajdonságai helyett. Ez a módszer kivetítés, nevű csökkenti a sávszélesség, és javíthatja a lekérdezési teljesítményt, különösen a nagyobb szervezetek. A lekérdezés, a következő kódrészlet csak személyek e-mail címét a táblázatban adja eredményül. Ez történik, **DynamicTableEntity** , és **EntityResolver**lekérdezés használatával. Többet is megtudhat a [Upsert bemutatása és a lekérdezés kivetítés blog közzé][]a kivetítés. Figyelje meg, hogy a helyi tároló irányító nem támogatott kivetítés, a kód csak fut, ha a táblázat szolgáltatás egy fiókot használ.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Define the query, and select only the Email property.
    TableQuery<DynamicTableEntity> projectionQuery = new TableQuery<DynamicTableEntity>().Select(new string[] { "Email" });

    // Define an entity resolver to work with the entity after retrieval.
    EntityResolver<string> resolver = (pk, rk, ts, props, etag) => props.ContainsKey("Email") ? props["Email"].StringValue : null;

    foreach (string projectedEmail in table.ExecuteQuery(projectionQuery, resolver, null, null))
    {
        Console.WriteLine(projectedEmail);
    }

## <a name="delete-an-entity"></a>Entitás törlése

Egy személy beolvasása után, a entitás frissítésének látható azonos minta használatával könnyen törölheti.  A következő kódot, és az ügyfél entitás törli.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Create a retrieve operation that expects a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute the operation.
    TableResult retrievedResult = table.Execute(retrieveOperation);

    // Assign the result to a CustomerEntity.
    CustomerEntity deleteEntity = (CustomerEntity)retrievedResult.Result;

    // Create the Delete TableOperation.
    if (deleteEntity != null)
    {
       TableOperation deleteOperation = TableOperation.Delete(deleteEntity);

       // Execute the operation.
       table.Execute(deleteOperation);

       Console.WriteLine("Entity deleted.");
    }

    else
       Console.WriteLine("Could not retrieve the entity.");

## <a name="delete-a-table"></a>Táblázat törlése

Végül a következő példa tárterület-fiókból törli a táblát. Egy táblázat, amely a törlése után nem érhetők el egy ideig a törlés után újra létre kell lesz.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Delete the table it if exists.
    table.DeleteIfExists();

## <a name="retrieve-entities-in-pages-asynchronously"></a>Aszinkron beolvasni a személyek lapon

Ha nagyszámú szervezetek olvasása, és a megjelenítendő folyamat/szervezetek, az adatok lekérése, ahelyett hogy mindet való visszatéréshez várakozik, meghallgathatja szervezetek szegmentált lekérdezés használatával. Ez a példa szemlélteti az aszinkron kerülve minta használatával, hogy a végrehajtás nem zárolt, az eredmény nagy számú várakozás közben a lapokon jelenjen meg eredmény. Az aszinkron-Await mintát használatáról a .NET, lásd: [aszinkron programozás aszinkron és Await (C# és a Visual Basic)](https://msdn.microsoft.com/library/hh191443.aspx).

    // Initialize a default TableQuery to retrieve all the entities in the table.
    TableQuery<CustomerEntity> tableQuery = new TableQuery<CustomerEntity>();

    // Initialize the continuation token to null to start from the beginning of the table.
    TableContinuationToken continuationToken = null;

    do
    {
        // Retrieve a segment (up to 1,000 entities).
        TableQuerySegment<CustomerEntity> tableQueryResult =
            await table.ExecuteQuerySegmentedAsync(tableQuery, continuationToken);

        // Assign the new continuation token to tell the service where to
        // continue on the next iteration (or null if it has reached the end).
        continuationToken = tableQueryResult.ContinuationToken;

        // Print the number of rows retrieved.
        Console.WriteLine("Rows retrieved {0}", tableQueryResult.Results.Count);

    // Loop until a null continuation token is received, indicating the end of the table.
    } while(continuationToken != null);

## <a name="next-steps"></a>Következő lépések

Most, hogy megtanulta táblatároló alapjait, kövesse ezeket a hivatkozásokat, ha többet szeretne tudni az összetettebb tároló tevékenységek:

- Lásd: a több tábla tárolási minta az [Első lépések a .NET Azure Táblatárolóhoz](https://azure.microsoft.com/documentation/samples/storage-table-dotnet-getting-started/)
- Táblázat szolgáltatás hivatkozás dokumentációjában találhat részletes adatait a rendelkezésre álló API-khoz megtekintése:
    - [Tárterület ügyfél dokumentumtár a .NET-hivatkozás](http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409)
    - [REST API-hivatkozás](http://msdn.microsoft.com/library/azure/dd179355)
- Megtudhatja, hogy miként írhat a Azure tárhely kezelése a [Azure WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk-get-started.md) használatával kódot egyszerűsítése érdekében
- További tudnivalók: további lehetőségeket adattárolásra Azure-ban több funkció útmutatók megtekintése.
    - [Első lépések az Azure Blob-tárolóhoz .NET használatával](storage-dotnet-how-to-use-blobs.md) strukturálatlan adatokat tároló.
    - [Csatlakozás SQL-adatbázis .NET (C#) használatával](../sql-database/sql-database-develop-dotnet-simple.md) relációs adatok tárolására.

  [Download and install the Azure SDK for .NET]: /develop/net/
  [Creating an Azure Project in Visual Studio]: http://msdn.microsoft.com/library/azure/ee405487.aspx

  [Blob5]: ./media/storage-dotnet-how-to-use-table-storage/blob5.png
  [Blob6]: ./media/storage-dotnet-how-to-use-table-storage/blob6.png
  [Blob7]: ./media/storage-dotnet-how-to-use-table-storage/blob7.png
  [Blob8]: ./media/storage-dotnet-how-to-use-table-storage/blob8.png
  [Blob9]: ./media/storage-dotnet-how-to-use-table-storage/blob9.png

  [Webnapló-hozzászólás bevezetéséről Upsert és a lekérdezés vetítés]: http://blogs.msdn.com/b/windowsazurestorage/archive/2011/09/15/windows-azure-tables-introducing-upsert-and-query-projection.aspx
  [.NET Client Library reference]: http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409
  [Azure Storage Team blog]: http://blogs.msdn.com/b/windowsazurestorage/
  [Configure Azure Storage connection strings]: http://msdn.microsoft.com/library/azure/ee758697.aspx
  [OData]: http://nuget.org/packages/Microsoft.Data.OData/5.0.2
  [Edm]: http://nuget.org/packages/Microsoft.Data.Edm/5.0.2
  [Spatial]: http://nuget.org/packages/System.Spatial/5.0.2
  [How to: Programmatically access Table storage]: #tablestorage
