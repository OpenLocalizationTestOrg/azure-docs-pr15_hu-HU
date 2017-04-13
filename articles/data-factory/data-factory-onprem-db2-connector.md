<properties 
    pageTitle="Adatok áthelyezése DB2 |} Azure Data Factory" 
    description="További tudnivalók az adatokat áthelyezheti az Azure Data Factory használatával DB2-adatbázishoz" 
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
    ms.date="09/06/2016" 
    ms.author="jingwang"/>

# <a name="move-data-from-db2-using-azure-data-factory"></a>Azure Data Factory használatával DB2 adatainak áthelyezése
Ez a cikk ismerteti, hogyan használhatja a Másolás tevékenység-Azure adatok gyári a áthelyezése egy másik adatok DB2 áruházból az adatokat. Ez a cikk az adatok mozgás egy általános áttekintése másolás állapotát tevékenysége és a támogatott adatokat tároló kombinációk eltéréseit [Mozgás a tevékenységekre vonatkozó adatok](data-factory-data-movement-activities.md) cikk épül.

Adatok gyári támogat a helyszíni DB2 adatforrások az adatkezelési átjáró segítségével kapcsolatot. Ha többet szeretne tudni az adatkezelési átjáró és részletes útmutatást talál az átjáró beállítása [a helyszíni helyek és a felhő között mozgó adatok](data-factory-move-data-between-onprem-and-cloud.md) cikkében talál. 

> [AZURE.NOTE]
Az átjáró segítségével DB2 kapcsolódni, akkor is, ha az Azure IaaS VMs helyezkedik el. Ha egy példány a felhőben tárolt DB2 csatlakozni próbál, a IaaS virtuális is a átjárópéldány is telepítheti.

Adatok gyári jelenleg csak mozgó adatok az egyéb adatok áruházhoz DB2 el, nem egyéb adatok áruházak DB2 támogatja. 

## <a name="installation"></a>Telepítés 

Az adatkezelési átjáró tartalmaz egy beépített DB2 eszközillesztőt, amely támogatja az alábbiakat: 

- SQLAM 9 / 10 ÉS 11
- DB2-LUW (Linux, Unix, Windows)
- DB2 z/OS és DB2-e (más néven, és 400)

Ezért már nincs szüksége manuálisan az illesztőprogramjainak telepítését, ha az adatok másolása DB2.

