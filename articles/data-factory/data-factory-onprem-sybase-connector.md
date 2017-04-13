<properties 
    pageTitle="Adatok áthelyezése Sybase |} Azure Data Factory" 
    description="További tudnivalók Azure Data Factory használatával Sybase-adatbázisból származó adatok áthelyezése." 
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

# <a name="move-data-from-sybase-using-azure-data-factory"></a>Az adatokat áthelyezheti az Azure Data Factory használatával Sybase 

Ez a cikk azt ismerteti, hogyan használhatja a Másolás tevékenység az Azure adatok gyári az adatok áthelyezése egy másik adattár Sybase. Ez a cikk az adatok mozgás egy általános áttekintése másolás állapotát tevékenysége és a támogatott adatokat tároló kombinációk eltéréseit [Mozgás a tevékenységekre vonatkozó adatok](data-factory-data-movement-activities.md) cikk épül.

Adatok gyári szolgáltatás lehetővé teszi, a helyszíni Sybase adatforrások az adatkezelési átjáró használatával csatlakozik. Ha többet szeretne tudni az adatkezelési átjáró és részletes útmutatást talál az átjáró beállítása [a helyszíni helyek és a felhő között mozgó adatok](data-factory-move-data-between-onprem-and-cloud.md) cikkében talál. 

> [AZURE.NOTE]
> Átjáró szükség, akkor is, ha a Sybase-adatbázis-IaaS Azure virtuális a üzemelteti. Telepítheti az átjáró a adatokat tároló virtuális IaaS gép vagy egy másik virtuális mindaddig, amíg az átjáró csatlakozhat az adatbázist. 

Adatok gyári jelenleg csak mozgó adatok az egyéb adatok áruházhoz Sybase el, nem Sybase egyéb adatokat tárolja.

## <a name="installation"></a>Telepítés

