<properties
    pageTitle="SQL-adatbázis: Mi az a DTU? | Microsoft Azure"
    description="Mi az Azure SQL-adatbázis ismertetése tranzakció mértékegysége."
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
    ms.workload="NA"
    ms.date="09/06/2016"
    ms.author="carlrab"/>

# <a name="explaining-database-transaction-units-dtus-and-elastic-database-transaction-units-edtus"></a>Adatbázis-tranzakción egységek (DTUs) és rugalmas adatbázis tranzakció egységek (eDTUs)

Ez a cikk ismerteti, hogy adatbázis-tranzakción egységek (DTUs) és rugalmas adatbázis tranzakció egységek (eDTUs), és mi történik, ha a maximális DTUs vagy eDTUs gombra kattint.  

## <a name="what-are-database-transaction-units-dtus"></a>Mik azok az adatbázis-tranzakción egységek (DTUs)

Egy DTU, amelyek a specifikus teljesítmény szinten egy [különálló adatbázis szolgáltatási réteg](sql-database-service-tiers.md#standalone-database-service-tiers-and-performance-levels)belül önálló Azure SQL-adatbázishoz elérhetővé szeretné tenni az erőforrások mértékegységet. Egy DTU kombinált méri Processzor, memória és adatátviteli, valamint tranzakció napló I/O határozza meg egy OLTP javasolt terhelést megtervezni a való életben OLTP munkaterhelésekből jellemző arány az adatokat. A DTUs cérnázó teljesítményével kapcsolatos adatbázis növelésével megfelel az erőforrás elérhető az adatbázis megadása cérnázó. Például 1750 DTUs prémium P11 adatbázis nyújt további DTU x 350 mint 5 DTUs egyszerű adatbázis power számítja ki. Azt határozza meg, a DTU keverése OLTP javasolt terhelését mögött módszertan megértéséhez, az [SQL-adatbázis javasolt áttekintése](sql-database-benchmark-overview.md)című témakörben találhat.

![Bevezetés az SQL-adatbázishoz: egy adatbázis DTUs réteg és szint](./media/sql-database-what-is-a-dtu/single_db_dtus.png)

Módosítható [szolgáltatási rétegek](sql-database-scale-up.md) minimális legrövidebb leállás bármikor az alkalmazás (általában átlagolása négy másodperc alatt). Sok vállalkozások és alkalmazások esetén nem hozza létre az adatbázisokat és igény szerinti egyetlen adatbázis teljesítményét felfelé vagy lefelé is elegendő, különösen ha szokásai viszonylag kiszámíthatóan. De ha járhat szokásai, azt teheti azt nehéz költségeket és az üzleti modell kezelése. Az ebben az esetben egy rugalmas készlet az megadott számú eDTUs használja.

## <a name="what-are-elastic-database-transaction-units-edtus"></a>Mik azok a rugalmas adatbázis tranzakció egységek (eDTUs)

Egy eDTU az erőforrások (DTUs)-kiszolgálókon üzemelő Azure SQL - nevű egy [rugalmas készlet](sql-database-elastic-pool.png)adatbázisok közötti megosztható közül mértékegységet. Rugalmas készletek kezelése a teljesítmény célok több létrehozott adatbázisok körben változó és járhat szokásai egyszerű tényleges költség megoldást nyújtanak. Lásd: [rugalmas készletek és szolgáltatási rétegek](sql-database-service-tiers.md#elastic-pool-service-tiers-and-performance-in-edtus) további információt.

![Bevezetés az SQL-adatbázishoz: eDTUs réteg és szint](./media/sql-database-what-is-a-dtu/sqldb_elastic_pools.png)

Egy készlet eDTUs rögzített áron számú kap. Belül a készlet egyes adatbázisokat kapnak a rugalmasság automatikus méretezése belül paramétereinek beállítása. Nagy terhelés alatt adatbázis igénybe vehet, további eDTUs igényeknek. Az egyszerűsített terhelések adatbázisok felhasználása kisebb és terhelés nélküli adatbázisok felhasználása nincs eDTUs. A felügyeleti feladatok erőforrásokat, nem egyetlen adatbázisok, hanem a teljes gyűjtő kiépítési egyszerűbbé teszi. Valamint a készlet kiszámíthatóan költségvetést van.

További eDTUs is hozzá egy meglévő készlet nincs adatbázis legrövidebb leállás és az adatbázisok rugalmas készletben nincs hatással. Hasonlóképpen ha már nincs szükség külön eDTUs azok távolítható el egy meglévő készletből bármely pontján időben. Felvehet, és a készlet adatbázisok kivonása. Ha egy adatbázis található előre a-felhasználásával erőforrások, helyezze.

## <a name="how-can-i-determine-the-number-of-dtus-needed-by-my-workload"></a>Hogyan tudhatom meg, szükség szerint a terhelést DTUs száma?

Ha a keresett áttelepítendő egy meglévő helyszíni vagy SQL Server virtuális gép terhelést Azure SQL-adatbázishoz, használhatja a [DTU Számológép](http://dtucalculator.azurewebsites.net/) közelítő értékét számító a szükséges DTUs számát. Egy meglévő Azure SQL-adatbázis-terhelés [SQL adatbázis lekérdezési teljesítmény betekintést](sql-database-query-performance.md) is használhatja az adatbázis erőforrás-felhasználás (DTUs) hogyan optimalizálhatja a terhelést mélyebb betekintést megszerezni megértéséhez. A [sys.dm_db_ resource_stats](https://msdn.microsoft.com/library/dn800981.aspx) DMV az utolsó egy órával erőforrás felhasználási információkért is használhatja. Azt is megteheti a katalógus nézet [sys.resource_stats](http://msdn.microsoft.com/library/dn269979.aspx) is lekérdezhető szeretne letölteni a ugyanazokat az adatokat az utóbbi 14 nap, annak ellenére, egy alsó funkcióvesztést az 5 perc átlagokat.

## <a name="how-do-i-know-if-i-could-benefit-from-an-elastic-pool-of-resources"></a>Honnan tudhatom, hogy ha e kihasználhatók egy rugalmas erőforráskészlethez tartozik?

Készletek alkalmasak a arra, hogy az adott kihasználtsági mintázatú adatbázisok sok. Az adott adatbázis a mintázat jellemzi az általában viszonylag ritka kihasználtsági kiugrásainak megfelelő alacsony átlagos kihasználtsági. SQL-adatbázis automatikusan kiértékeli a korábbi erőforrás-kihasználtság egy meglévő SQL-adatbázis kiszolgálója az adatbázisok és javasolja, a megfelelő készlet konfiguráció az Azure-portálon. További tudnivalókért lásd: [Amikor egy rugalmas adatbázis készlet kell használni?](sql-database-elastic-pool-guidance.md)

## <a name="what-happens-when-i-hit-my-maximum-dtus"></a>Mi történik, amikor e találati a maximális DTUs

Teljesítményszint kalibrált és a szükséges erőforrások biztosítására az adatbázis terhelést felfelé a kijelölt szolgáltatás szint/teljesítmény szint megengedett maximális határokon futtatásához szabályozott. A terhelési van szerezze meg az egyik Processzor/adatok IO/Log IO határa korlátozások, ha nem szűnnek meg a következőt a megengedett szint, de valószínű, hogy a lekérdezések nagyobb késések látható. Ezek a korlátok nem vezethet a hibák, de inkább egy sebesség a terhelést a, a csökkenése kivéve, ha a sebesség csökkenése válik, hogy a lekérdezések időzítés indítása szigorú. Korlátozások az maximális engedélyezett egyidejű felhasználói munkamenetek/összehívások (dolgozó szál) vannak szerezze meg, ha közvetlen hibák látni. [Azure SQL-adatbázis erőforrás korlátai](sql-database-resource-limits.md) eltérő Processzor, a memóriahasználat, az adatok I/O erőforrások vonatkozó információkat, illetve tranzakció napló I/O talál.

## <a name="next-steps"></a>Következő lépések

- [Szolgáltatási réteg](sql-database-service-tiers.md) DTUs és önálló adatbázisok és rugalmas készletek eDTUs információkat talál.
- [Azure SQL-adatbázis erőforrás korlátai](sql-database-resource-limits.md) eltérő Processzor, a memóriahasználat, az adatok I/O erőforrások vonatkozó információkat, illetve tranzakció napló I/O talál.
- Lásd: az [SQL adatbázis lekérdezési teljesítmény betekintést](sql-database-query-performance.md) a (DTUs) felhasználási megértéséhez.
- [SQL-adatbázis javasolt áttekintése](sql-database-benchmark-overview.md) , azt határozza meg, a DTU keverése OLTP javasolt terhelését mögött módszertan megértéséhez című témakörben találhat.