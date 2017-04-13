<properties
   pageTitle="Az SQL-kódot áttelepítése SQL adatraktár |} Microsoft Azure"
   description="Tippek az áttelepítés az SQL-kódot az Azure SQL-adatraktár, a megoldások fejlesztésére."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/02/2016"
   ms.author="lodipalm;barbkess;sonyama;jrj"/>

# <a name="migrate-your-sql-code-to-sql-data-warehouse"></a>Az SQL-kódot áttelepítése SQL adatraktár

Ha egy másik adatbázist a kód áttérés SQL adatraktár, valószínűleg szüksége lesz módosításához a kód alap. SQL-adatraktár funkciókkal célja, hogy elosztott módon működik, jelentősen növelheti teljesítményét. Azonban a teljesítmény és a méretezés megtartásához egyes szolgáltatások nem érhetők el is.

## <a name="common-t-sql-limitations"></a>Gyakori az SQL-T korlátai

Az alábbiakban összefoglaljuk az leggyakoribb funkció, amely az Azure SQL-adatraktár nem támogatja. A hivatkozásokra kattintva a nem támogatott funkciók megoldásai:

- [A frissítések ANSI illesztések][]
- [ANSI illesztések törlésekről][]
- [Egyesítés utasítás][]
- adatbázis-határokon illesztések
- [kurzor][]
- [JELÖLJE KI. AZ][]
- [BESZÚRÁS. VÉGREHAJTÁSI][]
- kimeneti záradék
- beágyazott felhasználó által definiált függvények
- több elem utasítás függvények
- [gyakori táblázatkifejezésből](#Common-table-expressions)
- [rekurzív közös táblázatkifejezésből (CTE)] (#Recursive-common-table-expressions-(CTE)
- CLR funkciók és eljárások
- $partition függvény
- táblázat változók
- táblában értéket paraméterei
- az elosztott tranzakciók
- jóváhagyás / munka visszaállítása
- transaction mentése
- végrehajtási környezetek (végrehajtás AS)
- [Group by záradék összesítő / kocka / csoportosítási beállítások beállítása][]
- [8 túl beágyazási szintek száma][]
- [nézetek frissítése][]
- [Jelölje ki a változó hozzárendelés használata][]
- [nem MAX típusú adat dinamikus SQL-karakterlánc][]

A legtöbb e korlátozások Szerencsére dolgozhat körül. Magyarázatok a megfelelő fejlesztési cikkekben a fentiekben találhatók.

## <a name="supported-cte-features"></a>Támogatott CTE szolgáltatások

Gyakori táblázatkifejezésből (CTEs) részben az SQL adatraktár támogatott.  A következő CTE funkció jelenleg támogatott:

- A SELECT utasítás egy CTE adható meg.
- CREATE VIEW utasítás egy CTE adható meg.
- Egy CTE létrehozása táblában szerint jelölje be (CTAS) utasításokban adható meg.
- Egy CTE létrehozása távoli táblázat szerint jelölje be (CRTAS) utasításokban adható meg.
- Egy CTE adható meg a kimutatás létrehozása külső tábla szerint jelölje be (CETAS).
- Távoli táblázat a egy CTE hivatkozhat.
- Külső tábla egy CTE a hivatkozhat.
- Több CTE lekérdezés definíciók egy CTE definiálható.

## <a name="cte-limitations"></a>CTE korlátai

Gyakori táblázatkifejezésből SQL adatraktár beleértve néhány korlátozás van:

- Egy CTE egyetlen SELECT utasítás kell követnie. Beszúrás, frissítés, törlés és EGYESÍTÉS kimutatások nem működnek.
- A közös táblázatkifejezés, amely hivatkozásokat tartalmaznak, saját maga (rekurzív közös táblázatkifejezés) nem támogatott (lásd az alábbi szakaszt).
- Egy CTE megadása több záradék a nem engedélyezett. Például ha egy CTE_query_definition segédlekérdezés tartalmaz, a művelethez a segédlekérdezésben nem tartalmazhat egy beágyazott záradékot, amely definiálja egy másik CTE a.
- ORDER BY záradék nem használhatók a a CTE_query_definition, kivéve ha TOP záradékot meg van adva.
- Egy CTE egy köteg részét képező utasításokban használata esetén az előtte utasítás pontosvesszővel kell követnie.
- Sp_prepare által készített kimutatásokban használatakor a CTEs ugyanúgy, mint a többi SELECT utasítás a párhuzamos Adatraktárhoz fognak működni. Azonban CTEs sp_prepare által készített CETAS részeként használata esetén a vezérlő viselkedése is addig elhalasztani az SQL Server és a más PDW kimutatások kötés használható sp_prepare módja miatt. Lehetőséget VÁLASZTJA, hogy hivatkozások CTE használ a nem megfelelő oszlopban, amely nem létezik CTE, ha a sp_prepare továbbítja a probléma észlelésére nélkül, de a hiba lesz kell elő során sp_execute helyette.

## <a name="recursive-ctes"></a>Rekurzív CTEs

Az SQL adatraktár a rekurzív CTEs által nem támogatott.  Lehet, hogy a migraion a rekurzív CTE némileg teljes és a legjobb folyamat szeretné bontani a be több lépéseket. A szokásos ismétlődve használja, és feltölteni az ideiglenes táblázat, akkor ismétlés rekurzív köztes lekérdezések. Miután az ideiglenes tábla van töltve majd térhet vissza az adatok egyetlen eredményhalmaz. A hasonló megközelítés használt megoldása `GROUP BY WITH CUBE` [group by záradék összesítő / kocka / csoportosítási beállítások beállítása][] című témakörben.

## <a name="unsupported-system-functions"></a>A rendszer nem támogatott funkciók

Vannak bizonyos rendszer függvények által nem támogatott. A fő lehetőségekből általában Észreveheti adattárolási használt a következők:

- NEWSEQUENTIALID()
- @@NESTLEVEL()
- @@IDENTITY()
- @@ROWCOUNT()
- ROWCOUNT_BIG
- ERROR_LINE()

A problémák dolgozhat körül.

## <a name="rowcount-workaround"></a>@@ROWCOUNTmegoldás:

Kerülő támogatásának hiánya @@ROWCOUNT, létrehozása a tárolt eljárás, amely az utolsó sorok száma lekérése sys.dm_pdw_request_steps és hajthat végre `EXEC LastRowCount` DML-utasítás után.

```sql
CREATE PROCEDURE LastRowCount AS
WITH LastRequest as 
(   SELECT TOP 1    request_id
    FROM            sys.dm_pdw_exec_requests
    WHERE           session_id = SESSION_ID()
    AND             resource_class IS NOT NULL
    ORDER BY end_time DESC
),
LastRequestRowCounts as
(
    SELECT  step_index, row_count
    FROM    sys.dm_pdw_request_steps
    WHERE   row_count >= 0
    AND     request_id IN (SELECT request_id from LastRequest)
)
SELECT TOP 1 row_count FROM LastRequestRowCounts ORDER BY step_index DESC
;
```

## <a name="next-steps"></a>Következő lépések
Az összes támogatott T-SQL-utasítások listáját tanulmányozza a [Transact-SQL nyelvben témaköröket][].

<!--Image references-->

<!--Article references-->
[A frissítések ANSI illesztések]: ./sql-data-warehouse-develop-ctas.md#ansi-join-replacement-for-update-statements
[ANSI illesztések törlésekről]: ./sql-data-warehouse-develop-ctas.md#ansi-join-replacement-for-delete-statements
[Egyesítés utasítás]: ./sql-data-warehouse-develop-ctas.md#replace-merge-statements
[BESZÚRÁS. VÉGREHAJTÁSI]: ./sql-data-warehouse-tables-temporary.md#modularizing-code
[A Transact-SQL nyelvben témakörök]: ./sql-data-warehouse-reference-tsql-statements.md

[kurzor]: ./sql-data-warehouse-develop-loops.md
[JELÖLJE KI. AZ]: ./sql-data-warehouse-develop-ctas.md#selectinto
[Group by záradék összesítő / kocka / csoportosítási beállítások beállítása]: ./sql-data-warehouse-develop-group-by-options.md
[8 túl beágyazási szintek száma]: ./sql-data-warehouse-develop-transactions.md
[nézetek frissítése]: ./sql-data-warehouse-develop-views.md
[Jelölje ki a változó hozzárendelés használata]: ./sql-data-warehouse-develop-variable-assignment.md
[nem MAX típusú adat dinamikus SQL-karakterlánc]: ./sql-data-warehouse-develop-dynamic-sql.md

<!--MSDN references-->

<!--Other Web references-->
