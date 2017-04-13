<properties
    pageTitle="A .NET SDK használatával Azure keresési index létrehozása |} Microsoft Azure |} A felhőben tárolt keresési szolgáltatás"
    description="Index létrehozása az Azure keresési .NET SDK használatával kódot."
    services="search"
    documentationCenter=""
    authors="brjohnstmsft"
    manager="jhubbard"
    editor=""
    tags="azure-portal"/>

<tags
    ms.service="search"
    ms.devlang="dotnet"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="brjohnst"/>

# <a name="create-an-azure-search-index-using-the-net-sdk"></a>A .NET SDK használatával Azure keresési index létrehozása
> [AZURE.SELECTOR]
- [– Áttekintés](search-what-is-an-index.md)
- [Portál](search-create-index-portal.md)
- [.NET](search-create-index-dotnet.md)
- [TÖBBI](search-create-index-rest-api.md)


Ez a cikk azt ismerteti, hogy azon a folyamaton, használja az [Azure keresési .NET SDK](https://msdn.microsoft.com/library/azure/dn951165.aspx)Azure keresési [index](https://msdn.microsoft.com/library/azure/dn798941.aspx) létrehozása.

Ez az útmutató következő és az index létrehozása előtt rendelkeznie kell már [létrehozott egy Azure keresési szolgáltatás](search-create-service-portal.md).

Figyelje meg, hogy az összes minta kód, a jelen cikkben írott C#. A teljes forrást találhat [a GitHub](http://aka.ms/search-dotnet-howto)kód.

## <a name="i-identify-your-azure-search-services-admin-api-key"></a>Lehet. Az Azure keresési szolgáltatás felügyeleti api-kulcs azonosítása
Most, hogy az Azure keresési szolgáltatás van kiépítve, szinte készen áll kibocsátása kérések szemben a szolgáltatás végpontjának a .NET SDK használatával. Először is szüksége lesz szerezze be a rendszergazda api-kulcsok a keresési szolgáltatás, a kiépítéstől létrehozott közül. A .NET SDK küld a api-kulcs minden kérésre a szolgáltatásban. Kérés / alapján között az alkalmazás küldése a kérést, és a szolgáltatást, amely kezeli, akkor az adatvédelmi rendelkező érvényes kulcsot hoz létre.

1. A szolgáltatás api-kulcsok keresése be kell jelentkeznie őket az [Azure-portál](https://portal.azure.com/)
2. Nyissa meg az Azure keresőszolgáltatás lap
3. Kattintson a "Billentyűparancsok" ikon

A szolgáltatás *felügyeleti* és *lekérdezés kulcsokról*fog rendelkezni.

  - Az elsődleges és másodlagos *felügyeleti kulcsok* az összes művelet, beleértve az azt jelenti, hogy a szolgáltatás kezelése, létrehozása és törlése az indexek, indexelő és adatforrások teljes engedélyek biztosítása. Két kulcsa van, hogy továbbra is a másodlagos billentyűt használja, ha úgy dönt, hogy az elsődleges kulcsot, és fordítva újragenerálása.
  - A *lekérdezés billentyűk* indexek és a dokumentumok csak olvasási hozzáférést, és a szokásos osztják meg ezt a problémát a keresési kérelem ügyfélalkalmazások.

Index létrehozása alkalmazásában is használhatja az elsődleges és másodlagos felügyeleti billentyűt.

<a name="CreateSearchServiceClient"></a>
## <a name="ii-create-an-instance-of-the-searchserviceclient-class"></a>II. Hozzon létre egy példányát a SearchServiceClient osztály
Az Azure keresési .NET SDK használatának megkezdéséhez szüksége lesz egy példányának létrehozása a `SearchServiceClient` osztály. Az osztály több konstruktorok tartalmaz. Az áthelyezni kívánt megnyitja a keresési szolgáltatás neve és a `SearchCredentials` paraméterek az objektumot. `SearchCredentials`tördelése a api-használatával.

Az alábbi kód létrehoz egy új `SearchServiceClient` a keresési szolgáltatás neve és az api-billentyűt, az alkalmazás a konfigurációs fájl a tárolt értékekkel (`app.config` vagy `web.config`):

```csharp
string searchServiceName = ConfigurationManager.AppSettings["SearchServiceName"];
string adminApiKey = ConfigurationManager.AppSettings["SearchServiceAdminApiKey"];

SearchServiceClient serviceClient = new SearchServiceClient(searchServiceName, new SearchCredentials(adminApiKey));
```

`SearchServiceClient`van egy `Indexes` tulajdonság. Ez a tulajdonság minden olyan módszert, létre kell hoznia a listában, frissíthet és törölhet Azure keresési indexek biztosít.

> [AZURE.NOTE] A `SearchServiceClient` osztály kezeli a keresési szolgáltatás-kapcsolatot. Megnyitása túl sok kapcsolatok elkerülése érdekében célszerű kísérel meg megosztani egy példányát és `SearchServiceClient` az alkalmazásban, ha lehetséges. A módszereket szál vannak ilyen megosztásához.

<a name="DefineIndex"></a>
## <a name="iii-define-your-azure-search-index-using-the-index-class"></a>III. Az Azure keresési index használatával meghatározása a `Index` osztály
Egyetlen hívást kezdeményez a `Indexes.Create` módszer a tárgymutatóhoz hoz létre. Ez a módszer paraméterként megnyitja az `Index` objektumra, amely definiálja az Azure keresési index. Létre kell hoznia egy `Index` objektum és inicializálni a következőképpen:

1. Állítsa a `Name` tulajdonsága az `Index` nevét a tárgymutató-objektumot.
2. Állítsa a `Fields` tulajdonsága az `Index` tömbje objektum `Field` objektumok. Minden egyes a `Field` objektumok határozza meg, hogy a mező viselkedésére a tárgymutatóhoz. Megadhatja, hogy a konstruktor adattípus (vagy analyzer a karakterlánc-mezők) együtt a mező nevét. Is beállíthatja, hogy más tulajdonságokat, például `IsSearchable`, `IsFilterable`stb.

Fontos folyamatosan keresés felhasználói felület és a vállalati igényei szem előtt az indexet, minden egyes tervezésekor `Field` kell-e kiosztani a [megfelelő beállításokat](https://msdn.microsoft.com/library/azure/dn798941.aspx). Melyik mezőkre vonatkozó alábbi tulajdonságok szabályozására keresése szolgáltatások (szűrés, faceting, rendezése a teljes szöveges keresési, stb.). Bármely tulajdonság nincs explicit módon beállított a `Field` alapértelmezés szerint a megfelelő keresési szolgáltatás letiltása, kivéve, ha kifejezetten engedélyezése osztály.

Példa hogy már indexben "Szállodák" nevű, és a mezőket a következők:

```csharp
var definition = new Index()
{
    Name = "hotels",
    Fields = new[]
    {
        new Field("hotelId", DataType.String)                       { IsKey = true, IsFilterable = true },
        new Field("baseRate", DataType.Double)                      { IsFilterable = true, IsSortable = true, IsFacetable = true },
        new Field("description", DataType.String)                   { IsSearchable = true },
        new Field("description_fr", AnalyzerName.FrLucene),
        new Field("hotelName", DataType.String)                     { IsSearchable = true, IsFilterable = true, IsSortable = true },
        new Field("category", DataType.String)                      { IsSearchable = true, IsFilterable = true, IsSortable = true, IsFacetable = true },
        new Field("tags", DataType.Collection(DataType.String))     { IsSearchable = true, IsFilterable = true, IsFacetable = true },
        new Field("parkingIncluded", DataType.Boolean)              { IsFilterable = true, IsFacetable = true },
        new Field("smokingAllowed", DataType.Boolean)               { IsFilterable = true, IsFacetable = true },
        new Field("lastRenovationDate", DataType.DateTimeOffset)    { IsFilterable = true, IsSortable = true, IsFacetable = true },
        new Field("rating", DataType.Int32)                         { IsFilterable = true, IsSortable = true, IsFacetable = true },
        new Field("location", DataType.GeographyPoint)              { IsFilterable = true, IsSortable = true }
    }
};
```

Azt gondosan választotta, a tulajdonság értékek minden egyes `Field` alapján, hogy megítélése szerint hogyan az alkalmazásokban használandó. Például akkor valószínű, hogy a személyek keresése szállodák kulcsszót a találatokat érdekli lesz a `description` mezőben, így azt teljes szöveges keresés engedélyezése az adott mező megadásával `IsSearchable` való `true`.

Felhívjuk a figyelmét arra, hogy pontosan egy mezőt a index típusú `DataType.String` beállításával és a _kulcs_ mező a kijelölt kell lennie `IsKey` való `true` (lásd: `hotelId` a fenti példában).

A fenti index definíció használja az egy egyéni nyelvi analyzer a `description_fr` mezőt, mert az francia nyelvű szöveget tárolására szolgál. Lásd: [a nyelvet támogatja az MSDN webhelyen témakör](https://msdn.microsoft.com/library/azure/dn879793.aspx) , valamint a megfelelő [blog közzé](https://azure.microsoft.com/blog/language-support-in-azure-search/) nyelvi gázelemzők kapcsolatban további tudnivalókat.

> [AZURE.NOTE]  Megjegyzendő, hogy megkerülhetők `AnalyzerName.FrLucene` konstruktorához, a `Field` automatikusan lesz típusú `DataType.String` fogják `IsSearchable` beállítása `true`.

## <a name="iv-create-the-index"></a>IV. Az index létrehozása
Most, hogy van egy inicializált `Index` objektum meghívásával egyszerűen hozhat létre az index `Indexes.Create` a a `SearchServiceClient` objektum:

```csharp
serviceClient.Indexes.Create(definition);
```

A sikeres kérelme módszer az általában ad vissza. A kérelmet, például az érvénytelen paraméter probléma esetén a módszerrel fog throw `CloudException`.

Miután végzett a tárgymutató, és szeretne törölni, csak hívja fel a `Indexes.Delete` módszer a `SearchServiceClient`. Például ez hogyan azt lenne a "Szállodák" index törlése:

```csharp
serviceClient.Indexes.Delete("hotels");
```

> [AZURE.NOTE] A példa kód, a jelen cikkben a szinkronizált módszerek az Azure keresési .NET SDK egyszerűség használja. Azt javasoljuk, hogy az aszinkron módszerek alkalmazásban használható a saját alkalmazások méretezhető és válaszol tartásához őket. Ha például a fenti példákban használhatja is `CreateAsync` és `DeleteAsync` helyett `Create` és `Delete`.

## <a name="next"></a>Következő
Azure keresési index létrehozása után fogja töltse fel [a tartalmat az index](search-what-is-data-import.md) készen áll arra, hogy máris az adatok keresése.
