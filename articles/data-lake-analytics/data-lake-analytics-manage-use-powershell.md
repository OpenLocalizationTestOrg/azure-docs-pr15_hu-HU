<properties 
   pageTitle="Azure adatok tó Analytics Azure PowerShell használatával kezelése |} Azure" 
   description="Útmutató: adatok tó Analytics-feladatok, az adatforrások, a felhasználók kezelése " 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="edmacauley" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="05/16/2016"
   ms.author="edmaca"/>

# <a name="manage-azure-data-lake-analytics-using-azure-powershell"></a>Azure adatok tó Analytics Azure PowerShell használatával kezelése

[AZURE.INCLUDE [manage-selector](../../includes/data-lake-analytics-selector-manage.md)]

Megtudhatja, hogy miként kezelheti az Azure adatok tó Analytics-fiókokat, az adatforrások, a felhasználók és a feladatok az Azure PowerShell használatával. Egyéb eszközök segítségével management témakörök megtekintéséhez kattintson a fenti lapon válasszon.

**Előfeltételek**

Ebben az oktatóanyagban megkezdése előtt a következőket kell rendelkeznie:

- **Az Azure-előfizetés**. Lásd: [Ismerkedés az Azure ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/).


<!-- ################################ -->
<!-- ################################ -->


##<a name="install-azure-powershell-10-or-greater"></a>Telepítse az Azure PowerShell 1,0-s vagy újabb

