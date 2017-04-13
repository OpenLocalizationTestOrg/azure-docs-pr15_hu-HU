<properties
    pageTitle="A lekérdezés a Azure keresési Index a .NET SDK használatával |} Microsoft Azure |} A felhőben tárolt keresési szolgáltatás"
    description="Azure keresés keresési lekérdezés összeállítása és a keresési találatok szűrés és rendezés paramétereivel."
    services="search"
    manager="jhubbard"
    documentationCenter=""
    authors="brjohnstmsft"
/>

<tags
    ms.service="search"
    ms.devlang="dotnet"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="brjohnst"/>

# <a name="query-your-azure-search-index-using-the-net-sdk"></a>A lekérdezés a Azure keresési index, a .NET SDK használatával
> [AZURE.SELECTOR]
- [– Áttekintés](search-query-overview.md)
- [Portál](search-explorer.md)
- [.NET](search-query-dotnet.md)
- [TÖBBI](search-query-rest-api.md)

Ez a cikk bemutatja, hogyan használja az [Azure keresési .NET SDK](https://msdn.microsoft.com/library/azure/dn951165.aspx)index lekérdezéséhez.

Mielőtt elkezdené az útmutató, már rendelkeznie kell [Azure keresési index létrehozása](search-what-is-an-index.md) és [töltve adatokkal](search-what-is-data-import.md).

Figyelje meg, hogy az összes minta kód, a jelen cikkben írott C#. A teljes forrást találhat [a GitHub](http://aka.ms/search-dotnet-howto)kód.

## <a name="i-identify-your-azure-search-services-query-api-key"></a>Lehet. Az Azure keresőszolgáltatás lekérdezés api-kulcs azonosítása
Most, hogy a létrehozott Azure keresési index, szinte készen áll a .NET SDK használatával lekérdezést. Először is szüksége lesz szerezze be a lekérdezés api-kulcsok a keresési szolgáltatás, a kiépítéstől létrehozott közül. A .NET SDK küld a api-kulcs minden kérésre a szolgáltatásban. Kérés / alapján között az alkalmazás küldése a kérést, és a szolgáltatást, amely kezeli, akkor az adatvédelmi rendelkező érvényes kulcsot hoz létre.

1. A szolgáltatás api-kulcsok keresése be kell jelentkeznie őket az [Azure-portál](https://portal.azure.com/)
2. Nyissa meg az Azure keresőszolgáltatás lap
3. Kattintson a "Billentyűparancsok" ikon

A szolgáltatás *felügyeleti* és *lekérdezés kulcsokról*fog rendelkezni.

  - Az elsődleges és másodlagos *felügyeleti kulcsok* az összes művelet, beleértve az azt jelenti, hogy a szolgáltatás kezelése, létrehozása és törlése az indexek, indexelő és adatforrások teljes engedélyek biztosítása. Két kulcsa van, hogy továbbra is a másodlagos billentyűt használja, ha úgy dönt, hogy az elsődleges kulcsot, és fordítva újragenerálása.
  - A *lekérdezés billentyűk* indexek és a dokumentumok csak olvasási hozzáférést, és a szokásos osztják meg ezt a problémát a keresési kérelem ügyfélalkalmazások.

Index lekérdezése alkalmazásában is használhatja a lekérdezés kulcsok közül. A felügyeleti billentyűk lekérdezések is használható, de kell használnia a lekérdezés kulcs alkalmazás kódban jobban követi a [legalacsonyabb jogosultság elve](https://en.wikipedia.org/wiki/Principle_of_least_privilege)szerint.

## <a name="ii-create-an-instance-of-the-searchindexclient-class"></a>II. Hozzon létre egy példányát a SearchIndexClient osztály
Az Azure keresési .NET SDK a lekérdezések ki kell létrehozni egy példányát a `SearchIndexClient` osztály. Az osztály több konstruktorok tartalmaz. Azt, amelyiket megnyitja a keresési szolgáltatás nevet, index, és egy `SearchCredentials` paraméterek az objektumot. `SearchCredentials`tördelése a api-használatával.

Az alábbi kód létrehoz egy új `SearchIndexClient` az "Szállodák" index (létrehozása [az Azure keresési index a .NET SDK használatával](search-create-index-dotnet.md)létrehozott) használata értékeket a keresési szolgáltatás neve és az alkalmazás a konfigurációs fájl a tárolt API-ja kulcs (`app.config` vagy `web.config`):

```csharp
string searchServiceName = ConfigurationManager.AppSettings["SearchServiceName"];
string queryApiKey = ConfigurationManager.AppSettings["SearchServiceQueryApiKey"];

SearchIndexClient indexClient = new SearchIndexClient(searchServiceName, "hotels", new SearchCredentials(queryApiKey));
```

`SearchIndexClient`van egy `Documents` tulajdonság. Ez a tulajdonság bemutat összes az indexek Azure keresési lekérdezést kell.

## <a name="iii-query-your-index"></a>III. A lekérdezés az index
A .NET SDK a keresés – tárcsáz, egyszerűen a `Documents.Search` módszer a `SearchIndexClient`. Ez a módszer vesz igénybe néhány paramétereket, beleértve a keresett szöveget mentén egy `SearchParameters` objektumra, amely is használható, ha tovább szeretné finomítani a lekérdezést.

#### <a name="types-of-queries"></a>Lekérdezések típusai
A két fő [lekérdezés típusai](search-query-overview.md#types-of-queries) használni kívánt `search` és `filter`. A `search` a lekérdezés minden _kereshető_ mezőjében a index egy vagy több feltétel keres. A `filter` lekérdezés kiértékeli a logikai kifejezés folyamatosan _szűrhető_ mezők indexet.

Keresés és a szűrők előfordulnak használatával a `Documents.Search` módot. Keresési lekérdezés az továbbíthatók a `searchText` paraméter, miközben szűrőkifejezés küldhető a `Filter` tulajdonsága az `SearchParameters` osztály. Keresés nélkül szűréséhez csak a sikeres `"*"` számára a `searchText` paraméter. A keresés szűrése nélkül, hagyja a `Filter` tulajdonság Adatbázisjelszó törlése, vagy ad át egy `SearchParameters` egyáltalán példány.

#### <a name="example-queries"></a>Példa lekérdezések

A következő példa kódot néhány változatos módszerekkel, amelyekkel a lekérdezés az "Szállodák" index létrehozása [a .NET SDK használatával Azure keresési index](search-create-index-dotnet.md#DefineIndex)definiált jeleníti meg. Ne feledje, hogy a dokumentumokat a keresési eredmények vissza példányát a `Hotel` osztály adták meg az [Adatok importálása az Azure-keresés a .NET SDK használatával](search-import-data-dotnet.md#HotelClass). A minta kódot él egy `WriteDocuments` módszer kimeneti a konzol a találatok között. Ez a módszer a következő szakaszban ismertetett.

```csharp
SearchParameters parameters;
DocumentSearchResult<Hotel> results;

Console.WriteLine("Search the entire index for the term 'budget' and return only the hotelName field:\n");

parameters =
    new SearchParameters()
    {
        Select = new[] { "hotelName" }
    };

results = indexClient.Documents.Search<Hotel>("budget", parameters);

WriteDocuments(results);

Console.Write("Apply a filter to the index to find hotels cheaper than $150 per night, ");
Console.WriteLine("and return the hotelId and description:\n");

parameters =
    new SearchParameters()
    {
        Filter = "baseRate lt 150",
        Select = new[] { "hotelId", "description" }
    };

results = indexClient.Documents.Search<Hotel>("*", parameters);

WriteDocuments(results);

Console.Write("Search the entire index, order by a specific field (lastRenovationDate) ");
Console.Write("in descending order, take the top two results, and show only hotelName and ");
Console.WriteLine("lastRenovationDate:\n");

parameters =
    new SearchParameters()
    {
        OrderBy = new[] { "lastRenovationDate desc" },
        Select = new[] { "hotelName", "lastRenovationDate" },
        Top = 2
    };

results = indexClient.Documents.Search<Hotel>("*", parameters);

WriteDocuments(results);

Console.WriteLine("Search the entire index for the term 'motel':\n");

parameters = new SearchParameters();
results = indexClient.Documents.Search<Hotel>("motel", parameters);

WriteDocuments(results);
```

## <a name="iv-handle-search-results"></a>IV. Keresési eredmények kezelése
A `Documents.Search` módszer eredménye egy `DocumentSearchResult` objektumra, amely tartalmazza a lekérdezés eredményét. A példában az előző szakaszban használt nevű módszer `WriteDocuments` kimeneti a konzol a találatok között:

```csharp
private static void WriteDocuments(DocumentSearchResult<Hotel> searchResults)
{
    foreach (SearchResult<Hotel> result in searchResults.Results)
    {
        Console.WriteLine(result.Document);
    }

    Console.WriteLine();
}
```

Az alábbiakban az eredmények néz a lekérdezések az előző szakaszban, feltéve, hogy a "Szállodák" index kitölti a mintaadatokat az [Adatok importálása az Azure-keresés a .NET SDK használatával](search-import-data-dotnet.md):

```
Search the entire index for the term 'budget' and return only the hotelName field:

Name: Roach Motel

Apply a filter to the index to find hotels cheaper than $150 per night, and return the hotelId and description:

ID: 2   Description: Cheapest hotel in town
ID: 3   Description: Close to town hall and the river

Search the entire index, order by a specific field (lastRenovationDate) in descending order, take the top two results, and show only hotelName and lastRenovationDate:

Name: Fancy Stay        Last renovated on: 6/27/2010 12:00:00 AM +00:00
Name: Roach Motel       Last renovated on: 4/28/1982 12:00:00 AM +00:00

Search the entire index for the term 'motel':

ID: 2   Base rate: 79.99        Description: Cheapest hotel in town     Description (French): Hôtel le moins cher en ville      Name: Roach Motel       Category: Budget        Tags: [motel, budget]   Parking included: yes   Smoking allowed: yes    Last renovated on: 4/28/1982 12:00:00 AM +00:00 Rating: 1/5     Location: Latitude 49.678581, longitude -122.131577

```

A fenti minta kódot a konzol használja a keresési eredmények kimeneti. Hasonlóképpen megjelenítheti a találatokat a saját alkalmazásban kell. Lásd: [Ez a példa a GitHub](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetSample) példa bemutatja, hogyan jeleníti meg a keresési eredmények az ASP.NET MVC-alapú webes alkalmazásokban.
