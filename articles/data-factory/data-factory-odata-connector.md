<properties 
    pageTitle="Az adatokat áthelyezheti az OData-adatforrások |} Azure Data Factory" 
    description="Ismerje meg az adatokat áthelyezheti az Azure Data Factory használatával OData-adatforrások használatáról." 
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
    ms.date="09/26/2016" 
    ms.author="jingwang"/>

# <a name="move-data-from-a-odata-source-using-azure-data-factory"></a>Adatok Azure Data Factory használata a OData az adatforrás áthelyezése
Ez a cikk azt ismerteti, hogyan használhatja a Másolás tevékenység-Azure adatok gyári az OData forrásból származó adatok áthelyezése egy másik adattár. Ez a cikk az adatok mozgás egy általános áttekintése másolás állapotát tevékenysége és a támogatott adatokat tároló kombinációk eltéréseit [Mozgás a tevékenységekre vonatkozó adatok](data-factory-data-movement-activities.md) cikk épül.

> [AZURE.NOTE] Ez az OData összekötő támogatási mindkét felhőből OData adatok és a helyszíni OData-adatforrások másolása. Az utóbbi frissítenie kell az adatkezelési átjáró telepítése. Lásd: [a helyszíni és felhőbeli között adatok áthelyezése a](data-factory-move-data-between-onprem-and-cloud.md) cikk az adatkezelési átjáró kapcsolatban további tájékoztatást.

## <a name="copy-data-wizard"></a>Az adatok varázsló másolása
A hozzon létre egy folyamat, amely az adatokat másolja az OData-forrásból legegyszerűbben a Másolás adatok varázsló használatával. Lásd: [oktatóprogram: hozzon létre egy példány varázslóval folyamat](data-factory-copy-data-wizard-tutorial.md) létrehozása egy folyamat adatok másolása varázsló segítségével a rövid ismertetését megtalálja. 

