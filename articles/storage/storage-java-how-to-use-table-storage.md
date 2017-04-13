<properties
    pageTitle="A Java táblatároló használata |} Microsoft Azure"
    description="Strukturált adatokat tárolja a felhőben Azure táblatárolóhoz, egy NoSQL adattár használatával."
    services="storage"
    documentationCenter="java"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>


# <a name="how-to-use-table-storage-from-java"></a>A Java táblatároló használata

[AZURE.INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>– Áttekintés

Ez az útmutató megtudhatja, miként végezheti el a gyakori alkalmazási területek, az Azure-táblából tároló szolgáltatással. A minták Java írt, és az [Azure tároló SDK Java][]használja. Az érintett esetek **létrehozása**, **amelyben**és **törlése** a táblázatok, valamint **beszúrása**, **lekérdezés**, **módosítása**és **törlése** személyek egy táblázatban. További információt a táblázatokban című [következő lépések](#Next-Steps) .

Megjegyzés: Az SDK érhető el a fejlesztők számára, akik a Azure tároló Android-eszközökön. További tudnivalókért lásd: az [Azure tároló SDK csomagjában talál az Android-alapú][].

[AZURE.INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-java-application"></a>Java-alkalmazás létrehozása

Ebből az útmutatóból tárolási lehetőségek, amely a másolni helyileg vagy a webes szerepkör vagy dolgozó szerepkör Azure-ban futó kód egy Java alkalmazáson belül futtathatja fogja használni.

Ehhez, meg kell Java fejlesztési Kit (JDK) telepítése, és Azure tároló fiók létrehozása az Azure-előfizetésben. Miután elvégezte a úgy, szüksége lesz annak ellenőrzése, hogy a fejlesztői rendszer megfelel-e a minimális követelmények és függőségek, amelyek jelennek meg a GitHub [Java Azure tároló SDK][] tárházából. Ha a rendszer megfelel ezeknek a követelményeknek, kövesse az utasításokat a letöltése és telepítése az Azure tároló tárakat Java, hogy a tárházba a számítógépre. Miután elvégezte ezeket a feladatokat, lesz a példák a jelen cikkben használó Java-alkalmazás létrehozása.

## <a name="configure-your-application-to-access-table-storage"></a>Állítsa be az alkalmazás táblatároló eléréséhez

Adja hozzá a következő importálása kimutatások felső részén a Java-fájlt, amelyhez Microsoft Azure tároló API-khoz táblák eléréséhez használni:

    // Include the following imports to use table APIs
    import com.microsoft.azure.storage.*;
    import com.microsoft.azure.storage.table.*;
    import com.microsoft.azure.storage.table.TableQuery.*;

## <a name="setup-an-azure-storage-connection-string"></a>Az Azure tároló kapcsolati karakterlánc beállítása

Az Azure tároló ügyfél tároló kapcsolati karakterlánc használja a végpontok és adatok kezelése szolgáltatások eléréséhez szükséges hitelesítő adatok tárolására szolgáló. Ügyfélalkalmazás fut, meg kell adnia a tárhely kapcsolati karakterláncot, a következő formátumban *fióknév* és *AccountKey* az értékek az [Azure-portálon](https://portal.azure.com) szereplő tároló partner nevét a tárterület-fiók és az access elsődleges kulcs használ. Ez a példa mutatja, hogy hogyan tartsa lenyomva az ujját a kapcsolati karakterlánc statikus mező is deklarálhatnak:

    // Define the connection-string with your values.
    public static final String storageConnectionString =
        "DefaultEndpointsProtocol=http;" +
        "AccountName=your_storage_account;" +
        "AccountKey=your_storage_account_key";

Az operációs rendszert futtató belül a Microsoft Azure szerepkörbe alkalmazásokban a karakterlánc szolgáltatás konfigurációs fájl *ServiceConfiguration.cscfg*, tárolhatók és a hívást a **RoleEnvironment.getConfigurationSettings** módszerrel is elérhető. Íme egy példa a kapcsolati karakterlánc ahhoz, hogy a **beállítás** elem nevű *StorageConnectionString* a szolgáltatás konfigurációs fájl:

    // Retrieve storage account from connection-string.
    String storageConnectionString =
        RoleEnvironment.getConfigurationSettings().get("StorageConnectionString");

Az alábbi példa feltételezi, hogy már használta az alábbi két módszerek egyikét a tárhely kapcsolati karakterláncát.

## <a name="how-to-create-a-table"></a>Útmutató: táblázat létrehozása

Egy **CloudTableClient** objektum lehetővé teszi a táblák és a személyek hivatkozást objektumok beszerzése. A következő kódot létrehoz egy **CloudTableClient** objektumot, és hozzon létre egy új **CloudTable** objektumot, amely a "személyek" nevű táblázat segítségével. (Megjegyzés: más módokon **CloudStorageAccount** objektumok; létrehozásához további tudnivalókért lásd: **CloudStorageAccount** az [Azure tároló ügyfél SDK hivatkozást].)

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

       // Create the table client.
       CloudTableClient tableClient = storageAccount.createCloudTableClient();

       // Create the table if it doesn't exist.
       String tableName = "people";
       CloudTable cloudTable = tableClient.getTableReference(tableName);
       cloudTable.createIfNotExists();
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-list-the-tables"></a>Útmutató: a Táblák listában

Táblák listájának, hívja a **CloudTableClient.listTables()** módszer beolvasni egy táblázatnevek iterable listáját.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Loop through the collection of table names.
        for (String table : tableClient.listTables())
        {
          // Output each table name.
          System.out.println(table);
       }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-add-an-entity-to-a-table"></a>Útmutató: entitás hozzáadása táblához

Személyek hozzárendelése a Java-objektumok használata osztály egyéni **TableEntity**végrehajtása. Az egyszerűség kedvéért **TableServiceEntity** osztály hajtja végre **TableEntity** és tükröződés használatával tulajdonságok megfeleltetése lekérdező és beállító metódus módszerek az a tulajdonságokat nevű. Egy személy hozzáadása egy táblához, először létre kell hoznia egy osztály, amely meghatározza a szervezet tulajdonságainak. A következő kódot határozza meg, hogy egy egyed osztály, amely a sor billentyűt, és a vezetéknevet partíciót kulcsként a vevő első nevét használja. Közös egy személy partíciót és a sor kulcs egyedileg azonosító a szervezet a táblázatban. Személyek ugyanazzal a partíciót kulccsal gyorsabb, mint a másik partíciót billentyűkkel lekérdezhető.

    public class CustomerEntity extends TableServiceEntity {
        public CustomerEntity(String lastName, String firstName) {
            this.partitionKey = lastName;
            this.rowKey = firstName;
        }

        public CustomerEntity() { }

        String email;
        String phoneNumber;

        public String getEmail() {
            return this.email;
        }

        public void setEmail(String email) {
            this.email = email;
        }

        public String getPhoneNumber() {
            return this.phoneNumber;
        }

        public void setPhoneNumber(String phoneNumber) {
            this.phoneNumber = phoneNumber;
        }
    }

Táblázat műveletek szervezetek érintő szükség egy **TableOperation** objektumot. Az objektum határozza meg, hogy egy egyed, amely **CloudTable** objektummal futtatható kell elvégezni a műveletet. A következő kódot néhány ügyféladatokkal tárolni hoz létre egy új példányát az **CustomerEntity** osztály. A kód következő felhívja **TableOperation.insertOrReplace** entitás beszúrása egy táblázatba **TableOperation** objektumot szeretne létrehozni, és az új **CustomerEntity** társít. Végül a kód megadása a "személyek" táblázat és az új **TableOperation**, majd a kérést küld a tároló szolgáltatás az új ügyfél entitás beszúrni a "személyek" táblába, vagy a szervezet cseréje, ha már létezik, amely a **CloudTable** objektum hívások metódus **végrehajtása** .

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Create a cloud table object for the table.
        CloudTable cloudTable = tableClient.getTableReference("people");

        // Create a new customer entity.
        CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
        customer1.setEmail("Walter@contoso.com");
        customer1.setPhoneNumber("425-555-0101");

        // Create an operation to add the new customer to the people table.
        TableOperation insertCustomer1 = TableOperation.insertOrReplace(customer1);

        // Submit the operation to the table service.
        cloudTable.execute(insertCustomer1);
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-insert-a-batch-of-entities"></a>Útmutató: egy köteg szervezetek beszúrása

A táblázat szolgáltatás szervezetek egy köteg szúrhatja be egy írási művelet. A következő kódot létrehoz egy **TableBatchOperation** objektumot, majd összeadja a három beszúrása műveletek rá. Minden egyes Beszúrás művelet megjelenik egy új egyed-objektum létrehozása, és állítsa be az értékeket, majd hívja fel a **beszúrása** módszer az a személy társítása egy új beillesztési művelet **TableBatchOperation** objektumra. Kattintson a kód hívások **hajtsa végre** a **CloudTable** objektum, a "személyek" táblázat és a **TableBatchOperation** objektum, amely a táblázat műveletek a köteg küld a tárhelyszolgáltatáshoz egyetlen összehívásban.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Define a batch operation.
        TableBatchOperation batchOperation = new TableBatchOperation();

        // Create a cloud table object for the table.
        CloudTable cloudTable = tableClient.getTableReference("people");

        // Create a customer entity to add to the table.
        CustomerEntity customer = new CustomerEntity("Smith", "Jeff");
        customer.setEmail("Jeff@contoso.com");
        customer.setPhoneNumber("425-555-0104");
        batchOperation.insertOrReplace(customer);

       // Create another customer entity to add to the table.
       CustomerEntity customer2 = new CustomerEntity("Smith", "Ben");
       customer2.setEmail("Ben@contoso.com");
       customer2.setPhoneNumber("425-555-0102");
       batchOperation.insertOrReplace(customer2);

       // Create a third customer entity to add to the table.
       CustomerEntity customer3 = new CustomerEntity("Smith", "Denise");
       customer3.setEmail("Denise@contoso.com");
       customer3.setPhoneNumber("425-555-0103");
       batchOperation.insertOrReplace(customer3);

       // Execute the batch of operations on the "people" table.
       cloudTable.execute(batchOperation);
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

Megjegyzés a köteg műveletek műveleteket:

- Legfeljebb 100 Beszúrás végrehajtani, törlésével, egyesítése, cseréje, beszúrása vagy egyesíteni, és beszúrása vagy műveletek egyetlen kötegben tetszőleges helyére.
- Egy köteg művelet a lekérés művelet állhat, ha csak a műveletet a köteget.
- Egyetlen köteg művelet összes entitás ugyanazt a partíciót kulcsot kell rendelkeznie.
- A köteg művelet 4 MB-os adatok hasznos korlátozódik.

## <a name="how-to-retrieve-all-entities-in-a-partition"></a>Útmutató: az összes partíciót vállalkozások beolvasása

Személyek felvétele a táblát, lekérdezést, egy **TableQuery**is használhatja. Hívja fel a **TableQuery.from** lekérdezés egy adott tábla, amely visszaadja a megadott találattípus létrehozásához. A következő kódot adja meg a személyek hol "Kovács"-e a partíciót kulcs szűrőt. **TableQuery.generateFilterCondition** segítő módszerrel lekérdezések szűrőket készíthet. Telefonál **ahová** a hivatkozást a **TableQuery.from** módszert alkalmazhatja a szűrőt a lekérdezés által visszaadott. A lekérdezés futtatásakor a hívást, **hajtsa végre** a **CloudTable** objektum egy **léptető** akkor eredménye a megadott **CustomerEntity** eredmény típusa. Ezután felhasználhatja a visszaadott **léptető** egy az egyes hurok használhatnak az eredményeket. Ez a kód nyomtatja ki a mezőket a lekérdezés eredményében a konzolhoz minden személy.

    try
    {
        // Define constants for filters.
        final String PARTITION_KEY = "PartitionKey";
        final String ROW_KEY = "RowKey";
        final String TIMESTAMP = "Timestamp";

        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

       // Create a cloud table object for the table.
       CloudTable cloudTable = tableClient.getTableReference("people");

        // Create a filter condition where the partition key is "Smith".
        String partitionFilter = TableQuery.generateFilterCondition(
           PARTITION_KEY,
           QueryComparisons.EQUAL,
           "Smith");

       // Specify a partition query, using "Smith" as the partition key filter.
       TableQuery<CustomerEntity> partitionQuery =
           TableQuery.from(CustomerEntity.class)
           .where(partitionFilter);

        // Loop through the results, displaying information about the entity.
        for (CustomerEntity entity : cloudTable.execute(partitionQuery)) {
            System.out.println(entity.getPartitionKey() +
                " " + entity.getRowKey() +
                "\t" + entity.getEmail() +
                "\t" + entity.getPhoneNumber());
       }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-retrieve-a-range-of-entities-in-a-partition"></a>Útmutató: partíciót vállalkozások tartomány beolvasása

Ha nem szeretné a lekérdezés partíciót az összes entitás, megadhatja a tartomány összehasonlító operátorok szűrő használatával. A következő kódot két szűrők egyben az összes partíciót vállalkozások "Kovács", ahol az a sor kulcs (Utónév) felfelé "E" betűvel kezdődik az ábécé egyesíti. Kattintson a lekérdezés eredményének nyomtat történik. Ha ez az útmutató a személyek bekerült a táblába a köteg szakasztartalom, a csak két entitás (Zsolt és Kovács Dezső); Ez esetben a visszaadott Kovács függvény nem található.

    try
    {
        // Define constants for filters.
        final String PARTITION_KEY = "PartitionKey";
        final String ROW_KEY = "RowKey";
        final String TIMESTAMP = "Timestamp";

        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

       // Create the table client.
       CloudTableClient tableClient = storageAccount.createCloudTableClient();

       // Create a cloud table object for the table.
       CloudTable cloudTable = tableClient.getTableReference("people");

        // Create a filter condition where the partition key is "Smith".
        String partitionFilter = TableQuery.generateFilterCondition(
           PARTITION_KEY,
           QueryComparisons.EQUAL,
           "Smith");

        // Create a filter condition where the row key is less than the letter "E".
        String rowFilter = TableQuery.generateFilterCondition(
           ROW_KEY,
           QueryComparisons.LESS_THAN,
           "E");

        // Combine the two conditions into a filter expression.
        String combinedFilter = TableQuery.combineFilters(partitionFilter,
            Operators.AND, rowFilter);

        // Specify a range query, using "Smith" as the partition key,
        // with the row key being up to the letter "E".
        TableQuery<CustomerEntity> rangeQuery =
           TableQuery.from(CustomerEntity.class)
           .where(combinedFilter);

        // Loop through the results, displaying information about the entity
        for (CustomerEntity entity : cloudTable.execute(rangeQuery)) {
            System.out.println(entity.getPartitionKey() +
                " " + entity.getRowKey() +
                "\t" + entity.getEmail() +
                "\t" + entity.getPhoneNumber());
        }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-retrieve-a-single-entity"></a>Útmutató: egy entitás beolvasása

Egy adott személy beolvasásához lekérdezés is írhat. A következő kódot hívások **TableOperation.retrieve** partíciót billentyűt, és a sor kulcs paraméterek adja meg az ügyfél "Kovács Kovács" helyett egy **TableQuery** létrehozásáról és használatáról a szűrők is ugyanezt a célt szolgálja. Amikor végrehajtása, a lekérés művelet gyűjteménye, hanem csak egy egyed adja eredményül. A **getResultAsType** módszer árnyékot, az eredmény a hozzárendelés cél, egy **CustomerEntity** objektum típusát. Ha ez a típus nem kompatibilis a megadott a lekérdezés típusa, fog kell kivétel. Null értéket eredményül, ha nincs entitás nem pontos partíciót és a megfelelő sor billentyűt. Ez leggyorsabban egy entitás beolvasni a táblázat szolgáltatásból való partíciót vagy a sor megadása a lekérdezés.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Create a cloud table object for the table.
        CloudTable cloudTable = tableClient.getTableReference("people");

        // Retrieve the entity with partition key of "Smith" and row key of "Jeff"
        TableOperation retrieveSmithJeff =
           TableOperation.retrieve("Smith", "Jeff", CustomerEntity.class);

       // Submit the operation to the table service and get the specific entity.
       CustomerEntity specificEntity =
            cloudTable.execute(retrieveSmithJeff).getResultAsType();

        // Output the entity.
        if (specificEntity != null)
        {
            System.out.println(specificEntity.getPartitionKey() +
                " " + specificEntity.getRowKey() +
                "\t" + specificEntity.getEmail() +
                "\t" + specificEntity.getPhoneNumber());
       }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-modify-an-entity"></a>Útmutató: entitás módosítása

Entitás módosítása megtalálja a táblázat szolgáltatásból, végezze el a módosításokat a szervezet objektumra, és az a táblázat szolgáltatásba, és a csere vagy az egyesítés műveletet a módosítások mentéséhez. A következő kódrészlet egy meglévő vevő telefonszám változik. Ez a kód helyett **TableOperation.insert** hívásához a megszokott azt szeretne beszúrni, hívja fel a **TableOperation.replace**. A **CloudTable.execute** módszer felhívja a táblázat szolgáltatást, és a szervezet helyettesíti, kivéve, ha egy másik alkalmazás módosították az idő óta visszakeresése Ez az alkalmazás. Ebben az esetben kivétel van, és a szervezet kell beolvasni, módosíthatók, és mentése ismét. Ez az optimista feldolgozási újrapróbálkozási mintája közös elosztott tároló rendszerrel.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Create a cloud table object for the table.
        CloudTable cloudTable = tableClient.getTableReference("people");

        // Retrieve the entity with partition key of "Smith" and row key of "Jeff".
        TableOperation retrieveSmithJeff =
           TableOperation.retrieve("Smith", "Jeff", CustomerEntity.class);

        // Submit the operation to the table service and get the specific entity.
        CustomerEntity specificEntity =
          cloudTable.execute(retrieveSmithJeff).getResultAsType();

        // Specify a new phone number.
        specificEntity.setPhoneNumber("425-555-0105");

        // Create an operation to replace the entity.
        TableOperation replaceEntity = TableOperation.replace(specificEntity);

        // Submit the operation to the table service.
        cloudTable.execute(replaceEntity);
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-query-a-subset-of-entity-properties"></a>Útmutató: a lekérdezés csak egy részhalmazát entitás tulajdonságai

Táblázat lekérdezés néhány tulajdonságok beolvasható entitás. Ez a módszer kivetítés, nevű csökkenti a sávszélesség, és javíthatja a lekérdezési teljesítményt, különösen a nagyobb szervezetek. A lekérdezés, a következő kódrészlet használja a **Válassza ki** a módszer csak személyek e-mail címét a táblázatban. Az eredmények segítségével egy **EntityResolver**, amely jelent a konvertálás írja be a személyek, a kiszolgáló által visszaadott **karakterlánc** gyűjteménybe vannak tervezett. További információ a kivetítés [Azure táblák: Upsert bemutatása és a lekérdezés kivetítés][]. Figyelje meg, hogy a helyi tároló irányító nem támogatott kivetítés, a kód csak fut, ha a táblázat szolgáltatás-fiókot használ.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Create a cloud table object for the table.
        CloudTable cloudTable = tableClient.getTableReference("people");

        // Define a projection query that retrieves only the Email property
        TableQuery<CustomerEntity> projectionQuery =
           TableQuery.from(CustomerEntity.class)
           .select(new String[] {"Email"});

        // Define a Entity resolver to project the entity to the Email value.
        EntityResolver<String> emailResolver = new EntityResolver<String>() {
            @Override
            public String resolve(String PartitionKey, String RowKey, Date timeStamp, HashMap<String, EntityProperty> properties, String etag) {
                return properties.get("Email").getValueAsString();
            }
        };

        // Loop through the results, displaying the Email values.
        for (String projectedString :
            cloudTable.execute(projectionQuery, emailResolver)) {
                System.out.println(projectedString);
        }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-insert-or-replace-an-entity"></a>Útmutató: beszúrása vagy entitás cseréje

Gyakran érdemes entitás hozzáadása egy táblához nélkül arra, ha már létezik a táblázatban. Beszúrás vagy csere művelet lehetővé teszi a egyetlen kérést, amelyet a szervezet helyezze be, ha nem létezik vagy cseréje a meglévővel, ha igen. Előzetes példák támaszkodva, az alábbi kód beszúrása vagy cseréli a szervezet "Walter grönlandi". Miután létrehozott egy új entitás, a kód hívások **TableOperation.insertOrReplace** módszer. Ez a kód majd **végrehajtása** felhívja a táblázat és a Beszúrás **CloudTable** objektumon, vagy lecserélheti a táblázat művelet paraméterek. Csak egy személy részére frissítéséhez **TableOperation.insertOrMerge** módszer helyett használható. Figyelje meg, hogy Beszúrás – vagy – csere nem támogatott a helyi tároló irányító, a kód csak fut, ha a táblázat szolgáltatás-fiókot használ. További információ a Beszúrás vagy csere és Beszúrás – vagy – a körlevél ez talál [Azure táblák: Upsert bemutatása és a lekérdezés kivetítés][].

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Create a cloud table object for the table.
        CloudTable cloudTable = tableClient.getTableReference("people");

        // Create a new customer entity.
        CustomerEntity customer5 = new CustomerEntity("Harp", "Walter");
        customer5.setEmail("Walter@contoso.com");
        customer5.setPhoneNumber("425-555-0106");

        // Create an operation to add the new customer to the people table.
        TableOperation insertCustomer5 = TableOperation.insertOrReplace(customer5);

        // Submit the operation to the table service.
        cloudTable.execute(insertCustomer5);
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-delete-an-entity"></a>Útmutató: entitás törlése

Egy személy beolvasása azt után könnyen törölheti. Miután a szervezet veszi, hívja fel a **TableOperation.delete** az a személy törlése. Akkor hívja meg **végrehajtani** a **CloudTable** objektum. A következő kódot, és az ügyfél entitás törli.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Create a cloud table object for the table.
        CloudTable cloudTable = tableClient.getTableReference("people");

        // Create an operation to retrieve the entity with partition key of "Smith" and row key of "Jeff".
        TableOperation retrieveSmithJeff = TableOperation.retrieve("Smith", "Jeff", CustomerEntity.class);

        // Retrieve the entity with partition key of "Smith" and row key of "Jeff".
        CustomerEntity entitySmithJeff =
            cloudTable.execute(retrieveSmithJeff).getResultAsType();

        // Create an operation to delete the entity.
        TableOperation deleteSmithJeff = TableOperation.delete(entitySmithJeff);

        // Submit the delete operation to the table service.
        cloudTable.execute(deleteSmithJeff);
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-delete-a-table"></a>Útmutató: táblázat törlése

Végül a következő kódot táblázat törlése egy tárterület-fiókból. Egy táblázatban, amely a törlése után nem érhető el, létre kell hozni egy ideig a törlés, általában kisebb, mint körülbelül 40 másodpercig követő lesz.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Delete the table and all its data if it exists.
        CloudTable cloudTable = new CloudTable("people",tableClient);
        cloudTable.deleteIfExists();
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="next-steps"></a>Következő lépések

Most, hogy megtanulta táblatároló alapjait, kövesse az alábbi hivatkozásokat követve megtudhatja, hogy miként összetettebb tároló feladatokhoz.

- [Azure tároló Java SDK][]
- [Azure tároló ügyfél SDK hivatkozás][]
- [Azure tároló REST API-val][]
- [Azure tároló csapatának blogja][]

További tudnivalókért lásd még: a [Java Developer Center](/develop/java/).


[Azure SDK for Java]: http://go.microsoft.com/fwlink/?LinkID=525671
[Azure tároló Java SDK]: https://github.com/azure/azure-storage-java
[Azure tároló SDK androidra]: https://github.com/azure/azure-storage-android
[Azure tároló ügyfél SDK hivatkozás]: http://dl.windowsazure.com/storage/javadoc/
[Azure tároló REST API-val]: https://msdn.microsoft.com/library/azure/dd179355.aspx
[Azure tároló csapatának blogja]: http://blogs.msdn.com/b/windowsazurestorage/
[Azure táblák: Upsert és a lekérdezés kivetítés bemutatása]: http://blogs.msdn.com/b/windowsazurestorage/archive/2011/09/15/windows-azure-tables-introducing-upsert-and-query-projection.aspx
