<properties
   pageTitle="A megoldás áttelepítése SQL adatraktár |} Microsoft Azure"
   description="Áttérési útmutató a Azure SQL-adatraktár platform, hogy a megoldás."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="barbkess"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/30/2016"
   ms.author="barbkess;jrj;sonyama"/>

# <a name="migrate-your-solution-to-sql-data-warehouse"></a>A megoldás áttelepítése SQL adatraktár

SQL-adatraktár elosztott adatbázis rendszer elastically méretezze át a igényeknek megfelelően. SQL Server alkalmazásban nem az összes szolgáltatások teljesítmény és a méretezés megtartásához SQL adatraktár belül alkalmazza. A következő témakörök áttelepítési érintse meg az áttelepítés a megoldás SQL adatraktár főbb tényezők közül. Különböző tervezési mintázatok és így hagyományos megközelítés nem mindig a legjobb tervezése a skála adatraktárakhoz vezet be. Ezért előfordulhat, hogy a meglévő megoldás kiigazítása biztosít, akkor kihasználhatja teljes SQL adatraktár által biztosított a megosztott platform.

Is fontos ne feledje, hogy SQL adatraktár alapján a Microsoft Azure platform. Ezért a áttelepítési részét is tartalmazhatnak az adatok átvitele a felhőben. Adatátvitel saját jobb egy témát, és gondosan tekintendő; különösen növekedésével kötet. Adatátvitel és az adatok betöltésének különálló témaköröket.

## <a name="migration-guidance"></a>Áttérési útmutató

Ellenőrizze, hogy elolvasta – ezek a cikkek annak érdekében, hogy megismeri néhány alapvető fogalmak és termék különbségek megélénkült az áttelepítés előtt.

- [A séma áttelepítése][]
- [Az adatok áttelepítése][]
- [A kód áttelepítése][]

## <a name="next-steps"></a>Következő lépések

A MACSKA (ügyfél tanácsadó csoport) tartalmaz néhány nagyszerű – blogok közzétett SQL adatraktár útmutatást.  A cikk a [Azure SQL-adatraktár a gyakorlatban áttelepítése az adatokat][] az áttelepítési további útmutatást találhat.

<!--Image references-->

<!--Article references-->
[A séma áttelepítése]: sql-data-warehouse-migrate-schema.md
[Az adatok áttelepítése]: sql-data-warehouse-migrate-data.md
[A kód áttelepítése]: sql-data-warehouse-migrate-code.md


<!--MSDN references-->


<!--Other Web references-->
[A gyakorlatban Azure SQL-adatraktár áttelepítése adatok]: https://blogs.msdn.microsoft.com/sqlcat/2016/08/18/migrating-data-to-azure-sql-data-warehouse-in-practice/
