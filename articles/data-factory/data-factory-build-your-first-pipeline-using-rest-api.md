<properties
    pageTitle="Az első adatok gyári (REST) összeállítása |} Microsoft Azure"
    description="Ebben az oktatóanyagban hozzon létre egy minta Azure Data Factory folyamat adatok gyári REST API-t használja."
    services="data-factory"
    documentationCenter=""
    authors="spelluru"
    manager="jhubbard"
    editor="monicar"
/>

<tags
    ms.service="data-factory"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="08/16/2016"
    ms.author="spelluru"/>

# <a name="tutorial-build-your-first-azure-data-factory-using-data-factory-rest-api"></a>Oktatóprogram: Az első Azure adatok gyári adatok gyári REST API segítségével összeállítása
> [AZURE.SELECTOR]
- [Áttekintés és a vonatkozó követelmények](data-factory-build-your-first-pipeline.md)
- [Azure portál](data-factory-build-your-first-pipeline-using-editor.md)
- [Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
- [A PowerShell](data-factory-build-your-first-pipeline-using-powershell.md)
- [Erőforrás-kezelő sablon](data-factory-build-your-first-pipeline-using-arm.md)
- [REST API-VAL](data-factory-build-your-first-pipeline-using-rest-api.md)

Ebben a cikkben a adatok gyári REST API-t az első Azure adatok gyári létrehozásához használhatja.

## <a name="prerequisites"></a>Előfeltételek
- Olvassa el a [Oktatóprogram áttekintése](data-factory-build-your-first-pipeline.md) cikket, és hajtsa végre a **előfeltétel** .
- [Curl](https://curl.haxx.se/dlwiz/) telepítheti a számítógépre. További parancsok a CURL eszköz használata adatok gyár létrehozásához. 
- Kövesse az utasításokat a [Ez a cikk](../resource-group-create-service-principal-portal.md) a: 
    1. Névvel ellátott **ADFGetStartedApp** az Azure Active Directory webalkalmazás létrehozása.
    2. **Ügyfél-azonosító** és a **titkos kulcs**juthat. 
    3. **Bérlői ID azonosító**beszerzése. 
    4. Rendelje hozzá az **Adatok gyári munkatársi** szerepkörök **ADFGetStartedApp** alkalmazását.  
- [Azure PowerShell](../powershell-install-configure.md)telepítése.  
- Indítsa el a **PowerShell** , és futtassa a következő parancsot. Azure PowerShell nyitva hagyása ebben az oktatóanyagban végéig. Ha bezárja és újra megnyitja, meg kell a parancsokat.
    1. Futtassa a **Bejelentkezési-AzureRmAccount** , és adja meg a felhasználónevet és jelszót, jelentkezzen be az Azure portálra.  
    2. Futtassa a **Get-AzureRmSubscription** ehhez a fiókhoz az előfizetések megtekintése.
    3. Futtassa a Get-AzureRmSubscription-- SubscriptionName NameOfAzureSubscription **|} Set-AzureRmContext** kattintva jelölje ki a használni kívánt előfizetést. **NameOfAzureSubscription** cserélje le az Azure előfizetése nevére. 
3. Hozzon létre egy Azure erőforráscsoport **ADFTutorialResourceGroup** nevű a PowerShell a következő parancs futtatásával:  

        New-AzureRmResourceGroup -Name ADFTutorialResourceGroup  -Location "West US"

    Néhány lépést ebben az oktatóanyagban feltételezik, hogy használja az erőforráscsoport ADFTutorialResourceGroup nevű. Ha egy másik erőforráscsoport használja, a csoport nevét, az erőforrás ADFTutorialResourceGroup helyett használja az oktatóprogram szeretne.

## <a name="create-json-definitions"></a>JSON-definíciók létrehozása
Létrehozása után a mappát, ahonnan curl.exe JSON fájlok. 

### <a name="datafactoryjson"></a>DataFactory.JSON 
> [AZURE.IMPORTANT] Nevét kell lennie globálisan egyedi, ezért érdemes előtag és utótag ADFCopyTutorialDF, így azok egy egyedi nevet. 

    {  
        "name": "FirstDataFactoryREST",  
        "location": "WestUS"
    }  

### <a name="azurestoragelinkedservicejson"></a>azurestoragelinkedservice.JSON
> [AZURE.IMPORTANT] **Fióknév** és **accountkey** cserélje nevét és Azure tároló fiókjának billentyűt. Megtudhatja, hogyan hozhatja ki a tárhely hívóbetű, lásd: a [nézet, a másolás és a hívóbetűk újragenerálása tároló](../storage/storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys).

    {
        "name": "AzureStorageLinkedService",
        "properties": {
            "type": "AzureStorage",
            "typeProperties": {
                "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
            }
        }
    }


### <a name="hdinsightondemandlinkedservicejson"></a>hdinsightondemandlinkedservice.JSON

    {
        "name": "HDInsightOnDemandLinkedService",
        "properties": {
            "type": "HDInsightOnDemand",
            "typeProperties": {
                "version": "3.2",
                "clusterSize": 1,
                "timeToLive": "00:30:00",
                "linkedServiceName": "AzureStorageLinkedService"
            }
        }
    }

Az alábbi táblázat ismertetését a kódtöredék használt JSON tulajdonságait:

| A tulajdonság | Leírás |
| :------- | :---------- |
| Verzió | Itt adhatja meg, hogy a HDInsight verziójában létrehozott 3,2. | 
| ClusterSize | A HDInsight fürt méretét. | 
| Élettartam | Meghatározza, hogy az üresjárati idejére a HDInsight fürt törlés előtt. |
| linkedServiceName | Adja meg a tárterület-fiókot, amely a naplókat HDInsight által létrehozott tárolására szolgál |

Vegye figyelembe az alábbiakat: 

- Az adatok gyári hoz a **Windows-alapú** HDInsight fürtre meg a fenti JSON. Azt **Linux-alapú** HDInsight fürt létrehozása is lehet. [Igény szerinti HDInsight csatolt szolgáltatás](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) információt talál. 
- Az igény szerinti HDInsight fürt használata helyett használhatja is **saját HDInsight fürt** . A részletekért [HDInsight csatolt szolgáltatás](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) témakörben olvashat.
- A HDInsight fürt **alapértelmezett tároló** a JSON (**linkedServiceName**) megadott blob-tárolóhoz hoz létre. HDInsight nem törli a tároló törlésekor a fürt. Ez a jelenség szándékosan van így. Igény szerinti csatolt HDInsight szolgáltatáshoz egy fürt hoz létre, minden alkalommal, amikor egy szeletet feldolgozása van, kivéve, ha van egy meglévő HDInsight live fürt (**élettartam**), és a program törli, ha elkészült a feldolgozása.

    További szeletek feldolgozása, mint az Azure blob-tárolóhoz sok tárolók látható. Ha nincs szüksége rájuk a feladatok hibaelhárítási, érdemes törölheti őket a tárhely költség csökkentése érdekében. Ezek a tárolók azoknak a hajtsa végre a minta: "adf**yourdatafactoryname**-**linkedservicename**- datetimestamp". Például a [Microsoft tároló Explorer](http://storageexplorer.com/) eszközök segítségével az Azure blob-tárolóban lévő tárolók törlése.

[Igény szerinti HDInsight csatolt szolgáltatás](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) információt talál. 

### <a name="inputdatasetjson"></a>inputdataset.JSON

    {
        "name": "AzureBlobInput",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "fileName": "input.log",
                "folderPath": "adfgetstarted/inputdata",
                "format": {
                    "type": "TextFormat",
                    "columnDelimiter": ","
                }
            },
            "availability": {
                "frequency": "Month",
                "interval": 1
            },
            "external": true,
            "policy": {}
        }
    }


