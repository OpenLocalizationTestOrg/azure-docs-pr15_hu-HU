<properties 
    pageTitle="Adatok áthelyezése a Teradata |} Azure Data Factory" 
    description="Tudnivalók a Teradata-összekötő a Data Factory szolgáltatás, amellyel a Teradata-adatbázisból származó adatok áthelyezése" 
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

# <a name="move-data-from-teradata-using-azure-data-factory"></a>Adatok áthelyezése a Teradata Azure Data Factory használatával

Ez a cikk ismerteti, hogyan használhatja a Másolás tevékenység-Azure adatok gyári a Ha az adatokat a Teradata egy másik adatok tárolására. Ez a cikk az adatok mozgás egy általános áttekintése másolás állapotát tevékenysége és a támogatott adatokat tároló kombinációk eltéréseit [Mozgás a tevékenységekre vonatkozó adatok](data-factory-data-movement-activities.md) cikk épül.

Adatok gyári támogat a helyszíni Teradata-adatforrások az adatkezelési átjáró keresztül csatlakozik. Ha többet szeretne tudni az adatkezelési átjáró és részletes útmutatást talál az átjáró beállítása [a helyszíni helyek és a felhő között mozgó adatok](data-factory-move-data-between-onprem-and-cloud.md) cikkében talál. 

> [AZURE.NOTE]
Átjáró szükség, akkor is, ha a Teradata-IaaS Azure virtuális a üzemelteti. Telepítheti az átjáró a adatokat tároló virtuális IaaS gép vagy egy másik virtuális mindaddig, amíg az átjáró csatlakozhat az adatbázist. 

Adatok gyári az egyéb adatokat tárolja a Teradata el, nem egyéb adatokat tárolja a Teradata-csak mozgó adatot támogatja.

## <a name="installation"></a>Telepítés 

