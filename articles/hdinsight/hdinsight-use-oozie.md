<properties
    pageTitle="Hadoop Oozie használata a HDInsight |} Microsoft Azure"
    description="Hadoop Oozie HDInsight, nagy adatok szolgáltatás használja. Megtudhatja, hogy miként egy Oozie munkafolyamat megadása, és küldjön egy Oozie feladatot."
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/25/2016"
    ms.author="jgao"/>


# <a name="use-oozie-with-hadoop-to-define-and-run-a-workflow-in-hdinsight"></a>Hadoop meghatározása és a munkafolyamat futása HDInsight Oozie használata

[AZURE.INCLUDE [oozie-selector](../../includes/hdinsight-oozie-selector.md)]

Megtudhatja, hogy miként Apache Oozie használatával egy munkafolyamat megadása, és futtassa a munkafolyamatot, a hdinsight szolgáltatásból lehetőségre. A Oozie koordinátor kapcsolatos további tudnivalókért lásd: [Időalapú Hadoop Oozie koordinátor használata a HDInsight][hdinsight-oozie-coordinator-time]. Azure Data Factory információ [használata malac és az adatok gyári struktúra][azure-data-factory-pig-hive].

Apache Oozie munkafolyamat/effektusával rendszer kezelése a Hadoop feladatokat. A Hadoop Papírhalom integrálva van, illetve Hadoop feladatok Apache MapReduce, Apache malac, Apache struktúra és Apache Sqoop támogatja. Azt is használható a rendszer, például Java-programok vagy rendszerhéj parancsfájlok jellemző feladatok ütemezése.

A munkafolyamat alkalmazhat a képernyőn megjelenő utasításokat követve ebben az oktatóanyagban két művelet tartalmazza:

![Munkafolyamat-diagramokhoz.][img-workflow-diagram]

1. A struktúra művelet megszámlálására log4j fájlban minden napló szintű típusú HiveQL parancsfájl fut. Minden egyes log4j fájlt, hogy a típus súlyosságát, például egy [NAPLÓZÁSI szintjének] mezőt tartalmazó mezők vonal áll:

        2012-02-03 18:35:34 SampleClass6 [INFO] everything normal for id 577725851
        2012-02-03 18:35:34 SampleClass4 [FATAL] system problem at id 1991281254
        2012-02-03 18:35:34 SampleClass3 [DEBUG] detail for id 1304807656
        ...

    A struktúra eredményébe hasonlít:

        [DEBUG] 434
        [ERROR] 3
        [FATAL] 1
        [INFO]  96
        [TRACE] 816
        [WARN]  4

    Struktúra kapcsolatos további tudnivalókért lásd: a [Használata a HDInsight struktúra][hdinsight-use-hive].

2.  Egy Sqoop művelet exportálása a HiveQL kimeneti Azure SQL-adatbázisban. Sqoop kapcsolatos további tudnivalókért lásd: [Használata Hadoop Sqoop a HDInsight][hdinsight-use-sqoop].

> [AZURE.NOTE] A HDInsight fürt támogatott Oozie verziók című [HDInsight által biztosított Hadoop fürt verziók újdonságai?] [hdinsight-versions].

###<a name="prerequisites"></a>Előfeltételek

Ebben az oktatóanyagban megkezdése előtt a következőket kell rendelkeznie:

- **Azure PowerShell munkaállomás**. 

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]
    
    A Windows PowerShell-parancsfájlokat végrehajtás, futtatása rendszergazdaként, és állítsa az adatvégrehajtás házirendet *RemoteSigned*. További tudnivalókért lásd: [futtassa a Windows PowerShell-parancsfájlokat][powershell-script].

##<a name="define-oozie-workflow-and-the-related-hiveql-script"></a>Oozie munkafolyamat és a kapcsolódó HiveQL parancsfájl meghatározása

