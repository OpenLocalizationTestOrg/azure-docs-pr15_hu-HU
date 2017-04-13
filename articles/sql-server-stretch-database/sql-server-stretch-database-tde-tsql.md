<properties
   pageTitle="Az SQL Server-Nyújtás adatbázis Azure TSQL átlátszó titkosítás (TDE) engedélyezéséhez |} Microsoft Azure"
   description="Az SQL Server-Nyújtás adatbázis Azure TSQL átlátszó titkosítás (TDE) engedélyezéséhez"
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
   ms.author="douglaslMS"/>

# <a name="enable-transparent-data-encryption-tde-for-stretch-database-on-azure-transact-sql"></a>Áttetsző adatok titkosítási (TDE) engedélyezése Nyújtás adatbázis Azure (Transact-SQL)
> [AZURE.SELECTOR]
- [Azure portál](sql-server-stretch-database-encryption-tde.md)
- [TSQL](sql-server-stretch-database-tde-tsql.md)

Áttetsző adatok titkosítási (TDE) segítségével rosszindulatú tevékenység veszélyével vírusvédelmet anélkül, hogy az alkalmazás módosításainak valós idejű titkosítás és az adatbázis, társított biztonsági mentés és a többi tranzakció naplófájlok visszafejtés végrehajtásával.

TDE titkosítja a teljes adatbázisra tárolására nevű adatbázist titkosítókulcs szimmetrikus kulcs használatával. Az adatbázis titkosítókulcs beépített kiszolgálói tanúsítvány védi. A beépített kiszolgálói tanúsítvány egyediségét minden Azure-kiszolgálóhoz. A Microsoft hozzávetőlegesen minden 90 napon automatikusan elforgatása tanúsítványok. TDE általános leírása olvassa el a [Átlátszó adatok titkosítási (TDE)]című témakört.

##<a name="enabling-encryption"></a>Titkosítás engedélyezése

TDE ahhoz, hogy az Azure-adatbázist, amely a adattárolás áttelepített Nyújtás engedélyező SQL Server-adatbázishoz, tegye a következőket:

1. A *fő* az adatbázist, amely egy rendszergazdának vagy a fő adatbázist az **dbmanager** szerepköre tagja bejelentkezési kiszolgálójához Azure-adatbázis csatlakoztatása
2. Hajtsa végre a következő utasítást, az adatbázis titkosítása.

```sql
ALTER DATABASE [database_name] SET ENCRYPTION ON;
```

##<a name="disabling-encryption"></a>Titkosítás letiltása

Tiltsa le a TDE az Azure adatbázist, amely a adattárolás áttelepített Nyújtás engedélyező SQL Server-adatbázishoz, tegye a következőket:

1. A *fő* adatbázist, amely egy rendszergazdának vagy a fő adatbázist az **dbmanager** szerepköre tagja bejelentkezési csatlakoztatása
2. Hajtsa végre a következő utasítást, az adatbázis titkosítása.

```sql
ALTER DATABASE [database_name] SET ENCRYPTION OFF;
```

##<a name="verifying-encryption"></a>Titkosítási ellenőrzése

Titkosítás állapota, amely a adattárolás Azure adatbázisok áttelepítését a Nyújtás engedélyező SQL Server-adatbázis ellenőrzéséhez végezze el az alábbiakat:

1. Csatlakozás a *fő* vagy példány adatbázishoz, amely egy rendszergazdának vagy a fő adatbázist az **dbmanager** szerepköre tagja bejelentkezési használatával
2. Hajtsa végre a következő utasítást, az adatbázis titkosítása.

```sql
SELECT
    [name],
    [is_encrypted]
FROM
    sys.databases;
```

Eredmény ```1``` azt jelzi, egy titkosított adatbázis ```0``` nem titkosított adatbázis jelzi.


<!--Anchors-->
[Áttetsző adatok titkosítási (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx


<!--Image references-->

<!--Link references-->
