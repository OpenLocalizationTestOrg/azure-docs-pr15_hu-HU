<properties
    pageTitle="Ütemezés- és az adatok gyári végrehajtás |} Microsoft Azure"
    description="Ismerje meg, hogy Azure Data Factory alkalmazásmodell ütemezése és a végrehajtás szempontjait."
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
    ms.topic="article"
    ms.date="08/22/2016"
    ms.author="spelluru"/>

# <a name="data-factory-scheduling-and-execution"></a>Adatok gyári ütemezés- és végrehajtása
Ez a cikk ismerteti az Azure Data Factory alkalmazásmodell ütemezés- és végrehajtás tulajdonságát. 

## <a name="prerequisites"></a>Előfeltételek
Ez a cikk tartalma feltételezi, hogy megismeri Data Factory alkalmazás modell fogalmak megértéséhez, beleértve a tevékenységet, folyamatok, csatolt szolgáltatások és adatkészleteket alapjait. Azure Data Factory alapfogalmak az alábbi cikkekben talál:

- [Adatok gyári – bevezetés](data-factory-introduction.md)
- [Folyamatok](data-factory-create-pipelines.md)
- [Adatkészletek](data-factory-create-datasets.md) 

## <a name="schedule-an-activity"></a>Tevékenység ütemezése

A tevékenység JSON ütemező csoporttal egy tevékenység ismétlődő ütemezést is megadhat. Ha például ütemezhet tevékenység óránként az alábbi képlettel történik:

    "scheduler": {
        "frequency": "Hour",
        "interval": 1
    },  

![Példa ütemezőt](./media/data-factory-scheduling-and-execution/scheduler-example.png)

Ahogy a diagramot, adja meg a tevékenység ütemezése hoz létre a tumbling windows sorozata. Tumbling windows rendszer rögzített méretű, nem egymást átfedő, összefüggő időtartományok sorozata. A logikai tumbling windows a tevékenységhez tartozó *tevékenység windows*nevezik.

