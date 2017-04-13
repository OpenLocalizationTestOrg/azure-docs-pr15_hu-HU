<properties
    pageTitle="Azure SQL-adatbázis V12 PowerShell használatával frissítés |} Microsoft Azure"
    description="Megtudhatja, hogyan frissítése a Microsoft Azure SQL-adatbázis V12 például üzleti és a webes adatbázis frissítése, az adatbázis áttelepítése közvetlenül egy PowerShell használatával rugalmas adatbázis készlet V11 kiszolgáló frissítéséről."
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
    ms.date="09/19/2016"
    ms.author="sstein"/>

# <a name="upgrade-to-azure-sql-database-v12-using-powershell"></a>Azure SQL-adatbázis V12 frissítés PowerShell használatával


> [AZURE.SELECTOR]
- [Azure portál](sql-database-upgrade-server-portal.md)
- [A PowerShell](sql-database-upgrade-server-powershell.md)


SQL-adatbázis V12 legújabb verziója, az SQL-adatbázis V12 való frissítés ajánlott.
SQL-adatbázis V12 számos [előnye a az előző verzióhoz képest](sql-database-v12-whats-new.md) , többek között foglalja magában:

- Az SQL Server megnövelt kompatibilitásának.
- Továbbfejlesztett prémium teljesítmény és új teljesítményszint.
- [Rugalmas adatbázis készletek](sql-database-elastic-pool.md).

Ebben a cikkben utasításokat-re történő frissítésének meglévő SQL-adatbázis V11 kiszolgálók és adatbázisok SQL-adatbázis V12.

V12 való frissítés során frissítenie bármely webes és az üzleti adatbázis egy új szolgáltatási réteg, üzleti és a webes adatbázis-re történő frissítésének Útbaigazítás számításba veszi.

Ezenkívül egy [készlet rugalmas adatbázis](sql-database-elastic-pool.md) áttelepítése lehet költség hatékonyabb, mint (árak rétegek) egyes teljesítményszint egyetlen adatbázisok frissítése. Készletek is egyszerűbbé teszik a adatbázis kezelése, mert csak akkor van szüksége a készlet, hanem külön-külön az egyes adatbázisokat a teljesítmény szintjeinek kezelése teljesítmény beállításainak kezelése. Több kiszolgálón Ha adatbázisok, fontolja meg az ugyanarra a kiszolgálóra, és egy készletbe helyezésének kihasználni áthelyezi őket.

A kövesse a jelen cikkben egyszerűen áttelepítése adatbázisok V11 kiszolgálók közvetlenül egy rugalmas adatbázis készletek.

Ne feledje, hogy a az adatbázisok online marad, és továbbra is működnek a frissítési művelet során. Az ideiglenes kapcsolatot az adatbázis leválasztása új teljesítményszint tényleges áttűnés időpontjában akkor fordulhat elő, amely általában körülbelül 90 másodperc nagyon kis ideig, de szerint az 5 perc lehet. Az alkalmazás rendelkezik [kezelésére szolgáló kapcsolatot végződnek tranziens hibafa](sql-database-connectivity-issues.md) majd esetén elegendő és vírusvédelmet kapcsolatok megszakadnak a frissítés végén.

SQL-adatbázis V12 való frissítés nem vonható vissza. Frissítés után a kiszolgáló nem állítható vissza V11.

V12 történő frissítés után [szolgáltatás réteg javaslatok](sql-database-service-tier-advisor.md) és [rugalmas készlet javaslatok](sql-database-elastic-pool-create-portal.md) nem azonnal lesz elérhető mindaddig, amíg a szolgáltatás rendelkezik-e az új kiszolgálón a munkaterhelésekből kiértékelése időt. V11 kiszolgáló ajánlási előzmények nem vonatkozik V12 kiszolgálók, nem őrzi meg.  

## <a name="prepare-to-upgrade"></a>Felkészülés a frissítése

