<properties
    pageTitle="Adatbázis áttekintése egymástól |} Microsoft Azure"
    description="Megtudhatja, hogyan Nyújtás adatbázis áttelepítése az hideg adatok átlátszó és biztonságos módon a Microsoft Azure felhőbe."
    services="sql-server-stretch-database"
    documentationCenter=""
    authors="douglaslMS"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-server-stretch-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="06/27/2016"
    ms.author="douglasl"/>

# <a name="stretch-database-overview"></a>Egymástól adatbázis – áttekintés

Kitöltés adatbázis áttelepíti hideg adatok átlátszó és biztonságos módon a Microsoft Azure felhőbe.

Ha csak szeretne – első lépések Nyújtás adatbázis azonnal, az [első lépések a engedélyezése adatbázis Nyújtás varázsló futtatásával](sql-server-stretch-database-wizard.md)című.

## <a name="what-are-the-benefits-of-stretch-database"></a>Mik azok a Nyújtás adatbázisok előnyei?
Kitöltés adatbázis a következő előnyökkel jár:

### <a name="provides-cost-effective-availability-for-cold-data"></a>Itt költség\-hideg-adatok hatékony elérhetősége
Egymástól hideg- és szeretetteli tranzakció alapú adatok dinamikusan az SQL Server Microsoft Azure SQL Server Nyújtás adatbázissal. Eltérően tipikus hideg adattárolás az adatok értéke mindig online állapotban lekérdezéséhez. Akkor megadhatja, hogy hosszabb adatok adatmegőrzési ütemtervek a nagy táblázatok, például a vevői rendelés előzmények bank megszakítása nélkül. Az alacsony költség Azure helyett a méretezés költséges, a összekapcsolhatók\-tároló helyiségek. Válassza ki a árak réteg, és az Azure portál beállítási lehetőségekre költségek megőrzéséhez beállításainak konfigurálása. Szükség szerint átméretezheti a felfelé vagy lefelé. Látogassa meg [Az SQL Server Nyújtás adatbázis árak](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/) további információt.

### <a name="doesnt-require-changes-to-queries-or-applications"></a>Használatához nincs szükség az alkalmazások és a lekérdezések
Az SQL Server zökkenőmentes függetlenül attól, hogy-e az adatok eléréséhez\-helyiségek, illetve a felhőbe kiterjesztett.  Beállíthatja, hogy a házirendet, amely azt határozza meg, hol tárolja az adatokat, és SQL Server kezeli a az adatok mozgását, a háttérben. A teljes táblázatot értéke mindig online és queryable. És a Nyújtás adatbázis használatához nincs szükség a meglévő lekérdezések vagy alkalmazások módosításokat – az adatok helye teljesen átlátszó az alkalmazás.

### <a name="streamlines-on-premises-data-maintenance"></a>A egyszerűsítik\-helyiségek adatok karbantartása
Csökkentse a\-helyiségek karbantartási és az adatok tárhelyet. A biztonsági másolatok a a\-helyszíni adatok gyorsabban és a Karbantartás ablakban időszakon kívülre. Biztonsági mentés a felhőbe részének az adatok automatikusan lefut. A a\-helyileg tároló igényeinek nagyban csökken. Azure tár lehet 80 %-os kevésbé drága be hozzáadása mint\-SSD helyiségek.

### <a name="keeps-your-data-secure-even-during-migration"></a>Továbbra is az adatok biztonságos még az áttelepítés során
Külön foglalkozni örömének érdekében, mint egymástól a legfontosabb alkalmazásait biztonságosan a felhőben. Mozgás az adatokhoz titkosítást SQL Server mindig titkosított biztosít. Sor szintű biztonsági (RLS) és egyéb speciális SQL Server biztonsági szolgáltatások is dolgozhat Nyújtás adatbázist, hogy az adatok védelme.

## <a name="what-does-stretch-database-do"></a>Mit jelent a Nyújtás adatbázis?
Miután engedélyezte a Nyújtás adatbázist az SQL Server-példányt, adatbázis és legalább egy táblázat, nyújtás adatbázis meg automatikusan elkezdi hideg adatok áttelepítése az Azure.

-   Ha külön táblában hideg adatokat tárolja, áttelepítheti a teljes táblázatot.

-   Ha a táblázat hideg- és melegvíz adatokat tartalmaz, jelölje ki a sorokat, az áttelepítendő szűrő függvényt is megadhat.

**Nem kell a meglévő lekérdezések és ügyfélalkalmazások módosítása.** Továbbra is Önnek hozzáférése zökkenőmentes helyi és a távoli adatokat, még akkor is adatok áttelepítés során. A távoli lekérdezések késés kis mennyiségű, de csak tapasztal, a késés, amikor a hideg lekérdezést.

