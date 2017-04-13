<properties
    pageTitle="Tiltsa le a Nyújtás adatbázist, és vissza távoli adatokat vihet |} Microsoft Azure"
    description="Megtudhatja, hogyan tiltható le a Nyújtás adatbázis tábla, és tetszés szerint visszaadni távoli adatforrásból elemet."
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
    ms.date="08/05/2016"
    ms.author="douglasl"/>

# <a name="disable-stretch-database-and-bring-back-remote-data"></a>Tiltsa le a Nyújtás adatbázist, és vissza távoli adatokat vihet

Tábla Nyújtás adatbázis letiltásához jelölje be **Nyújtás** az SQL Server Management Studio tábla. Válassza az alábbi lehetőségek közül.

-   **Letiltása |} Adatokat vihet vissza az Azure**. A távoli adatok el a táblázatot az Azure SQL Server, készítsen biztonsági másolatot tiltsa le Nyújtás adatbázis el a táblázatot. Ez a művelet adatok átadás költségeket vonz, és nem állítható le.

-   **Letiltása |} Adatok hagyja az Azure**. Tiltsa le a Nyújtás adatbázis a táblázatot.  A tábla Azure-ban a távoli adatok elhagyja.

Is használhatja a Transact\-Nyújtás adatbázis letiltása, az adatbázis és tábla SQL.

Nyújtás adatbázis tábla letiltásakor, adatok áttelepítési végpontok és a lekérdezés eredményét már nem tartalmazza a távoli tábla származó találatok jelenjenek meg.

Ha egyszerűen szeretne mutasson az adatok áttelepítése, olvassa el a [Szünet és az önéletrajz Nyújtás adatbázis](sql-server-stretch-database-pause.md).

>   [AZURE.NOTE] Kitöltés adatbázis letiltása, az adatbázis és tábla nem törli a távoli objektum. Ha törölni szeretné a távoli tábla vagy a távoli adatbázis, akkor engedje el az Azure adatkezelési portál használatával. A távoli objektumok továbbra is Azure költségek merülnek fel, amíg nem törli őket. További információt a című témakörben talál [Az SQL Server Nyújtás adatbázis árak](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/).

## <a name="disable-stretch-database-for-a-table"></a>Táblázat Nyújtás adatbázis letiltása

### <a name="use-sql-server-management-studio-to-disable-stretch-database-for-a-table"></a>Tiltsa le a Nyújtás adatbázis tábla SQL Server Management Studio segítségével

1.  Az SQL Server Management Studio eszközben, az objektum Explorerben válassza ki a táblázatot, amelynek le szeretné tiltani a Nyújtás adatbázis.

2.  Jobb\-kattintson, és válassza a **Nyújtás**, és válasszon az alábbi lehetőségek közül.

    -   **Letiltása |} Adatokat vihet vissza az Azure**. A távoli adatok el a táblázatot az Azure SQL Server, készítsen biztonsági másolatot tiltsa le Nyújtás adatbázis el a táblázatot. Ez a parancs nem mondhatók le.

        >   [AZURE.NOTE] A távoli adatok másolása el a táblázatot az Azure vissza az SQL Server adatok átadás költségeket vonz. További információt a című témakörben talál [Átvitelek árak Adatrészletek](https://azure.microsoft.com/pricing/details/data-transfers/).

        Miután az összes a távoli adatok átmásolt Azure SQL Server biztonsági másolatot, nyújtás le van tiltva a táblázat.

    -   **Letiltása |} Adatok hagyja az Azure**. Tiltsa le a Nyújtás adatbázis a táblázatot.  A tábla Azure-ban a távoli adatok elhagyja.

    >   [AZURE.NOTE] Kitöltés adatbázis letiltása tábla nem törli a távoli adatok és a távoli. Ha törölni szeretné a távoli tábla, akkor engedje el az Azure adatkezelési portál használatával. A távoli táblázat továbbra is Azure költségek merülnek fel, amíg nem törli azokat. További információt a című témakörben talál [Az SQL Server Nyújtás adatbázis árak](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/).

