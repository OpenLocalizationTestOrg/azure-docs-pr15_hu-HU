<properties
    pageTitle="A lekérdezés a Azure keresési Index a REST API |} Microsoft Azure |} A felhőben tárolt keresési szolgáltatás"
    description="Azure keresés keresési lekérdezés összeállítása és a keresési találatok szűrés és rendezés paramétereivel."
    services="search"
    documentationCenter=""
    manager="jhubbard"
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

# <a name="query-your-azure-search-index-using-the-rest-api"></a>A lekérdezés a Azure keresési index, a REST API
> [AZURE.SELECTOR]
- [– Áttekintés](search-query-overview.md)
- [Portál](search-explorer.md)
- [.NET](search-query-dotnet.md)
- [TÖBBI](search-query-rest-api.md)

Ez a cikk bemutatja, hogyan a [Azure keresési REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx)index lekérdezéséhez.

Mielőtt elkezdené az útmutató, már rendelkeznie kell [Azure keresési index létrehozása](search-what-is-an-index.md) és [töltve adatokkal](search-what-is-data-import.md).

## <a name="i-identify-your-azure-search-services-query-api-key"></a>Lehet. Az Azure keresőszolgáltatás lekérdezés api-kulcs azonosítása
A minden keresési művelet szemben az Azure keresési REST API fő összetevője *api-billentyűt* , a szolgáltatás, a kiépítéstől létrehozott. Kérés / alapján között az alkalmazás küldése a kérést, és a szolgáltatást, amely kezeli, akkor az adatvédelmi rendelkező érvényes kulcsot hoz létre.

