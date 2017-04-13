<properties
 pageTitle="HA IoT központi és DR |} Microsoft Azure"
 description="Funkcióval segíti a könnyen hozzáférhető IoT megoldások katasztrófa össze a helyreállítási lehetőségeket ismerteti."
 services="iot-hub"
 documentationCenter=""
 authors="fsautomata"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="02/03/2016"
 ms.author="elioda"/>

# <a name="iot-hub-high-availability-and-disaster-recovery"></a>IoT központi magas rendelkezésre állásának és katasztrófa helyreállítás

Azure szolgáltatásként IoT központi itt létszámcsökkentések használatával Azure régió szintre, de a megoldást által igényelt további anélkül magas elérhetőség (HA). Ezeken kívül Azure számos szolgáltatásokat, amelyek segítségével katasztrófa (DR) helyreállítási lehetőségeket vagy határokon-régió elérhetősége megoldások létrehozásához, ha szükséges. Közbeni kell tervezése és a megoldások kihasználhatja az ezek DR szolgáltatások, ha azt szeretné, hogy globális, határokon-régió magas elérhetősége eszközök vagy a felhasználók számára. [Azure üzleti folytonosságot technikai útmutató](../resiliency/resiliency-technical-guidance.md) cikkből üzemkészségének és DR Azure-ban a beépített funkciók. A [vészhelyreállítás és Azure-alkalmazásokhoz magas elérhetősége][] papír Azure alkalmazások érhet el, HA és DR stratégiák architektúra útmutatást nyújt.

## <a name="azure-iot-hub-dr"></a>Azure IoT központi DR
HA belüli-régió, kívül IoT központi feladatátvevő mechanizmusok vészhelyreállítás felhasználói beavatkozás nélkül igénylő valósítja meg. IoT központi DR önálló által kezdeményezett és a helyreállítási idő célkitűzés 2-26 óra (RTO), a következő helyreállítási pont célkitűzések (RPOs).

| Funkciók | KÉSZLETBEN |
| ------------- | --- |
| A beállításjegyzék és a kommunikáció műveletekhez szolgáltatáselérhetőség | Elvesztésének CName |
| Személyes adatok eszköz identitás beállításjegyzékben | 0-5 perc az adatok elvesztését |
| Üzenetek eszköz a felhőbe | Minden olvasatlan levél elvesznek. |
| Műveletek üzenetek figyelése | Minden olvasatlan levél elvesznek. |
| Üzenetek cloud-eszköz | 0-5 perc az adatok elvesztését |
| Felhőalapú-eszköz visszajelzések várólista | Minden olvasatlan levél elvesznek. |

## <a name="regional-failover-with-iot-hub"></a>A központi IoT területi feladatátvevő

Egy teljes telepítési topológiák a IoT megoldások kezelése a hatókör, a jelen cikk, de nagy elérhetőségét, és a helyreállítás azt veszi figyelembe a *regionális feladatátvevő* telepítési modell céljából kívülre esik.

Területi feladatátvevő adatmodell a megoldást háttér elsősorban egy adatközponthoz helyen fut, de egy további IoT központi, és a hátsó vége van telepítve egy másik adatközponthoz helyen feladatátvevő célokra abban az esetben az IoT-központban, az elsődleges adatközpontban szenved egy üzemszünetek, vagy a hálózati kapcsolat az eszközről a elsődleges adatközponthoz valami megszakad. Eszközök egy másodlagos szolgáltatás végpontjának használnak, amikor az elsődleges átjáró nem érhető el. Egy keresztté-régió feladatátvevő lehetőséggel megoldás elérhetősége javítható túl az egyetlen terület magas elérhető.

Magas szintű területi feladatátvevő modell megvalósítását a IoT-központban, a következőkre lesz szüksége.

* **A másodlagos IoT központi és a továbbítás logika eszköz**: a szolgáltatási zavarok elsődleges régiójában, amíg eszközök kell indítani a másodlagos régió csatlakozik. Az állapot-et természeti a legtöbb érintett szolgáltatások ismeretében megoldás rendszergazdáknak indíthatja el a közötti régió feladatátvételi folyamat gyakori. Legegyszerűbben úgy, hogy az új végpontot, az eszközök, kommunikáció a folyamat irányításának megőrzésével, hogy rendszeresen az aktuális aktív végpontot *segítõ* szolgáltatás. A segítõ szolgáltatás lehet egy egyszerű webalkalmazás replikált és tartani elérhető DNS-átirányítást módszereket (például [Azure forgalom Manager][]).
* A **beállításjegyzék-replikációs azonosító** – annak érdekében, hogy használható, a másodlagos IoT központban tartalmaznia kell az eszköz identitások, hogy a megoldás csatlakozhat. A megoldás kell biztonsági másolatok geo replikált eszköz identitások hagyja, és töltse fel őket a másodlagos IoT hubon keresztül csatlakozott az aktív végpontot eszközök váltása előtt. Az eszköz identitás exportálása a IoT központi funkció nagyon hasznos a környezetben. További információ a [IoT központi-Fejlesztőeszközök útmutató - identitás beállításjegyzék][]című témakörben.
* Az állapot és az adatok, a másodlagos webhelyen létrehozott át kell telepíteni az összes biztonsági **logika egyesítése** – az elsődleges régió elérhetővé újra, az elsődleges területre. Ez leginkább eszköz identitások és vonatkozik alkalmazás metaadat-adatokat, amelyeket egyesíteni kell az elsődleges IoT-központban és más alkalmazás-specifikus tárolja az elsődleges területen. Ebben a lépésben leegyszerűsítése általában ajánlott idempotent műveletekre használható. Ez – a-hatásai nemcsak az esetleges egységes terjesztési események, hanem az ismétlődő elemeket vagy események sorrendben kimenő kézbesítési ikonméretűre kicsinyítése. Ezeken kívül az alkalmazás logika elviselni az esetleges következetlenségekre, illetve "kissé" dátum-állapot ki kell megtervezni. Ez a további szükséges időt a rendszer "javítandó" miatt alapuló helyreállítási pont célkitűzések (Készletben).

## <a name="next-steps"></a>Következő lépések

Kövesse ezeket a hivatkozásokat, ha többet szeretne tudni az Azure IoT központi:

- [Első lépések a IoT hubok (oktatóprogram)][lnk-get-started]
- [Mi az Azure IoT központi?][]

[Vészhelyreállítás és Azure-alkalmazásokhoz magas elérhetősége]: ../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md
[Azure Business Continuity Technical Guidance]: https://azure.microsoft.com/documentation/articles/resiliency-technical-guidance/
[Azure forgalom Manager]: https://azure.microsoft.com/documentation/services/traffic-manager/
[IoT központi Fejlesztőeszközök útmutató - identitás beállításjegyzék]: iot-hub-devguide-identity-registry.md

[lnk-get-started]: iot-hub-csharp-csharp-getstarted.md
[Mi az Azure IoT központi?]: iot-hub-what-is-iot-hub.md
