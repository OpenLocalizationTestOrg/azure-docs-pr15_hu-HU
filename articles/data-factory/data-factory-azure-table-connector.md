<properties 
    pageTitle="Adatok áthelyezése és Azure-táblából |} Microsoft Azure" 
    description="Megtudhatja, hogy miként Azure Táblatárolóhoz Azure Data Factory használatával vagy az adatokat." 
    services="data-factory" 
    documentationCenter="" 
    authors="linda33wj" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/13/2016" 
    ms.author="jingwang"/>

# <a name="move-data-to-and-from-azure-table-using-azure-data-factory"></a>Adatok áthelyezése és az Azure-táblából Azure Data Factory használatával

Ez a cikk azt ismerteti, hogyan használhatja a Másolás tevékenység-Azure adatok gyári az-adatok áthelyezése és az Azure-táblából származó/egy másik adattár. Ez a cikk az adatok mozgását és a támogatott adatokat tároló kombinációk általános áttekintése másolás tevékenység eltéréseit [Mozgás a tevékenységekre vonatkozó adatok](data-factory-data-movement-activities.md) cikk épül.

## <a name="copy-data-wizard"></a>Az adatok varázsló másolása
A hozzon létre egy folyamat, amely másolja az adatokat, és az Azure Táblatárolóhoz legegyszerűbben a Másolás adatok varázsló használatával. Lásd: [oktatóprogram: hozzon létre egy példány varázslóval folyamat](data-factory-copy-data-wizard-tutorial.md) létrehozása egy folyamat adatok másolása varázsló segítségével a rövid ismertetését megtalálja. 

Az alábbi példák adnia minta JSON-definíciók létrehozása egy folyamat [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) segítségével vagy [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) vagy [Azure Powershellhez](data-factory-copy-activity-tutorial-using-powershell.md)használható. Adatok másolása, Azure Táblatárolóhoz és az Azure Blob-adatbázis mutatnak. Azonban adatokat másolt **közvetlenül** bármilyen forrásból valamelyik, az mosdók megadott [Itt](data-factory-data-movement-activities.md#supported-data-stores) a Másolás tevékenység használata Azure Data Factory is lehet.

## <a name="sample-copy-data-from-azure-table-to-azure-blob"></a>Minta: Másolása adatok Azure-táblából az Azure Blob

A következő példában:

1.  Csatolt szolgáltatás típusú [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties) (blob & tábla használt).
2.  A beviteli [adatkészlet](data-factory-create-datasets.md) [AzureTable](#azure-table-dataset-type-properties)típusú.
3.  Egy kimenet [adatkészlet](data-factory-create-datasets.md) [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties)típusú. 
3.  A [folyamat](data-factory-create-pipelines.md) [AzureTableSource](#azure-table-copy-activity-type-properties) és [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties)használó másolása a tevékenységhez. 

A minta másolja át az alapértelmezett partíciót egy Azure blob táblázat óránként tartozó adatok. A minták következő szakaszok ezeket mintákban használt JSON tulajdonságok témakörben olvashat.

**Azure csatolt tárhelyszolgáltatáshoz:**

    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

Azure Data Factory támogatja az Azure csatolt tárolása kétféle: **AzureStorage** és **AzureStorageSas**. Az első szakasz, adja meg a kapcsolati karakterlánc, amely tartalmazza a fiókkulcs és a későbbi egy, adja meg a megosztott Access-aláírás (Társítások) Uri. Lásd: [Csatolt](#linked-services) szakaszában további információt.  

**Azure táblázat beviteli adatkészlet:**

A példa feltételezi, hogy Azure-táblából létrehozott tábla "Táblanév".
 
"Külső" beállítást: "igaz" tájékoztatja a Data Factory szolgáltatást, hogy az adatkészlet külső adatok gyári és az adatok gyári tevékenységet nem készül.

    {
      "name": "AzureTableInput",
      "properties": {
        "type": "AzureTable",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "tableName": "MyTable"
        },
        "external": true,
        "availability": {
          "frequency": "Hour",
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

**Azure Blob kimeneti adatkészlet:**

Adatok egy új blob íródott óránként (gyakoriság: óra, intervallum: 1). A mappa elérési útját a blob dinamikusan kiértékeli a kezdési időpontot a szelet, amely feldolgozása alapján. A mappa elérési útja használja az év, hónap, nap és a kezdési időpontot óra részei. 

    {
      "name": "AzureBlobOutput",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
          "partitionedBy": [
            {
              "name": "Year",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "yyyy"
              }
            },
            {
              "name": "Month",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "MM"
              }
            },
            {
              "name": "Day",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "dd"
              }
            },
            {
              "name": "Hour",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "HH"
              }
            }
          ],
          "format": {
            "type": "TextFormat",
            "columnDelimiter": "\t",
            "rowDelimiter": "\n"
          }
        },
        "availability": {
          "frequency": "Hour",
          "interval": 1
        }
      }
    }