Munkafolyamat-definíciók Oozie írt hPDL (egy XML folyamat Definition Language). Az alapértelmezett munkafolyamat fájl neve *workflow.xml*. Az alábbiakban megtalálja a munkafolyamat-fájlt, ebben az oktatóanyagban használandó.

    <workflow-app name="useooziewf" xmlns="uri:oozie:workflow:0.2">
        <start to = "RunHiveScript"/>

        <action name="RunHiveScript">
            <hive xmlns="uri:oozie:hive-action:0.2">
                <job-tracker>${jobTracker}</job-tracker>
                <name-node>${nameNode}</name-node>
                <configuration>
                    <property>
                        <name>mapred.job.queue.name</name>
                        <value>${queueName}</value>
                    </property>
                </configuration>
                <script>${hiveScript}</script>
                <param>hiveTableName=${hiveTableName}</param>
                <param>hiveDataFolder=${hiveDataFolder}</param>
                <param>hiveOutputFolder=${hiveOutputFolder}</param>
            </hive>
            <ok to="RunSqoopExport"/>
            <error to="fail"/>
        </action>

        <action name="RunSqoopExport">
            <sqoop xmlns="uri:oozie:sqoop-action:0.2">
                <job-tracker>${jobTracker}</job-tracker>
                <name-node>${nameNode}</name-node>
                <configuration>
                    <property>
                        <name>mapred.compress.map.output</name>
                        <value>true</value>
                    </property>
                </configuration>
            <arg>export</arg>
            <arg>--connect</arg>
            <arg>${sqlDatabaseConnectionString}</arg>
            <arg>--table</arg>
            <arg>${sqlDatabaseTableName}</arg>
            <arg>--export-dir</arg>
            <arg>${hiveOutputFolder}</arg>
            <arg>-m</arg>
            <arg>1</arg>
            <arg>--input-fields-terminated-by</arg>
            <arg>"\001"</arg>
            </sqoop>
            <ok to="end"/>
            <error to="fail"/>
        </action>

        <kill name="fail">
            <message>Job failed, error message[${wf:errorMessage(wf:lastErrorNode())}] </message>
        </kill>

        <end name="end"/>
    </workflow-app>

Vannak olyan definiált az munkafolyamatokban két művelet. A kezdő művelet *RunHiveScript*. A művelet sikeresen elindul, a következő műveletet esetén *RunSqoopExport*.

A RunHiveScript többváltozós tartalmaz. Az értékeket a elküldésekor a Oozie feladat a munkaállomásról Azure PowerShell használatával továbbítja.

<table border = "1">
<tr><th>Munkafolyamat-változó</th><th>Leírás</th></tr>
<tr><td>${jobTracker}</td><td>Adja meg a Hadoop feladat nyilvántartása URL-CÍMÉT. <strong>Jobtrackerhost:9010</strong> HDInsight 3.0-s és 2.1-es verzióját használja.</td></tr>
<tr><td>${nameNode}</td><td>Itt adhatja meg az URL-CÍMÉT a Hadoop neve csomópontot. Használhatja az alapértelmezett fájl rendszer cím, például <i>wasbs: / /&lt;containerName&gt;@&lt;storageAccountName&gt;. blob.core.windows.net</i>.</td></tr>
<tr><td>${Várólistanév}</td><td>Megadja, hogy a feladat terjeszteni várólista nevét. Használja az <strong>alapértelmezett</strong>.</td></tr>
</table>

<table border = "1">
<tr><th>Művelet változó struktúra</th><th>Leírás</th></tr>
<tr><td>${hiveDataFolder}</td><td>Itt adhatja meg a forrás könyvtár a Struktúratáblát létrehozása parancsot.</td></tr>
<tr><td>${hiveOutputFolder}</td><td>Adja meg a kimeneti mappát a BESZÚRÁSA FELÜLÍRJA a kimutatáshoz.</td></tr>
<tr><td>${hiveTableName}</td><td>Az adatfájlok log4j hivatkozó struktúratáblával nevét adja meg.</td></tr>
</table>

<table border = "1">
<tr><th>Sqoop művelet változó</th><th>Leírás</th></tr>
<tr><td>${sqlDatabaseConnectionString}</td><td>Itt adhatja meg az Azure SQL-adatbázis kapcsolati karakterláncot.</td></tr>
<tr><td>${sqlDatabaseTableName}</td><td>Adja meg a Ha az adatok exportálásának módját az Azure SQL adatbázistáblát.</td></tr>
<tr><td>${hiveOutputFolder}</td><td>Adja meg a kimeneti mappát a struktúra BESZÚRÁSA FELÜLÍRJA a kimutatáshoz. Ez az ugyanabban a mappában Sqoop exportálásához (export-dir).</td></tr>
</table>

