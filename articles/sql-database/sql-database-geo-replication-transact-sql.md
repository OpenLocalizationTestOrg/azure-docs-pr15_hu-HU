<properties
    pageTitle="A Transact-SQL Azure SQL-adatbázis Geo replikációs beállítása |} Microsoft Azure"
    description="Azure SQL-adatbázis használata a Transact-SQL nyelvben Geo-replikáció konfigurálása"
    services="sql-database"
    documentationCenter=""
    authors="CarlRabeler"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"
    ms.workload="NA"
    ms.date="10/13/2016"
    ms.author="carlrab"/>

# <a name="configure-geo-replication-for-azure-sql-database-with-transact-sql"></a>A replikáció Geo beállítása a Transact-SQL Azure SQL-adatbázis

> [AZURE.SELECTOR]
- [– Áttekintés](sql-database-geo-replication-overview.md)
- [Azure portál](sql-database-geo-replication-portal.md)
- [A PowerShell](sql-database-geo-replication-powershell.md)
- [AZ SQL-T](sql-database-geo-replication-transact-sql.md)

Ez a cikk bemutatja, hogyan a szolgáltatáshoz való konfigurálása az Active Geo replikációs Azure SQL-adatbázis Transact-SQL nyelvben.

Kezdeményezhet feladatátvevő Transact-SQL nyelvben használatával, olvassa el [a Transact-SQL Azure SQL-adatbázis tervezett vagy nem tervezett feladatátvevő kezdeményezhet](sql-database-geo-replication-failover-transact-sql.md).

>[AZURE.NOTE] Aktív Geo replikációs (olvasható formátumú másodlagos zónák) az összes szolgáltatási rétegek adatbázisokra most érhető el. Április 2017 a másodlagos nem olvasható típusa már inaktív, és nem olvasható létező adatbázisokat olvasható formátumú másodlagos zónák automatikusan frissülnek.

Aktív Geo-replikáció konfigurálása Transact-SQL nyelvben használata esetén szüksége van a következőket:

- Egy Azure-előfizetést.
- Egy logikai Azure SQL-adatbázis kiszolgálója <MyLocalServer> és SQL-adatbázis <MyDB> – az elsődleges adatbázist, amelynek a való replikáció.
- Egy vagy több logikai Azure SQL-adatbázis kiszolgálók < MySecondaryServer(n) > – a logikai, amely a partner kiszolgálók másodlagos adatbázisok hoz létre.
- Az elsődleges, DBManager található bejelentkezési van db_ownership a helyi adatbázis lehetővé teszi a geo-replikáció, és a partnerek kiszolgálói, amelyhez a replikáció Geo fog konfigurálni DBManager.
- SQL Server Management Studio (SSMS)

