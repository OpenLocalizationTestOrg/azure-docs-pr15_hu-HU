<properties
    pageTitle="Biztonsági másolat hibaelhárítási, és az Azure SQL-adatbázis visszaállítása"
    description="Megtudhatja, hogy miként felhő adatbázis helyreállítása hibák és a biztonsági mentés és a kópiák használata SQL Azure-adatbázis kimaradások."
    services="sql-database"
    documentationCenter=""
    authors="dalechen"
    manager="felixwu"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/31/2016"
    ms.author="daleche"/>

# <a name="restore-a-database-to-a-previous-point-in-time-restore-a-deleted-database-or-recover-from-a-data-center-outage"></a>Adatbázis visszaállítása az előző pontjához időben, törölt adatbázis visszaállítása vagy egy adatok központ üzemszünetek helyreállítása

SQL-adatbázis továbbra is az adatbázis kópiák, így helyreállítása kimaradások és felhasználói hiba. Rendelkezésre álló lehetőségek attól függenek, az adatbázis szolgáltatási réteg és a kiválasztott beállítások. Az [Üzleti folytonosságot áttekintés](sql-database-business-continuity.md) információ és tervezési szempontjait című témakörben találhat.

## <a name="to-restore-a-database-to-a-previous-point-in-time"></a>Ha vissza szeretne állítani egy adatbázis egy előző pont időben
1.  Az [Azure-portálon](https://azure.microsoft.com/)kattintson az **SQL-adatbázisait**.
2.  Jelölje ki az adatbázist a listából, és kattintson a **Visszaállítás**gombra.
3.  Írja be az adatbázis új nevét, válassza ki azt a dátumot és időt szeretne visszaállítani, és kattintson a **Létrehozás.**
4.  Alkalmazás módosításokat szükség esetén az új adatbázis hivatkozni szeretne. Lásd: [az idő pontjához adatbázis visszaállítása](sql-database-recovery-using-backups.md#point-in-time-restore).

## <a name="to-restore-an-accidentally-deleted-database"></a>Egy véletlenül törölt adatbázis visszaállítása
1.  Az [Azure-portálon](https://azure.microsoft.com/)kattintson az **SQL-kiszolgálók**.
2.  Jelölje ki a kiszolgálón tárolt az adatbázist a listából.
3.  A kiszolgáló lap a görgessen lefelé, és kattintson a **Törölt elemek adatbázisok**.
4.  Jelölje ki a visszaállítandó az adatbázist, és kattintson a **Létrehozás**gombra.
5.  Alkalmazás módosításokat szükség esetén az új adatbázis hivatkozni szeretne. Olvassa el a [törölt adatbázis helyreállítása](sql-database-recovery-using-backups.md#deleted-database-restore)című témakört.

## <a name="to-recover-from-a-regional-datacenter-outage"></a>A regionális adatközpont üzemszünetek helyreállítása
A szokásos és prémium adatbázisok beállítása geo replikált formátumú másodlagos zónák, ha visszaállíthatja használatáról a következő formátumú másodlagos zónák. Biztosít az adatok elvesztését kevesebb lehetőségeit adatbázis visszaállítása. Lásd: a [helyreállítása az Azure SQL-adatbázisokkal automatikus adatbázis biztonsági mentése](sql-database-disaster-recovery.md) további információt.

Azure is biztosít egy másik régióbeli (geo felesleges biztonsági) minden adatbázis biztonsági másolatait. A biztonsági mentés, vagyis Geo-visszaállítási, hozhat létre új adatbázist, de valószínű, hogy fogja tapasztalni adatvesztés esetén a ezt a módszert használja arra.

**Egy adatbázist geo-visszaállítási helyreállítása:**

- Az [Azure-portálon](https://azure.microsoft.com/)kattintson az **Új**gombra, kattintson az **adatok és a tárhely**, kattintson az **SQL-adatbázis**, és válassza a **biztonsági mentése** az adatbázis forrása:. Lásd: a [helyreállítása az Azure SQL-adatbázis-üzemszünetek](sql-database-disaster-recovery.md) részletes.