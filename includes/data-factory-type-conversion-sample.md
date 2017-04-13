### <a name="type-conversion-sample"></a>Konvertálás minta
Az alábbi példa az adatok másolása Blob SQL Azure rendelkező típus konverziója során nem.

Tegyük fel, hogy a Blob-adatkészlet CSV-formátumban van, és a 3 oszlopot tartalmaz. Őket egyik dátumidő oszlop egy egyéni datetime formátumban, a hét napjának rövidített francia nevekkel.

A következő típusú definíciók az oszlopok együtt az Blob-adatforrás adatkészlet határozza meg.

    {
        "name": "AzureBlobTypeSystemInput",
        "properties":
        {
             "structure": 
              [
                    { "name": "userid", "type": "Int64"},
                    { "name": "name", "type": "String"},
                    { "name": "lastlogindate", "type": "Datetime", "culture": "fr-fr", "format": "ddd-MM-YYYY"}
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
            "external": true,
            "availability":
            {
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

Adott az SQL .NET típus leképezési táblázat fölött, típus volna megadása az Azure SQL-táblázat az alábbi séma használata

| Oszlopnév | SQL-típus |
| ----------- | -------- |
| felhasználóazonosító | bigint |
| név | szöveg |
| lastlogindate | dátum és idő |

Ezután az Azure SQL-adatkészlet következőképpen fogja meghatározni. Megjegyzés: Nem szükséges adja meg a "struktúra" szakasz típusú adatokkal óta a típus információ már szerepel a mögöttes adatok áruházból.

    {
        "name": "AzureSQLOutput",
        "properties": {
            "type": "AzureSqlTable",
            "linkedServiceName": "AzureSqlLinkedService",
            "typeProperties": {
                "tableName": "MyTable"
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }

Ebben az esetben adatok gyári automatikusan elvégezni a dokumentumkonvertálás, beleértve az egyéni datetime a dátum és idő mező formázása a fr-fr kulturális környezet használatával, ha az adatok áthelyezése Azure SQL Blob típusát.


