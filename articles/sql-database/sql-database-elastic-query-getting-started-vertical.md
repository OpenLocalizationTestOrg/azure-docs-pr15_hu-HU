<properties
    pageTitle="Első lépések az idegen adatbázis-lekérdezések (függőleges szétválasztás) |} Microsoft Azure"   
    description="függőlegesen particionált adatbázisok rugalmas adatbázis-lekérdezés használata"
    services="sql-database"
    documentationCenter=""  
    manager="jhubbard"
    authors="torsteng"/>

<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/23/2016"
    ms.author="torsteng" />

# <a name="get-started-with-cross-database-queries-vertical-partitioning-preview"></a>Első lépések az idegen adatbázis-lekérdezések (függőleges szétválasztás) (előzetes verzió)

Rugalmas adatbázis-lekérdezés (előzetes verzió) Azure SQL-adatbázis több adatbázisait egyetlen csatlakozási pontot átterjedő T-SQL-lekérdezések futtatása teszi lehetővé. Ez a témakör a [függőlegesen particionálva adatbázisok](sql-database-elastic-query-vertical-partitioning.md)vonatkozik.  

Amikor befejeződik, fog: megtudhatja, hogy miként beállítása és használata az Azure SQL-adatbázis több kapcsolódó adatbázisok átterjedő lekérdezések végrehajtásához. 

A rugalmas adatbázis-lekérdezés szolgáltatással kapcsolatos további tudnivalókért lásd a [Azure SQL-adatbázis rugalmas adatbázis a lekérdezések áttekintése](sql-database-elastic-query-overview.md). 

## <a name="create-the-sample-databases"></a>A mintavállalati adatbázis létrehozása

Kezdésként szükség hozzon létre két adatbázist, a **Vevők** és a **rendelések**, akár ugyanazon vagy másik logikai Servers.   

Hajtsa végre a következő lekérdezések a **rendelések** adatbázist hozhat létre a **OrderInformation** táblázatot, és a bemeneti mintaadatokat. 

    CREATE TABLE [dbo].[OrderInformation]( 
        [OrderID] [int] NOT NULL, 
        [CustomerID] [int] NOT NULL 
        ) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (123, 1) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (149, 2) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (857, 2) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (321, 1) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (564, 8) 

Most hajtsa végre követően a **CustomerInformation** táblázat létrehozása és a bemeneti mintaadatokat az **ügyfelek** adatbázis lekérdezéséhez. 

    CREATE TABLE [dbo].[CustomerInformation]( 
        [CustomerID] [int] NOT NULL, 
        [CustomerName] [varchar](50) NULL, 
        [Company] [varchar](50) NULL 
        CONSTRAINT [CustID] PRIMARY KEY CLUSTERED ([CustomerID] ASC) 
    ) 
    INSERT INTO [dbo].[CustomerInformation] ([CustomerID], [CustomerName], [Company]) VALUES (1, 'Jack', 'ABC') 
    INSERT INTO [dbo].[CustomerInformation] ([CustomerID], [CustomerName], [Company]) VALUES (2, 'Steve', 'XYZ') 
    INSERT INTO [dbo].[CustomerInformation] ([CustomerID], [CustomerName], [Company]) VALUES (3, 'Lylla', 'MNO') 

## <a name="create-database-objects"></a>Adatbázis-objektumok létrehozása
### <a name="database-scoped-master-key-and-credentials"></a>Adatbázis hatóköre fő billentyű és a hitelesítő adatok

1. Nyissa meg az SQL Server Management Studio vagy SQL Server Data Tools a Visual Studióban.
2. Csatlakozás a rendelések adatbázishoz, és hajtsa végre a következő T-SQL-parancsok:

        CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<password>'; 
        CREATE DATABASE SCOPED CREDENTIAL ElasticDBQueryCred 
        WITH IDENTITY = '<username>', 
        SECRET = '<password>';  

    A "felhasználónév" és "jelszó" a használt felhasználónév és jelszó kell lennie az ügyfelek adatbázisba történő bejelentkezéshez.
    A rugalmas lekérdezések használja az Azure Active Directory Authentication jelenleg nem támogatott.

### <a name="external-data-sources"></a>Külső adatforrások
Hozzon létre egy külső adatforráshoz, hajtsa végre a következő parancsot a rendelések-adatbázishoz: 

    CREATE EXTERNAL DATA SOURCE MyElasticDBQueryDataSrc WITH 
        (TYPE = RDBMS, 
        LOCATION = '<server_name>.database.windows.net', 
        DATABASE_NAME = 'Customers', 
        CREDENTIAL = ElasticDBQueryCred, 
    ) ;

### <a name="external-tables"></a>Külső tábla
Hozzon létre egy külső tábla a rendelések adatbázist, amely megfelel a CustomerInformation tábla meghatározása:

    CREATE EXTERNAL TABLE [dbo].[CustomerInformation] 
    ( [CustomerID] [int] NOT NULL, 
      [CustomerName] [varchar](50) NOT NULL, 
      [Company] [varchar](50) NOT NULL) 
    WITH 
    ( DATA_SOURCE = MyElasticDBQueryDataSrc) 

## <a name="execute-a-sample-elastic-database-t-sql-query"></a>Egy minta rugalmas adatbázis-T az SQL-lekérdezés végrehajtása

Miután meghatározta a külső adatforrásban és a külső táblázatok most már használhatja az SQL-T a külső táblák lekérdezéséhez. A lekérdezés végrehajtása a rendelések adatbázishoz: 

    SELECT OrderInformation.CustomerID, OrderInformation.OrderId, CustomerInformation.CustomerName, CustomerInformation.Company 
    FROM OrderInformation 
    INNER JOIN CustomerInformation 
    ON CustomerInformation.CustomerID = OrderInformation.CustomerID 

## <a name="cost"></a>Költség

Jelenleg a rugalmas adatbázis-lekérdezés szolgáltatás szerepel az Azure SQL-adatbázis költségét.  

Árinformációkat [SQL-adatbázis árak](/pricing/details/sql-database)talál. 


[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->

<!--anchors-->
