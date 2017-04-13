<properties
    pageTitle="A lista, a Microsoft Azure tároló ügyfél tárral Azure tárolási erőforrásokat C++ |} Microsoft Azure"
    description="További tudnivalók a listaelem API-k használata a Microsoft Azure tároló ügyfél tárban C++ számba veheti a tárolók, BLOB, sorok, táblázatok és szervezetek."
    documentationCenter=".net"
    services="storage"
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

# <a name="list-azure-storage-resources-in-c"></a>Lista Azure tároló C++-erőforrások

Listaelem-műveletek kulcsfontosságúak sok fejlesztési esetek Azure adathordozós. Ez a cikk ismerteti a leghatékonyabban számba veheti a nevére a partnerlistában a Microsoft Azure tároló ügyfél tárban előírt C++ API-k használata Azure-tárolóban lévő objektumokhoz.

>[AZURE.NOTE] Ez az útmutató célként az Azure tároló ügyfél tárat C++ verzió 2.x [NuGet](http://www.nuget.org/packages/wastorage) vagy [GitHub](https://github.com/Azure/azure-storage-cpp)keresztül érhető el.

A tárhely ügyfél tár Itt számos módszerrel lista vagy a lekérdezés Azure-tárolóban lévő objektumokhoz. Ez a cikk szünteti meg az alábbi esetekben:

-   A fiók lista tárolók
-   Lista BLOB-tárolóhoz vagy virtuális blob-címtár
-   A fiók lista sorok
-   A fiók lista táblák
-   Lekérdezés személyek egy táblázatban

A fenti módszerek különböző túlterhelések használata különböző forgatókönyvek látható.

## <a name="asynchronous-versus-synchronous"></a>Aszinkron vagy szinkron

Az ügyfél tároló tárat C++ épül a [többi C++ tár](https://github.com/Microsoft/cpprestsdk)fölött, mert eleve támogatjuk aszinkron műveletek [pplx::task](http://microsoft.github.io/cpprestsdk/classpplx_1_1task.html)használatával. Példa:

    pplx::task<list_blob_item_segment> list_blobs_segmented_async(continuation_token& token) const;

Szinkronizált műveletek tördelése a megfelelő aszinkron műveletek:

    list_blob_item_segment list_blobs_segmented(const continuation_token& token) const
    {
        return list_blobs_segmented_async(token).get();
    }

A több szálon futó műveletvégzés alkalmazások és szolgáltatások dolgozik, akkor azt javasoljuk, hogy a aszinkron API-khoz közvetlenül helyett használhatja a szál létrehozása hívja fel a szinkronizálás API-hoz, amelyek jelentősen hatással van a teljesítmény.

## <a name="segmented-listing"></a>Szegmentált nevére a partnerlistában

Felhőbeli tárhelyről beosztásának szükséges szegmentált nevére a partnerlistában. Lehet például egy millió BLOB-Azure blob-tárolóban vagy egy milliárd szervezetek Azure-táblából az keresztül. Ezek nem elméleti számok, de valós ügyfél használatát esetek.

Éppen ezért praktikus egyetlen válasz összes objektumának megjelenítéséhez. Ehelyett lapozási használó objektumok jeleníthet meg. A listaelem API-khoz mindegyikének egy *szegmentált* túlterhelés.

A válasz szegmentált nevére a partnerlistában művelet tartalmazza:

-   <i>_segment</i>, mely a nevére a partnerlistában API-híváshoz egyetlen találat a készlete tartalmazza.
-   *continuation_token*, amely a következő hívás annak érdekében, hogy az eredmények a következő oldal első átadott. Ha nincs több eredmény, a folytatását jelző token értéke null.

Ha például tipikus hívást kezdeményez, a lista összes BLOB-tárolóban nézhet ki a következő kódrészletet. A kódot a [minták](https://github.com/Azure/azure-storage-cpp/blob/master/Microsoft.WindowsAzure.Storage/samples/BlobsGettingStarted/Application.cpp)érhető el:

    // List blobs in the blob container
    azure::storage::continuation_token token;
    do
    {
        azure::storage::list_blob_item_segment segment = container.list_blobs_segmented(token);
        for (auto it = segment.results().cbegin(); it != segment.results().cend(); ++it)
    {
        if (it->is_blob())
        {
            process_blob(it->as_blob());
        }
        else
        {
            process_diretory(it->as_directory());
        }
    }

        token = segment.continuation_token();
    }
    while (!token.empty());

Tartsa szem előtt, hogy az weblapon visszaadott találatok száma szabályozhatja az egyes API, a túlterhelés paraméter *max_results* például:

    list_blob_item_segment list_blobs_segmented(const utility::string_t& prefix, bool use_flat_blob_listing,
        blob_listing_details::values includes, int max_results, const continuation_token& token,
        const blob_request_options& options, operation_context context)

Ha nem adja meg a *max_results* paraméter, az alapértelmezett találatlista 5000 DB találatok maximális értéke jelez az egy oldalra.

Tartsa szem előtt, hogy egy Azure táblatárolóhoz lekérdezése esetleg nem, vagy kevesebb rekordok megadott, *max_results* paraméter értéknél még akkor is, ha a folytatását jelző token nem üres. Több oka lehet, hogy a lekérdezés nem hajtható végre öt másodpercben. Mindaddig, amíg a folytatását jelző token nem üres, továbbra is a lekérdezést, és a kód nem kell feltételezik szakasz eredményeinek a méretét.

Találatokkal ajánlott coding mintájának szegmentált van, a találatok között, amely nyújt explicit végrehajtásának nevére a partnerlistában vagy lekérdezése, valamint a szolgáltatás minden kérés válaszát. Különösen C++ alkalmazások és szolgáltatások, a nevére a partnerlistában végrehajtását alsóbb szintű irányításának segíthet a vezérlő memória és a teljesítmény.

## <a name="greedy-listing"></a>Greedy nevére a partnerlistában

Az ügyfél tároló tárat C++ korábbi verzióiban (előzetes verzió 0.5.0 és korábbi) nem szegmentált nevére a partnerlistában API-khoz táblák és a sorok, ahogy a következő példában szereplő:

    std::vector<cloud_table> list_tables(const utility::string_t& prefix) const;
    std::vector<table_entity> execute_query(const table_query& query) const;
    std::vector<cloud_queue> list_queues() const;

Ezeket a módszereket csomagolást szegmentált API-t, amelyeket végrehajtani. Az egyes válasza szegmentált nevére a partnerlistában a kód hozzáfűzi az eredményeket egy vektoros, és az összes eredményt adja vissza, a teljes tárolók nélkül beolvasott után.

Ezt a módszert akkor működnek, a tárterület-fiók vagy a táblázatot tartalmazó objektumok kisszámú. Azonban az objektumok számát növekedését szükséges memóriát növelheti korlátozás nélkül, mivel minden eredmény maradt a memóriában. Egy listaelem művelet sokáig, amelynek során a hívó volt a meghívási folyamat kapcsolatos információ nem készíthet.

Ezek a C#, Java vagy a JavaScript Node.js környezet nem léteznek greedy nevére a partnerlistában a SDK API-khoz. Az alábbi greedy API-k használata a lehetséges problémák elkerülése érdekében azt eltávolítja őket verziójában 0.6.0 előzetes.

Ha a kód hív ezek greedy API-hoz:

    std::vector<azure::storage::table_entity> entities = table.execute_query(query);
    for (auto it = entities.cbegin(); it != entities.cend(); ++it)
    {
        process_entity(*it);
    }

Ezután módosítania kell a kódot, és használja a szegmentált listaelem API-hoz:

    azure::storage::continuation_token token;
    do
    {
        azure::storage::table_query_segment segment = table.execute_query_segmented(query, token);
        for (auto it = segment.results().cbegin(); it != segment.results().cend(); ++it)
        {
            process_entity(*it);
        }

        token = segment.continuation_token();
    } while (!token.empty());

A szakasz *max_results* paramétere megadásával is egyenleg kérelem a számok és a memóriahasználat, hogy megfeleljenek az alkalmazás a teljesítménnyel kapcsolatos szempontok között.

Ezenkívül szegmentált nevére a partnerlistában API-khoz használ, de tárolja az adatokat egy helyi webhelycsoport stílussal "szőke szót tartalmazzák", ha is javasoljuk, hogy a kezelheti az adatokat tároljon egy helyi gyűjteményében körültekintően a méretezés a kód refactor.

## <a name="lazy-listing"></a>Lusta nevére a partnerlistában

Bár greedy nevére a partnerlistában a potenciális problémákra emelni, célszerű a Ha nincs nem túl sok objektumok a tároló.

Ha a C# vagy Oracle Java SDK is használ, és a megszámlálható programozási modellt, amely egy Lusta stílusú bejegyzésére, ahol az adatokat egy bizonyos eltolás csak olvas be szükség esetén tisztában kell lennie. A C++ a léptető-alapú sablon is biztosít egy hasonló módon.

Egy tipikus Lusta nevére a partnerlistában API, használja a **list_blobs** szerepel példaként, így néz ki:

    list_blob_item_iterator list_blobs() const;

Egy tipikus kódtöredék a Lusta nevére a partnerlistában minta használó előfordulhat, hogy néz ki:

    // List blobs in the blob container
    azure::storage::list_blob_item_iterator end_of_results;
    for (auto it = container.list_blobs(); it != end_of_results; ++it)
    {
        if (it->is_blob())
        {
            process_blob(it->as_blob());
        }
        else
        {
            process_directory(it->as_directory());
        }
    }

Ne feledje, hogy Lusta nevére a partnerlistában csak szinkronizált módban érhető el.

Összehasonlítva greedy nevére a partnerlistában, Lusta nevére a partnerlistában beolvassa az adatok csak akkor, ha szükséges. A magában foglalja a azt beolvassa adatok Azure-tárhelyről csak akkor, amikor a következő léptető áthelyezi a következő oldalt. Ezért memóriahasználat körülhatárolható méretű szabályozható, és a művelet gyors.

Lusta nevére a partnerlistában API-khoz szerepelnek a tárterület-ügyfél tár for C++ 2.2.0 verziójában.

## <a name="conclusion"></a>Elfogadásáról

Ez a cikk azoknak API-hoz a tárterület-ügyfél tárban különféle objektumok C++ a különböző túlterhelések azt tárgyalja. Összefoglalva:

-   Aszinkron API-k csoportban a több szálon futó műveletvégzés esetek erősen ajánlott.
-   Szegmentált nevére a partnerlistában találatokkal ajánlott.
-   Lusta nevére a partnerlistában a tár szinkronizált esetben egy kényelmes csomagolópapír adni.
-   Nem ajánlott greedy nevére a partnerlistában, és el lett távolítva a tárból.

##<a name="next-steps"></a>Következő lépések

C++ Azure-tárhely és a ügyfél tár kapcsolatos további információt az alábbi forrásokban talál.

-   [Használatáról a C++ Blob-tárolóhoz](storage-c-plus-plus-how-to-use-blobs.md)
-   [A C++ Táblatároló használata](storage-c-plus-plus-how-to-use-tables.md)
-   [A C++ várólista-tároló használata](storage-c-plus-plus-how-to-use-queues.md)
-   [Azure tároló ügyfél tár C++ API dokumentációjának esetén.](http://azure.github.io/azure-storage-cpp/)
-   [Azure tároló csapatának blogja](http://blogs.msdn.com/b/windowsazurestorage/)
-   [Azure tároló dokumentáció](https://azure.microsoft.com/documentation/services/storage/)
