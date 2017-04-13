<properties
    pageTitle="A PHP táblatároló használata |} Microsoft Azure"
    description="A táblázat PHP szolgáltatás használata létrehozása és törlése a táblához, és beszúrása, törlése és lekérdezés a táblázat."
    services="storage"
    documentationCenter="php"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="php"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>


# <a name="how-to-use-table-storage-from-php"></a>A PHP táblatároló használata

[AZURE.INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>– Áttekintés

Ez az útmutató megtudhatja, miként végezheti el a gyakori alkalmazási területek, az Azure-táblából szolgáltatással. A minták PHP írt, és használja az [Azure SDK PHP][download]. Érintett esetek **létrehozása és töröl egy táblát, és beszúrása, törlése, és lekérdezés a szervezetek egy táblázatban**. Az Azure-táblából szolgáltatása további tudnivalókért lásd: a [következő lépésekkel](#next-steps) szakaszban.

[AZURE.INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-php-application"></a>PHP-alkalmazás létrehozása

Csak egy az Azure-táblából szolgáltatás hozzáférő PHP-alkalmazás létrehozása kötelező, a hivatkozási az Azure SDK az osztályok a PHP-belül a kódot. Bármely Fejlesztőeszközök hozhat létre a az alkalmazásokban, például a Jegyzettömbben.

Ebből az útmutatóból szolgáltatás táblázatszolgáltatások hívható a helyi meghajtóra PHP alkalmazásból, illetve az Azure webes szerepkör, a dolgozó szerepkör és a webhely futó kód, amely használja.

## <a name="get-the-azure-client-libraries"></a>Ismerkedés az Azure ügyfél-tárak

[AZURE.INCLUDE [get-client-libraries](../../includes/get-client-libraries.md)]

## <a name="configure-your-application-to-access-the-table-service"></a>A táblázat szolgáltatás eléréséhez az alkalmazás beállítása

Az Azure-táblából szolgáltatás API-khoz használatához kell:

1. Hivatkozás az automatikus betöltő fájlt a [require_once] [ require_once] utasítás, és
2. Hivatkozás bármelyik osztályok használhat.

A következő példa bemutatja, hogyan lehet az automatikus betöltő fájl és hivatkozást a **ServicesBuilder** osztály.

> [AZURE.NOTE] Ez a példa (és más, a jelen cikkben szereplő példák) tegyük fel, a PHP ügyfél képtárak Azure zeneszerző keresztül telepítette. A tárak manuálisan telepíteni, szükségességének hivatkozni szeretne a <code>WindowsAzure.php</code> automatikus betöltő fájlt.

    require_once 'vendor/autoload.php';
    use WindowsAzure\Common\ServicesBuilder;


Az alábbi példában a `require_once` utasítás mindig látható, de csak az osztályok a példa végrehajtásához szükséges hivatkozunk.

## <a name="set-up-an-azure-storage-connection"></a>Azure tárterület-alapú beállítása

Az Azure-táblából szolgáltatás ügyfél elindítását, először rendelkeznie kell érvényes kapcsolat-karakterlánc. Az a táblázat szolgáltatás kapcsolati karakterlánc esetén:

Élő szolgáltatás eléréséhez:

    DefaultEndpointsProtocol=[http|https];AccountName=[yourAccount];AccountKey=[yourKey]

A irányító tároló eléréséhez:

    UseDevelopmentStorage=true


Azure service jegyzetlapon létrehozásához kell használni az **ServicesBuilder** osztály. képes vagy:

* átadni a kapcsolati karakterlánc közvetlenül vagy
* Jelölje be a kapcsolati karakterlánc több külső forrásból használja a **CloudConfigurationManager (CCM)** :
    * Alapértelmezés szerint előtelepített egy külső adatforrás - környezeti változók támogatása
    * a **ConnectionStringSource** osztály időtartamának felvehet új források

Az itt ismertetett példák a kapcsolati karakterlánc fog átadandó közvetlenül.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;

    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);


## <a name="create-a-table"></a>Táblázat létrehozása

Hozzon létre egy táblázatot a **createTable** módszer egy **TableRestProxy** objektum segítségével. Tábla létrehozása a táblázat szolgáltatás időtúllépése azt is megadhatja. (A táblázat szolgáltatás időtúllépésről szóló további információkért lásd a [Táblázat szolgáltatási műveletek beállítás időtúllépései][table-service-timeouts].)

    require_once 'vendor\autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    try {
        // Create table.
        $tableRestProxy->createTable("mytable");
    }
    catch(ServiceException $e){
        $code = $e->getCode();
        $error_message = $e->getMessage();
        // Handle exception based on error codes and messages.
        // Error codes and messages can be found here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
    }

Korlátozások a táblázatnevek információt talál [az táblázat szolgáltatás adatmodell ismertetése][table-data-model].

## <a name="add-an-entity-to-a-table"></a>Egy személy hozzáadása táblához

Entitás hozzáadása egy táblához, hozzon létre egy új **Egyed** -objektumot, és adja át **TableRestProxy -> insertEntity**. Megfigyelheti, hogy entitás létrehozásakor meg kell adnia egy `PartitionKey` és `RowKey`. Ezek a egyedi azonosítókat entitás és értékek sokkal gyorsabban többi személy tulajdonságok lekérdezett. A rendszer által használt `PartitionKey` automatikusan terjesztheti személyek a táblázat sok tároló csomópontok fölé. Az ugyanazon szervezetek `PartitionKey` ugyanazon a csomóponton tárolja. (A több személy ugyanazon a csomóponton tárolt műveletek végrehajtása jobb, mint a szervezetek keresztül csomópontjai tárolt.) A `RowKey` belül partíciót entitás egyedi azonosítója.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;
    use MicrosoftAzure\Storage\Table\Models\Entity;
    use MicrosoftAzure\Storage\Table\Models\EdmType;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    $entity = new Entity();
    $entity->setPartitionKey("tasksSeattle");
    $entity->setRowKey("1");
    $entity->addProperty("Description", null, "Take out the trash.");
    $entity->addProperty("DueDate",
                         EdmType::DATETIME,
                         new DateTime("2012-11-05T08:15:00-08:00"));
    $entity->addProperty("Location", EdmType::STRING, "Home");

    try{
        $tableRestProxy->insertEntity("mytable", $entity);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
    }

Táblázat tulajdonságai és típusú információt talál [az táblázat szolgáltatás adatmodell ismertetése][table-data-model].

A **TableRestProxy** osztály kínál szervezetek beszúrására szolgáló két alternatív módszer: **insertOrMergeEntity** és **insertOrReplaceEntity**. Ezeket a módszereket, hozzon létre egy új **entitás** és paraméterként átadni mindkét módszer. Mindegyik módszernek szúrja be a személy, ha még nem létezik. Ha a szervezet már létezik, **insertOrMergeEntity** frissíti a tulajdonság értékeit, ha már megtalálható a tulajdonságok, és hozzáadja az új tulajdonságait, ha nem létezik, míg a **insertOrReplaceEntity** teljesen lecseréli a meglévő entitás. A következő példa bemutatja a **insertOrMergeEntity**használatáról. Ha egyed `PartitionKey` "tasksSeattle" és `RowKey` "1" még nem létezik, szúrja be. Azonban azt korábban beszúrt (mint a fenti példában látható), ha a `DueDate` tulajdonság frissülnek, és a `Status` tulajdonság, hozzáadódik. A `Description` és `Location` tulajdonságok is frissülnek, de értékű, amely hatékony meghagyása eredeti változatlan marad. Ha ezeket a két utóbbi tulajdonságokat volt nem példában látható módon megjelenik, de a cél entitás létezik, értékükre volna változatlanok maradnak.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;
    use MicrosoftAzure\Storage\Table\Models\Entity;
    use MicrosoftAzure\Storage\Table\Models\EdmType;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    //Create new entity.
    $entity = new Entity();

    // PartitionKey and RowKey are required.
    $entity->setPartitionKey("tasksSeattle");
    $entity->setRowKey("1");

    // If entity exists, existing properties are updated with new values and
    // new properties are added. Missing properties are unchanged.
    $entity->addProperty("Description", null, "Take out the trash.");
    $entity->addProperty("DueDate", EdmType::DATETIME, new DateTime()); // Modified the DueDate field.
    $entity->addProperty("Location", EdmType::STRING, "Home");
    $entity->addProperty("Status", EdmType::STRING, "Complete"); // Added Status field.

    try {
        // Calling insertOrReplaceEntity, instead of insertOrMergeEntity as shown,
        // would simply replace the entity with PartitionKey "tasksSeattle" and RowKey "1".
        $tableRestProxy->insertOrMergeEntity("mytable", $entity);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }


## <a name="retrieve-a-single-entity"></a>Egyetlen egységet beolvasása

A **TableRestProxy -> getEntity** módszer lehetővé teszi, hogy egy entitás beolvasása lekérdezésével annak `PartitionKey` és `RowKey`. A partíciók billentyűt az alábbi példában a `tasksSeattle` és sor kulcs `1` át a **getEntity** módszer a.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    try {
        $result = $tableRestProxy->getEntity("mytable", "tasksSeattle", 1);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

    $entity = $result->getEntity();

    echo $entity->getPartitionKey().":".$entity->getRowKey();

## <a name="retrieve-all-entities-in-a-partition"></a>Az összes partíciót vállalkozások beolvasása

Személy lekérdezések épülnek fel szűrők használata (további tudnivalókért lásd: a [táblázatok lekérdezése és szervezetek][filters]). Az összes partíciót vállalkozások beolvasásához, használja a szűrő "PartitionKey eq *partíció_neve*". A következő példa bemutatja, hogyan az összes entitás beolvasni a `tasksSeattle` partíciót a **queryEntities** módszerrel szűrő megkerülhetők.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    $filter = "PartitionKey eq 'tasksSeattle'";

    try {
        $result = $tableRestProxy->queryEntities("mytable", $filter);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

    $entities = $result->getEntities();

    foreach($entities as $entity){
        echo $entity->getPartitionKey().":".$entity->getRowKey()."<br />";
    }

## <a name="retrieve-a-subset-of-entities-in-a-partition"></a>Személyek felvétele egy részét beolvasása

Az előző példában használt azonos típusúak használható beolvasásához partíciót a szervezetek bármely részét. Személyek beolvashatja részhalmazát határozzák meg a szűrő használata (további tudnivalókért lásd: a [táblázatok lekérdezése és szervezetek][filters]). A következő példa bemutatja, hogyan szűrő segítségével beolvashatja egy adott összes entitás `Location` és egy `DueDate` kisebb, mint egy adott dátum.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    $filter = "Location eq 'Office' and DueDate lt '2012-11-5'";

    try {
        $result = $tableRestProxy->queryEntities("mytable", $filter);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

    $entities = $result->getEntities();

    foreach($entities as $entity){
        echo $entity->getPartitionKey().":".$entity->getRowKey()."<br />";
    }

## <a name="retrieve-a-subset-of-entity-properties"></a>Beolvashatja egy részét entitás tulajdonságai

A lekérdezés meghallgathatja entitás a tulajdonságok egy részét. Ez a módszer *kivetítés*nevű csökkenti a sávszélesség, és javíthatja a lekérdezési teljesítményt, különösen a nagyobb szervezetek. A **Lekérdezés -> addSelectField** módszer a tulajdonság neve át lesz visszakeresve, amely tulajdonság megadásához. Is többször hív meg ezt a módszert hozzáadni, hogy más tulajdonságokat. **TableRestProxy -> queryEntities**végrehajtja a visszaadott személyek csak a kijelölt tulajdonságok van. (Vissza a tábla szervezetek csak egy részhalmazát szeretné, ha szűrővel ahogy a fenti lekérdezések.)

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;
    use MicrosoftAzure\Storage\Table\Models\QueryEntitiesOptions;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    $options = new QueryEntitiesOptions();
    $options->addSelectField("Description");

    try {
        $result = $tableRestProxy->queryEntities("mytable", $options);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

    // All entities in the table are returned, regardless of whether
    // they have the Description field.
    // To limit the results returned, use a filter.
    $entities = $result->getEntities();

    foreach($entities as $entity){
        $description = $entity->getProperty("Description")->getValue();
        echo $description."<br />";
    }

## <a name="update-an-entity"></a>Egy személy frissítése

Meglévő entitás **Egyed -> tulajdonságbeállítása** és **Egyed -> addProperty** módszerekkel a entitás, és kattintson a **TableRestProxy -> updateEntity**hívása frissíthető. A következő példa beolvassa entitás, módosítja egy tulajdonságot, szünteti meg egy másik tulajdonság és felveszi az új tulajdonság. Figyelje meg, hogy egy tulajdonságot eltávolíthatja az érték megadásával **Null**.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;
    use MicrosoftAzure\Storage\Table\Models\Entity;
    use MicrosoftAzure\Storage\Table\Models\EdmType;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    $result = $tableRestProxy->getEntity("mytable", "tasksSeattle", 1);

    $entity = $result->getEntity();

    $entity->setPropertyValue("DueDate", new DateTime()); //Modified DueDate.

    $entity->setPropertyValue("Location", null); //Removed Location.

    $entity->addProperty("Status", EdmType::STRING, "In progress"); //Added Status.

    try {
        $tableRestProxy->updateEntity("mytable", $entity);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

## <a name="delete-an-entity"></a>Entitás törlése

Egy személy törléséhez adják át a táblázat neve és a szervezet által `PartitionKey` és `RowKey` a **TableRestProxy -> deleteEntity** módszerrel.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    try {
        // Delete entity.
        $tableRestProxy->deleteEntity("mytable", "tasksSeattle", "2");
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

Figyelje meg, hogy feldolgozási ellenőrzését, beállíthatja, hogy a Etag entitás törlődik a **DeleteEntityOptions -> setEtag** módszerrel, és átadja a **DeleteEntityOptions** objektum **deleteEntity** , mint egy negyedik paraméter.

## <a name="batch-table-operations"></a>Köteg táblázat műveletek

A **Köteg TableRestProxy ->** módszerrel egyetlen kérelem több műveletet hajtson végre. A minta itt magában foglalja a műveletek hozzáadása **BatchRequest** objektumra, és majd átadja a **BatchRequest** objektum **TableRestProxy -> Köteg** módszer. Művelet hozzáadása egy **BatchRequest** objektumhoz, is többször hív meg az alábbi módszerek egyikét:

* **addInsertEntity** (insertEntity művelet hozzáadása)
* **addUpdateEntity** (updateEntity művelet hozzáadása)
* **addMergeEntity** (hozzáad egy mergeEntity művelet)
* **addInsertOrReplaceEntity** (insertOrReplaceEntity művelet hozzáadása)
* **addInsertOrMergeEntity** (insertOrMergeEntity művelet hozzáadása)
* **addDeleteEntity** (hozzáad egy deleteEntity művelet)

A következő példa bemutatja, hogyan egyetlen kérelem **insertEntity** és **deleteEntity** műveleteket hajtson végre:

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;
    use MicrosoftAzure\Storage\Table\Models\Entity;
    use MicrosoftAzure\Storage\Table\Models\EdmType;
    use MicrosoftAzure\Storage\Table\Models\BatchOperations;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    // Create list of batch operation.
    $operations = new BatchOperations();

    $entity1 = new Entity();
    $entity1->setPartitionKey("tasksSeattle");
    $entity1->setRowKey("2");
    $entity1->addProperty("Description", null, "Clean roof gutters.");
    $entity1->addProperty("DueDate",
                          EdmType::DATETIME,
                          new DateTime("2012-11-05T08:15:00-08:00"));
    $entity1->addProperty("Location", EdmType::STRING, "Home");

    // Add operation to list of batch operations.
    $operations->addInsertEntity("mytable", $entity1);

    // Add operation to list of batch operations.
    $operations->addDeleteEntity("mytable", "tasksSeattle", "1");

    try {
        $tableRestProxy->batch($operations);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

Táblázat műveletek kötegelés kapcsolatos további tudnivalókért lásd a [Elvégzéséhez entitás csoport tranzakciók][entity-group-transactions].

## <a name="delete-a-table"></a>Táblázat törlése

Végül ha törölni szeretne egy táblát, adják át a táblázat neve a **TableRestProxy -> deleteTable** módszerrel.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    try {
        // Delete table.
        $tableRestProxy->deleteTable("mytable");
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

## <a name="next-steps"></a>Következő lépések

Most, hogy megtanulta az Azure-táblából szolgáltatás alapjait, kövesse ezeket a hivatkozásokat, ha többet szeretne tudni az összetettebb tároló tevékenységek.

- Keresse fel az [Azure tároló csapatának blogja](http://blogs.msdn.com/b/windowsazurestorage/)

További tudnivalókért lásd még: a [PHP Developer Center](/develop/php/).

[download]: http://go.microsoft.com/fwlink/?LinkID=252473
[require_once]: http://php.net/require_once
[table-service-timeouts]: http://msdn.microsoft.com/library/azure/dd894042.aspx

[table-data-model]: http://msdn.microsoft.com/library/azure/dd179338.aspx
[filters]: http://msdn.microsoft.com/library/azure/dd894031.aspx
[entity-group-transactions]: http://msdn.microsoft.com/library/azure/dd894038.aspx