Adatkezelési átjáró a Sybase-adatbázishoz szeretne kapcsolódni akkor telepítenie kell a [Sybase adatszolgáltatója](http://go.microsoft.com/fwlink/?linkid=324846) meg ugyanazon a számítógépen az adatkezelési átjáró.

> [AZURE.NOTE] Lásd: a kapcsolat/átjáró hibaelhárítási tippek [problémák elhárítása átjáró](data-factory-data-management-gateway.md#troubleshoot-gateway-issues) kapcsolatos problémákat. 

## <a name="copy-data-wizard"></a>Az adatok varázsló másolása
A hozzon létre egy folyamat, amely másolja az adatokat egy Sybase-adatbázisból valamelyik támogatott gyűjtő adatok tárolóban legegyszerűbben a Másolás adatok varázsló használatával. Lásd: [oktatóprogram: hozzon létre egy példány varázslóval folyamat](data-factory-copy-data-wizard-tutorial.md) létrehozása egy folyamat adatok másolása varázsló segítségével a rövid ismertetését megtalálja. 

Az alábbi példa a minta JSON-definíciók létrehozása egy folyamat [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) segítségével vagy [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) vagy [Azure Powershellhez](data-factory-copy-activity-tutorial-using-powershell.md)használható biztosít. Sybase-adatbázisból származó adatok másolása, Azure Blob-tárolóhoz mutatnak. Adatok másolni lehet valamelyik, az mosdók megadott [Itt](data-factory-data-movement-activities.md#supported-data-stores) a Másolás tevékenység használata Azure Data Factory.   

## <a name="sample-copy-data-from-sybase-to-azure-blob"></a>Minta: Adatok másolása Sybase Azure Blob
Ez a példa bemutatja, hogyan Sybase-adatbázis adatainak másolása az Azure Blob-tárolóhoz. Azonban adatokat másolt **közvetlenül** valamelyik, az mosdók megadott [Itt](data-factory-data-movement-activities.md#supported-data-stores) a Másolás tevékenység használata Azure Data Factory is lehet.  
 
A mintában a következő adatok gyári vállalkozások foglalja magában:

1.  Csatolt szolgáltatás típusú [OnPremisesSybase](data-factory-onprem-sybase-connector.md#sybase-linked-service-properties).
2.  A típus [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties)liked szolgáltatás.
3.  A beviteli [adatkészlet](data-factory-create-datasets.md) [RelationalTable](data-factory-onprem-sybase-connector.md#sybase-dataset-type-properties)típusú.
4.  Egy kimenet [adatkészlet](data-factory-create-datasets.md) [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties)típusú.
4.  A [folyamat](data-factory-create-pipelines.md) [RelationalSource](data-factory-onprem-sybase-connector.md#sybase-copy-activity-type-properties) és [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties)használó másolása a tevékenységhez.

A minta adatokat másolja a lekérdezés eredményét Sybase-adatbázis blob óránként. A minták következő szakaszok ezeket mintákban használt JSON tulajdonságok témakörben olvashat. 

Első lépésként az adatkezelési átjáró beállítása. A képernyőn megjelenő utasításokat a [helyszíni helyek és a felhő között mozgó adatok](data-factory-move-data-between-onprem-and-cloud.md) cikk szerepelnek.

**Csatolt Sybase-szolgáltatás:**

    {
        "name": "OnPremSybaseLinkedService",
        "properties": {
            "type": "OnPremisesSybase",
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


**Sybase beviteli adatkészlet:**

A minta feltételezi létrehozott táblázat "Táblanév" Sybase és az idő adatsor "időbélyeg" című oszlop tartalmaz.

"Külső" beállítást: igaz tájékoztatja a Data Factory szolgáltatás a adatkészlet adatok gyári mutató külső, és nem készül az adatok gyári tevékenységet. Figyelje meg, hogy a **Írja be** a csatolt szolgáltatás beállítása: **RelationalTable**. 
    
    {
        "name": "SybaseDataSet",
        "properties": {
            "type": "RelationalTable",
            "linkedServiceName": "OnPremSybaseLinkedService",
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
        "name": "AzureBlobSybaseDataSet",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/sybase/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

A folyamat egy példány tevékenységet, amely a bemeneti és kimeneti adatkészleteket használatára van beállítva, és van ütemezve óránkénti tartalmazza. A során JSON megadása az **adatforrás** típusa **RelationalSource** van állítva, és **BlobSink** **gyűjtő** típusának beállítása. Az SQL-lekérdezés a **lekérdezés** tulajdonságban meghatározott adatokat a kereskedelmi választja ki. Az adatbázis Orders táblában.


    {
        "name": "CopySybaseToBlob",
        "properties": {
            "description": "pipeline for copy activity",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "RelationalSource",
                            "query": "select * from DBA.Orders"
                        },
                        "sink": {
                            "type": "BlobSink"
                        }
                    },
                    "inputs": [
                        {
                            "name": "SybaseDataSet"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobSybaseDataSet"
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
                    "name": "SybaseToBlob"
                }
            ],
            "start": "2014-06-01T18:00:00Z",
            "end": "2014-06-01T19:00:00Z"
        }
    }


## <a name="sybase-linked-service-properties"></a>Csatolt Sybase szolgáltatás tulajdonságai

Az alábbi táblázat ismerteti a csatolt Sybase szolgáltatásra JSON elemek leírását.

A tulajdonság | Leírás | Szükséges
-------- | ----------- | --------
típus | Meg kell a típusa tulajdonság: **OnPremisesSybase** | igen
kiszolgáló | A Sybase-kiszolgáló neve. | igen
adatbázis | Sybase-adatbázis neve. | igen 
séma  | A séma az adatbázis nevét. | nem
authenticationType | A Sybase-adatbázishoz való csatlakozáshoz használt hitelesítés típusa. Lehetséges értékek a következők: nélküli Basic és Windows. | igen
felhasználónév | Adja meg a felhasználónevet, ha Basic és Windows-hitelesítés használatával. | nem
jelszó | Adja meg a felhasználói fiók felhasználónév megadott jelszót. |  nem
átjárónév | Az adatok gyári szolgáltatás kell a helyszíni Sybase-adatbázishoz való csatlakozáshoz használt az átjáró neve. | igen 

Olvassa el [a beállítás hitelesítő adatok és](data-factory-move-data-between-onprem-and-cloud.md#set-credentials-and-security) biztonsági egy helyszíni Sybase-adatforráshoz tartozó hitelesítő adatok beállításával kapcsolatban további tájékoztatást.

## <a name="sybase-dataset-type-properties"></a>Sybase adatkészlet tulajdonságai

Szakaszok és a rendelkezésre álló adatkészleteket meghatározásának tulajdonságok teljes listáját a [létrehozása adatkészleteket](data-factory-create-datasets.md) témakört is. Szakaszok, például a struktúra, elérhetőségét és egy adatkészletből JSON házirend hasonlóak az adatkészlet diagramtípusokat (Azure SQL Azure blob, Azure táblázat stb.).

A typeProperties szakasz eltérő adatkészlet hibatípusonként és az adatok tárolása az adatok helyének adatait. A következő tulajdonságokat a **typeProperties** szakaszát adatkészlet **RelationalTable** (Sybase adatkészlet tartalmazó) típusú foglalja magában:

A tulajdonság | Leírás | Szükséges
-------- | ----------- | --------
Táblanév | A tábla a csatolt szolgáltatás hivatkozó Sybase-adatbázis-példány nevét. | Nem (Ha nincs megadva a **lekérdezés** **RelationalSource** )

## <a name="sybase-copy-activity-type-properties"></a>Tevékenység típusának tulajdonságok Sybase másolása 

Szakaszok és a tevékenységek, lásd: [létrehozása csővezetékek](data-factory-create-pipelines.md) cikk definiálása elérhető tulajdonságok teljes listáját. Az összes tevékenységtípusokhoz tulajdonságait, például a név, leírás, a bemeneti és kimeneti táblák és házirend érhetők el. 

Minden tevékenység típusa azonban függenek tulajdonságok elérhető typeProperties szakaszában a tevékenységet. Másolás tevékenységhez azok eltérőek attól függően, hogy milyen típusú adatforrások és mosdók.

**RelationalSource** (Sybase tartalmazó) típusú adatforrások esetén az alábbi tulajdonságok szakaszában érhetők **typeProperties** :

A tulajdonság | Leírás | Megengedett érték | Szükséges
-------- | ----------- | -------------- | --------
lekérdezés | Az egyéni lekérdezés használatával olvassa el az adatok. | SQL-lekérdezés karakterlánc. Példa: válassza a * from tábla. | Nem (Ha a **táblanév** **adatkészletből** nincs megadva)

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## <a name="type-mapping-for-sybase"></a>Sybase hozzárendelése típusa

Az [Elmozdulásnak a tevékenységekre vonatkozó adatok](data-factory-data-movement-activities.md) a cikkben említett, a Másolás tevékenységet a forrástípus gyűjtése a következő lépés 2 megközelítés az egyik automatikus típus konverzió hajtja végre:

1. Natív forrástípus konvertálása a .NET-típus
2. .NET-típusból származó átalakítása natív gyűjtő típusa

Sybase támogatja az SQL-T, és az SQL-T típusú. A leképezési táblázat az sql-típusok .NET típusra, [Azure SQL-összekötő](data-factory-azure-sql-connector.md) című cikket.

[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-type-repeatability-for-relational-sources](../../includes/data-factory-type-repeatability-for-relational-sources.md)]

## <a name="performance-and-tuning"></a>A teljesítmény és a finombeállítása  
Lásd: a [Másolás tevékenység teljesítmény és útmutató beállítása](data-factory-copy-activity-performance.md) Ha többet szeretne tudni a főbb tényezők hatása teljesítmény mozgásának adatok (másolása a tevékenység) az Azure Data Factory és a különféle módokon optimalizálása azt.