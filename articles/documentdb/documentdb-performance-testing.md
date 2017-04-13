<properties 
    pageTitle="DocumentDB méretezése és a teljesítmény tesztelése |} Microsoft Azure" 
    description="Megtudhatja, hogy miként skála és a teljesítmény az Azure DocumentDB tesztelés végrehajtása"
    keywords="a teljesítmény tesztelése"
    services="documentdb" 
    authors="arramac" 
    manager="jhubbard" 
    editor="" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/27/2016" 
    ms.author="arramac"/>

# <a name="performance-and-scale-testing-with-azure-documentdb"></a>A teljesítmény és a méretezés az Azure DocumentDB tesztelése
A teljesítmény és a méretezés tesztelése Ez alkalmazások fejlesztése fő lépés. Számos alkalmazáshoz a adatbázis réteg általános a teljesítmény és méretezhetőség a jelentős hatással van, és így a teljesítmény tesztelése kritikus összetevője. [Azure DocumentDB](https://azure.microsoft.com/services/documentdb/) purpose-built skála rugalmas és kiszámítható teljesítményt, és ezért egy nagy illesztését alkalmazások, a nagy teljesítményű adatbázis réteg van szükség. 

Ez a témakör az teljesítmény próba csomagok esetében a DocumentDB munkaterhelésekből végrehajtása, illetve DocumentDB kiértékelése nagy teljesítményű alkalmazás felhasználási területei fejlesztők számára hivatkozást. Elsősorban koncentrál elszigetelt teljesítmény tesztelése az adatbázisról, de is tartalmazza a gyártási alkalmazások gyakorlati tanácsok.

Ez a cikk elolvasása, után fogja tudni az alábbi kérdésekre választ:   

- Hol találom meg a minta .NET ügyfélalkalmazás Azure DocumentDB teljesítmény teszteléshez? 
- Hogyan elérése az Azure DocumentDB magas átviteli szintek az ügyfél alkalmazásból?

Az első lépések a kódot, töltse le a projekt mintából [DocumentDB teljesítmény tesztelése](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark). 

> [AZURE.NOTE] Ennek az alkalmazásnak a cél, bemutatják a gyakorlati tanácsok a DocumentDB kívül jobb teljesítményt kiolvasó amelyben kisszámú az ügyfélszámítógépek. Ez nem történt bemutatják a szolgáltatást, amely limitlessly méretezheti csúcs kapacitása.

Ha a keresett ügyféloldali konfigurációs beállítások DocumentDB a teljesítmény javítása érdekében, olvassa el a [DocumentDB látható tippeket](documentdb-performance-tips.md).

## <a name="run-the-performance-testing-application"></a>Futtassa a Teljesítmény alkalmazás tesztelése
Első lépések a leggyorsabban összeállítása és a .NET-az alábbi példa futtassa az alábbi lépésekkel leírtak szerint is. Tekintse át a kódot is, és hasonló beállítások alkalmazása a saját ügyfélalkalmazásokban elemre.

**Lépés: 1:** Töltse le a projekt [DocumentDB teljesítmény tesztelése minta](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark), vagy a Github tárházba ágaznak.

**Lépés: 2:** Módosítsa a beállításokat az EndpointUrl, AuthorizationKey, CollectionThroughput és DocumentTemplate App.config (nem kötelező).

> [AZURE.NOTE] Előtt a magas átviteli gyűjtemények kiépítési, olvassa el a [Lap árak](https://azure.microsoft.com/pricing/details/documentdb/) becsüljük meg a webhelycsoport használati költségek. DocumentDB számlák tárolási és átviteli független alapon óránkénti, így mentheti költségek törlésével vagy csökkentse a kapacitásának a DocumentDB gyűjtemények ellenőrzése után.

**3 lépés:** Állíthat össze, és a konzol alkalmazás futtatásához a parancssorból. Meg kell jelennie kimeneti, például a következőket:

    Summary:
    ---------------------------------------------------------------------
    Endpoint: https://docdb-scale-demo.documents.azure.com:443/
    Collection : db.testdata at 50000 request units per second
    Document Template*: Player.json
    Degree of parallelism*: 500
    ---------------------------------------------------------------------

    DocumentDBBenchmark starting...
    Creating database db
    Creating collection testdata
    Creating metric collection metrics
    Retrying after sleeping for 00:03:34.1720000
    Starting Inserts with 500 tasks
    Inserted 661 docs @ 656 writes/s, 6860 RU/s (18B max monthly 1KB reads)
    Inserted 6505 docs @ 2668 writes/s, 27962 RU/s (72B max monthly 1KB reads)
    Inserted 11756 docs @ 3240 writes/s, 33957 RU/s (88B max monthly 1KB reads)
    Inserted 17076 docs @ 3590 writes/s, 37627 RU/s (98B max monthly 1KB reads)
    Inserted 22106 docs @ 3748 writes/s, 39281 RU/s (102B max monthly 1KB reads)
    Inserted 28430 docs @ 3902 writes/s, 40897 RU/s (106B max monthly 1KB reads)
    Inserted 33492 docs @ 3928 writes/s, 41168 RU/s (107B max monthly 1KB reads)
    Inserted 38392 docs @ 3963 writes/s, 41528 RU/s (108B max monthly 1KB reads)
    Inserted 43371 docs @ 4012 writes/s, 42051 RU/s (109B max monthly 1KB reads)
    Inserted 48477 docs @ 4035 writes/s, 42282 RU/s (110B max monthly 1KB reads)
    Inserted 53845 docs @ 4088 writes/s, 42845 RU/s (111B max monthly 1KB reads)
    Inserted 59267 docs @ 4138 writes/s, 43364 RU/s (112B max monthly 1KB reads)
    Inserted 64703 docs @ 4197 writes/s, 43981 RU/s (114B max monthly 1KB reads)
    Inserted 70428 docs @ 4216 writes/s, 44181 RU/s (115B max monthly 1KB reads)
    Inserted 75868 docs @ 4247 writes/s, 44505 RU/s (115B max monthly 1KB reads)
    Inserted 81571 docs @ 4280 writes/s, 44852 RU/s (116B max monthly 1KB reads)
    Inserted 86271 docs @ 4273 writes/s, 44783 RU/s (116B max monthly 1KB reads)
    Inserted 91993 docs @ 4299 writes/s, 45056 RU/s (117B max monthly 1KB reads)
    Inserted 97469 docs @ 4292 writes/s, 44984 RU/s (117B max monthly 1KB reads)
    Inserted 99736 docs @ 4192 writes/s, 43930 RU/s (114B max monthly 1KB reads)
    Inserted 99997 docs @ 4013 writes/s, 42051 RU/s (109B max monthly 1KB reads)
    Inserted 100000 docs @ 3846 writes/s, 40304 RU/s (104B max monthly 1KB reads)

    Summary:
    ---------------------------------------------------------------------
    Inserted 100000 docs @ 3834 writes/s, 40180 RU/s (104B max monthly 1KB reads)
    ---------------------------------------------------------------------
    DocumentDBBenchmark completed successfully.


**Lépés: 4 (ha szükséges):** A eszközről (Licencelési/s) jelzett átviteli meg kell egyezniük, vagy nagyobb, mint a gyűjtemény a kiépített kapacitása. Ha nem, a kis lépésekben DegreeOfParallelism növelésével segíthet eléri. Ha az ügyfél-alkalmazás az átviteli trületek, több példányon az egyforma vagy különböző számítógépeken az alkalmazás indítása segítséget végig a különböző példányok kiépített eléri. Ha ezt a lépést segítségre van szüksége, kérjük, írja meg az e-mailben askdocdb@microsoft.com vagy annak kitöltéséhez támogatási jegy.

Ha befejezte a alkalmazást futtató, megpróbálhatja különböző [indexelés házirendek](documentdb-indexing-policies.md) és [egységesebb szintek](documentdb-consistency-levels.md) hatásuk teljesítmény és a késés megértéséhez. Tekintse át a forráskódot is, és a saját próba-csomagok vagy -alkalmazások gyártási hasonló konfigurációk végrehajtása.

## <a name="next-steps"></a>Következő lépések
Ez a cikk azt tekintette meg, hogyan végezheti el a teljesítmény és tesztelése a .NET konzol alkalmazással DocumentDB skála. Olvassa el a hivatkozásokra kattintva elmerülhet a DocumentDB további tudnivalókat.

* [DocumentDB teljesítmény tesztelés minta](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark)
* [Ügyfél konfigurációs beállításai DocumentDB a teljesítmény javítása érdekében](documentdb-performance-tips.md)
* [Kiszolgálóoldali DocumentDB a szétválasztás](documentdb-partition-data.md)
* [DocumentDB webhelycsoportjában és a teljesítmény szintek](documentdb-performance-levels.md)
* [DocumentDB .NET SDK dokumentáció az MSDN webhelyen](https://msdn.microsoft.com/library/azure/dn948556.aspx)
* [DocumentDB .NET minták](https://github.com/Azure/azure-documentdb-net)
* [Tippek a teljesítmény DocumentDB blog](https://azure.microsoft.com/blog/2015/01/20/performance-tips-for-azure-documentdb-part-1-2/)
