<properties
   pageTitle="Adatok betöltése SQL adatraktár (PolyBase) az Azure blob-tárolóhoz |} Microsoft Azure"
   description="Megtudhatja, hogy miként Azure blob-tárolóhoz adatok betöltése SQL adatraktár PolyBase használatával. Néhány táblák nyilvános adatokból betöltése a Contoso kiskereskedelmi adatraktár sémában."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="ckarst"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/25/2016"
   ms.author="cakarst;barbkess;sonyama"/>


# <a name="load-data-from-azure-blob-storage-into-sql-data-warehouse-polybase"></a>Adatok betöltése SQL adatraktár (PolyBase) az Azure blob-tárolóhoz

> [AZURE.SELECTOR]
- [Adatok gyári](sql-data-warehouse-load-from-azure-blob-storage-with-data-factory.md)
- [PolyBase](sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md)

PolyBase és parancsokkal T az SQL Azure blob-tárolóhoz adatok betöltése Azure SQL-adatraktár. 

Ahhoz, hogy egyszerű, ebben az oktatóanyagban betölti két tábla az Azure Blob nyilvános tárhely a a Contoso kiskereskedelmi adatraktár sémában. Töltse be a teljes adatok, futtassa a példában [a teljes Contoso kiskereskedelmi adatraktár betöltése][] a Microsoft SQL Server minták tárat.

Ebben az oktatóanyagban lesz:

1. Állítsa be a PolyBase való betöltéséhez Azure blob-tárolóhoz
2. Nyilvános adatok betöltése az adatbázis
3. A betöltés befejeződése után optimalizálásokat végrehajtása


## <a name="before-you-begin"></a>Első lépések
Ebben az oktatóanyagban futtatásához szükséges egy Azure SQL-adatraktár adatbázis már rendelkező fiókkal. Ha még nem rendelkezik ezzel, lásd: az [létrehozása egy SQL adatraktár][].

## <a name="1-configure-the-data-source"></a>1. a adatforrás konfigurálása

PolyBase helyét és a külső adatok tulajdonságainak megadása külső objektumok az SQL-T használja. A külső objektum SQL adatraktár tárolja. Az adatok magát kívülről vannak tárolva.

### <a name="11-create-a-credential"></a>1.1. Hozzon létre egy hitelesítő adatok

**Kihagyhatja ezt a lépést** , ha a Contoso nyilvános adatot tölt be. Nyilvános adatok biztonságos hozzáférés nem szükséges, mivel már bárki megnyithatja.

**Ez a lépés nem hagyhat ki** alkalmazás használatakor ebben az oktatóanyagban sablonként való betöltésének a saját adatain. Keresztül a hitelesítő adatok eléréséhez a következő parancsfájl használatával hozzon létre egy adatbázis – munkafüzetszintű hitelesítő adatait, és használja azt az adatforrás helyének megadásakor.


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
```

Ugrás a következő lépés: 2.

### <a name="12-create-the-external-data-source"></a>az 1.2. A külső adatforrás létrehozása

A [Külső ADATFORRÁS létrehozása][] paranccsal tárolja az adatokat, és az adatok típusát. 

```sql
CREATE EXTERNAL DATA SOURCE AzureStorage_west_public
WITH 
(  
    TYPE = Hadoop 
,   LOCATION = 'wasbs://contosoretaildw-tables@contosoretaildw.blob.core.windows.net/'
); 
```

> [AZURE.IMPORTANT] Ha úgy dönt, hogy az azure blob tároló nyilvános, ne feledje, hogy az adatok tulajdonosaként, ráterheljük adatok kilépési díjak adatok elhagyásakor az Adatközpont. 

## <a name="2-configure-data-format"></a>2. a adatformátum beállítása

Azure blob-tárolóhoz a szövegfájlban tárolja az adatokat, és az egyes mezők elválasztó van elválasztva. A [Külső FÁJLFORMÁTUM létrehozása][] parancsot, a szövegfájlok formátumban kell az adatokat adhat meg. A Contoso adatok nem tömörített és függőleges vonás tagolva.

```sql
CREATE EXTERNAL FILE FORMAT TextFileFormat 
WITH 
(   FORMAT_TYPE = DELIMITEDTEXT
,   FORMAT_OPTIONS  (   FIELD_TERMINATOR = '|'
                    ,   STRING_DELIMITER = ''
                    ,   DATE_FORMAT      = 'yyyy-MM-dd HH:mm:ss.fff'
                    ,   USE_TYPE_DEFAULT = FALSE 
                    )
);
``` 

## <a name="3-create-the-external-tables"></a>3. a külső táblázatok létrehozása

Most, hogy a forrás- és adatformátum adta meg, készen áll a külső táblákat létrehozni. 

### <a name="31-create-a-schema-for-the-data"></a>3.1. Hozzon létre egy séma az adatokat. 

A hely a Contoso adatokat tárolja az adatbázis, hozzon létre egy séma.

```sql
CREATE SCHEMA [asb]
GO
```

### <a name="32-create-the-external-tables"></a>a 3,2. A külső táblázatok létrehozása. 

Futtassa a a DimProduct és FactOnlineSales külső tábla létrehozása. Az összes azt az alábbi módon oszlopnevek és adattípusok meghatározása, és kötése a helyét és az Azure blob-tároló fájlok formátumának. A definíció SQL adatraktár tárolja, és az adatok még a tárhely Azure Blob van.

A paraméter **HELYET** a legfelső szintű mappa a tárhely Azure Blob mappájában. A táblázat minden egyes van egy másik mappába.


```sql

