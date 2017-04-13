<properties
    pageTitle="Létrehozhatja és kezelheti a méretezett Azure SQL-adatbázisait rugalmas feladatok ki |} Micosoft Azure"
    description="Haladjon végig és létrehozásának és kezelésének egy rugalmas adatbázis feladatot."
    services="sql-database"
    documentationCenter=""
    manager="jhubbard"
    authors="ddove"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/27/2016"
    ms.author="ddove"/>

# <a name="create-and-manage-scaled-out-azure-sql-databases-using-elastic-jobs-preview"></a>Létrehozhatja és kezelheti a méretezett ki Azure SQL-adatbázisait rugalmas feladatok (előzetes verzió)

> [AZURE.SELECTOR]
- [Azure portál](sql-database-elastic-jobs-create-and-manage.md)
- [A PowerShell](sql-database-elastic-jobs-powershell.md)


**Rugalmas adatbázis feladatok** végrehajtása felügyeleti műveletek, például a séma módosításai, a hitelesítő adatok kezelése, a hivatkozási adatok frissítések, a teljesítmény adatgyűjtés vagy a bérlői (ügyfél) telemetriai webhelycsoport egyszerűbbé csoportok az adatbázisok kezelése. Rugalmas adatbázis feladatok jelenleg áll rendelkezésre az Azure-portál és -PowerShell-parancsmagok között. Azonban az Azure portál felületek csökkenteni funkció át egy [Rugalmas adatbázis készlet (előzetes verzió)](sql-database-elastic-pool.md)adatbázisokra végrehajtás korlátozódik. Az adatbázisok, például egyénileg definiált gyűjteményt vagy egy shard beállítása ( [Rugalmas adatbázis ügyfél tár](sql-database-elastic-scale-introduction.md)használatával létrehozott) csoport keresztül férhetnek hozzá a Továbbiak és parancsfájlok végrehajtását, olvassa el a [létrehozása és kezelése a PowerShell használatá feladatok](sql-database-elastic-jobs-powershell.md)című témakört. További információt a feladatok [Rugalmas adatbázis feladatok áttekintése](sql-database-elastic-jobs-overview.md)című témakörben találhat. 

## <a name="prerequisites"></a>Előfeltételek

* Egy Azure-előfizetést. Az ingyenes próbaverzióra olvassa el a [hónap próba-előfizetésre](https://azure.microsoft.com/pricing/free-trial/)című témakört.
* Egy rugalmas adatbázis készlet. Lásd: [A rugalmas adatbázis-készletek](sql-database-elastic-pool.md)
* Rugalmas adatbázis feladat szolgáltatás-összetevők telepítése. Lásd: [a projekt rugalmas adatbázis-szolgáltatás telepítése](sql-database-elastic-jobs-service-installation.md).

## <a name="creating-jobs"></a>Feladatok létrehozása

1. Az [Azure portál](https://portal.azure.com)használatával, készletből egy meglévő adatbázis rugalmas feladatot, kattintson a **feladat létrehozása**.
2. Írja be a felhasználónevét és jelszavát, az adatbázis-adminisztrátorhoz (létrehozott feladatok telepítéskor), a feladatok vezérlő adatbázis (metaadat-tároló feladatokhoz).

    ![Nevezze el a feladatot, írja be vagy illessze be a kódot, és kattintson a Futtatás gombra][1]
2. A **Feladat létrehozása** a lap írja be a feladat nevét.
3. Írja be a felhasználónevét és a jelszavát a céladatbázis parancsfájl végrehajtására sikeres megfelelő engedélyekkel rendelkező csatlakozni.
4. Illessze be, vagy írja be a T-SQL-parancsfájlokat.
5. Kattintson a **Mentés** gombra, és kattintson a **Futtatás**gombra.

    ![Feladatok létrehozása és futtatása][5]

## <a name="run-idempotent-jobs"></a>Idempotent feladatok futtatása

Parancsfájl összehasonlítja az adatbázisok futtatásakor meg róla, hogy a parancsprogram idempotent kell lennie. Ez azt jelenti, hogy a parancsprogram kell tennie többször, futtatásához akkor is, ha nem járt sikerrel előtt hiányos állapotú. Például ha nem sikerül egy parancsfájlt, a feladat automatikusan megpróbálja mindaddig, amíg a sikeres (korlátok, mint az Ismét gombra logika ahányat megszűnik az újbóli próbálkozás). Ehhez a módja a egy "Ha létezik" záradék, és törölje a talált példányokban, mielőtt hoz létre egy új objektum. Példa az alábbi:

    IF EXISTS (SELECT name FROM sys.indexes
            WHERE name = N'IX_ProductVendor_VendorID')
    DROP INDEX IX_ProductVendor_VendorID ON Purchasing.ProductVendor;
    GO
    CREATE INDEX IX_ProductVendor_VendorID
    ON Purchasing.ProductVendor (VendorID);

Azt is megteheti használja az "Ha NOT EXISTS" záradék egy új példányát létrehozása előtt:

    IF NOT EXISTS (SELECT name FROM sys.tables WHERE name = 'TestTable')
    BEGIN
     CREATE TABLE TestTable(
      TestTableId INT PRIMARY KEY IDENTITY,
      InsertionTime DATETIME2
     );
    END
    GO

    INSERT INTO TestTable(InsertionTime) VALUES (sysutcdatetime());
    GO

Ez a parancsfájl frissíti a korábban létrehozott táblát.

    IF NOT EXISTS (SELECT columns.name FROM sys.columns INNER JOIN sys.tables on columns.object_id = tables.object_id WHERE tables.name = 'TestTable' AND columns.name = 'AdditionalInformation')
    BEGIN

    ALTER TABLE TestTable

    ADD AdditionalInformation NVARCHAR(400);
    END
    GO

    INSERT INTO TestTable(InsertionTime, AdditionalInformation) VALUES (sysutcdatetime(), 'test');
    GO


## <a name="checking-job-status"></a>Feladat állapotának ellenőrzése

Amikor megkezdődött a feladatot, ellenőrizheti, hogy a meghívási folyamat.

1. Rugalmas adatbázis készlet lapján kattintson a **feladatok kezelése**elemre.

    ![Kattintson a "Manage projektek"][2]

2. Kattintson a (a) a feladat nevét. Lehet, hogy az **állapot** "Befejeződött" vagy "Nem sikerült." A feladat részletei a dátum és idő létrehozása és futtatása (b) jelennek meg. A lista (c) a alatt látható a minden adatbázisban a parancsprogram-készletben, a dátum és idő részletek megadása.

    ![Egy kész feladatot ellenőrzése][3]


## <a name="checking-failed-jobs"></a>Feladatok ellenőrzése sikertelen volt.

Ha nem sikerül egy feladatot, végrehajtása során naplója is található. Kattintson a nevére kattintva megtekintheti a részleteket a sikertelen feladatot.

![Jelölje be a sikertelen feladat][4]


[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-jobs-create-and-manage/screen-1.png
[2]: ./media/sql-database-elastic-jobs-create-and-manage/click-manage-jobs.png
[3]: ./media/sql-database-elastic-jobs-create-and-manage/running-jobs.png
[4]: ./media/sql-database-elastic-jobs-create-and-manage/failed.png
[5]: ./media/sql-database-elastic-jobs-create-and-manage/screen-2.png

 
