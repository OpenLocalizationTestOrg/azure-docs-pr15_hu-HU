<properties 
   pageTitle="SQL adatbázis katasztrófa helyreállítási gyakorlatokat |} Microsoft Azure" 
   description="További útmutatást és a gyakorlati tanácsok a Azure SQL-adatbázissal, amely segítséget nyújt a katasztrófa helyreállítási gyakorlatokat végrehajtásához megőrzése a misszió kritikus hibák és kimaradások rugalmassá üzleti alkalmazások." 
   services="sql-database" 
   documentationCenter="" 
   authors="anosov1960" 
   manager="jhubbard" 
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-management" 
   ms.date="07/31/2016"
   ms.author="sstein; sashan"/>

#<a name="performing-disaster-recovery-drill"></a>Katasztrófa helyreállítási részletező végrehajtása

Javasoljuk, hogy az alkalmazás készenléti helyreállítási munkafolyamat érvényesség rendszeres időközönként. Az alkalmazás működésének és a következmények ellenőrzése az adatok elvesztését, illetve az állásidőt azzal, hogy feladatátvevő tényezői mérnöki javasolt. Érdemes emellett annak követelménye, a legtöbb iparági szabványok által az üzleti folytonosságot hitelesítő részeként.

Egy katasztrófa helyreállítási részletező elvégzéséhez az alábbiakból áll:

- Amelyek adatok réteg üzemszünetek
- Helyreállítása 
- Alkalmazás integritását bejegyzés helyreállítási ellenőrzése

Hogyan, [az alkalmazás az üzleti folytonosságot verziójához](sql-database-business-continuity.md), a munkafolyamat végrehajtása a részletező eltérőek lehetnek attól függően. Az alábbi azt ismertetik a gyakorlati tanácsok a katasztrófa helyreállítási részletezési Azure SQL-adatbázis környezetében tartja. 

##<a name="geo-restore"></a>GEO-visszaállítása

Egy katasztrófa helyreállítási részletezési tartja az esetleges adatvesztés elkerülése javasoljuk elvégzéséhez a részletező próba környezet használatával hozhat létre egy példányt az éles üzemi környezetben, és ellenőrizze az alkalmazás feladatátvevő munkafolyamat használatával.
 
####<a name="outage-simulation"></a>Üzemszünetek szimulációk

A üzemszünetek hasonlóan törlése, és nevezze át a forrásadatbázis. Ennek hatására alkalmazáshiba-kapcsolatot. 

####<a name="recovery"></a>Helyreállítási

- Ténykedés Geo-visszaállítása az adatbázis be egy másik webkiszolgálóra leírt [Itt](sql-database-disaster-recovery.md). 
- A helyreállított adatbázis(ok) kapcsolódhat, és kövesse a [helyreállítás adatbázis konfigurálása](sql-database-disaster-recovery.md) útmutató elvégzéséhez a helyreállítás alkalmazás beállításainak módosításához.

####<a name="validation"></a>Adatérvényesítési

- Végezze el a részletező ellenőrzése alkalmazás integritását bejegyzés helyreállítási (azaz a csatlakozási_karakterlánc, bejelentkezések, alapvető funkcióit tesztelése vagy más szabványos alkalmazás signoffs eljárások ellenőrzések részét).

##<a name="geo-replication"></a>A replikáció GEO

Az adatbázis Geo-replikációs szolgáltatásával védett a részletező torna magában foglalja a másodlagos adatbázis tervezett áttérni. A tervezett feladatátvételt biztosítja, hogy az elsődleges és másodlagos adatbázisok maradjanak szinkronizálja a szerepkörök bekapcsolásakor. Eltérően a tervezett feladatátvételt ezt a műveletet nem eredményezi az adatok elvesztését, így a részletező végezheti el az éles üzemi környezetben. 

####<a name="outage-simulation"></a>Üzemszünetek szimulációk

A üzemszünetek hasonlóan letilthatja a webalkalmazás vagy a virtuális gép csatlakozik az adatbázishoz. Ez a webes ügyfelek számára a csatlakozási hibák eredményez.

####<a name="recovery"></a>Helyreállítási

- Győződjön meg arról, hogy az alkalmazás konfigurációjának a DR térség mutat a korábbi másodlagos, amely új elsődleges teljes körűen elérhető lesz. 
- A [tervezett feladatátvevő](sql-database-geo-replication-powershell.md#initiate-a-planned-failover) , hogy a másodlagos adatbázis új elsődleges végrehajtása
- Kövesse a [helyreállítás adatbázis konfigurálása](sql-database-disaster-recovery.md) útmutató a helyreállítás befejezéséhez.

####<a name="validation"></a>Adatérvényesítési

- Végezze el a részletező ellenőrzése alkalmazás integritását bejegyzés helyreállítási (azaz a csatlakozási_karakterlánc, bejelentkezések, alapvető funkcióit tesztelése vagy más szabványos alkalmazás signoffs eljárások ellenőrzések részét).


## <a name="next-steps"></a>Következő lépések

- Üzleti folytonosságot forgatókönyvek kapcsolatos további tudnivalókért lásd: a [folyamatosság felhasználási területei](sql-database-business-continuity.md)
- Azure SQL-adatbázis automatikus biztonsági mentés kapcsolatos további tudnivalókért lásd: [az automatikus biztonsági mentés SQL-adatbázis](sql-database-automated-backups.md)
- Az automatikus biztonsági mentés helyreállítási használata című témakörben talál [a szolgáltatás által kezdeményezett biztonsági mentés adatbázis visszaállítása](sql-database-recovery-using-backups.md)
- Gyorsabb helyreállítási lehetőségeket kapcsolatos további tudnivalókért lásd: [A replikáció Geo aktív](sql-database-geo-replication-overview.md)  