**A Másolás tevékenységgel csővezeték:**

A folyamat egy példány tevékenységet, amely a bemeneti és kimeneti adatkészleteket használatára van beállítva, és van ütemezve óránként tartalmazza. A során JSON megadása az **adatforrás** típusa **AzureTableSource** van állítva, és **BlobSink** **gyűjtő** típusának beállítása. Az SQL-lekérdezés **AzureTableSourceQuery** tulajdonság megadott választja ki az adatokat az alapértelmezett partíciót óránként másolásához.

    {  
        "name":"SamplePipeline",
        "properties":{  
            "start":"2014-06-01T18:00:00",
            "end":"2014-06-01T19:00:00",
            "description":"pipeline for copy activity",
            "activities":[  
                {
                    "name": "AzureTabletoBlob",
                    "description": "copy activity",
                    "type": "Copy",
                    "inputs": [
                        {
                            "name": "AzureTableInput"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobOutput"
                        }
                    ],
                    "typeProperties": {
                        "source": {
                            "type": "AzureTableSource",
                            "AzureTableSourceQuery": "PartitionKey eq 'DefaultPartitionKey'"
                        },
                        "sink": {
                            "type": "BlobSink"
                        }
                    },
                    "scheduler": {
                        "frequency": "Hour",
                        "interval": 1
                    },              
                    "policy": {
                        "concurrency": 1,
                        "executionPriorityOrder": "OldestFirst",
                        "retry": 0,
                        "timeout": "01:00:00"
                    }
                }
             ]  
        }
    }

## <a name="sample-copy-data-from-azure-blob-to-azure-table"></a>Minta: Adatok másolása Azure Blob Azure-táblából

A következő példában:

1.  A csatolt típusú szolgáltatás [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties) (használt blob & tábla)
3.  A beviteli [adatkészlet](data-factory-create-datasets.md) [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties)típusú.
4.  Egy kimenet [adatkészlet](data-factory-create-datasets.md) [AzureTable](#azure-table-dataset-type-properties)típusú. 
4.  A [folyamat](data-factory-create-pipelines.md) [BlobSource](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) és [AzureTableSink](#azure-table-copy-activity-type-properties)használó másolása a tevékenységhez. 


A minta másolatok-idősorok adatainak egy Azure blob-szeretne egy Azure óránkénti tábla. A minták következő szakaszok ezeket mintákban használt JSON tulajdonságok témakörben olvashat.

**Azure (az Azure-táblából és Blob) kapcsolódó tárhelyszolgáltatáshoz:**

    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

Azure Data Factory támogatja az Azure csatolt tárolása kétféle: **AzureStorage** és **AzureStorageSas**. Az első szakasz, adja meg a kapcsolati karakterlánc, amely tartalmazza a fiókkulcs és a későbbi egy, adja meg a megosztott Access-aláírás (Társítások) Uri. Lásd: [Csatolt](#linked-services) szakaszában további információt. 

**Azure Blob beviteli adatkészlet:**

Adatok van kiválasztott az új blob óránként (gyakoriság: óra, intervallum: 1). A mappa elérési útját és nevét a blob-dinamikusan értékeli ki a kezdési időpontot a szelet, amely feldolgozása alapján. A mappa elérési útja használ év, hónap és nap része a kezdési időpontot, és a fájlnevet használja a kezdési időpontot a óra részét. "külső": "igaz" beállítás tájékoztatja a Data Factory szolgáltatást, hogy az adatkészlet külső adatok gyári és az adatok gyári tevékenységet nem készül.
    
    {
      "name": "AzureBlobInput",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}",
          "fileName": "{Hour}.csv",
          "partitionedBy": [
            {
              "name": "Year",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "yyyy"
              }
            },
            {
              "name": "Month",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "MM"
              }
            },
            {
              "name": "Day",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "dd"
              }
            },
            {
              "name": "Hour",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "HH"
              }
            }
          ],
          "format": {
            "type": "TextFormat",
            "columnDelimiter": ",",
            "rowDelimiter": "\n"
          }
        },
        "external": true,
        "availability": {
          "frequency": "Hour",
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

**Azure-táblából az adatkészlet kimeneti:**

A minta Azure-táblából az "táblanév" nevű táblázat másolja az adatokat. Hozzon létre egy Azure táblázatot ugyanannyi oszlopot megfelelően működnek a Blob CSV-fájl, amely tartalmazhat. Új sorok kerülnek be a táblába óránként. 

    {
      "name": "AzureTableOutput",
      "properties": {
        "type": "AzureTable",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "tableName": "MyOutputTable"
        },
        "availability": {
          "frequency": "Hour",
          "interval": 1
        }
      }
    }

**A Másolás tevékenységgel csővezeték:**

A folyamat egy példány tevékenységet, amely a bemeneti és kimeneti adatkészleteket használatára van beállítva, és van ütemezve óránként tartalmazza. A során JSON megadása az **adatforrás** típusa **BlobSource** van állítva, és **AzureTableSink** **gyűjtő** típusának beállítása. 


    {  
        "name":"SamplePipeline",
        "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline with copy activity",
        "activities":[  
          {
            "name": "AzureBlobtoTable",
            "description": "Copy Activity",
            "type": "Copy",
            "inputs": [
              {
                "name": "AzureBlobInput"
              }
            ],
            "outputs": [
              {
                "name": "AzureTableOutput"
              }
            ],
            "typeProperties": {
              "source": {
                "type": "BlobSource"
              },
              "sink": {
                "type": "AzureTableSink",
                "writeBatchSize": 100,
                "writeBatchTimeout": "01:00:00"
              }
            },
            "scheduler": {
              "frequency": "Hour",
              "interval": 1
            },                      
            "policy": {
              "concurrency": 1,
              "executionPriorityOrder": "OldestFirst",
              "retry": 0,
              "timeout": "01:00:00"
            }
          }
          ]
       }
    }

## <a name="linked-services"></a>Kapcsolt szolgáltatások
Csatolt szolgáltatások is használhatja az Azure adatok gyári Azure blob-tárolóhoz csatolása két típusa van. Azok: **AzureStorage** csatolva, és csatolt **AzureStorageSas** szolgáltatást. Az Azure csatolt tárhelyszolgáltatáshoz globális hozzáféréssel rendelkező adatok gyári Azure tárolására tartalmazza. Mivel az Azure tároló Társítások (megosztott Access-aláírás) kapcsolódó szolgáltatás biztosít az adatok gyári korlátozott/idő – kötött hozzáféréssel rendelkező az Azure-tárolóhoz. Az alábbi két csatolt szolgáltatások között nincs más különbségek vannak. Válassza a csatolt szolgáltatás, hogy igényeinek. A következő szakaszokban további részleteket a két csatolt szolgáltatásokra.

[AZURE.INCLUDE [data-factory-azure-storage-linked-services](../../includes/data-factory-azure-storage-linked-services.md)]

