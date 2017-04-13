<properties 
    pageTitle="Adatok áthelyezése a MySQL |} Azure Data Factory" 
    description="Tudnivalók az adatokat áthelyezheti az Azure Data Factory használatával MySQL-adatbázishoz." 
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
    ms.date="09/20/2016" 
    ms.author="jingwang"/>

# <a name="move-data-from-mysql-using-azure-data-factory"></a>A MySQL Azure Data Factory használatával adatok áthelyezése

Ez a cikk azt ismerteti, hogyan használhatja a Másolás tevékenység-Azure adatok gyári a Ha az adatokat a MySQL egy másik adatok tárolására. Ez a cikk az adatok mozgás egy általános áttekintése másolás állapotát tevékenysége és a támogatott adatokat tároló kombinációk eltéréseit [Mozgás a tevékenységekre vonatkozó adatok](data-factory-data-movement-activities.md) cikk épül.

Adatok gyári szolgáltatás lehetővé teszi, a helyszíni MySQL-adatforrások az adatkezelési átjáró használatával csatlakozik. Ha többet szeretne tudni az adatkezelési átjáró és részletes útmutatást talál az átjáró beállítása [a helyszíni helyek és a felhő között mozgó adatok](data-factory-move-data-between-onprem-and-cloud.md) cikkében talál. 

> [AZURE.NOTE] Átjáró szükség, akkor is, ha a MySQL-adatbázis-Azure IaaS virtuális gépen (virtuális) üzemelteti. Telepítheti az átjáró a azonos virtuális az adatokat tároló vagy egy másik virtuális mindaddig, amíg az átjáró csatlakozhat az adatbázist.  

Adatok gyári jelenleg csak mozgó adatainak MySQL egyéb adatokat tárolja, de nem az adatok áthelyezése más adatokat tárolja a MySQL támogatja.

