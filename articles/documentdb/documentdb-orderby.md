<properties 
    pageTitle="Order By használatával DocumentDB adatok rendezése |} Microsoft Azure" 
    description="Megtudhatja, hogyan használható ORDER BY DocumentDB lekérdezések LINQ és az SQL és ORDER BY lekérdezések az indexelési házirend megadása." 
    services="documentdb" 
    authors="arramac" 
    manager="jhubbard" 
    editor="cgronlun" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/03/2016" 
    ms.author="arramac"/>

# <a name="sorting-documentdb-data-using-order-by"></a>Order By használatával DocumentDB adatok rendezése
Microsoft Azure DocumentDB SQL használata JSON dokumentumok lekérdezésekor dokumentumok támogatja. Lekérdezés eredménye a ORDER BY záradék használata a lekérdezés SQL-utasítások kell rendelni.

Ez a cikk elolvasása, után is az alábbi kérdésekre választ: 

- Hogyan lekérdezése az Order By?
- Hogyan állítható be az indexelési házirend-Order By?
- Milyen újdonságokat következő?

[Minták](#samples) és [Gyakori kérdések](#faq) is találhatók.

SQL-lekérdezés teljes hivatkozást a [lekérdezés DocumentDB oktatóprogram](documentdb-sql-query.md)című témakör tartalmaz.

## <a name="how-to-query-with-order-by"></a>Hogyan Order By lekérdezés
Például az ANSI-SQL nyelvben most felvehet az SQL-utasítások választható Order By záradék DocumentDB lekérdezése esetén. A záradék a sorrendben, amelyben eredmények lesz visszakeresve megadása nem kötelező növekvő vagy CSÖKKENŐ argumentumot tartalmazhat. 

### <a name="ordering-using-sql"></a>Rendezés SQL használatával
Ha például az alábbi történő a 10 legjobb könyvek csökkenő sorrendben, a címlista beolvasásához. 

    SELECT TOP 10 * 
    FROM Books 
    ORDER BY Books.Title DESC

### <a name="ordering-using-sql-with-filtering"></a>Rendezés, szűrés SQL használata
Minden olyan beágyazott tulajdonságával dokumentumokat, például Books.ShippingDetails.Weight belül sorrendbe is, és a további szűrők megadhatók a WHERE záradék Order By együtt, mint ebben a példában:

    SELECT * 
    FROM Books 
    WHERE Books.SalePrice > 4000
    ORDER BY Books.ShippingDetails.Weight

### <a name="ordering-using-the-linq-provider-for-net"></a>Rendezés a LINQ-szolgáltató használata a .NET rendszerhez
A .NET SDK 1.2.0 verzióját használja, és magasabb is használhatja a OrderBy() vagy OrderByDescending() záradék belül például LINQ lekérdezések ebben a példában:

    foreach (Book book in client.CreateDocumentQuery<Book>(UriFactory.CreateDocumentCollectionUri("db", "books"))
        .OrderBy(b => b.PublishTimestamp)
        .Take(100))
    {
        // Iterate through books
    }

DocumentDB támogatja a lekérdezés további típus hamarosan egyetlen numerikus, karakterláncot vagy logikai tulajdonság lekérdezése, rendezés. Kérjük, [Milyen újdonságokat következő](#Whats_coming_next) további információt talál.

## <a name="configure-an-indexing-policy-for-order-by"></a>Az indexelési házirend beállítása Order By

Visszahívása, hogy DocumentDB támogatja-e a kétféle indexek (kivonat és tartomány), amely beállítható, hogy az adott elérési út/tulajdonságainak, adattípusok (karakterláncok/számok) és a pontosság eltérő értékek (maximális pontosság vagy rögzített pontosságú értéket). Mivel DocumentDB alapértelmezett indexelés kivonat használ, létre kell hoznia egy új webhelycsoport egyéni indexelési házirendet cellatartomány számokat, karakterláncot vagy mindkettőt Order By használatához. 

>[AZURE.NOTE] String tartományát indexek vezetett be a július 7, 2015 verzió 2015-06-03 REST API-val. Az Order By házirendek létrehozásához az karakterláncok ellen, SDK változatának .NET SDK vagy verzió Python, Node.js vagy Java SDK 1.1.0 1.2.0 kell használnia.
>
>Előtt REST API 2015-06-03 verzió az alapértelmezett webhelycsoport indexelési házirend volt kivonat karakterláncokat és a számokat. Ez módosítását követően kivonat karakterláncot, és a tartomány számokhoz. 

Lásd: [DocumentDB indexelési házirendeket](documentdb-indexing-policies.md).

### <a name="indexing-for-order-by-against-all-properties"></a>Az Order By elleni minden tulajdonság indexelés
Az alábbiakban hogyan, létrehozhat egy webhelycsoport a "Minden tartomány" indexelés az Order By elleni bármely és minden numerikus vagy JSON dokumentumokból benne megjelenő tulajdonságok karakterlánc. Itt azt felülbírálása az alapértelmezett index típusa a karakterlánc értéket tartományát, és a maximális pontosság (-1).
                   
    DocumentCollection books = new DocumentCollection();
    books.Id = "books";
    books.IndexingPolicy = new IndexingPolicy(new RangeIndex(DataType.String) { Precision = -1 });
    
    await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), books);  

