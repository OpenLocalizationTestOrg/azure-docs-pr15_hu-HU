<properties
    pageTitle="Az Azure-portálon BACPAC fájlba Azure SQL-adatbázisból archiválása"
    description="Az Azure-portálon BACPAC fájlba Azure SQL-adatbázisból archiválása"
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="08/15/2016"
    ms.author="sstein"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="archive-an-azure-sql-database-to-a-bacpac-file-using-the-azure-portal"></a>Az Azure-portálon BACPAC fájlba Azure SQL-adatbázisból archiválása

> [AZURE.SELECTOR]
- [Azure portál](sql-database-export.md)
- [A PowerShell](sql-database-export-powershell.md)

Ebben a cikkben utasításokat az Azure SQL-adatbázishoz (Azure blob-tárolóhoz tárolt) BACPAC fájlba archiválásra az [Azure-portálon](https://portal.azure.com).

Kell archiválhatja Azure SQL-adatbázishoz, amikor a adatbázissémát és az adatokat exportálhatja BACPAC fájlba. Egy fájl BACPAC egyszerűen egy ZIP-fájl BACPAC kiterjesztésű fájlt. Azure blob-tárolóhoz vagy a helyi tároló helyszíni helyen BACPAC fájl később tárolható és Azure SQL-adatbázis vagy SQL Server később importált vissza a helyszíni telepítésével. 

***Megfontolandó szempontok***

- Egy archívumhoz biztosítani szeretné konzisztens tranzakción keresztül, gondoskodnia kell arról, hogy nincs írási tevékenység Mi az exportálás során, vagy egy [tranzakción keresztül egységes másolása](sql-database-copy.md) az Azure SQL-adatbázis exportál.
- Azure Blob-tárolóhoz archivált BACPAC fájlok maximális mérete 200 GB. Helyi tárolóba nagyobb BACPAC fájl archiválja, segédprogrammal [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx) parancssor parancsot. Ez a segédprogram a Visual Studio és az SQL Server rendszerhez. Is megtekintheti, [Töltse le](https://msdn.microsoft.com/library/mt204009.aspx) az SQL Server Data Tools a segédprogram megszerezni legújabb verzióját.
- Azure prémium tárolóhoz BACPAC fájlból archiválás nem támogatott.
- Ha az exportálási művelet túllépi 20 órát, előfordulhat, hogy megszakítható. Exportálás során teljesítmény növelése érdekében a következőkre van lehetősége:
 - A szolgáltatás szintjének ideiglenes növelése
 - Az összes olvasási és írási tevékenység az exportálás során jogosultsága szűnik.
 - Használja a [csoportosított index](https://msdn.microsoft.com/library/ms190457.aspx) létrehozása az összes nagy táblázatok nem null értékű. Csoportosított indexek, nélkül exportálása előfordulhat, hogy sikertelen, ha a 6-12 óránál hosszabb ideig tart. Ennek oka az, próbálja ki a teljes táblázat exportálása egy táblázat keresés befejezéséhez szükséges az Exportálás szolgáltatást. Határozza meg, ha a táblák exportálása szolgáltatáshoz optimalizált jó módszer, hogy futtatni **DBCC SHOW_STATISTICS** , és ügyeljen arra, hogy a *RANGE_HI_KEY* nem null és az érték helyes terjesztési. A részletekért olvassa [DBCC SHOW_STATISTICS](https://msdn.microsoft.com/library/ms174384.aspx).


> [AZURE.NOTE] BACPACs nem célja, hogy a visszaállítási művelet és biztonsági másolat használható. Azure SQL-adatbázis automatikusan létrehozza a biztonsági másolatok minden felhasználó adatbázishoz. További információ a [Üzleti folytonosságot áttekintése](sql-database-business-continuity.md)című témakörben találhat.

Ez a cikk elvégzéséhez az alábbiakra van szükség:

- Egy Azure-előfizetést.
- Microsoft Azure SQL-adatbázisban. 
- Egy [szabványos tároló Azure-fiók](../storage/storage-create-storage-account.md) blob-tároló a BACPAC tárolása a szabványos tároló.

## <a name="export-your-database"></a>Az adatbázis exportálása

Nyissa meg az SQL-adatbázis lap az adatbázishoz, amelyet exportálni szeretne.

> [AZURE.IMPORTANT] Zökkenőmentes tranzakción keresztül egységes BACPAC fájl el első [Hozzon létre egy másolatot az adatbázisról](sql-database-copy.md) , majd exportálja az adatbázis-példány. 

1.  Nyissa meg az [Azure-portálon](https://portal.azure.com).
2.  Kattintson az **SQL-adatbázisait**.
3.  Kattintson az adatbázis archiválását.
4.  Az SQL-adatbázis lap kattintson az **Exportálás** nyissa meg az **adatbázis exportálása** lap:

    ![Exportálás gomb][1]

5.  Kattintson a **tárhely** , és jelölje ki azt a tárterület-fiók és blob tárolót a BACPAC tároló:

    ![adatbázis exportálása][2]

6. Jelölje ki a hitelesítés típusa. 
7.  Írja be a megfelelő hitelesítő az Azure SQL-kiszolgálót az exportálni kívánt adatbázist tartalmazó.
8.  **Az OK gombra** az adatbázis archiválását gombra. **Az OK** gombra kattintva hoz létre az Exportálás adatbázisba felkérés, és a szolgáltatás elküldi. Az Exportálás hosszabb idő hosszának méretétől és összetettségétől az adatbázist, és a szolgáltatási szint függ. Értesítést kap.

    ![értesítés exportálása][3]

## <a name="monitor-the-progress-of-the-export-operation"></a>Az exportálási művelet állapotának nyomon követéséhez

1.  Kattintson az **SQL-kiszolgálók**.
2.  Kattintson a kiszolgálóra, az eredeti () adatbázis csak archivált tartalmazó.
3.  Görgessen le a műveletek.
4.  Az SQL server lap kattintson **az importálás/exportálás előzmények**:

    ![importálás, exportálás előzmények][4]

## <a name="verify-the-bacpac-is-in-your-storage-container"></a>A BACPAC van-e a tárterület-tárolóhoz

1.  **Tárterület-fiókok**parancsára.
2.  Válassza a tárterület-fiók a BACPAC archívumba tároló.
3.  **Tárolók** és válassza a részletek (letöltése és a további lehetőségek BACPAC mentése) be az adatbázis exportált tároló.

    ![.bacpac fájl adatai][5]  

## <a name="next-steps"></a>Következő lépések

- Importál egy BACPAC Azure SQL-adatbázissal kapcsolatos további tudnivalókért lásd: az [Importálás egy BACPCAC Azure SQL-adatbázishoz](sql-database-import.md)
- Egy BACPAC importálása SQL Server-adatbázishoz kapcsolatos további tudnivalókért lásd: [importálása egy BACPCAC SQL Server-adatbázishoz](https://msdn.microsoft.com/library/hh710052.aspx)



<!--Image references-->
[1]: ./media/sql-database-export/export.png
[2]: ./media/sql-database-export/export-blade.png
[3]: ./media/sql-database-export/export-notification.png
[4]: ./media/sql-database-export/export-history.png
[5]: ./media/sql-database-export/bacpac-archive.png

