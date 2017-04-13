<properties
    pageTitle="Mutasson az egérrel, és folytassa az adatok áttelepítése (Nyújtás adatbázis) |} Microsoft Azure"
    description="Megtudhatja, hogy miként szüneteltetheti vagy folytathatja Azure adatainak áttelepítése."
    services="sql-server-stretch-database"
    documentationCenter=""
    authors="douglaslMS"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-server-stretch-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/14/2016"
    ms.author="douglasl"/>

# <a name="pause-and-resume-data-migration-stretch-database"></a>Mutasson az egérrel, és folytassa az adatok áttelepítése (Nyújtás adatbázis)

Mutasson az egérrel, és Azure adatainak áttelepítése folytatása, válassza a **Nyújtás** az SQL Server Management Studio tábla, és jelölje ki **mutasson az** adatok áttelepítése szüneteltetheti vagy **folytathatja** adatok áttelepítése folytatni szeretné. Is használhatja a Transact\-SQL szüneteltetheti vagy folytathatja adatok áttelepítése.

Szünet adatok áttelepítése egyes táblázatokban, ha azt szeretné, hogy a helyi kiszolgálón hibáinak elhárítása, vagy teljes méretűre állíthatja a rendelkezésre álló hálózati sávszélesség.

## <a name="pause-data-migration"></a>Szünet adatok áttelepítése

### <a name="use-sql-server-management-studio-to-pause-data-migration"></a>Mutasson az adatok áttelepítése az SQL Server Management Studio segítségével

1.  Az SQL Server Management Studio eszközben, az objektum Explorerben, válassza a Nyújtás\-engedélyezve van a táblát, amelynek meg szeretné mutasson az adatok áttelepítése.

2.  Jobb\-gombra, és válassza a **Nyújtás**, és válassza a **Szünet**.

### <a name="use-transact-sql-to-pause-data-migration"></a>Használja a Transact\-SQL szünet adatok áttelepítése
A következő parancsot.

```tsql
USE <Stretch-enabled database name>;
GO
ALTER TABLE <Stretch-enabled table name>  
    SET ( REMOTE_DATA_ARCHIVE ( MIGRATION_STATE = PAUSED ) ) ;  
GO
```

## <a name="resume-data-migration"></a>Önéletrajz adatok áttelepítése

### <a name="use-sql-server-management-studio-to-resume-data-migration"></a>SQL Server Management Studio segítségével adatokat áttelepítési folytatása

1.  Az SQL Server Management Studio eszközben, az objektum Explorerben, válassza a Nyújtás\-engedélyezve van a táblát, amelynek, amelyen folytatni szeretné adatok áttelepítése.

2.  Jobb\-kattintson, és válassza a **Nyújtás**, és válassza a **Folytatás**.

### <a name="use-transact-sql-to-resume-data-migration"></a>Használja a Transact\-SQL koppintva folytathatja a adatok áttelepítése
A következő parancsot.

```tsql
USE <Stretch-enabled database name>;
GO
ALTER TABLE <Stretch-enabled table name>   
    SET ( REMOTE_DATA_ARCHIVE ( MIGRATION_STATE = OUTBOUND ) ) ;  
 GO
```

## <a name="check-whether-migration-is-active-or-paused"></a>Ellenőrizze, hogy az áttelepítési nem aktív vagy szüneteltetése

### <a name="use-sql-server-management-studio-to-check-whether-migration-is-active-or-paused"></a>Ellenőrizze, hogy az áttelepítési nem aktív vagy felfüggesztett az SQL Server Management Studio segítségével
Az SQL Server Management Studio nyissa meg a **Nyújtás adatbázis Monitor** , és ellenőrizze az **Áttelepítési állapot** oszlop értékét. További információt a című témakörben talál [Monitor és problémáinak megoldása adatok áttelepítése](sql-server-stretch-database-monitor.md).

### <a name="use-transact-sql-to-check-whether-migration-is-active-or-paused"></a>Ellenőrizze, hogy az áttelepítési nem aktív vagy felfüggesztett Transact-SQL nyelvben használatával
A lekérdezés a katalógus nézet **sys.remote_data_archive_tables** , és ellenőrizze a **is_migration_paused** oszlop értékét. Ha további információra olvassa el a [sys.remote_data_archive_tables](https://msdn.microsoft.com/library/dn935003.aspx)című témakört.

## <a name="see-also"></a>Lásd még:

[ALTER TABLE (Transact-SQL nyelvben)](https://msdn.microsoft.com/library/ms190273.aspx)
[Monitor és problémáinak megoldása adatok áttelepítése](sql-server-stretch-database-monitor.md)
