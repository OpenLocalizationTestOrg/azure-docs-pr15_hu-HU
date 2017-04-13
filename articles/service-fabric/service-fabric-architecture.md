<properties
   pageTitle="Szolgáltatás háló architektúra |} Microsoft Azure"
   description="Szolgáltatás háló az a felhő méretezhető, megbízható és könnyen felügyelt alkalmazások létrehozásához használt elosztott rendszerek platformot. Ez a cikk bemutatja a szolgáltatás háló architektúráját."
   services="service-fabric"
   documentationCenter=".net"
   authors="rishirsinha"
   manager="timlt"
   editor="rishirsinha"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="06/09/2016"
   ms.author="rsinha"/>

# <a name="service-fabric-architecture"></a>Szolgáltatás háló architektúra

A réteges alrendszerek szolgáltatás háló épül fel. Ezek az alrendszerek szeretne írni, amelyek alkalmazások engedélyezése:

* Könnyen hozzáférhető
* Méretezhető
* Kezelhető
* Testable

Az alábbi ábra mutatja a fő alrendszerek szolgáltatás készült.

![A szolgáltatás háló architektúra diagram](media/service-fabric-architecture/service-fabric-architecture.png)

Elosztott rendszerben az azt jelenti, hogy fürt csomópontjai közötti biztonságos kommunikáció elengedhetetlen. Az objektumhalomban alján található az átviteli alrendszer, amely magában foglalja a csomópontok közötti biztonságos kommunikáció. A szállítás feletti alrendszer az összevonási alrendszer különböző csomópontjait fürtök a (más néven fürt) egy entitás be, hogy a szolgáltatás háló hibák feltárása, végezze el a vezető election és egységes útválasztás nyújt helyezkedik el. A megbízhatóság alrendszer, az összevonási alrendszer fölött réteges feladata szolgáltatás háló szolgáltatások – például replikációs, az erőforrás-kezelés és feladatátvevő mechanizmusok megbízhatóságát. Az összevonási alrendszer is, amelyek kezelése az életciklus-alkalmazás egyetlen csomóponton az szolgáltatója és aktiválási alrendszer alapjául szolgáló. Az adatkezelési alrendszer kezelése az életciklus az alkalmazások és szolgáltatások. A tesztelhetőségi alrendszer tesztelje a szolgáltatások szimulált hibák keresztül, előtt és után az alkalmazások és szolgáltatások bevezetéshez gyártási környezetekben alkalmazásfejlesztő segítségével. Háló szolgáltatás lehetővé teszi a szolgáltatási helyeket a kapcsolati alrendszer keresztül feloldásához. Az alkalmazás csak akkor érhető el a fejlesztők programozási modelljei is réteges ezek az együtt az ahhoz, hogy szerszámok alkalmazásmodell alrendszerek fölött.

## <a name="transport-subsystem"></a>Átviteli alrendszer
Az átviteli alrendszer végrehajtja a pontok közötti kommunikáció datagramcsatornából. A csatorna használható szolgáltatás háló fürt belül és a szolgáltatás háló fürt és az ügyfelek közötti kommunikációt. Egyirányú támogat és alapjául szolgáló szórás és a csoportos végrehajtása az összevonási rétegben kérés / válasz kommunikációs mintázatok. Az átviteli alrendszer titkosítja kommunikációs X509 használatával tanúsítványok vagy a Windows biztonsági. Ez az alrendszer szolgáltatás háló használatú, és nem a fejlesztők számára alkalmazásprogramozási közvetlenül elérhető.

## <a name="federation-subsystem"></a>Összevonás alrendszer
Annak érdekében, hogy oka kapcsolatos meg csomópontok elosztott rendszerben, a rendszer egységes nézetének van szükség. Az összevonási alrendszer a kapcsolati primitívek az átviteli alrendszer által biztosított használ, és a különböző csomópontok összefűzi azt is OK kapcsolatos egyetlen egységesített fürt be. Az elosztott rendszerek primitívek szükség szerint az egyéb alrendszerek - hibaészlelési technológia vezető election és egységes útválasztás biztosít. Az összevonási alrendszer elosztott kivonat táblák 128 bites jogkivonat szóközzel fölött jön létre. A alrendszer egy csengetés topológia fölé a csomópontok az egyes csomópontot a kicsengetés a token szóköz csak egy részhalmazát van kiosztva a tulajdonjog hoz létre. A réteg hibaészlelési technológia, használja a bérleti mechanizmusa zúzása szívecskés és döntőbírói alapján. Az összevonási alrendszer is bonyolult illesztés és indulási protokollok csak egyetlen tulajdonosát a jogkivonat létezik bármikor biztosítja. A kitöltés elkötelezett régiók választási és a egységes útválasztási garanciákkal biztosítanak.

## <a name="reliability-subsystem"></a>Megbízhatósága alrendszer
A megbízhatóság alrendszer a módot, elérhetővé teheti szolgáltatás háló szolgáltatás állapotának erősen a _replikációs_, _Feladatátvevő-kezelő_és _Erőforrás-terheléselosztó_való biztosít.

