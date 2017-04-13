<properties
    pageTitle="Fiddler használata felmérése, és Azure keresési REST API teszteléséhez |} Microsoft Azure |} A felhőben tárolt keresési szolgáltatás"
    description="Azure keresési elérhetőségének ellenőrzése és a REST API-k ki szeretné próbálni kód ingyenes megközelítése Fiddler használatával."
    services="search"
    documentationCenter=""
    authors="HeidiSteen"
    manager="mblythe"
    editor=""/>

<tags
    ms.service="search"
    ms.devlang="rest-api"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="10/17/2016"
    ms.author="heidist"/>

# <a name="use-fiddler-to-evaluate-and-test-azure-search-rest-apis"></a>Használja a Fiddler felmérése, és Azure keresési REST API teszteléséhez
> [AZURE.SELECTOR]
- [– Áttekintés](search-query-overview.md)
- [Keresési ablak](search-explorer.md)
- [Fiddler](search-fiddler.md)
- [.NET](search-query-dotnet.md)
- [TÖBBI](search-query-rest-api.md)

Ez a cikk ismerteti, hogyan Fiddler [Telerik ingyenesen letölthető](http://www.telerik.com/fiddler), a probléma HTTP-kérelmek és Azure keresési REST API segítségével anélkül, hogy minden olyan kódírás válaszok megtekintése használni. Azure keresési teljesen-kezelik, felhőalapú keresési szolgáltatás a Microsoft Azure, egyszerűen programozható .NET és a REST API-khoz is. Az Azure keresési szolgáltatás REST API-khoz [MSDN](https://msdn.microsoft.com/library/azure/dn798935.aspx)ismerteti.

A következőképpen, fog index létrehozása, dokumentumok feltöltése, a lekérdezés az index és majd a szolgáltatás információt a rendszer lekérdezés.

A lépések elvégzéséhez szüksége lesz az Azure keresési szolgáltatás és `api-key`. Lásd: a [létrehozása az Azure keresési szolgáltatás a portálon](search-create-service-portal.md) kapcsolatos tudnivalókat a kezdéshez.

## <a name="create-an-index"></a>Tárgymutató létrehozása

1. Indítsa el a Fiddler. Kattintson a **fájl** menü kapcsolja ki a **Forgalom rögzítése** , amely független ezt a feladatot a felesleges HTTP-tevékenység elrejtése.

3. A **Zeneszerző** lapon, amely hasonlít az alábbi képernyőfelvételen kérelem megfogalmazásához van fogja.

    ![][1]

2. Jelölje ki az **ELHELYEZNI**.

3. Adjon meg egy URL-címet, amely meghatározza a szolgáltatás URL-CÍMÉT, attribútumok és az api-verzió. Néhány fontos adatokra, amelyre érdemes figyelni:
   + Használja az HTTPS előtagot.
   + Kérés attribútum "/ indexek/szállodák". Ez jelzi, hogy a keresés "Szállodák" nevű index létrehozása gombra.
   + API-verzió pedig kisbetű, megadott "? api-verzió 2015-02-28 =". API-verziók fontosak, mivel az Azure keresési üzembe helyezése frissítések rendszeresen. Ritka esetekben a szolgáltatás frissítését az API-nak vezethetnek szakítószilárdságának módosítás. Emiatt Azure kereséshez api-verziójú minden kérésének megfelelően, hogy Ön a teljes hozzáférés fölé melyiket használja.

    A teljes URL-CÍMÉT az alábbi példa hasonlóan kell kinéznie.

            https://my-app.search.windows.net/indexes/hotels?api-version=2015-02-28

4.  Adja meg a kérelem fejléc, a host és az api-kulcs helyettesítve a szolgáltatásbeli érvényes értékeket.

            User-Agent: Fiddler
            host: my-app.search.windows.net
            content-type: application/json
            api-key: 1111222233334444

5.  Illessze be a mezők indexet meghatározása alkotó összehívás törzsébe.
            
             {
            "name": "hotels",  
            "fields": [
              {"name": "hotelId", "type": "Edm.String", "key":true, "searchable": false},
              {"name": "baseRate", "type": "Edm.Double"},
              {"name": "description", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false},
              {"name": "hotelName", "type": "Edm.String"},
              {"name": "category", "type": "Edm.String"},
              {"name": "tags", "type": "Collection(Edm.String)"},
              {"name": "parkingIncluded", "type": "Edm.Boolean"},
              {"name": "smokingAllowed", "type": "Edm.Boolean"},
              {"name": "lastRenovationDate", "type": "Edm.DateTimeOffset"},
              {"name": "rating", "type": "Edm.Int32"},
              {"name": "location", "type": "Edm.GeographyPoint"}
             ]
            }

6.  Kattintson a **végrehajtása**.

Néhány másodperc alatt meg kell jelennie a munkamenet listájában HTTP 201 választ jelezve, hogy az indexelés megtörtént.

Ha a HTTP 504, ellenőrizze az URL-címet adja meg a HTTPS. Ha megjelenik a HTTP-400 és 404-es, jelölje be az összehívás törzsébe ellenőrizze, hogy nincs másolás, Beillesztés hiba történt. Egy HTTP 403 általában az api-termékkulccsal (érvénytelen kulcsot vagy szintaxis probléma hogyan az api-kulcs meg van adva) hibát jelez.

## <a name="load-documents"></a>Dokumentumok betöltése

A **Zeneszerző** lap kérését dokumentumait elküldheti fog megjelenni a következő. A kérelem törzsében 4 szállodák keresési adatait tartalmazza.

   ![][2]

1. Kattintson a **Közzététel**.

2.  Írja be az URL-cím kezdődő HTTPS, majd a szolgáltatás URL-CÍMÉT, valamint az utána "/indexes/ < index_neve >/dokumentumok/index? api-verzió 2015-02-28 =". A teljes URL-CÍMÉT az alábbi példa hasonlóan kell kinéznie.

            https://my-app.search.windows.net/indexes/hotels/docs/index?api-version=2015-02-28

3.  Kérés fejléce ugyanaz, mint előtt kell lennie. Ne feledje, hogy Ön cseréli a host és az api-kulcs értékek, amelyek a szolgáltatásbeli érvényesek.

            User-Agent: Fiddler
            host: my-app.search.windows.net
            content-type: application/json
            api-key: 1111222233334444

4.  A kérés szöveg tartalmazza a felvenni kívánt az szállodák index négy dokumentumok.

            {
            "value": [
            {
                "@search.action": "upload",
                "hotelId": "1",
                "baseRate": 199.0,
                "description": "Best hotel in town",
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
                "@search.action": "upload",
                "hotelId": "3",
                "baseRate": 279.99,
                "description": "Surprisingly expensive",
                "hotelName": "Dew Drop Inn",
                "category": "Bed and Breakfast",
                "tags": ["charming", "quaint"],
                "parkingIncluded": true,
                "smokingAllowed": false,
                "lastRenovationDate": null,
                "rating": 4,
                "location": { "type": "Point", "coordinates": [-122.33207, 47.60621] }
              },
              {
                "@search.action": "upload",
                "hotelId": "4",
                "baseRate": 220.00,
                "description": "This could be the one",
                "hotelName": "A Hotel for Everyone",
                "category": "Basic hotel",
                "tags": ["pool", "wifi"],
                "parkingIncluded": true,
                "smokingAllowed": false,
                "lastRenovationDate": null,
                "rating": 4,
                "location": { "type": "Point", "coordinates": [-122.12151, 47.67399] }
              }
             ]
            }

8.  Kattintson a **végrehajtása**.

Néhány másodpercig megjelenik egy HTTP 200 válasz, a munkamenet listájában. Ez azt jelzi, hogy a dokumentumok sikeresen megtörtént. Ha egy 207, legalább egy dokumentum feltöltése nem sikerült. Ha egy 404, ha az élőfej vagy az összehívás törzsében szintaktikai hiba.

## <a name="query-the-index"></a>Lekérdezés az index

Most, hogy az index és a dokumentumok betöltött, elküldheti őket lekérdezéseket.  A **Zeneszerző** lapon a szolgáltatást **első** parancs az alábbi képernyőfelvételen hasonlóan fog megjelenni.

   ![][3]

1.  Jelölje ki az **első**.

2.  Írja be az URL-cím kezdődő HTTPS, majd a szolgáltatás URL-CÍMÉT, valamint az utána "/indexes/ < index_neve > / dokumentumok?", a lekérdezés paraméterei követ. Példaként használja az alábbi URL-címet, a minta állomásnév cseréje egy érvényes a szolgáltatásban.
    
            https://my-app.search.windows.net/indexes/hotels/docs?search=motel&facet=category&facet=rating,values:1|2|3|4|5&api-version=2015-02-28

    Ezt a lekérdezést a "amelyben" kifejezést a Keresés és vissza minősítések dimenzió kategóriákat.

3.  Kérés fejléce ugyanaz, mint előtt kell lennie. Ne feledje, hogy Ön cseréli a host és az api-kulcs értékek, amelyek a szolgáltatásbeli érvényesek.

            User-Agent: Fiddler
            host: my-app.search.windows.net
            content-type: application/json
            api-key: 1111222233334444

A válasz kódot kell 200, és a válasz eredménye az alábbi képernyőfelvételen hasonlóan kell kinéznie.

   ![][4]

A következő példa lekérdezés a [keresési Index művelet (Azure keresési API)](http://msdn.microsoft.com/library/dn798927.aspx) MSDN származik. Ez a témakör a példa lekérdezések számos olyan szóközök, amelyek nem használhatók a Fiddler. A csere minden szóköz a + karaktert a lekérdezés beillesztésével karakterlánc, mielőtt megnyitná a lekérdezés Fiddler előtt.

**Mielőtt a szóköz helyett jelennek meg:**

        GET /indexes/hotels/docs?search=*&$orderby=lastRenovationDate desc&api-version=2015-02-28

**Szóköz cseréli, amelyek után +:**

        GET /indexes/hotels/docs?search=*&$orderby=lastRenovationDate+desc&api-version=2015-02-28

## <a name="query-the-system"></a>A rendszer a lekérdezés

A rendszer a dokumentum száma és a tárterület-felhasználás is is lekérdezhetők. A **Zeneszerző** lapon a kérelem fog kinézni a következőhöz hasonló, és a válasz a száma, a dokumentumok és területből felhasznált számú ad vissza.

 ![][5]

1.  Jelölje ki az **első**.

2.  Egy URL-címet, amely tartalmazza a szolgáltatás URL-CÍMÉT, majd adja meg "/ indexek/szállodák/stat? api-verzió 2015-02-28 =":

            https://my-app.search.windows.net/indexes/hotels/stats?api-version=2015-02-28

3.  Adja meg a kérelem fejléc, a host és az api-kulcs helyettesítve a szolgáltatásbeli érvényes értékeket.

            User-Agent: Fiddler
            host: my-app.search.windows.net
            content-type: application/json
            api-key: 1111222233334444

4.  Hagyja üresen az összehívás törzsébe.

5.  Kattintson a **végrehajtása**. Meg kell jelennie egy HTTP 200 állapotkód a munkamenet listájában. Jelölje ki a bejegyzést, a parancs közzétett.

6.  Kattintson a **Felügyelőkben** fülre, kattintson a **fejléc** fülre, és válassza ki a JSON formátum. Meg kell jelennie a dokumentum darab és a tároló méretét (KB).

## <a name="next-steps"></a>Következő lépések

Lásd: [Azure a keresési szolgáltatás kezelése](search-manage.md) a kódmentes kezelését és Azure keresőfunkcióval megközelíthető.


<!--Image References-->
[1]: ./media/search-fiddler/AzureSearch_Fiddler1_PutIndex.png
[2]: ./media/search-fiddler/AzureSearch_Fiddler2_PostDocs.png
[3]: ./media/search-fiddler/AzureSearch_Fiddler3_Query.png
[4]: ./media/search-fiddler/AzureSearch_Fiddler4_QueryResults.png
[5]: ./media/search-fiddler/AzureSearch_Fiddler5_QueryStats.png
