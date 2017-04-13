<properties
    pageTitle="Jelölje be a sorok (Nyújtás adatbázis) szűrő függvény segítségével az áttelepítendő |} Microsoft Azure"
    description="Megtudhatja, hogy miként jelölje be a sorok szűrése függvény segítségével az áttelepítendő."
    services="sql-server-stretch-database"
    documentationCenter=""
    authors="douglaslMS"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-server-stretch-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/28/2016"
    ms.author="douglasl"/>

# <a name="select-rows-to-migrate-by-using-a-filter-function-stretch-database"></a>Jelölje be a sorok áttelepítendő függvénnyel szűrő (Nyújtás adatbázis)

Ha külön táblában hideg adatokat tárolja, beállíthatja a Nyújtás adatbázis áttelepítése a teljes táblázatot. Ha a táblázat hideg- és melegvíz adatokat tartalmaz, azonban, megadhatja a filter függvény, jelölje be a sorokat, az áttelepítendő. A szűrő predikátumok egy beágyazott táblázat\-értékelni függvény. Ez a témakör ismerteti, hogyan kell írni egy beágyazott táblázat\-értékelni, jelölje be a sorok áttelepítendő függvény.

>   [AZURE.NOTE] Ha megad egy filter függvény, amely rosszul hajt végre, adatok áttelepítése is hajt végre rosszul. Kitöltés adatbázis vonatkozik a filter függvény a táblázat az idegen alkalmazása operátor használatával

A filter függvény nem ad meg, ha a teljes táblázatot áttelepítése.

A engedélyezése adatbázis Nyújtás varázsló futtatásakor áttelepítheti a teljes táblázat, vagy egy egyszerű szűrő függvény adhatja meg a varázslóban. Ha szeretne egy másik típusú filter függvény használatával jelölje ki az áttelepítendő sorokat, az alábbi lehetőség közül.

-   Lépjen ki a varázsló, és futtassa az ALTER TABLE utasítás Nyújtás ahhoz a táblához, és adja meg a szűrő függvényt.

-   Futtassa a ALTER TABLE utasítás után a kilépéshez szűrő függvényt adhatja meg.

ALTER TABLE szintaxisát függvény hozzáadása a témakör későbbi leírása.

## <a name="basic-requirements-for-the-filter-function"></a>A filter függvény alapvető követelményei
A beágyazott táblázat\-egy Nyújtás adatbázis szűrő predikátumok szükséges értéket tartalmazó függvény a következő példa néz ki.

```tsql
CREATE FUNCTION dbo.fn_stretchpredicate(@column1 datatype1, @column2 datatype2 [, ...n])
RETURNS TABLE
WITH SCHEMABINDING
AS
RETURN  SELECT 1 AS is_eligible
        WHERE <predicate>
```
A függvény a paraméterek kell lennie a táblázat oszlopainak azonosítóját.

Séma kötés, hogy a kihagyott és módosítani a filter függvény által használt oszlopok szükséges.

### <a name="return-value"></a>Visszatérési érték
Ha a függvény eredménye nem\-üres eredményt, a sor az áttelepítendő jogosult. Egyéb esetben \- Ez azt jelenti, hogy ha a függvény nem eredményezhet \- nem jogosult a telepíthető át a sort.

### <a name="conditions"></a>Feltételek
A &lt; *predikátumok* &gt; egyetlen feltétel esetén, illetve a tartományhoz a AND logikai operátorral több feltételt tartalmazhat.

```
<predicate> ::= <condition> [ AND <condition> ] [ ...n ]
```
Minden feltétel viszont állhat egy egyszerű feltétel, illetve a tartományhoz a logikai vagy operátorral több egyszerű feltételt.

```
<condition> ::= <primitive_condition> [ OR <primitive_condition> ] [ ...n ]
```

### <a name="primitive-conditions"></a>Egyszerű feltételek
Egy egyszerű feltétel lehetőségek közül választhat az alábbi összehasonlítása.

