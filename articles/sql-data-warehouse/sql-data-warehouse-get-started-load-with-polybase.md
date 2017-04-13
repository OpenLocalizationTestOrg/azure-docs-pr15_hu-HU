<properties
   pageTitle="SQL-adatok raktári oktatóanyagban PolyBase |} Microsoft Azure"
   description="Ismerje meg, mi PolyBase és az esetek adattárolási használatához."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="ckarst"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="10/10/2016"
   ms.author="cakarst;barbkess;sonyama"/>


# <a name="load-data-with-polybase-in-sql-data-warehouse"></a>Adatok betöltése és az SQL adatraktár PolyBase

> [AZURE.SELECTOR]
- [Redgate](sql-data-warehouse-load-with-redgate.md)  
- [Adatok gyári](sql-data-warehouse-get-started-load-with-azure-data-factory.md)  
- [PolyBase](sql-data-warehouse-get-started-load-with-polybase.md)  
- [BCP](sql-data-warehouse-load-with-bcp.md)

Ebből az oktatóanyagból megtudhatja, hogy miként adatok betöltése SQL adatraktár AzCopy és PolyBase használatával. Ha végzett, akkor tudni fogja, hogy miként:

- Adatok másolása Azure blob-tárolóhoz AzCopy használatával
- Az adatok megadását adatbázis-objektumok létrehozása
- Töltse be az adatokat az SQL-T lekérdezések futtatása

>[AZURE.VIDEO loading-data-with-polybase-in-azure-sql-data-warehouse]

## <a name="prerequisites"></a>Előfeltételek

Ebben az oktatóanyagban lépéseinek, szüksége van

- SQL-adatraktár adatbázis.
- Azure tároló fiók típusú normál helyileg felesleges tároló (normál-LRS) szabványos Geo-redundáns tároló (Standard, GRS) vagy a szabványos olvasásra Geo-redundáns tároló (Standard, RAGRS).
- AzCopy parancssori segédprogramot. Töltse le és telepítve van a Microsoft Azure tároló eszközzel [AzCopy legújabb verzióját][] .

    ![Azure tároló eszközök](./media/sql-data-warehouse-get-started-load-with-polybase/install-azcopy.png)


## <a name="step-1-add-sample-data-to-azure-blob-storage"></a>Lépés: 1: Vehet fel mintaadatokat Azure blob-tárolóhoz

Annak érdekében, hogy az adat betöltése, mintaadatokat is tartalmazó üzembe Azure blob-tárolóhoz szükség. Ebben a lépésben a mintaadatokat tartalmazó-tároló Azure blob feltölteni azt. Később fogjuk használni PolyBase betöltése a mintaadatokat az SQL adatraktár adatbázisba.

### <a name="a-prepare-a-sample-text-file"></a>VÁLASZOK PARANCSRA. Felkészülés a minta szövegfájl

Szöveg mintafájl előkészítése:

1. Nyissa meg a Jegyzettömböt, és a következő sornyi adatot másol egy új fájlt. Mentse a a helyi temp könyvtár % temp%\DimDate2.txt.

```
20150301,1,3
20150501,2,4
20151001,4,2
20150201,1,3
20151201,4,2
20150801,3,1
20150601,2,4
20151101,4,2
20150401,2,4
20150701,3,1
20150901,3,1
20150101,1,3
```

### <a name="b-find-your-blob-service-endpoint"></a>B Keresse meg a blob-szolgáltatás végpontjának

A blob-szolgáltatás végpontjának megkeresése:

1. Az Azure-portálon kattintson a **Tallózás** > **Tároló fiókok**.
2. Kattintson a használni kívánt fiókra tároló.
3. Kattintson a tárolás fiók lap BLOB

    ![Kattintson a BLOB](./media/sql-data-warehouse-get-started-load-with-polybase/click-blobs.png)

1. Mentés a blob szolgáltatás végpontjának URL-CÍMÉT.

    ![BLOB-szolgáltatás végpontjának](./media/sql-data-warehouse-get-started-load-with-polybase/blob-service.png)

### <a name="c-find-your-azure-storage-key"></a>C. Keresse meg a Azure tároló használatával

Az Azure tároló kulcs megkeresése:

1. Az Azure portálról, jelölje be a **Tallózás** > **Tároló fiókok**.
2. Kattintson a használni kívánt tárterület-fiókot.
3. Válassza a **minden beállítások** > **hívóbetűk**.
4. Jelölje be az másolása a vágólapra másolása a hívóbetűk közül.

    ![Azure tároló kulcs másolása](./media/sql-data-warehouse-get-started-load-with-polybase/access-key.png)

### <a name="d-copy-the-sample-file-to-azure-blob-storage"></a>D Másolja a vágólapra a mintafájl Azure blob-tárolóhoz

Az adatok másolása Azure blob-tárolóhoz:

1. Nyisson meg egy parancssort, és módosítsa a könyvtárak a AzCopy telepítési címtárhoz. Ez a parancs az alapértelmezett könyvtár 64 bites Windows ügyfélen változik.

    ```
    cd /d "%ProgramFiles(x86)%\Microsoft SDKs\Azure\AzCopy"
    ```