A **Nyújtás adatbázis biztosítja, hogy adatokat nem vesznek el** , ha hiba történik az áttelepítés során. Azt is újrapróbálkozási logika áttelepítés során fellépő kapcsolódási problémák kezelése. Dinamikus kezelése nézet nyújt az áttelepítés állapota.

Az **adatok áttelepítése szüneteltetheti** problémáinak megoldása a helyi kiszolgálón vagy teljes méretűre állíthatja a rendelkezésre álló hálózati sávszélesség.

![Kitöltés adatbázis – áttekintés][StretchOverviewImage1]

## <a name="is-stretch-database-for-you"></a>Az adatbázis Nyújtás?
A következő utasítások végezhet, ha Nyújtás adatbázis segíthet a követelményeknek, és a problémák megoldásában.

|Ha Ön egy döntés Maker alkalmazása|Ha Ön egy kereskedelmi|
|------------------------------|-------------------|
|Kell tranzakció alapú adatok megtartása hosszú időn keresztül.|A táblázatok méretének kapják ki a vezérlőt.|
|Időnként kell a hideg lekérdezést.|A felhasználók számára tegyük fel, hogy vágynak hideg adatokhoz való hozzáférés, de ezek csak ritkán használata.|
|Alkalmazások, beleértve a régebbi alkalmazások, amelyeket nem szeretnék frissítse-e.|Megtartása beszerzése és további tárterület hozzáadása van.|
|Szeretném táblaként menthetők pénzt tárolón találja.|Nem lehet biztonsági mentése vagy visszaállítása ilyen nagy táblázatok a SLA belül.|

## <a name="what-kind-of-databases-and-tables-are-candidates-for-stretch-database"></a>Milyen típusú adatbázisok és táblák Nyújtás adatbázis jelöltek?
Adatbázis célok tranzakció alapú adatbázisok hideg, a táblázatok kisszámú általában tárolt adatok nagy mennyiségű egymástól. Az alábbi táblázat tartalmazhat több, mint egy milliárd sorokat.

Ha az SQL Server 2016 időbeli táblázat funkció használatához Nyújtás adatbázis használata részben vagy egészben a költség táblázat társított előzmények áttelepítendő\-hatékony tároló Azure-ban. További című témakörben [Kezelése adatmegőrzési a korábbi táblában található adatok rendszer verziószámmal időbeli](https://msdn.microsoft.com/library/mt637341.aspx).

Kitöltés adatbázis tanácsadó, SQL Server 2016 észlel, szolgáltatása segítségével adatbázisok és táblák azonosítása Nyújtás adatbázis. További információt a című témakörben [azonosítása adatbázisok és táblák Nyújtás adatbázis](sql-server-stretch-database-identify-databases.md). További tudnivalók a lehetséges blokkoló, lásd: [Nyújtás adatbázis korlátozások vonatkoznak](sql-server-stretch-database-limitations.md).

## <a name="test-drive-stretch-database"></a>Kitöltés adatbázis tesztelése
**Tesztelje a AdventureWorks mintaadatbázis Nyújtás adatbázis meghajtó.** Úgy juthat az AdventureWorks mintaadatbázis, töltse le a legalább az adatbázis és a minták és parancsfájlok fájlt [Itt](https://www.microsoft.com/download/details.aspx?id=49502). A mintavállalati adatbázis visszaállítása az SQL Server 2016 egy példányát, után bontsa ki a minták fájlt, és a Nyújtás DB minták fájlra a KCS2 kitöltés mappából. A parancsfájlok ezt a fájlt, ellenőrizze, hogy mennyi helyet foglalnak előtt az adatok és adatbázis Nyújtás, a nyomon követési adatok áttelepítése, és győződjön meg arról, hogy továbbra is a meglévő adatok a lekérdezés, és közben és az új adatok beszúrása engedélyezése után adatokat az áttelepítés után futtathatók.

## <a name="next-step"></a>Következő lépés
**Azonosítsa az adatbázisok és táblák jelölt Nyújtás adatbázis.** Töltse le az SQL Server 2016 észlel, és futtassa a Nyújtás adatbázis Advisor adatbázisok és táblák Nyújtás adatbázis jelölt azonosításához. Kitöltés adatbázis Advisor is azonosítja blokkoló problémák. További információt a című témakörben [azonosítása adatbázisok és táblák Nyújtás adatbázis](sql-server-stretch-database-identify-databases.md).

<!--Image references-->
[StretchOverviewImage1]: ./media/sql-server-stretch-database-overview/StretchDBOverview.png
[StretchOverviewImage2]: ./media/sql-server-stretch-database-overview/StretchDBOverview1.png
[StretchOverviewImage3]: ./media/sql-server-stretch-database-overview/StretchDBOverview2.png
