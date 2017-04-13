<properties
    pageTitle="Hogyan adatok profil adatforrásokhoz"
    description="Ennek a kiemelés között tábla - és oszlop szintű adatok profilokat, amikor regisztrál az adatforrások Azure adatkatalógusában és a profilok használatáról az adatforrások megértéséhez cikk."
    services="data-catalog"
    documentationCenter=""
    authors="spelluru"
    manager="NA"
    editor=""
    tags=""/>
<tags
    ms.service="data-catalog"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"
    ms.workload="data-catalog"
    ms.date="09/13/2016"
    ms.author="spelluru"/>

# <a name="data-profile-data-sources"></a>Adatok profil adatforrások

## <a name="introduction"></a>– Bevezetés

**Microsoft Azure adatkatalógus** egy teljes körű felügyelt felhőalapú szolgáltatásba, amely a bejegyzés és a rendszer a vállalati adatforrások feltárás szolgál. Más szóval **Azure adatkatalógus** az összes ezzel megkönnyítve a személyek Fedezze fel, megértését és adatforrásokból, és használata segítve a szervezet több értéket a meglévő adatokból. Adatforrás regisztrálása az **Azure adatkatalógus**esetén a metaadatait másolta, és a szolgáltatás által indexelve, de a szövegegység ott nem befejezése.

**Azure adatkatalógus** **Adatok adatgyűjtés** jellemzője be a támogatott adatforrások adatainak megvizsgálja, majd a statisztika és az adatok információkat gyűjti össze. Nagyon egyszerűen, amelyet fel szeretne venni egy profilt az adatok eszközök. A adatok eszköz regisztrálásakor Profilválasztás **olyan adatokat** az adatok forrása regisztrációs eszközben.

## <a name="what-is-data-profiling"></a>Mi az adatok adatgyűjtés

Adatok adatgyűjtés megvizsgálja az adatforrásban, hogy regisztrált az adatokat, és statisztika és az adatok információkat gyűjti össze. Során forrás felhőbeli adatfeltárási ezek a statisztikai adatok segíthetnek annak eldöntésében, hogy az adatokat az üzleti probléma megoldására megfelelőségét.

<!-- In [How to discover data sources](data-catalog-how-to-discover.md), you learn about **Azure Data Catalog's** extensive search capabilities including searching for data assets that have a profile. See [How to include a data profile when registering a data source](#howto). -->

A következő adatforrásokhoz támogatja az adatok adatgyűjtés:

- (Beleértve az Azure SQL-adatbázis és Azure SQL-adatraktár) SQL Server-táblák és nézetek használatával
- Az Oracle-táblák és nézetek
- Teradata-táblák és nézetek
- Táblázatok struktúra

Regisztrálás adatok eszközök segítségével a felhasználók is beleértve adatok-profilok adatforrások – köztük kapcsolatos kérdésekre választ adni:

-   Akkor használható a vállalati probléma?
-   Az adatok felel adott szabványok és mintát?
-   Melyek az adatforrás rendellenességeket?
-   Mik azok a lehetséges problémáit az adatok integrálása az alkalmazás?

> [AZURE.NOTE] Egy tárgyi eszköz, ahhoz, hogyan lehet-e a adatok integrált alkalmazásba felveheti dokumentáció is. [Dokumentum-adatforrások](data-catalog-how-to-documentation.md)tájékozódhat.


<a name="howto"/>
## <a name="how-to-include-a-data-profile-when-registering-a-data-source"></a>Hogyan adatok profil felvenni, amikor regisztrál az adatforrás

Nagyon egyszerűen, amelyet fel szeretne venni egy profilt az adatforrást. Adatforrás, az adatok forrása regisztrációs eszköz **regisztrálását objektumok** panelen regisztrálásakor a Profilválasztás párbeszédpanel **olyan adatokat**.

![](media\data-catalog-data-profile\data-catalog-register-profile.png)

Adatforrások regisztrálását kapcsolatos további információért olvassa el a [hogyan lehet rögzíteni az adatforrások](data-catalog-how-to-register.md) és [Azure adatkatalógus – első lépések](data-catalog-get-started.md)című témakört.


## <a name="filtering-on-data-assets-that-include-data-profiles"></a>Szűrés a profilok tartalmazó adatok eszközök
Adatok profil tartalmazó adatok eszközök felderítésére, is `has:tableDataProfiles` vagy `has:columnsDataProfiles` a keresési kifejezések egyikét.

> [AZURE.NOTE] **Adatok profillal együtt** az adatok forrása regisztrációs eszköz kijelölése és is magában foglalja táblázat oszlop szintű profil adatait. Az adatok katalógus API azonban lehetővé teszi, hogy csak egy része profiladatok kezdődő regisztrálását adatok eszközök.

## <a name="viewing-data-profile-information"></a>Felhasználóiprofil-adatok megtekintése

Miután megtalálta a megfelelő adatforrás azzal a profillal, az adatok profiladatok tekinthet meg. Az adatok profil megtekintése, válassza ki a adatok eszközt, és válassza a **Profil adatait** az adatkatalógusban portál ablakban.

![](media\data-catalog-data-profile\data-catalog-view.png)

**Azure** adatkatalógusában adatok profil jeleníti meg a táblázat és az oszlop profil információkat:

### <a name="object-data-profile"></a>Objektum adatok profil

-   A sorok száma
-   A táblázat mérete
-   Ha az objektum legutolsó frissítés

### <a name="column-data-profile"></a>Oszlop adatok profil

- Az oszlopok
- Különböző értékek száma
- NULL értéket tartalmazó sorok száma
- Minimum, maximum, átlag és oszlopértékek szórása

## <a name="summary"></a>Összefoglalás
Adatgyűjtés adatok statisztikai és regisztrált adatok eszközökre, hogy az üzleti problémáinak megoldása adatok megfelelőségét vonatkozó információkat tartalmaz. Jegyzetet fűz és adatforrások dokumentálás, profilok felhasználók számára betekintést ad a az adatok mélyebb megértése.


## <a name="see-also"></a>Lásd még:
-   [Hogyan lehet rögzíteni az adatforrások](data-catalog-how-to-register.md)
-   [Első lépések az Azure-adatkatalógus](data-catalog-get-started.md)