--DimProduct
CREATE EXTERNAL TABLE [asb].DimProduct (
    [ProductKey] [int] NOT NULL,
    [ProductLabel] [nvarchar](255) NULL,
    [ProductName] [nvarchar](500) NULL,
    [ProductDescription] [nvarchar](400) NULL,
    [ProductSubcategoryKey] [int] NULL,
    [Manufacturer] [nvarchar](50) NULL,
    [BrandName] [nvarchar](50) NULL,
    [ClassID] [nvarchar](10) NULL,
    [ClassName] [nvarchar](20) NULL,
    [StyleID] [nvarchar](10) NULL,
    [StyleName] [nvarchar](20) NULL,
    [ColorID] [nvarchar](10) NULL,
    [ColorName] [nvarchar](20) NOT NULL,
    [Size] [nvarchar](50) NULL,
    [SizeRange] [nvarchar](50) NULL,
    [SizeUnitMeasureID] [nvarchar](20) NULL,
    [Weight] [float] NULL,
    [WeightUnitMeasureID] [nvarchar](20) NULL,
    [UnitOfMeasureID] [nvarchar](10) NULL,
    [UnitOfMeasureName] [nvarchar](40) NULL,
    [StockTypeID] [nvarchar](10) NULL,
    [StockTypeName] [nvarchar](40) NULL,
    [UnitCost] [money] NULL,
    [UnitPrice] [money] NULL,
    [AvailableForSaleDate] [datetime] NULL,
    [StopSaleDate] [datetime] NULL,
    [Status] [nvarchar](7) NULL,
    [ImageURL] [nvarchar](150) NULL,
    [ProductURL] [nvarchar](150) NULL,
    [ETLLoadID] [int] NULL,
    [LoadDate] [datetime] NULL,
    [UpdateDate] [datetime] NULL
)
WITH
(
    LOCATION='/DimProduct/' 
,   DATA_SOURCE = AzureStorage_west_public
,   FILE_FORMAT = TextFileFormat
,   REJECT_TYPE = VALUE
,   REJECT_VALUE = 0
)
;
 
