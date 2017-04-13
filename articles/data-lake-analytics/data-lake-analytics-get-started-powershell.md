<properties 
   pageTitle="Első lépések az Azure adatok tó Analytics Azure PowerShell használatával |} Azure" 
   description="Megtudhatja, hogy miként tó adattár-fiók létrehozása, U – SQL használatával adatok tó Analytics feladat létrehozása az Azure PowerShell használata, és a feladat elküldése. " 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="edmacauley" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="09/21/2016"
   ms.author="edmaca"/>

# <a name="tutorial-get-started-with-azure-data-lake-analytics-using-azure-powershell"></a>Oktatóprogram: első lépések a Azure adatok tó Analytics Azure PowerShell használatával

[AZURE.INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

Megtudhatja, hogy miként Azure adatok tó Analytics-fiókokat létrehozni, adatok tó Analytics feladatok meghatározása a [U-SQL nyelvben](data-lake-analytics-u-sql-get-started.md)és feladatok adatok tó analitikus fiókokhoz elküldése az Azure PowerShell használatával. További információt a adatok tó Analytics [Azure adatok tó Analytics áttekintése](data-lake-analytics-overview.md)című témakörben találhat.

Ebben az oktatóanyagban a feladat, amely beolvassa a lap elválasztott értékek (TSV) fájlt, és konvertálja a vesszővel elválasztott értékek (CSV) fájl fejleszthet olyan. Az azonos oktatóprogram más támogatott eszközökkel folyamatát, kattintson a lap tetején lévő ebben a szakaszban.

##<a name="prerequisites"></a>Előfeltételek

Ebben az oktatóanyagban megkezdése előtt a következőket kell rendelkeznie:

- **Az Azure-előfizetés**. Lásd: [Ismerkedés az Azure ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/).
- **Azure PowerShell munkaállomás**. [Telepítse és állítsa be a Azure PowerShell](../powershell-install-configure.md)tájékozódhat.
    
##<a name="create-data-lake-analytics-account"></a>Adatok tó Analytics-fiók létrehozása

Adatok tó Analytics-fiókkal kell rendelkeznie, bármilyen feladat futtatása előtt. Adatok tó Analytics-fiók létrehozása, adja meg a következőket:

- **Azure erőforráscsoport**: Azure erőforráscsoport belül A adatok tó Analytics-fiókot kell létrehozni. [Erőforrás-kezelő Azure](../azure-resource-manager/resource-group-overview.md) lehetővé teszi, hogy az erőforrások az alkalmazásban csoportosan kezelheti. Telepítése, frissítése, és egyetlen, összehangolt műveletben az alkalmazás törlése az összes erőforrás.  

    Az erőforrások számbavétele csoportosítja az előfizetésben:
    
        Get-AzureRmResourceGroup
    
    Új erőforráscsoport létrehozása:

        New-AzureRmResourceGroup `
            -Name "<Your resource group name>" `
            -Location "<Azure Data Center>" # For example, "East US 2"

- **Adatok tó Analytics fióknév**
- **Hely**: az Azure adatközpontokban, amely támogatja az adatok tó Analytics közül.
- **Alapértelmezett adatok tó fiók**: minden adat tó Analytics-fiók alapértelmezett adatok tó fiókkal rendelkezik.

    Új adatok tó fiók létrehozása:

        New-AzureRmDataLakeStoreAccount `
            -ResourceGroupName "<Your Azure resource group name>" `
            -Name "<Your Data Lake account name>" `
            -Location "<Azure Data Center>"  # For example, "East US 2"

    > [AZURE.NOTE] Az adatok tó fióknév csak kell tartalmaznia a kisbetűket és számokat.



**Adatok tó Analytics-fiók létrehozása**

1. Nyissa meg a PowerShell ISE a Windows munkaállomásról.
2. Futtassa a következő:

        $resourceGroupName = "<ResourceGroupName>"
        $dataLakeStoreName = "<DataLakeAccountName>"
        $dataLakeAnalyticsName = "<DataLakeAnalyticsAccountName>"
        $location = "East US 2"
        
        Write-Host "Create a resource group ..." -ForegroundColor Green
        New-AzureRmResourceGroup `
            -Name  $resourceGroupName `
            -Location $location
        
        Write-Host "Create a Data Lake account ..."  -ForegroundColor Green
        New-AzureRmDataLakeStoreAccount `
            -ResourceGroupName $resourceGroupName `
            -Name $dataLakeStoreName `
            -Location $location 
        
        Write-Host "Create a Data Lake Analytics account ..."  -ForegroundColor Green
        New-AzureRmDataLakeAnalyticsAccount `
            -Name $dataLakeAnalyticsName `
            -ResourceGroupName $resourceGroupName `
            -Location $location `
            -DefaultDataLake $dataLakeStoreName
        
        Write-Host "The newly created Data Lake Analytics account ..."  -ForegroundColor Green
        Get-AzureRmDataLakeAnalyticsAccount `
            -ResourceGroupName $resourceGroupName `
            -Name $dataLakeAnalyticsName  

##<a name="upload-data-to-data-lake"></a>Adatok tó feltölteni az adatokat

Ebben az oktatóanyagban a bizonyos keresési naplók dolgozza.  A keresési napló adatok tó tár vagy a Azure Blob-tárolóhoz tárolható. 

Egy minta keresési naplófájl nyilvános Azure Blob-tárolóhoz másolása megtörtént. Használja az alábbi PowerShell parancsprogramot munkaállomástól töltse le a fájlt, és az alapértelmezett adattár tó fiókba az adatok tó Analytics-fiók töltse ki a fájlt.

    $dataLakeStoreName = "<The default Data Lake Store account name>"
    
    $localFolder = "C:\Tutorials\Downloads\" # A temp location for the file. 
    $storageAccount = "adltutorials"  # Don't modify this value.
    $container = "adls-sample-data"  #Don't modify this value.

    # Create the temp location  
    New-Item -Path $localFolder -ItemType Directory -Force 

    # Download the sample file from Azure Blob storage
    $context = New-AzureStorageContext -StorageAccountName $storageAccount -Anonymous
    $blobs = Azure\Get-AzureStorageBlob -Container $container -Context $context
    $blobs | Get-AzureStorageBlobContent -Context $context -Destination $localFolder

    # Upload the file to the default Data Lake Store account    
    Import-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path $localFolder"SearchLog.tsv" -Destination "/Samples/Data/SearchLog.tsv"

Az alábbi PowerShell parancsprogramot megtudhatja, hogy miként adatok tó Analytics-fiók alapértelmezett tó adattár nevének:


    $resourceGroupName = "<ResourceGroupName>"
    $dataLakeAnalyticsName = "<DataLakeAnalyticsAccountName>"
    $dataLakeStoreName = (Get-AzureRmDataLakeAnalyticsAccount -ResourceGroupName $resourceGroupName -Name $dataLakeAnalyticsName).Properties.DefaultDataLakeAccount
    echo $dataLakeStoreName

>[AZURE.NOTE] Az Azure portál felhasználói felületet, a mintaadatok fájljai másolása az alapértelmezett adattár tó fiókba. Útmutatásért lásd: az [Első lépések az Azure adatok tó Analytics Azure-portálon](data-lake-analytics-get-started-portal.md#upload-data-to-the-default-data-lake-store-account).

Adatok tó Analytics Azure Blob-tárolóhoz is elérheti.  Feltöltéshez Azure Blob-tárolóhoz, olvassa el a [Azure PowerShell használatá Azure adathordozós](../storage/storage-powershell-guide-full.md)című témakört.

##<a name="submit-data-lake-analytics-jobs"></a>Adatok tó Analytics feladatok elküldése

Az adatok tó Analytics feladatok U-SQL nyelvben nyelvű kerülnek. Többet szeretne tudni az SQL-U, [U – SQL nyelv használatába](data-lake-analytics-u-sql-get-started.md) és [U-SQL nyelv reference](http://go.microsoft.com/fwlink/?LinkId=691348)témakörben olvashat.

**Adatok tó Analytics feladat parancsfájl létrehozása**

- Hozzon létre a következő U-SQL nyelvben forgatókönyvvel egy szövegfájlt, és mentse a szövegfájlt munkaállomástól:

        @searchlog =
            EXTRACT UserId          int,
                    Start           DateTime,
                    Region          string,
                    Query           string,
                    Duration        int?,
                    Urls            string,
                    ClickedUrls     string
            FROM "/Samples/Data/SearchLog.tsv"
            USING Extractors.Tsv();
        
        OUTPUT @searchlog   
            TO "/Output/SearchLog-from-Data-Lake.csv"
        USING Outputters.Csv();

    A U-SQL nyelvben parancsfájl beolvassa az adatokat forrásfájl **Extractors.Tsv()**használatával, és ekkor létrehoz egy csv-fájl használatával **Outputters.Csv()**. 
    
    Ne módosítsa a két elérési útját, kivéve, ha a forrásfájl egy másik helyre másolja.  Adatok tó Analytics a kimeneti mappát hoz létre, ha még nem létezik.
    
    Sokkal egyszerűbb a fájlok alapértelmezett tárolt adatok tó fiókok relatív elérési utak használatával. Abszolút elérési út is használhatja.  Ha például 
    
        adl://<Data LakeStorageAccountName>.azuredatalakestore.net:443/Samples/Data/SearchLog.tsv
        
    Abszolút elérési út csatolt tároló fiókok fájlok eléréséhez kell használnia.  A csatolt Azure tárterület-fiókjában tárolt fájlok szintaxisa a következő:
    
        wasb://<BlobContainerName>@<StorageAccountName>.blob.core.windows.net/Samples/Data/SearchLog.tsv

    >[AZURE.NOTE] Nyilvános BLOB vagy nyilvános tárolók hozzáférési engedélyek Azure Blob-tárolóban jelenleg nem támogatott.    
    
    
**A feladat elküldése**

1. Nyissa meg a PowerShell ISE a Windows munkaállomásról.
2. Futtassa a következő:

        $dataLakeAnalyticsName = "<DataLakeAnalyticsAccountName>"
        $usqlScript = "c:\tutorials\data-lake-analytics\copyFile.usql"
        
        $job = Submit-AzureRmDataLakeAnalyticsJob -Name "convertTSVtoCSV" -AccountName $dataLakeAnalyticsName –ScriptPath $usqlScript 

        Wait-AdlJob -Account $dataLakeAnalyticsName -JobId $job.JobId

        Get-AzureRmDataLakeAnalyticsJob -AccountName $dataLakeAnalyticsName -JobId $job.JobId

    A parancsfájl a U-SQL nyelvben parancsfájl a c:\tutorials\data-lake-analytics\copyFile.usql vannak tárolva. A fájl elérési útja árnyalatait.
 
A feladat befejezése után az alábbi parancsmag használatával a fájlt, és töltse le a fájlt:
    
    $resourceGroupName = "<Resource Group Name>"
    $dataLakeAnalyticName = "<Data Lake Analytic Account Name>"
    $destFile = "C:\tutorials\data-lake-analytics\SearchLog-from-Data-Lake.csv"
    
    $dataLakeStoreName = (Get-AzureRmDataLakeAnalyticsAccount -ResourceGroupName $resourceGroupName -Name $dataLakeAnalyticName).Properties.DefaultDataLakeAccount
    
    Get-AzureRmDataLakeStoreChildItem -AccountName $dataLakeStoreName -path "/Output"
    
    Export-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path "/Output/SearchLog-from-Data-Lake.csv" -Destination $destFile

## <a name="see-also"></a>Lásd még:

- Az egyéb eszközök segítségével azonos oktatóprogram megtekintéséhez kattintson a lap választók lap tetején.
- Összetett lekérdezés talál további [elemzés webhely naplók Azure adatok tó Analytics használatával](data-lake-analytics-analyze-weblogs.md).
- Első lépésként U – SQL-alkalmazások fejlesztésével lásd: [adatok tó Tools for Visual Studio segítségével kidolgozása U – SQL-parancsfájlok](data-lake-analytics-data-lake-tools-get-started.md).
- Az SQL-U című témakörben talál [Azure adatok tó Analytics U – SQL nyelv használatába](data-lake-analytics-u-sql-get-started.md).
- Adatkezelési feladatok [Kezelése Azure adatok tó Analytics Azure portál használatával](data-lake-analytics-manage-use-portal.md)című témakör tartalmaz.
- Adatok tó Analytics áttekinti a [Azure adatok tó Analytics áttekintése](data-lake-analytics-overview.md)című témakörben találhat.

