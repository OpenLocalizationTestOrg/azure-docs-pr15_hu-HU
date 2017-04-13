<properties
 pageTitle="Azure esemény hubok Azure IoT elosztót összehasonlítása |} Microsoft Azure"
 description="A kiemelés funkcionális eltérések és használati eset Azure IoT központi és Azure esemény hubok szolgáltatások összehasonlítása."
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
 ms.date="06/06/2016"
 ms.author="elioda"/>

# <a name="comparison-of-azure-iot-hub-and-azure-event-hubs"></a>Azure IoT központi és Azure esemény hubok összehasonlítása

A fő használati eset IoT központi az egyik telemetriai gyűjtése eszközökről. Emiatt IoT központi gyakran [Azure esemény hubok][]összehasonlítva. IoT központi, például esemény hubok szolgáltatása, amely lehetővé teszi, hogy a felhőbe tömeges méreteket, a bejövő Eseménynapló és telemetriai adatok kis késés és a magas megbízhatóság feldolgozása eseményről szó.

A szolgáltatások azonban van a különbségeket, amelyek vannak a következő táblázat részletesen.

| Terület | IoT központi | Esemény hubok |
| ---- | ------- | ---------- |
| Kapcsolati mintázatok | Lehetővé teszi, hogy eszköz a felhőbe, és a felhő-eszköz üzenetek. | Mindössze engedélyezi az esemény bejövő adatok (általában eszköz a felhőbe eseteket figyelembe venni). |
| Eszköztámogatás protokoll | Támogatja a MQTT, AMQP, AMQP WebSockets és a HTTP felett. Ezenkívül IoT központi működik-e a az [Azure IoT protocol átjáró][lnk-azure-protocol-gateway], egyéni protokollt támogató és a testre szabható protocol átjáró megvalósítás. | AMQP, WebSockets és a HTTP AMQP támogatja. |
| Biztonsági | Eszköz identitás és visszavonható hozzáférés-vezérlés biztosít. A [Fejlesztőeszközök IoT központi Guide biztonság szakaszában]olvashat. | Esemény hubok szintű [átengedése házirendeket]biztosít[Event Hubs - security], korlátozott visszavonási támogatja [a publisher]házirendek[Event Hubs publisher policies]. IoT megoldások gyakran eszköz hitelesítő adatok és mértékek anti hamisított egyéni megoldást végrehajtásához szükséges. |
| Műveletek figyelése | Lehetővé teszi, hogy IoT megoldások eszköz identitás kezelése és kapcsolódási például egyedi eszköz hitelesítési hibákat, szabályozásának és hibás formátumú kivételek események széles körű előfizetéséhez. Az események lehetővé teszi az egyes eszköz szintjén kapcsolódási problémák gyors azonosítását. | Csak az összesítő mértékek közzététele. |
| Méretarány | Támogatás milliós egyidejű csatlakoztatott eszközök van optimalizálva. | Egyidejű kapcsolatok – legfeljebb 5 000 AMQP kapcsolatok [Azure Service Bus kvóták][]szerinti további korlátozott számú is támogatja. Azonban esemény hubok lehetővé teszi, hogy minden küldött üzenet a partíciót adja meg. |
| Eszköz SDK | Itt [eszköz SDK] [ Azure IoT Hub SDKs] platformokon és nyelvek nagy sokféle. | A .NET rendszerhez és a c támogatott AMQP és a HTTP-küldése felületek is tartalmaz. |
| A fájl feltöltése | Lehetővé teszi, hogy a felhőbe eszközökről fájlok feltöltése IoT megoldások. Egy fájl értesítés végpontot, a munkafolyamat-integrációs és -műveletek ellenőrzésére támogatási hibakereséshez kategória tartalmazza. | Használja a felelős minta manuálisan eszközökről kérése a fájlokat, és adja meg a eszközök a tranzakció tároló használatával. |

Összefoglalásként tehát elmondhatjuk akkor is, ha csak a használati eset eszköz a felhőbe telemetriai bejövő adatok, IoT központi szolgáltatást nyújt kifejezetten IoT eszköz kapcsolat. Bontsa ki a IoT nyelvspecifikus funkciókat tartalmazó forgatókönyvekben az az érték értékűeknek továbbra is. Esemény hubok esemény bejövő adatok a tömeges méreteket, mind a többek adatközponthoz és belüli-adatközponthoz forgatókönyvek környezetében készült.

Még nem ritka IoT központi és a esemény hubok ugyanabban a megoldásban – hol IoT központi kezeli az eszköz a felhőbe kommunikációs, és esemény hubok kezeli a későbbi időpontban esemény bejövő adatok átalakítása valós idejű feldolgozás motorok segítségével.

## <a name="next-steps"></a>Következő lépések

A IoT központi üzembe megtervezésével kapcsolatos további tudnivalókért lásd: a [Méretezés lehetőséget, HA és DR][lnk-scaling].

További feltárása a IoT központi funkcióinak, olvassa el:

- [Fejlesztőeszközök útmutató][lnk-devguide]
- [Egy eszközt a IoT átjáró SDK csomagjában talál, amelyek][lnk-gateway]

[Azure esemény hubok]: ../event-hubs/event-hubs-what-is-event-hubs.md
[A központi IoT Fejlesztőeszközök útmutató biztonsági szakasza]: iot-hub-devguide-security.md
[Event Hubs - security]: ../event-hubs/event-hubs-authentication-and-security-model-overview.md
[Event Hubs publisher policies]: ../event-hubs/event-hubs-overview.md#common-publisher-tasks
[Azure Service Bus kvóták]: ../service-bus-messaging/service-bus-quotas.md
[Azure IoT Hub SDKs]: https://github.com/Azure/azure-iot-sdks/blob/master/readme.md
[lnk-azure-protocol-gateway]: iot-hub-protocol-gateway.md

[lnk-scaling]: iot-hub-scaling.md
[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
