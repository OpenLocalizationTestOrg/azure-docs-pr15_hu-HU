<properties
    pageTitle="Figyelheti a XTP a memóriában tároló |} Microsoft Azure"
    description="Becslés és monitor XTP a memóriában tárterületet használ, kapacitás; 41823 kapacitás hiba megoldása"
    services="sql-database"
    documentationCenter=""
    authors="jodebrui"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="jodebrui"/>


# <a name="monitor-in-memory-oltp-storage"></a>Monitor a memóriában OLTP tárhely

[A memóriában OLTP](sql-database-in-memory.md)használatakor a memória optimalizálása táblák és a táblázat változók a memóriában OLTP tároló található. Minden egyes prémium szolgáltatási réteg egy maximális a memóriában OLTP tároló mérete, amit az [SQL-adatbázis szolgáltatási rétegek cikkben](sql-database-service-tiers.md#service-tiers-for-single-databases)ismertetett tartalmaz. Miután a korlát túllépésekor, beszúrhat és frissíthet műveletek kezdheti adatkapcsolat (hibával 41823). Ezen a ponton lesz szükség vagy adatot törölne memória felszabadításához, vagy a teljesítmény réteg az adatbázis frissítése.

## <a name="determine-whether-data-will-fit-within-the-in-memory-storage-cap"></a>Határozza meg, hogy az adatok belül a a memóriában tárhelyet kap nem férnek

A tárhely kap meghatározása: olvassa el az [SQL-adatbázis szolgáltatási rétegek a cikk](sql-database-service-tiers.md#service-tiers-for-single-databases) a tárhely CAPS LOCK a különböző támogatási szolgáltatás meghatározási.

Becslése memória vonatkozó követelmények a memória optimalizálása táblázat működik, mint az SQL Server ugyanúgy működik Azure SQL-adatbázisban. Tekintse át az [MSDN webhelyen](https://msdn.microsoft.com/library/dn282389.aspx)témakör néhány percet igénybe vehet.

Figyelje meg, hogy a táblázat és változó Táblázatsorok, valamint az indexek, megszámolása felé max felhasználói adatok méretét. Ezeken kívül ALTER TABLE szüksége van elég hely az a teljes táblázatot, és az indexeket az új verzió létrehozása.

## <a name="monitoring-and-alerting"></a>Figyelés és riasztási

Figyelheti a [tárhely cap a teljesítmény réteg számára](sql-database-service-tiers.md#service-tiers-for-single-databases) az Azure- [portálon](https://portal.azure.com/)százalékában a memóriában tároló használatára: 

- Az adatbázis lap keresse meg az erőforrás-kihasználtság mezőbe, és kattintson a Szerkesztés gombra.
- Válassza ki a mérőszám `In-Memory OLTP Storage percentage`.
- Értesítés hozzáadásához kattintson az erőforrás-kihasználtság jelölőnégyzetet a metrikus lap megnyitásához, majd a Hozzáadás értesítés.

Vagy az alábbi lekérdezéssel kattintva jelenítse meg a a memóriában tárterület-felhasználás:

    SELECT xtp_storage_percent FROM sys.dm_db_resource_stats


## <a name="correct-out-of-memory-situations---error-41823"></a>Javítsa ki a memória helyzetekben - 41823 hiba

Beszúrás, frissítése és létrehozása műveletek nem működnek a hibaüzenet 41823 futásának blokkolás memória eredményez.

Hibaüzenet 41823, az azt jelzi, hogy a memória optimalizálása táblák és a táblázat változók túllépte a maximális méretet.

Ez a hiba vagy megoldása:


- A memória optimalizálása tábla, esetleg a az adatok hagyományos, a merevlemez-alapú táblákhoz, mert az adatok törlése másik lehetőségként
- Frissítse a szolgáltatási réteg olyanra, amely elegendő memória optimalizálása táblákat érdemes a szükséges adatok a memóriában tárhelyet.

## <a name="next-steps"></a>Következő lépések
További forrásokat találhat a kapcsolatos [figyelése Azure SQL-adatbázis kezelése dinamikus nézetek használata](sql-database-monitoring-with-dmvs.md)