```
<primitive_condition> ::=
{
<function_parameter> <comparison_operator> constant
| <function_parameter> { IS NULL | IS NOT NULL }
| <function_parameter> IN ( constant [ ,...n ] )
}
```

-   Hasonlítsa össze a konstans kifejezésben függvény paramétert. Ha például `@column1 < 1000`.

    Íme egy példa, hogy annak vizsgálata, hogy egy *dátumot* tartalmazó oszlopból értékének &lt; 1/1/2016-ban.

    ```tsql
    CREATE FUNCTION dbo.fn_stretchpredicate(@column1 datetime)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE @column1 < CONVERT(datetime, '1/1/2016', 101)
    GO

    ALTER TABLE stretch_table_name SET ( REMOTE_DATA_ARCHIVE = ON (
        FILTER_PREDICATE = dbo.fn_stretchpredicate(date),
        MIGRATION_STATE = OUTBOUND
    ) )
    ```

-   Az IS NULL vagy nem NULL operátor alkalmazása függvény paraméter.

-   Az IN operátor segítségével összehasonlíthatja a függvény paraméteres állandók listáját.

    Íme egy példa, amely ellenőrzi, hogy értékének egy *szállítmányok\_állapot* oszlop `IN (N'Completed', N'Returned', N'Cancelled')`.

    ```tsql
    CREATE FUNCTION dbo.fn_stretchpredicate(@column1 nvarchar(15))
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE @column1 IN (N'Completed', N'Returned', N'Cancelled')
    GO

    ALTER TABLE table1 SET ( REMOTE_DATA_ARCHIVE = ON (
        FILTER_PREDICATE = dbo.fn_stretchpredicate(shipment_status),
        MIGRATION_STATE = OUTBOUND
    ) )
    ```

### <a name="comparison-operators"></a>Összehasonlító operátorok
Az alábbi összehasonlító operátorok használhatók.

`<, <=, >, >=, =, <>, !=, !<, !>`

```
<comparison_operator> ::= { < | <= | > | >= | = | <> | != | !< | !> }
```

### <a name="constant-expressions"></a>Állandó kifejezések
A tömbállandók szűrő függvényt használó lehet bármely mérvadó kifejezés, amely a függvény megadásakor kiértékelhető. Állandó kifejezések a következő műveleteket is tartalmazhat.

-   Literálok. Ha például `N’abc’, 123`.

-   A kifejezések algebrai. Ha például `123 + 456`.

-   Mérvadó funkciók. Ha például `SQRT(900)`.

-   Mérvadó Dokumentumkonvertálás CAST vagy konvertálás használó. Ha például `CONVERT(datetime, '1/1/2016', 101)`.

### <a name="other-expressions"></a>Egyéb kifejezéseket
A BETWEEN és nem a BETWEEN operátor, használhatja, ha az eredményül kapott függvény megfelel-e az itt leírt a BETWEEN cseréje után, és nem a megfelelő és és vagy kifejezések szereplők közötti szabályokat.

Nem használhatja segédlekérdezések vagy nem\-mérvadó funkciókat, például RAND() vagy GETDATE().

## <a name="add-a-filter-function-to-a-table"></a>A filter függvény hozzáadása táblához
A filter függvény hozzáadása egy táblához az **ALTER TABLE** utasítás fut, és egy meglévő beágyazott táblázat megadásával\-függvény értékelni, a program a **szűrő\_PREDIKÁTUMALAKZAT** paraméter. Példa:

```tsql
ALTER TABLE stretch_table_name SET ( REMOTE_DATA_ARCHIVE = ON (
    FILTER_PREDICATE = dbo.fn_stretchpredicate(column1, column2),
    MIGRATION_STATE = <desired_migration_state>
) )
```
Miután a függvény a táblázatra, mint egy predikátumok kötést létrehozni, a következő műveleteket teljesül.

-   A következő alkalommal adatok áttelepítése fordul elő, csak azokat a sorokat, amelynek a függvény nem ad\-áttelepítése üres érték.

