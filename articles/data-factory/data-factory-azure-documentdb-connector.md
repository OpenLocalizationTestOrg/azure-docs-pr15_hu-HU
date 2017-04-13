<properties 
    pageTitle="Adatok áthelyezése és DocumentDB |} Microsoft Azure" 
    description="Megtudhatja, hogyan adatok áthelyezése és Azure DocumentDB gyűjteményből Azure Data Factory használatával" 
    services="data-factory, documentdb" 
    documentationCenter="" 
    authors="linda33wj" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="multiple" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016" 
    ms.author="jingwang"/>

# <a name="move-data-to-and-from-documentdb-using-azure-data-factory"></a>Adatok áthelyezése és az Azure Data Factory használatával DocumentDB

Ez a cikk azt ismerteti, hogyan használhatja a Másolás tevékenység-Azure adatok gyári a áthelyezése egy másik adatokból Azure DocumentDB adatok tárolására és DocumentDB adatok áthelyezése egy másik adattár. Ez a cikk az adatok mozgás egy általános áttekintése másolás állapotát tevékenysége és a támogatott adatokat tároló kombinációk eltéréseit [Mozgás a tevékenységekre vonatkozó adatok](data-factory-data-movement-activities.md) cikk épül.

Az alábbi példák bemutatják, hogyan másolhatja az adatokat, és az Azure DocumentDB és Azure Blob-tárolóhoz. Azonban adatokat másolt **közvetlenül** bármilyen forrásból valamelyik, az mosdók megadott [Itt](data-factory-data-movement-activities.md#supported-data-stores) a Másolás tevékenység használata Azure Data Factory is lehet.  

> [AZURE.NOTE] A-telepítésű/Azure IaaS adatok adatainak másolása az Azure DocumentDB tárolja, és ez fordítva is igaz támogatottak az adatkezelési átjáró 2.1-es verziójával és a fenti.

## <a name="sample-copy-data-from-documentdb-to-azure-blob"></a>Minta: Adatok másolása DocumentDB Azure Blob

Az alábbi példa látható:

1. Csatolt szolgáltatás típusú [DocumentDb](#azure-documentdb-linked-service-properties).
2. Csatolt szolgáltatás típusú [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties). 
3. A beviteli [adatkészlet](data-factory-create-datasets.md) [DocumentDbCollection](#azure-documentdb-dataset-type-properties)típusú. 
4. Egy kimenet [adatkészlet](data-factory-create-datasets.md) [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties)típusú.
4. A [folyamat](data-factory-create-pipelines.md) [DocumentDbCollectionSource](#azure-documentdb-copy-activity-type-properties) és [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties)használó másolása a tevékenységhez.

A minta Azure DocumentDB adatokat Azure Blob másolja. A minták következő szakaszok ezeket mintákban használt JSON tulajdonságok témakörben olvashat.

**Azure csatolt DocumentDB szolgáltatás:**

    {
      "name": "DocumentDbLinkedService",
      "properties": {
        "type": "DocumentDb",
        "typeProperties": {
          "connectionString": "AccountEndpoint=<EndpointUrl>;AccountKey=<AccessKey>;Database=<Database>"
        }
      }
    }

**Azure Blob-tárolóhoz összekapcsolt szolgáltatás:**

    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

**Azure dokumentum DB beviteli adatkészlet:**

A példa feltételezi, hogy van-e egy webhelycsoport **személy** nevű egy Azure DocumentDB-adatbázis.
 
"Külső" beállítást: "true", és adja meg a externalData házirend adatai az Azure Data Factory szolgáltatás a táblázatot az adatok gyári mutató külső, és nem készített adatok gyári tevékenységet.

    {
      "name": "PersonDocumentDbTable",
      "properties": {
        "type": "DocumentDbCollection",
        "linkedServiceName": "DocumentDbLinkedService",
        "typeProperties": {
          "collectionName": "Person"
        },
        "external": true,
        "availability": {
          "frequency": "Day",
          "interval": 1
        }
      }
    }


**Azure Blob kimeneti adatkészlet:**

Adatok egy új blob az adott datetime tükröző óra granularitással blob útvonala óránként másolja.

    {
      "name": "PersonBlobTableOut",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "docdb",
          "format": {
            "type": "TextFormat",
            "columnDelimiter": ",",
            "nullValue": "NULL"
          }
        },
        "availability": {
          "frequency": "Day",
          "interval": 1
        }
      }
    }

Minta JSON dokumentum a személy gyűjteményben DocumentDB adatbázisban: 

    {
      "PersonId": 2,
      "Name": {
        "First": "Jane",
        "Middle": "",
        "Last": "Doe"
      }
    }

DocumentDB lekérdezésekor dokumentumok felett hierarchikus JSON-dokumentumok például szintaxisát SQL használatával támogatja. 

Példa: MiddleName, Person.Name.Last AS Utónév Person.Name.Middle Person.PersonId, Person.Name.First AS Utónév, jelölje be a személy

A következő folyamat adatokat másolja az Azure blob az DocumentDB adatbázis személy gyűjteményből. Kimeneti és a Másolás tevékenységet a bemeneti részét képező adatkészleteket megadva.  
    
    {
      "name": "DocDbToBlobPipeline",
      "properties": {
        "activities": [
          {
            "type": "Copy",
            "typeProperties": {
              "source": {
                "type": "DocumentDbCollectionSource",
                "query": "SELECT Person.Id, Person.Name.First AS FirstName, Person.Name.Middle as MiddleName, Person.Name.Last AS LastName FROM Person",
                "nestingSeparator": "."
              },
              "sink": {
                "type": "BlobSink",
                "blobWriterAddHeader": true,
                "writeBatchSize": 1000,
                "writeBatchTimeout": "00:00:59"
              }
            },
            "inputs": [
              {
                "name": "PersonDocumentDbTable"
              }
            ],
            "outputs": [
              {
                "name": "PersonBlobTableOut"
              }
            ],
            "policy": {
              "concurrency": 1
            },
            "name": "CopyFromDocDbToBlob"
          }
        ],
        "start": "2015-04-01T00:00:00Z",
        "end": "2015-04-02T00:00:00Z"
      }
    }

## <a name="sample-copy-data-from-azure-blob-to-azure-documentdb"></a>Minta: Adatok másolása Azure Blob Azure DocumentDB

Az alábbi példa látható:

1. Csatolt szolgáltatás típusú [DocumentDb](#azure-documentdb-linked-service-properties).
2. Csatolt szolgáltatás típusú [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
3. A beviteli [adatkészlet](data-factory-create-datasets.md) [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties)típusú.
4. Egy kimenet [adatkészlet](data-factory-create-datasets.md) [DocumentDbCollection](#azure-documentdb-dataset-type-properties)típusú. 
4. A [folyamat](data-factory-create-pipelines.md) [BlobSource](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) és [DocumentDbCollectionSink](#azure-documentdb-copy-activity-type-properties)használó másolása a tevékenységhez.


A minta adatokat másolja az Azure blob az Azure DocumentDB. A minták következő szakaszok ezeket mintákban használt JSON tulajdonságok témakörben olvashat.

**Azure Blob-tárolóhoz összekapcsolt szolgáltatás:**
    
    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

**Azure csatolt DocumentDB szolgáltatás:**
    
    {
      "name": "DocumentDbLinkedService",
      "properties": {
        "type": "DocumentDb",
        "typeProperties": {
          "connectionString": "AccountEndpoint=<EndpointUrl>;AccountKey=<AccessKey>;Database=<Database>"
        }
      }
    }

**Azure Blob beviteli adatkészlet:**

    {
      "name": "PersonBlobTableIn",
      "properties": {
        "structure": [
          {
            "name": "Id",
            "type": "Int"
          },
          {
            "name": "FirstName",
            "type": "String"
          },
          {
            "name": "MiddleName",
            "type": "String"
          },
          {
            "name": "LastName",
            "type": "String"
          }
        ],
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "fileName": "input.csv",
          "folderPath": "docdb",
          "format": {
            "type": "TextFormat",
            "columnDelimiter": ",",
            "nullValue": "NULL"
          }
        },
        "external": true,
        "availability": {
          "frequency": "Day",
          "interval": 1
        }
      }
    }

**Azure DocumentDB kimeneti adatkészlet:**

A minta "Személy" nevű gyűjtemény másolja az adatokat.

    {
      "name": "PersonDocumentDbTableOut",
      "properties": {
        "structure": [
          {
            "name": "Id",
            "type": "Int"
          },
          {
            "name": "Name.First",
            "type": "String"
          },
          {
            "name": "Name.Middle",
            "type": "String"
          },
          {
            "name": "Name.Last",
            "type": "String"
          }
        ],
        "type": "DocumentDbCollection",
        "linkedServiceName": "DocumentDbLinkedService",
        "typeProperties": {
          "collectionName": "Person"
        },
        "availability": {
          "frequency": "Day",
          "interval": 1
        }
      }
    }

A következő folyamat Azure Blob lévő adatokat a DocumentDB az a személy gyűjteménybe másolja. Kimeneti és a Másolás tevékenységet a bemeneti részét képező adatkészleteket megadva. 
    
    {
      "name": "BlobToDocDbPipeline",
      "properties": {
        "activities": [
          {
            "type": "Copy",
            "typeProperties": {
              "source": {
                "type": "BlobSource"
              },
              "sink": {
                "type": "DocumentDbCollectionSink",
                "nestingSeparator": ".",
                "writeBatchSize": 2,
                "writeBatchTimeout": "00:00:00"
              }
              "translator": {
                  "type": "TabularTranslator",
                  "ColumnMappings": "FirstName: Name.First, MiddleName: Name.Middle, LastName: Name.Last, BusinessEntityID: BusinessEntityID, PersonType: PersonType, NameStyle: NameStyle, Title: Title, Suffix: Suffix, EmailPromotion: EmailPromotion, rowguid: rowguid, ModifiedDate: ModifiedDate"
              }
            },
            "inputs": [
              {
                "name": "PersonBlobTableIn"
              }
            ],
            "outputs": [
              {
                "name": "PersonDocumentDbTableOut"
              }
            ],
            "policy": {
              "concurrency": 1
            },
            "name": "CopyFromBlobToDocDb"
          }
        ],
        "start": "2015-04-14T00:00:00Z",
        "end": "2015-04-15T00:00:00Z"
      }
    }
 
Ha a minta blob bemeneti szerint 

    1,John,,Doe

Kattintson a kimenet DocumentDB a JSON lesz megfelelően:

    {
      "Id": 1,
      "Name": {
        "First": "John",
        "Middle": null,
        "Last": "Doe"
      },
      "id": "a5e8595c-62ec-4554-a118-3940f4ff70b6"
    }
    
DocumentDB NoSQL áruházból JSON dokumentumok, ahol a beágyazott szerkezetek engedélyezettek. Azure Data Factory lehetővé teszi, hogy a felhasználói hierarchia keresztül **nestingSeparator**, amely jelölésére "." Ebben a példában. Az elválasztó karaktert, a Másolás tevékenység hoz létre a "Név" objektum három gyermekek elemekkel először középső név kezdőbetűjét és vezetéknevet, "Name.First", "Name.Middle" és "Name.Last" a táblázat definícióban megfelelően.

## <a name="azure-documentdb-linked-service-properties"></a>Azure DocumentDB csatolt szolgáltatás tulajdonságai

Az alábbi táblázat ismerteti a csatolt Azure DocumentDB szolgáltatásra JSON elemek leírását. 

| **A tulajdonság** | **Leírás** | **Szükséges** |
| -------- | ----------- | --------- |
| típus | Meg kell a típusa tulajdonság: **DocumentDb** | igen |
| connectionString | Adja meg az Azure DocumentDB adatbázishoz való csatlakozáshoz szükséges adatokat. | igen |

## <a name="azure-documentdb-dataset-type-properties"></a>Azure DocumentDB adatkészlet tulajdonságai

Teljes listáját a szakaszok és a rendelkezésre álló adatkészleteket definiálása tulajdonságok hivatkozzon [létrehozása adatkészleteket](data-factory-create-datasets.md) olvashatók. Szakaszok, például a struktúra, elérhetőségét és egy adatkészletből JSON házirend hasonlóak az adatkészlet diagramtípusokat (Azure SQL Azure blob, Azure táblázat stb.).
 
A typeProperties szakasz eltérő adatkészlet hibatípusonként és az adatok tárolása az adatok helyének adatait. Az typeProperties szakaszban az adatkészlet típusú **DocumentDbCollection** tartalmaz, a következő tulajdonságokat.

| **A tulajdonság** | **Leírás** | **Szükséges** |
| -------- | ----------- | -------- |
| Lekérdezés_neve | A DocumentDB dokumentum a webhelycsoport neve. | igen |


Példa:

    {
      "name": "PersonDocumentDbTable",
      "properties": {
        "type": "DocumentDbCollection",
        "linkedServiceName": "DocumentDbLinkedService",
        "typeProperties": {
          "collectionName": "Person"
        },
        "external": true,
        "availability": {
          "frequency": "Day",
          "interval": 1
        }
      }
    }

### <a name="schema-by-data-factory"></a>Adatok gyári által séma
Az adatok séma ingyenes tárolja, például DocumentDB az adatok gyári szolgáltatás a séma infers az alábbi módon:  

1.  Adatok struktúrájára adatkészlet meghatározása a **struktúra** tulajdonság használatával adja meg, ha az adatok gyári szolgáltatás, a séma végzett betartja a struktúra. Ebben az esetben ha a sor egy értéket, ha az oszlop nem tartalmaz, a null érték nyújtanak.
2.  Ha nem adja meg az adatok struktúrájára adatkészlet meghatározása **struktúra** tulajdonságával, az adatok gyári szolgáltatás infers a séma az adatok az első sor használatával. Ebben az esetben ha az első sor nem tartalmaz a teljes séma, oszloppal lesz hiányzik a másolási művelet eredményét.

Ezért séma ingyenes adatforrások esetén a legjobb módszer, hogy adja meg a **struktúra** tulajdonságával adatok struktúrájára.

## <a name="azure-documentdb-copy-activity-type-properties"></a>Azure DocumentDB másolás tevékenység típusának tulajdonságai

Teljes listáját a szakaszok és a tevékenységek definiálásával használható tulajdonságok olvassa el a [Folyamatok létrehozása](data-factory-create-pipelines.md) cikk. Az összes tevékenységtípusokhoz tulajdonságait, például a név, leírás, a bemeneti és kimeneti táblák és házirend érhetők el.
 
**Megjegyzés:** A Másolás tevékenység csak egy beviteli tart, és csak egy kimenet eredményez.

TypeProperties szakaszában a tevékenységet a rendelkezésre álló tulajdonságok azonban változnak minden tevékenység típusát, és attól függően, hogy milyen típusú adatforrások és mosdók változik másolás tevékenység esetén.

Abban az esetben, ha másolás tevékenység **DocumentDbCollectionSource** típusú adatforrások esetén az alábbi tulajdonságok érhetők el **typeProperties** szakasz:

| **A tulajdonság** | **Leírás** | **Megengedett érték** | **Szükséges** |
| ------------ | --------------- | ------------------ | ------------ |
| lekérdezés | Adja meg a lekérdezést, hogy olvassa el az adatok. | A lekérdezési karakterlánc DocumentDB által támogatott. <br/><br/>Példa:`SELECT c.BusinessEntityID, c.PersonType, c.NameStyle, c.Title, c.Name.First AS FirstName, c.Name.Last AS LastName, c.Suffix, c.EmailPromotion FROM c WHERE c.ModifiedDate > \"2009-01-01T00:00:00\"` | nem <br/><br/>Ha nincs megadva, az SQL-utasítás végrehajtott:`select <columns defined in structure> from mycollection` 
| nestingSeparator | Speciális karakter jelzi, hogy a dokumentum van beágyazva | Tetszőleges karakter. <br/><br/>DocumentDB NoSQL áruházból JSON dokumentumok, ahol a beágyazott szerkezetek engedélyezettek. Azure Data Factory lehetővé teszi, hogy a felhasználói hierarchia keresztül nestingSeparator, amely jelölésére "." a fenti példákban. Az elválasztó karaktert, a Másolás tevékenység hoz létre a "Név" objektum három gyermekek elemekkel először középső név kezdőbetűjét és vezetéknevet, "Name.First", "Name.Middle" és "Name.Last" a táblázat definícióban megfelelően. | nem

**DocumentDbCollectionSink** támogatja a következő tulajdonságokat:

| **A tulajdonság** | **Leírás** | **Megengedett érték** | **Szükséges** |
| -------- | ----------- | -------------- | -------- |
| nestingSeparator | A forrásoszlop nevének jelzi, hogy egymásba ágyazott dokumentumokban lévő speciális karakter van szükség. <br/><br/>Példa a fenti: `Name.First` kimeneti táblázatot hoz létre a következő JSON-struktúra a DocumentDB dokumentumban:<br/><br/>"Név": {<br/>  "First": "Kovács"<br/>}, | Az karakter, amely elválaszthatja egymástól a beágyazási szintek száma.<br/><br/>Alapértelmezett értéke `.` (pont). | Az karakter, amely elválaszthatja egymástól a beágyazási szintek száma. <br/><br/>Alapértelmezett értéke `.` (pont). | nem | 
| writeBatchSize | A párhuzamos kérések DocumentDB szolgáltatás a dokumentumok létrehozására száma.<br/><br/>Ez a tulajdonság segítségével DocumentDB/származó adatok másolásakor, finomhangolására teljesítményét. Számíthat a jobb teljesítmény writeBatchSize növelése, mivel több párhuzamos DocumentDB való összehívásokat. Azonban kell elkerülése szabályozásának, amely is throw hibaüzenet: "A ráta nagy kérése".<br/><br/>Szabályozásának számos tényező, többek között a dokumentumok, a dokumentumok kifejezések száma méretét, cél webhelycsoport stb szabályzatának indexelés határozza meg. Másolás műveletekhez használhatja jobb gyűjtemény (pl. S3), hogy az elérhető a legtöbb átviteli (2500 kérés erőforrás-mennyiség/második). | Egész szám | Nem (alapértelmezett: 10000) |
| writeBatchTimeout | Várja meg, hogy a művelet végrehajtása előtt időtúllépés történik meg időt. | időszak<br/><br/> Példa: "00: 30:00" (30 perc). | nem |
 
## <a name="appendix"></a>Függelék
1. **Kérdés:**  
    jelent, a Másolás támogatási tevékenységfrissítés a meglévő rekordok?

    **Válasz:**  
    nem értékre.

2. **Kérdés:**  
    Honnan tudja már DocumentDB kezelésére másolatának próbálkozásra másolt rekordok?

    **Válasz:**  
    rekord tartalmaz egy "Azonosító" mezőt, és a Másolás megpróbál ugyanilyen azonosítójú rekordot szeretne beszúrni, ha a másolási művelet adatérvényesítés során hibát jelez.  
 
3. **Kérdés:** 
    Data Factory támogatja a [tartomány vagy a szétválasztás kivonat-alapú adatok](https://azure.microsoft.com/documentation/articles/documentdb-partition-data/)? 

    **Válasz:** 
    nem értékre. 
4. **Kérdés:** 
    megadhatok egynél több DocumentDB webhelycsoport tábla?
    
    **Válasz:** 
    nem értékre. A webhelycsoport csak egy adott időben adható meg.
     
## <a name="performance-and-tuning"></a>A teljesítmény és a finombeállítása  
Lásd: a [Másolás tevékenység teljesítmény és útmutató beállítása](data-factory-copy-activity-performance.md) Ha többet szeretne tudni a főbb tényezők hatása teljesítmény mozgásának adatok (másolása a tevékenység) az Azure Data Factory és a különféle módokon optimalizálása azt.
