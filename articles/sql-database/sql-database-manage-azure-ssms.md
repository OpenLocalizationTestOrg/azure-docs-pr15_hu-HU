<properties 
    pageTitle="SSMS SQL-adatbázis kezelése |} Microsoft Azure" 
    description="Megtudhatja, hogyan lehet az SQL Server Management Studio segítségével SQL-adatbázis-kiszolgáló és az adatbázisok kezelése." 
    services="sql-database" 
    documentationCenter=".net" 
    authors="stevestein" 
    manager="jhubbard" 
    editor="tysonn"/>

<tags 
    ms.service="sql-database" 
    ms.workload="data-management" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/29/2016" 
    ms.author="sstein"/>


# <a name="managing-azure-sql-database-using-sql-server-management-studio"></a>SQL Server Management Studio segítségével Azure SQL-adatbázis kezelése 


> [AZURE.SELECTOR]
- [Azure portál](sql-database-manage-portal.md)
- [SSMS](sql-database-manage-azure-ssms.md)
- [A PowerShell](sql-database-manage-powershell.md)

Az SQL Server Management Studio (SSMS) segítségével Azure SQL-adatbázis-kiszolgáló és az adatbázisok kezelése. Ez a témakör bemutatja az SSMS gyakori műveletek. A kiszolgáló és első lépések az Azure SQL-adatbázisban létrehozott adatbázis már kell rendelkeznie. További információt talál [az első Azure SQL-adatbázis létrehozása](sql-database-get-started.md) és a [Csatlakozás és a lekérdezések SSMS](sql-database-connect-query-ssms.md) .

Azure SQL-adatbázis használatakor SSMS legújabb verziójának használata ajánlott. 