## <a name="azure-table-dataset-type-properties"></a>Azure adatkészlet táblázat tulajdonságai

Szakaszok és a rendelkezésre álló adatkészleteket definiálása tulajdonságok teljes listáját a [létrehozása adatkészleteket](data-factory-create-datasets.md) témakört is. Szakaszok, például a struktúra, elérhetőségét és egy adatkészletből JSON házirend hasonlóak az adatkészlet diagramtípusokat (Azure SQL Azure blob, Azure táblázat stb.).

A typeProperties szakasz eltérő adatkészlet hibatípusonként és az adatok tárolása az adatok helyének adatait. Az **typeProperties** szakaszban az adatkészlet típusú **AzureTable** tartalmaz, a következő tulajdonságokat.

| A tulajdonság | Leírás | Szükséges |
| -------- | ----------- | -------- |
| Táblanév | A tábla a csatolt szolgáltatás hivatkozó Azure-táblából adatbázis-példány nevét. | igen. A táblanév meg van adva egy azureTableSourceQuery nélkül, amikor a rendszer a cél másolja a táblázat összes rekordot. Ha egy azureTableSourceQuery is van megadva, a tábla, amely megfelel a lekérdezés rekordjai másolja a cél. |

### <a name="schema-by-data-factory"></a>Adatok gyári által séma
A séma ingyenes adatokat tárolja, például az Azure-táblából az adatok gyári szolgáltatás a séma infers az alábbi módon:

1.  Adatok struktúrájára adatkészlet meghatározása a **struktúra** tulajdonság használatával adja meg, ha az adatok gyári szolgáltatás, a séma végzett betartja a struktúra. Ebben az esetben ha a sor egy értéket, ha az oszlop nem tartalmaz, a null érték megadva azt.
2. Adatok struktúrájára adatkészlet meghatározása **struktúra** tulajdonságával nem adja meg, ha a Data Factory a séma infers az adatok az első sor használatával. Ebben az esetben, ha az első sor nem tartalmaz a teljes séma, oszloppal vannak kihagyott a másolási művelet eredményét.

Ezért séma ingyenes adatforrások esetén a legjobb módszer, hogy adja meg a **struktúra** tulajdonságával adatok struktúrájára.

## <a name="azure-table-copy-activity-type-properties"></a>Táblázat másolása tevékenység Azure tulajdonságai

Szakaszok és a tevékenységek definiálásával használható tulajdonságok teljes listáját a [Folyamatok létrehozása](data-factory-create-pipelines.md) témakört is. Az összes tevékenységtípusokhoz tulajdonságait, például a név, leírás, a bemeneti és kimeneti adatkészleteket és házirendek érhetők el. 

Minden tevékenység típusa azonban függenek tulajdonságok elérhető typeProperties szakaszában a tevékenységet. Másolás tevékenységhez azok eltérőek attól függően, hogy milyen típusú adatforrások és mosdók.

**AzureTableSource** typeProperties szakasz támogatja a következő tulajdonságokat:

A tulajdonság | Leírás | Megengedett érték | Szükséges
-------- | ----------- | -------------- | -------- 
azureTableSourceQuery | Az egyéni lekérdezés használatával olvassa el az adatok. | Azure-táblából lekérdezési karakterlánc. Példák a következő szakaszban. | nem. A táblanév meg van adva egy azureTableSourceQuery nélkül, amikor a rendszer a cél másolja a táblázat összes rekordot. Ha egy azureTableSourceQuery is van megadva, a tábla, amely megfelel a lekérdezés rekordjai másolja a cél.
azureTableSourceIgnoreTableNotFound | Ezzel azt jelzi, hogy a kivétel táblázat swallow nem létezik. | IGAZ<br/>HAMIS | nem |

### <a name="azuretablesourcequery-examples"></a>azureTableSourceQuery példák

Ha az Azure-táblából oszlop karakterlánc típusú: 

    azureTableSourceQuery": "$$Text.Format('PartitionKey ge \\'{0:yyyyMMddHH00_0000}\\' and PartitionKey le \\'{0:yyyyMMddHH00_9999}\\'', SliceStart)"

