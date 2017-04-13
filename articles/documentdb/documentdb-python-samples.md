<properties 
    pageTitle="Példák NoSQL Python DocumentDB |} Microsoft Azure" 
    description="Található NoSQL Python példák github a gyakori feladatok a DocumentDB, beleértve a JSON dokumentumok CRUD műveleteket NoSQL adatbázisokban." 
    keywords="Python példák"
    services="documentdb" 
    authors="moderakh" 
    manager="jhubbard" 
    editor="monicar" 
    documentationCenter="python"/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="04/18/2016" 
    ms.author="moderakh"/>


# <a name="documentdb-python-examples"></a>DocumentDB Python példák

> [AZURE.SELECTOR]
- [.NET-példák](documentdb-dotnet-samples.md)
- [NODE.js példák](documentdb-nodejs-samples.md)
- [Python példák](documentdb-python-samples.md)
- [Azure kód minta gyűjtemény](https://azure.microsoft.com/documentation/samples/?service=documentdb)

Minta megoldások, amelyek CRUD műveletek és a más Azure DocumentDB erőforrások gyakori műveletek végrehajtása az [azure-documentdb-python](https://github.com/Azure/azure-documentdb-python/tree/master/samples) GitHub tárházba szerepelnek. Ebben a cikkben:

- A feladatok mindegyik a Python példa projektfájlok mutató hivatkozásokat tartalmaz. 
- A kapcsolódó API mutató hivatkozásokat tartalma hivatkozást.

**Előfeltételek**

1. Ezek a példák Python használni kívánt Azure fiók van szüksége:
    - [Nyissa meg az ingyenes Azure-fiók](https://azure.microsoft.com/pricing/free-trial/)van lehetősége: jóváírások kap próbálja ki az fizetett Azure szolgáltatások is használhatja, és a megszokott ezek után akár tarthatja a fiók, és használata ingyenes Azure szolgáltatások, például webhelyek. A hitelkártya soha nem megterheljük, kivéve, ha kifejezetten az beállítások módosítása és kérdezzen rá kell fizetnie.
   - [Visual Studio előfizetői előnyeinek aktiválása](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/)közül választhat: A Visual Studio előfizetés lépve jóváírások havonta fizetett Azure szolgáltatások használható.
2. A [Python SDK](documentdb-sdk-python.md)is szükséges. 

    > [AZURE.NOTE] Minden egyes minta önálló, beállítja magát, és a törli a köteggel maga után. Például a minták hiba [document_client több hívásokat. CreateCollection](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html). Minden alkalommal, amikor ez történik az előfizetés fognak számlát kapni a teljesítmény réteg eredményezne a webhelycsoport használati használatát az 1 óra esetében. 

## <a name="database-examples"></a>Adatbázis-példák

A [Program.py](https://github.com/Azure/azure-documentdb-python/tree/master/samples/DatabaseManagement/Program.py) fájlt a [DatabaseManagement](https://github.com/Azure/azure-documentdb-python/tree/master/samples/DatabaseManagement) projekt megtudhatja, hogy miként végezze el az alábbi műveleteket.

Tevékenység | API-hivatkozás
--- | ---
[Adatbázis létrehozása](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L65-L76) | [document_client. CreateDatabase](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html)
[Lekérdezés az adatbázis-fiók](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L49-L62) | [document_client. QueryDatabases](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html)
[Olvassa el az adatbázis-azonosító szerint](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L79-L96) | [document_client. ReadDatabase](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html)
[Lista-adatbázisokat a fiókjához](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L99-L110) | [document_client. ReadDatabases](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html)
[Adatbázis törlése](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L113-L126) | [document_client. DeleteDatabase](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html)

## <a name="collection-examples"></a>A webhelycsoport-példák 

A [Program.py](https://github.com/Azure/azure-documentdb-python/tree/master/samples/CollectionManagement/Program.py) fájlt a [CollectionManagement](https://github.com/Azure/azure-documentdb-python/tree/master/samples/CollectionManagement) projekt megtudhatja, hogy miként végezze el az alábbi műveleteket.

Tevékenység | API-hivatkozás
--- | ---
[A webhelycsoport létrehozása](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L84-L135) | [document_client. CreateCollection](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection)
[Olvassa el az adatbázis összes webhelycsoportok listáját](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L198-L225) | [document_client. ListCollections](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection)
[A webhelycsoport-azonosító szerint beszerzése](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L178-L195) | [document_client. ReadCollection](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection)
[Ismerkedés a teljesítmény réteg gyűjtemény](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L139-L161) | [DocumentQueryable.QueryOffers](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection)
[Gyűjtemény a teljesítmény szint módosítása](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L163-L175) | [document_client. ReplaceOffer](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection)
[Egy webhelycsoport törlése](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L212-L225) | [document_client. DeleteCollection](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection)