* A replikációs biztosítja, hogy a program automatikusan másodlagos replikák replikált a állapotra vonatkozó módosításokat a elsődleges szolgáltatás kópiában szolgáltatás kópiakészlet elsődleges és másodlagos kópiájába közötti összhangot fenntartása. A replikációs feladata kvórum kezelése a kópiakészlet kópiájába között. Azt a listát a műveletek való replikáció feladatátvevő mértékegységet kommunikáljon, és a konfigurálás agent biztosít a kópiakészlet beállításokkal. A konfiguráció azt jelzi, hogy milyen műveleteket kell lennie replikált kópiák. Szolgáltatás háló egy alapértelmezett replikációs háló replikációs, amelyek segítségével a programozási modell API-t, hogy a szolgáltatás állapota, nagyon a rendelkezésre álló és a megbízható nevű biztosít.
* A Feladatátvevő kezelő biztosítja, hogy csomópontok hozzáadni vagy el vannak távolítva a fürt, amikor a betöltés automatikusan veszi át a rendelkezésre álló csomópontok. Ha nem sikerül a fürt, a a fürt automatikusan újra a szolgáltatás rendelkezésre álljon kópiákat.
* Az erőforrás-kezelő szolgáltatás kópiák helyezi át a fürt hiba tartományt, és biztosítja, hogy minden feladatátvevő egység működési. Az erőforrás-kezelő szolgáltatás erőforrások is egyenlege végig az alapul szolgáló megosztott készlete fürt csomópontok optimális egységes terheléselosztási eléréséhez.

## <a name="management-subsystem"></a>Adatkezelési alrendszer
Az adatkezelési alrendszer végpont szolgáltatás és az alkalmazás életciklus-kezelése biztosít. PowerShell-parancsmagok és felügyeleti API-k lehetővé teszi kiépítése, terjesztése, javítás, frissítése és vonja kiépítése elérhetősége veszteségmentes alkalmazásait. Az adatkezelési alrendszer hajt végre, ez az alábbi szolgáltatások keresztül.

* **Kezelő**: Ez az elsődleges szolgáltatás, hogy hogyan kommunikáljon a Feladatátvevő kezelő használatával a megbízhatóság az alkalmazások elhelyezése a csomópontok a szolgáltatás elhelyezési kényszerek alapján. Az erőforrás-kezelő feladatátvevő alrendszer biztosítja, hogy az érvényességi tartományán soha nem hibás. A kezelő kezeli a vonja kiépítése rendelkezni az alkalmazást az életciklus. A rendszerállapot kezelő annak érdekében, hogy alkalmazás elérhetősége frissítéskor ilyenkor nem vesznek el, a szemantikai állapot szemszögéből integrálódik.
* **Kezelő állapota**: Ez a szolgáltatás lehetővé teszi a rendszerállapot figyelése alkalmazások, a szolgáltatások és a fürt szervezetek. Fürt egységek (például a csomópontok szolgáltatás partíciót és kópiák) jelentheti a rendszerállapot adatait, amelyek a program majd összesíti a központi állapot tárolóba. A rendszerállapot adatait a szolgáltatások és a fürt úgy minden szükséges korrekciós műveletekhez több csomópontok elosztva csomópontok teljes pont igény állapot pillanatképét biztosít. Az állapot alrendszer jelenteni API-k engedélyezése, hogy a lekérdezés az állapot események állapot lekérdezést. A rendszerállapot lekérdezés, API-khoz visszaadja nyers állapot adataihoz, az állapot tároló vagy az összesített értelmezhető egy adott fürt entitás rendszerállapot adatait.
* **Kép áruházból**: Ez a szolgáltatás tárolása és az alkalmazás bináris elosztása biztosít. Ez a szolgáltatás egy egyszerű elosztott fájlrendszer áruházból alkalmazásokat töltenek fel és le hol biztosít.


## <a name="hosting-subsystem"></a>Alrendszer szolgáltatónál
A kezelő tájékoztat arról, hogy mely szolgáltatások szüksége van egy adott csomópont kezelése üzemeltetési alrendszer (minden csomóponton fut). Az üzemeltetési alrendszer majd kezelése az életciklus az alkalmazás a csomóponton. A megbízhatóság és állapot összetevőket győződjön meg arról, hogy a replikák megfelelően kerülnek, és megfelelő kommunikáljon.

## <a name="communication-subsystem"></a>Kapcsolati alrendszer
Ez az alrendszer itt megbízható üzenetkezelés belül fürt- és felfedezése a névhasználati szolgáltatáson keresztül. A névhasználati szolgáltatás szolgáltatásnevek megszünteti a fürt helyre, és lehetővé teszi a felhasználóknak szolgáltatásnevek és a tulajdonságok kezelése. Az névhasználati szolgáltatással ügyfelek biztonságosan kommunikálni a fürt megoldani a szolgáltatás neve, valamint szolgáltatás metaadat-alapú bármely csomópontjának. Egy egyszerű névhasználati ügyfélprogram API-t használ, felhasználói szolgáltatás háló fejleszthet szolgáltatások és ügyfelek alkalmas az aktuális hálózati hely ellentétben csomópont dinamizmus vagy a fürt újra méretező megoldása.

## <a name="testability-subsystem"></a>Tesztelhetőségi alrendszer
Tesztelhetőségi kifejezetten services szolgáltatás háló épül tesztelése eszközök együttese. Az eszközök segítségével egyszerűen jutva értelmes hibák és próba forgatókönyvek számos államok és áttűnések, amely a szolgáltatás fog tapasztalni egészében élettartama összes ellenőrzött és biztonságos módon, és a Futtatás fejlesztői. Tesztelhetőségi is lehetővé teszi a hosszabb azt vizsgálja, hogy is találta futtatásához keresztül különféle lehetséges hibák elérhetősége megtartásával. Ez a próba-a-munkakörnyezetben teszi lehetővé.