Az aktuális végrehajtási tevékenység ablak az időszakot, a tevékenység ablak [WindowStart](data-factory-functions-variables.md#data-factory-system-variables) és [WindowEnd](data-factory-functions-variables.md#data-factory-system-variables) rendszer változóval JSON tevékenységben rendelt érheti el. Ezek a változók különböző célokra a tevékenység JSON használata. Ha például is használhatja őket bemeneti és kimeneti adatkészleteket, amely idő adatsor összegyűjteni az adatokat.

Az **Ütemező** tulajdonság támogatja az azonos altulajdonságok az **Elérhetőség** tulajdonságként adatkészletet. [Adatkészlet elérhetősége](data-factory-create-datasets.md#Availability) talál további információt. Példák: egy adott időpontban eltolás a ütemezési, vagy feldolgozás elején vagy a tevékenység ablakban az időszak végén igazíthatja a mód beállítást.

Megadhatja, hogy egy tevékenység **Ütemező** tulajdonságainak, de ez a tulajdonság nem **kötelező**. Ha megad egy tulajdonság, annak egyeznie kell az adja meg, a kimeneti adatkészlet definition cadence. Kimeneti adatkészlet jelenleg milyen meghajtók az ütemezést, így akkor is, ha a tevékenység nem hozhatók létre bármely kimeneti létre kell hoznia egy kimenet adatkészlet. Ha a tevékenység bármely bevitel nem kerül, kihagyhatja a bemeneti adatkészlet létrehozása.

## <a name="time-series-datasets-and-data-slices"></a>Idő sorozat adatkészleteket, és az adatok szeletek

Idő adatsor a folyamatos sorozatában adatpontok, amely általában egymást követő mérések időintervallum fölé. Gyakori idő adatsor többek között szenzoradatokat és az alkalmazás telemetriai adatokat.

Az adatok gyári Listaszerű folyamatábra adatsor tevékenységeket használó kötegelt módon futtatásakor. Általában nem egy ismétlődő cadence bemeneti adatok megérkezik és kimeneti adatokat kell mutatni. Ez a cadence modellezni megadásával **Elérhetőség** az adatkészlet az alábbi képlettel történik:

    "availability": {
      "frequency": "Hour",
      "interval": 1
    },

Adatok elfogyasztott és egy tevékenység Futtatás által létrehozott minden egyes mértékegység egy szeletre neve. A következő ábrán egy példa egy beviteli adatkészlet a tevékenységet, és egy kimeneti adatkészlet. Ezek adatkészleteket **Elérhetőség** az óránkénti gyakoriság értékűre van.

![Elérhetőség ütemező](./media/data-factory-scheduling-and-execution/availability-scheduler.png)

A fenti ábrán óránkénti adatok szeletet a bemeneti és kimeneti adatkészlet számára. A diagram, amely készen áll a feldolgozás három beviteli szeletek jeleníti meg. A 10-11-es AM tevékenység már folyamatban van, a 10-11-es AM kimeneti szeletet előállító.

Az időszakot, az aktuális szelet alatt az adatkészlet JSON változóval [SliceStart](data-factory-functions-variables.md#data-factory-system-variables) és [SliceEnd](data-factory-functions-variables.md#data-factory-system-variables)előállított társított érheti el.

Jelenleg Data Factory igényel, hogy pontosan meghatározott a tevékenység az ütemterv megfelel-e az ütemezésben, a kimeneti adatkészlet **elérhetősége** megadott. Ezért **WindowStart**, **WindowEnd**, **SliceStart**és **SliceEnd** mindig rendelje hozzá az egy időszakra és egy kimeneti szelet.

Érhetők el a elérhetősége különböző tulajdonságok kapcsolatos további tudnivalókért olvassa el a [adatkészleteket létrehozása](data-factory-create-datasets.md)című témakört.

## <a name="move-data-from-sql-database-to-blob-storage"></a>Adatok áthelyezése SQL-adatbázisból Blob-tárolóhoz

Vegyük helyezhető el néhány dolog, amit közös és működését: hozzon létre egy folyamat, amely az adatokat másolja az SQL Azure-adatbázis táblából Azure Blob-tárolóhoz óránként.

**Beviteli: Azure SQL-adatbázis adatkészlet**

    {
        "name": "AzureSqlInput",
        "properties": {
            "published": false,
            "type": "AzureSqlTable",
            "linkedServiceName": "AzureSqlLinkedService",
            "typeProperties": {
                "tableName": "MyTable"
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true,
            "policy": {}
        }
    }


**Gyakoriság** értéke **órát** , és **intervallum** a elérhetősége értéke **1** .

**Kimenet: Azure Blob-tároló adatkészlet**

    {
        "name": "AzureBlobOutput",
        "properties": {
            "published": false,
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
                "folderPath": "mypath/{Year}/{Month}/{Day}/{Hour}",
                "format": {
                    "type": "TextFormat"
                },
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
                            "format": "%M"
                        }
                    },
                    {
                        "name": "Day",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "%d"
                        }
                    },
                    {
                        "name": "Hour",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "%H"
                        }
                    }
                ]
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }


**Gyakoriság** értéke **órát** , és **intervallum** a elérhetősége értéke **1** .



**Tevékenység: Másolás tevékenység**

    {
        "name": "SamplePipeline",
        "properties": {
            "description": "copy activity",
            "activities": [
                {
                    "type": "Copy",
                    "name": "AzureSQLtoBlob",
                    "description": "copy activity",
                    "typeProperties": {
                        "source": {
                            "type": "SqlSource",
                            "sqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 100000,
                            "writeBatchTimeout": "00:05:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "AzureSQLInput"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobOutput"
                        }
                    ],
                    "scheduler": {
                        "frequency": "Hour",
                        "interval": 1
                    }
                }
            ],
            "start": "2015-01-01T08:00:00Z",
            "end": "2015-01-01T11:00:00Z"
        }
    }


A példa bemutatja a tevékenység ütemezése, és az óránkénti gyakoriság adatkészlet elérhetősége szakaszok állítva. A minta jeleníti meg, hogyan használható **WindowStart** , és futtassa **WindowEnd** jelölje ki a tevékenységet vonatkozó adatokat, és másolja a vágólapra a megfelelő **Mappa_útvonala**a blob. A **Mappa_útvonala** akkor paraméteres, hogy az óránkénti külön mappát.