Oozie munkafolyamat és a munkafolyamat-műveletek használatával kapcsolatos további tudnivalókért lásd: [Apache Oozie 4.0 dokumentáció] [ apache-oozie-400] (a HDInsight 3.0-s verziója) vagy [Apache Oozie 3.3.2 dokumentáció] [ apache-oozie-332] (a HDInsight 2.1-es verzió).


A munkafolyamat a struktúra műveletet HiveQL parancsfájl hívások. A parancsprogram három HiveQL kimutatások tartalmazza:

    DROP TABLE ${hiveTableName};
    CREATE EXTERNAL TABLE ${hiveTableName}(t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) ROW FORMAT DELIMITED FIELDS TERMINATED BY ' ' STORED AS TEXTFILE LOCATION '${hiveDataFolder}';
    INSERT OVERWRITE DIRECTORY '${hiveOutputFolder}' SELECT t4 AS sev, COUNT(*) AS cnt FROM ${hiveTableName} WHERE t4 LIKE '[%' GROUP BY t4;

1. **Húzza a TABLE utasítás** a log4j struktúratáblával törli, ha van ilyen.
2. **A CREATE TABLE utasítás** létrehoz egy log4j külső struktúratáblával, hogy a megfelelő helyre a log4j naplófájl pontok. A mező határolójel ",". Az alapértelmezett sor "\n" határolójel. Egy külső struktúratáblával eltávolítása a csoportból az eredeti helyre Ha többször is a Oozie munkafolyamatot futtatni szeretné az adatokat tartalmazó fájl elkerülése érdekében használják.
3. **A Beszúrás FELÜLÍRJA a kimutatás** a log4j struktúratáblával napló szintű típusonként előfordulását megszámolja, és menti a kimenet Azure-tárolóban lévő blob.


Létezik a parancsfájl használt három változóval:

- ${hiveTableName}
- ${hiveDataFolder}
- ${hiveOutputFolder}

A munkafolyamat-definíciós fájl (ebben az oktatóanyagban workflow.xml) ezeket az értékeket ez HiveQL parancsfájl futásidőben továbbítja.

A munkafolyamat és a HiveQL fájlt is blob-tároló tárolja.  A PowerShell-parancsprogramot, később az oktatóprogram használandó mindkét fájlok másolása az alapértelmezett tárterület-fiók. 

##<a name="submit-oozie-jobs-using-powershell"></a>A PowerShell használatával Oozie feladatok elküldése

Azure PowerShell jelenleg nem nyújt bármely parancsmagok definiáló Oozie feladatokat. A **Invoke-RestMethod** parancsmag Oozie webszolgáltatásokhoz meghívásához is használhatja. A Oozie web services API a HTTP REST API-val JSON. A Oozie webszolgáltatásokhoz API-val kapcsolatos további tudnivalókért lásd: [Apache Oozie 4.0 dokumentáció] [ apache-oozie-400] (a HDInsight 3.0-s verziója) vagy [Apache Oozie 3.3.2 dokumentáció] [ apache-oozie-332] (a HDInsight 2.1-es verzió).

Ebben a szakaszban a PowerShell parancsprogramot hajtja végre az alábbi lépéseket:

1. Azure csatlakozni.
2. Hozzon létre egy Azure erőforráscsoport. További információ a [Azure PowerShell Azure erőforrás-kezelővel](../powershell-azure-resource-manager.md)témakörben.
3. Azure SQL-adatbázis-kiszolgáló, a Azure SQL-adatbázishoz és a két tábla létrehozása. Ezek az munkafolyamatokban Sqoop művelet használják.

    A táblanév nem *log4jLogCount*.

4. Hozzon létre egy HDInsight fürthöz Oozie feladatok futtatásához használt.

    Vizsgálja meg a fürt, az Azure portálja vagy az Azure PowerShell is használhatja.