> [AZURE.NOTE] Lásd: a kapcsolat/átjáró hibaelhárítási tippek [problémák elhárítása átjáró](data-factory-data-management-gateway.md#troubleshoot-gateway-issues) kapcsolatos problémákat. 

## <a name="copy-data-wizard"></a>Az adatok varázsló másolása
A hozzon létre egy folyamat, amely egy DB2-adatbázisból származó adatokat másolja legegyszerűbben a Másolás adatok varázsló használatával. Lásd: [oktatóprogram: hozzon létre egy példány varázslóval folyamat](data-factory-copy-data-wizard-tutorial.md) létrehozása egy folyamat adatok másolása varázsló segítségével a rövid ismertetését megtalálja. 

Az alábbi példák adnia minta JSON-definíciók létrehozása egy folyamat [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) segítségével vagy [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) vagy [Azure Powershellhez](data-factory-copy-activity-tutorial-using-powershell.md)használható. Adatok másolása DB2-adatbázishoz, és Azure Blob-tárolóhoz mutatnak. Adatok másolni lehet valamelyik, az mosdók megadott [Itt](data-factory-data-movement-activities.md#supported-data-stores) a Másolás tevékenység használata Azure Data Factory.

## <a name="sample-copy-data-from-db2-to-azure-blob"></a>Minta: Adatok másolása DB2 Azure Blob

Ez a példa bemutatja az adatok másolása a helyszíni DB2-adatbázis-Azure Blob-tárolóhoz. Azonban adatokat másolt **közvetlenül** valamelyik, az mosdók megadott [Itt](data-factory-data-movement-activities.md#supported-data-stores) a Másolás tevékenység használata Azure Data Factory is lehet.  
 
A mintában a következő adatok gyári vállalkozások foglalja magában:

1.  Csatolt szolgáltatás típusú [OnPremisesDb2](data-factory-onprem-db2-connector.md#db2-linked-service-properties).
2.  Csatolt szolgáltatás típusú [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties). 
3.  A beviteli [adatkészlet](data-factory-create-datasets.md) [RelationalTable](data-factory-onprem-db2-connector.md#db2-dataset-type-properties)típusú.
4.  Egy kimenet [adatkészlet](data-factory-create-datasets.md) [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties)típusú. 
5.  A [folyamat](data-factory-create-pipelines.md) [RelationalSource](data-factory-onprem-db2-connector.md#db2-copy-activity-type-properties) és [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties)használó másolása a tevékenységhez. 

A minta másolja át adatokat DB2-adatbázishoz a lekérdezés eredményét egy Azure blob óránként. A minták következő szakaszok ezeket mintákban használt JSON tulajdonságok témakörben olvashat. 

Első lépésként telepítse és állítsa be az adatkezelési átjáró. Útmutatást a [helyszíni helyek és a felhő között mozgó adatok](data-factory-move-data-between-onprem-and-cloud.md) cikk szerepelnek.

**Csatolt DB2 szolgáltatás:**

    {
        "name": "OnPremDb2LinkedService",
        "properties": {
            "type": "OnPremisesDb2",
            "typeProperties": {
                "server": "<server>",
                "database": "<database>",
                "schema": "<schema>",
                "authenticationType": "<authentication type>",
                "username": "<username>",
                "password": "<password>",
                "gatewayName": "<gatewayName>"
            }
        }
    }


**Azure Blob-tárolóhoz összekapcsolt szolgáltatás:**

    {
        "name": "AzureStorageLinkedService",
        "properties": {
            "type": "AzureStorageLinkedService",
            "typeProperties": {
                "connectionString": "DefaultEndpointsProtocol=https;AccountName=<AccountName>;AccountKey=<AccountKey>"
            }
        }
    }

**DB2 beviteli adatkészlet:**

A minta feltételezi létrehozott táblázat "Táblanév" DB2 és az idő adatsor "időbélyeg" című oszlop tartalmaz.

"Külső" beállítást: igaz tájékoztatja a Data Factory szolgáltatás a adatkészlet adatok gyári mutató külső, és nem készül az adatok gyári tevékenységet. Figyelje meg, hogy a **típus** **RelationalTable**értéke. 

    {
        "name": "Db2DataSet",
        "properties": {
            "type": "RelationalTable",
            "linkedServiceName": "OnPremDb2LinkedService",
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


**Azure Blob kimeneti adatkészlet:**

Adatok egy új blob íródott óránként (gyakoriság: óra, intervallum: 1). A mappa elérési útját a blob dinamikusan kiértékeli a kezdési időpontot a szelet, amely feldolgozása alapján. A mappa elérési útja használja az év, hónap, nap és a kezdési időpontot óra részei.

    {
        "name": "AzureBlobDb2DataSet",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/db2/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

**Csővezeték másolása a tevékenységhez:**

A folyamat egy példány tevékenységet, amely a bemeneti és kimeneti adatkészleteket használatára van beállítva, és van ütemezve óránként tartalmazza. A során JSON megadása az **adatforrás** típusa **RelationalSource** van állítva, és **BlobSink** **gyűjtő** típusának beállítása. A **lekérdezés** tulajdonságban meghatározott SQL-lekérdezés kijelöli az adatokat, a Rendelések táblából.

    {
        "name": "CopyDb2ToBlob",
        "properties": {
            "description": "pipeline for copy activity",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "RelationalSource",
                            "query": "select * from \"Orders\""
                        },
                        "sink": {
                            "type": "BlobSink"
                        }
                    },
                    "inputs": [
                        {
                            "name": "Db2DataSet"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobDb2DataSet"
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
                    "name": "Db2ToBlob"
                }
            ],
            "start": "2014-06-01T18:00:00Z",
            "end": "2014-06-01T19:00:00Z"
        }
    }


## <a name="db2-linked-service-properties"></a>Csatolt DB2 szolgáltatás tulajdonságai

Az alábbi táblázat ismerteti a csatolt DB2 szolgáltatásra JSON elemek leírását. 

| A tulajdonság | Leírás | Szükséges |
| -------- | ----------- | -------- | 
| típus | Meg kell a típusa tulajdonság: **OnPremisesDB2** | igen |
| kiszolgáló | A DB2-kiszolgáló neve. | igen |
| adatbázis | A DB2-adatbázis neve. | igen |
| séma | A séma az adatbázis nevét. A séma neve megegyezik a kis-és nagybetűk. | nem |
| authenticationType | A DB2-adatbázishoz való csatlakozáshoz használt hitelesítés típusa. Lehetséges értékek a következők: nélküli Basic és Windows. | igen |
| felhasználónév | Adja meg a felhasználónevet, ha Basic és Windows-hitelesítés használatával. | nem |
| jelszó | Adja meg a felhasználói fiók felhasználónév megadott jelszót. | nem |
| átjárónév | Az adatok gyári szolgáltatás kell a helyszíni DB2-adatbázishoz való csatlakozáshoz használt az átjáró neve. | igen |

Olvassa el [a beállítás hitelesítő adatok és](data-factory-move-data-between-onprem-and-cloud.md#set-credentials-and-security) biztonsági egy helyszíni DB2 adatforráshoz tartozó hitelesítő adatok beállításával kapcsolatban további tájékoztatást. 


## <a name="db2-dataset-type-properties"></a>DB2 adatkészlet tulajdonságai

Szakaszok és a rendelkezésre álló adatkészleteket definiálása tulajdonságok teljes listáját a [létrehozása adatkészleteket](data-factory-create-datasets.md) témakört is. Szakaszok, például a struktúra, elérhetőségét és egy adatkészletből JSON házirend hasonlóak az adatkészlet diagramtípusokat (Azure SQL Azure blob, Azure táblázat stb.).

A typeProperties szakasz eltérő adatkészlet hibatípusonként és az adatok tárolása az adatok helyének adatait. Az typeProperties szakaszban adatkészlet típusú RelationalTable (DB2 adatkészlet tartalmazó) tartalmaz, a következő tulajdonságokat.

| A tulajdonság | Leírás | Szükséges |
| -------- | ----------- | -------- | 
| Táblanév | A tábla a csatolt szolgáltatás hivatkozó DB2-adatbázis-példány nevét. A táblanév a kis-és nagybetűk. | Nem (Ha nincs megadva a **lekérdezés** **RelationalSource** ) |

## <a name="db2-copy-activity-type-properties"></a>Tevékenység típusának tulajdonságok DB2 másolása

Szakaszok és a tevékenységek definiálásával használható tulajdonságok teljes listáját a [Folyamatok létrehozása](data-factory-create-pipelines.md) témakört is. Az összes tevékenységtípusokhoz tulajdonságait, például a név, leírás, a bemeneti és kimeneti táblák és házirendek érhetők el. 

Minden tevékenység típusa azonban függenek tulajdonságok elérhető typeProperties szakaszában a tevékenységet. Másolás tevékenységhez azok eltérőek attól függően, hogy milyen típusú adatforrások és mosdók.

Másolás tevékenységhez, **RelationalSource** (DB2 tartalmazó) típusú adatforrások esetén az alábbi tulajdonságok érhetők el typeProperties szakasz:


| A tulajdonság | Leírás | Megengedett érték | Szükséges |
| -------- | ----------- | -------- | -------------- |
| lekérdezés | Az egyéni lekérdezés használatával olvassa el az adatok. | SQL-lekérdezés karakterlánc. Példa: `"query": "select * from "MySchema"."MyTable""`. | Nem (Ha a **táblanév** **adatkészletből** nincs megadva)|

> [AZURE.NOTE] Séma és táblanevek nagybetűk között. Idézőjelek közé kell tenni a neveket a "" (dupla idézőjel) a lekérdezésben.  

**Példa:**

    "query": "select * from "DB2ADMIN"."Customers""


[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## <a name="type-mapping-for-db2"></a>Típus DB2 hozzárendelése
Az [elmozdulásnak a tevékenységekre vonatkozó adatok](data-factory-data-movement-activities.md) a cikkben említett, a Másolás tevékenységet a forrástípus gyűjtése a következő lépés 2 megközelítés az egyik automatikus típus konverzió hajtja végre:

1. Natív forrástípus konvertálása a .NET-típus
2. .NET-típusból származó átalakítása natív gyűjtő típusa

Adatok áthelyezése a DB2, amikor a következő hozzárendelések szolgálnak DB2-típusból származó .NET típus.

A típus DB2-adatbázishoz | .NET-keretrendszer típusa 
----------------- | ------------------- 
SmallInt | Int16
Egész szám | Int32
BigInt | Int64
Valós | Egyetlen
Dupla | Dupla
Lebegtetés | Dupla
Decimális | Decimális
DecimalFloat | Decimális
Numerikus | Decimális
Dátum | Dátum és idő
Idő | Időszak
Időbélyeg | Dátum és idő
XML | Byte]
Karakter | Karakterlánc
VarChar | Karakterlánc
LongVarChar | Karakterlánc
DB2DynArray | Karakterlánc
Bináris | Byte]
VarBinary | Byte]
LongVarBinary | Byte]
Ábra | Karakterlánc
VarGraphic | Karakterlánc
LongVarGraphic | Karakterlánc
CLOB | Karakterlánc
BLOB | Byte]
DbClob | Karakterlánc
SmallInt | Int16
Egész szám | Int32
BigInt | Int64
Valós | Egyetlen
Dupla | Dupla
Lebegtetés | Dupla
Decimális | Decimális
DecimalFloat | Decimális
Numerikus | Decimális
Dátum | Dátum és idő
Idő | Időszak
Időbélyeg | Dátum és idő
XML | Byte]
Karakter | Karakterlánc


[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]


[AZURE.INCLUDE [data-factory-type-repeatability-for-relational-sources](../../includes/data-factory-type-repeatability-for-relational-sources.md)]

## <a name="performance-and-tuning"></a>A teljesítmény és a finombeállítása  
Lásd: a [Másolás tevékenység teljesítmény és útmutató beállítása](data-factory-copy-activity-performance.md) Ha többet szeretne tudni a főbb tényezők hatása teljesítmény mozgásának adatok (másolása a tevékenység) az Azure Data Factory és a különféle módokon optimalizálása azt.