Három, 8 – 11 AM közötti szelet végrehajtani, amikor az Azure SQL-adatbázis adatai az alábbi képlettel történik:

![Minta beviteli](./media/data-factory-scheduling-and-execution/sample-input-data.png)

A folyamat üzembe helyezése, miután az Azure blob töltve az alábbi képlettel történik:

-   A fájl mypath/2015/1/1/8/adatokat. &lt;Globálisan egyedi azonosítója&gt;.txt adatokkal

            10002345,334,2,2015-01-01 08:24:00.3130000
            10002345,347,15,2015-01-01 08:24:00.6570000
            10991568,2,7,2015-01-01 08:56:34.5300000

    > [AZURE.NOTE] &lt;Globálisan egyedi azonosítója&gt; van egy tényleges globálisan egyedi azonosítója cseréli. Példa fájlnév: Data.bcde1348-7620-4f93-bb89-0eed3455890b.txt
-   A fájl mypath/2015/1/1/9/adatokat. &lt;Globálisan egyedi azonosítója&gt;.txt adatokkal:

            10002345,334,1,2015-01-01 09:13:00.3900000
            24379245,569,23,2015-01-01 09:25:00.3130000
            16777799,21,115,2015-01-01 09:47:34.3130000
-   A fájl mypath/2015/1/1/10/adatokat. &lt;Globálisan egyedi azonosítója&gt;adatokat nem tartalmazó .txt.


## <a name="active-period-for-pipeline"></a>Tölcsér aktív időszak

[Folyamatok létrehozása](data-factory-create-pipelines.md) , amely egy aktív időszakban az egy folyamat a **kezdete** és **vége** tulajdonság beállításával megadott a jelent meg.

Beállíthatja, hogy a folyamat aktív időszak kezdő dátuma múltbeli. Adatok gyári automatikusan számítja ki (vissza kitöltések) az összes adat szeletek múltbeli, és megkezdi a feldolgozása őket.

## <a name="parallel-processing-of-data-slices"></a>Adatok szeletek párhuzamos feldolgozása
A házirend szakaszában a tevékenységet JSON **feldolgozási** tulajdonság beállításával párhuzamosan futó vissza kitöltött adatok szeletek is beállíthatja. Ez a tulajdonság kapcsolatos további tudnivalókért olvassa el a [folyamatok létrehozása](data-factory-create-pipelines.md)című témakört.

## <a name="rerun-a-failed-data-slice"></a>Futtassa újra a sikertelen szeletre 
Szeletek végrehajtását figyelheti a sokoldalú, vizuális útján. Lásd: a [Figyelés és Azure portál pengéit használatával folyamatok kezelése](data-factory-monitor-manage-pipelines.md) vagy [figyelő és felügyeleti alkalmazás](data-factory-monitor-manage-app.md) további információt.

Vegye figyelembe az alábbi példa, amely két tevékenységet jeleníti meg. Activity1 szeletek idő sorozat adatkészletet felhasznált bemeneteként kapcsol az kimenetként idő sorozat adatkészlet Activity2 kimenetként hoz létre.

![Nem sikerült szelet](./media/data-factory-scheduling-and-execution/failed-slice.png)

A diagram azt mutatja, hogy ki három legutóbbi szeletek, rajta a 9-10 AM szeletet Dataset2 hiba történt. Adatok gyári automatikusan nyomon követi a függőséget az az idő adatsor adatkészlet. Emiatt nem indul el a tevékenységet, futtassa a 9-10: AM lefelé irányuló szeletet.

Adatok gyári figyelő és felügyeleti eszközök hatoljon le a diagnosztikai naplók az egyszerűen a kiváltóok keresse meg a problémát, és javítsa ki a hibás szeletek teszi lehetővé. A probléma van rögzített, akkor egyszerűen elkezdje a tevékenységet, futtassa a sikertelen szeletet kapcsol. Futtassa újra és állapot áttűnések adatok szeletek megértéséhez, lásd: [Figyelés és Azure portál pengéit használatával folyamatok kezelése](data-factory-monitor-manage-pipelines.md) vagy [figyelő és felügyeleti alkalmazás](data-factory-monitor-manage-app.md).

