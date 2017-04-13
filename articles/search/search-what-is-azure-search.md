<properties
    pageTitle="Mi az Azure keresési |} Microsoft Azure |} A felhőben tárolt keresési szolgáltatás"
    description="Azure keresés az egy teljes körű felügyelt szolgáltatott keresési felhőszolgáltatásba. Ez a funkció az áttekintő további."
    services="search"
    manager="jhubbard"
    authors="ashmaka"
    documentationCenter=""/>

<tags
    ms.service="search"
    ms.devlang="NA"
    ms.workload="search"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="ashmaka"/>

# <a name="what-is-azure-search"></a>Mi az Azure keresési?

Azure keresés az server és a infrastruktúra kezelése a Microsoftnak, így az adatok adataival, és elhelyezheti a Keresés a weben vagy mobilalkalmazás használatra kész szolgáltatás delegálja cloud-mint-a-keresőszolgáltatás megoldás. Azure keresési lehetővé teszi, hogy könnyen hatékony keresést felvétele az alkalmazások egyszerű [REST API -t](https://msdn.microsoft.com/library/azure/dn798935.aspx) vagy [.NET SDK](search-howto-dotnet-sdk.md) használata keresési infrastruktúra kezelése és a keresési terület szakértője valamit nélkül.

## <a name="give-your-users-a-powerful-search-experience"></a>Adjon a felhasználóknak egy hatékony keresési funkcióval

Az [Egyszerű lekérdezés szintaxisát](https://msdn.microsoft.com/library/azure/dn798920.aspx), amely logikai operátorokat, kifejezést a keresés operátorok, utótag operátorok, operátorok végrehajtási sorrendje **hatékony lekérdezések** kell kidolgozni. Zavaros keresési, a közelség keresési, a kifejezés szolgáló lehetőségekről és a reguláris kifejezésekkel [Lucene lekérdezés szintaxisa](https://msdn.microsoft.com/library/azure/mt589323.aspx) is engedélyezése. Azure keresési támogatja az egyéni lexikai gázelemzők ahhoz, hogy a reguláris kifejezések és a fonetikus egyező összetett keresési lekérdezések kezelése alkalmazás is.

**Nyelvi támogatás** [56 a különféle nyelvekhez tartozó tartalmazza](https://msdn.microsoft.com/library/azure/dn879793.aspx). Lucene gázelemzők és a Microsoft gázelemzők (az Office és a Bing feldolgozása természetes nyelvi év pontosító) használ, Azure keresési elemezheti az alkalmazás Keresés mezőbe, ezután kezelése nyelvspecifikus nyelvi elemek feldolgozása, köztük igei eseteit., gender, szabálytalan többes számú nouns (például "egér" és "egér" összehasonlítása), a word vonja összetétel, word aktuális (nyelvekhez szóközök nélkül), és más szöveget.

**Keresési javaslatok** engedélyezhető az automatikus kiegészítéses keresési sávokat, illetve javaslatokat lekérdezések. [A index tényleges dokumentumok használata javasolt](https://msdn.microsoft.com/library/azure/dn798936.aspx) felhasználóként adja meg a bemeneti részleges keresést.

**Kattintson a kiemelés** [lehetővé teszi, hogy](https://msdn.microsoft.com/library/azure/dn798927.aspx) a felhasználók lásd: szöveg minden, amely tartalmazza a találatokat a lekérdezés eredményében a kódtöredék. Is kijelölhet mely időtartammezők a kiemelt kódrészletek.

A keresési eredményeket tartalmazó lap az Azure keresés **síklapos navigációs** egyszerűen megjelenik. [Csak egyetlen lekérdezési paraméter](https://msdn.microsoft.com/library/azure/dn798927.aspx)használatával, Azure keresés minden szükséges információt összeállításához síklapos keresőmoduljának, az alkalmazás felhasználói felületén engedélyezése a felhasználóknak Lehatolás és szűrhető a Keresés eredménye (például szűrő katalógustételek ár-tartomány vagy arculatának) ad vissza.

**GEO térbeli** támogatási ezután feldolgozása, szűréséhez és megjelenítéséhez a földrajzi helyek teszi lehetővé. Azure keresési lehetővé teszi a felhasználóknak, hogy a keresési eredmény közelében adott helyre vonatkozó alapján vagy egy adott földrajzi régióban adatok feltárása. Ebből a videóból megtudhatja, hogy hogyan működik: [csatorna 9: Azure szöveg.keres és a földrajzi adatokat](https://channel9.msdn.com/Shows/Data-Exposed/Azure-Search-and-Geospatial-Data).

**Szűrők** egyszerűen síklapos navigációs részévé tenni az alkalmazás felhasználói felület, növeli a lekérdezés összetétel használható, és a szűrő alapján a felhasználó által vagy Fejlesztőeszközök-megadott feltételeknek. Az [OData-szintaxis](https://msdn.microsoft.com/library/azure/dn798921.aspx)hatékony szűrőket készíthet.

## <a name="empower-your-developers-with-an-easy-to-use-service"></a>A fejlesztők egy könnyen használható szolgáltatással kapjon.

**Magas elérhetősége** egy rendkívül megbízható keresési szolgáltatás élményt biztosítja. Amikor méretezett megfelelően [Azure keresési 99,9 % szolgáltatásiszint-szerződés kínál](https://azure.microsoft.com/support/legal/sla/search/v1_0/).

A **teljes körű felügyelt** megoldásként végpontok közötti Azure keresési szükséges nem feltétlenül infrastruktúra kezelését. A szolgáltatás egyszerűen testreszabható igényeinek megfelelően méretezéssel két dimenzióban kezelje a több dokumentum tárhely, újabb lekérdezésekkel vagy mindkettőt.

**Adatok integrálása** [Indexelő](https://msdn.microsoft.com/library/azure/dn946891.aspx) használatával lehetővé teszi, hogy automatikusan bejárja Azure SQL-adatbázis Azure DocumentDB és [Azure Blob-tárolóhoz](search-howto-indexing-azure-blob-storage.md) a keresési index az elsődleges adattár-tartalom szinkronizálása Azure keresést.

**A dokumentum repedés** , érhető el (jelenleg előzetes verzió) [olvashatók, és a tárgymutató-fő fájlformátumok](search-howto-indexing-azure-blob-storage.md) többek között a Microsoft Office, valamint a PDF-fájlt, és a HTML-dokumentumok.

**Keresés forgalom analytics** pedig [egyszerűen gyűjtött elemezni](search-traffic-analytics.md) zárolásának feloldásához mi felhasználók vannak írja be a keresőmezőbe az összefüggéseket.

**Egyszerű pontozási** egy Azure keresési kulcs előnyeit. [Profilok pontozási](https://msdn.microsoft.com/library/azure/dn798928.aspx) engedélyezni az értékek modell relevancia függvényében szervezetek maguk a dokumentumok használják. Például érdemes újabb termékek vagy diszkontált termékekkel, és a keresési eredmények előrébb jelenjen meg. Címkék használata a személyre szabott pontozási ügyfél keresési beállítások már követ, és külön-külön tárolt alapján pontozási profilok is készíthet.

**Rendezés** több mező alapján, az index séma keresztül kínált és majd állítottuk be egy keresési paraméterrel lekérdezés-időben.

**Személyhívó** szabályozásának a keresési eredmény az [egyszerű és a frappáns vezérlő](search-pagination-page-layout.md) , amely Azure keresési felajánlja a találatokra mutat.  

**Keresési ablak** lehetővé teszi a probléma lekérdezéseket az összes az indexek jobbra a fiók Azure portálról, egyszerűen tesztelése lekérdezések és finomíthatja a pontozási profilok.

## <a name="how-it-works"></a>Működése

### <a name="1-provision-service"></a>1. a szolgáltatás kiépítése
Is be az [Azure portál](https://portal.azure.com/) vagy az [Erőforrás-kezelés API Azure](https://msdn.microsoft.com/library/azure/dn832684.aspx)Azure keresőszolgáltatás léptetőnyíl vezérlőelem.

A keresési szolgáltatás beállításaitól függően vagy a szolgáltatás megosztja-e más Azure keresési előfizetők ingyenes réteg, vagy egy [réteg kifizetett](https://azure.microsoft.com/pricing/details/search/) , amely csak a szolgáltatás által használt erőforrások dedicates használni. Kiépítésekor a szolgáltatást, választhat is a régió az az adatközpont, amelyen a szolgáltatásban.

Attól függően, hogy melyik réteg szolgáltatás úgy dönt, méretezheti a szolgáltatás két dimenzióban: 1) kópiák hozzáadása a kapacitása ahhoz, hogy nehéz lekérdezésekkel kezelni, és a 2) hozzáadása a több dokumentum tár hozzáadása partíciót növekedéséhez. Dokumentum-tárhely és a lekérdezés átviteli külön-külön kezelésre, a keresési szolgáltatás az igényeinek megfelelően testre szabhatja.

### <a name="2-create-index"></a>2. a index létrehozása
A tartalom az Azure keresési szolgáltatás is feltölthet, mielőtt Azure keresési index először meg kell adnia. Index hasonlít az adatok rendelkezik, és elfogadhatja a keresési lekérdezések adatbázistáblát. A tárgymutató séma hozzárendelése a dokumentumok ki szeretne keresni, az adatbázis mezők hasonló szerkezetének megadása

Az indexek sémájának vagy létrehozhatók az Azure-portálon vagy programozás útján [a .NET SDK használ](search-howto-dotnet-sdk.md) , vagy a [REST API -t](https://msdn.microsoft.com/library/azure/dn798941.aspx). Ha az index van megadva, majd feltöltheti az adatok az Azure keresési szolgáltatás, ahol később indexelése.

### <a name="3-index-data"></a>3. adatait indexelni
Miután meghatározta a mezők és a tárgymutatóhoz attribútumait, készen áll a tartalom feltölteni az index. A leküldéses vagy ki modell feltölteni az adatokat az indexhez is használhatja.

A leküldéses modell igény szerint, vagy a ütemezett frissítések (lásd: [az indexelési műveletek (Azure keresési szolgáltatás REST API -val)](https://msdn.microsoft.com/library/azure/dn946891.aspx)), amely lehetővé teszi, hogy könnyen ingest az adatok és Azure DocumentDB, Azure SQL-adatbázissal, Azure Blob-tárolóhoz vagy SQL Server kiszolgálón egy Azure virtuális adatmódosítás konfigurálható az indexelő keresztül kapja.

A leküldéses modell SDK vagy frissített dokumentumok küldése index használható REST API-khoz keresztül kapja. Adatok az gyakorlatilag bármilyen JSON formátumú adatkészlet is leküldéses. Lásd: [hozzáadása, módosítása, és a dokumentumok törlése](https://msdn.microsoft.com/library/azure/dn798930.aspx) vagy [használatáról a .NET SDK)](search-howto-dotnet-sdk.md) adatok betöltése találhat.

### <a name="4-search"></a>4. Keresés
Miután az Azure keresési index van töltve, akkor most [probléma keresési lekérdezések](https://msdn.microsoft.com/library/azure/dn798927.aspx) , a szolgáltatás végpontjának egyszerű HTTP-kérések a REST API-használja a .NET SDK csomagjában talál.

## <a name="try-it-now-for-free"></a>Próbálja ki most (ingyenes!)
Próbálja meg Azure keresési még ma! Ha már van egy Azure-fiók, [az ingyenes réteg szolgáltatás kiépítése](search-create-service-portal.md)is.

Ha nincs próbálkozhat a nem ingyenes és 60 perces munkamenetet Azure fiók regisztráció szükséges. Nyissa meg az [Azure-alkalmazás szolgáltatás próbálja meg](http://go.microsoft.com/fwlink/p/?LinkId=618214) és jelölje be a "Web App." Válassza a "ASP.NET + Azure kereső" sablont a kezdéshez.
