<properties
   pageTitle="SQL-adatraktár kapacitás korlátai |} Microsoft Azure"
   description="A kapcsolatok, táblák és lekérdezések SQL adatraktár maximális értékek."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/01/2016"
   ms.author="sonyama;barbkess;jrj"/>

# <a name="sql-data-warehouse-capacity-limits"></a>SQL-adatraktár kapacitás korlátai

Az alábbi táblázatban az Azure SQL-adatraktár különféle összetevőket számára engedélyezett maximális értéket tartalmaznak.


## <a name="workload-management"></a>Terhelést kezelése

| Kategória            | Leírás                                       | Maximum            |
| :------------------ | :------------------------------------------------ | :----------------- |
| [Adatok raktári egységek (DWU)][]| Egy egyetlen SQL adatraktár a Max DWU | 6000               |
| [Adatok raktári egységek (DWU)][]| Max DWU egy egyetlen SQL Server         | Alapértelmezés szerint 6000<br/><br/> Alapértelmezés szerint minden SQL server (pl. myserver.database.windows.net) rendelkezik DTU kvóta 45,000, amely lehetővé teszi, hogy 6000 DWU felfelé. Ez a kvóta egyszerűen biztonsági korlátozás. A [támogatás jegy létrehozását][] , és válassza a *Kvóta* kérelem típusúként kvóta növelésével.  A DTU kiszámításához van szüksége, a 7.5 szorzása a teljes DWU szükséges. Az aktuális DTU felhasználási a az SQL server-lap megtekintése a portálon. Felfüggesztett és a nem felfüggesztett adatbázis megszámolása DTU kvóta felé. |
| Adatbázis-kapcsolatot | Egyidejű megnyitott munkamenetek                          | 1024<br/><br/>Legfeljebb 1024 aktív kapcsolatot, egy időben adatraktár SQL-adatbázishoz kérelmeket küldhessenek, amelyek támogatjuk. Fontos tudni, hogy nincsenek-e a ténylegesen végrehajtható egyidejűleg lekérdezések számával. A párhuzamos korlát túllépésekor a kérelem vált át egy belső várólista ahol vár feldolgoztatni.|
| Adatbázis-kapcsolatot | A maximális memóriaméret készített kimutatásokhoz            | 20 MB              |
| [Terhelést kezelése][] | Egyidejű lekérdezések maximális                    | 32<br/><br/> Alapértelmezés szerint a SQL adatraktár legfeljebb 32 egyidejű lekérdezések és lekérdezések hátralévő sorban várakozó hajthat végre.<br/><br/>A párhuzamos szint lecsökkenhet felhasználók vannak rendelve egy újabb erőforrás osztály, vagy ha SQL adatraktár alacsony DWU van beállítva. Bizonyos lekérdezések DMV lekérdezéseket, például mindig engedélyezettek futtatásához.|
| [TempDB][]          | Tempdb maximális mérete                                | 399 GB adatot DW100. Ezért DWU1000 Tempdb, melynek a mérete megegyezik 3,99 TB |


## <a name="database-objects"></a>Az adatbázis-objektumok