1. A szolgáltatás api-kulcsok keresése be kell jelentkeznie őket az [Azure-portál](https://portal.azure.com/)
2. Nyissa meg az Azure keresőszolgáltatás lap
3. Kattintson a "Billentyűparancsok" ikon

A szolgáltatás *felügyeleti* és *lekérdezés kulcsokról*fog rendelkezni.

 - Az elsődleges és másodlagos *felügyeleti kulcsok* az összes művelet, beleértve az azt jelenti, hogy a szolgáltatás kezelése, létrehozása és törlése az indexek, indexelő és adatforrások teljes engedélyek biztosítása. Két kulcsa van, hogy továbbra is a másodlagos billentyűt használja, ha úgy dönt, hogy az elsődleges kulcsot, és fordítva újragenerálása.
 - A *lekérdezés billentyűk* indexek és a dokumentumok csak olvasási hozzáférést, és a szokásos osztják meg ezt a problémát a keresési kérelem ügyfélalkalmazások.

Index lekérdezése alkalmazásában is használhatja a lekérdezés kulcsok közül. A felügyeleti billentyűk lekérdezések is használható, de kell használnia a lekérdezés kulcs alkalmazás kódban jobban követi a [legalacsonyabb jogosultság elve](https://en.wikipedia.org/wiki/Principle_of_least_privilege)szerint.

## <a name="ii-formulate-your-query"></a>II. A lekérdezés megfogalmazásához
Két módon [felszabadításával a REST API -t használja](https://msdn.microsoft.com/library/azure/dn798927.aspx). Egy úgy, hogy HTTP POST felkérés hiba, ahol a lekérdezés paraméterei határozza meg a JSON-objektum összehívás törzsébe. A más úgy, hogy ki egy HTTP GET kérés, ahol a lekérdezés paramétereit határozza meg belül a kérelem URL-címe. Ne feledje, hogy bejegyzés további [korlátozások enyhíteni](https://msdn.microsoft.com/library/azure/dn798927.aspx) a lekérdezés paraméterei GET-nál méretét. Emiatt azt javasoljuk használata a BEJEGYZÉST, kivéve, ha rendelkezik különleges körülmények hol GET használatával lenne kényelmesebb.

POST és a GET-meg kell adnia a *szolgáltatás neve*, *index neve*és a TNÉV *API-verziót* (az aktuális API-verzió `2015-02-28` a dokumentum közzététele idején) kérelem URL-címét. GET a *lekérdezési karakterlánc* végén található az URL-cím lesz a lekérdezés paraméterei adni. Lásd az alábbi az URL-formátuma:

    https://[service name].search.windows.net/indexes/[index name]/docs?[query string]&api-version=2015-02-28

KÜLDÉS formátuma az azonos, de a verziójával csak api-a lekérdezési karakterlánc paraméterek.



#### <a name="example-queries"></a>Példa lekérdezések

Íme néhány példa lekérdezések a "Szállodák" nevű index. Ezek a lekérdezések GET és a bejegyzés formátumban jelennek meg.

A teljes indexben keresés a "költségvetést" kifejezés, és térjen csak a `hotelName` mező:

```
GET https://[service name].search.windows.net/indexes/hotels/docs?search=budget&$select=hotelName&api-version=2015-02-28

POST https://[service name].search.windows.net/indexes/hotels/docs/search?api-version=2015-02-28
{
    "search": "budget",
    "select": "hotelName"
}
```

Szűrő alkalmazása az index szállodák éjszakai 150 $ olcsóbbak megkeresése, és vissza az `hotelId` és `description`:

```
GET https://[service name].search.windows.net/indexes/hotels/docs?search=*&$filter=baseRate lt 150&$select=hotelId,description&api-version=2015-02-28

POST https://[service name].search.windows.net/indexes/hotels/docs/search?api-version=2015-02-28
{
    "search": "*",
    "filter": "baseRate lt 150",
    "select": "hotelId,description"
}
```

Keresse meg a teljes, egy adott mező sorrend (`lastRenovationDate`) csökkenő sorrendben, hogy várnia kell a tetején két eredményt, valamint a csak `hotelName` és `lastRenovationDate`:

```
GET https://[service name].search.windows.net/indexes/hotels/docs?search=*&$top=2&$orderby=lastRenovationDate desc&$select=hotelName,lastRenovationDate&api-version=2015-02-28

POST https://[service name].search.windows.net/indexes/hotels/docs/search?api-version=2015-02-28
{
    "search": "*",
    "orderby": "lastRenovationDate desc",
    "select": "hotelName,lastRenovationDate",
    "top": 2
}
```

## <a name="iii-submit-your-http-request"></a>III. A HTTP kérelem
Most, hogy a lekérdezés a HTTP kérelem URL-címe (számára beolvasása) vagy a szöveg (-bejegyzés) részeként van megfogalmazott, meghatározása a kérelem fejlécek, és a lekérdezés elküldése.

#### <a name="request-and-request-headers"></a>Kérés és a kérelem fejlécek
Két kérelem fejlécek meg kell adni a GET vagy három bejegyzés.
1. A `api-key` élőfej kell beállítani a megtalált lépésben lehet a fenti lekérdezés billentyűt. Figyelje meg, hogy egy rendszergazda kulcs megjelölése is használhatja a `api-key` fejléc, de ajánlott egy lekérdezés billentyűt használja, akkor kizárólag hozzáférést biztosít a csak olvasható indexek és a dokumentumokat.
2. A `Accept` meg kell élőfej `application/json`.
3. Bejegyzés csak a `Content-Type` élőfej is állíthatók be `application/json`.

Lásd az alábbi HTTP GET kérés keresse meg a "Szállodák" a Azure keresési REST API, egyszerű lekérdezést, amely a Keresés a "amelyben" kifejezés használatával:

```
GET https://[service name].search.windows.net/indexes/hotels/docs?search=motel&api-version=2015-02-28
Accept: application/json
api-key: [query key]
```

Íme egy példa a lekérdezésre ezúttal HTTP POST:

```
POST https://[service name].search.windows.net/indexes/hotels/docs/search?api-version=2015-02-28
Content-Type: application/json
Accept: application/json
api-key: [query key]

{
    "search": "motel"
}
```

A sikeres kérést állapotkódot eredményezi `200 OK` és a keresési eredmények között a visszaadott JSON válasz törzsébe. Az alábbiakban milyen az eredmények a fenti lekérdezés néznek, feltéve, hogy a "Szállodák" index kitölti a mintaadatokat az [Adatok importálása az Azure-keresés a REST API](search-import-data-rest-api.md) (figyelje meg, hogy a JSON áttekinthetővé formátumú).

```JSON
{
    "value": [
        {
            "@search.score": 0.59600675,
            "hotelId": "2",
            "baseRate": 79.99,
            "description": "Cheapest hotel in town",
            "description_fr": "Hôtel le moins cher en ville",
            "hotelName": "Roach Motel",
            "category": "Budget",
            "tags":["motel", "budget"],
            "parkingIncluded": true,
            "smokingAllowed": true,
            "lastRenovationDate": "1982-04-28T00:00:00Z",
            "rating": 1,
            "location": {
                "type": "Point",
                "coordinates": [-122.131577, 49.678581],
                "crs": {
                    "type":"name",
                    "properties": {
                        "name": "EPSG:4326"
                    }
                }
            }
        }
    ]
}
```

További információért látogasson el a [Dokumentumok keresése](https://msdn.microsoft.com/library/azure/dn798927.aspx)"Választ" című szakaszát. Hiba esetén vissza kell HTTP-más állapot-kódok kapcsolatos további tudnivalókért olvassa el a [HTTP állapot kódok (Azure keresés)](https://msdn.microsoft.com/library/azure/dn798925.aspx)című témakört.
