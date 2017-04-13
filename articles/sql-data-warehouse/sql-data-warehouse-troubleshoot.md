<properties
   pageTitle="Azure SQL-adatraktár elhárítása |} Microsoft Azure"
   description="Az SQL Azure-adatraktár hibaelhárítás."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/30/2016"
   ms.author="sonyama;barbkess"/>

# <a name="troubleshooting-azure-sql-data-warehouse"></a>Az SQL Azure-adatraktár – hibaelhárítás

Ez a témakör felsorolja a gyakrabban hibaelhárítási kérdésekre, hogy ügyfeleink meghallgatásához.

## <a name="connecting"></a>Csatlakozás

| A probléma                              | Megoldás                                      |
| :----------------------------------| :---------------------------------------------- |
| Bejelentkezés a felhasználó "NT AUTHORITY\NÉVTELEN bejelentkezés" nem sikerült. (A Microsoft SQL Server, hiba: 18456) | Ez a hiba akkor fordul elő, ha egy AAD felhasználó próbálja meg a fő adatbázishoz való csatlakozáshoz, de a felhasználó nem rendelkezik a diaminta.  Ez a probléma megoldására vagy adja meg az SQL-adatraktár kapcsolódáskor csatlakozni, vagy vegye fel a felhasználót a fő adatbázist szeretne.  Lásd: [biztonság áttekintése][] a cikk további információt.|
|Egyszerű "sajátfelhasználónév" nem áll a kiszolgáló hozzáférhet az adatbázis "mesteralakzat" a biztonsági az aktuális környezetben. Nem tudja megnyitni a felhasználó alapértelmezett adatbázist. Sikertelen bejelentkezés. Bejelentkezés a felhasználó "Sajátfelhasználónév" nem sikerült. (A Microsoft SQL Server, hiba: 916) | Ez a hiba akkor fordul elő, ha egy AAD felhasználó próbálja meg a fő adatbázishoz való csatlakozáshoz, de a felhasználó nem rendelkezik a diaminta.  Ez a probléma megoldására vagy adja meg az SQL-adatraktár kapcsolódáskor csatlakozni, vagy vegye fel a felhasználót a fő adatbázist szeretne.  Lásd: [biztonság áttekintése][] a cikk további információt.|
| CTAIP hiba                        | Ez a hiba akkor fordulhat elő, a bejelentkezési létrehozás az SQL-kiszolgáló adatbázis, de nem az adatraktár SQL-adatbázisban.  Ha ezt a hibát, nézze meg a [biztonsági áttekintése][] cikk.  Ez a cikk bemutatja, hogyan hozhat létre egy bejelentkezési és a felhasználó létrehozása a diaminta és a felhasználók létrehozása az adatraktár SQL-adatbázisban.|
| Tűzfal blokkolja                |Azure SQL-adatbázisait kiszolgáló és az adatbázis szintű tűzfallal védett győződjön meg arról, hogy csak a hozzáférést az adatbázishoz IP-címek ismert. A tűzfalak biztonságosak alapértelmezett, ami azt jelenti, hogy engedélyeznie kell a kifejezetten és IP-cím vagy tartomány címek, a csatlakozás előtt.  A tűzfalbeállításokat hozzáférést, kövesse a [konfigurálása kiszolgáló tűzfal access-ügyfél IP-címe][] a [létesítése utasításokat][].|
| Nem tud kapcsolatba lépni eszközzel vagy illesztőprogram | SQL-adatraktár javasolja [SSMS][], [Visual Studio 2015 SSDT][]vagy [sqlcmd][] az adatok lekérdezéséhez. Illesztőprogramok és Csatlakozás SQL adatraktár, lásd: [Azure SQL-adatraktár illesztőprogramjainak][] és a [Csatlakozás az Azure SQL-adatraktár][] cikkeket.|


## <a name="tools"></a>Eszközök

| A probléma                              | Megoldás                                      |
| :----------------------------------| :---------------------------------------------- |
| Visual Studio objektum explorer AAD felhasználók hiányzik | Az ismert probléma.  Kerülő megtekintheti a felhasználók [sys.database_principals][].  Lásd: [Azure SQL-adatraktár hitelesítés][] adatraktár SQL Azure Active Directory használatáról további információt.|

## <a name="performance"></a>Teljesítmény

