<properties
    pageTitle="Adatok fel az Azure-keresés a REST API |} Microsoft Azure |} A felhőben tárolt keresési szolgáltatás"
    description="Megtudhatja, hogy miként tölthet fel adatokat a REST API Azure keresési index."
    services="search"
    documentationCenter=""
    authors="ashmaka"
    manager="jhubbard"
    editor=""
    tags=""/>

<tags
    ms.service="search"
    ms.devlang="rest-api"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="ashmaka"/>

# <a name="upload-data-to-azure-search-using-the-rest-api"></a>Feltölteni az adatokat a REST API Azure-keresés
> [AZURE.SELECTOR]
- [– Áttekintés](search-what-is-data-import.md)
- [.NET](search-import-data-dotnet.md)
- [TÖBBI](search-import-data-rest-api.md)

Ez a cikk bemutatja, hogyan az adatok importálása az Azure keresési index az [Azure keresési REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx) segítségével.

Mielőtt elkezdené az útmutató, már rendelkeznie kell [Azure keresési index létrehozása](search-what-is-an-index.md).

Annak érdekében, hogy a leküldéses dokumentumok a tárgymutatóhoz a REST API-be, HTTP POST felkérés ki lesz a tárgymutatóhoz URL-cím végpontot. HTTP összehívás törzsébe törzse a JSON-objektumra, amely a dokumentumok hozzáadása, módosítása vagy törölték.

## <a name="i-identify-your-azure-search-services-admin-api-key"></a>Lehet. Az Azure keresési szolgáltatás felügyeleti api-kulcs azonosítása
HTTP-kérések szemben a szolgáltatást a REST API kiállító, amikor *minden* API kérés tartalmaznia kell a keresési szolgáltatás, a kiépítéstől létrehozott api-billentyűt. Kérés / alapján között az alkalmazás küldése a kérést, és a szolgáltatást, amely kezeli, akkor az adatvédelmi rendelkező érvényes kulcsot hoz létre.