A JSON **AzureBlobInput**, amely jelöl tevékenységet a során a bemeneti adatok nevű adatkészletet határozza meg. Ezeken kívül azt adja meg, hogy a bemeneti adatok a blob-tároló **adfgetstarted** neve és a **inputdata**nevű mappa található.

Az alábbi táblázat ismertetését a kódtöredék használt JSON tulajdonságait:

| A tulajdonság | Leírás |
| :------- | :---------- |
| típus | A type tulajdonság AzureBlob beállítani, mert az Azure blob-tárolóhoz tárolt adatokhoz. |  
| linkedServiceName | a korábban létrehozott StorageLinkedService hivatkozik. |
| Fájlnév | Ez a tulajdonság nem kötelező. Ha ez a tulajdonság nincs megadva, az a Mappa_útvonala származó fájlok vannak kiválasztott. Ebben az esetben csak a input.log feldolgozása. |
| típus | A naplófájlok-szöveg formátumban, így TextFormat használjuk. | 
| columnDelimiter | a naplófájlok oszlopai vessző (,) karaktert határolja |
| gyakoriság/intervallum | Hónap és az intervallum beállítása gyakoriság értéke 1, ami azt jelenti, hogy beviteli szeletet elérhetők havi. | 
| külső | Ez a tulajdonság értéke igaz, ha a bemeneti adatok Data Factory szolgáltatás nem jön létre. | 