-   A függvény által használt oszlopok kötött séma. Ezek az oszlopok nem változtathatja meg, feltéve, hogy egy tábla a szűrő predikátumok a függvény a használja.

A beágyazott táblázat nem leválaszt\-függvény értékelni, feltéve, hogy egy tábla a szűrő predikátumok a függvény a használja.

>   [AZURE.NOTE] A filter függvény, a teljesítmény javítása érdekében a függvény által használt oszlopok index létrehozása.

### <a name="passing-column-names-to-the-filter-function"></a>A filter függvény oszlopnevek átadása
A filter függvény rendel egy táblázat, adja meg az oszlopnevek átadni a filter függvény egy része néven. Ha adja meg a neveket három részből álló nevét, ha az oszlopban adja meg a Nyújtás későbbi lekérdezéseket\-engedélyezett táblázat sikertelen lesz.

Például három részből álló az oszlop nevét az alábbi példában látható módon adja meg, ha a kimutatás sikeresen elindul, de a táblázat későbbi lekérdezéseket sikertelen lesz.

```tsql
ALTER TABLE SensorTelemetry
  SET ( REMOTE_DATA_ARCHIVE = ON (
    FILTER_PREDICATE=dbo.fn_stretchpredicate(dbo.SensorTelemetry.ScanDate),
    MIGRATION_STATE = OUTBOUND )
  )
```

Ehelyett adja meg a filter függvény egy része az oszlop nevét az alábbi példában látható módon.

```tsql
ALTER TABLE SensorTelemetry
  SET ( REMOTE_DATA_ARCHIVE = ON  (
    FILTER_PREDICATE=dbo.fn_stretchpredicate(ScanDate),
    MIGRATION_STATE = OUTBOUND )
  )
```

## <a name="addafterwiz"></a>A filter függvény hozzáadása a varázsló futtatása után  

