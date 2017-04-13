<properties
   pageTitle="Az SQL adatraktár tranzakciók |} Microsoft Azure"
   description="Ötletek a tranzakciók az Azure SQL-adatraktár megoldások fejlesztésével."
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
   ms.date="07/31/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="transactions-in-sql-data-warehouse"></a>Az SQL adatraktár tranzakciók

Megfelelően működik, mint az SQL adatraktár tranzakciók támogatja az adatok raktári terhelés részeként. Azonban SQL adatraktár teljesítményének a méretezés karbantartása ahhoz bizonyos szolgáltatásai korlátozott SQL Server képest. Ez a cikk a különbségeket kiemeli, és megjeleníti a többi. 

## <a name="transaction-isolation-levels"></a>Transaction elkülönítési szintek
SQL-adatraktár savas tranzakciók hajtja végre. Azonban a tranzakció-támogatás elkülönítése korlátozódik `READ UNCOMMITTED` és ezt nem módosíthatók. Számos módszert megakadályozhatja, hogy az adatok dirty olvasás számunkra esetén coding alkalmazhat. A leggyakrabban használt módszerek szeretné megakadályozni, hogy a felhasználóknak, hogy továbbra is készített adatok lekérdezése kihasználhatja az CTAS és a táblázat partíciót váltás (csúsztatható ablak szerkezet gyakran ismert). Nézetek előre szűrheti az adatokat, akkor is a népszerű megközelítés.  

## <a name="transaction-size"></a>Transaction mérete
Módosítását álló adatkapcsolatú tranzakció mérete korlátozott. A korlát ma érvényes "egy"terjesztési. Az összes felosztás ezért kiszámítható a korlátot megszorozva az eloszlás száma. A közelítő a tranzakció sorainak maximális száma nullával való osztás a terjesztési kap minden egyes sorára teljes méretét. Változó hosszúságú oszlopokat érdemes megfontolni a maximális méret használata helyett egy oszlop átlagos hosszát véve.

A táblázat alatt a következő feltételezések esett:

* Egy egész terjesztési adatok történt. 
* Az átlag sor hossza 250 bájt

| [DWU][]    | Egy eloszlás (orrot) Cap | A felosztott száma | MAX tranzakció méret (orrot) | # Egy eloszlás sorok | A tranzakciók sorok maximális száma |
| ------ | -------------------------- | ----------------------- | -------------------------- | ----------------------- | ------------------------ |
| DW100  |  1                         | 60                      |   60                       |   4,000,000             |    240,000,000           |
| DW200  |  az 1,5                       | 60                      |   90                       |   6,000,000             |    360,000,000           |
| DW300  |  2,25                      | 60                      |  135                       |   9,000,000             |    540,000,000           |
| DW400  |  3                         | 60                      |  180                       |  12,000,000             |    720,000,000           |
| DW500  |  3,75                      | 60                      |  225                       |  15,000,000             |    900,000,000           |
| DW600  |  4.5.                       | 60                      |  270                       |  18,000,000             |  1,080,000,000           |
| DW1000 |  7.5                       | 60                      |  450                       |  30,000,000             |  1,800,000,000           |
| DW1200 |  9                         | 60                      |  540                       |  36,000,000             |  2,160,000,000           |
| DW1500 | 11,25                      | 60                      |  675                       |  45,000,000             |  2,700,000,000           |
| DW2000 | 15                         | 60                      |  900                       |  60,000,000             |  3,600,000,000           |
| DW3000 | 22.5                       | 60                      |  1,350                     |  90,000,000             |  5,400,000,000           |
| DW6000 | 45                         | 60                      |  2,700                     | 180,000,000             | 10,800,000,000           |

A tranzakciók által tranzakció vagy művelet eső érvényes. Nem érvényesül az összes egyidejű tranzakció keresztül. Ezért tranzakciók engedélyezve van a mennyiségű adat írni a napló. 

Optimalizálása és a napló írt adatok csökkentése érdekében olvassa el a [Gyakorlati tanácsok a tranzakciók][] cikk tartalmazza.

> [AZURE.WARNING] A tranzakció maximális mérete csak a kivonat lehet elérni, vagy az adatokat a várható esetén elosztott ROUND_ROBIN táblák esetleg. A tranzakció ír adatok nyomdai módon a felosztás majd a korlát valószínűleg előtt tranzakció maximális mérete érhető el.
<!--REPLICATED_TABLE-->

## <a name="transaction-state"></a>Tranzakció állapota
SQL-adatraktár a XACT_STATE() függvényt használja a -2 érték használata sikertelen tranzakció jelentést. Ez azt jelenti, hogy a tranzakció nem sikerült, és csak a visszaállítás van megjelölve

> [AZURE.NOTE] A -2 sikertelen tranzakció jelölésére a XACT_STATE függvény használatát az SQL Server másik viselkedés jelöli. Az SQL Server egy nem committable tranzakció jelenítik meg a -1 értéket használja. Az SQL Server elviseli tranzakción belül hibák anélkül, hogy committable, nem kell jelölni. Ha például `SELECT 1/0` volna hibát jelez, de egy nem committable állapotba tranzakció nem kötelező. Az SQL Server olvasás is lehetővé teszi a nem committable műveletben. SQL-adatraktár nem adhatja meg a művelet. Ha hiba lép fel egy SQL adatraktár tranzakción belül azt automatikusan beírja a-2 állapot, és nem fogja tudni, hogy minden további jelölje ki a kimutatások, mindaddig, amíg vissza lett állítva az utasítás. Ezért fontos, hogy ellenőrizze, hogy az alkalmazás kód megtekintéséhez, ha Ön használja XACT_STATE() szükséges módosításokat a kódot.

