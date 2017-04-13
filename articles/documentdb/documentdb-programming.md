<properties 
    pageTitle="DocumentDB programozás: tárolt eljárások, adatbázis eseményindítók és UDF |} Microsoft Azure" 
    description="Megtudhatja, hogy miként tárolt eljárások, az adatbázis eseményindítók és a felhasználó által definiált függvények (UDF) írja be a JavaScript DocumentDB használatával. Adatbázis programing tippeket és további juthat." 
    keywords="Eseményindítók, tárolt eljárás, tárolt eljárás, adatbázis-kezelő program, sproc, documentdb, azure, a Microsoft azure-adatbázis"
    services="documentdb" 
    documentationCenter="" 
    authors="aliuy" 
    manager="jhubbard" 
    editor="mimig"/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/14/2016" 
    ms.author="andrl"/>

# <a name="documentdb-server-side-programming-stored-procedures-database-triggers-and-udfs"></a>DocumentDB kiszolgálóoldali programozás: tárolt eljárások, adatbázis eseményindítók és függvényekben

Megtudhatja, hogyan Azure DocumentDB nyelvi integrált, JavaScript tranzakció alapú végrehajtását lehetővé teszi, hogy a fejlesztők írás **tárolt eljárások**, **Eseményindítók** és a **felhasználó által definiált függvények (UDF)** natív módon a JavaScript. Ez lehetővé teszi, hogy írni az adatbázis program alkalmazás logika szállított, és közvetlenül az adatbázis tárolási partíciót végrehajtása 

Azt javasoljuk, hogy az első lépések a következő videót, ahol Andrew Liu rendelkezik-e DocumentDB a kiszolgálóoldali programozási adatbázismodell rövid ismertetése. 

> [AZURE.VIDEO azure-demo-a-quick-intro-to-azure-documentdbs-server-side-javascript]

Ezután térjen vissza Ez a cikk Ha megismerheti az alábbi kérdésekre adott válaszok:  

- Hogyan írni a tárolt eljárás, eseményindító vagy UDF JavaScript használatával?
- Hogyan garantálja DocumentDB sav?
- Hogyan működnek a tranzakciók a DocumentDB?
- Mik azok a eseményindítók előtti és utáni eseményindítók, és hogyan végezze el írni egy?
- Hogyan lehet regisztrálni és hajtsa végre a tárolt eljárás, eseményindító vagy UDF RESTful módon HTTP használatával?
- Mi DocumentDB SDK érhetők el az létrehozása, majd hajtsa végre tárolt eljárások, eseményindítók és UDF?

## <a name="introduction-to-stored-procedure-and-udf-programming"></a>Tárolt eljárás és UDF programozás – bevezetés

Ezt a megközelítést *"* JavaScript modern napjaként az SQL-T" szabadítja alkalmazások fejlesztői számára, a rendszer eltérnek és objektum relációs leképezés technológiák a bonyodalmainak. Egy szám, amely készíthet sokoldalú alkalmazásokat fogja alkalmazni belső előnnyel is van:  

-   **Lépésről összefüggés:** A magas szintű programozási nyelven, mint a JavaScript üzleti logikai funkcióinak Express gazdag és ismerős felületet biztosít. Elvégezhető műveletek közelebb az adatokhoz összetett sorozatok.

-   **Atomi tranzakciók:** DocumentDB garantálja, hogy adatbázis műveleteket hajtja végre egyetlen tárolt eljárás belül vagy eseményindító atomi. Ezzel a lehetőséggel a kérelmet, hogy az összes oldalról a sikeresek, vagy az egyik sem sikerül kombinálása a kapcsolódó műveletek egyetlen kötegben. 

-   **Teljesítmény:** Hogy a JSON alapvetően a Javascript nyelvi típus rendszer van rendelve, és egyben DocumentDB tárolás alapegység optimalizálásokat, például a pufferelési készlet- és végez elérhető igény szerinti a futó kódot JSON dokumentumok Lusta biztosítási esemény számos tesz lehetővé. Előnyökkel további teljesítmény társított szállítási logikájának az adatbázishoz:
    -   Kötegelés – fejlesztők csoport műveletek, például szúr be, és a tömegesen küldéséhez. A hálózati forgalmat késés költség és a tár általános külön tranzakcióinak létrehozásához jelentősen csökkenti. 
    -   Az összeállítás előtti – DocumentDB precompiles tárolt eljárások, eseményindítók és a felhasználó által definiált függvények (UDF) JavaScript fordítási költség minden meghíváshoz elkerülése érdekében. Az általános tudnivalóit lépésről logika bájt kódját a minimális érték van amortized.
    -   Sorrendjét – sok műveletek segítségre van szüksége egy oldal-hatás ("eseményindító"), amelyek esetleg az egy vagy több másodlagos tár műveleteket is jár. Azon atomicity Ez a további performant áthelyezésekor a kiszolgálóra. 
-   **Beágyazás:** Csoport üzleti logikai funkcióinak – egy helyen tárolt eljárások használható. Ennek két több előnye is van:
    -   Egy hardverabsztrakciós réteg felett a nyers adatokból, amely lehetővé teszi az adatok építészek egymástól függetlenül az adatokból kérelmeiket is alapkoncepciójára oszlopaihoz. Ez akkor különösen hasznos, ha a séma-kisebb, a rideg feltételezések sütött az alkalmazásba, ha szeretné kezelni az adatokat közvetlenül is kell miatt adatai.  
    -   A hardverabsztrakciós lehetővé teszi, hogy azok az adatok egyszerűsítése az access a parancsfájlok biztonsága nagyvállalatoknak.  

