<properties
    pageTitle="A lekérdezés a Azure keresési Index |} Microsoft Azure |} A felhőben tárolt keresési szolgáltatás"
    description="Azure keresés keresési lekérdezés összeállítása és a keresési találatok szűrés és rendezés paramétereivel."
    services="search"
    manager="jhubbard"
    documentationCenter=""
    authors="ashmaka"
/>

<tags
    ms.service="search"
    ms.devlang="na"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="ashmaka"/>

# <a name="query-your-azure-search-index"></a>A lekérdezés a Azure keresési index
> [AZURE.SELECTOR]
- [– Áttekintés](search-query-overview.md)
- [Portál](search-explorer.md)
- [.NET](search-query-dotnet.md)
- [TÖBBI](search-query-rest-api.md)

Amikor keresés kérelmet Azure keresés, számos mellett a tényleges szavakat az alkalmazás a Keresés mezőbe beírt megadható paraméterek. A lekérdezés paraméterei lehetővé teszi, a teljes szöveges keresési élményt néhány mélyebb irányítását elérése.

Az alábbi felsoroljuk, amely röviden bemutatja az Azure keresési lekérdezés paraméterei közös használatára. A lekérdezés paraméterei és viselkedése teljes körét olvassa el a részletes lapok a [REST API -val](https://msdn.microsoft.com/library/azure/dn798927.aspx) és a [.NET SDK csomagjában talál](https://msdn.microsoft.com/library/azure/microsoft.azure.search.models.searchparameters_properties.aspx).

## <a name="types-of-queries"></a>Lekérdezések típusai

Azure keresési rendkívül hatékony lekérdezések létrehozásával számos lehetőséget kínál. A fő használandó lekérdezési kétféle `search` és `filter`. A `search` lekérdezés keresést végez az egy vagy több feltétel, hogy minden _kereshető_ mezőjében a index és a keresőmotor, például a Google vagy a Bing várható módja. A `filter` lekérdezés kiértékeli a logikai kifejezés folyamatosan _szűrhető_ mezők indexet. Eltérően `search` lekérdezések `filter` lekérdezések felel meg a pontos egy mezőt, ami azt jelenti, hogy nagybetűk karakterlánc mezők tartalmát.

Keresés és a szűrők együtt vagy külön-külön használhatja. Ha együtt használva, a szűrő először a teljes indexben, és kattintson a Keresés a szűrés eredményét történik. Szűrők ezért lehet hasznos technika lekérdezési teljesítmény javítása érdekében, mivel azok csökkentheti a keresési lekérdezést kell feldolgozása, hogy a dokumentumokat a csoportját.

A szűrőkifejezések szintaxisa [OData szűrő nyelvi](https://msdn.microsoft.com/library/azure/dn798921.aspx)egy részét. A keresési lekérdezések az [egyszerűsített szintaxis](https://msdn.microsoft.com/library/azure/dn798920.aspx) és a alatti tárgyalt [Lucene lekérdezés szintaxisa](https://msdn.microsoft.com/library/azure/mt589323.aspx) is használhatja.

### <a name="simple-query-syntax"></a>Egyszerű lekérdezés szintaxisa
Az [Egyszerű lekérdezés szintaxisa](https://msdn.microsoft.com/library/azure/dn798920.aspx) Azure keresés használt alapértelmezett lekérdezési nyelv. Az egyszerű lekérdezés-szintaxis támogatja-e, többek között az és, közös keresési operátorok, kifejezést, utótag és operátorok végrehajtási sorrendje számos.

### <a name="lucene-query-syntax"></a>Lucene lekérdezés szintaxisa
A [lekérdezés-szintaxis Lucene](https://msdn.microsoft.com/library/azure/mt589323.aspx) [Apache Lucene](https://lucene.apache.org/core/4_10_2/queryparser/org/apache/lucene/queryparser/classic/package-summary.html)részeként fejlesztett körben elfogadott és kifejező lekérdezésnyelv használatát teszi lehetővé.

A lekérdezés szintaxisát használatával lehetővé teszi, hogy könnyen elérheti az alábbi funkciókat: [lekérdezések mező – munkafüzetszintű](https://msdn.microsoft.com/library/azure/mt589323.aspx#bkmk_fields), [zavaros keresési](https://msdn.microsoft.com/library/azure/mt589323.aspx#bkmk_fuzzy), [közelség keresési](https://msdn.microsoft.com/library/azure/mt589323.aspx#bkmk_proximity), [kifejezés szolgáló lehetőségekről](https://msdn.microsoft.com/library/azure/mt589323.aspx#bkmk_termboost), [reguláris kifejezésekkel keresési](https://msdn.microsoft.com/library/azure/mt589323.aspx#bkmk_regex), [helyettesítő keresési](https://msdn.microsoft.com/library/azure/mt589323.aspx#bkmk_wildcard), [szintaxis kapcsolatos alapismeretekről](https://msdn.microsoft.com/library/azure/mt589323.aspx#bkmk_syntax)és [lekérdezések logikai operátorokat használó](https://msdn.microsoft.com/library/azure/mt589323.aspx#bkmk_boolean).



## <a name="ordering-results"></a>Rendezés eredménye
A keresési lekérdezésének eredménye fogadásakor kérheti, hogy az Azure keresési szolgál az eredményeket egy adott mező értékeit szerint rendezett. Alapértelmezés szerint az Azure keresési rendelések a keresési eredmények rangját [TF-IDF](https://en.wikipedia.org/wiki/Tf%E2%80%93idf)származó minden dokumentum keresési pontszám alapján.

Ha azt szeretné, hogy Azure találatok kívül a keresési eredmény értéke szerint rendezett az eredmények is használhatja a `orderby` keresési feltétel. Megadhatja, hogy az az érték a `orderby` paramétere tartalmazza a mezőneveket és a hívások átirányítása a [ `geo.distance()` függvény](https://msdn.microsoft.com/library/azure/dn798921.aspx) térinformatikai az értékek. Minden egyes kifejezés követhetnek `asc` jelzi, hogy eredmények igényelt növekvő sorrendben, és `desc` jelzi, hogy eredmények igényelt csökkenő sorrendben. Az alapértelmezett rangsorolási növekvő sorrendben.

## <a name="paging"></a>Lapozás
Azure keresési egyszerűen a keresési eredmények lapozási végrehajtásához. Használatával a `top` és `skip` paraméterek, amelyek lehetővé teszik a keresési eredmények teljes készlete fogadása kezelhető, rendezett részhalmazának jó keresés felhasználói felület tanácsok egyszerűen teszik a keresési kérelem egyenletesen hiba. Ezek a találatok kisebb részhalmazának fogadásakor is kaphatnak a dokumentumok száma a keresési eredmények teljes megadása.

Ismerje meg a Keresés eredménye a következő cikkben: [hogyan lapra az Azure keresési találatok](search-pagination-page-layout.md)személyhívó.


## <a name="hit-highlighting"></a>Kattintson a kiemelés
Azure keresés, a keresési eredmények, amelyek megegyeznek a keresési lekérdezés pontos részét hangsúlyozása lett egyszerűen használatával a `highlight`, `highlightPreTag`, és `highlightPostTag` paramétereket. _Kereshető_ mezőket adhatja meg kell rendelkeznie a megfeleltetett szöveg kiemelten megjelenítő és a pontos karakterlánc címkék hozzáfűzni elején és végén, a megfeleltetett szöveg, hogy Azure a Keresés eredménye megadása.
