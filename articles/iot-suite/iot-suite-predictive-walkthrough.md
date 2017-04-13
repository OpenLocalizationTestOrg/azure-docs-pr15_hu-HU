<properties
 pageTitle="Prediktív karbantartási forgatókönyv |} Microsoft Azure"
 description="Útmutató az Azure IoT prediktív karbantartási előre megoldás."
 services=""
 suite="iot-suite"
 documentationCenter=""
 authors="aguilaaj"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-suite"
 ms.devlang="na"
 ms.topic="get-started-article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="08/17/2016"
 ms.author="araguila"/>

# <a name="predictive-maintenance-preconfigured-solution-walkthrough"></a>Előre beállított prediktív karbantartási megoldás útmutató

## <a name="introduction"></a>– Bevezetés

A IoT csomagja előre prediktív karbantartási megoldás, a = a pont esetben előrejelzése, ha a hiba valószínű üzleti példa egy végpont megoldás. Használhatja a beépített megoldás ezzel kapcsolatban beérkező például optimalizálása karbantartási tevékenységek. A megoldás egyesíti a fő Azure IoT csomagja szolgáltatást, többek között a az [Azure gépi tanulási] [ lnk_machine_learning] munkaterület. A munkaterület kísérletek, a hátralévő hasznos élettartama (Szabályainak) repülőgép motor előrejelzésére egy nyilvános minta adathalmaz alapján tartalmazza. A megoldás tervezése és megvalósítása, amely megfelel a saját meghatározott üzleti megoldást kiindulási pontként teljesen hajtja végre a IoT üzleti forgatókönyvet.

## <a name="logical-architecture"></a>Logikai architektúra

Az alábbi ábra az előre beállított megoldás logikai összetevői ismerteti:

![][img-architecture]

A kék cikkeket kiépített környezetet nyújt a helyet, ha Ön az előre beállított megoldás kiépítése jelölje ki az Azure szolgáltatásokat. Az előre beállított megoldás a kelet-Amerikai Egyesült Államok, Észak-Európa vagy kelet-ázsiai régióban is kiépítése.

Források a régiók, ahol, az előre beállított megoldás kiépítése nem érhetők el. A diagram a narancssárga elemeket jelenítik meg az Azure szolgáltatások, a legközelebbi elérhető régióban (Dél központi US, Európa nyugati vagy Délkelet-ázsiai) adni a kijelölt terület kiépítve.

Zöld elem egy szimulált eszközt, amely egy repülőgép motor. Ismerje meg ezeket a következő szakaszban szimulált eszközök.

A szürke elemek *eszköz felügyeleti* lehetőségeket megvalósítása összetevőket jelenítik meg. Az előre beállított prediktív karbantartási megoldás a jelenlegi verziójában nem kiépítése ezek az erőforrások. Eszközök felügyelete kapcsolatos további tudnivalókért olvassa el a [távoli előre beállított megoldás figyelése][lnk-remote-monitoring].

## <a name="simulated-devices"></a>Szimulált eszközök

Az előre beállított megoldás szimulált eszköz repülőgép motor jelöl. A megoldást, amely egy egyetlen repülőgép megfeleltetése két motorok van kiépítve. Minden egyes motor bocsát telemetriai négyféle: érzékelő 9 érzékelő 11, érzékelő 14 és érzékelő 15 adja meg a számítja ki a hátralévő hasznos élettartama (Szabályainak) a motor a gépi tanulási modell szükséges adatokat. Minden szimulált eszköz a következő telemetriai üzeneteket küld IoT központi:

*Ciklikus száma*. A ciklus egy befejezett repülőút, amelyben telemetriai adatokat a repülőút minden fél órán rögzíthető 2 – 10 óra közötti változó hosszúságú jelöli.

*Telemetriai*. Vannak olyan motor attribútumok megjelenítő négy érzékelők. A érzékelőket vannak általában címkézett érzékelő 9 érzékelő 11, érzékelő 14 és érzékelő 15. Ezek a 4 érzékelők elegendő a gépi tanulási modell hasznos eredmények beszerzése Szabályainak telemetriai jelenítik meg. A modell hoz létre, amely tartalmazza a valós motor szenzoradatokat nyilvános adatokból. A modell létrehozási módjától az eredeti adatokból a további tudnivalókért lásd: a [Cortana intelligencia gyűjtemény prediktív karbantartási sablon][lnk-cortana-analytics].

Szimulált eszközök képes kezelni az alábbi parancsok küldött IoT-központban:

| Parancs | Leírás |
|---------|-------------|
| StartTelemetry | Határozza meg a szimulációk állapotát.<br/>Elindítja az eszköz telemetriai küldése     |
| StopTelemetry  | Határozza meg a szimulációk állapotát.<br/>Az eszköz telemetriai küldése leállítása |

IoT központi eszköz parancs nyugta biztosít.

## <a name="azure-stream-analytics-job"></a>Azure Értékáram-elemzés feladat

**Feladat: Telemetriai** működik, a bejövő eszköz telemetriai adatfolyam két kimutatások használatával. Az első kijelöli az összes telemetriai az eszközökről, és értesítést küld az adat blob-tároló származó, ahol azt formájában jelenik meg a web App alkalmazásban. A második utasítás átlagos érzékelő értékek kiszámítja a kétperces csúsztatható ablak fölé, és ezeket az adatokat a rendezvény hubon keresztül küld egy **esemény processzor**.

