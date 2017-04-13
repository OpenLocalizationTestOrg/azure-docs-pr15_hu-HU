<properties
   pageTitle="SQL-adatraktár tárolt eljárások |} Microsoft Azure"
   description="Ötletek a tárolt eljárásokat az Azure SQL-adatraktár megoldások fejlesztésével."
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
   ms.date="06/30/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="stored-procedures-in-sql-data-warehouse"></a>Az SQL adatraktár tárolt eljárások

SQL-adatraktár támogatja az SQL Server található Transact-SQL nyelvben szolgáltatásainak nagy része. Fontosabb skála ki fogja szeretnénk, hogy a megoldás teljesítményének maximalizálása kihasználhatja szolgáltatások vannak.

Azonban megőrzéséhez a méretezés és teljesítménye SQL adatraktár nem is néhány és viselkedési különbségek és mások által nem támogatott funkciók.

Ez a cikk ismerteti, hogyan tudnak megvalósítani a tárolt eljárásokat SQL adatraktár belül.

## <a name="introducing-stored-procedures"></a>Tárolt eljárások bemutatása
Tárolt eljárások nagyszerű módját az encapsulating az SQL-kódot; tárolja az adatokat a adatraktár közelébe. A kód encapsulating be kezelhető-mennyiség szerint tárolt eljárások súgó fejlesztők modularize megoldásukat; nagyobb re-usability kód megkönnyítése. Minden egyes tárolt eljárás is elfogadhatja, hogy még több rugalmas paramétereket.

SQL-adatraktár tartalmaz egy egyszerűsített és egyesíti a tárolt eljárás végrehajtása. A legnagyobb és összehasonlítása az SQL Server különbség, hogy a tárolt eljárás nem előre lefordított kódot. Adatok raktáron belüli vagyunk általában kisebb érintett, a fordítás idővel. Több fontos, hogy a tárolt eljárás kódot megfelelően optimalizálni, amikor nagy mennyiségű adaton ellen működő. A cél, órák, percek és másodpercek nem ezredmásodperc mentéséhez. Éppen ezért további hasznos tekintsen úgy tárolt eljárások, mint az SQL-logika tárolók.     

Ha SQL adatraktár végrehajtja a tárolt eljárás az SQL-utasítások elemzésének, lefordított és optimalizált futásidőben. A folyamat során minden utasítás konvertálja megosztott lekérdezéseket. Az SQL-kódot az adatok ellen ténylegesen végrehajtott eltér a lekérdezés elküldése.

## <a name="nesting-stored-procedures"></a>A beágyazási tárolt eljárások
Ha tárolt eljárások más tárolt eljárások hívás vagy dinamikus SQL-utasítás végrehajtása, majd a belső tárolt eljárás, illetve a kód hívás van said lehet eltávolítani.

SQL-adatraktár 8 beágyazási szintek maximális támogatja. Az SQL Server némileg eltérő. Az SQL Server fészekből szintje 32.

A felső szintű tárolt eljárás hívás megfelel a függvények egymásba 1-es szint

```sql
EXEC prc_nesting
```
Ha a tárolt eljárás is lehetővé teszi a hívás másik végrehajtási majd a művelet növeli a fészekből szintjét 2
```sql
CREATE PROCEDURE prc_nesting
AS
EXEC prc_nesting_2  -- This call is nest level 2
GO
EXEC prc_nesting
```
Ha a második eljárás végrehajtja néhány dinamikus sql, majd a művelet növeli a fészekből szint 3
```sql
CREATE PROCEDURE prc_nesting_2
AS
EXEC sp_executesql 'SELECT 'another nest level'  -- This call is nest level 2
GO
EXEC prc_nesting
```

Megjegyzés: az SQL-adatraktár jelenleg nem támogatja a @@NESTLEVEL. Meg kell nyomon követhető a a fészekből szint saját magának. Valószínű, 8 fészekből szintű vonatkozó fog gombra kattint, de ha mégis szüksége lesz újra a kód és a "összeolvasztása" azt, hogy ne haladják ezt a korlátot.

## <a name="insertexecute"></a>BESZÚRÁS. VÉGREHAJTÁSA
SQL-adatraktár nem teszi lehetővé az eredményhalmaz-utasítást a tárolt eljárás használhatnak. Azonban nem használhat helyettesítő megközelítés.

Olvassa el a következő cikk [ideiglenes táblák] ennek módjáról a példát.

## <a name="limitations"></a>Korlátozások

Vannak olyan Transact-SQL nyelvben tárolt eljárások SQL adatraktár nem végrehajtott egyes szempontjait.

Ezek a következők:

- ideiglenes tárolt eljárások
- számozott tárolt eljárások
- kiterjesztett tárolt eljárások
- A CLR tárolt eljárások
- titkosítási beállítás
- replikációs beállítás
- táblázat értékű paraméterei
- csak olvasható paraméterei
- alapértelmezett paraméterei
- végrehajtási környezetek
- vissza a utasítás

## <a name="next-steps"></a>Következő lépések
Fejlesztési vonatkozó további tippek [fejlesztési áttekintése][]című témakörben találhat.

<!--Image references-->

<!--Article references-->
[ideiglenes táblák]: ./sql-data-warehouse-tables-temporary.md#modularizing-code
[fejlesztési – áttekintés]: ./sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[nest level]: https://msdn.microsoft.com/library/ms187371.aspx

<!--Other Web references-->
