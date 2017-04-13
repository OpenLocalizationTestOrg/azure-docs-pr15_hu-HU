<properties
   pageTitle="A meglévő Azure SQL-adatraktár áttelepítése prémium tároló |} Microsoft Azure"
   description="Egy meglévő SQL adatraktár áttelepítésének prémium tárolóhoz utasítások"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="happynicolle"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/24/2016"
   ms.author="nicw;barbkess;sonyama"/>

# <a name="migration-to-premium-storage-details"></a>Áttelepítés az prémium tároló részletei
SQL-adatraktár nemrégiben [Prémium tároló nagyobb teljesítmény kiszámítható][]jelent meg.  Hogy jelenleg szabványos tárolón meglévő adatraktárakhoz prémium tárolóhoz áttelepítendő készen állnak.  Ha többet szeretne tudni, hogyan és mikor automatikus áttelepítések fordul elő, és hogyan önálló áttelepítéséhez, ha inkább szabályozhatja az állásidőt bekövetkezésekor Olvasson tovább.

Ha egynél több adatraktár, a az [áttelepítés az Automatikus ütemezés][] az alábbi használatával azt állapíthatja meg, amikor is áttelepítendőként.

## <a name="determine-storage-type"></a>Tárhely típusának megállapítása
Ha létrehozott egy DW előtt a dátumokat az alábbi, jelenleg a szabványos tárterületet használ.  Egyes adatraktár e, továbbá automatikus áttelepítése szabványos tárolón van értesítés a adatraktár a lap tetején az [Azure-portálra][] , amely szerint az "egy közelgő*frissítés prémium tárolóhoz szükség egy üzemszünetek.  Ismerje meg, hogy több ->*. "

| **Régió**          | **Ez a dátum előtt létrehozott DW**   |
| :------------------ | :-------------------------------- |
| Ausztrália keleti      | Még nem áll rendelkezésre prémium tárhely |
| Ausztrália Könyvesbolt is. | 2016 augusztus 5                    |
| Brazília Dél        | 2016 augusztus 5                    |
| Közép Kanadán      | 2016 május 25                      |
| Kanada-keleti         | 2016 május 26.                      |
| A központi Amerikai Egyesült Államok          | 2016 május 26.                      |
| Kína keleti          | Még nem áll rendelkezésre prémium tárhely |
| Kína Észak         | Még nem áll rendelkezésre prémium tárhely |
| Kelet-ázsiai           | 2016 május 25                      |
| Kelet-Amerikai Egyesült Államok             | 2016 május 26.                      |
| Kelet-US2            | 2016 május 27                      |
| India központi       | 2016 május 27                      |
| India Dél         | 2016 május 26.                      |
| India nyugati          | Még nem áll rendelkezésre prémium tárhely |
| Japán keleti          | 2016 augusztus 5                    |
| Japán nyugati          | Még nem áll rendelkezésre prémium tárhely |
| A központi Észak-amerikai    | Még nem áll rendelkezésre prémium tárhely |
| Észak-Európa        | 2016 augusztus 5                    |
| A központi Dél-Amerikai Egyesült Államok    | 2016 május 27                      |
| Délkelet-ázsiai      | 2016 május 24                      |
| Nyugati Európa         | 2016 május 25                      |
| Nyugati központi Amerikai Egyesült Államok     | 2016 augusztus 26.                   |
| Nyugati Amerikai Egyesült Államok             | 2016 május 26.                      |
| Nyugati US2            | 2016 augusztus 26.                   |

## <a name="automatic-migration-details"></a>Automatikus áttelepítése részletei
Alapértelmezés szerint azt áttelepíti az adatbázis-6 du és 6: 00 a régió helyi idő alatt alatt az [automatikus áttelepítés ütemezése][] során.  A meglévő adatraktár használhatatlanná lesz az áttelepítés során.  Azt adja meg, hogy az áttelepítés lép, hogy körülbelül egy órával per TB-tárterületét per adatraktár.  Azt is biztosítja, hogy nincs-előfizetést terhelő bármely részének az automatikus áttelepítés során.

