<properties
   pageTitle="Az Azure SQL-adatraktár (REST API-val) visszaállítása |} Microsoft Azure"
   description="Az Azure SQL-adatraktár visszaállításához feladatok REST API-t."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="Lakshmi1812"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/21/2016"
   ms.author="lakshmir;barbkess;sonyama"/>

# <a name="restore-an-azure-sql-data-warehouse-rest-api"></a>Az Azure SQL-adatraktár (REST API-val) visszaállítása

> [AZURE.SELECTOR]
- [– Áttekintés][]
- [Portál][]
- [A PowerShell][]
- [TÖBBI][]

Ez a cikk megtanulhatja, hogy miként állíthatja vissza az Azure SQL adatraktár a REST API.

## <a name="before-you-begin"></a>Első lépések

**Ellenőrizze a DTU kapacitása.** Minden egyes SQL adatraktár egy SQL server (pl. myserver.database.windows.net), amely egy alapértelmezett DTU kvótája üzemelteti.  Egy SQL adatraktár a visszaállítás előtt ellenőrizze, hogy a az SQL server van elegendő az adatbázist visszaállítják hátralévő DTU kvótája. Megtudhatja, hogyan számítja ki a szükséges DTU, vagy kérjen további DTU, olvassa el a [DTU kvóta módosításának kérése][]című témakört.

## <a name="restore-an-active-or-paused-database"></a>Az aktív vagy felfüggesztett adatbázis visszaállítása

Adatbázis visszaállítása:

1. A lista beszerzése az adatbázis visszaállítása pontok a Get-adatbázis visszaállítása pontok művelet használatával.
2. Kezdje el a visszaállítása az [adatbázis létrehozása][] visszaállítás kérelem használatával.
3. A visszaállítás állapotának nyomon az [adatbázis-művelet állapota][] művelet használatával.

>[AZURE.NOTE] A visszaállítás befejeződése után az alábbi [állítsa be az adatbázis helyreállítás][]helyreállított adatbázis is beállíthatja.

## <a name="restore-a-deleted-database"></a>A törölt adatbázis visszaállítása

A törölt adatbázis visszaállítása:

1.  A [lista visszaállítható kihagyott adatbázisok][] művelet segítségével összes az visszaállítható törölt adatbázisok listázása.
2.  A részletek a törölt adatbázis visszaállítása az [első visszaállítható kihagyott adatbázis][] művelet használatával szeretne.
3.  Kezdje el a visszaállítása az [adatbázis létrehozása][] visszaállítás kérelem használatával.
4.  A visszaállítás állapotának nyomon az [adatbázis-művelet állapota][] művelet használatával.

>[AZURE.NOTE] A visszaállítás befejeződése után adja meg az adatbázist, olvassa el [a helyreállítás adatbázis konfigurálása][]. 


## <a name="next-steps"></a>Következő lépések
Az üzleti folytonosságot funkciók Azure SQL-adatbázis kiadásainak kapcsolatos további tudnivalókért olvassa el az [Azure SQL-adatbázis üzleti folytonosságot áttekintése][].

<!--Image references-->

<!--Article references-->
[Azure SQL-adatbázis üzleti folytonosságot – áttekintés]: ./sql-database-business-continuity.md
[Kvóta DTU módosításának kérése]: ./sql-data-warehouse-get-started-create-support-ticket.md#request-quota-change
[Az adatbázis konfigurálása helyreállítás]: ./sql-database-disaster-recovery.md#configure-your-database-after-recovery
[How to install and configure Azure PowerShell]: ./powershell-install-configure.md
[– Áttekintés]: ./sql-data-warehouse-restore-database-overview.md
[Portál]: ./sql-data-warehouse-restore-database-portal.md
[A PowerShell]: ./sql-data-warehouse-restore-database-powershell.md
[TÖBBI]: ./sql-data-warehouse-restore-database-rest-api.md

<!--MSDN references-->
[Adatbázis visszaállítása kérelem létrehozása]: https://msdn.microsoft.com/library/azure/dn509571.aspx
[Adatbázis-művelet állapota]: https://msdn.microsoft.com/library/azure/dn720371.aspx
[Visszaállítható kihagyott adatbázis beszerzése]: https://msdn.microsoft.com/library/azure/dn509574.aspx
[A lista visszaállítható kihagyott adatbázisok]: https://msdn.microsoft.com/library/azure/dn509562.aspx
[Restore-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt693390.aspx

<!--Other Web references-->
[Azure Portal]: https://portal.azure.com/
[Microsoft Web Platform Installer]: https://aka.ms/webpi-azps
