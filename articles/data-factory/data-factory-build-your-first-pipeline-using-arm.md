<properties
    pageTitle="Az első adatok gyári (erőforrás-kezelő sablon) összeállítása |} Microsoft Azure"
    description="Ebben az oktatóanyagban hozzon létre egy minta Azure Data Factory folyamat egy erőforrás-kezelő Azure-sablon segítségével."
    services="data-factory"
    documentationCenter=""
    authors="spelluru"
    manager="jhubbard"
    editor="monicar"/>

<tags
    ms.service="data-factory"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="10/12/2016"
    ms.author="spelluru"/>

# <a name="tutorial-build-your-first-azure-data-factory-using-azure-resource-manager-template"></a>Oktatóprogram: Az első Azure adatok gyári Azure erőforrás-kezelő sablonnal összeállítása
> [AZURE.SELECTOR]
- [Áttekintés és a vonatkozó követelmények](data-factory-build-your-first-pipeline.md)
- [Azure portál](data-factory-build-your-first-pipeline-using-editor.md)
- [Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
- [A PowerShell](data-factory-build-your-first-pipeline-using-powershell.md)
- [Erőforrás-kezelő sablon](data-factory-build-your-first-pipeline-using-arm.md)
- [REST API-VAL](data-factory-build-your-first-pipeline-using-rest-api.md)

Ebben a cikkben egy erőforrás-kezelő Azure-sablon létrehozása az Azure adatok első gyári használja.

## <a name="prerequisites"></a>Előfeltételek
- Olvassa el a [Oktatóprogram áttekintése](data-factory-build-your-first-pipeline.md) cikket, és hajtsa végre a **előfeltétel** .
- Kövesse az Azure PowerShell legújabb verziójának telepítése a számítógépen [Telepítse és állítsa be a Azure PowerShell módjáról](../powershell-install-configure.md) a cikk utasításait.
- Lásd: [Azure erőforrás-kezelő sablonok létrehozása](../resource-group-authoring-templates.md) , ha többet szeretne tudni az Azure erőforrás-kezelő sablonok. 

## <a name="in-this-tutorial"></a>Ebben az oktatóanyagban
Személy | Leírás  
------ | ----------- 
Azure csatolt tárhelyszolgáltatáshoz | Azure tároló fiókja csatol adatokat gyári. A tároló Azure-fiók adatait tartalmazza bemeneti és kimeneti a folyamat az alábbi példa a. 
HDInsight igény szerinti csatolt szolgáltatás| Az igény szerinti HDInsight hivatkozások fürt az adatok gyári. A fürt automatikusan létrejön folyamat adatokat, és a feldolgozás után a program törli.
Azure Blob-beviteli adatkészlet | Az Azure csatolt tárhelyszolgáltatáshoz hivatkozik. A csatolt szolgáltatás Azure tároló fiók hivatkozik, és az Azure Blob-adatkészlet adja meg a tároló, a mappa és a fájlnevet, amely a bemeneti adatok tárolására. 
Azure Blob-kimeneti adatkészlet | Az Azure csatolt tárhelyszolgáltatáshoz hivatkozik. A csatolt szolgáltatás Azure tároló fiók hivatkozik, és az Azure Blob-adatkészlet adja meg a tároló, a mappa és a fájl nevét a tárhely, a kimeneti adatokat tartalmazó. 
Adatok folyamat | Rendelkezik a folyamat típusú HDInsightHive tevékenységet a bemeneti adatkészlet fogyaszt, és a kimeneti adatkészlet eredményez.   

Adatok gyár beállíthatja, hogy egy vagy több folyamatok. Egy folyamat beállíthatja, hogy egy vagy több tevékenységek rajta. Tevékenységek két típusa van: [adatok mozgását tevékenységeket](data-factory-data-movement-activities.md) és [adatok átalakítása tevékenységeket](data-factory-data-transformation-activities.md). Ebben az oktatóanyagban hoz létre egy folyamat egy tevékenység (Másolás tevékenység).

A következő szakaszban a teljes erőforrás-kezelő sablon nyújt a Data Factory szervezetek definiálása az, hogy gyorsan futtathatja az oktatóprogram és tesztelje a sablont. Megértéséhez, hogy hogyan minden adat gyári entitás van megadva, lásd: [a sablonban Data Factory személyek](#data-factory-entities-in-the-template) szakaszban.

## <a name="data-factory-json-template"></a>Adatok gyári JSON-sablon
A legfelső szintű erőforrás-kezelő sablon adatok gyár definiálása van: 

    {
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": { ...
        },
        "variables": { ...
        },
        "resources": [
            {
                "name": "[parameters('dataFactoryName')]",
                "apiVersion": "[variables('apiVersion')]",
                "type": "Microsoft.DataFactory/datafactories",
                "location": "westus",
                "resources": [
                    { ... },
                    { ... },
                    { ... },
                    { ... }
                ]
            }
        ]
    }

Névvel ellátott **ADFTutorialARM.json** **C:\ADFGetStarted** mappában, a következő tartalommal JSON-fájl létrehozása:

    {
        "contentVersion": "1.0.0.0",
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "parameters": {
            "storageAccountName": { "type": "string", "metadata": { "description": "Name of the Azure storage account that contains the input/output data." } },
            "storageAccountKey": { "type": "securestring", "metadata": { "description": "Key for the Azure storage account." } },
            "blobContainer": { "type": "string", "metadata": { "description": "Name of the blob container in the Azure Storage account." } },
            "inputBlobFolder": { "type": "string", "metadata": { "description": "The folder in the blob container that has the input file." } },
            "inputBlobName": { "type": "string", "metadata": { "description": "Name of the input file/blob." } },
            "outputBlobFolder": { "type": "string", "metadata": { "description": "The folder in the blob container that will hold the transformed data." } },
            "hiveScriptFolder": { "type": "string", "metadata": { "description": "The folder in the blob container that contains the Hive query file." } },
            "hiveScriptFile": { "type": "string", "metadata": { "description": "Name of the hive query (HQL) file." } }
        },
        "variables": {
            "dataFactoryName": "[concat('HiveTransformDF', uniqueString(resourceGroup().id))]",
            "azureStorageLinkedServiceName": "AzureStorageLinkedService",
            "hdInsightOnDemandLinkedServiceName": "HDInsightOnDemandLinkedService",
            "blobInputDatasetName": "AzureBlobInput",
            "blobOutputDatasetName": "AzureBlobOutput",
            "pipelineName": "HiveTransformPipeline"
        },
        "resources": [
        {
            "name": "[variables('dataFactoryName')]",
            "apiVersion": "2015-10-01",
            "type": "Microsoft.DataFactory/datafactories",
            "location": "West US",
            "resources": [
            {
                "type": "linkedservices",
                "name": "[variables('azureStorageLinkedServiceName')]",
                "dependsOn": [
                    "[variables('dataFactoryName')]"
                ],
                "apiVersion": "2015-10-01",
                "properties": {
                    "type": "AzureStorage",
                    "description": "Azure Storage linked service",
                    "typeProperties": {
                        "connectionString": "[concat('DefaultEndpointsProtocol=https;AccountName=',parameters('storageAccountName'),';AccountKey=',parameters('storageAccountKey'))]"
                    }
                }
            },
            {
                "type": "linkedservices",
                "name": "[variables('hdInsightOnDemandLinkedServiceName')]",
                "dependsOn": [
                    "[variables('dataFactoryName')]",
                    "[variables('azureStorageLinkedServiceName')]"
                ],
                "apiVersion": "2015-10-01",
                "properties": {
                    "type": "HDInsightOnDemand",
                    "typeProperties": {
                        "clusterSize": 1,
                        "version": "3.2",
                        "timeToLive": "00:05:00",
                        "osType": "windows",
                        "linkedServiceName": "[variables('azureStorageLinkedServiceName')]"
                    }
                }
            },
            {
                "type": "datasets",
                "name": "[variables('blobInputDatasetName')]",
                "dependsOn": [
                    "[variables('dataFactoryName')]",
                    "[variables('azureStorageLinkedServiceName')]"
                ],
                "apiVersion": "2015-10-01",
                "properties": {
                    "type": "AzureBlob",
                    "linkedServiceName": "[variables('azureStorageLinkedServiceName')]",
                    "typeProperties": {
                        "fileName": "[parameters('inputBlobName')]",
                        "folderPath": "[concat(parameters('blobContainer'), '/', parameters('inputBlobFolder'))]",
                        "format": {
                            "type": "TextFormat",
                            "columnDelimiter": ","
                        }
                    },
                    "availability": {
                        "frequency": "Month",
                        "interval": 1
                    },
                    "external": true
                }
            },
            {
                "type": "datasets",
                "name": "[variables('blobOutputDatasetName')]",
                "dependsOn": [
                    "[variables('dataFactoryName')]",
                    "[variables('azureStorageLinkedServiceName')]"
                ],
                "apiVersion": "2015-10-01",
                "properties": {
                    "type": "AzureBlob",
                    "linkedServiceName": "[variables('azureStorageLinkedServiceName')]",
                    "typeProperties": {
                        "folderPath": "[concat(parameters('blobContainer'), '/', parameters('outputBlobFolder'))]",
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
            },
            {
                "type": "datapipelines",
                "name": "[variables('pipelineName')]",
                "dependsOn": [
                    "[variables('dataFactoryName')]",
                    "[variables('azureStorageLinkedServiceName')]",
                    "[variables('hdInsightOnDemandLinkedServiceName')]",
                    "[variables('blobInputDatasetName')]",
                    "[variables('blobOutputDatasetName')]"
                ],
                "apiVersion": "2015-10-01",
                "properties": {
                    "description": "Pipeline that transforms data using Hive script.",
                    "activities": [
                    {
                        "type": "HDInsightHive",
                        "typeProperties": {
                            "scriptPath": "[concat(parameters('blobContainer'), '/', parameters('hiveScriptFolder'), '/', parameters('hiveScriptFile'))]",
                            "scriptLinkedService": "[variables('azureStorageLinkedServiceName')]",
                            "defines": {
                                "inputtable": "[concat('wasb://', parameters('blobContainer'), '@', parameters('storageAccountName'), '.blob.core.windows.net/', parameters('inputBlobFolder'))]",
                                "partitionedtable": "[concat('wasb://', parameters('blobContainer'), '@', parameters('storageAccountName'), '.blob.core.windows.net/', parameters('outputBlobFolder'))]"
                            }
                        },
                        "inputs": [
                            {
                                "name": "[variables('blobInputDatasetName')]"
                            }
                        ],
                        "outputs": [
                            {
                                "name": "[variables('blobOutputDatasetName')]"
                            }
                        ],
                        "policy": {
                            "concurrency": 1,
                            "retry": 3
                        },
                        "scheduler": {
                            "frequency": "Month",
                            "interval": 1
                        },
                        "name": "RunSampleHiveActivity",
                        "linkedServiceName": "[variables('hdInsightOnDemandLinkedServiceName')]"
                    }
                    ],
                    "start": "2016-10-01T00:00:00Z",
                    "end": "2016-10-02T00:00:00Z",
                    "isPaused": false
                }
            }
            ]
        }
        ]
    }

> [AZURE.NOTE] Erőforrás-kezelő sablon lássunk még egy példát talál létrehozása az Azure adatok gyári [oktatóprogram: hozzon létre egy folyamat másolás tevékenység, erőforrás-kezelő Azure-sablon segítségével](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md).  

## <a name="parameters-json"></a>Paraméterek JSON 
Az erőforrás-kezelő Azure-sablon paramétereket tartalmazó **ADFTutorialARM-Parameters.json** nevű JSON-fájl létrehozása.  

> [AZURE.IMPORTANT] Adja meg a paraméter fájl nevét és a **storageAccountName** és **storageAccountKey** paraméterekkel Azure tároló fiókjának billentyűt. 

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "storageAccountName": {
                "value": "<Name of your Azure Storage account>"
            },
            "storageAccountKey": {
                "value": "<Key of your Azure Storage account>"
            },
            "blobContainer": {
                "value": "adfgetstarted"
            },
            "inputBlobFolder": {
                "value": "inputdata"
            },
            "inputBlobName": {
                "value": "input.log"
            },
            "outputBlobFolder": {
                "value": "partitioneddata"
            },
            "hiveScriptFolder": {
                "value": "script"
            },
            "hiveScriptFile": {
                "value": "partitionweblogs.hql"
            }
        }
    }

> [AZURE.IMPORTANT] Előfordulhat, hogy külön paraméter JSON fájlok fejlesztése, tesztelése és gyártási környezetek azonos adatokat gyári JSON sablonra használhatja. Egy Power parancsprogram használatával automatizálhatja a központi telepítés Data Factory szervezetek ezen környezetekben. 

## <a name="create-data-factory"></a>Adatok gyári létrehozása

1. Indítsa el a **Azure Powershellt** , és futtassa a következő parancsot: 
    - Futtatása `Login-AzureRmAccount` , és adja meg a felhasználónevet és jelszót, jelentkezzen be az Azure portálra.  
    - Futtatása `Get-AzureRmSubscription` ehhez a fiókhoz az előfizetések megtekintése.
    - Futtatása `Get-AzureRmSubscription -SubscriptionName <SUBSCRIPTION NAME> | Set-AzureRmContext` kattintva jelölje ki a használni kívánt előfizetést. Az előfizetés ugyanaz, mint az Azure-portálon használt kell lennie.
1. A következő parancsot a Data Factory szervezetek, az erőforrás-kezelő sablonnal üzembe létrehozott az 1. 

        New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile C:\ADFGetStarted\ADFTutorialARM.json -TemplateParameterFile C:\ADFGetStarted\ADFTutorialARM-Parameters.json

## <a name="monitor-pipeline"></a>Monitor folyamat
 
1.  Az [Azure portált](https://portal.azure.com/), kattintson bejelentkezés után **tallózással keresse meg** és választó **adatok gyárak**.
        ![Tallózás -> adatok gyárak](./media/data-factory-build-your-first-pipeline-using-arm/BrowseDataFactories.png)
2.  Kattintson az **Adatok gyárak** lap létrehozott adatok gyári (**TutorialFactoryARM**).   
2.  Az adatok factory- **Adatok gyári** lap kattintson a **Diagram**gombra.
        ![Diagram csempe](./media/data-factory-build-your-first-pipeline-using-arm/DiagramTile.png)
4.  A **Diagram nézetben**című témakörben áttekintést a folyamatok, és ebben az oktatóanyagban használt adatkészleteket.
    
    ![Diagram nézetben](./media/data-factory-build-your-first-pipeline-using-arm/DiagramView.png) 
8. A Diagram nézetben kattintson duplán az adatkészlet **AzureBlobOutput**. Láthatja, hogy a szeletet, amely jelenleg feldolgozása.

    ![Adatkészlet](./media/data-factory-build-your-first-pipeline-using-arm/AzureBlobOutput.png)
9. Feldolgozási befejezése után a szeletet **kész** állapotban látni. Az igény szerinti HDInsight fürt kibocsátása rendszerint valamikor vesz igénybe (körülbelül 20 perc). Ezért várjon **körülbelül 30 percig** a szeletet feldolgozása érvénybe a folyamat.

    ![Adatkészlet](./media/data-factory-build-your-first-pipeline-using-arm/SliceReady.png) 
10. Ha a szeletet **kész** állapotban van, ellenőrizze, hogy a blob-tárolóban a kimeneti adatokhoz a **adfgetstarted** tároló **partitioneddata** mappájában.  

Lásd: a [Monitor adatkészleteket, és a folyamat](data-factory-monitor-manage-pipelines.md) kapcsolatos tudnivalókat figyelheti a folyamat és adatkészleteket Azure portál rögzítéséhez használatával létrehozott, ebben az oktatóanyagban.

Az adatok folyamatok figyelése a Monitor és App kezelése is használhatja. Lásd: [Monitor és figyelése alkalmazással Azure Data Factory folyamatok kezelése](data-factory-monitor-manage-app.md) további információt az alkalmazás használatáról. 

> [AZURE.IMPORTANT] A bemeneti fájl kap törlésekor a szeletet importálni. Ezért, ha azt szeretné, futtassa újra a szeletet, és végezze el ismét az oktatóanyagot, a beviteli fájl feltöltése (input.log) a adfgetstarted tároló inputdata mappájába.

## <a name="data-factory-entities-in-the-template"></a>Adatok gyári személyek a sablonban
### <a name="define-data-factory"></a>Adatok gyári meghatározása
Adatok gyár definiálása az erőforrás-kezelő sablon az alábbi példa látható módon:  

    "resources": [
    {
        "name": "[variables('dataFactoryName')]",
        "apiVersion": "2015-10-01",
        "type": "Microsoft.DataFactory/datafactories",
        "location": "West US"
    }

A dataFactoryName határozható meg: 
      
      "dataFactoryName": "[concat('HiveTransformDF', uniqueString(resourceGroup().id))]",

Egy egyedi karakterlánc alapján az erőforrás csoport azonosítója.  

### <a name="defining-data-factory-entities"></a>Adatok gyári személyek meghatározása
A következő adatok gyári vállalkozások a JSON sablonban meghatározása: 

- [Azure csatolt tárhelyszolgáltatáshoz](#azure-storage-linked-service)
- [HDInsight igény szerinti csatolt szolgáltatás](#hdinsight-on-demand-linked-service)
- [Azure blob-beviteli adatkészlet](#azure-blob-input-dataset)
- [Azure blob-kimeneti adatkészlet](#azure-blob-output-dataset)
- [Adatok folyamat egy másolatot a tevékenységhez](#data-pipeline)

#### <a name="azure-storage-linked-service"></a>Azure csatolt tárhelyszolgáltatáshoz
Akkor adja meg a nevét és billentyűt az Azure tárterület-fiók ebben a szakaszban. Kapcsolatos további tudnivalók: [Azure tároló csatolt szolgáltatás](data-factory-azure-blob-connector.md#azure-storage-linked-service) használta az Azure csatolt tárhelyszolgáltatáshoz JSON tulajdonságait. 

      {
        "type": "linkedservices",
        "name": "[variables('azureStorageLinkedServiceName')]",
        "dependsOn": [
          "[variables('dataFactoryName')]"
        ],
        "apiVersion": "2015-10-01",
        "properties": {
          "type": "AzureStorage",
          "description": "Azure Storage linked service",
          "typeProperties": {
            "connectionString": "[concat('DefaultEndpointsProtocol=https;AccountName=',parameters('storageAccountName'),';AccountKey=',parameters('storageAccountKey'))]"
          }
        }
      }

A **connectionString** a storageAccountName és storageAccountKey paramétereket használ. Konfigurációs fájl használatával átadott paraméterértékek értékeket. A definíció is használja a változók: azureStroageLinkedService és dataFactoryName határozza meg a sablont. 
    
#### <a name="hdinsight-on-demand-linked-service"></a>HDInsight igény szerinti csatolt szolgáltatás
Lásd: [számítja ki a csatolt szolgáltatások](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) cikk igény szerinti csatolt szolgáltatásainak HDInsight definiálásakor JSON tulajdonságok kapcsolatban további tájékoztatást.  

      {
        "type": "linkedservices",
        "name": "[variables('hdInsightOnDemandLinkedServiceName')]",
        "dependsOn": [
          "[variables('dataFactoryName')]"
        ],
        "apiVersion": "2015-10-01",
        "properties": {
          "type": "HDInsightOnDemand",
          "typeProperties": {
            "clusterSize": 1,
            "version": "3.2",
            "timeToLive": "00:05:00",
            "osType": "windows",
            "linkedServiceName": "[variables('azureStorageLinkedServiceName')]"
          }
        }
      }

Vegye figyelembe az alábbiakat: 

- Az adatok gyári hoz a **Windows-alapú** HDInsight fürtre meg a fenti JSON. Azt **Linux-alapú** HDInsight fürt létrehozása is lehet. [Igény szerinti HDInsight csatolt szolgáltatás](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) információt talál. 
- Az igény szerinti HDInsight fürt használata helyett használhatja is **saját HDInsight fürt** . A részletekért [HDInsight csatolt szolgáltatás](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) témakörben olvashat.
- A HDInsight fürt **alapértelmezett tároló** a JSON (**linkedServiceName**) megadott blob-tárolóhoz hoz létre. HDInsight nem törli a tároló törlésekor a fürt. Ez a jelenség szándékosan van így. Igény szerinti csatolt HDInsight szolgáltatáshoz egy fürtre hoz létre, minden alkalommal, amikor egy szeletet kell dolgozható fel, kivéve, ha van egy meglévő HDInsight live fürtre (**élettartam**), és a program törli, ha elkészült a feldolgozása.

    További szeletek feldolgozása, mint az Azure blob-tárolóhoz sok tárolók látható. Ha nincs szüksége rájuk a feladatok hibaelhárítási, érdemes törölheti őket a tárhely költség csökkentése érdekében. Ezek a tárolók azoknak a hajtsa végre a minta: "adf**yourdatafactoryname**-**linkedservicename**- datetimestamp". Például a [Microsoft tároló Explorer](http://storageexplorer.com/) eszközök segítségével az Azure blob-tárolóban lévő tárolók törlése.

[Igény szerinti HDInsight csatolt szolgáltatás](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) információt talál.



#### <a name="azure-blob-input-dataset"></a>Azure blob-beviteli adatkészlet
Megadhatja, hogy blob-tárolóhoz, a mappa és a bemeneti adatokat tartalmazó fájl nevét. Kapcsolatos további tudnivalók: [Azure Blob-adatkészlet tulajdonságok](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties) használta az Azure Blob-adatkészlet JSON tulajdonságait. 

      {
        "type": "datasets",
        "name": "[variables('blobInputDatasetName')]",
        "dependsOn": [
          "[variables('dataFactoryName')]",
          "[variables('azureStorageLinkedServiceName')]"
        ],
        "apiVersion": "2015-10-01",
        "properties": {
          "type": "AzureBlob",
          "linkedServiceName": "[variables('azureStorageLinkedServiceName')]",
          "typeProperties": {
            "fileName": "[parameters('inputBlobName')]",
            "folderPath": "[concat(parameters('blobContainer'), '/', parameters('inputBlobFolder'))]",
            "format": {
              "type": "TextFormat",
              "columnDelimiter": ","
            }
          },
          "availability": {
            "frequency": "Month",
            "interval": 1
          },
          "external": true
        }
      }

Ez a meghatározás használja a következő paraméterek paraméter sablon: blobContainer, inputBlobFolder, és inputBlobName. 

#### <a name="azure-blob-output-dataset"></a>Azure Blob-kimeneti adatkészlet
Megadhatja, hogy blob-tárolóhoz és a kimeneti adatokat tároló mappa nevét. Kapcsolatos további tudnivalók: [Azure Blob-adatkészlet tulajdonságok](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties) használta az Azure Blob-adatkészlet JSON tulajdonságait.  

      {
        "type": "datasets",
        "name": "[variables('blobOutputDatasetName')]",
        "dependsOn": [
          "[variables('dataFactoryName')]",
          "[variables('azureStorageLinkedServiceName')]"
        ],
        "apiVersion": "2015-10-01",
        "properties": {
          "type": "AzureBlob",
          "linkedServiceName": "[variables('azureStorageLinkedServiceName')]",
          "typeProperties": {
            "folderPath": "[concat(parameters('blobContainer'), '/', parameters('outputBlobFolder'))]",
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

Ez a meghatározás használja a következő paraméterek a paraméter sablonban definiált: blobContainer és outputBlobFolder. 

#### <a name="data-pipeline"></a>Adatok folyamat
Egy folyamat, amely egy igény szerinti Azure hdinsight szolgáltatáshoz fürt struktúra parancsfájl futtatásával adatok átalakítása megadása Ebben a példában egy folyamat használta JSON elemeit [Folyamat JSON](data-factory-create-pipelines.md#pipeline-json) talál. 

    {
        "type": "datapipelines",
        "name": "[variables('pipelineName')]",
        "dependsOn": [
          "[variables('dataFactoryName')]",
          "[variables('azureStorageLinkedServiceName')]",
          "[variables('hdInsightOnDemandLinkedServiceName')]",
          "[variables('blobInputDatasetName')]",
          "[variables('blobOutputDatasetName')]"
        ],
        "apiVersion": "2015-10-01",
        "properties": {
          "description": "Pipeline that transforms data using Hive script.",
          "activities": [
            {
              "type": "HDInsightHive",
              "typeProperties": {
                "scriptPath": "[concat(parameters('blobContainer'), '/', parameters('hiveScriptFolder'), '/', parameters('hiveScriptFile'))]",
                "scriptLinkedService": "[variables('azureStorageLinkedServiceName')]",
                "defines": {
                  "inputtable": "[concat('wasb://', parameters('blobContainer'), '@', parameters('storageAccountName'), '.blob.core.windows.net/', parameters('inputBlobFolder'))]",
                  "partitionedtable": "[concat('wasb://', parameters('blobContainer'), '@', parameters('storageAccountName'), '.blob.core.windows.net/', parameters('outputBlobFolder'))]"
                }
              },
              "inputs": [
                {
                  "name": "[variables('blobInputDatasetName')]"
                }
              ],
              "outputs": [
                {
                  "name": "[variables('blobOutputDatasetName')]"
                }
              ],
              "policy": {
                "concurrency": 1,
                "retry": 3
              },
              "scheduler": {
                "frequency": "Month",
                "interval": 1
              },
              "name": "RunSampleHiveActivity",
              "linkedServiceName": "[variables('hdInsightOnDemandLinkedServiceName')]"
            }
          ],
          "start": "2016-10-01T00:00:00Z",
          "end": "2016-10-02T00:00:00Z",
          "isPaused": false
        }
      }

## <a name="reuse-the-template"></a>A sablon újrafelhasználása 
Az oktatóprogram során létrehozott egy sablont, definiálása Data Factory vállalatok és a paraméterek értékeit átadása egy sablont. Ugyanazt a sablont használják a telepítéshez használni Data Factory egységek különböző környezetekben, környezetben paraméter fájl létrehozása, és használni, hogy a környezetben való telepítésekor.     

Példa:  

    New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFTutorialARM.json -TemplateParameterFile ADFTutorialARM-Parameters-Dev.json

    New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFTutorialARM.json -TemplateParameterFile ADFTutorialARM-Parameters-Test.json

    New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFTutorialARM.json -TemplateParameterFile ADFTutorialARM-Parameters-Production.json

Figyelje meg, hogy az első parancs paraméter fájlt használja, a fejlesztői környezet, a próba-környezetben másodikat és a harmadik egyet az az éles üzemi környezetben.  

Az ismétlődő tevékenységek végrehajtásához a sablon is felhasználhat. Például létre kell hoznia a sok adatok gyárak együtt, ugyanazon logika végrehajtása egy vagy több folyamatok, de egyes adatok gyári használja, más-más Azure tárolási és Azure SQL-adatbázis fiókok. Ebben az esetben használhatja ugyanazt a sablont a azonos környezetben (fejlesztők, vizsgálat vagy gyártási) különböző paraméter fájlokkal adatok gyárak létrehozásához. 

## <a name="resource-manager-template-for-creating-a-gateway"></a>Erőforrás-kezelő sablon az átjáró létrehozása
Az alábbiakban az erőforrás-kezelő űrlapsablonok logikai az átjáró létrehozásához a a Vissza gombra. Az átjáró telepítése a helyszíni számítógépén vagy Azure IaaS virtuális, és regisztrálja az átjárót egy billentyűje segítségével adatokat gyári szolgáltatással. Lásd: az [adatok helyszíni és felhőbeli közötti áthelyezése](data-factory-move-data-between-onprem-and-cloud.md) további információt.

    {
        "contentVersion": "1.0.0.0",
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "parameters": {
        },
        "variables": {
            "dataFactoryName":  "GatewayUsingArmDF",
            "apiVersion": "2015-10-01",
            "singleQuote": "'"
        },
        "resources": [
            {
                "name": "[variables('dataFactoryName')]",
                "apiVersion": "[variables('apiVersion')]",
                "type": "Microsoft.DataFactory/datafactories",
                "location": "eastus",
                "resources": [
                    {
                        "dependsOn": [ "[concat('Microsoft.DataFactory/dataFactories/', variables('dataFactoryName'))]" ],
                        "type": "gateways",
                        "apiVersion": "[variables('apiVersion')]",
                        "name": "GatewayUsingARM",
                        "properties": {
                            "description": "my gateway"
                        }
                    }            
                ]
            }
        ]
    }

Ez a sablon létrehoz GatewayUsingArmDF nevű nevű az átjárók adatok gyár: GatewayUsingARM. 

## <a name="see-also"></a>Lásd még:
| A témakör | Leírás |
| :---- | :---- |
| [A tevékenységekre vonatkozó adatok transzformációt hajt végre.](data-factory-data-transformation-activities.md) | Ebben a cikkben egy tevékenységlista adatainak átalakítása (például az oktatóprogram használt HDInsight-struktúra átalakítása) Azure Data Factory által támogatott. |
| [Ütemezés- és végrehajtása](data-factory-scheduling-and-execution.md) | Ez a cikk ismerteti az Azure Data Factory alkalmazásmodell ütemezési és a végrehajtás szempontjait. |
| [Folyamatok](data-factory-create-pipelines.md) | Ez a cikk segít megérteni a folyamatok és Azure Data Factory és használatuk összeállításához végpontok közötti adatalapú munkafolyamatok forgatókönyv vagy üzleti tevékenységek. |
| [Adatkészletek](data-factory-create-datasets.md) | Ez a cikk segít megérteni az Azure Data Factory adatkészleteket.
| [Figyelésére és figyelése alkalmazással folyamatok kezelése](data-factory-monitor-manage-app.md) | Ez a cikk leírja, hogy miként figyelheti, kezelése és használata a figyelő és felügyeleti alkalmazás folyamatok hibakeresési. 

  

