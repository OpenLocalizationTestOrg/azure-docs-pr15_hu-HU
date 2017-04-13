<properties 
    pageTitle="Adatkészletek létrehozása az Azure Data Factory |} Microsoft Azure" 
    description="Megtudhatja, hogy miként adatkészleteket létrehozása az Azure Data Factory példák, amelyek tulajdonságait, például az eltolás és anchorDateTime."
    keywords="adatkészlet, például adatkészlet létrehozása, például eltolása"
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
    ms.date="09/13/2016" 
    ms.author="shlo"/>

# <a name="datasets-in-azure-data-factory"></a>Az Azure Data Factory adatkészleteket
Ez a cikk ismerteti az Azure Data Factory adatkészleteket, és eltolás, anchorDateTime és eltolás/stílus adatbázisok például példákat.

Amikor létrehoz egy adatkészletet, az adatok feldolgozása, amelyet mutató készít. Adatok feldolgozása (bemeneti és kimeneti) tevékenységet, majd a tevékenység egy folyamat lévő. Egy beviteli adatkészlet egy tevékenység, a során a bemeneti pedig egy kimenet adatkészlet a kimenet a tevékenység.

Adatkészleteket azonosítása: a különböző tárolja, például a táblázatok, fájlok, mappák és a dokumentumok adatait. Miután létrehozott egy adatkészletet, használhatja a tevékenységekhez egy során. Egy adathalmaz például egy példány vagy egy HDInsightHive tevékenység, a bemeneti és kimeneti adatkészlet is lehet. Az Azure portal lehetővé teszi az adatok és folyamatok ráfordítások és a kimeneti értékeket vizuális formában. Ablaktáblában egy szempillantás alatt megjelenik a kapcsolatok és a folyamatok, függőségek minden forrás hogy mindig képben ahonnan adatokat érkezik, és hol fogom.

Az Azure Data Factory, amely letölthető adatok egy adatkészlet egy során másolatot tevékenység használatával.

> [AZURE.NOTE] Ha még kezdő az Azure Data Factory, című témakörben [Azure Data Factory](data-factory-introduction.md) Azure Data Factory szolgáltatás áttekintése. Lásd: [az első adatok gyári összeállítása](data-factory-build-your-first-pipeline.md) oktatóanyagot hozhat létre az adatok első gyári. E két cikkekben háttér-e ez a cikk jobb megértéséhez szükséges információkat. 

## <a name="define-datasets"></a>Adatkészletek meghatározása
Az Azure Data Factory egy adathalmaz következők: 


    {
        "name": "<name of dataset>",
        "properties": {
            "type": "<type of dataset: AzureBlob, AzureSql etc...>",
            "external": <boolean flag to indicate external data. only for input datasets>,
            "linkedServiceName": "<Name of the linked service that refers to a data store.>",
            "structure": [
                {
                    "name": "<Name of the column>",
                    "type": "<Name of the type>"
                }
            ],
            "typeProperties": {
                "<type specific property>": "<value>",
                "<type specific property 2>": "<value 2>",
            },
            "availability": {
                "frequency": "<Specifies the time unit for data slice production. Supported frequency: Minute, Hour, Day, Week, Month>",
                "interval": "<Specifies the interval within the defined frequency. For example, frequency set to 'Hour' and interval set to 1 indicates that new data slices should be produced hourly>"
            },
           "policy": 
            {      
            }
        }
    }

Az alábbi táblázat ismerteti a fenti JSON tulajdonságok:   

| A tulajdonság | Leírás | Szükséges | Alapértelmezett |
| -------- | ----------- | -------- | ------- |
| név | Az adatkészlet nevét. Lásd: [Azure adatok Factory - elnevezési szabályai](data-factory-naming-rules.md) elnevezési szabályokat. | igen | HIÁNYZIK |
| típus | Az adatkészlet típusát. Adja meg az Azure Data Factory által támogatott típusok közül (például: AzureBlob, AzureSqlTable). <br/><br/>[Adatkészlet típus](#Type) talál további információt. | igen | HIÁNYZIK |
| struktúra | Az adathalmaz séma<br/><br/>Részletekért lásd az [Adatkészlet struktúra](#Structure) szakaszban. | nem. | HIÁNYZIK |
| typeProperties | A kijelölt típusának megfelelő tulajdonságait. Támogatott fájltípusok és tulajdonságaik kapcsolatos részletekért lásd: [Adatkészlet típusa](#Type) szakaszban. | igen | HIÁNYZIK |
| külső | Logikai jelző megadhatja, hogy egy adatok gyári folyamat kifejezetten előállított egy adatkészlet-e.  | nem | hamis | 
| elérhetőség | Határozza meg a feldolgozás ablak vagy az adatkészlet előállításához slicing modell. <br/><br/>Részletekért lásd az [Adatkészlet elérhetősége](#Availability) szakaszban. <br/><br/>Az adatkészlet szeletelés modell, lásd: [értekezlet ütemezése, illetve végrehajtás](data-factory-scheduling-and-execution.md) cikk részletesen. | igen | HIÁNYZIK
| házirend | A feltétel vagy az adatkészlet szeletek meg kell felelniük feltétel határozza meg. <br/><br/>A részletekért lásd: az [Adatkészlet házirend](#Policy) szakaszban. | nem | HIÁNYZIK |

## <a name="dataset-example"></a>Adatkészlet példa
Az alábbi példa az adatkészlet **táblanév** **Azure SQL-adatbázis**nevű táblázat jelöl. 

    {
        "name": "DatasetSample",
        "properties": {
            "type": "AzureSqlTable",
            "linkedServiceName": "AzureSqlLinkedService",
            "typeProperties": 
            {
                "tableName": "MyTable"
            },
            "availability": 
            {
                "frequency": "Day",
                "interval": 1
            }
        }
    }

Vegye figyelembe az alábbiakat: 

- típus értéke AzureSqlTable.
- Táblanév típusa (adott AzureSqlTable típusa) tulajdonsága táblanév.
- a csatolt típusú szolgáltatás AzureSqlDatabase linkedServiceName hivatkozik. Lásd a következő csatolt szolgáltatás. 
- elérhetőség gyakoriság értéke nap, és intervallum értéke 1, ami azt jelenti, hogy a szeletet naponta készül.  

AzureSqlLinkedService a következők:

    {
        "name": "AzureSqlLinkedService",
        "properties": {
            "type": "AzureSqlDatabase",
            "description": "",
            "typeProperties": {
                "connectionString": "Data Source=tcp:<servername>.database.windows.net,1433;Initial Catalog=<databasename>;User ID=<username>@<servername>;Password=<password>;Integrated Security=False;Encrypt=True;Connect Timeout=30"
            }
        }
    }

A fenti JSON: a 

- AzureSqlDatabase típusának beállítása
- connectionString típusa tulajdonság megadja az adatokat egy Azure SQL-adatbázishoz való csatlakozáshoz.  


Amint látható, a csatolt szolgáltatás definiálja, hogyan az Azure SQL-adatbázishoz való csatlakozáshoz. Az adatkészlet határozza meg, hogy melyik táblát egy bemeneti szolgál a tevékenység egy során. A [folyamat](data-factory-create-pipelines.md) JSON tevékenység szakaszában adja meg, hogy az adatkészlet-bemeneti és kimeneti adatkészlet használja.


> [AZURE.IMPORTANT] Egy adathalmaz Azure Data Factory készített, kivéve akkor kell megjelölni **külső**. Ez a beállítás általában a folyamat első tevékenység bemenetben vonatkozik.   

## <a name="Type"></a>Adatkészlet típusa
A támogatott adatforrások és adatkészlet típusok igazítva. A [Mozgás a tevékenységekre vonatkozó adatok](data-factory-data-movement-activities.md#supported-data-stores) a cikkben információt típusok és konfigurációs adatkészletek hivatkozott témakörökben. Például ha Azure SQL-adatbázisból származó adatok használata esetén kattintson Azure SQL-adatbázis a listáját a támogatott adatokat tárolja a részletes adatok megjelenítéséhez.  

## <a name="Structure"></a>Adatkészlet-struktúra
A **szerkezet** szakaszban határozza meg az adatkészlet sémája. Tartalmaz olyan gyűjteménye, nevét és az oszlopok adattípusát.  A következő példában az adatkészlet van három oszlop slicetimestamp, a projektnév és a pageviews, és írja be: karakterláncot, a karakterlánc és a tizedes kurzor.

    structure:  
    [ 
        { "name": "slicetimestamp", "type": "String"},
        { "name": "projectname", "type": "String"},
        { "name": "pageviews", "type": "Decimal"}
    ]

## <a name="Availability"></a>Adatkészlet elérhetősége
**A elérhetősége egy adatkészlet az** határozza meg a feldolgozás ablak (óránkénti, napi, heti stb.) vagy a slicing modell az adatkészlet számára. Lásd: [értekezlet ütemezése, illetve végrehajtása](data-factory-scheduling-and-execution.md) a cikk további információt az adatkészlet szeletelés és függőség modell. 

A következő elérhetősége Megadja, hogy a kimeneti adatkészlet gyártott óránkénti (vagy) bemeneti adatkészlet óránkénti akkor érhető el:

    "availability": 
    {   
        "frequency": "Hour",        
        "interval": 1   
    }

Az alábbi táblázat ismerteti a elérhetősége használható tulajdonságokat: 

| A tulajdonság | Leírás | Szükséges | Alapértelmezett |
| -------- | ----------- | -------- | ------- |
| gyakoriság | Adja meg az adatkészlet szeletet gyártási időegységének.<br/><br/>**Támogatott gyakoriság**: perc, óra, nap, hét, hónap | igen | HIÁNYZIK |
| intervallum | Adja meg az ismétlődési egy mezősokszorozó<br/><br/>"X gyakoriság intervallum" határozza meg, hogy milyen gyakran a szeletet elő.<br/><br/>Ha az adatkészlet az óránkénti kombinálásával lehet felszeletelni van szüksége, beállíthatja, **gyakoriság** **órát**és **intervallum** **1**.<br/><br/>**Megjegyzés:** Ha a gyakoriság ad meg percben, azt javasoljuk, és 15-nél kisebb időköz beállítása | igen | HIÁNYZIK |
| stílus | Itt adhatja meg, hogy a szeletet kell mutatni a kezdete/vége intervallum.<ul><li>StartOfInterval</li><li>EndOfInterval</li></ul><br/><br/>Ha a gyakoriság értéke hónap, és a stíluskészlet EndOfInterval, a szeletet a hónap utolsó napja elő. Ha a stílus StartOfInterval van beállítva, a szeletet elő a hónap első napját.<br/><br/>Ha a gyakoriság értéke nap, és a stíluskészlet EndOfInterval, a szeletet az utolsó napjának órában elő.<br/><br/>Ha a gyakoriság értéke órát, és a stíluskészlet EndOfInterval, a szeletet az órát végén elő. Például az du. 1 – 2 PM időszakra szeletek, a szeletet elő a 2 du. | nem | EndOfInterval |
| anchorDateTime | Megadja az adatkészlet szeletet határai számítja ki a Feladatütemező által használt időpontban abszolút helyét. <br/><br/>**Megjegyzés:** Ha a AnchorDateTime részekből dátum, amelyek finomabb, mint a gyakorisági finomabb részek is figyelmen kívül hagyja. <br/><br/>Ha például az **intervallum** , **óránként** (gyakoriság: órát és intervallum: 1) a **AnchorDateTime** **percek és másodpercek**tartalmazza, majd a AnchorDateTime **percek és másodpercek** részei a program figyelmen kívül hagyja. | nem | 01/01/0001 |
| eltolás | Időszak, amelynek a kezdési és befejezési adatkészlet összes szelet eltolt. <br/><br/>**Megjegyzés:** Ha nincs megadva anchorDateTime és az eltolás, a kombinált shift eredménye. | nem | HIÁNYZIK |

### <a name="offset-example"></a>eltolás példa

Napi kezdődő szeletre, a 6: 00 az alapértelmezett éjfél helyett. 

    "availability":
    {
        "frequency": "Day",
        "interval": 1,
        "offset": "06:00:00"
    }

A **gyakoriság** értéke **nap** , és **intervallum** értéke **1** (naponta egyszer): Ha azt szeretné, hogy a szeletet a 6: 00 helyett alapértelmezett időben készítendő: 12 AM. Ne feledje, hogy ez esetben egy világidő. 

## <a name="anchordatetime-example"></a>Példa anchorDateTime

**Példa:** 23 órát adatkészlet szeletek indítsa el a 2007-04-19T08:00:00

    "availability": 
    {   
        "frequency": "Hour",        
        "interval": 23, 
        "anchorDateTime":"2007-04-19T08:00:00"  
    }

## <a name="offsetstyle-example"></a>eltolás/stílus példa

Ha van szüksége a havi rendszerességgel adatkészlet meghatározott dátum- és a (tegyük fel, hogy a 3rd a 8:00 de minden hónap), működnie kell az **eltolás** címkét a állítja be a dátumot és időzítése használata. 

    {
      "name": "MyDataset",
      "properties": {
        "type": "AzureSqlTable",
        "linkedServiceName": "AzureSqlLinkedService",
        "typeProperties": {
          "tableName": "MyTable"
        },
        "availability": {
          "frequency": "Month",
          "interval": 1,
          "offset": "3.08:10:00",
          "style": "StartOfInterval"
        }
      }
    }


## <a name="Policy"></a>Adatkészlet házirend

Az adatkészlet-definícióban **házirend** szakaszban határozza meg, a feltételeket vagy a feltétel, amely az adatkészlet szeletek meg kell felelniük.

### <a name="validation-policies"></a>Érvényességi szabályok

| Házirend neve | Leírás | Alkalmazott | Szükséges | Alapértelmezett |
| ----------- | ----------- | ---------- | -------- | ------- |
| minimumSizeMB | Ellenőrzi, hogy az adatokat az **Azure blob-** megfelel a minimális méret (megabájtban). | Azure Blob | nem | HIÁNYZIK |
|minimumRows | Ellenőrzi, hogy az adatokat egy **Azure SQL-adatbázis** vagy az **Azure-táblából** tartalmaz-e a minimális sorok számát. | <ul><li>Azure SQL-adatbázis</li><li>Azure-táblából</li></ul> | nem | HIÁNYZIK

#### <a name="examples"></a>Példák

**minimumSizeMB:**

    "policy":
    
    {
        "validation":
        {
            "minimumSizeMB": 10.0
        }
    }

**minimumRows**

    "policy":
    {
        "validation":
        {
            "minimumRows": 100
        }
    }

### <a name="external-datasets"></a>Külső adatkészleteket

Külső adatkészleteket lépnek adatok gyári egy futó folyamat nem készül. Ha az adathalmazban **külső**megjelölve, előfordulhat, hogy **ExternalData** házirend adatkészlet szeletet elérhetősége működését befolyásolják kell megadni. 

Egy adathalmaz Azure Data Factory készített, kivéve akkor kell megjelölni **külső**. Ez a beállítás általában vonatkozik a bemeneti adatok alapján, az első tevékenység egy folyamat, kivéve, ha a tevékenységhez vagy a folyamat láncolás van használatban. 

| név | Leírás | Szükséges | Alapérték  |
| ---- | ----------- | -------- | -------------- |
| dataDelay | A külső adatokat az adott szeletet elérhetőségének ellenőrzése késleltetése időt. Ha például az adatok értendő óránként elérhetővé tenni, ha a ellenőrizze, hogy a külső adatok érhető el, és a megfelelő szeletet elkészült az dataDelay használatával késhet.<br/><br/>Csak a jelen idő vonatkozik.  Például 1:00 PM pillanatban és értéke 10 perc, ha az érvényesítési kezdődik 1:10 du.<br/><br/>Ez a beállítás nem befolyásolja a szeletek múltbeli (szeletek szeletet befejezési idő + dataDelay < most) késedelem nélkül feldolgozása.<br/><br/>Nagyobb, mint 23:59 óra kell megadott day.hours:minutes:seconds formátumú időt. Ha például 24 óra megadásához nem használja 24:00:00; 1.00:00:00 kell használni. Ha 24:00:00 használja, akkor a függvény 24 napokként (24.00:00:00). 1 nap és 4 óra 1:04:00:00 mezőben adja meg. | nem | 0 |
| retryInterval | A várakozási idő hiba és a következő között kísérlet próbálkozzon újra. Bemutató idő; vonatkozik az előző próbál sikertelen, ha azt várja meg a legutolsó kísérlet után hosszú. <br/><br/>Ha az 1:00 pm pillanatban, az első lépések az első próbálja meg. Ha az első ellenőrzés elvégzéséhez az időtartam 1 perc és a művelet nem sikerült, a következő újrapróbálkozási jelenleg 1:00 + 1 min (időtartam) + 1min (újrapróbálkozási interval) = 1:02 du. <br/><br/>A szeletek múltbeli nincs késleltetve. Az Ismét gombra a azonnal történik. | nem | 00:01:00 (1 perc) | 
| retryTimeout | Az egyes kísérletek kísérlet során időtúllépése.<br/><br/>Ez a tulajdonság értéke 10 perc, ha az érvényesítési elvégzendő 10 percen belül. Ha az érvényesítési végrehajtásához 10 percnél hosszabb ideig tart, a kísérletek időtúllépés történik.<br/><br/>Az adatérvényesítés összes kísérletek időtúllépés történik, ha a szeletet időtúllépése van megjelölve. | nem | 00:10:00 (10 perc) |
| maximumRetry | A külső adatok elérhetőségének ellenőrzéséhez ismétlődésének száma. A megengedett maximális értéke 10. | nem | 3 | 

## <a name="scoped-datasets"></a>Hatókörű adatkészleteket
Létrehozhat, amelyek a folyamat **adatkészleteket** tulajdonságával tükrözik adatkészleteket. Ezek a adatkészleteket csak akkor használható, ez a folyamat tevékenységeit, de nem a más folyamatok tevékenységeket. Az alábbi példa a két adatkészleteket - InputDataset-távoli asztali kapcsolat és OutputDataset-távoli asztali kapcsolat - belül a folyamat használandó egy folyamat határozza meg:  

> [AZURE.IMPORTANT] Hatókörű adatkészleteket használhatók csak a egyszeri folyamatok (**pipelineMode** **OneTime**meg). [Onetime folyamat](data-factory-scheduling-and-execution.md#onetime-pipeline) talál további információt.

    {
        "name": "CopyPipeline-rdc",
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
                            "name": "InputDataset-rdc"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "OutputDataset-rdc"
                        }
                    ],
                    "scheduler": {
                        "frequency": "Day",
                        "interval": 1,
                        "style": "StartOfInterval"
                    },
                    "name": "CopyActivity-0"
                }
            ],
            "start": "2016-02-28T00:00:00Z",
            "end": "2016-02-28T00:00:00Z",
            "isPaused": false,
            "pipelineMode": "OneTime",
            "expirationTime": "15.00:00:00",
            "datasets": [
                {
                    "name": "InputDataset-rdc",
                    "properties": {
                        "type": "AzureBlob",
                        "linkedServiceName": "InputLinkedService-rdc",
                        "typeProperties": {
                            "fileName": "emp.txt",
                            "folderPath": "adftutorial/input",
                            "format": {
                                "type": "TextFormat",
                                "rowDelimiter": "\n",
                                "columnDelimiter": ","
                            }
                        },
                        "availability": {
                            "frequency": "Day",
                            "interval": 1
                        },
                        "external": true,
                        "policy": {}
                    }
                },
                {
                    "name": "OutputDataset-rdc",
                    "properties": {
                        "type": "AzureBlob",
                        "linkedServiceName": "OutputLinkedService-rdc",
                        "typeProperties": {
                            "fileName": "emp.txt",
                            "folderPath": "adftutorial/output",
                            "format": {
                                "type": "TextFormat",
                                "rowDelimiter": "\n",
                                "columnDelimiter": ","
                            }
                        },
                        "availability": {
                            "frequency": "Day",
                            "interval": 1
                        },
                        "external": false,
                        "policy": {}
                    }
                }
            ]
        }
    }