## <a name="event-processor"></a>Esemény processzor

Az **esemény processzor** átlagos érzékelő értékeket egy befejezett ciklus szükséges időt. Azt a fázisokat ezeket az értékeket az API-hoz, amely a gépi tanulási közzététele képzett model-motor a Szabályainak számítja ki.

## <a name="azure-machine-learning"></a>Azure gépi tanulási

A modell létrehozási módjától az eredeti adatokból a további tudnivalókért lásd: a [Cortana intelligencia gyűjtemény prediktív karbantartási sablon][lnk-cortana-analytics].

## <a name="lets-start-walking"></a>Lássuk a kiállniuk

Ez a szakasz végigvezeti a megoldást összetevői bemutatja az olyan eset és példákat mutat be.

### <a name="predictive-maintenance-dashboard"></a>Prediktív karbantartási irányítópult

Ez a lap, a webalkalmazásban használ a PowerBI JavaScript-vezérlők (lásd: a [PowerBI-látványelemek tárházba][lnk-powerbi]) ábrázolásához:

- A kimenet blob-tárolóhoz Értékáram-elemzés projektjeit adatait.
- A használati repülőgép motor Szabályainak és ciklus számát.

### <a name="observing-the-behavior-of-the-cloud-solution"></a>A vezérlő viselkedése a felhőben megoldás betartásával

Az Azure-portálon nyissa meg az erőforráscsoport úgy döntött, hogy a kiépített erőforrások megtekintése nevű megoldás.

![][img-resource-group]

Az előre beállított megoldás kiépítése meg, amikor a gépi tanulási munkaterület mutató hivatkozást tartalmazó e-mail kap. Is megkeresheti a gépi tanulási munkaterület a [azureiotsuite.com] a[ lnk-azureiotsuite] lapon a kiépített megoldás, amikor **készen áll** a állapotban van.

![][img-machine-learning]

A megoldás portálon megtekintheti, hogy a minta kiépítve-e négy szimulált eszköz a két motorok repülőgépek, négy érzékelők mindegyiket per két repülőgép ábrázolásához. Amikor, először nyissa meg a megoldást portált, a szimulációk leállt.

![][img-simulation-stopped]

Kattintson a **Start szimulációk** a szimulációk, megjelenik a érzékelő előzmények Szabályainak, ciklus és Szabályainak előzmények feltölteni az irányítópulton a kezdéshez.

![][img-simulation-running]

Ha Szabályainak kisebb, mint 160 (egy tetszőleges bemutató célokra választott küszöb), a megoldást portál jeleníti meg a Szabályainak megjelenítése mellett egy figyelmeztető jel emeli ki a repülőgép motor sárga. Figyelje meg, hogyan Szabályainak értékeket van-e egy általános általános lefelé trend, de általában kezük felfelé vagy lefelé. Ez a probléma a változó ciklus hossza és a modell pontosságát eredményei.

![][img-simulation-warning]

A teljes szimulációk perc körülbelül 35 148 ciklus befejezéséhez. A 160 Szabályainak küszöbértékét körülbelül 5 perccel az első alkalommal teljesül, és mindkét motorok küszöbértéket találati körülbelül 8 perccel.

A szimulációk fut, a teljes adatkészlet keresztül 148 ciklus, és a végleges Szabályainak és ciklus értékek rendezi.

A szimulációk bármikor leállíthatja, de a szimulációk az adathalmaz az elejétől gombra kattintva **Indítsa el szimulációk** replays.

## <a name="next-steps"></a>Következő lépések

Most már futtatja a cserélendő karbantartási előre megoldást, érdemes módosítani, akkor olvassa el az [előre definiált megoldások testreszabása útmutatást][lnk-customize].

A TechNet [IoT csomagja – a motorháztető alatt - prediktív karbantartási](http://social.technet.microsoft.com/wiki/contents/articles/33527.iot-suite-under-the-hood-predictive-maintenance.aspx) blogbejegyzés az előre beállított prediktív karbantartási megoldás kapcsolatban további információt tartalmaz.

Egyéb funkciók közül egyesek és funkciók előre IoT csomagja megoldást is feltárása:

- [Gyakori kérdések IoT csomagja][lnk-faq]
- [IoT biztonsági alapoktól][lnk-security-groundup]


[img-architecture]: media/iot-suite-predictive-walkthrough/architecture.png
[img-resource-group]: media/iot-suite-predictive-walkthrough/resource-group.png
[img-machine-learning]: media/iot-suite-predictive-walkthrough/machine-learning.png
[img-simulation-stopped]: media/iot-suite-predictive-walkthrough/simulation-stopped.png
[img-simulation-running]: media/iot-suite-predictive-walkthrough/simulation-running.png
[img-simulation-warning]: media/iot-suite-predictive-walkthrough/simulation-warning.png

[lnk-powerbi]: https://www.github.com/Microsoft/PowerBI-visuals
[lnk_machine_learning]: https://azure.microsoft.com/services/machine-learning/
[lnk-remote-monitoring]: iot-suite-remote-monitoring-sample-walkthrough.md
[lnk-cortana-analytics]: http://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-Template-3
[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk-customize]: iot-suite-guidance-on-customizing-preconfigured-solutions.md
[lnk-faq]: iot-suite-faq.md
[lnk-security-groundup]: securing-iot-ground-up.md