Után a 9-10: AM szeletet az **Dataset2**futtassa újra, adatok gyári a Futtatás a 9-10 AM függő szeletet-az utolsó adatkészlet kezdi.

![Nem sikerült szeletet ismétlése](./media/data-factory-scheduling-and-execution/rerun-failed-slice.png)

## <a name="run-activities-in-a-sequence"></a>Futtassa a tevékenységek sorrendben
A kimenet adatkészlet egy tevékenység beállításával, mint a bemeneti adatkészlet a többi tevékenység (az egyik tevékenység futtatása egymás után), a két tevékenység fűzhetők össze. A tevékenységek lehet azonos vagy más folyamatok. A második tevékenység hajt végre, csak ha az első szakasz sikeres befejeződését követően.

Például érdemes megvizsgálni a következő esetben:

1.  Folyamat P1 tevékenység A1, amely a külső beviteli adatkészlet D1 igényel, és a kimeneti adatkészlet D2 eredménye tartalmaz.
2.  Folyamat P2 tevékenység A2, az adatkészlet D2 beavatkozást igényel, és a kimeneti adatkészlet D3 eredménye tartalmaz.

Ebben az esetben az A1 és A2 tevékenységek különböző folyamatok szerepelnek. A tevékenység a A1 fut, amikor érhető el a külső adatok és az ütemezett elérhetősége gyakoriság eléri. A tevékenység a A2 fut, amikor a D2 ütemezett szeletet elérhetővé válnak, és az ütemezett elérhetősége gyakoriság eléri. Egy adathalmaz D2 szeleteinek hiba esetén A2 nem működik a kör közepétől mindaddig, amíg elérhetővé válik.

A Diagram nézetben a következő diagramon jelenne meg:

![A két folyamatok láncolási tevékenységek](./media/data-factory-scheduling-and-execution/chaining-two-pipelines.png)

A korábban említett a tevékenységek lehet a azonos során. A Diagram nézetben a két tevékenység a azonos során a következő diagramon jelenne meg:

![Az azonos folyamat láncolási tevékenységek](./media/data-factory-scheduling-and-execution/chaining-one-pipeline.png)

### <a name="copy-sequentially"></a>Egymás után másolása
Futtatásához több másolatot műveletek egymás után/szekvenciális rendezett módon lehetőség. Például lehet, hogy két másolás tevékenységek során (CopyActivity1 és CopyActivity2), a következő bemeneti adatok kimeneti adatkészleteket:   

CopyActivity1

Beviteli: adatkészlet. Kimenet: Dataset2.

CopyActivity2

Beviteli: Dataset2.  Kimenet: Dataset3.

CopyActivity2 szeretné futtatni, csak akkor, ha a CopyActivity1 sikeresen lefut és Dataset2 elérhető.

Az alábbiakban a minta folyamat JSON:

    {
        "name": "ChainActivities",
        "properties": {
            "description": "Run activities in sequence",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "BlobSource"
                        },
                        "sink": {
                            "type": "BlobSink",
                            "copyBehavior": "PreserveHierarchy",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "Dataset1"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "Dataset2"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00"
                    },
                    "scheduler": {
                        "frequency": "Hour",
                        "interval": 1
                    },
                    "name": "CopyFromBlob1ToBlob2",
                    "description": "Copy data from a blob to another"
                },
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "BlobSource"
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "Dataset2"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "Dataset3"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00"
                    },
                    "scheduler": {
                        "frequency": "Hour",
                        "interval": 1
                    },
                    "name": "CopyFromBlob2ToBlob3",
                    "description": "Copy data from a blob to another"
                }
            ],
            "start": "2016-08-25T01:00:00Z",
            "end": "2016-08-25T01:00:00Z",
            "isPaused": false
        }
    }

Figyelje meg, hogy a példában az első példányt tevékenység (Dataset2) a kimeneti adatkészlet meg van adva a második tevékenység bemenetként. Emiatt a második tevékenység futtatja, csak akkor, amikor készen áll-e a kimeneti adatkészlet: az első tevékenységhez.  