5. Az alapértelmezett fájlrendszer másolja a oozie munkafolyamat és a HiveQL parancsprogram-fájlt.

    Egy nyilvános Blob-tárolóhoz tárolja a fájlokat.
    
    - Másolja a HiveQL parancsfájl (useoozie.hql) Azure tárolóhoz (wasbs:///tutorials/useoozie/useoozie.hql).
    - Másolja a workflow.xml wasbs:///tutorials/useoozie/workflow.xml.
    - Másolja az adatokat tartalmazó fájl (/ example/data/sample.log) való wasbs:///tutorials/useoozie/data/sample.log.
     
6. Küldjön egy Oozie feladatot.

    A feladat OOzie eredmények vizsgálatához segítségével Visual Studio vagy egyéb eszközök az Azure SQL-adatbázis csatlakoztatása.

Az alábbiakban a parancsfájl.  A Windows PowerShell ISE futtatását is lehetővé teszi a parancsfájl. Csak kell konfigurálni az első 7 változók.

    #region - provide the following values
    
    $subscriptionID = "<Enter your Azure subscription ID>"
    
    # SQL Database server login credentials used for creating and connecting
    $sqlDatabaseLogin = "<Enter SQL Database Login Name>"
    $sqlDatabasePassword = "<Enter SQL Database Login Password>"
    
    # HDInsight cluster HTTP user credential used for creating and connectin
    $httpUserName = "admin"  # The default name is "admin"
    $httpPassword = "<Enter HDInsight Cluster HTTP User Password>"
    
    # Used for creating Azure service names
    $nameToken = "<Enter an Alias>"
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
    catch{
        Login-AzureRmAccount
        Select-AzureRmSubscription -SubscriptionId $subscriptionID
    }
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
        $sqlLoginCredentials = New-Object System.Management.Automation.PSCredential($sqlDatabaseLogin,$sqlDatabasePW)
    
        $sqlDatabaseServerName = (New-AzureRmSqlServer `
                                    -ResourceGroupName $resourceGroupName `
                                    -ServerName $sqlDatabaseServerName `
                                    -SqlAdministratorCredentials $sqlLoginCredentials `
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
    Write-Host "`nCreating SQL Database, $sqlDatabaseName ..."  -ForegroundColor Green
    
    try {
        Get-AzureRmSqlDatabase `
            -ResourceGroupName $resourceGroupName `
            -ServerName $sqlDatabaseServerName `
            -DatabaseName $sqlDatabaseName
    }
    catch {
        New-AzureRMSqlDatabase `
            -ResourceGroupName $resourceGroupName `
            -ServerName $sqlDatabaseServerName `
            -DatabaseName $sqlDatabaseName `
            -Edition "Standard" `
            -RequestedServiceObjectiveName "S1"
    }
    #endregion
    
    #region - Create SQL database tables
    Write-Host "Creating the log4jlogs table  ..." -ForegroundColor Green
    
    $sqlDatabaseTableName = "log4jLogsCount"
    $cmdCreateLog4jCountTable = " CREATE TABLE [dbo].[$sqlDatabaseTableName](
            [Level] [nvarchar](10) NOT NULL,
            [Total] float,
        CONSTRAINT [PK_$sqlDatabaseTableName] PRIMARY KEY CLUSTERED
        (
        [Level] ASC
        )
        )"
    
    $conn = New-Object System.Data.SqlClient.SqlConnection
    $conn.ConnectionString = $sqlDatabaseConnectionString
    $conn.Open()
    
    # Create the log4jlogs table and index
    $cmd = New-Object System.Data.SqlClient.SqlCommand
    $cmd.Connection = $conn
    $cmd.CommandText = $cmdCreateLog4jCountTable
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
    
    #region - copy Oozie workflow and HiveQL files
    
    Write-Host "Copy workflow definition and HiveQL script file ..." -ForegroundColor Green
    
    # Both files are stored in a public Blob
    $publicBlobContext = New-AzureStorageContext -StorageAccountName "hditutorialdata" -Anonymous
    
    # WASB folder for storing the Oozie tutorial files.
    $destFolder = "tutorials/useoozie"  # Do NOT use the long path here
    
    Start-CopyAzureStorageBlob `
        -Context $publicBlobContext `
        -SrcContainer "useoozie" `
        -SrcBlob "useooziewf.hql"  `
        -DestContext $defaultStorageAccountContext `
        -DestContainer $defaultBlobContainerName `
        -DestBlob "$destFolder/useooziewf.hql" `
        -Force
    
    Start-CopyAzureStorageBlob `
        -Context $publicBlobContext `
        -SrcContainer "useoozie" `
        -SrcBlob "workflow.xml"  `
        -DestContext $defaultStorageAccountContext `
        -DestContainer $defaultBlobContainerName `
        -DestBlob "$destFolder/workflow.xml" `
        -Force
    
    #validate the copy
    Get-AzureStorageBlob `
        -Context $defaultStorageAccountContext `
        -Container $defaultBlobContainerName `
        -Blob $destFolder/workflow.xml
    
    Get-AzureStorageBlob `
        -Context $defaultStorageAccountContext `
        -Container $defaultBlobContainerName `
        -Blob $destFolder/useooziewf.hql
    
    #endregion
    
    #region - copy the sample.log file
    
    Write-Host "Make a copy of the sample.log file ... " -ForegroundColor Green
    
    Start-CopyAzureStorageBlob `
        -Context $defaultStorageAccountContext `
        -SrcContainer $defaultBlobContainerName `
        -SrcBlob "example/data/sample.log"  `
        -DestContext $defaultStorageAccountContext `
        -DestContainer $defaultBlobContainerName `
        -destBlob "$destFolder/data/sample.log" 
    
    #validate the copy
    Get-AzureStorageBlob `
        -Context $defaultStorageAccountContext `
        -Container $defaultBlobContainerName `
        -Blob $destFolder/data/sample.log
    
    #endregion
    
    #region - submit Oozie job
    
    $storageUri="wasbs://$defaultBlobContainerName@$defaultStorageAccountName.blob.core.windows.net"
    
    $oozieJobName = $namePrefix + "OozieJob"
    
    #Oozie WF variables
    $oozieWFPath="$storageUri/tutorials/useoozie"  # The default name is workflow.xml. And you don't need to specify the file name.
    $waitTimeBetweenOozieJobStatusCheck=10
    
    #Hive action variables
    $hiveScript = "$storageUri/tutorials/useoozie/useooziewf.hql"
    $hiveTableName = "log4jlogs"
    $hiveDataFolder = "$storageUri/tutorials/useoozie/data"
    $hiveOutputFolder = "$storageUri/tutorials/useoozie/output"
    
    #Sqoop action variables
    $sqlDatabaseConnectionString = "jdbc:sqlserver://$sqlDatabaseServerName.database.windows.net;user=$sqlDatabaseLogin@$sqlDatabaseServerName;password=$sqlDatabasePassword;database=$sqlDatabaseName"
    
    $OoziePayload =  @"
    <?xml version="1.0" encoding="UTF-8"?>
    <configuration>
    
    <property>
        <name>nameNode</name>
        <value>$storageUrI</value>
    </property>
    
    <property>
        <name>jobTracker</name>
        <value>jobtrackerhost:9010</value>
    </property>
    
    <property>
        <name>queueName</name>
        <value>default</value>
    </property>
    
    <property>
        <name>oozie.use.system.libpath</name>
        <value>true</value>
    </property>
    
    <property>
        <name>hiveScript</name>
        <value>$hiveScript</value>
    </property>
    
    <property>
        <name>hiveTableName</name>
        <value>$hiveTableName</value>
    </property>
    
    <property>
        <name>hiveDataFolder</name>
        <value>$hiveDataFolder</value>
    </property>
    
    <property>
        <name>hiveOutputFolder</name>
        <value>$hiveOutputFolder</value>
    </property>
    
    <property>
        <name>sqlDatabaseConnectionString</name>
        <value>&quot;$sqlDatabaseConnectionString&quot;</value>
    </property>
    
    <property>
        <name>sqlDatabaseTableName</name>
        <value>$SQLDatabaseTableName</value>
    </property>
    
    <property>
        <name>user.name</name>
        <value>$httpUserName</value>
    </property>
    
    <property>
        <name>oozie.wf.application.path</name>
        <value>$oozieWFPath</value>
    </property>
    
    </configuration>
    "@
    
    Write-Host "Checking Oozie server status..." -ForegroundColor Green
    $clusterUriStatus = "https://$hdinsightClusterName.azurehdinsight.net:443/oozie/v2/admin/status"
    $response = Invoke-RestMethod -Method Get -Uri $clusterUriStatus -Credential $httpCredential -OutVariable $OozieServerStatus
    
    $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
    $oozieServerSatus = $jsonResponse[0].("systemMode")
    Write-Host "Oozie server status is $oozieServerSatus."
    
    # create Oozie job
    Write-Host "Sending the following Payload to the cluster:" -ForegroundColor Green
    Write-Host "`n--------`n$OoziePayload`n--------"
    $clusterUriCreateJob = "https://$hdinsightClusterName.azurehdinsight.net:443/oozie/v2/jobs"
    $response = Invoke-RestMethod -Method Post -Uri $clusterUriCreateJob -Credential $httpCredential -Body $OoziePayload -ContentType "application/xml" -OutVariable $OozieJobName #-debug
    
    $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
    $oozieJobId = $jsonResponse[0].("id")
    Write-Host "Oozie job id is $oozieJobId..."
    
    # start Oozie job
    Write-Host "Starting the Oozie job $oozieJobId..." -ForegroundColor Green
    $clusterUriStartJob = "https://$hdinsightClusterName.azurehdinsight.net:443/oozie/v2/job/" + $oozieJobId + "?action=start"
    $response = Invoke-RestMethod -Method Put -Uri $clusterUriStartJob -Credential $httpCredential | Format-Table -HideTableHeaders #-debug
    
    # get job status
    Write-Host "Sleeping for $waitTimeBetweenOozieJobStatusCheck seconds until the job metadata is populated in the Oozie metastore..." -ForegroundColor Green
    Start-Sleep -Seconds $waitTimeBetweenOozieJobStatusCheck
    
    Write-Host "Getting job status and waiting for the job to complete..." -ForegroundColor Green
    $clusterUriGetJobStatus = "https://$hdinsightClusterName.azurehdinsight.net:443/oozie/v2/job/" + $oozieJobId + "?show=info"
    $response = Invoke-RestMethod -Method Get -Uri $clusterUriGetJobStatus -Credential $httpCredential
    $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
    $JobStatus = $jsonResponse[0].("status")
    
    while($JobStatus -notmatch "SUCCEEDED|KILLED")
    {
        Write-Host "$(Get-Date -format 'G'): $oozieJobId is in $JobStatus state...waiting $waitTimeBetweenOozieJobStatusCheck seconds for the job to complete..."
        Start-Sleep -Seconds $waitTimeBetweenOozieJobStatusCheck
        $response = Invoke-RestMethod -Method Get -Uri $clusterUriGetJobStatus -Credential $httpCredential
        $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
        $JobStatus = $jsonResponse[0].("status")
        $jobStatus
    }
    
    Write-Host "$(Get-Date -format 'G'): $oozieJobId is in $JobStatus state!" -ForegroundColor Green
    
    #endregion


**Az oktatóprogram ismételt futtatása**

Futtassa újra a munkafolyamatot, törölnie kell a következőket:

- A struktúra kimeneti parancsprogram
- A log4jLogsCount szereplő adatokat

Íme egy példa egy PowerShell-parancsprogramot, amelyek segítségével használhatja:

    $resourceGroupName = "<AzureResourceGroupName>"
    
    $defaultStorageAccountName = "<AzureStorageAccountName>"
    $defaultBlobContainerName = "<ContainerName>"

    #SQL database variables
    $sqlDatabaseServerName = "<SQLDatabaseServerName>"
    $sqlDatabaseLogin = "<SQLDatabaseLoginName>"
    $sqlDatabasePassword = "<SQLDatabaseLoginPassword>"
    $sqlDatabaseName = "<SQLDatabaseName>"
    $sqlDatabaseTableName = "log4jLogsCount"

    Write-host "Delete the Hive script output file ..." -ForegroundColor Green
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey `
                                -ResourceGroupName $resourceGroupName `
                                -Name $defaultStorageAccountName)[0].Value
    $destContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccountName -StorageAccountKey $defaultStorageAccountKey
    Remove-AzureStorageBlob -Context $destContext -Blob "tutorials/useoozie/output/000000_0" -Container $defaultBlobContainerName

    Write-host "Delete all the records from the log4jLogsCount table ..." -ForegroundColor Green
    $conn = New-Object System.Data.SqlClient.SqlConnection
    $conn.ConnectionString = "Data Source=$sqlDatabaseServerName.database.windows.net;Initial Catalog=$sqlDatabaseName;User ID=$sqlDatabaseLogin;Password=$sqlDatabasePassword;Encrypt=true;Trusted_Connection=false;"
    $conn.open()
    $cmd = New-Object System.Data.SqlClient.SqlCommand
    $cmd.connection = $conn
    $cmd.commandtext = "delete from $sqlDatabaseTableName"
    $cmd.executenonquery()

    $conn.close()