### <a name="outputdatasetjson"></a>outputdataset.JSON

    {
        "name": "AzureBlobOutput",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "adfgetstarted/partitioneddata",
                "format": {
                    "type": "TextFormat",
                    "columnDelimiter": ","
                }
            },
            "availability": {
                "frequency": "Month",
                "interval": 1
            }
        }
    }

A JSON **AzureBlobOutput**, amely egy tevékenység, a során kimeneti adatait képviselő nevű adatkészletet határozza meg. Ezenkívül adja meg, hogy az eredmények tárolása a blob-tároló **adfgetstarted** neve és a **partitioneddata**nevű mappát. **A elérhetősége** Itt adhatja meg, hogy a kimeneti adatkészlet havi rendszerességgel elő.

### <a name="pipelinejson"></a>pipeline.JSON
> [AZURE.IMPORTANT] **Storageaccountname** cserélje le a Azure tárterület-fiókja nevét. 


    {
        "name": "MyFirstPipeline",
        "properties": {
            "description": "My first Azure Data Factory pipeline",
            "activities": [{
                "type": "HDInsightHive",
                "typeProperties": {
                    "scriptPath": "adfgetstarted/script/partitionweblogs.hql",
                    "scriptLinkedService": "AzureStorageLinkedService",
                    "defines": {
                        "inputtable": "wasb://adfgetstarted@<stroageaccountname>.blob.core.windows.net/inputdata",
                        "partitionedtable": "wasb://adfgetstarted@<stroageaccountname>t.blob.core.windows.net/partitioneddata"
                    }
                },
                "inputs": [{
                    "name": "AzureBlobInput"
                }],
                "outputs": [{
                    "name": "AzureBlobOutput"
                }],
                "policy": {
                    "concurrency": 1,
                    "retry": 3
                },
                "scheduler": {
                    "frequency": "Month",
                    "interval": 1
                },
                "name": "RunSampleHiveActivity",
                "linkedServiceName": "HDInsightOnDemandLinkedService"
            }],
            "start": "2016-07-10T00:00:00Z",
            "end": "2016-07-11T00:00:00Z",
            "isPaused": false
        }
    }

A JSON kódtöredékének a készít egy folyamat, amely egy tevékenység folyamat adatok HDInsight fürtre struktúrát használó áll.

A struktúra parancsfájl, **partitionweblogs.hql**, Azure tároló fiók (az scriptLinkedService **StorageLinkedService**nevű által megadott), és a tároló **adfgetstarted** **parancsfájl** mappájában vannak tárolva.

A **definiálja** szakasz határozza meg a struktúra parancsfájl struktúra értékként átadott futtatókörnyezet beállítások (például ${hiveconf: inputtable}, {hiveconf:partitionedtable} $).

A folyamat **kezdete** és **vége** tulajdonságainak a folyamat aktív időt adja meg.

A tevékenység JSON adja meg, hogy a struktúra parancsfájl fut-e a számítási a **linkedServiceName** – **HDInsightOnDemandLinkedService**határozza meg.

