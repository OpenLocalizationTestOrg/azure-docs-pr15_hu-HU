<properties
   pageTitle="Az Azure SQL-adatraktár (áttekintés) számítási power kezelése |} Microsoft Azure"
   description="Teljesítmény skála funkciók az Azure SQL-adatraktár meg. Eredményhez tartozó DWUs méretezése, vagy mutasson az egérrel, és folytassa a számítási erőforrások költségek mentéséhez."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="barbkess"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/03/2016"
   ms.author="barbkess;sonyama"/>

# <a name="manage-compute-power-in-azure-sql-data-warehouse-overview"></a>Az Azure SQL-adatraktár (áttekintés) számítási power kezelése

> [AZURE.SELECTOR]
- [– Áttekintés](sql-data-warehouse-manage-compute-overview.md)
- [Portál](sql-data-warehouse-manage-compute-portal.md)
- [A PowerShell](sql-data-warehouse-manage-compute-powershell.md)
- [TÖBBI](sql-data-warehouse-manage-compute-rest-api.md)
- [TSQL](sql-data-warehouse-manage-compute-tsql.md)

Az SQL adatraktár architektúrája elválasztja a tárhely és a számítási, minden egyes, ha át kívánja méretezni, egymástól függetlenül lehetővé. Emiatt méretezheti, csak a teljesítmény elérése érdekében fizet, szükség esetén a költségek mentése közben teljesítményét. 

Az Áttekintés SQL adatraktár alábbi teljesítmény méretezési funkciókat ismerteti, és ad arra vonatkozó javaslatokat, hogy hogyan és mikor érdemes használni. 

- Méretezés power módosításával az [adatok raktári egységek (DWUs)][] számítja ki.
- Szüneteltetheti vagy folytathatja a számítási erőforrások

<a name="scale-performance-bk"></a>

## <a name="scale-performance"></a>Méretezés teljesítmény

Az SQL-adatraktár gyorsan méretezheti, vagy újra növelésével vagy csökkentésével számítási erőforrások Processzor, memória és I/O sávszélesség teljesítmény. Ha át kívánja méretezni a teljesítményt, kell tennie mindössze módosítsa az [adatok raktári egységek (DWUs)][] , amely az adatbázishoz lefoglalja SQL adatraktár számát. SQL-adatraktár gyors végrehajtja a módosítást, és az alapul szolgáló változtatásainak hardveres és szoftveres kezeli.

Megszűnt: a nap, hol kell kutatási processzorok milyen típusú, memória vagy tároló típusának van szükség nagy teljesítmény a adatraktár a. Ha elhelyez a adatraktár a felhőben, nincs már követő hardver problémák foglalkozik. Ehelyett SQL adatraktár kéri, ez a kérdés: milyen gyorsan meg szeretné az adatok elemzése? 

### <a name="how-do-i-scale-performance"></a>Hogyan méretezheti meg a teljesítmény?

Elastically növeléséhez vagy csökkentéséhez a számítási power, egyszerűen módosítsa az [adatok raktári egységek (DWUs)][] beállítást, az adatbázis. Teljesítmény lineárisan növekszik, több DWU hozzáadásakor.  Magasabb DWU szinten kell 100-nál több DWUs figyelje meg a teljesítmény jelentős javítását szeretne hozzáadni. Szeretné kijelölni a metszéspontok értelmes DWUs, kínálunk, amely a legjobb eredményt ad DWU szintjeit.
 
Ha módosítani szeretné DWUs, egyes módszerekben használhatja.

- [Méretezés az Azure portal power számítja ki.][]
- [Méretezés PowerShell power számítja ki.][]
- [Méretezés számítási power a REST API-hoz][]
- [Méretezés a TSQL power számítja ki.][]

### <a name="how-many-dwus-should-i-use"></a>Hány DWUs érdemes használnom?
 
A teljesítmény SQL adatraktár lineárisan méretezze át, és a másodpercben megadott (mondjuk a 100 DWUs 2000 DWUs való) között egy számítási skála módosítása történik. Ez a rugalmasságot biztosít kísérletezés a különböző DWU beállításokat, mindaddig, amíg a forgatókönyv legjobb illesztés meg.

Megtudhatja, hogy mi az a ideális DWU érték, próbálja meg méretezés felfelé és lefelé, és néhány lekérdezések futtatása az adatok betöltése után. Mivel a méretezés gyors, megpróbálhatja különböző szintjeit, teljesítmény egy óra vagy annál kisebb szám. Tartsa szem előtt, hogy SQL adatraktár lett tervezve nagy mennyiségű adatot feldolgozása, illetve láthatja a méretezés igaz képességei, különösen a nagyobb értékekhez kínálunk, a fogja használni kívánt közelít, illetve meghaladja az 1 TB nagy mennyiségű adathalmaz.

Javaslatok a terhelést a legjobb DWU megkeresése:

1. Egy fejlesztés alatt álló adatraktár kezdje el kijelölésével kisszámú DWUs.  Jó kiindulási pont DW400 vagy DW200.
2. Az alkalmazás teljesítmény figyelését, és a teljesítmény erőforrásigények betartásával DWUs számának kijelölése összehasonlítása.
3. Határozza meg, hogy mennyi gyorsabban vagy lassabban teljesítmény szeretné elérni az igényeinek megfelelően az optimális teljesítmény szint feltételezve, hogy a lineáris skála kell lennie.
4. Növelje vagy csökkentse a DWUs arányában hogyan sokkal gyorsabban vagy lassabban a terhelést a végrehajtani kívánt számát. A szolgáltatás válaszol gyorsan és módosítsa a kívánt számítási erőforrások DWU új követelményeknek.
5. Továbbra is finomítások végrehajtása, amíg el nem éri az optimális teljesítmény szint az üzleti igényeinek megfelelően.