1. Töltse fel a fájlt a következő parancs futtatásával. Adja meg a blob szolgáltatás végpontjának URL-címe <blob service endpoint URL> és a Azure tároló fiókkulcs < azure_storage_account_key > számára.

    ```
    .\AzCopy.exe /Source:C:\Temp\ /Dest:<blob service endpoint URL> /datacontainer/datedimension/ /DestKey:<azure_storage_account_key> /Pattern:DimDate2.txt
    ```

Lásd még: [első lépések a AzCopy parancssori segédprogramot][].

### <a name="e-explore-your-blob-storage-container"></a>M BILLENTYŰKOMBINÁCIÓT. Ismerje meg a blob-tároló tároló

A fájl megtekintéséhez feltöltött blob-tárolóhoz:

1. Térjen vissza a Blob-szolgáltatás lap.
2. A tárolók kattintson duplán a **datacontainer**.
3. Ismerje meg az elérési út az adatokban, kattintson a mappa **datedimension** , és látni fogja a feltöltött fájl **DimDate2.txt**.
4. Tulajdonságainak megtekintéséhez kattintson a **DimDate2.txt**.
5. Figyelje meg, hogy a Blob-Tulajdonságok lap a letöltheti, vagy törölje a fájlt.

    ![Nézet tároló Azure blob](./media/sql-data-warehouse-get-started-load-with-polybase/view-blob.png)


## <a name="step-2-create-an-external-table-for-the-sample-data"></a>Lépés: 2: A mintaadatokat egy külső tábla létrehozása

Ebben a részben hozzunk létre egy külső táblában, amely meghatározza a mintaadatokat.

PolyBase külső táblákat használ Azure blob-tárolóban lévő adatok eléréséhez. Belül SQL adatraktár nem tárolja az adatokat, mivel PolyBase kezeli a külső adatok hitelesítési egy adatbázis – munkafüzetszintű hitelesítő adatok használatával.

Az ebben a lépésben a példában a Transact-SQL-utasítások külső tábla létrehozása.

- [Diaminta kulcs létrehozása (Transact-SQL nyelvben)][] , a titkos az adatbázis titkosítása hatóköre a hitelesítő adatait.
- [Adatbázis létrehozása az hitelesítő hatóköre (Transact-SQL nyelvben)][] hitelesítési adatait az Azure tároló fiók megadásához.
- [A külső adatforrás létrehozása (Transact-SQL nyelvben)][] az Azure blob-tárolóhoz helyének megadásához.
- [Hozzon létre külső fájlformátum (Transact-SQL nyelvben)][] adja meg az adatok formátumát.
- [Külső tábla létrehozása (Transact-SQL nyelvben)][] adja meg a tábla megadása és az adatok helyét.

A lekérdezés futtatása a adatraktár SQL-adatbázisban. Egy külső táblázat DimDate2External, amely a DimDate2.txt mintaadatokat az Azure blob-tárolóhoz mutat dbo sémában hoz létre.


```sql
-- A: Create a master key.
-- Only necessary if one does not already exist.
-- Required to encrypt the credential secret in the next step.

CREATE MASTER KEY;


-- B: Create a database scoped credential
-- IDENTITY: Provide any string, it is not used for authentication to Azure storage.
-- SECRET: Provide your Azure storage account key.


CREATE DATABASE SCOPED CREDENTIAL AzureStorageCredential
WITH
    IDENTITY = 'user',
    SECRET = '<azure_storage_account_key>'
;


-- C: Create an external data source
-- TYPE: HADOOP - PolyBase uses Hadoop APIs to access data in Azure blob storage.
-- LOCATION: Provide Azure storage account name and blob container name.
-- CREDENTIAL: Provide the credential created in the previous step.

CREATE EXTERNAL DATA SOURCE AzureStorage
WITH (
    TYPE = HADOOP,
    LOCATION = 'wasbs://<blob_container_name>@<azure_storage_account_name>.blob.core.windows.net',
    CREDENTIAL = AzureStorageCredential
);


-- D: Create an external file format
-- FORMAT_TYPE: Type of file format in Azure storage (supported: DELIMITEDTEXT, RCFILE, ORC, PARQUET).
-- FORMAT_OPTIONS: Specify field terminator, string delimiter, date format etc. for delimited text files.
-- Specify DATA_COMPRESSION method if data is compressed.

CREATE EXTERNAL FILE FORMAT TextFile
WITH (
    FORMAT_TYPE = DelimitedText,
    FORMAT_OPTIONS (FIELD_TERMINATOR = ',')
);


-- E: Create the external table
-- Specify column names and data types. This needs to match the data in the sample file.
-- LOCATION: Specify path to file or directory that contains the data (relative to the blob container).
-- To point to all files under the blob container, use LOCATION='.'

CREATE EXTERNAL TABLE dbo.DimDate2External (
    DateId INT NOT NULL,
    CalendarQuarter TINYINT NOT NULL,
    FiscalQuarter TINYINT NOT NULL
)
WITH (
    LOCATION='/datedimension/',
    DATA_SOURCE=AzureStorage,
    FILE_FORMAT=TextFile
);


-- Run a query on the external table

SELECT count(*) FROM dbo.DimDate2External;

```


