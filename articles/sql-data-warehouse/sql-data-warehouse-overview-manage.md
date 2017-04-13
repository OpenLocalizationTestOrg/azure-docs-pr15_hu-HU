<properties
   pageTitle="Azure SQL-adatraktár az adatbázisok kezelése |} Microsoft Azure"
   description="Áttekintése adatraktár SQL-adatbázisok kezelésében. DWUs és méretezési teljesítmény elérése érdekében a lekérdezési teljesítmény, jó biztonsági házirendek létrehozása és az adatbázis visszaállítása az adatvesztést vagy egy területi üzemszünetek hibaelhárítási-kezelő eszközök tartalmazza."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="barbkess"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/16/2016"
   ms.author="barbkess;sonyama;"/>

# <a name="manage-databases-in-azure-sql-data-warehouse"></a>Azure SQL-adatraktár az adatbázisok kezelése

SQL-adatraktár automatizálja számos tulajdonságát az adatbázisok kezelése. Például ha át kívánja méretezni a teljesítményt, csak kell módosítsa a megfelelő szintű számítási erőforrások kifizetéséhez és hagyja meg méretezése és méretezését vissza az összes munkavégzés SQL adatraktár. 

Kétségkívül érdemes figyelni a terhelést a teljesítmény igényeinek megfelelően azonosítása, valamint a hosszabb ideig futó lekérdezések – Problémamegoldás. Akkor is kell néhány biztonsági feladatokat a felhasználók és a bejelentkezési engedélyeinek kezelése.

Az Áttekintés terjed ki ezeket a szempontokat SQL adatraktár kezelését.

- Kezelő eszközök
- Méretezés számítási
- Szünet és az önéletrajz
- Ajánlott eljárások a teljesítmény
- Lekérdezés figyelése
- Biztonsági
- Biztonsági mentési és visszaállítási

## <a name="management-tools"></a>Kezelő eszközök

Számos eszköz segítségével adatraktár SQL-adatbázisok kezelése. Adatbázisok kezelése során, az egyes: a feladat végrehajtásához az eszköz beállításainak fejleszt lesz.

### <a name="azure-portal"></a>Azure portál
Az [Azure portál][] webes portált, ahol létrehozása, módosítása, és adatbázisok törlése és figyelheti az adatbázis-erőforrások. Ez az eszköz kiválóan akkor, ha éppen csak most ismerkedik az Azure-kisszámú adatok raktári adatbázisok kezelése, vagy kell elvégeznie gyorsan.

Az első lépések az Azure-portálra, olvassa el a [egy SQL-adatraktár (Azure portal) létrehozása][]című témakört.

### <a name="sql-server-data-tools-in-visual-studio"></a>Az SQL Server Data Tools a Visual Studio
[Az SQL Server Data Tools][] (SSDT) a Visual Studióban lehetővé teszi, hogy csatlakozzon, kezelése és az adatbázisok fejlesztését. Ha ismeri a Visual Studio vagy más integrált fejlesztői környezet (IDEs) alkalmazásfejlesztő, próbáljon SSDT Visual Studio.

SSDT az SQL Server objektum Explorer, amely lehetővé teszi, hogy ábrázolása, a csatlakozás és a végrehajtás adatraktár SQL-adatbázisok parancsprogramok tartalmazza. Gyorsan SQL adatraktár szeretne csatlakozni, akkor is egyszerűen a **Megnyitás** gombra a Visual Studióban parancssávon, ha az adatbázis adatainak megtekintése a klasszikus Azure-portálon.  

Első lépések a Visual Studióban SSDT, olvassa el a [Lekérdezés Azure SQL adatraktár a Visual Studio][].

### <a name="command-line-tools"></a>Parancssori eszközök
Parancssori eszközök állnak a feladatok automatizálása ideális.  A PowerShell és sqlcmd kétféleképpen remek a folyamatok automatizálása.  Azt javasoljuk, hogy ezeket az eszközöket logikai kiszolgálók nagyszámú kezelésére, és üzembe helyezése erőforrás módosítások üzemi környezetben, a szükséges feladatok parancsfájl alapú, és kattintson az automatikus.

### <a name="dynamic-management-views"></a>Dinamikus kezelése nézetek 

DMVs a be-és vaj SQL adatraktár kezelése. Szinte minden információt, amely megjeleníti a portálon DMVs támaszkodik. Az SQL-adatok raktári DMVs listájának megtekintéséhez lásd: az [SQL adatraktár rendszer nézetek][].

Első lépésként olvassa el a [Csatlakozás sqlcmd lekérdezésre][], és [egy adatbázis (PowerShell) létrehozása][]című témakört.

## <a name="scale-compute"></a>Méretezés számítási

Az SQL-adatraktár gyorsan méretezheti, vagy újra növelésével vagy csökkentésével számítási erőforrások Processzor, memória és I/O sávszélesség teljesítmény. Ha át kívánja méretezni a teljesítményt, kell tennie mindössze módosítsa az adatok raktári egységek számát (DWUs) SQL adatraktár osztja ki az adatbázishoz. SQL-adatraktár gyors végrehajtja a módosítást, és az alapul szolgáló változtatásainak hardveres és szoftveres kezeli.

