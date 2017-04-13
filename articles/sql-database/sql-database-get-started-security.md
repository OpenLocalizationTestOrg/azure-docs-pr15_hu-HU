<properties
    pageTitle="SQL-adatbázis oktatóprogram: biztonsági – első lépések"
    description="Megtudhatja, hogy miként hozhat létre felhasználói fiókot, elérésére és kezelése egy adatbázisban."
    keywords=""
    services="sql-database"
    documentationCenter=""
    authors="CarlRabeler"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="08/17/2016"
    ms.author="carlrab"/>

# <a name="sql-database-tutorial-create-sql-database-user-accounts-to-access-and-manage-a-database"></a>SQL-adatbázis oktatóprogram: SQL-adatbázis eléréséhez, és egy adatbázis kezelése a felhasználói fiókok létrehozása


> [AZURE.SELECTOR]
- [Első lépések oktatóprogram](sql-database-get-started-security.md)
- [Engedélyek biztosítása](sql-database-manage-logins.md)

Ebből az oktatóanyagból megismerheti, hogyan SQL Server Management Studio (SSMS) való használatára:

- Jelentkezzen be egy kiszolgálói szintű fő bejelentkezés használata SQL-adatbázishoz.
- SQL-adatbázis-felhasználói fiók létrehozása.
- Egy SQL-adatbázis felhasználói [db_owner engedélyek](https://msdn.microsoft.com/library/ms189121.aspx#Anchor_0)kiosztása.
- Csatlakozás egy felhasználói fiókhoz, nem a kiszolgálói szintű rendszerbiztonsági tag SQL-adatbázishoz.

[AZURE.INCLUDE [Login](../../includes/azure-getting-started-portal-login.md)]


[AZURE.INCLUDE [Create SQL Database logical server](../../includes/sql-database-sql-server-management-studio-connect-server-principal.md)]


[AZURE.INCLUDE [Create SQL Database database](../../includes/sql-database-create-new-database-user.md)]


[AZURE.INCLUDE [Create SQL Database database](../../includes/sql-database-grant-database-user-dbo-permissions.md)]


[AZURE.INCLUDE [Create SQL Database database](../../includes/sql-database-sql-server-management-studio-connect-user.md)]


## <a name="next-steps"></a>Következő lépések
Most, hogy befejeződött SQL-adatbázis oktatóanyag és létrehozott egy felhasználói fiókot, és engedéllyel a felhasználói fiók dbo, készen áll a további tudnivalók az [SQL-adatbázis biztonsági](sql-database-manage-logins.md).


