<properties
   pageTitle="Azure SQL-adatbázis általános korlátai és útmutató"
   description="A lap azt ismerteti, hogy bizonyos korlátozások, amelyek az általános Azure SQL-adatbázissal, valamint az együttműködési és támogatási részeinek."
   services="sql-database"
   documentationCenter="na"
   authors="CarlRabeler"
   manager="jhubbard"
   editor="monicar" />
<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="09/06/2016"
   ms.author="carlrab" />

# <a name="azure-sql-database-general-limitations-and-guidelines"></a>Azure SQL-adatbázis általános korlátai és útmutató

Ez a témakör általános korlátai és irányelveket talál az Azure SQL-adatbázis. Kvóták, az erőforrás-kezelés és támogatási értelmezését olvassa el a [További erőforrások](#additional-guidelines) Ez a témakör végén.

## <a name="connectivity-and-authentication"></a>Csatlakozási és a hitelesítéshez

  - Nem támogatott a Windows-hitelesítés használatával. [Adatbázisok és bejelentkezés az Azure SQL-adatbázis kezelése](sql-database-manage-logins.md)című részben talál. Azure Active Directory-hitelesítés, bizonyos korlátozások támogatott. Lásd: [az Azure Active Directory Authentication SQL-adatbázis csatlakoztatása](sql-database-aad-authentication.md).

  - Microsoft Azure SQL-adatbázis támogatja táblázatadatokat adatfolyam (TDS) protokoll ügyfél 7.3 vagy újabb verziója.

  - Csak a TCP/IP kapcsolatok engedélyezve vannak.

  - Mivel a Microsoft Azure SQL-adatbázis nincs dinamikus portok, csak port 1433 az SQL Server 2008 SQL Server böngésző nem támogatott.

## <a name="sql-server-agentjobs"></a>Az SQL Server-Agent/feladatok

Microsoft Azure SQL-adatbázis nem támogatja az SQL Server Agent, azonban a különböző egy a sok adatbázis futtatása a feladatok rugalmas feladatok is használhatja. Rugalmas feladatok kapcsolatos további tudnivalókért olvassa el a [rugalmas feladatok](sql-database-elastic-jobs-overview.md)című témakört.

## <a name="sql-server-collation-support"></a>Az SQL Server illesztés támogatása

A Microsoft Azure SQL-adatbázis által használt alapértelmezett adatbázis illesztés **SQL_LATIN1_GENERAL_CP1_CI_AS**, ahol **LATIN1_GENERAL** az angol (Egyesült államokbeli), **CP1** kódlap 1252, **ki** nagybetűk, pedig **AS** ékezet érzékeny. Még nem változtatja meg a rendezési sorrend V12 adatbázisok is. Az illesztés beállításával kapcsolatos további tudnivalókért olvassa el a [SZÉTVÁLOGATÁS (Transact-SQL nyelvben)](https://msdn.microsoft.com/library/ms184391.aspx)című témakört.

## <a name="naming-requirements"></a>Elnevezési követelmények

Biztonsági okokból egyes felhasználónevek nem használhatók. A következő nevek nem használható:

 - **rendszergazda**
 - **rendszergazda**
 - **a vendégként való bekapcsolódáshoz**
 - **legfelső szintű**
 - **rendszergazdai**

Minden új objektum neve meg kell felelnie az SQL Server szabályok azonosítóját. További tudnivalókért lásd: [azonosítóját](https://msdn.microsoft.com/library/ms175874.aspx).

Emellett a bejelentkezési adatok és a felhasználó neve nem tartalmazhat a \ (Windows-hitelesítés nem támogatott) karaktert.

## <a name="additional-guidelines"></a>További útmutató

- A cikkben ismertetett általános korlátozások mellett a SQL-adatbázis van a adott kiszolgálóerőforrás-kvótájának és a korlátozások a **szolgáltatási réteg**alapján. Szolgáltatási rétegek áttekintése olvassa el az [SQL-adatbázis szolgáltatási rétegek](sql-database-service-tiers.md)című témakört.

- Más SQL-adatbázis korlátozásairól című cikkben találhat [Azure SQL adatbázis erőforrás](sql-database-resource-limits.md).

- Biztonsági kapcsolódó irányelveihez témakörben [Azure SQL-adatbázis biztonsági irányelveket](sql-database-security-guidelines.md).

- Kapcsolódó másik területére Azure SQL-adatbázis tartalmazó helyszíni SQL Server, például SQL Server 2014-es és az SQL Server 2016 verzióra, és a kompatibilitás körül. A V12 legújabb Azure SQL-adatbázis tett sok javítása ezen a területen. További információra kíváncsi olvassa el [az SQL-adatbázis V12 újdonságai](sql-database-v12-whats-new.md)című témakört.

- Információt illesztőprogram elérhetőségével és SQL-adatbázis támogatása című témakörben talál [a kapcsolat SQL-adatbázis és az SQL Server képtárak](sql-database-libraries.md).