| Kategória          | Leírás                                  | Maximum            |
| :---------------- | :------------------------------------------- | :----------------- |
| Adatbázis          | Maximális méret                                     | Lemezen tömörített 240 TB<br/><br/>Ezt a területet független tempdb vagy napló terület, és ezért ezt a területet állandó táblák van hozzárendelve.  Csoportosított columnstore tömörítés 5 X becsült.  A tömörítési lehetővé teszi, hogy az adatbázist, hogy körülbelül 1 nő PB, ha minden olyan tábla csoportosított columnstore (az alapértelmezett táblázat típus).|
| Táblázat             | Maximális méret                                     | Lemezen tömörített 60 TB   |
| Táblázat             | Táblák adatbázisonként                          | 2 milliárd          |
| Táblázat             | Tábla oszlopainak száma                            | 1024 oszlopok       |
| Táblázat             | Bájt / oszlop                             | Függő oszlop [adattípusát][].  A korlát 8000 karakter adattípusokhoz, az nvarchar 4000 vagy a MAX adattípusokhoz 2 GB.|
| Táblázat             | Bájt soronként megadott mérete                  | 8060 bájt<br/><br/>A soronként bájtok számát ugyanúgy kiszámítása, mert az SQL Server-tömörítéssel lapra. SQL Server, például SQL adatraktár támogatja a sor-túlcsordulás tárhely, amely lehetővé teszi a sor kell eltolni **változó hosszúságú oszlopokat** . Ha a sor változó hosszúságú sorok elküldte azokat, csak a 24 bájtos legfelső szintű a fő rekord vannak tárolva. További tudnivalókért lásd: a [sor-túlcsordulás adatok meghaladó 8 KB][] MSDN-cikket.|
| Táblázat             | Egy táblázat partíciót                    | 15 000<br/><br/>A nagy teljesítmény elérése érdekében azt javasoljuk, számának minimalizálása partíciók meg kell miközben továbbra is támogató az üzleti igényeknek megfelelően alakíthatja. Partíciók száma növekedésével az általános adatok Definition Language (DDL) és adatok kezelése nyelvet (DML) műveletekhez megnő, és lassabban okoz.|
| Táblázat             | Partition oszlopazonosító értékenként karaktereket.| 4000 |
| Index             | Nem csoportosított indexek egy táblázatot.        | 999<br/><br/>Csak a táblázatok rowstore vonatkozik.|
| Index             | Csoportosított indexek egy táblázatot.            | 1<br><br/>Rowstore és a columnstore tábla vonatkozik.|
| Index             | Index kulcs méretét.                          | 900 bájt.<br/><br/>Csak az indexek rowstore vonatkozik.<br/><br/>Indexek varchar oszlopok maximális több, mint 900 bájt méretű hozhat létre, ha a meglévő adatok az oszlop nem, mint 900 bájt az index létrehozása. Azonban később BESZÚRÁSA, illetve az oszlopok haladja meg a 900 bájt összesített méretét okozó műveletek frissítés sikertelen lesz.|
| Index             | Kulcs oszlopainak száma index.                   | 16<br/><br/>Csak az indexek rowstore vonatkozik. Csoportosított columnstore indexek összes oszlopot tartalmaz.|
| Statisztika        | A kombinált oszlopértékek méretét.      | 900 bájt.         |
| Statisztika        | Oszlopok használati statisztika objektumot.           | 32                 |
| Statisztika        | A tábla oszlopainak száma létrehozott statisztikákat. | 30 000 feletti számig            |
| Tárolt eljárás | A beágyazási szintek maximális.               | 8                 |
| Nézet              | Oszlopainak száma megtekintése                         | 1024             |


## <a name="loads"></a>Terhelések

| Kategória          | Leírás                                  | Maximum            |
| :---------------- | :------------------------------------------- | :----------------- |
| Polybase terhelések    | Bájt soronként                                | 32 768<br/><br/>Polybase terhelések sorok mindkét 32K-nál kisebb betöltése korlátozódik, és nem tölthető VARCHR(MAX), NVARCHAR(MAX) és VARBINARY(MAX).  Ezt a korlátozást még ma létezik, amíg el lesz távolítva meglehetősen hamarosan.<br/><br/>


## <a name="queries"></a>Lekérdezések