|  A probléma                             | Megoldás                                      |
| :----------------------------------| :---------------------------------------------- |
| Lekérdezési teljesítmény – hibaelhárítás  | Ha egy adott lekérdezés kapcsolatos hibák elhárítása, kezdje [Útmutató a Lync-a lekérdezések][].|
| Gyenge lekérdezési teljesítmény és a csomagok gyakran eredménye a hiányzó statisztika   | A gyenge teljesítményt legvalószínűbb oka hiányoznak a statisztikai adatokat a táblázatokban.  Lásd: [karbantartása táblázat statisztika] [ Statistics] megtudhatja, hogyan hozhat létre statisztika, és miért fontos, hogy a teljesítmény.|
| Alacsony feldolgozási / lekérdezések aszinkron   | Ismertetése [terhelést management][] fontos megtudhatja, hogyan memóriafoglalást egyenlege feldolgozási érdekében.|
| Ajánlott eljárások megvalósításáról.    | A legjobb helyezze megtudhatja, módokon javíthatja a lekérdezési teljesítmény [SQL adatraktár gyakorlati tanácsok][] a cikk indításához.|
| Hogyan lehet a méretezés a teljesítmény javítása érdekében  | Előfordul, hogy a megoldást a teljesítmény javítása, ha egyszerűen hozzáadhatja a további kiszámítására [Az SQL adatraktár][]méretezéssel a lekérdezések power.|
| Gyenge lekérdezés végrehajtása indexe gyenge hangminőség eredményeként | Egyes alkalommal lekérdezések sebesség csökkenése miatt [gyenge columnstore index minőségét][]is megteheti.  Ez a cikk további információt talál, és hogyan [Indexek szakasz minőségének][]újraépítéséhez.|

## <a name="system-management"></a>Rendszer kezelése

|  A probléma                             | Megoldás                                      |
| :----------------------------------| :---------------------------------------------- |
| 40847 üzenet: Nem tudta végrehajtani a műveletet, mert a kiszolgáló meghaladja a megengedett adatbázis-tranzakción egység kvótáját 45000. | A [DWU][] a létrehozni kívánt adatbázist vagy [kérése egy kvóta növelése][]vagy csökkentése|
| Vizsgálat alatt térköz kihasználtság    | Lásd: a [táblázat méretét][] a rendszer terület hasznosítása megértéséhez.|
| Súgó: táblázatok kezelése          | A [táblázat áttekintése] című témakörben találhat[ Overview] cikk segítséget kezelése a táblákat.  Ez a cikk hivatkozásokat is tartalmaz részletesebb témakörökre, például [táblázat adattípusok][Data types], [táblázat terjesztése][Distribute], [táblázat indexelés][Index], [táblázat szétválasztás][Partition], [karbantartása táblázat statisztikák] [ Statistics] és [ideiglenes táblázatok][Temporary].|

## <a name="polybase"></a>Polybase

|  A probléma                             | Megoldás                                      |
| :----------------------------------| :---------------------------------------------- |
| UTF-8 hiba                        |  Jelenleg PolyBase csak akkor támogatja, amelyeket UTF-8 kódolású adatfájlok betöltése.  Megnézheti, amelyekkel megkerülheti ezt a korlátozást találhat [körül PolyBase UTF-8 kötelező dolgozik][] .|
| Nagy sorok miatt sikertelen betöltése   | Nagy sor támogatási jelenleg Polybase számára nem érhető el.  Ez azt jelenti, hogy ha a táblázat tartalmaz, VARCHAR(MAX), NVARCHAR(MAX) vagy VARBINARY(MAX), külső táblák nem használhatók az adatok betöltéséhez.  Nagy sorok terhelések jelenleg csak Azure Data Factory (a BCP), Azure Értékáram-elemzés, SSIS, BCP vagy a .NET SQLBulkCopy osztály támogatnak. Nagy sorok PolyBase támogatása belekerül az elkövetkező kiadásokban.|
| táblázat MAX adattípusú BCP betöltés sikertelen | Van egy ismert probléma, amely megköveteli, VARCHAR(MAX), NVARCHAR(MAX) vagy VARBINARY(MAX) kerüljenek, bizonyos esetekben a tábla végén.  Próbálja meg a MAX oszlopok áthelyezése a táblázat végéhez.|

## <a name="differences-from-sql-database"></a>Különbségek az SQL-adatbázis

|  A probléma                             | Megoldás                                      |
| :----------------------------------| :---------------------------------------------- |
| Nem támogatott SQL-adatbázis-szolgáltatások  | Lásd: [nem támogatott táblázatszolgáltatások][].|
| Nem támogatott SQL-adatbázis-adattípusok  | Lásd: [nem támogatott adattípusok][].|
| DELETE és UPDATE korlátozások      | [Frissítés megoldásokat alkalmazhatja][], [törölje a lehetséges megoldások][] és [CTAS használata nem támogatott frissítési és törlési szintaxis kerülhető][]témakörben talál.  |
| EGYESÍTÉS utasítás nem támogatott   | Lásd: [EGYESÍTÉS megoldásokat alkalmazhatja][].|
| Tárolt eljárás korlátai       | Lásd: [a tárolt eljárás korlátozások][] megértéséhez, tárolt eljárások korlátozások egy része.|
| UDF nem támogatják a SELECT utasítás | Az aktuális korlátozása a UDF.  [Hozzon létre FÜGGVÉNY][] is találhat az alábbi támogatjuk.   |

