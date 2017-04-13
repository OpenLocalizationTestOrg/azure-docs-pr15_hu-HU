<properties 
    pageTitle="Egy rugalmas adatbázis készlet (PowerShell) kezelése |} Microsoft Azure" 
    description="Megtudhatja, hogy miként egy rugalmas adatbázis készlet kezelése a PowerShell használatával."  
    services="sql-database" 
    documentationCenter="" 
    authors="srinia" 
    manager="jhubbard" 
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="powershell"
    ms.workload="data-management" 
    ms.date="06/22/2016"
    ms.author="srinia"/>

# <a name="monitor-and-manage-an-elastic-database-pool-with-powershell"></a>Figyelése és kezelése a PowerShell-rugalmas adatbázis készlet 

> [AZURE.SELECTOR]
- [Azure portál](sql-database-elastic-pool-manage-portal.md)
- [A PowerShell](sql-database-elastic-pool-manage-powershell.md)
- [C#](sql-database-elastic-pool-manage-csharp.md)
- [AZ SQL-T](sql-database-elastic-pool-manage-tsql.md)

Egy PowerShell-parancsmagok használata [Rugalmas adatbázis készlet](sql-database-elastic-pool.md) kezelése. 

Gyakori hibakódok című [SQL-hibakódok az SQL-adatbázis-ügyfélalkalmazásokban: adatbázis-kapcsolati hiba és egyéb problémák](sql-database-develop-error-messages.md).

Egyesített értékeket [eDTU és tárolási](sql-database-elastic-pool.md#eDTU-and-storage-limits-for-elastic-pools-and-elastic-databases)korlátok találhatók. 

## <a name="prerequisites"></a>Előfeltételek

* Azure PowerShell 1,0 vagy újabb verziójában. Információ megtudhatja, [hogy miként telepítheti, állíthatja Azure PowerShell](../powershell-install-configure.md).
* Rugalmas adatbázis készletek SQL-adatbázis V12 kiszolgálókon csak érhetők el. Ha a [készlet létrehozása és frissítése a V12 PowerShell-lel](sql-database-upgrade-server-portal.md) egy lépésben egy SQL-adatbázis V11 kiszolgálóval.


## <a name="move-a-database-into-an-elastic-pool"></a>Adatbázis áthelyezése egy rugalmas készlet

Áthelyezheti az adatbázis és elhalványítása és a [Set-AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt619433.aspx)erőforráskészlethez tartozik. 

    Set-AzureRmSqlDatabase -ResourceGroupName "resourcegroup1" -ServerName "server1" -DatabaseName "database1" -ElasticPoolName "elasticpool1"

## <a name="change-performance-settings-of-a-pool"></a>Egy készlet teljesítmény beállításainak módosítása

Teljesítmény szenved, amikor a készlet beállításai NÖV igazodik módosíthatja. A [Set-AzureRmSqlElasticPool](https://msdn.microsoft.com/library/azure/mt603511.aspx) parancsmag használata. A - Dtu paraméter értéke a készlet per eDTUs. Lásd: a lehetséges értékek [eDTU és tárolási korlátai](sql-database-elastic-pool.md#eDTU-and-storage-limits-for-elastic-pools-and-elastic-databases) .  

    Set-AzureRmSqlElasticPool –ResourceGroupName “resourcegroup1” –ServerName “server1” –ElasticPoolName “elasticpool1” –Dtu 1200 –DatabaseDtuMax 100 –DatabaseDtuMin 50 


## <a name="get-the-status-of-pool-operations"></a>Készlet műveletek állapotuk

Egy készlet létrehozása időt vehet igénybe. A [Get-AzureRmSqlElasticPoolActivity](https://msdn.microsoft.com/library/azure/mt603812.aspx) parancsmag használatával nyomon követheti a készlet műveletek létrehozása és frissítések állapotát.

    Get-AzureRmSqlElasticPoolActivity –ResourceGroupName “resourcegroup1” –ServerName “server1” –ElasticPoolName “elasticpool1” 


## <a name="get-the-status-of-moving-an-elastic-database-into-and-out-of-a-pool"></a>Egy rugalmas adatbázis áthelyezése a be- és kijelentkezés a készlet állapotuk

Adatbázis áthelyezése időt vehet igénybe. A [Get-AzureRmSqlDatabaseActivity](https://msdn.microsoft.com/library/azure/mt603687.aspx) parancsmaggal áthelyezés állapot nyomon.

    Get-AzureRmSqlDatabaseActivity -ResourceGroupName "resourcegroup1" -ServerName "server1" -DatabaseName "database1" -ElasticPoolName "elasticpool1"

## <a name="get-resource-usage-data-for-a-pool"></a>Erőforrás-használati adatok beszerzése a készletben

Az erőforrás-készlet korlát százalékában eredményező mértékek:   


| Metrikus neve | Leírás |
| :-- | :-- |
| processzor\_százalék | Átlag százalékos aránya a által biztosított pedig a kihasználtság számítja ki. |
| fizikai\_adatok\_további\_százalék | A százalékos értékét a készlet határértékén átlagos I/O kihasználtsági. |
| log\_írása\_százalék | Átlag erőforrás-kihasználtság százalékos aránya a készlet határértékén írhat. | 
| DTU\_felhasználási\_százalék | A százalékos aránya a készlet eDTU korlátját átlagos eDTU kihasználtság | 
| tárterület\_százalék | Átlagos tároló kihasználtsági mutatkozó pedig a Tárhelykorlát érvényes. |  
| munkatársak\_százalék | Maximális egyidejű dolgozók (kérelmek) százalékos értékét a készlet korlátot. |  
| munkamenetek\_százalék | Az egyidejű munkamenetek maximális százalékos értékét a által biztosított pedig a. | 
| eDTU_limit | Max rugalmas készlet DTU az jelenlegi beállítása rugalmas készlet megadott időintervallum alatt. |
| tárterület\_korlát | Rugalmas készlet megabájtban megadott időintervallum alatt beállítása aktuális max rugalmas készlet méretkorlátot. |
| eDTU\_használt | A megadott időintervallum készletébe által használt átlagos eDTUs. |
| tárterület\_használt | Átlagos, a bájtban megadott intervallum készletébe által használt tárterület |

**Mértékek Granularitás és az adatmegőrzési időszak:**

* Adatok visszatér az 5 perc granularitása.  
* Adatmegőrzés 35 napig tart.  

E parancsmagjáról és API korlátozza a sorokat, amelyeket tudja visszaszerezni egy hívásban (körülbelül 5 perc Granularitás adatainak 3 nap) 1000 sorok számát. Ez a parancs neve többször a kezdete/vége különböző időtartományok további adatok beolvasásához lehet, de 

A mérési módja miatt beolvasásához:

    $metrics = (Get-AzureRmMetric -ResourceId /subscriptions/<subscriptionId>/resourceGroups/FabrikamData01/providers/Microsoft.Sql/servers/fabrikamsqldb02/elasticPools/franchisepool -TimeGrain ([TimeSpan]::FromMinutes(5)) -StartTime "4/18/2015" -EndTime "4/21/2015")  


## <a name="get-resource-usage-data-for-an-elastic-database"></a>Erőforrás-használati adatok beolvasása rugalmas adatbázisok

Ezek az API-khoz ugyanazok, mint az erőforrás-kihasználtság önálló adatbázis, kivéve a következő szemantikai különbség grafikonján az aktuális (V12) API-khoz. 

Százalékban kifejezett visszakeresve a mértékek ehhez az API a max eDTUs (vagy a mögöttes mérőszám, például a Processzor, IO stb egyenértékű vonalvég) egy adott készletre beállítása. Például 50 %-os kihasználtsági valamely ezek mértékek azt jelzi, hogy az adott erőforrás-felhasználás: 50 %-át az adatbázis vonalvég vonatkoznak azokra az adott erőforrás a szülő-készletre per. 

A mérési módja miatt beolvasásához:

    $metrics = (Get-AzureRmMetric -ResourceId /subscriptions/<subscriptionId>/resourceGroups/FabrikamData01/providers/Microsoft.Sql/servers/fabrikamsqldb02/databases/myDB -TimeGrain ([TimeSpan]::FromMinutes(5)) -StartTime "4/18/2015" -EndTime "4/21/2015") 

## <a name="add-an-alert-to-a-pool-resource"></a>Értesítés készlet erőforrás hozzáadása

Erőforrások értesítő e-mailek vagy küldeni riasztási karakterláncok [Végpontok URL-CÍMÉT](https://msdn.microsoft.com/library/mt718036.aspx) , ha az erőforrás találatok beállított kihasználtsági küszöbérték figyelmeztetési szabályokat vehet. A Hozzáadás-AzureRmMetricAlertRule parancsmag használata. 

> [AZURE.IMPORTANT]Erőforrás-kihasználtság rugalmas készletek figyelése tartalmaz legalább 20 perc késés. Rugalmas készletek vonatkozó értesítések legfeljebb 30 percig beállítása jelenleg nem támogatott. Ezeket az értesítéseket pontra beállítása rugalmas készletek (paraméter neve "-ablakméret" PowerShell API) legfeljebb 30 percig nem lehet indítani. Győződjön meg arról, hogy ezeket az értesítéseket kell megadni rugalmas készletek használata időtartama (ablakméret) legalább 30 percet vesz igénybe.

Ez a példa összeadja az első értesítés, amikor egy készlet eDTU felhasználási feletti bizonyos küszöbértéket értesítés.

    # Set up your resource ID configurations
    $subscriptionId = '<Azure subscription id>'      # Azure subscription ID
    $location =  '<location'                         # Azure region
    $resourceGroupName = '<resource group name>'     # Resource Group
    $serverName = '<server name>'                    # server name
    $poolName = '<elastic pool name>'                # pool name 

    #$Target Resource ID
    $ResourceID = '/subscriptions/' + $subscriptionId + '/resourceGroups/' +$resourceGroupName + '/providers/Microsoft.Sql/servers/' + $serverName + '/elasticpools/' + $poolName

    # Create an email action
    $actionEmail = New-AzureRmAlertRuleEmail -SendToServiceOwners -CustomEmail JohnDoe@contoso.com

    # create a unique rule name
    $alertName = $poolName + "- DTU consumption rule"

    # Create an alert rule for DTU_consumption_percent
    Add-AzureRMMetricAlertRule -Name $alertName -Location $location -ResourceGroup $resourceGroupName -TargetResourceId $ResourceID -MetricName "DTU_consumption_percent"  -Operator GreaterThan -Threshold 80 -TimeAggregationOperator Average -WindowSize 00:60:00 -Actions $actionEmail 

## <a name="add-alerts-to-all-databases-in-a-pool"></a>Értesítéseket vehet fel az összes adatbázisokat a készletben

Riasztási szabályok hozzáadása egy rugalmas készlet értesítő e-mailek küldése az összes adatbázis vagy [URL-cím végpontok](https://msdn.microsoft.com/library/mt718036.aspx) , amikor egy erőforrás riasztási karakterláncok találatok állítsa be a figyelmeztetést kihasználtsági küszöbértéket. 

> [AZURE.IMPORTANT] Erőforrás-kihasználtság rugalmas készletek figyelése tartalmaz legalább 20 perc késés. Rugalmas készletek vonatkozó értesítések legfeljebb 30 percig beállítása jelenleg nem támogatott. Ezeket az értesítéseket pontra beállítása rugalmas készletek (paraméter neve "-ablakméret" PowerShell API) legfeljebb 30 percig nem lehet indítani. Győződjön meg arról, hogy ezeket az értesítéseket kell megadni rugalmas készletek használata időtartama (ablakméret) legalább 30 percet vesz igénybe.

Ez a példa jelzést ad összes egy készletben első értesítést arra az adatbázisra DTU felhasználási bizonyos küszöbértéket fölé kerül az adatbázist.

    # Set up your resource ID configurations
    $subscriptionId = '<Azure subscription id>'      # Azure subscription ID
    $location = '<location'                          # Azure region
    $resourceGroupName = '<resource group name>'     # Resource Group
    $serverName = '<server name>'                    # server name
    $poolName = '<elastic pool name>'                # pool name 

    # Get the list of databases in this pool.
    $dbList = Get-AzureRmSqlElasticPoolDatabase -ResourceGroupName $resourceGroupName -ServerName $serverName -ElasticPoolName $poolName

    # Create an email action
    $actionEmail = New-AzureRmAlertRuleEmail -SendToServiceOwners -CustomEmail JohnDoe@contoso.com

    # Get resource usage metrics for a database in an elastic database for the specified time interval.
    foreach ($db in $dbList)
    {
    $dbResourceId = '/subscriptions/' + $subscriptionId + '/resourceGroups/' + $resourceGroupName + '/providers/Microsoft.Sql/servers/' + $serverName + '/databases/' + $db.DatabaseName

    # create a unique rule name
    $alertName = $db.DatabaseName + "- DTU consumption rule"

    # Create an alert rule for DTU_consumption_percent
    Add-AzureRMMetricAlertRule -Name $alertName  -Location $location -ResourceGroup $resourceGroupName -TargetResourceId $dbResourceId -MetricName "dtu_consumption_percent"  -Operator GreaterThan -Threshold 80 -TimeAggregationOperator Average -WindowSize 00:60:00 -Actions $actionEmail

    # drop the alert rule
    #Remove-AzureRmAlertRule -ResourceGroup $resourceGroupName -Name $alertName
    } 



## <a name="collect-and-monitor-resource-usage-data-across-multiple-pools-in-a-subscription"></a>Összegyűjtése és erőforrás-használati adatok figyelése át az előfizetés több készletek

Az adatbázisok sok előfizetést is van, esetén minden rugalmas készlet külön-külön figyelése lehető. Ehelyett SQL-adatbázis PowerShell-parancsmagok és az SQL-T lekérdezések kombinálhatók gyűjthetők össze az erőforrás-használati adatok több készletek és az adatbázisát figyelemmel kísérésére és erőforrás-kihasználtság elemzését. A [minta végrehajtása](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-sql-db-elastic-pools) a powershell-parancsfájlokat csoportja dokumentáció rendeltetés, és hogyan használható együtt a GitHub SQL Server minták adattárban találhatók.

Használata ezt a minta megvalósítás kövesse az alábbi lépéseket, az alább felsorolt.


1. Töltse le a [parancsprogramokat és a dokumentáció](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-sql-db-elastic-pools):
2. Módosítsa a parancsfájlok környezetben. Adja meg, hogy egy vagy több kiszolgálók, amelyen a rugalmas készletek is.
3. Adja meg a telemetriai adatbázisához tárolja az összegyűjtött mértékek esetén. 
4. Testre szabhatja az parancsprogramok alapján elkészítheti végrehajtási idejének hosszát adja meg a parancsfájlt.

Magas szintű a parancsfájlok tegye a következőket:

*   Összes kiszolgálón a megadott Azure előfizetést (vagy kiszolgálók megadott listája) felsorolása.
*   A háttér feladat minden kiszolgáló fut. A feladat rendszeres időközönként ismétlődő fut, és telemetriai adatokat nyújt a kiszolgálón lévő összes készletek gyűjti össze. Ezután betölti gyűjtött adatokat a megadott telemetriai adatbázisba.
*   Az adatbázisok adatbázis erőforrás-használati adatok összegyűjtéséhez minden egyes készletben lista felsorolása. A telemetriai adatbázisába a gyűjtött adatok majd betölti.

Lync-állapota rugalmas készletek és a benne lévő adatbázisok a összegyűjtött mérési módja miatt a telemetriai adatbázisához elemezheti. A parancsfájl telepíti az előre definiált táblázat értékű függvény (TVF) segíti az összesítés telemetriai adatbázisban egy megadott idő ablak a mérési módja miatt. Például a TVF eredményeit használva megjelenítése "legfelső N rugalmas készletek az adott idő alatt ablakban a maximális eDTU kihasználtsági." Tetszés szerint analitikus eszközök segítségével például Excelben vagy a Power BI lekérdezést, és a gyűjtött adatok elemzése.

## <a name="example-retrieve-resource-consumption-metrics-for-a-pool-and-its-databases"></a>Példa: az erőforrás felhasználási mértékek erőforráskészlethez tartozik, és az adatbázisok beolvasása

Ez a példa beolvassa a felhasználás mérési módja miatt az egy adott rugalmas készlet és minden adatbázisban. Gyűjtött adatok formázva, és egy .csv formátumú fájl írni. A fájl is érhető el az Excellel.

    $subscriptionId = '<Azure subscription id>'       # Azure subscription ID
    $resourceGroupName = '<resource group name>'             # Resource Group
    $serverName = <server name>                              # server name
    $poolName = <elastic pool name>                          # pool name
        
    # Login to Azure account and select the subscription.
    Login-AzureRmAccount
    Set-AzureRmContext -SubscriptionId $subscriptionId
    
    # Get resource usage metrics for an elastic pool for the specified time interval.
    $startTime = '4/27/2016 00:00:00'  # start time in UTC
    $endTime = '4/27/2016 01:00:00'    # end time in UTC
    
    # Construct the pool resource ID and retrive pool metrics at 5 minute granularity.
    $poolResourceId = '/subscriptions/' + $subscriptionId + '/resourceGroups/' + $resourceGroupName + '/providers/Microsoft.Sql/servers/' + $serverName + '/elasticPools/' + $poolName
    $poolMetrics = (Get-AzureRmMetric -ResourceId $poolResourceId -TimeGrain ([TimeSpan]::FromMinutes(5)) -StartTime $startTime -EndTime $endTime) 
    
    # Get the list of databases in this pool.
    $dbList = Get-AzureRmSqlElasticPoolDatabase -ResourceGroupName $resourceGroupName -ServerName $serverName -ElasticPoolName $poolName
    
    # Get resource usage metrics for a database in an elastic database for the specified time interval.
    $dbMetrics = @()
    foreach ($db in $dbList)
    {
        $dbResourceId = '/subscriptions/' + $subscriptionId + '/resourceGroups/' + $resourceGroupName + '/providers/Microsoft.Sql/servers/' + $serverName + '/databases/' + $db.DatabaseName
        $dbMetrics = $dbMetrics + (Get-AzureRmMetric -ResourceId $dbResourceId -TimeGrain ([TimeSpan]::FromMinutes(5)) -StartTime $startTime -EndTime $endTime)
    }
    
    #Optionally you can format the metrics and output as .csv file using the following script block.
    $command = {
    param($metricList, $outputFile)

    # Format metrics into a table.
    $table = @()
    foreach($metric in $metricList) { 
      foreach($metricValue in $metric.MetricValues) {
        $sx = New-Object PSObject -Property @{
            Timestamp = $metricValue.Timestamp.ToString()
            MetricName = $metric.Name; 
            Average = $metricValue.Average;
            ResourceID = $metric.ResourceId 
          }
          $table = $table += $sx
      }
    }
    
    # Output the metrics into a .csv file.
    write-output $table | Export-csv -Path $outputFile -Append -NoTypeInformation
    }
    
    # Format and output pool metrics
    Invoke-Command -ScriptBlock $command -ArgumentList $poolMetrics,c:\temp\poolmetrics.csv
    
    # Format and output database metrics
    Invoke-Command -ScriptBlock $command -ArgumentList $dbMetrics,c:\temp\dbmetrics.csv



## <a name="latency-of-elastic-pool-operations"></a>Rugalmas készlet műveletek késleltetése

- A szokásos módosítása a min eDTUs egy adatbázist, vagy a max eDTUs adatbázisonként egészíti ki az 5 perc vagy annál kisebb.
- A használati készlet eDTUs módosítása attól függ, hogy a készletben adatbázisokra által használt terület mennyiségét. Módosítások átlagos 90 perc vagy annál kevesebb 100 GB. Például a teljes méret által használt adatbázisokra készletben esetén 200 GB, akkor a várható időtartam módosítása a készlet eDTU készlet / 3 órát, vagy annál kisebb.

## <a name="migrate-from-v11-to-v12-servers"></a>V12 kiszolgálók V11 áttelepítése

Indítsa el, leállítása vagy figyelheti a frissítést az Azure SQL-adatbázis V12 V11 vagy bármely más előtti-V12 verzió PowerShell-parancsmagok érhetők el.

- [SQL-adatbázis V12 frissítés PowerShell használatával](sql-database-upgrade-server-powershell.md)

Hivatkozás dokumentációjában az alábbi PowerShell-parancsmagok a következő cikkekben talál:


- [Get-AzureRMSqlServerUpgrade](https://msdn.microsoft.com/library/azure/mt603582.aspx)
- [Kezdés-AzureRMSqlServerUpgrade](https://msdn.microsoft.com/library/azure/mt619403.aspx)
- [Leállítás-AzureRMSqlServerUpgrade](https://msdn.microsoft.com/library/azure/mt603589.aspx)


A Stop parancsmag azt jelenti, hogy visszavonása, nem mutasson az egérrel. Nincs mód a frissítést, indítsa újra az elejétől eltérő önéletrajz nem. A Stop parancsmag törli a köteggel és elengedi az összes szükséges források.

## <a name="next-steps"></a>Következő lépések

- [Rugalmas feladatok létrehozása](sql-database-elastic-jobs-overview.md) Rugalmas feladatok lehetővé teszik az SQL-T parancsprogramokat futtassanak adatbázisokat a készletben tetszőleges számú.
- Lásd: a [Méretezés ki az Azure SQL-adatbázis](sql-database-elastic-scale-introduction.md): rugalmas adatbázis eszközök segítségével méretezési, az adatok áthelyezése, lekérdezés, vagy hozzon létre a tranzakciók.