Az alábbi példák adnia minta JSON-definíciók létrehozása egy folyamat [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) segítségével vagy [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) vagy [Azure Powershellhez](data-factory-copy-activity-tutorial-using-powershell.md)használható. Adatok másolása egy OData-adatforrások az Azure Blob-tárolóhoz mutatnak. Adatok másolni lehet valamelyik, az mosdók megadott [Itt](data-factory-data-movement-activities.md#supported-data-stores) a Másolás tevékenység használata Azure Data Factory.


## <a name="sample-copy-data-from-odata-source-to-azure-blob"></a>Minta: Adatok másolja az OData-adatforrások Azure Blob

Ez a példa bemutatja az adatok másolása egy OData-adatforrások Azure Blob-tárolóhoz. Azonban adatokat másolt **közvetlenül** valamelyik, az mosdók megadott [Itt](data-factory-data-movement-activities.md#supported-data-stores) a Másolás tevékenység használata Azure Data Factory is lehet.  
 
A mintában a következő adatok gyári vállalkozások foglalja magában:

1.  [Az OData](#odata-linked-service-properties)-típus csatolt szolgáltatás.
2.  Csatolt szolgáltatás típusú [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
3.  A beviteli [adatkészlet](data-factory-create-datasets.md) [ODataResource](#odata-dataset-type-properties)típusú.
4.  Egy kimenet [adatkészlet](data-factory-create-datasets.md) [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties)típusú.
4.  A [folyamat](data-factory-create-pipelines.md) [RelationalSource](#odata-copy-activity-type-properties) és [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties)használó másolása a tevékenységhez.

A minta adatait másolja az Azure blob-OData forrás óránként lekérdezését. A minták következő szakaszok ezeket mintákban használt JSON tulajdonságok témakörben olvashat. 

**Az OData-csatolt szolgáltatás** Ebben a példában az alapszintű hitelesítés. Lásd: hitelesítés használható különböző típusú [OData csatolt szolgáltatás](#odata-linked-service-properties) szakaszát. 

    {
        "name": "ODataLinkedService",
        "properties": 
        {
            "type": "OData",
            "typeProperties": 
            {
                "url": "http://services.odata.org/OData/OData.svc",
               "authenticationType": "Anonymous"
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

**Beviteli adatkészlet OData**

"Külső" beállítást: "igaz" tájékoztatja a Data Factory szolgáltatást, hogy az adatkészlet külső adatok gyári és az adatok gyári tevékenységet nem készül.
    
    {
        "name": "ODataDataset",
        "properties": 
        {
            "type": "ODataResource",
            "typeProperties": 
            {
                "path": "Products" 
            },
            "linkedServiceName": "ODataLinkedService",
            "structure": [],
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true,
            "policy": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3               
            }
        }
    }

Az adathalmazban **elérési** útjának megadása nem kötelező. 


**Azure Blob-kimeneti adatkészlet**

Adatok egy új blob íródott óránként (gyakoriság: óra, intervallum: 1). A mappa elérési útját a blob dinamikusan kiértékeli a kezdési időpontot a szelet, amely feldolgozása alapján. A mappa elérési útja használja az év, hónap, nap és a kezdési időpontot óra részei.

    {
        "name": "AzureBlobODataDataSet",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/odata/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

A folyamat egy példány tevékenységet, amely a bemeneti és kimeneti adatkészleteket használatára van beállítva, és van ütemezve óránként tartalmazza. A során JSON megadása az **adatforrás** típusa **RelationalSource** van állítva, és **BlobSink** **gyűjtő** típusának beállítása. A **lekérdezés** tulajdonságban meghatározott SQL-lekérdezés a legfrissebb (legújabb) adatokat a OData forrásból származó kijelölése
    
    {
        "name": "CopyODataToBlob",
        "properties": {
            "description": "pipeline for copy activity",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "RelationalSource",
                            "query": "?$select=Name, Description&$top=5",
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "ODataDataSet"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobODataDataSet"
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
                    "name": "ODataToBlob"
                }
            ],
            "start": "2016-02-01T18:00:00Z",
            "end": "2016-02-03T19:00:00Z"
        }
    }


A folyamat meghatározása **lekérdezés** megadása nem kötelező. Az adatok beolvasásához használja az adatok gyári szolgáltatás **URL-címe** : a csatolt szolgáltatás (kötelező) a megadott URL-cím + a (nem kötelező) adatkészlet + lekérdezés (nem kötelező) a során megadott elérési út. 

## <a name="odata-linked-service-properties"></a>OData csatolt szolgáltatás tulajdonságai

Az alábbi táblázat ismerteti a csatolt OData szolgáltatásra JSON elemek leírását.

| A tulajdonság | Leírás | Szükséges |
| -------- | ----------- | -------- | 
| típus | Meg kell a típusa tulajdonság: **OData** | igen |
| URL-címe| Az OData-szolgáltatás URL-címe. | igen |
| authenticationType | Az OData-adatforrások eléréséhez használt hitelesítés típusa. <br/><br/> Az OData felhőben lehetséges értékei névtelen és Basic. Az OData-helyszíni lehetséges értékei névtelen, Basic és Windows. | igen | 
| felhasználónév | Adja meg a felhasználónevet, alapszintű hitelesítés használatakor. | Igen (csak ha alapszintű hitelesítés esetén) | 
| jelszó | Adja meg a felhasználói fiók felhasználónév megadott jelszót. | Igen (csak ha alapszintű hitelesítés esetén) | 
| átjárónév | Az adatok gyári szolgáltatás kell a helyszíni OData szolgáltatáshoz való kapcsolódáshoz használó az átjáró neve. Csak adja meg, ha a másolandó adatok helyszíni OData-adatforrások. | nem |

### <a name="using-basic-authentication"></a>Alapszintű hitelesítés használatával

    {
        "name": "inputLinkedService",
        "properties": 
        {
            "type": "OData",
            "typeProperties": 
            {
               "url": "http://services.odata.org/OData/OData.svc",
               "authenticationType": "Basic",
                "username": "username",
               "password": "password"
           }
       }
    }

### <a name="using-anonymous-authentication"></a>Névtelen hitelesítés használatával
    
    {
        "name": "ODataLinkedService",
        "properties": 
        {
            "type": "OData",
            "typeProperties": 
            {
               "url": "http://services.odata.org/OData/OData.svc",
               "authenticationType": "Anonymous"
           }
       }
    }

### <a name="using-windows-authentication-accessing-on-premises-odata-source"></a>Helyszíni OData-adatforrások elérése a Windows-hitelesítés használatával

    {
        "name": "inputLinkedService",
        "properties": 
        {
            "type": "OData",
            "typeProperties": 
            {
               "url": "<endpoint of on-premises OData source e.g. Dynamics CRM>",
               "authenticationType": "Windows",
                "username": "domain\\user",
               "password": "password",
               "gatewayName": "mygateway"
           }
       }
    }



## <a name="odata-dataset-type-properties"></a>OData adatkészlet tulajdonságai

Szakaszok és a rendelkezésre álló adatkészleteket definiálása tulajdonságok teljes listáját a [létrehozása adatkészleteket](data-factory-create-datasets.md) témakört is. Szakaszok, például a struktúra, elérhetőségét és egy adathalmaz JSON házirend hasonlóak az adatkészlet diagramtípusokat (Azure SQL Azure blob, Azure táblázat stb.).

A **typeProperties** szakasz eltérő adatkészlet hibatípusonként és az adatok tárolása az adatok helyének adatait. Az typeProperties szakaszban adatkészlet **ODataResource** (OData adatkészlet tartalmazó) típusú rendelkezik az alábbi tulajdonságok

| A tulajdonság | Leírás | Szükséges |
| -------- | ----------- | -------- |
| elérési út | Az OData-erőforrás elérési útja | nem | 

## <a name="odata-copy-activity-type-properties"></a>OData-példány tevékenység típusának tulajdonságai

Szakaszok és a tevékenységek definiálásával használható tulajdonságok teljes listáját a [Folyamatok létrehozása](data-factory-create-pipelines.md) témakört is. Az összes tevékenységtípusokhoz tulajdonságait, például a név, leírás, a bemeneti és kimeneti táblák és házirend érhetők el. 

Minden tevékenység típusa azonban függenek tulajdonságok elérhető typeProperties szakaszában a tevékenységet. Másolás tevékenységhez azok eltérőek attól függően, hogy milyen típusú adatforrások és mosdók.

**RelationalSource** (OData tartalmazó) típusú adatforrások esetén az alábbi tulajdonságok érhetők el typeProperties szakasz:

| A tulajdonság | Leírás | Példa | Szükséges |
| -------- | ----------- | -------------- | -------- |
| lekérdezés | Az egyéni lekérdezés használatával olvassa el az adatok. | "? $select név, leírás és $top = = 5" | nem | 

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

### <a name="type-mapping-for-odata"></a>Az OData-leképezésének

A [tevékenységekre vonatkozó adatok mozgás](data-factory-data-movement-activities.md) a cikkben említett, a Másolás tevékenység forrástípus gyűjtése a következő lépés 2 megközelítés az egyik automatikus típus konverzió hajtja végre:

1. Natív forrástípus konvertálása a .NET-típus
2. .NET-típusból származó átalakítása natív gyűjtő típusa

Ha az OData-adatok áthelyezése adatait tárolja, az OData-adattípusok .NET típusokat vannak megfeleltetve.


[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-type-repeatability-for-relational-sources](../../includes/data-factory-type-repeatability-for-relational-sources.md)]

## <a name="performance-and-tuning"></a>A teljesítmény és a finombeállítása  
Lásd: a [Másolás tevékenység teljesítmény és útmutató beállítása](data-factory-copy-activity-performance.md) Ha többet szeretne tudni a főbb tényezők hatása teljesítmény mozgásának adatok (másolása a tevékenység) az Azure Data Factory és különböző módokon optimalizálhatja azt.

