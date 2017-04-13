<properties
 pageTitle="Fejlesztőeszközök útmutató - IoT központi végpontok |} Microsoft Azure"
 description="Azure IoT központi Fejlesztőeszközök útmutató - hivatkozás információt IoT központi végpontok"
 services="iot-hub"
 documentationCenter=".net"
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/30/2016" 
 ms.author="dobett"/>

# <a name="reference---iot-hub-endpoints"></a>Hivatkozás - IoT központi végpontok

## <a name="list-of-iot-hub-endpoints"></a>IoT központi végpontok listája

Azure IoT is többszörös bérlői szolgáltatás, amely a különböző szereplők funkció teszi lehetővé. Az alábbi ábra mutatja, hogy IoT központban elérhetővé teszi a különböző végpontok.

![IoT központi végpontok][img-endpoints]

Az alábbiakban látható a végpontokon leírása:

* **Erőforrás-szolgáltató**. A központi IoT erőforrás szolgáltató közzététele egy [Erőforrás-kezelő Azure] [ lnk-arm] felület, amely lehetővé teszi, hogy a Azure előfizetés a tulajdonosok létrehozása és törlése a IoT hubok és IoT központi tulajdonság frissítése. IoT központi tulajdonságok szabályozzák [központi szintű biztonsági házirendek][lnk-accesscontrol], nem pedig az eszköz szintű hozzáférés-vezérlés és az eszköz a felhőbe és a felhő-eszköz üzenetek funkcionális beállításait. A központi IoT erőforrás szolgáltató is lehetővé teszi, exportálása [eszköz identitások][lnk-importexport].
* **Eszköz Identitáskezelés**. Minden egyes IoT központi közzététele egy sor olyan HTTP többi végpontok eszköz-felhasználók kezelése (létrehozása, beolvasásához, frissítése és törlése). [Eszköz identitások] [ lnk-device-identities] eszköz-hitelesítés és a hozzáférés-vezérlés segítségével.
* **Kettős kezelés**. Minden egyes IoT központi közzététele egy sor olyan szolgáltatás elérésű HTTP többi végpontot, lekérdezés, és frissítse az [eszköz twins] [ lnk-twins] (frissíteni a címkék és tulajdonságok).
* **Projektek kezelése**. Minden egyes IoT központi közzététele egy sor olyan szolgáltatás elérésű HTTP többi végpontot, lekérdezés, és [feladatok]kezelése[lnk-jobs].
* **Eszköz végpontok**. Az egyes eszközök eszköz identitás beállításjegyzékbeli kiépítéstől IoT központi közzététele egy sor olyan eszköz használható üzenetek küldése és fogadása végpontok:
    - Az *eszköz a felhőbe küldésére*. Használni ezt a végpontot [eszköz a felhőbe üzenetek]küldéséhez[lnk-d2c].
    - *Üzenetek fogadása cloud-eszközt*. Egy eszközt használ a végpont célzott [cloud-eszköz üzenetek]fogadására[lnk-c2d].
    - A *fájlfeltöltések kezdeményezhet*. Egy eszközt használ a végpont IoT központi feltölteni a [fájlt]egy Azure tároló Társítások URI kapott[lnk-upload].
    - *Lekérés és frissítés kettős tulajdonságait*. Egy eszközt használ a végpontok eléréséhez az [eszköz kettős][lnk-twins]'s tulajdonságait.
    - *Receive közvetlen módszerek kérések*. Egy eszközt használ a végpontok [közvetlen módszerek]meghallgatásához[lnk-methods]'s kérések.

    Ezeket a végpontokat [MQTT v3.1.1]elérhetővé tett[lnk-mqtt], HTTP 1.1-es és [AMQP 1.0] [ lnk-amqp] protokollok. Ne feledje, hogy AMQP is elérhető fölé [WebSockets] [ lnk-websockets] a 443-as port.
    
    A twins' és módszerek endpoins használatával érhetők el csak [MQTT v3.1.1][lnk-mqtt].

