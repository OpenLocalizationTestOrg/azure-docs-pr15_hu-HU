<properties
   pageTitle="Az SQL adatraktár dinamikus SQL |} Microsoft Azure"
   description="Tippek dinamikus SQL Azure SQL-adatraktár a megoldások fejlesztésével."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="jrowlandjones"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/14/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="dynamic-sql-in-sql-data-warehouse"></a>Az SQL adatraktár dinamikus SQL
SQL-adatraktár alkalmazás kódja fejlesztésekor előfordulhat dinamikus sql segítségével rugalmas, általános és elemes megoldásokat. SQL-adatraktár blob-adattípusok egyelőre nem támogatja. Ez a karakterláncok méretének korlátozhatja, blob-típusok varchar(max) és a nvarchar(max) tartoznak. Ha ilyen használta az alkalmazás kódja nagyon nagy karakterláncok készítésekor, szüksége lesz a kód szétválasztani szövegadattömb és a végrehajtási utasítást használja helyette.

Egy egyszerű példa:

```sql
DECLARE @sql_fragment1 VARCHAR(8000)=' SELECT name '
,       @sql_fragment2 VARCHAR(8000)=' FROM sys.system_views '
,       @sql_fragment3 VARCHAR(8000)=' WHERE name like ''%table%''';

EXEC( @sql_fragment1 + @sql_fragment2 + @sql_fragment3);
```

Ha a karakterlánc rövid szokásos [sp_executesql][] is használhatja.

> [AZURE.NOTE] Dinamikus SQL végrehajtásra kimutatások továbbra is fizetnie összes TSQL érvényességi szabály.

## <a name="next-steps"></a>Következő lépések
Fejlesztési vonatkozó további tippek [fejlesztési áttekintése][]című témakörben találhat.

<!--Image references-->

<!--Article references-->
[fejlesztési – áttekintés]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[Sp_executesql]: https://msdn.microsoft.com/library/ms188001.aspx

<!--Other Web references-->