| Kategória          | Leírás                                  | Maximum            |
| :---------------- | :------------------------------------------- | :----------------- |
| Lekérdezés             | Aszinkron lekérdezések felhasználói táblázatokban.               | 1000               |
| Lekérdezés             | Egyidejű lekérdezések rendszer nézetek.          | 100                |
| Lekérdezés             | A rendszer nézetek várólistás lekérdezések               | 1000               |
| Lekérdezés             | Maximális paraméterei                           | 2098               |
| Köteg             | Maximális mérete                                 | 65, 536 * 4096        |
| SELECT eredménye    | Oszlopok soronként                              | 4096<br/><br/>A SELECT eredmény soha nem lehet soronként több, mint 4096 oszlopok. Nincs garancia, hogy mindig legyen a 4096 nem. Ha a lekérdezéstervet ideiglenes táblázat igényel, a 1024 tábla oszlopainak száma maximális vonatkozhatnak.|
| VÁLASSZA A            | Beágyazott segédlekérdezések                            | 32<br/><br/>SELECT utasítás soha nem lehet 32-nél több beágyazott segédlekérdezések. Nincs semmilyen garanciát, hogy mindig legyen a 32. Ha például a ILLESZTÉS segédlekérdezés is kiegészíteni a lekérdezéstervet. Segédlekérdezések számát is is rendelkezésre álló memória függvényében korlátozva.|
| VÁLASSZA A            | Csatlakozás oszloponként                             | 1024 oszlopok<br/><br/>Az ILLESZTÉS soha nem lehet legfeljebb 1024 oszlopokat. Nincs garancia, hogy mindig legyen a 1024 nem. Ha a csatlakozás csomagot kell ideiglenes az ILLESZTÉS eredmény-nél több oszlopot tartalmazó táblázat, a 1024 által az ideiglenes tábla vonatkozik. |
| VÁLASSZA A            | Egy GROUP BY oszlopok bájt.                  | 8060<br/><br/>Az oszlopok a GROUP BY záradékban beállíthatja, hogy legfeljebb 8060 bájt.|
| VÁLASSZA A            | Bájt / ORDER BY oszlopok                   | 8060 bájt.<br/><br/>Az oszlopokat az ORDER BY záradék legfeljebb 8060 bájt állhat.|
| Azonosítók és állandók / utasítás | Hivatkozott azonosítóját és az állandók száma. | 65535<br/><br/>Az SQL adatraktár azonosítóját és egy egyetlen kifejezés egy lekérdezés tartalmazó állandók korlátozza. Ez a korlát 65535. A szám eredménye az SQL Server hiba 8632 meghaladó. További tudnivalókért lásd: [belső hiba: egy kifejezés szolgáltatások elérte][].|


## <a name="metadata"></a>Metaadatok

| Rendszerek nézet                        | Sorok maximális száma |
| :--------------------------------- | :------------|
| sys.dm_pdw_component_health_alerts | 10 000       |
| sys.dm_pdw_dms_cores               | 100          |
| sys.dm_pdw_dms_workers             | A legutóbbi 1000 Dokumentumkezelő dolgozók száma SQL kéri. |
| sys.dm_pdw_errors                  | 10 000       |
| sys.dm_pdw_exec_requests           | 10 000       |
| sys.dm_pdw_exec_sessions           | 10 000       |
| sys.dm_pdw_request_steps           | A legutóbbi 1000 SQL kéréseket, amelyeket sys.dm_pdw_exec_requests lépései száma. |
| sys.dm_pdw_os_event_logs           | 10 000       |
| sys.dm_pdw_sql_requests            | A legutóbbi 1000 SQL kéréseket, amelyeket sys.dm_pdw_exec_requests. |


## <a name="next-steps"></a>Következő lépések
Hivatkozás kapcsolatos további tudnivalókért [adatraktár SQL referencia áttekintése][]című témakörben találhat.

<!--Image references-->

<!--Article references-->
[Adatok raktári egységek (DWU)]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units
[Adatraktár SQL referencia – áttekintés]: ./sql-data-warehouse-overview-reference.md
[Terhelést kezelése]: ./sql-data-warehouse-develop-concurrency.md
[TempDB]: ./sql-data-warehouse-tables-temporary.md
[adattípus]: ./sql-data-warehouse-tables-data-types.md
[a támogatási jegyek létrehozása]: /sql-data-warehouse-get-started-create-support-ticket.md

<!--MSDN references-->
[Sor-túlcsordulás 8 KB meghaladó adatok]: https://msdn.microsoft.com/library/ms186981.aspx
[Belső hiba: egy kifejezés szolgáltatások elérte]: https://support.microsoft.com/kb/913050