Az SQL Server objektum Explorerben a Visual Studióban láthatja, hogy a külső fájlformátumot, a külső adatforrás és a DimDate2External táblában.

![Külső tábla megtekintése](./media/sql-data-warehouse-get-started-load-with-polybase/external-table.png)

## <a name="step-3-load-data-into-sql-data-warehouse"></a>3 lépés: Az SQL-adatraktár adatok betöltése

Amikor létrejött a külső táblában, betöltheti az adatokat az új táblába, vagy szúrja be a meglévő táblához.

- Új táblába, töltse be az adatokat, futtassa az [Létrehozása táblában szerint jelölje be (Transact-SQL nyelvben)][] utasítás. Az új tábla a lekérdezésben oszlop lesz. Az oszlopok adattípusát fognak egyezni a adattípusok meghatározása a külső tábla.
- Adatok betöltése meglévő táblához, használja a [Beszúrás... Jelölje be (Transact-SQL nyelvben)][] utasítás.

```sql
-- Load the data from Azure blob storage to SQL Data Warehouse

CREATE TABLE dbo.DimDate2
WITH
(   
    CLUSTERED COLUMNSTORE INDEX,
    DISTRIBUTION = ROUND_ROBIN
)
AS
SELECT * FROM [dbo].[DimDate2External];
```

## <a name="step-4-create-statistics-on-your-newly-loaded-data"></a>Lépés: 4: Az újonnan betöltött adatok statisztikai létrehozása

SQL-adatraktár nem automatikus létrehozása vagy az automatikus frissítés statisztikai adatokat. Ezért magas lekérdezési teljesítmény elérése érdekében fontos statisztikák létrehozásához az egyes tábla minden egyes oszlopát az első betöltése után. Fontos a statisztika frissítése után az adatok jelentős módosításokat is.

Ez a példa egyoszlopos statisztikák DimDate2 új táblát hoz létre.

```sql
CREATE STATISTICS [DateId] on [DimDate2] ([DateId]);
CREATE STATISTICS [CalendarQuarter] on [DimDate2] ([CalendarQuarter]);
CREATE STATISTICS [FiscalQuarter] on [DimDate2] ([FiscalQuarter]);
```

További tudnivalókért lásd: [statisztikai adatokat][].  


## <a name="next-steps"></a>Következő lépések
További tudnivalók PolyBase használó megoldást fejleszt, információk [PolyBase útmutató][] című témakörben.

<!--Image references-->


<!--Article references-->
[PolyBase in SQL Data Warehouse Tutorial]: ./sql-data-warehouse-get-started-load-with-polybase.md
[Load data with bcp]: ./sql-data-warehouse-load-with-bcp.md
[Statisztika]: ./sql-data-warehouse-tables-statistics.md
[PolyBase útmutató]: ./sql-data-warehouse-load-polybase-guide.md
[Első lépések a AzCopy parancssori segédprogram]: ../storage/storage-use-azcopy.md
[AzCopy legújabb verziója]: ../storage/storage-use-azcopy.md

<!--External references-->
[supported source/sink]: https://msdn.microsoft.com/library/dn894007.aspx
[copy activity]: https://msdn.microsoft.com/library/dn835035.aspx
[SQL Server destination adapter]: https://msdn.microsoft.com/library/ms141095.aspx
[SSIS]: https://msdn.microsoft.com/library/ms141026.aspx


[Hozzon létre külső ADATFORRÁST (Transact-SQL)]:https://msdn.microsoft.com/library/dn935022.aspx
[Hozzon létre külső FÁJLFORMÁTUM (Transact-SQL)]:https://msdn.microsoft.com/library/dn935026.aspx
[KÜLSŐ tábla (Transact-SQL nyelvben) létrehozása]:https://msdn.microsoft.com/library/dn935021.aspx

[DROP EXTERNAL DATA SOURCE (Transact-SQL)]:https://msdn.microsoft.com/library/mt146367.aspx
[DROP EXTERNAL FILE FORMAT (Transact-SQL)]:https://msdn.microsoft.com/library/mt146379.aspx
[DROP EXTERNAL TABLE (Transact-SQL)]:https://msdn.microsoft.com/library/mt130698.aspx

[TÁBLÁZAT kijelölése szerint (Transact-SQL nyelvben) létrehozása]:https://msdn.microsoft.com/library/mt204041.aspx
[BESZÚRÁS... Jelölje ki (a Transact-SQL)]:https://msdn.microsoft.com/library/ms174335.aspx
[DIAMINTA kulcs (Transact-SQL nyelvben) létrehozása]:https://msdn.microsoft.com/library/ms174382.aspx
[CREATE CREDENTIAL (Transact-SQL)]:https://msdn.microsoft.com/library/ms189522.aspx
[ADATBÁZIS LAPSZINTŰ hitelesítő adatok (a Transact-SQL nyelvben) létrehozása]:https://msdn.microsoft.com/library/mt270260.aspx
[DROP CREDENTIAL (Transact-SQL)]:https://msdn.microsoft.com/library/ms189450.aspx