A létrehozás és adatbázis eseményindítók, a tárolt eljárás és az egyéni lekérdezés operátorok végrehajtási át a [REST API -t](https://msdn.microsoft.com/library/azure/dn781481.aspx), a [DocumentDB Studio](https://github.com/mingaliu/DocumentDBStudio/releases)és az [ügyfél SDK](documentdb-sdk-dotnet.md) sok platformok, ideértve a .NET rendszerhez, a Node.js és a JavaScript támogatott.

**Ebben az oktatóanyagban használja a [Node.js SDK a Q tett](http://azure.github.io/azure-documentdb-node-q/) ** és tárolt eljárások, eseményindítók és UDF használatát mutatja be.   

## <a name="stored-procedures"></a>Tárolt eljárás

### <a name="example-write-a-simple-stored-procedure"></a>Példa: Írja be egy egyszerű tárolt eljárás 
Kezdjük a egyszerű tárolt eljárás, amely visszaadja a "Helló, világ" választ.

    var helloWorldStoredProc = {
        id: "helloWorld",
        body: function () {
            var context = getContext();
            var response = context.getResponse();
    
            response.setBody("Hello, World");
        }
    }


Tárolt eljárások webhelycsoport használati regisztrált, és a dokumentumot, és adott webhelycsoport jelen melléklet is működik. A következő kódtöredékének megtudhatja, hogy miként regisztrálhatja a helloWorld tárolt eljárás gyűjteménye. 

    // register the stored procedure
    var createdStoredProcedure;
    client.createStoredProcedureAsync('dbs/testdb/colls/testColl', helloWorldStoredProc)
        .then(function (response) {
            createdStoredProcedure = response.resource;
            console.log("Successfully created stored procedure");
        }, function (error) {
            console.log("Error", error);
        });


Miután regisztrálta a tárolt eljárás, hogy szemben a webhelycsoport végre, és olvassa el az ügyfélen vissza az eredményt. 

    // execute the stored procedure
    client.executeStoredProcedureAsync('dbs/testdb/colls/testColl/sprocs/helloWorld')
        .then(function (response) {
            console.log(response.result); // "Hello, World"
        }, function (err) {
            console.log("Error", error);
        });


A helyi objektum itt összes művelet DocumentDB tároló elvégezhető való hozzáférést, illetve hozzáférést biztosít a kérelem és válasz objektumokhoz. Ebben az esetben azt használatával a válasz objektum beállítása a szervezet, a válasz, amely véletlenül lett az ügyfélnek. További információra kíváncsi tekintse át a [DocumentDB JavaScript-kiszolgáló SDK dokumentációt](http://azure.github.io/azure-documentdb-js-server/).  

Tudassa velünk ebben a példában a bontsa ki és hozzáadása több adatbázis-e a tárolt eljárás kapcsolódó funkciókat. Tárolt eljárások hozhat létre, frissítés, olvassa el, a lekérdezés és törölhet dokumentumokat és a mellékletek webhelycsoporton belül.    

### <a name="example-write-a-stored-procedure-to-create-a-document"></a>Példa: Írja be a tárolt eljárás dokumentum létrehozása 
A következő kódtöredékének megtudhatja, hogy hogyan a helyi objektum vezérléséhez DocumentDB erőforrásokat.

    var createDocumentStoredProc = {
        id: "createMyDocument",
        body: function createMyDocument(documentToCreate) {
            var context = getContext();
            var collection = context.getCollection();

            var accepted = collection.createDocument(collection.getSelfLink(),
                  documentToCreate,
                  function (err, documentCreated) {
                      if (err) throw new Error('Error' + err.message);
                      context.getResponse().setBody(documentCreated.id)
                  });
            if (!accepted) return;
        }
    }


A tárolt eljárás megnyitja, mint a bemeneti documentToCreate, létre kell hozni az aktuális gyűjtemény a dokumentum törzséhez az. Az ilyen a műveletek aszinkron és JavaScript függvény visszahívást függnek. A visszahívási függvénynek két paraméterrel, egy abban az esetben a művelet sikertelen lesz a hiba-objektum, és egy a létrehozott objektum. Belül a visszahívás felhasználók kezelése a kivétel, vagy hibaüzenetet küldjön. Abban az esetben nem szerepel a visszahívás, és hiba lép fel, akkor a DocumentDB futtatókörnyezet hibát okoz.   

A fenti példában a visszahívás hibát okoz, a művelet sikertelen, ha. A létrehozott dokumentum azonosítója, a válasz, az ügyfél szövegtörzseként állítja. Az alábbiakban a tárolt eljárás a bemeneti paramétereket végrehajtásának módját.

    // register the stored procedure
    client.createStoredProcedureAsync('dbs/testdb/colls/testColl', createDocumentStoredProc)
        .then(function (response) {
            var createdStoredProcedure = response.resource;
    
            // run stored procedure to create a document
            var docToCreate = {
                id: "DocFromSproc",
                book: "The Hitchhiker’s Guide to the Galaxy",
                author: "Douglas Adams"
            };
    
            return client.executeStoredProcedureAsync('dbs/testdb/colls/testColl/sprocs/createMyDocument',
                  docToCreate);
        }, function (error) {
            console.log("Error", error);
        })
    .then(function (response) {
        console.log(response); // "DocFromSproc"
    }, function (error) {
        console.log("Error", error);
    });

    
Figyelje meg, hogy a tárolt eljárás módosítható, hogy bemeneteként dokumentum szervezetek tömb készíthet, és hozza létre őket az összes azonos tárolt eljárás végrehajtásához több hálózati kérelmeket, mindegyiket külön-külön létrehozása helyett. Ez egy hatékony tömeges importáló végrehajtása (ebben az oktatóanyagban a tárgyalt) DocumentDB is használható.   

A példa leírt igazolni tárolt eljárások használata. Témákat vesszük sorra eseményindítók és a felhasználó által definiált függvények (UDF) az oktatóprogram belül.

## <a name="database-program-transactions"></a>Adatbázis-tranzakciók program
Egy tipikus adatbázisban tranzakció egy egységként logikai munka végrehajtott műveletek sorozata adható meg. Tranzakciók Itt **sav garantálja**. SAV egy jól ismert betűszó, amely négy tulajdonságai – Atomicity, összhangot, elkülönítési és tartóssági képviseli.  

Röviden, atomicity biztosítja, hogy minden a munkát, egy tranzakción belül egységként van-e kezelni hol vagy kötelességének tekinti azt minden vagy a Nincs értéket. Egységesebb ellenőrzi, hogy adatai mindig belső állapota jó tranzakciók keresztül. Elkülönítési garantálja, hogy nincsenek két tranzakciók egymást zavarják – általában, a legtöbb kereskedelmi rendszerek adja meg az alkalmazás igényeinek megfelelően elkülönítési többszintű használható. Tartóssági biztosítja, hogy mindig fog bemutató bármilyen módosítást, amely az adatbázis elkötelezte magát.   

A DocumentDB a sokoldalú, adatbázisszerű azonos memória a JavaScript helyezkedik el. Így kérések belül tárolt eljárások és indítók végrehajtása egy adatbázis-munkamenetet azonos hatálya alá. Zökkenőmentes sav az összes művelet egyetlen tárolt eljárás/eseményindító részét képező DocumentDB szolgáltatás lehetővé teszi. Vegye figyelembe az alábbi tárolt eljárás meghatározása:

    // JavaScript source code
    var exchangeItemsSproc = {
        name: "exchangeItems",
        body: function (playerId1, playerId2) {
            var context = getContext();
            var collection = context.getCollection();
            var response = context.getResponse();
    
            var player1Document, player2Document;
    
            // query for players
            var filterQuery = 'SELECT * FROM Players p where p.id  = "' + playerId1 + '"';
            var accept = collection.queryDocuments(collection.getSelfLink(), filterQuery, {},
                function (err, documents, responseOptions) {
                    if (err) throw new Error("Error" + err.message);
    
                    if (documents.length != 1) throw "Unable to find both names";
                    player1Document = documents[0];
    
                    var filterQuery2 = 'SELECT * FROM Players p where p.id = "' + playerId2 + '"';
                    var accept2 = collection.queryDocuments(collection.getSelfLink(), filterQuery2, {},
                        function (err2, documents2, responseOptions2) {
                            if (err2) throw new Error("Error" + err2.message);
                            if (documents2.length != 1) throw "Unable to find both names";
                            player2Document = documents2[0];
                            swapItems(player1Document, player2Document);
                            return;
                        });
                    if (!accept2) throw "Unable to read player details, abort ";
                });
    
            if (!accept) throw "Unable to read player details, abort ";
    
            // swap the two players’ items
            function swapItems(player1, player2) {
                var player1ItemSave = player1.item;
                player1.item = player2.item;
                player2.item = player1ItemSave;
    
                var accept = collection.replaceDocument(player1._self, player1,
                    function (err, docReplaced) {
                        if (err) throw "Unable to update player 1, abort ";
    
                        var accept2 = collection.replaceDocument(player2._self, player2,
                            function (err2, docReplaced2) {
                                if (err) throw "Unable to update player 2, abort"
                            });
    
                        if (!accept2) throw "Unable to update player 2, abort";
                    });
    
                if (!accept) throw "Unable to update player 1, abort";
            }
        }
    }
    
    // register the stored procedure in Node.js client
    client.createStoredProcedureAsync(collection._self, exchangeItemsSproc)
        .then(function (response) {
            var createdStoredProcedure = response.resource;
        }
    );

Ez a tárolt eljárás játék-alkalmazásból kereskedelmi elemek között két játékosokat egy lépésben tranzakciók használja. Az argumentumként átadott a lejátszó azonosítók megfelelő két dokumentumok olvasása a tárolt eljárás kísérel meg. Ha mindkét player dokumentumok találhatók, majd a tárolt eljárás frissíti a dokumentumokat azok az elemek lecserélése. Ha a hibák menet találkozik, azt, amely implicit módon megszakítja a tranzakció JavaScript kivételt okoz.

Ha a gyűjtemény a tárolt eljárás van regisztrálva ellen egyetlen-partíciót gyűjteménye, akkor a tranzakció webhelycsoporton belüli összes docuemnts adatbázisokban. Ha a webhelycsoport particionálva van, akkor tárolt eljárások vannak hajtja végre a partíciót kulcs tranzakció hatálya alá. Minden tárolt eljárás végrehajtási majd tartalmaznia kell a hatókör a tranzakció kell futtatni a megfelelő partíciót fő érték. További információra kíváncsi olvassa el a [Szétválasztás DocumentDB](documentdb-partition-data.md)című témakört.

### <a name="commit-and-rollback"></a>Véglegesítés és visszaállítás
Tranzakciók mélyen és natív módon közös DocumentDB a JavaScript programozási modell. A JavaScript függvény belül összes művelet vannak automatikusan csomagolt sportautó egyetlen tranzakció csoportban. A JavaScript bármely kivétel nélkül befejeződött, a műveletek az adatbázishoz is lekötött. Gyakorlatilag azt mondja a relációs adatbázisok "MEGKEZDÉSÉHEZ TRANZAKCIÓ" és "VÉGLEGESÍTÉSE TRANZAKCIÓ" kimutatások implicit DocumentDB.  
 
Ha bármelyik kivétel, amely a forgatókönyv propagálja, DocumentDB a JavaScript futtatókörnyezet fog vonhatók vissza a teljes tranzakció. Ahogy a korábbi példa, Kivétel kiváltása megegyezik hatékony DocumentDB a "VISSZAÁLLÍTÁS TRANZAKCIÓ".
 
### <a name="data-consistency"></a>Adatok konzisztencia
Tárolt eljárások és indítók mindig hajtja végre, a DocumentDB webhelycsoport elsődleges replikáját. Ezzel biztosíthatja, hogy belül az olvasás tárolt eljárások ajánlat erős konzisztencia. Felhasználó által definiált függvények lekérdezések az elsődleges és másodlagos kópia futtatható, de azt a megfelelő replika kiválasztása a kért konzisztencia szint teljesítéséhez biztosítása érdekében.

## <a name="bounded-execution"></a>Körülhatárolható végrehajtása
Minden DocumentDB műveletet végre kell hajtania belül a megadott kiszolgáló időtúllépése időtartam kérése. Ezt a korlátot JavaScript-függvények (tárolt eljárások, eseményindítók és felhasználó által definiált függvények) is érinti. Ha egy művelet nem fejezi be a határidőt, a tranzakció visszaáll. JavaScript-függvények kell a határidő időszakon kívülre vagy egy köteg/önéletrajz végrehajtás Lábjegyzetfolytatás-alapú modellt.  

Tárolt eljárások és kezelheti a határidőket indítók fejlesztésének egyszerűsítése érdekében a webhelycsoport objektumot (létrehozása, olvasása, a csere és a dokumentumok és a mellékletek delete) csoportban az összes függvény visszatérési egy logikai érték, amely, hogy az adott művelet befejeződik. Ha ez az érték hamis, érdemes jelzi, hogy a határidő lejárta és, hogy az eljárás a végrehajtás be kell tördelése.  Műveletek aszinkron az első nem fogadott áruházból művelet előtt, ha a tárolt eljárás időben befejeződik, és nem várólista bármilyen további kérések befejezéséhez vannak garantált.  

JavaScript-függvények is vannak határolt a erőforrás-felhasználás. DocumentDB fenntartja egy adatbázis-fiókot kiépített méretének alapján webhelycsoport kapacitása. Átviteli Processzor memóriát, valamint a kérelem mennyiség vagy RUs nevű IO felhasználási normalizált időegység van kifejezhető. JavaScript-függvények is használhat egy rövid idő belül RUs nagyszámú felfelé, és jelenhet meg, ha a webhelycsoport letelik ráta-korlátozott. Erőforrás intenzív tárolt eljárások előfordulhat, hogy is karanténba helyezett állásának egyszerű adatbázis-műveletek.  

### <a name="example-bulk-importing-data-into-a-database-program"></a>Példa: Tömeges adatokat importál egy adatbázis-kezelő program
Az alábbi képen egy példa a tárolt eljárás tömeges – importálás dokumentumok gyűjteménybe írt. Megjegyzés: a tárolt eljárás kezelésének körülhatárolható végrehajtás, jelölje be a logikai createDocument, az eredmény, és kattintson az egyes függvénymeghívás a tárolt eljárás a beszúrt dokumentumok darabszámát nyomon követésére és folytathatja a haladás kötegekben történik át.

    function bulkImport(docs) {
        var collection = getContext().getCollection();
        var collectionLink = collection.getSelfLink();
    
        // The count of imported docs, also used as current doc index.
        var count = 0;
    
        // Validate input.
        if (!docs) throw new Error("The array is undefined or null.");
    
        var docsLength = docs.length;
        if (docsLength == 0) {
            getContext().getResponse().setBody(0);
        }
    
        // Call the create API to create a document.
        tryCreate(docs[count], callback);
    
        // Note that there are 2 exit conditions:
        // 1) The createDocument request was not accepted. 
        //    In this case the callback will not be called, we just call setBody and we are done.
        // 2) The callback was called docs.length times.
        //    In this case all documents were created and we don’t need to call tryCreate anymore. Just call setBody and we are done.
        function tryCreate(doc, callback) {
            var isAccepted = collection.createDocument(collectionLink, doc, callback);
    
            // If the request was accepted, callback will be called.
            // Otherwise report current count back to the client, 
            // which will call the script again with remaining set of docs.
            if (!isAccepted) getContext().getResponse().setBody(count);
        }
    
        // This is called when collection.createDocument is done in order to process the result.
        function callback(err, doc, options) {
            if (err) throw err;
    
            // One more document has been inserted, increment the count.
            count++;
    
            if (count >= docsLength) {
                // If we created all documents, we are done. Just set the response.
                getContext().getResponse().setBody(count);
            } else {
                // Create next document.
                tryCreate(docs[count], callback);
            }
        }
    }

## <a id="trigger"></a>Adatbázis indítók
### <a name="database-pre-triggers"></a>Adatbázis előtti indítók
Eseményindítók, amelyeket végrehajtva vagy dokumentum műveletet által indított DocumentDB ismertetése Megadhatja például előtti az eseményindító, amikor hoz létre egy dokumentumot – Ez az eseményindító előtti fog futni előtt a dokumentum jön létre. Az alábbi képen hogyan előtti indítók segítségével a létrehozandó dokumentum tulajdonságainak ellenőrzése:

    var validateDocumentContentsTrigger = {
        name: "validateDocumentContents",
        body: function validate() {
            var context = getContext();
            var request = context.getRequest();
    
            // document to be created in the current operation
            var documentToCreate = request.getBody();
    
            // validate properties
            if (!("timestamp" in documentToCreate)) {
                var ts = new Date();
                documentToCreate["my timestamp"] = ts.getTime();
            }
    
            // update the document that will be created
            request.setBody(documentToCreate);
        },
        triggerType: TriggerType.Pre,
        triggerOperation: TriggerOperation.Create
    }


És a kiváltó ok mező a megfelelő Node.js ügyféloldali regisztrációs kódot:

    // register pre-trigger
    client.createTriggerAsync(collection.self, validateDocumentContentsTrigger)
        .then(function (response) {
            console.log("Created", response.resource);
            var docToCreate = {
                id: "DocWithTrigger",
                event: "Error",
                source: "Network outage"
            };
    
            // run trigger while creating above document 
            var options = { preTriggerInclude: "validateDocumentContents" };
    
            return client.createDocumentAsync(collection.self,
                  docToCreate, options);
        }, function (error) {
            console.log("Error", error);
        })
    .then(function (response) {
        console.log(response.resource); // document with timestamp property added
    }, function (error) {
        console.log("Error", error);
    });


Eseményindítók előtti nem lehet esetleges bemeneti paramétereket. A kérés objektum kezelhetik a összehívási üzenetben, a művelet társított használható. Ebben az esetben az előzetes eseményindító dokumentum létrehozása a Futtatás, és a kérelem üzenettörzs tartalmaz JSON formátumban létrehozni a dokumentumot.   

Eseményindítók van regisztrálva, amikor a felhasználók beállíthatják, amely a futtatását is lehetővé teszi a műveletek. Ez az eseményindító készült TriggerOperation.Create, ami azt jelenti, hogy a következő nem engedélyezett.

    var options = { preTriggerInclude: "validateDocumentContents" };
    
    client.replaceDocumentAsync(docToReplace.self,
                  newDocBody, options)
    .then(function (response) {
        console.log(response.resource);
    }, function (error) {
        console.log("Error", error);
    });
    
    // Fails, can’t use a create trigger in a replace operation

### <a name="database-post-triggers"></a>Adatbázis utáni eseményindítók
Utáni eseményindítók, előtti eseményindítók, például a dokumentum műveletet, és nem lép az esetleges bemeneti paramétereket. Futtatása **után** a művelet befejeződött, és a válaszüzenetben az ügyfél küldött hozzáférést.   

A következő példa bemutatja a utáni indítók működését:

    var updateMetadataTrigger = {
        name: "updateMetadata",
        body: function updateMetadata() {
            var context = getContext();
            var collection = context.getCollection();
            var response = context.getResponse();
    
            // document that was created
            var createdDocument = response.getBody();
    
            // query for metadata document
            var filterQuery = 'SELECT * FROM root r WHERE r.id = "_metadata"';
            var accept = collection.queryDocuments(collection.getSelfLink(), filterQuery,
                updateMetadataCallback);
            if(!accept) throw "Unable to update metadata, abort";
     
            function updateMetadataCallback(err, documents, responseOptions) {
                if(err) throw new Error("Error" + err.message);
                         if(documents.length != 1) throw 'Unable to find metadata document';
                         
                         var metadataDocument = documents[0];
                         
                         // update metadata
                         metadataDocument.createdDocuments += 1;
                         metadataDocument.createdNames += " " + createdDocument.id;
                         var accept = collection.replaceDocument(metadataDocument._self,
                               metadataDocument, function(err, docReplaced) {
                                      if(err) throw "Unable to update metadata, abort";
                               });
                         if(!accept) throw "Unable to update metadata, abort";
                         return;                    
            }                                                                                           
        },
        triggerType: TriggerType.Post,
        triggerOperation: TriggerOperation.All
    }


Az eseményindító regisztrálni lehet a következő példa látható módon.

    // register post-trigger
    client.createTriggerAsync('dbs/testdb/colls/testColl', updateMetadataTrigger)
        .then(function(createdTrigger) { 
            var docToCreate = { 
                name: "artist_profile_1023",
                artist: "The Band",
                albums: ["Hellujah", "Rotators", "Spinning Top"]
            };
    
            // run trigger while creating above document 
            var options = { postTriggerInclude: "updateMetadata" };
        
            return client.createDocumentAsync(collection.self,
                  docToCreate, options);
        }, function(error) {
            console.log("Error" , error);
        })
    .then(function(response) {
        console.log(response.resource); 
    }, function(error) {
        console.log("Error" , error);
    });


Ez az eseményindító lekérdezi a metaadat-dokumentum, majd frissíti azt az újonnan létrehozott dokumentum részleteit.  

Fontos megjegyzés egyvalamihez **tranzakció alapú** végrehajtása során az DocumentDB eseménykódok. Ez az eseményindító utáni fut, ugyanazon a tranzakción, mint az eredeti dokumentum létrehozása részeként. Emiatt azt kivételhibát az utáni eseményindító (Ha nem frissíti a metaadatokat dokumentumot mondjuk ki) az, ha a teljes tranzakció meghiúsul, és állítható vissza. Nincs dokumentum jön létre, és a kivétel vissza kell.  

##<a id="udf"></a>Felhasználó által definiált függvények
Felhasználó által definiált függvények (UDF) DocumentDB SQL lekérdezési nyelv nyelvtani és egyéni logikájának megvalósítása használják. Azok a csak hívható lekérdezések belül. Ezeket a nem rendelkezik hozzáféréssel a helyi objektumra, és van kialakítva, hogy csak a számítási JavaScript használható. Ezért UDF a DocumentDB szolgáltatás példányon másodlagos futtathatók.  
 
Az alábbi példa létrehoz egy UDF adó megadása különböző bevétel szögletes zárójelek között alapján számítja ki, és azt használja a lekérdezésbe összes azoknak, akik több, mint 20 000 $ fizetett áfával kereséséhez.

    var taxUdf = {
        name: "tax",
        body: function tax(income) {
    
            if(income == undefined) 
                throw 'no input';
    
            if (income < 1000) 
                return income * 0.1;
            else if (income < 10000) 
                return income * 0.2;
            else
                return income * 0.4;
        }
    }


Az UDF ezt követően a következő példa hasonló lekérdezések használható:

    // register UDF
    client.createUserDefinedFunctionAsync('dbs/testdb/colls/testColl', taxUdf)
        .then(function(response) { 
            console.log("Created", response.resource);
    
            var query = 'SELECT * FROM TaxPayers t WHERE udf.tax(t.income) > 20000'; 
            return client.queryDocuments('dbs/testdb/colls/testColl',
                   query).toArrayAsync();
        }, function(error) {
            console.log("Error" , error);
        })
    .then(function(response) {
        var documents = response.feed;
        console.log(response.resource); 
    }, function(error) {
        console.log("Error" , error);
    });

## <a name="javascript-language-integrated-query-api"></a>A JavaScript nyelvi integrált lekérdezés API
Nemcsak a kiállító lekérdezések DocumentDB meg nyelvhelyesség-ellenőrzés SQL, a kiszolgálóoldali SDK lehetővé teszi, hogy optimalizált lekérdezések SQL ismerete nélkül fluent JavaScript felhasználói felülete végezheti el. A JavaScript-lekérdezés API lehetővé teszi, hogy a programozás útján is készíthet lekérdezéseket predikátumalakzat funkciók megkerülhetők chainable függvény a hívások, egy már jól ismert ECMAScript5 a tömb built-ins és a népszerű JavaScript tárakat, például lodash szintaxis. Lekérdezések elemzi a JavaScript futtatókörnyezet hatékony használata az DocumentDB's indexek végrehajtandó vannak.

> [AZURE.NOTE]`__` (dupla, aláhúzás) egy aliasnevet `getContext().getCollection()`.
> <br/>
> Más szóval használható `__` vagy `getContext().getCollection()` a JavaScript API lekérdezés eléréséhez.

Támogatott funkciók többek között:
<ul>
<li>
<b>.. chain(). érték ([visszahívási] [, beállítások])</b>
<ul>
<li>
Egy láncolt hívást, amely be kell fejezni value() kezdődik.
</li>
</ul>
</li>
<li>
<b>szűrő (predicateFunction [, beállítások] [, a visszahívás])</b>
<ul>
<li>
Szűri a bemeneti predikátumalakzat függvénnyel, amely annak érdekében, hogy a szűrő be/ki beviteli dokumentumok eredő az IGAZ vagy hamis értéket adja eredményül. Ez a viselkedik hasonlóan, mint az SQL WHERE záradék.
</li>
</ul>
</li>
<li>
<b>térkép (transformationFunction [, beállítások] [, a visszahívás])</b>
<ul>
<li>
Vonatkozik, amelyhez beviteli elemeire leképezi JavaScript-objektum vagy érték átalakítása függvény megadott leképezésének. Ez működése hasonlít a SELECT záradékban az SQL.
</li>
</ul>
</li>
<li>
<b>pluck ([tulajdonságnév] [, beállítások] [, a visszahívás])</b>
<ul>
<li>
Ez a parancsikont egy térképen, amely egyetlen tulajdonság értékét kibontja az összes beviteli elemet.
</li>
</ul>
</li>
<li>
<b>összeolvasztása ([isShallow] [, beállítások] [, a visszahívás])</b>
<ul>
<li>
Egyesíti, és egy egyetlen tömbképlettel minden beviteli elemből tömbök összeolvasztja. Ez működése hasonlít a LINQ SelectMany.
</li>
</ul>
</li>
<li>
<b>sortBy ([predikátumok] [, beállítások] [, a visszahívás])</b>
<ul>
<li>
A dokumentumok új készlet készít a bemeneti dokumentum adatfolyamban növekvő sorrendben, a megadott predikátumok használata a dokumentumok rendezésével. Ez működése hasonlít egy SQL-ORDER BY záradék.
</li>
</ul>
</li>
<li>
<b>sortByDescending ([predikátumok] [, beállítások] [, a visszahívás])</b>
<ul>
<li>
A dokumentumok új készlet konzerv a bemeneti dokumentum adatfolyamban csökkenő sorrendben, a megadott predikátumok használata a dokumentumok rendezésével. Ez működése hasonlít egy SQL-x DESC ORDER BY záradék.
</li>
</ul>
</li>
</ul>


Belül predikátumok és/vagy nézetkijelölő funkciókat tartalmazza, amikor a következő JavaScript szerkezeteket első automatikusan optimalizálva közvetlenül a DocumentDB tárgymutatók használata:

* Egyszerű operátor: =-* / % |} ^ &amp; == != === !=== &lt; &gt; &lt;= &gt;= || &amp;&amp; &lt;&lt; &gt;&gt; &gt;&gt;&gt;! ~
* Szövegkonstansok, beleértve az objektum konstans: {}
* var és a feladó

A következő JavaScript elem nem első optimalizálva DocumentDB tárgymutatók használata:

* Control flow (például ha, miközben)
* Hívás függvény

További tudnivalókért lásd a [Kiszolgálóoldali JSDocs](http://azure.github.io/azure-documentdb-js-server/).

### <a name="example-write-a-stored-procedure-using-the-javascript-query-api"></a>Példa: Írja be a tárolt eljárás a JavaScript API lekérdezés használata

A következő kódot minta képen, a lekérdezés JavaScript API hogyan használható a tárolt eljárás környezetében. A tárolt eljárás szúr be egy dokumentumot, és adja meg a bemeneti paraméterre és frissíti a metaadat-alapú dokumentumok, használja a `__.filter()` módszer minSize, maxSize és totalSize a bemeneti dokumentum mérete tulajdonság alapján.

    /**
     * Insert actual doc and update metadata doc: minSize, maxSize, totalSize based on doc.size.
     */
    function insertDocumentAndUpdateMetadata(doc) {
      // HTTP error codes sent to our callback funciton by DocDB server.
      var ErrorCode = {
        RETRY_WITH: 449,
      }

      var isAccepted = __.createDocument(__.getSelfLink(), doc, {}, function(err, doc, options) {
        if (err) throw err;

        // Check the doc (ignore docs with invalid/zero size and metaDoc itself) and call updateMetadata.
        if (!doc.isMetadata && doc.size > 0) {
          // Get the meta document. We keep it in the same collection. it's the only doc that has .isMetadata = true.
          var result = __.filter(function(x) {
            return x.isMetadata === true
          }, function(err, feed, options) {
            if (err) throw err;

            // We assume that metadata doc was pre-created and must exist when this script is called.
            if (!feed || !feed.length) throw new Error("Failed to find the metadata document.");

            // The metadata document.
            var metaDoc = feed[0];

            // Update metaDoc.minSize:
            // for 1st document use doc.Size, for all the rest see if it's less than last min.
            if (metaDoc.minSize == 0) metaDoc.minSize = doc.size;
            else metaDoc.minSize = Math.min(metaDoc.minSize, doc.size);

            // Update metaDoc.maxSize.
            metaDoc.maxSize = Math.max(metaDoc.maxSize, doc.size);

            // Update metaDoc.totalSize.
            metaDoc.totalSize += doc.size;

            // Update/replace the metadata document in the store.
            var isAccepted = __.replaceDocument(metaDoc._self, metaDoc, function(err) {
              if (err) throw err;
              // Note: in case concurrent updates causes conflict with ErrorCode.RETRY_WITH, we can't read the meta again 
              //       and update again because due to Snapshot isolation we will read same exact version (we are in same transaction).
              //       We have to take care of that on the client side.
            });
            if (!isAccepted) throw new Error("replaceDocument(metaDoc) returned false.");
          });
          if (!result.isAccepted) throw new Error("filter for metaDoc returned false.");
        }
      });
      if (!isAccepted) throw new Error("createDocument(actual doc) returned false.");
    }

## <a name="sql-to-javascript-cheat-sheet"></a>SQL Javascript-csal lapra
Az alábbi táblázat bemutatja az SQL-lekérdezések és a megfelelő JavaScript-lekérdezések.

Az SQL-lekérdezések dokumentum tulajdonság kulcsok, (pl. `doc.id`)-és nagybetűk.

<br/>
<table border="1" width="100%">
<colgroup>
<col span="1" style="width: 40%;">
<col span="1" style="width: 40%;">
<col span="1" style="width: 20%;">
</colgroup>
<tbody>
<tr>
<th>SQL</th>
<th>A JavaScript lekérdezés API</th>
<th>Részletek</th>
</tr>
<tr>
<td>
<pre>
VÁLASSZA a * a dokumentumok
</pre>
</td>
<td>
<pre>
__.Map(Function(doc) {visszatérési dokumentum;});
</pre>
</td>
<td>Minden dokumentum (paginated folytatását jelző jogkivonat együtt), az eredmény van.</td>
</tr>
<tr>
<td>
<pre>
SELECT docs.id, docs.message, üzenet, docs.actions FROM dokumentumok
</pre>
</td>
<td>
<pre>
__.Map(Function(doc) {visszatérési {azonosító: doc.id, az üzenet: doc.message, műveletek: doc.actions};});
</pre>
</td>
<td>Projektek a azonosító, az üzenet (az üzenet alias) és a művelet az összes dokumentumot.</td>
</tr>
<tr>
<td>
<pre>
VÁLASSZA a * a dokumentumok WHERE docs.id="X998_Y998"
</pre>
</td>
<td>
<pre>
__.Filter(Function(doc) {doc.id visszatérési === "X998_Y998";});
</pre>
</td>
<td>A kulcsszó-dokumentumokat lekérdezések: id = "X998_Y998".</td>
</tr>
<tr>
<td>
<pre>
VÁLASSZA a * a dokumentumok, ahol ARRAY_CONTAINS(docs. Címkék, 123)
</pre>
</td>
<td>
<pre>
__.Filter(Function(x) {x.Tags visszatérési & & x.Tags.indexOf(123) > -1;});
</pre>
</td>
<td>Dokumentumok, amelyek a címkék tulajdonság és a címkék lekérdezések 123 értéket tartalmazó tömb.</td>
</tr>
<tr>
<td>
<pre>
SELECT docs.id, mint üzenet FROM dokumentumok docs.message WHERE docs.id="X998_Y998"
</pre>
</td>
<td>
<pre>
__.chain() .filter(function(doc) {doc.id visszatérési === "X998_Y998";}) .map(function(doc) {visszatérési {azonosító: doc.id, az üzenet: doc.message};}) .value();
</pre>
</td>
<td>Dokumentumok egy predikátumok, a lekérdezések id = "X998_Y998", és kattintson a projektek az azonosító és az üzenet (az üzenet aliassal).</td>
</tr>
<tr>
<td>
<pre>
Jelölje ki a érték címke a dokumentumok ILLESZTÉS címke lévő dokumentumok. Címkék ORDER BY docs._ts
</pre>
</td>
<td>
<pre>
__.chain() .filter(function(doc) {visszatérési dokumentumot. Címkék & & Array.isArray (dokumentumot. Címkék); }) .sortBy(function(doc) {visszatérési doc._ts;}) .pluck("tags") .flatten() .value()
</pre>
</td>
<td>Dokumentumok, amelyek egy tömb tulajdonságot, a címkéket, és az eredményül kapott dokumentumok rendezése _ts időbélyeg rendszer tulajdonság és a projektek, majd + összeolvasztja a címkék tömb szűrőket.</td>
</tr>
</tbody>
</table>

## <a name="runtime-support"></a>Futási idejű támogatás
A fő JavaScript nyelvekkel kapcsolatos szolgáltatások által [ECMA-262](http://www.ecma-international.org/publications/standards/Ecma-262.htm)használ, a legtöbb [DocumentDB JavaScript kiszolgálóoldali SDK](http://azure.github.io/azure-documentdb-js-server/) nyújt támogatást.

### <a name="security"></a>Biztonsági
A JavaScript tárolt eljárások és indítók védőfal mögé helyezett, hogy egy parancsprogramot hatásai nem előfordulhat a másik adatbázis szintre pillanatkép tranzakció elkülönítése keresztül nélkül. A futási idejű környezetben készletezett, de a után minden futtatásakor a környezet törölve. Így azok is garantált megbízható minden várt hatásai, egymástól.

### <a name="pre-compilation"></a>Előzetes összeállítása
Tárolt eljárások, eseményindítók és UDF hogy a jövőben elkerülhesse az összeállítás költség az egyes parancsfájl meghívási időpontjában implicit módon lefordított bájt kód formátumra. Biztosítja, tárolt eljárások meghívásához gyors, és egy alacsony helyigénye van.

## <a name="client-sdk-support"></a>Ügyfél SDK támogatása
A [Node.js](documentdb-sdk-node.md) ügyfél mellett a DocumentDB [.NET](documentdb-sdk-dotnet.md), [Java](documentdb-sdk-java.md), [JavaScript](http://azure.github.io/azure-documentdb-js/)és [Python SDK](documentdb-sdk-python.md)támogat. Tárolt eljárások, eseményindítók és UDF hozhatja létre, és végrehajtása, valamint ezek SDK bármelyikét használja. A következő példa bemutatja, hogyan hozhat létre, és hajtsa végre a tárolt eljárás a .NET ügyfélprogramban. Figyelje meg, hogyan a .NET típusokat a tárolt eljárás szerint JSON átadott és olvastatni.

    var markAntiquesSproc = new StoredProcedure
    {
        Id = "ValidateDocumentAge",
        Body = @"
                function(docToCreate, antiqueYear) {
                    var collection = getContext().getCollection();    
                    var response = getContext().getResponse();    
    
                    if(docToCreate.Year != undefined && docToCreate.Year < antiqueYear){
                        docToCreate.antique = true;
                    }
    
                    collection.createDocument(collection.getSelfLink(), docToCreate, {}, 
                        function(err, docCreated, options) { 
                            if(err) throw new Error('Error while creating document: ' + err.message);                              
                            if(options.maxCollectionSizeInMb == 0) throw 'max collection size not found'; 
                            response.setBody(docCreated);
                    });
            }"
    };
    
    // register stored procedure
    StoredProcedure createdStoredProcedure = await client.CreateStoredProcedureAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"), markAntiquesSproc);
    dynamic document = new Document() { Id = "Borges_112" };
    document.Title = "Aleph";
    document.Year = 1949;
    
    // execute stored procedure
    Document createdDocument = await client.ExecuteStoredProcedureAsync<Document>(UriFactory.CreateStoredProcedureUri("db", "coll", "sproc"), document, 1920);


Ez a példa bemutatja a [.NET SDK](https://msdn.microsoft.com/library/azure/dn948556.aspx) használata létrehozása előtti az eseményindító, és hozzon létre egy dokumentumot az eseményindító engedélyezve van. 

    Trigger preTrigger = new Trigger()
    {
        Id = "CapitalizeName",
        Body = @"function() {
            var item = getContext().getRequest().getBody();
            item.id = item.id.toUpperCase();
            getContext().getRequest().setBody(item);
        }",
        TriggerOperation = TriggerOperation.Create,
        TriggerType = TriggerType.Pre
    };
    
    Document createdItem = await client.CreateDocumentAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"), new Document { Id = "documentdb" },
        new RequestOptions
        {
            PreTriggerInclude = new List<string> { "CapitalizeName" },
        });


És a következő példa bemutatja, hogyan hozhat létre a felhasználó által definiált függvény (UDF), és vele [DocumentDB SQL-lekérdezés](documentdb-sql-query.md).

    UserDefinedFunction function = new UserDefinedFunction()
    {
        Id = "LOWER",
        Body = @"function(input) 
        {
            return input.toLowerCase();
        }"
    };
    
    foreach (Book book in client.CreateDocumentQuery(UriFactory.CreateDocumentCollectionUri("db", "coll"),
        "SELECT * FROM Books b WHERE udf.LOWER(b.Title) = 'war and peace'"))
    {
        Console.WriteLine("Read {0} from query", book);
    }

## <a name="rest-api"></a>REST API-VAL
Az összes DocumentDB műveleteket végezheti el RESTful módon. Tárolt eljárások, eseményindítók és a felhasználó által definiált függvények lehet regisztrálni gyűjtemény a HTTP-bejegyzés használatával. A következő példa bemutatja, hogyan lehet regisztrálni a tárolt eljárás:

    POST https://<url>/sprocs/ HTTP/1.1
    authorization: <<auth>>
    x-ms-date: Thu, 07 Aug 2014 03:43:10 GMT
    
    
    var x = {
      "name": "createAndAddProperty",
      "body": function (docToCreate, addedPropertyName, addedPropertyValue) {
                var collectionManager = getContext().getCollection();
                collectionManager.createDocument(
                    collectionManager.getSelfLink(),
                    docToCreate,
                    function(err, docCreated) {
                      if(err) throw new Error('Error:  ' + err.message);
                      docCreated[addedPropertyName] = addedPropertyValue;
                      getContext().getResponse().setBody(docCreated);
                    });
            }
    }


A tárolt eljárás hajtja végre a URI adatbázisok/testdb/colls/testColl/sprocs elleni POST kérelem tartalmazó hozhat létre a tárolt eljárás a szervezethez regisztrált. Eseményindítók és UDF lehet regisztrálni hasonlóan egy BEJEGYZÉSBEN/eseményindítók és /udfs szemben a kurzor azáltal.
Tárolt eljárás lehet, majd a bejegyzés kérelmet szemben az erőforrás-hivatkozás küldene hajtja végre:

    POST https://<url>/sprocs/<sproc> HTTP/1.1
    authorization: <<auth>>
    x-ms-date: Thu, 07 Aug 2014 03:43:20 GMT
    
    
    [ { "name": "TestDocument", "book": "Autumn of the Patriarch"}, "Price", 200 ]


Ebben az esetben a tárolt eljárás a bemeneti átadott összehívás törzsébe. Megjegyzés: a bemeneti átadott JSON tömbként bemeneti paramétereket. A tárolt eljárás a első bemeneti válasz szervezet dokumentumként vesz igénybe. A válasz, akkor kap a következőképpen történik:

    HTTP/1.1 200 OK
     
    { 
      name: 'TestDocument',
      book: ‘Autumn of the Patriarch’,
      id: ‘V7tQANV3rAkDAAAAAAAAAA==‘,
      ts: 1407830727,
      self: ‘dbs/V7tQAA==/colls/V7tQANV3rAk=/docs/V7tQANV3rAkDAAAAAAAAAA==/’,
      etag: ‘6c006596-0000-0000-0000-53e9cac70000’,
      attachments: ‘attachments/’,
      Price: 200
    }


Eseményindítók, tárolt eljárások, eltérően közvetlenül nem hajtható végre. Ehelyett végrehajtás dokumentum műveletet részeként. Azt adhatja meg a HTTP-fejlécek használatával összehívással futtatásához eseményindítók. Az alábbiakban kérelem dokumentumot szeretne készíteni.

    POST https://<url>/docs/ HTTP/1.1
    authorization: <<auth>>
    x-ms-date: Thu, 07 Aug 2014 03:43:10 GMT
    x-ms-documentdb-pre-trigger-include: validateDocumentContents 
    x-ms-documentdb-post-trigger-include: bookCreationPostTrigger
    
    
    {
       "name": "newDocument",
       “title”: “The Wizard of Oz”,
       “author”: “Frank Baum”,
       “pages”: 92
    }


A kérés futtatásához a előtti eseményindító Itt x-ms-documentdb-pre-trigger-include fejlécében van megadva. Ennek megfelelően bármely utáni indítók kapnak a x-ms-documentdb-post-trigger-include fejlécében. Megjegyzendő, hogy mindkét előtti és utáni indítók egy adott kérelem adható meg.

## <a name="sample-code"></a>Minta kódot.

A [Github tárházba](https://github.com/Azure/azure-documentdb-js-server/tree/master/samples)talál további kiszolgálóoldali kód példákat (beleértve a [tömeges törlési](https://github.com/Azure/azure-documentdb-js-server/tree/master/samples/stored-procedures/bulkDelete.js)és [frissítése](https://github.com/Azure/azure-documentdb-js-server/tree/master/samples/stored-procedures/update.js)).

Meg szeretné osztani a Soft tárolt eljárás? Küldjön nekünk az kérelmének ki! 

## <a name="next-steps"></a>Következő lépések

Után egy vagy több tárolt eljárások, eseményindítók és a felhasználó által definiált függvények létrehozott töltse be őket, és megtekintheti azokat a parancsprogram Intézővel Azure-portálon. További tudnivalókért olvassa el a [Nézet tárolt eljárások, eseményindítók, és a DocumentDB parancsfájl Intézővel felhasználó által definiált függvények](documentdb-view-scripts.md)című témakört.

Is találhat az alábbi hivatkozások és források hasznos, ha többet szeretne tudni a DocumentDB kiszolgálóoldali programozás a PATH:

- [Azure DocumentDB SDK](https://msdn.microsoft.com/library/azure/dn781482.aspx)
- [DocumentDB Studio](https://github.com/mingaliu/DocumentDBStudio/releases)
- [JSON](http://www.json.org/) 
- [A JavaScript ECMA-262](http://www.ecma-international.org/publications/standards/Ecma-262.htm)
- [A JavaScript – JSON típus rendszer](http://www.json.org/js.html) 
- [Biztonságos és hordozható adatbázis bővítési](http://dl.acm.org/citation.cfm?id=276339) 
- [Szolgáltatás lépések adatbázis-architektúra](http://dl.acm.org/citation.cfm?id=1066267&coll=Portal&dl=GUIDE) 
- [A Microsoft SQL Server-alapú .NET futtatókörnyezet szolgáltatónál](http://dl.acm.org/citation.cfm?id=1007669)
