<properties
    pageTitle="SQL - adatbázis-kapcsolatot hiba hibakódok |} Microsoft Azure"
    description="Tudjon meg többet az SQL-adatbázis ügyfélalkalmazásokban, például adatbázis kapcsolat kapcsolatos gyakori hibákra, az adatbázis-példány problémák és a általános hibák SQL-hibakódok. "
    keywords="SQL-hibakód, az access sql, hiba az adatbázis-kapcsolatot, sql hibakódok esetén"
    services="sql-database"
    documentationCenter=""
    authors="annemill"
    manager="jhubbard"
    editor="" />


<tags
    ms.service="sql-database"
    ms.workload="drivers"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/12/2016"
    ms.author="annemill"/>


# <a name="sql-error-codes-for-sql-database-client-applications-database-connection-error-and-other-issues"></a>SQL-hibakódok az SQL-adatbázis-ügyfélalkalmazásokban: adatbázis-kapcsolati hiba és egyéb problémák


<!--
Old Title on MSDN:  Error Messages (Azure SQL Database)
ShortId on MSDN:  ff394106.aspx
Dx 4cff491e-9359-4454-bd7c-fb72c4c452ca
-->


Ez a cikk felsorolja az SQL-hibakódok SQL-adatbázis ügyfél-alkalmazásokhoz, például adatbázis internetkapcsolat hibáinak, ideiglenes (tranziens) hibák (más néven tranziens hibák), az erőforrás irányítási hibákat, adatbázis-példány problémák, rugalmas készlet, és más hibákat. A legtöbb kategóriák adott Azure SQL-adatbázishoz, és a Microsoft SQL Server nem vonatkoznak.

## <a name="database-connection-errors-transient-errors-and-other-temporary-errors"></a>Adatbázis-kapcsolatot hibákat, tranziens hibák és egyéb ideiglenes hibák

Az alábbi táblázat bemutatja az SQL hibakódok kapcsolat veszteség hibák és egyéb az alkalmazás SQL-adatbázis eléréséhez alkalommal, amikor előforduló tranziens hibák. Első lépések – oktatóanyagok hogyan Azure SQL-adatbázishoz való csatlakozáshoz, olvassa el a [Csatlakozás Azure SQL-adatbázishoz](sql-database-libraries.md).

### <a name="most-common-database-connection-errors-and-transient-fault-errors"></a>Adatbázis-kapcsolatot leggyakoribb hibákat és tranziens hibafa hibák

Az Azure infrastruktúra kiszolgálók dinamikusan dolgozóját, ha az SQL-adatbázis-szolgáltatás nehéz munkaterhelésekből merülnek fel, hogy lehetősége van.  A dinamikus viselkedés miatt megtelhet az ügyfélprogram elveszíti a kapcsolat SQL-adatbázishoz. Ez a fajta hiba egy *tranziens hibafa*neve.

Ha az ügyfélprogram újrapróbálkozási logika, kipróbálhatja helyreállítani az ideiglenes (tranziens) hibafa idő javítása magát megadása után a kapcsolatot.  Azt javasoljuk, hogy késleltetése a 5 másodpercig előtt az első újra gombra. Újra próbálkozik rövidebb, mint a felhőbeli szolgáltatástól overwhelming 5 másodperc kockázatok késleltetés után. Az egyes későbbi újra kell a késleltetés exponenciálisan, nagyobb legfeljebb 60 másodperc.

Ideiglenes (tranziens) hibafa hibák általában megegyezik az ügyfélprogramok az a következő hibaüzenetek egyike cikkét:

- Adatbázis < db_name > kiszolgálón < Azure_instance > jelenleg nem áll rendelkezésre. A kapcsolat később próbálkozzon újra. Ha a probléma továbbra is fennáll, forduljon az ügyfélszolgálathoz, és küldje el nekik a munkamenet-nyomkövetés azonosítója < session_id >

- Adatbázis < db_name > kiszolgálón < Azure_instance > jelenleg nem áll rendelkezésre. A kapcsolat később próbálkozzon újra. Ha a probléma továbbra is fennáll, forduljon az ügyfélszolgálathoz, és küldje el nekik a munkamenet-nyomkövetés azonosítója < session_id >. (A Microsoft SQL Server, hiba: 40613)

- Egy meglévő kapcsolat kényszerített bezárta a távoli állomás.

- System.Data.Entity.Core.EntityCommandExecutionException: Hiba történt a parancs meghatározása végrehajtása. Lásd: További információ a belső kivételt. ---> System.Data.SqlClient.SqlException: átviteli szintű hiba történt A találatok a kiszolgálóról fogadásakor. (szolgáltató: munkamenet-szolgáltató: 19 - fizikai kapcsolat nem használható)