Ha azt szeretné, hogy nem hozható létre a **Nyújtás engedélyezése adatbázis** varázsló függvény használata, futtatását is lehetővé teszi a ALTER TABLE utasítás függvényt adhatja meg, miután a kilépéshez. Mielőtt függvény alkalmazhat, azonban van leállítása az adatok áttelepítés, amely már folyamatban van, és vissza áttelepített adatokat vihet. (További tudnivalókat a miért erre akkor szükség, lásd: [cseréje egy meglévő filter függvény](#replacePredicate).  

1. Áttelepítési irányának megfordítása, és jelenítse meg az adatok már áttelepített vissza. Ez a művelet nem lehet visszavonni, követően kezdődik. Kimenő adatátviteli is merülnek Azure költségek \(kilépési\). További információt a olvassa el a [hogyan Azure árak works](https://azure.microsoft.com/pricing/details/data-transfers/)című témakört.  

    ```tsql  
    ALTER TABLE <table name>  
         SET ( REMOTE_DATA_ARCHIVE ( MIGRATION_STATE = INBOUND ) ) ;   
    ```  

2. Várja meg az áttelepítési befejezéséhez. Ellenőrizheti, hogy az állapot **Nyújtás adatbázis Monitor** az SQL Server Management Studio, vagy a **sys.dm_db_rda_migration_status** nézetben is lekérdezhetők. További információt a című témakörben talál [Monitor és problémáinak megoldása adatok áttelepítése](sql-server-stretch-database-monitor.md) vagy [sys.dm_db_rda_migration_status](https://msdn.microsoft.com/library/dn935017.aspx).  

3. A filter függvény, amelyet alkalmazni a tábla létrehozása  

4. A függvény elhelyezése a táblázatban, és indítsa újra az Azure adatainak áttelepítése.  

    ```tsql  
    ALTER TABLE <table name>  
        SET ( REMOTE_DATA_ARCHIVE  
            (           
                FILTER_PREDICATE = <predicate>,  
                MIGRATION_STATE = OUTBOUND  
            )  
        );   
    ```  

## <a name="filter-rows-by-date"></a>A sorok szűrése dátum szerint
Az alábbi példa a sorok, ahol a **dátum** oszlopból értéket tartalmaz 2016 január 1-nél korábbi áttelepíti.

```tsql
-- Filter by date
--
CREATE FUNCTION dbo.fn_stretch_by_date(@date datetime2)
RETURNS TABLE
WITH SCHEMABINDING
AS
       RETURN SELECT 1 AS is_eligible WHERE @date < CONVERT(datetime2, '1/1/2016', 101)
GO
```

## <a name="filter-rows-by-the-value-in-a-status-column"></a>Az Állapot oszlop értéke szerint sorok szűrése
A következő példa áttelepíti sorok, ahol a az **állapot** oszlopban a megadott értékek egyikét tartalmazza.

```tsql
-- Filter by status column
--
CREATE FUNCTION dbo.fn_stretch_by_status(@status nvarchar(128))
RETURNS TABLE
WITH SCHEMABINDING
AS
       RETURN SELECT 1 AS is_eligible WHERE @status IN (N'Completed', N'Returned', N'Cancelled')
GO
```

## <a name="filter-rows-by-using-a-sliding-window"></a>A sorok szűrése csúsztatható ablak használatával
Sorok szűrése egy csúszó ablakban, tartsa szem előtt a a filter függvény vonatkozó alábbi követelményeket.

-   A függvény nem mérvadó lehet. Egy funkció, amely automatikusan újraszámítja a csúszó ablak idő eltelte, ezért nem hozható létre.

-   A függvény a séma kötés használja. Ezért nem lehet egyszerűen frissíteni a függvényben "hely" mindennap áthelyezése a csúszó ablak **ALTER FÜGGVÉNY** hívásával.

Nyisson meg egy filter függvény, például a következő példa, amely áttelepíti sorok, ahol a **systemEndTime** oszlop értéke 2016 január 1-nél korábbi.

```tsql
CREATE FUNCTION dbo.fn_StretchBySystemEndTime20160101(@systemEndTime datetime2)
RETURNS TABLE
WITH SCHEMABINDING  
AS  
RETURN SELECT 1 AS is_eligible
  WHERE @systemEndTime < CONVERT(datetime2, '2016-01-01T00:00:00', 101) ;
```

A filter függvény alkalmazása a táblázatra.

```tsql
ALTER TABLE <table name>
SET (
        REMOTE_DATA_ARCHIVE = ON
                (
                        FILTER_PREDICATE = dbo.fn_StretchBySystemEndTime20160101 (SysEndTime)
                                , MIGRATION_STATE = OUTBOUND
                )
        )
;
```

Ha meg szeretné frissíteni a csúszó ablak, végezze el a következő műveleteket.

1.  Hozzon létre egy új függvény, amely meghatározza az új csúsztatható ablak. A következő példa kijelölése a január 2, 2016, hanem 2016 január 1-nél korábbi dátumokat.

2.  Cserélje ki az előző filter függvény az újba hívó **ALTER TABLE**, az alábbi példában látható módon.

3. Tetszés szerint engedje el a korábbi filter függvény, amelyet már nem használ hívó **HÚZHATJA FÜGGVÉNY**. (Ez a lépés nem jelenik meg a példában.)

```tsql
BEGIN TRAN
GO
        /*(1) Create new predicate function definition */
        CREATE FUNCTION dbo.fn_StretchBySystemEndTime20160102(@systemEndTime datetime2)
        RETURNS TABLE
        WITH SCHEMABINDING
        AS
        RETURN SELECT 1 AS is_eligible
               WHERE @systemEndTime < CONVERT(datetime2,'2016-01-02T00:00:00', 101)
        GO

        /*(2) Set the new function as the filter predicate */
        ALTER TABLE <table name>
        SET
        (
               REMOTE_DATA_ARCHIVE = ON
               (
                       FILTER_PREDICATE = dbo.fn_StretchBySystemEndTime20160102(SysEndTime),
                       MIGRATION_STATE = OUTBOUND
               )
        )
COMMIT ;
```

## <a name="more-examples-of-valid-filter-functions"></a>További példákat érvényes szűrő függvények

-   A következő példa egyesíti a két egyszerű feltételek szerint és logikai operátorral.

    ```tsql
    CREATE FUNCTION dbo.fn_stretchpredicate((@column1 datetime, @column2 nvarchar(15))
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
      WHERE @column1 < N'20150101' AND @column2 IN (N'Completed', N'Returned', N'Cancelled')
    GO

    ALTER TABLE table1 SET ( REMOTE_DATA_ARCHIVE = ON (
        FILTER_PREDICATE = dbo.fn_stretchpredicate(date, shipment_status),
        MIGRATION_STATE = OUTBOUND
    ) )
    ```

-   Az alábbi példában több feltétel és a mérvadó konvertálás konvertálja.

    ```tsql
    CREATE FUNCTION dbo.fn_stretchpredicate_example1(@column1 datetime, @column2 int, @column3 nvarchar)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
        WHERE @column1 < CONVERT(datetime, '1/1/2015', 101)AND (@column2 < -100 OR @column2 > 100 OR @column2 IS NULL)AND @column3 IN (N'Completed', N'Returned', N'Cancelled')
    GO
    ```

-   Az alábbi példában matematikai operátorok és funkciók.

    ```tsql
    CREATE FUNCTION dbo.fn_stretchpredicate_example2(@column1 float)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE @column1 < SQRT(400) + 10
    GO
    ```

-   Az alábbi példában a BETWEEN és nem a BETWEEN operátor. A használati függvény érvényes, mivel az eredményül kapott függvény megfelel-e az itt leírt a BETWEEN cseréje után, és nem a megfelelő és és vagy kifejezések szereplők közötti szabályokat.

    ```tsql
    CREATE FUNCTION dbo.fn_stretchpredicate_example3(@column1 int, @column2 int)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE @column1 BETWEEN 0 AND 100
                AND (@column2 NOT BETWEEN 200 AND 300 OR @column1 = 50)
    GO
    ```
    Az előző függvény megegyezik az alábbi függvény a BETWEEN és nem a BETWEEN operátor egyenértékű és és vagy kifejezés cseréje után.

    ```tsql
    CREATE FUNCTION dbo.fn_stretchpredicate_example4(@column1 int, @column2 int)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE @column1 >= 0 AND @column1 <= 100AND (@column2 < 200 OR @column2 > 300 OR @column1 = 50)
    GO
    ```

## <a name="examples-of-filter-functions-that-arent-valid"></a>Példák, amelyek nem érvényes szűrőfüggvények

-   A következő függvény nem érvényes, mert nem tartalmaz\-mérvadó átalakítás.

    ```tsql
    CREATE FUNCTION dbo.fn_example5(@column1 datetime)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE @column1 < CONVERT(datetime, '1/1/2016')
    GO
    ```

-   A következő függvény nem érvényes, mert nem tartalmaz\-mérvadó függvényhívásban.

    ```tsql
    CREATE FUNCTION dbo.fn_example6(@column1 datetime)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE @column1 < DATEADD(day, -60, GETDATE())
    GO
    ```

-   A következő függvény nem érvényes, mert a művelethez a segédlekérdezésben tartalmaz.

    ```tsql
    CREATE FUNCTION dbo.fn_example7(@column1 int)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE @column1 IN (SELECT SupplierID FROM Supplier WHERE Status = 'Defunct'))
    GO
    ```

-   A következő funkciók nem érvényes mivel algebrai operátorok vagy beépített kifejezésekre\-függvényekben kiértékelésének eredménye csak állandó megadásakor a függvény. Oszlophivatkozásokat algebrai kifejezésekben vagy a hívás függvény nem lehet.

    ```tsql
    CREATE FUNCTION dbo.fn_example8(@column1 int)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE @column1 % 2 =  0
    GO

    CREATE FUNCTION dbo.fn_example9(@column1 int)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE SQRT(@column1) = 30
    GO
    ```

-   A következő függvény nem érvényes, mert azt sérti után a BETWEEN operátor helyettesítése a AND függvénnyel egyenértékű kifejezés az itt ismertetett szabályokat.

    ```tsql
    CREATE FUNCTION dbo.fn_example10(@column1 int, @column2 int)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE (@column1 BETWEEN 1 AND 200 OR @column1 = 300) AND @column2 > 1000
    GO
    ```
    Az előző függvény akkor a következő függvény egyenértékű a BETWEEN operátor helyettesítése a AND függvénnyel egyenértékű kifejezés. Ez a funkció nem érvényes, mert egyszerű feltételek csak a vagy logikai operátorok használhatók.

    ```tsql
    CREATE FUNCTION dbo.fn_example11(@column1 int, @column2 int)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE (@column1 >= 1 AND @column1 <= 200 OR @column1 = 300) AND @column2 > 1000
    GO
    ```

## <a name="how-stretch-database-applies-the-filter-function"></a>Hogyan a Nyújtás adatbázis alkalmazza a filter függvény
Nyújtás adatbázis a filter függvény vonatkozik a táblázatot, és a KERESZTHIVATKOZÁS alkalmazása operátor segítségével határozza meg, hogy jogosult sorokat. Példa:

```tsql
SELECT * FROM stretch_table_name CROSS APPLY fn_stretchpredicate(column1, column2)
```
Ha a függvény eredménye nem\-üres sor, a sor eredménye jogosult a telepíthető át.

## <a name="replacePredicate"></a>Cserélje le egy meglévő filter függvény
Egy korábban megadott szűrő függvény lecseréléséhez ismét a **ALTER TABLE** utasítás fut, és az új érték megadása a **szűrő\_PREDIKÁTUMALAKZAT** paraméter. Példa:

```tsql
ALTER TABLE stretch_table_name SET ( REMOTE_DATA_ARCHIVE = ON (
    FILTER_PREDICATE = dbo.fn_stretchpredicate2(column1, column2),
    MIGRATION_STATE = <desired_migration_state>
```
Az új beágyazott tábla\-értékelni függvény rendelkezik az alábbi követelményeket.

-   Az új függvénnyel nem lehet kisebb korlátozásokat alkalmaz, mint az előző függvény.

-   Az operátorok a régi függvény visszaállításához léteznie kell az új függvénnyel.

-   Az új függvénnyel operátorok, amelyek nem szerepelnek a régi függvény nem tartalmazhat.

-   Operátor argumentumok sorrendje nem módosítható.

-   Részét képező csak állandók egy `<, <=, >, >=` összehasonlító módosítható oly módon, hogy a kevésbé korlátozó lehetővé teszi a függvény.

### <a name="example-of-a-valid-replacement"></a>Példa egy érvényes helyettesítő
Feltételezik, hogy az alábbi függvény az aktuális szűrő predikátumok.

```tsql
CREATE FUNCTION dbo.fn_stretchpredicate_old (@column1 datetime, @column2 int)
RETURNS TABLE
WITH SCHEMABINDING
AS
RETURN  SELECT 1 AS is_eligible
        WHERE @column1 < CONVERT(datetime, '1/1/2016', 101)
            AND (@column2 < -100 OR @column2 > 100)
GO
```
A következő függvényt egy érvényes helyettesítő oka az új dátumot állandó (amely megadja egy újabb megadott dátum) a függvény kevésbé korlátozó teszi.

```tsql
CREATE FUNCTION dbo.fn_stretchpredicate_new (@column1 datetime, @column2 int)
RETURNS TABLE
WITH SCHEMABINDING
AS
RETURN  SELECT 1 AS is_eligible
        WHERE @column1 < CONVERT(datetime, '2/1/2016', 101)
            AND (@column2 < -50 OR @column2 > 50)
GO
```

### <a name="examples-of-replacements-that-arent-valid"></a>Példák a javaslatok nem érvényes
A következő függvény nem érvényes mert az új dátumot állandó (amely megadja egy korábban megadott dátum) nem a függvény kevésbé korlátozó.

```tsql
CREATE FUNCTION dbo.fn_notvalidreplacement_1 (@column1 datetime, @column2 int)
RETURNS TABLE
WITH SCHEMABINDING
AS
RETURN  SELECT 1 AS is_eligible
        WHERE @column1 < CONVERT(datetime, '1/1/2015', 101)
            AND (@column2 < -100 OR @column2 > 100)
GO
```
A következő függvény nem érvényes, mert a összehasonlító operátorok egyike el lett távolítva.

```tsql
CREATE FUNCTION dbo.fn_notvalidreplacement_2 (@column1 datetime, @column2 int)
RETURNS TABLE
WITH SCHEMABINDING
AS
RETURN  SELECT 1 AS is_eligible
        WHERE @column1 < CONVERT(datetime, '1/1/2016', 101)
            AND (@column2 < -50)
GO
```
A következő függvény nem érvényes, mert a AND logikai operátorral egy új feltétel hozzá lett adva.

```tsql
CREATE FUNCTION dbo.fn_notvalidreplacement_3 (@column1 datetime, @column2 int)
RETURNS TABLE
WITH SCHEMABINDING
AS
RETURN  SELECT 1 AS is_eligible
        WHERE @column1 < CONVERT(datetime, '1/1/2016', 101)
            AND (@column2 < -100 OR @column2 > 100)
            AND (@column2 <> 0)
GO
```

## <a name="remove-a-filter-function-from-a-table"></a>A filter függvény eltávolítása
Áttérés a kijelölt sorok helyett a teljes táblázatot, távolítsa el a meglévő függvény megadásával **szűrő\_PREDIKÁTUMALAKZAT** null értékre. Példa:

```tsql
ALTER TABLE stretch_table_name SET ( REMOTE_DATA_ARCHIVE = ON (
    FILTER_PREDICATE = NULL,
    MIGRATION_STATE = <desired_migration_state>
) )
```
Miután eltávolította a filter függvény, a táblázat összes sorának jogosultak az áttelepítésre. Tábla később szűrő függvény eredményt adja, kivéve, ha hozása vissza a távoli adatok el a táblázatot az Azure először nem adhat. Ezt a korlátozást létezik a helyzet, ahol, amelyek nem áttelepítési használatára jogosult, amikor egy új filter függvény a szükséges sorok már áttelepítette Azure elkerülése érdekében.

## <a name="check-the-filter-function-applied-to-a-table"></a>Jelölje be a táblázat alkalmazott szűrő függvény
A táblázat alkalmazott szűrő függvény ellenőrzéséhez nyissa meg a katalógus nézet **sys.remote\_adatok\_archív\_táblák** , és jelölje be a értékét a **szűrő\_predikátumok** oszlop. Ha az érték üres, a teljes táblázatot használatára jogosult az archiválás. További információt a című témakörben talál [sys.remote_data_archive_tables (Transact-SQL nyelvben)](https://msdn.microsoft.com/library/dn935003.aspx).

## <a name="security-notes-for-filter-functions"></a>Jegyzetek biztonsági szűrőfüggvények  
Db_owner jogosultságokkal rendelkező biztonságos fiókok a következő műveleteket hajthat végre.  

-   Hozzon létre, és táblázat értékű függvény, nagy mennyiségű kiszolgálói erőforrásokat fogyaszt vagy vár, így a szolgáltatás megtagadását hosszabb ideig alkalmazhat.  

-   Hozzon létre, és alkalmazhat a táblázat értékű függvény, amely lehetővé teszi annak előállítani egy táblát, amelynek a felhasználónak kifejezetten megtagadva olvasási hozzáférést tartalma alapján.  

## <a name="see-also"></a>Lásd még:

[ALTER TABLE (Transact-SQL)](https://msdn.microsoft.com/library/ms190273.aspx)
