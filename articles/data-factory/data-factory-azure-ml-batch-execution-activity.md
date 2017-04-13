<properties 
    pageTitle="Gépi tanulási tevékenységekhez |} Microsoft Azure" 
    description="Megtudhatja, hogy miként hozhat létre az Azure Data Factory és Azure gépi tanulási prediktív folyamatok létrehozása" 
    services="data-factory" 
    documentationCenter="" 
    authors="sharonlo101" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/06/2016" 
    ms.author="shlo"/>

# <a name="create-predictive-pipelines-using-azure-machine-learning-activities"></a>Azure gépi tanulási tevékenységek segítségével prediktív folyamatok létrehozása   
> [AZURE.SELECTOR]
[A struktúra](data-factory-hive-activity.md)  
[Malac](data-factory-pig-activity.md)  
[MapReduce](data-factory-map-reduce.md)  
[A folyamatos átvitelű Hadoop](data-factory-hadoop-streaming-activity.md)
[Gépi tanulási](data-factory-azure-ml-batch-execution-activity.md) 
[Tárolt eljárás](data-factory-stored-proc-activity.md)
[U-SQL nyelvben adatok tó Analytics](data-factory-usql-activity.md)
[egyéni .NET](data-factory-use-custom-activities.md)

## <a name="introduction"></a>– Bevezetés

