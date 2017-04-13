<properties
    pageTitle="A HDInsight Hadoop nézetbeli késleltetés adatok elemzése |} Microsoft Azure"
    description="Megtudhatja, hogy miként egy Windows PowerShell-parancsprogramot használatával hozzon létre egy HDInsight fürthöz, a struktúra feladat futtatása, egy Sqoop feladat futtatása és a csoport törlése."
    services="hdinsight"
    documentationCenter=""
    authors="mumian"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="jgao"/>

#<a name="analyze-flight-delay-data-by-using-hive-in-hdinsight"></a>Nézetbeli késleltetés adatok elemzése HDInsight struktúra használatával

Struktúra lehetővé teszi a kipróbált Hadoop MapReduce feladatok egy SQL-hasonló nevű parancsfájlok futtatásának nyelv * [HiveQL][hadoop-hiveql]*, felé engedélyeiről lekérdezése és elemzése a nagy mennyiségű adattal alkalmazhatók.

> [AZURE.NOTE] A dokumentumban a lépéseket a Windows-alapú HDInsight fürtre szükség. A Linux-alapú fürtre működő című témakör tartalmazza [elemzés nézetbeli késedelem az adatok HDInsight (Linux) struktúra használatával](hdinsight-analyze-flight-delay-data-linux.md).

Azure hdinsight szolgáltatáshoz fő előnyeiről egyik adattárolás és a számítási szétválasztása. Adattárolás Azure Blob-tárolóhoz HDInsight használja. Egy tipikus feladat három részből foglalja magában:

1. **Azure Blob-tárolóban lévő tárolja az adatokat.**  Például az adatok, szenzoradatokat, webes naplók időjárás, és ebben az esetben nézetbeli késési adataival menti a program az Azure Blob-tárolóhoz.
2. **Futtassa a feladatokat.** Az adatokat, a Windows PowerShell-parancsprogramot (vagy egy ügyfélalkalmazás) hozhat létre egy HDInsight fürthöz futtatása feldolgozása időpontjában feladatok futtatása és a fürt törlése. A feladatok Azure Blob-tárolóhoz kimeneti adatok mentése. Kimeneti adatai törlődik a fürt után is megmarad. Ezzel a módszerrel kifizeti csak mi meg van elfogyasztott mennyiség megjelenítésére.
3. **Azure Blob-tárolóhoz kimenetét beolvasásához**, és ebben az oktatóanyagban exportálja az adatokat egy Azure SQL-adatbázist.

Az alábbi ábra bemutatja az alkalmazási példát, és ebben az oktatóanyagban felépítésének:

![HDI. FlightDelays.flow][img-hdi-flightdelays-flow]

**Megjegyzés**: A számokat, a diagram felelnek meg a szakaszok címét. A fő folyamat **M** jelöli. **A** rövidítése függelékében tartalmát.

Az oktatóprogram fő része megtudhatja, hogy miként használata egy Windows PowerShell-parancsprogramot, végezze el az alábbi műveleteket:

- Hozzon létre egy HDInsight fürthöz.
- A repülőterek átlagos késéseit tartalmazó kiszámításához fürt struktúra feladat futtatása A nézetbeli késési adataival Azure Blob-tároló fiók vannak tárolva.
- Feladat futtatása olyan Sqoop szövegfájlnak struktúra feladat Azure SQL-adatbázishoz.
- A HDInsight fürt törléséhez.

A függelékek megtalálhatja a nézetbeli késési adataival feltöltése, létrehozása és feltöltése a struktúra lekérdezési karakterláncban és az Azure SQL-adatbázis előkészítése a Sqoop feladat a képernyőn megjelenő utasításokat.

> [AZURE.NOTE] A dokumentumban a lépéseket a Windows-alapú HDInsight fürt vonatkoznak. A Linux-alapú fürtre működő című témakör tartalmazza [elemzés nézetbeli késési adataival struktúra HDInsight (Linux) használata](hdinsight-analyze-flight-delay-data-linux.md)

###<a name="prerequisites"></a>Előfeltételek

Ebben az oktatóanyagban megkezdése előtt az alábbiakat kell rendelkeznie:

