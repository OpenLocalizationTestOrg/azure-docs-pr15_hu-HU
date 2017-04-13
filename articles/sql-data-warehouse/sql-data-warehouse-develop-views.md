<properties
   pageTitle="Az SQL adatraktár nézetek |} Microsoft Azure"
   description="Tippek a Transact-SQL nyelvben nézetek az Azure SQL-adatraktár, a megoldások fejlesztésére."
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
   ms.date="07/01/2016"
   ms.author="jrj;barbkess;sonyama"/>


# <a name="views-in-sql-data-warehouse"></a>Az SQL adatraktár nézetek

Nézetek különösen hasznosak az SQL adatraktár. Számos különböző módokon javíthatja minőségét a megoldás használható.  Ez a cikk néhány példa, hogy miként nézetek, sőt, figyelembe kell venni korlátozások a megoldás kiegészítése kiemeli.

> [AZURE.NOTE] Az alábbi `CREATE VIEW` nincs jelen cikkben ismertetett. Ezeket az információkat az MSDN webhelyen [NÉZET létrehozása][] cikke nyújt útmutatást.

## <a name="architectural-abstraction"></a>Építészeti hardverabsztrakciós
Gyakori alkalmazás mintát is hozza létre a táblázatokat hozzon létre táblázat szerint jelölje be (CTAS) objektum átnevezése mintát, miközben az adatok betöltése követ.

Az alábbi példában a dátum dimenzió rekordok új dátumot ad. Megjegyzés: hogyan új tabble, DimDate_New, először létre és majd átnevezett le szeretné cserélni a táblázat eredeti változatát.

```sql
CREATE TABLE dbo.DimDate_New
WITH (DISTRIBUTION = ROUND_ROBIN
, CLUSTERED INDEX (DateKey ASC)
)
AS
SELECT *
FROM   dbo.DimDate  AS prod
UNION ALL
SELECT *
FROM   dbo.DimDate_stg AS stg
;

RENAME OBJECT DimDate TO DimDate_Old;
RENAME OBJECT DimDate_New TO DimDate;

```

Ezt a megközelítést azonban kapott, és egy felhasználói nézet, valamint a "táblázat nem létezik" hibaüzenet eltűnik táblázatot eredményez. Nézetek a felhasználók biztosítson egységes bemutató réteg, miközben az egyes objektumok átnevezi használható. Azáltal, hogy a felhasználók a nézetek között az adatokhoz való hozzáférés, azt jelenti, hogy a felhasználók nem kell az alapul szolgáló táblák olvashatóságát. Ez a egységes felhasználói felületet nyújt, mialatt biztosítva, hogy az adatok raktári tervezők is is alapkoncepciójára az adatmodell teljesítmény maximalizálása az adatok betöltésének folyamat során CTAS használatával.    

## <a name="performance-optimization"></a>Az optimalizálása
Nézetek is meg lehessen őrizni a táblák közötti optimális teljesítményének illesztések fel lehet használni. Például az nézet is beépítése az egy felesleges terjesztési billentyűt az adatok mozgását minimalizálásához csatlakozó feltételeik részeként.  A lekérdezés adott vagy csatlakozó tipp kényszerítése nézet egy másik előnyeit lehet. Nézetek használata ily módon garantálja, hogy illesztések mindig történik meg a felhasználóknak, hogy ne feledje, hogy az illető illesztések a megfelelő szerkezet elkerülésére optimális módon.

## <a name="limitations"></a>Korlátozások
SQL-adatraktár nézetek csak a metaadatok.  Ezért a következő lehetőségek nem érhetők el:

-   Nincs séma kötés lehetőség
-   Alap táblák nem lehet frissíteni a nézet között
-   Nézetek nem hozható létre ideiglenes táblák keresztül
-   Nem támogatja az a KIBONTÁS / NOEXPAND tippek
-   SQL-adatraktár nem indexelt nézetek találhatók


## <a name="next-steps"></a>Következő lépések
Fejlesztési vonatkozó további tippek [SQL adatraktár fejlesztési áttekintése][]című témakörben találhat.
A `CREATE VIEW` szintaxis olvassa el [A NÉZET létrehozása][].

<!--Image references-->

<!--Article references-->
[SQL-adatraktár fejlesztése – áttekintés]: ./sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[NÉZET LÉTREHOZÁSA]: https://msdn.microsoft.com/en-us/library/ms187956.aspx

<!--Other Web references-->
