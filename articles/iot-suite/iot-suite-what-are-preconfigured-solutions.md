<properties
 pageTitle="Azure IoT előre megoldások |} Microsoft Azure"
 description="Az Azure IoT leírását a megoldások és a további forrásokra mutató hivatkozásokat tartalmazó architektúráját előre."
 services=""
 suite="iot-suite"
 documentationCenter=""
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-suite"
 ms.devlang="na"
 ms.topic="get-started-article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="08/09/2016"
 ms.author="dobett"/>

# <a name="what-are-the-azure-iot-suite-preconfigured-solutions"></a>Mik azok az Azure IoT csomagja előre megoldások?

Az előre beállított Azure IoT csomagja megoldások olyan gyakori Azure-előfizetése segítségével telepítheti IoT megoldás mintázatok példányait. Az előre definiált megoldások is használhatók:

- Kiindulási pontként saját IoT megoldások.
- További információt a gyakori mintázatok IoT megoldás tervezése és fejlesztés.

Minden egyes előre beállított megoldás, és a teljes, a végpontok közötti megvalósítás által szimulált eszközök létrehozásához telemetriai.

Nemcsak a üzembe helyezése, és a megoldások fut Azure-ban, letöltheti a teljes forráskód és majd testreszabása és bővítése a megoldást igényeknek IoT.

> [AZURE.NOTE] Az előre definiált megoldások telepítéséhez látogasson el [A Microsoft Azure IoT Suite][lnk-azureiotsuite]. A cikk [az előre beállított IoT megoldások – első lépések] [ lnk-getstarted-preconfigured] telepítése és futtatása a megoldások közül további információt tartalmaz.

A következő táblázat mutatja, hogy hogyan feleltesse meg a megoldások szolgáltatások IoT:

| Megoldás | Adatok bevitel | Eszköz azonosító | Parancs és a vezérlő | Szabályok és műveletek | Prediktív Analytics |
|------------------------|-----|-----|-----|-----|-----|
| [Távoli figyelése][lnk-getstarted-preconfigured] | igen | igen | igen | igen | -   |
| [Prediktív karbantartása][lnk-predictive-maintenance] | igen | igen | igen | igen | igen |

- *Adatok bevitel*: az adatok a méretezés a felhőbe bejövő adatok.
- *Eszköz identitás*: kezelheti az egyedi azonosító minden csatlakoztatott eszköz.
- *Parancs és a vezérlőelem*: az eszközre néhány művelet végrehajtása a okozhat a felhőből üzeneteket küldeni egy eszközt.
- *Szabályok és műveletek*: A megoldás háttér szabályok segítségével meghatározott eszköz a felhőbe adatokat működésbe lépnek.
- *Prediktív analytics*: A megoldás háttér vonatkozik elemzi a eszköz a felhőbe adatok előrejelzésére, amikor a meghatározott műveletekhez kerüljön sor. Ha például elemzése repülőgép motor telemetriai határozza meg, amikor motor karbantartási esedékes.

## <a name="remote-monitoring-preconfigured-solution-overview"></a>Távoli előre figyelés megoldás – áttekintés

Azt választotta, ez a cikk a távoli felügyeleti előre beállított megoldást megvitatása, mert azt szemlélteti, hogy hány közös képi elemek megosztása más megoldást.

A következő diagramon azt szemlélteti, hogy a távoli felügyeleti megoldást fő elemeivel. Az alábbi szakaszokban további információt a ezeket az elemeket.

![Távoli figyelés előre megoldási][img-remote-monitoring-arch]

## <a name="devices"></a>Eszközök

Amikor rendszerbe állítják a távoli felügyeleti előre beállított megoldást, négy szimulált-e a megoldás egy hűtési eszközt imitáló előre kiépített. Ezekre az eszközökre szimulált rendelkezik beépített hőmérséklet és páratartalom modell, amely telemetriai bocsát ki. Ezekre az eszközökre szimulált elemzéssel hatékonyan szemléltethető végpont – a megoldást az adatok és telemetriai és cél kényelmes forrást nyújt parancsok, ha a megoldás használja kiindulási pontként egy egyéni végrehajtásához háttéradatbázist a fejlesztők tartoznak.

