<properties
    pageTitle="A Node.js Blob-tárolóhoz használata |} Microsoft Azure"
    description="Strukturálatlan adatokat tárolja az Azure Blob-tárolóhoz (objektum tároló) a felhőben."
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



# <a name="how-to-use-blob-storage-from-nodejs"></a>Használatáról a Node.js Blob-tárolóhoz

[AZURE.INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>– Áttekintés

Ez a cikk bemutatja, hogyan Blob-tárolóhoz használatával esetei végrehajtásához. A minták kerülnek a Node.js API keresztül. Az érintett esetek hogyan tölthet fel, lista, töltse le és BLOB törlése.

[AZURE.INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-nodejs-application"></a>Node.js-alkalmazás létrehozása

Node.js-alkalmazás létrehozása című témakörben találhat [létrehozása az Azure alkalmazás szolgáltatás Node.js webalkalmazást], [létre és helyezhetnek üzembe a Node.js alkalmazások, ha egy felhőalapú szolgáltatásba, Azure] – használatáról a Windows PowerShell és a [építse fel és telepítse az Azure használata a webes mátrix Node.js webalkalmazást].

## <a name="configure-your-application-to-access-storage"></a>A tároló eléréséhez konfigurálása

Azure tároló használatához szükséges az Azure tároló SDK Node.js egy sor olyan kényelmesebbé olyan tárat, amellyel kommunikálni a tároló többi szolgáltatást tartalmazó.

### <a name="use-node-package-manager-npm-to-obtain-the-package"></a>A csomag beszerzése csomópont csomag Manager (NPM) használatával

1.  Nyissa meg azt a mappát, amelyen létrehozta a minta alkalmazás a **PowerShell** (Windows), a **terminált** (Mac) vagy a **Bash** (Unix), például a parancssor használatával.

2.  Írja be a **npm telepítse az azure-tárhelyet** a parancssorablakban. A parancs a következőhöz hasonló kódot.

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

3.  Manuális futtatását is lehetővé teszi annak ellenőrzésére, hogy az **ls** parancs a **csomópont\_modulok** mappa létrehozásának. Egy adott mappába keresse meg az **azure-tároló** csomagra, amely tartalmazza a tárak tároló eléréséhez szükséges.

### <a name="import-the-package"></a>A csomag importálása

Hol tároló használni kívánt **server.js** fájlt az alkalmazás tetején Jegyzettömb vagy egy másik text szerkesztő használata esetén adja hozzá a következő:

    var azure = require('azure-storage');

## <a name="set-up-an-azure-storage-connection"></a>Tárterület Azure-alapú beállítása

Az Azure modul felirat jelenik meg a környezeti változók `AZURE_STORAGE_ACCOUNT` és `AZURE_STORAGE_ACCESS_KEY`, vagy `AZURE_STORAGE_CONNECTION_STRING`, az Azure tároló fiókhoz való csatlakozáshoz szükséges információkat. Ha ezek a környezeti változók nincsenek beállítva, **createBlobService**hívásakor meg kell adnia a fiók adatait.

Példa a környezeti változók beállítása az [Azure portál](https://portal.azure.com) az Azure webappokban [az Azure-táblából szolgáltatással Node.js webalkalmazás]talál.

## <a name="create-a-container"></a>A tároló létrehozása

A **BlobService** objektum lehetővé teszi a tárolók és BLOB. A következő kódot létrehoz egy **BlobService** -objektumot. Adja hozzá a következő **server.js**tetején:

    var blobSvc = azure.createBlobService();

> [AZURE.NOTE] Elérheti blob névtelenül **createBlobServiceAnonymous** használ, és az állomás címének megadásával. Ha például az `var blobSvc = azure.createBlobServiceAnonymous('https://myblob.blob.core.windows.net/');`.

[AZURE.INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

Hozzon létre egy új tároló, használja a **createContainerIfNotExists**. Az alábbi példa létrehoz "mycontainer" nevű új tároló:

    blobSvc.createContainerIfNotExists('mycontainer', function(error, result, response){
        if(!error){
          // Container exists and is private
        }
    });

Ha a tároló újonnan létrehozott `result.created` értéke igaz. Ha a tároló már létezik, `result.created` hamis. `response`a művelet, beleértve a tároló ETag adatait vonatkozó információkat tartalmazza.

### <a name="container-security"></a>A tároló biztonsági

Alapértelmezés szerint új tárolók magánjellegű, és nem érhető el névtelenül. Kell a tároló nyilvános, hogy névtelenül érhető el, beállíthatja, hogy a container hozzáférési szintet **blob** vagy **tároló**.

* **blob** - blob-tartalom és a metaadat-tároló belül, de nem tároló metaadatokat, például a bejegyzésére a tárolóban lévő valamennyi BLOB névtelen olvasási hozzáférést biztosít.

* **a tároló** - lehetővé teszi, hogy a névtelen olvasási hozzáférést blob-tartalom és a metaadatokat, valamint a tároló metaadatok

A következő példa bemutatja, hogy a hozzáférési szint **blob**:

    blobSvc.createContainerIfNotExists('mycontainer', {publicAccessLevel : 'blob'}, function(error, result, response){
        if(!error){
          // Container exists and allows
          // anonymous read access to blob
          // content and metadata within this container
        }
    });

A tároló hozzáférési szintjének módosíthatja azt is megteheti, **setContainerAcl** segítségével adja meg a hozzáférési szintet. A következő példa container hozzáférési szintjének változik:

    blobSvc.setContainerAcl('mycontainer', null /* signedIdentifiers */, {publicAccessLevel : 'container'} /* publicAccessLevel*/, function(error, result, response){
      if(!error){
        // Container access level set to 'container'
      }
    });

Az eredmény a művelet, beleértve az aktuális **ETag** a tároló információkat tartalmazza.

### <a name="filters"></a>Szűrők

Választható szűrési műveletek végrehajtott műveletek naplóadatai **BlobService**alkalmazhat. Szűrési műveletek is elhelyezhet, naplózás, automatikusan újra próbálkozik stb. Szűrők olyan objektumok, egy módszer a aláírással végrehajtása:

    function handle (requestOptions, next)

Után hajtsa végre a előfeldolgozása az kérelem beállításai, módszer kell "Tovább" hívás átadása a visszahívás, a következő aláírással:

    function (returnObject, finalCallback, next)

A visszahívási, és a returnObject (válasza a kérelmet a kiszolgálón) feldolgozás után a visszahívás meghívása Ezután ha létezik továbbra is a további szűrők feldolgozása vagy egyszerűen finalCallback a szolgáltatás hívás befejezése van szüksége.

Két szűrő látható, hogy ismét logikájának megvalósítása is megjelennek az Azure SDK Node.js, **ExponentialRetryPolicyFilter** és **LinearRetryPolicyFilter**. A következő objektumot hoz létre **BlobService** , amely a **ExponentialRetryPolicyFilter**használja:

    var retryOperations = new azure.ExponentialRetryPolicyFilter();
    var blobSvc = azure.createBlobService().withFilter(retryOperations);

## <a name="upload-a-blob-into-a-container"></a>A tároló blob feltölteni

A BLOB három típusba sorolhatók: BLOB letiltani, oldal BLOB, és a Hozzáfűzés BLOB. Blokkokból álló BLOB lehetővé teszi, hogy több hatékony feltöltése nagy adatok. Hozzáfűző BLOB szolgáltatáshoz optimalizált Hozzáfűzés művelet. Lap BLOB olvasási/írási műveletek vannak optimalizálva. [További információ ismertetése blokk BLOB, hozzáfűző BLOB és](http://msdn.microsoft.com/library/azure/ee691964.aspx)megtekintése lapon BLOB

### <a name="block-blobs"></a>Továbbfejlesztett fájlblokkolás BLOB

Blokkokból álló blob feltölteni az adatokat, használja az alábbiakat:

* **createBlockBlobFromLocalFile** - hoz létre egy új, továbbfejlesztett fájlblokkolás blob és feltöltések fájl tartalmának megjelenítése

* **createBlockBlobFromStream** - hoz létre egy új, továbbfejlesztett fájlblokkolás blob és feltöltések megjelenítése az adatfolyam tartalma

* **createBlockBlobFromText** - hoz létre egy új, továbbfejlesztett fájlblokkolás blob és feltöltések megjelenítése az karakterlánc tartalmának

* **createWriteStreamToBlockBlob** - biztosít a továbbfejlesztett fájlblokkolás blob írási adatfolyam

A következő példa **myblob**be feltöltések megjelenítése az **teszt.txt** fájl tartalmát.

    blobSvc.createBlockBlobFromLocalFile('mycontainer', 'myblob', 'test.txt', function(error, result, response){
      if(!error){
        // file uploaded
      }
    });

A `result` ezeket a módszereket által visszaadott működéséről, például a **ETag** , a blob információkat tartalmaz.

### <a name="append-blobs"></a>Hozzáfűző BLOB

Új hozzáfűző blob feltölteni az adatokat, használja az alábbiakat:

* **createAppendBlobFromLocalFile** - hoz létre egy új hozzáfűző blob és feltöltések fájl tartalmának megjelenítése

* **createAppendBlobFromStream** - hoz létre egy új hozzáfűző blob és feltöltések megjelenítése az adatfolyam tartalma

* **createAppendBlobFromText** - hoz létre egy új hozzáfűző blob és feltöltések megjelenítése az karakterlánc tartalmának

* **createWriteStreamToNewAppendBlob** – majd a hozzá írási adatfolyam bejegyzései, és létrehoz egy új hozzáfűző blob

A következő példa **myappendblob**be feltöltések megjelenítése az **teszt.txt** fájl tartalmát.

    blobSvc.createAppendBlobFromLocalFile('mycontainer', 'myappendblob', 'test.txt', function(error, result, response){
      if(!error){
        // file uploaded
      }
    });

Egy hozzáfűzése egy már meglévő hozzáfűző blob, használja az alábbiakat:

* **appendFromLocalFile** - fájl tartalmának hozzáfűzése egy már meglévő hozzáfűző blob

* **appendFromStream** - adatfolyam tartalmának hozzáfűzése egy már meglévő hozzáfűző blob

* **appendFromText** - karakterlánc tartalmának hozzáfűzése egy már meglévő hozzáfűző blob

* **appendBlockFromStream** - adatfolyam tartalmának hozzáfűzése egy már meglévő hozzáfűző blob

* **appendBlockFromText** - karakterlánc tartalmának hozzáfűzése egy már meglévő hozzáfűző blob

> [AZURE.NOTE] appendFromXXX API-khoz néhány ügyféloldali érvényességi gyors unncessary kiszolgáló hívása elkerülésére meghiúsító hajt végre. nem appendBlockFromXXX.

A következő példa **myappendblob**be feltöltések megjelenítése az **teszt.txt** fájl tartalmát.

    blobSvc.appendFromText('mycontainer', 'myappendblob', 'text to be appended', function(error, result, response){
      if(!error){
        // text appended
      }
    });


### <a name="page-blobs"></a>Lap BLOB

Lap blob feltölteni az adatokat, használja az alábbiakat:

* **createPageBlob** - hoz létre egy új oldal blob egy adott hosszúságú

* **createPageBlobFromLocalFile** - hoz létre egy új oldal blob és feltöltések fájl tartalmának megjelenítése

* **createPageBlobFromStream** - hoz létre egy új oldal blob és feltöltések megjelenítése az adatfolyam tartalma

* **createWriteStreamToExistingPageBlob** - tartalmaz egy meglévő lap blob egy írási adatfolyam

* **createWriteStreamToNewPageBlob** – majd a hozzá írási adatfolyam bejegyzései, és létrehoz egy új oldal blob

A következő példa **mypageblob**be feltöltések megjelenítése az **teszt.txt** fájl tartalmát.

    blobSvc.createPageBlobFromLocalFile('mycontainer', 'mypageblob', 'test.txt', function(error, result, response){
      if(!error){
        // file uploaded
      }
    });

> [AZURE.NOTE] Lap BLOB 512 bájtos "lapok" áll. Hibaüzenetet kap, amely nem 512 többszöröse méretű adatok feltöltésekor.

## <a name="list-the-blobs-in-a-container"></a>A BLOB tárolóban lista

A **listBlobsSegmented** módszert a BLOB tárolóban listában. Ha szeretné, hogy egy adott előtaggal BLOB térni, használja a **listBlobsSegmentedWithPrefix**.

    blobSvc.listBlobsSegmented('mycontainer', null, function(error, result, response){
      if(!error){
          // result.entries contains the entries
          // If not all blobs were returned, result.continuationToken has the continuation token.
      }
    });

A `result` tartalmaz egy `entries` a webhelycsoportban, amelyek bemutatják, minden egyes blob-objektumok tömböt alkotó. Valamennyi BLOB nem adhatók vissza, ha a `result` is biztosít egy `continuationToken`, mellyel előfordulhat, hogy a második paraméterként további bejegyzések beolvasásához.

## <a name="download-blobs"></a>BLOB letöltése

Adatok letöltése a blob, használja az alábbi:

* **getBlobToLocalFile** - fájl tartalmának blob ír

* ír blob tartalmát **getBlobToStream** - adatfolyam

* ír blob tartalmát **getBlobToText** - karakterlánc

* **createReadStream** – itt olvasható a blob adatfolyam

A következő példa bemutatja, töltse le a **myblob** blob tartalmát, és tárolni a **kimenet.txt** fájlt adatfolyam használatával **getBlobToStream** használatával:

    var fs = require('fs');
    blobSvc.getBlobToStream('mycontainer', 'myblob', fs.createWriteStream('output.txt'), function(error, result, response){
      if(!error){
        // blob retrieved
      }
    });

A `result` a blob, például az **ETag** információkat tartalmaz.

## <a name="delete-a-blob"></a>Blob törlése

Végezetül blob törléséhez hívja a **deleteBlob**. Az alábbi példa a **myblob**nevű blob törli.

    blobSvc.deleteBlob(containerName, 'myblob', function(error, response){
      if(!error){
        // Blob has been deleted
      }
    });

## <a name="concurrent-access"></a>Egyidejű hozzáférés

Több ügyfél vagy több folyamat példányai a támogatás hivatkozással blob egyidejű hozzáférést, **ETags** vagy **bérletek**is használhatja.

* **Etag** - módszert kínál a észleli, hogy a blob vagy tároló módosították egy másik folyamat

* **Bérleti** - megoldást kizárólagos, megújuló beszerzése, írja be vagy blob hozzáférést törlése a ideje

### <a name="etag"></a>ETag

Egyidejű Blob-ETags használata, ha több ügyfelek vagy példányok Blob vagy lap Tiltás írni engedélyeznie kell. A ETag lehetővé teszi annak megállapítását, ha a tároló vagy blob óta módosították kezdetben olvasni vagy létrehozta, amely lehetővé teszi, hogy a változtatások úgy, hogy egy másik ügyfél vagy a folyamat felülírni.

Beállíthatja, hogy ETag feltételek használatával a választható `options.accessConditions` paraméter. Az alábbi példa csak feltölti a **teszt.txt** fájlt, ha már létezik az blob és magában az ETag érték található `etagToMatch`.

    blobSvc.createBlockBlobFromLocalFile('mycontainer', 'myblob', 'test.txt', { accessConditions: { EtagMatch: etagToMatch} }, function(error, result, response){
        if(!error){
        // file uploaded
      }
    });

ETags használ, az általános mintát esetén:

1. Szerezze be a ETag, létrehozása, a lista vagy a művelet eredménye.

2. Egy műveletet, ellenőrzése, hogy a ETag értéket nem módosították.

Ha az érték módosították, ez azt jelzi, hogy egy másik ügyfél vagy egy példány módosította a blob vagy tároló ETag értékét szerezte be.

### <a name="lease"></a>Bérleti

Egy új bérleti szerezheti be a **acquireLease** módszerrel, a blob vagy tároló, amelyet el szeretne kapni a bérleti a megadásával. Például a következő kódot megszerzi a bérleti a **myblob**.

    blobSvc.acquireLease('mycontainer', 'myblob', function(error, result, response){
      if(!error) {
        console.log('leaseId: ' + result.id);
      }
    });

További műveletek a **myblob** meg kell adnia a `options.leaseId` paraméter. Az azonosító visszatérési bérleti `result.id` a **acquireLease**.

> [AZURE.NOTE] Alapértelmezés szerint a bérleti időtartam végtelen. (15 és 60 másodperc között) nem végtelen időtartamot adhatja meg, mert a `options.leaseDuration` paraméter.

Ha el szeretne távolítani egy bérleti, használja a **releaseLease**. A bérleti megszakítja, de szeretné akadályozni, hogy mások egy új bérleti mindaddig, amíg az eredeti időtartam lejárt, használja a **breakLease**.

## <a name="work-with-shared-access-signatures"></a>Megosztott access aláírások használata

Megosztott access aláírások (Társítások) is egy biztonságos részletes hozzáférést biztosítanak BLOB és tárolók anélkül, hogy a tárhely fióknév vagy is. Az adatok, például lehetővé tevő BLOB eléréséhez mobilalkalmazásban korlátozott hozzáférést biztosít a megosztott access aláírások gyakran használják.

> [AZURE.NOTE] Közben is engedélyezheti a névtelen hozzáférés BLOB, átengedése aláírások lehetővé teszik több ellenőrzött hozzáférés adnak, miként Ez a Társítások kell létrehozni.

Egy megbízható alkalmazást, például egy felhőalapú szolgáltatás átengedése aláírások használata a **generateSharedAccessSignature** , a **BlobService**hoz létre, és például mobilalkalmazásban nem megbízható vagy félig megbízható az alkalmazás biztosítja. Megosztott hozzáféréssel aláírások jönnek létre használata egy szabályt, amely bemutatja az indítási és befejezési dátuma, amely során az átengedése aláírások érvényesek, valamint a hozzáférési szintet biztosított a megosztott access aláírások jogosult.

Az alábbi példa létrehoz egy új megosztott hozzáférési házirendet, amely lehetővé teszi a megosztott access aláírások jogosult a **myblob** blob olvasási műveletek hajthatók végre, és az idő létrehozás után 100 perc múlva lejár.

    var startDate = new Date();
    var expiryDate = new Date(startDate);
    expiryDate.setMinutes(startDate.getMinutes() + 100);
    startDate.setMinutes(startDate.getMinutes() - 100);

    var sharedAccessPolicy = {
      AccessPolicy: {
        Permissions: azure.BlobUtilities.SharedAccessPermissions.READ,
        Start: startDate,
        Expiry: expiryDate
      },
    };

    var blobSAS = blobSvc.generateSharedAccessSignature('mycontainer', 'myblob', sharedAccessPolicy);
    var host = blobSvc.host;

Figyelje meg, hogy a host meg kell adni információkat is, mivel az szükséges a megosztott access aláírások jogosult hozzáférni a tároló alkalommal, amikor.

Kattintson az ügyfélalkalmazás átengedése aláírások **BlobServiceWithSAS** a blob elleni művelet elvégzéséhez használja. A következő kap információt **myblob**.

    var sharedBlobSvc = azure.createBlobServiceWithSas(host, blobSAS);
    sharedBlobSvc.getBlobProperties('mycontainer', 'myblob', function (error, result, response) {
      if(!error) {
        // retrieved info
      }
    });

Mivel a megosztott access aláírások csak olvasható hozzáféréssel rendelkező jött létre, ha a kísérlet az blob módosításához, hibaüzenetet fog rendszer.

### <a name="access-control-lists"></a>Hozzáférés-vezérlési listák

Hozzáférés-vezérlési lista (vezérlés) segítségével is a hozzáférési házirend beállítása Társítások. Ez akkor hasznos, ha ki szeretne több ügyfelek tároló elérhető, de egyes ügyfelek a különböző hozzáférési szükséges.

Egy vezérlés access házirendek tömbje segítségével társított egyes házirend azonosítójú működik. Az alábbi példa a két házirendek, egy "felhasználó1" és "felhasználó2" egy határozza meg:

    var sharedAccessPolicy = {
      user1: {
        Permissions: azure.BlobUtilities.SharedAccessPermissions.READ,
        Start: startDate,
        Expiry: expiryDate
      },
      user2: {
        Permissions: azure.BlobUtilities.SharedAccessPermissions.WRITE,
        Start: startDate,
        Expiry: expiryDate
      }
    };

A következő példa beolvassa az aktuális vezérlés tartozó **mycontainer**, és majd összeadja az új házirendek **setBlobAcl**használatával. Ezt a megközelítést lehetővé teszi, hogy:

    var extend = require('extend');
    blobSvc.getBlobAcl('mycontainer', function(error, result, response) {
      if(!error){
        var newSignedIdentifiers = extend(true, result.signedIdentifiers, sharedAccessPolicy);
        blobSvc.setBlobAcl('mycontainer', newSignedIdentifiers, function(error, result, response){
          if(!error){
            // ACL set
          }
        });
      }
    });

Amikor megadja a vezérlés, majd létrehozhat megosztott access aláírások házirend-azonosító alapján. Az alábbi példa létrehoz "felhasználó2" megosztott access új aláírás:

    blobSAS = blobSvc.generateSharedAccessSignature('mycontainer', { Id: 'user2' });

## <a name="next-steps"></a>Következő lépések

Az alábbi forrásokban talál további információt.

-   [Azure tároló SDK csomópont API-hivatkozás][]
-   [Azure tároló csapatának blogja][]
-   [Azure tároló SDK csomópont][] tárházából GitHub
-   [NODE.js Developer Center](/develop/nodejs/)
-   [A AzCopy parancssori segédprogrammal adatátviteli](storage-use-azcopy.md)

[Azure tároló SDK csomóponthoz]: https://github.com/Azure/azure-storage-node

[Node.js webalkalmazást létrehozása az Azure alkalmazás szolgáltatás]: ../app-service-web/web-sites-nodejs-develop-deploy-mac.md
[Node.js Cloud Service with Storage]: ../cloud-services/storage-nodejs-use-table-storage-cloud-service-app.md
[Az Azure-táblából szolgáltatással node.js webalkalmazás]: ../app-service-web/storage-nodejs-use-table-storage-web-site.md
[Létre és helyezhetnek üzembe a megfelelő Node.js web App alkalmazásban való használata a webes mátrix Azure]: ../app-service-web/web-sites-nodejs-use-webmatrix.md
[Using the REST API]: http://msdn.microsoft.com/library/azure/hh264518.aspx
[Azure Portal]: https://portal.azure.com
[Építse fel és telepítse az Azure Felhőszolgáltatásba egy Node.js alkalmazás]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
[Azure tároló csapatának blogja]: http://blogs.msdn.com/b/windowsazurestorage/
[Azure tároló SDK csomópont API-hivatkozás]: http://dl.windowsazure.com/nodestoragedocs/index.html