> [AZURE.NOTE] [Egy folyamat felépítése](data-factory-create-pipelines.md#anatomy-of-a-pipeline) részletes tudnivalókat az előző példában használt JSON-tulajdonságok megtekintése 

## <a name="set-global-variables"></a>Globális változók beállítása

Az Azure PowerShell hajtsa végre a következő parancsok végrehajtása az értékek cseréje a saját után:

> [AZURE.IMPORTANT] Lásd: [Előfeltételek](#prerequisites) szakasz útmutatásait ahhoz, hogy ügyfél-azonosító ügyfél titkos, bérlői azonosító, és az előfizetés azonosítóval.   

    $client_id = "<client ID of application in AAD>"
    $client_secret = "<client key of application in AAD>"
    $tenant = "<Azure tenant ID>";
    $subscription_id="<Azure subscription ID>";

    $rg = "ADFTutorialResourceGroup"
    $adf = "FirstDataFactoryREST"



## <a name="authenticate-with-aad"></a>AAD a hitelesítéshez

    $cmd = { .\curl.exe -X POST https://login.microsoftonline.com/$tenant/oauth2/token  -F grant_type=client_credentials  -F resource=https://management.core.windows.net/ -F client_id=$client_id -F client_secret=$client_secret };
    $responseToken = Invoke-Command -scriptblock $cmd;
    $accessToken = (ConvertFrom-Json $responseToken).access_token;
    
    (ConvertFrom-Json $responseToken) 



## <a name="create-data-factory"></a>Adatok gyári létrehozása

Ebben a lépésben létrehoz egy Azure Data Factory **FirstDataFactoryREST**nevű. Adatok gyár beállíthatja, hogy egy vagy több folyamatok. Egy folyamat beállíthatja, hogy egy vagy több tevékenységek rajta. Például egy példány tevékenység másolandó adatok forrásból származó adatok céltárat és a HDInsight-struktúra tevékenységének struktúra parancsfájl az adatokat a futtatására. Futtassa a következő parancsok végrehajtása az adatok gyári létrehozása: 

1. A parancs hozzárendelése **cmd**nevű változó. 

    Győződjön meg arról, hogy az adatok gyári nevét az itt megadott (ADFCopyTutorialDF) a **datafactory.json**megadott név megegyezik. 

        $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data “@datafactory.json” https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/FirstDataFactoryREST?api-version=2015-10-01};
2. A parancs futtatásával **Invoke-parancs**használatával.

        $results = Invoke-Command -scriptblock $cmd;
3. Az eredmények megtekintéséhez. Ha az adatok gyári sikeresen hozott létre, megjelenik a JSON az adatok gyári **eredmények**; egyéb esetben egy hibaüzenet jelenik meg.  

        Write-Host $results

Vegye figyelembe az alábbiakat:
 
- Az Azure Data Factory neve globálisan egyedinek kell lennie. Ha az eredményeket hibaüzenet jelenik meg: **nem érhető el adatok gyári neve "FirstDataFactoryREST"**, hajtsa végre az alábbi lépéseket:  
    1. Módosítsa a **datafactory.json** fájl nevét (például yournameFirstDataFactoryREST). [Adatok Factory - elnevezési szabályai](data-factory-naming-rules.md) témakört vonatkozó adatok gyári eltérések elnevezési szabályokat.
    2. Az első parancs, ahol a **$cmd** változó érték van hozzárendelve, a FirstDataFactoryREST cserélje ki az új nevet, és futtassa a parancsot. 
    3. A következő két parancsokat meghívása a REST API-t a data factory létrehozása és nyomtatása a a művelet eredményét. 
- Adatok Factory-példányok létrehozásához kell lennie a közös munka/rendszergazdája az Azure előfizetés
- Az adatok gyári neve bejegyezhető DNS nevével ennélfogva és a jövőben nyilvánosan láthatóvá válnak.
- Ha a hiba jelenik meg: "**az előfizetés nem regisztrált névtér Microsoft.DataFactory**", tegye a következők egyikét, és próbálja meg újra közzétenni: 

    - Az Azure PowerShell, az adatok gyári szolgáltató regisztrálhatja a következő parancsot: 
        
            Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
    
        Győződjön meg arról, hogy a következő parancsot a szolgáltató van regisztrálva Data Factory futtatható: 
    
            Get-AzureRmResourceProvider
    - Jelentkezzen be az [Azure portál](https://portal.azure.com) az Azure-előfizetést használ, és keresse meg a Data Factory lap (vagy) létrehozása adatok gyár az Azure-portálon. Ez a művelet automatikusan regisztrálja a szolgáltató meg.

Mielőtt hoz létre egy folyamat, néhány adatot gyári szervezetek először létrehozásához szükséges. Először létre csatolt szolgáltatások hivatkozás adatokat tárolja/képlet ki az adatokat tároló, határozza meg a bemeneti és kimeneti adatkészleteket jelképező csatolt adatokat tárolja az adatokat. 

## <a name="create-linked-services"></a>Hozzon létre csatolt szolgáltatások 
Ebben a lépésben összekapcsol Azure tárterület-fiókját, és a az igény szerinti Azure hdinsight szolgáltatáshoz fürt az adatok gyári. A tároló Azure-fiók adatait tartalmazza bemeneti és kimeneti a folyamat az alábbi példa a. A csatolt HDInsight szolgáltatást használják, a tevékenység a folyamat az alábbi példa a megadott struktúra parancsfájl futtatásához. 

### <a name="create-azure-storage-linked-service"></a>Azure csatolt tárhelyszolgáltatáshoz létrehozása
Ebben a lépésben összekapcsol Azure tárterület-fiókját az adatok gyári. Az ebben az oktatóanyagban használatával Azure tároló ugyanazzal a fiókkal bemeneti és kimeneti adatok és a HQL parancsprogram tárolásához.

1. A parancs hozzárendelése **cmd**nevű változó. 

        $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data “@azurestoragelinkedservice.json” https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/linkedservices/AzureStorageLinkedService?api-version=2015-10-01};
2. A parancs futtatásával **Invoke-parancs**használatával.
 
        $results = Invoke-Command -scriptblock $cmd;
3. Az eredmények megtekintéséhez. Ha a csatolt szolgáltatás sikeresen hozott létre, megjelenik a JSON a csatolt szolgáltatás az **eredmények**; egyéb esetben egy hibaüzenet jelenik meg.
  
        Write-Host $results

### <a name="create-azure-hdinsight-linked-service"></a>Azure hdinsight szolgáltatáshoz kapcsolódó szolgáltatás hozzon létre
Ebben a lépésben csatol egy igény szerinti HDInsight fürthöz az adatok gyári. A HDInsight fürt automatikusan létrejön futásidőben, és törölni a megadott számú alkalommal az áttelepítés feldolgozása és üresjárati. Az igény szerinti HDInsight fürt használata helyett használhatja is saját HDInsight fürt. Című témakör tartalmaz [számítja ki a csatolt](data-factory-compute-linked-services.md) további információt.  

1. A parancs hozzárendelése **cmd**nevű változó.
 
        $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@hdinsightondemandlinkedservice.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/linkedservices/hdinsightondemandlinkedservice?api-version=2015-10-01};
2. A parancs futtatásával **Invoke-parancs**használatával.

        $results = Invoke-Command -scriptblock $cmd;
3. Az eredmények megtekintéséhez. Ha a csatolt szolgáltatás sikeresen hozott létre, megjelenik a JSON az **eredményt**, a csatolt szolgáltatás egyéb esetben egy hibaüzenet jelenik meg.  

        Write-Host $results

## <a name="create-datasets"></a>Adatkészletek létrehozása
Ebben a lépésben létrehoz jelenítik meg a bemeneti és kimeneti struktúra feldolgozásra adatok adatkészleteket. Olvassa el az alábbi adatkészleteket az oktatóprogram korábbi részében létrehozott **StorageLinkedService** . A csatolt szolgáltatás pontok Azure tároló fiókot és adatkészleteket megadása tároló, a mappa, a fájl nevét az beviteli betöltő tárolására és a kimeneti adatok.   

### <a name="create-input-dataset"></a>Beviteli adatkészlet létrehozása
Ebben a lépésben a bemeneti adatkészlet az Azure Blob-tárolóban lévő bemeneti adatok ábrázolásához hoz létre.

1. A parancs hozzárendelése **cmd**nevű változó. 

        $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@inputdataset.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datasets/AzureBlobInput?api-version=2015-10-01};
