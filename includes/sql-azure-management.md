
# <a name="managing-azure-sql-database-using-sql-server-management-studio"></a>SQL Server Management Studio segítségével Azure SQL-adatbázis kezelése 

Az SQL Server Management Studio (SSMS) segítségével logikai Azure SQL-adatbázis-kiszolgáló és az adatbázisok kezelése. Ez a témakör bemutatja az SSMS gyakori műveletek. Egy logikai kiszolgáló és az első lépések készült Azure SQL-adatbázis adatbázis már kell rendelkeznie. Első lépésként olvassa el [az első Azure SQL-adatbázis létrehozása](sql-database-get-started.md) , és térjen vissza.

Azure SQL-adatbázis használatakor SSMS legújabb verziójának használata ajánlott. Látogasson el a [Letöltés SQL Server Management Studio](https://msdn.microsoft.com/en-us/library/mt238290.aspx) el. 


## <a name="connect-to-a-sql-database-logical-server"></a>Csatlakozás SQL-adatbázis logikai Serverhez

Hogy a kiszolgáló nevét, a Azure SQL-adatbázis csatlakoztatása szükséges. Szükség lehet a tájékozódáshoz jelentkezzen be a portálra.

1.  Jelentkezzen be az [Azure Kezelőportálja segítségével](http://manage.windowsazure.com).

2.  A bal oldali ablaktáblában kattintson az **SQL-adatbázisait**.

3.  Az SQL-adatbázisait a Kezdőlap lapon kattintson a **SERVERS** elemre a lap tetején a kiszolgálókat az előfizetéséhez társított összes. Keresse meg a kiszolgáló nevének a kívánt kapcsolatot, és másolja a vágólapra.

    Ezután a tűzfalbeállításokat SQL-adatbázis egy a helyi gépről kapcsolatok engedélyezésére. Ehhez az helyi gépek IP-címének hozzáadása a tűzfal kivételek listájára.

1.  SQL-adatbázisait a Kezdőlap lapon kattintson a **KISZOLGÁLÓKAT** , és válassza a a kiszolgálót, amelyhez szeretne csatlakozni.

2.  Kattintson a **beállítás** a lap tetején.

3.  Az IP-cím másolása az aktuális ügyfél IP-cím.

4.  A lap **Engedélyezett IP-címek** három mezőben megadhatja a szabály nevét és az IP-címeket tartalmazó, kezdési és befejezési értékeket tartalmazza. A szabály nevét előfordulhat, hogy adja meg annak a számítógépnek a nevét. A kezdő és záró tartomány a számítógép az IP-cím beillesztése mindkettőben, és kattintson a megjelenő jelölőnégyzetet.

    A szabály nevét egyedinek kell lennie. A fejlesztői számítógép esetén az IP-címet a IP tartomány kezdő és a IP-tartomány záró mezőben adhatja meg. Egyéb esetben lehet kell adni a kapcsolatok további számítógépekről a szervezet igazodik szélesebb az IP-címeket tartalmazó.
 
5. A lap alján a **MENTÉS** gombra.

    **Megjegyzés:** Lehetnek, hogy be, mint amennyit a tűzfal beállításai csak akkor lépnek érvénybe módosítások késleltetése 5 perc.

    Most már készen áll Management Studio segítségével SQL-adatbázishoz való csatlakozáshoz.

1.  Kattintson a tálcán kattintson a **Start**gombra, mutasson a **Minden program**, mutasson a **Microsoft SQL Server 2014-es**, és válassza az **SQL Server Management Studio eszközben**.

2.  A **Csatlakozás a kiszolgálóhoz**, adja meg a kiszolgáló teljesen minősített neve *kiszolgálónév*. database.windows.net. A Azure a kiszolgáló neve, az automatikusan létrehozott karakterlánc álló alfanumerikus karaktereket.

3.  Jelölje ki az **SQL Server-hitelesítés**.

4.  A **bejelentkezési** mezőbe írja be az SQL Server rendszergazdája bejelentkezés megadott a portálon a kiszolgáló létrehozásakor.

5.  A **jelszó** mezőbe írja be a megadott jelszót a portálon a kiszolgáló létrehozásakor.

8.  Kattintson a **Csatlakozás** a csatlakozást.

Az SQL Server 2014-es SSMS a legújabb frissítésekben például Azure SQL-adatbázisokat létrehozó és módosító tevékenységek kibontott támogatást nyújt. Emellett használhatja a Transact-SQL-utasítások elvégezheti az alábbi műveleteket. Az alábbi lépésekkel példákon ezeket az utasításokat. További információt a Transact-SQL nyelvben használata SQL-adatbázis, beleértve a Részletek arról, hogy melyik parancsok támogatottak olvassa el a [Transact-SQL referencia (SQL-adatbázis)](http://msdn.microsoft.com/library/bb510741.aspx)című témakört.

## <a name="create-and-manage-azure-sql-databases"></a>Hozzon létre és Azure SQL-adatbázisok kezelése

A **fő** adatbázist csatlakozva hozza létre az új adatbázisokat a kiszolgálón és módosítása, vagy húzza a létező adatbázisokat. Az alábbi lépéseket ismertetik, hogyan lehet több közös adatbázis felügyeleti feladatok elvégzéséhez Management Studio keresztül. Ezeket a műveleteket, ellenőrizze, hogy csatlakozva van a **Diaminta** a kiszolgálói szintű egyszerű felhasználónév állította be a kiszolgáló létrehozott adatbázis.

Management Studio egy lekérdezés ablak megnyitásához nyissa meg az adatbázisok mappát, bontsa ki a **Rendszer adatbázisok** mappát, kattintson a jobb gombbal a **fő**, és kattintson az **Új lekérdezést**.

-   Az **Adatbázis létrehozása** utasítás használatával hozzon létre új adatbázist. További tudnivalókért lásd: az [Adatbázis létrehozása (SQL-adatbázis)](https://msdn.microsoft.com/library/dn268335.aspx). Az alábbi utasítás **myTestDB** nevű új adatbázist hoz létre, és azt határozza meg, hogy egy alapértelmezett maximális mérete 250 GB Standard S0 Edition adatbázis.

        CREATE DATABASE myTestDB
        (EDITION='Standard',
         SERVICE_OBJECTIVE='S0');

Kattintson a **végrehajtás** futtassa a lekérdezést.

-   Az **Adatbázis módosítása** utasítás használatával módosíthatja a meglévő adatbázis, például ha meg szeretné változtatni a nevét és az adatbázis kiadását. További tudnivalókért lásd: [Az adatbázis módosítása (SQL-adatbázis)](https://msdn.microsoft.com/library/ms174269.aspx). Az alábbi utasítás módosítja az adatbázist, ha módosítani szeretné az előző lépésben létrehozott Standard S1 edition.

        ALTER DATABASE myTestDB
        MODIFY
        (SERVICE_OBJECTIVE='S1');

-   **Adatbázis LEVÁLASZTÁSA** utasítás használata egy meglévő adatbázis törlése.
    További tudnivalókért lásd: az [Adatbázis LEVÁLASZTÁSA (SQL-adatbázis)](https://msdn.microsoft.com/library/ms178613.aspx). Az alábbi utasítás törli az **myTestDB** adatbázis, de nem engedje el most használja, mert azt létrehozása bejelentkezések a következő lépésben.

        DROP DATABASE myTestBase;

-   A fő adatbázist az összes adatbázis adatainak megjelenítéséhez használható **sys.databases** nézet tartalmaz. Meglévő adatbázisokra megtekintéséhez hajtsa végre a következő utasítást:

        SELECT * FROM sys.databases;

-   SQL-adatbázisban a **használati** utasítás nem támogatott adatbázisok közötti váltáshoz. Ehelyett kell közvetlenül a céladatbázisban kapcsolatot létesíteni.

>[AZURE.NOTE] A Transact-SQL-utasítások, amely létrehozása vagy módosítása az adatbázis számos saját köteg belül kell futtatni, és nem kell csoportosítani, egyéb Transact-SQL-utasításait a. További tudnivalókért lásd: a kimutatás adatait a fent felsorolt hivatkozások elérhető.

## <a name="create-and-manage-logins"></a>Létrehozhatja és kezelheti a bejelentkezések

A **fő** adatbázist nyomon követi a bejelentkezések, és milyen bejelentkezések létrehozására jogosító engedéllyel adatbázisok vagy más bejelentkezések. A kiszolgálói szintű egyszerű felhasználónév állította be a kiszolgáló létrehozott és a **fő** adatbázishoz való csatlakozáskor bejelentkezések kezelheti. A fő adatbázist is fogja kezelni a bejelentkezést a teljes kiszolgálón keresztül lekérdezéseket végrehajtani a **Bejelentkezési létrehozása**, **Módosítása bejelentkezési**vagy **HÚZHATJA a jelentkezzen be** kimutatásokat is használhatja. További tudnivalókért lásd: [az adatbázisok kezelése és bejelentkezések SQL-adatbázisban](http://msdn.microsoft.com/library/azure/ee336235.aspx). 


-   A **Bejelentkezési létrehozása** utasítás használatával hozzon létre egy új kiszolgálói szintű bejelentkezést. További tudnivalókért lásd: [Bejelentkezés létrehozása (SQL-adatbázis)](https://msdn.microsoft.com/library/ms189751.aspx). Az alábbi utasítás hoz létre egy új bejelentkezési neve **login1**. Cserélje le **Jelszo1** lehetőség a jelszót.

        CREATE LOGIN login1 WITH password='password1';

-   A **CREATE USER** utasítás segítségével adatbázis szintű engedélyek kiosztása. Az összes bejelentkezések kell létrehozni a **fő** adatbázist, de egy bejelentkezés csatlakozhat egy másik adatbázist, engedélyt kell adni azt adatbázis-engedélyek beállítása a **CREATE USER** utasítás használata az adatbázist. További tudnivalókért lásd: [Felhasználó létrehozása (SQL-adatbázis)](https://msdn.microsoft.com/library/ms173463.aspx). 

-   Login1 engedélyezhetik **myTestDB**nevű adatbázis, hajtsa végre az alábbi lépéseket:

 1.  Megtekintheti az éppen létrehozott **myTestDB** adatbázis-objektum tallózó frissítéséhez kattintson a jobb gombbal a kiszolgáló nevét, az objektum Explorerben, és kattintson a **frissítés**gombra.  

     Ha bezárta a kapcsolatot, akkor kattintson a Fájl menü **Objektum Explorer Csatlakozás** kiválasztásával újra.

 2. Kattintson a jobb gombbal a **myTestDB** adatbázist, és válassza az **Új lekérdezést**.

    3.  Hajtsa végre az alábbi utasítás hozhat létre, amely megfelel a kiszolgálói szintű bejelentkezési **login1** **login1User** nevű adatbázist felhasználót myTestDB adatbázison.

            CREATE USER login1User FROM LOGIN login1;

-   Használja a **sp\_addrolemember** tárolt eljárás adjon a felhasználói fiókot az engedélyeket az adatbázist a megfelelő szintű. További tudnivalókért lásd: [sp_addrolemember (Transact-SQL nyelvben)](http://msdn.microsoft.com/library/ms187750.aspx). Az alábbi utasítás engedélyekkel ruházza fel **login1User** csak olvasható az adatbázis **login1User** való hozzáadásával a **db\_datareader** szerepkör.

        exec sp_addrolemember 'db_datareader', 'login1User';    

-   A **Bejelentkezési ALTER** utasítás használatával módosíthatja a meglévő bejelentkezések, például ha meg szeretné változtatni a bejelentkezési jelszavát. További tudnivalókért lásd: [ALTER bejelentkezési (SQL-adatbázis)](https://msdn.microsoft.com/library/ms189828.aspx). A **Bejelentkezési ALTER** utasítás kell futtatni a **fő** adatbázissal. Váltson vissza a lekérdezés ablak, amely arra az adatbázisra csatlakozik. 

    Adatvédelmi nyilatkozat módosítása a **login1** jelentkezzen be a jelszó alaphelyzetbe állítása.
    **ÚjJelszó** cserélje le a választási lehetőségek, és **Régi_jelszó** az aktuális jelszót a bejelentkezés a jelszavát.

        ALTER LOGIN login1
        WITH PASSWORD = 'newPassword'
        OLD_PASSWORD = 'oldPassword';

-   **Az EGÉRREL bejelentkezés** utasítás segítségével törölheti a meglévő bejelentkezések.
    A bejelentkezési kiszolgáló szintre törlésével is törli a bármely társított adatbázis-felhasználói fiókok. További tudnivalókért lásd: az [Adatbázis LEVÁLASZTÁSA (SQL-adatbázis)](https://msdn.microsoft.com/library/ms178613.aspx). A **Bejelentkezési DROP** utasítás kell futtatni a **fő** adatbázissal. Az alábbi utasítás törli az **login1** bejelentkezés.

        DROP LOGIN login1;

-   A fő adatbázist a **sys.sql\_bejelentkezések** megtekintheti, hogy segítségével bejelentkezések megtekintése. Minden meglévő bejelentkezések megtekintéséhez hajtsa végre a következő utasítást:

        SELECT * FROM sys.sql_logins;

## <a name="monitor-sql-database-using-dynamic-management-viewsh2"></a>Dinamikus kezelése nézetek használata SQL-adatbázis figyelése</h2>

SQL-adatbázis több dinamikus kezelése nézetet, amelyek egy adott adatbázis Lync támogatja. Alul néhány példa monitor adattípus – ezek a nézetek meghallgathatja láthatók. Részletes adatainak és további példa a használatra című témakörben talál [figyelése SQL-adatbázis használata a dinamikus kezelése nézeteket](https://msdn.microsoft.com/library/azure/ff394114.aspx).

-   Dinamikus kezelése nézet lekérdezése és a **VIEW adatbázis STATE** engedéllyel kell rendelkeznie. A **NÉZET adatbázis állam** engedélyt egy adott adatbázis-felhasználó, csatlakozzon az adatbázis kezelhetik a kiszolgálói szintű elve bejelentkezés, és hajtsa végre a következő utasítást az adatbázis adatainak ellenőrzése:

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
