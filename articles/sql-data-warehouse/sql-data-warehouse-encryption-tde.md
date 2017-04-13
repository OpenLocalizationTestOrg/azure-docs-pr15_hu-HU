<properties
   pageTitle="Az SQL-adatraktár (Portal) átlátszó adatok titkosítása |} Microsoft Azure"
   description="SQL-adatraktár átlátszó adatok titkosítás (TDE)"
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

# <a name="get-started-with-transparent-data-encryption-tde-in-sql-data-warehouse"></a>Az áttetsző adatok titkosítási (TDE) SQL adatraktár az első lépések

> [AZURE.SELECTOR]
- [Biztonsági – áttekintés](sql-data-warehouse-overview-manage-security.md)
- [Hitelesítés](sql-data-warehouse-authentication.md)
- [Titkosítás (Portal)](sql-data-warehouse-encryption-tde.md)
- [Titkosítás (T – SQL)](sql-data-warehouse-encryption-tde-tsql.md)

## <a name="required-permssions"></a>Szükséges engedélyekkel

Ahhoz, hogy az áttetsző adatok titkosítási (TDE), a rendszergazda vagy a dbmanager szerepkör tagjának kell lennie.

## <a name="enabling-encryption"></a>Titkosítás engedélyezése

Ha engedélyezni szeretné TDE egy SQL adatraktár, kövesse az alábbi lépéseket:

1. Nyissa meg az adatbázist az [Azure portál](https://portal.azure.com)
2. Az adatbázis lap kattintson a **Beállítások** gomb
3. Jelölje ki a **átlátszó adatok titkosítási** lehetőséget.![][1]
4. Engedélyezze **a**![][2]
5. Válassza a **Mentés**
![][3]  

## <a name="disabling-encryption"></a>Titkosítás letiltása

Egy SQL adatraktár TDE letiltásához kövesse az alábbi lépéseket:

1. Nyissa meg az adatbázist az [Azure portál](https://portal.azure.com)
2. Az adatbázis lap kattintson a **Beállítások** gomb
3. Jelölje ki a **átlátszó adatok titkosítási** lehetőséget.![][1]
4. Jelölje ki a **Kikapcsolva** beállítást![][4]
5. Válassza a **Mentés**
![][5]  

## <a name="encryption-dmvs"></a>Titkosítási DMVs

A következő DMVs a titkosítási lehet erősíteni:

- [sys.Databases]
- [sys.dm_pdw_nodes_database_encryption_keys]

<!--MSDN references-->
[Transparent Data Encryption (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx
[sys.Databases]: http://msdn.microsoft.com/library/ms178534.aspx
[sys.dm_pdw_nodes_database_encryption_keys]: https://msdn.microsoft.com/library/mt203922.aspx

<!--Image references-->
[1]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings.png
[2]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-on.png
[3]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-save.png
[4]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-off.png
[5]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-save2.png

<!--Link references-->