> [AZURE.NOTE] Nem fogja tudni használni a meglévő adatraktár az áttelepítés során.  Miután az áttelepítés befejeződött, a adatraktár vissza online lesz.

A részletek, az alábbi lépéseket, hogy a Microsoft tart az Ön nevében az áttelepítéssel, és nem igénylő bármely bevonása hajtana.  Ebben a példában tegyük fel, hogy a meglévő DW szabványos tárolón jelenleg neve "MyDW."

1.  A Microsoft átnevezése a "MyDW" a "MyDW_DO_NOT_USE_ [időbélyeg]"
2.  A Microsoft felfüggeszti a "MyDW_DO_NOT_USE_ [időbélyeg]."  Ez idő alatt a biztonsági másolatot készített van.  Ha azt a kapcsolatos problémák megoldásához folyamat során felmerülő több szünet/önéletrajzok jelenhet meg.
3.  A Microsoft létrehoz egy új DW prémium tároló "MyDW" nevű a 2 készített biztonsági másolatból.  "MyDW" addig, amíg nem fog megjelenni, a helyreállítás befejezése után.
4.  A visszaállítás elkészülte "MyDW" az azonos DWUs és fel van függesztve vagy aktív állapotába az áttelepítés előtt adja eredményül.
5.  Miután az áttelepítés befejeződött, a Microsoft "MyDW_DO_NOT_USE_ [időbélyeg]" törlése
    
> [AZURE.NOTE] Ezeket a beállításokat nem kerülnek át az áttelepítés részeként:
> 
>   -  Naplózás az adatbázis szintjén kell lennie, újraengedélyezheti
>   -  Az **adatbázis** szintjén tűzfalszabályokat újra kell hozzáadva kell.  A **kiszolgáló** szintjén tűzfalszabályokat nem vannak hatással.

### <a name="automatic-migration-schedule"></a>Az automatikus áttelepítés ütemezése
Az automatikus áttelepítések a 18: 00 – 6: 00 (helyi idő után, területenként) során a következő üzemszünetek ütemterv fordul elő.

| **Régió**          | **Becsült kezdő dátuma**     | **Becsült befejezési dátum**       |
| :------------------ | :--------------------------- | :--------------------------- |
| Ausztrália keleti      | Még nem meghatározott           | Még nem meghatározott           |
| Ausztrália Könyvesbolt is. | 2016 augusztus 10              | 2016 augusztus 24              |
| Brazília Dél        | 2016 augusztus 10              | 2016 augusztus 24              |
| Közép Kanadán      | 2016 június 23                | 2016 július 1                 |
| Kanada-keleti         | 2016 június 23                | 2016 július 1                 |
| A központi Amerikai Egyesült Államok          | 2016 június 23                | 2016 július 4                 |
| Kína keleti          | Még nem meghatározott           | Még nem meghatározott           |
| Kína Észak         | Még nem meghatározott           | Még nem meghatározott           |
| Kelet-ázsiai           | 2016 június 23                | 2016 július 1                 |
| Kelet-Amerikai Egyesült Államok             | 2016 június 23                | 2016 július 11                |
| Kelet-US2            | 2016 június 23                | 2016 július 8-ban                 |
| India központi       | 2016 június 23                | 2016 július 1                 |
| India Dél         | 2016 június 23                | 2016 július 1                 |
| India nyugati          | Még nem meghatározott           | Még nem meghatározott           |
| Japán keleti          | 2016 augusztus 10              | 2016 augusztus 24              |
| Japán nyugati          | Még nem meghatározott           | Még nem meghatározott           |
| A központi Észak-amerikai    | Még nem meghatározott           | Még nem meghatározott           |
| Észak-Európa        | 2016 augusztus 10              | 2016 augusztus 31-én              |
| A központi Dél-Amerikai Egyesült Államok    | 2016 június 23                | 2016 július 2                 |
| Délkelet-ázsiai      | 2016 június 23                | 2016 július 1                 |
| Nyugati Európa         | 2016 június 23                | 2016 július 8-ban                 |
| Nyugati központi Amerikai Egyesült Államok     | 2016 augusztus 14              | 2016 augusztus 31-én              |
| Nyugati Amerikai Egyesült Államok             | 2016 június 23                | 2016 július 7                 |
| Nyugati US2            | 2016 augusztus 14              | 2016 augusztus 31-én              |

