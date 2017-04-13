<properties
    pageTitle="Áttekintés: az SQL-adatbázis eszközeit |} Microsoft Azure"
    description="Miben más az eszközök és Azure SQL-adatbázis kezelésére szolgáló beállítások"
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/24/2016"
    ms.author="sstein"/>

# <a name="overview-management-tools-for-sql-database"></a>Áttekintés: eszközeit SQL-adatbázis

Ez a témakör tallózása, és miben más az eszközök és a beállítások a Azure SQL-adatbázisok kezelése, így a megfelelő eszköz a feladatot, a vállalkozás és meg is választhat. Válassza a megfelelő eszköz attól függ, hogy hány adatbázisok felügyeletét látja el, a feladatot, és milyen gyakran történik a tevékenység.

## <a name="azure-portal"></a>Azure portál

Az [Azure portal](https://portal.azure.com) webes alkalmazás, ahol létrehozása, módosítása, és adatbázisok és logikai kiszolgálók törlése és adatbázis figyelése. Ez az eszköz kiválóan, ha éppen csak most ismerkedik az Azure-néhány-adatbázisok kezelése vagy kell elvégeznie gyorsan.

A portál használatával kapcsolatos további tudnivalókért olvassa el az [SQL-adatbázisok kezelése az Azure portál használatával](sql-database-manage-portal.md)című témakört.

## <a name="sql-server-management-studio-and-sql-server-data-tools-in-visual-studio"></a>SQL Server Management Studio és az SQL Server Data Tools a Visual Studio

Az SQL Server Management Studio (SSMS) és az SQL Server Data Tools (SSDT) kezelésére, és a felhőben az adatbázis elkészítésének a számítógépen futó ügyféleszközök elől. Ha ismeri a Visual Studio vagy más integrált fejlesztői környezetekben (IDEs), [használja a Visual Studióban SSDT](https://msdn.microsoft.com/library/mt204009.aspx)alkalmazásfejlesztő. Adatbázis-rendszergazdák jól ismert SSMS, amely használható az Azure SQL-adatbázisait. [Töltse le a SSMS legújabb verzióját](https://msdn.microsoft.com/library/mt238290) , és mindig a legújabb kiadásra Azure SQL-adatbázishoz való munkavégzés során. Az Azure SQL-adatbázisok a SSMS kezelésével kapcsolatban további tudnivalókért olvassa el a [SQL-adatbázisok kezelése SSMS használatával](sql-database-manage-azure-ssms.md)című témakört.

> [AZURE.IMPORTANT] Mindig az [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290) és az [SQL Server Data Tools](https://msdn.microsoft.com/library/mt204009.aspx) legújabb verzióját használja továbbra is a frissítéseket és a Microsoft Azure SQL-adatbázis szinkronizálja.


## <a name="powershell"></a>A PowerShell

A PowerShell-adatbázisok és rugalmas adatbázis készletek kezelése és Azure erőforrás-telepítések automatizálása használhatja. A Microsoft Ez az eszköz a sok az adatbázisok kezelése és telepítési és erőforrás-módosítások munkakörnyezetben automatizálása javasolja.

További tudnivalókért lásd: az [SQL-adatbázis kezelése a PowerShell](sql-database-manage-powershell.md)

## <a name="elastic-database-tools"></a>Rugalmas Adatbáziseszközök
Használja a rugalmas Adatbáziseszközök végzett műveletek, például: 

* Az SQL-T parancsfájl összehasonlítja egy [rugalmas feladat](sql-database-elastic-jobs-overview.md) használatával adatbázisokat a végrehajtása
* Több elem bérlői modell adatbázisok áthelyezése egyetlen-bérlői modell az [eszköz felosztása és egyesítése](sql-database-elastic-scale-overview-split-and-merge.md)
* Egy egyetlen-bérlői modell vagy a [skála rugalmas ügyfél tár](sql-database-elastic-database-client-library.md)használatával több bérlői modell adatbázisok kezelésében.
 

## <a name="additional-resources"></a>További források

- [Azure erőforrás-kezelő](https://azure.microsoft.com/features/resource-manager/)
- [Azure automatizálási](https://azure.microsoft.com/documentation/services/automation/)
- [Azure ütemező](https://azure.microsoft.com/documentation/services/scheduler/)