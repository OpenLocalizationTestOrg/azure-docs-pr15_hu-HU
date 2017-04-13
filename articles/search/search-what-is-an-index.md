<properties
    pageTitle="Azure keresési index létrehozása |} Microsoft Azure |} A felhőben tárolt keresési szolgáltatás"
    description="Mi az Azure keresési index és a használata?"
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

# <a name="create-an-azure-search-index"></a>Azure keresési index létrehozása
> [AZURE.SELECTOR]
- [– Áttekintés](search-what-is-an-index.md)
- [Portál](search-create-index-portal.md)
- [.NET](search-create-index-dotnet.md)
- [TÖBBI](search-create-index-rest-api.md)

## <a name="what-is-an-index"></a>Mi az index?

*Index* a *dokumentumok* és más tartalmazza az Azure keresési szolgáltatás által használt állandó áruházból. Dokumentum a tárgymutató-kereshető adatok egyetlen egységet. Például egy e-kereskedelmi viszonteladótól előfordulhat, hogy a dokumentumokban a minden elem értékesítik, előfordulhat, hogy egy hírek szervezet a dokumentumokban a minden cikk stb. E fogalmak megfeleltetése ismerősebb adatbázis egymásnak megfelelő elemei: *index* adatbázistáblákhoz hasonló egy *táblázatot*, és *dokumentumok* nagyjából megegyeznek a tábla *sorainak* .

Miután dokumentumok hozzáadása/feltöltése és Azure keresés keresési lekérdezések elküldése, a keresési szolgáltatás a részvételi egyedi index létrehozása céljából.

## <a name="field-types-and-attributes-in-an-azure-search-index"></a>Mezőtípusok és Azure keresési index attribútumok

A séma megadásakor, meg kell adnia nevét, írja be, és az egyes mezők attribútumainak a index. A mező típusa sorolja a mezőben tárolt adatokhoz. Attribútumok vannak beállítva az egyes mezőket adja meg, hogyan a mezővel. Az alábbi táblázatokban számba veheti a webhely típusa és attribútumok adhatja meg.


### <a name="field-types"></a>Mezőtípusok
|Típus|Leírás|
|------------|-----------|
|*Edm.String*|Szöveg, amely segítségével tetszés szerint tokenekre bontott teljes szöveges keresés (word aktuális, amelynek stb).|
|*Collection(EDM.String)*|Karakterláncok, amelyek segítségével tetszés szerint tokenekre bontott teljes szöveges keresés listája. Nem elméleti felső korlátozott gyűjtemény elemeinek száma, de a tartalom mérete 16 MB felső határ gyűjtemények vonatkozik.|
|*Edm.Boolean*|IGAZ vagy hamis értéket tartalmaz.|
|*Edm.Int32*|a 32 bites egész értékeket.|
|*Edm.Int64*|64 bites egész értékeket.|
|*Edm.Double*|Dupla pontosságú numerikus adatokat.|
|*Edm.DateTimeOffset*|Dátum: az időértékeket a OData V4 formátumban (például `yyyy-MM-ddTHH:mm:ss.fffZ` vagy `yyyy-MM-ddTHH:mm:ss.fff[+/-]HH:mm`).|
|*Edm.GeographyPoint*|Földrajzi tetszőleges pontjára a földgömb, amely pont.|

További információt az Azure keresési [támogatott adattípusok az MSDN webhelyen](https://msdn.microsoft.com/library/azure/dn798938.aspx)talál.



### <a name="field-attributes"></a>A mező attribútumok
|Attribútum|Leírás|
|------------|-----------|
|*Kulcs*|Egy karakterlánc, amely tartalmaz minden dokumentum esetén dokumentum keresse meg a használt egyedi azonosítója. Minden index egy kulcsot kell rendelkeznie. Csak egy mezőt a billentyűt, és típusának megfelelő Edm.String kell állítani.|
|*Beolvasható*|Itt adhatja meg, hogy egy mezőt a keresési eredmény adhatók vissza.|
|*Szűrhető*|Lehetővé teszi, hogy a mező a szűrő lekérdezésekben használható.|
|*Rendezhető*|Lehetővé teszi, hogy ez a mező használata a keresési találatok rendezése lekérdezés.|
|*Facetable*|Lehetővé teszi, hogy egy önálló Irányított szűrés [síklapos navigációs](search-faceted-navigation.md) struktúrában használandó mezőt. A szokásos több dokumentum egy csoportba használható ismétlődő értékeket tartalmazó mezők (például több dokumentum that fall under egy egyetlen arculatának vagy szolgáltatás kategóriába tartozó) működnek a legjobban, metszettel.|
|*Kereshető*|Megjelöli a mező, a teljes szöveges kereshető.|

További információt az Azure keresési [index attribútumok MSDN webhelyen](https://msdn.microsoft.com/library/azure/dn798941.aspx)talál.



## <a name="guidance-for-defining-an-index-schema"></a>Útmutató a definiálásáról az index séma

Tervezésekor az indexet, hogy a tervezési fázis gondolja, hogy minden döntés keresztül az Ön ideje. Fontos megtartása keresés felhasználói felület és a vállalati igényei szem előtt a tárgymutatóhoz tervezésekor, minden mezőben a [megfelelő attribútumok](https://msdn.microsoft.com/library/azure/dn798941.aspx)e kiosztani. Index módosítása a telepítés után Újraépítés, és az adatok újbóli betöltése jelenti.


Tárhelyre adatok időbeli változását, ha növelheti vagy csökkentheti a kapacitás hozzáadásával és eltávolításával partíciót. A részletekért olvassa [kezelése a keresési szolgáltatás Azure-ban](search-manage.md) vagy a [Szolgáltatás korlátai](search-limits-quotas-capacity.md).