##<a name="next-steps"></a>Következő lépések
Ebben az oktatóanyagban szüksége a tanultakhoz megadása egy Oozie munkafolyamatot, és hogyan tehető függővé egy Oozie feladat PowerShell használatával. További tudnivalókért lásd: az alábbi cikkekben:

- [Időalapú Oozie koordinátor használata hdinsight szolgáltatáshoz][hdinsight-oozie-coordinator-time]
- [Elkezdeni használni Hadoop HDInsight struktúra mobil kézibeszélőt használati elemzéséhez][hdinsight-get-started]
- [Azure Blob-tárolóhoz használata hdinsight szolgáltatáshoz][hdinsight-storage]
- [A PowerShell használatával HDInsight felügyelete][hdinsight-admin-powershell]
- [Töltse fel az adatok HDInsight a Hadoop feladatokhoz][hdinsight-upload-data]
- [A HDInsight Hadoop Sqoop használata][hdinsight-use-sqoop]
- [A HDInsight Hadoop struktúra használata][hdinsight-use-hive]
- [A HDInsight Hadoop malac használata][hdinsight-use-pig]
- [Fejleszthet olyan Java MapReduce programok hdinsight szolgáltatáshoz][hdinsight-develop-mapreduce]


[hdinsight-cmdlets-download]: http://go.microsoft.com/fwlink/?LinkID=325563



