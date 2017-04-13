<properties 
    pageTitle="Az adatokat áthelyezheti az adatok gyári használatával Cassandra |} Microsoft Azure" 
    description="Megismerheti az adatok áthelyezése egy helyszíni Cassandra adatbázisból Azure Data Factory használatával." 
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
    ms.date="09/07/2016" 
    ms.author="jingwang"/>

# <a name="move-data-from-an-on-premises-cassandra-database-using-azure-data-factory"></a>Azure Data Factory használata a helyszíni Cassandra adatbázisból származó adatok áthelyezése
Ez a cikk azt ismerteti, hogyan használhatja a Másolás tevékenység-Azure adatok gyári a helyszíni Cassandra adatbázisból származó adatok átmásolása bármely gyűjtő oszlopban a [támogatott adatforrások és mosdók](data-factory-data-movement-activities.md#supported-data-stores) szakaszban felsorolt adattár. Ez a cikk az adatok mozgás egy általános áttekintése másolás állapotát tevékenysége és a támogatott adatokat tároló kombinációk eltéréseit [Mozgás a tevékenységekre vonatkozó adatok](data-factory-data-movement-activities.md) cikk épül.

Adatok gyári jelenleg csak mozgóátlag-adatbázisból egy Cassandra [gyűjtő adatokat tárolja](data-factory-data-movement-activities.md#supported-data-stores)támogatott, de nem mozgó adatok Cassandra adatbázis más adatokat tárolja.

## <a name="prerequisites"></a>Előfeltételek
Az Azure Data Factory szolgáltatás tudja a helyszíni Cassandra adatbázishoz való csatlakozáshoz telepítenie kell az alábbiak: 

- Az adatkezelési átjáró 2.0-s vagy újabb ugyanazon a gépen az adatbázist tároló vagy egy másik számítógépre, az adatbázis-kezelő erőforrások gyorskorcsolyázásban elkerülése érdekében. Az adatkezelési átjáró a helyszíni adatforrások kapcsolódó felhőszolgáltatások biztonságos és felügyelt módon szoftver. Lásd: [az adatok helyszíni és felhőbeli közötti áthelyezése a](data-factory-move-data-between-onprem-and-cloud.md) cikk az adatkezelési átjáró kapcsolatban további tájékoztatást.
  
    Ha telepíti az átjárót, a Microsoft Cassandra ODBC-illesztőprogram Cassandra adatbázishoz való csatlakozáshoz használt automatikusan telepíti. 

> [AZURE.NOTE] Lásd: a kapcsolat/átjáró hibaelhárítási tippek [problémák elhárítása átjáró](data-factory-data-management-gateway.md#troubleshoot-gateway-issues) kapcsolatos problémákat. 

## <a name="copy-data-wizard"></a>Az adatok varázsló másolása
A hozzon létre egy folyamat, amely az adatokat másolja Cassandra adatbázisból valamelyik a támogatott gyűjtő adatokat tárolja, legegyszerűbben a Másolás adatok varázsló használatával. Lásd: [oktatóprogram: hozzon létre egy példány varázslóval folyamat](data-factory-copy-data-wizard-tutorial.md) létrehozása egy folyamat adatok másolása varázsló segítségével a rövid ismertetését megtalálja. 

Az alábbi példa a minta JSON-definíciók létrehozása egy folyamat [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) segítségével vagy [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) vagy [Azure Powershellhez](data-factory-copy-activity-tutorial-using-powershell.md)használható biztosít. Adatok másolása Cassandra adatbázisból Azure Blob-tárolóhoz mutatnak. Adatok másolni lehet valamelyik, az mosdók megadott [Itt](data-factory-data-movement-activities.md#supported-data-stores) a Másolás tevékenység használata Azure Data Factory.   


## <a name="sample-copy-data-from-cassandra-to-blob"></a>Minta: Adatok másolása Cassandra Blob
A minta átmásolja adatok Cassandra adatbázis-Azure blob óránként. A minták következő szakaszok ezeket mintákban használt JSON tulajdonságok témakörben olvashat. Adatok másolhatók közvetlenül az Azure Data Factory a Másolás tevékenység használatával a [Tevékenységekre vonatkozó adatok mozgását](data-factory-data-movement-activities.md#supported-data-stores) cikk jelzett mosdók közül. 

- Csatolt szolgáltatás típusú [OnPremisesCassandra](#onpremisescassandra-linked-service-properties).
- Csatolt szolgáltatás típusú [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
- A beviteli [adatkészlet](data-factory-create-datasets.md) [CassandraTable](#cassandratable-properties)típusú.
- Egy kimenet [adatkészlet](data-factory-create-datasets.md) [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties)típusú.
- A [folyamat](data-factory-create-pipelines.md) [CassandraSource](#cassandrasource-type-properties) és [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties)használó másolása a tevékenységhez.

**Csatolt Cassandra szolgáltatás**

Ebben a példában a csatolt **Cassandra** szolgáltatást. Lásd: a tulajdonságok, ez csatolt szolgáltatás által támogatott [Cassandra csatolt szolgáltatás](#onpremisescassandra-linked-service-properties) szakaszban.  

    {
        "name": "CassandraLinkedService",
        "properties":
        {
            "type": "OnPremisesCassandra",
            "typeProperties":
            {
                "authenticationType": "Basic",
                "host": "mycassandraserver",
                "port": 9042,
                "username": "user",
                "password": "password",
                "gatewayName": "mygateway"
            }
        }
    }


**Azure csatolt tárhelyszolgáltatáshoz**

    {
        "name": "AzureStorageLinkedService",
        "properties": {
        "type": "AzureStorage",
            "typeProperties": {
                "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
            }
        }
    }

**Beviteli adatkészlet Cassandra**

    {
        "name": "CassandraInput",
        "properties": {
            "linkedServiceName": "CassandraLinkedService",
            "type": "CassandraTable",
            "typeProperties": {
                "tableName": "mytable",
                "keySpace": "mykeyspace" 
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true,
            "policy": {
                "externalData": {
                    "retryInterval": "00:01:00",
                    "retryTimeout": "00:10:00",
                    "maximumRetry": 3
                }
            }
        }
    }

Az adatok gyári szolgáltatás **Igaz** beállítás **külső** tájékoztatja, az adatkészlet adatok gyári mutató külső, és nem készül az adatok gyári tevékenységet.

**Azure Blob-kimeneti adatkészlet**

Adatok egy új blob íródott óránként (gyakoriság: óra, intervallum: 1). 

    {
        "name": "AzureBlobOutput",
        "properties":
        {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties":
            {
                "folderPath": "adfgetstarted/fromcassandra"
            },
            "availability":
            {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }


**Másolás tevékenységeket használó csővezeték**

A folyamat egy példány tevékenységet, amely a bemeneti és kimeneti adatkészleteket használatára van beállítva, és van ütemezve óránként tartalmazza. A során JSON megadása az **adatforrás** típusa **CassandraSource** van állítva, és **BlobSink** **gyűjtő** típusának beállítása. 

A listában a RelationalSource tulajdonságbeállítások [RelationalSource típusú tulajdonságok](#cassandrasource-type-properties) megtekintése 
    
    {  
        "name":"SamplePipeline",
        "properties":{  
            "start":"2016-06-01T18:00:00",
            "end":"2016-06-01T19:00:00",
            "description":"pipeline with copy activity",
            "activities":[  
            {
                "name": "CassandraToAzureBlob",
                "description": "Copy from Cassandra to an Azure blob",
                "type": "Copy",
                "inputs": [
                {
                    "name": "CassandraInput"
                }
                ],
                "outputs": [
                {
                    "name": "AzureBlobOutput"
                }
                ],
                "typeProperties": {
                    "source": {
                        "type": "CassandraSource",
                        "query": "select id, firstname, lastname from mykeyspace.mytable"
        
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
## <a name="onpremisescassandra-linked-service-properties"></a>Csatolt OnPremisesCassandra szolgáltatás tulajdonságai

Az alábbi táblázat ismerteti a csatolt Cassandra szolgáltatásra JSON elemek leírását.

| A tulajdonság | Leírás | Szükséges |
| -------- | ----------- | -------- | 
| típus | Meg kell a típusa tulajdonság: **OnPremisesCassandra** | igen |
| a Host | Egy vagy több IP-címek vagy Cassandra kiszolgáló állomás nevét.<br/><br/>Adja meg az IP-címek vagy az összes kiszolgálón a egyidejűleg csatlakozhat állomásnevek vesszővel tagolt listáját. | igen | 
| port | A TCP-port a Cassandra kiszolgáló által használt meghallgatásához az ügyfél-kapcsolatot. | Nem, alapértelmezett érték: 9042 |
| authenticationType | Alapszintű és névtelen | igen |
| felhasználónév |Adja meg a felhasználói fiókhoz tartozó felhasználónevet. | Igen, ha authenticationType Basic van beállítva. |
| jelszó | Adja meg a felhasználói fiók jelszava.  | Igen, ha authenticationType Basic van beállítva. |
| átjárónév | Az átjáró a helyszíni Cassandra adatbázishoz való csatlakozáshoz használt neve. | igen |
| encryptedCredential | A hitelesítő adatok az átjáró titkosítja. | nem | 

## <a name="cassandratable-properties"></a>CassandraTable tulajdonságai

Szakaszok és a rendelkezésre álló adatkészleteket meghatározásának tulajdonságok teljes listáját a [létrehozása adatkészleteket](data-factory-create-datasets.md) témakört is. Szakaszok, például a struktúra, elérhetőségét és egy adatkészletből JSON házirend hasonlóak az adatkészlet diagramtípusokat (Azure SQL Azure blob, Azure táblázat stb.).

A **typeProperties** szakasz eltérő adatkészlet hibatípusonként és az adatok tárolása az adatok helyének adatait. Az typeProperties szakaszban adatkészlet **CassandraTable** típusú rendelkezik az alábbi tulajdonságok

| A tulajdonság | Leírás | Szükséges |
| -------- | ----------- | -------- |
| keyspace | A keyspace vagy séma Cassandra adatbázis neve. | Igen (Ha nincs megadva a **lekérdezés** **CassandraSource** ). |
| Táblanév | A táblázat Cassandra adatbázis neve. | Igen (Ha nincs megadva a **lekérdezés** **CassandraSource** ). |


## <a name="cassandrasource-type-properties"></a>CassandraSource tulajdonságai
Szakaszok és a tevékenységek definiálásával használható tulajdonságok teljes listáját a [Folyamatok létrehozása](data-factory-create-pipelines.md) témakört is. Az összes tevékenységtípusokhoz tulajdonságait, például a név, leírás, a bemeneti és kimeneti táblák és házirend érhetők el. 

Minden tevékenység típusa azonban függenek tulajdonságok elérhető typeProperties szakaszában a tevékenységet. Másolás tevékenységhez azok eltérőek attól függően, hogy milyen típusú adatforrások és mosdók.

Ha forrás **CassandraSource**típusú, a következő tulajdonságokat érhetők el typeProperties szakasz:

| A tulajdonság | Leírás | Megengedett érték | Szükséges |
| -------- | ----------- | -------------- | -------- |
| lekérdezés | Az egyéni lekérdezés használatával olvassa el az adatok. | SQL-92 vagy CQL lekérdezés. [CQL – segédlet](https://docs.datastax.com/en/cql/3.1/cql/cql_reference/cqlReferenceTOC.html)című témakörben. <br/><br/>SQL-lekérdezés használata esetén adja meg a **keyspace name.table nevét** a lekérdezni kívánt táblázatot ábrázolásához. | Nem (Ha a táblanév és a adatkészlet keyspace vannak megadva).  |
| consistencyLevel | A konzisztencia szint határozza meg, hány kópiák kell olvasási-összehívás megválaszolása előtt a ügyfélalkalmazás adatokat ad vissza. Cassandra ellenőrzi a megadott számú kópiák az adatokat az olvasási kérelmet igényeinek kielégítéséhez. | EGY, A KÉT, HÁROM, KVÓRUM, AZ ÖSSZES LOCAL_QUORUM, EACH_QUORUM, LOCAL_ONE. [Konfigurálása adatok konzisztencia](http://docs.datastax.com/en//cassandra/2.0/cassandra/dml/dml_config_consistency_c.html) információt talál. | nem. Alapértelmezett értéket az egyik. |  


### <a name="type-mapping-for-cassandra"></a>Típus Cassandra hozzárendelése
Cassandra típusa | .NET-alapú típusa
--------------- | ---------------
ASCII | Karakterlánc 
BIGINT | Int64
BLOB | Byte]
LOGIKAI ÉRTÉK | Logikai érték
DECIMÁLIS | Decimális
DUPLA | Dupla
LEBEGTETÉS | Egyetlen
INET | Karakterlánc
INT | Int32
SZÖVEG | Karakterlánc
IDŐBÉLYEG | Dátum és idő
TIMEUUID | Globálisan egyedi azonosítója
UUID | Globálisan egyedi azonosítója
VARCHAR | Karakterlánc
VARINT | Decimális

> [AZURE.NOTE]  
> Webhelycsoport típusok (térkép, beállítása, lista, stb.), tekintse át a [munka használatáról virtuális tábla Cassandra webhelycsoport rajztípusok](#work-with-collections-using-virtual-table) szakaszban. 
> 
> Felhasználó által definiált már nem támogatott.
> 
> Bináris oszlop- és oszlop karakterlánc hossza hossza nem lehet nagyobb, mint 4000. 

## <a name="work-with-collections-using-virtual-table"></a>Virtuális táblázat segítségével webhelycsoportok kezelése
Azure Data Factory használja a beépített ODBC-illesztőprogram való kapcsolódáshoz és adatok másolása a Cassandra adatbázisból. A webhelycsoport típusai, így a térképet, beállítása és a lista a vezető újra normalizálja az adatok megfelelő virtuális táblákra osztja. Kifejezetten Ha egy táblázat minden webhelycsoport oszlopot tartalmaz, a az illesztőprogram az alábbi virtuális táblázatok hoz létre:

-   Egy **Alap táblázat**, amely tartalmazza a valódi asztal kivételével a webhelycsoport oszlopok ugyanazokat az adatokat. Az alap táblázat ugyanazt a nevet a valódi asztal, amely azt használja.
-   **Virtuális tábla** minden egyes webhelycsoport oszlophoz kibővíti az egymásba ágyazott adatokat. A virtuális táblákra gyűjtemények megjelenítő neve nevet a valódi asztal, az elválasztó "_vt_" és az oszlop nevét.

Virtuális táblázatok adatait a valódi asztal, így az nem normalizált adatok eléréséhez az illesztőprogram utalnak. Példa szakasz lásd: további információt. A tartalom Cassandra gyűjtemények lekérdezése és csatlakozás a virtuális táblákra is elérheti.

Kihasználhatja a [Másolás varázsló](data-factory-data-movement-activities.md#data-factory-copy-wizard) intuitív a táblák listájának megtekintéséhez Cassandra adatbázissal, amely a virtuális táblákat, és megtekintheti az belül az adatokat. Is másolás varázsló lekérdezés létrehozása és az eredmény a érvényesítése.

### <a name="example"></a>Példa
Például az alábbi "ExampleTable" egy Cassandra adatbázistáblát egy egész szám elsődleges kulcs nevű oszlopot "pk_int", megnevezett érték szöveg oszlop, a lista oszlopának, térkép oszlop és megadása ("StringSet" nevű) oszlop.

pk_int | Érték | Lista | Térkép |   StringSet
------ | ----- | ---- | --- | --------
1 | "minta értéke 1" | ["1", "2", "3"]  | {"S1": "a", "S2": "b"} | {"A", "B", "C"}
3 | "minta érték 3" | ["100", "101", "102", "105"] | {"S1": "t"} | {"A", "E"}

A vezető eredményezne több virtuális táblát az alábbi egyetlen táblázat ábrázolásához. Az idegen kulcs oszlopát a virtuális táblázatokban a valódi asztal az elsődleges kulcs oszlopok hivatkozást, és mely valós sor felel meg a virtuális táblázatsor jelzik. 

Az első virtuális tábla, a alap, a "ExampleTable" nevű táblázat az alábbi táblázatban látható. Az alap táblázat tartalmazza ugyanazokat az adatokat az eredeti adatbázis tábla a gyűjtemények, amelyek ebből a táblából hiányzik, és más virtuális táblák kibontva kivételével.

pk_int | Érték
------ | -----
1 | "minta értéke 1"
3 | "minta érték 3"

Az alábbi táblázatban a a listák és térkép, StringSet az adatok renormalize virtuális táblázat megjelenítéséhez. Az oszlopokat, hogy a "_index" vagy "_key" végződnie neveket azt jelzik, hogy az adatok az eredeti listát vagy a térképre belüli helyzetét. Az oszlopokat, hogy a "_value" végződjön neveket a gyűjteményből kibontott adatokat tartalmazzák.

#### <a name="table-exampletablevtlist"></a>Táblázat "ExampleTable_vt_List":
pk_int | List_index | List_value
------ | ---------- | ----------
1 | 0 | 1
1 | 1 | 2
1 | 2 | 3
3 | 0 | 100
3 | 1 | 101
3 | 2 | 102
3 | 3 | 103

#### <a name="table-exampletablevtmap"></a>Táblázat "ExampleTable_vt_Map":
pk_int | Map_key | Map_value
------ | ------- | ---------
1 | S1 | A
1 | S2 | b
3 | S1 | Kétmintás t

#### <a name="table-exampletablevtstringset"></a>Táblázat "ExampleTable_vt_StringSet":
pk_int | StringSet_value
------ | ---------------
1 | A
1 | B
1 | C
3 | A
3 | E





[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]
[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## <a name="performance-and-tuning"></a>A teljesítmény és a finombeállítása  
Lásd: a [Másolás tevékenység teljesítmény és útmutató beállítása](data-factory-copy-activity-performance.md) Ha többet szeretne tudni a főbb tényezők hatása teljesítmény mozgásának adatok (másolása a tevékenység) az Azure Data Factory és a különféle módokon optimalizálása azt.
