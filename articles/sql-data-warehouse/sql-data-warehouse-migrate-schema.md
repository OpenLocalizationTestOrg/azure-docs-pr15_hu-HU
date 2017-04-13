<properties
   pageTitle="A séma áttelepítése SQL adatraktár |} Microsoft Azure"
   description="Tippek a séma áttelepítés Azure SQL-adatraktár megoldások fejlesztésével."
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
   ms.date="08/25/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="migrate-your-schema-to-sql-data-warehouse"></a>A séma áttelepítése SQL adatraktár#

A következő összefoglalók megismerheti az SQL Server és a SQL adatraktár segít áttelepítése az adatbázis közötti különbségek.

## <a name="table-migration"></a>Táblázat-áttelepítés

A táblázatok áttelepítéséhez, célszerű Ismerkedjen meg az SQL adatraktár táblák táblázat funkcióival.  A [táblázat áttekintése][] az remek kiindulási.  Ez a cikk bemutatja a legfontosabb szempont egy táblázat, például táblázat statisztika, terjesztési, szétválasztás, és az indexelés létrehozásakor.  Egyes [nem támogatott táblázatszolgáltatások][] és a lehetséges megoldások is foglalkozik.

SQL-adatraktár támogatja a gyakori üzleti adattípusok.  Olvassa el az [adattípusok][] listáját a támogatott és a [nem támogatott adattípusok][].  Az [adattípusok][] a cikk a lekérdezés [nem támogatott adattípusok][]azonosítása is tartalmaz.  Az adattípusok konvertáláskor feltétlenül nézze meg az [adattípus gyakorlati tanácsokat][].

## <a name="next-steps"></a>Következő lépések
Miután sikeresen áttelepített az adatbázis sémája SQL adatraktár, folytassa a következő cikkek egyikét:

- [Az adatok áttelepítése][]
- [A kód áttelepítése][]

SQL-adatraktár gyakorlati tanácsokat olvashat a [Gyakorlati tanácsok][] témakört is.

<!--Image references-->

<!--Article references-->
[A kód áttelepítése]: ./sql-data-warehouse-migrate-code.md
[Az adatok áttelepítése]: ./sql-data-warehouse-migrate-data.md
[ajánlott eljárások]: ./sql-data-warehouse-best-practices.md
[táblázat áttekintése]: ./sql-data-warehouse-tables-overview.md
[nem támogatott táblázatszolgáltatások]: ./sql-data-warehouse-tables-overview.md#unsupported-table-features
[adattípusok]: ./sql-data-warehouse-tables-data-types.md
[nem támogatott adattípusai]: ./sql-data-warehouse-tables-data-types.md#unsupported-data-types
[adattípus ajánlott eljárások]: ./sql-data-warehouse-tables-data-types.md#data-type-best-practices

<!--MSDN references-->


<!--Other Web references-->
