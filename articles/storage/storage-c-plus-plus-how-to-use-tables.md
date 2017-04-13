<properties
    pageTitle="Táblatároló (C++) használata |} Microsoft Azure"
    description="Strukturált adatokat tárolja a felhőben Azure táblatárolóhoz, egy NoSQL adattár használatával."
    services="storage"
    documentationCenter=".net"
    authors="dineshmurthy"
    manager="jahogg"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="dineshm"/>

# <a name="how-to-use-table-storage-from-c"></a>A C++ táblatároló használata

[AZURE.INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>– Áttekintés  
Ez az útmutató megtudhatja, miként végezheti el a gyakori alkalmazási területek az Azure-táblából tárhelyszolgáltatáshoz használatával. A minták C++ írt, és a [Azure tároló ügyfél-tár C++](https://github.com/Azure/azure-storage-cpp/blob/master/README.md). Az érintett esetek **létrehozása és törlése a táblázat** és a **táblázat szervezetek használata**.

>[AZURE.NOTE] Ez az útmutató az Azure tároló ügyfél tár célként, C++ verzió 1.0.0 és felett. A javasolt verziója tároló ügyfél tár 2.2.0, melyiket érdemes [NuGet](http://www.nuget.org/packages/wastorage) vagy [GitHub](https://github.com/Azure/azure-storage-cpp/)keresztül érhető el.

[AZURE.INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]
[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]


## <a name="create-a-c-application"></a>C++-alkalmazás létrehozása  
Ebből az útmutatóból futtatását is lehetővé teszi a C++ alkalmazáson belül tároló funkciói fogja használni. Ehhez, meg kell telepítse az Azure tároló ügyfél tár C++, és Azure tároló fiók létrehozása az Azure-előfizetésben.  

Az Azure tároló ügyfél tár telepítése C++, az alábbi módszerek használhatja:

-   **Linux:** Kövesse a lapon [Azure tároló ügyfél-tár C++ – fontos fájl](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) kapott utasításokat.  
-   **Windows:** Kattintson a Visual Studióban, **Eszközök > NuGet csomag kezelő > csomag kezelője konzol**. A következő parancsot írja be a [NuGet csomag kezelője konzol](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) , és nyomja le az ENTER billentyűt.  

        Install-Package wastorage

## <a name="configure-your-application-to-access-table-storage"></a>Állítsa be az alkalmazás táblatároló eléréséhez  
Adja meg a következő utasítások tetején a C++ fájlt, amelyre az Azure tároló API-khoz táblák eléréséhez használni a tartalmazza:  

    #include "was/storage_account.h"
    #include "was/table.h"

## <a name="set-up-an-azure-storage-connection-string"></a>Az Azure tároló kapcsolati karakterlánc beállítása  
Az Azure tároló ügyfél tároló kapcsolati karakterlánc használja a végpontok és adatok kezelése szolgáltatások eléréséhez szükséges hitelesítő adatok tárolására szolgáló. Ügyfélalkalmazás fut, a tárhely kapcsolati karakterláncot, a következő formátumban kell megadnia. A *fióknév* és *AccountKey* az értékek [Azure-portálon](https://portal.azure.com) szereplő tároló partner nevét a tárterület-fiók és a tárhely hívóbetű használata. A tároló partnereket és a hívóbetűk további tudnivalókért lásd [kapcsolatos Azure tároló fiókok](storage-create-storage-account.md). Ez a példa mutatja, hogy hogyan tartsa lenyomva az ujját a kapcsolati karakterlánc statikus mező is deklarálhatnak:  

    // Define the connection string with your values.
    const utility::string_t storage_connection_string(U("DefaultEndpointsProtocol=https;AccountName=your_storage_account;AccountKey=your_storage_account_key"));

Ha tesztelni szeretné az alkalmazás a Windows-alapú számítógépre, az Azure [tároló irányító](storage-use-emulator.md) , hogy telepítve van az [Azure SDK](https://azure.microsoft.com/downloads/)is használhatja. A tároló irányító egy, amely a helyi fejlesztési számítógépen elérhető Azure Blob várólista és táblázat szolgáltatások segédprogramot. A következő példa bemutatja, hogyan szeretné a helyi tároló irányító tartsa lenyomva az ujját a kapcsolati karakterlánc statikus mező is deklarálhatnak:  

    // Define the connection string with Azure storage emulator.
    const utility::string_t storage_connection_string(U("UseDevelopmentStorage=true;"));  

Az Azure tároló irányító elindításához kattintson a **Start** gombra, vagy nyomja le a Windows billentyűt. Írja be az **Azure tároló irányító**, és az alkalmazások listájából válassza ki **A Microsoft Azure tároló irányító** .  

Az alábbi példa feltételezi, hogy már használta az alábbi két módszerek egyikét a tárhely kapcsolati karakterláncát.  

## <a name="retrieve-your-connection-string"></a>A kapcsolati karakterlánc beolvasása  
A tároló fiókadatok ábrázolásához **cloud_storage_account** osztály is használhatja. A tároló fiókadatok lekérése a tárterület-kapcsolati karakterláncot, a elemzési módszert is használhatja.

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

Ezután kerülni a hivatkozást egy **cloud_table_client** osztály lehetővé teszi, hogy a hivatkozás objektumok beszerzése a táblák és a táblázat tárhelyszolgáltatáshoz tárolva szervezetek. A következő kódrészlet egy **cloud_table_client** objektum feletti visszakeresve, amely azt tárterület-fiók objektum használatával hozza létre:  

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

## <a name="create-a-table"></a>Táblázat létrehozása
Egy **cloud_table_client** objektum lehetővé teszi a táblák és a személyek hivatkozást objektumok beszerzése. A következő kódot létrehoz egy **cloud_table_client** objektumot, és hozzon létre egy új táblát használja.

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);  

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Retrieve a reference to a table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Create the table if it doesn't exist.
    table.create_if_not_exists();  

## <a name="add-an-entity-to-a-table"></a>Egy személy hozzáadása táblához
Egy személy hozzáadása egy táblához, hozzon létre egy új **table_entity** objektumot, és adja át **table_operation::insert_entity**. A következő kódot a vevő első nevét használja a sor billentyűt, és a vezetéknevet partíciót kulcsként. Közös egy személy partíciót és a sor kulcs egyedileg azonosító a szervezet a táblázatban. Személyek ugyanazzal a partíciót kulccsal gyorsabb, mint a másik partíciót billentyűkkel lekérdezhető, de használata különböző partíciót billentyűk lehetővé teszi a nagyobb, párhuzamosan művelet méretezhetőség. További információt a [Microsoft Azure tároló teljesítmény és méretezhetőség feladatlista](storage-performance-checklist.md)témakörben talál.

A következő kódrészlet egy új példányát **table_entity** néhány ügyféladatokkal tárolni hoz létre. A kód következő felhívja **table_operation::insert_entity** entitás beszúrása egy táblázatba **table_operation** objektumot szeretne létrehozni, és az új tábla entitás társít. Végül a kódot a végrehajtás módszer a **cloud_table** objektum hívások. És az új **table_operation** kérést küld a táblázat szolgáltatás az új ügyfél entitás beszúrása a "személyek" táblázatba.  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Retrieve a reference to a table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Create the table if it doesn't exist.
    table.create_if_not_exists();

    // Create a new customer entity.
    azure::storage::table_entity customer1(U("Harp"), U("Walter"));

    azure::storage::table_entity::properties_type& properties = customer1.properties();
    properties.reserve(2);
    properties[U("Email")] = azure::storage::entity_property(U("Walter@contoso.com"));

    properties[U("Phone")] = azure::storage::entity_property(U("425-555-0101"));

    // Create the table operation that inserts the customer entity.
    azure::storage::table_operation insert_operation = azure::storage::table_operation::insert_entity(customer1);

    // Execute the insert operation.
    azure::storage::table_result insert_result = table.execute(insert_operation);

## <a name="insert-a-batch-of-entities"></a>A köteg szervezetek beszúrása
A táblázat szolgáltatás szervezetek egy köteg szúrhatja be egy írási művelet. A következő kódot létrehoz egy **table_batch_operation** objektumot, és hozzáadja három beszúrása műveletek rá. Minden egyes Beszúrás művelet megjelenik egy új egyed-objektum létrehozása, és állítsa be az értékeket, majd hívja fel a Beszúrás módszer az a személy társítása egy új beillesztési művelet **table_batch_operation** objektumra. Ezután **cloud_table.execute** neve végrehajtani a műveletet.  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Define a batch operation.
    azure::storage::table_batch_operation batch_operation;

    // Create a customer entity and add it to the table.
    azure::storage::table_entity customer1(U("Smith"), U("Jeff"));

    azure::storage::table_entity::properties_type& properties1 = customer1.properties();
    properties1.reserve(2);
    properties1[U("Email")] = azure::storage::entity_property(U("Jeff@contoso.com"));
    properties1[U("Phone")] = azure::storage::entity_property(U("425-555-0104"));

    // Create another customer entity and add it to the table.
    azure::storage::table_entity customer2(U("Smith"), U("Ben"));

    azure::storage::table_entity::properties_type& properties2 = customer2.properties();
    properties2.reserve(2);
    properties2[U("Email")] = azure::storage::entity_property(U("Ben@contoso.com"));
    properties2[U("Phone")] = azure::storage::entity_property(U("425-555-0102"));

    // Create a third customer entity to add to the table.
    azure::storage::table_entity customer3(U("Smith"), U("Denise"));

    azure::storage::table_entity::properties_type& properties3 = customer3.properties();
    properties3.reserve(2);
    properties3[U("Email")] = azure::storage::entity_property(U("Denise@contoso.com"));
    properties3[U("Phone")] = azure::storage::entity_property(U("425-555-0103"));

    // Add customer entities to the batch insert operation.
    batch_operation.insert_or_replace_entity(customer1);
    batch_operation.insert_or_replace_entity(customer2);
    batch_operation.insert_or_replace_entity(customer3);

    // Execute the batch operation.
    std::vector<azure::storage::table_result> results = table.execute_batch(batch_operation);

Megjegyzés a köteg műveletek műveleteket:  

-   Egyetlen kötegben tetszőleges legfeljebb 100 Beszúrás, törlés, egyesítés, csere, Beszúrás és körlevelek és Beszúrás vagy csere műveleteket végezheti el.  
-   Egy köteg művelet a lekérés művelet állhat, ha csak a műveletet a köteget.  
-   Egyetlen köteg művelet összes entitás ugyanazt a partíciót kulcsot kell rendelkeznie.  
-   A köteg művelet 4 MB-os adatok hasznos korlátozódik.  

## <a name="retrieve-all-entities-in-a-partition"></a>Az összes partíciót vállalkozások beolvasása
A lekérdezés minden entitás partíciót a táblázat, használja a **table_query** objektum. A következő példa adja meg a személyek hol "Kovács"-e a partíciót kulcs szűrőt. Ebben a példában a mezőket a lekérdezés eredményében a konzolhoz minden személy nyomtatja.  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Construct the query operation for all customer entities where PartitionKey="Smith".
    azure::storage::table_query query;

    query.set_filter_string(azure::storage::table_query::generate_filter_condition(U("PartitionKey"), azure::storage::query_comparison_operator::equal, U("Smith")));

    // Execute the query.
    azure::storage::table_query_iterator it = table.execute_query(query);

    // Print the fields for each customer.
    azure::storage::table_query_iterator end_of_results;
    for (; it != end_of_results; ++it)
    {
        const azure::storage::table_entity::properties_type& properties = it->properties();

        std::wcout << U("PartitionKey: ") << it->partition_key() << U(", RowKey: ") << it->row_key()
            << U(", Property1: ") << properties.at(U("Email")).string_value()
            << U(", Property2: ") << properties.at(U("Phone")).string_value() << std::endl;
    }  

Ebben a példában a lekérdezés megjeleníti a szűrő feltételeknek megfelelő összes entitás. Ha nagy táblázatok és van szüksége a táblázat személyek letöltéséhez gyakran, akkor javasoljuk, hogy Ön tárolja az adatokat tároló Azure BLOB helyette.

## <a name="retrieve-a-range-of-entities-in-a-partition"></a>Személyek felvétele egy cellatartomány beolvasása
Ha nem szeretné a lekérdezés partíciót az összes entitás, megadhatja a tartomány a partíciót kulcs szűrő sor kulcs szűrővel kombinálva. A következő példa két szűrő használatával egyben az összes entitás partíciót "Kovács", ahol a sor kulcs (Utónév) verziónál az ábécé "E" betűvel kezdődik, és majd kinyomtatja a lekérdezés eredményét.  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Create the table query.
    azure::storage::table_query query;

    query.set_filter_string(azure::storage::table_query::combine_filter_conditions(
        azure::storage::table_query::generate_filter_condition(U("PartitionKey"),
        azure::storage::query_comparison_operator::equal, U("Smith")),
        azure::storage::query_logical_operator::op_and,
        azure::storage::table_query::generate_filter_condition(U("RowKey"), azure::storage::query_comparison_operator::less_than, U("E"))));

    // Execute the query.
    azure::storage::table_query_iterator it = table.execute_query(query);

    // Loop through the results, displaying information about the entity.
    azure::storage::table_query_iterator end_of_results;
    for (; it != end_of_results; ++it)
    {
        const azure::storage::table_entity::properties_type& properties = it->properties();

        std::wcout << U("PartitionKey: ") << it->partition_key() << U(", RowKey: ") << it->row_key()
            << U(", Property1: ") << properties.at(U("Email")).string_value()
            << U(", Property2: ") << properties.at(U("Phone")).string_value() << std::endl;
    }  

## <a name="retrieve-a-single-entity"></a>Egyetlen egységet beolvasása
Egy adott személy beolvasásához lekérdezés is írhat. A következő kódot **table_operation::retrieve_entity** segítségével adja meg az ügyfél "Kovács". Ez a módszer adja eredményül egy webhelycsoport helyett csak egy személy, és a visszaadott érték szerepel-e **table_result**. Ez leggyorsabban egy entitás beolvasni a táblázat szolgáltatásból való partíciót vagy a sor megadása a lekérdezés.  

    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Retrieve the entity with partition key of "Smith" and row key of "Jeff".
    azure::storage::table_operation retrieve_operation = azure::storage::table_operation::retrieve_entity(U("Smith"), U("Jeff"));
    azure::storage::table_result retrieve_result = table.execute(retrieve_operation);

    // Output the entity.
    azure::storage::table_entity entity = retrieve_result.entity();
    const azure::storage::table_entity::properties_type& properties = entity.properties();

    std::wcout << U("PartitionKey: ") << entity.partition_key() << U(", RowKey: ") << entity.row_key()
        << U(", Property1: ") << properties.at(U("Email")).string_value()
        << U(", Property2: ") << properties.at(U("Phone")).string_value() << std::endl;

## <a name="replace-an-entity"></a>Egy személy cseréje
Entitás cseréjéhez megtalálja a táblázat szolgáltatásból, entitás objektumok módosíthatók, és mentse a változtatásokat az a tábla-szolgáltatásba. A következő kódrészlet egy meglévő vevő telefonszám és e-mail címe változik. Helyett **table_operation::insert_entity**hang-és a kód **table_operation::replace_entity**használja. A szervezet teljesen cserélendő a kiszolgálón ennek hatására, kivéve, ha a szervezet a kiszolgálón megváltozott a beolvasás, amely esetben a művelet sikertelen lesz óta. Ezt a hibát is megakadályozhatja, hogy véletlenül a frissítés és a lekérés között egy másik összetevő az alkalmazás által elvégzett módosítás felülírja az alkalmazást. A megfelelő kezelését, ezt a hibát, hogy a szervezet ismételt lekéréséhez, végezze el a módosításokat (Ha még érvényes), majd végrehajtása egy másik **table_operation::replace_entity** . A következő szakaszban kell követnie Ez a működési módjának felülbírálására.  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Replace an entity.
    azure::storage::table_entity entity_to_replace(U("Smith"), U("Jeff"));
    azure::storage::table_entity::properties_type& properties_to_replace = entity_to_replace.properties();
    properties_to_replace.reserve(2);

    // Specify a new phone number.
    properties_to_replace[U("Phone")] = azure::storage::entity_property(U("425-555-0106"));

    // Specify a new email address.
    properties_to_replace[U("Email")] = azure::storage::entity_property(U("JeffS@contoso.com"));

    // Create an operation to replace the entity.
    azure::storage::table_operation replace_operation = azure::storage::table_operation::replace_entity(entity_to_replace);

    // Submit the operation to the Table service.
    azure::storage::table_result replace_result = table.execute(replace_operation);

## <a name="insert-or-replace-an-entity"></a>Beszúrás – vagy – csere entitás
**table_operation::replace_entity** műveletek sikertelen lesz, ha a szervezet óta lehívni a kiszolgálóról megváltozott. Ezenkívül a szervezet kell lekérni a kiszolgálóról, először annak érdekében, hogy **table_operation::replace_entity** sikeres. Egyes esetekben azonban nem tudja, hogy ha a szervezet a kiszolgálón található, és az aktuális benne tárolt értékei nem számít – a frissítés célszerű felülírni összes. Ehhez használja egy **table_operation::insert_or_replace_entity** műveletet. Ez a művelet szúrja be a személy, ha nem létezik, vagy ha igen, függetlenül attól, amikor az utolsó frissítés elvégezték váltja fel. A következő példában kódot az ügyfél entitás Kovács a program továbbra is, de majd mentésekor a kiszolgálón keresztül **table_operation::insert_or_replace_entity**. A szervezet a lekérés és a frissítési művelet között frissítéseiről felülíródnak.  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Insert-or-replace an entity.
    azure::storage::table_entity entity_to_insert_or_replace(U("Smith"), U("Jeff"));
    azure::storage::table_entity::properties_type& properties_to_insert_or_replace = entity_to_insert_or_replace.properties();

    properties_to_insert_or_replace.reserve(2);

    // Specify a phone number.
    properties_to_insert_or_replace[U("Phone")] = azure::storage::entity_property(U("425-555-0107"));

    // Specify an email address.
    properties_to_insert_or_replace[U("Email")] = azure::storage::entity_property(U("Jeffsm@contoso.com"));

    // Create an operation to insert-or-replace the entity.
    azure::storage::table_operation insert_or_replace_operation = azure::storage::table_operation::insert_or_replace_entity(entity_to_insert_or_replace);

    // Submit the operation to the Table service.
    azure::storage::table_result insert_or_replace_result = table.execute(insert_or_replace_operation);

## <a name="query-a-subset-of-entity-properties"></a>A lekérdezés csak egy részhalmazát entitás tulajdonságai  
Táblázat lekérdezés néhány tulajdonságok beolvasható entitás. A lekérdezés, a következő kódrészlet használja a **table_query::set_select_columns** módszer csak személyek e-mail címét a táblázatban.  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Define the query, and select only the Email property.
    azure::storage::table_query query;
    std::vector<utility::string_t> columns;

    columns.push_back(U("Email"));
    query.set_select_columns(columns);

    // Execute the query.
    azure::storage::table_query_iterator it = table.execute_query(query);

    // Display the results.
    azure::storage::table_query_iterator end_of_results;
    for (; it != end_of_results; ++it)
    {
        std::wcout << U("PartitionKey: ") << it->partition_key() << U(", RowKey: ") << it->row_key();

        const azure::storage::table_entity::properties_type& properties = it->properties();
        for (auto prop_it = properties.begin(); prop_it != properties.end(); ++prop_it)
        {
            std::wcout << ", " << prop_it->first << ": " << prop_it->second.str();
        }

        std::wcout << std::endl;
    }

>[AZURE.NOTE] Hatékonyabb működését, mint az összes tulajdonság beolvasása entitás néhány tulajdonságok lekérdezése.

## <a name="delete-an-entity"></a>Entitás törlése
Egy személy beolvasása azt után könnyen törölheti. Miután a szervezet veszi, hívja fel a **table_operation::delete_entity** az a személy törlése. A **cloud_table.execute** módszer akkor hívja meg. A következő kódot, és az entitás töröl egy partíciót kulcsa "Kovács" és "Kovács" egy sor kulcsa.  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Create an operation to retrieve the entity with partition key of "Smith" and row key of "Jeff".
    azure::storage::table_operation retrieve_operation = azure::storage::table_operation::retrieve_entity(U("Smith"), U("Jeff"));
    azure::storage::table_result retrieve_result = table.execute(retrieve_operation);

    // Create an operation to delete the entity.
    azure::storage::table_operation delete_operation = azure::storage::table_operation::delete_entity(retrieve_result.entity());

    // Submit the delete operation to the Table service.
    azure::storage::table_result delete_result = table.execute(delete_operation);  

## <a name="delete-a-table"></a>Táblázat törlése
Végül a következő példa tárterület-fiókból törli a táblát. Egy táblázat, amely a törlése után nem érhetők el egy ideig a törlés után újra létre kell lesz.  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Create an operation to retrieve the entity with partition key of "Smith" and row key of "Jeff".
    azure::storage::table_operation retrieve_operation = azure::storage::table_operation::retrieve_entity(U("Smith"), U("Jeff"));
    azure::storage::table_result retrieve_result = table.execute(retrieve_operation);

    // Create an operation to delete the entity.
    azure::storage::table_operation delete_operation = azure::storage::table_operation::delete_entity(retrieve_result.entity());

    // Submit the delete operation to the Table service.
    azure::storage::table_result delete_result = table.execute(delete_operation);

## <a name="next-steps"></a>Következő lépések
Most, hogy megtanulta táblatároló alapjait, kövesse ezeket a hivatkozásokat, ha többet szeretne tudni az Azure tároló:  

-   [Használatáról a C++ Blob-tárolóhoz](storage-c-plus-plus-how-to-use-blobs.md)
-   [A C++ várólista-tároló használata](storage-c-plus-plus-how-to-use-queues.md)
-   [A lista C++ Azure tároló-erőforrások](storage-c-plus-plus-enumeration.md)
-   [Tárterület ügyfél tár C++ hivatkozás](http://azure.github.io/azure-storage-cpp)
-   [Azure tároló dokumentáció](https://azure.microsoft.com/documentation/services/storage/)