2. A parancs futtatásával **Invoke-parancs**használatával.

        $results = Invoke-Command -scriptblock $cmd;
3. Az eredmények megtekintéséhez. Ha az adathalmazban sikeresen hozott létre, megjelenik a JSON **eredmények**; az adatkészlet számára egyéb esetben egy hibaüzenet jelenik meg.
  
        Write-Host $results
### <a name="create-output-dataset"></a>Kimeneti adatkészlet létrehozása
Ebben a lépésben a kimeneti adatkészlet az Azure Blob-tárolóban lévő kimeneti adatok ábrázolásához hoz létre.

1. A parancs hozzárendelése **cmd**nevű változó.
 
        $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@outputdataset.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datasets/AzureBlobOutput?api-version=2015-10-01};
2. A parancs futtatásával **Invoke-parancs**használatával.

        $results = Invoke-Command -scriptblock $cmd;
3. Az eredmények megtekintéséhez. Ha az adathalmazban sikeresen hozott létre, megjelenik a JSON **eredmények**; az adatkészlet számára egyéb esetben egy hibaüzenet jelenik meg.
  
        Write-Host $results 

## <a name="create-pipeline"></a>Folyamat létrehozása
Ebben a lépésben létrehoz az első folyamat **HDInsightHive** tevékenységgel. Beviteli szeletet havi áll rendelkezésre (gyakoriság: intervallum, hónap: 1), kimeneti szeletet havi készül, és az ütemező a tevékenységhez tartozó is tulajdonsága havi. A kimenet adatkészlet és a tevékenység ütemező beállításainak kell lennie. Kimeneti adatkészlet jelenleg milyen meghajtók az ütemezést, így akkor is, ha a tevékenység nem hozhatók létre bármely kimeneti létre kell hoznia egy kimenet adatkészlet. Ha a tevékenység bármely bevitel nem kerül, kihagyhatja a bemeneti adatkészlet létrehozása.  