A példában szereplő CopyActivity2 beállíthatja, hogy egy másik beviteli Dataset3, például, de Dataset2 CopyActivity2, a bemeneti adataiként megadhatja, hogy a tevékenység nem fut, amíg be nem fejeződik CopyActivity1. Példa:

CopyActivity1

Beviteli: Dataset1. Kimenet: Dataset2.

CopyActivity2

Bemenetben: Dataset3, Dataset2. Kimenet: Dataset4.

    {
        "name": "ChainActivities",
        "properties": {
            "description": "Run activities in sequence",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "BlobSource"
                        },
                        "sink": {
                            "type": "BlobSink",
                            "copyBehavior": "PreserveHierarchy",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "Dataset1"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "Dataset2"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00"
                    },
                    "scheduler": {
                        "frequency": "Hour",
                        "interval": 1
                    },
                    "name": "CopyFromBlobToBlob",
                    "description": "Copy data from a blob to another"
                },
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "BlobSource"
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "Dataset3"
                        },
                        {
                            "name": "Dataset2"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "Dataset4"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00"
                    },
                    "scheduler": {
                        "frequency": "Hour",
                        "interval": 1
                    },
                    "name": "CopyFromBlob3ToBlob4",
                    "description": "Copy data from a blob to another"
                }
            ],
            "start": "2017-04-25T01:00:00Z",
            "end": "2017-04-25T01:00:00Z",
            "isPaused": false
        }
    }


Figyelje meg, hogy a példában a két bemeneti adatkészleteket adható a második másolás tevékenységhez. Ha több bemenetben meg vannak adva, csak az első beviteli adatkészlet szolgál az adatok másolása, de más adatkészleteket függőségek használják. CopyActivity2 kezdene csak azt követően a következő feltételek teljesülése:

- CopyActivity1 sikeresen befejeződött, és Dataset2 érhető el. Ez az adatkészlet Dataset4 adatok másolásakor nem használható. Ez csak végpontként ütemezési függőség CopyActivity2 az.   
- Dataset3 áll rendelkezésre. Ez az adatkészlet az adatokat a cél a másolt jelöli.  



## <a name="model-datasets-with-different-frequencies"></a>Eltérő gyakorisággal modell adatkészleteket

A minták a bemeneti és kimeneti adatkészleteket, és a tevékenység ütemezése ablak gyakoriságok azonos volt. Bizonyos esetekben csak az azt jelenti, hogy a kimeneti ugyanaz, mint a gyakoriságok egy vagy több ráfordítások gyakorisággal kiszámítására. Adatok gyári támogatja a forgatókönyvekben modellezése.

### <a name="sample-1-produce-a-daily-output-report-for-input-data-that-is-available-every-hour"></a>Minta 1: A bemeneti elérhető adatok óránként napi kimeneti jelentést készít.

Fontolja meg, amelyben meg kell mérési adatok beviteléhez érzékelőktől elérhető Azure Blob-tárolóban lévő óránként példa. A statisztikai adatokat, például az átlag, a legnagyobb és legkisebb napi összesítő jelentést készít a nap, az [adatok gyári struktúra tevékenység](data-factory-hive-activity.md)szeretne.

Az alábbiakban hogyan ebben az esetben az adatok gyári modellezheti:

**Beviteli adatkészlet**

Az óránkénti bemeneti fájlok megszakadnak a mappa az adott napra. Elérhetőség adatbevitelt megadása **óra** (gyakoriság: óra, intervallum: 1).

    {
      "name": "AzureBlobInput",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
          "partitionedBy": [
            { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
            { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "%M"}},
            { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "%d"}}
          ],
          "format": {
            "type": "TextFormat"
          }
        },
        "external": true,
        "availability": {
          "frequency": "Hour",
          "interval": 1
        }
      }
    }

**Kimeneti adatkészlet**

Egy kimeneti fájl minden nap a nap mappában jön létre. Kimeneti elérhetősége megadása **nap** (gyakoriság: nap és intervallum: 1).


    {
      "name": "AzureBlobOutput",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
          "partitionedBy": [
            { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
            { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "%M"}},
            { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "%d"}}
          ],
          "format": {
            "type": "TextFormat"
          }
        },
        "availability": {
          "frequency": "Day",
          "interval": 1
        }
      }
    }