> [AZURE.IMPORTANT] Javasoljuk, hogy mindig a legújabb verzióját használja Management Studio maradjon szinkronizálja a frissítéseket és a Microsoft Azure SQL-adatbázis. Az [SQL Server Management Studio frissítése](https://msdn.microsoft.com/library/mt238290.aspx).


## <a name="add-secondary-database"></a>Másodlagos adatbázis hozzáadása

Az **Adatbázis módosítása** utasítás hozzon létre a másodlagos geo replikált adatbázist a partner kiszolgálón is használhatja. Ez az utasítás hajtsa végre a fő adatbázist az kell replikált adatbázist tartalmazó kiszolgáló. A geo replikált adatbázist (a "elsődleges adatbázis") fog rendelkezni neve megegyezik a replikált adatbázist, és alapértelmezés szerint lesz az elsődleges adatbázis azonos szolgáltatás szintre. A másodlagos adatbázis lehet olvasható vagy nem olvashatók, és egy adatbázist, és egy rugalmas databbase is lehet. További tudnivalókért lásd: [Az adatbázis módosítása (Transact-SQL nyelvben)](https://msdn.microsoft.com/library/mt574871.aspx) és [Szolgáltatási rétegek](sql-database-service-tiers.md).
A másodlagos adatbázis létrejön, és rendezés, miután adatok elindul az elsődleges adatbázisból aszinkron replikálása. Az alábbi lépéseket ismertetik, hogyan lehet a replikáció Geo konfigurálása Management Studio segítségével. Nem olvasható és olvasható formátumú másodlagos zónák, egy adatbázist, vagy egy rugalmas adatbázis létrehozásának lépései találhatók.

> [AZURE.NOTE] Ha egy adatbázis neve megegyezik az elsődleges adatbázist az adott partner kiszolgálón létezik a parancs sikertelen lesz.


### <a name="add-non-readable-secondary-single-database"></a>Nem olvasható másodlagos (egyetlen adatbázis) hozzáadása

Egyetlen adatbázisként nem olvasható másodlagos létrehozásához kövesse az alábbi lépéseket.

1. Verziójával 13.0.600.65 vagy újabb verzió az SQL Server Management Studio eszközben.

     > [AZURE.IMPORTANT] Töltse le az SQL Server Management Studio [legújabb](https://msdn.microsoft.com/library/mt238290.aspx) verzióját. Mindig a legújabb Management Studio való használható marad az Azure-portálra frissítések szinkronban ajánlott.


2. Nyissa meg az adatbázisok mappát, bontsa ki a **Rendszer adatbázisok** mappát, kattintson a jobb gombbal a **fő**, és kattintson az **Új lekérdezést**.

3. Segítségével a következő **Adatbázis módosítása** utasítás helyi adatbázist be egy Geo replikációs elsődleges, a hol MySecondaryServer1-e a rövid kiszolgálónév MySecondaryServer1 nem olvasható másodlagos adatbázissal.

        ALTER DATABASE <MyDB>
           ADD SECONDARY ON SERVER <MySecondaryServer1> WITH (ALLOW_CONNECTIONS = NO);

4. Kattintson a **végrehajtás** futtassa a lekérdezést.


### <a name="add-readable-secondary-single-database"></a>Adja hozzá a olvasható másodlagos (egyetlen adatbázis)
Egyetlen adatbázisként olvasható másodlagos létrehozásához kövesse az alábbi lépéseket.

1. Management Studio eszközben kapcsolódni a logikai Azure SQL-adatbázis-kiszolgálóhoz.

2. Nyissa meg az adatbázisok mappát, bontsa ki a **Rendszer adatbázisok** mappát, kattintson a jobb gombbal a **fő**, és kattintson az **Új lekérdezést**.

3. Segítségével a következő **Adatbázis módosítása** utasítás helyi adatbázist egy Geo replikációs az elsődleges, másodlagos kiszolgálón olvasható másodlagos adatbázissal.

        ALTER DATABASE <MyDB>
           ADD SECONDARY ON SERVER <MySecondaryServer2> WITH (ALLOW_CONNECTIONS = ALL);

4. Kattintson a **végrehajtás** futtassa a lekérdezést.



### <a name="add-non-readable-secondary-elastic-database"></a>Nem olvasható másodlagos (rugalmas adatbázis) hozzáadása

Egy rugalmas adatbázis-olvasható másodlagos létrehozásához kövesse az alábbi lépéseket.

1. Management Studio eszközben kapcsolódni a logikai Azure SQL-adatbázis-kiszolgálóhoz.

2. Nyissa meg az adatbázisok mappát, bontsa ki a **Rendszer adatbázisok** mappát, kattintson a jobb gombbal a **fő**, és kattintson az **Új lekérdezést**.

3. Segítségével a következő **Adatbázis módosítása** utasítás helyi adatbázist egy Geo replikációs az elsődleges, másodlagos kiszolgálón egy rugalmas készletben nem olvasható másodlagos adatbázissal.

        ALTER DATABASE <MyDB>
           ADD SECONDARY ON SERVER <MySecondaryServer3> WITH (ALLOW_CONNECTIONS = NO
           , SERVICE_OBJECTIVE = ELASTIC_POOL (name = MyElasticPool1));

4. Kattintson a **végrehajtás** futtassa a lekérdezést.



### <a name="add-readable-secondary-elastic-database"></a>Adja hozzá a olvasható másodlagos (rugalmas adatbázis)
Egy rugalmas adatbázisként olvasható másodlagos létrehozásához kövesse az alábbi lépéseket.

1. Management Studio eszközben kapcsolódni a logikai Azure SQL-adatbázis-kiszolgálóhoz.

2. Nyissa meg az adatbázisok mappát, bontsa ki a **Rendszer adatbázisok** mappát, kattintson a jobb gombbal a **fő**, és kattintson az **Új lekérdezést**.

3. Segítségével a következő **Adatbázis módosítása** utasítás helyi adatbázist egy Geo replikációs az elsődleges, másodlagos kiszolgálón egy rugalmas készletben olvasható másodlagos adatbázissal.

        ALTER DATABASE <MyDB>
           ADD SECONDARY ON SERVER <MySecondaryServer4> WITH (ALLOW_CONNECTIONS = ALL
           , SERVICE_OBJECTIVE = ELASTIC_POOL (name = MyElasticPool2));

4. Kattintson a **végrehajtás** futtassa a lekérdezést.



## <a name="remove-secondary-database"></a>Másodlagos adatbázis törlése

Az **Adatbázis módosítása** utasítás is használhatja a replikáció partnerség egy másodlagos adatbázis és elsődleges között véglegesen befejezéséhez. A fő adatbázist, amelyen a fő adatbázis található ez az utasítás végrehajtása. A kapcsolat megszüntetése után a másodlagos adatbázis lesz egy normál írási és olvasási adatbázisban. Ha a másodlagos adatbázis kapcsolat megszakad a parancsot sikerült, de a másodlagos írási és olvasási után válik helyreáll a kapcsolat. További tudnivalókért lásd: [Az adatbázis módosítása (Transact-SQL nyelvben)](https://msdn.microsoft.com/library/mt574871.aspx) és [Szolgáltatási rétegek](sql-database-service-tiers.md).

Másodlagos geo replikált eltávolítása a replikáció Geo partnerség kövesse az alábbi lépéseket.

1. Management Studio eszközben kapcsolódni a logikai Azure SQL-adatbázis-kiszolgálóhoz.

2. Nyissa meg az adatbázisok mappát, bontsa ki a **Rendszer adatbázisok** mappát, kattintson a jobb gombbal a **fő**, és kattintson az **Új lekérdezést**.

3. Az alábbi **Adatbázis módosítása** utasítás geo replikált másodlagos eltávolításához használja.

        ALTER DATABASE <MyDB>
           REMOVE SECONDARY ON SERVER <MySecondaryServer1>;

4. Kattintson a **végrehajtás** futtassa a lekérdezést.

## <a name="monitor-geo-replication-configuration-and-health"></a>Geo-replikációs konfiguráció és a rendszerállapot figyelése

Felügyeleti feladatok közé tartozik, az Geo replikációs beállításainak ellenőrzése és adatok replikációs rendszerállapot figyelése.  A fő adatbázist a **sys.dm_geo_replication_links** dinamikus szolgáló nézet használatával való replikáció hivatkozás adatbázisonként kapcsolatos adatok visszaadására logikai Azure SQL-adatbázis-kiszolgálón. Ez a nézet tartalmazza a sor az egyes elsődleges és másodlagos adatbázisok között a replikáció hivatkozásra. A **sys.dm_replication_link_status** dinamikus szolgáló nézet visszaadott sorok egyes Azure SQL-adatbázissal, amely jelenleg folytat replikációs replikációs hivatkozás is használhatja. Ide tartoznak a elsődleges és másodlagos adatbázisok. Ha egynél több folyamatos replikációs hivatkozásra az adott elsődleges adatbázis létezik, az alábbi táblázat az egyes a kapcsolatok tartalmaz egy sort. A nézet összes adatbázisok, például a logikai fő jön létre. Azonban ebben a nézetben a logikai minta lekérdezése eredménye üres. A **sys.dm_operation_status** dinamikus szolgáló nézet helyzetről, az összes adatbázis-műveletek, többek között a replikáció hivatkozások állapotának is használhatja. További tudnivalókért lásd: [sys.geo_replication_links (Azure SQL-adatbázis)](https://msdn.microsoft.com/library/mt575501.aspx), [sys.dm_geo_replication_link_status (Azure SQL-adatbázis)](https://msdn.microsoft.com/library/mt575504.aspx)és [sys.dm_operation_status (Azure SQL-adatbázis)](https://msdn.microsoft.com/library/dn270022.aspx).

Lync-Geo replikációs partnerség kövesse az alábbi lépéseket.

1. Management Studio eszközben kapcsolódni a logikai Azure SQL-adatbázis-kiszolgálóhoz.

2. Nyissa meg az adatbázisok mappát, bontsa ki a **Rendszer adatbázisok** mappát, kattintson a jobb gombbal a **fő**, és kattintson az **Új lekérdezést**.

3. Az alábbi utasítás használatával adatbázisokra Geo replikációs hivatkozásokkal megjelenik.

        SELECT database_id, start_date, modify_date, partner_server, partner_database, replication_state_desc, role, secondary_allow_connections_desc FROM [sys].geo_replication_links;

4. Kattintson a **végrehajtás** futtassa a lekérdezést.
5. Nyissa meg az adatbázisok mappát, bontsa ki a **Rendszer adatbázisok** mappát, kattintson a jobb gombbal a **MyDB**, és kattintson az **Új lekérdezést**.
6. A következő utasítást használja a replikáció késedelmes jelentések és a legutóbbi replikáció megjelenítése a másodlagos adatbázisok MyDB az időt.

        SELECT link_guid, partner_server, last_replication, replication_lag_sec FROM sys.dm_geo_replication_link_status

7. Kattintson a **végrehajtás** futtassa a lekérdezést.
8. Az alábbi utasítás használatával a legutóbbi geo replikációs műveletek társított adatbázis MyDB megjelenik.

        SELECT * FROM sys.dm_operation_status where major_resource_id = 'MyDB'
        ORDER BY start_time DESC

9. Kattintson a **végrehajtás** futtassa a lekérdezést.

## <a name="upgrade-a-non-readable-secondary-to-readable"></a>Nem olvasható másodlagos olvasható frissítése

Április 2017 a másodlagos nem olvasható típusa már inaktív, és nem olvasható létező adatbázisokat olvasható formátumú másodlagos zónák automatikusan frissülnek. Ha nem olvasható formátumú másodlagos zónák ma használ, és szeretné frissíteni szeretné olvasható, a következő egyszerű lépéseket minden másodlagos is használhatja.

> [AZURE.IMPORTANT] Nincs semmilyen módszerrel nem önkiszolgáló helyi frissítése az olvasható, hogy nem olvasható másodlagos. Ha a csak a másodlagos leválaszt, akkor az elsődleges adatbázis marad védett mindaddig, amíg az új másodlagos teljesen szinkronizálja a rendszer. Ha az alkalmazás SLA igényel, hogy az elsődleges mindig védett, érdemes egy másik server párhuzamos másodlagos létrehozása a fenti lépéseket a frissítés alkalmazása előtt. Megjegyzés: egyes elsődleges 4 másodlagos adatbázisok állhat.


1. Első lépésként *másodlagos* kapcsolódni, és nem olvasható másodlagos adatbázis leválasztása:  
        
        DROP DATABASE <MyNonReadableSecondaryDB>;

2. Most az *elsődleges* Serverhez való csatlakozás és egy új olvasható másodlagos hozzáadása

        ALTER DATABASE <MyDB>
            ADD SECONDARY ON SERVER <MySecondaryServer> WITH (ALLOW_CONNECTIONS = ALL);




## <a name="next-steps"></a>Következő lépések

- Ha többet szeretne tudni aktív Geo replikációs, lásd: - [Aktív Geo-replikáció](sql-database-geo-replication-overview.md)
- Egy üzleti folytonosságot – áttekintés és -esetek [áttekintése](sql-database-business-continuity.md) című témakörben találhat üzleti folytonosságot