> [AZURE.IMPORTANT] A legújabb SSMS mindig akkor használható, mert a folyamatosan elérését, a legújabb frissítésekben és Azure SQL-adatbázis használata. A legújabb verzióra című témakörben kaphat [Letöltése az SQL Server Management Studio eszközben](https://msdn.microsoft.com/library/mt238290.aspx).



## <a name="create-and-manage-azure-sql-databases"></a>Hozzon létre és Azure SQL-adatbázisok kezelése

A **fő** adatbázist csatlakozva a kiszolgálón hozza létre az adatbázisokat és módosítása, vagy húzza a létező adatbázisokat. Az alábbi lépéseket ismertetik, hogyan lehet több közös adatbázis felügyeleti feladatok elvégzéséhez Management Studio keresztül. Ezeket a műveleteket, ellenőrizze, hogy csatlakozva van a **Diaminta** a kiszolgálói szintű fő jelentkezzen be a kiszolgáló beállításakor létrehozott adatbázis.

Management Studio egy lekérdezés ablak megnyitásához nyissa meg az adatbázisok mappát, bontsa ki a **Rendszer adatbázisok** mappát, kattintson a jobb gombbal a **fő**, és kattintson az **Új lekérdezést**.

-   Az **Adatbázis létrehozása** utasítás használatával hozzon létre egy adatbázist. További tudnivalókért lásd: az [Adatbázis létrehozása (SQL-adatbázis)](https://msdn.microsoft.com/library/dn268335.aspx). Az alábbi utasítás egy **myTestDB** nevű adatbázist hoz létre, és azt határozza meg, hogy egy alapértelmezett maximális mérete 250 GB Standard S0 Edition adatbázis.

        CREATE DATABASE myTestDB
        (EDITION='Standard',
         SERVICE_OBJECTIVE='S0');

Kattintson a **végrehajtás** futtassa a lekérdezést.

-   Az **Adatbázis módosítása** utasítás használatával módosíthatja a meglévő adatbázis, például ha meg szeretné változtatni a nevét és az adatbázis kiadását. További tudnivalókért lásd: [Az adatbázis módosítása (SQL-adatbázis)](https://msdn.microsoft.com/library/ms174269.aspx). Az alábbi utasítás módosítja az adatbázist, ha módosítani szeretné az előző lépésben létrehozott Standard S1 edition.

        ALTER DATABASE myTestDB
        MODIFY
        (SERVICE_OBJECTIVE='S1');

-   **Adatbázis LEVÁLASZTÁSA** utasítás használata egy meglévő adatbázis törlése. További tudnivalókért lásd: az [Adatbázis LEVÁLASZTÁSA (SQL-adatbázis)](https://msdn.microsoft.com/library/ms178613.aspx). Az alábbi utasítás törli az **myTestDB** adatbázis, de nem engedje el most mert szüksége lesz rájuk bejelentkezések létrehozása a következő lépésben.

        DROP DATABASE myTestBase;

-   A fő adatbázist az összes adatbázis adatainak megjelenítéséhez használható **sys.databases** nézet tartalmaz. Meglévő adatbázisokra megtekintéséhez hajtsa végre a következő utasítást:

        SELECT * FROM sys.databases;

-   SQL-adatbázisban a **használati** utasítás nem támogatott adatbázisok közötti váltáshoz. Ehelyett kell közvetlenül a céladatbázisban kapcsolatot létesíteni.

>[AZURE.NOTE] A Transact-SQL-utasítások, amely létrehozása vagy módosítása az adatbázis számos saját köteg belül kell futtatni, és nem kell csoportosítani, egyéb Transact-SQL-utasításait a. További tudnivalókért lásd: a kimutatás adatait.

## <a name="create-and-manage-logins"></a>Létrehozhatja és kezelheti a bejelentkezések

A **fő** adatbázist bejelentkezések tartalmazza, és mely bejelentkezések létrehozására jogosító engedéllyel-adatbázisok és más bejelentkezési. A kiszolgálói szintű fő jelentkezzen be a kiszolgáló beállításakor létrehozott és a **fő** adatbázishoz való csatlakozáskor bejelentkezések kezelheti. A fő adatbázissal, a teljes kiszolgálón keresztül bejelentkezések kezelő lekérdezések végrehajtani a **Bejelentkezési létrehozása**, **Módosítása bejelentkezési**vagy **HÚZHATJA a jelentkezzen be** kimutatásokat is használhatja. További tudnivalókért lásd: [az adatbázisok kezelése és bejelentkezések SQL-adatbázisban](http://msdn.microsoft.com/library/azure/ee336235.aspx). 


-   A **Bejelentkezési létrehozása** utasítás használatával hozzon létre egy kiszolgálói szintű bejelentkezést. További tudnivalókért lásd: [Bejelentkezés létrehozása (SQL-adatbázis)](https://msdn.microsoft.com/library/ms189751.aspx). Az alábbi utasítás létrehoz egy bejelentkezési neve **login1**. Cserélje le **Jelszo1** lehetőség a jelszót.

        CREATE LOGIN login1 WITH password='password1';

-   A **CREATE USER** utasítás segítségével adatbázis szintű engedélyek kiosztása. Az összes bejelentkezések kell létrehozni a **fő** adatbázist. A bejelentkezés egy másik adatbázishoz való csatlakozáshoz csak adja meg azt adatbázis-engedélyek beállítása a **CREATE USER** utasítás használata az adatbázist. További tudnivalókért lásd: [Felhasználó létrehozása (SQL-adatbázis)](https://msdn.microsoft.com/library/ms173463.aspx). 

-   Login1 engedélyezhetik **myTestDB**nevű adatbázis, hajtsa végre az alábbi lépéseket:

 1.  Megtekintheti az Ön által létrehozott **myTestDB** adatbázis-objektum tallózó frissítéséhez kattintson a jobb gombbal a kiszolgáló nevét, az objektum Explorerben, és kattintson a **frissítés**gombra.  

     Ha bezárta a kapcsolatot, akkor kattintson a Fájl menü **Objektum Explorer Csatlakozás** kiválasztásával újra.

 2. Kattintson a jobb gombbal a **myTestDB** adatbázist, és válassza az **Új lekérdezést**.

    3.  Hajtsa végre az alábbi utasítás hozhat létre, amely megfelel a kiszolgálói szintű bejelentkezési **login1** **login1User** nevű adatbázist felhasználót myTestDB adatbázison.

            CREATE USER login1User FROM LOGIN login1;

-   Használja a **sp\_addrolemember** tárolt eljárás adjon a felhasználói fiókot az engedélyeket az adatbázist a megfelelő szintű. További tudnivalókért lásd: [sp_addrolemember (Transact-SQL nyelvben)](http://msdn.microsoft.com/library/ms187750.aspx). Az alábbi utasítás engedélyekkel ruházza fel **login1User** csak olvasható az adatbázis **login1User** való hozzáadásával a **db\_datareader** szerepkör.

        exec sp_addrolemember 'db_datareader', 'login1User';    

-   A **Bejelentkezési ALTER** utasítás használatával módosíthatja a meglévő bejelentkezések, például ha meg szeretné változtatni a bejelentkezési jelszavát. További tudnivalókért lásd: [ALTER bejelentkezési (SQL-adatbázis)](https://msdn.microsoft.com/library/ms189828.aspx). A **Bejelentkezési ALTER** utasítás kell futtatni a **fő** adatbázissal. Váltson vissza a lekérdezés ablak, amely arra az adatbázisra csatlakozik. Az alábbi utasítás módosítja a **login1** jelentkezzen be a jelszó alaphelyzetbe állítása. **ÚjJelszó** cserélje le a választási lehetőségek, és **Régi_jelszó** az aktuális jelszót a bejelentkezés a jelszavát.

        ALTER LOGIN login1
        WITH PASSWORD = 'newPassword'
        OLD_PASSWORD = 'oldPassword';

-   **Az EGÉRREL bejelentkezés** utasítás segítségével törölheti a meglévő bejelentkezések. A bejelentkezési kiszolgáló szintre törlésével is törli a bármely társított adatbázis-felhasználói fiókok. További tudnivalókért lásd: az [Adatbázis LEVÁLASZTÁSA (SQL-adatbázis)](https://msdn.microsoft.com/library/ms178613.aspx). A **Bejelentkezési DROP** utasítás kell futtatni a **fő** adatbázissal. A kimutatás törli a **login1** bejelentkezést.

        DROP LOGIN login1;

-   A fő adatbázist a **sys.sql\_bejelentkezések** megtekintheti, hogy segítségével bejelentkezések megtekintése. Minden meglévő bejelentkezések megtekintéséhez hajtsa végre a következő utasítást:

        SELECT * FROM sys.sql_logins;

## <a name="monitor-sql-database-using-dynamic-management-views"></a>Dinamikus kezelése nézetek használata SQL-adatbázis figyelése

SQL-adatbázis több dinamikus kezelése nézetet, amelyek egy adott adatbázis Lync támogatja. Ezek a nézetek között meghallgathatja monitor adattípus néhány példa követik. Részletes adatainak és további példa a használatra című témakörben talál [figyelése SQL-adatbázis használata a dinamikus kezelése nézeteket](https://msdn.microsoft.com/library/azure/ff394114.aspx).

-   Dinamikus kezelése nézet lekérdezése és a **VIEW adatbázis STATE** engedéllyel kell rendelkeznie. A **VIEW adatbázis STATE** engedélyt egy adott adatbázis-felhasználó, csatlakozni az adatbázishoz, és hajtsa végre a következő utasítást az adatbázis adatainak ellenőrzése:

        GRANT VIEW DATABASE STATE TO login1User;

-   Kiszámítja az adatbázis mérete használja, a **sys.dm\_db\_partition\_stat** megtekintése. A **sys.dm\_db\_partition\_stat** megtekintése az adatbázisban, amely kiszámítja az adatbázis mérete is használhatja az összes partíciót lap és a sor-szám információk adja eredményül. Az alábbi lekérdezés megabájtban adja vissza az adatbázis mérete:

        SELECT SUM(reserved_page_count)*8.0/1024
        FROM sys.dm_db_partition_stats;   

-   Használja a **sys.dm\_végrehajtási\_kapcsolatok** és **sys.dm\_végrehajtási\_munkamenetek** nézetek lekérdezni az aktuális felhasználói kapcsolatok és az adatbázis társított belső feladatok információt. A következő lekérdezés a jelenlegi kapcsolatra vonatkozó információkat adja eredményül:

        SELECT
            e.connection_id,
            s.session_id,
            s.login_name,
            s.last_request_end_time,
            s.cpu_time
        FROM
            sys.dm_exec_sessions s
            INNER JOIN sys.dm_exec_connections e
              ON s.session_id = e.session_id;

-   Használja a **sys.dm\_végrehajtási\_lekérdezés\_stat** összesítő Teljesítménystatisztikák számára beolvasása nézet gyorsítótárazott lekérdezés tervek. A következő lekérdezés a átlagos Processzor időpontonként rangsorban legfelső öt lekérdezésekkel kapcsolatos információkat adja eredményül.

        SELECT TOP 5 query_stats.query_hash AS "Query Hash",
            SUM(query_stats.total_worker_time), SUM(query_stats.execution_count) AS "Avg CPU Time",
            MIN(query_stats.statement_text) AS "Statement Text"
        FROM
            (SELECT QS.*,
            SUBSTRING(ST.text, (QS.statement_start_offset/2) + 1,
            ((CASE statement_end_offset
                WHEN -1 THEN DATALENGTH(ST.text)
                ELSE QS.statement_end_offset END
                    - QS.statement_start_offset)/2) + 1) AS statement_text
             FROM sys.dm_exec_query_stats AS QS
             CROSS APPLY sys.dm_exec_sql_text(QS.sql_handle) as ST) as query_stats
        GROUP BY query_stats.query_hash
        ORDER BY 2 DESC;
 
 
