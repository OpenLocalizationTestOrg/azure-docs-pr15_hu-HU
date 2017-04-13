<properties
   pageTitle="Hozzon létre SQL adatraktár PowerShell használatával |} Microsoft Azure"
   description="Hozzon létre SQL adatraktár PowerShell használatával"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/25/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

# <a name="create-sql-data-warehouse-using-powershell"></a>Hozzon létre SQL adatraktár PowerShell használatával

> [AZURE.SELECTOR]
- [Azure portál](sql-data-warehouse-get-started-provision.md)
- [TSQL](sql-data-warehouse-get-started-create-database-tsql.md)
- [A PowerShell](sql-data-warehouse-get-started-provision-powershell.md)

Ez a cikk bemutatja, hogyan hozhat létre egy SQL-adatraktár PowerShell használatával.

## <a name="prerequisites"></a>Előfeltételek

Első lépések a következők szükségesek:

- **Azure-fiók**: látogatás [Azure ingyenes próbaverziót][] vagy az [MSDN Azure Credits][] hozhat létre fiókot.
- **Azure SQL server**: [Hozzon létre egy logikai Azure SQL-adatbázis-kiszolgálót, az Azure portálján][] , vagy [Hozzon létre egy logikai Azure SQL-adatbázis-kiszolgálót, a PowerShell][] talál további információt.
- **Erőforráscsoport**: az azonos erőforráscsoport használni az Azure SQL server vagy [erőforráscsoport létrehozásának][]módját.
- **1.0.3 verzió PowerShell-s vagy újabb**: futtatásával ellenőrizze az új verzióját **Get-modul - ListAvailable-neve Azure**.  A legújabb [Microsoft webes Platform telepítő][]a telepíthető.  A legújabb verziójának telepítésével kapcsolatos további tudnivalókért megtudhatja, [hogy miként telepítheti, állíthatja Azure PowerShell][].

> [AZURE.NOTE] Új számlázható szolgáltatás vonhat egy SQL adatraktár létrehozása.  Lásd: az [SQL adatraktár árak][] árak rendszeren.

## <a name="create-a-sql-data-warehouse"></a>Hozzon létre egy SQL adatraktár

1. Nyissa meg a Windows PowerShell.
2. Ez a parancsmag futtatásával jelentkezzen be az Azure erőforrás-kezelő.

    ```Powershell
    Login-AzureRmAccount
    ```
    
3. Jelölje ki azt az előfizetést, az aktuális munkamenete használni kívánt.

    ```Powershell
    Get-AzureRmSubscription -SubscriptionName "MySubscription" | Select-AzureRmSubscription
    ```

4.  Adatbázis létrehozása. Ez a példa létrehoz egy adatbázist, "mynewsqldw", szolgáltatás objektív szinthez "DW400", "sqldwserver1", amely a "mywesteuroperesgp1" nevű erőforráscsoport nevű kiszolgálóra való nevű.

    ```Powershell
    New-AzureRmSqlDatabase -RequestedServiceObjectiveName "DW400" -DatabaseName "mynewsqldw" -ServerName "sqldwserver1" -ResourceGroupName "mywesteuroperesgp1" -Edition "DataWarehouse" -CollationName "SQL_Latin1_General_CP1_CI_AS" -MaxSizeBytes 10995116277760
    ```

Szükséges paramétert következők:

- **RequestedServiceObjectiveName**: [DWU][] kért mennyiségét.  Támogatott érték: DW100, DW200, DW300, DW400, DW500, DW600, DW1000, DW1200, DW1500, DW2000, DW3000 és DW6000.
- **Adatbázisnév**: a létrehozni kívánt SQL adatraktár nevét.
- **Kiszolgálónév**: A kiszolgáló nevének a létrehozásához használni kívánt (V12 kell lennie).
- **ResourceGroupName**: erőforráscsoport használja.  Keresse meg az erőforrás elérhető az előfizetése csoportok Get-AzureResource használja.
- **Edition**: "DataWarehouse" kell lennie egy SQL adatraktár létrehozásához.

Nem kötelező paraméterek a következők:

- **CollationName**: Ha nincs megadva, az alapértelmezett illesztés SQL_Latin1_General_CP1_CI_AS.  Rendezési sorrend egy adatbázisban nem módosíthatók.
- **MaxSizeBytes**: az alapértelmezés szerinti maximális adatbázis mérete 10 gigabájtos Korlátot.


A paraméter beállításai, lásd: [Új-AzureRmSqlDatabase][] és [-Adatbázis létrehozása (Azure SQL-adatraktár)][].

## <a name="next-steps"></a>Következő lépések

Az SQL adatraktár a kiépítési befejeződése után célszerű megpróbálja a [mintaadatok betöltése][] , vagy ellenőrizze, hogy miként [fejlesztése][], [betölteni][]vagy [áttelepítendő][].

Ha érdekli a további SQL adatraktár programozás útján kezeléséről, olvassa el a cikk a [PowerShell-parancsmagok és REST API -k][]használata.

<!--Image references-->

<!--Article references-->
[DWU]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units
[áttelepítése]: ./sql-data-warehouse-overview-migrate.md
[kidolgozása]: ./sql-data-warehouse-overview-develop.md
[betöltése]: ./sql-data-warehouse-load-with-bcp.md
[mintaadatok betöltése]: ./sql-data-warehouse-load-sample-databases.md
[PowerShell-parancsmagok és a REST API-hoz]: ./sql-data-warehouse-reference-powershell-cmdlets.md
[firewall rules]: ../sql-database-configure-firewall-settings.md

[Telepítse és állítsa be a Azure PowerShell hogyan]: ../powershell/powershell-install-configure.md
[how to create a SQL Data Warehouse from the Azure Portal]: ./sql-data-warehouse-get-started-provision.md
[Egy logikai Azure SQL-adatbázis-kiszolgáló létrehozása az Azure-portálra]: ../sql-database/sql-database-get-started.md#create-an-azure-sql-database-logical-server
[Hozzon létre egy logikai Azure SQL-adatbázis-kiszolgáló szolgáltatást a PowerShell használatával]: ../sql-database/sql-database-get-started-powershell.md#database-setup-create-a-resource-group-server-and-firewall-rule
[Erőforráscsoport létrehozása]: ../resource-group-template-deploy-portal.md#create-resource-group

<!--MSDN references--> 
[MSDN]: https://msdn.microsoft.com/library/azure/dn546722.aspx
[Új AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619339.aspx
[Adatbázis (Azure SQL-adatraktár) létrehozása]: https://msdn.microsoft.com/library/mt204021.aspx

<!--Other Web references-->
[A Microsoft webes Platform telepítő]: https://aka.ms/webpi-azps
[SQL-adatraktár árak]: https://azure.microsoft.com/pricing/details/sql-data-warehouse/
[Azure ingyenes próbaverziója]: https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F
[MSDN Azure Credits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F