Győződjön meg arról, hogy az Azure blob-tárolóhoz **adfgetstarted/inputdata** mappában **input.log** -fájl megtekintéséhez, és futtassa a következő parancsot a folyamat terjesztése. Mivel a **Kezdés** és a **Záró** időpont múltbeli vannak beállítva, és **isPaused** hamis értékre van állítva, a folyamat (a tevékenység a során) fut, közvetlenül a telepítése után. 

1. A parancs hozzárendelése **cmd**nevű változó.
 
        $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@pipeline.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datapipelines/MyFirstPipeline?api-version=2015-10-01};
2. A parancs futtatásával **Invoke-parancs**használatával.

        $results = Invoke-Command -scriptblock $cmd;
3. Az eredmények megtekintéséhez. Ha az adathalmazban sikeresen hozott létre, megjelenik a JSON **eredmények**; az adatkészlet számára egyéb esetben egy hibaüzenet jelenik meg.  

        Write-Host $results
5. Gratulálunk, az első folyamat Azure PowerShell használatával sikeresen létrehozott!

## <a name="monitor-pipeline"></a>Monitor folyamat
Ebben a lépésben használatával adatok gyári REST API figyelheti a folyamat készített szeletek.

    $ds ="AzureBlobOutput"

    $cmd = {.\curl.exe -X GET -H "Authorization: Bearer $accessToken" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datasets/$ds/slices?start=1970-01-01T00%3a00%3a00.0000000Z"&"end=2016-08-12T00%3a00%3a00.0000000Z"&"api-version=2015-10-01};

    $results2 = Invoke-Command -scriptblock $cmd;

    IF ((ConvertFrom-Json $results2).value -ne $NULL) {
        ConvertFrom-Json $results2 | Select-Object -Expand value | Format-Table
    } else {
            (convertFrom-Json $results2).RemoteException
    }


> [AZURE.IMPORTANT] 
> Az igény szerinti HDInsight fürt kibocsátása rendszerint valamikor vesz igénybe (körülbelül 20 perc). Ezért várjon **körülbelül 30 percig** a szeletet feldolgozása érvénybe a folyamat.  

Futtassa a Invoke parancs és a következő, amíg meg nem jelenik a szeletre **készen áll arra,** vagy **nem sikerült** állam. Ha a szeletet kész állapotban van, ellenőrizze, hogy a blob-tárolóban a kimeneti adatokhoz a **adfgetstarted** tároló **partitioneddata** mappájában.  Az igény szerinti HDInsight fürt kibocsátása rendszerint egy kis időt vesz igénybe.