### <a name="use-transact-sql-to-disable-stretch-database-for-a-table"></a>Használja a Transact\-SQL-adatbázis Nyújtás tiltsa le a táblázat

-   Egy táblázat és a távoli adatok el a táblázatot az Azure SQL Server biztonsági másolat tiltsa le a Nyújtás, a következő parancsot. Miután az összes a távoli adatok átmásolt Azure SQL Server biztonsági másolatot, nyújtás le van tiltva a táblázat.

    Ez a parancs nem mondhatók le.

    ```tsql
    USE <Stretch-enabled database name>;
    GO
    ALTER TABLE <Stretch-enabled table name>  
       SET ( REMOTE_DATA_ARCHIVE ( MIGRATION_STATE = INBOUND ) ) ;
    GO
    ```
    >   [AZURE.NOTE] A távoli adatok másolása a táblához, az Azure vissza az SQL Server adatok átadás költségeket vonz. További információt a című témakörben talál [Átvitelek árak Adatrészletek](https://azure.microsoft.com/pricing/details/data-transfers/).

-   Le szeretne tiltani Nyújtás tábla elhagyja a távoli adatok, a következő parancsot.

    ```tsql
    ALTER TABLE <table_name>
       SET ( REMOTE_DATA_ARCHIVE = OFF_WITHOUT_DATA_RECOVERY ( MIGRATION_STATE = PAUSED ) ) ;
    ```

>   [AZURE.NOTE] Kitöltés adatbázis letiltása tábla nem törli a távoli adatok és a távoli. Ha törölni szeretné a távoli tábla, akkor engedje el az Azure adatkezelési portál használatával. A távoli táblázat továbbra is Azure költségeket merülnek fel, amíg nem törli azokat. További információt a című témakörben talál [Az SQL Server Nyújtás adatbázis árak](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/).

## <a name="disable-stretch-database-for-a-database"></a>Adatbázis Nyújtás adatbázis letiltása
Mielőtt Nyújtás adatbázis letilthatja az adatbázis, el kell-e az egyes Nyújtás Nyújtás adatbázis letiltása\-engedélyezve van az adatbázisból származó táblázatot.

### <a name="use-sql-server-management-studio-to-disable-stretch-database-for-a-database"></a>SQL Server Management Studio segítségével Nyújtás adatbázis letiltása az adatbázis

1.  Az SQL Server Management Studio eszközben, az objektum Explorerben jelölje ki az adatbázist, amelynek le szeretné tiltani a Nyújtás adatbázis.

2.  Jobb\-gombra, és jelölje ki a **tevékenységeket**, és válassza a **Nyújtás**, és válassza a **letiltása**.

>   [AZURE.NOTE] Kitöltés adatbázis letiltása az adatbázis nem törli a távoli adatbázis. Ha törölni szeretné a távoli adatbázis, akkor engedje el az Azure adatkezelési portál használatával. A távoli adatbázis továbbra is Azure költségek merülnek fel, amíg nem törli azokat. További információt a című témakörben talál [Az SQL Server Nyújtás adatbázis árak](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/).

### <a name="use-transact-sql-to-disable-stretch-database-for-a-database"></a>Használja a Transact\-SQL-adatbázis tiltsa le a Nyújtás adatbázis
A következő parancsot.

```tsql
ALTER DATABASE <database name>
    SET REMOTE_DATA_ARCHIVE = OFF ;
```

>   [AZURE.NOTE] Kitöltés adatbázis letiltása az adatbázis nem törli a távoli adatbázis. Ha törölni szeretné a távoli adatbázis, akkor engedje el az Azure adatkezelési portál használatával. A távoli adatbázis továbbra is Azure költségek merülnek fel, amíg nem törli azokat. További információt a című témakörben talál [Az SQL Server Nyújtás adatbázis árak](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/).

## <a name="see-also"></a>Lásd még:

[ALTER adatbázis megadása (Transact-SQL)](https://msdn.microsoft.com/library/bb522682.aspx)

[Mutasson az egérrel, és folytassa a Nyújtás adatbázis](sql-server-stretch-database-pause.md)
