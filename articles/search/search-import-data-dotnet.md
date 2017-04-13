<properties
    pageTitle="A .NET SDK használatával Azure keresése az adatok betöltésének |} Microsoft Azure |} A felhőben tárolt keresési szolgáltatás"
    description="Megtudhatja, hogy miként tölthet fel adatokat a .NET SDK használatával Azure keresési index."
    services="search"
    documentationCenter=""
    authors="brjohnstmsft"
    manager="jhubbard"
    editor=""
    tags=""/>

<tags
    ms.service="search"
    ms.devlang="dotnet"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="brjohnst"/>

# <a name="upload-data-to-azure-search-using-the-net-sdk"></a>Feltölteni az adatokat a .NET SDK használatával Azure-keresés
> [AZURE.SELECTOR]
- [– Áttekintés](search-what-is-data-import.md)
- [.NET](search-import-data-dotnet.md)
- [TÖBBI](search-import-data-rest-api.md)

Ez a cikk bemutatja, hogyan az adatok importálása az Azure keresési index az [Azure keresési .NET SDK](https://msdn.microsoft.com/library/azure/dn951165.aspx) használatával.

Mielőtt elkezdené az útmutató, már rendelkeznie kell [Azure keresési index létrehozása](search-what-is-an-index.md). Ez a cikk is feltételezi, hogy már létrehozott egy `SearchServiceClient` objektum, ahogy azt [a .NET SDK használatával Azure keresési index létrehozása](search-create-index-dotnet.md#CreateSearchServiceClient).

Figyelje meg, hogy az összes minta kód, a jelen cikkben írott C#. A teljes forrást találhat [a GitHub](http://aka.ms/search-dotnet-howto)kód.

Annak érdekében, hogy a leküldéses dokumentumok a indexbe a .NET SDK használatával, meg kell:

  1. Hozzon létre egy `SearchIndexClient` való csatlakozáshoz a keresési index objektumot.
  2. Hozzon létre egy `IndexBatch` tartalmazó hozzá kell adni a dokumentumokat módosítás, vagy törölték.
  3. Hívja fel a `Documents.Index` módszer a `SearchIndexClient` küldése a `IndexBatch` a keresési indexhez.

## <a name="i-create-an-instance-of-the-searchindexclient-class"></a>Lehet. Hozzon létre egy példányát a SearchIndexClient osztály
Adatok importálása az index az Azure keresési .NET SDK használatával, hogy szüksége lesz egy példányának létrehozása a `SearchIndexClient` osztály. Állíthat össze példány saját magát, de a könnyebb, ha már van egy `SearchServiceClient` példány hívja fel a `Indexes.GetClient` módot. Ha például az alábbiakban hogyan érhet egy `SearchIndexClient` "Szállodák" nevű az index egy `SearchServiceClient` nevű `serviceClient`:

```csharp
SearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");
```

> [AZURE.NOTE] Egy tipikus keresési alkalmazásban index kezelési és statisztikai sokaságra kezeli a keresési lekérdezések egy külön összetevőt. `Indexes.GetClient`index feltöltése, mert azt takaríthat meg, a problémát, hogy egy másik alkalmas `SearchCredentials`. A felügyeleti kulcs létrehozásához használt megkerülhetők mindezt a `SearchServiceClient` az új `SearchIndexClient`. Jó helyen jár, az alkalmazás, amely végrehajtja a lekérdezések részében, akkor célszerűbb létrehozása a `SearchIndexClient` közvetlenül, hogy egy rendszergazda billentyű helyett a lekérdezés kulcs továbbíthatja. Ez a [legalacsonyabb jogosultság elve](https://en.wikipedia.org/wiki/Principle_of_least_privilege) egységes és, hogy az alkalmazás nagyobb biztonságot nyújt segítséget. További tudnivalók a felügyeleti és lekérdezés kulcsokról a [Azure keresési REST API-hivatkozás MSDN webhelyen](https://msdn.microsoft.com/library/azure/dn798935.aspx)talál.

`SearchIndexClient`van egy `Documents` tulajdonság. Ez a tulajdonság kell hozzáadása, módosítása, törlése vagy dokumentumok a index lekérdezés összes módszerek biztosít.

## <a name="ii-decide-which-indexing-action-to-use"></a>II. Döntse el, mely az indexelési művelet használata
Az adatimportálás a .NET SDK használatával, meg kell csomag készítése az adatokról egy `IndexBatch` objektumot. Egy `IndexBatch` gyűjteménye beágyazza `IndexAction` objektumok, amelyek tartalmaz egy dokumentumot, és egy tulajdonság, amely arra utasítja az Azure keresési milyen műveletet a dokumentumon (feltöltés, egyesítés, törlés, stb.). Attól függően, hogy melyik a műveletek alatti lehetőséget választja, csak bizonyos mezőket szerepelnie kell minden dokumentum esetében:

Művelet | Leírás | Az egyes dokumentumokhoz a szükséges mezőket | Jegyzetek
--- | --- | --- | ---
`Upload` | Egy `Upload` művelet hasonlít egy "upsert" Ha a dokumentum fog illeszti be, ha új és frissített/helyett Ha létezik. | kulcs, valamint a többi mezőt kíván definiálni | Cseréjekor frissítése vagy egy meglévő dokumentumot, minden olyan mező, amely nem szerepel a kérelem van-e meg a mezőben található `null`. Ez akkor fordul elő, akkor is, ha a mező korábban megadott nem null értékre.
`Merge` | Meglévő dokumentum a megadott mezők frissítése. Ha a dokumentumot a tárgymutató nem létezik, az egyesítés sikertelen lesz. | kulcs, valamint a többi mezőt kíván definiálni | Minden mezőben adja meg, a körlevél lecseréli a meglévő mező a dokumentumban. Ide tartoznak a típusú mezők `DataType.Collection(DataType.String)`. Ha például a dokumentum tartalmaz egy mező `tags` értékkel `["budget"]` értéket az egyesítés végrehajtása és `["economy", "pool"]` az `tags`, végső értéke a `tags` mező lesz `["economy", "pool"]`. Nem lesznek `["budget", "economy", "pool"]`.
`MergeOrUpload` | Ez a művelet úgy viselkednek, mint `Merge` , ha az index már létezik az adott billentyűt a dokumentumokat. Ha a dokumentum nem létezik, viselkedik `Upload` új dokumentumot. | kulcs, valamint a többi mezőt kíván definiálni | -
`Delete` | Az index eltávolítja a megadott dokumentumot. | csak a kulcs | Minden olyan mezők, eltérő figyelmen kívül hagyja a fő mezőben adja meg. Ha el szeretné távolítani egy önálló mező a dokumentumban lévő, `Merge` ehelyett és egyszerűen mező beállítása kifejezetten null.

Megadhatja, hogy milyen különböző statikus módszerek a használni kívánt művelet a `IndexBatch` és `IndexAction` osztályok, a következő szakaszban látható módon.

## <a name="iii-construct-your-indexbatch"></a>III. A IndexBatch létrehozása
Most, hogy milyen műveleteket kell végrehajtani a dokumentumokon, készen áll össze a `IndexBatch`. Az alábbi példa bemutatja, hogyan hozzon létre egy köteg néhány különböző műveleteket. Megjegyzés: a példában nevű osztály egyéni `Hotel` , amely a "Szállodák" tárgymutató dokumentum rendeli.

```csharp
var actions =
    new IndexAction<Hotel>[]
    {
        IndexAction.Upload(
            new Hotel()
            {
                HotelId = "1",
                BaseRate = 199.0,
                Description = "Best hotel in town",
                DescriptionFr = "Meilleur hôtel en ville",
                HotelName = "Fancy Stay",
                Category = "Luxury",
                Tags = new[] { "pool", "view", "wifi", "concierge" },
                ParkingIncluded = false,
                SmokingAllowed = false,
                LastRenovationDate = new DateTimeOffset(2010, 6, 27, 0, 0, 0, TimeSpan.Zero),
                Rating = 5,
                Location = GeographyPoint.Create(47.678581, -122.131577)
            }),
        IndexAction.Upload(
            new Hotel()
            {
                HotelId = "2",
                BaseRate = 79.99,
                Description = "Cheapest hotel in town",
                DescriptionFr = "Hôtel le moins cher en ville",
                HotelName = "Roach Motel",
                Category = "Budget",
                Tags = new[] { "motel", "budget" },
                ParkingIncluded = true,
                SmokingAllowed = true,
                LastRenovationDate = new DateTimeOffset(1982, 4, 28, 0, 0, 0, TimeSpan.Zero),
                Rating = 1,
                Location = GeographyPoint.Create(49.678581, -122.131577)
            }),
        IndexAction.MergeOrUpload(
            new Hotel()
            {
                HotelId = "3",
                BaseRate = 129.99,
                Description = "Close to town hall and the river"
            }),
        IndexAction.Delete(new Hotel() { HotelId = "6" })
    };

var batch = IndexBatch.New(actions);
```

Ebben az esetben azt használatával `Upload`, `MergeOrUpload`, és `Delete` a keresési műveletként megadott nevű a módszerek a `IndexAction` osztály.

Tegyük fel, hogy ez például az "Szállodák" index már kitölti a dokumentumok száma. Ne feledje, hogy hogyan azt nem kellett használata esetén, adja meg az összes lehetséges dokumentum mezőket `MergeOrUpload` , és hogyan akkor csak meg a dokumentum billentyűt (`HotelId`) használata esetén `Delete`.

Figyeljen arra, hogy csak is legfeljebb 1000 dokumentumok egyetlen indexelési kérelem.

> [AZURE.NOTE] Ebben a példában azt is alkalmazása különböző műveletek különböző dokumentumot. Ha azt át a hívó helyett köteg minden dokumentum azonos műveleteket `IndexBatch.New`, használhatja a más statikus módszereket is `IndexBatch`. Ha például úgy lehetett létrehozni, kötegekben hívja fel `IndexBatch.Merge`, `IndexBatch.MergeOrUpload`, vagy `IndexBatch.Delete`. Ezeket a módszereket dokumentumgyűjtemények készítése (típusú objektumok `Hotel` ebben a példában) helyett `IndexAction` objektumok.

## <a name="iv-import-data-to-the-index"></a>IV. Adatok importálása az index
Most, hogy van egy inicializált `IndexBatch` objektum, elküldheti a indexhez meghívásával `Documents.Index` a a `SearchIndexClient` objektumot. A következő példa bemutatja, hogyan hívni `Index`, továbbá, néhány további lépést kell végrehajtania:

```csharp
try
{
    indexClient.Documents.Index(batch);
}
catch (IndexBatchException e)
{
    // Sometimes when your Search service is under load, indexing will fail for some of the documents in
    // the batch. Depending on your application, you can take compensating actions like delaying and
    // retrying. For this simple demo, we just log the failed document keys and continue.
    Console.WriteLine(
        "Failed to index some of the documents: {0}",
        String.Join(", ", e.IndexingResults.Where(r => !r.Succeeded).Select(r => r.Key)));
}

Console.WriteLine("Waiting for documents to be indexed...\n");
Thread.Sleep(2000);
```

Megjegyzés: a `try` / `catch` körül a hívást a `Index` módot. A tényleges blokk egy fontos hiba esetet indexelés kezeli. Ha az Azure keresési szolgáltatás nem sikerült indexelni a dokumentumokat a köteg részét egy `IndexBatchException` által elő `Documents.Index`. Ez akkor fordulhat elő, ha a dokumentumok vannak indexelés, miközben a szolgáltatás nagy terhelés alatt áll. **Kifejezetten ajánljuk, ebben az esetben a kód kifejezetten kezelése.** Késleltetés, és próbálkozzon újra a dokumentumok, amelyeket nem sikerült az indexelés, vagy jelentkezzen be, és továbbra is, például a minta nem, vagy mást attól függően, hogy az alkalmazás konzisztencia adatszolgáltatási végezhető.

Végül a kódot a példában a fenti két másodpercig késések. Az indexelés történik aszinkron a az Azure keresési szolgáltatás, így a minta alkalmazás kell Várjon egy kis ideig, annak érdekében, hogy a dokumentumokhoz való kereséshez használható. Ilyen késések szükségesek általában csak a bemutatók, a vizsgálatok és a minta alkalmazások.

<a name="HotelClass"></a>
### <a name="how-the-net-sdk-handles-documents"></a>Hogyan kezeli a .NET SDK a dokumentumok

Akkor is kell tudni szeretné, hogy miként tölthet fel, például a felhasználó által definiált osztály példányainak-e az Azure keresési .NET SDK `Hotel` az indexhez. Szeretné, hogy a kérdés, tekintse meg a `Hotel` osztály, megfelelteti a tárgymutató sémának definiált [a .NET SDK használatával Azure keresési index](search-create-index-dotnet.md#DefineIndex)létrehozása:

```csharp
[SerializePropertyNamesAsCamelCase]
public partial class Hotel
{
    public string HotelId { get; set; }

    public double? BaseRate { get; set; }

    public string Description { get; set; }

    [JsonProperty("description_fr")]
    public string DescriptionFr { get; set; }

    public string HotelName { get; set; }

    public string Category { get; set; }

    public string[] Tags { get; set; }

    public bool? ParkingIncluded { get; set; }

    public bool? SmokingAllowed { get; set; }

    public DateTimeOffset? LastRenovationDate { get; set; }

    public int? Rating { get; set; }

    public GeographyPoint Location { get; set; }

    // ToString() method omitted for brevity...
}
```

A legfontosabb dolog figyelje meg, hogy minden nyilvános tulajdonsága `Hotel` megfelel egy mezőt a tárgymutató-definícióban, de az egyik legfontosabb különbség: minden mező nevének betűvel kisbetűk ("teve eset"), miközben az egyes nyilvános tulajdonság neve `Hotel` egy csupa nagybetűvel ("Pascal eset") betűvel kezdődő. Ez a közös példa az adatok kötelező végezze el a cél séma esetén az alkalmazás fejlesztője irányításának kívüli .NET-alkalmazásokban. Ahelyett, hogy a .NET-irányelvek elnevezési azáltal, hogy a tulajdonság neve teve dobozos megsérthetik, a tulajdonságnév hozzárendelése teve dobozos automatikusan a SDK kiderítheti a `[SerializePropertyNamesAsCamelCase]` attribútum.

> [AZURE.NOTE] Az Azure keresési .NET SDK a [NewtonSoft JSON.NET](http://www.newtonsoft.com/json/help/html/Introduction.htm) tár szerializálni és az egyéni modell objektumai és a JSON deszerializálni használja. Testre szabhatja a szerializálási, ha szükséges. További részleteket a [frissítése az Azure keresési .NET SDK 1.1-es verzió](search-dotnet-sdk-migration.md#WhatsNew)található. Egy példa erre-e a `[JsonProperty]` a attribútum a `DescriptionFr` tulajdonság a fenti példakódot.

A második fontos tudnivaló a kapcsolatban a `Hotel` osztály a nyilvános tulajdonságok adattípusai. A tulajdonságok .NET típusú feleltesse meg azok a tárgymutató-definícióban egyenértékű mezőtípusok. Például a `Category` tulajdonság térképek karakterlánc a `category` mezőt, amely típusú `DataType.String`. Vannak olyan közötti hasonló típusú hozzárendelés `bool?` és `DataType.Boolean`, `DateTimeOffset?` és `DataType.DateTimeOffset`stb. A típus hozzárendelés bizonyos szabályok vannak dokumentált a a `Documents.Get` módszer az [MSDN webhelyen](https://msdn.microsoft.com/library/azure/dn931291.aspx).

Ez azt jelenti, hogy a saját osztályok tartományként a dokumentumok működik mindkét irányba; Keresési eredmények beolvasásához, és a automatikusan deszerializálni őket a választási lehetőség típusú SDK van, ahogy a [következő cikk](search-query-dotnet.md)is.

> [AZURE.NOTE] Az Azure keresési .NET SDK is támogat dinamikusan típusú dokumentumok használata a `Document` osztály kulcs/érték megfeleltetésének mezőértékek mező nevét. Az esetekben lehet hasznos, ha nem tudja, hogy az index séma tervezéskor, vagy ha kényelmesen adott osztályok kötést létrehozni. A SDK csomagjában talál, amelyek a dokumentumok foglalkozik módszerek van, amely a túlterhelések a `Document` osztály, valamint erősen beírta túlterhelések viselő általános típusú paramétert. Csak ez utóbbi használt ebben a cikkben a minta kódot. Láthatja, hogy további tudnivalók a `Document` osztály az [MSDN webhelyen](https://msdn.microsoft.com/library/azure/microsoft.azure.search.models.document.aspx).

**Egy fontos megjegyzés adattípusai**

A saját modell osztályok Azure keresési index hozzárendelése tervezésekor azt javasoljuk, például deklarálása értéktípusok tulajdonságok `bool` és `int` kell nullable (például `bool?` helyett `bool`). Egy nem nullable tulajdonsággal esetén annak **biztosítására** , hogy a tárgymutató nélkül dokumentumok esetében a megfelelő mező null értéket tartalmaz. A SDK csomagjában talál, sem a Azure keresési szolgáltatás segít, hogy ez a hivatkozási.

Ez nem egy hipotetikus problémát: Képzelje el, ahol új mező felvétele meglévő indexet típusú példa `DataType.Int32`. Miután frissítette a tárgymutató-definícióját, minden dokumentum lesz az új mező null érték (mivel diagramtípusokat nullable Azure keresés). Ha egy nem-nullable az majd használja a modell osztályához `int` kaphat az adott mező tulajdonság, egy `JsonSerializationException` így dokumentumok beolvasása közben:

    Error converting value {null} to type 'System.Int32'. Path 'IntValue'.

Emiatt azt javasoljuk, hogy használja az nullable típusok modell órájához a legjobb.

## <a name="next"></a>Következő
Az Azure keresési index feltöltése, után nem fogja készen áll a kezdésre kiállító lekérdezések dokumentumok kereséséhez. [Lekérdezés az Azure keresési Index](search-query-overview.md) talál további információt.
