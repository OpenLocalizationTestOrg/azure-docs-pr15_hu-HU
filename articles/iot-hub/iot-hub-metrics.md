<properties
 pageTitle="Diagnosztikai mértékek IoT központi"
 description="Azure IoT központi mértékek, engedélyezése a felhasználóknak, hogy mérje fel, hogy az erőforrás teljes állapotának áttekintése"
 services="iot-hub"
 documentationCenter=""
 authors="nberdy"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="08/11/2016"
 ms.author="nberdy"/>

# <a name="introduction-to-diagnostic-metrics"></a>Diagnosztikai mértékek – bevezetés

Diagnosztikai mértékek ad az Azure erőforrások állapotával kapcsolatos adatok jobb az Azure-előfizetésben. Mértékek lehetővé teszi mérje fel, hogy a szolgáltatás és a hozzá kapcsolódó eszközök általános állapotának. Felhasználói elérésű statisztikai adatokat is fontos, mert azok könnyebben átláthatja a legyen a történésekkel kapcsolatban a IoT központi és a Súgó kiváltóok-problémák megoldásához anélkül, hogy az Azure ügyfélszolgálatát.

Diagnosztikai mértékek az Azure portálról engedélyezheti.

## <a name="how-to-enable-diagnostic-metrics"></a>Diagnosztikai mértékek engedélyezése

1. Hozzon létre egy IoT hubhoz. Hogyan hozhat létre egy IoT központ az [Első lépések] útmutató megtalálhatja[ lnk-get-started] útmutató.

2. Nyissa meg a lap, a IoT-központját. Itt kattintson a **Diagnosztika lehetőségre**.

    ![][1]

3. Az állapot beállítása **a** , majd kiválasztja a diagnosztikai adatok tárolására Azure tároló fiók beállítása: a diagnosztikai. Jelölje be a **Mértékek**, és nyomja le a **Mentés**gombra. Ne feledje, hogy az Azure tárterület-fiókot kell létrehozni időszakokra, hogy az előfizetést terhelő külön-külön tárterület. A diagnosztikai adatok küldése a esemény hubok zárólap is választhat.

    ![][2]

4. A diagnosztikai beállítása után vissza az **Áttekintés** IoT központi lap. Mértékek adatokat a lap **Figyelés** szakaszában van kitöltve. A diagram gombra kattintva megnyílik a mértékek ablaktábla, ahol a IoT központi a mértékek információk megtekintése és szerkesztése a mértékek, a diagram megjelenített kijelölésének. Értesítések metrikus értékek alapján is beállítható.

    ![][3]

## <a name="metrics-and-how-to-use-them"></a>Mértékek és használatuk

IoT központi Itt számos mértékek nyújt a központi és eszköz csatlakozik, a teljes oldalszám állapotának áttekintése. Több mértékek és a IoT központi szerinti nagyobb képet festhet származó információkat is összevonhatja. Az alábbi táblázat ismerteti az egyes IoT központi nyomon követi a mértékek, és hogyan minden metrikus vonatkozik-e a IoT központi általános állapotát.

| Metrikus | Metrikus leírása | Mire használható a mérőszám |
| ---- | ---- | ---- |
| d2c.telemetry.ingress.allProtocol | Az összes eszközökön keresztül küldött üzenetek száma | Üzenet küldése adatok áttekintése |
| d2c.telemetry.ingress.SUCCESS | Az összes sikeres üzenet az elosztóhoz darabszámát | A bejövő az elosztóhoz sikeres üzenet adatok áttekintése |
| c2d.commands.egress.Complete.SUCCESS | Az száma az összes parancs a fogadó eszköz elvégeznie minden minden eszközön | A mértékek, eseményértesítéshez és Elvetés együtt áttekintést általános C2D parancsot sikerült ráta |
| c2d.commands.egress.Abandon.SUCCESS | Az üzenetek sikeresen minden minden eszközön hagyva által a fogadó eszköz száma | Ha az üzenetek vártnál gyakran törlődnek az első kiemeli a lehetséges problémák |
| c2d.commands.egress.Reject.SUCCESS | Az üzenetek sikeresen elutasítja a fogadó eszköz minden minden eszközön száma | Ha az üzenetek elutasították a vártnál gyakran első kiemeli a lehetséges problémák |
| devices.totalDevices | Az átlag, a minimum és a max eszközök számának regisztrált IoT-hubon keresztül csatlakozott. | A központi bejegyzett eszközök száma |
| devices.connectedDevices.allProtocol | Az átlag, a min és a max egyidejű csatlakoztatott eszközök száma | A számot a elosztóhoz csatlakoztatott eszközökön – áttekintés |

## <a name="next-steps"></a>Következő lépések

Most, hogy a diagnosztikai mértékek áttekintése láthatta, hajtsa végre ezt a hivatkozást, ha többet szeretne tudni az Azure IoT központi kezelése:

- [Műveletek figyelése][lnk-monitor]

További feltárása a IoT központi funkcióinak, olvassa el:

- [Fejlesztőeszközök útmutató][lnk-devguide]
- [Egy eszközt a IoT átjáró SDK csomagjában talál, amelyek][lnk-gateway]

<!-- Links and images -->
[1]: media/iot-hub-metrics/enable-metrics-1.png
[2]: media/iot-hub-metrics/enable-metrics-2.png
[3]: media/iot-hub-metrics/enable-metrics-3.png

[lnk-get-started]: iot-hub-csharp-csharp-getstarted.md
[lnk-operations-monitoring]: iot-hub-operations-monitoring.md
[lnk-scaling]: iot-hub-scaling.md
[lnk-dr]: iot-hub-ha-dr.md

[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