[Azure gépi tanulási](https://azure.microsoft.com/documentation/services/machine-learning/) lehetővé teszi, hogy a összeállítása, tesztelése és az előrejelzési analytics-megoldások telepítése. Magas szintű szempontból akkor történik, az alábbi három lépésből áll: 

1. **Egy oktatás kísérlet létrehozása**. Megteheti ezt a lépést az Azure Machine Learning Studio segítségével. A Machine Learning studio egy olyan együttműködési vizuális fejlesztői környezet, képzése és a prediktív analytics-modellben oktatóanyag adatok vizsgálata használt.
2. **Átalakíthatja a cserélendő kísérlet**. Miután a modell van már képzett meglévő adatokkal, és készen áll új adatok pontszám használatával, előkészítése, és egyszerűsítheti a kísérlet az pontozási.
3. A **webes szolgáltatás üzembe**. A pontozási kísérlet Azure webes szolgáltatásként közzéteheti. Adatok küldése a modell a webes szolgáltatás végpontjának keresztül, és fogadása eredmény az előrejelzések céljából a modell.  

Azure Data Factory lehetővé teszi az eszközzel egyszerűen készíthet egy közzétett [Azure gépi tanulási] használó folyamatok[ azure-machine-learning] webszolgáltatás prediktív ábrázolja. [Azure Data Factory – bevezetés](data-factory-introduction.md) és [az első folyamat összeállítása](data-factory-build-your-first-pipeline.md) gyorsan – első lépések az Azure Data Factory szolgáltatás cikkekben olvashat bővebben. 

Használja a **Köteg végrehajtás tevékenység** -Azure Data Factory során, az előrejelzések kinagyíthatja a köteget az adatokat az Azure Machine Learning webszolgáltatás hívása. Lásd: [az Azure Machine Learning meghívása a köteg végrehajtás tevékenység használatával webszolgáltatás](#invoking-an-azure-ml-web-service-using-the-batch-execution-activity) szakaszban további információt.

Az idő múlásával a cserélendő modellekben, a kísérletek pontozási Azure Machine Learning kell kell retrained új beviteli adatkészleteket használatával. A Data Factory folyamat egy Azure Machine Learning modell is Újraépítés, hajtsa végre az alábbi lépéseket: 

1. Közzététel az oktatás kísérletet (nem prediktív kísérlet) egy webszolgáltatásból. Ezt a lépést az Azure Machine Learning Studio el kell végeznie, mint az előző helyzetben webszolgáltatás prediktív kísérlet elérhetővé szeretné.
2. Azure Machine Learning köteg végrehajtás tevékenység segítségével a oktatás kísérlet a webszolgáltatás hívása. Az Azure Machine Learning köteg végrehajtás tevékenység segítségével alapvetően oktatás webszolgáltatás és a pontozási webszolgáltatás hívása. 
  
Miután végzett a átképzés, frissítse az újonnan képzett modell a pontozási webszolgáltatás (prediktív kísérlet webszolgáltatás közzétéve) szeretne. A lépések a következők: 

1. A pontozási webszolgáltatás alapértelmezettől vége pontot hozzáadni. A webszolgáltatás az alapértelmezett végpont nem lehet frissíteni, így az alapértelmezettől egyik végpontját az Azure portálon létrehozásához szükséges. A [Végpontok létrehozása](../machine-learning/machine-learning-create-endpoint.md) témakört is tájékoztatást és lépésről lépéseket.
2. Azure Machine Learning meglévő frissítése csatolt pontozási az alapértelmezettől végpontot használandó szolgáltatásokat. A webes szolgáltatással, amely frissül az új végpontot használatának megkezdése.
3. Az **Azure Machine Learning frissítése erőforrás tevékenység** segítségével frissítse a webszolgáltatás az újonnan képzett modell.  

Lásd: [a frissítés erőforrás tevékenység használatával frissítése Azure Machine Learning modellek](#updating-azure-ml-models-using-the-update-resource-activity) szakaszban további információt. 

## <a name="invoking-a-web-service-using-batch-execution-activity"></a>Használatával a köteg végrehajtás tevékenység webszolgáltatás meghívása

Adatok mozgását és feldolgozása téve Azure Data Factory használja, és hajtsa végre az Azure gépi tanulási köteg végrehajtását. A legfelső szintű lépések a következők:

1. Hozzon létre egy csatolt Azure gépi tanulási szolgáltatás. Az alábbiakra van szükség:
    1. A köteg végrehajtás API a **URI kérése** . A kérés URI az weblapon szolgáltatások **KÖTEG végrehajtás** hivatkozására kattintva megtalálhatja.
    1. A közzétett Azure gépi tanulási webszolgáltatás **API billentyűt** . A webes szolgáltatás, amit közzétett gombra kattintva megtalálhatja a API billentyűt. 
 2. A **AzureMLBatchExecution** tevékenység használata.

    ![Gépi tanulási irányítópult](./media/data-factory-azure-ml-batch-execution-activity/AzureMLDashboard.png)

    ![A köteg URI](./media/data-factory-azure-ml-batch-execution-activity/batch-uri.png)


### <a name="scenario-experiments-using-web-service-inputsoutputs-that-refer-to-data-in-azure-blob-storage"></a>Alkalmazási helyzetek: Kísérletek Web service bemenetben/kimeneti értékeket hivatkozó Azure Blob-tárolóban lévő adatok használata
Ebben az esetben a Azure gépi tanulási webszolgáltatás lehetővé teszi az adatok használata az Azure blob-tárolóhoz fájlból előrejelzések és az előrejelzési eredményt blob-tárolóban lévő. A következő JSON egy Data Factory folyamat egy AzureMLBatchExecution tevékenységgel határozza meg. A tevékenység van az adatkészlet **DecisionTreeInputBlob** bemenetként és **DecisionTreeResultBlob** a kimeneti. A **típus** JSON tulajdonság segítségével a **DecisionTreeInputBlob** átadott a webszolgáltatás-bemeneteként. A **DecisionTreeResultBlob** átadott olyan eredménye megegyezik a webszolgáltatás a **webServiceOutputs** JSON tulajdonság segítségével.  

> [AZURE.IMPORTANT] 
> Ha a webes szolgáltatásnak több bemeneti adatok alapján, a **webServiceInputs** tulajdonsággal **típus**használata helyett. Példa a webServiceInputs tulajdonságot a [webszolgáltatás igényel több bemenetben](#web-service-requires-multiple-inputs) szakaszban olvashat.
>  
> A **típus**által hivatkozott adatkészleteket/**webServiceInputs** és **webServiceOutputs** tulajdonságok (az **typeProperties**) is szerepelnie kell a tevékenység **ráfordítások** és a **Kimenet**.
> 
> Az Azure Machine Learning kísérlet a web service bemeneti és kimeneti portokat és globális paraméterek alapértelmezett névvel rendelkeznek e ("input1", "input2"), amelyet testre szabhat. A nevek webServiceInputs, webServiceOutputs és globalParameters beállítások használt pontosan egyeznie kell a kísérletek nevek. A minta kérelem tartalom megtekintése az Azure Machine Learning végpontot ellenőrizze a várt leképezést köteg végrehajtás súgó lapon. 


    {
      "name": "PredictivePipeline",
      "properties": {
        "description": "use AzureML model",
        "activities": [
          {
            "name": "MLActivity",
            "type": "AzureMLBatchExecution",
            "description": "prediction analysis on batch input",
            "inputs": [
              {
                "name": "DecisionTreeInputBlob"
              }
            ],
            "outputs": [
              {
                "name": "DecisionTreeResultBlob"
              }
            ],
            "linkedServiceName": "MyAzureMLLinkedService",
            "typeProperties":
            {
                "webServiceInput": "DecisionTreeInputBlob",
                "webServiceOutputs": {
                    "output1": "DecisionTreeResultBlob"
                }                
            },
            "policy": {
              "concurrency": 3,
              "executionPriorityOrder": "NewestFirst",
              "retry": 1,
              "timeout": "02:00:00"
            }
          }
        ],
        "start": "2016-02-13T00:00:00Z",
        "end": "2016-02-14T00:00:00Z"
      }
    }

> [AZURE.NOTE] Csak a ráfordítások és a kimeneti értékeket a AzureMLBatchExecution tevékenység is lehet paraméterként a webszolgáltatás. A fenti JSON kódtöredékének az például DecisionTreeInputBlob AzureMLBatchExecution műveletre, a webszolgáltatás-bemeneteként keresztül típus paraméter átadott bemenetként is.   

### <a name="example"></a>Példa

Ez a példa Azure tárhely a bemeneti és kimeneti adatokat tartalmaznak. 

Javasoljuk, hogy feldolgozzuk [a Data Factory az első folyamat összeállítása] [ adf-build-1st-pipeline] oktatóanyag előtt ebben a példában keresztül. Az adatok Factory-szerkesztő használata hozhat létre az adatok gyári eltérések (csatolt szolgáltatásokat, adatkészleteket, folyamat) ebben a példában.   
 

1. Hozzon létre egy **csatolt szolgáltatás** az **Azure tároló**. Ha a bemeneti és kimeneti fájl az eltérő tárolási fiókok, két csatolt szolgáltatások szüksége. Íme egy JSON példa:

        {
          "name": "StorageLinkedService",
          "properties": {
            "type": "AzureStorage",
            "typeProperties": {
              "connectionString": "DefaultEndpointsProtocol=https;AccountName=[acctName];AccountKey=[acctKey]"
            }
          }
        }

2. A **beviteli** Azure Data Factory **adatkészlet**létrehozása Néhány egyéb adatok gyári adatkészleteket eltérően a következő adatkészleteket **Mappa_útvonala** és a **fájlnév** értékeket kell tartalmaznia. Szétválasztás segítségével okozhat a folyamat vagy konzerv egyedi bemeneti és kimeneti fájl minden köteg végrehajtása (minden szeletre). Előfordulhat, hogy a bemeneti alakíthat ki, a CSV-fájl formátuma, és helyezze el az egyes szeletek tárterület fiók néhány felsőbb szintű tevékenység felvenni. Ebben az esetben, nem tartalmazza a **külső** és **externalData** beállításait az alábbi példában, és a DecisionTreeInputBlob lenne a kimeneti adatkészlet, egy másik tevékenységeket.

        {
          "name": "DecisionTreeInputBlob",
          "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
              "folderPath": "azuremltesting/input",
              "fileName": "in.csv",
              "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
              }
            },
            "external": true,
            "availability": {
              "frequency": "Day",
              "interval": 1
            },
            "policy": {
              "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
              }
            }
          }
        }
    
    A beviteli csv-fájlba kell rendelkeznie az oszlopfejléceket tartalmazó sor. Ha a **Másolás tevékenység** létrehozása és helyezze a CSV-fájlok blob-tárolóhoz szolgáltatást használ, beállíthatja a gyűjtő tulajdonság **blobWriterAddHeader** **Igaz**. Példa:
    
         sink: 
         {
             "type": "BlobSink",     
             "blobWriterAddHeader": true 
         }
     
    A csv-fájl nem rendelkezik a táblázat fejlécsora, ha a következő hiba jelenhet: **Hiba a tevékenység: hiba karakterlánc olvasásakor. Nem várt token: StartObject. Elérési út ", a sor 1, 1 vigye**.
3. A **Kimenet** Azure Data Factory **adatkészlet**létrehozása. Ez a példa szétválasztás hozhat létre egy egyedi kimeneti elérési útja minden szelet végrehajtása. A szétválasztás nélkül a tevékenység felülírja a fájlt.

        {
          "name": "DecisionTreeResultBlob",
          "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
              "folderPath": "azuremltesting/scored/{folderpart}/",
              "fileName": "{filepart}result.csv",
              "partitionedBy": [
                {
                  "name": "folderpart",
                  "value": {
                    "type": "DateTime",
                    "date": "SliceStart",
                    "format": "yyyyMMdd"
                  }
                },
                {
                  "name": "filepart",
                  "value": {
                    "type": "DateTime",
                    "date": "SliceStart",
                    "format": "HHmmss"
                  }
                }
              ],
              "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
              }
            },
            "availability": {
              "frequency": "Day",
              "interval": 15
            }
          }
        }

4. Hozzon létre egy **csatolt szolgáltatás** típusú: **AzureMLLinkedService**, az API nyújtó főbb és modellezése a köteg végrehajtás URL-CÍMÉT.
        
        {
          "name": "MyAzureMLLinkedService",
          "properties": {
            "type": "AzureML",
            "typeProperties": {
              "mlEndpoint": "https://[batch execution endpoint]/jobs",
              "apiKey": "[apikey]"
            }
          }
        }
5. Végezetül szerzője egy **AzureMLBatchExecution** tevékenységet tartalmazó folyamat. Futásidőben a folyamat az alábbi lépésekkel hajtja végre:
    1. A beviteli fájl helyét megszerzi a bemeneti adatkészleteket.
    2. Elindítja az Azure gépi tanulási köteg végrehajtása API
    3. A köteg végrehajtás eredménye a megadott a kimeneti adatkészlet blob másolja. 

    > [AZURE.NOTE] AzureMLBatchExecution tevékenység beállíthatja, hogy nulla vagy több ráfordítások és egy vagy több kimeneti értékeket.

        {
          "name": "PredictivePipeline",
          "properties": {
            "description": "use AzureML model",
            "activities": [
              {
                "name": "MLActivity",
                "type": "AzureMLBatchExecution",
                "description": "prediction analysis on batch input",
                "inputs": [
                  {
                    "name": "DecisionTreeInputBlob"
                  }
                ],
                "outputs": [
                  {
                    "name": "DecisionTreeResultBlob"
                  }
                ],
                "linkedServiceName": "MyAzureMLLinkedService",
                "typeProperties":
                {
                    "webServiceInput": "DecisionTreeInputBlob",
                    "webServiceOutputs": {
                        "output1": "DecisionTreeResultBlob"
                    }                
                },
                "policy": {
                  "concurrency": 3,
                  "executionPriorityOrder": "NewestFirst",
                  "retry": 1,
                  "timeout": "02:00:00"
                }
              }
            ],
            "start": "2016-02-13T00:00:00Z",
            "end": "2016-02-14T00:00:00Z"
          }
        }

    **Kezdete** és **vége** időpontok [ISO](http://en.wikipedia.org/wiki/ISO_8601)-formátumban kell lennie. Példa: 2014-10-14T16:32:41Z. A **befejezési** idő nem kötelező. Ha nem adja meg a **befejezési** tulajdonság értékét, mint számított "**Kezdés + 48 óra.**" A folyamat végtelen időre szóló futtatásához adja meg az érték a **befejezési** tulajdonság **9999-09-09** . Kapcsolatos további tudnivalók: [JSON parancsfájlok hivatkozás](https://msdn.microsoft.com/library/dn835050.aspx) JSON tulajdonságait.

    > [AZURE.NOTE] Adja meg a AzureMLBatchExecution bevitelt tevékenység nem kötelező. 

### <a name="scenario-experiments-using-readerwriter-modules-to-refer-to-data-in-various-storages"></a>Alkalmazási helyzetek: Kísérletek olvasó/író modul használatával különböző tárolók hivatkozhat

A képernyőolvasók és a író modulok egy másik leggyakoribb helyzet Azure Machine Learning kísérletek létrehozásakor környezetbe. Adatok betöltése kísérlet szolgál a olvasó modul és a író modul adatainak mentése a kísérletek. A képernyőolvasók és a író modulok kapcsolatos részletekért témakörökben [olvasó](https://msdn.microsoft.com/library/azure/dn905997.aspx) és [író](https://msdn.microsoft.com/library/azure/dn905984.aspx) MSDN-tár.     

Amikor az olvasót és író modulok, tanácsos egy webszolgáltatási paraméter használható minden tulajdonság olvasó/író modulok. Ezek a paraméterek lehetővé teszi az értékek konfigurálása futtatókörnyezet során. Ha például Azure SQL-adatbázis használó olvasó modul segítségével létrehozhat egy kísérlet: XXX.database.windows.net. A webszolgáltatás telepítését követően a webszolgáltatás másik Azure SQL Server YYY.database.windows.net nevű megadhatja a fogyasztói engedélyezni szeretné. Egy webszolgáltatási paraméter használatával lehetővé teszi a ezt az értéket kell beállítani.

> [AZURE.NOTE] Webes szolgáltatás bemeneti és kimeneti eltérnek a webes szolgáltatás paraméterei. Az első eset van látható, hogyan egy bemeneti és kimeneti adható meg az Azure Machine Learning webszolgáltatás. Ebben az esetben, paramétereket át egy webes szolgáltatás, amelyek megfelelnek olvasó/író modulok tulajdonságait. 

Nézzük meg, webes szolgáltatás paramétereket alkalmaz a példa. Van egy telepített Azure gépi tanulási webszolgáltatásból használó olvasó modul adatok beolvasása az Azure gépi tanulási által támogatott adatforrások közül (például: Azure SQL-adatbázis). A végrehajtott köteg végrehajtása után az eredmények kerülnek író modulról (Azure SQL-adatbázis).  A kísérletek nem webes szolgáltatás ráfordítások és a kimeneti értékeket definiálva. Ebben az esetben azt javasoljuk, hogy a megfelelő web service paramétereinek az olvasót és író modulok konfigurálása. Ez a beállítás lehetővé teszi, hogy az olvasót író modulokat konfigurálhatók a AzureMLBatchExecution tevékenység használata esetén. Paramétert Web service **globalParameters** szakaszában a tevékenységet JSON az alábbi képlettel történik. 


    "typeProperties": {
        "globalParameters": {
            "Param 1": "Value 1",
            "Param 2": "Value 2"
        }
    }

[Adatok gyári függvények](https://msdn.microsoft.com/library/dn835056.aspx) használata továbbítása a webes szolgáltatás az alábbi példában látható módon paraméterekkel értékeket:

    "typeProperties": {
        "globalParameters": {
           "Database query": "$$Text.Format('SELECT * FROM myTable WHERE timeColumn = \\'{0:yyyy-MM-dd HH:mm:ss}\\'', Time.AddHours(WindowStart, 0))"
        }
    }
 
> [AZURE.NOTE] A webes szolgáltatás paramétereit-és nagybetűk, így biztosítva, hogy a nevét adja meg, ha a tevékenység JSON megegyeznek a webszolgáltatás által elérhetővé tett. 

### <a name="using-a-reader-module-to-read-data-from-multiple-files-in-azure-blob"></a>Adatok beolvasása az Azure Blob több fájl egy olvasó modulról
Nagy adatok csővezetékek például malac tevékenységekkel és struktúrát hozhat létre egy vagy több kimeneti nincs kiterjesztésű fájlok. Ha például egy külső struktúratáblával megadásakor a külső struktúratáblával adatait tárolható a következő nevet 000000_0 az Azure blob-tárolóhoz. Egy kísérlet a olvasó modul segítségével olvassa el a fájlokat, és az előrejelzések használni őket. 

Amikor a rendszer olvasó alkalmazásának modul Azure gépi tanulási kísérlet, Azure Blob-bemenetként is megadhat. A fájlokat az Azure blob-tárolóban lévő lehet a kimeneti fájl (Példa: 000000_0), amely darab termék készült parancsfájlokkal malac és struktúra futó hdinsight szolgáltatásból lehetőségre. A képernyőolvasók modul lehetővé teszi, hogy a (nincs kiterjesztésű) fájlok olvasása konfigurálásával a **elérési útját, tároló címtár/blob**. A mappát, amely tartalmazza a fájlokat, az alábbi képen látható módon a tároló és **címtár/blob** pontok **elérési útját tároló** mutat. A csillag Ez azt jelenti, hogy \*) **meghatározza, hogy a tároló mappa összes fájlt (Ez azt jelenti, hogy adatokat/aggregateddata/év 2014-es/hónap-6 = /\*)** a kísérlet részeként olvasnak.

![Azure Blob-tulajdonságok](./media/data-factory-create-predictive-pipelines/azure-blob-properties.png)


### <a name="example"></a>Példa 
#### <a name="pipeline-with-azuremlbatchexecution-activity-with-web-service-parameters"></a>Webes szolgáltatás paraméterekkel AzureMLBatchExecution tevékenységeket használó csővezeték

    {
      "name": "MLWithSqlReaderSqlWriter",
      "properties": {
        "description": "Azure ML model with sql azure reader/writer",
        "activities": [
          {
            "name": "MLSqlReaderSqlWriterActivity",
            "type": "AzureMLBatchExecution",
            "description": "test",
            "inputs": [
              {
                "name": "MLSqlInput"
              }
            ],
            "outputs": [
              {
                "name": "MLSqlOutput"
              }
            ],
            "linkedServiceName": "MLSqlReaderSqlWriterDecisionTreeModel",
            "typeProperties":
            {
                "webServiceInput": "MLSqlInput",
                "webServiceOutputs": {
                    "output1": "MLSqlOutput"
                }
                "globalParameters": {
                    "Database server name": "<myserver>.database.windows.net",
                    "Database name": "<database>",
                    "Server user account name": "<user name>",
                    "Server user account password": "<password>"
                }              
            },
            "policy": {
              "concurrency": 1,
              "executionPriorityOrder": "NewestFirst",
              "retry": 1,
              "timeout": "02:00:00"
            },
          }
        ],
        "start": "2016-02-13T00:00:00Z",
        "end": "2016-02-14T00:00:00Z"
      }
    }
 
A fenti JSON példában:

- A telepített Azure gépi tanulási webszolgáltatás olvasási/írási adatot származó/Azure SQL-adatbázissal az olvasót és író modul használja. Ez a webszolgáltatás az alábbi négy paramétereket közzététele: adatbázis-kiszolgáló nevét, az adatbázis neve, kiszolgáló felhasználói fiók nevét és kiszolgáló felhasználói fiók jelszava.  
- **Kezdete** és **vége** időpontok [ISO](http://en.wikipedia.org/wiki/ISO_8601)-formátumban kell lennie. Példa: 2014-10-14T16:32:41Z. A **befejezési** idő nem kötelező. Ha nem adja meg a **befejezési** tulajdonság értékét, mint számított "**Kezdés + 48 óra.**" A folyamat végtelen időre szóló futtatásához adja meg az érték a **befejezési** tulajdonság **9999-09-09** . Kapcsolatos további tudnivalók: [JSON parancsfájlok hivatkozás](https://msdn.microsoft.com/library/dn835050.aspx) JSON tulajdonságait.

### <a name="other-scenarios"></a>Más felhasználási területei

#### <a name="web-service-requires-multiple-inputs"></a>Webszolgáltatás több bemenetben van szükség.
Ha a webes szolgáltatásnak több bemeneti adatok alapján, a **webServiceInputs** tulajdonsággal **típus**használata helyett. A **webServiceInputs** által hivatkozott adatkészleteket is szerepelnie kell a tevékenység **bemeneti adatok alapján**.
 
Az Azure Machine Learning kísérlet a web service bemeneti és kimeneti portokat és globális paraméterek alapértelmezett névvel rendelkeznek e ("input1", "input2"), amelyet testre szabhat. A nevek webServiceInputs, webServiceOutputs és globalParameters beállítások használt pontosan egyeznie kell a kísérletek nevek. A minta kérelem tartalom megtekintése az Azure Machine Learning végpontot ellenőrizze a várt leképezést köteg végrehajtás súgó lapon.


    {
        "name": "PredictivePipeline",
        "properties": {
            "description": "use AzureML model",
            "activities": [{
                "name": "MLActivity",
                "type": "AzureMLBatchExecution",
                "description": "prediction analysis on batch input",
                "inputs": [{
                    "name": "inputDataset1"
                }, {
                    "name": "inputDataset2"
                }],
                "outputs": [{
                    "name": "outputDataset"
                }],
                "linkedServiceName": "MyAzureMLLinkedService",
                "typeProperties": {
                    "webServiceInputs": {
                        "input1": "inputDataset1",
                        "input2": "inputDataset2"
                    },
                    "webServiceOutputs": {
                        "output1": "outputDataset"
                    }
                },
                "policy": {
                    "concurrency": 3,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 1,
                    "timeout": "02:00:00"
                }
            }],
            "start": "2016-02-13T00:00:00Z",
            "end": "2016-02-14T00:00:00Z"
        }
    }

#### <a name="web-service-does-not-require-an-input"></a>Webszolgáltatás-beviteli nem igényel

Azure Machine Learning köteg végrehajtás webszolgáltatásokhoz használható munkafolyamatok, például R vagy Python parancsfájlok, bármilyen ráfordítások nem igénylő futtatásához. Vagy a kísérlet előfordulhat, hogy konfigurálva a olvasó modul, amely bármely GlobalParameters nem jelenítik meg. Ebben az esetben a AzureMLBatchExecution tevékenység volna kell konfigurálni az alábbi képlettel történik:

    {
        "name": "scoring service",
        "type": "AzureMLBatchExecution",
        "outputs": [
            {
                "name": "myBlob"
            }
        ],
        "typeProperties": {
            "webServiceOutputs": {
                "output1": "myBlob"
            }              
         },
        "linkedServiceName": "mlEndpoint",
        "policy": {
            "concurrency": 1,
            "executionPriorityOrder": "NewestFirst",
            "retry": 1,
            "timeout": "02:00:00"
        }
    },
   

#### <a name="web-service-does-not-require-an-inputoutput"></a>Webszolgáltatás-bemeneti nem igényel
Azure Machine Learning köteg végrehajtás webszolgáltatás nem feltétlenül bármely webszolgáltatás kimeneti beállítva. Ebben a példában nem tartozik webszolgáltatás beviteli vagy a kibocsátás, sem bármely GlobalParameters konfigurálva vannak. Van még egy konfigurálva a tevékenységet, magát a kimeneti, de nincs megadva, mint egy webServiceOutput.

    {
        "name": "retraining",
        "type": "AzureMLBatchExecution",
        "outputs": [
            {
                "name": "placeholderOutputDataset"
            }
        ],
        "typeProperties": {
         },
        "linkedServiceName": "mlEndpoint",
        "policy": {
            "concurrency": 1,
            "executionPriorityOrder": "NewestFirst",
            "retry": 1,
            "timeout": "02:00:00"
        }
    },

#### <a name="web-service-uses-readers-and-writers-and-the-activity-runs-only-when-other-activities-have-succeeded"></a>Webes szolgáltatás felhasználási olvasóknak és szövegírói, és a tevékenység futása, csak akkor, ha másik tevékenységeket sikeres volt

Az Azure Machine Learning web service olvasó és író modulok előfordulhat, hogy beállítania, hogy futtatni vagy bármely GlobalParameters nélkül. Azonban érdemes szolgáltatás hívások beágyazása egy folyamat használó adatkészlet függőségek meghívni a szolgáltatást, csak akkor, ha egyes felsőbb szintű feldolgozási befejeződött. Más műveletet a köteg végrehajtása a használja ezt a megközelítést befejeződése után is válthat. Ebben az esetben is Express terméket a függőségeket használata a tevékenység ráfordítások és a kimeneti értékeket, anélkül, hogy elnevezési bármelyiküket webszolgáltatás ráfordítások vagy a kimeneti értékeket.

    {
        "name": "retraining",
        "type": "AzureMLBatchExecution",
        "inputs": [
            {
                "name": "upstreamData1"
            },
            {
                "name": "upstreamData2"
            }
        ],
        "outputs": [
            {
                "name": "downstreamData"
            }
        ],
        "typeProperties": {
         },
        "linkedServiceName": "mlEndpoint",
        "policy": {
            "concurrency": 1,
            "executionPriorityOrder": "NewestFirst",
            "retry": 1,
            "timeout": "02:00:00"
        }
    },

A **takeaways** a következők:

-   Ha a kísérlet végpont használja a típus: képviseli blob adatkészletet, és a a tevékenység ráfordítások és a típus tulajdonság szerepel. A típus tulajdonság nincs megadva. 
-   Ha a kísérlet végpont használ webServiceOutput(s): jeleníti meg blob adatkészleteket, és szerepelnek a tevékenység kimeneti értékeket, és a webServiceOutputs tulajdonságban. A tevékenység exportálja és webServiceOutputs a nevét a kísérlet az minden kimenő vannak megfeleltetve. A webServiceOutputs tulajdonság nincs megadva.
-   A kísérlet végpont globalParameter(s) közzététele, ha a tevékenység globalParameters tulajdonság felsorolásukat, fő, érték párokká. A globalParameters tulajdonság nincs megadva. A billentyűk nagybetűk között. [Azure Data Factory függvények](data-factory-scheduling-and-execution.md#data-factory-functions-reference) is használhatók az értékeket. 
- További adatkészleteket szerepelni fog a tevékenység ráfordítások és a kimeneti értékeket tulajdonság, anélkül, hogy a tevékenység typeProperties hivatkoznak. Ezek adatkészleteket szeletet függőségek végrehajtását szabályozzák, de egyébként figyelmen kívül hagyja a AzureMLBatchExecution tevékenységet. 


## <a name="updating-models-using-update-resource-activity"></a>Az adatmodellek használatával frissítés erőforrás tevékenység frissítése
Az idő múlásával a cserélendő modellekben, a kísérletek pontozási Azure Machine Learning kell kell retrained új beviteli adatkészleteket használatával. Miután végzett a átképzés, frissítse a pontozási webszolgáltatás a retrained Machine Learning modell szeretne. A szokásos átképzési és frissítése Azure Machine Learning modellek keresztül web services engedélyezése használatának lépései: 

1. Hozzon létre egy kísérlet [Azure Machine Learning Studio](https://studio.azureml.net). 
2. Ha elégedett, és a modellt, Azure Machine Learning Studio segítségével webes szolgáltatásainak mindkét a **oktatás kísérletezhet** közzététele és pontozási /**prediktív kísérlet**.

Az alábbi táblázat ismerteti a webes szolgáltatások ebben a példában használt.  [Gépi tanulási Újraépítés modellek programozás útján](../machine-learning/machine-learning-retrain-models-programmatically.md) talál további információt.

| Webes szolgáltatás típusa | Leírás 
| :------------------ | :---------- 
| **A webszolgáltatás – oktatás** | Oktatóanyag adatok kap, és képzett modellek eredményez. Átképzésének kimenetét az Azure Blob-tárolóban lévő .ilearner fájlként.  Az **alapértelmezett végpont** automatikusan létrejön a oktatás kísérlet webes szolgáltatásként közzétételekor. További végpontokat is létrehozhat, azonban a példa csak az alapértelmezett végpont |
| **Webszolgáltatás pontozás** | Adatok nélküli példa fogadása, és lehetővé teszi az előrejelzések. A kimenet előrejelzési különböző űrlapok, például .csv fájl és Azure SQL-adatbázishoz, attól függően, hogy a kísérlet a konfiguráció sorának lehetnek. Az alapértelmezett végpont automatikusan létrejön a cserélendő kísérlet mint webszolgáltatás közzétételkor. A második **alapértelmezettől és frissíthető végpont** létrehozása az [Azure portal](https://manage.windowsazure.com)segítségével. További végpontok hozhat létre, de ez a példa csak egy nem alapértelmezett frissíthető végpontot. A [Végpontok létrehozása](../machine-learning/machine-learning-create-endpoint.md) cikke lépéseket.       
 
Az alábbi ábra szemlélteti képzés és Azure Machine Learning a végpontok pontozás közötti kapcsolatra. 

![Webszolgáltatások](./media/data-factory-azure-ml-batch-execution-activity/web-services.png)


A **Tanfolyam webszolgáltatás** meghívásához az **Azure Machine Learning köteg végrehajtás tevékenység**használatával. Oktatás webszolgáltatás meghívása megegyezik az Azure Machine Learning webszolgáltatás (pontozási webszolgáltatás) meghívása pontozási adatokhoz. Az előző szakaszokban terjed ki, hogy hogyan indítsa az Azure Machine Learning webszolgáltatásnak az Azure Data Factory folyamat részletesen a. 
  
A **pontozási webszolgáltatás** meghívásához frissítse a webszolgáltatás az újonnan képzett modell az **Azure Machine Learning frissítése erőforrás tevékenység** használatával. A fenti táblázatban említett, létre kell hoznia és használata az alapértelmezettől frissíthető végpontot. Ezeken kívül frissítése bármelyik meglévő csatolt-szolgáltatások használatához az alapértelmezettől végpontot, így mindig a legújabb retrained modell használata az adatok gyári. 

A következő esetben részletesen ismerteti. Példa átképzés, és az Azure Data Factory folyamat Azure Machine Learning modelljei frissítésére van. 
 
### <a name="scenario-retraining-and-updating-an-azure-ml-model"></a>Alkalmazási helyzetek: átképzés és frissítése az Azure Machine Learning modell
Ez a témakör a minta folyamat, amely az **Azure Machine Learning köteg végrehajtás tevékenység** modell Újraépítés használ. A folyamat az **Azure Machine Learning frissítése erőforrás tevékenység** is használja a modell az pontozási webszolgáltatás frissítése. A szakasz is biztosít JSON kódrészletek csatolt szolgáltatások, adatkészleteket és a példában a folyamat. 

Az alábbiakban a diagram nézetben a minta folyamat. Amint látható, az Azure Machine Learning köteg végrehajtás tevékenység megnyitja a oktatás bemeneti, és hoz létre egy oktatás kimenet (iLearner-fájl). Azure Machine Learning frissítés erőforrás tevékenység megnyitja a oktatás kimeneti és frissíti a modellt a pontozási webes szolgáltatás végpontjának. A frissítés erőforrás tevékenység nem hozhatók létre bármely kimeneti. A placeholderBlob csak egy Azure Data Factory szolgáltatás a folyamat futtatásához szükséges üres kimeneti adatkészlet. 

![folyamat diagram](./media/data-factory-azure-ml-batch-execution-activity/update-activity-pipeline-diagram.png)


#### <a name="azure-blob-storage-linked-service"></a>Azure Blob-tárolóhoz összekapcsolt szolgáltatás:
Az Azure tároló rendelkezik az alábbi adatokat:

- oktatóanyag adatok. A bemeneti adatok a Azure Machine Learning oktatás webszolgáltatás.  
- iLearner fájlt. A kimenet az Azure Machine Learning oktatás webszolgáltatásból. A fájl is a bemeneti frissítés erőforrás műveletre.  
   
Az alábbiakban a csatolt szolgáltatás minta JSON meghatározása: 

    {
        "name": "StorageLinkedService",
        "properties": {
            "type": "AzureStorage",
            "typeProperties": {
                "connectionString": "DefaultEndpointsProtocol=https;AccountName=name;AccountKey=key"
            }
        }
    }


#### <a name="training-input-dataset"></a>Oktatás beviteli adatkészlet:
Az alábbi adatkészlet jelöli az Azure Machine Learning oktatás webszolgáltatás beviteli oktatás adatait. Az Azure Machine Learning köteg végrehajtás tevékenység e adatkészlet-bemeneteként vesz igénybe. 

    {
        "name": "trainingData",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
                "folderPath": "labeledexamples",
                "fileName": "labeledexamples.arff",
                "format": {
                    "type": "TextFormat"
                }
            },
            "availability": {
                "frequency": "Week",
                "interval": 1
            },
            "policy": {          
                "externalData": {
                    "retryInterval": "00:01:00",
                    "retryTimeout": "00:10:00",
                    "maximumRetry": 3
                }
            }
        }
    }

#### <a name="training-output-dataset"></a>Oktatás kimeneti adatkészlet:
Az alábbi adatkészlet a kimeneti iLearner fájlt jelöl, az Azure Machine Learning oktatás webszolgáltatásból. Azure Machine Learning köteg végrehajtás tevékenységet a adatkészlet eredményez. Ez az adatkészlet is a bemeneti Azure Machine Learning frissítés erőforrás műveletre.

    {
        "name": "trainedModelBlob",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
                "folderPath": "trainingoutput",
                "fileName": "model.ilearner",
                "format": {
                    "type": "TextFormat"
                }
            },
            "availability": {
                "frequency": "Week",
                "interval": 1
            }
        }
    }

#### <a name="linked-service-for-azure-ml-training-endpoint"></a>Azure Machine Learning oktatás végpont csatolt szolgáltatás 
A következő JSON kódtöredékének határozza meg, hogy egy kapcsolódó Azure gépi tanulási szolgáltatás, amely az alapértelmezett végpont oktatás webszolgáltatás mutat. 

    {   
        "name": "trainingEndpoint",
        "properties": {
            "type": "AzureML",
            "typeProperties": {
                "mlEndpoint": "https://ussouthcentral.services.azureml.net/workspaces/xxx/services/--training experiment--/jobs",
                "apiKey": "myKey"
            }
        }
    }

**Azure Machine Learning Studio**tegye a következőket értékek beszerzése **mlEndpoint** és **apiKey**:

1. A bal oldali menüben kattintson a **WEBSZOLGÁLTATÁSOKHOZ** .
2. Kattintson a **oktatás webszolgáltatás** webes szolgáltatások listában. 
3. **API-ja kulcs** szövegmező melletti Másolás gombra. Illessze be a kulcsot a vágólapra a adatok gyári JSON-szerkesztőben.
4. **Azure Machine Learning studio**, kattintson a **KÖTEG végrehajtás** hivatkozásra.
5. Másolja az **URI kérése** a **kérése** szakasz, és illessze be az adatok gyári JSON-szerkesztőben.   


#### <a name="linked-service-for-azure-ml-updatable-scoring-endpoint"></a>Azure Machine Learning frissíthető pontozási végpont csatolt szolgáltatás:
A következő JSON kódtöredékének határozza meg, hogy egy kapcsolódó Azure gépi tanulási szolgáltatás, amely az alapértelmezettől frissíthető végpontot pontozási webszolgáltatás mutat.  

    {
        "name": "updatableScoringEndpoint2",
        "properties": {
            "type": "AzureML",
            "typeProperties": {
                "mlEndpoint": "https://ussouthcentral.services.azureml.net/workspaces/xxx/services/--scoring experiment--/jobs",
                "apiKey": "endpoint2Key",
                "updateResourceEndpoint": "https://management.azureml.net/workspaces/xxx/webservices/--scoring experiment--/endpoints/endpoint2"
            }
        }
    }


Létrehozásával és telepítésével az Azure Machine Learning csatolt szolgáltatást, mielőtt a ismertetett lépésekkel [Végpontok hozzon létre](../machine-learning/machine-learning-create-endpoint.md) egy második (alapértelmezettől és frissíthető) a pontozási webszolgáltatás végpont létrehozásához.

Miután létrehozta az alapértelmezettől frissíthető végpontot, tegye a következőket:

- Kattintson a **KÖTEG végrehajtási** **mlEndpoint** JSON tulajdonság URI érték beolvasása gombra.
- **Erőforrás a frissítés** hivatkozásra kattintva URI érték beolvasása **updateResourceEndpoint** JSON tulajdonság. Az API kulcsa végpont közvetlenül a lapokon (a jobb alsó sarokban). 

![frissíthető végpont](./media/data-factory-azure-ml-batch-execution-activity/updatable-endpoint.png)

 
#### <a name="placeholder-output-dataset"></a>Helyőrző kimeneti adatkészlet:
Az Azure Machine Learning frissítés erőforrás tevékenység nem hoz létre bármely kimeneti. Azure Data Factory azonban egy kimenet adatkészlet az ütemezésre egy folyamat való használatához szükséges. Egy próbabábutest/helyőrző adatkészlet ezért ebben a példában használjuk.  

    {
        "name": "placeholderBlob",
        "properties": {
            "availability": {
                "frequency": "Week",
                "interval": 1
            },
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
                "folderPath": "any",
                "format": {
                    "type": "TextFormat"
                }
            }
        }
    }


#### <a name="pipeline"></a>Folyamat
A folyamat még a két tevékenység: **AzureMLBatchExecution** és **AzureMLUpdateResource**. Az Azure Machine Learning köteg végrehajtás tevékenység megnyitja az oktatóanyag adatok bemeneteként, és létrehoz egy iLearner fájlt egy kimenetként. A tevékenység elindítja a oktatás webszolgáltatás (oktatás kísérlet webszolgáltatás közzétéve) a bemeneti oktatóanyag adatok, és a ilearner fájl fogad a webszolgáltatás. A placeholderBlob csak egy Azure Data Factory szolgáltatás a folyamat futtatásához szükséges üres kimeneti adatkészlet. 

![folyamat diagram](./media/data-factory-azure-ml-batch-execution-activity/update-activity-pipeline-diagram.png)


    {
        "name": "pipeline",
        "properties": {
            "activities": [
                {
                    "name": "retraining",
                    "type": "AzureMLBatchExecution",
                    "inputs": [
                        {
                            "name": "trainingData"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "trainedModelBlob"
                        }
                    ],
                    "typeProperties": {
                        "webServiceInput": "trainingData",
                        "webServiceOutputs": {
                            "output1": "trainedModelBlob"
                        }              
                     },
                    "linkedServiceName": "trainingEndpoint",
                    "policy": {
                        "concurrency": 1,
                        "executionPriorityOrder": "NewestFirst",
                        "retry": 1,
                        "timeout": "02:00:00"
                    }
                },
                {
                    "type": "AzureMLUpdateResource",
                    "typeProperties": {
                        "trainedModelName": "Training Exp for ADF ML [trained model]",
                        "trainedModelDatasetName" :  "trainedModelBlob"
                    },
                    "inputs": [
                        {
                            "name": "trainedModelBlob"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "placeholderBlob"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00",
                        "concurrency": 1,
                        "retry": 3
                    },
                    "name": "AzureML Update Resource",
                    "linkedServiceName": "updatableScoringEndpoint2"
                }
            ],
            "start": "2016-02-13T00:00:00Z",
            "end": "2016-02-14T00:00:00Z"
        }
    }


### <a name="reader-and-writer-modules"></a>A képernyőolvasók és a író modulok

A leggyakoribb helyzet Web service paramétereket az Azure SQL-olvasók és Szövegírói. Adatok betöltése management adatszolgáltatások Azure gépi tanulási Studio kívül a kísérlet az olvasót modul szolgál. A író modul adatainak mentése a kísérletek az adatkezelési adatszolgáltatások Azure gépi tanulási Studio kívül.  

Azure Blob/Azure SQL-olvasó/író kapcsolatos részletekért témakörökben [olvasó](https://msdn.microsoft.com/library/azure/dn905997.aspx) és [író](https://msdn.microsoft.com/library/azure/dn905984.aspx) a MSDN-tárhoz. A példában az előző szakaszban az Azure Blob-olvasó és Azure Blob-író használja. Ez a szakasz ismerteti, hogy az Azure SQL-olvasó és Azure SQL-író.


## <a name="frequently-asked-questions"></a>Gyakori kérdések

**Q:** A nagy adatok folyamatok által létrehozott fájlokat-e. A AzureMLBatchExecution tevékenység segítségével összes fájlok?

**A:** igen. A részletek **olvasó modulról-fájlokat az Azure Blob-ből származó adatok** szakaszban olvashat. 

## <a name="azure-ml-batch-scoring-activity"></a>Azure Machine Learning köteg pontozási tevékenység
A **AzureMLBatchScoring** tevékenység integrálása az Azure gépi tanulási szolgáltatást használ, azt javasoljuk, hogy használja-e a legújabb **AzureMLBatchExecution** tevékenységet. 

A AzureMLBatchExecution tevékenység bevezetett a Azure SDK és Azure PowerShell augusztus 2015 verziójában.

Ha továbbra is a AzureMLBatchScoring tevékenység használni szeretne, továbbra is – ez a szakasz olvasási.  

### <a name="azure-ml-batch-scoring-activity-using-azure-storage-for-inputoutput"></a>Azure-tárhely használatának bemeneti és kimeneti Azure Machine Learning köteg pontozási tevékenység 

    {
      "name": "PredictivePipeline",
      "properties": {
        "description": "use AzureML model",
        "activities": [
          {
            "name": "MLActivity",
            "type": "AzureMLBatchScoring",
            "description": "prediction analysis on batch input",
            "inputs": [
              {
                "name": "ScoringInputBlob"
              }
            ],
            "outputs": [
              {
                "name": "ScoringResultBlob"
              }
            ],
            "linkedServiceName": "MyAzureMLLinkedService",
            "policy": {
              "concurrency": 3,
              "executionPriorityOrder": "NewestFirst",
              "retry": 1,
              "timeout": "02:00:00"
            }
          }
        ],
        "start": "2016-02-13T00:00:00Z",
        "end": "2016-02-14T00:00:00Z"
      }
    }

### <a name="web-service-parameters"></a>Webes szolgáltatás paraméterei
Webes szolgáltatás paraméterekkel értékek megadásához a folyamat az alábbi példában látható módon JSON a **AzureMLBatchScoringActivty** című **typeProperties** szakasz hozzáadása: 

    "typeProperties": {
        "webServiceParameters": {
            "Param 1": "Value 1",
            "Param 2": "Value 2"
        }
    }


[Adatok gyári függvények](https://msdn.microsoft.com/library/dn835056.aspx) használata továbbítása a webes szolgáltatás az alábbi példában látható módon paraméterekkel értékeket:

    "typeProperties": {
        "webServiceParameters": {
           "Database query": "$$Text.Format('SELECT * FROM myTable WHERE timeColumn = \\'{0:yyyy-MM-dd HH:mm:ss}\\'', Time.AddHours(WindowStart, 0))"
        }
    }
 
> [AZURE.NOTE] A webes szolgáltatás paramétereit-és nagybetűk, így biztosítva, hogy a nevét adja meg, ha a tevékenység JSON megegyeznek a webszolgáltatás által elérhetővé tett. 

## <a name="see-also"></a>Lásd még:

- [Azure blogbejegyzés: Azure Data Factory és Azure gépi tanulási első lépések](https://azure.microsoft.com/blog/getting-started-with-azure-data-factory-and-azure-machine-learning-4/)





[adf-build-1st-pipeline]: data-factory-build-your-first-pipeline.md

[azure-machine-learning]: http://azure.microsoft.com/services/machine-learning/


 