**Tevékenység: egy folyamat struktúra tevékenység**

A struktúra parancsfájl kapja meg a megfelelő *DateTime* adatait a **WindowStart** változó használja az alábbi kódtöredékének látható paraméterként. A struktúra parancsfájl a változó betöltheti az adatokat a megfelelő mappát a nap, és futtassa az összesítés a kimenet létrehozásához használja.

        {  
            "name":"SamplePipeline",
            "properties":{  
            "start":"2015-01-01T08:00:00",
            "end":"2015-01-01T11:00:00",
            "description":"hive activity",
            "activities": [
                {
                    "name": "SampleHiveActivity",
                    "inputs": [
                        {
                            "name": "AzureBlobInput"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobOutput"
                        }
                    ],
                    "linkedServiceName": "HDInsightLinkedService",
                    "type": "HDInsightHive",
                    "typeProperties": {
                        "scriptPath": "adftutorial\\hivequery.hql",
                        "scriptLinkedService": "StorageLinkedService",
                        "defines": {
                            "Year": "$$Text.Format('{0:yyyy}',WindowStart)",
                            "Month": "$$Text.Format('{0:%M}',WindowStart)",
                            "Day": "$$Text.Format('{0:%d}',WindowStart)"
                        }
                    },
                    "scheduler": {
                        "frequency": "Day",
                        "interval": 1
                    },          
                    "policy": {
                        "concurrency": 1,
                        "executionPriorityOrder": "OldestFirst",
                        "retry": 2,
                        "timeout": "01:00:00"
                    }
                 }
             ]
           }
        }

Az alábbi ábra mutatja az alkalmazási példát, a adatok-függőség szempontjából.

![Adatok függőség](./media/data-factory-scheduling-and-execution/data-dependency.png)

A kimenet szeletet minden nap egy beviteli adatkészlet a 24 óránként szeletek függ. Adatok gyári függőségeket automatikusan meg a bemeneti adatok böngészésére, amely az idő alatt, a kimeneti szeletet készítendő szeletek számítja ki. A 24 beviteli szeletek közül nem érhető el, ha Data Factory megvárja, a bemeneti szeletet készen álljon a Futtatás napi tevékenység megkezdése előtt.


### <a name="sample-2-specify-dependency-with-expressions-and-data-factory-functions"></a>Minta 2: Adja meg a függőség a kifejezések és a Data Factory függvények

Nézzünk meg egy másik forgatókönyv. Tegyük fel, hogy van-e a struktúra tevékenységének, amely a két bemeneti adatkészleteket dolgoz fel. Azok közül az új adatok naponta rendelkezik, de ezek egyikét kapja új adatok hetente. Tegyük fel, hogy végezze el a csatlakozás végig a két bemeneti adatok alapján, és készít egy kimenet mindennap szeretett volna.

Melyik Data Factory egyszerű megközelítése automatikusan ábrák, jobbra a szövegbeviteli szeletek az adatok szeletet idő időszak nem működik a kimeneti igazításával feldolgozása.

Meg kell adnia, hogy minden tevékenység futtatása, az adatok gyári kell használni a múlt héten szeletre a heti beviteli adatkészlet. Segítségével Azure Data Factory funkciók a következő kódtöredékének látható módon megoldásához végrehajtja.

**Input1: Azure blob**

Az első bemeneti érték az Azure blob-napi frissítést.

    {
      "name": "AzureBlobInputDaily",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
          "partitionedBy": [
            { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
            { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "%M"}},
            { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "%d"}}
          ],
          "format": {
            "type": "TextFormat"
          }
        },
        "external": true,
        "availability": {
          "frequency": "Day",
          "interval": 1
        }
      }
    }

**Input2: Azure blob**

Input2 az Azure blob-heti frissítést.

    {
      "name": "AzureBlobInputWeekly",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
          "partitionedBy": [
            { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
            { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "%M"}},
            { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "%d"}}
          ],
          "format": {
            "type": "TextFormat"
          }
        },
        "external": true,
        "availability": {
          "frequency": "Day",
          "interval": 7
        }
      }
    }

**Kimenet: Azure blob**

