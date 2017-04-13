<properties
    pageTitle="Azure táblatárolóhoz a Node.js használata |} Microsoft Azure"
    description="Strukturált adatokat tárolja a felhőben Azure táblatárolóhoz, egy NoSQL adattár használatával."
    services="storage"
    documentationCenter="nodejs"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="tamram"/>


# <a name="how-to-use-azure-table-storage-from-nodejs"></a>Azure táblatárolóhoz Node.js való használatáról

[AZURE.INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>– Áttekintés

Ez a témakör bemutatja, hogyan végezhetők el a gyakori alkalmazási területek, az Azure-táblából szolgáltatással Node.js alkalmazásban.

Ebben a témakörben mintakódok tegyük fel, már van egy Node.js alkalmazást. Információ arról, hogy miként Node.js-alkalmazás létrehozása az Azure-ban, akkor olvassa el az alábbi témakörök egyikét:

- [Node.js webalkalmazást létrehozása az Azure alkalmazás szolgáltatás](../app-service-web/web-sites-nodejs-develop-deploy-mac.md)
- [Építse fel és telepítse a Node.js webalkalmazást az Azure WebMatrix használatával](../app-service-web/web-sites-nodejs-use-webmatrix.md)
- [Építse fel és telepítse az Azure Felhőszolgáltatásba egy Node.js alkalmazás](../cloud-services/cloud-services-nodejs-develop-deploy-app.md) (a Windows PowerShell használatá)


[AZURE.INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]


## <a name="configure-your-application-to-access-azure-storage"></a>Azure tároló eléréséhez az alkalmazás beállítása

Azure tárolási használatához szükséges az Azure tárolási SDK Node.js egy sor olyan kényelmesebbé olyan tárat, amellyel kommunikálni a tároló többi szolgáltatást tartalmazó.

### <a name="use-node-package-manager-npm-to-install-the-package"></a>A csomag telepítéséhez csomópont csomag Manager (NPM) használatával

1.  A parancssor, például **PowerShell** (Windows), a **terminált** (Mac) vagy a **Bash** (Unix) használja, és nyissa meg azt a mappát, amelyen létrehozta az alkalmazást.

2.  Írja be a **npm telepítse az azure-tárhelyet** a parancssorablakban. A parancs az alábbi példa hasonlít.

        azure-storage@0.5.0 node_modules\azure-storage
        +-- extend@1.2.1
        +-- xmlbuilder@0.4.3
        +-- mime@1.2.11
        +-- node-uuid@1.4.3
        +-- validator@3.22.2
        +-- underscore@1.4.4
        +-- readable-stream@1.0.33 (string_decoder@0.10.31, isarray@0.0.1, inherits@2.0.1, core-util-is@1.0.1)
        +-- xml2js@0.2.7 (sax@0.5.2)
        +-- request@2.57.0 (caseless@0.10.0, aws-sign2@0.5.0, forever-agent@0.6.1, stringstream@0.0.4, oauth-sign@0.8.0, tunnel-agent@0.4.1, isstream@0.1.2, json-stringify-safe@5.0.1, bl@0.9.4, combined-stream@1.0.5, qs@3.1.0, mime-types@2.0.14, form-data@0.2.0, http-signature@0.11.0, tough-cookie@2.0.0, hawk@2.3.1, har-validator@1.8.0)

3.  Manuális futtatását is lehetővé teszi annak ellenőrzésére, hogy az **ls** parancs a **csomópont\_modulok** mappa létrehozásának. Adott mappában találja az **azure-tárhely** csomagra, mely tartalmazza a tárak tárolási eléréséhez szükséges.

### <a name="import-the-package"></a>A csomag importálása

A következő kód hozzáadása a **server.js** fájlt az alkalmazás tetején:

    var azure = require('azure-storage');

## <a name="set-up-an-azure-storage-connection"></a>Tárterület Azure-alapú beállítása

Az azure modul felirat jelenik meg a környezeti változók AZURE\_tároló\_fiók és AZURE\_tároló\_ACCESS\_BILLENTYŰT, vagy az AZURE\_tároló\_kapcsolat\_karakterlánc az Azure tároló fiókhoz való csatlakozáshoz szükséges információkat. Ha ezek a környezeti változók nincsenek beállítva, **TableService**hívásakor meg kell adnia a fiók adatait.

Példa a környezeti változók beállítása az [Azure portál](https://portal.azure.com) az Azure-webhelyhez [az Azure-táblából szolgáltatással Node.js webalkalmazás]talál.

## <a name="create-a-table"></a>Táblázat létrehozása

A következő kódot létrehoz egy **TableService** objektumot, és hozzon létre egy új táblát használja. Adja hozzá a következő **server.js**felső részén.

    var tableSvc = azure.createTableService();

A hívás **createTableIfNotExists** az még nem létezik új tábla létrehozása a megadott névvel. Az alábbi példa létrehoz egy új táblát "táblanév" nevű, ha még nem létezik:

    tableSvc.createTableIfNotExists('mytable', function(error, result, response){
      if(!error){
        // Table exists or created
      }
    });

A `result.created` lesz `true` , ha egy új táblát hoz létre, és `false` , ha a táblázat már létezik. A `response` a kérelem információt fog tartalmazni.

### <a name="filters"></a>Szűrők

Választható szűrési műveletek végrehajtott műveletek naplóadatai **TableService**alkalmazhatók. Szűrési műveletek is elhelyezhet, naplózás, automatikusan újra próbálkozik stb. Szűrők olyan objektumok, egy módszer a aláírással végrehajtása:

    function handle (requestOptions, next)

Után hajtsa végre a előfeldolgozása az kérelem beállításai, módszer kell "Tovább" hívás átadása a visszahívás, a következő aláírással:

    function (returnObject, finalCallback, next)

A visszahívási, és a returnObject (válasza a kérelmet a kiszolgálón) feldolgozás után a visszahívás meghívása Ezután ha létezik továbbra is a további szűrők feldolgozása vagy egyszerűen finalCallback egyébként a szolgáltatás hívás befejezése van szüksége.

Két szűrő látható, hogy ismét logikájának megvalósítása is megjelennek az Azure SDK Node.js, **ExponentialRetryPolicyFilter** és **LinearRetryPolicyFilter**. A következő objektumot hoz létre **TableService** , amely a **ExponentialRetryPolicyFilter**használja:

    var retryOperations = new azure.ExponentialRetryPolicyFilter();
    var tableSvc = azure.createTableService().withFilter(retryOperations);

## <a name="add-an-entity-to-a-table"></a>Egy személy hozzáadása táblához

Entitás felvenni, először létre kell hoznia egy objektum, amely meghatározza a szervezet tulajdonságainak. Az összes szervezetek tartalmaznia kell egy **PartitionKey** és **RowKey**, amelyek a szervezet egyedi azonosítóját.

* **PartitionKey** – határozza meg, hogy a partíciók, amely a szervezet található

* **RowKey** - egyedileg kell azonosítania a szervezet a partíciót belül

**PartitionKey** és a **RowKey** megadott értékeket kell lennie. További tudnivalókért olvassa el a [a táblázat szolgáltatás-adatmodell ismertetése](http://msdn.microsoft.com/library/azure/dd179338.aspx)című témakört.

Az alábbi képen egy entitás meghatározása. Figyelje meg, hogy **dueDate** egy **Edm.DateTime**típusú definíciója. A típus megadása nem kötelező, és típusok fog kell következtetett Ha nincs megadva.

    var task = {
      PartitionKey: {'_':'hometasks'},
      RowKey: {'_': '1'},
      description: {'_':'take out the trash'},
      dueDate: {'_':new Date(2015, 6, 20), '$':'Edm.DateTime'}
    };

> [AZURE.NOTE] Minden rekord, amely Azure szerint be van állítva, amikor egy entitás beszúrt vagy frissített **időbélyegző** mezőt is van.

A **entityGenerator** segítségével szervezetek. Az alábbi példa létrehoz ugyanahhoz a tevékenység személyhez a **entityGenerator**használatával.

    var entGen = azure.TableUtilities.entityGenerator;
    var task = {
      PartitionKey: entGen.String('hometasks'),
      RowKey: entGen.String('1'),
      description: entGen.String('take out the trash'),
      dueDate: entGen.DateTime(new Date(Date.UTC(2015, 6, 20))),
    };

Entitás hozzáadni a táblázathoz, átadni a **insertEntity** módszer a szervezet objektum.

    tableSvc.insertEntity('mytable',task, function (error, result, response) {
      if(!error){
        // Entity inserted
      }
    });

Ha a művelet sikeresen, `result` fogja tartalmazni a [ETag](http://en.wikipedia.org/wiki/HTTP_ETag) a beszúrt rekord és `response` a művelet információt fog tartalmazni.

Példa válasz:

    { '.metadata': { etag: 'W/"datetime\'2015-02-25T01%3A22%3A22.5Z\'"' } }

> [AZURE.NOTE] Alapértelmezés szerint **insertEntity** függvény nem ad vissza a beszúrt entitás részeként a `response` információkat. Ha a szervezet más műveletek elvégzéséhez a terv, vagy szeretné az adatokat a gyorsítótárban, célszerű lehet, hogy azt a részét adja vissza az `result`. Ezt megteheti a következőképpen **echoContent** engedélyezésével:
>
> `tableSvc.insertEntity('mytable', task, {echoContent: true}, function (error, result, response) {...}`

## <a name="update-an-entity"></a>Egy személy frissítése

Több rendelkezésre állnak módszerek a meglévő entitás frissítése:

* **replaceEntity** - meglévő entitás frissíti a csere

* **mergeEntity** - meglévő entitás frissíti a meglévő entitás új tulajdonság értékek egyesítése

* **insertOrReplaceEntity** - meglévő entitás frissíti a C2: C7 alakú azt. Ha nincs entitás létezik, egy új szúrja be a

* Új tulajdonság értékek egyesítése a meglévő **insertOrMergeEntity** - meglévő entitás frissíti. Ha nincs entitás létezik, egy új szúrja be a

Az alábbi példa bemutatja, hogy egy **replaceEntity**használatával entitás frissítése:

    tableSvc.replaceEntity('mytable', updatedTask, function(error, result, response){
      if(!error) {
        // Entity updated
      }
    });

> [AZURE.NOTE] Alapértelmezés szerint egy entitás frissítése nem ellenőrizze, hogy ha az adatok frissítése a korábban módosították egy másik folyamat. A támogatási egyidejű frissítések:
>
> 1. Ismerkedés a ETag az objektum frissítést. Ez a részeként ad vissza az `response` olyan személy kapcsolódó művelet, és tudja visszaszerezni keresztül `response['.metadata'].etag`.
>
> 2. Frissítési művelet entitás végrehajtásakor adja meg a korábban olvassa be az új entitás ETag adatokat. Példa:
>
>     `entity2['.metadata'].etag = currentEtag;`
>
> 3. A frissítés végrehajtása. Ha a szervezet módosult, mivel a ETag érték, például az alkalmazás, egy másik példányában lekért egy `error` arról, hogy a kérelem megadott feltétel frissítése nem hagyták visszatér.

**ReplaceEntity** és **mergeEntity**Ha a szervezet frissítendő nem létezik, majd a frissítés sikertelen lesz. Ezért ha ki szeretne tárolni entitás függetlenül attól, hogy már létezik ilyen, **insertOrReplaceEntity** vagy **insertOrMergeEntity**.

A `result` sikeres frissítés műveletek a **Etag** a frissített entitás fog tartalmazni.

## <a name="work-with-groups-of-entities"></a>Személyek, csoportok kezelése

Egyes esetekben célszerű elküldése több művelet egy köteg atomi a kiszolgáló által feldolgozása biztosítása érdekében a közös. Elvégezheti, hogy, a **TableBatch** osztály egy köteg létrehozásához használja, és majd használja a kötegelt műveletek elvégzéséhez **TableService** **executeBatch** metódusát.

 Az alábbi példa bemutatja, hogy egy köteg két entitás elküldése:

    var task1 = {
      PartitionKey: {'_':'hometasks'},
      RowKey: {'_': '1'},
      description: {'_':'Take out the trash'},
      dueDate: {'_':new Date(2015, 6, 20)}
    };
    var task2 = {
      PartitionKey: {'_':'hometasks'},
      RowKey: {'_': '2'},
      description: {'_':'Wash the dishes'},
      dueDate: {'_':new Date(2015, 6, 20)}
    };

    var batch = new azure.TableBatch();

    batch.insertEntity(task1, {echoContent: true});
    batch.insertEntity(task2, {echoContent: true});

    tableSvc.executeBatch('mytable', batch, function (error, result, response) {
      if(!error) {
        // Batch completed
      }
    });

A sikeres köteg műveletekhez `result` a köteg minden művelethez kapcsolatos adatokat fog tartalmazni.

### <a name="work-with-batched-operations"></a>Kötegelt műveletek használata

Műveletek hozzáadott egy köteg megtekintése ellenőrizni is kell a `operations` tulajdonság. Az alábbi módszerek segítségével műveletek használata:

* **törölje a jelet** - törlése egy köteg az összes művelet

* **getOperations** - művelet megszerzi a köteg

* **hasOperations** - eredménye IGAZ, ha a köteg tartalmaz műveletek

* **removeOperations** - eltávolítja a művelet

* **méret** - műveletek számát adja meg a köteg

## <a name="retrieve-an-entity-by-key"></a>Egy személy beolvasásához billentyű

Egy adott személy **PartitionKey** és **RowKey**alapján, használja a **retrieveEntity** módszert.

    tableSvc.retrieveEntity('mytable', 'hometasks', '1', function(error, result, response){
      if(!error){
        // result contains the entity
      }
    });

Ez a művelet befejeződött, miután `result` fogja tartalmazni a szervezet.

## <a name="query-a-set-of-entities"></a>A lekérdezés személyek csoportja

A lekérdezés egy táblázat, használja a **TableQuery** objektum felépítheti egy lekérdezés kifejezés használata a következő záradékok szerepelnek:

* **Jelölje ki** – a mezők, a lekérdezés által visszaadott

* **Ha** – a where záradék

    * **és** – egy `and` where feltétel

    * **vagy** -- `or` where feltétel

* **felső** - ken elemek száma


Az alábbi példában egy lekérdezést, amely visszaadja a "hometasks" egy PartitionKey a az első öt elem hoz létre.

    var query = new azure.TableQuery()
      .top(5)
      .where('PartitionKey eq ?', 'hometasks');

**Válassza a** nem használt, mivel minden mező visszatér. A lekérdezés táblákon végrehajtásához használja a **queryEntities**. Az alábbi példában a lekérdezés szervezetek térjen from "tábla".

    tableSvc.queryEntities('mytable',query, null, function(error, result, response) {
      if(!error) {
        // query was successful
      }
    });

Ha sikeres, `result.entries` a lekérdezésnek megfelelő személyek tömb fog tartalmazni. Ha a lekérdezés nem tudott összes szervezetek térjen `result.continuationToken` nem*üres* lesz, és is alkalmazható a harmadik paraméter **queryEntities** a további eredmények beolvasásához. A kezdeti lekérdezés használata *null* a harmadik paraméter.

### <a name="query-a-subset-of-entity-properties"></a>A lekérdezés csak egy részhalmazát entitás tulajdonságai

Táblázat lekérdezés néhány mezőket beolvasható entitás.
Ez a csökkenti a sávszélesség és javíthatja a lekérdezési teljesítményt, különösen a nagyobb szervezetek. A **Válassza ki** záradékkal, és a neveket a visszaadandó mező adják át. A következő lekérdezés például csak a **Leírás** és **dueDate** mezők ad eredményül.

    var query = new azure.TableQuery()
      .select(['description', 'dueDate'])
      .top(5)
      .where('PartitionKey eq ?', 'hometasks');

## <a name="delete-an-entity"></a>Entitás törlése

A partíciók, és a sor kulcsokkal entitás törölheti. Ebben a példában az a **task1** objektum törlődik a személy **RowKey** és **PartitionKey** értékeket tartalmazza. Kattintson az objektumra a **deleteEntity** módszerrel átadott.

    var task = {
      PartitionKey: {'_':'hometasks'},
      RowKey: {'_': '1'}
    };

    tableSvc.deleteEntity('mytable', task, function(error, response){
      if(!error) {
        // Entity deleted
      }
    });

> [AZURE.NOTE] Fontolja meg inkább ETags elemek törlése esetén annak érdekében, hogy az elem nem módosítottak egy másik folyamat. Lásd: a [frissítés entitás](#update-an-entity) ETags használatáról bővebben.

## <a name="delete-a-table"></a>Táblázat törlése

A következő kódot táblázat törlése a tárterület-fiókból.

    tableSvc.deleteTable('mytable', function(error, response){
        if(!error){
            // Table deleted
        }
    });

Ha biztos benne, hogy a tábla létezik, használja a **deleteTableIfExists**.

## <a name="use-continuation-tokens"></a>Folytatását jelző tokenek használata

Ha nagy mennyiségű eredmények táblák lekérdezett keresse meg a folytatását jelző tokenek. Nagy mennyiségű adatot feltétlenül vehető igénybe a lekérdezés, akkor előfordulhat, hogy nem figyelmen kívül hagyja, ha nem generál felismer, ha egy folytatását jelző token nem tartalmaz adatokat.

Az eredmények objektum lekérdezése szervezetek beállítása során vissza egy `continuationToken` tulajdonság, ha egy ilyen token nem tartalmaz adatokat. Használhatja a lekérdezés végrehajtásakor továbbra is áthelyezése a partíciót és a táblázat entitás át.

Lekérdezés, amikor continuationToken paraméter a lekérdezés objektum példány és a visszahívás függvény között ellátni:

```
var nextContinuationToken = null;
dc.table.queryEntities(tableName,
    query,
    nextContinuationToken,
    function (error, results) {
        if (error) throw error;

        // iterate through results.entries with results

        if (results.continuationToken) {
            nextContinuationToken = results.continuationToken;
        }

    });
```

Ha nézze meg a `continuationToken` objektumra, látni fogja tulajdonságok például `nextPartitionKey`, `nextRowKey` és `targetLocation`, használt Végigléptetés az összes találatot.

Mintát is egy folytatását jelző az Azure tároló Node.js repó a GitHub belül. Keressen `examples/samples/continuationsample.js`.

## <a name="work-with-shared-access-signatures"></a>Megosztott access aláírások használata

Megosztott access aláírások (Társítások) is egy biztonságos anélkül, hogy a tárhely fióknév vagy is táblák részletes hozzáférést biztosít. Az adatok, például lehetővé teszi a lekérdezés rekordok mobilalkalmazásban korlátozott hozzáférést biztosít a Társítások gyakran használják.

A megbízható alkalmazások, például egy felhőalapú szolgáltatás létrehoz egy, a **TableService**a **generateSharedAccessSignature** használatával Társítások, és mobilalkalmazásban például nem megbízható vagy félig megbízható az alkalmazás biztosítja. A Társítások jön létre egy szabályt, amely ismerteti a kezdési és befejezési dátumot, ameddig a Társítások érvényes, valamint a hozzáférési szintet a Társítások jogosult nyújtott használatával.

Az alábbi példa létrehoz egy új megosztott hozzáférési házirendet, amely lehetővé teszi, hogy a lekérdezés ("r") a tábla Társítások jogosult, és az idő létrehozás után 100 perc múlva lejár.

    var startDate = new Date();
    var expiryDate = new Date(startDate);
    expiryDate.setMinutes(startDate.getMinutes() + 100);
    startDate.setMinutes(startDate.getMinutes() - 100);

    var sharedAccessPolicy = {
      AccessPolicy: {
        Permissions: azure.TableUtilities.SharedAccessPermissions.QUERY,
        Start: startDate,
        Expiry: expiryDate
      },
    };

    var tableSAS = tableSvc.generateSharedAccessSignature('mytable', sharedAccessPolicy);
    var host = tableSvc.host;

Figyelje meg, hogy a host meg kell adni információkat is, mivel az szükséges a Társítások jogosult hozzáférni a táblázat alkalommal, amikor.

Kattintson az ügyfélalkalmazás a Társítások **TableServiceWithSAS** a táblázat elleni művelet elvégzéséhez használja. Az alábbi példa a táblázat csatlakozik, és a lekérdezést hajt végre.

    var sharedTableService = azure.createTableServiceWithSas(host, tableSAS);
    var query = azure.TableQuery()
      .where('PartitionKey eq ?', 'hometasks');

    sharedTableService.queryEntities(query, null, function(error, result, response) {
      if(!error) {
        // result contains the entities
      }
    });

A Társítások csak a lekérdezés Access jött létre, ha az azok kísérlet beszúrása, frissítése és törlése a szervezetek, mivel a művelet hibát volna vissza.

### <a name="access-control-lists"></a>Hozzáférés-vezérlési listák

Az Access lista (vezérlés) a hozzáférési házirend beállítása a Társítások is használhatja. Ez akkor hasznos, ha ki szeretne hozzáférni a táblázatot, de egyes ügyfelek a különböző hozzáférési szükséges több ügyfél engedélyezése.

Egy vezérlés access házirendek tömbje segítségével társított egyes házirend azonosítójú működik. Az alábbi példa a két házirendek, egy "felhasználó1" és "felhasználó2" egy határozza meg:

    var sharedAccessPolicy = {
      user1: {
        Permissions: azure.TableUtilities.SharedAccessPermissions.QUERY,
        Start: startDate,
        Expiry: expiryDate
      },
      user2: {
        Permissions: azure.TableUtilities.SharedAccessPermissions.ADD,
        Start: startDate,
        Expiry: expiryDate
      }
    };

Az alábbi példa az aktuális vezérlés kapja a **hometasks** táblához, és hozzáadja az új házirendek **setTableAcl**használatával. Ezt a megközelítést lehetővé teszi, hogy:

    var extend = require('extend');
    tableSvc.getTableAcl('hometasks', function(error, result, response) {
    if(!error){
        var newSignedIdentifiers = extend(true, result.signedIdentifiers, sharedAccessPolicy);
        tableSvc.setTableAcl('hometasks', newSignedIdentifiers, function(error, result, response){
          if(!error){
            // ACL set
          }
        });
      }
    });

Amikor a vezérlés van megadva, majd készíthet egy Társítások házirend-azonosító alapján. Az alábbi példa létrehoz egy új biztonsági Társítások "felhasználó2":

    tableSAS = tableSvc.generateSharedAccessSignature('hometasks', { Id: 'user2' });

## <a name="next-steps"></a>Következő lépések

Az alábbi forrásokban talál további információt.

-   [Azure tároló csapatának blogja][].
-   [Azure tároló SDK csomópont][] tárházából GitHub.
-   [NODE.js Developer Center](/develop/nodejs/)

  [Azure tároló SDK csomóponthoz]: https://github.com/Azure/azure-storage-node
  [OData.org]: http://www.odata.org/
  [Using the REST API]: http://msdn.microsoft.com/library/azure/hh264518.aspx
  [Azure Portal]: portal.azure.com

  [Node.js Cloud Service]: ../cloud-services-nodejs-develop-deploy-app.md
  [Azure tároló csapatának blogja]: http://blogs.msdn.com/b/windowsazurestorage/
  [Website with WebMatrix]: ../web-sites-nodejs-use-webmatrix.md
  [Node.js Cloud Service with Storage]: ../storage-nodejs-use-table-storage-cloud-service-app.md
  [Az Azure-táblából szolgáltatással node.js webalkalmazás]: ../storage-nodejs-use-table-storage-web-site.md
  [Create and deploy a Node.js application to an Azure website]: ../web-sites-nodejs-develop-deploy-mac.md