## <a name="self-migration-to-premium-storage"></a>Önálló áttelepítési prémium-tárolóhoz
Ha azt szeretné, ha akkor fordul elő a legrövidebb leállás szabályozhatja, az alábbi lépésekkel segítségével egy meglévő adatraktár szabványos tárolón áttelepítése prémium tároló.  Ha úgy dönt, hogy önálló áttelepítéséhez, el kell végeznie az önálló áttelepítést, az automatikus áttelepítése az adott régióban ütközést automatikus áttelepítés veszélyének elkerülése érdekében megkezdése előtt (lásd az [áttelepítés az Automatikus ütemezés][]).

### <a name="self-migration-instructions"></a>Önálló áttelepítési útmutató
Ha szeretné, hogy a legrövidebb leállás szabályozhatja, önálló áttelepítheti a adatraktár biztonsági mentési és visszaállítási használatával.  Az áttelepítés visszaállítása részét várhatóan TB-tárterületét DW / / körülbelül egy órával állapotba.  Ha meg szeretné őrizni a ugyanazt a nevet, miután az áttelepítés befejeződött, kövesse a lépéseket [az áttelepítés során nevezze át][]. 

1.  [Szünet][] a DW, amely megnyitja az automatikus biztonsági mentés
2.  A legutóbbi pillanatkép [visszaállítása][]
3.  Törölje a meglévő DW szabványos tárolón. **Ha ez a lépés nem, akkor ráterheljük mindkét DWs.**

> [AZURE.NOTE] Ezeket a beállításokat nem kerülnek át az áttelepítés részeként:
> 
>   -  Naplózás az adatbázis szintjén kell lennie, újraengedélyezheti
>   -  Az **adatbázis** szintjén tűzfalszabályokat újra kell hozzáadva kell.  A **kiszolgáló** szintjén tűzfalszabályokat nem vannak hatással.

#### <a name="optional-steps-to-rename-during-migration"></a>Nem kötelező: áttelepítés során nevezze át a lépéseket 
Az azonos logikai kiszolgálón két adatbázist nem lehet ugyanaz a neve. SQL-adatraktár egy DW átnevezésének támogatja.

Ebben a példában tegyük fel, hogy a meglévő DW szabványos tárolón jelenleg neve "MyDW."

1.  Nevezze át a "MyDW", a paranccsal az adatbázis módosítása, hogy egy üzenetet az alábbiak szerint például "MyDW_BeforeMigration."  Ez a parancs az űrlapokat állítjuk leáll minden meglévő tranzakciók, és a fő adatbázist sikeres kell elvégezni.
```
ALTER DATABASE CurrentDatabasename MODIFY NAME = NewDatabaseName;
```
2.  [Szünet][] "MyDW_BeforeMigration", amely megnyitja az automatikus biztonsági mentés
3.  A nevét, hogy az új adatbázis [visszaállítása][] a legutóbbi pillanatfelvétel (ex: "MyDW")
4.  Törölje a "MyDW_BeforeMigration".  **Ha ez a lépés nem, akkor ráterheljük mindkét DWs.**

> [AZURE.NOTE] Ezeket a beállításokat nem kerülnek át az áttelepítés részeként:
> 
>   -  Naplózás az adatbázis szintjén kell lennie, újraengedélyezheti
>   -  Az **adatbázis** szintjén tűzfalszabályokat újra kell hozzáadva kell.  A **kiszolgáló** szintjén tűzfalszabályokat nem vannak hatással.