Egy kimenő fájl mappában jön létre minden nap az adott nap. Kimeneti elérhetőségének beállítása **nap** (gyakoriság: nap, intervallum: 1).

    {
      "name": "AzureBlobOutputDaily",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
          "partitionedBy": [
            { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
            { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "%M"}},
            { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "%d"}}
          ],
          "format": {
            "type": "TextFormat"
          }
        },
        "availability": {
          "frequency": "Day",
          "interval": 1
        }
      }
    }

**Tevékenység: egy folyamat struktúra tevékenység**

A struktúra tevékenység megnyitja a két bemeneti adatok alapján, és készít egy kimenet szeletet minden nap. Megadhatja, hogy minden nap kimeneti szelet attól függenek, az előző hét beviteli szeletet heti adatbevitelt az alábbi képlettel történik.

    {  
        "name":"SamplePipeline",
        "properties":{  
        "start":"2015-01-01T08:00:00",
        "end":"2015-01-01T11:00:00",
        "description":"hive activity",
        "activities": [
          {
            "name": "SampleHiveActivity",
            "inputs": [
              {
                "name": "AzureBlobInputDaily"
              },
              {
                "name": "AzureBlobInputWeekly",
                "startTime": "Date.AddDays(SliceStart, - Date.DayOfWeek(SliceStart))",
                "endTime": "Date.AddDays(SliceEnd,  -Date.DayOfWeek(SliceEnd))"  
              }
            ],
            "outputs": [
              {
                "name": "AzureBlobOutputDaily"
              }
            ],
            "linkedServiceName": "HDInsightLinkedService",
            "type": "HDInsightHive",
            "typeProperties": {
              "scriptPath": "adftutorial\\hivequery.hql",
              "scriptLinkedService": "StorageLinkedService",
              "defines": {
                "Year": "$$Text.Format('{0:yyyy}',WindowStart)",
                "Month": "$$Text.Format('{0:%M}',WindowStart)",
                "Day": "$$Text.Format('{0:%d}',WindowStart)"
              }
            },
            "scheduler": {
              "frequency": "Day",
              "interval": 1
            },          
            "policy": {
              "concurrency": 1,
              "executionPriorityOrder": "OldestFirst",
              "retry": 2,  
              "timeout": "01:00:00"
            }
           }
         ]
       }
    }


## <a name="data-factory-functions-and-system-variables"></a>Gyári adatfüggvényei és a rendszer változói   

Című témakörben [Data Factory függvények és a rendszer változók](data-factory-functions-variables.md) és a rendszer változói, amely támogatja az adatok gyári.

## <a name="data-dependency-deep-dive"></a>Adatok-mély merülési függőség

Szeretne egy adatkészlet szeletet egy tevékenység Futtatás szerint, adatok gyári használja az alábbi *típusú függés modell* határozza meg az elfogyasztott és a tevékenység készített adathalmazok közötti kapcsolatokat.

Időtartomány kötelező, a kimeneti adatkészlet szeletek létrehozásához a beviteli adatkészletek *függőség időszak*neve.

Egy tevékenység Futtatás adatkészlet szelet hoz létre, csak azt követően a bemeneti adatkészleteket függőség időszakán belül az adatok szeletek érhetők el. Más szóval függőség időszak alkotó beviteli szeletet **készen áll** a tevékenység állapotban kell lennie az összes futtatása egy kimenet adatkészlet szeletek létrehozásához.

Az adatkészlet szelet [**indítása**, **vége**] szeretne létrehozni, függvény az adatkészlet szelet kell rendelni az függőség időszak alatt. Ez a funkció akkor lényegében, ha a képletet, amely a kezdési és függőség időszak kezdete és vége az adatkészlet szelet konvertál. További hivatalos:

    DatasetSlice = [start, end]
    DependecyPeriod = [f(start, end), g(start, end)]

**F** és **g** vannak leképezés alapján számítja ki a kezdő és az egyes tevékenységek bemeneti függőség időszak függvényeket.

Minta naplójában függőség időszak megegyezik a létrehozott szeletre időszakra. Ezekben az esetekben Data Factory automatikusan a függőség időszakra eső beviteli szeletet számítja ki.  