- **Az Azure-előfizetés**. Lásd: [Ismerkedés az Azure ingyenes próbaverziót](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- **Azure PowerShell munkaállomás**.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

**Ebben az oktatóanyagban használható fájlok**

Ebben az oktatóanyagban használja a [légitársaságok nézetbeli kutatási és innovatív technológia felügyeleti, pihenőhelyek statisztikai hivatal vagy]adatait RITA időben való ellátása [rita-website]. Az adatok egy példányát a nyilvános Blob-hozzáférési engedély-Azure Blob-tároló tárolóban feltöltötte. A PowerShell-parancsprogramot része a nyilvános blob-tárolóból az adatokat másolja az alapértelmezett blob-tárolóhoz a fürt. A HiveQL parancsfájl ugyanabban a Blob-tárolóban is másolja. Ha szeretné az útmutató: a get/feltöltés saját tárterület-fiókját az adatokat, és a HiveQL parancsfájl létrehozása, feltöltés, olvassa el a [mintája](#appendix-a) és a [B függelékében](#appendix-b).

Az alábbi táblázat az ebben az oktatóanyagban használt fájlok:

<table border="1">
<tr><th>Fájlok</th><th>Leírás</th></tr>
<tr><td>wasbs://flightdelay@hditutorialdata.blob.core.windows.net/flightdelays.hql</td><td>A feldolgozás által használt struktúra HiveQL parancsprogram. Ez a parancsfájl-Azure Blob-tároló fiókhoz a nyilvános hozzáféréssel rendelkező feltöltötte. <a href="#appendix-b">A B függelék</a> előkészítése és a fájl feltöltése a saját Azure Blob-tároló fiókjába útmutatást tartalmaz.</td></tr>
<tr><td>wasbs://flightdelay@hditutorialdata.blob.core.windows.net/2013Data</td><td>A bemeneti adatok a struktúra projektre vonatkozóan. Az adatok-Azure Blob-tároló fiókhoz a nyilvános hozzáféréssel rendelkező feltöltötte. <a href="#appendix-a">Függelék</a> első az adatokat, és az adatok feltöltése saját Azure Blob-tároló fiókját útmutatást tartalmaz.</td></tr>
<tr><td>\tutorials\flightdelays\output</td><td>A kimenet elérési útját a struktúra feladatot. Az alapértelmezett tároló a kimeneti adatok tárolására szolgál.</td></tr>
<tr><td>\tutorials\flightdelays\jobstatus</td><td>A struktúra feladat állapotának mappába a alapértelmezett tároló.</td></tr>
</table>


##<a name="create-cluster-and-run-hivesqoop-jobs"></a>Csoport létrehozása és futtatása a struktúra/Sqoop feladatok

Hadoop MapReduce parancsfájl. A legköltséghatékonyabb úgy a struktúra feladat futtatása, a feladat fürt létrehozása és törlése a feladatot, a feladat befejezése után. A következő parancsfájl terjed ki a teljes folyamat. Egy HDInsight fürt létrehozása és futtatása a struktúra feladatok a további tudnivalókért lásd: [a HDInsight létrehozása Hadoop fürt] [ hdinsight-provision] , és [Használja a HDInsight struktúra][hdinsight-use-hive].

**Az Azure PowerShell a struktúra lekérdezések futtatása**

1. Hozzon létre Azure SQL-adatbázishoz, és a táblázat a Sqoop feladat kimeneti [útmutatásai](#appendix-c)utasításait követve.
3. Nyissa meg a Windows PowerShell ISE, és futtassa a következő:

        $subscriptionID = "<Azure Subscription ID>"
        $nameToken = "<Enter an Alias>" 
        
        ###########################################
        # You must configure the follwing variables
        # for an existing Azure SQL Database
        ###########################################
        $existingSqlDatabaseServerName = "<Azure SQL Database Server>"
        $existingSqlDatabaseLogin = "<Azure SQL Database Server Login>"
        $existingSqlDatabasePassword = "<Azure SQL Database Server login password>"
        $existingSqlDatabaseName = "<Azure SQL Database name>"
        
        $localFolder = "E:\Tutorials\Downloads\" # A temp location for copying files. 
        $azcopyPath = "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy" # depends on the version, the folder can be different
        
        ###########################################
        # (Optional) configure the following variables
        ###########################################
        
        $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")

        $resourceGroupName = $namePrefix + "rg"
        $location = "EAST US 2"
        
        $HDInsightClusterName = $namePrefix + "hdi"
        $httpUserName = "admin"
        $httpPassword = "<Enter the Password>"
        
        $defaultStorageAccountName = $namePrefix + "store"
        $defaultBlobContainerName = $HDInsightClusterName # use the cluster name
        
        $existingSqlDatabaseTableName = "AvgDelays"
        $sqlDatabaseConnectionString = "jdbc:sqlserver://$existingSqlDatabaseServerName.database.windows.net;user=$existingSqlDatabaseLogin@$existingSqlDatabaseServerName;password=$existingSqlDatabaseLogin;database=$existingSqlDatabaseName"
        
        $hqlScriptFile = "/tutorials/flightdelays/flightdelays.hql" 
        
        $jobStatusFolder = "/tutorials/flightdelays/jobstatus"
        
        ###########################################
        # Login
        ###########################################
        try{
            $acct = Get-AzureRmSubscription
        }
        catch{
            Login-AzureRmAccount
        }
        Select-AzureRmSubscription -SubscriptionID $subscriptionID
        
        ###########################################
        # Create a new HDInsight cluster
        ###########################################
        
        # Create ARM group
        New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
        
        # Create the default storage account
        New-AzureRmStorageAccount -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName -Location $location -Type Standard_LRS
        
        # Create the default Blob container
        $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
        $defaultStorageAccountContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccountName -StorageAccountKey $defaultStorageAccountKey 
        New-AzureStorageContainer -Name $defaultBlobContainerName -Context $defaultStorageAccountContext 
        
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
            -DefaultStorageContainer $existingDefaultBlobContainerName 
        
        
        ###########################################
        # Prepare the HiveQL script and source data
        ###########################################
        
        # Create the temp location  
        New-Item -Path $localFolder -ItemType Directory -Force 
        
        # Download the sample file from Azure Blob storage
        $context = New-AzureStorageContext -StorageAccountName "hditutorialdata" -Anonymous
        $blobs = Get-AzureStorageBlob -Container "flightdelay" -Context $context
        #$blobs | Get-AzureStorageBlobContent -Context $context -Destination $localFolder
        
        # Upload data to default container
        
        $azcopycmd = "cmd.exe /C '$azcopyPath\azcopy.exe' /S /Source:'$localFolder' /Dest:'https://$defaultStorageAccountName.blob.core.windows.net/$defaultBlobContainerName/tutorials/flightdelays' /DestKey:$defaultStorageAccountKey"
        
        Invoke-Expression -Command:$azcopycmd
        
        ###########################################
        # Submit the Hive job
        ###########################################
        Use-AzureRmHDInsightCluster -ClusterName $HDInsightClusterName -HttpCredential $httpCredential
        $response = Invoke-AzureRmHDInsightHiveJob `
                        -Files $hqlScriptFile `
                        -DefaultContainer $defaultBlobContainerName `
                        -DefaultStorageAccountName $defaultStorageAccountName `
                        -DefaultStorageAccountKey $defaultStorageAccountKey `
                        -StatusFolder $jobStatusFolder 
        
        write-Host $response
        
        
        ###########################################
        # Submit the Sqoop job
        ###########################################
        $exportDir = "wasbs://$defaultBlobContainerName@$defaultStorageAccountName.blob.core.windows.net/tutorials/flightdelays/output"
        
        $sqoopDef = New-AzureRmHDInsightSqoopJobDefinition `
                        -Command "export --connect $sqlDatabaseConnectionString --table $sqlDatabaseTableName --export-dir $exportDir --fields-terminated-by \001 "
        $sqoopJob = Start-AzureRmHDInsightJob `
                        -ResourceGroupName $resourceGroupName `
                        -ClusterName $hdinsightClusterName `
                        -HttpCredential $httpCredential `
                        -JobDefinition $sqoopDef #-Debug -Verbose
        
        Wait-AzureRmHDInsightJob `
            -ResourceGroupName $resourceGroupName `
            -ClusterName $HDInsightClusterName `
            -HttpCredential $httpCredential `
            -WaitTimeoutInSeconds 3600 `
            -Job $sqoopJob.JobId
        
        
        Get-AzureRmHDInsightJobOutput `
            -ResourceGroupName $resourceGroupName `
            -ClusterName $hdinsightClusterName `
            -HttpCredential $httpCredential `
            -DefaultContainer $existingDefaultBlobContainerName `
            -DefaultStorageAccountName $defaultStorageAccountName `
            -DefaultStorageAccountKey $defaultStorageAccountKey `
            -JobId $sqoopJob.JobId `
            -DisplayOutputType StandardError
        
        ###########################################
        # Delete the cluster
        ###########################################
        Remove-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $hdinsightClusterName

6. Az SQL-adatbázis csatlakoztatása a termékkulcsról átlagos nézetbeli késések az AvgDelays táblázat település szerint:

    ![HDI. FlightDelays.AvgDelays.Dataset][image-hdi-flightdelays-avgdelays-dataset]



---
##<a id="appendix-a"></a>Mintája - feltöltés nézetbeli késési adataival Azure Blob-tárolóhoz
Az adatfájl és az HiveQL parancsfájlok (lásd: [A B függelék](#appendix-b)) feltöltése csak néhány tervezési. A funkció lényege az adatfájlok és -HDInsight fürt létrehozása és a struktúra feladat futtatása előtt a HiveQL fájlt tárolja. Két lehetőség közül választhat:

- **Használja a azonos, alapértelmezett fájlrendszer a HDInsight fürt által használt Azure tároló fiókot.** A HDInsight fürt a tárterület-fiók hívóbetű lesz, mert nem kell további módosításokat.
- **A fájlrendszerből HDInsight fürt alapértelmezett Azure tároló másik fiókkal használja.** Ha ez így, módosítania kell a Windows PowerShell-parancsprogramot [fürt HDInsight létrehozása és futtatása struktúra/Sqoop feladatok](#runjob) kapcsolja a tárterület-fiókot, további tárterület fiókként található létrehozásának része. Című cikkben olvashat [a HDInsight létrehozása Hadoop fürt][hdinsight-provision]. A HDInsight fürt majd tudja, hogy a hívóbetű a tárterület-fiókom.

>[AZURE.NOTE] Az adatfájl Blob tároló elérési nehéz a HiveQL parancsprogram kódolni. Ennek megfelelően kell frissítenie.

**Nézetbeli adatok letöltéséhez**

1. Tallózással keresse meg azt a [kutatási és innovatív technológiák felügyeleti, pihenőhelyek statisztikai hivatala][rita-website].
2. A lapon jelölje be az alábbi értékeket:

    <table border="1">
    <tr><th>név</th><th>Érték</th></tr>
    <tr><td>Szűrő év</td><td>2013-ban </td></tr>
    <tr><td>Szűrés időszak</td><td>Január</td></tr>
    <tr><td>Mezők</td><td>*Év*, *FlightDate*, *UniqueCarrier*, *Carrier*, *FlightNum*, *OriginAirportID*, *Origin*, *OriginCityName*, *OriginState*, *DestAirportID*, *cél*, *DestCityName*, *DestState*, *DepDelayMinutes*, *ArrDelay*, *ArrDelayMinutes*, *CarrierDelay*, *WeatherDelay*, *NASDelay*, *SecurityDelay*, *LateAircraftDelay* (az összes többi mező törlése)</td></tr>
    </table>

3. Kattintson a **Letöltés**gombra.
4. Bontsa ki a fájlt a **C:\Tutorials\FlightDelay\2013Data** mappába. Minden fájl CSV-fájlba és körülbelül 60 GB méretű.
5.  Nevezze át a fájlt a rajta lévő adatokat a hónap neve. Például a január adatokat tartalmazó fájlt szeretne neve *January.csv*.
6. Ismételje meg a 2 és 5 töltheti le egy fájlt a 12 hónapon a 2013-ban. Az oktatóprogram futtatásához egy fájl legalább szüksége lesz.  

**Töltse fel a nézetbeli késési adataival Azure Blob-tárolóhoz**

1. Felkészülés a Paraméterek:

    <table border="1">
    <tr><th>Változó neve</th><th>Jegyzetek</th></tr>
    <tr><td>$storageAccountName</td><td>Az Azure tároló fiók, amelyre az adatok feltöltése.</td></tr>
    <tr><td>$blobContainerName</td><td>A Blob-tároló, amelyhez feltölteni az adatokat.</td></tr>
    </table>
2. Nyissa meg az Azure PowerShell ISE.
3. A következő parancsprogram beillesztése a parancsprogram-ablakban:

        [CmdletBinding()]
        Param(

            [Parameter(Mandatory=$True,
                       HelpMessage="Enter the Azure storage account name for creating a new HDInsight cluster. If the account doesn't exist, the script will create one.")]
            [String]$storageAccountName,

            [Parameter(Mandatory=$True,
                       HelpMessage="Enter the Azure blob container name for creating a new HDInsight cluster. If not specified, the HDInsight cluster name will be used.")]
            [String]$blobContainerName
        )

        #Region - Variables
        $localFolder = "C:\Tutorials\FlightDelay\2013Data"  # The source folder
        $destFolder = "tutorials/flightdelay/2013data"     #The blob name prefix for the files to be uploaded
        #EndRegion
        
        #Region - Connect to Azure subscription
        Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
        try{Get-AzureRmContext}
        catch{Login-AzureRmAccount}
        #EndRegion
        
        #Region - Validate user input
        Write-Host "`nValidating the Azure Storage account and the Blob container..." -ForegroundColor Green
        # Validate the Storage account
        if (-not (Get-AzureRmStorageAccount|Where-Object{$_.StorageAccountName -eq $storageAccountName}))
        {
            Write-Host "The storage account, $storageAccountName, doesn't exist." -ForegroundColor Red
            exit
        }
        else{
            $resourceGroupName = (Get-AzureRmStorageAccount|Where-Object{$_.StorageAccountName -eq $storageAccountName}).ResourceGroupName
        }
        
        # Validate the container
        $storageAccountKey = (Get-AzureRmStorageAccountKey -StorageAccountName $storageAccountName -ResourceGroupName $resourceGroupName)[0].Value
        $storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
        
        if (-not (Get-AzureStorageContainer -Context $storageContext |Where-Object{$_.Name -eq $blobContainerName}))
        {
            Write-Host "The Blob container, $blobContainerName, doesn't exist" -ForegroundColor Red
            Exit
        }
        #EngRegion
        
        #Region - Copy the file from local workstation to Azure Blob storage  
        if (test-path -Path $localFolder)
        {
            foreach ($item in Get-ChildItem -Path $localFolder){
                $fileName = "$localFolder\$item"
                $blobName = "$destFolder/$item"
        
                Write-Host "Copying $fileName to $blobName" -ForegroundColor Green
        
                Set-AzureStorageBlobContent -File $fileName -Container $blobContainerName -Blob $blobName -Context $storageContext
            }
        }
        else
        {
            Write-Host "The source folder on the workstation doesn't exist" -ForegroundColor Red
        }
        
        # List the uploaded files on HDInsight
        Get-AzureStorageBlob -Container $blobContainerName  -Context $storageContext -Prefix $destFolder
        #EndRegion




4. Nyomja le az **F5** futtatása.

Ha úgy dönt, hogy egy másik módszert a fájlok feltöltéséről, ellenőrizze, hogy a fájl elérési útja oktatóanyagok/flightdelay/adatokat. A fájlok elérése szintaxisa:

    wasbs://<ContainerName>@<StorageAccountName>.blob.core.windows.net/tutorials/flightdelay/data

Elérési út oktatóanyagok/flightdelay/adatai a virtuális mappa, akkor jön létre, ha a feltöltött fájlokat. Győződjön meg arról, hogy nincsenek-e egy, az egyes 12 fájlokat.

>[AZURE.NOTE] Frissítenie kell a struktúra lekérdezés olvashatja őket az új helyre.

> A nyilvános vagy kötést létrehozni a tárterület-fiókot a HDInsight fürt container hozzáférési engedélyt vagy meg kell adnia. Egyéb esetben a struktúra lekérdezési karakterlánc nem tudnak a fájljai eléréséhez.

---
##<a id="appendix-b"></a>"B" függelék - létrehozása, és töltse fel a HiveQL parancsfájl

Azure PowerShell használata esetén futtassa a több HiveQL kimutatások egy egyszerre, vagy a HiveQL utasítás parancsprogram-fájlba csomag. Ez a szakasz bemutatja, hogyan hozhat létre egy HiveQL parancsfájlt, és töltse fel a parancsfájl Azure Blob-tárolóhoz Azure PowerShell használatával. Struktúra szükséges a HiveQL parancsfájlok Azure Blob-tárolóban lévő tárolja.

A HiveQL parancsfájl hajtsa végre a következő lesz:

1. **A delays_raw tábla eldobása**, abban az esetben a táblázat már létezik.
2. **A külső delays_raw struktúratáblával létrehozása** nézetbeli késleltetés fájlokkal Blob tárolási helyére mutat. Ezt a lekérdezést, adja meg, hogy a mezők szerint vannak tagolva "," és az, hogy vonalak megszakítja "\n". A problémát jelent, mezőértékek vessző tartalmaz, mivel a struktúra, amely a mező elválasztó vesszővel és egy része egy mező értéke nem különbözteti akkor (amely a helyzet az ORIGIN mező értékének\_VÁROS\_nevét, és a cél\_VÁROS\_neve). Ez az a cím, a lekérdezés TEMP oszlopokat, hogy a rendszer helytelen oszlopokba történő felosztás adatok tárolására hoz létre.  
3. **A késések tábla eldobása**, abban az esetben a táblázat már létezik.
4. **A késések tábla létrehozása**. Célszerű az adatok karbantartása feldolgozásra előtt. A lekérdezés új táblát hoz létre, *késések*, a delays_raw táblából. Ne feledje, hogy a TEMP oszlopokba (korábban említett) nem kerülnek, szolgál, hogy a **karakterlánc** függvény idézőjelek közé eltávolítása az adatokat.
5. **Az átlag időjárási késleltetés és a csoportok az eredmények kiszámítására város neve.** Ezt fogja kimeneti Blob-tárolóhoz az eredmények is. Ne feledje, hogy a lekérdezés távolítsa el az aposztrófot az adatok kizárja a sorok, ahol az **weather_delay** értéke null. Ennek oka szükséges Sqoop, ebben az oktatóanyagban használatban nem kezeli ezeket az értékeket biztonságosan alapértelmezés szerint.

A HiveQL parancsok teljes listáját, lásd: az [Adatok Definition Language struktúra][hadoop-hiveql]. Minden egyes HiveQL parancs pontosvesszővel kell befejezéséhez.

**HiveQL script fájl létrehozása**

1. Felkészülés a Paraméterek:

    <table border="1">
    <tr><th>Változó neve</th><th>Jegyzetek</th></tr>
    <tr><td>$storageAccountName</td><td>A Azure tároló fiókot, amelyhez a HiveQL parancsfájl feltöltése.</td></tr>
    <tr><td>$blobContainerName</td><td>A Blob-tároló, amelyre a HiveQL parancsfájl feltöltése.</td></tr>
    </table>
2. Nyissa meg az Azure PowerShell ISE.

3. Másolja a vágólapra, és a következő parancsprogram beillesztése a parancsprogram-ablakban:

        [CmdletBinding()]
        Param(

            # Azure Blob storage variables
            [Parameter(Mandatory=$True,
                       HelpMessage="Enter the Azure storage account name for creating a new HDInsight cluster. If the account doesn't exist, the script will create one.")]
            [String]$storageAccountName,

            [Parameter(Mandatory=$True,
                       HelpMessage="Enter the Azure blob container name for creating a new HDInsight cluster. If not specified, the HDInsight cluster name will be used.")]
            [String]$blobContainerName
        )

        #region - Define variables
        # Treat all errors as terminating
        $ErrorActionPreference = "Stop"
        
        # The HiveQL script file is exported as this file before it's uploaded to Blob storage
        $hqlLocalFileName = "e:\tutorials\flightdelay\flightdelays.hql"
        
        # The HiveQL script file will be uploaded to Blob storage as this blob name
        $hqlBlobName = "tutorials/flightdelay/flightdelays.hql"
        
        # These two constants are used by the HiveQL script file
        #$srcDataFolder = "tutorials/flightdelay/data"
        $dstDataFolder = "/tutorials/flightdelay/output"
        #endregion

        #Region - Connect to Azure subscription
        Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
        try{Get-AzureRmContext}
        catch{Login-AzureRmAccount}
        #EndRegion
        
        #Region - Validate user input
        Write-Host "`nValidating the Azure Storage account and the Blob container..." -ForegroundColor Green
        # Validate the Storage account
        if (-not (Get-AzureRmStorageAccount|Where-Object{$_.StorageAccountName -eq $storageAccountName}))
        {
            Write-Host "The storage account, $storageAccountName, doesn't exist." -ForegroundColor Red
            exit
        }
        else{
            $resourceGroupName = (Get-AzureRmStorageAccount|Where-Object{$_.StorageAccountName -eq $storageAccountName}).ResourceGroupName
        }
        
        # Validate the container
        $storageAccountKey = (Get-AzureRmStorageAccountKey -StorageAccountName $storageAccountName -ResourceGroupName $resourceGroupName)[0].Value
        $storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
        
        if (-not (Get-AzureStorageContainer -Context $storageContext |Where-Object{$_.Name -eq $blobContainerName}))
        {
            Write-Host "The Blob container, $blobContainerName, doesn't exist" -ForegroundColor Red
            Exit
        }
        #EngRegion
        
        #region - Validate the file and file path
        
        # Check if a file with the same file name already exists on the workstation
        Write-Host "`nvalidating the folder structure on the workstation for saving the HQL script file ..."  -ForegroundColor Green
        if (test-path $hqlLocalFileName){
        
            $isDelete = Read-Host 'The file, ' $hqlLocalFileName ', exists.  Do you want to overwirte it? (Y/N)'
        
            if ($isDelete.ToLower() -ne "y")
            {
                Exit
            }
        }
        
        # Create the folder if it doesn't exist
        $folder = split-path $hqlLocalFileName
        if (-not (test-path $folder))
        {
            Write-Host "`nCreating folder, $folder ..." -ForegroundColor Green
        
            new-item $folder -ItemType directory  
        }
        #end region
        
        #region - Write the Hive script into a local file
        Write-Host "`nWriting the Hive script into a file on your workstation ..." `
                    -ForegroundColor Green
        
        $hqlDropDelaysRaw = "DROP TABLE delays_raw;"
        
        $hqlCreateDelaysRaw = "CREATE EXTERNAL TABLE delays_raw (" +
                "YEAR string, " +
                "FL_DATE string, " +
                "UNIQUE_CARRIER string, " +
                "CARRIER string, " +
                "FL_NUM string, " +
                "ORIGIN_AIRPORT_ID string, " +
                "ORIGIN string, " +
                "ORIGIN_CITY_NAME string, " +
                "ORIGIN_CITY_NAME_TEMP string, " +
                "ORIGIN_STATE_ABR string, " +
                "DEST_AIRPORT_ID string, " +
                "DEST string, " +
                "DEST_CITY_NAME string, " +
                "DEST_CITY_NAME_TEMP string, " +
                "DEST_STATE_ABR string, " +
                "DEP_DELAY_NEW float, " +
                "ARR_DELAY_NEW float, " +
                "CARRIER_DELAY float, " +
                "WEATHER_DELAY float, " +
                "NAS_DELAY float, " +
                "SECURITY_DELAY float, " +
                "LATE_AIRCRAFT_DELAY float) " +
            "ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' " +
            "LINES TERMINATED BY '\n' " +
            "STORED AS TEXTFILE " +
            "LOCATION 'wasbs://flightdelay@hditutorialdata.blob.core.windows.net/2013Data';"
        
        $hqlDropDelays = "DROP TABLE delays;"
        
        $hqlCreateDelays = "CREATE TABLE delays AS " +
            "SELECT YEAR AS year, " +
                "FL_DATE AS flight_date, " +
                "substring(UNIQUE_CARRIER, 2, length(UNIQUE_CARRIER) -1) AS unique_carrier, " +
                "substring(CARRIER, 2, length(CARRIER) -1) AS carrier, " +
                "substring(FL_NUM, 2, length(FL_NUM) -1) AS flight_num, " +
                "ORIGIN_AIRPORT_ID AS origin_airport_id, " +
                "substring(ORIGIN, 2, length(ORIGIN) -1) AS origin_airport_code, " +
                "substring(ORIGIN_CITY_NAME, 2) AS origin_city_name, " +
                "substring(ORIGIN_STATE_ABR, 2, length(ORIGIN_STATE_ABR) -1)  AS origin_state_abr, " +
                "DEST_AIRPORT_ID AS dest_airport_id, " +
                "substring(DEST, 2, length(DEST) -1) AS dest_airport_code, " +
                "substring(DEST_CITY_NAME,2) AS dest_city_name, " +
                "substring(DEST_STATE_ABR, 2, length(DEST_STATE_ABR) -1) AS dest_state_abr, " +
                "DEP_DELAY_NEW AS dep_delay_new, " +
                "ARR_DELAY_NEW AS arr_delay_new, " +
                "CARRIER_DELAY AS carrier_delay, " +
                "WEATHER_DELAY AS weather_delay, " +
                "NAS_DELAY AS nas_delay, " +
                "SECURITY_DELAY AS security_delay, " +
                "LATE_AIRCRAFT_DELAY AS late_aircraft_delay " +
            "FROM delays_raw;"
        
        $hqlInsertLocal = "INSERT OVERWRITE DIRECTORY '$dstDataFolder' " +
            "SELECT regexp_replace(origin_city_name, '''', ''), " +
                "avg(weather_delay) " +
            "FROM delays " +
            "WHERE weather_delay IS NOT NULL " +
            "GROUP BY origin_city_name;"
        
        $hqlScript = $hqlDropDelaysRaw + $hqlCreateDelaysRaw + $hqlDropDelays + $hqlCreateDelays + $hqlInsertLocal
        
        $hqlScript | Out-File $hqlLocalFileName -Encoding ascii -Force
        #endregion
        
        #region - Upload the Hive script to the default Blob container
        Write-Host "`nUploading the Hive script to the default Blob container ..." -ForegroundColor Green
        
        # Create a storage context object
        $storageAccountKey = (Get-AzureRmStorageAccountKey -StorageAccountName $storageAccountName -ResourceGroupName $resourceGroupName)[0].Value
        $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
        
        # Upload the file from local workstation to Blob storage
        Set-AzureStorageBlobContent -File $hqlLocalFileName -Container $blobContainerName -Blob $hqlBlobName -Context $destContext
        #endregion
        
        Write-host "`nEnd of the PowerShell script" -ForegroundColor Green

    Az alábbiakban a használt a parancsfájl változók:

    - **$hqlLocalFileName** - a parancsfájl az HiveQL script fájl mentése helyi meghajtóra előtt feltöltése a Blob-tárolóhoz. Ez a fájl nevét. Az alapértelmezett érték <u>C:\tutorials\flightdelay\flightdelays.hql</u>.
    - **$hqlBlobName** - a HiveQL parancsfájl blob nevét az Azure Blob-tárolóhoz használatban. Az alapértelmezett érték tutorials/flightdelay/flightdelays.hql. A fájl közvetlenül Azure Blob-tárolóhoz írt, mert nem szerepel a "/" blob nevét az elején. A fájl elérhető Blob-tárolóhoz szeretné, ha szüksége lesz egy "/" a fájl nevét az elején hozzáadni.
    - **$srcDataFolder** és **$dstDataFolder** - = "oktatóanyagok/flightdelay /-adatok" = "oktatóanyagok/flightdelay/kimeneti"


---
##<a id="appendix-c"></a>C - függelék előkészítése a Sqoop feladat eredménye Azure SQL-adatbázishoz
**Az SQL-adatbázis előkészítése (egyesítés ezt a Sqoop parancsfájl)**

1. Felkészülés a Paraméterek:

    <table border="1">
    <tr><th>Változó neve</th><th>Jegyzetek</th></tr>
    <tr><td>$sqlDatabaseServerName</td><td>Az Azure SQL adatbázis-kiszolgáló neve. Adja meg, hozzon létre egy új kiszolgálót el.</td></tr>
    <tr><td>$sqlDatabaseUsername</td><td>Az Azure SQL-adatbázis kiszolgálója bejelentkezési nevét. Ha egy meglévő kiszolgáló $sqlDatabaseServerName, a bejelentkezési és a bejelentkezési jelszavát használatosak hitelesítést végezni a kiszolgálóval. Egyéb esetben szolgálnak hozzon létre egy új kiszolgálót.</td></tr>
    <tr><td>$sqlDatabasePassword</td><td>Az Azure SQL-adatbázis kiszolgálója bejelentkezési jelszavát.</td></tr>
    <tr><td>$sqlDatabaseLocation</td><td>Ezt az értéket a csak egy új Azure-adatbázis azon kiszolgálóját létrehozásakor használatos.</td></tr>
    <tr><td>$sqlDatabaseName</td><td>A AvgDelays tábla a Sqoop feladat létrehozásához használt SQL-adatbázishoz. Hagyja üresen HDISqoop nevű adatbázist hozza létre. A táblázat neve a Sqoop feladat kimeneti AvgDelays. </td></tr>
    </table>
2. Nyissa meg az Azure PowerShell ISE.
3. Másolja a vágólapra, és a következő parancsprogram beillesztése a parancsprogram-ablakban:

        [CmdletBinding()]
        Param(
        
            # Azure Resource group variables
            [Parameter(Mandatory=$True,
                    HelpMessage="Enter the Azure resource group name. It will be created if it doesn't exist.")]
            [String]$resourceGroupName,
        
            # SQL database server variables
            [Parameter(Mandatory=$True,
                    HelpMessage="Enter the Azure SQL Database Server Name. It will be created if it doesn't exist.")]
            [String]$sqlDatabaseServer, 
        
            [Parameter(Mandatory=$True,
                    HelpMessage="Enter the Azure SQL Database admin user.")]
            [String]$sqlDatabaseLogin,
        
            [Parameter(Mandatory=$True,
                    HelpMessage="Enter the Azure SQL Database admin user password.")]
            [String]$sqlDatabasePassword,
        
            [Parameter(Mandatory=$True,
                    HelpMessage="Enter the region to create the Database in.")]
            [String]$sqlDatabaseLocation,   #For example, West US.
        
            # SQL database variables
            [Parameter(Mandatory=$True,
                    HelpMessage="Enter the database name. It will be created if it doesn't exist.")]
            [String]$sqlDatabaseName # specify the database name if you have one created. Otherwise use "" to have the script create one for you.
        )
        
        # Treat all errors as terminating
        $ErrorActionPreference = "Stop"
        
        #region - Constants and variables
        
        # IP address REST service used for retrieving external IP address and creating firewall rules
        [String]$ipAddressRestService = "http://bot.whatismyipaddress.com"
        [String]$fireWallRuleName = "FlightDelay"
        
        # SQL database variables
        [String]$sqlDatabaseMaxSizeGB = 10
        
        #SQL query string for creating AvgDelays table
        [String]$sqlDatabaseTableName = "AvgDelays"
        [String]$sqlCreateAvgDelaysTable = " CREATE TABLE [dbo].[$sqlDatabaseTableName](
                    [origin_city_name] [nvarchar](50) NOT NULL,
                    [weather_delay] float,
                CONSTRAINT [PK_$sqlDatabaseTableName] PRIMARY KEY CLUSTERED
                (
                    [origin_city_name] ASC
                )
                )"
        #endregion
        
        #Region - Connect to Azure subscription
        Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
        try{Get-AzureRmContext}
        catch{Login-AzureRmAccount}
        #EndRegion
        
        #region - Create and validate Azure resouce group
        try{
            Get-AzureRmResourceGroup -Name $resourceGroupName
        }
        catch{
            New-AzureRmResourceGroup -Name $resourceGroupName -Location $sqlDatabaseLocation
        }
        
        #EndRegion
        
        #region - Create and validate Azure SQL database server
        try{
            Get-AzureRmSqlServer -ServerName $sqlDatabaseServer -ResourceGroupName $resourceGroupName}
        catch{
            Write-Host "`nCreating SQL Database server ..."  -ForegroundColor Green
        
            $sqlDatabasePW = ConvertTo-SecureString -String $sqlDatabasePassword -AsPlainText -Force
            $credential = New-Object System.Management.Automation.PSCredential($sqlDatabaseLogin,$sqlDatabasePW)
        
            $sqlDatabaseServer = (New-AzureRmSqlServer -ResourceGroupName $resourceGroupName -ServerName $sqlDatabaseServer -SqlAdministratorCredentials $credential -Location $sqlDatabaseLocation).ServerName
            Write-Host "`tThe new SQL database server name is $sqlDatabaseServer." -ForegroundColor Cyan
        
            Write-Host "`nCreating firewall rule, $fireWallRuleName ..." -ForegroundColor Green
            $workstationIPAddress = Invoke-RestMethod $ipAddressRestService
            New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourceGroupName -ServerName $sqlDatabaseServer -FirewallRuleName "$fireWallRuleName-workstation" -StartIpAddress $workstationIPAddress -EndIpAddress $workstationIPAddress
        
            #To allow other Azure services to access the server add a firewall rule and set both the StartIpAddress and EndIpAddress to 0.0.0.0. Note that this allows Azure traffic from any Azure subscription to access the server.
            New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourceGroupName -ServerName $sqlDatabaseServer -FirewallRuleName "$fireWallRuleName-Azureservices" -StartIpAddress "0.0.0.0" -EndIpAddress "0.0.0.0"
        }
        
        #endregion
        
        #region - Create and validate Azure SQL database
        
        try {
            Get-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $sqlDatabaseServer -DatabaseName $sqlDatabaseName
        }
        catch {
            Write-Host "`nCreating SQL Database, $sqlDatabaseName ..."  -ForegroundColor Green
            New-AzureRMSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $sqlDatabaseServer -DatabaseName $sqlDatabaseName -Edition "Standard" -RequestedServiceObjectiveName "S1"
        }
        
        #endregion
        
        #region -  Execute an SQL command to create the AvgDelays table
        
        Write-Host "`nCreating SQL Database table ..."  -ForegroundColor Green
        $conn = New-Object System.Data.SqlClient.SqlConnection
        $conn.ConnectionString = "Data Source=$sqlDatabaseServer.database.windows.net;Initial Catalog=$sqlDatabaseName;User ID=$sqlDatabaseLogin;Password=$sqlDatabasePassword;Encrypt=true;Trusted_Connection=false;"
        $conn.open()
        $cmd = New-Object System.Data.SqlClient.SqlCommand
        $cmd.connection = $conn
        $cmd.commandtext = $sqlCreateAvgDelaysTable
        $cmd.executenonquery()
        
        $conn.close()
        
        Write-host "`nEnd of the PowerShell script" -ForegroundColor Green

    >[AZURE.NOTE] A parancsprogram egy representational állam transfer (REST) szolgáltatást használja, http://bot.whatismyipaddress.com, a külső IP-cím beolvasásához. Az IP-címet a SQL-adatbázis kiszolgálója tartozó tűzfal szabály létrehozásához használható.  

    Íme néhány a parancsfájl használt változók:

    - **$ipAddressRestService** – az alapértelmezett érték http://bot.whatismyipaddress.com. Egy nyilvános IP-cím többi szolgáltatást ismertető a külső IP-cím. Más szolgáltatásokat is használhatja, ha azt szeretné. A külső IP-cím, a szolgáltatáson keresztül beolvasott használandó tűzfal szabály létrehozása az Azure SQL-adatbázis kiszolgálója, hogy akkor is hozzáférést az adatbázishoz a munkaállomásról (a Windows PowerShell-parancsprogramot) segítségével.
    - **$fireWallRuleName** – a tűzfal szabály az Azure SQL-adatbázis kiszolgálója a neve. Az alapértelmezett neve <u>FlightDelay</u>. Ha azt szeretné, átnevezheti.
    - **$sqlDatabaseMaxSizeGB** – Ez az érték a csak egy új Azure SQL-adatbáziskiszolgálóhoz létrehozásakor használatos. Az alapértelmezett érték 10 gigabájtos Korlátot. 10 GB-os is elegendő az ebben az oktatóanyagban.
    - **$sqlDatabaseName** – Ez az érték a csak egy új Azure SQL-adatbázis létrehozásakor használatos. Az alapértelmezett érték HDISqoop. Ha átnevez, frissítse a Sqoop a Windows PowerShell-parancsprogramot.

4. Nyomja le az **F5** futtatása.
5. A művelet eredményébe ellenőrzése. Győződjön meg arról, hogy a parancsprogram sikeresen futtatta.

##<a id="nextsteps"></a>Következő lépések
Most már ismeri fájl feltöltése Azure Blob-tárolóhoz, egy struktúratáblával kitöltéséhez Azure Blob-tárolóhoz adatainak használatával hogyan, hogyan struktúra lekérdezések futtatása és Sqoop használata Fájlrendszerhez adatainak exportálása Azure SQL-adatbázishoz. További tudnivalókért lásd: az alábbi cikkekben:

* [Első lépések – hdinsight szolgáltatáshoz][hdinsight-get-started]
* [HDInsight struktúra használata][hdinsight-use-hive]
* [HDInsight Oozie használata][hdinsight-use-oozie]
* [HDInsight Sqoop használata][hdinsight-use-sqoop]
* [Malac használata hdinsight szolgáltatáshoz][hdinsight-use-pig]
* [Fejleszthet olyan Java MapReduce programok hdinsight szolgáltatáshoz][hdinsight-develop-mapreduce]



[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/


[rita-website]: http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time
[powershell-install-configure]: powershell-install-configure.md

[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-develop-mapreduce]: hdinsight-develop-deploy-java-mapreduce-linux.md

[hadoop-hiveql]: https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL
[hadoop-shell-commands]: http://hadoop.apache.org/docs/r0.18.3/hdfs_shell.html

[technetwiki-hive-error]: http://social.technet.microsoft.com/wiki/contents/articles/23047.hdinsight-hive-error-unable-to-rename.aspx

[image-hdi-flightdelays-avgdelays-dataset]: ./media/hdinsight-analyze-flight-delay-data/HDI.FlightDelays.AvgDelays.DataSet.png
[img-hdi-flightdelays-run-hive-job-output]: ./media/hdinsight-analyze-flight-delay-data/HDI.FlightDelays.RunHiveJob.Output.png
[img-hdi-flightdelays-flow]: ./media/hdinsight-analyze-flight-delay-data/HDI.FlightDelays.Flow.png