## <a name="installation"></a>Telepítés 
Adatkezelési átjáró a MySQL-adatbázishoz szeretne kapcsolódni akkor telepítenie kell a [MySQL-összekötő/nettó 6.6.5 a Microsoft Windows](http://go.microsoft.com/fwlink/?LinkId=278885) meg az adatkezelési átjáró ugyanazon a számítógépen.

> [AZURE.NOTE] Lásd: a kapcsolat/átjáró hibaelhárítási tippek [problémák elhárítása átjáró](data-factory-data-management-gateway.md#troubleshoot-gateway-issues) kapcsolatos problémákat. 

## <a name="copy-data-wizard"></a>Az adatok varázsló másolása
A hozzon létre egy folyamat, amely a MySQL-adatbázishoz valamelyik a támogatott gyűjtő adatokat tárolja, adatait másolja legegyszerűbben a Másolás adatok varázsló használatával. Lásd: [oktatóprogram: hozzon létre egy példány varázslóval folyamat](data-factory-copy-data-wizard-tutorial.md) létrehozása egy folyamat adatok másolása varázsló segítségével a rövid ismertetését megtalálja. 

Az alábbi példa a minta JSON-definíciók létrehozása egy folyamat használható biztosít. Egy teljes forgatókönyv lépésenkénti útmutatást a [helyszíni helyek és a felhő között mozgó adatok](data-factory-move-data-between-onprem-and-cloud.md) cikkében talál. Adatok valamelyik, az mosdók megadott [Itt](data-factory-data-movement-activities.md#supported-data-stores) a Másolás tevékenység használata Azure Data Factory másolható.   

## <a name="sample-copy-data-from-mysql-to-azure-blob"></a>Minta: Adatok másolása MySQL Azure Blob
Ez a példa bemutatja az adatok másolása a helyszíni MySQL-adatbázis-Azure Blob-tárolóhoz. Azonban adatokat másolt **közvetlenül** valamelyik, az mosdók megadott [Itt](data-factory-data-movement-activities.md#supported-data-stores) a Másolás tevékenység használata Azure Data Factory is lehet.  

> [AZURE.IMPORTANT] Ez a példa a JSON kódrészletek biztosít. Nem tartalmazza a adatok gyártás létrehozására vonatkozó lépésenkénti útmutatót. Részletes útmutatást [a helyszíni helyek és a felhő között mozgó adatok](data-factory-move-data-between-onprem-and-cloud.md) cikkében talál. 
 
A mintában a következő adatok gyári vállalkozások foglalja magában:

1.  Csatolt szolgáltatás típusú [OnPremisesMySql](data-factory-onprem-mysql-connector.md#mysql-linked-service-properties).
2.  Csatolt szolgáltatás típusú [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
3.  A beviteli [adatkészlet](data-factory-create-datasets.md) [RelationalTable](data-factory-onprem-mysql-connector.md#mysql-dataset-type-properties)típusú.
4.  Egy kimenet [adatkészlet](data-factory-create-datasets.md) [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties)típusú.
4.  A [folyamat](data-factory-create-pipelines.md) [RelationalSource](data-factory-onprem-mysql-connector.md#mysql-copy-activity-type-properties) és [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties)használó másolása a tevékenységhez.

A minta adatokat másolja a lekérdezés eredményét a MySQL-adatbázis blob óránként. A minták következő szakaszok ezeket mintákban használt JSON tulajdonságok témakörben olvashat. 

Első lépésként az adatkezelési átjáró beállítása. A képernyőn megjelenő utasításokat a [helyszíni helyek és a felhő között mozgó adatok](data-factory-move-data-between-onprem-and-cloud.md) cikk szerepelnek. 

**Csatolt MySQL-szolgáltatás**

    {
      "name": "OnPremMySqlLinkedService",
      "properties": {
        "type": "OnPremisesMySql",
        "typeProperties": {
          "server": "<server name>",
          "database": "<database name>",
          "schema": "<schema name>",
          "authenticationType": "<authentication type>",
          "userName": "<user name>",
          "password": "<password>",
          "gatewayName": "<gateway>"
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

**MySQL beviteli adatkészlet**

A minta feltételezi létrehozott táblázat "Táblanév" MySQL, és azt az időpontot adatsor "timestampcolumn" című oszlopot tartalmaz.

"Külső" beállítást: "igaz" tájékoztatja a Data Factory szolgáltatás a táblázatot az adatok gyári mutató külső, és nem készül az adatok gyári tevékenységet.
    
    {
        "name": "MySqlDataSet",
        "properties": {
            "published": false,
            "type": "RelationalTable",
            "linkedServiceName": "OnPremMySqlLinkedService",
            "typeProperties": {},
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



**Azure Blob-kimeneti adatkészlet**

Adatok egy új blob íródott óránként (gyakoriság: óra, intervallum: 1). A mappa elérési útját a blob dinamikusan kiértékeli a kezdési időpontot a szelet, amely feldolgozása alapján. A mappa elérési útja használja az év, hónap, nap és a kezdési időpontot óra részei.

    {
        "name": "AzureBlobMySqlDataSet",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/mysql/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
                "format": {
                    "type": "TextFormat",
                    "rowDelimiter": "\n",
                    "columnDelimiter": "\t"
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
                ]
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }



**Másolás tevékenységeket használó csővezeték**

A folyamat egy példány tevékenységet, amely a bemeneti és kimeneti adatkészleteket használatára van beállítva, és van ütemezve óránként tartalmazza. A során JSON megadása az **adatforrás** típusa **RelationalSource** van állítva, és **BlobSink** **gyűjtő** típusának beállítása. Az SQL-lekérdezés a **lekérdezés** tulajdonságban meghatározott adatok másolása az elmúlt órában kijelölése
    
    {
        "name": "CopyMySqlToBlob",
        "properties": {
            "description": "pipeline for copy activity",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "RelationalSource",
                            "query": "$$Text.Format('select * from MyTable where timestamp >= \\'{0:yyyy-MM-ddTHH:mm:ss}\\' AND timestamp < \\'{1:yyyy-MM-ddTHH:mm:ss}\\'', WindowStart, WindowEnd)"
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "MySqlDataSet"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobMySqlDataSet"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00",
                        "concurrency": 1
                    },
                    "scheduler": {
                        "frequency": "Hour",
                        "interval": 1
                    },
                    "name": "MySqlToBlob"
                }
            ],
            "start": "2014-06-01T18:00:00Z",
            "end": "2014-06-01T19:00:00Z"
        }
    }



## <a name="mysql-linked-service-properties"></a>Csatolt MySQL-szolgáltatás tulajdonságai

Az alábbi táblázat ismerteti a csatolt MySQL szolgáltatásra JSON elemek leírását.

| A tulajdonság | Leírás | Szükséges |
| -------- | ----------- | -------- | 
| típus | Meg kell a típusa tulajdonság: **OnPremisesMySql** | igen |
| kiszolgáló | A MySQL-kiszolgáló neve. | igen |
| adatbázis | A MySQL-adatbázis neve. | igen | 
| séma  | A séma az adatbázis nevét. | nem | 
| authenticationType | A MySQL-adatbázishoz való csatlakozáshoz használt hitelesítés típusa. Lehetséges értékek a következők: nélküli Basic és Windows. | igen | 
| felhasználónév | Adja meg a felhasználónevet, ha Basic és Windows-hitelesítés használatával. | nem | 
| jelszó | Adja meg a felhasználói fiók felhasználónév megadott jelszót. | nem | 
| átjárónév | Az adatok gyári szolgáltatás kell a helyszíni MySQL-adatbázishoz való csatlakozáshoz használt az átjáró neve. | igen |

Olvassa el [a beállítás hitelesítő adatok és](data-factory-move-data-between-onprem-and-cloud.md#set-credentials-and-security) biztonsági egy helyszíni MySQL-adatforráshoz tartozó hitelesítő adatok beállításával kapcsolatban további tájékoztatást.

## <a name="mysql-dataset-type-properties"></a>MySQL adatkészlet tulajdonságai

Szakaszok és a rendelkezésre álló adatkészleteket definiálása tulajdonságok teljes listáját a [létrehozása adatkészleteket](data-factory-create-datasets.md) témakört is. Szakaszok, például a struktúra, elérhetőségét és egy adatkészletből JSON házirend hasonlóak az adatkészlet diagramtípusokat (Azure SQL Azure blob, Azure táblázat stb.).

A **typeProperties** szakasz eltérő adatkészlet hibatípusonként és az adatok tárolása az adatok helyének adatait. Az typeProperties szakaszban adatkészlet **RelationalTable** (amely tartalmazza a MySQL-adatkészlet) típusú rendelkezik az alábbi tulajdonságok

| A tulajdonság | Leírás | Szükséges |
| -------- | ----------- | -------- |
| Táblanév | A tábla a csatolt szolgáltatás hivatkozó MySQL-adatbázis-példány nevét. | Nem (Ha nincs megadva a **lekérdezés** **RelationalSource** ) | 

## <a name="mysql-copy-activity-type-properties"></a>Tevékenység típusának tulajdonságok MySQL másolása

Szakaszok és a tevékenységek definiálásával használható tulajdonságok teljes listáját a [Folyamatok létrehozása](data-factory-create-pipelines.md) témakört is. Tulajdonságait, például a név, leírás, a bemeneti és kimeneti táblázatok, olyan házirendek érhetők el a tevékenységek diagramtípusokat. 

Minden tevékenység típusa azonban függenek tulajdonságok elérhető **typeProperties** szakaszában a tevékenységet. Másolás tevékenységhez azok eltérőek attól függően, hogy milyen típusú adatforrások és mosdók.

Adatforrás másolása tevékenység típusú **RelationalSource** (MySQL tartalmazó) esetén a következő tulajdonságokat érhetők el typeProperties szakasz:

| A tulajdonság | Leírás | Megengedett érték | Szükséges |
| -------- | ----------- | -------------- | -------- |
| lekérdezés | Az egyéni lekérdezés használatával olvassa el az adatok. | SQL-lekérdezés karakterlánc. Példa: válassza a * from tábla. | Nem (Ha a **táblanév** **adatkészletből** nincs megadva) | 

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

### <a name="type-mapping-for-mysql"></a>MySQL-leképezésének

A [tevékenységekre vonatkozó adatok mozgás](data-factory-data-movement-activities.md) a cikkben említett, a Másolás tevékenység forrástípus gyűjtése a következő két lépésből álló módszert az egyik automatikus típus konverzió hajtja végre:

1. Natív forrástípus konvertálása a .NET-típus
2. .NET-típusból származó átalakítása natív gyűjtő típusa

Adatok áthelyezése a MySQL, amikor a következő hozzárendelések használják a MySQL-típusok .NET típusokat.

| MySQL-adatbázis típusa | .NET-keretrendszer típusa |
| ------------------- | ------------------- | 
| aláíratlan bigint | Decimális |
| bigint | Int64 |
| bit | Decimális |
| BLOB | Byte] |
| logikai |  Logikai érték | 
| karakter | Karakterlánc |
| dátum | Dátum és idő |
| dátum és idő | Dátum és idő |
| decimális | Decimális |
| dupla pontosság | Dupla |
| dupla | Dupla |
| felsorolás | Karakterlánc |
| Lebegtetés | Egyetlen |
| aláíratlan int | Int64 |
| int | Int32 |
| aláíratlan egész szám | Int64 |
| egész szám | Int32 | 
| hosszú varbinary | Byte] |
| hosszú varchar | Karakterlánc |
| longblob | Byte] |
| LONGTEXT | Karakterlánc | 
| mediumblob |  Byte] | 
| aláíratlan mediumint | Int64 |
| mediumint | Int32 | 
| mediumtext | Karakterlánc |
| numerikus | Decimális |
| valós |  Dupla |
| beállítása | Karakterlánc |
| aláíratlan smallint | Int32 |
| smallint | Int16 |
| szöveg | Karakterlánc |
| idő | Időszak |
| időbélyeg | Dátum és idő |
| tinyblob | Byte] |
| aláíratlan tinyint |  Int16 | 
| tinyint | Int16 |
| tinytext | Karakterlánc | 
| varchar | Karakterlánc | 
| év | Int | 


[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-type-repeatability-for-relational-sources](../../includes/data-factory-type-repeatability-for-relational-sources.md)]

## <a name="performance-and-tuning"></a>A teljesítmény és a finombeállítása  
Lásd: a [Másolás tevékenység teljesítmény és útmutató beállítása](data-factory-copy-activity-performance.md) Ha többet szeretne tudni a főbb tényezők hatása teljesítmény mozgásának adatok (másolása a tevékenység) az Azure Data Factory és a különféle módokon optimalizálása azt.