1. A szolgáltatás api-kulcsok keresése be kell jelentkeznie őket az [Azure-portál](https://portal.azure.com/)
2. Nyissa meg az Azure keresőszolgáltatás lap
3. Kattintson a "Billentyűparancsok" ikon

A szolgáltatás *felügyeleti* és *lekérdezés kulcsokról*fog rendelkezni.

  - Az elsődleges és másodlagos *felügyeleti kulcsok* az összes művelet, beleértve az azt jelenti, hogy a szolgáltatás kezelése, létrehozása és törlése az indexek, indexelő és adatforrások teljes engedélyek biztosítása. Két kulcsa van, hogy továbbra is a másodlagos billentyűt használja, ha úgy dönt, hogy az elsődleges kulcsot, és fordítva újragenerálása.
  - A *lekérdezés billentyűk* indexek és a dokumentumok csak olvasási hozzáférést, és a szokásos osztják meg ezt a problémát a keresési kérelem ügyfélalkalmazások.

Adatok importálása az index alkalmazásában is használhatja az elsődleges és másodlagos felügyeleti billentyűt.

## <a name="ii-decide-which-indexing-action-to-use"></a>II. Döntse el, mely az indexelési művelet használata
Amikor a REST API-t, lesz a HTTP POST kérések JSON kérelem szervezetek a hiba az Azure keresési index végpont URL-címre. A JSON-objektum, a HTTP-összehívás törzsébe is tartalmaz, amely a dokumentumok, amelyhez hozzá szeretné adni az indexet, frissítés, vagy törölje a JSON objektumokat tartalmazó "érték" nevű egyetlen JSON tömb.

A "érték" tömb JSON objektumokra indexelendő dokumentum jelöli. Az objektumok tartalmaz a dokumentum billentyűt, és adja meg a kívánt indexelési művelet (feltöltés, egyesítés, törlés, stb.). Attól függően, hogy melyik a műveletek alatti lehetőséget választja, csak bizonyos mezőket szerepelnie kell minden dokumentum esetében:

@search.action | Leírás | Az egyes dokumentumokhoz a szükséges mezőket | Jegyzetek
--- | --- | --- | ---
`upload` | Egy `upload` művelet hasonlít egy "upsert" Ha a dokumentum fog illeszti be, ha új és frissített/helyett Ha létezik. | kulcs, valamint a többi mezőt kíván definiálni | Cseréjekor frissítése vagy egy meglévő dokumentumot, minden olyan mező, amely nem szerepel a kérelem van-e meg a mezőben található `null`. Ez akkor fordul elő, akkor is, ha a mező korábban megadott nem null értékre.
`merge` | Meglévő dokumentum a megadott mezők frissítése. Ha a dokumentumot a tárgymutató nem létezik, az egyesítés sikertelen lesz. | kulcs, valamint a többi mezőt kíván definiálni | Minden mezőben adja meg, a körlevél lecseréli a meglévő mező a dokumentumban. Ide tartoznak a típusú mezők `Collection(Edm.String)`. Ha például a dokumentum tartalmaz egy mező `tags` értékkel `["budget"]` értéket az egyesítés végrehajtása és `["economy", "pool"]` az `tags`, végső értéke a `tags` mező lesz `["economy", "pool"]`. Nem lesznek `["budget", "economy", "pool"]`.
`mergeOrUpload` | Ez a művelet úgy viselkednek, mint `merge` , ha az index már létezik az adott billentyűt a dokumentumokat. Ha a dokumentum nem létezik, viselkedik `upload` új dokumentumot. | kulcs, valamint a többi mezőt kíván definiálni | -
`delete` | Az index eltávolítja a megadott dokumentumot. | csak a kulcs | Minden olyan mezők, eltérő figyelmen kívül hagyja a fő mezőben adja meg. Ha el szeretné távolítani egy önálló mező a dokumentumban lévő, `merge` ehelyett és egyszerűen mező beállítása kifejezetten null.

## <a name="iii-construct-your-http-request-and-request-body"></a>III. A HTTP-kérés és a szervezet kérése
Most, hogy a szükséges mező értékét a tárgymutató-műveletek összegyűjtötte, készen áll a tényleges HTTP-kérés és JSON összehívás törzsébe, az adatok összeállításához.

#### <a name="request-and-request-headers"></a>Kérés és a kérelem fejlécek
Az URL-cím meg kell adnia a szolgáltatás neve, az index neve (ebben az esetben a "Szállodák" jelöli), valamint a megfelelő API-verzió (az aktuális API-verzió `2015-02-28` a dokumentum közzététele idején). Meg kell adni a `Content-Type` és `api-key` kérése a fejléceket. Az utóbbi használja a szolgáltatás-rendszergazda kulcsok közül.

    POST https://[search service].search.windows.net/indexes/hotels/docs/index?api-version=2015-02-28
    Content-Type: application/json
    api-key: [admin key]

#### <a name="request-body"></a>Szervezet kérése

```JSON
{
    "value": [
        {
            "@search.action": "upload",
            "hotelId": "1",
            "baseRate": 199.0,
            "description": "Best hotel in town",
            "description_fr": "Meilleur hôtel en ville",
            "hotelName": "Fancy Stay",
            "category": "Luxury",
            "tags": ["pool", "view", "wifi", "concierge"],
            "parkingIncluded": false,
            "smokingAllowed": false,
            "lastRenovationDate": "2010-06-27T00:00:00Z",
            "rating": 5,
            "location": { "type": "Point", "coordinates": [-122.131577, 47.678581] }
        },
        {
            "@search.action": "upload",
            "hotelId": "2",
            "baseRate": 79.99,
            "description": "Cheapest hotel in town",
            "description_fr": "Hôtel le moins cher en ville",
            "hotelName": "Roach Motel",
            "category": "Budget",
            "tags": ["motel", "budget"],
            "parkingIncluded": true,
            "smokingAllowed": true,
            "lastRenovationDate": "1982-04-28T00:00:00Z",
            "rating": 1,
            "location": { "type": "Point", "coordinates": [-122.131577, 49.678581] }
        },
        {
            "@search.action": "mergeOrUpload",
            "hotelId": "3",
            "baseRate": 129.99,
            "description": "Close to town hall and the river"
        },
        {
            "@search.action": "delete",
            "hotelId": "6"
        }
    ]
}
```

Ebben az esetben azt használatával `upload`, `mergeOrUpload`, és `delete` a keresési műveletként.

Tegyük fel, hogy ez például az "Szállodák" index már kitölti a dokumentumok száma. Ne feledje, hogy hogyan azt nem kellett használata esetén, adja meg az összes lehetséges dokumentum mezőket `mergeOrUpload` , és hogyan akkor csak meg a dokumentum billentyűt (`hotelId`) használata esetén `delete`.

Megjegyzés:, hogy csak is legfeljebb 1000 dokumentumok (vagy 16 MB) egyetlen indexelési összehívásban.

## <a name="iv-understand-your-http-response-code"></a>IV. A HTTP-válasz kód ismertetése
#### <a name="200"></a>200
Sikeres indexelési kérelem elküldése után kap egy HTTP-válasz, a állapotkód `200 OK`. A HTTP-válasz JSON törzsében a következők:

```JSON
{
    "value": [
        {
            "key": "unique_key_of_document",
            "status": true,
            "errorMessage": null
        },
        ...
    ]
}
```

#### <a name="207"></a>207
Állapotkódot `207` fog adja vissza, ha nem tud indexelt legalább egy elemet. A HTTP-válasz JSON törzsében az sikertelen elutasítani információt fog tartalmazni.

```JSON
{
    "value": [
        {
            "key": "unique_key_of_document",
            "status": false,
            "errorMessage": "The search service is too busy to process this document. Please try again later."
        },
        ...
    ]
}
```

> [AZURE.NOTE] Ez általában azt jelenti, hogy a keresési szolgáltatás terhelését alkalmazáson-e a pontot, ahol kérések indexelés meg is kezdi a való visszatéréshez `503` válaszokat. Ebben az esetben hogy nagyon a érheti el, ha az ügyfél kód leállás és várakozás után újra próbálkozik. Meg fogalmat ad a rendszer egy kis időt, szeretné helyreállítani, növelése az esélye, amely a jövőbeli kérések fog sikerülni. Gyorsan újbóli próbálkozás igényeinek csak meghosszabbítani a helyzetet.

#### <a name="429"></a>429
Állapotkódot `429` visszaadandó, amikor túllépte a kvóta a dokumentumok / index számára.

#### <a name="503"></a>503
Állapotkódot `503` Ha az elemeket a kérelem egyike sem sikerült indexelt visszatér. Ez a hiba azt jelenti, hogy a rendszer nagy terhelés alatt áll, és egyelőre nem lehet feldolgozni a kérését.

> [AZURE.NOTE] Ebben az esetben hogy nagyon a érheti el, ha az ügyfél kód leállás és várakozás után újra próbálkozik. Meg fogalmat ad a rendszer egy kis időt, szeretné helyreállítani, növelése az esélye, amely a jövőbeli kérések fog sikerülni. Gyorsan újbóli próbálkozás igényeinek csak meghosszabbítani a helyzetet.

További információt a dokumentum műveletek és sikeres/hibaüzenetek című [hozzáadása, frissítése, vagy törölje dokumentumok](https://msdn.microsoft.com/library/azure/dn798930.aspx). Hiba esetén vissza kell HTTP-más állapot-kódok kapcsolatos további tudnivalókért olvassa el a [HTTP állapot kódok (Azure keresés)](https://msdn.microsoft.com/library/azure/dn798925.aspx)című témakört.

## <a name="next"></a>Következő
Az Azure keresési index feltöltése, után nem fogja készen áll a kezdésre kiállító lekérdezések dokumentumok kereséséhez. [Lekérdezés az Azure keresési Index](search-query-overview.md) talál további információt.