* **Szolgáltatási végpontok**. Minden egyes IoT központi közzététele egy sor olyan végpontok az alkalmazás vissza vége segítségével kommunikálni az eszközén. Ezeket a végpontokat is jelenleg csak elérhető használ a [AMQP] [ lnk-amqp] Protocol (protokoll), kivéve az módszer meghívási végpontot, amely a HTTP 1.1 keresztül.
    - *Üzenetek fogadása eszköz a felhőbe*. A végpont az [Azure esemény hubok]kompatibilis[lnk-event-hubs]. A háttéradatbázist szolgáltatás vele az [eszköz a felhőbe üzenetek] olvasása[ lnk-d2c] eszközén által küldött.
    - A *felhő-eszköz üzenetek küldése és fogadása a kézbesítési visszaigazolások*. Ezeket a végpontokat engedélyezése az alkalmazás vissza vége megbízható [cloud-eszköz üzenetek]küldésére[lnk-c2d], és a megfelelő kézbesítési vagy a lejárat visszaigazolások kaphat.
    - *Receive fájl értesítéseket*. A Csevegés végpont lehetővé teszi a értesítést kaphat az eszközökről sikerült feltölteni egy fájlt. 
    - *Közvetlen módszer meghívása*. A végpont lehetővé teszi, hogy a háttéradatbázist szolgáltatás [közvetlen módszer] [ lnk-methods] eszközön.

A [IoT központi API -k és SDK] [ lnk-sdks] a cikk ismerteti a különböző módszereket ezeket a végpontokat eléréséhez.

Végezetül fontos tudni, hogy az összes IoT központi végpontok használja-e a [TLS] [ lnk-tls] Protocol (protokoll), és nincs végpont minden eddiginél van téve a titkosítatlan/nem biztonságos csatornát.

## <a name="field-gateways"></a>A mező átjárók

A IoT megoldás a *mező átjáró* között az eszközén, és a IoT központi végpontok helyezkedik el. A szokásos található, közelébe az eszközöket. Az eszközök kommunikálni közvetlenül a mező átjáró eszközök támogatják protokoll használatával. A mező átjáró IoT központ által támogatott protokollon keresztül zárólap IoT hubhoz csatlakozik. Mező az átjárók lehet erősen speciális hardvereszközt vagy a szoftvert, a végpontok közötti forgatókönyvet az átjáró szánták teljesít alacsony power számítógépre.

Használhatja az [Azure IoT átjáró SDK] [ lnk-gateway-sdk] mező az átjárók végrehajtásához. Ez az SDK funkcióiról, például az azt jelenti, hogy át a multiplex jeleket kapcsolatba lépni a különféle eszközökön mindig ugyanazt a kapcsolatot IoT központi kínál.

## <a name="next-steps"></a>Következő lépések

Hivatkozás témakörök IoT központi Fejlesztőeszközök tartalom a következők:

- [Lekérdezésnyelv twins, módszerek és feladatok][lnk-devguide-query]
- [Kvóták és szabályozása][lnk-devguide-quotas]
- [IoT központi MQTT támogatás][lnk-devguide-mqtt]

[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk

[img-endpoints]: ./media/iot-hub-devguide-endpoints/endpoints.png
[lnk-amqp]: https://www.amqp.org/
[lnk-mqtt]: http://mqtt.org/
[lnk-websockets]: https://tools.ietf.org/html/rfc6455
[lnk-arm]: ../azure-resource-manager/resource-group-overview.md
[lnk-event-hubs]: http://azure.microsoft.com/documentation/services/event-hubs/

[lnk-tls]: https://tools.ietf.org/html/rfc5246


[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-accesscontrol]: iot-hub-devguide-security.md#access-control-and-permissions
[lnk-importexport]: iot-hub-devguide-identity-registry.md#import-and-export-device-identities
[lnk-d2c]: iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-device-identities]: iot-hub-devguide-identity-registry.md
[lnk-upload]: iot-hub-devguide-file-upload.md
[lnk-c2d]: iot-hub-devguide-messaging.md#cloud-to-device-messages
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-jobs]: iot-hub-devguide-jobs.md

[lnk-devguide-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-devguide-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md