Ha az Azure-táblából oszlop dátum/idő típusú: 

    "azureTableSourceQuery": "$$Text.Format('DeploymentEndTime gt datetime\\'{0:yyyy-MM-ddTHH:mm:ssZ}\\' and DeploymentEndTime le datetime\\'{1:yyyy-MM-ddTHH:mm:ssZ}\\'', SliceStart, SliceEnd)"


**AzureTableSink** typeProperties szakasz támogatja a következő tulajdonságokat:


A tulajdonság | Leírás | Megengedett érték | Szükséges  
-------- | ----------- | -------------- | -------- 
azureTableDefaultPartitionKeyValue | Alapértelmezett partíciót fő érték, amely a gyűjtő használható. | Egy karakterlánc értéket. | nem 
azureTablePartitionKeyName | Adja meg, amelynek értékeit partíciót kulcsként használt oszlopot. Ha nincs megadva, AzureTableDefaultPartitionKeyValue szolgál a partíciót billentyűt. | Az oszlop nevére. | nem |
azureTableRowKeyName | Adja meg nevét, amelynek oszlopértékek sor kulcsként használt oszlop. Ha nincs megadva, használja a GUID minden egyes sorára. | Az oszlop nevére. | nem  
azureTableInsertType | Adatok beillesztése az Azure-táblából az módja.<br/><br/>Ez a tulajdonság szabályozza, hogy rendelkezik-e a kimeneti tábla egyező partíciót vagy a sort tartalmazó meglévő sorok értékeik helyett, illetve egyesített. <br/><br/>Ezeket a beállításokat (egyesítés és a csere) működésének megismerése, [Beszúrás vagy entitás egyesítés](https://msdn.microsoft.com/library/azure/hh452241.aspx) és [Beszúrás vagy entitás cseréje](https://msdn.microsoft.com/library/azure/hh452242.aspx) témaköröket ajánljuk. <br/><br> Ezt a beállítást alkalmazza a sor szintre, de ne a táblázat szintet, és sem lehetőséget a kimenet táblázatban nem léteznek a bemeneti sorokat törlése | Egyesítés (alapértelmezett)<br/>Csere | nem 
writeBatchSize | Amikor a writeBatchSize vagy writeBatchTimeout találati adatok beillesztése az Azure-táblából. | Egész szám (sorok száma)| Nem (alapértelmezett: 10000) 
writeBatchTimeout | Adatok beszúrása az Azure táblázatba, amikor a writeBatchSize vagy writeBatchTimeout találati | időszak<br/><br/>Példa: "00: 20:00" (20 perc) | Nem (tárhely ügyfél alapértelmezett időtúllépés alapértelmezett értéke 90 másodperc)

### <a name="azuretablepartitionkeyname"></a>azureTablePartitionKeyName
Feleltesse meg egy forrásoszlop cél oszlop a fordító JSON tulajdonság előtt a azureTablePartitionKeyName használata a célként megadott oszlop.

A következő példában a forrásoszlop DivisionID a cél oszlop van rendelve: DivisionID.  

    "translator": {
        "type": "TabularTranslator",
        "columnMappings": "DivisionID: DivisionID, FirstName: FirstName, LastName: LastName"
    } 

A partíciók billentyűt a DivisionID van megadva. 

    "sink": {
        "type": "AzureTableSink",
        "azureTablePartitionKeyName": "DivisionID",
        "writeBatchSize": 100,
        "writeBatchTimeout": "01:00:00"
    }


[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

### <a name="type-mapping-for-azure-table"></a>Azure tábla leképezésének

[Mozgás a tevékenységekre vonatkozó adatok](data-factory-data-movement-activities.md) cikkben említett, a Másolás tevékenység forrástípus gyűjtése a következő két lépésből álló módszert az egyik automatikus típus konverzió hajt végre.

1. Natív forrástípus konvertálása a .NET-típus
2. .NET-típusból származó átalakítása natív gyűjtő típusa

Mozgatásakor adatok Azure-táblából az & [hozzárendelések Azure-táblából szolgáltatás által meghatározott](https://msdn.microsoft.com/library/azure/dd179338.aspx) következő szolgálnak az Azure-táblából OData-típusok .NET típusát, és ez fordítva is igaz. 

| Az OData-adattípus | .NET-típus | Részletek |
| --------------- | --------- | ------- |
| Edm.Binary | byte] | Egy tömb bájtok számát akár 64 KB. |
| Edm.Boolean | logikai | Olyan logikai értéket. |
| Edm.DateTime | Dátum és idő | Egy 64 bites érték kifejezett, egyezményes világidő (UTC) szerint. A támogatott DateTime tartomány éjfél 12:00 január 1, 1601 I.E. kezdődik. (C.E.), EGYEZMÉNYES. A tartomány végződik December 31-én 9999. |
| Edm.Double | dupla | Egy 64 bites lebegőpontos értéket. |
| Edm.Guid | Globálisan egyedi azonosítója | 128 bites globálisan egyedi azonosítója. |
| Edm.Int32 | Int32, illetve az int | A 32 bites egész szám. |
| Edm.Int64 | Int64 vagy hosszú | Egy 64 bites egész szám. |
| Edm.String | Karakterlánc | Egy UTF-16 kódolású érték. Lehet, hogy a karakterláncok legfeljebb 64 KB. |

### <a name="type-conversion-sample"></a>Konvertálás minta

A következő példa olyan adatokat másol egy Azure Blob Azure-táblából az rendelkező típus dokumentumkonvertálás. 

Tegyük fel, hogy az Blob adatkészlet az a CSV formátum, és három oszlopot tartalmaz. Dátumidő oszlop egy egyéni datetime formátumban, a hét napjának rövidített francia nevekkel, az ezek egyikét. 

Adja meg az Blob-forrás adatkészlet következőképpen típus definíciók az oszlopok együtt.
    
    {
        "name": " AzureBlobInput",
        "properties":
        {
             "structure": 
              [
                    { "name": "userid", "type": "Int64"},
                    { "name": "name", "type": "String"},
                    { "name": "lastlogindate", "type": "Datetime", "culture": "fr-fr", "format": "ddd-MM-YYYY"}
              ],
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/myfolder",
                "fileName":"myfile.csv",
                "format":
                {
                    "type": "TextFormat",
                    "columnDelimiter": ","
                }
            },
            "external": true,
            "availability":
            {
                "frequency": "Hour",
                "interval": 1,
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

A típus hozzárendelés megadott Azure-táblából OData-típusból származó .NET típus, volna definiált a táblázat Azure-táblából az alábbi séma használata. 

**Azure táblázat séma:**

Oszlopnév | Típus
----------- | --------
felhasználóazonosító | Edm.Int64
név | Edm.String 
lastlogindate | Edm.DateTime

Ezután határozza meg az Azure-táblából adatkészlet a következőképpen. Nem kell adja meg a "struktúra" szakasz a típusú adatokkal óta a típus információ már szerepel a mögöttes adatok áruházból.

    {
      "name": "AzureTableOutput",
      "properties": {
        "type": "AzureTable",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "tableName": "MyOutputTable"
        },
        "availability": {
          "frequency": "Hour",
          "interval": 1
        }
      }
    }

Ebben az esetben Data Factory automatikusan adattípus-konverzió korlátai többek között a Datetime mezőben a Blob Azure-táblából az adatok áthelyezése a "fr-hu" kulturális környezet használata egyéni datetime formátumban.



[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

## <a name="performance-and-tuning"></a>A teljesítmény és a finombeállítása  
Főbb tényezők kapcsolatos adatok mozgását (Másolás tevékenység) az Azure Data Factory és a különféle módokon optimalizálása azt hatása teljesítmény című témakörben talál [Másolás tevékenység teljesítmény és finombeállítása útmutató](data-factory-copy-activity-performance.md).