Amikor egy eszközt először a távoli felügyeleti előre beállított megoldásban IoT hubhoz csatlakozik, az eszköz tájékoztató üzenetet küldött IoT-hubon keresztül csatlakozott felsorolja a parancsok az eszköz reagálni tud. A távoli felügyeleti előre beállított megoldást a parancsokat a következők: 

- *Ping eszköz*: az eszköz válasza a visszaigazolást erre a parancsra. Ez akkor hasznos, annak ellenőrzése, hogy az eszköz továbbra is aktív, és figyeli a.
- *Telemetriai indítása*: arra utasítja el szeretné elküldeni a telemetriai az eszközt.
- *Telemetriai leállítása*: arra utasítja az eszközt, hogy ne küldjenek telemetriai.
- *Módosítás pont beállítása hőmérsékleti*: az eszköz küld szimulált hőmérsékleti telemetriai értékeket meghatározza. Ez akkor hasznos háttéradatbázist logikai vizsgálathoz.
- *Diagnosztikai Telemetriai*: Ha az eszköz küldje el a külső hőmérsékleti telemetriai szabályozza.
- *Módosítás az eszköz állapotát*.: eszköz állam metaadat-tulajdonságok az eszköz jelentések állítja be. Ez akkor hasznos háttéradatbázist logikai vizsgálathoz.

A megoldás, hogy az azonos telemetriai elhelyezése és ugyanazok a parancsok válaszol is adhat további szimulált eszközök. 

## <a name="iot-hub"></a>IoT központi

Előre beállított megoldásban IoT központi példány felel meg az *Átjáró a Felhőbeli* egy tipikus [IoT megoldási][lnk-what-is-azure-iot].

Egy IoT központi telemetriai kap egy végpontot, az eszközök. Egy IoT központi kezel eszköz adott végpontok hol az egyes eszközök meghallgathatja a parancsok küldött el is.

Az IoT-központban elérhetővé teszi a kapott telemetriai – a szolgáltatás mellett telemetriai, olvassa el a végpontot.

## <a name="azure-stream-analytics"></a>Azure Értékáram-elemzés

Az előre beállított megoldás használ három [Azure Értékáram-elemzés] [ lnk-asa] (ASA) feladatok az telemetriai adatfolyam a eszközökről szűrése:


- *DeviceInfo feladat* - exportálja az adatokat egy esemény-hubon keresztül csatlakozott eszköz regisztrációs adott üzeneteit, először csatlakozik egy eszközt, vagy **módosítása az eszköz állapotát** menüparancs válaszként küldeni a megoldás eszköz beállításjegyzék (DocumentDB adatbázis). 
- *Telemetriai feladat* - összes nyers telemetriai küld az Azure blob-tárolóhoz hideg tárolására, és a megoldás irányítópulton megjelenítő telemetriai összesítések számítja ki.
- *Szabályok feladat* - szűri az értékeket haladja meg a minden olyan szabály küszöbértékek exportálja az adatokat, esemény-hubon keresztül csatlakozott a telemetriai falának. Szabály akkor indul el, ha a megoldás portál irányítópult nézetben jeleníti meg az esemény, mint a riasztás előzmények tábla egy új sort, és elindítja a szabályok és műveletek nézeteket a megoldást portál definiált beállítások alapján művelet.

Előre beállított megoldásban ASA feladatok **IoT megoldás biztonsági vége** részét képezik egy tipikus [IoT megoldási][lnk-what-is-azure-iot].

## <a name="event-processor"></a>Esemény processzor

Előre beállított megoldásban az esemény processzor **IoT megoldás biztonsági vége** részét képezi egy tipikus [IoT megoldási][lnk-what-is-azure-iot].

