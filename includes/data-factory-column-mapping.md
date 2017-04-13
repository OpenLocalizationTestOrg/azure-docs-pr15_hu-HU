## <a name="column-mapping-with-translator-rules"></a>Oszlopok megfeleltetése Minifordító szabályokkal
Adja meg, hogyan oszlopok "felépítésének" adatforrás táblázat térkép oszlopok gyűjtő táblázat "elfoglalt" megadott megadott oszlopok megfeleltetése használható. A Másolás tevékenység **typeProperties** szakasza a **oszlopleképezést** tulajdonság érhető el.

Oszlopok megfeleltetése támogatja az alábbi esetekben:

- A forrástáblához "szerkezet" az összes oszlop összes oszlop "szerkezet" gyűjtő táblázatban vannak megfeleltetve.
- Egy részét a forrástáblához "struktúra" oszlopait "struktúra" gyűjtő táblázatban az összes oszlop vannak megfeleltetve.

Az alábbi hiba feltételeket és a kivétel eredményezi:

- Kevesebb oszlopok vagy a "szerkezetében" gyűjtő táblázat mint a hozzárendelés megadott további hasábok.
- Az ismétlődő hozzárendelési.
- SQL-lekérdezés eredménye nem rendelkezik a hozzárendelés megadott az oszlop nevét.

## <a name="column-mapping-samples"></a>Oszlop leképezés minták
> [AZURE.NOTE] Az alábbi példák az Azure SQL- és Azure Blob, de minden adattár, amely támogatja a téglalap alakú adatkészleteket alkalmazandó. Be kell adatkészlet és a csatolt szolgáltatás definíciók az alábbi példákban módosítása, hogy a megfelelő adatforrással mutassanak. 

### <a name="sample-1--column-mapping-from-azure-sql-to-azure-blob"></a>Oszlop megfeleltetése Azure SQL Azure blob 1 – minta
Ebben a példában a beviteli tábla szerkezete és SQL Azure SQL-adatbázisban táblázat mutat.

    {
        "name": "AzureSQLInput",
        "properties": {
            "structure": 
             [
               { "name": "userid"},
               { "name": "name"},
               { "name": "group"}
             ],
            "type": "AzureSqlTable",
            "linkedServiceName": "AzureSqlLinkedService",
            "typeProperties": {
                "tableName": "MyTable"
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

Ez a példa a kimeneti táblázatok szerkezete?, és az Azure blob-tárolóhoz blob mutat.

    {
        "name": "AzureBlobOutput",
        "properties":
        {
             "structure": 
              [
                    { "name": "myuserid"},
                    { "name": "myname" },
                    { "name": "mygroup"}
              ],
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/myfolder",
                "fileName":"myfile.csv",
                "format":
                {
                    "type": "TextFormat",
                    "columnDelimiter": ","
                }
            },
            "availability":
            {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }

A JSON a tevékenység alatt jelenik meg. Az oszlopok **Minifordító** tulajdonság segítségével oszlopok gyűjtő (**columnMappings**) a megfeleltetni forrásból származó.

    {
        "name": "CopyActivity",
        "description": "description", 
        "type": "Copy",
        "inputs":  [ { "name": "AzureSQLInput"  } ],
        "outputs":  [ { "name": "AzureBlobOutput" } ],
        "typeProperties":    {
            "source":
            {
                "type": "SqlSource"
            },
            "sink":
            {
                "type": "BlobSink"
            },
            "translator": 
            {
                "type": "TabularTranslator",
                "ColumnMappings": "UserId: MyUserId, Group: MyGroup, Name: MyName"
            }
        },
       "scheduler": {
              "frequency": "Hour",
              "interval": 1
            }
    }

**Oszlop megfeleltetés folyamat:**

![Oszlop megfeleltetés továbbításához](./media/data-factory-data-stores-with-rectangular-tables/column-mapping-flow.png)

### <a name="sample-2--column-mapping-with-sql-query-from-azure-sql-to-azure-blob"></a>Az Azure SQL lekérdezés SQL Azure blob-leképezés oszlop 2 – minta
Ebben a példában az SQL-lekérdezés használatos adatok kinyerése Azure SQL-egyszerűen megadása "struktúra" szakasz a táblázat neve és az oszlop neve helyett. 

    {
        "name": "CopyActivity",
        "description": "description", 
        "type": "CopyActivity",
        "inputs":  [ { "name": " AzureSQLInput"  } ],
        "outputs":  [ { "name": " AzureBlobOutput" } ],
        "typeProperties":
        {
            "source":
            {
                "type": "SqlSource",
                "SqlReaderQuery": "$$Text.Format('SELECT * FROM MyTable WHERE StartDateTime = \\'{0:yyyyMMdd-HH}\\'', WindowStart)"
            },
            "sink":
            {
                "type": "BlobSink"
            },
            "Translator": 
            {
                "type": "TabularTranslator",
                "ColumnMappings": "UserId: MyUserId, Group: MyGroup,Name: MyName"
            }
        },
        "scheduler": {
              "frequency": "Hour",
              "interval": 1
            }
    }

A lekérdezés eredményének ebben az esetben előbb vannak megfeleltetve "elfoglalt" forrás megadott oszlopokra. Ezután az oszlopok "struktúra" forrásból vannak megfeleltetve gyűjtő "struktúra" oszlopai columnMappings megadott szabályoknak.  Tegyük fel, hogy a lekérdezés eredménye 5 oszlopok, két további oszlopokat, majd azokat a forrás "struktúrában" megadott.

**Oszlop megfeleltetés továbbításához**

![Munkafolyamat-2 oszlop megfeleltetés](./media/data-factory-data-stores-with-rectangular-tables/column-mapping-flow-2.png)







