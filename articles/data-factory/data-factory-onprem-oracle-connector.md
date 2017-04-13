<properties 
    pageTitle="Adatok áthelyezése és az adatok gyári használatával Oracle |} Microsoft Azure" 
    description="Megtudhatja, hogy miként áthelyezése az Oracle-adatbázishoz, amely/származó adatokat a helyszíni Azure Data Factory használatával." 
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

# <a name="move-data-tofrom-on-premises-oracle-using-azure-data-factory"></a>Adatok áthelyezése és a helyszíni Oracle Azure Data Factory használatával 

Ez a cikk ismerteti, hogyan használhatja adatok gyári másolás tevékenység adatok áthelyezése és az Oracle származó és más adatok mentése. Ez a cikk az adatok mozgás egy általános áttekintése másolás állapotát tevékenysége és a támogatott adatokat tároló kombinációk eltéréseit [Mozgás a tevékenységekre vonatkozó adatok](data-factory-data-movement-activities.md) cikk épül.

## <a name="installation"></a>Telepítés 
Az Azure Data Factory szolgáltatás tudja a helyszíni Oracle-adatbázishoz való csatlakozáshoz telepítenie kell az alábbi összetevőket: 

- Az adatkezelési átjáró ugyanazon a gépen, amelyen az adatbázist, vagy másik számítógépre elkerülése érdekében, az adatbázis-kezelő erőforrások gyorskorcsolyázásban. Az adatkezelési átjáró a helyszíni adatforrások kapcsolódó felhőszolgáltatások biztonságos és felügyelt módon ügyfél ügynök. Lásd: [az adatok helyszíni és felhőbeli közötti áthelyezése a](data-factory-move-data-between-onprem-and-cloud.md) cikk az adatkezelési átjáró kapcsolatban további tájékoztatást. 
- Oracle-adatszolgáltató a .NET rendszerhez. [Az Oracle adatok Access Windows összetevők](http://www.oracle.com/technetwork/topics/dotnet/downloads/)összetevő szerepel. A megfelelő verziót (32 vagy 64 bites) telepíthető az állomásgép, amelyen telepítve van-e az átjáró. [Az Oracle adatokat szolgáltató .NET 12.1](http://docs.oracle.com/database/121/ODPNT/InstallSystemRequirements.htm#ODPNT149) 10 g 2-es vagy újabb verzió megjelenési érheti el az Oracle-adatbázishoz.

    Ha úgy dönt, hogy a "Telepítés XCopy", a readme.htm kövesse. Azt javasoljuk, hogy úgy dönt, hogy a telepítő a felhasználói felület (nem-XCopy egyet). 
 
    A szolgáltató telepítése után indítsa újra a számítógépen, a szolgáltatások kisalkalmazás (vagy) az adatkezelési átjáró konfigurációkezelőjének a adatok adatkezelési átjáró Host szolgáltatás.  

> [AZURE.NOTE] Lásd: a kapcsolat/átjáró hibaelhárítási tippek [problémák elhárítása átjáró](data-factory-data-management-gateway.md#troubleshoot-gateway-issues) kapcsolatos problémákat. 

## <a name="copy-data-wizard"></a>Az adatok varázsló másolása
A hozzon létre egy folyamat, amely valamelyik támogatott gyűjtő adatok tárolóban másolja az adatokat a/az Oracle-adatbázishoz legegyszerűbben a Másolás adatok varázsló használatával. Lásd: [oktatóprogram: hozzon létre egy példány varázslóval folyamat](data-factory-copy-data-wizard-tutorial.md) létrehozása egy folyamat adatok másolása varázsló segítségével a rövid ismertetését megtalálja. 

Az alábbi példa a minta JSON-definíciók létrehozása egy folyamat [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) segítségével vagy [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) vagy [Azure Powershellhez](data-factory-copy-activity-tutorial-using-powershell.md)használható biztosít. Azok az adatok másolása a vagy Oracle-adatbázishoz, és az Azure Blob-tárolóhoz módját mutatják. Adatok másolni lehet valamelyik, az mosdók megadott [Itt](data-factory-data-movement-activities.md#supported-data-stores) a Másolás tevékenység használata Azure Data Factory.   

## <a name="sample-copy-data-from-oracle-to-azure-blob"></a>Minta: Adatainak másolása az Oracle Azure Blob
Ez a példa bemutatja az Azure Blob-tárolóhoz adatok másolása a helyszíni Oracle-adatbázishoz. Azonban adatokat másolt **közvetlenül** valamelyik, az mosdók megadott [Itt](data-factory-data-movement-activities.md#supported-data-stores) a Másolás tevékenység használata Azure Data Factory is lehet.  
 
A mintában a következő adatok gyári vállalkozások foglalja magában:

1.  Csatolt szolgáltatás típusú [OnPremisesOracle](data-factory-onprem-oracle-connector.md#oracle-linked-service-properties).
2.  Csatolt szolgáltatás típusú [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
3.  A beviteli [adatkészlet](data-factory-create-datasets.md) [OracleTable](data-factory-onprem-oracle-connector.md#oracle-dataset-type-properties)típusú. 
4.  Egy kimenet [adatkészlet](data-factory-create-datasets.md) [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties)típusú.
5.  A [folyamat](data-factory-create-pipelines.md) használó [OracleSource](data-factory-onprem-oracle-connector.md#oracle-copy-activity-type-properties) forrás és [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) gyűjtő másolása a tevékenységhez.

A minta adatokat másolja a táblázat a helyszíni Oracle-adatbázishoz blob óránként. További információt a különböző tulajdonságok használja a mintában a minták követő szakaszok dokumentációjában olvasható.

**Az Oracle csatolt szolgáltatás:**

    {
      "name": "OnPremisesOracleLinkedService",
      "properties": {
        "type": "OnPremisesOracle",
        "typeProperties": {
          "ConnectionString": "data source=<data source>;User Id=<User Id>;Password=<Password>;",
          "gatewayName": "<gateway name>"
        }
      }
    }

**Azure Blob-tárolóhoz összekapcsolt szolgáltatás:**

    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<Account key>"
        }
      }
    }

**Az Oracle beviteli adatkészlet:**

A minta feltételezi létrehozott táblázat "Táblanév" Oracle, és azt az időpontot adatsor "timestampcolumn" című oszlopot tartalmaz. 

"Külső" beállítást: "igaz" tájékoztatja a Data Factory szolgáltatást, hogy az adatkészlet külső adatok gyári és az adatok gyári tevékenységet nem készül.

    {
        "name": "OracleInput",
        "properties": {
            "type": "OracleTable",
            "linkedServiceName": "OnPremisesOracleLinkedService",
            "typeProperties": {
                "tableName": "MyTable"
            },
               "external": true,
            "availability": {
                "offset": "01:00:00",
                "interval": "1",
                "anchorDateTime": "2014-02-27T12:00:00",
                "frequency": "Hour"
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

Adatok egy új blob íródott óránként (gyakoriság: óra, intervallum: 1). A mappa elérési útját és nevét a blob-dinamikusan értékeli ki a kezdési időpontot a szelet, amely feldolgozása alapján. A mappa elérési útja használja az év, hónap, nap és a kezdési időpontot óra részei.
    
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


**Csővezeték másolása a tevékenységhez:**

A folyamat egy példány tevékenységet, amely a bemeneti és kimeneti adatkészleteket használatára van beállítva, és van ütemezve óránként tartalmazza. A során JSON megadása az **adatforrás** típusa **OracleSource** van állítva, és **BlobSink** **gyűjtő** típusának beállítása.  Az SQL-lekérdezés **oracleReaderQuery** tulajdonság megadott az adatok másolása az elmúlt órában kijelölése

    
    {  
        "name":"SamplePipeline",
        "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline for copy activity",
        "activities":[  
          {
            "name": "OracletoBlob",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [
              {
                "name": " OracleInput"
              }
            ],
            "outputs": [
              {
                "name": "AzureBlobOutput"
              }
            ],
            "typeProperties": {
              "source": {
                "type": "OracleSource",
                "oracleReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
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


Módosítania kell a lekérdezési karakterlánc az Oracle-adatbázis beállításaitól dátum alapján. Ha az alábbi hibaüzenet jelenik meg: 

    Message=Operation failed in Oracle Database with the following error: 'ORA-01861: literal does not match format string'.,Source=,''Type=Oracle.DataAccess.Client.OracleException,Message=ORA-01861: literal does not match format string,Source=Oracle Data Provider for .NET,'.

Előfordulhat, hogy a lekérdezés módosításához, ahogy az alábbi példa (a függvénnyel to_date):

    "oracleReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= to_date(\\'{0:MM-dd-yyyy HH:mm}\\',\\'MM/DD/YYYY HH24:MI\\')  AND timestampcolumn < to_date(\\'{1:MM-dd-yyyy HH:mm}\\',\\'MM/DD/YYYY HH24:MI\\') ', WindowStart, WindowEnd)"

## <a name="sample-copy-data-from-azure-blob-to-oracle"></a>Minta: Adatok másolása Azure Blob Oracle
Ez a példa bemutatja, hogyan Azure Blob-tárolóhoz adatainak másolása a helyszíni Oracle-adatbázishoz. Azonban adatokat másolt **közvetlenül** bármilyen a források megadott [Itt](data-factory-data-movement-activities.md#supported-data-stores) a Másolás tevékenység használata Azure Data Factory is lehet.  
 
A mintában a következő adatok gyári vállalkozások foglalja magában:

1.  Csatolt szolgáltatás típusú [OnPremisesOracle](data-factory-onprem-oracle-connector.md#oracle-linked-service-properties).
2.  Csatolt szolgáltatás típusú [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
3.  A beviteli [adatkészlet](data-factory-create-datasets.md) [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties)típusú.
4.  Egy kimenet [adatkészlet](data-factory-create-datasets.md) [OracleTable](data-factory-onprem-oracle-connector.md#oracle-dataset-type-properties)típusú. 
5.  A [folyamat](data-factory-create-pipelines.md) [BlobSource](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) , [OracleSink](data-factory-onprem-oracle-connector.md#oracle-copy-activity-type-properties) gyűjtő mint adatforrás használó másolása a tevékenységhez.

A minta adatait másolja blob-helyszíni Oracle-adatbázis táblájához óránként. További információt a különböző tulajdonságok használja a mintában a minták követő szakaszok dokumentációjában olvasható.

**Az Oracle csatolt szolgáltatás:**

    {
      "name": "OnPremisesOracleLinkedService",
      "properties": {
        "type": "OnPremisesOracle",
        "typeProperties": {
          "ConnectionString": "data source=<data source>;User Id=<User Id>;Password=<Password>;",
          "gatewayName": "<gateway name>"
        }
      }
    }

**Azure Blob-tárolóhoz összekapcsolt szolgáltatás:**

    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<Account key>"
        }
      }
    }

**Azure Blob-beviteli adatkészlet**

Adatok van kiválasztott az új blob óránként (gyakoriság: óra, intervallum: 1). A mappa elérési útját és nevét a blob-dinamikusan értékeli ki a kezdési időpontot a szelet, amely feldolgozása alapján. A mappa elérési útja használ év, hónap és nap része a kezdési időpontot, és a fájlnevet használja a kezdési időpontot a óra részét. "külső": "igaz" beállítás tájékoztatja a Data Factory szolgáltatás az alábbi táblázat az adatok gyári mutató külső, és nem készül az adatok gyári tevékenységet.

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

**Az Oracle kimeneti adatkészlet:**

A példa feltételezi, hogy az Oracle létrehozott táblázat "Táblanév". A táblázat létrehozása a ugyanannyi oszlopot az Oracle megfelelően működnek a Blob CSV-fájl, amely tartalmazhat. Új sorok kerülnek be a táblába óránként.

    {
        "name": "OracleOutput",
        "properties": {
            "type": "OracleTable",
            "linkedServiceName": "OnPremisesOracleLinkedService",
            "typeProperties": {
                "tableName": "MyTable"
            },
            "availability": {
                "frequency": "Hour",
                "interval": "1"
            }
        }
    }


**Csővezeték másolása a tevékenységhez:**

A folyamat egy példány tevékenységet, amely a bemeneti és kimeneti adatkészleteket használatára van beállítva, és van ütemezve óránként tartalmazza. A során JSON megadása az **adatforrás** típusa **BlobSource** van állítva, és a **gyűjtő** típusának beállítása **OracleSink**.  

    
    {  
        "name":"SamplePipeline",
        "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline with copy activity",
        "activities":[  
          {
            "name": "AzureBlobtoOracle",
            "description": "Copy Activity",
            "type": "Copy",
            "inputs": [
              {
                "name": "AzureBlobInput"
              }
            ],
            "outputs": [
              {
                "name": "OracleOutput"
              }
            ],
            "typeProperties": {
              "source": {
                "type": "BlobSource"
              },
              "sink": {
                "type": "OracleSink"
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


## <a name="oracle-linked-service-properties"></a>Az Oracle csatolt szolgáltatás tulajdonságai

Az alábbi táblázat ismerteti a csatolt Oracle szolgáltatásra JSON elemek leírását. 

A tulajdonság | Leírás | Szükséges
-------- | ----------- | --------
típus | Meg kell a típusa tulajdonság: **OnPremisesOracle** | igen
connectionString | Adja meg az Oracle-adatbázis példányához connectionString tulajdonság szükséges információkat. | igen 
átjárónév | Az átjáró neve, amely a helyszíni Oracle-kiszolgálóhoz való csatlakozáshoz használt | igen

Olvassa el [a beállítás hitelesítő adatok és](data-factory-move-data-between-onprem-and-cloud.md#set-credentials-and-security) biztonsági egy helyszíni Oracle-adatforrás hitelesítő adatok beállításával kapcsolatban további tájékoztatást.
## <a name="oracle-dataset-type-properties"></a>Az Oracle adatkészlet tulajdonságai

Szakaszok és a rendelkezésre álló adatkészleteket definiálása tulajdonságok teljes listáját a [létrehozása adatkészleteket](data-factory-create-datasets.md) témakört is. Szakaszok, például a struktúra, elérhetőségét és egy adathalmaz JSON házirend hasonlóak az adatkészlet diagramtípusokat (Oracle, Azure blob, Azure táblázat stb.).
 
A typeProperties szakasz eltérő adatkészlet hibatípusonként és az adatok tárolása az adatok helyének adatait. A következő tulajdonságokat az typeProperties szakaszban az adatkészlet OracleTable típusú foglalja magában:

A tulajdonság | Leírás | Szükséges
-------- | ----------- | --------
Táblanév | Az Oracle-adatbázishoz, amely a csatolt szolgáltatás hivatkozik a tábla neve. | Nem (Ha **oracleReaderQuery** **OracleSource** , nincs megadva)

## <a name="oracle-copy-activity-type-properties"></a>Az Oracle a tevékenység típusának Tulajdonságok másolása

Szakaszok és a tevékenységek definiálásával használható tulajdonságok teljes listáját a [Folyamatok létrehozása](data-factory-create-pipelines.md) témakört is. Az összes tevékenységtípusokhoz tulajdonságait, például a név, leírás, a bemeneti és kimeneti táblák és házirend érhetők el. 

> [AZURE.NOTE]
A Másolás tevékenység csak egy beviteli tart, és csak egy kimenet eredményez.

Minden tevékenység típusa azonban függenek tulajdonságok elérhető typeProperties szakaszában a tevékenységet. Másolás tevékenységhez azok eltérőek attól függően, hogy milyen típusú adatforrások és mosdók.

### <a name="oraclesource"></a>OracleSource
Másolás tevékenységet Ha a forrás **OracleSource** típusú a következő tulajdonságokat érhetők el **typeProperties** szakasz:

A tulajdonság | Leírás |Megengedett érték | Szükséges
-------- | ----------- | ------------- | --------
oracleReaderQuery | Az egyéni lekérdezés használatával olvassa el az adatok. | SQL-lekérdezés karakterlánc. Példa: válassza a *from tábla <br/> <br/>, ha nincs megadva, az SQL-utasítás végrehajtott: válassza a* from tábla | Nem (Ha a **táblanév** **adatkészletből** nincs megadva)

### <a name="oraclesink"></a>OracleSink
**OracleSink** támogatja a következő tulajdonságokat:

A tulajdonság | Leírás | Megengedett érték | Szükséges
-------- | ----------- | -------------- | --------
writeBatchTimeout | Várja meg, hogy a köteg beillesztési művelet elvégzéséhez előtt időtúllépés történik meg időt. | időszak<br/><br/> Példa: 00:30:00 (30 perc). | nem
writeBatchSize | Adatok beillesztése az SQL-táblázat writeBatchSize elérésekor pufferelési méretét.   | Egész szám (sorok száma)| Nem (alapértelmezett: 10000)  
sqlWriterCleanupScript | Adja meg a lekérdezés másolás tevékenység végrehajtani, hogy az adatok, egy adott szeletet törlődnek. | Egy lekérdezés utasítást. | nem
sliceIdentifierColumnName | Adja meg a Másolás tevékenység automatikusan létrejön szeletet azonosítójú, amellyel a egy adott szeletet esetén futtassa újra az adatok karbantartása kitöltéséhez oszlop nevét. | Egy oszlop adattípusát binary(32) oszlop neve. | nem


[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

### <a name="type-mapping-for-oracle"></a>Az Oracle leképezésének

Az említett a a [tevékenységekre vonatkozó adatok mozgását](data-factory-data-movement-activities.md) cikk másolás tevékenység forrástípus gyűjtése a következő lépés 2 megközelítés az egyik automatikus típus konverzió hajtja végre:

1. Natív forrástípus konvertálása a .NET-típus
2. .NET-típusból származó átalakítása natív gyűjtő típusa

Adatok az Oracle mozgatásakor következő megfeleltetések használt adattípusról az Oracle .NET típusát, és ez fordítva is igaz.

Az Oracle-adattípus | .NET-keretrendszer adattípus
---------------- | ------------------------
BFÁJL | Byte]
BLOB | Byte]
KARAKTER | Karakterlánc
CLOB | Karakterlánc
DÁTUM | Dátum és idő
LEBEGTETÉS | Decimális
EGÉSZ SZÁM | Decimális
INTERVALLUM ÉV HÓNAPRA. | Int32
MÁSODIK INTERVALLUM NAP | Időszak
HOSSZÚ | Karakterlánc
HOSSZÚ NYERS | Byte]
NCHAR | Karakterlánc
NCLOB | Karakterlánc
SZÁM | Decimális
NVARCHAR2 | Karakterlánc
NYERS | Byte]
ROWID | Karakterlánc
IDŐBÉLYEG | Dátum és idő
AZ IDŐZÓNA IDŐBÉLYEG | Dátum és idő
AZ IDŐZÓNA IDŐBÉLYEG | Dátum és idő
ALÁ NEM ÍRT EGÉSZ SZÁM | Szám
VARCHAR2 | Karakterlánc
XML | Karakterlánc

## <a name="troubleshooting-tips"></a>Hibaelhárítási tippek

**Problémát:** A következő **hibaüzenet**jelenik meg: másolás tevékenység éri el a Paraméterek: "UnknownParameterName" részletes üzenet: nem található meg a kért .net keretrendszerbeli adatszolgáltató. Valószínűleg nincs telepítve".  

**Lehetséges okai:**

1. A .NET keretrendszer adatszolgáltatója az Oracle nincs telepítve.
2. A .NET keretrendszer adatszolgáltatója az Oracle telepítve van a .NET-keretrendszer 2.0, és nem szerepel a .NET-keretrendszer 4.0 mappákat. 

**Megoldás/kerülő megoldás:**

1. Ha még nem telepítette a .NET-szolgáltató az Oracle, [telepítse](http://www.oracle.com/technetwork/topics/dotnet/downloads/) újra az alkalmazási példát. 
2. Ha a szolgáltató telepítése után is megjelenik a hibaüzenet, hajtsa végre az alábbi lépéseket: 
    1. Nyissa meg a gép config .NET 2.0 a mappából: <system disk>: \Windows\Microsoft.NET\Framework64\v2.0.50727\CONFIG\machine.config.
    2. Keresés az **Oracle-adatszolgáltató a .NET rendszerhez**, és tudja megkeresni egy bejegyzést, ahogy az alábbi példa unwn az alábbiakat kell minta under **system.data** -> **DbProviderFactories**:
            “<add name="Oracle Data Provider for .NET" invariant="Oracle.DataAccess.Client" description="Oracle Data Provider for .NET" type="Oracle.DataAccess.Client.OracleClientFactory, Oracle.DataAccess, Version=2.112.3.0, Culture=neutral, PublicKeyToken=89b483f429c47342" />”
2.  Ez a bejegyzés másolása a machine.config fájlhoz, a következő 4.0 mappában: <system disk>: \Windows\Microsoft.NET\Framework64\v4.0.30319\Config\machine.config, és miként módosíthatja a 4.xxx.x.x verziót.
3.  Telepítse a "< ODP.NET telepített elérési út > \11.2.0\client_1\odp.net\bin\4\Oracle.DataAccess.dll" (GAC) globális összeállítási gyorsítótárba futtatásával `gacutil /i [provider path]`.



[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]


## <a name="performance-and-tuning"></a>A teljesítmény és a finombeállítása  
Lásd: a [Másolás tevékenység teljesítmény és útmutató beállítása](data-factory-copy-activity-performance.md) Ha többet szeretne tudni a főbb tényezők hatása teljesítmény mozgásának adatok (másolása a tevékenység) az Azure Data Factory és különböző módokon optimalizálhatja azt.
