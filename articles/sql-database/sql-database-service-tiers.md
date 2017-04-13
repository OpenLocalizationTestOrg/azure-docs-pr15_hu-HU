<properties
    pageTitle="SQL-adatbázis teljesítmény és beállítások: rétegek szolgáltatás |} Microsoft Azure"
    description="SQL-adatbázis teljesítmény és üzleti folytonosságot szolgáltatások egyenleg a költség- és videofunkcióinak szerint átméretezheti, hogy a szolgáltatás meghatározási összehasonlítása."
    keywords="adatbázis-beállítások adatbázis teljesítménye"
    services="sql-database"
    documentationCenter=""
    authors="CarlRabeler"
    manager="jhubbard"
    editor="CarlRabeler"/>

<tags
    ms.service="sql-database"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="data-management"
    ms.date="08/10/2016"
    ms.author="carlrab"/>

# <a name="sql-database-options-and-performance-understand-whats-available-in-each-service-tier"></a>SQL-adatbázis beállítások és a teljesítmény: Mi érhető el az egyes szolgáltatási réteg ismertetése

[Azure SQL-adatbázis](sql-database-technical-overview.md) kezelése eltérő munkaterhelésekből, többszintű teljesítmény három szolgáltatási rétegek kínál. Minden egyes teljesítményszint növekvő egyre nagyobb átviteli továbbít erőforrások biztosít. A saját [szolgáltatási réteg](sql-database-service-tiers.md#standalone-database-service-tiers-and-performance-levels) a saját teljesítményszint adatbázisonként kezelheti. Egy [rugalmas készlet](sql-database-service-tiers.md#elastic-pool-service-tiers-and-performance-in-edtus) megosztott erőforrásokat tartalmazó több adatbázishoz is kezelheti. Az erőforrások önálló adatbázisokhoz adatbázis-tranzakción egységek (DTUs) számát tekintve, valamint rugalmas DTUs vagy eDTUs rugalmas készletek kell megadni. Bővebben DTUs és eDTUs lásd: [Mi az a DTU](sql-database-what-is-a-dtu.md). 

Mindkét esetben a a szolgáltatási rétegek **egyszerű**, **normál**és **Premium**tartalmazzák. Adatbázis-beállítások az alábbi rétegek hasonlítanak a különálló és rugalmas készletek, de további megfontolandó szempontok rugalmas készletek. Ebben a cikkben részletes szolgáltatás meghatározási a különálló és rugalmas készletek.

## <a name="service-tiers-and-database-options"></a>Szolgáltatási rétegek és az adatbázis-beállításai
Egyszerű, szabványos, és prémium szolgáltatási rétegek minden rendelkezik egy üzemidőt 99.99 %-os SLA kínálnak kiszámíthatóan teljesítmény, rugalmas üzleti folytonosságot beállítások, biztonsági funkciókat és óránkénti számlázási. Az alábbi táblázat példákat legjobb meghatározási különböző alkalmazásokat meghatározáshoz.

| Szolgáltatási réteg | Cél munkaterhelésekből |
|---|---|
| **Egyszerű** | A legjobban támogató általában egyetlen aktív művelettel egy adott időben kis-adatbázishoz. Többek között fejlesztéshez vagy teszteléshez használt adatbázisok, vagy kisüzemi ritkán használt alkalmazásokat. |
| **Normál** | Az Ugrás a beállítást a legtöbb felhő alkalmazások esetén több egyidejű lekérdezés támogatására. Munkacsoport vagy a webes alkalmazás többek között. |
| **Támogatás** | Tervezett tranzakció alapú nagy, sok egyidejű felhasználó támogatására, és kezelésük is a legmagasabb szintű üzleti folytonosságot funkciók. Példák az adatbázisok alapvető fontosságú alkalmazások támogatására. |

>[AZURE.NOTE] Webes és üzleti kiadásában vannak lejárt. Ha webről és az üzleti kiadásai használja továbbra is szeretne, olvassa el a [Naplemente – gyakori kérdések](https://azure.microsoft.com/pricing/details/sql-database/web-business/) .

## <a name="standalone-database-service-tiers-and-performance-levels"></a>Különálló adatbázis szolgáltatási rétegek és a teljesítmény szintek
Különálló-adatbázisok minden szolgáltatási réteg belül több teljesítményszint vannak. Ha a lehetősége, hogy adja meg a szintet, amely a legjobban megfelel a terhelést igényeivel. Ha szeretne méretezni a felfelé vagy lefelé, a rétegek az adatbázis egyszerűen módosítható. [Adatbázis-szolgáltatási rétegek módosítása és a teljesítményszint](sql-database-scale-up.md) talál részleteket.

Az itt felsorolt teljesítményt nyújt [SQL-adatbázis V12](sql-database-v12-whats-new.md)használatával létrehozott adatbázisokra vonatkoznak. Adatbázisok üzemeltetett számát, függetlenül az adatbázis kap garantált erőforráskészletek, és az adatbázis várható teljesítményének jellemzőit nem érinti.

[AZURE.INCLUDE [SQL DB service tiers table](../../includes/sql-database-service-tiers-table.md)]

>[AZURE.NOTE] A szolgáltatás a fenti táblázatban szereplő összes többi sor részletes leírást olvassa el a [szolgáltatás réteg funkciók és korlátai](sql-database-performance-guidance.md#service-tier-capabilities-and-limits)című témakört.

## <a name="elastic-pool-service-tiers-and-performance-in-edtus"></a>Rugalmas készlet szolgáltatási rétegek és a teljesítmény eDTUs
Kívül létrehozásáról és méretezését önálló adatbázis, akkor is egy [rugalmas készlet](sql-database-elastic-pool.md)belül több adatbázisok kezelése lehetőséget. Az erőforrások általános egy rugalmas készlet adatbázisokra megoszthatja. A teljesítmény jellemzők kell mérni *Rugalmas adatbázis-tranzakción egységek* (eDTUs). Különálló adatbázisokkal készletek beépített részei, és a három szolgáltatási rétegek,: **egyszerű**, **normál**és **Premium**. Készletek, az alábbi három szolgáltatási rétegek továbbra is megadása az általános teljesítményt korlátai és többféle funkcióval

Adatbázis megosztása és felhasználása DTU erőforrások anélkül, hogy egy adott teljesítményszint rendelhet minden egyes készletben adatbázis készletek engedélyezése Egy szabványos készletben önálló adatbázis látogathat például 0 eDTUs az adatbázis maximális eDTU beállítása meg a készlet konfigurálásakor való használatától. Készletek több adatbázis hatékony használatához eDTU erőforrások érhető el a teljes kvótáját változó munkaterhelésekből teszi lehetővé. [Egy rugalmas készlet árát és teljesítménybeli szempontok](sql-database-elastic-pool-guidance.md) talál további információt.

Az alábbi táblázat ismerteti a készlet szolgáltatási rétegek jellemzői.

[AZURE.INCLUDE [SQL DB service tiers table for elastic pools](../../includes/sql-database-service-tiers-table-elastic-db-pools.md)]

Egy készlet belül adatbázisonként is nakhttp://go.microsoft.com/fwlink/?LinkId=311864 megfelelően igazodik a különálló adatbázis tulajdonságait, hogy a réteg számára. Például az egyszerű készlet a max folyamatokhoz korlátozott 4800-28800, erőforráskészlethez tartozik egy, de az egyes adatbázis belül egy egyszerű készlet van adatbázis legfeljebb 300 munkamenetek.

## <a name="choosing-a-service-tier"></a>A szolgáltatás szint kiválasztása

Annak megállapítása, hogy az adatbázis egy különálló adatbázis vagy egy rugalmas erőforráskészlet része legyen a szolgáltatási réteg használata mellett dönt, hogy először. 

### <a name="choosing-a-service-tier-for-a-standalone-database"></a>Egy szolgáltatási réteg önálló adatbázis kiválasztása

Annak megállapítása, hogy meg kell adnia az SQL-adatbázis edition adatbázis-szolgáltatásokra egy szolgáltatási réteg önálló adatbázis használata mellett dönt, hogy először:

- Adatbázis mérete (legfeljebb 2 GB Basic, szabványos legfeljebb 250 GB és 500 GB-os teljesítménybeli szinttől függően prémium verzióban - 1 TB legnagyobb)
- Adatbázis biztonsági Adatmegőrzés időtartama (7 nap alapszintű hitelesítés, a 35 nap szabványos, és a 35 nap prémium verzióban)

Miután megadta, hogy az SQL-adatbázis edition, készen áll a teljesítményszint az adatbázis (DTUs száma) határozza meg. Akkor tud törni majd [méretezése felfelé vagy lefelé, dinamikusan](sql-database-scale-up.md) tényleges élmény alapján. A [DTU Számológép](http://dtucalculator.azurewebsites.net/) közelítő értékét számító a szükséges DTUs számát is használhatja. 

### <a name="choosing-a-service-tier-for-an-elastic-database-pool"></a>Választás egy szolgáltatási réteg rugalmas adatbázis-készlet.

A szolgáltatás réteg rugalmas adatbázis-készlet használata mellett dönt, hogy először annak megállapítása, hogy meg kell adnia a szolgáltatási réteg a készletet az adatbázis-szolgáltatásokra.

- Az adatbázis mérete (2 GB-os Basic standard 250 GB és 500 GB prémium verzióban)
- Adatbázis biztonsági Adatmegőrzés időtartama (7 nap alapszintű hitelesítés, a 35 nap szabványos, és a 35 nap prémium verzióban)
- Adatbázisok kiszolgálónként (400 Basic 400 standard és 50 prémium verzióban) készlet száma
- Egy készlet maximális tárolási (117 GB Basic 1200 szabványos, és 750 prémium verzióban)

Miután megadta, hogy a szolgáltatási réteg a készlet, készen áll határozza meg a készlet (eDTUs) teljesítményével. Akkor tud törni, és majd [méretezheti, vagy méretezni dinamikusan](sql-database-elastic-pool-manage-portal.md#change-performance-settings-of-a-pool) tényleges tapasztalat alapján. A [DTU Számológép](http://dtucalculator.azurewebsites.net/) közelíteni egy készletben adatbázisonként segítségével megállapítható, hogy a felső határ gyűjtő szükséges DTUs számát is használhatja.

## <a name="next-steps"></a>Következő lépések
- Részletesebben az alábbi [SQL-adatbázis árak](https://azure.microsoft.com/pricing/details/sql-database/)a réteg díjazást.
- Ismerje meg, hogy [rugalmas készletek](sql-database-elastic-pool-guidance.md) és [rugalmas készletek árát és a teljesítmény szempontjai](sql-database-elastic-pool-guidance.md)részleteit.
- Ismerje meg [, kezelése és rugalmas készletek átméretezése Monitor](sql-database-elastic-pool-manage-portal.md) és [figyelése a különálló adatbázisok teljesítménye](sql-database-single-database-monitor.md).
- Most, hogy az SQL-adatbázis rétegek kapcsolatban, próbálja ki láthatóságukat [ingyenes fiókot](https://azure.microsoft.com/pricing/free-trial/) , és megtudhatja, [hogy miként hozhat létre az első SQL-adatbázis](sql-database-get-started.md).

## <a name="additional-resources"></a>További források

- [Több elem bérlői szoftver alkalmazások Azure SQL-adatbázis használata mintázatok tervezése](sql-database-design-patterns-multi-tenancy-saas-applications.md)
- [Microsoft virtuális Academy videotanfolyam a rugalmas adatbázis-funkciók az Azure SQL-adatbázis](https://mva.microsoft.com/en-US/training-courses/elastic-database-capabilities-with-azure-sql-db-16554)
