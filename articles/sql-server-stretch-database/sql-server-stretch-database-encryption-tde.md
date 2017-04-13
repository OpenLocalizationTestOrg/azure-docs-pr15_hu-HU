<properties
   pageTitle="Az SQL Server Nyújtás adatbázis Azure átlátszó titkosítás (TDE) engedélyezéséhez |} Microsoft Azure"
   description="Az SQL Server Nyújtás adatbázis Azure átlátszó titkosítás (TDE) engedélyezéséhez"
   services="sql-server-stretch-database"
   documentationCenter=""
   authors="douglaslMS"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-server-stretch-database"
   ms.workload="data-management"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="06/14/2016"
   ms.author="douglaslMS"/>

# <a name="enable-transparent-data-encryption-tde-for-stretch-database-on-azure"></a>Áttetsző adatok titkosítási (TDE) engedélyezése az Azure Nyújtás adatbázis
> [AZURE.SELECTOR]
- [Azure portál](sql-server-stretch-database-encryption-tde.md)
- [TSQL](sql-server-stretch-database-tde-tsql.md)

Áttetsző adatok titkosítási (TDE) segítségével rosszindulatú tevékenység veszélyével vírusvédelmet anélkül, hogy az alkalmazás módosításainak valós idejű titkosítás és az adatbázis, társított biztonsági mentés és a többi tranzakció naplófájlok visszafejtés végrehajtásával.

TDE titkosítja a teljes adatbázisra tárolására nevű adatbázist titkosítókulcs szimmetrikus kulcs használatával. Az adatbázis titkosítókulcs beépített kiszolgálói tanúsítvány védi. A beépített kiszolgálói tanúsítvány egyediségét minden Azure-kiszolgálóhoz. A Microsoft hozzávetőlegesen minden 90 napon automatikusan elforgatása tanúsítványok. TDE általános leírása olvassa el a [Átlátszó adatok titkosítási (TDE)]című témakört.

##<a name="enabling-encryption"></a>Titkosítás engedélyezése

TDE ahhoz, hogy az Azure-adatbázist, amely a adattárolás áttelepített Nyújtás engedélyező SQL Server-adatbázishoz, tegye a következőket:

1. Nyissa meg az adatbázist az [Azure portál](https://portal.azure.com)
2. Az adatbázis lap kattintson a **Beállítások** gomb
3. Jelölje ki a **átlátszó adatok titkosítási** lehetőséget.![][1]
4. Jelölje ki **a** beállítást, és válassza a **Mentés**
![][2]


##<a name="disabling-encryption"></a>Titkosítás letiltása

Tiltsa le a TDE az Azure adatbázist, amely a adattárolás áttelepített Nyújtás engedélyező SQL Server-adatbázishoz, tegye a következőket:

1. Nyissa meg az adatbázist az [Azure portál](https://portal.azure.com)
2. Az adatbázis lap kattintson a **Beállítások** gomb
3. Jelölje ki a **átlátszó adatok titkosítási** lehetőséget.
4. Jelölje ki a **Kikapcsolva** beállítást, és válassza a **Mentés**




<!--Anchors-->
[Áttetsző adatok titkosítási (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx


<!--Image references-->
[1]: ./media/sql-server-stretch-database-encryption-tde/stretchtde1.png
[2]: ./media/sql-server-stretch-database-encryption-tde/stretchtde2.png


<!--Link references-->