- **Az összes webhelyen és üzleti adatbázisok frissítése**: a portálon használja, vagy [a PowerShell használatá adatbázisok és kiszolgáló frissítése](sql-database-upgrade-server-powershell.md).
- **Áttekintése és felfüggeszti a replikáció Geo**: Ha be van állítva az Azure SQL-adatbázis Geo-replikáció kell a dokumentum a szoftver jelenlegi konfigurációjának megfelelően és [leállítása Geo-replikáció](sql-database-geo-replication-portal.md#remove-secondary-database). A frissítés befejezése után újra az adatbázist Geo-replikáció.
- **Nyissa meg az alábbi portokat az Azure virtuális ügyfelek esetén**: Ha az ügyfélprogram SQL-adatbázis V12 csatlakozik, az ügyfél-Azure virtuális gépen (virtuális) futása közben, meg kell nyitnia 11000-11999 és 14000-14999 a virtuális a port tartományok. Részletekért olvassa el [az SQL-adatbázis V12 portokat](sql-database-develop-direct-route-ports-adonet-v12.md).


## <a name="prerequisites"></a>Előfeltételek

A PowerShell V12 kiszolgáló frissítéséhez a legújabb Azure PowerShell telepítése és futtatása van szükség. Információ megtudhatja, [hogy miként telepítheti, állíthatja Azure PowerShell](../powershell-install-configure.md).


## <a name="configure-your-credentials-and-select-your-subscription"></a>Állítsa be a hitelesítő adatait, és válassza az előfizetés

Az Azure előfizetés PowerShell-parancsmagok futtassanak létre kell hoznia az access az Azure-fiókjába. Futtassa az alábbi, és a bejelentkezési képernyőn írja be hitelesítő adatait a bemutatni. Használja az azonos e-mail címével és jelszavával jelentkezzen be az Azure portal segítségével.

    Add-AzureRmAccount

Sikeresen bejelentkezés után meg kell jelennie néhány információt, amely tartalmazza az azonosító jelentkezett be, és van hozzáférése az Azure előfizetések képernyőn.

Jelölje ki azt az előfizetést, azt szeretné, hogy együtt dolgozzanak Önnel van szüksége a előfizetés azonosítója (**-SubscriptionId**) vagy az előfizetése neve (**-SubscriptionName**). Az előző lépésben másolhatja, vagy ha több előfizetéssel rendelkezik futtatását is lehetővé teszi a **Get-AzureRmSubscription** parancsmag és a kívánt előfizetési adatokat másolja a resultset.

Az előfizetési adatokat beállítása az aktuális előfizetése futtassa a következő parancsmagot:

    Set-AzureRmContext -SubscriptionId 4cac86b0-1e56-bbbb-aaaa-000000000000

Az alábbi parancsok fog futni szemben az előfizetés meg az imént kiválasztott felett.

## <a name="get-recommendations"></a>Javaslatok beszerzése

A kiszolgáló ajánlása megszerezni frissítés futtassa a következő parancsmagot:

    $hint = Get-AzureRmSqlServerUpgradeHint -ResourceGroupName “resourcegroup1” -ServerName “server1”

További tudnivalókért lásd: [egy rugalmas adatbázis készlet létrehozása](sql-database-elastic-pool-create-portal.md) és [Azure SQL-adatbázis árak réteg javaslatok](sql-database-service-tier-advisor.md).



## <a name="start-the-upgrade"></a>A frissítés megkezdése

A kiszolgáló, futtassa a következő parancsmagot a frissítés megkezdése:

    Start-AzureRmSqlServerUpgrade -ResourceGroupName “resourcegroup1” -ServerName “server1” -ServerVersion 12.0 -DatabaseCollection $hint.Databases -ElasticPoolCollection $hint.ElasticPools  


Futtatásakor a parancs frissítési folyamat elindul. Az ajánlási kimenetét testreszabása, és adja meg a szerkesztett javaslatot, hogy ezzel a parancsmaggal.


## <a name="upgrade-a-server"></a>A kiszolgáló frissítése


    # Adding the account
    #
    Add-AzureRmAccount

    # Setting the variables
    #
    $SubscriptionName = 'YOUR_SUBSCRIPTION'
    $ResourceGroupName = 'YOUR_RESOURCE_GROUP'
    $ServerName = 'YOUR_SERVER'

    # Selecting the right subscription
    #
    Set-AzureRmContext -SubscriptionName $SubscriptionName

    # Getting the upgrade recommendations
    #
    $hint = Get-AzureRmSqlServerUpgradeHint -ResourceGroupName $ResourceGroupName -ServerName $ServerName

    # Starting the upgrade process
    #
    Start-AzureRmSqlServerUpgrade -ResourceGroupName $ResourceGroupName -ServerName $ServerName -ServerVersion 12.0 -DatabaseCollection $hint.Databases -ElasticPoolCollection $hint.ElasticPools  


## <a name="custom-upgrade-mapping"></a>Egyéni megfeleltetés frissítése

Ha a javaslatok nem megfelelő a kiszolgáló és az üzleti terv, majd választhat hogyan az adatbázisok frissülnek, és egyetlen, vagy rugalmas adatbázis képezhető őket.

ElasticPoolCollection és DatabaseCollection paraméterek nem kötelező:

    # Creating elastic pool mapping
    #
    $elasticPool = New-Object -TypeName Microsoft.Azure.Management.Sql.Models.UpgradeRecommendedElasticPoolProperties
    $elasticPool.DatabaseDtuMax = 100
    $elasticPool.DatabaseDtuMin = 0
    $elasticPool.Dtu = 800
    $elasticPool.Edition = "Standard"
    $elasticPool.DatabaseCollection = ("DB1", “DB2”, “DB3”, “DB4”)
    $elasticPool.Name = "elasticpool_1"
    $elasticPool.StorageMb = 800

    # Creating single database mapping for 2 databases. DBMain1 mapped to S0 and DBMain2 mapped to S2
    #
    $databaseMap1 = New-Object -TypeName Microsoft.Azure.Management.Sql.Models.UpgradeDatabaseProperties
    $databaseMap1.Name = "DBMain1"
    $databaseMap1.TargetEdition = "Standard"
    $databaseMap1.TargetServiceLevelObjective = "S0"

    $databaseMap2 = New-Object -TypeName Microsoft.Azure.Management.Sql.Models.UpgradeDatabaseProperties
    $databaseMap2.Name = "DBMain2"
    $databaseMap2.TargetEdition = "Standard"
    $databaseMap2.TargetServiceLevelObjective = "S2"

    # Starting the upgrade
    #
    Start-AzureRmSqlServerUpgrade –ResourceGroupName resourcegroup1 –ServerName server1 -ServerVersion 12.0 -DatabaseCollection @($databaseMap1, $databaseMap2) -ElasticPoolCollection @($elasticPool)



## <a name="monitor-databases-after-upgrading-to-sql-database-v12"></a>Lync-adatbázisok SQL-adatbázis V12-re frissítés után


A frissítés után ajánlott Lync-aktívan alkalmazást futtatja a kívánt teljesítményű és biztosításához optimalizálása használatát, szükség szerint az adatbázist.

Emellett az egyes adatbázisokat figyelheti rugalmas adatbázis készletek [a portálon](sql-database-elastic-pool-manage-portal.md) felügyeletre vagy a [PowerShell](sql-database-elastic-pool-manage-powershell.md)


**Erőforrás felhasználási adatok:** Egyszerű, normál és Premium adatbázisok erőforrás felhasználási adatok keresztül is elérhető a [sys.dm_ db_ resource_stats](http://msdn.microsoft.com/library/azure/dn800981.aspx) DMV a felhasználói adatbázisban. A DMV biztosít a valós idejű erőforrás felhasználási adatait a 15 második Granularitás az előző óra művelet közelében. A DTU százalékértéket felhasználás intervallumhoz számítja ki, hogy a maximális százalékos egységarányt felhasználás Processzor, IO, és jelentkezzen be méretek közül. A következő lekérdezés számítja ki az utolsó óra fölé átlagos DTU százalékos felhasználás:

    SELECT end_time
         , (SELECT Max(v)
             FROM (VALUES (avg_cpu_percent)
                         , (avg_data_io_percent)
                         , (avg_log_write_percent)
           ) AS value(v)) AS [avg_DTU_percent]
    FROM sys.dm_db_resource_stats
    ORDER BY end_time DESC;

További felügyeleti információk:

- [Egyetlen adatbázisok azure SQL-adatbázis teljesítmény útmutatást](http://msdn.microsoft.com/library/azure/dn369873.aspx).
- [Rugalmas adatbázis-készlet árát és teljesítménybeli szempontok](sql-database-elastic-pool-guidance.md).
- [Figyelés Azure SQL-adatbázis kezelése dinamikus nézetek használata](sql-database-monitoring-with-dmvs.md)



**Értesítések:** Beállíthat "Figyelmeztetés" az Azure-portálon értesítést a DTU felhasználás frissített adatbázisok közelít bizonyos magas szintű. Adatbázis-értesítések az Azure-portálon különböző teljesítménymutatók, például a DTU, Processzor, IO és Log for állíthat be. Tallózással keresse meg az adatbázist, és válassza ki **a figyelmeztetési szabályok** a **Beállítások** lap.

Például beállíthatja, hogy be egy e-mailben riasztás "DTU százalékos" Ha átlagos DTU százalékos értéke meghaladja a 75 %-át az utolsó 5 perc alatt. Értesítések beállításáról további információt a hivatkozott riasztási értesítést szeretne [kapni](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) .



## <a name="next-steps"></a>Következő lépések

- [Egy rugalmas adatbázis készlet létrehozása](sql-database-elastic-pool-create-portal.md) és némelyikét vagy mindegyikét az adatbázisok hozzáadása a készletbe.
- [Az adatbázis szolgáltatás réteg és a teljesítmény szintjének módosítása](sql-database-scale-up.md).



## <a name="related-information"></a>Kapcsolódó információ

- [Get-AzureRmSqlServerUpgrade](https://msdn.microsoft.com/library/azure/mt603582.aspx)
- [Kezdés-AzureRmSqlServerUpgrade](https://msdn.microsoft.com/library/azure/mt619403.aspx)
- [Leállítás-AzureRmSqlServerUpgrade](https://msdn.microsoft.com/library/azure/mt603589.aspx)
