<properties
 pageTitle="IoT központi MQTT támogatási |} Microsoft Azure"
 description="IoT központi szintű támogatást MQTT leírása"
 services="iot-hub"
 documentationCenter=".net"
 authors="kdotchkoff"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="10/24/2016"
 ms.author="kdotchko"/>

# <a name="iot-hub-mqtt-support"></a>IoT központi MQTT támogatás

IoT központi lehetővé teszi, hogy az eszköz használatával kommunikál a [MQTT v3.1.1] használatával IoT központi eszköz végpontjait[ lnk-mqtt-org] protokoll port 8883 vagy MQTT v3.1.1 a 443-as port WebSocket protokollon keresztül. IoT központi szükséges az összes eszközt kommunikáció biztonságát használja az SSL/TLS (így IoT központi nem támogatja a nem biztonságos kapcsolatok port 1883 felett).

## <a name="connecting-to-iot-hub"></a>Kapcsolódás IoT központi

Egy eszközt a MQTT protokoll elérésére használható keresztül a tárakat a [Microsoft Azure IoT SDK] IoT hubhoz[ lnk-device-sdks] vagy a MQTT használatával protokoll közvetlenül.

## <a name="using-the-device-client-sdks"></a>Az eszköz ügyfél SDK használatával

[Eszköz ügyfél SDK] [ lnk-device-sdks] támogató MQTT protokollt érhetők el a Java, Node.js, C, C# és Python. Az eszköz ügyfél SDK IoT-hubon keresztül csatlakozott kapcsolatot létesíteni használja a szabványos IoT központi a kapcsolati karakterláncot. A MQTT protokoll használatára, az ügyfél jegyzőkönyv paraméter meg kell **MQTT**. Alapértelmezés szerint az eszköz ügyfél SDK csatlakozás és a **CleanSession** IoT hubhoz jelző értéke **0** és **QoS 1** az üzenet exchange a IoT hubhoz.

Amikor egy eszközt IoT-hubhoz csatlakozik, az eszköz ügyfél SDK biztosítanak a módszerek, amelyek lehetővé teszik az eszközre a üzenetek üzenetek küldése és fogadása IoT-központban.

Az alábbi táblázat az egyes támogatott nyelvekhez mintakódok mutató hivatkozásokat tartalmaz, és adja meg a paraméter IoT hubon keresztül csatlakozott, a MQTT protokollal kapcsolatot létesíteni használni.

