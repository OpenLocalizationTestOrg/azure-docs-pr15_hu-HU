<properties
    pageTitle="Oktatóprogram: Hozzon létre egy erőforrás-kezelő sablonnal folyamat |} Microsoft Azure"
    description="Ebben az oktatóanyagban létrehozása az Azure Data Factory folyamat egy másolatot a tevékenységhez a Azure erőforrás-kezelő sablon használatával."
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
    ms.topic="get-started-article"
    ms.date="10/10/2016"
    ms.author="spelluru"/>

# <a name="tutorial-create-a-pipeline-with-copy-activity-using-azure-resource-manager-template"></a>Oktatóprogram: Hozzon létre egy folyamat másolás tevékenység, erőforrás-kezelő Azure-sablon segítségével
> [AZURE.SELECTOR]
- [Áttekintés és a vonatkozó követelmények](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
- [Másolja a varázsló](data-factory-copy-data-wizard-tutorial.md)
- [Azure portál](data-factory-copy-activity-tutorial-using-azure-portal.md)
- [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
- [A PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
- [Erőforrás-kezelő Azure-sablon](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
- [REST API-VAL](data-factory-copy-activity-tutorial-using-rest-api.md)
- [.NET API](data-factory-copy-activity-tutorial-using-dotnet-api.md)


Ebből az oktatóanyagból megtudhatja, hogyan hozhat létre, és figyelemmel az Azure adatok gyári erőforrás-kezelő Azure-sablon segítségével. Az adatok gyári folyamat Azure Blob-tárolóhoz lévő adatokat másolja Azure SQL-adatbázishoz.

## <a name="prerequisites"></a>Előfeltételek
- Hajtsa végre a [oktatóanyag – áttekintés és előfeltételekről](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) , és hajtsa végre a **előfeltétel** .
- Kövesse az Azure PowerShell legújabb verziójának telepítése a számítógépen [Telepítse és állítsa be a Azure PowerShell módjáról](../powershell-install-configure.md) a cikk utasításait. Ebben az oktatóanyagban használatával PowerShell üzembe Data Factory szervezetek. 
- (nem kötelező) Lásd: [Azure erőforrás-kezelő sablonok létrehozása](../resource-group-authoring-templates.md) , ha többet szeretne tudni az Azure erőforrás-kezelő sablonok.


## <a name="in-this-tutorial"></a>Ebben az oktatóanyagban

Ebben az oktatóanyagban adatok gyár a következő személyekkel Data Factory létrehozása:

Személy | Leírás  
------ | ----------- 
Azure csatolt tárhelyszolgáltatáshoz | Azure tároló fiókja csatol adatokat gyári. Azure tárhely a forrás adatokat tároló pedig Azure SQL-adatbázis a gyűjtő adatokat tároló a Másolás tevékenység az oktatóprogram során. Adja meg a tárterület-fiókot, a Másolás tevékenységhez tartozó bemeneti adatokat tartalmazó. 
Azure SQL-adatbázishoz kapcsolva szolgáltatás| Az Azure SQL-adatbázis csatolása az adatok gyári. Adja meg a Másolás tevékenység kimeneti adatokat tároló Azure SQL-adatbázishoz. 
Azure Blob-beviteli adatkészlet | Az Azure csatolt tárhelyszolgáltatáshoz hivatkozik. A csatolt szolgáltatás Azure tároló fiók hivatkozik, és az Azure Blob-adatkészlet adja meg a tároló, a mappa és a fájlnevet, amely a bemeneti adatok tárolására. 
Azure SQL-kimeneti adatkészlet | A csatolt Azure SQL-szolgáltatás hivatkozik. A csatolt az SQL Azure szolgáltatás egy Azure SQL-kiszolgáló hivatkozik, és az Azure SQL-adatkészlet neve a kimeneti adatokat tartalmazó táblát. 
Adatok folyamat | A folyamat tartalmaz egy tevékenység típusának másolja a vágólapra, hogy mi az Azure blob-adatkészlet-bemenetként és az Azure SQL-adatkészlet-kimenetként. A Másolás tevékenység adatait másolja az Azure blob az Azure SQL-adatbázis táblájához.  

Adatok gyár beállíthatja, hogy egy vagy több folyamatok. Egy folyamat beállíthatja, hogy egy vagy több tevékenységek rajta. Tevékenységek két típusa van: [adatok mozgását tevékenységeket](data-factory-data-movement-activities.md) és [adatok átalakítása tevékenységeket](data-factory-data-transformation-activities.md). Ebben az oktatóanyagban hoz létre egy folyamat egy tevékenység (Másolás tevékenység).

![Másolja az Azure Blob Azure SQL-adatbázishoz](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/CopyBlob2SqlDiagram.png) 

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

Névvel ellátott **ADFCopyTutorialARM.json** **C:\ADFGetStarted** mappában, a következő tartalommal JSON-fájl létrehozása:


    {
        "contentVersion": "1.0.0.0",
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "parameters": {
          "storageAccountName": { "type": "string", "metadata": { "description": "Name of the Azure storage account that contains the data to be copied." } },
          "storageAccountKey": { "type": "securestring", "metadata": { "description": "Key for the Azure storage account." } },
          "sourceBlobContainer": { "type": "string", "metadata": { "description": "Name of the blob container in the Azure Storage account." } },
          "sourceBlobName": { "type": "string", "metadata": { "description": "Name of the blob in the container that has the data to be copied to Azure SQL Database table" } },
          "sqlServerName": { "type": "string", "metadata": { "description": "Name of the Azure SQL Server that will hold the output/copied data." } },
          "databaseName": { "type": "string", "metadata": { "description": "Name of the Azure SQL Database in the Azure SQL server." } },
          "sqlServerUserName": { "type": "string", "metadata": { "description": "Name of the user that has access to the Azure SQL server." } },
          "sqlServerPassword": { "type": "securestring", "metadata": { "description": "Password for the user." } },
          "targetSQLTable": { "type": "string", "metadata": { "description": "Table in the Azure SQL Database that will hold the copied data." } 
          } 
        },
        "variables": {
          "dataFactoryName": "[concat('AzureBlobToAzureSQLDatabaseDF', uniqueString(resourceGroup().id))]",
          "azureSqlLinkedServiceName": "AzureSqlLinkedService",
          "azureStorageLinkedServiceName": "AzureStorageLinkedService",
          "blobInputDatasetName": "BlobInputDataset",
          "sqlOutputDatasetName": "SQLOutputDataset",
          "pipelineName": "Blob2SQLPipeline"
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
                "name": "[variables('azureSqlLinkedServiceName')]",
                "dependsOn": [
                  "[variables('dataFactoryName')]"
                ],
                "apiVersion": "2015-10-01",
                "properties": {
                  "type": "AzureSqlDatabase",
                  "description": "Azure SQL linked service",
                  "typeProperties": {
                    "connectionString": "[concat('Server=tcp:',parameters('sqlServerName'),'.database.windows.net,1433;Database=', parameters('databaseName'), ';User ID=',parameters('sqlServerUserName'),';Password=',parameters('sqlServerPassword'),';Trusted_Connection=False;Encrypt=True;Connection Timeout=30')]"
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
                  "structure": [
                    {
                      "name": "Column0",
                      "type": "String"
                    },
                    {
                      "name": "Column1",
                      "type": "String"
                    }
                  ],
                  "typeProperties": {
                    "folderPath": "[concat(parameters('sourceBlobContainer'), '/')]",
                    "fileName": "[parameters('sourceBlobName')]",
                    "format": {
                      "type": "TextFormat",
                      "columnDelimiter": ","
                    }
                  },
                  "availability": {
                    "frequency": "Day",
                    "interval": 1
                  },
                  "external": true
                }
              },
              {
                "type": "datasets",
                "name": "[variables('sqlOutputDatasetName')]",
                "dependsOn": [
                  "[variables('dataFactoryName')]",
                  "[variables('azureSqlLinkedServiceName')]"
                ],
                "apiVersion": "2015-10-01",
                "properties": {
                  "type": "AzureSqlTable",
                  "linkedServiceName": "[variables('azureSqlLinkedServiceName')]",
                  "structure": [
                    {
                      "name": "FirstName",
                      "type": "String"
                    },
                    {
                      "name": "LastName",
                      "type": "String"
                    }
                  ],
                  "typeProperties": {
                    "tableName": "[parameters('targetSQLTable')]"
                  },
                  "availability": {
                    "frequency": "Day",
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
                  "[variables('azureSqlLinkedServiceName')]",
                  "[variables('blobInputDatasetName')]",
                  "[variables('sqlOutputDatasetName')]"
                ],
                "apiVersion": "2015-10-01",
                "properties": {
                  "activities": [
                    {
                      "name": "CopyFromAzureBlobToAzureSQL",
                      "description": "Copy data frm Azure blob to Azure SQL",
                      "type": "Copy",
                      "inputs": [
                        {
                          "name": "[variables('blobInputDatasetName')]"
                        }
                      ],
                      "outputs": [
                        {
                          "name": "[variables('sqlOutputDatasetName')]"
                        }
                      ],
                      "typeProperties": {
                        "source": {
                          "type": "BlobSource"
                        },
                        "sink": {
                          "type": "SqlSink",
                          "sqlWriterCleanupScript": "$$Text.Format('DELETE FROM {0}', 'emp')"
                        },
                        "translator": {
                          "type": "TabularTranslator",
                          "columnMappings": "Column0:FirstName,Column1:LastName"
                        }
                      },
                      "Policy": {
                        "concurrency": 1,
                        "executionPriorityOrder": "NewestFirst",
                        "retry": 3,
                        "timeout": "01:00:00"
                      }
                    }
                  ],
                  "start": "2016-10-02T00:00:00Z",
                  "end": "2016-10-03T00:00:00Z"
                }
              }
            ]
          }
        ]
      }

## <a name="parameters-json"></a>Paraméterek JSON 
Az erőforrás-kezelő Azure-sablon paramétereket tartalmazó **ADFCopyTutorialARM-Parameters.json** nevű JSON-fájl létrehozása. 

> [AZURE.IMPORTANT] Adja meg a nevét és Azure tároló fiókjának **storageAccountName** és **storageAccountKey** paraméterekkel billentyűt.  

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
        "contentVersion": "1.0.0.0",
        "parameters": { 
            "storageAccountName": { "value": "<Name of the Azure storage account>"  },
            "storageAccountKey": {
                "value": "<Key for the Azure storage account>"
            },
            "sourceBlobContainer": { "value": "adftutorial" },
            "sourceBlobName": { "value": "emp.txt" },
            "sqlServerName": { "value": "<Name of the Azure SQL server>" },
            "databaseName": { "value": "<Name of the Azure SQL database>" },
            "sqlServerUserName": { "value": "<Name of the user who has access to the Azure SQL database>" },
            "sqlServerPassword": { "value": "<password for the user>" },
            "targetSQLTable": { "value": "emp" }
        }
    }

> [AZURE.IMPORTANT] Előfordulhat, hogy külön paraméter JSON fájlok fejlesztése, tesztelése és gyártási környezetek azonos adatokat gyári JSON sablonra használhatja. Egy Power parancsprogram használatával automatizálhatja a központi telepítés Data Factory szervezetek ezen környezetekben.  

## <a name="create-data-factory"></a>Adatok gyári létrehozása
1. Indítsa el a **Azure Powershellt** , és futtassa a következő parancsot:
    - Futtatása `Login-AzureRmAccount` , és adja meg a felhasználónevet és jelszót, jelentkezzen be az Azure portálra.  
    - Futtatása `Get-AzureRmSubscription` ehhez a fiókhoz az előfizetések megtekintése.
    - Futtatása `Get-AzureRmSubscription -SubscriptionName <SUBSCRIPTION NAME> | Set-AzureRmContext` kattintva jelölje ki a használni kívánt előfizetést. 
2. A következő parancsot a Data Factory szervezetek, az erőforrás-kezelő sablonnal üzembe létrehozott az 1.

        New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile C:\ADFGetStarted\ADFCopyTutorialARM.json -TemplateParameterFile C:\ADFGetStarted\ADFCopyTutorialARM-Parameters.json

## <a name="monitor-pipeline"></a>Monitor folyamat
1. Az [Azure portál](https://portal.azure.com) az Azure-fiók használatával jelentkezzen be.
2. **Adatok gyárak** kattintson a bal oldali menüben (vagy) kattintson a **További szolgáltatások** , és kattintson az **adatok gyárak** **ÜZLETIINTELLIGENCIA- + ANALYTICS** kategóriában.

    ![Az adatok gyárak menü](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/data-factories-menu.png)
3. Az **adatok gyárak** lapon keresse meg, és keresse meg az adatok gyári. 

    ![Adatok gyári keresése](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/search-for-data-factory.png)  
4. Kattintson az Azure adatok gyári. Az adatok gyári kezdőlapjának megtekintése

    ![Az adatok factory kezdőlapján](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/data-factory-home-page.png)  
5. Kattintson az adatok gyári diagram megtekintheti a **Diagram** csempére.

    ![Az adatok gyári diagram nézetben](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/data-factory-diagram-view.png)
6. A diagram nézetben kattintson duplán az adatkészlet **SQLOutputDataset**. A szeletek állapotának megtekintése A másolási művelet befejezése után állapotát állította, hogy **készen áll**.

    ![Kimeneti szeletet kész állapotban](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/output-slice-ready.png)
7. Amikor a szeletet **kész** állapotban van, győződjön meg arról, hogy az adatokat másolja a **Vállalati projektirányítási** táblázat az Azure SQL-adatbázisban.

Lásd: a [Monitor adatkészleteket, és a folyamat](data-factory-monitor-manage-pipelines.md) kapcsolatos tudnivalókat figyelheti a folyamat és adatkészleteket Azure portál rögzítéséhez használatával létrehozott, ebben az oktatóanyagban.

Az adatok folyamatok figyelése a Monitor és App kezelése is használhatja. Lásd: [Monitor és figyelése alkalmazással Azure Data Factory folyamatok kezelése](data-factory-monitor-manage-app.md) további információt az alkalmazás használatáról.


## <a name="data-factory-entities-in-the-template"></a>Adatok gyári személyek a sablonban

### <a name="define-data-factory"></a>Adatok gyári meghatározása
Adatok gyár definiált az erőforrás manager sablon az alábbi példa látható módon:  

    "resources": [
    {
        "name": "[variables('dataFactoryName')]",
        "apiVersion": "2015-10-01",
        "type": "Microsoft.DataFactory/datafactories",
        "location": "West US"
    }

A dataFactoryName határozható meg: 
      
    "dataFactoryName": "[concat('AzureBlobToAzureSQLDatabaseDF', uniqueString(resourceGroup().id))]"

Az erőforrás csoport azonosító alapján egyedi karakterlánc  

### <a name="defining-data-factory-entities"></a>Adatok gyári személyek meghatározása
A következő adatok gyári vállalkozások a JSON sablonban meghatározása: 

1. [Azure csatolt tárhelyszolgáltatáshoz](#azure-storage-linked-service)
2. [Azure SQL csatolt szolgáltatás](#azure-sql-database-linked-service)
3. [Azure blob-adatkészlet](#azure-blob-dataset)
4. [Azure SQL-adatkészlet](#azure-sql-dataset)
5. [Adatok folyamat egy másolatot a tevékenységhez](#data-pipeline)

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

A connectionString a storageAccountName és storageAccountKey paramétereket használ. Konfigurációs fájl használatával átadott paraméterértékek értékeket. A definíció is használja a változók: azureStroageLinkedService és dataFactoryName határozza meg a sablont. 
    
#### <a name="azure-sql-database-linked-service"></a>Azure SQL-adatbázishoz kapcsolva szolgáltatás
Az Azure SQL-kiszolgáló neve, az adatbázis neve, a felhasználónév és a jelszó megadása ebben a szakaszban. Kapcsolatos további tudnivalók: [az SQL Azure szolgáltatás csatolt](data-factory-azure-sql-connector.md#azure-sql-linked-service-properties) használta az Azure SQL-csatolt szolgáltatásainak JSON tulajdonságait.  

    {
        "type": "linkedservices",
        "name": "[variables('azureSqlLinkedServiceName')]",
        "dependsOn": [
          "[variables('dataFactoryName')]"
        ],
        "apiVersion": "2015-10-01",
        "properties": {
            "type": "AzureSqlDatabase",
            "description": "Azure SQL linked service",
            "typeProperties": {
                "connectionString": "[concat('Server=tcp:',parameters('sqlServerName'),'.database.windows.net,1433;Database=', parameters('databaseName'), ';User ID=',parameters('sqlServerUserName'),';Password=',parameters('sqlServerPassword'),';Trusted_Connection=False;Encrypt=True;Connection Timeout=30')]"
            }
        }
    }

A connectionString SQLKiszolgálónév, adatbázisnév, sqlServerUserName és értékük át az konfigurációs fájl használatával sqlServerPassword paramétereket használ. A definíció is használja az alábbi változók a sablonból: azureSqlLinkedServiceName, dataFactoryName.

#### <a name="azure-blob-dataset"></a>Azure blob-adatkészlet
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
            "structure": [
            {
                "name": "Column0",
                "type": "String"
            },
            {
                "name": "Column1",
                "type": "String"
            }
            ],
            "typeProperties": {
                "folderPath": "[concat(parameters('sourceBlobContainer'), '/')]",
                "fileName": "[parameters('sourceBlobName')]",
                "format": {
                    "type": "TextFormat",
                    "columnDelimiter": ","
                }
            },
            "availability": {
                "frequency": "Day",
                "interval": 1
            },
            "external": true
        }
    }

#### <a name="azure-sql-dataset"></a>Azure SQL-adatkészlet
Annak a táblának a nevére az Azure SQL-adatbázishoz, amely a másolt adatok a Azure Blob-tárolóból ad meg. Kapcsolatos további tudnivalók: [Azure SQL-adatkészlet tulajdonságok](data-factory-azure-sql-connector.md#azure-sql-dataset-type-properties) használta az Azure SQL-adatkészlet JSON tulajdonságait. 

    {
        "type": "datasets",
        "name": "[variables('sqlOutputDatasetName')]",
        "dependsOn": [
            "[variables('dataFactoryName')]",
            "[variables('azureSqlLinkedServiceName')]"
        ],
        "apiVersion": "2015-10-01",
        "properties": {
            "type": "AzureSqlTable",
            "linkedServiceName": "[variables('azureSqlLinkedServiceName')]",
            "structure": [
            {
                "name": "FirstName",
                "type": "String"
            },
            {
                "name": "LastName",
                "type": "String"
            }
            ],
            "typeProperties": {
                "tableName": "[parameters('targetSQLTable')]"
            },
            "availability": {
                "frequency": "Day",
                "interval": 1
            }
        }
    }

#### <a name="data-pipeline"></a>Adatok folyamat
Egy folyamat, amely másolja az adatokat az Azure blob-adatkészlet az Azure SQL-adatkészlet megadása Ebben a példában egy folyamat használta JSON elemeit [Folyamat JSON](data-factory-create-pipelines.md#pipeline-json) talál. 

    {
        "type": "datapipelines",
        "name": "[variables('pipelineName')]",
        "dependsOn": [
            "[variables('dataFactoryName')]",
            "[variables('azureStorageLinkedServiceName')]",
            "[variables('azureSqlLinkedServiceName')]",
            "[variables('blobInputDatasetName')]",
            "[variables('sqlOutputDatasetName')]"
        ],
        "apiVersion": "2015-10-01",
        "properties": {
            "activities": [
            {
                "name": "CopyFromAzureBlobToAzureSQL",
                "description": "Copy data frm Azure blob to Azure SQL",
                "type": "Copy",
                "inputs": [
                {
                    "name": "[variables('blobInputDatasetName')]"
                }
                ],
                "outputs": [
                {
                    "name": "[variables('sqlOutputDatasetName')]"
                }
                ],
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "sink": {
                        "type": "SqlSink",
                        "sqlWriterCleanupScript": "$$Text.Format('DELETE FROM {0}', 'emp')"
                    },
                    "translator": {
                        "type": "TabularTranslator",
                        "columnMappings": "Column0:FirstName,Column1:LastName"
                    }
                },
                "Policy": {
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 3,
                    "timeout": "01:00:00"
                }
            }
            ],
            "start": "2016-10-02T00:00:00Z",
            "end": "2016-10-03T00:00:00Z"
        }
    }

## <a name="reuse-the-template"></a>A sablon újrafelhasználása 
Az oktatóprogram során létrehozott egy sablont, definiálása Data Factory vállalatok és a paraméterek értékeit átadása egy sablont. A folyamat adatokat másolja Azure tárterület-fiókból egy megadott paramétereket keresztül Azure SQL-adatbázishoz. Ugyanazt a sablont használják a telepítéshez használni Data Factory egységek különböző környezetekben, környezetben paraméter fájl létrehozása, és használni, hogy a környezetben való telepítésekor.     

Példa:  

    New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFCopyTutorialARM.json -TemplateParameterFile ADFCopyTutorialARM-Parameters-Dev.json

    New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFCopyTutorialARM.json -TemplateParameterFile ADFCopyTutorialARM-Parameters-Test.json

    New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFCopyTutorialARM.json -TemplateParameterFile ADFCopyTutorialARM-Parameters-Production.json

Figyelje meg, hogy az első parancs paraméter fájlt használja, a fejlesztői környezet, a próba-környezetben másodikat és a harmadik egyet az az éles üzemi környezetben.  

Az ismétlődő tevékenységek végrehajtásához a sablon is felhasználhat. Például létre kell hoznia a sok adatok gyárak együtt, ugyanazon logika végrehajtása egy vagy több folyamatok, de egyes adatok gyári használja, más-más Azure tárolási és Azure SQL-adatbázis fiókok. Ebben az esetben használhatja ugyanazt a sablont a azonos környezetben (fejlesztők, vizsgálat vagy gyártási) különböző paraméter fájlokkal adatok gyárak létrehozásához.   