A **DeviceInfo** és **szabályok** ASA feladatok küldje el a kimeneti esemény veszik fel a kézbesítési egyéb vissza vége szolgáltatásokhoz szükséges. A megoldás használ egy [EventPocessorHost] [ lnk-event-processor] példánya fut a [WebJob][lnk-web-job], ezek esemény hubok az üzenetek olvasására. A **EventProcessorHost** a **DeviceInfo** az adatokat használja az eszköz adatainak a DocumentDB adatbázist, és a **szabályok** az adatokat használja a logika alkalmazás és a frissítési értesítések megjelenítése a megoldást portálon meghívása.

## <a name="device-identity-registry-and-documentdb"></a>Eszköz identitás beállításjegyzék és DocumentDB

Minden IoT központi tartalmaz egy [eszköz identitás beállításjegyzék] [ lnk-identity-registry] eszköz billentyűk tárol. IoT központi használja ezt az információt eszközök hitelesítő - regisztrálni kell egy eszközt, és van érvényes kulcsot, csak a központi csatlakozhat.

Ez a megoldás eszközök, például állapotukban, az általuk támogatott parancsok és más metaadatok további adatait tárolja. A megoldás a megoldás-specifikus eszköz adatainak tárolására DocumentDB adatbázis használja, és a megoldás portál megjelenítése és szerkesztése az adatbázis DocumentDB adatokat ad vissza.

A megoldást is kell megőrizze az információk szinkronizálja a DocumentDB adatbázis tartalmát az eszköz identitás beállításjegyzékbeli. A **EventProcessorHost** **DeviceInfo** adatfolyam analytics projekt adatainak kezelése a szinkronizálás használja.

## <a name="solution-portal"></a>Megoldás portál

![Megoldás irányítópult][img-dashboard]

A megoldás portál egy webes felhasználói Felületét, amely az előre beállított megoldás részeként telepíti, a felhőben. Lehetővé teszi:

- Telemetriai és riasztás előzményeinek megtekintése az irányítópulton.
- Létrehozhatnak új eszközökön.
- Kezelése, és figyelemmel követheti a eszközöket.
- Küldje el parancsok bizonyos eszközöket.
- Szabályok és a tevékenységek kezelése.

Előre beállított megoldásban a megoldást portál űrlapok **IoT megoldás biztonsági befejező** része és részét képező **feldolgozása és az Üzleti kapcsolatszolgáltatások** a szokásos [IoT megoldási][lnk-what-is-azure-iot].

## <a name="next-steps"></a>Következő lépések

IoT megoldás architektúrákban kapcsolatos további tudnivalókért lásd: [Microsoft Azure IoT szolgáltatások: hivatkozás architektúra][lnk-refarch].

Most már tudja, hogy milyen előre beállított megoldást, a, *távoli figyelés* előre megoldást üzembe helyezésével használatának megkezdéséhez: [első lépések az előre definiált megoldások][lnk-getstarted-preconfigured].

[img-remote-monitoring-arch]: ./media/iot-suite-what-are-preconfigured-solutions/remote-monitoring-arch1.png
[img-dashboard]: ./media/iot-suite-what-are-preconfigured-solutions/dashboard.png
[lnk-what-is-azure-iot]: iot-suite-what-is-azure-iot.md
[lnk-asa]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-event-processor]: ../event-hubs/event-hubs-programming-guide.md#event-processor-host
[lnk-web-job]: ../app-service-web/web-sites-create-web-jobs.md
[lnk-identity-registry]: ../iot-hub/iot-hub-devguide-identity-registry.md
[lnk-predictive-maintenance]: iot-suite-predictive-overview.md
[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk-refarch]: http://download.microsoft.com/download/A/4/D/A4DAD253-BC21-41D3-B9D9-87D2AE6F0719/Microsoft_Azure_IoT_Reference_Architecture.pdf
[lnk-getstarted-preconfigured]: iot-suite-getstarted-preconfigured-solutions.md