![kimeneti adatai](./media/data-factory-build-your-first-pipeline-using-rest-api/three-ouptut-files.png)

> [AZURE.IMPORTANT] A bemeneti fájl kap törlésekor a szeletet importálni. Ezért, ha azt szeretné, futtassa újra a szeletet, és végezze el ismét az oktatóanyagot, a beviteli fájl feltöltése (input.log) a adfgetstarted tároló inputdata mappájába.

Azure portal segítségével szeletek figyelésére és kapcsolatos problémák megoldásához. [Azure portálon folyamatok figyelése](data-factory-build-your-first-pipeline-using-editor.md##monitor-pipeline) részletei című részben találja.  

## <a name="summary"></a>Összefoglalás 
Ebben az oktatóprogramban egy Azure adatok gyári folyamat adatok struktúra parancsprogram futtatása a HDInsight hadoop fürthöz hozta létre. Használva a adatok Factory-szerkesztőben az Azure-portálon végezze el az alábbi lépéseket:  

1.  Az Azure **adatok gyári**létre.
2.  Hozzon létre két **csatolt szolgáltatások**:
    1.  **Azure tároló** csatolja a szolgáltatást, a bemeneti és kimeneti fájlok tárolja az adatok gyári Azure blob-tárolóhoz hivatkozás.
    2.  **Azure hdinsight szolgáltatáshoz** igény szerinti csatolt a szolgáltatást az igény szerinti HDInsight Hadoop fürt csatolása az adatok gyári. Azure Data Factory hoz létre egy HDInsight Hadoop fürt csak igény bemeneti adatok folyamat, és a kimeneti adatai kiszámítására. 
3.  Létre két **adatkészleteket**, amelyek leírják a bemeneti és kimeneti adatok HDInsight-struktúra a tevékenységhez a során. 
4.  A **folyamat** létrehozott **HDInsight-struktúra** tevékenységgel. 

## <a name="next-steps"></a>Következő lépések
Ebben a cikkben egy folyamat átalakítási tevékenységet (HDInsight tevékenység), amelyet a struktúra parancsfájl futtat egy igény szerinti Azure hdinsight szolgáltatáshoz fürt hozott létre. A Másolás tevékenységének használata adatok másolása Azure SQL-Azure Blob megtekintéséhez kattintson a [oktatóanyag: adatok másolása Azure SQL-Azure Blob](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

## <a name="see-also"></a>Lásd még:
| A témakör | Leírás |
| :---- | :---- |
| [Adatok gyári REST API-hivatkozás](https://msdn.microsoft.com/library/azure/dn906738.aspx) |  Teljes dokumentációjában gyári adatok parancsmagok |
| [A tevékenységekre vonatkozó adatok transzformációt hajt végre.](data-factory-data-transformation-activities.md) | Ebben a cikkben egy tevékenységlista adatainak átalakítása (például az oktatóprogram használt HDInsight-struktúra átalakítása) Azure Data Factory által támogatott. |
| [Ütemezés- és végrehajtása](data-factory-scheduling-and-execution.md) | Ez a cikk ismerteti az Azure Data Factory alkalmazásmodell ütemezési és a végrehajtás szempontjait. |
| [Folyamatok](data-factory-create-pipelines.md) | Ez a cikk segít megérteni a folyamatok és Azure Data Factory és használatuk összeállításához végpontok közötti adatalapú munkafolyamatok forgatókönyv vagy üzleti tevékenységek. |
| [Adatkészletek](data-factory-create-datasets.md) | Ez a cikk segít megérteni az Azure Data Factory adatkészleteket.
| [Figyelésére és kezelheti a folyamatok Azure portál pengéit használatával](data-factory-monitor-manage-pipelines.md) | Ez a cikk leírja, hogy miként figyelheti, kezelése és a folyamatok Azure portál pengéit használatával hibakeresési. |
| [Figyelésére és figyelése alkalmazással folyamatok kezelése](data-factory-monitor-manage-app.md) | Ez a cikk leírja, hogy miként figyelheti, kezelése és használata a figyelő és felügyeleti alkalmazás folyamatok hibakeresési. 