## <a name="next-steps"></a>Következő lépések

Ha Ön nem lehet a fenti probléma megoldását, Íme néhány más erőforrások: próbálja meg.

- [Blogok]
- [Frissítéseiről]
- [Videók]
- [Csapat-blogok MACSKA]
- [Támogatási jegyek létrehozása]
- [Fórum az MSDN webhelyen]
- [Egymást fedő túlcsordulás fórum]
- [Twitter]

<!--Image references-->

<!--Article references-->
[Biztonsági – áttekintés]: ./sql-data-warehouse-overview-manage-security.md
[SSMS]: https://msdn.microsoft.com/library/mt238290.aspx
[Visual Studio 2015 SSDT]: ./sql-data-warehouse-install-visual-studio.md
[Adatraktár Azure SQL-illesztőprogramok]: ./sql-data-warehouse-connection-strings.md
[Csatlakozás Azure SQL-adatraktár]: ./sql-data-warehouse-connect-overview.md
[Támogatási jegyek létrehozása]: ./sql-data-warehouse-get-started-create-support-ticket.md
[Az SQL adatraktár méretezése]: ./sql-data-warehouse-manage-compute-overview.md
[DWU]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units
[a kvóta növekedés kérése]: ./sql-data-warehouse-get-started-create-support-ticket.md#request-quota-change 
[Útmutató a Lync-lekérdezések]: ./sql-data-warehouse-manage-monitor.md
[A kiépítési útmutató]: ./sql-data-warehouse-get-started-provision.md
[Ügyfél IP-címe kiszolgálói tűzfal hozzáférés konfigurálása]: ./sql-data-warehouse-get-started-provision.md#create-a-new-azure-sql-server-level-firewall
[SQL-adatraktár ajánlott eljárások]: ./sql-data-warehouse-best-practices.md
[Táblázat mérete]: ./sql-data-warehouse-tables-overview.md#table-size-queries
[Nem támogatott táblázatszolgáltatások]: ./sql-data-warehouse-tables-overview.md#unsupported-table-features
[Nem támogatott adattípusai]: ./sql-data-warehouse-tables-data-types.md#unsupported-data-types
[Overview]: ./sql-data-warehouse-tables-overview.md
[Data types]: ./sql-data-warehouse-tables-data-types.md
[Distribute]: ./sql-data-warehouse-tables-distribute.md
[Index]: ./sql-data-warehouse-tables-index.md
[Partition]: ./sql-data-warehouse-tables-partition.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md
[Temporary]: ./sql-data-warehouse-tables-temporary.md
[Gyenge columnstore index minőség]: ./sql-data-warehouse-tables-index.md#causes-of-poor-columnstore-index-quality
[A szakasz minőségének indexek újraépítéséhez]: ./sql-data-warehouse-tables-index.md#rebuilding-indexes-to-improve-segment-quality
[Terhelést kezelése]: ./sql-data-warehouse-develop-concurrency.md
[CTAS használata a nem támogatott frissítési és törlési szintaxisa]: ./sql-data-warehouse-develop-ctas.md#using-ctas-to-work-around-unsupported-features
[Frissítse a lehetséges megoldások]: ./sql-data-warehouse-develop-ctas.md#ansi-join-replacement-for-update-statements
[Megoldások törlése]: ./sql-data-warehouse-develop-ctas.md#ansi-join-replacement-for-delete-statements
[Megoldások EGYESÍTÉSE]: ./sql-data-warehouse-develop-ctas.md#replace-merge-statements
[Tárolt eljárás korlátai]: ./sql-data-warehouse-develop-stored-procedures.md#limitations
[Adatraktár Azure SQL-hitelesítés]: ./sql-data-warehouse-authentication.md
[Munka PolyBase UTF-8 kötelező körül]: ./sql-data-warehouse-load-polybase-guide.md#working-around-the-polybase-utf-8-requirement

<!--MSDN references-->
[sys.database_principals]: https://msdn.microsoft.com/library/ms187328.aspx
[HOZZON LÉTRE FÜGGVÉNY]: https://msdn.microsoft.com/library/mt203952.aspx
[Sqlcmd]: https://azure.microsoft.com/en-us/documentation/articles/sql-data-warehouse-get-started-connect-sqlcmd/

<!--Other Web references-->
[Blogok]: https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/
[Csapat-blogok MACSKA]: https://blogs.msdn.microsoft.com/sqlcat/tag/sql-dw/
[Frissítéseiről]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[Fórum az MSDN webhelyen]: https://social.msdn.microsoft.com/Forums/home?forum=AzureSQLDataWarehouse
[Egymást fedő túlcsordulás fórum]: http://stackoverflow.com/questions/tagged/azure-sqldw
[Twitter]: https://twitter.com/hashtag/SQLDW
[Videók]: https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse

