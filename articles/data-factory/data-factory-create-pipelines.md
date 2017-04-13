<properties 
    pageTitle="Folyamatok létrehozása, ütemezés, az adatok gyári tevékenységek lánc |} Microsoft Azure" 
    description="Ismerje meg, a adatok folyamat létrehozása az Azure Data Factory áthelyezése és az adatokat. Kész létrehozásához használandó adatokat alapú adatok munkafolyamat létrehozása." 
    keywords="adatok folyamat, munkafolyamat alapú adatok"
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
    ms.date="09/12/2016" 
    ms.author="shlo"/>

# <a name="pipelines-and-activities-in-azure-data-factory"></a>Folyamatok és Azure Data Factory tevékenységek
Ez a cikk segít megérteni a folyamatok és Azure Data Factory tevékenységet, és segítségével a adatok mozgását és adatfeldolgozási esetek végpontok közötti adatalapú munkafolyamatok létrehozása.  

> [AZURE.NOTE] A témakör tartalma feltételezi, hogy telt-keresztül [Azure Data Factory bemutatása](data-factory-introduction.md). Ha nem rendelkezik hands-a-élmény létrehozásával gyárak, [az adatok első gyári összeállítása](data-factory-build-your-first-pipeline.md) oktatóprogram keresztül segít az adatok ismernie jobb Ez a cikk.  

## <a name="what-is-a-data-pipeline"></a>Mi az a adatok folyamat?
**Folyamat** logikailag kapcsolódó **tevékenységek**csoportosítása. Csoporttevékenységek egy feladatot hajt végre egységbe foglalására szolgál. Jobb megértéséhez folyamatok, szüksége egy tevékenység először megértéséhez. 

## <a name="what-is-an-activity"></a>Mi az, hogy egy tevékenység?
Tevékenységek megadása a műveletek elvégzéséhez az adatokon Az egyes tevékenységek nulla vagy több [adatkészleteket](data-factory-create-datasets.md) megnyitja a bemeneti adatok alapján, és hoz létre egy vagy több adatkészleteket ezt az eredményt. 

Például egy példány tevékenység segítségével előfordulhat, hogy egy másik adattár egy adattár másolása adatok téve. Hasonlóképpen egy HDInsight-struktúra tevékenység segítségével előfordulhat, hogy az Azure hdinsight szolgáltatáshoz fürt az adatokat a struktúra lekérdezést futtat. Azure Data Factory sokféle [átalakítási](data-factory-data-transformation-activities.md)és [adatok mozgását](data-factory-data-movement-activities.md) tevékenységek biztosít. Dönthet úgy is, a saját kód futtatásához egyéni .NET tevékenység létrehozása. 

## <a name="sample-copy-pipeline"></a>Példa másolása folyamat
A következő példa során nem **másolása** a **tevékenységek** szakasz típusú egy tevékenységet. Ebben a példában a [Tevékenység másolása](data-factory-data-movement-activities.md) lévő adatokat másolja az Azure Blob-tárolóhoz Azure SQL-adatbázishoz. 

    {
      "name": "CopyPipeline",
      "properties": {
        "description": "Copy data from a blob to Azure SQL table",
        "activities": [
          {
            "name": "CopyFromBlobToSQL",
            "type": "Copy",
            "inputs": [
              {
                "name": "InputDataset"
              }
            ],
            "outputs": [
              {
                "name": "OutputDataset"
              }
            ],
            "typeProperties": {
              "source": {
                "type": "BlobSource"
              },
              "sink": {
                "type": "SqlSink",
                "writeBatchSize": 10000,
                "writeBatchTimeout": "60:00:00"
              }
            },
            "Policy": {
              "concurrency": 1,
              "executionPriorityOrder": "NewestFirst",
              "retry": 0,
              "timeout": "01:00:00"
            }
          }
        ],
        "start": "2016-07-12T00:00:00Z",
        "end": "2016-07-13T00:00:00Z"
      }
    } 

Vegye figyelembe az alábbiakat:

- A tevékenységek csoportban található csak egy tevékenység, amelynek a **típus** értéke **másolása**
- A tevékenységhez tartozó bemeneti **InputDataset** van állítva, és a tevékenység kimeneti **OutputDataset**van beállítva.
- **TypeProperties** csoportban a forrás típusa **BlobSource** van megadva, és **SqlSink** van megadva, gyűjtő típust.

Lásd: a teljes ismertetését megtalálja a folyamat létrehozása, [oktatóanyag: adatok másolása Blob-tárolóhoz SQL-adatbázis](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md). 

## <a name="sample-transformation-pipeline"></a>Minta átalakítása folyamat
A következő példa során nem **HDInsightHive** a **tevékenységek** szakasz típusú egy tevékenységet. Az ebben a példában a [HDInsight-struktúra tevékenység](data-factory-hive-activity.md) átalakítások az Azure Blob-tárolóhoz adatainak egy Azure hdinsight szolgáltatáshoz Hadoop fürt struktúra parancsfájl futtatásával. 

    {
        "name": "TransformPipeline",
        "properties": {
            "description": "My first Azure Data Factory pipeline",
            "activities": [
                {
                    "type": "HDInsightHive",
                    "typeProperties": {
                        "scriptPath": "adfgetstarted/script/partitionweblogs.hql",
                        "scriptLinkedService": "AzureStorageLinkedService",
                        "defines": {
                            "inputtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/inputdata",
                            "partitionedtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/partitioneddata"
                        }
                    },
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
                }
            ],
            "start": "2016-04-01T00:00:00Z",
            "end": "2016-04-02T00:00:00Z",
            "isPaused": false
        }
    }

Vegye figyelembe az alábbiakat: 

- A tevékenységek csoportban található csak egy tevékenység, amelynek **típusa** **HDInsightHive**beállítása
- A struktúra parancsfájl, **partitionweblogs.hql**, Azure tároló fiók (az scriptLinkedService **AzureStorageLinkedService**nevű által megadott), és a tároló **adfgetstarted** **parancsfájl** mappájában vannak tárolva.
- A **definiálja** szakasz szolgál adja meg a struktúra értékként a struktúra parancsfájl átadott futtatókörnyezet beállításokat (például ${hiveconf: inputtable}, {hiveconf:partitionedtable} $).

Lásd: a teljes ismertetését megtalálja a folyamat létrehozása, [oktatóprogram: az első folyamat folyamat adatokhoz Hadoop fürt összeállítása](data-factory-build-your-first-pipeline.md). 

## <a name="chaining-activities"></a>Tevékenységek láncolás
Ha több tevékenység is van egy folyamat és kimeneti egy tevékenység nem egy másik tevékenységeket bevitele, a tevékenységek párhuzamosan futtathatnak, ha készen áll a tevékenységekhez tartozó bemeneti adatok szeletek. 

Két tevékenységet úgy, hogy egy tevékenység, mint a bemeneti adatkészlet a többi tevékenység a kimeneti adatkészlet fűzhetők össze. A tevékenységek lehet azonos vagy más folyamatok. A második tevékenység hajt végre, csak ha az első szakasz befejeződik. 

Például érdemes megvizsgálni a következő esetben:
 
1.  Folyamat P1 tartalmaz, amely a külső beviteli adatkészlet D1 igényel, és a **kimeneti** adatkészlet **D2**konzerv tevékenység A1.
2.  Folyamat P2 tevékenység A2 **beviteli** az adatkészlet **D2**igényel, és a kimeneti adatkészlet D3 eredménye tartalmaz.
 
Ebben az esetben a tevékenység A1 fut, amikor a külső adatok érhető el, és az ütemezett elérhetősége gyakoriság eléri.  A tevékenység a A2 fut, amikor a D2 ütemezett szeletet elérhetővé válnak, és az ütemezett elérhetősége gyakoriság eléri. Egy adathalmaz D2 szeleteinek hiba esetén A2 nem működik a kör közepétől mindaddig, amíg elérhetővé válik.

Diagram nézet:

![A két folyamatok láncolási tevékenységek](./media/data-factory-create-pipelines/chaining-two-pipelines.png)

Diagram nézetben a két tevékenység a azonos során: 

![Az azonos folyamat láncolási tevékenységek](./media/data-factory-create-pipelines/chaining-one-pipeline.png)