Kód példák újrapróbálkozási logikájának című témakörben talál:

- [A kapcsolat SQL-adatbázis és az SQL Server képtárak](sql-database-libraries.md) 
- [Műveletek csatlakozási hibák és tranziens hibák megoldásához SQL-adatbázisban](sql-database-connectivity-issues.md)

Az [SQL Server egyesítése (ADO.NET)](http://msdn.microsoft.com/library/8xx3tyca.aspx)az az *időszak blokkolása* ADO.NET használó ügyfelek vitafórum érhető el.

### <a name="transient-fault-error-codes"></a>Ideiglenes (tranziens) hibakódok hiba

Az alábbi hibák ideiglenes (tranziens), és újra meg kell próbálni az alkalmazás logikája 

| Kódszámú hiba jelenik meg | Súlyosságát | Leírás |
| ---: | ---: | :--- |
| 4060 | 16 | Adatbázis nem nyitható meg "% & #x2a; ls" a bejelentkezést igénylő. A bejelentkezési sikertelen volt. |
|40197|17|A szolgáltatás hibát a kérést. Próbálkozzon újra. A hibakód: %d.<br/><br/>Ezt a hibát, ha a szolgáltatás nem működik szoftver vagy hardver-frissítések, hardver hibák vagy bármely más feladatátvevő probléma miatt kap. Az üzenet hiba 40197 ágyazott hibakód (%d) hiba vagy az, hogy mikor történt feladatátvevő típusú további információt tartalmaz. Néhány példa a hiba az üzenetben hiba 40197 beágyazott kódok 40020, 40143, 40166 és 40540.<br/><br/>Újracsatlakozás a SQL-adatbázis kiszolgálója automatikusan kapcsolódik, a megfelelő másolatot az adatbázisról. Az alkalmazás kell elfog 40197, hibanaplójának a beágyazott hibakód (%d) az üzenet hibaelhárítási belül, majd próbálkozzon a mindaddig, amíg az erőforrások érhetők el, és újra létrejött a kapcsolat SQL-adatbázis újracsatlakozás.|
|40501|20|A szolgáltatás jelenleg túlterhelt. Ismételje meg a kérelmet, 10 másodperc után. Az esemény azonosító: %ls. Kód: %d.<br/><br/>*Megjegyzés:* További tudnivalókért lásd:<br/>• [Azure SQL-adatbázis erőforrás korlátai](sql-database-resource-limits.md).
|40613|17|Adatbázis "% & #x2a; ls" kiszolgálón "% & #x2a; ls" jelenleg nem áll rendelkezésre. A kapcsolat később próbálkozzon újra. Ha a probléma továbbra is fennáll, forduljon az ügyfélszolgálathoz, és küldje el nekik a munkamenet nyomkövetési Azonosítójával "% & #x2a; ls".|
|49918|16|Nem lehet feldolgozni kérelmet. Nincs elegendő erőforrások összehívás feldolgozása.<br/><br/>A szolgáltatás jelenleg túlterhelt. A kérés később próbálkozzon újra. |
|49919|16|Nem lehet folyamatábra létrehozása vagy módosítása a kérést. Túl sok műveletek létrehozása vagy módosítása a haladás előfizetéshez "% ld".<br/><br/>A szolgáltatás nem elfoglalt feldolgozása több létrehozása vagy előfizetés vagy server kérelmek frissítése. Az erőforrás optimalizálási jelenleg zárolásának kérések. A lekérdezés [sys.dm_operation_status](https://msdn.microsoft.com/library/dn270022.aspx) a függőben lévő műveletek. Várja meg, amíg a függőben lévő létrehozás és frissítés kérések fejeződtek vagy törlése a folyamatban lévő kérelmek közül, és próbálja meg később a kérést. |
|49920|16|Nem lehet feldolgozni kérelmet. Folyamatban lévő túl sok műveletek az előfizetéshez "% ld".<br/><br/>A szolgáltatás nem elfoglalt az előfizetéshez tartozó több kérelmek feldolgozása. Az erőforrás-optimalizálási jelenleg zárolásának kérések. A lekérdezés [sys.dm_operation_status](https://msdn.microsoft.com/library/dn270022.aspx) művelet állapota. Várakozás kérések függőben fejeződtek vagy törlése a folyamatban lévő kérelmek közül, és próbálja meg később a kérést. |

## <a name="database-copy-errors"></a>Adatbázis-példány hibák

Az alábbi hiba is történt a adatbázis másolása Azure SQL-adatbázisban. További tudnivalókért olvassa el a [Azure SQL-adatbázis másolása](sql-database-copy.md)című témakört.


|Kódszámú hiba jelenik meg|Súlyosságát|Leírás|
|---:|---:|:---|
|40635|16|Ügyfél IP-címet "% & #x2a; ls" ideiglenesen le van tiltva.|
|40637|16|Hozzon létre adatbázis másolás jelenleg le van tiltva.|
|40561|16|Adatbázis-példány sikertelen volt. A forráslista vagy a Céllista adatbázis nem létezik.|
|40562|16|Adatbázis-példány sikertelen volt. A forrásadatbázis megszakadt.|
|40563|16|Adatbázis-példány sikertelen volt. Megszakadt a céladatbázisban.|
|40564|16|Adatbázis-példány belső hiba miatt sikertelen volt. Cél adatbázis, és próbálkozzon újra.|
|40565|16|Adatbázis-példány sikertelen volt. 1-nél több egyidejű adatbázis másolása ugyanazon adatforrásból engedélyezve van. Cél adatbázis törlése, majd próbálkozzon újra.|
|40566|16|Adatbázis-példány belső hiba miatt sikertelen volt. Cél adatbázis, és próbálkozzon újra.|
|40567|16|Adatbázis-példány belső hiba miatt sikertelen volt. Cél adatbázis, és próbálkozzon újra.|
|40568|16|Adatbázis-példány sikertelen volt. Forrásadatbázis vált, hogy nem érhető el. Cél adatbázis, és próbálkozzon újra.|
|40569|16|Adatbázis-példány sikertelen volt. Céladatbázisban vált, hogy nem érhető el. Cél adatbázis, és próbálkozzon újra.|
|40570|16|Adatbázis-példány belső hiba miatt sikertelen volt. Cél adatbázis törlése, majd próbálkozzon újra.|
|40571|16|Adatbázis-példány belső hiba miatt sikertelen volt. Cél adatbázis törlése, majd próbálkozzon újra.|

## <a name="resource-governance-errors"></a>Erőforrás irányítási hibák

Az alábbi hibák okozzák erőforrások fölösleges használatának Azure SQL-adatbázis használata közben. Példa:

- Egy tranzakció volt nyitva túl sokáig.
- A tranzakciók túl sok zárolva van.
- Az alkalmazás használata van más túl sok memória.
- Az alkalmazások túl sok használata más `TempDb` helyet.

Kapcsolódó témakörök:

* További információt itt rendelkezésre álló: [Azure SQL-adatbázis erőforrás korlátai](sql-database-resource-limits.md)

|Kódszámú hiba jelenik meg|Súlyosságát|Leírás|
|---:|---:|:---|
|10928|20|Erőforrás-azonosító: %d. Az adatbázis (% s) korlátját %d., és jött létre. További tudnivalókért olvassa el a [http://go.microsoft.com/fwlink/?LinkId=267637](http://go.microsoft.com/fwlink/?LinkId=267637)című témakört.<br/><br/>Az erőforrás-azonosító azt jelzi, hogy az erőforrás, amely elérte a korlátot. A dolgozó szálak, az erőforrás-azonosító = 1. Munkamenetek, az erőforrás-azonosító = 2.<br/><br/>*Megjegyzés:* Ezt a hibát és megoldásával kapcsolatos további tudnivalókért lásd:<br/>• [Azure SQL-adatbázis erőforrás korlátai](sql-database-resource-limits.md). |
|10929|20|Erőforrás-azonosító: %d. A (% s) minimális garancia %d, maximális értéke %d. pedig az aktuális használatát az adatbázis %d. A kiszolgáló azonban jelenleg a támogatási kérelmek nagyobb, mint %d. adatbázis elfoglalt. További tudnivalókért olvassa el a [http://go.microsoft.com/fwlink/?LinkId=267637](http://go.microsoft.com/fwlink/?LinkId=267637)című témakört. Egyéb esetben próbálja meg később újra.<br/><br/>Az erőforrás-azonosító azt jelzi, hogy az erőforrás, amely elérte a korlátot. A dolgozó szálak, az erőforrás-azonosító = 1. Munkamenetek, az erőforrás-azonosító = 2.<br/><br/>*Megjegyzés:* Ezt a hibát és megoldásával kapcsolatos további tudnivalókért lásd:<br/>• [Azure SQL-adatbázis erőforrás korlátai](sql-database-resource-limits.md).|
|40544|20|Az adatbázis mérete kvótáját elérte. Partition vagy törlés, húzza az indexek, illetve dokumentációjában lehetséges megoldásokat.|
|40549|16|Munkamenet megszakad, mert egy hosszabb ideig futó tranzakció. Próbálja meg a tranzakció térközszakaszok rövidítése.|
|40550|16|A munkamenet véget ért, mert túl sok zárolások szerzett. Próbálja meg olvasási vagy kevesebb sorainak egyetlen tranzakció módosítása.|
|40551|16|A munkamenet véget ért miatt fölösleges `TEMPDB` használatát. Próbálja ki az ideiglenes tábla lemezterület csökkentheti a lekérdezés módosítása.<br/><br/>*Tipp:* Ha ideiglenes objektumok használata esetén a helymegtakarítás a `TEMPDB` adatbázis leválasztása ideiglenes objektumok, miután a munkamenet már nincs rájuk szüksége.|
|40552|16|A munkamenet fölösleges tranzakció napló lemezterület-használat miatt leállt. Próbálja meg egyetlen tranzakció kevesebb sorainak módosítása.<br/><br/>*Tipp:* Ha hajtja végre tömeges illeszt be, használja a `bcp.exe` segédprogram vagy a `System.Data.SqlClient.SqlBulkCopy` osztály, használja a a `-b batchsize` vagy `BatchSize` beállítások korlátozhatja a sorok számát a másolt tranzakciók kiszolgálójába. Ha az index meg vannak Újraépítés a `ALTER INDEX` utasítás, használja a `REBUILD WITH ONLINE = ON` lehetőséget.|
|40553|16|A munkamenet fölösleges memóriahasználat miatt leállt. Próbálja meg a lekérdezés kevesebb sorok feldolgozása módosítása.<br/><br/>*Tipp:* Számának csökkentése `ORDER BY` és `GROUP BY` a Transact-SQL-kódot a műveletek csökkenti a lekérdezés memória követelményeinek.|

## <a name="elastic-pool-errors"></a>Rugalmas készlet hibák

Az alábbi hibák létrehozásának és használatának Elastics készletek kapcsolódnak.

| ErrorNumber | ErrorSeverity | ErrorFormat | ErrorInserts | ErrorCause | ErrorCorrectiveAction |
| :-- | :-- | :-- | :-- | :-- | :-- |
| 1132 | EX_RESOURCE | A rugalmas készlet megérkezett a Tárhelykorlát érvényes. Rugalmas gyűjtő tárterület-használat nem haladhatja meg (%d) MB. | Rugalmas készlet terület határérték MB. | Próbál adatokat írni egy adatbázist, a tárterületkorlát rugalmas készlet elérésekor. | Kérjük, érdemes megfontolni a DTUs növelheti annak Tárhelykorlát érvényes, az egyes adatbázisokat a rugalmas készlet belül által használt tárterület csökkentése érdekében lehetőség szerint rugalmas készlet és a adatbázisokat a rugalmas készletből. |
| 10929 | EX_USER | A (% s) minimális garancia %d, maximális értéke %d. pedig az aktuális használatát az adatbázis %d. A kiszolgáló azonban jelenleg a támogatási kérelmek nagyobb, mint %d. adatbázis elfoglalt. Lásd: [http://go.microsoft.com/fwlink/?LinkId=267637](http://go.microsoft.com/fwlink/?LinkId=267637) segítségét. Egyéb esetben próbálja meg később újra. | DTU min adatbázisonként; DTU max adatbázisonként | A teljes oldalszám egyidejű dolgozók (kérelmek) keresztül a rugalmas készletben adatbázisokra megpróbált haladja meg a készlet által. | Kérjük, érdemes megfontolni a DTUs, ha lehetséges rugalmas készlet annak érdekében, hogy a dolgozó maximális méretét növelheti és a adatbázisokat a rugalmas készletből. |
| 40844 | EX_USER | Adatbázis "%ls", "%ls" kiszolgálón egy rugalmas készletben "%ls" edition adatbázis, és nem lehet a folyamatos másolás kapcsolat. | az adatbázis neve, az adatbázis edition, a kiszolgáló neve | StartDatabaseCopy parancs ki van egy nem prémium db az rugalmas készletben. | hamarosan |
| 40857 | EX_USER | Nem található a kiszolgáló rugalmas készlet: "%ls", az rugalmas készlet nevét: "%ls". | kiszolgáló; neve rugalmas alkalmazáskészlet neve | Megadott rugalmas készlet nem szerepel a megadott kiszolgáló. | Adjon meg egy érvényes rugalmas készlet nevet. |
| 40858 | EX_USER | Rugalmas készlet "%ls" már létezik a kiszolgáló: "%ls" | rugalmas alkalmazáskészlet neve, a kiszolgáló neve | Megadott rugalmas készlet már szerepel a megadott logikai kiszolgáló. | Írjon be új rugalmas készlet nevet. |
| 40859 | EX_USER | Rugalmas készlet nem támogatja a szolgáltatás szint (%ls). | rugalmas készlet szolgáltatási réteg | Rugalmas készlet kiépítési megadott szolgáltatási réteg nem támogatott. | Adja meg a megfelelő edition, vagy hagyja üresen az alapértelmezett szolgáltatási réteg használni a szolgáltatási réteg. |
| 40860 | EX_USER | Rugalmas készlet "%ls" és a szolgáltatás cél "%ls" kombinációja érvénytelen. | rugalmas alkalmazáskészlet neve; szolgáltatás szintű objektív neve | Rugalmas készlet és szolgáltatás cél megadható együtt csak akkor, ha a szolgáltatás cél "ElasticPool" van megadva. | Adja meg a megfelelő kombinációját rugalmas készlet és szolgáltatás cél. |
| 40861 | EX_USER | Az adatbázis edition "%. *ls' nem lehet ugyanaz, mint a rugalmas készlet szolgáltatási réteg, amely "%.* ls'. | adatbázis edition rugalmas készlet szolgáltatási réteg | Az adatbázis edition eltér a rugalmas készlet szolgáltatási réteg. | Kérjük, nem ad meg egy adatbázis edition, amely ugyanaz, mint a rugalmas készlet szolgáltatási réteg.  Fontos tudni, hogy az adatbázis edition nem kell megadni. |
| 40862 | EX_USER | Rugalmas készlet kell lennie a Ha a készlet rugalmas szolgáltatást cél meg van adva. | Nincs lehetőség | Rugalmas készlet szolgáltatás cél egyedileg nem azonosítja egy rugalmas készlet. | Adja meg a készlet rugalmas nevét, a készlet rugalmas szolgáltatást cél használata. |
| 40864 | EX_USER | A DTUs rugalmas gyűjtő legalább kell lennie (%d) DTUs a szolgáltatási réteg "%. * ls'. | Rugalmas készlet; DTUs rugalmas készlet szolgáltatási réteg. | A minimális határérték alatti rugalmas gyűjtő DTUs beállítása kísérel meg. | Próbálja meg újra a DTUs beállítása a elasztikus-készlet legalább a minimális érték. |
| 40865 | EX_USER | A DTUs rugalmas gyűjtő nem haladhatja meg a (%d) DTUs a szolgáltatási réteg "%. * ls'. | Rugalmas készlet; DTUs rugalmas készlet szolgáltatási réteg. | A fent a maximális érték rugalmas gyűjtő DTUs beállítása kísérel meg. | Próbálja meg újra a DTUs rugalmas gyűjtő beállítása nem lehet nagyobb, mint a maximális érték. |
| 40867 | EX_USER | A max adatbázisonként DTU kell lennie a legalacsonyabb (%d) a szolgáltatási réteg "%. * ls'. | DTU max adatbázisonként; rugalmas készlet szolgáltatási réteg | Állítsa a DTU max a vonatkozó alábbi adatbázisonként kísérel meg. | Kérjük, akkor fontolja meg a készlet rugalmas szolgáltatást réteg, amely támogatja a kívánt beállításra. |
| 40868 | EX_USER | A max adatbázisonként DTU nem haladhatja meg (%d) a szolgáltatási réteg "%. * ls'. | DTU max adatbázisonként; rugalmas készlet szolgáltatási réteg. | Állítsa a DTU max túl a vonatkozó adatbázisonként kísérel meg. | Kérjük, akkor fontolja meg a készlet rugalmas szolgáltatást réteg, amely támogatja a kívánt beállításra. |
| 40870 | EX_USER | A DTU min adatbázisonként nem haladhatja meg (%d) a szolgáltatási réteg "%. * ls'. | DTU min adatbázisonként; rugalmas készlet szolgáltatási réteg. | Állítsa a DTU min, a vonatkozó túl adatbázisonként kísérel meg. | Kérjük, akkor fontolja meg a készlet rugalmas szolgáltatást réteg, amely támogatja a kívánt beállításra. |
| 40873 | EX_USER | Az adatbázisok (%d) és a DTU min (%d) adatbázisonként száma nem haladhatja meg a a DTUs rugalmas készlet (%d). | Szám adatbázisok rugalmas készletben DTU min adatbázisonként; Rugalmas készlet DTUs. | Adja meg az adatbázisok DTU min meghaladja a DTUs rugalmas készlet rugalmas készletben kísérel meg. | Érdemes megfontolni a DTUs rugalmas készlet, és csökkentheti a DTU min adatbázisonként, vagy csökkentse a rugalmas készletben adatbázisok számát. |
| 40877 | EX_USER | Egy rugalmas készlet nem törölhető, hacsak nem tartalmazó adatbázist. | Nincs lehetőség | A rugalmas készlet tartalmaz egy vagy több adatbázist, és ezért nem törölhető. | Távolítsa el adatbázisokat a rugalmas készletből annak érdekében, hogy törölheti azt. |
| 40881 | EX_USER | A rugalmas készlet "%. * ls' elérte az adatbázis maximális száma.  Az adatbázis maximális száma a rugalmas gyűjtő egy rugalmas készlet (% d) DTUs nem haladhatja meg (%d). | Rugalmas készlet; neve adatbázis maximális száma rugalmas készlet; DTUs az erőforráskészlet-e. | Próbál létrehozni vagy az adatbázis maximális száma a rugalmas készlet elérésekor adatbázis rugalmas készlet hozzáadása. | Kérjük, érdemes megfontolni a DTUs, ha lehetséges rugalmas készlet annak érdekében, hogy a adatbázis maximális méretét növelheti és a adatbázisokat a rugalmas készletből. |
| 40889 | EX_USER | A DTUs vagy Tárhelykorlát érvényes rugalmas gyűjtő "%. * ls' nem lehet kisebb, mivel, amely elegendő tárterület nem ad az adatbázisok. | Rugalmas készlet nevét. | Csökkentse az alatt a tárterület-használat rugalmas készlet tárhelykorláttal kísérel meg. | Fontolja meg az egyes adatbázisok rugalmas készletben tárterület-használat csökkentését, vagy távolítsa el a adatbázisokat a készletből DTUs vagy a tárterületkorlát csökkentése érdekében. |
| 40891 | EX_USER | A DTU min (%d) adatbázisonként nem haladhatja meg a DTU max adatbázisonként (%d). | DTU min adatbázisonként; Max adatbázisonként DTU. | Állítsa a DTU min nagyobb, mint a DTU max adatbázisonként adatbázisonként kísérel meg. | Ellenőrizze, hogy a DTU min per adatbázisokban nem haladja meg a DTU max adatbázisonként. |
| TBD | EX_USER | A tároló méretét egy rugalmas készletben egyes adatbázisok nem haladhatja meg a által megengedett maximális méret "%. * ls' szolgáltatás réteg rugalmas készlet. | rugalmas készlet szolgáltatási réteg | Az adatbázis maximális mérete meghaladja a rugalmas készlet szolgáltatási réteg által megengedett maximális méretét. | Állítsa be az adatbázis a a rugalmas készlet szolgáltatási réteg által megengedett maximális méret keretén belül a maximális méret. |

Kapcsolódó témakörök:

* [Hozzon létre egy rugalmas adatbázis készlet (C#)](sql-database-elastic-pool-create-csharp.md) 
* [Egy rugalmas adatbázis készlet (C#) kezelése](sql-database-elastic-pool-manage-csharp.md). 
* [Létrehozhat egy rugalmas adatbázis (PowerShell)](sql-database-elastic-pool-create-powershell.md) 
* [Monitor és kezelése egy rugalmas adatbázis készlet (PowerShell)](sql-database-elastic-pool-manage-powershell.md).

## <a name="general-errors"></a>Általános hibák

Az alábbi hibák nem minden előző kategóriába sorolhatók.

|Kódszámú hiba jelenik meg|Súlyosságát|Leírás|
|---:|---:|:---|
|15006|16|<AdministratorLogin>nincs érvényes nevet mert érvénytelen karaktereket tartalmaz.|
|18452|14|Sikertelen bejelentkezés. A bejelentkezés egy nem megbízható tartomány, és nem használható a Windows authentication.% & #x2a; ls (Windows bejelentkezések nem támogatottak az SQL Server jelen verzióban.)|
|18456|14|Felhasználó sikertelen bejelentkezés "%. #x2a;ls'.%. & #x2a ls % & #x2a; ls (a bejelentkezési felhasználó nem sikerült"% & #x2a; ls". A jelszó módosítása sikertelen volt. A jelszó módosítása bejelentkezés során nem támogatott az SQL Server jelen verzióban.)|
|18470|14|Sikertelen bejelentkezés felhasználó "% & #x2a; ls". OK: A fiók disabled.% & #x2a; ls|
|40014|16|Több adatbázisokat a azonos műveletben nem használhatók.|
|40054|16|A csoportosított tárgymutató nélkül táblázatok nem támogatottak az SQL Server ezen verziójában. Csoportosított index létrehozása, és próbálkozzon újra.|
|40133|15|Ez a művelet nem támogatott SQL Server verziójában.|
|40506|16|Ez a verzió az SQL Server megadott biztonsági AZONOSÍTÓK érvénytelen.|
|40507|16|"%. & #x2a; ls' paraméterekkel SQL Server e verziójában nem hívhatók.|
|40508|16|Kimutatás használata nem támogatott adatbázisok közötti váltáshoz. Új kapcsolatot használja egy másik adatbázishoz való csatlakozáshoz.|
|40510|16|Kimutatás "% & #x2a; ls" SQL Server e verziójában nem támogatott|
|40511|16|Beépített függvény "% & #x2a; ls" SQL Server e verziójában nem támogatott.|
|40512|16|Az SQL Server e verziójában nem támogatott elavult szolgáltatás "%ls".|
|40513|16|Kiszolgáló változó "% & #x2a; ls" SQL Server e verziójában nem támogatott.|
|40514|16|az SQL Server e verziójában nem támogatott "%ls".|
|40515|16|Hivatkozás az adatbázis és/vagy a kiszolgáló nevét a "% & #x2a; ls" SQL Server e verziójában nem támogatott.|
|40516|16|Globális temp objektumok az SQL Server e verziójában nem támogatottak.|
|40517|16|Kulcsszó vagy a kimutatás lehetőséget "% & #x2a; ls" SQL Server e verziójában nem támogatott.|
|40518|16|DBCC parancs "% & #x2a; ls" SQL Server e verziójában nem támogatott.|
|40520|16|Az SQL Server e verziójában nem támogatott S_MSG biztonságos osztály "%".|
|40521|16|A kiszolgáló hatókör SQL Server e verziójában nem támogatott S_MSG biztonságos osztály "%".|
|40522|16|Egyszerű "% & #x2a; ls" adatbázistípus SQL Server e verziójában nem támogatott.|
|40523|16|Implicit felhasználók "% & #x2a; ls" létrehozása az SQL Server e verziójában nem támogatott. Explicit módon létrehozása a felhasználó használat előtt.|
|40524|16|Adattípus "% & #x2a; ls" SQL Server e verziójában nem támogatott.|
|40525|16|"%.Ls" a nem támogatott SQL Server verziójában.|
|40526|16|"%. & #x2a; ls' sorhalmazon szolgáltató az SQL Server e verziójában nem támogatott.|
|40527|16|Csatolt kiszolgáló az SQL Server e verziójában nem támogatottak.|
|40528|16|Felhasználók tanúsítványok, aszimmetrikus kulcsok vagy Windows bejelentkezések SQL Server e verziójában nem lehet rendelni.|
|40529|16|Beépített függvény "% & #x2a; ls" megszemélyesítési az SQL Server e verziójában nem támogatott környezetben.|
|40532|11|Nem nyitható meg a kiszolgáló "% & #x2a; ls" a bejelentkezést igénylő. A bejelentkezési sikertelen volt.|
|40553|16|A munkamenet fölösleges memóriahasználat miatt leállt. Próbálja meg a lekérdezés kevesebb sorok feldolgozása módosítása.<br/><br/>*Megjegyzés:* Számának csökkentése `ORDER BY` és `GROUP BY` a Transact-SQL-kódot a műveletek csökkentheti a lekérdezés memória követelményeinek.|
|40604|16|Nem nem létrehozása/ALTER ADATBÁZIST, mert túllépte a kiszolgáló kvóta volna.|
|40606|16|Adatbázisok csatolása SQL Server e verziójában nem támogatott.|
|40607|16|Windows bejelentkezések az SQL Server e verziójában nem támogatottak.|
|40611|16|Kiszolgálók legfeljebb 128 tűzfalszabályokat definiálva van.|
|40614|16|Szabály kezdő IP-címe nem haladhatja meg a záró IP-cím.|
|40615|16|Nem nyitható meg a kiszolgáló a bejelentkezési adatokat kéri le "{0}". Ügyfél IP-címet "{1}" nem engedélyezett a kiszolgáló elérésére.  Ahhoz, hogy az access, az SQL-adatbázis portálon használja, vagy sp_set_firewall_rule futtassa a fő adatbázist az adott IP-cím vagy a cím tartomány tűzfal szabályt szeretne létrehozni.  A módosítás érvénybe léptetéséhez legfeljebb öt percig is eltarthat.|
|40617|16|A tűzfal szabály neve kezdődik <rule name> túl hosszú. Maximális hossza 128.|
|40618|16|A tűzfal szabály neve nem lehet üres.|
|40620|16|A bejelentkezési felhasználó nem sikerült "% & #x2a; ls". A jelszó módosítása sikertelen volt. A jelszó módosítása bejelentkezés során az SQL Server e verziójában nem támogatott.|
|40627|20|A kiszolgáló "{0}" és az adatbázis "{1}" művelet folyamatban van. Várjon néhány percet, mielőtt megpróbálja újra.|
|40630|16|Nem sikerült a jelszó érvényességi. A jelszó nem felel meg házirend mert túl rövid.|
|40631|16|A megadott jelszava túl hosszú. A jelszó van, hogy legfeljebb 128 karaktert.|
|40632|16|Nem sikerült a jelszó érvényességi. A jelszó nem felel meg házirend mivel az nem elég bonyolult.|
|40636|16|Nem használhatja egy fenntartott adatbázis neve "% & #x2a; ls" az ehhez a művelethez.|
|40638|16|Érvénytelen előfizetés azonosítója < előfizetés azonosítója >. Előfizetés nem létezik.|
|40639|16|Kérés nem felel meg a séma: <schema error>.|
|40640|20|A kiszolgáló kivételhiba ütközött.|
|40641|16|A megadott helyen érvénytelen.|
|40642|17|A kiszolgáló jelenleg elfoglalt. Próbálja meg később újra.|
|40643|16|A megadott x-ms-verzió élőfej érték érvénytelen.|
|40644|14|A megadott előfizetés való hozzáférés engedélyezése nem sikerült.|
|40645|16|Kiszolgálónév <servername> nem lehet üres vagy null. Azt is csak tevődik össze kisbetűket "a"-"z", a 0 – 9-es szám és a kötőjel. A kötőjel nem vezet vagy nyomkövetési nevét.|
|40646|16|Előfizetés azonosítója nem lehet üres.|
|40647|16|Előfizetés < előfizetés-alapú nincs kiszolgálónév kiszolgálón.|
|40648|17|Túl sok kérések végrehajtott. Kérjük, később újra.|
|40649|16|Érvénytelen tartalomtípus-van megadva. Csak alkalmazás/xml használata támogatott.|
|40650|16|Előfizetés < előfizetés azonosítója > nem létezik vagy nem áll készen a művelethez.|
|40651|16|Nem sikerült kiszolgálót hozhat létre, mert az előfizetés < előfizetés azonosítója > le van tiltva.|
|40652|16|Nem helyezi át vagy kiszolgálót hozhat létre. Előfizetés < előfizetés azonosítója > fog haladhatja meg a kiszolgáló kvóta.|
|40671|17|A kapcsolati hiba az átjáró és az alkalmazáskezelési szolgáltatás között. Kérjük, később újra.|
|40852|16|Adatbázis nem nyitható meg "%. *ls' kiszolgálón "%.* a bejelentkezést igénylő ls'. Az adatbázishoz való hozzáférést csak akkor engedélyezett védelemmel ellátott kapcsolati karakterlánc használatával. Az adatbázis elérése, módosítania kell a kapcsolati karakterláncot, amely tartalmazhat "biztonságos", a kiszolgáló teljesen minősített tartománynév - "kiszolgáló neve".database.windows .net módosítani kell a "kiszolgáló neve".database. `secure`. windows.net.|
|45168|16|Az SQL Azure-rendszer terhelés alatt, és egyetlen kiszolgáló egyidejű DB CRUD műveletek hibafüggvény helyezi (például adatbázis létrehozása). A hibaüzenet megadott kiszolgáló túllépte egyidejű kapcsolatok maximális száma. Próbálja meg később.|
|45169|16|Az SQL azure-rendszer terhelés alatt, és hibafüggvény helyezi a egyidejű server CRUD műveletek egyetlen előfizetéshez számára (például kiszolgálót hozhat létre). Az előfizetés hibaüzenet megadott egyidejű kapcsolatok maximális száma meghaladta, és a kérelem megtagadva. Próbálja meg később.|

## <a name="related-links"></a>Kapcsolódó hivatkozások

- [Azure SQL-adatbázis általános korlátai és útmutató](sql-database-general-limitations.md)
- [Azure SQL-adatbázis erőforrás korlátai](sql-database-resource-limits.md)
