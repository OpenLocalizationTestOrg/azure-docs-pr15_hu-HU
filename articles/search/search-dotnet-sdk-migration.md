<properties
   pageTitle="Az Azure keresési .NET SDK 1.1-es verzió való frissítés |} Microsoft Azure |} A felhőben tárolt keresési szolgáltatás"
   description="Az Azure keresési .NET SDK 1.1-es verzió frissítése"
   services="search"
   documentationCenter=""
   authors="brjohnstmsft"
   manager="pablocas"
   editor=""/>

<tags
   ms.service="search"
   ms.devlang="dotnet"
   ms.workload="search"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.date="08/15/2016"
   ms.author="brjohnst"/>

# <a name="upgrading-to-the-azure-search-net-sdk-version-20-preview"></a>Az Azure keresési .NET SDK verzió 2.0-előzetes verzióra történő frissítés

Ha 1.1-es vagy az [Azure keresési .NET SDK](https://msdn.microsoft.com/library/azure/dn951165.aspx)régebbi verzióját használja, ez a cikk segítséget nyújt az alkalmazás használatához a következő főverzió, 2.0-Előnézet frissítése.

További általános ismertetését megtalálja a SDK és példák megtudhatja, [hogy miként Azure keresés .NET-alkalmazásból](search-howto-dotnet-sdk.md).

Az Azure keresési .NET SDK verzió 2.0-előnézetét a korábbi verzióinak módosításokat tartalmazza. Ezek a főként kisebb, így a kód megváltoztatása érdekében szükséges minimális munkamennyiség csak. [Lépéseket követve frissítse](#UpgradeSteps) talál útmutatást a kód megváltoztatása az új SDK verzióját használja.

> [AZURE.NOTE] Ha 0,13 – előnézet vagy régebbi verzióját használja, frissítenie kell az első, és kattintson a frissítés 2.0-előzetes 1.1-es verzió. Lásd: [függelék: 1.1-es verziójára való lépéseket](#UpgradeStepsV1) utasításokat.

<a name="WhatsNew"></a>
## <a name="whats-new-in-version-20-preview"></a>2.0 – előzetes verzió újdonságai

2.0 – előzetes verzió az első verziója az Azure keresési .NET SDK célba az Azure keresési REST API kifejezetten 2015-02-28 – előzetes verzió előzetes verziójára. Ez lehetővé teszi a .NET-alkalmazások, többek között az alábbi Azure keresési sok előzetes verzió funkcióinak használata:

- [Egyéni gázelemzők](https://aka.ms/customanalyzers)
- [Azure Blob-tárolóhoz](search-howto-indexing-azure-blob-storage.md) és [Azure Táblatárolóhoz](search-howto-indexing-azure-tables.md) indexelő támogatás
- Indexelő testreszabási [mező megfeleltetésének](search-indexer-field-mappings.md) keresztül
- ETags ahhoz, hogy megbízható egyidejű frissítése index definíciókat, indexelő és adatforrások támogatása
- .NET Core és a .NET hordozható profil 111 támogatása

<a name="UpgradeSteps"></a>
## <a name="steps-to-upgrade"></a>Frissítés lépései

Először frissíteni az a NuGet ismertetője `Microsoft.Azure.Search` vagy a konzolon NuGet csomag Manager vagy szerint kattintson a jobb gombbal a a project hivatkozás, és válassza a "Kezelése NuGet csomagok..." a Visual Studióban. Ellenőrizze, hogy az előzetes verziójú csomagok "Olyan előzetes" a "Kezelése csomagok" NuGet kiválasztásával engedélyezi a Visual Studióban, illetve használatával ablakban a `-IncludePrerelease` váltás NuGet csomag kezelője konzol használata.

Miután NuGet letölti az új csomagok és a függőségek, újbóli létrehozása a projekt. Attól függően, hogy a kód strukturált akkor előfordulhat, hogy újraépítéséhez sikeresen. Ha igen, akkor készen is!

Ha nem sikerül a fejlesztése, meg kell jelennie egy összeállítás hiba, az alábbihoz hasonló:

    Program.cs(31,45,31,86): error CS0266: Cannot implicitly convert type 'Microsoft.Azure.Search.ISearchIndexClient' to 'Microsoft.Azure.Search.SearchIndexClient'. An explicit conversion exists (are you missing a cast?)

A következő lépésként build hiba kijavításához. Lásd: a [változások 2.0 – előzetes verzió megszakítása](#ListOfChanges) megtudhatja, mi okozza a hibát, és a javítás módja.

Feleslegessé vált módszerekről és tulajdonságok kapcsolatban további build figyelmeztetések jelenhetnek meg. A figyelmeztetések tartalmazni fogja a Mi az elavult funkció helyett használja a megjelenő utasításokat. Ha például az alkalmazás a `SearchRequestOptions.RequestId` tulajdonság, akkor érdemes figyelmeztetés jelenik meg, amely szerint`"This property is deprecated. Please use ClientRequestId instead."`

Miután kijavította a build hibák, módosíthatja az új funkciók előnyeinek kihasználása, ha azt szeretné, hogy az alkalmazás. Új funkciók a SDK részletesen [a 2.0-s – előzetes verzió újdonságai](#WhatsNew).

<a name="ListOfChanges"></a>
## <a name="breaking-changes-in-version-20-preview"></a>Módosítások megszakítása verziójában 2.0 – előzetes verzió

Csak egy szakítószilárdságának módosítása verziója 2.0-előzetes verzióban, amely mellett az alkalmazás Újraépítés kód módosításai megkövetelheti van.

### <a name="indexesgetclient-return-type"></a>Indexes.GetClient visszatérési típus

A `Indexes.GetClient` mód van új visszatérési típus. Korábban visszaadott `SearchIndexClient`, ez módosítását követően, de `ISearchIndexClient` verzió 2.0-előzetes verzióban. A támogatási szeretné mock: az a `GetClient` egység vizsgálatok mock végrehajtásának visszaadó módszer `ISearchIndexClient`.

#### <a name="example"></a>Példa

Ha a kód néz ki:

```csharp
SearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");
```

Módosíthatja a bármely build hibáinak elhárításához:

```csharp
ISearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");
```

## <a name="conclusion"></a>Elfogadásáról
Ha módosítania kell a Azure keresési .NET SDK használatáról, olvassa el a a nemrég frissített [ennek](search-howto-dotnet-sdk.md).

Üdvözöljük a visszajelzését a SDK. Ha problémákat tapasztal, nyugodtan segítségkérés nekünk az [Azure keresési MSDN-fórum](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=azuresearch). Ha a hibát talál, a probléma is fájl a az [Azure .NET SDK GitHub tárházba](https://github.com/Azure/azure-sdk-for-net/issues). Ügyeljen arra, hogy a probléma a lapcím előtag "Keresés SDK:".

Köszönjük, hogy az Azure keresőfunkcióval!

<a name="UpgradeStepsV1"></a>
## <a name="appendix-steps-to-upgrade-to-version-11"></a>. Melléklet: 1.1-es verziójára való lépések 

> [AZURE.NOTE] Ez a szakasz csak a felhasználók az Azure keresési .NET SDK verzió 0,13 – előzetes verzió és a régebbi vonatkozik.

Először frissíteni az a NuGet ismertetője `Microsoft.Azure.Search` vagy a konzolon NuGet csomag Manager vagy szerint kattintson a jobb gombbal a a project hivatkozás, és válassza a "Kezelése NuGet csomagok..." a Visual Studióban.

Miután NuGet letölti az új csomagok és a függőségek, újbóli létrehozása a projekt.

Ha a korábban használt verzióját 1.0.0-preview, 1.0.1-preview vagy 1.0.2-preview, összeállítása kell sikeres, és készen a küldésre!

Ha a korábban használt verzióját 0.13.0-preview vagy régebbi, meg kell jelennie összeállítása hibákat, például a következőket:

    Program.cs(137,56,137,62): error CS0117: 'Microsoft.Azure.Search.Models.IndexBatch' does not contain a definition for 'Create'
    Program.cs(137,99,137,105): error CS0117: 'Microsoft.Azure.Search.Models.IndexAction' does not contain a definition for 'Create'
    Program.cs(146,41,146,54): error CS1061: 'Microsoft.Azure.Search.IndexBatchException' does not contain a definition for 'IndexResponse' and no extension method 'IndexResponse' accepting a first argument of type 'Microsoft.Azure.Search.IndexBatchException' could be found (are you missing a using directive or an assembly reference?)
    Program.cs(163,13,163,42): error CS0246: The type or namespace name 'DocumentSearchResponse' could not be found (are you missing a using directive or an assembly reference?)

A következő lépésként az összeállítás egyesével hibáinak elhárításához. A legtöbb esetén kell néhány átnevezett a SDK osztály és módszer nevek módosítása. [Módosítások 1.1-es verzió megszakítása listája](#ListOfChangesV1) nevét a módosítások listáját tartalmazza.

Ha, hogy használja egyéni osztályok modell dokumentumait, és ezeket az osztályok van nem nullable egyszerű típusokhoz tulajdonságai (például `int` vagy `bool` C#), a hibajavítás a SDK csomagjában talál, amelyek tudnia kell 1.1-es verziójában van. További információt talál [az 1.1-es verzió hibajavítás](#BugFixesV1) .

Végül Miután kijavította a build hibák, módosíthatja az új funkciók előnyeinek kihasználása, ha azt szeretné, hogy az alkalmazás.

<a name="ListOfChangesV1"></a>
### <a name="list-of-breaking-changes-in-version-11"></a>Módosítások megszakítása 1.1-es verzió listája

Az alábbi lista megrendelte annak valószínűsége, hogy a változtatások hatással van az alkalmazás kódját.

#### <a name="indexbatch-and-indexaction-changes"></a>IndexBatch és a IndexAction

`IndexBatch.Create`a átnevezése `IndexBatch.New` és a továbbiakban nem rendelkezik egy `params` argumentum. Használható `IndexBatch.New` tételek, amely keverjen ki különféle műveletek (összevonása, törlése, stb.). Ezeken kívül vannak kötegekben hozhat létre új statikus módszerek amelyben összes művelet megegyeznek: `Delete`, `Merge`, `MergeOrUpload`, és `Upload`.

`IndexAction`Nyilvános konstruktorok a továbbiakban nem rendelkezik, és annak tulajdonságait most megváltoztatható. Műveletek létrehozása különböző célokra kell használni az új statikus módszerek: `Delete`, `Merge`, `MergeOrUpload`, és `Upload`. `IndexAction.Create`megszűnt. Ha korábban a túlterhelés, amely csak egy dokumentumot, ellenőrizze, hogy használni `Upload` helyette.

##### <a name="example"></a>Példa

Ha a kód néz ki:

    var batch = IndexBatch.Create(documents.Select(doc => IndexAction.Create(doc)));
    indexClient.Documents.Index(batch);

Módosíthatja a bármely build hibáinak elhárításához:

    var batch = IndexBatch.New(documents.Select(doc => IndexAction.Upload(doc)));
    indexClient.Documents.Index(batch);

Ha azt szeretné, további egyszerűsíthető, erre:

    var batch = IndexBatch.Upload(documents);
    indexClient.Documents.Index(batch);

#### <a name="indexbatchexception-changes"></a>IndexBatchException módosítása

A `IndexBatchException.IndexResponse` tulajdonság átnevezett `IndexingResults`, és típusának megfelelő most már `IList<IndexingResult>`.

##### <a name="example"></a>Példa

Ha a kód néz ki:

    catch (IndexBatchException e)
    {
        Console.WriteLine(
            "Failed to index some of the documents: {0}",
            String.Join(", ", e.IndexResponse.Results.Where(r => !r.Succeeded).Select(r => r.Key)));
    }

Módosíthatja a bármely build hibáinak elhárításához:

    catch (IndexBatchException e)
    {
        Console.WriteLine(
            "Failed to index some of the documents: {0}",
            String.Join(", ", e.IndexingResults.Where(r => !r.Succeeded).Select(r => r.Key)));
    }

<a name="OperationMethodChanges"></a>
#### <a name="operation-method-changes"></a>A művelet módszer módosítása

Minden egyes művelet az Azure keresési .NET SDK módszer túlterhelések halmazának szinkron és aszinkron hívók van közzétéve. Az aláírások és az alábbi módszerrel túlterhelések faktoring 1.1-es verzió megváltozott.

Ha például az "Első Index statisztika" művelet a SDK régebbi verzióiban elérhetővé tett ezeket az aláírások:

In `IIndexOperations`:

    // Asynchronous operation with all parameters
    Task<IndexGetStatisticsResponse> GetStatisticsAsync(
        string indexName,
        CancellationToken cancellationToken);

In `IndexOperationsExtensions`:

    // Asynchronous operation with only required parameters
    public static Task<IndexGetStatisticsResponse> GetStatisticsAsync(
        this IIndexOperations operations,
        string indexName);

    // Synchronous operation with only required parameters
    public static IndexGetStatisticsResponse GetStatistics(
        this IIndexOperations operations,
        string indexName);

A módszer aláírások 1.1-es verzió ugyanahhoz a művelethez a következőhöz hasonlóan:

In `IIndexesOperations`:

    // Asynchronous operation with lower-level HTTP features exposed
    Task<AzureOperationResponse<IndexGetStatisticsResult>> GetStatisticsWithHttpMessagesAsync(
        string indexName,
        SearchRequestOptions searchRequestOptions = default(SearchRequestOptions),
        Dictionary<string, List<string>> customHeaders = null,
        CancellationToken cancellationToken = default(CancellationToken));

In `IndexesOperationsExtensions`:

    // Simplified asynchronous operation
    public static Task<IndexGetStatisticsResult> GetStatisticsAsync(
        this IIndexesOperations operations,
        string indexName,
        SearchRequestOptions searchRequestOptions = default(SearchRequestOptions),
        CancellationToken cancellationToken = default(CancellationToken));

    // Simplified synchronous operation
    public static IndexGetStatisticsResult GetStatistics(
        this IIndexesOperations operations,
        string indexName,
        SearchRequestOptions searchRequestOptions = default(SearchRequestOptions));

1.1-es verzió kezdve, az Azure keresési .NET SDK rendezi művelet módszerek másképp:

 - Választható paraméterek most modellezni, inkább paraméterek alapértelmezettként további módszer túlterhelések mint. Néha jelentősen csökkenti a módszer túlterhelések, számát.
 - A bővítmény módszerek most elrejtése sok HTTP felesleges részleteit a kíván a hívó féllel. Például a régebbi verziói a SDK visszaadott állapotkód-HTTP, amelyek gyakran nem kell ellenőrizni, mert a művelet módszerek throw egy válasz objektum `CloudException` bármely állapot kód, amely a hibát jelez. Az új bővítmény módszerek csak vissza modell objektumai mentése, a problémát, hogy a kód tördelésének őket.
 - Az alapvető felületek, viszont most ad a HTTP szintjén több szabályozási Ha szüksége lenne rá elérhetővé tétele módszereket. Most már az egyéni HTTP fejlécek bekerülnek az kérelmeket, és az új átviheti `AzureOperationResponse<T>` visszatérési típus közvetlen hozzáférés az lehetővé teszi a `HttpRequestMessage` és `HttpResponseMessage` a művelethez. `AzureOperationResponse`a könyvjelzőnév a `Microsoft.Rest.Azure` névtér és felváltja `Hyak.Common.OperationResponse`.

#### <a name="scoringparameters-changes"></a>ScoringParameters módosítása

Nevű új osztály `ScoringParameter` a legújabb SDK adja meg a paraméterek egy keresési feltételt a profilok pontozás könnyebb hozzá lett adva. A korábban a `ScoringProfiles` tulajdonsága az `SearchParameters` osztály adta meg `IList<string>`; Most írta-e be `IList<ScoringParameter>`.

##### <a name="example"></a>Példa

Ha a kód néz ki:

    var sp = new SearchParameters();
    sp.ScoringProfile = "jobsScoringFeatured";      // Use a scoring profile
    sp.ScoringParameters = new[] { "featuredParam-featured", "mapCenterParam-" + lon + "," + lat };

Módosíthatja a bármely build hibáinak elhárításához: 

    var sp = new SearchParameters();
    sp.ScoringProfile = "jobsScoringFeatured";      // Use a scoring profile
    sp.ScoringParameters =
        new[]
        {
            new ScoringParameter("featuredParam", new[] { "featured" }),
            new ScoringParameter("mapCenterParam", GeographyPoint.Create(lat, lon))
        };

#### <a name="model-class-changes"></a>Modell osztályához módosítása

Az aláírás miatt változik, a [művelet mód megváltozik](#OperationMethodChanges), sok osztályai a `Microsoft.Azure.Search.Models` névtér átnevezése vagy eltávolítása. Példa:

 - `IndexDefinitionResponse`felváltotta`AzureOperationResponse<Index>`
 - `DocumentSearchResponse`a átnevezése`DocumentSearchResult`
 - `IndexResult`a átnevezése`IndexingResult`
 - `Documents.Count()`most függvény egy `long` a dokumentum Count helyett egy`DocumentCountResponse`
 - `IndexGetStatisticsResponse`a átnevezése`IndexGetStatisticsResult`
 - `IndexListResponse`a átnevezése`IndexListResult`

Összefoglalva, `OperationResponse`-származtatott osztályai, hogy csak úgy veheti körül a modell objektum el lett távolítva. A hátralévő osztályok kellett volna, a másik van utótag `Response` való `Result`.

##### <a name="example"></a>Példa

Ha a kód néz ki:

    IndexerGetStatusResponse statusResponse = null;

    try
    {
        statusResponse = _searchClient.Indexers.GetStatus(indexer.Name);
    }
    catch (Exception ex)
    {
        Console.WriteLine("Error polling for indexer status: {0}", ex.Message);
        return;
    }

    IndexerExecutionResult lastResult = statusResponse.ExecutionInfo.LastResult;

Módosíthatja a bármely build hibáinak elhárításához:

    IndexerExecutionInfo status = null;

    try
    {
        status = _searchClient.Indexers.GetStatus(indexer.Name);
    }
    catch (Exception ex)
    {
        Console.WriteLine("Error polling for indexer status: {0}", ex.Message);
        return;
    }

    IndexerExecutionResult lastResult = status.LastResult;

##### <a name="response-classes-and-ienumerable"></a>Válasz osztályok és IEnumerable

Egy további módosítást, amely hatással lehetnek a kód válasz osztályok gyűjtemények élvező már nem valósítja `IEnumerable<T>`. Ehelyett a webhelycsoport tulajdonság közvetlenül is elérheti. Ha például a kód így néz ki:

    DocumentSearchResponse<Hotel> response = indexClient.Documents.Search<Hotel>(searchText, sp);
    foreach (SearchResult<Hotel> result in response)
    {
        Console.WriteLine(result.Document);
    }

Módosíthatja a bármely build hibáinak elhárításához:

    DocumentSearchResult<Hotel> response = indexClient.Documents.Search<Hotel>(searchText, sp);
    foreach (SearchResult<Hotel> result in response.Results)
    {
        Console.WriteLine(result.Document);
    }

##### <a name="important-note-for-web-applications"></a>Fontos megjegyzés webalkalmazások számára

Ha serializes webalkalmazás `DocumentSearchResponse` közvetlenül a keresési eredmények küldése a böngészőben, a kód módosítani kell, vagy az eredmények nem szerializálni megfelelően. Ha például a kód így néz ki:

    public ActionResult Search(string q = "")
    {
        // If blank search, assume they want to search everything
        if (string.IsNullOrWhiteSpace(q))
            q = "*";

        return new JsonResult
        {
            JsonRequestBehavior = JsonRequestBehavior.AllowGet,
            Data = _featuresSearch.Search(q)
        };
    }

Az első szerint módosíthatja a `.Results` tulajdonság javíthatja a keresési eredmények megjelenítését a keresési válasz:

    public ActionResult Search(string q = "")
    {
        // If blank search, assume they want to search everything
        if (string.IsNullOrWhiteSpace(q))
            q = "*";

        return new JsonResult
        {
            JsonRequestBehavior = JsonRequestBehavior.AllowGet,
            Data = _featuresSearch.Search(q).Results
        };
    }

Be kell keressen ilyen esetben a kódot a saját maga; **A fordító nem figyelmeztet,** mert `JsonResult.Data` típusú `object`.

#### <a name="cloudexception-changes"></a>CloudException módosítása

A `CloudException` osztály helyezte át a `Hyak.Common` névtér az `Microsoft.Rest.Azure` névtér. Emellett a `Error` tulajdonság átnevezett `Body`.

#### <a name="searchserviceclient-and-searchindexclient-changes"></a>SearchServiceClient és a SearchIndexClient

Milyen típusú a `Credentials` tulajdonság megváltozott `SearchCredentials` az alap osztály `ServiceClientCredentials`. Ha el szeretné érni a `SearchCredentials` , egy `SearchIndexClient` vagy `SearchServiceClient`, használja az új `SearchCredentials` tulajdonság.

A SDK régebbi verzióiban `SearchServiceClient` és `SearchIndexClient` beállításikon konstruktorok kellett egy `HttpClient` paraméter. Ezek került a helyére konstruktorok használatát, amelyek egy `HttpClientHandler` és tömbje `DelegatingHandler` objektumok. Ezzel megkönnyíti az telepítheti az egyéni kezelők feldolgozása előre a HTTP-kérelmeket, ha szükséges.

Végül a beállításikon konstruktorok egy `Uri` és `SearchCredentials` megváltozott. Ha például telepítette a kódot, amely a következőképpen néz ki:

    var client =
        new SearchServiceClient(
            new SearchCredentials("abc123"),
            new Uri("http://myservice.search.windows.net"));

Módosíthatja a bármely build hibáinak elhárításához:

    var client =
        new SearchServiceClient(
            new Uri("http://myservice.search.windows.net"),
            new SearchCredentials("abc123"));

Is vegye figyelembe, hogy a hitelesítő adatok paraméter típusú módosítását `ServiceClientCredentials`. Valószínű, hogy a kód óta befolyásolja a `SearchCredentials` származik `ServiceClientCredentials`.

#### <a name="passing-a-request-id"></a>A kérés azonosító átadása

A SDK régebbi verzióiban, a kérelem Azonosítóra sikerült beállítani a `SearchServiceClient` vagy `SearchIndexClient` és felveszik minden összehívásban a REST API-hoz. Ez akkor hasznos, az alábbiakkal a keresési szolgáltatás kapcsolatos problémákat, ha módosítani szeretné a kapcsolatfelvétel az ügyfélszolgálattal. Azonban érdemes több egy kérés egyedi Azonosítót az összes művelet beállítása, hanem az azonos azonosító összes művelet használni. Emiatt a `SetClientRequestId` módszerek `SearchServiceClient` és `SearchIndexClient` el lett távolítva. Ehelyett átviheti a kérelem azonosító mindegyik művelet módszerrel keresztül a választható `SearchRequestOptions` paraméter.

> [AZURE.NOTE] Az elkövetkező kiadásokban SDK hogy új szolgáló eljárás beállítása egy kérés ID globálisan az ügyfél objektumok, amely megfelel a többi Azure SDK által használt megközelítés ad hozzá.

#### <a name="example"></a>Példa

Ha kódot, amely a következőképpen néz ki:

    client.SetClientRequestId(Guid.NewGuid());
    ...
    long count = client.Documents.Count();

Módosíthatja a bármely build hibáinak elhárításához:

    long count = client.Documents.Count(new SearchRequestOptions(requestId: Guid.NewGuid()));

#### <a name="interface-name-changes"></a>Felület nevének módosítása

A művelet csoportnevei felület biztosítani szeretné konzisztens a megfelelő tulajdonságot nevük módosultak:

 - Milyen típusú `ISearchServiceClient.Indexes` az átnevezett `IIndexOperations` való `IIndexesOperations`.
 - Milyen típusú `ISearchServiceClient.Indexers` az átnevezett `IIndexerOperations` való `IIndexersOperations`.
 - Milyen típusú `ISearchServiceClient.DataSources` az átnevezett `IDataSourceOperations` való `IDataSourcesOperations`.
 - Milyen típusú `ISearchIndexClient.Documents` az átnevezett `IDocumentOperations` való `IDocumentsOperations`.

Ez a változás valószínű, hogy a kód érinti, kivéve a létrehozott mocks ezen felületek tesztelése céljából.

<a name="BugFixesV1"></a>
### <a name="bug-fixes-in-version-11"></a>Hibajavítás 1.1-es verzió

Hiba történt az egyéni modell osztályok szerializálási Azure keresési .NET SDK régebbi verzióiban. A hiba akkor fordulhat elő, ha létrehozott egy egyéni modell osztályához egy tulajdonság, egy érték nem nullable típusú.

#### <a name="steps-to-reproduce"></a>Reprodukálja lépései

Hozzon létre egy egyéni modell osztályához tulajdonság nem nullable érték típusú. Ha például hozzáadása a nyilvános `UnitCount` típusú tulajdonság `int` helyett `int?`.

Ha indexelést állít be az alapértelmezett érték, hogy az adott típusú dokumentumot (például a 0 értéket az `int`), a mező üres Azure keresés lesz. Ha később kereshet, hogy a dokumentumot, a `Search` hívás fog throw `JsonSerializationException` panaszos, amely nem alakítható át `null` való `int`.

Szűrők is, nem működnek meg a várt módon, mivel az index helyett a kívánt értéket null írt.

#### <a name="fix-details"></a>Részletek javítása

Ez a probléma 1.1-es verzió SDK rögzített azt. Most, ha van egy modell osztályához jelennek meg:

    public class Model
    {
        public string Key { get; set; }

        public int IntValue { get; set; }
    }

Beállíthatja, és `IntValue` 0, ez az érték most megfelelően a vezetékes a 0-nak szerializálásának és tárolja a 0 értéket a tárgymutató. Kerekítés esetén is a várt módon működik.

Az eljárás tudatában kell lennie egy potenciális probléma: nem nullable tulajdonság egy modell típust használja, esetén annak **biztosítására** , hogy a tárgymutató nélkül dokumentumok esetében a megfelelő mező null értéket tartalmaz. A SDK sem az Azure keresési REST API segít, hogy ez a hivatkozási.

Ez nem egy hipotetikus problémát: Képzelje el, ahol új mező felvétele meglévő indexet típusú példa `Edm.Int32`. Miután frissítette a tárgymutató-definícióját, minden dokumentum lesz az új mező null érték (mivel diagramtípusokat nullable Azure keresés). Ha egy nem-nullable az majd használja a modell osztályához `int` kaphat az adott mező tulajdonság, egy `JsonSerializationException` így dokumentumok beolvasása közben:

    Error converting value {null} to type 'System.Int32'. Path 'IntValue'.

Emiatt továbbra is ajánljuk, hogy használja az nullable típusok modell órájához a legjobb módszer.

Ezt a hibát, és a fix további információra kíváncsi olvassa el [a probléma a GitHub](https://github.com/Azure/azure-sdk-for-net/issues/1063).
