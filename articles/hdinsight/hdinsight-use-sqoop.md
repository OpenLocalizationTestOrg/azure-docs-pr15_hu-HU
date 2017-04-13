<properties
    pageTitle="Hadoop Sqoop használata a HDInsight |} Microsoft Azure"
    description="Megtudhatja, hogy miként Azure a PowerShell használatával munkaállomásról Sqoop importálás futtatása és exportálása egy Hadoop fürthöz és Azure SQL-adatbázishoz között."
    editor="cgronlun"
    manager="jhubbard"
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/02/2016"
    ms.author="jgao"/>

#<a name="use-sqoop-with-hadoop-in-hdinsight"></a>A HDInsight Hadoop Sqoop használata

[AZURE.INCLUDE [sqoop-selector](../../includes/hdinsight-selector-use-sqoop.md)]

Megtudhatja, hogy miként Sqoop használata HDInsight importálásához és exportálásához HDInsight fürt és Azure SQL-adatbázis vagy SQL Server-adatbázis között.

Bár Hadoop semistructured strukturált és strukturálatlan adatokat, például naplókról és a fájlok feldolgozásra természetes választási lehetőséget is lehetnek feldolgozása relációs adatbázisok tárolt strukturált adatokat kell.

[Sqoop] [ sqoop-user-guide-1.4.4] egy eszköz célja, hogy az adatok átvitele Hadoop fürt és a relációs adatbázisok között. Használhatja az SQL Server, MySQL vagy Oracle be a Hadoop elosztott fájlrendszer () Fájlrendszerhez, az adatokat a Hadoop MapReduce vagy struktúra, és majd exportálja az adatokat egy RDBMS az importálandó adatokat egy relációs adatbázis-kezelő rendszer (RDBMS). Ebben az oktatóanyagban esetén a relációs adatbázisok SQL Server-adatbázishoz.

A HDInsight fürt támogatott Sqoop verziók című [HDInsight által biztosított fürt verziók újdonságai?] [hdinsight-versions].

##<a name="understand-the-scenario"></a>Eset bemutatása

HDInsight fürt megtalálható mintaadatokat tartalmaz. A következő két minta fogja használni:

- Egy log4j naplófájlt, amely */example/data/sample.log*helyen található. A következő naplók bontja a fájlból:

        2012-02-03 18:35:34 SampleClass6 [INFO] everything normal for id 577725851
        2012-02-03 18:35:34 SampleClass4 [FATAL] system problem at id 1991281254
        2012-02-03 18:35:34 SampleClass3 [DEBUG] detail for id 1304807656
        ...

- Egy struktúratáblával nevű *hivesampletable*, amely az adatfájl */hive/warehouse/hivesampletable*található hivatkozást. A tábla mobileszköz adatokat tartalmazza. 

  	| A mező | Adattípus |
  	| ----- | --------- |
  	| ClientID | karakterlánc |
  	| querytime | karakterlánc |
  	| piaci | karakterlánc |
  	| deviceplatform | karakterlánc |
  	| devicemake | karakterlánc |
  	| devicemodel | karakterlánc |
  	| állam | karakterlánc |
  	| ország | karakterlánc |
  	| querydwelltime | dupla |
  	| munkamenet-azonosító | bigint |
  	| sessionpagevieworder | bigint |

Először exportálása *sample.log* és *hivesampletable* az Azure SQL-adatbázis vagy SQL Server lesz, és importálja a táblázat az alábbi elérési út segítségével vissza HDInsight a mobileszköz adatokat tartalmazó:

    /tutorials/usesqoop/importeddata

## <a name="create-cluster-and-sql-database"></a>Fürt és SQL-adatbázis létrehozása

