<properties
    pageTitle="A memóriában OLTP javítja SQL txn perf |} Microsoft Azure"
    description="Fogadja el a memóriában OLTP egy meglévő SQL-adatbázis tranzakció alapú teljesítmény javítása érdekében."
    services="sql-database"
    documentationCenter=""
    authors="jodebrui"
    manager="jhubbard"
    editor="MightyPen"/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="jodebrui"/>


# <a name="use-in-memory-oltp-preview-to-improve-your-application-performance-in-sql-database"></a>SQL-adatbázisban az alkalmazás teljesítményének javítása használata a memóriában OLTP (előzetes verzió)

[A memóriában OLTP](sql-database-in-memory.md) OLTP terhelést a [prémium](sql-database-service-tiers.md) Azure SQL-adatbázisait teljesítményének javítása a teljesítményszint növelése nélkül használható.

Kövesse ezeket a lépéseket követve fogadja el a memóriában OLTP a meglévő adatbázis.

## <a name="step-1-ensure-your-premium-database-supports-in-memory-oltp"></a>Lépés: 1: Győződjön meg róla, a prémium adatbázis támogatja a memóriában OLTP

A Skype 2015 vállalati November vagy újabb létrehozott prémium adatbázisok támogatja a memóriában funkciót. Annak megállapítása, hogy támogatja-e a prémium adatbázis a memóriában funkció az alábbi Transact-SQL-utasítás futtatásával. A memóriában támogatott 1 esetén a visszaadott eredmény (0):

```
SELECT DatabasePropertyEx(Db_Name(), 'IsXTPSupported');
```

*XTP* rövidítése *Szélső tranzakció feldolgozása*

Ha a meglévő adatbázist kell áthelyezni V12 prémium verzió új adatbázist, majd az adatok importálásához és exportálásához a következőket is használhatja.

#### <a name="export-steps"></a>Exportálási lépések

A gyártási adatbázis exportálása egy bacpac egyikét használja:

- Az [Exportálás](sql-database-export.md) funkciókat a [portálon](https://portal.azure.com/).

- Az **Exportálás adatok szintű alkalmazás** funkciók, a egy [naprakész SSMS.exe](http://msdn.microsoft.com/library/mt238290.aspx) (SQL Server Management Studio).
 1. Az **Objektum Explorer**bontsa ki az **adatbázisok** csomópontot.
 2. Kattintson a jobb gombbal az adatbázis csomópontot.
 3. Kattintson a **feladatok** > **adatok szintű alkalmazás exportálása**.
 4. A varázsló ablakában megjelenített működniük.


#### <a name="import-steps"></a>Az importálási lépéseket

A bacpac importálása a prémium verzió új adatbázist.

1. Az Azure- [portál](https://portal.azure.com/)
 - Nyissa meg a kiszolgáló.
 - Válassza az [Adatbázis importálása](sql-database-import.md) lehetőséget.
 - Jelölje ki az egy réteg árak prémium.

2. Importálhatja a bacpac SSMS használata:
 - Az **Objektum Intéző**kattintson a jobb gombbal az **adatbázisok** csomópontot.
 - Válassza az **Importálás adatok szintű alkalmazás**.
 - A varázsló ablakában megjelenített működnek.


## <a name="step-2-identify-objects-to-migrate-to-in-memory-oltp"></a>Lépés: 2: Azonosítsa a memóriában OLTP áttelepítése objektumok

SSMS tartalmazza, amelyek az aktív terhelést adatbázis futtatja **Tranzakció teljesítmény elemzése áttekintése** jelentés. A jelentés a táblák és a memóriában OLTP való áttérés jelölt tárolt eljárások azonosítja.

A SSMS, a jelentés létrehozásához:
- Az **Objektum Intéző**kattintson a jobb gombbal az adatbázis csomópontot.
- Kattintson a **jelentések** > **Standard Reports** > **tranzakció teljesítmény elemzése áttekintése**.

További tudnivalókért lásd: [Ha a táblázat vagy a tárolt eljárás kell használatát a memóriában OLTP meghatározása](http://msdn.microsoft.com/library/dn205133.aspx).


## <a name="step-3-create-a-comparable-test-database"></a>Lépés 3: Hasonló próba-adatbázis létrehozása

Tegyük fel, hogy a jelentés azt jelzi, hogy az adatbázis rendelkezik egy olyan táblát, alatt álló memória optimalizálása táblázattá alakított előnyös. Azt javasoljuk, hogy először tesztelje Ha teszteli a jelzés megerősítéséhez.

A gyártási adatbázis másolata próba van szüksége. A próba-adatbázist a tagolási szintre szolgáltatás réteg a termelési adatbázisként kell lennie.

Annak érdekében, hogy tesztelés, hogy az animáció működését a próba-adatbázis az alábbi képlettel történik:

1. A próba-adatbázis csatlakoztatása a SSMS használatával.

2. Az a (PILLANATKÉP) beállítás a lekérdezések erre szolgáló elkerülése érdekében az adatbázis-beállításra, ahogy az alábbi T-SQL-utasítás beállítása:
```
ALTER DATABASE CURRENT
    SET
        MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT = ON;
```


## <a name="step-4-migrate-tables"></a>Lépés: 4: Táblázatok áttelepítése

Létre kell hoznia és feltöltése a memória optimalizálása másolatot a vizsgálni kívánt táblázat. Létrehozhat vagy használatával:

- A praktikus memória optimalizálása varázsló SSMS.
- Kézi T-SQL nyelvben.


#### <a name="memory-optimization-wizard-in-ssms"></a>A memória optimalizálása varázsló a SSMS

Áttelepítési beállítás használata:

1. A próba adatbázis SSMS csatlakozni.

2. Az **Objektum Intéző**kattintson a jobb gombbal a táblázatra, és válassza a **Memória optimalizálása Advisor**.
 - A **Táblázat memória optimalizáló Advisor** varázsló jelenik meg.

3. Az a varázslóban kattintson a szeretné, ha a táblázat van-e bármilyen nem támogatott szolgáltatások által nem támogatott a memória optimalizálása táblázatokban **áttelepítési érvényességi** (vagy a **következő** gombra). További tudnivalókért lásd:
 - A *memória optimalizálása ellenőrzőlista* a [Memória optimalizálása Advisor](http://msdn.microsoft.com/library/dn284308.aspx).
 - A [transact-SQL nyelvben elem nem támogatják a memóriában OLTP](http://msdn.microsoft.com/library/dn246937.aspx).
 - [A memóriában OLTP áttelepítés](http://msdn.microsoft.com/library/dn247639.aspx).

4. Ha a táblázat nem nem támogatott funkciókat tartalmaz, a advisor végre tud hajtani a tényleges sémafájl és az adatok áttelepítése meg.


#### <a name="manual-t-sql"></a>Kétmintás T-SQL nyelvben kézi

Áttelepítési beállítás használata:

1. A próba-adatbázis csatlakoztatása a SSMS (vagy egy hasonló segédprogram) segítségével.

2. Szerezze be a teljes az SQL-T parancsfájl a táblázat és az indexeket.
 - A SSMS kattintson a jobb gombbal a táblázat csomópontot.
 - Kattintson a **tábla parancsfájl** > **Létrehozás** > **Új lekérdezés ablak**.

3. A parancsprogram ablakban adja meg WITH (MEMORY_OPTIMIZED = ON) a TÁBLÁZATOT LÉTREHOZNI nyilatkozatát.

4. Ha csoportosított index létrehozása, módosítása NONCLUSTERED.

5. Nevezze át a meglévő táblához SP_RENAME használatával.

6. A memória optimalizálása másolatot ment a tábla létrehozása a szerkesztett CREATE TABLE parancsfájl futtatásával.

7. Másolja az adatokat a memória optimalizálása táblázat használatával Beszúrás... VÁLASSZA A * BE:
    
```
INSERT INTO <new_memory_optimized_table>
        SELECT * FROM <old_disk_based_table>;
```


## <a name="step-5-optional-migrate-stored-procedures"></a>(Nem kötelező) 5 lépésben: tárolt eljárások áttelepítése

A memóriában funkció is módosíthat a tárolt eljárás a jobb teljesítmény elérése érdekében.


### <a name="considerations-with-natively-compiled-stored-procedures"></a>A natív lefordított tárolt eljárásokkal kapcsolatos szempontok

A natív módon lefordított tárolt eljárás kell rendelkeznie a az SQL-T a záradék a következő lehetőségek:

- NATIVE_COMPILATION

- SCHEMABINDING: azaz táblája, amelyek a tárolt eljárás nem a módosított befolyásolhatja a tárolt eljárás semmiképpen, kivéve, ha a tárolt eljárás leválaszt oszlop definíciókra.


A natív modul egy nagy [ATOMI blokkok](http://msdn.microsoft.com/library/dn452281.aspx) tranzakciók kezelése kell használnia. Nincs explicit MEGKEZDÉSÉHEZ TRANZAKCIÓ, illetve a VISSZAÁLLÍTÁS TRANZAKCIÓ szerepkör nem. Ha azt észleli, a kód üzleti szabály megsértése, azt is ér véget, [THROW](http://msdn.microsoft.com/library/ee677615.aspx) utasítással atomi tiltás.


### <a name="typical-create-procedure-for-natively-compiled"></a>A szokásos CREATE PROCEDURE natív módon lefordított

A T SQL natív módon lefordított tárolt eljárás létrehozása általában hasonlóan, mint a következő sablon:

```
CREATE PROCEDURE schemaname.procedurename
    @param1 type1, …
    WITH NATIVE_COMPILATION, SCHEMABINDING
    AS
        BEGIN ATOMIC WITH
            (TRANSACTION ISOLATION LEVEL = SNAPSHOT,
            LANGUAGE = N'your_language__see_sys.languages'
            )
        …
        END;
```

- A TRANSACTION_ISOLATION_LEVEL PILLANATKÉP esetében a natív módon lefordított tárolt eljárás a leggyakrabban előforduló számot. Azonban más értékeket egy részét szintén támogatottak:
 - MEGISMÉTELHETŐ OLVASÁSA
 - SZERIALIZÁLHATÓ


- A NYELV érték szerepel a sys.languages nézetben kell lennie.


### <a name="how-to-migrate-a-stored-procedure"></a>Hogy miként telepítheti át a tárolt eljárás

Az áttelepítési lépések a következők:


1. Szerezze be a normál parancsértelmezős tárolt eljárás a CREATE PROCEDURE parancsfájl.

2. A fejléc, az előző sablonban megfelelően újraírása.

3. Annak megállapítása, hogy a tárolt eljárás, T – SQL-kódot használ-e bármilyen natív módon lefordított tárolt eljárások nem támogatott funkciók. Ha szükséges, hajtja végre megoldásokat alkalmazhatja.
 - [Áttelepítési hibák tárolt eljárások natív módon lefordított](http://msdn.microsoft.com/library/dn296678.aspx)témakörben talál.

4. Nevezze át a régi tárolt eljárás SP_RENAME használatával. Vagy egyszerűen ENGEDJE el.

5. A szerkesztett létrehozása eljárás T-SQL nyelvben parancsfájl futtatásához.


## <a name="step-6-run-your-workload-in-test"></a>Lépés 6: Futtassa a terhelést tesztelése

Futtassa a terhelési a próba-adatbázisban, amely hasonlít az a terhelést a éles adatbázis futó. Ez célszerű jelenítse meg a teljesítmény nyereség megvalósítani a táblák és a tárolt eljárásokat a memóriában szolgáltatás használatát.

A terhelést a fő jellemzői a következők:

- Egyidejű kapcsolatok száma.

- Olvasási/írási arány.


Szabja testre, és futtassa a próba-terhelés, fontolja meg inkább a praktikus ostress.exe az [alábbi](sql-database-in-memory.md)illusztrált eszközben.


Hálózati késés minimalizálásához futtassa a próba a azonos Azure földrajzi régióban, az adatbázis csak tartalmazó.


## <a name="step-7-post-implementation-monitoring"></a>7 lépés: Megvalósítás utáni figyelése

Fontolja meg a memóriában megvalósítás gyártási teljesítmény hatásai ellenőrzése:

- [Monitor a memóriában tárhelyet](sql-database-in-memory-oltp-monitoring.md).

- [Figyelés Azure SQL-adatbázis kezelése dinamikus nézetek használata](sql-database-monitoring-with-dmvs.md)


## <a name="related-links"></a>Kapcsolódó hivatkozások

- [A memóriában OLTP (a memóriában optimalizálás)](http://msdn.microsoft.com/library/dn133186.aspx)

- [A natív lefordított tárolt eljárások – bevezetés](http://msdn.microsoft.com/library/dn133184.aspx)

- [A memória optimalizálása Advisor](http://msdn.microsoft.com/library/dn284308.aspx)