Ha például SQL Server-alapú jelenhetnek meg egy tranzakció, így néz ki:

```sql
SET NOCOUNT ON;
DECLARE @xact_state smallint = 0;

BEGIN TRAN
    BEGIN TRY
        DECLARE @i INT;
        SET     @i = CONVERT(INT,'ABC');
    END TRY
    BEGIN CATCH
        SET @xact_state = XACT_STATE();

        SELECT  ERROR_NUMBER()    AS ErrNumber
        ,       ERROR_SEVERITY()  AS ErrSeverity
        ,       ERROR_STATE()     AS ErrState
        ,       ERROR_PROCEDURE() AS ErrProcedure
        ,       ERROR_MESSAGE()   AS ErrMessage
        ;
        
        IF @@TRANCOUNT > 0
        BEGIN
            PRINT 'ROLLBACK';
            ROLLBACK TRAN;
        END

    END CATCH;

IF @@TRANCOUNT >0
BEGIN
    PRINT 'COMMIT';
    COMMIT TRAN;
END

SELECT @xact_state AS TransactionState;
```

Ha a kód feletti állapotában majd jelenik meg az alábbi hibaüzenet:

Üzenet 111233, Level 16, 1, 1 111233; sor állam Az aktuális tranzakció megszakadt, és minden függőben lévő változásokat közzétételének vissza. Okai lehetnek: Csak a visszaállítás állapotban tranzakció volt nem kifejezetten visszavonva DDL, DML vagy SELECT utasítás előtti.

Még nem jelennek meg a kimenet ERROR_ * függvények.

Az SQL adatraktár a kódot kell kissé változtatható meg:

```sql
SET NOCOUNT ON;
DECLARE @xact_state smallint = 0;

BEGIN TRAN
    BEGIN TRY
        DECLARE @i INT;
        SET     @i = CONVERT(INT,'ABC');
    END TRY
    BEGIN CATCH
        SET @xact_state = XACT_STATE();
        
        IF @@TRANCOUNT > 0
        BEGIN
            PRINT 'ROLLBACK';
            ROLLBACK TRAN;
        END

        SELECT  ERROR_NUMBER()    AS ErrNumber
        ,       ERROR_SEVERITY()  AS ErrSeverity
        ,       ERROR_STATE()     AS ErrState
        ,       ERROR_PROCEDURE() AS ErrProcedure
        ,       ERROR_MESSAGE()   AS ErrMessage
        ;
    END CATCH;

IF @@TRANCOUNT >0
BEGIN
    PRINT 'COMMIT';
    COMMIT TRAN;
END

SELECT @xact_state AS TransactionState;
```

Az elvárt viselkedés most figyelhető meg. A hiba a műveletben kezeli, és a függvények ERROR_ * adja meg a várt módon értékeket.

Minden megváltozott, hogy a `ROLLBACK` tranzakció volt fordulhat elő, az olvasás az a hiba adatait, mielőtt a `CATCH` blokk.

## <a name="errorline-function"></a>Error_Line() függvény
Érdemes is megjegyezni, hogy SQL adatraktár nem valósítja vagy támogatja a ERROR_LINE() függvényt. Ha ez a kód, meg kell, hogy megfeleljenek az SQL adatraktár eltávolítása. Használja a lekérdezés a feliratok a kód inkább megfelelő funkciók végrehajtásához. Olvassa el a [CÍMKÉT][] a cikk a szolgáltatásról további információt.

## <a name="using-throw-and-raiserror"></a>THROW és RAISERROR használatával
THROW emelése az SQL adatraktár kivételek korszerűbb végrehajtására, de RAISERROR is támogatott. Amelyek fizet a figyelmet azonban érdemes néhány különbségek vannak.

- Felhasználó által definiált számok THROW 100 000-150 000 tartományát nem lehet hibaüzenetek
- A 50 000 javított RAISERROR hibaüzenetek
- Sys.messages használata nem támogatott

## <a name="limitiations"></a>Limitiations
SQL-adatraktár van néhány egyéb korlátozások vonatkoznak a tranzakciók.

Ezek a következők:

- Nincs elosztott tranzakciók
- Nem engedélyezett tranzakciók egymásba ágyazása
- A program nem menti a megengedett pontok
- Névvel ellátott tranzakciók
- Nem jelölt tranzakciók
- Nem támogatja a DDL például `CREATE TABLE` belül a felhasználó által definiált tranzakció

## <a name="next-steps"></a>Következő lépések
További információk a tranzakciók optimalizálása, olvassa el a [Gyakorlati tanácsok a tranzakciók][]című témakört.  SQL-adatraktár gyakorlati tanácsokat című témakörben talál [SQL adatraktár gyakorlati tanácsokat][].

<!--Image references-->

<!--Article references-->
[DWU]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units
[development overview]: ./sql-data-warehouse-overview-develop.md
[Ajánlott eljárások a tranzakciók]: ./sql-data-warehouse-develop-best-practices-transactions.md
[SQL-adatraktár ajánlott eljárások]: ./sql-data-warehouse-best-practices.md
[CÍMKE]: ./sql-data-warehouse-develop-label.md

<!--MSDN references-->

<!--Other Web references-->