Ez a szakasz bemutatja, hogyan hozhat létre egy fürt és az SQL adatbázis sémák futó az oktatóprogram az Azure-portálra, és egy erőforrás-kezelő Azure-sablon segítségével.  Ha inkább Azure PowerShell-lel, olvassa el a [mintája](#appendix-a---a-powershell-sample)című témakört.

1. Kattintson az alábbi képen egy erőforrás manager sablon megnyitásához az Azure-portálon.         

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Fusesqoop%2Fcreate-linux-based-hadoop-cluster-in-hdinsight-and-sql-database.json" target="_blank"><img src="https://acom.azurecomcdn.net/80C57D/cdn/mediahandler/docarticles/dpsmedia-prod/azure.microsoft.com/en-us/documentation/articles/hdinsight-hbase-tutorial-get-started-linux/20160201111850/deploy-to-azure.png" alt="Deploy to Azure"></a>
    
    Az erőforrás-kezelő sablon nyilvános blob tároló, *https://hditutorialdata.blob.core.windows.net/usesqoop/create-linux-based-hadoop-cluster-in-hdinsight-and-sql-database.json*található. 
    
    Az erőforrás-kezelő sablon hívások SQL-adatbázissal szeretne telepíteni, a táblázat sémák bacpac csomagot.  A bacpac csomag nyilvános blob tároló, https://hditutorialdata.blob.core.windows.net/usesqoop/SqoopTutorial-2016-2-23-11-2.bacpac is található. Ha szeretne egy személyes tároló bacpac fájlokhoz használt, a sablon a következő értékeket használja:
    
        "storageKeyType": "Primary",
        "storageKey": "<TheAzureStorageAccountKey>",
    
2. A Paraméterek lap az adja meg az alábbiakat:

    - **Fürtnév**: Adja meg a létrehozandó Hadoop fürt nevét.
    - **Fürt felhasználónév és jelszó**: az alapértelmezett bejelentkezési neve rendszergazdaként.
    - **SSH felhasználónevet és jelszót**.
    - **SQL-adatbázis-bejelentkezési kiszolgáló nevét és jelszavát**.

    Az alábbi értékekre szoftveresen kötött változók szakaszában:
    
  	|Alapértelmezett tároló fióknév|<CluterName>tár|
  	|----------------------------|-----------------|
  	|Azure SQL-adatbázis kiszolgáló neve|<ClusterName>dbserver|
  	|Azure SQL-adatbázis neve|<ClusterName>KCS2|
    
    Kérjük, jegyezze fel ezeket az értékeket.  Szüksége lesz rájuk az oktatóprogram belül.
    
3.a mentéséhez kattintson **az OK** gombra a paraméterek.

4. az **Egyéni telepítés** lap, kattintson az **erőforrás csoport** legördülő listában, és kattintson az **Új** hozhat létre új erőforráscsoport. Az erőforráscsoport, amely a fürt, a függő tárterület-fiók és más csatolt erőforrás csoportosítja tároló.

5.a **jogi feltételek**gombra, és kattintson a **Létrehozás**gombra.

6. Kattintson **létrehozása**parancsra. Új sablon telepítéshez Submitting telepítési című csempe jelenik meg. Szükséges időt körülbelül 20 perc kapcsolatban a fürt és SQL-adatbázis létrehozása.

Ha úgy dönt, hogy a meglévő Azure SQL-adatbázis vagy a Microsoft SQL Server

- **Azure SQL-adatbázishoz**: meg kell adnia az Azure SQL adatbázis-kiszolgáló engedélyezze, hogy hozzáférjen a munkaállomásról tűzfal szabályt. A utasítások létrehozása az Azure SQL-adatbázis, és konfigurálása a tűzfalat, az [első lépések]című Azure SQL-adatbázis használata[sqldatabase-get-started]. 

    > [AZURE.NOTE] Alapértelmezés szerint az Azure SQL-adatbázishoz engedélyezi a csatlakozást az Azure szolgáltatásokból, például az Azure hdinsight szolgáltatáshoz. Ha a tűzfal beállítás be van kapcsolva, az Azure portálról kell engedélyezett. Az Azure SQL-adatbázis létrehozása és konfigurálása a tűzfalszabályokat kapcsolatos útmutatás, [létrehozása és SQL-adatbázis konfigurálása]című[sqldatabase-create-configue].

- **Az SQL Server**: Ha a HDInsight fürt SQL Server Azure-ban az azonos virtuális hálózaton, segítségével a lépéseket a jelen cikkben adatok importálása és exportálása az SQL Server-adatbázishoz.

    > [AZURE.NOTE] HDInsight támogatja csak helyfüggő virtuális hálózatok, és azt jelenleg nem működik a virtuális hálózatok affinitás csoport-alapú.

    * Szeretne létrehozni, és állítsa be a virtuális hálózat, lásd: az [létrehozása az Azure portálon virtuális hálózat](../virtual-network/virtual-networks-create-vnet-arm-pportal.md).

        * SQL Server használatakor a adatközpontban meg kell adnia a virtuális hálózati *hely közötti* vagy a *webhely-pont*.

            > [AZURE.NOTE] **Webhely-pont** virtuális hálózatok SQL Server futnia kell a VPN-ügyfél configuration alkalmazást, amely érhető el az **Irányítópult** az Azure virtuális hálózati konfiguráció.

        * Használatakor az SQL Server Azure virtuális-gépen, bármely virtuális hálózati konfigurálásról is használható, ha az SQL Server szolgáltatója virtuális gép virtuális hdinsight szolgáltatásból lehetőségre hálózatán tagja.

    * Hozzon létre egy HDInsight fürthöz egy virtuális hálózaton, lásd: [az egyéni beállításokkal HDInsight létrehozása Hadoop fürt](hdinsight-provision-clusters.md)

    > [AZURE.NOTE] Az SQL Server is engedélyeznie kell a hitelesítési. Egy SQL Server jelentkezzen be az Ez a cikk lépéseinek kell használnia.


## <a name="run-sqoop-jobs"></a>Sqoop feladatok futtatása

HDInsight futtatását is lehetővé teszi a Sqoop feladatok többféle módszerrel használatával. Döntse el, melyik módszert szüksége az alábbi táblázat segítségével, majd hajtsa végre az útmutató hivatkozását.

| **Ezzel a** ...                                   | .. .an **interaktív** rendszerhéj | ... **parancsfájl** | .. .with **fürt operációs rendszer** | .. .from **ügyfél operációs rendszer** |
|:--------------------------------------------------------------|:---------------------------:|:-----------------------:|:------------------------------------------|:-----------------------------------------|
| [SSH](hdinsight-use-sqoop-mac-linux.md)                        |              ✔              |            ✔            | Linux                                     | Linux rendszerhez, a Unix, a Mac OS X vagy a Windows        |
| [.NET SDK Hadoop-](hdinsight-hadoop-use-sqoop-dotnet-sdk.md) |           &nbsp;            |            ✔            | Linux vagy a Windows                          | Windows (egyelőre)                        |
| [Azure PowerShell](hdinsight-hadoop-use-sqoop-powershell.md)  |           &nbsp;            |            ✔            | Linux vagy a Windows                          | A Windows                                  |

##<a name="limitations"></a>Korlátozások

* Tömeges export - a Linux-alapú HDInsight, az adatok exportálása a Microsoft SQL Server- vagy SQL Azure-adatbázis használt Sqoop összekötő jelenleg nem támogatja a tömeges szúr be.

* Kötegelés – a HDInsight Linux-alapú használata esetén a `-batch` beszúrása végrehajtásakor váltás, Sqoop több beszúrása a Beszúrás műveletek kötegelés helyett hajt végre.

##<a name="next-steps"></a>Következő lépések

Most már megtanulta azt is van Sqoop használatáról. További tudnivalókért lásd:

- [HDInsight struktúra használata](hdinsight-use-hive.md)
- [Malac használata hdinsight szolgáltatáshoz](hdinsight-use-pig.md)
- [Oozie használata HDInsight][hdinsight-use-oozie]: használata Sqoop művelet Oozie-munkafolyamatokban.
- [Adatelemzés nézetbeli késleltetés segítségével HDInsight][hdinsight-analyze-flight-data]: nézetbeli elemzéséhez használható struktúra adatok késleltetése, majd Sqoop adatainak exportálása Azure SQL-adatbázishoz.
- [Töltse fel az adatok hdinsight szolgáltatáshoz][hdinsight-upload-data]: más módszerek a feltöltéshez HDInsight/Azure Blob-tárolóhoz találja.


## <a name="appendix-a---a-powershell-sample"></a>Mintája - PowerShell minta

A PowerShell minta hajtja végre az alábbi lépéseket:

1. Azure csatlakozni.
2. Hozzon létre egy Azure erőforráscsoport. További tudnivalókért lásd: [Azure PowerShell használatá a Azure-kezelő eszközzel](../powershell-azure-resource-manager.md)
3. Azure SQL-adatbázis-kiszolgáló, a Azure SQL-adatbázishoz és a két tábla létrehozása. 

    Ha inkább az SQL-kiszolgálót használ, a következő kimutatások használatával a táblázatok létrehozása:
    
        CREATE TABLE [dbo].[log4jlogs](
         [t1] [nvarchar](50),
         [t2] [nvarchar](50),
         [t3] [nvarchar](50),
         [t4] [nvarchar](50),
         [t5] [nvarchar](50),
         [t6] [nvarchar](50),
         [t7] [nvarchar](50))

        CREATE TABLE [dbo].[mobiledata](
         [clientid] [nvarchar](50),
         [querytime] [nvarchar](50),
         [market] [nvarchar](50),
         [deviceplatform] [nvarchar](50),
         [devicemake] [nvarchar](50),
         [devicemodel] [nvarchar](50),
         [state] [nvarchar](50),
         [country] [nvarchar](50),
         [querydwelltime] [float],
         [sessionid] [bigint],
         [sessionpagevieworder][bigint])

    A vizsgálja meg az adatbázist és táblákat legegyszerűbben a Visual Studio használni. Az adatbázis-kiszolgáló és az adatbázis az Azure portálon vizsgálni el.

4. Hozzon létre egy HDInsight fürthöz.

    Vizsgálja meg a fürt, az Azure portálja vagy az Azure PowerShell is használhatja.

5. A forrásfájl adatok előre feldolgozása.

    Ebben az oktatóanyagban exportálja log4j naplófájlt (tagolt fájl), és egy struktúratáblával Azure SQL-adatbázishoz. A tagolt fájlt nevezzük */example/data/sample.log*. Az oktatóprogram során log4j naplók néhány mintái bemutattuk. A naplófájlban létezik néhány üres és bizonyos sorokat ezek hasonló:
    
        java.lang.Exception: 2012-02-03 20:11:35 SampleClass2 [FATAL] unrecoverable system problem at id 609774657
            at com.osa.mocklogger.MockLogger$2.run(MockLogger.java:83)
    
    Ez nem kell aggódnia további példákat, amely ezeket az adatokat, de azt el kell távolítania a kivételek azt be az Azure SQL-adatbázis vagy az SQL Server importálásához. Sqoop exportálás sikertelen lesz, ha a nem üres karakterláncot vagy egy sor olyan annál kevesebb, mint az Azure SQL-adatbázis táblázatban megadott mezőket száma elemek száma. A log4jlogs táblának 7 karakterlánc típusú mezők.

    Ez az eljárás létrehoz egy új fájlt a fürt a: tutorials/usesqoop/data/sample.log. Vizsgálja meg a módosítás dátuma fájlt, az Azure-portálra, az Azure tároló explorer eszköz és Azure PowerShell is használhatja. [Első lépések a HDInsight] [ hdinsight-get-started] Azure PowerShell használata egy fájl letöltésére, valamint a fájl tartalmának megjelenítése a minta kódot tartalmaz.

6. Adatfájl exportálása az Azure SQL-adatbázishoz.

    A forrásfájl tutorials/usesqoop/data/sample.log. A táblázat, ahol az adatok exportálása log4jlogs neve.
    
    > [AZURE.NOTE] Eltérő kapcsolat-karakterlánc adatait vagy az SQL Server Azure SQL-adatbázis kell működniük ebben a szakaszban szereplő lépéseket. Az alábbi beállítások használatával ellenőrzött ezeket a lépéseket:
    >
    > * **Azure virtuális hálózat pont-webhely konfigurálása**: virtuális hálózat a HDInsight fürt csatlakozik egy SQL Server egy privát adatközponthoz. További információt talál [az adatkezelési portálon virtuális Magánhálózattal pont-webhely konfigurálása](../vpn-gateway/vpn-gateway-point-to-site-create.md) .
    > * **Azure hdinsight szolgáltatáshoz 3.1**: lásd: [az egyéni beállításokkal HDInsight létrehozása Hadoop fürt](hdinsight-provision-clusters.md) fürt létrehozása egy virtuális hálózaton információt.
    > * **Az SQL Server 2014-es**: ahhoz, hogy hitelesítési és futtatása a VPN-ügyfél biztonságosan csatlakozni a virtuális hálózati konfigurációja csomag konfigurált.

7. Az Azure SQL-adatbázis struktúra táblázat exportálása.

8. A mobiledata táblázat importálása a HDInsight fürt.

    Vizsgálja meg a módosítás dátuma fájlt, az Azure-portálra, az Azure tároló explorer eszköz és Azure PowerShell is használhatja.  [Első lépések a HDInsight] [ hdinsight-get-started] Azure PowerShell használata egy fájl letöltésére, és a fájl tartalmának megjelenítése kód minta rendelkezik.


### <a name="the-powershell-sample"></a>A PowerShell-minta

    # Prepare an Azure SQL database to be used by the Sqoop tutorial
    
    #region - provide the following values
    
    $subscriptionID = "<Enter your Azure Subscription ID>"
    
    $sqlDatabaseLogin = "<Enter a SQL Database Login name>" #SQL Database server login
    $sqlDatabasePassword = "<Enter a Password>"
    
    $httpUserName = "admin"  #HDInsight cluster username
    $httpPassword = "<Enter a Password>"
    
    # used for creating Azure service names
    $nameToken = "<Enter an alias>" 
    $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")
    #endregion
    
    #region - variables
    
    # Resource group variables
    $resourceGroupName = $namePrefix + "rg"
    $location = "East US 2" # used by all Azure services defined in this tutorial
    
    # SQL database varialbes
    $sqlDatabaseServerName = $namePrefix + "sqldbserver"
    $sqlDatabaseName = $namePrefix + "sqldb"
    $sqlDatabaseConnectionString = "Data Source=$sqlDatabaseServerName.database.windows.net;Initial Catalog=$sqlDatabaseName;User ID=$sqlDatabaseLogin;Password=$sqlDatabasePassword;Encrypt=true;Trusted_Connection=false;"
    $sqlDatabaseMaxSizeGB = 10
    
    # Used for retrieving external IP address and creating firewall rules
    $ipAddressRestService = "http://bot.whatismyipaddress.com"
    $fireWallRuleName = "UseSqoop"
    
    # Used for creating tables and clustered indexes
    $cmdCreateLog4jTable = "CREATE TABLE [dbo].[log4jlogs](
        [t1] [nvarchar](50),
        [t2] [nvarchar](50),
        [t3] [nvarchar](50),
        [t4] [nvarchar](50),
        [t5] [nvarchar](50),
        [t6] [nvarchar](50),
        [t7] [nvarchar](50))"
    
    $cmdCreateLog4jClusteredIndex = "CREATE CLUSTERED INDEX log4jlogs_clustered_index on log4jlogs(t1)"
    
    $cmdCreateMobileTable = " CREATE TABLE [dbo].[mobiledata](
    [clientid] [nvarchar](50),
    [querytime] [nvarchar](50),
    [market] [nvarchar](50),
    [deviceplatform] [nvarchar](50),
    [devicemake] [nvarchar](50),
    [devicemodel] [nvarchar](50),
    [state] [nvarchar](50),
    [country] [nvarchar](50),
    [querydwelltime] [float],
    [sessionid] [bigint],
    [sessionpagevieworder][bigint])"
    
    $cmdCreateMobileDataClusteredIndex = "CREATE CLUSTERED INDEX mobiledata_clustered_index on mobiledata(clientid)"
    
    # HDInsight variables
    $hdinsightClusterName = $namePrefix + "hdi"
    $defaultStorageAccountName = $namePrefix + "store"
    $defaultBlobContainerName = $hdinsightClusterName
    #endregion
    
    # Treat all errors as terminating
    $ErrorActionPreference = "Stop"
    
    #region - Connect to Azure subscription
    Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
    try{Get-AzureRmContext}
    catch{Login-AzureRmAccount}
    #endregion
    
    #region - Create Azure resouce group
    Write-Host "`nCreating an Azure resource group ..." -ForegroundColor Green
    try{
        Get-AzureRmResourceGroup -Name $resourceGroupName
    }
    catch{
        New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
    }
    #endregion
    
    #region - Create Azure SQL database server
    Write-Host "`nCreating an Azure SQL Database server ..." -ForegroundColor Green
    try{
        Get-AzureRmSqlServer -ServerName $sqlDatabaseServerName -ResourceGroupName $resourceGroupName}
    catch{
        Write-Host "`nCreating SQL Database server ..."  -ForegroundColor Green
    
        $sqlDatabasePW = ConvertTo-SecureString -String $sqlDatabasePassword -AsPlainText -Force
        $credential = New-Object System.Management.Automation.PSCredential($sqlDatabaseLogin,$sqlDatabasePW)
    
        $sqlDatabaseServerName = (New-AzureRmSqlServer `
                                    -ResourceGroupName $resourceGroupName `
                                    -ServerName $sqlDatabaseServerName `
                                    -SqlAdministratorCredentials $credential `
                                    -Location $location).ServerName
        Write-Host "`tThe new SQL database server name is $sqlDatabaseServerName." -ForegroundColor Cyan
    
        Write-Host "`nCreating firewall rule, $fireWallRuleName ..." -ForegroundColor Green
        $workstationIPAddress = Invoke-RestMethod $ipAddressRestService
        New-AzureRmSqlServerFirewallRule `
            -ResourceGroupName $resourceGroupName `
            -ServerName $sqlDatabaseServerName `
            -FirewallRuleName "$fireWallRuleName-workstation" `
            -StartIpAddress $workstationIPAddress `
            -EndIpAddress $workstationIPAddress
    
        #To allow other Azure services to access the server add a firewall rule and set both the StartIpAddress and EndIpAddress to 0.0.0.0. 
        #Note that this allows Azure traffic from any Azure subscription to access the server.
        New-AzureRmSqlServerFirewallRule `
            -ResourceGroupName $resourceGroupName `
            -ServerName $sqlDatabaseServerName `
            -FirewallRuleName "$fireWallRuleName-Azureservices" `
            -StartIpAddress "0.0.0.0" `
            -EndIpAddress "0.0.0.0"
    }
    
    #endregion
    
    #region - Create and validate Azure SQL database
    Write-Host "`nCreating an Azure SQL database ..." -ForegroundColor Green
    
    try {
        Get-AzureRmSqlDatabase `
            -ResourceGroupName $resourceGroupName `
            -ServerName $sqlDatabaseServerName `
            -DatabaseName $sqlDatabaseName
    }
    catch {
        Write-Host "`nCreating SQL Database, $sqlDatabaseName ..."  -ForegroundColor Green
        New-AzureRMSqlDatabase `
            -ResourceGroupName $resourceGroupName `
            -ServerName $sqlDatabaseServerName `
            -DatabaseName $sqlDatabaseName `
            -Edition "Standard" `
            -RequestedServiceObjectiveName "S1"
    }
    
    #endregion
    
    #region - Create tables
    Write-Host "Creating the log4jlogs table and the mobiledata table ..." -ForegroundColor Green
    
    $conn = New-Object System.Data.SqlClient.SqlConnection
    $conn.ConnectionString = $sqlDatabaseConnectionString
    $conn.Open()
    
    # Create the log4jlogs table and index
    $cmd = New-Object System.Data.SqlClient.SqlCommand
    $cmd.Connection = $conn
    $cmd.CommandText = $cmdCreateLog4jTable
    $ret = $cmd.ExecuteNonQuery()
    $cmd.CommandText = $cmdCreateLog4jClusteredIndex
    $cmd.ExecuteNonQuery()
    
    # Create the mobiledata table and index
    $cmd.CommandText = $cmdCreateMobileTable
    $cmd.ExecuteNonQuery()
    $cmd.CommandText = $cmdCreateMobileDataClusteredIndex
    $cmd.ExecuteNonQuery()
    
    $conn.close()
    
    #endregion
    
    
    #region - Create HDInsight cluster
    
    Write-Host "Creating the HDInsight cluster and the dependent services ..." -ForegroundColor Green
    
    # Create the default storage account
    New-AzureRmStorageAccount `
        -ResourceGroupName $resourceGroupName `
        -Name $defaultStorageAccountName `
        -Location $location `
        -Type Standard_LRS
    
    # Create the default Blob container
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey `
                                    -ResourceGroupName $resourceGroupName `
                                    -Name $defaultStorageAccountName)[0].Value
    $defaultStorageAccountContext = New-AzureStorageContext `
                                        -StorageAccountName $defaultStorageAccountName `
                                        -StorageAccountKey $defaultStorageAccountKey 
    New-AzureStorageContainer `
        -Name $defaultBlobContainerName `
        -Context $defaultStorageAccountContext 
    
    # Create the HDInsight cluster
    $pw = ConvertTo-SecureString -String $httpPassword -AsPlainText -Force
    $httpCredential = New-Object System.Management.Automation.PSCredential($httpUserName,$pw)
    
    New-AzureRmHDInsightCluster `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $HDInsightClusterName `
        -Location $location `
        -ClusterType Hadoop `
        -OSType Windows `
        -ClusterSizeInNodes 2 `
        -HttpCredential $httpCredential `
        -DefaultStorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultStorageContainer $defaultBlobContainerName 
    
    # Validate the cluster
    Get-AzureRmHDInsightCluster -ClusterName $hdinsightClusterName
    #endregion
    
    #region - pre-process the source file
    
    Write-Host "Preprocessing the source file ..." -ForegroundColor Green
    
    # This procedure creates a new file with $destBlobName
    $sourceBlobName = "example/data/sample.log"
    $destBlobName = "tutorials/usesqoop/data/sample.log"
    
    # Define the connection string
    $storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=$defaultStorageAccountName;AccountKey=$defaultStorageAccountKey"
    
    # Create block blob objects referencing the source and destination blob.
    $storageAccount = [Microsoft.WindowsAzure.Storage.CloudStorageAccount]::Parse($storageConnectionString)
    $storageClient = $storageAccount.CreateCloudBlobClient();
    $storageContainer = $storageClient.GetContainerReference($defaultBlobContainerName)
    $sourceBlob = $storageContainer.GetBlockBlobReference($sourceBlobName)
    $destBlob = $storageContainer.GetBlockBlobReference($destBlobName)
    
    # Define a MemoryStream and a StreamReader for reading from the source file
    $stream = New-Object System.IO.MemoryStream
    $stream = $sourceBlob.OpenRead()
    $sReader = New-Object System.IO.StreamReader($stream)
    
    # Define a MemoryStream and a StreamWriter for writing into the destination file
    $memStream = New-Object System.IO.MemoryStream
    $writeStream = New-Object System.IO.StreamWriter $memStream
    
    # Pre-process the source blob
    $exString = "java.lang.Exception:"
    while(-Not $sReader.EndOfStream){
        $line = $sReader.ReadLine()
        $split = $line.Split(" ")
    
        # remove the "java.lang.Exception" from the first element of the array
        # for example: java.lang.Exception: 2012-02-03 19:11:02 SampleClass8 [WARN] problem finding id 153454612
        if ($split[0] -eq $exString){
            #create a new ArrayList to remove $split[0]
            $newArray = [System.Collections.ArrayList] $split
            $newArray.Remove($exString)
    
            # update $split and $line
            $split = $newArray
            $line = $newArray -join(" ")
        }
    
        # remove the lines that has less than 7 elements
        if ($split.count -ge 7){
            write-host $line
            $writeStream.WriteLine($line)
        }
    }
    
    # Write to the destination blob
    $writeStream.Flush()
    $memStream.Seek(0, "Begin")
    $destBlob.UploadFromStream($memStream)
    
    #endregion
    
    #region - export a log file from the cluster to the SQL database
    
    Write-Host "Preprocessing the source file ..." -ForegroundColor Green
    
    $tableName_log4j = "log4jlogs"
    
    # Connection string for Azure SQL Database.
    # Comment if using SQL Server
    $connectionString = "jdbc:sqlserver://$sqlDatabaseServerName.database.windows.net;user=$sqlDatabaseLogin@$sqlDatabaseServerName;password=$sqlDatabasePassword;database=$sqlDatabaseName"
    # Connection string for SQL Server.
    # Uncomment if using SQL Server.
    #$connectionString = "jdbc:sqlserver://$sqlDatabaseServerName;user=$sqlDatabaseLogin;password=$sqlDatabasePassword;database=$sqlDatabaseName"
    
    $exportDir_log4j = "/tutorials/usesqoop/data"
    
    # Submit a Sqoop job
    $sqoopDef = New-AzureRmHDInsightSqoopJobDefinition `
        -Command "export --connect $connectionString --table $tableName_log4j --export-dir $exportDir_log4j --input-fields-terminated-by \0x20 -m 1"
    $sqoopJob = Start-AzureRmHDInsightJob `
                    -ClusterName $hdinsightClusterName `
                    -HttpCredential $httpCredential `
                    -JobDefinition $sqoopDef #-Debug -Verbose
    Wait-AzureRmHDInsightJob `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId
    
    Write-Host "Standard Error" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput -ResourceGroupName $resourceGroupName -ClusterName $hdinsightClusterName -DefaultStorageAccountName $defaultStorageAccountName -DefaultStorageAccountKey $defaultStorageAccountKey -DefaultContainer $defaultBlobContainerName -HttpCredential $httpCredential -JobId $sqoopJob.JobId -DisplayOutputType StandardError
    Write-Host "Standard Output" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput -ResourceGroupName $resourceGroupName -ClusterName $hdinsightClusterName -DefaultStorageAccountName $defaultStorageAccountName -DefaultStorageAccountKey $defaultStorageAccountKey -DefaultContainer $defaultBlobContainerName -HttpCredential $httpCredential -JobId $sqoopJob.JobId -DisplayOutputType StandardOutput
    
    #endregion
    
    #region - export a Hive table
    
    $tableName_mobile = "mobiledata"
    $exportDir_mobile = "/hive/warehouse/hivesampletable"
    
    $sqoopDef = New-AzureRmHDInsightSqoopJobDefinition `
        -Command "export --connect $connectionString --table $tableName_mobile --export-dir $exportDir_mobile --fields-terminated-by \t -m 1"
    $sqoopJob = Start-AzureRmHDInsightJob `
                    -ClusterName $hdinsightClusterName `
                    -HttpCredential $httpCredential `
                    -JobDefinition $sqoopDef #-Debug -Verbose
    
    Wait-AzureRmHDInsightJob `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId
    
    Write-Host "Standard Error" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -DefaultStorageAccountName $defaultStorageAccountName `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultContainer $defaultBlobContainerName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId `
        -DisplayOutputType StandardError
    
    Write-Host "Standard Output" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -DefaultStorageAccountName $defaultStorageAccountName `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultContainer $defaultBlobContainerName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId `
        -DisplayOutputType StandardOutput
    
    #endregion
    
    #region - import a database
    
    $targetDir_mobile = "/tutorials/usesqoop/importeddata/"
    
    $sqoopDef = New-AzureRmHDInsightSqoopJobDefinition `
        -Command "import --connect $connectionString --table $tableName_mobile --target-dir $targetDir_mobile --fields-terminated-by \t --lines-terminated-by \n -m 1"
    
    $sqoopJob = Start-AzureRmHDInsightJob `
                    -ClusterName $hdinsightClusterName `
                    -HttpCredential $httpCredential `
                    -JobDefinition $sqoopDef #-Debug -Verbose
    
    Wait-AzureRmHDInsightJob `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId
    
    Write-Host "Standard Error" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -DefaultStorageAccountName $defaultStorageAccountName `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultContainer $defaultBlobContainerName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId `
        -DisplayOutputType StandardError
    
    Write-Host "Standard Output" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -DefaultStorageAccountName $defaultStorageAccountName `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultContainer $defaultBlobContainerName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId `
        -DisplayOutputType StandardOutput
    
    #endregion



[azure-management-portal]: https://portal.azure.com/

[hdinsight-versions]:  hdinsight-component-versioning.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-storage]: ../hdinsight-hadoop-use-blob-storage.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md

[sqldatabase-get-started]: ../sql-database/sql-database-get-started.md
[sqldatabase-create-configue]: ../sql-database/sql-database-get-started.md

[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-install]: powershell-install-configure.md
[powershell-script]: http://technet.microsoft.com/library/ee176949.aspx

[sqoop-user-guide-1.4.4]: https://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html