A mintában aggregáció hol kimeneti a naponta érkezik, a bemeneti adatok érhető el Óránként, például az adatok szeletet időszak 24 óra is. Adatok gyári megtalálja a megfelelő óránkénti bemeneti szeletre, a következő időszakban jelölőnégyzetet, és lehetővé teszi a kimeneti szeletet a bemeneti szeletet függ.

Akkor is megadhatja, saját hozzárendelése a függőség időszakra, ahogy azt a minta, ha a bemenetben egyik heti és a kimeneti szeletet naponta elő.

## <a name="data-dependency-and-validation"></a>Adatok függőség és érvényesítése

Egy adathalmaz egy definiált érvényességi házirendet, amely meghatározza, hogy hogyan egy szeletet végrehajtás által létrehozott adatokat is ellenőrizni kell, mielőtt fogyasztási készen is van. Lásd: [létrehozása adatkészleteket](data-factory-create-datasets.md) további információt.

Ebben az esetben, a szeletet végrehajtás befejeződése után a kimenő szeletet állapotúra változik **Várakozás** az **érvényesítési**egy részállapot. A szeletek vannak érvényesítése után a szelet állapotra változik **készen áll**.

Ha egy szeletre készült, de nem felelt meg az érvényesítési, tevékenység fut ez a cikk szöge függő lefelé irányuló szeletek feldolgozása nem.

[Monitor és folyamatok kezelése](data-factory-monitor-manage-pipelines.md) adatok gyári körcikkei adatok különböző államban foglalkozik.

## <a name="external-data"></a>Külső adatok

Egy adathalmaz külső (ahogy az alábbi JSON kódtöredékének), ami azt jelenti, nem hozta létre Data Factory jelölhető ki. Ebben az esetben az adatkészlet házirend paraméterek érvényességi leíró további beállított, és ismételje meg az adatkészlet házirend. [Folyamatok létrehozása](data-factory-create-pipelines.md) látható minden tulajdonság leírását.

Hasonló adatokat gyári által gyártott adatkészleteket, külső adatokhoz az adatok szeletek kell lennie, készen áll arra, mielőtt függő szeletek dolgozható.

    {
        "name": "AzureSqlInput",
        "properties":
        {
            "type": "AzureSqlTable",
            "linkedServiceName": "AzureSqlLinkedService",
            "typeProperties":
            {
                "tableName": "MyTable"
            },
            "availability":
            {
                "frequency": "Hour",
                "interval": 1     
            },
            "external": true,
            "policy":
            {
                "externalData":
                {
                    "retryInterval": "00:01:00",
                    "retryTimeout": "00:10:00",
                    "maximumRetry": 3
                }
            }  
        }
    }


## <a name="onetime-pipeline"></a>Alkalommal folyamat
Létrehozhat és a folyamat futtatása rendszeres ütemezése (például: óránként vagy naponta) a folyamat meghatározása ad meg a kezdő és záró időpont belül. [A tevékenységek ütemezési](#scheduling-and-execution) talál további információt. Egy folyamat csak egyszer futó is létrehozhat. Ehhez a **pipelineMode** tulajdonság beállítja a folyamat definíció, **alkalommal** a következő JSON minta látható módon. Ez a tulajdonság alapértelmezett értéke **ütemezett**.

    {
        "name": "CopyPipeline",
        "properties": {
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "BlobSource",
                            "recursive": false
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "InputDataset"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "OutputDataset"
                        }
                    ]
                    "name": "CopyActivity-0"
                }
            ]
            "pipelineMode": "OneTime"
        }
    }

Vegye figyelembe az alábbiakat:

- A folyamat **elindítása** és a **Záró** időpontjának nincs megadva.
- **Elérhetőség** bemeneti és kimeneti adatkészleteket megadva a (**gyakoriság** és **interval**), akkor is, ha az adatok gyári nem használja az értékeket.  
- Diagram nézetben a egyszeri folyamatok ne jelenjen meg. Ez a jelenség szándékosan van így.
- Nem lehet frissíteni az egyszeri folyamatok. Egy egyszeri folyamat klónozhatja, nevezze át, módosítása és azokat a másikat is hozhat létre.