>[AZURE.NOTE] Figyelje meg, hogy Order By csak ad vissza az adattípusok (karakterlánc és számot) egy RangeIndex az indexelt eredményeit. Például ha az alapértelmezett indexelés házirendet, amely csak a számokat a RangeIndex mellett, egy Order By ellen megadott értékeket egy elérési utat ad vissza nem dokumentumokat.

### <a name="indexing-for-order-by-for-a-single-property"></a>A tulajdonságok egy Order By az indexelő
Itt látható, hogyan hozhat létre egy webhelycsoport ellen csak a cím tulajdonság, egy karakterlánc, amely az Order By indexelés. Vannak olyan két elérési utak listában egy, a cím tulajdonság ("vagy cím /?)" tartomány az indexelés és a másik pedig az alapértelmezett indexelési séma, amely kivonat karakterláncok és tartomány számokhoz minden tulajdonsághoz.                    
    
    booksCollection.IndexingPolicy.IncludedPaths.Add(
        new IncludedPath { 
            Path = "/Title/?", 
            Indexes = new Collection<Index> { 
                new RangeIndex(DataType.String) { Precision = -1 } } 
            });
    
    await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), booksCollection);  


## <a name="samples"></a>A minták
Nézze meg a [Github minták projektet](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples/Queries) , amely bemutatja, hogyan kell használni Order By, többek között az indexelő házirendek létrehozása és személyhívó Order By használatával. A minták Megnyitás és javasoljuk, hogy ki kérések adományok, amely a többi DocumentDB fejlesztők kihasználhatók a elküldéséhez. Olvassa el szeretné küldeni a találhat a [hozzájárulása irányelveket](https://github.com/Azure/azure-documentdb-net/blob/master/Contributing.md) .  

## <a name="faq"></a>GYAKORI KÉRDÉSEK

**Mi az a várt kérése egység (Licencelési) felhasználás Order By lekérdezések?**

Order By keresések DocumentDB indexe használja, mivel az elfogyasztott Order By lekérdezések kérelem egységek számát a egyenértékű lekérdezések nélkül Order By hasonló lesz. DocumentDB bármilyen más műveletet, például kérelem mennyiségek függ, hogy a dokumentumok méretű/alakzatokat, valamint a lekérdezés komplexitását. 


**Mi az várt indexelésének beleszámítva az Order By?**

Az indexelési tároló általános tulajdonságok száma arányos lesz. Legrosszabb eset az index terhelést lesznek 100 %-át az adatokat. Nincs átviteli (kérése egységek) terhelést tartomány/Order By indexelés, és az alapértelmezett kivonat indexelés közötti különbség nem.

**Hogyan a DocumentDB Order By használata a meglévő adatok lekérdezés?**

Annak érdekében, hogy a lekérdezés eredményének használata Order By rendezni, módosítania kell a gyűjtemény használata szemben a tulajdonság rendezéséhez használt tartomány index típus az indexelési házirend. Lásd: [az indexelő házirend módosítása](documentdb-indexing-policies.md#modifying-the-indexing-policy-of-a-collection). 

**Mik azok a Order By aktuális korlátozások?**

Order By megadható csak egy tulajdonság, vagy numerikus szemben, vagy a karakterlánc tartomány esetén indexelt maximális pontosságú (-1).

Nem tudja elvégezni a következőket:
 
- Order By belső karakterlánc tulajdonságok például azonosítóját, illetve _rid és _self (hamarosan).
- Order By tulajdonságok származik, az eredmény egy dokumentumon belüli kívülre illesztés (hamarosan).
- Rendezés több tulajdonságok (hamarosan).
- Order By lekérdezések adatbázisok, a webhelycsoportok, a felhasználók, a engedélyek vagy a mellékletek (hamarosan).
- Order By kiszámított tulajdonságait, például egy kifejezést, vagy UDF/beépített-a függvény eredménye.

Order By jelenleg nem támogatott határokon-partíciót lekérdezések lekérdezés Explorer használata esetén az Azure-portálon.

## <a name="troubleshooting"></a>Hibaelhárítás

Ha hibaüzenet jelenik meg, hogy Order By nem támogatott, ellenőrizze, hogy győződjön meg arról, hogy egy [SDK csomagjában talál](documentdb-sdk-dotnet.md) , amely támogatja az Order By verzióját használja. 

## <a name="next-steps"></a>Következő lépések

A [Github minták projekt](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples/Queries) ágaznak, és indítsa el, az adatok rendezésekor! 

## <a name="references"></a>Hivatkozások
* [A lekérdezés DocumentDB hivatkozása](documentdb-sql-query.md)
* [DocumentDB indexelés házirend-hivatkozás](documentdb-indexing-policies.md)
* [DocumentDB SQL referencia](https://msdn.microsoft.com/library/azure/dn782250.aspx)
* [A minták DocumentDB rendezés](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples/Queries)
 

