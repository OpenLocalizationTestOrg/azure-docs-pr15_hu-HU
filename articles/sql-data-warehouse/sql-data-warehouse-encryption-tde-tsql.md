<properties
   pageTitle="Az SQL-adatraktár (T-SQL) átlátszó adatok titkosítása |} Microsoft Azure"
   description="Áttetsző adatok titkosítás (TDE) SQL adatraktár (T – SQL)"
   services="sql-data-warehouse"
   documentationCenter=""
   authors="ronortloff"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.workload="data-management"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="09/24/2016"
   ms.author="rortloff;barbkess;sonyama"/>

# <a name="get-started-with-transparent-data-encryption-tde"></a>Első lépések az áttetsző adatok titkosítási (TDE)


> [AZURE.SELECTOR]
- [Biztonsági – áttekintés](sql-data-warehouse-overview-manage-security.md)
- [Hitelesítés](sql-data-warehouse-authentication.md)
- [Titkosítás (Portal)](sql-data-warehouse-encryption-tde.md)
- [Titkosítás (T – SQL)](sql-data-warehouse-encryption-tde-tsql.md)

## <a name="required-permssions"></a>Szükséges engedélyekkel

Ahhoz, hogy az áttetsző adatok titkosítási (TDE), a rendszergazda vagy a dbmanager szerepkör tagjának kell lennie.

## <a name="enabling-encryption"></a>Titkosítás engedélyezése

Kövesse ezeket a lépéseket követve egy SQL adatraktár TDE engedélyezése:

1. A *fő* adatbázist, amely egy rendszergazdának vagy a fő adatbázist az **dbmanager** szerepköre tagja bejelentkezési használatával tároló kiszolgáló-adatbázis csatlakoztatása
2. Hajtsa végre a következő utasítást, az adatbázis titkosítása.

```sql
ALTER DATABASE [AdventureWorks] SET ENCRYPTION ON;
```

## <a name="disabling-encryption"></a>Titkosítás letiltása

Kövesse ezeket a lépéseket követve egy SQL adatraktár TDE letiltása:

1. A *fő* adatbázist, amely egy rendszergazdának vagy a fő adatbázist az **dbmanager** szerepköre tagja bejelentkezési csatlakoztatása
2. Hajtsa végre a következő utasítást, az adatbázis titkosítása.

```sql
ALTER DATABASE [AdventureWorks] SET ENCRYPTION OFF;
```

> [AZURE.NOTE] A szüneteltetett SQL adatraktár TDE beállításainak módosítása előtt kell folytatható.

## <a name="verifying-encryption"></a>Titkosítási ellenőrzése

Egy SQL adatraktár titkosítási állapotának ellenőrzéséhez kövesse az alábbi lépéseket:

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

## <a name="encryption-dmvs"></a>Titkosítási DMVs  

- [sys.Databases][] 
- [sys.dm_pdw_nodes_database_encryption_keys][]


<!--Anchors-->
[Transparent Data Encryption (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx
[sys.Databases]: http://msdn.microsoft.com/library/ms178534.aspx  
[sys.dm_pdw_nodes_database_encryption_keys]: https://msdn.microsoft.com/library/mt203922.aspx  

<!--Image references-->

<!--Link references-->
