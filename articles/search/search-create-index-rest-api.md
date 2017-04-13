<properties
    pageTitle="A REST API Azure keresési index létrehozása |} Microsoft Azure |} A felhőben tárolt keresési szolgáltatás"
    description="Index létrehozása a Azure keresési HTTP REST API-kódban."
    services="search"
    documentationCenter=""
    authors="ashmaka"
    manager="jhubbard"
    editor=""
    tags="azure-portal"/>

<tags
    ms.service="search"
    ms.devlang="rest-api"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="ashmaka"/>

# <a name="create-an-azure-search-index-using-the-rest-api"></a>A REST API Azure keresési index létrehozása
> [AZURE.SELECTOR]
- [– Áttekintés](search-what-is-an-index.md)
- [Portál](search-create-index-portal.md)
- [.NET](search-create-index-dotnet.md)
- [TÖBBI](search-create-index-rest-api.md)


Ez a cikk azt ismerteti, hogy azon a folyamaton, a Azure keresési REST API-Azure keresési [index](https://msdn.microsoft.com/library/azure/dn798941.aspx) létrehozása.

Ez az útmutató következő és az index létrehozása előtt rendelkeznie kell már [létrehozott egy Azure keresési szolgáltatás](search-create-service-portal.md).

A REST API Azure keresési index létrehozásához egyetlen HTTP POST kérelem ki lesz az Azure keresési szolgáltatás URL-cím végpontot. Index definíciójának szabályos JSON tartalomként összehívás törzsében fog tartalmazni.


## <a name="i-identify-your-azure-search-services-admin-api-key"></a>Lehet. Az Azure keresési szolgáltatás felügyeleti api-kulcs azonosítása
Most, hogy az Azure keresési szolgáltatás van kiépítve, elküldheti a HTTP-kérések szemben a REST API a szolgáltatás URL-cím végpontot. Azonban *összes* API kérésében szerepelnie kell a keresési szolgáltatás, a kiépítéstől létrehozott api-billentyűt. Kérés / alapján között az alkalmazás küldése a kérést, és a szolgáltatást, amely kezeli, akkor az adatvédelmi rendelkező érvényes kulcsot hoz létre.

1. A szolgáltatás api-kulcsok keresése be kell jelentkeznie őket az [Azure-portál](https://portal.azure.com/)
2. Nyissa meg az Azure keresőszolgáltatás lap
3. Kattintson a "Billentyűparancsok" ikon

A szolgáltatás *felügyeleti* és *lekérdezés kulcsokról*fog rendelkezni.

 - Az elsődleges és másodlagos *felügyeleti kulcsok* az összes művelet, beleértve az azt jelenti, hogy a szolgáltatás kezelése, létrehozása és törlése az indexek, indexelő és adatforrások teljes engedélyek biztosítása. Két kulcsa van, hogy továbbra is a másodlagos billentyűt használja, ha úgy dönt, hogy az elsődleges kulcsot, és fordítva újragenerálása.
 - A *lekérdezés billentyűk* indexek és a dokumentumok csak olvasási hozzáférést, és a szokásos osztják meg ezt a problémát a keresési kérelem ügyfélalkalmazások.

Index létrehozása alkalmazásában is használhatja az elsődleges és másodlagos felügyeleti billentyűt.

## <a name="ii-define-your-azure-search-index-using-well-formed-json"></a>II. Az Azure keresési index szabályos JSON használatával megadása
A szolgáltatás egyetlen HTTP POST kérelem a tárgymutatóhoz hoz létre. A szervezet a HTTP-bejegyzés kérés egy JSON objektumra, amely definiálja az Azure keresési index fog tartalmazni.

1. Az első a JSON-objektum tulajdonsága az index nevét.
2. A második a JSON-objektum tulajdonsága egy névvel ellátott JSON tömböt `fields` , amely tartalmazza az index az egyes mezők egy külön JSON-objektumot. Az objektumok JSON tartalmazhat több név/érték párokká az egyes a mező attribútumok például a "név", "típus," stb.

Fontos megtartása keresés felhasználói felület és a vállalati igényei szem előtt a tárgymutatóhoz tervezésekor, minden mezőben a [megfelelő attribútumok](https://msdn.microsoft.com/library/azure/dn798941.aspx)e kiosztani. Melyik mezőkre vonatkozó alábbi attribútumok szabályozására keresése szolgáltatások (szűrés, faceting, rendezése a teljes szöveges keresési, stb.). Az alapértelmezett bármely attribútum nem ad meg, a megfelelő keresési szolgáltatás engedélyezéséhez, kivéve, ha kifejezetten letiltása lesz.

Példa hogy már indexben "Szállodák" nevű, és a mezőket a következők:

```JSON
{
    "name": "hotels",  
    "fields": [
        {"name": "hotelId", "type": "Edm.String", "key": true, "searchable": false, "sortable": false, "facetable": false},
        {"name": "baseRate", "type": "Edm.Double"},
        {"name": "description", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false},
        {"name": "description_fr", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false, "analyzer": "fr.lucene"},
        {"name": "hotelName", "type": "Edm.String", "facetable": false},
        {"name": "category", "type": "Edm.String"},
        {"name": "tags", "type": "Collection(Edm.String)"},
        {"name": "parkingIncluded", "type": "Edm.Boolean", "sortable": false},
        {"name": "smokingAllowed", "type": "Edm.Boolean", "sortable": false},
        {"name": "lastRenovationDate", "type": "Edm.DateTimeOffset"},
        {"name": "rating", "type": "Edm.Int32"},
        {"name": "location", "type": "Edm.GeographyPoint"}
    ]
}
```

Gondosan választotta, azt az index attribútumok minden egyes mezőhöz, hogy megítélése szerint hogyan az alkalmazásokban használandó alapján. Például `hotelId` van egy egyedi kulcsot, hogy a felhasználók keresve szállodák valószínűleg nem fogja tudni, így tiltjuk le a teljes szöveges keresés mező megadásával `searchable` való `false`, amely helyet menti a tárgymutató.

Felhívjuk a figyelmét arra, hogy pontosan egy mezőt a index típusú `Edm.String` és a "kulcs" mező a kijelölt kell lennie.

A fenti index definíció használja az egy egyéni nyelvi analyzer a `description_fr` mezőt, mert az francia nyelvű szöveget tárolására szolgál. Lásd: [a nyelvet támogatja az MSDN webhelyen témakör](https://msdn.microsoft.com/library/azure/dn879793.aspx) , valamint a megfelelő [blog közzé](https://azure.microsoft.com/blog/language-support-in-azure-search/) nyelvi gázelemzők kapcsolatban további tudnivalókat.

## <a name="iii-issue-the-http-request"></a>III. Probléma a HTTP-kérés
1. HTTP-bejegyzés felkérés a használja az index definíciója összehívás törzsébe, ki az Azure keresési szolgáltatás végpontjának URL-címe. Az URL-cím ne felejtse el a szolgáltatás neve használja az állomásnév, és elhelyezi a megfelelő `api-version` lekérdezési karakterlánc paraméterként (az aktuális API-verzió `2015-02-28` a dokumentum közzététele idején).
2. A kérés fejlécek, adja meg a `Content-Type` mint `application/json`. Is szüksége lesz a szolgáltatás felügyeleti kulcs, amely a i. lépés azonosította, adja meg a `api-key` fejléc.


Meg kell adnia a saját szolgáltatás neve és az api billentyűt a kérést, az alábbi hiba:


    POST https://[service name].search.windows.net/indexes?api-version=2015-02-28
    Content-Type: application/json
    api-key: [api-key]


A sikeres kérelme látnia kell a állapotkód 201 (létrehozás). A REST API keresztül index létrehozásával kapcsolatos további tudnivalókért látogasson el az API-hivatkozás [MSDN webhelyen](https://msdn.microsoft.com/library/azure/dn798941.aspx). Hiba esetén vissza kell HTTP-más állapot-kódok kapcsolatos további tudnivalókért olvassa el a [HTTP állapot kódok (Azure keresés)](https://msdn.microsoft.com/library/azure/dn798925.aspx)című témakört.

Miután végzett a tárgymutató, és szeretne törölni, csak hiba HTTP törlése kérelmet. Például ez hogyan azt lenne a "Szállodák" index törlése:

    DELETE https://[service name].search.windows.net/indexes/hotels?api-version=2015-02-28
    api-key: [api-key]


## <a name="next"></a>Következő
Azure keresési index létrehozása után fogja töltse fel [a tartalmat az index](search-what-is-data-import.md) készen áll arra, hogy máris az adatok keresése.