További tudnivalókért lásd: [Ütemezés- és végrehajtása](#chaining-activities). 

## <a name="scheduling-and-execution"></a>Ütemezés- és végrehajtása
Az eddigi kell értelmezni, folyamatok és a tevékenységek vannak. Van még nézett módját, hogy azok definiált és a magas szintű Azure Data Factory a tevékenységek nézetben. Most már tudassa velünk tekintse meg hogyan első végrehajtása. 

A folyamat akkor aktív, csak a kezdési idő és befejezési idő között. Nem hajtja végre, mielőtt a kezdési időpontot vagy a befejezési idő után. Ha a folyamat fel van függesztve, azt nem kérjen végrehajtva függetlenül annak kezdési és befejezési időpontot. Az egy folyamat futtatása meg kell nem kell fel van függesztve. Valójában akkor sem, hajtsa végre a folyamat. A tevékenységek a során végrehajtott első. Azonban ehhez a folyamat általános környezetében. 

Lásd: [értekezlet ütemezése, illetve végrehajtása](data-factory-scheduling-and-execution.md) az Azure adatok gyár az ütemezés és a végrehajtás működésének megértése.

## <a name="create-pipelines"></a>Folyamatok létrehozása
Azure Data Factory különböző mechanizmusok a tartalom-előállítás és telepíthetik a folyamatok (ami viszont tartalmaz egy vagy több tevékenységek benne) tartalmaz. 

### <a name="using-azure-portal"></a>Azure portál használatával
Adatok Factory-szerkesztőben az Azure-portálon hozhat létre egy folyamat. Egy végpont – útmutató – a [Azure Data Factory (adatok Factory-szerkesztőben) – első lépések](data-factory-build-your-first-pipeline-using-editor.md) talál. 

### <a name="using-visual-studio"></a>Visual Studio segítségével 
Tartalom-előállítás és folyamatok telepítse az Azure Data Factory Visual Studio segítségével. [Első lépések az Azure Data Factory (Visual Studio)](data-factory-build-your-first-pipeline-using-vs.md) talál egy végpont – útmutató. 

### <a name="using-azure-powershell"></a>Azure PowerShell használatával
Az Azure PowerShell folyamatok létrehozása az Azure Data Factory is használhatja. Tegyük fel definiált továbbítási folyamatát JSON c:\DPWikisample.json a fájlban. Is feltölthet a azt az Azure Data Factory-példányt az alábbi példában látható módon:

    New-AzureRmDataFactoryPipeline -ResourceGroupName ADF -Name DPWikisample -DataFactoryName wikiADF -File c:\DPWikisample.json

[Első lépések az Azure Data Factory (Azure PowerShell)](data-factory-build-your-first-pipeline-using-powershell.md) talál egy végpont – útmutató adatok gyár létrehozása egy folyamat. 

### <a name="using-net-sdk"></a>.NET SDK használatával
Létrehozhat és üzembe túl a folyamat .NET SDK keresztül. Ez az eljárás a folyamatok programozás útján létrehozására használható. További tudnivalókért lásd: [jelentések létrehozása, kezelése, és figyelemmel követheti a adatok gyárak programozás útján](data-factory-create-data-factories-programmatically.md). 


### <a name="using-azure-resource-manager-template"></a>Erőforrás-kezelő Azure-sablon segítségével
Létrehozhat és üzembe folyamat egy erőforrás-kezelő Azure-sablon segítségével. További tudnivalókért olvassa el a [Azure Data Factory (Azure erőforrás-kezelő) – első lépések](data-factory-build-your-first-pipeline-using-arm.md)című témakört. 

### <a name="using-rest-api"></a>REST API segítségével
Létrehozhat és üzembe folyamat REST API-khoz is használ. Ez az eljárás a folyamatok programozás útján létrehozására használható. További tudnivalókért olvassa el a [Létrehozás és frissítés egy folyamat](https://msdn.microsoft.com/library/azure/dn906741.aspx)című témakört. 


## <a name="monitor-and-manage-pipelines"></a>Figyelésére és folyamatok kezelése  
Egy folyamat telepíti, miután kezelése, és figyelje a folyamatok, szeletek és futtatása. Itt további információ: [Monitor és folyamatok kezelése](data-factory-monitor-manage-pipelines.md).


## <a name="pipeline-json"></a>A folyamat JSON   
Tudassa velünk, hogy hogyan egy folyamat definiálásának JSON formátumban megtekintése. Az általános struktúra az egy folyamat a következőképpen néz ki:

    {
        "name": "PipelineName",
        "properties": 
        {
            "description" : "pipeline description",
            "activities":
            [
    
            ],
            "start": "<start date-time>",
            "end": "<end date-time>"
        }
    }

A **tevékenységek** szakaszban beállíthatja, hogy egy vagy több tevékenységek definiált benne. Az egyes tevékenységek legfelső szintű szerkezete a következő:

    {
        "name": "ActivityName",
        "description": "description", 
        "type": "<ActivityType>",
        "inputs":  "[]",
        "outputs":  "[]",
        "linkedServiceName": "MyLinkedService",
        "typeProperties":
        {
    
        },
        "policy":
        {
        }
        "scheduler":
        {
        }
    }

Táblázat következő ismertetik a tevékenységet, és a folyamat JSON-definíciók belül tulajdonságait:

Címke | Leírás | Szükséges
--- | ----------- | --------
név | A tevékenység vagy a folyamat neve. Adjon meg egy nevet, amely a műveletet, hogy végezze el a tevékenységhez vagy a folyamat helyesek-e<br/><ul><li>Karakterek maximális száma: 260</li><li>Levél szám vagy az aláhúzásjel _ kell kezdődnie</li><li>A következő karakterek nem használhatók: ".", "+", "?", "/", "<";">", "*", "%", "&", ":","\\"</li></ul> | igen
Leírás | A tevékenységhez vagy a folyamat mire való leíró szöveget | igen
típus | Itt adhatja meg a tevékenység típusát. Cikkek a [Mozgás a tevékenységekre vonatkozó adatok](data-factory-data-movement-activities.md) és a [Tevékenységekre vonatkozó adatok átalakítása](data-factory-data-transformation-activities.md) különböző típusú tevékenységek. | igen
bemeneti adatok alapján | A tevékenység által használt beviteli tábla<br/><br/>egy beviteli tábla<br/>"bemenetben": [{"név": "inputtable1"}],<br/><br/>két beviteli tábla <br/>"ráfordítások": [{"név": "inputtable1"}, {"név": "inputtable2"}], | igen
kimeneti értékeket | Az egyik activity.// eredménytábla által használt eredménytábla<br/>"kimenet": [{"név": "outputtable1"}],<br/><br/>két kimeneti tábla<br/>"kimenet": [{"név": "outputtable1"}, {"név": "outputtable2"}], | igen
linkedServiceName | A csatolt szolgáltatást a tevékenység neve. <br/><br/>Egy tevékenység szükség lehet, hogy adja meg a csatolt szolgáltatás, amely a szükséges számítási környezetet. | Igen az HDInsight tevékenység és Azure gépi tanulási tevékenység pontozási köteg <br/><br/>Nem az összes többi
typeProperties | Tulajdonságok typeProperties szakaszában attól függenek, hogy a tevékenység típusát. | nem
házirend | Házirendek, amelyek a tevékenység futási idejű viselkedését. Ha nincs megadva, az alapértelmezett házirendek használhatók. | nem
indítása | Kezdő dátum-idő az folyamat. [ISO](http://en.wikipedia.org/wiki/ISO_8601)formátumban kell lennie. Példa: 2014-10-14T16:32:41Z. <br/><br/>Érdemes is megadhat egy helyi idővé, például egy becsült idő. Íme egy példa: "2016 – 02-27T06:00:00**-05: 00**", amely olyan, 6 AM becsült<br/><br/>A kezdő és záró tulajdonságok együtt az folyamat aktív időszak adja meg. Kimeneti szeletek csak darab termék készült ebben az aktív időszakban. | nem<br/><br/>Ha megadja a befejezési tulajdonság értékét, meg kell adnia a kezdő tulajdonság értékét.<br/><br/>A kezdő és záró időpont egyaránt lehet üres létrehozása egy folyamat. Meg kell adnia az futtatásához folyamat egy aktív időszakot állíthat mindkét értéket. Ha nem adja meg a kezdő és záró időpont létrehozása egy folyamat, beállíthatja, hogy később a Set-AzureRmDataFactoryPipelineActivePeriod parancsmaggal őket.
vége | A dátum / idő az folyamat befejezéséhez. Ha a megadott ISO-formátumban kell lennie. Példa: 2014-10-14T17:32:41Z <br/><br/>Érdemes is megadhat egy helyi idővé, például egy becsült idő. Íme egy példa: "2016 – 02-27T06:00:00**-05: 00**", amely olyan, 6 AM becsült<br/><br/>A folyamat végtelen időre szóló futtatásához adja meg az érték a befejezési tulajdonság 9999-09-09. | nem <br/><br/>Ha megadja a kezdő tulajdonság értékét, meg kell adnia a befejezési tulajdonság értékét.<br/><br/>Lásd a **start** tulajdonság megjegyzéseket.
isPaused | Ha nem működik a folyamat igaz értékre kell állítani. Alapértelmezett érték = false. Ez a tulajdonság segítségével engedélyezése vagy letiltása. | nem 
ütemezőt | "a Feladatütemező" tulajdonság megadása a tevékenység ütemezése kívánt szolgál. A altulajdonságok ugyanazok, mint az [Elérhetőség tulajdonság egy adatkészlet](data-factory-create-datasets.md#Availability)szerint a lehetőségekből. | nem |   
| pipelineMode | Az ütemezési módszer futtatja az folyamat. Engedélyezett értékek: (alapértelmezett), ütemezett alkalommal.<br/><br/>Ütemezett", az azt jelzi, hogy fut-e a folyamat az aktív időtartam (a kezdési és befejezési idő) szerint megadott időközönként. "Alkalommal" azt jelzi, hogy a folyamat csak egyszer fut-e. Egyszer létrehozott alkalommal folyamatok nem lehet módosítani vagy frissített jelenleg. Kapcsolatos további tudnivalók: [Onetime folyamat](data-factory-scheduling-and-execution.md#onetime-pipeline) alkalommal beállítást. | nem | 
| expirationTime | Időtartam, amelynek a folyamat érvényes, és továbbra is kiépített létrehozását követően. Ha nem rendelkezik minden aktív nem sikerült, vagy függőben fut, a során a rendszer automatikusan törli eléri a lejárati idő után. | nem | 
| adatkészletek | Használhatják a során meghatározott tevékenységek adatkészleteket listája. Ez a tulajdonság definiálása, amelyek a folyamat alkalmazásra, és nincs megadva, az adatok gyári belül adatkészleteket használható. Ez a folyamat belül definiált adatkészleteket csak ez a folyamat használható, és nem lehet megosztani. [Lapszintű adatkészleteket](data-factory-create-datasets.md#scoped-datasets) talál további információt.| nem |  
 

### <a name="policies"></a>Házirendek
Házirendek viselkedését futási idejű tevékenységet, kifejezetten, ha a táblázat szeletet feldolgozása. A következő táblázat részletesen.

A tulajdonság | Megengedett értékeket | Alapérték | Leírás
-------- | ----------- | -------------- | ---------------
feldolgozási | Egész szám <br/><br/>A maximális érték: 10 | 1 | A tevékenység egyidejű végrehajtások száma.<br/><br/>Azt határozza meg, amely akkor fordulhat elő, a különböző szeletek párhuzamos tevékenység végrehajtások számát. Például ha egy tevékenység folyamatát elérhető adatok nagyobb feldolgozási értékkel nagyszámú gyorsabbá az adatfeldolgozás. 
executionPriorityOrder | NewestFirst<br/><br/>OldestFirst | OldestFirst | Határozza meg, feldolgozott adatok szeletek sorrendjét.<br/><br/>Például ha 2 szeletre (4 du. egy jelet, és egy másikkal 5 du.), és mindkét függőben végrehajtása. Ha a executionPriorityOrder NewestFirst lesz, az 5 du. szelet van dolgozza fel először. Hasonlóképpen a executionPriorityORder OldestFIrst kell állít be, ha majd a 4-es du. szeletet feldolgozása. 
Ismételje meg | Egész szám<br/><br/>A maximális érték is lehet 10 | 3 | Mielőtt az adatfeldolgozás az a szeletek hiba van megjelölve újrapróbálkozások száma. Az adatok szelet tevékenység végrehajtása az ismétlési megadott számot felfelé megismétlése. Az Ismét gombra a minél korábban után a hiba történik.
határidő | Időszak | 00:00:00 | A tevékenység időtúllépés. Példa: (azt jelenti, időtúllépés 10 perc) 00:10:00<br/><br/>Ha egy érték nincs megadva, vagy értéke 0, az időtúllépési végtelen érték.<br/><br/>Az adatkezelési idő szelet meghaladja az időkorlát, ha megszakad, és a rendszer kísérel meg újra a feldolgozása. Az ismétlések számát a újrapróbálkozási tulajdonság függ. Időtúllépés történik, ha az állapot időtúllépése beállítása.
késleltetés | Időszak | 00:00:00 | Adja meg, a késleltetés, mielőtt a szeletek elindul az adatfeldolgozás.<br/><br/>Késleltetés múltbeli a várt időben végrehajtása után egy szeletre tevékenység végrehajtását el van indítva.<br/><br/>Példa: (azt jelenti, késleltetése 10 perc) 00:10:00
longRetry | Egész szám<br/><br/>A maximális érték: 10 | 1 | Nem sikerült a szeletek végrehajtása előtt hosszú újrapróbálkozási kísérletek száma.<br/><br/>longRetry kísérletek által longRetryInterval sorközt szeretne használni. Ha egy újrapróbálkozási kísérletek közötti időtartamot, így longRetry használja. Ismét és longRetry is meg vannak adva, ha minden longRetry kísérlet tartalmazza újrapróbálkozási kísérletek és a kísérletek maximális száma újrapróbálkozási * longRetry.<br/><br/>Ha például a tevékenység házirend van az alábbi beállításokat:<br/>Próbálkozzon újra: 3<br/>longRetry: 2<br/>longRetryInterval: 01:00:00<br/><br/>Tegyük fel, hogy csak egy szeletet végrehajtásához van (állapot várakozás) és a tevékenység végrehajtását minden alkalommal sikertelen lesz. Kezdetben van lenne 3 egymást követő végrehajtási kísérleteket. Minden egyes a kísérlet után szeletet állapotát lenne újra gombra. Miután első 3 kísérletek fölé, szelet állapotát lenne LongRetry.<br/><br/>Egy óra (Ez azt jelenti, hogy a longRetryInteval érték-) után 3 egymást követő végrehajtási kísérletek másik csoportjának következő lesz. Ezt követően szeletet állapotát volna kell sikertelen, és nincs több próbálkozás kísérlet a lenne. Így teljes 6 kísérlet történt.<br/><br/>Ha sikerült a minden végrehajtás, szelet állapota kész lenne, és nincs több próbálkozás nem megkísérlése.<br/><br/>longRetry esetekben, ahol függő adatok megérkezik időközönként nem – vagy a teljes környezet flaky milyen adatkezelési fordul elő a használhatók. Ebben az esetben ezzel újrapróbálkozások egymás után nem segíthet, és a Ha így után elvégezve a kívánt eredményt ad eredményt időt.<br/><br/>Amire érdemes ügyelni Word: nincs beállítva a longRetry vagy longRetryInterval maximális érték. Általában a magasabb értékek rendszeres problémákkal szemléltetnek. 
longRetryInterval | Időszak | 00:00:00 | A késleltetés hosszú újrapróbálkozási kísérletek között 


## <a name="next-steps"></a>Következő lépések

- Ismerje meg [az ütemezési és Azure Data Factory végrehajtását jelző bejegyzést](data-factory-scheduling-and-execution.md).  
- További információ: [adatok mozgását](data-factory-data-movement-activities.md) és Azure Data Factory [adatok átalakítási lehetőségei](data-factory-data-transformation-activities.md)
- Ismerje meg, [kezelése és Azure Data Factory ellenőrzése](data-factory-monitor-manage-pipelines.md).
- [Építse fel és telepítse a fist folyamat](data-factory-build-your-first-pipeline.md). 