--FactOnlineSales
CREATE EXTERNAL TABLE [asb].FactOnlineSales 
(
    [OnlineSalesKey] [int]  NOT NULL,
    [DateKey] [datetime] NOT NULL,
    [StoreKey] [int] NOT NULL,
    [ProductKey] [int] NOT NULL,
    [PromotionKey] [int] NOT NULL,
    [CurrencyKey] [int] NOT NULL,
    [CustomerKey] [int] NOT NULL,
    [SalesOrderNumber] [nvarchar](20) NOT NULL,
    [SalesOrderLineNumber] [int] NULL,
    [SalesQuantity] [int] NOT NULL,
    [SalesAmount] [money] NOT NULL,
    [ReturnQuantity] [int] NOT NULL,
    [ReturnAmount] [money] NULL,
    [DiscountQuantity] [int] NULL,
    [DiscountAmount] [money] NULL,
    [TotalCost] [money] NOT NULL,
    [UnitCost] [money] NULL,
    [UnitPrice] [money] NULL,
    [ETLLoadID] [int] NULL,
    [LoadDate] [datetime] NULL,
    [UpdateDate] [datetime] NULL
)
WITH
(
    LOCATION='/FactOnlineSales/' 
,   DATA_SOURCE = AzureStorage_west_public
,   FILE_FORMAT = TextFileFormat
,   REJECT_TYPE = VALUE
,   REJECT_VALUE = 0
)
;
```

## <a name="4-load-the-data"></a>4. a adat betöltése
Nincs változatos módszerekkel, amelyekkel a külső adatok eléréséhez.  A lekérdezés adatokat közvetlenül a külső tábla, az adatok betöltése új adatbázistáblák vagy külső adatok hozzáadása meglévő adatbázistáblák.  


### <a name="41-create-a-new-schema"></a>4.1. Hozzon létre egy új sémát

CTAS új táblát hoz létre, amely tartalmazza az adatokat.  Első lépésként hozzon létre a contoso adatok séma.

```sql
CREATE SCHEMA [cso]
GO
```

### <a name="42-load-the-data-into-new-tables"></a>4.2. Adatok betöltése új táblázatokhoz

Adatok betöltése Azure blob-tárolóhoz, és mentse a táblázatban szereplő az adatbázist, használja az [Létrehozása táblában AS SELECT (Transact-SQL nyelvben)][] utasítást. A CTAS betöltése az imént létrehozott erősen beírt külső táblák használja. Töltse be az adatokat az új táblákat, használja a tábla egy [CTAS][] kimutatást. 

CTAS új táblát hoz létre, és feltölti a select utasítás eredményeivel. CTAS határozza meg, hogy az azonos oszlopokat és adattípusok a select utasítás eredményeit, az új táblát. Ha bejelöli az összes oszlopot külső táblából, az új tábla lesz az oszlopokat és a külső tábla adattípusok replikáját.

Ebben a példában hozzunk létre a dimenzió és a ténytábla szerint elosztott táblák kivonat is. 


```sql
SELECT GETDATE();
GO

CREATE TABLE [cso].[DimProduct]            WITH (DISTRIBUTION = HASH([ProductKey]  ) ) AS SELECT * FROM [asb].[DimProduct]             OPTION (LABEL = 'CTAS : Load [cso].[DimProduct]             ');
CREATE TABLE [cso].[FactOnlineSales]       WITH (DISTRIBUTION = HASH([ProductKey]  ) ) AS SELECT * FROM [asb].[FactOnlineSales]        OPTION (LABEL = 'CTAS : Load [cso].[FactOnlineSales]        ');
```

### <a name="43-track-the-load-progress"></a>A 4,3 a betöltés nyilvántartása

A dinamikus kezelése nézetekben (DMVs) terhelés végrehajtásának nyomon követheti. 

```sql
-- To see all requests
SELECT * FROM sys.dm_pdw_exec_requests;

-- To see a particular request identified by its label
SELECT * FROM sys.dm_pdw_exec_requests as r;
WHERE r.[label] = 'CTAS : Load [cso].[DimProduct]             '
      OR r.[label] = 'CTAS : Load [cso].[FactOnlineSales]        '
;

-- To track bytes and files
SELECT
    r.command,
    s.request_id,
    r.status,
    count(distinct input_name) as nbr_files, 
    sum(s.bytes_processed)/1024/1024 as gb_processed
FROM
    sys.dm_pdw_exec_requests r
    inner join sys.dm_pdw_dms_external_work s
        on r.request_id = s.request_id
WHERE 
    r.[label] = 'CTAS : Load [cso].[DimProduct]             '
    OR r.[label] = 'CTAS : Load [cso].[FactOnlineSales]        '
GROUP BY
    r.command,
    s.request_id,
    r.status
ORDER BY
    nbr_files desc,
    gb_processed desc;
```

## <a name="5-optimize-columnstore-compression"></a>5. columnstore tömörítés optimalizálása

Alapértelmezés szerint SQL adatraktár szerint csoportosított columnstore index létrehozása a táblázat tárolja. A betöltés befejeződése után az adatsorok részét előfordulhat, hogy nem lehet tömöríteni a columnstore be.  Számos oka, hogy miért Ez akkor történhet meg van. További tudnivalókért olvassa el a [columnstore Indexek kezelése][]című témakört.

A lekérdezési teljesítmény és a betöltése után columnstore tömörítés optimalizálása, újraépítéséhez kényszeríthet ki az columnstore index, akkor az összes sort a tábla. 

```sql
SELECT GETDATE();
GO