Adatkezelési átjáró a Teradata-adatbázishoz szeretne kapcsolódni akkor telepítenie kell a [Teradata .NET adatszolgáltatója](http://go.microsoft.com/fwlink/?LinkId=278886) meg az adatkezelési átjáró ugyanazon a számítógépen.

> [AZURE.NOTE] Lásd: a kapcsolat/átjáró hibaelhárítási tippek [problémák elhárítása átjáró](data-factory-data-management-gateway.md#troubleshoot-gateway-issues) kapcsolatos problémákat. 

## <a name="copy-data-wizard"></a>Az adatok varázsló másolása
A hozzon létre egy folyamat, amely a Teradata valamelyik a támogatott gyűjtő adatokat tárolja, adatait másolja legegyszerűbben a Másolás adatok varázsló használatával. Lásd: [oktatóprogram: hozzon létre egy példány varázslóval folyamat](data-factory-copy-data-wizard-tutorial.md) létrehozása egy folyamat adatok másolása varázsló segítségével a rövid ismertetését megtalálja. 

Az alábbi példa a minta JSON-definíciók létrehozása egy folyamat [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) segítségével vagy [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) vagy [Azure Powershellhez](data-factory-copy-activity-tutorial-using-powershell.md)használható biztosít. Adatok másolása Teradata Azure Blob-tárolóhoz mutatnak. Adatok másolni lehet valamelyik, az mosdók megadott [Itt](data-factory-data-movement-activities.md#supported-data-stores) a Másolás tevékenység használata Azure Data Factory.   

### <a name="sample-copy-data-from-teradata-to-azure-blob"></a>Minta: Adatok másolása Teradata Azure Blob

Ez a példa bemutatja az Azure Blob-tárolóhoz adatok másolása Teradata-adatbázishoz. Azonban adatokat másolt **közvetlenül** valamelyik, az mosdók megadott [Itt](data-factory-data-movement-activities.md#supported-data-stores) a Másolás tevékenység használata Azure Data Factory is lehet.  
 
A mintában a következő adatok gyári vállalkozások foglalja magában:

1.  Csatolt szolgáltatás típusú [OnPremisesTeradata](data-factory-onprem-teradata-connector.md#teradata-linked-service-properties).
2.  Csatolt szolgáltatás típusú [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
3.  A beviteli [adatkészlet](data-factory-create-datasets.md) [RelationalTable](data-factory-onprem-teradata-connector.md#teradata-dataset-type-properties)típusú.
4.  Egy kimenet [adatkészlet](data-factory-create-datasets.md) [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties)típusú. 
4.  A [folyamat](data-factory-create-pipelines.md) [RelationalSource](data-factory-onprem-teradata-connector.md#teradata-copy-activity-type-properties) és [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties)használó másolása a tevékenységhez.

A minta adatokat másolja a lekérdezés eredményét a Teradata-adatbázis blob óránként. A minták következő szakaszok ezeket mintákban használt JSON tulajdonságok témakörben olvashat. 

Első lépésként az adatkezelési átjáró beállítása. A képernyőn megjelenő utasításokat a [helyszíni helyek és a felhő között mozgó adatok](data-factory-move-data-between-onprem-and-cloud.md) cikk szerepelnek.

**Csatolt Teradata-szolgáltatás:**

    {
        "name": "OnPremTeradataLinkedService",
        "properties": {
            "type": "OnPremisesTeradata",
            "typeProperties": {
                "server": "<server>",
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


**Teradata beviteli adatkészlet:**

A minta feltételezi létrehozott táblázat "Táblanév" Teradata és az idő adatsor "időbélyeg" című oszlop tartalmaz.

"Külső" beállítást: igaz tájékoztatja a Data Factory szolgáltatást, hogy a táblázat a adatok gyártás mutató külső, és nem készítette a adatok gyártás tevékenységet.

    {
        "name": "TeradataDataSet",
        "properties": {
            "published": false,
            "type": "RelationalTable",
            "linkedServiceName": "OnPremTeradataLinkedService",
            "typeProperties": {
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


**Azure Blob kimeneti adatkészlet:**

Adatok egy új blob íródott óránként (gyakoriság: óra, intervallum: 1). A mappa elérési útját a blob dinamikusan kiértékeli a kezdési időpontot a szelet, amely feldolgozása alapján. A mappa elérési útja használja az év, hónap, nap és a kezdési időpontot óra részei.

    {
        "name": "AzureBlobTeradataDataSet",
        "properties": {
            "published": false,
            "location": {
                "type": "AzureBlobLocation",
                "folderPath": "mycontainer/teradata/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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
                ],
                "linkedServiceName": "AzureStorageLinkedService"
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }


**Csővezeték másolása a tevékenységhez:**

A folyamat egy példány tevékenységet, amely a bemeneti és kimeneti adatkészleteket használatára van beállítva, és van ütemezve óránként tartalmazza. A során JSON megadása az **adatforrás** típusa **RelationalSource** van állítva, és **BlobSink** **gyűjtő** típusának beállítása. Az SQL-lekérdezés a **lekérdezés** tulajdonságban meghatározott adatok másolása az elmúlt órában kijelölése

    {
        "name": "CopyTeradataToBlob",
        "properties": {
            "description": "pipeline for copy activity",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "RelationalSource",
                            "query": "$$Text.Format('select * from MyTable where timestamp >= \\'{0:yyyy-MM-ddTHH:mm:ss}\\' AND timestamp < \\'{1:yyyy-MM-ddTHH:mm:ss}\\'', SliceStart, SliceEnd)"
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "TeradataDataSet"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobTeradataDataSet"
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
                    "name": "TeradataToBlob"
                }
            ],
            "start": "2014-06-01T18:00:00Z",
            "end": "2014-06-01T19:00:00Z",
            "isPaused": false
        }
    }


## <a name="teradata-linked-service-properties"></a>Csatolt Teradata-szolgáltatás tulajdonságai

Az alábbi táblázat ismerteti a csatolt Teradata szolgáltatásra JSON elemek leírását. 

A tulajdonság | Leírás | Szükséges
-------- | ----------- | --------
típus | Meg kell a típusa tulajdonság: **OnPremisesTeradata** | igen
kiszolgáló | A Teradata-kiszolgáló neve. | igen
authenticationType | A Teradata-adatbázishoz való csatlakozáshoz használt hitelesítés típusa. Lehetséges értékek a következők: nélküli Basic és Windows. | igen
felhasználónév | Adja meg a felhasználónevet, ha Basic és Windows-hitelesítés használatával. | nem 
jelszó | Adja meg a felhasználói fiók felhasználónév megadott jelszót. | nem 
átjárónév | Az adatok gyári szolgáltatás kell a helyszíni Teradata-adatbázishoz való csatlakozáshoz használt az átjáró neve. | igen

Olvassa el [a beállítás hitelesítő adatok és](data-factory-move-data-between-onprem-and-cloud.md#set-credentials-and-security) biztonsági egy helyszíni Teradata-adatforráshoz tartozó hitelesítő adatok beállításával kapcsolatban további tájékoztatást.

## <a name="teradata-dataset-type-properties"></a>Teradata adatkészlet tulajdonságai

Szakaszok és a rendelkezésre álló adatkészleteket definiálása tulajdonságok teljes listáját a [létrehozása adatkészleteket](data-factory-create-datasets.md) témakört is. Szakaszok, például a struktúra, elérhetőségét és egy adatkészletből JSON házirend hasonlóak az adatkészlet diagramtípusokat (Azure SQL Azure blob, Azure táblázat stb.).

A **typeProperties** szakasz eltérő adatkészlet hibatípusonként és az adatok tárolása az adatok helyének adatait. Vannak jelenleg nem támogatott a Teradata-adatkészlet tulajdonságokat. 


## <a name="teradata-copy-activity-type-properties"></a>Tevékenység típusának tulajdonságok Teradata másolása

Szakaszok és a tevékenységek definiálásával használható tulajdonságok teljes listáját a [Folyamatok létrehozása](data-factory-create-pipelines.md) témakört is. Az összes tevékenységtípusokhoz tulajdonságait, például a név, leírás, a bemeneti és kimeneti táblák és házirendek érhetők el. 

Minden tevékenység típusa azonban függenek tulajdonságok elérhető typeProperties szakaszában a tevékenységet. Másolás tevékenységhez azok eltérőek attól függően, hogy milyen típusú adatforrások és mosdók.

**RelationalSource** (amely tartalmazza a Teradata) típusú adatforrások esetén az alábbi tulajdonságok érhetők el **typeProperties** szakasz:

A tulajdonság | Leírás | Megengedett érték | Szükséges
-------- | ----------- | -------------- | --------
lekérdezés | Az egyéni lekérdezés használatával olvassa el az adatok. | SQL-lekérdezés karakterlánc. Példa: válassza a * from tábla. | igen

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## <a name="type-mapping-for-teradata"></a>Teradata típusú hozzárendelése

Az [elmozdulásnak a tevékenységekre vonatkozó adatok](data-factory-data-movement-activities.md) a cikkben említett, a Másolás tevékenységet a forrástípus gyűjtése a következő lépés 2 megközelítés az egyik automatikus típus konverzió hajtja végre:

1. Natív forrástípus konvertálása a .NET-típus
2. .NET-típusból származó átalakítása natív gyűjtő típusa

Adatok áthelyezése a Teradata, amikor a következő hozzárendeléseit szolgálnak a Teradata típusú .NET típusát.

Teradata-adatbázis típusa | .NET-keretrendszer típusa
----------------- | ---------------------------
Karakter | Karakterlánc
CLOB | Karakterlánc
Ábra | Karakterlánc
VarChar | Karakterlánc
VarGraphic | Karakterlánc
BLOB | Byte]
Bájt | Byte]
VarByte | Byte]
BigInt | Int64
ByteInt | Int16
Decimális | Decimális
Dupla | Dupla
Egész szám | Int32
Szám | Dupla
SmallInt | Int16
Dátum | Dátum és idő
Idő | Időszak
Az időzóna idő | Karakterlánc
Időbélyeg | Dátum és idő
Az időzóna időbélyeg | DateTimeOffset
Intervallum nap | Időszak
Intervallum nap óra | Időszak
Intervallum nap Perccé | Időszak
Második intervallum nap | Időszak
Intervallum óra | Időszak
Intervallum óra, perc alatt | Időszak
Intervallum óra második | Időszak
Intervallum perc | Időszak
Intervallum perc, másodperc | Időszak
Intervallum második | Időszak
Intervallum év | Karakterlánc
Intervallum év hónapra. | Karakterlánc
Intervallum hónap | Karakterlánc
Period(date) | Karakterlánc
Period(Time) | Karakterlánc
Időszak (az időzóna idő) | Karakterlánc
Period(Timestamp) | Karakterlánc
Időszak (az időzóna időbélyeg) | Karakterlánc
XML | Karakterlánc

[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-type-repeatability-for-relational-sources](../../includes/data-factory-type-repeatability-for-relational-sources.md)]

## <a name="performance-and-tuning"></a>A teljesítmény és a finombeállítása  
Lásd: a [Másolás tevékenység teljesítmény és útmutató beállítása](data-factory-copy-activity-performance.md) Ha többet szeretne tudni a főbb tényezők hatása teljesítmény mozgásának adatok (másolása a tevékenység) az Azure Data Factory és a különféle módokon optimalizálása azt.