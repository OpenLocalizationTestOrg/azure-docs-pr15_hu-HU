<properties
   pageTitle="Tervező döntéseket és az SQL adatraktár fejlesztési coding módszerek |} Microsoft Azure"
   description="Fejlesztési fogalmak, tervezés döntések, javaslatok és SQL adatraktár coding technikákat."
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
   ms.date="08/16/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="design-decisions-and-coding-techniques-for-sql-data-warehouse"></a>Tervező döntéseket és az SQL adatraktár coding módszerek

Nézze meg ezeket fejlesztési cikkekben a fő tervezési döntéseket, javaslatok és coding technikákat jobban megértheti az SQL adatraktár keresztül.

## <a name="key-design-decisions"></a>Fő tervezési döntések
Az alábbi cikkekben kiemelése néhány Alapfogalmak és tervezés döntések meg kell az SQL adatraktár használatával elosztott adatraktár fejlesztésének ismertetése:

- [kapcsolatok][]
- [feldolgozási][]
- [tranzakciók][]
- [felhasználó által definiált sémák][]
- [táblázat ki.][]
- [táblázat indexek][]
- [táblázat partíciót][]
- [CTAS][]
- [statisztika][]

## <a name="development-recommendations-and-coding-techniques"></a>Javaslatok és coding módszerek
Ezek a cikkek adott coding technikák, tippek és javaslatok a SQL adatraktár fejlesztésével kiemelése:

- [tárolt eljárás][]
- [címkék][]
- [nézetek][]
- [ideiglenes táblák][]
- [dinamikus SQL][]
- [a körkörös lejátszás][]
- [csoportosítási beállítások][]
- [változó hozzárendelés][]

## <a name="next-steps"></a>Következő lépések
Segítségével a fejlesztői cikkek, után nézze további információt a [Transact-SQL referencia][] lapon keresztül érhető támogatott szintaxis az SQL adatraktár.

<!--Image references-->

<!--Article references-->
[feldolgozási]: ./sql-data-warehouse-develop-concurrency.md
[kapcsolatok]: ./sql-data-warehouse-connect-overview.md
[CTAS]: ./sql-data-warehouse-develop-ctas.md
[dinamikus SQL]: ./sql-data-warehouse-develop-dynamic-sql.md
[csoportosítási beállítások]: ./sql-data-warehouse-develop-group-by-options.md
[címkék]: ./sql-data-warehouse-develop-label.md
[a körkörös lejátszás]: ./sql-data-warehouse-develop-loops.md
[statisztika]: ./sql-data-warehouse-tables-statistics.md
[tárolt eljárás]: ./sql-data-warehouse-develop-stored-procedures.md
[táblázat ki.]: ./sql-data-warehouse-tables-distribute.md
[táblázat indexek]: ./sql-data-warehouse-tables-index.md
[táblázat partíciót]: ./sql-data-warehouse-tables-partition.md
[ideiglenes táblák]: ./sql-data-warehouse-tables-temporary.md
[tranzakciók]: ./sql-data-warehouse-develop-transactions.md
[felhasználó által definiált sémák]: ./sql-data-warehouse-develop-user-defined-schemas.md
[változó hozzárendelés]: ./sql-data-warehouse-develop-variable-assignment.md
[nézetek]: ./sql-data-warehouse-develop-views.md
[A Transact-SQL referencia]: ./sql-data-warehouse-overview-reference.md

<!--MSDN references-->
[renaming objects]: https://msdn.microsoft.com/library/mt631611.aspx

<!--Other Web references-->