## <a name="next-steps"></a>Következő lépések
Prémium tárolóhoz módosítással azt is növekedett blob adatbázisfájlok a adatraktár az alapul szolgáló architektúrája számát.  Ez a változás teljesítmény előnyei maximalizálása, azt javasoljuk, hogy a a csoportosított Columnstore az indexek használatával az alábbi parancsprogramot újraépítéséhez.  Az alábbi parancsfájl működik, a további BLOB kényszerítése a meglévő adatainak egy részét.  Ha nincs művelet az adatokat fog természetesen újraterjeszteni az idő, több adat betöltése a adatraktár táblák.

**Előzetes követelményei:**

1.  Adatraktár fusson 1000 DWUs vagy annál újabb (lásd: a [Méretezés számítási power][])
2.  A felhasználó az parancsfájl [mediumrc szerepkör][] vagy magasabb kell lennie.
    1.  Felhasználó felvétele a szerepkör, hajtsa végre a következőket: 
        1.  ````EXEC sp_addrolemember 'xlargerc', 'MyUser'````

````sql
-------------------------------------------------------------------------------
-- Step 1: Create Table to control Index Rebuild
-- Run as user in mediumrc or higher
--------------------------------------------------------------------------------
create table sql_statements
WITH (distribution = round_robin)
as select 
    'alter index all on ' + s.name + '.' + t.NAME + ' rebuild;' as statement,
    row_number() over (order by s.name, t.name) as sequence
from 
    sys.schemas s
    inner join sys.tables t
        on s.schema_id = t.schema_id
where
    is_external = 0
;
go
 
--------------------------------------------------------------------------------
-- Step 2: Execute Index Rebuilds.  If script fails, the below can be rerun to restart where last left off
-- Run as user in mediumrc or higher
--------------------------------------------------------------------------------

declare @nbr_statements int = (select count(*) from sql_statements)
declare @i int = 1
while(@i <= @nbr_statements)
begin
      declare @statement nvarchar(1000)= (select statement from sql_statements where sequence = @i)
      print cast(getdate() as nvarchar(1000)) + ' Executing... ' + @statement
      exec (@statement)
      delete from sql_statements where sequence = @i
      set @i += 1
end;
go
-------------------------------------------------------------------------------
-- Step 3: Cleanup Table Created in Step 1
--------------------------------------------------------------------------------
drop table sql_statements;
go
````

Ha problémák lépnek fel a adatraktár, [Hozzon létre egy támogatási jegyek][] és a "Áttelepítési való prémium tároló" hivatkozás, lehetséges okai lehetnek.

<!--Image references-->

<!--Article references-->
[az automatikus áttelepítés ütemezése]: #automatic-migration-schedule
[self-migration to Premium Storage]: #self-migration-to-premium-storage
[a támogatási jegyek létrehozása]: sql-data-warehouse-get-started-create-support-ticket.md
[Azure paired region]: best-practices-availability-paired-regions.md
[main documentation site]: services/sql-data-warehouse.md
[Szünet]: sql-data-warehouse-manage-compute-portal.md/#pause-compute
[Visszaállítása]: sql-data-warehouse-restore-database-portal.md
[áttelepítés során átnevezésének lépései]: #optional-steps-to-rename-during-migration
[méretezés számítási power]: sql-data-warehouse-manage-compute-portal/#scale-compute-power
[mediumrc szerepkör]: sql-data-warehouse-develop-concurrency/#workload-management

<!--MSDN references-->


<!--Other Web references-->
[Nagyobb teljesítmény kiszámítható prémium tárhely]: https://azure.microsoft.com/en-us/blog/azure-sql-data-warehouse-introduces-premium-storage-for-greater-performance/
[Azure portál]: https://portal.azure.com