Olvassa el [Az Azure erőforrás-kezelő Azure a PowerShell használatával](powershell-azure-resource-manager.md#prerequisites)kapcsolatban előzetesen szükséges szakaszát.
    
## <a name="manage-accounts"></a>Fiókok kezelése

Mielőtt bármilyen adat tó Analytics-feladat futtatása, az adatok tó Analytics fiókkal kell rendelkeznie. Azure hdinsight szolgáltatáshoz, eltérően nem fizet a Analytics fiókjához, amikor nem fut a feladatot.  Csak az alkalommal, amikor egy projekt fut fizet.  További információ az [Azure adatok tó Analytics áttekintése](data-lake-analytics-overview.md)című témakörben találhat.  

###<a name="create-accounts"></a>Fiókok létrehozása

    $resourceGroupName = "<ResourceGroupName>"
    $dataLakeStoreName = "<DataLakeAccountName>"
    $dataLakeAnalyticsAccountName = "<DataLakeAnalyticsAccountName>"
    $location = "<Microsoft Data Center>"
    
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
        -Name $dataLakeAnalyticsAccountName `
        -ResourceGroupName $resourceGroupName `
        -Location $location `
        -DefaultDataLake $dataLakeStoreName
    
    Write-Host "The newly created Data Lake Analytics account ..."  -ForegroundColor Green
    Get-AzureRmDataLakeAnalyticsAccount `
        -ResourceGroupName $resourceGroupName `
        -Name $dataLakeAnalyticsAccountName  

Az erőforráscsoport Azure-sablon is használhatja. Sablon létrehozása egy adatok tó Analytics és a függő tó adattár számlát [mintája](#appendix-a)szerepel. A sablon mentéséhez .json sablonnal egy fájlba, és az alábbi PowerShell parancsprogramot segítségével hívja meg:


    $AzureSubscriptionID = "<Your Azure Subscription ID>"
    
    $ResourceGroupName = "<New Azure Resource Group Name>"
    $Location = "EAST US 2"
    $DefaultDataLakeStoreAccountName = "<New Data Lake Store Account Name>"
    $DataLakeAnalyticsAccountName = "<New Data Lake Analytics Account Name>"
    
    $DeploymentName = "MyDataLakeAnalyticsDeployment"
    $ARMTemplateFile = "E:\Tutorials\ADL\ARMTemplate\azuredeploy.json"  # update the Json template path 
    
    Login-AzureRmAccount
    
    Select-AzureRmSubscription -SubscriptionId $AzureSubscriptionID
    
    # Create the resource group
    New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Location
    
    # Create the Data Lake Analytics account with the default Data Lake Store account.
    $parameters = @{"adlAnalyticsName"=$DataLakeAnalyticsAccountName; "adlStoreName"=$DefaultDataLakeStoreAccountName}
    New-AzureRmResourceGroupDeployment -Name $DeploymentName -ResourceGroupName $ResourceGroupName -TemplateFile $ARMTemplateFile -TemplateParameterObject $parameters 

 
###<a name="list-account"></a>Lista-fiók

Lista adatainak tó Analytics fiókok belül az aktuális előfizetés

    Get-AzureRmDataLakeAnalyticsAccount
    
A kimenet:

    Id         : /subscriptions/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/resourceGroups/learn1021rg/providers/Microsoft.DataLakeAnalytics/accounts/learn1021adla
    Location   : eastus2
    Name       : learn1021adla
    Properties : Microsoft.Azure.Management.DataLake.Analytics.Models.DataLakeAnalyticsAccountProperties
    Tags       : {}
    Type       : Microsoft.DataLakeAnalytics/accounts

Lista adatainak tó Analytics fiókok egy adott erőforrás csoporton belül

    Get-AzureRmDataLakeAnalyticsAccount -ResourceGroupName $resourceGroupName

Részletek a meghatározott adatok tó Analytics-fiók

    Get-AzureRmDataLakeAnalyticsAccount -Name $adlAnalyticsAccountName

Egy adott adatok tó Analytics-fiók megléte tesztelése

    Test-AzureRmDataLakeAnalyticsAccount -Name $adlAnalyticsAccountName

A parancsmaggal ad vissza, **Igaz** vagy **hamis**.

###<a name="delete-data-lake-analytics-accounts"></a>Adatok tó Analytics-fiókokat törlése

    $resourceGroupName = "<ResourceGroupName>"
    $dataLakeAnalyticsAccountName = "<DataLakeAnalyticsAccountName>"
    
    Remove-AzureRmDataLakeAnalyticsAccount -Name $dataLakeAnalyticsAccountName 

Törlés adatok tó Analytics-fiók nem törlődik a függő tó adattárolás fiókot. Az alábbi példa az adatok tó Analytics-fiók és az az alapértelmezett adattár tó fiók törlése

    $resourceGroupName = "<ResourceGroupName>"
    $dataLakeAnalyticsAccountName = "<DataLakeAnalyticsAccountName>"
    $dataLakeStoreName = (Get-AzureRmDataLakeAnalyticsAccount -ResourceGroupName $resourceGroupName -Name $dataLakeAnalyticAccountName).Properties.DefaultDataLakeAccount

    Remove-AzureRmDataLakeAnalyticsAccount -ResourceGroupName $resourceGroupName -Name $dataLakeAnalyticAccountName 
    Remove-AzureRmDataLakeStoreAccount -ResourceGroupName $resourceGroupName -Name $dataLakeStoreName

<!-- ################################ -->
<!-- ################################ -->
## <a name="manage-account-data-sources"></a>Fiók adatforrások kezelése

Adatok tó Analytics jelenleg támogatja a következő adatforrásokhoz:

- [Azure tó adattárhoz](../data-lake-store/data-lake-store-overview.md)
- [Azure tárhely](storage-introduction.md)

Amikor létrehoz egy Analytics-fiókot, ki kell jelölnie Azure tó adattárolás fiókkal az alapértelmezett tárterület-fiók. Az alapértelmezett adattár tó fiók feladat metaadat- és a naplókat tárolására szolgál. Miután létrehozott egy Analytics-fiókot, további tó adattárolás fiókok, illetve Azure tárterület-fiókot is hozzáadhat. 

### <a name="find-the-default-data-lake-store-account"></a>Keresse meg az alapértelmezett adattár tó fiók

    $resourceGroupName = "<ResourceGroupName>"
    $dataLakeAnalyticsAccountName = "<DataLakeAnalyticsAccountName>"
    $dataLakeStoreName = (Get-AzureRmDataLakeAnalyticsAccount -ResourceGroupName $resourceGroupName -Name $dataLakeAnalyticAccountName).Properties.DefaultDataLakeAccount


### <a name="add-additional-azure-blob-storage-accounts"></a>Azure Blob-tároló további fiókok hozzáadása

    $resourceGroupName = "<ResourceGroupName>"
    $dataLakeAnalyticsAccountName = "<DataLakeAnalyticsAccountName>"
    $AzureStorageAccountName = "<AzureStorageAccountName>"
    $AzureStorageAccountKey = "<AzureStorageAccountKey>"
    
    Add-AzureRmDataLakeAnalyticsDataSource -ResourceGroupName $resourceGroupName -Account $dataLakeAnalyticAccountName -AzureBlob $AzureStorageAccountName -AccessKey $AzureStorageAccountKey

### <a name="add-additional-data-lake-store-accounts"></a>További tó adattár fiókok hozzáadása

    $resourceGroupName = "<ResourceGroupName>"
    $dataLakeAnalyticsAccountName = "<DataLakeAnalyticsAccountName>"
    $AzureDataLakeName = "<DataLakeStoreName>"
    
    Add-AzureRmDataLakeAnalyticsDataSource -ResourceGroupName $resourceGroupName -Account $dataLakeAnalyticAccountName -DataLake $AzureDataLakeName 

### <a name="list-data-sources"></a>Adatforrások felsorolása:

    $resourceGroupName = "<ResourceGroupName>"
    $dataLakeAnalyticsAccountName = "<DataLakeAnalyticsAccountName>"

    (Get-AzureRmDataLakeAnalyticsAccount -ResourceGroupName $resourceGroupName -Name $dataLakeAnalyticAccountName).Properties.DataLakeStoreAccounts
    (Get-AzureRmDataLakeAnalyticsAccount -ResourceGroupName $resourceGroupName -Name $dataLakeAnalyticAccountName).Properties.StorageAccounts
    


<!-- ################################ -->
<!-- ################################ -->
## <a name="manage-jobs"></a>Feladatok kezelése

Adatok tó Analytics-fiókkal kell rendelkeznie, mielőtt létrehozhat egy feladatot.  További tudnivalókért lásd: [kezelése adatok tó Analytics-fiókokat](#manage-data-lake-analytics-accounts).

### <a name="list-jobs"></a>Feladatok lista

    $dataLakeAnalyticsAccountName = "<DataLakeAnalyticsAccountName>"
    
    Get-AzureRmDataLakeAnalyticsJob -Account $dataLakeAnalyticAccountName
    
    Get-AzureRmDataLakeAnalyticsJob -Account $dataLakeAnalyticAccountName -State Running, Queued
    #States: Accepted, Compiling, Ended, New, Paused, Queued, Running, Scheduling, Starting
    
    Get-AzureRmDataLakeAnalyticsJob -Account $dataLakeAnalyticAccountName -Result Cancelled
    #Results: Cancelled, Failed, None, Successed 
    
    Get-AzureRmDataLakeAnalyticsJob -Account $dataLakeAnalyticAccountName -Name <Job Name>
    Get-AzureRmDataLakeAnalyticsJob -Account $dataLakeAnalyticAccountName -Submitter <Job submitter>

    # List all jobs submitted on January 1 (local time)
    Get-AzureRmDataLakeAnalyticsJob -Account $dataLakeAnalyticAccountName `
        -SubmittedAfter "2015/01/01"
        -SubmittedBefore "2015/01/02"   

    # List all jobs that succeeded on January 1 after 2 pm (UTC time)
    Get-AzureRmDataLakeAnalyticsJob -Account $dataLakeAnalyticAccountName `
        -State Ended
        -Result Succeeded
        -SubmittedAfter "2015/01/01 2:00 PM -0"
        -SubmittedBefore "2015/01/02 -0"

    # List all jobs submitted in the past hour
    Get-AzureRmDataLakeAnalyticsJob -Account $dataLakeAnalyticAccountName `
        -SubmittedAfter (Get-Date).AddHours(-1)

### <a name="get-job-details"></a>Feladat részletei beszerzése

    $dataLakeAnalyticsAccountName = "<DataLakeAnalyticsAccountName>"
    Get-AzureRmDataLakeAnalyticsJob -Account $dataLakeAnalyticAccountName -JobID <Job ID>
    
### <a name="submit-jobs"></a>Feladatok elküldése

    $dataLakeAnalyticsAccountName = "<DataLakeAnalyticsAccountName>"

    #Pass script via path
    Submit-AzureRmDataLakeAnalyticsJob -Account $dataLakeAnalyticAccountName `
        -Name $jobName `
        -ScriptPath $scriptPath

    #Pass script contents
    Submit-AzureRmDataLakeAnalyticsJob -Account $dataLakeAnalyticAccountName `
        -Name $jobName `
        -Script $scriptContents

> [AZURE.NOTE] Egy feladat alapértelmezett érték 1000, és a párhuzamos feladat alapértelmezett fokú érték 1.


### <a name="cancel-jobs"></a>Feladat megszakítása

    Stop-AzureRmDataLakeAnalyticsJob -Account $dataLakeAnalyticAccountName `
        -JobID $jobID


## <a name="manage-catalog-items"></a>Katalógus elemek kezelése

A U-SQL nyelvben katalógus adatok és a kód felépítését, így U-SQL nyelvben parancsfájlok megosztható szolgál. A katalógus lehetővé teszi, hogy az adatokkal az Azure adatok tó lehetséges legnagyobb teljesítményét. További tudnivalókért olvassa el a [U SQL katalógus](data-lake-analytics-use-u-sql-catalog.md)című témakört.

###<a name="list-catalog-items"></a>Katalógus elemek

    #List databases
    Get-AzureRmDataLakeAnalyticsCatalogItem `
        -Account $adlAnalyticsAccountName `
        -ItemType Database
    
    
    
    #List tables
    Get-AzureRmDataLakeAnalyticsCatalogItem `
        -Account $adlAnalyticsAccountName `
        -ItemType Table `
        -Path "master.dbo"

###<a name="get-catalog-item-details"></a>Katalógus elem részletek 

    #Get a database
    Get-AzureRmDataLakeAnalyticsCatalogItem `
        -Account $adlAnalyticsAccountName `
        -ItemType Database `
        -Path "master"
    
    #Get a table
    Get-AzureRmDataLakeAnalyticsCatalogItem `
        -Account $adlAnalyticsAccountName `
        -ItemType Table `
        -Path "master.dbo.mytable"

###<a name="test-existence-of--catalog-item"></a>Katalógus elem megléte tesztelése

    Test-AzureRmDataLakeAnalyticsCatalogItem  `
        -Account $adlAnalyticsAccountName `
        -ItemType Database `
        -Path "master"

###<a name="create-catalog-secret"></a>Titkos katalógus létrehozása
    New-AzureRmDataLakeAnalyticsCatalogSecret  `
            -Account $adlAnalyticsAccountName `
            -DatabaseName "master" `
            -Secret (Get-Credential -UserName "username" -Message "Enter the password")

### <a name="modify-catalog-secret"></a>Katalógus titkos kulcs módosítása
    Set-AzureRmDataLakeAnalyticsCatalogSecret  `
            -Account $adlAnalyticsAccountName `
            -DatabaseName "master" `
            -Secret (Get-Credential -UserName "username" -Message "Enter the password")



###<a name="delete-catalog-secret"></a>Katalógus titkos törlése
    Remove-AzureRmDataLakeAnalyticsCatalogSecret  `
            -Account $adlAnalyticsAccountName `
            -DatabaseName "master"


## <a name="use-azure-resource-manager-groups"></a>Erőforrás-kezelő Azure csoportok használata

Alkalmazások általában épülnek fel sok összetevőjét, például egy webalkalmazás, adatbázis, adatbázis-kiszolgáló, tárolási és 3 külső szolgáltatásokra. Azure erőforrás-kezelő (ARM) lehetővé teszi az erőforrások csoportként az Azure erőforráscsoport néven az alkalmazás használata. Telepítése, frissítése, figyelésére vagy törlése az összes erőforrás egyetlen, összehangolt műveletben az alkalmazás. Telepítés olyan sablont használ, és a sablonon használhatja például vizsgálat, átmeneti és üzemi különböző környezetekben. Egyértelművé teheti számlázási a szervezet megtekintésével, a teljes csoporton az összegzett költségét. További információ az [Azure erőforrás-kezelő áttekintése](../azure-resource-manager/resource-group-overview.md)című témakörben találhat. 

Adatok tó Analytics szolgáltatás az alábbi összetevőket a következők lehetnek:

- Azure adatok tó Analytics-fiók
- Szükséges alapértelmezett tó adattárolás Azure-fiók
- Azure-adatok tó további tárterület fiókok
- További tárterület Azure-fiókok

Létrehozhat egy ARM csoportban, hogy könnyebben kezelhető az összetevők.

![Azure adatok tó Analytics-fiók és tárolása](./media/data-lake-analytics-manage-use-portal/data-lake-analytics-arm-structure.png)

Az azonos Azure adatközpontban egy adatok tó Analytics-fiók és az függő tároló fiókok kell mutatnia.
A ARM csoport azonban is található egy másik adatközpont.  

##<a name="see-also"></a>Lásd még: 

- [Microsoft Azure adatok tó Analytics áttekintése](data-lake-analytics-overview.md)
- [Első lépések az adatok tó Analytics Azure portál használatával](data-lake-analytics-get-started-portal.md)
- [Azure adatok tó Analytics Azure portálon kezelése](data-lake-analytics-manage-use-portal.md)
- [Figyelésére és Azure portálon Azure adatok tó Analytics feladatok hibaelhárítása](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)

##<a name="appendix-a---data-lake-analytics-arm-template"></a>Mintája - adatok tó Analytics ARM sablon

A következő ARM-sablon használható egy adatok tó Analytics-fiók és a függő tó adattár fiók telepítéséhez.  A json-fájlként mentheti, és a PowerShell-parancsprogramot segítségével hívja fel a sablont. További tudnivalókért lásd: a [Deploy Azure erőforrás-kezelő sablonnal az alkalmazások](../resource-group-template-deploy.md#deploy-with-powershell) és a [szerzői Azure erőforrás-kezelő sablonokat](../resource-group-authoring-templates.md).

    {
      "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "adlAnalyticsName": {
          "type": "string",
          "metadata": {
            "description": "The name of the Data Lake Analytics account to create."
          }
        },
        "adlStoreName": {
          "type": "string",
          "metadata": {
            "description": "The name of the Data Lake Store account to create."
          }
        }
      },
      "resources": [
        {
          "name": "[parameters('adlStoreName')]",
          "type": "Microsoft.DataLakeStore/accounts",
          "location": "East US 2",
          "apiVersion": "2015-10-01-preview",
          "dependsOn": [ ],
          "tags": { }
        },
        {
          "name": "[parameters('adlAnalyticsName')]",
          "type": "Microsoft.DataLakeAnalytics/accounts",
          "location": "East US 2",
          "apiVersion": "2015-10-01-preview",
          "dependsOn": [ "[concat('Microsoft.DataLakeStore/accounts/',parameters('adlStoreName'))]" ],
          "tags": { },
          "properties": {
            "defaultDataLakeStoreAccount": "[parameters('adlStoreName')]",
            "dataLakeStoreAccounts": [
              { "name": "[parameters('adlStoreName')]" }
            ]
          }
        }
      ],
      "outputs": {
        "adlAnalyticsAccount": {
          "type": "object",
          "value": "[reference(resourceId('Microsoft.DataLakeAnalytics/accounts',parameters('adlAnalyticsName')))]"
        },
        "adlStoreAccount": {
          "type": "object",
          "value": "[reference(resourceId('Microsoft.DataLakeStore/accounts',parameters('adlStoreName')))]"
        }
      }
    }