| Nyelvi                   | Protocol (protokoll) paraméter        |
| -------------------------- | ------------------------- |
| [NODE.js][lnk-sample-node] | Azure-iot-eszköz-mqtt     |
| [Java][lnk-sample-java]    | IotHubClientProtocol.MQTT |
| [C][lnk-sample-c]          | MQTT_Protocol             |
| [C#][lnk-sample-csharp]    | TransportType.Mqtt        |
| [Python][lnk-sample-python] | IoTHubTransportProvider.MQTT |

### <a name="migrating-a-device-app-from-amqp-to-mqtt"></a>Egy eszköz alkalmazás MQTT való áttérés AMQP
Ha az [eszköz ügyfél SDK]böngészőt használ[lnk-device-sdks], az ügyfél inicializálni a jegyzőkönyv paraméter módosítása, ahogy azt már említettük-ről történő MQTT AMQP használatával igényel.

Ha így tesz, győződjön meg róla, hogy ellenőrizze az alábbiakat:

* AMQP hibák a sok feltételek adja eredményül, miközben MQTT megszünteti a kapcsolatot. Emiatt a kivétel logika kezelése szükség lehet a módosításokat.
* MQTT nem támogatja az *Elutasítás* műveletek [C2D üzenetek]fogadására[lnk-messaging]. Ha a hátsó vége válasznak a eszköz alkalmazásból, akkor fontolja meg [közvetlen módszerek][lnk-methods].

## <a name="using-the-mqtt-protocol-directly"></a>Közvetlenül a MQTT protokollal

Egy eszközt az eszköz ügyfél SDK nem használható, ha azt még csatlakozhat a nyilvános eszköz végpontok MQTT protokoll használatával. A **Csatlakozás** csomagban az eszköz kell a következő értékeket használja:

- A **ClientId** mező használata a **deviceId**. 
- A **Username** mezők használata `{iothubhostname}/{device_id}`, ahol a {iothubhostname} az IoT-központban, a teljes CName.

    Ha például a IoT központi neve **contoso.azure-devices.net** , ha és az eszköz neve **MyDevice01**a teljes **Username** mező tartalmaznia kell `contoso.azure-devices.net/MyDevice01`.

- A **jelszó** mezőben használja a biztonsági jogkivonat. A biztonsági jogkivonat formátuma ugyanaz, mint a HTTP- és a AMQP protokoll esetében:<br/>`SharedAccessSignature sig={signature-string}&se={expiry}&sr={URL-encoded-resourceURI}`.

    További információt a Társítások tokenek létrehozásához című eszköz [használatával IoT központi biztonsági]tokenek[lnk-sas-tokens].
    
    Vizsgálatakor is használhatja az [Eszköz Explorer] [ lnk-device-explorer] eszköz biztonsági jogkivonat, amely másolja a vágólapra, és illessze be a saját kód gyors létrehozásához:
    
    1. Nyissa meg **az eszköz Explorerben a lapra** .
    2. Kattintson a **Biztonsági jogkivonat** (jobbra fent).
    3. A **SASTokenForm**, jelölje ki a kívánt eszközt a **DeviceID** legördülő listája. A **TTL (élettartam)**beállítása.
    4. Kattintson a **Létrehozás** a token létrehozásához.
    
    A biztonsági jogkivonat létrehozott e szerkezete:   `HostName={your hub name}.azure-devices.net;DeviceId=javadevice;SharedAccessSignature=SharedAccessSignature sr={your hub name}.azure-devices.net%2fdevices%2fMyDevice01&sig=vSgHBMUG.....Ntg%3d&se=1456481802`.

    Ez a token **MQTT segítségével szeretne csatlakozni a jelszómező** használni kívánt részét:   `SharedAccessSignature sr={your hub name}.azure-devices.net%2fdevices%2fyDevice01&sig=vSgHBMUG.....Ntg%3d&se=1456481802g%3d&se=1456481802`.

MQTT csatlakozás és csomagok leválasztása, IoT központi a **Műveletek figyelése** csatornán esemény problémáinak további információkat, amelyek segítséget nyújtanak a csatlakozási hibáinak elhárításához.

### <a name="sending-messages-to-iot-hub"></a>Üzenetek küldése IoT központi

A sikeres csatlakozás után egy eszközt üzeneteket küldhet IoT központi használatával `devices/{device_id}/messages/events/` vagy `devices/{device_id}/messages/events/{property_bag}` **Témakör neve**. A `{property_bag}` elem lehetővé teszi, hogy az eszköz további tulajdonságokat tartalmazó üzenet küldése egy url-címként kódolt formátumban. Példa:

```
RFC 2396-encoded(<PropertyName1>)=RFC 2396-encoded(<PropertyValue1>)&RFC 2396-encoded(<PropertyName2>)=RFC 2396-encoded(<PropertyValue2>)…
```

> [AZURE.NOTE] Ez `{property_bag}` elem használja, mint a lekérdezési karakterlánc a HTTP protokoll ugyanazon kódolás.

Az eszköz ügyfélalkalmazás is használhatja `devices/{device_id}/messages/events/{property_bag}` , a **témakör neve fog** *üzenetek fog* telemetriai üzenetként is továbbíthassák meghatározásához.

IoT központi nem támogatja a QoS 2 üzeneteket. Ha egy eszköz ügyfél közzé, egy üzenet **QoS 2**, a IoT központi bezárása a hálózati kapcsolat.
IoT központi nem marad meg megtartása üzeneteket. Ha egy eszközt a **MEGTARTÁSA** jelölővel értéke 1 üzenetet küld, IoT központi összeadja az **x-kiválaszthatják-megőrzi az** alkalmazás tulajdonság az üzenethez. Ebben az esetben helyett pályától tartós megtartása üzenet, IoT központi továbbítja az kódmentes alkalmazást.

### <a name="receiving-messages"></a>Üzenetek fogadása

Üzenetek fogadására IoT központból, egy eszközt elő kell fizetnie használatával `devices/{device_id}/messages/devicebound/#` **Témakör szűrő**. A többszintű helyettesítő **#** a témakör szűrő csak szolgál engedélyezése fogadásához további tulajdonságokat a témakör neve az eszközön. IoT központi nem teszi lehetővé a használatát a **#** vagy **?** helyettesítő karakterek alszint témakörök szűrése. Mivel IoT központi nem általános célú pub-sub üzenetben ügynök, csak támogat a dokumentált témakör nevek és a tematikus szűrőket.

Kérjük, hogy az eszköz nem kap esetleges üzeneteket IoT központból azt sikeresen előfizetett eszköz adott végpontját, mielőtt a feljegyzésben jelöli a `devices/{device_id}/messages/devicebound/#` témakör szűrő. Miután létrejött a sikeres előfizetést, az eszköz elkezdenek csak azt az előfizetést, az idő után elküldött cloud-eszköz üzenetek fogadására. Ha az eszköz kapcsolódik **CleanSession** jelölővel értéke **0**, az előfizetés különböző munkamenetek között maradnak. Ebben az esetben a következő alkalommal, amikor azt csatlakoztatja az **CleanSession 0** nyitott üzenetek küldött hozzá lett a forráshoz fog kapni. Ha az eszköz **CleanSession** jelző értéke **1** , ha nem kap olyan üzeneteket IoT központból mindaddig, amíg a eszköz végpont előfizetője azt használja.

IoT központi lekéri az üzeneteket a **Témakör neve** `devices/{device_id}/messages/devicebound/`, vagy `devices/{device_id}/messages/devicebound/{property_bag}` bármelyik üzenet tulajdonságai esetén. `{property_bag}`url-címként kódolt kulcs/érték párokká, az üzenet tulajdonságok tartalmazza. A tulajdonság között csak alkalmazás és a felhasználó állítható be a rendszer tulajdonságai (például **messageId** vagy **correlationId**) szerepelnek. Rendszer tulajdonságnév van az előtag **$**, szolgáltatásalkalmazás tulajdonságainak használata nincs előtag az eredeti tulajdonság neve.

Amikor egy eszközt ügyfél **QoS**2 témakör előfizetője, IoT központi hozzárendeli a maximális QoS 1-es szint a **SUBACK** csomagban. Ezt követően az IoT központi az eszközre az QoS 1 lesz üzenetkézbesítés.

### <a name="additional-considerations"></a>További megfontolandó szempontok

Végleges szempontokat, mint testre kell szabnia a MQTT protokoll viselkedése a felhőben oldalon érdemes áttekintenie, ha az [Azure IoT protokoll átjáró] [ lnk-azure-protocol-gateway] , amely lehetővé teszi, hogy közvetlenül a IoT központi felületek nagy teljesítményű egyéni protokoll átjáró telepítése. Az Azure IoT jegyzőkönyv átjáró lehetővé teszi, hogy az eszköz protokoll brownfield MQTT telepítések elhelyezésére vagy más egyéni protokollok testre. Ezt a megközelítést kell-e, azonban futtatja, és egy egyéni protokoll átjáró működik.

## <a name="next-steps"></a>Következő lépések

További tudnivalókért lásd: [MQTT jegyzetek támogatja a] [ lnk-mqtt-devguide] az Azure IoT központi Fejlesztőeszközök útmutatóban olvashat.

Többet szeretne tudni a MQTT protokollt, lásd: a [MQTT dokumentáció][lnk-mqtt-docs].

A központi IoT üzembe megtervezésével kapcsolatos további információért lásd:

- [Azure jóváhagyott IoT eszköz katalógus][lnk-devices]
- [További protokollok támogatása][lnk-protocols]
- [Esemény hubok összehasonlítása][lnk-compare]
- [Méretezés, a HA és a DR][lnk-scaling]

További feltárása a IoT központi funkcióinak, olvassa el:

- [Fejlesztőeszközök útmutató][lnk-devguide]
- [Egy eszközt a IoT átjáró SDK csomagjában talál, amelyek][lnk-gateway]

[lnk-device-sdks]: https://github.com/Azure/azure-iot-sdks/blob/master/readme.md
[lnk-mqtt-org]: http://mqtt.org/
[lnk-mqtt-docs]: http://mqtt.org/documentation
[lnk-sample-node]: https://github.com/Azure/azure-iot-sdks/blob/develop/node/device/samples/simple_sample_device.js
[lnk-sample-java]: https://github.com/Azure/azure-iot-sdks/blob/develop/java/device/samples/send-receive-sample/src/main/java/samples/com/microsoft/azure/iothub/SendReceive.java
[lnk-sample-c]: https://github.com/Azure/azure-iot-sdks/tree/master/c/iothub_client/samples/iothub_client_sample_mqtt
[lnk-sample-csharp]: https://github.com/Azure/azure-iot-sdks/tree/master/csharp/device/samples
[lnk-sample-python]: https://github.com/Azure/azure-iot-sdks/tree/master/python/device/samples
[lnk-device-explorer]: https://github.com/Azure/azure-iot-sdks/blob/master/tools/DeviceExplorer/readme.md
[lnk-sas-tokens]: iot-hub-devguide-security.md#using-sas-tokens-as-a-device
[lnk-mqtt-devguide]: iot-hub-devguide-messaging.md#notes-on-mqtt-support
[lnk-azure-protocol-gateway]: iot-hub-protocol-gateway.md

[lnk-devices]: https://catalog.azureiotsuite.com/
[lnk-protocols]: iot-hub-protocol-gateway.md
[lnk-compare]: iot-hub-compare-event-hubs.md
[lnk-scaling]: iot-hub-scaling.md
[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md

[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-messaging]: iot-hub-devguide-messaging.md
