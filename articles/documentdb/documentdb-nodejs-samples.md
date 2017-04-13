<properties
    pageTitle="Példák NoSQL Node.js DocumentDB |} Microsoft Azure"
    description="Található NoSQL Node.js példák github a gyakori feladatok a DocumentDB, beleértve a JSON dokumentumok CRUD műveleteket NoSQL adatbázisokban."
    keywords="NODE.js példák"
    services="documentdb"
    authors="moderakh"
    manager="jhubbard"
    editor="monicar"
    documentationCenter="nodejs"/>

<tags
    ms.service="documentdb"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="moderakh"/>


# <a name="documentdb-nodejs-examples"></a>DocumentDB Node.js példák

> [AZURE.SELECTOR]
- [.NET-példák](documentdb-dotnet-samples.md)
- [NODE.js példák](documentdb-nodejs-samples.md)
- [Python példák](documentdb-python-samples.md)
- [Azure kód minta gyűjtemény](https://azure.microsoft.com/documentation/samples/?service=documentdb)

Minta megoldások, amelyek CRUD műveletek és a más Azure DocumentDB erőforrások gyakori műveletek végrehajtása az [azure-documentdb-nodejs](https://github.com/Azure/azure-documentdb-node/tree/master/samples) GitHub tárházba szerepelnek. Ebben a cikkben:

- A feladatok mindegyik a Node.js példa projektfájlok mutató hivatkozásokat tartalmaz.
- A kapcsolódó API mutató hivatkozásokat tartalma hivatkozást.

**Előfeltételek**

1. Ezek a példák Node.js használni kívánt Azure fiók van szüksége:
    - [Nyissa meg az ingyenes Azure-fiók](https://azure.microsoft.com/pricing/free-trial/)van lehetősége: jóváírások kap próbálja ki az fizetett Azure szolgáltatások is használhatja, és a megszokott ezek után akár tarthatja a fiók, és használata ingyenes Azure szolgáltatások, például webhelyek. A hitelkártya soha nem megterheljük, kivéve, ha kifejezetten az beállítások módosítása és kérdezzen rá kell fizetnie.
   - [Visual Studio előfizetői előnyeinek aktiválása](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/)közül választhat: A Visual Studio előfizetés lépve jóváírások havonta fizetett Azure szolgáltatások használható.
2. A [Node.js SDK](documentdb-sdk-node.md)is szükséges.

    > [AZURE.NOTE] Minden egyes minta önálló, beállítja magát, és a törli a köteggel maga után. Például a minták hiba [DocumentClient.createCollection](http://azure.github.io/azure-documentdb-node/DocumentClient.html#createCollection)több hívásokat. Minden alkalommal, amikor ez történik az előfizetés fognak számlát kapni a teljesítmény réteg eredményezne a webhelycsoport használati használatát az 1 óra esetében.

## <a name="database-examples"></a>Adatbázis-példák

A [app.js](https://github.com/Azure/azure-documentdb-node/blob/master/samples/DatabaseManagement/app.js) fájlt a [DatabaseManagement](https://github.com/Azure/azure-documentdb-node/tree/master/samples/DatabaseManagement) projekt megtudhatja, hogy miként végezze el az alábbi műveleteket.

Tevékenység | API-hivatkozás
--- | ---
[Adatbázis létrehozása](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DatabaseManagement/app.js#L121-L131) | [DocumentClient.createDatabase](http://azure.github.io/azure-documentdb-node/DocumentClient.html#createDatabase)
[Lekérdezés az adatbázis-fiók](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DatabaseManagement/app.js#L146-L171) | [DocumentClient.queryDatabases](http://azure.github.io/azure-documentdb-node/DocumentClient.html#queryDatabases)
[Olvassa el az adatbázis-azonosító szerint](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DatabaseManagement/app.js#L89-L99) | [DocumentClient.readDatabase](http://azure.github.io/azure-documentdb-node/DocumentClient.html#readDatabase)
[Lista-adatbázisokat a fiókjához](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DatabaseManagement/app.js#L111-L119) | [DocumentClient.readDatabases](http://azure.github.io/azure-documentdb-node/DocumentClient.html#readDatabase)
[Adatbázis törlése](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DatabaseManagement/app.js#L133-L144) | [DocumentClient.deleteDatabase](http://azure.github.io/azure-documentdb-node/DocumentClient.html#deleteDatabase)

## <a name="collection-examples"></a>A webhelycsoport-példák

A [app.js](https://github.com/Azure/azure-documentdb-node/blob/master/samples/CollectionManagement/app.js) fájlt a [CollectionManagement](https://github.com/Azure/azure-documentdb-node/tree/master/samples/CollectionManagement) projekt megtudhatja, hogy miként végezze el az alábbi műveleteket.

Tevékenység | API-hivatkozás
--- | ---
[A webhelycsoport létrehozása](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.CollectionManagement/app.js#L97-L118) | [DocumentClient.createCollection](http://azure.github.io/azure-documentdb-node/DocumentClient.html#createCollection)
[Olvassa el az adatbázis összes webhelycsoportok listáját](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.CollectionManagement/app.js#L120-L130) | [DocumentClient.readCollections](http://azure.github.io/azure-documentdb-node/DocumentClient.html#readCollections)
[Egy webhelycsoport ismerkedjen _self](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.CollectionManagement/app.js#L132-L141) | [DocumentClient.readCollection](http://azure.github.io/azure-documentdb-node/DocumentClient.html#readCollection)
[A webhelycsoport-azonosító szerint beszerzése](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.CollectionManagement/app.js#L143-L156) | [DocumentClient.readCollection](http://azure.github.io/azure-documentdb-node/DocumentClient.html#readCollection)
[Ismerkedés a teljesítmény réteg gyűjtemény](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.CollectionManagement/app.js#L158-L186) | [DocumentQueryable.queryOffers](http://azure.github.io/azure-documentdb-node/DocumentClient.html#queryOffers)
[Gyűjtemény a teljesítmény szint módosítása](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.CollectionManagement/app.js#L188-L202) | [DocumentClient.replaceOffer](http://azure.github.io/azure-documentdb-node/DocumentClient.html#replaceOffer)
[Egy webhelycsoport törlése](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.CollectionManagement/app.js#L204-L215) | [DocumentClient.deleteCollection](http://azure.github.io/azure-documentdb-node/DocumentClient.html#deleteCollection)

## <a name="document-examples"></a>Dokumentum-példák

A [app.js](https://github.com/Azure/azure-documentdb-node/blob/master/samples/DocumentManagement/app.js) fájlt a [DocumentManagement](https://github.com/Azure/azure-documentdb-node/tree/master/samples/DocumentManagement) projekt megtudhatja, hogy miként végezze el az alábbi műveleteket.

Tevékenység | API-hivatkozás
--- | ---
[Dokumentumok létrehozása](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DocumentManagement/app.js#L153-L177) | [DocumentClient.createDocument](http://azure.github.io/azure-documentdb-node/DocumentClient.html#createDocument)
[Olvassa el a dokumentum a webhelycsoport-hírcsatorna](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DocumentManagement/app.js#L179-L189) | [DocumentClient.readDocument](http://azure.github.io/azure-documentdb-node/DocumentClient.html#readDocument)
[Dokumentumok azonosító alapján olvasása](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DocumentManagement/app.js#L191-L201) | [DocumentClient.readDocument](http://azure.github.io/azure-documentdb-node/DocumentClient.html#readDocument)
[Ha a dokumentum módosítását, olvassa el a dokumentumot](https://github.com/Azure/azure-documentdb-node/blob/0778eadea7abb2af41e8c22a239dc872c584f421/samples/DocumentManagement/app.js#L79-L107) | [DocumentClient.readDocument](http://azure.github.io/azure-documentdb-node/DocumentClient.html#readDocument)<br/>[RequestOptions.accessCondition](http://azure.github.io/azure-documentdb-node/global.html#RequestOptions)
[Dokumentumok lekérdezés](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DocumentManagement/app.js#L82-L110) | [DocumentClient.queryDocuments](http://azure.github.io/azure-documentdb-node/DocumentClient.html#queryDocuments)
[Dokumentum cseréje](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DocumentManagement/app.js#L112-L119) |  [DocumentClient.replaceDocument](http://azure.github.io/azure-documentdb-node/DocumentClient.html#replaceDocument)
[Dokumentum cserélje feltételes ETag ellenőrzése](https://github.com/Azure/azure-documentdb-node/blob/0778eadea7abb2af41e8c22a239dc872c584f421/samples/DocumentManagement/app.js#L147-L164) |  [DocumentClient.replaceDocument](http://azure.github.io/azure-documentdb-node/DocumentClient.html#replaceDocument)<br/>[RequestOptions.accessCondition](http://azure.github.io/azure-documentdb-node/global.html#RequestOptions)
[Dokumentumok törlése](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DocumentManagement/app.js#L122-L133) | [DocumentClient.deleteDocument](http://azure.github.io/azure-documentdb-node/DocumentClient.html#deleteDocument)

## <a name="indexing-examples"></a>Az indexelési példák

A [app.js](https://github.com/Azure/azure-documentdb-node/blob/master/samples/IndexManagement/app.js) fájlt a [IndexManagement](https://github.com/Azure/azure-documentdb-node/tree/master/samples/IndexManagement) projekt megtudhatja, hogy miként végezze el az alábbi műveleteket.

Tevékenység | API-hivatkozás
--- | ---
Hozzon létre egy webhelycsoport alapértelmezett indexelés | [DocumentClient.createDocument](http://azure.github.io/azure-documentdb-node/DocumentClient.html)
[Egy adott dokumentum manuális indexelése](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.IndexManagement/app.js#L185-L238) | [indexingDirective: "include"](http://azure.github.io/azure-documentdb-node/global.html#indexingDirective)
[Egy adott dokumentum manuálisan zárni az index](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.IndexManagement/app.js#L120-L183) | [RequestOptions.indexingDirective](http://azure.github.io/azure-documentdb-node/global.html#RequestOptions)
[Tömeges importálás Lusta indexelés használja, vagy ismerje meg nehéz gyűjtemények](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.IndexManagement/app.js#L240-L269) | [IndexingMode.Lazy](http://azure.github.io/azure-documentdb-node/global.html#IndexingMode)
[A dokumentum adott útvonalak szerepeltetése az indexelés](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.IndexManagement/app.js#L433-L444) | [IndexingPolicy.IncludedPaths](http://azure.github.io/azure-documentdb-node/global.html#IndexingPolicy)
[Az indexelés bizonyos elérési utak kizárása](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.IndexManagement/app.js#L427-L450) | [ExcludedPath](http://azure.github.io/azure-documentdb-node/global.html#IndexingPolicy)
[A keresés engedélyezése a karakterlánc görbére tartomány művelet során](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.IndexManagement/app.js#L271-L347)| [ExcludedPath.EnableScanInQuery](http://azure.github.io/azure-documentdb-node/global.html#FeedOptions)
[Egy karakterlánc elérési tartomány index létrehozása](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.IndexManagement/app.js#L349-L425) | [DocumentClient.queryDocument](http://azure.github.io/azure-documentdb-node/DocumentClient.html#queryDocument)
[Hozzon létre egy webhelycsoport alapértelmezett indexPolicy, majd a online frissítése](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.IndexManagement/app.js#L519-L614) | [DocumentClient.createCollection](http://azure.github.io/azure-documentdb-node/DocumentClient.html#createCollection)<br> [DocumentClient.replaceCollection#replaceCollection](http://azure.github.io/azure-documentdb-node/DocumentClient.html)

Az indexelés kapcsolatos további tudnivalókért lásd: a [DocumentDB indexelési házirendek](documentdb-indexing-policies.md).

## <a name="server-side-programming-examples"></a>Kiszolgálóoldali programozási példákat

A [app.js](https://github.com/Azure/azure-documentdb-node/blob/master/samples/ServerSideScripts/app.js) fájlt a [ServerSideScripts](https://github.com/Azure/azure-documentdb-node/tree/master/samples/ServerSideScripts) projekt megtudhatja, hogy miként végezze el az alábbi műveleteket.

Tevékenység | API-hivatkozás
--- | ---
[Tárolt eljárás létrehozása](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.ServerSideScripts/app.js#L44-L71) | [DocumentClient.createStoredProcedure](http://azure.github.io/azure-documentdb-node/DocumentClient.html#createStoredProcedure)
[Hajtsa végre a tárolt eljárás](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.ServerSideScripts/app.js#L73-L90) | [DocumentClient.executeStoredProcedure](http://azure.github.io/azure-documentdb-node/DocumentClient.html#executeStoredProcedure)

Kiszolgálóoldali programozás kapcsolatos további tudnivalókért lásd [DocumentDB kiszolgálóoldali programozás: tárolt eljárások, adatbázis eseményindítók és UDF](documentdb-programming.md).

## <a name="partitioning-examples"></a>Példák szétválasztás

A [app.js](https://github.com/Azure/azure-documentdb-node/blob/master/samples/Partitioning/app.js) fájlt a [particionálására](https://github.com/Azure/azure-documentdb-node/tree/master/samples/Partitioning) projekt megtudhatja, hogy miként végezze el az alábbi műveleteket.

Tevékenység | API-hivatkozás
--- | ---
[Egy HashPartitionResolver használata](https://github.com/Azure/azure-documentdb-node/blob/ce0fc3c4e70b0279091a1e03620a668d93a14fc2/samples/Partitioning/app.js#L53-L103) | [HashPartitionResolver](http://azure.github.io/azure-documentdb-node/HashPartitionResolver.html)

További információt a szétválasztás DocumentDB adatainak [DocumentDB partíciót és arányának adatainak](documentdb-partition-data.md)megtekintése
