<properties
    pageTitle="Másolja az Azure portálon Azure SQL-adatbázisból |} Microsoft Azure"
    description="Azure SQL-adatbázishoz másolatának létrehozása"
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



# <a name="copy-an-azure-sql-database-using-the-azure-portal"></a>Másolja az Azure SQL-adatbázissal az Azure portál használatával

> [AZURE.SELECTOR]
- [– Áttekintés](sql-database-copy.md)
- [Azure portál](sql-database-copy-portal.md)
- [A PowerShell](sql-database-copy-powershell.md)
- [AZ SQL-T](sql-database-copy-transact-sql.md)

Az alábbi lépéseket a [portálon Azure](https://portal.azure.com) SQL-adatbázis másolása ugyanazon a kiszolgálón vagy egy másik kiszolgáló módját mutatják.

Másolja az SQL-adatbázishoz, van szükség az alábbiakat:

- Egy Azure-előfizetést. Ha van szüksége az Azure előfizetéssel egyszerűen **Ingyenes PRÓBAVERZIÓT** , ez a lap tetején kattintson, és ezután térjen vissza az Ez a cikk Befejezés.
- SQL-adatbázis másolásához. Ha nem rendelkezik egy SQL-adatbázissal, hozzon létre egy az ebben a cikkben leírt lépéseket követve: [az első Azure SQL-adatbázis létrehozása](sql-database-get-started.md).


## <a name="copy-your-sql-database"></a>Másolja az SQL-adatbázis

Nyissa meg a másolni kívánt adatbázis SQL adatbázis lapján:

1.  Nyissa meg az [Azure-portálon](https://portal.azure.com).
2.  Kattintson a **További szolgáltatások** > **SQL-adatbázisait**, és válassza ki a kívánt adatbázist.
3.  SQL-adatbázis lapon kattintson a **Másolás**gombra:

    ![SQL-adatbázis](./media/sql-database-copy-portal/sql-database-copy.png)

1.  A **Másolás** lapon az alapértelmezett adatbázis nevét kapja. Írjon be egy másik nevet, ha azt szeretné, (a kiszolgálón adatbázisokra egyedieknek kell lenniük).
2.  Jelölje be a **cél kiszolgálóhoz**. A tároló kiszolgáló verziója, ahol az adatbázis-példány jön létre. Az adatbázis másolhatja ugyanazon a kiszolgálón vagy egy másik kiszolgálóra. Kiszolgálót hozhat létre, vagy jelöljön ki egy meglévő kiszolgálót a listából. 
3.  Miután a **cél kiszolgálóhoz**, a **Rugalmas adatbázis készletre**és **árak réteg** beállítások engedélyezi. Ha a kiszolgáló erőforráskészlethez tartozik, másolhatja az adatbázist abba.
3.  Kattintson az **OK gombra** a másolás megkezdéséhez.

    ![SQL-adatbázis](./media/sql-database-copy-portal/copy-page.png)


## <a name="monitor-the-progress-of-the-copy-operation"></a>A Másolás állapotának nyomon követéséhez

- A Másolás indítás után kattintson a további információt a portál értesítés.

    ![értesítés][3]
 
    ![monitor][4]


## <a name="verify-the-database-is-live-on-the-server"></a>Ellenőrizze az adatbázis élő a kiszolgálón

- Kattintson a **További szolgáltatások** > **SQL-adatbázisait** , és ellenőrizze, hogy az új adatbázis egy **Online**.


## <a name="resolve-logins"></a>Bejelentkezések feloldása

A másolási művelet befejezése után bejelentkezések feloldásához lásd: a [bejelentkezések feloldása](sql-database-copy-transact-sql.md#resolve-logins-after-the-copy-operation-completes)


## <a name="next-steps"></a>Következő lépések

- Lásd: [Azure SQL-adatbázishoz másolása](sql-database-copy.md) Azure SQL-adatbázis másolása áttekintése.
- Lásd: a PowerShell használatá adatbázis másolása [Azure SQL-adatbázishoz másolása a PowerShell használatával](sql-database-copy-powershell.md) .
- Lásd: a [Másolás egy Azure SQL-adatbázis használata az SQL-T](sql-database-copy-transact-sql.md) a Transact-SQL-adatbázisokkal másolásához.
- [Azure SQL-adatbázis biztonsági után vészhelyreállítás kezelése](sql-database-geo-replication-security-config.md) című témakörben talál tudnivalókat a felhasználók és a bejelentkezés egy másik logikai-kiszolgáló adatbázis másolásakor témakörben találhat.



## <a name="additional-resources"></a>További források

- [Bejelentkezés kezelése](sql-database-manage-logins.md)
- [Az SQL Server Management Studio SQL-adatbázis csatlakoztatása, és hajtsa végre a minta T az SQL lekérdezés](sql-database-connect-query-ssms.md)
- [Az adatbázis exportálása egy BACPAC](sql-database-export.md)
- [Üzleti folytonosságot – áttekintés](sql-database-business-continuity.md)
- [SQL-adatbázis dokumentáció](https://azure.microsoft.com/documentation/services/sql-database/)




<!--Image references-->
[1]: ./media/sql-database-copy-portal/copy.png
[2]: ./media/sql-database-copy-portal/copy-ok.png
[3]: ./media/sql-database-copy-portal/copy-notification.png
[4]: ./media/sql-database-copy-portal/monitor-copy.png

