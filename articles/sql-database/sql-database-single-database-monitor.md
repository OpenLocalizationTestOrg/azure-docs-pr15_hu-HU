<properties
    pageTitle="Azure SQL-adatbázisban adatbázis teljesítmény figyelése |} Microsoft Azure"
    description="Tudjon meg többet az adatbázis Azure eszközök és dinamikus kezelése nézetek figyelemmel kísérésére a beállításokat."
    keywords="adatbázis felhő adatbázis teljesítmény figyelése"
    services="sql-database"
    documentationCenter=""
    authors="CarlRabeler"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="data-management"
    ms.date="09/27/2016"
    ms.author="carlrab"/>

# <a name="monitoring-database-performance-in-azure-sql-database"></a>Azure SQL-adatbázisban adatbázis teljesítmény figyelése
Az Azure SQL-adatbázishoz teljesítményének figyelése felügyeletet igényel, az erőforrás-kihasználtság viszonyított adatbázis teljesítményének kiválaszthatja, hogy a szintjét kezdődik. Figyelés elemzéssel könnyebben megállapítható, hogy az adatbázis fölösleges kapacitása, vagy van problémát tapasztal, mert erőforrások vannak maximumot, majd döntse el, hogy-e a teljesítményszint és [szolgáltatási réteg](sql-database-service-tiers.md) az adatbázis idő. Az adatbázis grafikus eszközök az [Azure portálon](https://portal.azure.com) vagy a használata SQL- [kezelés dinamikus nézetek](https://msdn.microsoft.com/library/ms188754.aspx)figyelheti meg.

## <a name="monitor-databases-using-the-azure-portal"></a>Lync-adatbázisok az Azure portál használatával

Jelölje ki az adatbázist, és kattintson a **Figyelés** diagram az [Azure portál](https://portal.azure.com/)figyelheti egy adatbázis-kihasználtság. Ekkor megjelenik egy **mérőszám** ablakot, módosíthatja a **diagram szerkesztése** gombra kattintva. Adja hozzá a következő mérési módja miatt:

- Processzor százalékos
- DTU százalékos
- Adatok IO százalékos
- Adatbázis méretének százalékos

Ha hozzáadta ezeket a mértékek, továbbra is megtekintheti azokat a **Figyelés** diagram **metrikus** ablakban a további részleteket. Minden négy mérőszám megjelenítése az adatbázis a **DTU** viszonyított átlagos kihasználtsági százalékos. A [szolgáltatási rétegek](sql-database-service-tiers.md) cikke DTUs kapcsolatban további tájékoztatást.

![A szolgáltatás réteg figyelemmel kísérése adatbázis teljesítményét.](./media/sql-database-service-tiers/sqldb_service_tier_monitoring.png)

Értesítések az teljesítménymutatók is konfigurálhatja. Kattintson a **Hozzáadás értesítés** gombra a **metrikus** ablakban. Az értesítés beállítása a varázsló utasításait követve. Ha a mértékek nagyobb, mint egy bizonyos küszöbértéket, vagy ha egy bizonyos küszöbértéket alá esik a mérőszám értesítés lehetősége van.

Ha például a terhelést a várt növekedéséhez az adatbázisban, ha megadhatja beállítása e-mailben riasztás, amikor az adatbázis bármelyik a teljesítménymutatók 80 %-os eléri. Használhatja ezt-előrejelző, megállapíthatja, ha nagyobb teljesítmény felsőfokon váltani lehet, hogy.

Az teljesítménymutatók is segíthetnek annak eldöntésében, hogy ha nézheti meg vissza léptetheti alacsonyabb teljesítmény szintre. Tegyük fel, szabványos S2 adatbázis használ, és az összes teljesítménymutatók megjelenítése, az adatbázis átlagosan ne használjon 10 %-nál nagyobb az adott időpontban. Valószínű, hogy az adatbázis jól szabványos S1 működnek. Jó helyen jár tartsa szem előtt, amely lefoglalását vagy áthelyezése a teljesítmény alacsonyabb szinten döntés végrehajtása előtt ingadozik munkaterhelésekből.

## <a name="monitor-databases-using-dmvs"></a>Monitor adatbázisait DMVs

Az azonos mértékek, amely a portál elérhető rendszer nézetek is elérhetők: [sys.resource_stats](https://msdn.microsoft.com/library/dn269979.aspx) a kiszolgáló és a [sys.dm_db_resource_stats](https://msdn.microsoft.com/library/dn800981.aspx) a felhasználói adatbázisban logikai **fő** adatbázisban. **Sys.resource_stats** használja, ha módosítani szeretné, hogy figyelheti a kevésbé részletes adatokat át egy hosszabb időszakra vetített állapotával. Ha módosítani szeretné egy kisebb időkereten belüli finomabb adatok figyelése **sys.dm_db_resource_stats** használata További tudnivalókért lásd: [Azure SQL-adatbázis teljesítmény útmutatást](sql-database-performance-guidance.md#monitoring-resource-use-with-sysresourcestats).

>[AZURE.NOTE] **sys.dm_db_resource_stats** -be, amikor használt webes és üzleti edition adatbázisok, amely után az üres eredményt adja.

Rugalmas adatbázis-készletek figyelheti a készletben egyes adatbázisokat, a jelen szakaszban ismertetett eljárások. De figyelheti a készlet egészének mérete is. Információ [Monitor és kezelése egy rugalmas adatbázis készlet](sql-database-elastic-pool-manage-portal.md).
