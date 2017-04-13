<properties 
    pageTitle="Másolja a vágólapra egy Azure SQL-adatbázisokkal Transact-SQL nyelvben |} Microsoft Azure" 
    description="Másolat egy használata a Transact-SQL Azure SQL-adatbázishoz" 
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="09/19/2016"
    ms.author="sstein"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="copy-an-azure-sql-database-using-transact-sql"></a>Másolja az használata a Transact-SQL Azure SQL-adatbázishoz


> [AZURE.SELECTOR]
- [– Áttekintés](sql-database-copy.md)
- [Azure portál](sql-database-copy-portal.md)
- [A PowerShell](sql-database-copy-powershell.md)
- [AZ SQL-T](sql-database-copy-transact-sql.md)


A következő lépéseket az SQL-adatbázis Transact-SQL nyelvben másolása ugyanazon a kiszolgálón vagy egy másik kiszolgáló módját mutatják. Az adatbázis-példány művelet az [Adatbázis létrehozása](https://msdn.microsoft.com/library/ms176061.aspx) utasítást használja.

A jelen cikkben ismertetett lépések elvégzéséhez az alábbiakra van szükség:

- Egy Azure-előfizetést. Ha van szüksége az Azure előfizetéssel egyszerűen **Ingyenes PRÓBAVERZIÓT** , ez a lap tetején kattintson, és ezután térjen vissza az Ez a cikk Befejezés.
- Microsoft Azure SQL-adatbázisban. Ha nem rendelkezik egy SQL-adatbázissal, hozzon létre egy az ebben a cikkben leírt lépéseket követve: [az első Azure SQL-adatbázis létrehozása](sql-database-get-started.md).
- [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/ms174173.aspx). Ha nincs telepítve a SSMS, vagy ha a jelen cikkben ismertetett funkciók nem találhatók, [Töltse le a legújabb verzióra](https://msdn.microsoft.com/library/mt238290.aspx).


## <a name="copy-your-sql-database"></a>Másolja az SQL-adatbázis

Jelentkezzen be a fő adatbázist a kiszolgálói szintű egyszerű felhasználónév és a bejelentkezési, amely a másolni kívánt adatbázist. Bejelentkezések, amelyek nem a kiszolgálói szintű tőketörlesztés kell lennie az dbmanager szerepkör tagjai annak érdekében, hogy az adatbázisok másolja. Többet szeretne tudni a bejelentkezési adatok és a csatlakozás a kiszolgálóhoz olvassa el a [bejelentkezések kezelése](sql-database-manage-logins.md)című témakört.

Indítsa el a Másolás a forrásadatbázis az [Adatbázis létrehozása](https://msdn.microsoft.com/library/ms176061.aspx) utasítással. Az adatbázis másolásának folyamata Ez az utasítás végrehajtása kezdeményez. Mivel a adatbázis másolása egy aszinkron folyamat, az adatbázis létrehozása utasítás az adatbázis befejezése másolás előtt adja eredményül.


### <a name="copy-a-sql-database-to-the-same-server"></a>SQL-adatbázis másolása ugyanazon a kiszolgálón

Jelentkezzen be a fő adatbázist a kiszolgálói szintű egyszerű felhasználónév és a bejelentkezési, amely a másolni kívánt adatbázist. Bejelentkezések, amelyek nem a kiszolgálói szintű tőketörlesztés kell lennie az dbmanager szerepkör tagjai annak érdekében, hogy az adatbázisok másolja.

Új adatbázis megjelenítheti a parancs másolatok Database1 Database2 nevű, ugyanazon a kiszolgálón. Az adatbázis méretétől függően a Másolás eltarthat egy kis időt befejezéséhez.

    -- Execute on the master database.
    -- Start copying.
    CREATE DATABASE Database1_copy AS COPY OF Database1;

### <a name="copy-a-sql-database-to-a-different-server"></a>SQL-adatbázis másolása egy másik kiszolgálóra

Jelentkezzen be a cél kiszolgáló, az Azure SQL-adatbáziskiszolgálóhoz létrejön az új adatbázis esetén az fő adatbázist. A bejelentkezési tartalmazó ugyanazt a nevet és jelszót, az adatbázis tulajdonosa (DBO) kattintson a forrás Azure SQL-adatbázis kiszolgálója a forrásadatbázis használja. A cél kiszolgáló a bejelentkezési is a dbmanager szerepkör tagja kell vagy a kiszolgálói szintű egyszerű felhasználónév kell.

Ez a parancs a kiszolgáló1 - Database1 másolása a kiszolgáló2 Database2 nevű új adatbázis. Az adatbázis méretétől függően a Másolás eltarthat egy kis időt befejezéséhez.


    -- Execute on the master database of the target server (server2)
    -- Start copying from Server1 to Server2
    CREATE DATABASE Database1_copy AS COPY OF server1.Database1;
    

## <a name="monitor-the-progress-of-the-copy-operation"></a>A Másolás állapotának nyomon követéséhez

A másolási folyamat figyelheti a sys.databases és sys.dm_database_copies nézetek lekérdezésével. Miközben folyamatban van a másolás, a sys.databases nézetben az új adatbázis state_desc oszlopa MÁSOLÁSA van beállítva.


- Ha nem sikerül a másolás, a sys.databases nézetben az új adatbázis state_desc oszlopa gyanús értéke. Ebben az esetben a DROP utasítás végrehajtása az új adatbázist, majd próbálkozzon újra.
- Ha sikerült a másolás, a sys.databases nézetben az új adatbázis state_desc oszlopa értéke ONLINE. Ebben az esetben befejeződött a másolás és az új adatbázis rendszeres adatbázis, a forrásadatbázis független módosíthatók.

> [AZURE.NOTE] – Ha úgy dönt, hogy a Mégse gombra, a másolás, miközben folyamatban van, hajtsa végre az [Adatbázis LEVÁLASZTÁSA](https://msdn.microsoft.com/library/ms178613.aspx) utasítás az új adatbázist. Másik lehetőségként az adatbázis LEVÁLASZTÁSA utasítás végrehajtása a forrás-adatbázishoz is lemondja a másolási folyamat.


## <a name="resolve-logins-after-the-copy-operation-completes"></a>A másolási művelet befejezése után megoldani a bejelentkezési

Után az új adatbázis a cél kiszolgálón online állapotban, a [ALTER USER](https://msdn.microsoft.com/library/ms176060.aspx) utasítás segítségével a felhasználók az új adatbázisból a cél kiszolgálón bejelentkezések megfelelteti. Elárvult felhasználók feloldásához lásd: az [Elárvult felhasználók kapcsolatos hibák elhárítása](https://msdn.microsoft.com/library/ms175475.aspx). Lásd még: [Azure SQL-adatbázis biztonsági után vészhelyreállítás kezelése](sql-database-geo-replication-security-config.md).

Az új adatbázis összes felhasználója karbantartása: a engedélyekkel rendelkeznek, amelyekkel a forrásadatbázis. A felhasználó által kezdeményezett az adatbázis-példányt az adatbázis tulajdonosának az új adatbázis lesz, és hozzá van rendelve egy új biztonsági azonosító (biztonsági AZONOSÍTÓK). Miután létrejött a másolás és a más felhasználók újra vannak társítva előtt csak kezdeményező, a másolás, az adatbázis tulajdonosa (DBO), a bejelentkezési is jelentkezzen be az új adatbázist.


## <a name="next-steps"></a>Következő lépések

- Lásd: [Azure SQL-adatbázishoz másolása](sql-database-copy.md) Azure SQL-adatbázis másolása áttekintése.
- Lásd: [az Azure portálon Azure SQL-adatbázisból másolása](sql-database-copy-portal.md) az Azure portálon adatbázis másolása.
- Lásd: a PowerShell használatá adatbázis másolása [Azure SQL-adatbázishoz másolása a PowerShell használatával](sql-database-copy-powershell.md) .
- [Azure SQL-adatbázis biztonsági után vészhelyreállítás kezelése](sql-database-geo-replication-security-config.md) című témakörben talál tudnivalókat a felhasználók és a bejelentkezés egy másik logikai-kiszolgáló adatbázis másolásakor témakörben találhat.



## <a name="additional-resources"></a>További források

- [Bejelentkezés kezelése](sql-database-manage-logins.md)
- [Az SQL Server Management Studio SQL-adatbázis csatlakoztatása, és hajtsa végre a minta T az SQL lekérdezés](sql-database-connect-query-ssms.md)
- [Az adatbázis exportálása egy BACPAC](sql-database-export.md)
- [Üzleti folytonosságot – áttekintés](sql-database-business-continuity.md)
- [SQL-adatbázis dokumentáció](https://azure.microsoft.com/documentation/services/sql-database/)


