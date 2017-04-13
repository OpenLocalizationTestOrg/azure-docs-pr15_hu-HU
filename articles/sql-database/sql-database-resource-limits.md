<properties
    pageTitle="Azure SQL-adatbázis erőforrás korlátai"
    description="Ezen az oldalon ismerteti néhány gyakori erőforrás Azure SQL-adatbázis."
    services="sql-database"
    documentationCenter="na"
    authors="CarlRabeler"
    manager="jhubbard"
    editor="monicar" />


<tags
    ms.service="sql-database"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-management"
    ms.date="10/13/2016"
    ms.author="carlrab" />


# <a name="azure-sql-database-resource-limits"></a>Azure SQL-adatbázis erőforrás korlátai

## <a name="overview"></a>– Áttekintés

Azure SQL-adatbázis kezelése két különböző mechanizmusok segítségével adatbázis rendelkezésére álló erőforrásokat: **Erőforrások** és a **Végrehajtás vonatkozó korlátozások**. Ebből a témakörből megtudhatja, az erőforrás-kezelés az alábbi két fő területen.

## <a name="resource-governance"></a>Erőforrás irányítási
A Basic, normál és Premium szolgáltatási rétegek tervezés céljainak egyik Azure SQL-adatbázis, mintha az adatbázis-fut a saját számítógépen, más adatbázisokból teljesen elszigetelt viselkedését. Erőforrás irányítási amelynek azonos a problémáról. Az összesített erőforrás-kihasználtság eléri a maximális elérhető Processzor, a memóriahasználat, a napló I/O és a adatok portterületre az adatbázis rendelt, a erőforrás irányítási végrehajtás lekérdezések sorba, és erőforrások hozzárendelése a várólistás lekérdezések, ahogy azok szabadítson fel.

Mint egy dedikált gépen az összes rendelkezésre álló erőforrásokat használó eredményezi jelenleg a lekérdezések eredményezhet, az ügyfél időtúllépése parancs végrehajtása hosszabb végrehajtását. Olyan szigorú újrapróbálkozási összefüggés-alkalmazások és -alkalmazások végrehajtása nagy gyakorisággal lekérdezéseket is gyakran hibák üzenetek meg az új lekérdezések végrehajtása egyidejű kérelmek határértékén elérésekor.

### <a name="recommendations"></a>Javaslatok:
Az erőforrás-kihasználtság szerint lekérdezések átlagos válaszidő figyelje meg, amikor egy adatbázis legnagyobb felhasználásának közelítő. Amikor a magasabb lekérdezés késések Ha általában három lehetőség közül választhat:

1.  Rövidítse le a bejövő felkérést időtúllépés és kérelmek felfelé bővítmény használatára az adatbázishoz.

2.  Nagyobb teljesítmény szint hozzárendelése az adatbázist.

3.  Lekérdezések csökkentheti az erőforrás-kihasználtság minden lekérdezés optimalizálása gombra. További információ című lekérdezés hangolása/Hinting Azure SQL-adatbázis teljesítmény útmutató című témakörben.

## <a name="enforcement-of-limits"></a>Korlátozások végrehajtása
Processzor, a memóriahasználat, a naplófájl I/O és a adatok I/O eltérő erőforrások által elutasítja az új kérések korlátok elérésekor kikényszerített. Ügyfelek kap egy [hibaüzenet jelenik meg](sql-database-develop-error-messages.md) attól függően, hogy a elérte korlátozást.

Az SQL-adatbázishoz kapcsolatok számát és dolgozható egyidejű kérelmek számát mindegyike korlátozott. SQL-adatbázis lehetővé teszi, hogy a lehet nagyobb, mint a támogatási kapcsolatkészletezés egyidejű kérelmek száma az adatbázis-kapcsolatot. Rendelkezésre álló kapcsolatokkal mennyiségét egyszerűen vezérelhető az alkalmazás, a párhuzamos kérések mennyiségét is gyakran nehezebb becslése és időpontok. Csúcs terhelés alatt a különösen amikor az alkalmazás akár küld túl sok kérelmek vagy az adatbázis eléri a erőforrás korlátozza, és elindítja a hosszabb futó lekérdezések miatt dolgozó szálak felhalmozódjanak hibák is előforduló.

## <a name="service-tiers-and-performance-levels"></a>Szolgáltatási rétegek és a teljesítmény szintek

Vannak szolgáltatási rétegek és két különálló adatbázis és elasztikus készletek teljesítményszint.

### <a name="standalone-databases"></a>Különálló adatbázisok

Az önálló adatbázis az adatbázis-szolgáltatás réteg és a teljesítmény szint adatbázis határain határozza meg. Az alábbi táblázat ismerteti a jellemzői egyszerű, szabványos, és Premium adatbázisokat a teljesítményszint változó.

[AZURE.INCLUDE [SQL DB service tiers table](../../includes/sql-database-service-tiers-table.md)]

### <a name="elastic-pools"></a>Rugalmas készletek

[Rugalmas készletek](sql-database-elastic-pool.md) megosztás erőforrások az erőforráskészlet-adatbázisok között. Az alábbi táblázat ismerteti a Basic, normál és Premium rugalmas adatbázis készletek jellemzői.

[AZURE.INCLUDE [SQL DB service tiers table for elastic databases](../../includes/sql-database-service-tiers-table-elastic-db-pools.md)]

Az egyes erőforrások az előző táblázatban található kibontott meghatározását leírásában olvasható a [szolgáltatási réteg funkciókkal és korlátok](sql-database-performance-guidance.md#service-tier-capabilities-and-limits). Szolgáltatási rétegek áttekintése olvassa el a [Azure SQL-adatbázis szolgáltatási rétegek és teljesítményszint](sql-database-service-tiers.md)című témakört.

## <a name="other-sql-database-limits"></a>Más SQL-adatbázis korlátai

| Terület | Határérték | Leírás |
|---|---|---|
| Az automatikus használatával adatbázisokat előfizetésenként exportálása | 10 | Az automatikus exportálás ütemezést létrehozni egy egyéni SQL-adatbázisait mentésével teszi lehetővé. További tudnivalókért lásd: [SQL-adatbázisait: az SQL-adatbázis exportnak automatikus támogatása](http://weblogs.asp.net/scottgu/windows-azure-july-updates-sql-database-traffic-manager-autoscale-virtual-machines).|
| Adatbázisok kiszolgálónként | Találatlista 5000 DB | Felfelé 5000 adatbázisok kiszolgálónként V12 kiszolgálókon engedélyezett. |  
| Kiszolgálónként DTUs | 45000 | A kiépítési, rugalmas készletek és adatraktárakhoz V12 kiszolgálókon kiszolgálónként 45000 DTUs érhetők el. |



## <a name="resources"></a>Erőforrások

[Azure előfizetés és szolgáltatás korlátozások, kvóták és korlátozások](../azure-subscription-service-limits.md)

[Azure SQL-adatbázis szolgáltatási rétegek és a teljesítmény szintek](sql-database-service-tiers.md)

[Hibaüzenetek jelennek meg az SQL-adatbázis-ügyfélprogramok](sql-database-develop-error-messages.md)