[azure-data-factory-pig-hive]: ../data-factory/data-factory-data-transformation-activities.md
[hdinsight-oozie-coordinator-time]: hdinsight-use-oozie-coordinator-time.md
[hdinsight-versions]:  hdinsight-component-versioning.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md


[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[hdinsight-develop-mapreduce]: hdinsight-develop-deploy-java-mapreduce-linux.md

[sqldatabase-create-configue]: ../sql-database-create-configure.md
[sqldatabase-get-started]: ../sql-database-get-started.md

[azure-management-portal]: https://portal.azure.com/
[azure-create-storageaccount]: ../storage-create-storage-account.md

[apache-hadoop]: http://hadoop.apache.org/
[apache-oozie-400]: http://oozie.apache.org/docs/4.0.0/
[apache-oozie-332]: http://oozie.apache.org/docs/3.3.2/

[powershell-download]: http://azure.microsoft.com/downloads/
[powershell-about-profiles]: http://go.microsoft.com/fwlink/?LinkID=113729
[powershell-install-configure]: ../powershell-install-configure.md
[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-script]: https://technet.microsoft.com/en-us/library/ee176961.aspx

[cindygross-hive-tables]: http://blogs.msdn.com/b/cindygross/archive/2013/02/06/hdinsight-hive-internal-and-external-tables-intro.aspx

[img-workflow-diagram]: ./media/hdinsight-use-oozie/HDI.UseOozie.Workflow.Diagram.png
[img-preparation-output]: ./media/hdinsight-use-oozie/HDI.UseOozie.Preparation.Output1.png  
[img-runworkflow-output]: ./media/hdinsight-use-oozie/HDI.UseOozie.RunWF.Output.png

[technetwiki-hive-error]: http://social.technet.microsoft.com/wiki/contents/articles/23047.hdinsight-hive-error-unable-to-rename.aspx