### <a name="when-should-i-scale-dwus"></a>Mikor érdemes méretezni DWUs?

Gyorsabb eredmények van szüksége, a DWUs növelése és fizet, a nagyobb teljesítmény elérése érdekében.  Ha szükséges kisebb számítja ki a power, csökkentheti a DWUs és fizetni csak akkor van szüksége. 

Mikor érdemes DWUs méretezése javaslatok:

1. Ha az alkalmazás hullámzó terhelést, skála DWU szintek felfelé vagy lefelé beleférjenek csúcsok és az alacsony pontok. Ha például a terhelést a szokásos csúcsaira a hónap végén, ha tervezi, hogy vegye fel a további DWUs e csúcs nap alatt, majd méretezze, a csúcs letelte után.
2. Mielőtt nehéz adatok betöltésének és átalakítási műveletet hajt végre, méretezze DWUs be, hogy az adatok érhető el gyorsan.

<a name="pause-compute-bk"></a>

## <a name="pause-compute"></a>Szünet számítási

[AZURE.INCLUDE [SQL Data Warehouse pause description](../../includes/sql-data-warehouse-pause-description.md)]

Mutasson egy adatbázist, használja az egyes módszerekben.

- [Szünet számítási Azure Portal segítségével][]
- [Szünet számítási szolgáltatást a PowerShell használatával][]
- [Szünet számítási a REST API-hoz][]

<a name="resume-compute-bk"></a>

## <a name="resume-compute"></a>Önéletrajz számítási

[AZURE.INCLUDE [SQL Data Warehouse resume description](../../includes/sql-data-warehouse-resume-description.md)]

Önéletrajz egy adatbázist, használja az egyes módszerekben.

- [Önéletrajz számítási Azure Portal segítségével][]
- [Önéletrajz számítási szolgáltatást a PowerShell használatával][]
- [Önéletrajz számítási a REST API-hoz][]

## <a name="permissions"></a>Engedélyek

Az adatbázis méretezés az engedélyeket az [Adatbázis módosítása][]leírt van szükség.  Szünet és az önéletrajz esetén kell az [SQL-adatbázis munkatársi][] engedéllyel, kifejezetten Microsoft.Sql/servers/databases/action.

<a name="next-steps-bk"></a>

## <a name="next-steps"></a>Következő lépések
Az alábbi cikkekben néhány további fő teljesítménymutatók fogalmak megértéséhez olvassa el:

- [Terhelést és feldolgozási kezelése][]
- [Tábla Tervező – áttekintés][]
- [Táblázat ki.][]
- [Táblázat indexelés][]
- [Táblázat szétválasztás][]
- [Táblázat statisztika][]
- [Ajánlott eljárások][]

<!--Image reference-->

<!--Article references-->
[adatok raktári egységek (DWUs)]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units

[Méretezés az Azure portal power számítja ki.]: ./sql-data-warehouse-manage-compute-portal.md#scale-compute-bk
[Méretezés PowerShell power számítja ki.]: ./sql-data-warehouse-manage-compute-powershell.md#scale-compute-bk
[Méretezés számítási power a REST API-hoz]: ./sql-data-warehouse-manage-compute-rest-api.md#scale-compute-bk
[Méretezés a TSQL power számítja ki.]: ./sql-data-warehouse-manage-compute-tsql.md#scale-compute-bk

[capacity limits]: ./sql-data-warehouse-service-capacity-limits.md

[Szünet számítási Azure Portal segítségével]:  ./sql-data-warehouse-manage-compute-portal.md#pause-compute-bk
[Szünet számítási szolgáltatást a PowerShell használatával]: ./sql-data-warehouse-manage-compute-powershell.md#pause-compute-bk
[Szünet számítási a REST API-hoz]: ./sql-data-warehouse-manage-compute-rest-api.md#pause-compute-bk

[Önéletrajz számítási Azure Portal segítségével]:  ./sql-data-warehouse-manage-compute-portal.md#resume-compute-bk
[Önéletrajz számítási szolgáltatást a PowerShell használatával]: ./sql-data-warehouse-manage-compute-powershell.md#resume-compute-bk
[Önéletrajz számítási a REST API-hoz]: ./sql-data-warehouse-manage-compute-rest-api.md#resume-compute-bk

[Terhelést és feldolgozási kezelése]: ./sql-data-warehouse-develop-concurrency.md
[Tábla Tervező – áttekintés]: ./sql-data-warehouse-tables-overview.md
[Táblázat ki.]: ./sql-data-warehouse-tables-distribute.md
[Táblázat indexelés]: ./sql-data-warehouse-tables-index.md
[Táblázat szétválasztás]: ./sql-data-warehouse-tables-partition.md
[Táblázat statisztika]: ./sql-data-warehouse-tables-statistics.md
[Ajánlott eljárások]: ./sql-data-warehouse-best-practices.md 
[development overview]: ./sql-data-warehouse-overview-develop.md

[SQL-adatbázis közös munka]: ../active-directory/role-based-access-built-in-roles.md#sql-db-contributor

<!--MSDN references-->
[ADATBÁZIS MÓDOSÍTÁSÁHOZ]: https://msdn.microsoft.com/library/mt204042.aspx

<!--Other Web references-->
[Azure portal]: http://portal.azure.com/