Méretezés DWUs kapcsolatos további információért lásd: a [Méretezés teljesítmény][].

##  <a name="pause-and-resume"></a>Szünet és az önéletrajz

Költségeket mentéséhez mutasson, és folytathatja a számítási erőforrások igény szerinti. Például ha nem használ-e az adatbázis az éjszakai és a hétvégén, akkor is mutasson az adott idő alatt, és folytassa azt a nap során. Nem kell fizetnie DWUs közben az adatbázis fel van függesztve.

További tudnivalókért lásd: a [Szünet számítja ki][], és az [Önéletrajz számítja ki][].

## <a name="performance-best-practices"></a>Ajánlott eljárások a teljesítmény

Első lépések az új technológiát, amikor a tippek és trükkök az elejétől legjobb jobbra működő felfedezése mentheti, sok idő.  Ajánlott eljárások a témakörök közül számos egész találja.

A legfontosabb szempontok sok összefoglalását a terhelést fejlesztésekor című témakör tartalmazza [SQL adatok raktári gyakorlati tanácsokat][].

## <a name="query-monitoring"></a>Lekérdezés figyelése

Előfordul, hogy a lekérdezés fut túl hosszú, de nem tudja biztosan, hogy melyik van a probléma oka. SQL-adatraktár megállapítása, hogy melyik lekérdezés túl sokáig tart használható dinamikus kezelése nézetek (DMVs) tartalmaz. 

Hosszú futó lekérdezések, olvassa el [a terhelést DMVs használatával figyelheti][].

## <a name="security"></a>Biztonsági

Egy biztonságos rendszere őrizni, a értesítésben kell, és ezzel az illetéktelen hozzáférést bármilyen típusú szüntetni. A biztonsági rendszert kell győződjön meg róla, tűzfalszabályokat helyen, hogy csak az engedélyezett IP-címek csatlakozhat. Felhasználói hitelesítő adatokat a megfelelő hitelesítési szüksége van. Egy felhasználó csatlakozik az adatbázishoz, a felhasználónak kell csak után engedélyek minimális számos műveletek elvégzéséhez. Védik az adatokat, hogy a titkosítási is használhatja. Fontos is, hogy naplózási és nyomon követése, események is végig, ha a gyanús tevékenységeket.

Ha érdeklik a biztonságkezelési, címsor fölé a [biztonsági – áttekintés][].

## <a name="backup-and-restore"></a>Biztonsági mentési és visszaállítási

Az adatok megbízható backps problémákat része alapvető bármilyen gyártási adatbázist. SQL-adatraktár továbbra is az adatok biztonságos mentésével automatikusan az aktív adatbázisok rendszeres időközönként. A biztonsági mentése az esetek, amikor az Ön által megsérült, az adatok vagy véletlenül kihagyott az adatok vagy az adatbázis visszaállítása teszi lehetővé.  Az adatok biztonsági ütemezés adatmegőrzési szabály, és hogyan visszaállít egy adatbázist, lásd: [pillanatkép visszaállítani][].

## <a name="next-steps"></a>Következő lépések
Jó adatbázisterv elvek megkönnyíti az adatbázisok az SQL adatraktár kezeléséhez használja. Bővebb, címsor fölé a [fejlesztés áttekintése][].

<!--Image references-->

<!--Article references-->
[Hozzon létre egy SQL-adatraktár (Azure Portal)]: sql-data-warehouse-get-started-provision.md
[Hozzon létre egy adatbázist (PowerShell)]: sql-data-warehouse-get-started-provision-powershell
[connection]: sql-data-warehouse-develop-connections.md
[Lekérdezés Azure SQL-adatraktár a Visual Studio]: sql-data-warehouse-query-visual-studio.md
[Csatlakozás és a lekérdezés sqlcmd]: sql-data-warehouse-get-started-connect-sqlcmd.md
[Fejlesztési – áttekintés]: sql-data-warehouse-overview-develop.md
[Figyelheti a terhelést DMVs használatával]: sql-data-warehouse-manage-monitor.md
[Szünet számítási]: sql-data-warehouse-manage-compute-overview.md#pause-compute-bk
[Pillanatkép visszaállítása]: sql-data-warehouse-restore-database-overview.md
[Önéletrajz számítási]: sql-data-warehouse-manage-compute-overview.md#resume-compute-performance-bk
[Méretezés teljesítmény]: sql-data-warehouse-manage-compute-overview.md#scale-performance-bk
[Biztonsági – áttekintés]: sql-data-warehouse-overview-manage-security.md
[Ajánlott eljárások a SQL-adatok raktári]: sql-data-warehouse-best-practices.md
[SQL-adatraktár rendszer nézetek]: sql-data-warehouse-reference-tsql-system-views.md

<!--MSDN references-->
[Az SQL Server Data Tools]: https://msdn.microsoft.com/library/mt204009.aspx

<!--Other web references-->
[Azure portál]: http://portal.azure.com/