ALTER INDEX ALL ON [cso].[DimProduct]               REBUILD;
ALTER INDEX ALL ON [cso].[FactOnlineSales]          REBUILD;
```

További tájékoztatást a columnstore indexek fenntartása témakörben [columnstore Indexek kezelése][] .

## <a name="6-optimize-statistics"></a>6. statisztikák optimalizálása

Célszerű egyoszlopos statisztikák létrehozása közvetlenül azután, hogy a betöltés. Vannak olyan, statisztikai lehetőség közül választhat. Például ha egyoszlopos statisztikák minden oszlopot hoz létre a statisztikai adatok újraépítéséhez túl hosszú ideig eltarthat. Ha tudja, hogy bizonyos oszlopok nem fog szerepelni lekérdezés predikátumok, kihagyhatja létrehozása statisztika a azokban az oszlopokban.

Ha úgy dönt, hogy minden oszlopra a mindegyik táblázat létrehozása a soron statisztika, használhatja a tárolt eljárás kód minta `prc_sqldw_create_stats` [Statisztika][] című témakörben.

A következő példa egy jó kiindulási pont statisztikai adatokat. A dimenzió tábla oszlopainak, és a ténytáblák csatlakozó oszlopainak egyoszlopos statisztika hoz létre. Mindig vehet egy vagy több oszlop statisztikák más FAKT Táblázatoszlopok később.


```sql
CREATE STATISTICS [stat_cso_DimProduct_AvailableForSaleDate] ON [cso].[DimProduct]([AvailableForSaleDate]);
CREATE STATISTICS [stat_cso_DimProduct_BrandName] ON [cso].[DimProduct]([BrandName]);
CREATE STATISTICS [stat_cso_DimProduct_ClassID] ON [cso].[DimProduct]([ClassID]);
CREATE STATISTICS [stat_cso_DimProduct_ClassName] ON [cso].[DimProduct]([ClassName]);
CREATE STATISTICS [stat_cso_DimProduct_ColorID] ON [cso].[DimProduct]([ColorID]);
CREATE STATISTICS [stat_cso_DimProduct_ColorName] ON [cso].[DimProduct]([ColorName]);
CREATE STATISTICS [stat_cso_DimProduct_ETLLoadID] ON [cso].[DimProduct]([ETLLoadID]);
CREATE STATISTICS [stat_cso_DimProduct_ImageURL] ON [cso].[DimProduct]([ImageURL]);
CREATE STATISTICS [stat_cso_DimProduct_LoadDate] ON [cso].[DimProduct]([LoadDate]);
CREATE STATISTICS [stat_cso_DimProduct_Manufacturer] ON [cso].[DimProduct]([Manufacturer]);
CREATE STATISTICS [stat_cso_DimProduct_ProductDescription] ON [cso].[DimProduct]([ProductDescription]);
CREATE STATISTICS [stat_cso_DimProduct_ProductKey] ON [cso].[DimProduct]([ProductKey]);
CREATE STATISTICS [stat_cso_DimProduct_ProductLabel] ON [cso].[DimProduct]([ProductLabel]);
CREATE STATISTICS [stat_cso_DimProduct_ProductName] ON [cso].[DimProduct]([ProductName]);
CREATE STATISTICS [stat_cso_DimProduct_ProductSubcategoryKey] ON [cso].[DimProduct]([ProductSubcategoryKey]);
CREATE STATISTICS [stat_cso_DimProduct_ProductURL] ON [cso].[DimProduct]([ProductURL]);
CREATE STATISTICS [stat_cso_DimProduct_Size] ON [cso].[DimProduct]([Size]);
CREATE STATISTICS [stat_cso_DimProduct_SizeRange] ON [cso].[DimProduct]([SizeRange]);
CREATE STATISTICS [stat_cso_DimProduct_SizeUnitMeasureID] ON [cso].[DimProduct]([SizeUnitMeasureID]);
CREATE STATISTICS [stat_cso_DimProduct_Status] ON [cso].[DimProduct]([Status]);
CREATE STATISTICS [stat_cso_DimProduct_StockTypeID] ON [cso].[DimProduct]([StockTypeID]);
CREATE STATISTICS [stat_cso_DimProduct_StockTypeName] ON [cso].[DimProduct]([StockTypeName]);
CREATE STATISTICS [stat_cso_DimProduct_StopSaleDate] ON [cso].[DimProduct]([StopSaleDate]);
CREATE STATISTICS [stat_cso_DimProduct_StyleID] ON [cso].[DimProduct]([StyleID]);
CREATE STATISTICS [stat_cso_DimProduct_StyleName] ON [cso].[DimProduct]([StyleName]);
CREATE STATISTICS [stat_cso_DimProduct_UnitCost] ON [cso].[DimProduct]([UnitCost]);
CREATE STATISTICS [stat_cso_DimProduct_UnitOfMeasureID] ON [cso].[DimProduct]([UnitOfMeasureID]);
CREATE STATISTICS [stat_cso_DimProduct_UnitOfMeasureName] ON [cso].[DimProduct]([UnitOfMeasureName]);
CREATE STATISTICS [stat_cso_DimProduct_UnitPrice] ON [cso].[DimProduct]([UnitPrice]);
CREATE STATISTICS [stat_cso_DimProduct_UpdateDate] ON [cso].[DimProduct]([UpdateDate]);
CREATE STATISTICS [stat_cso_DimProduct_Weight] ON [cso].[DimProduct]([Weight]);
CREATE STATISTICS [stat_cso_DimProduct_WeightUnitMeasureID] ON [cso].[DimProduct]([WeightUnitMeasureID]);
CREATE STATISTICS [stat_cso_FactOnlineSales_CurrencyKey] ON [cso].[FactOnlineSales]([CurrencyKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_CustomerKey] ON [cso].[FactOnlineSales]([CustomerKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_DateKey] ON [cso].[FactOnlineSales]([DateKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_OnlineSalesKey] ON [cso].[FactOnlineSales]([OnlineSalesKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_ProductKey] ON [cso].[FactOnlineSales]([ProductKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_PromotionKey] ON [cso].[FactOnlineSales]([PromotionKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_StoreKey] ON [cso].[FactOnlineSales]([StoreKey]);
```

## <a name="achievement-unlocked"></a>Az eredményszint zárolását!

Nyilvános adatok sikeres betöltött Azure SQL-adatraktár be. Remek munka!

Most már elkezdheti lekérdezése a táblák, lekérdezések, az alábbihoz hasonló:

```sql
SELECT  SUM(f.[SalesAmount]) AS [sales_by_brand_amount]
,       p.[BrandName]
FROM    [cso].[FactOnlineSales] AS f
JOIN    [cso].[DimProduct]      AS p ON f.[ProductKey] = p.[ProductKey]
GROUP BY p.[BrandName]
```

## <a name="next-steps"></a>Következő lépések
A teljes Contoso kiskereskedelmi adatraktár adat betöltése, a parancsfájlt, a további fejlesztés tippeket című témakörben olvashat [SQL adatraktár fejlesztési áttekintése][].

<!--Image references-->

<!--Article references-->
[Hozzon létre egy SQL adatraktár]: sql-data-warehouse-get-started-provision.md
[Load data into SQL Data Warehouse]: sql-data-warehouse-overview-load.md
[SQL-adatraktár fejlesztése – áttekintés]: sql-data-warehouse-overview-develop.md
[indexek columnstore kezelése]: sql-data-warehouse-tables-index.md
[Statisztika]: sql-data-warehouse-tables-statistics.md
[CTAS]: sql-data-warehouse-develop-ctas.md
[label]: sql-data-warehouse-develop-label.md

<!--MSDN references-->
[A KÜLSŐ ADATFORRÁS LÉTREHOZÁSA]: https://msdn.microsoft.com/en-us/library/dn935022.aspx
[HOZZON LÉTRE KÜLSŐ FÁJLFORMÁTUM]: https://msdn.microsoft.com/en-us/library/dn935026.aspx
[TÁBLÁZAT kijelölése szerint (Transact-SQL nyelvben) létrehozása]: https://msdn.microsoft.com/library/mt204041.aspx
[sys.dm_pdw_exec_requests]: https://msdn.microsoft.com/library/mt203887.aspx
[REBUILD]: https://msdn.microsoft.com/library/ms188388.aspx

<!--Other Web references-->
[Microsoft Download Center]: http://www.microsoft.com/download/details.aspx?id=36433
[A teljes Contoso kiskereskedelmi adatraktár betöltése]: https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/contoso-data-warehouse/readme.md
