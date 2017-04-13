<properties
 pageTitle="Fejlesztőeszközök útmutató – szószedet |} Microsoft Azure"
 description="IoT központi kapcsolatos egy közös szószedet"
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

# <a name="glossary-of-iot-hub-terms"></a>Szószedet IoT központi

Ez a cikk felsorolja a gyakran használt kifejezések társított IoT központi részét.

## <a name="advanced-message-queueing-protocol-amqp"></a>Speciális üzenet várólista Protocol (AMQP)

[AMQP](https://www.amqp.org/) IoT központi támogatja az eszközök kidolgozása üzenetben protokollok egyike. Az üzenetben protokollok IoT központi támogató kapcsolatos további tudnivalókért olvassa el a [IoT központi az üzenetek küldése és fogadása](iot-hub-devguide-messaging.md)című témakört.

## <a name="cloud-to-device-c2d"></a>Felhőalapú eszköz (C2D)

Általában használt IoT elosztóhoz csatlakoztatott eszközt küldött üzenetek vonatkozik. Az alábbi üzenetek gyakran, amely arra utasítja az eszközre néhány művelet végrehajtása a parancsok. További információt talál [az IoT központi üzenetek küldése és fogadása](iot-hub-devguide-messaging.md)

## <a name="condition"></a>Feltétel

Hivatkozik eszköz állapot adatai, például a csatlakozási módot használatban van, ahogyan azt egy eszköz alkalmazást. Eszközök is jelenthet azok a funkciók. Feltétel és használata az eszközön kettős lehetőséget is lekérdezhetők.

## <a name="data-point-message"></a>Az adatpont üzenet

Adatpont üzenet jelenik meg a felhőben-eszköz üzenet például szélsebesség vagy hőmérsékleti telemetriai adatokat tartalmazó.

## <a name="desired-properties"></a>A kívánt tulajdonságokat.

Az eszköz twins környezetben a kívánt tulajdonságokat szinkronizálása az eszköz konfigurációs vagy feltétel használatosak *Tulajdonságok jelentett* együtt. A kívánt tulajdonságokat csak akkor állítható vissza az alkalmazás által vége, és betartják eszközhöz alkalmazással. 

## <a name="device-to-cloud-d2c"></a>Eszköz a felhőbe (D2C)

Általában használt IoT elosztóhoz csatlakoztatott eszközt küldött üzenetek vonatkozik. Az alábbi üzenetek lehet, hogy [az adatpont](#data-point-message) vagy [interaktív](#interactive-message) üzenetet. További tudnivalókért olvassa el a [IoT központi az üzenetek küldése és fogadása](iot-hub-devguide-messaging.md)című témakört.

## <a name="device-identity-registry"></a>Eszköz identitás beállításjegyzék

Az [eszköz identitás beállításjegyzék](iot-hub-devguide-identity-registry.md) a beépített egy IoT hubhoz csatlakozhat egy központi engedélyezett egyedi eszközökkel kapcsolatos adatokat tároló része.

## <a name="device"></a>Eszköz

Az IoT környezetben, egy általában kisüzemi, különálló számítógépes eszközről, amely adatgyűjtés vagy egyéb eszközök szabályozhatja. Példa egy eszközt lehet egy környezeti ellenőrző eszköz vagy a vezérlő üvegházhatású itatási- és légtechnikai rendszerek.

## <a name="device-twin"></a>Kettős eszköz

Egy [eszköz kettős](iot-hub-devguide-device-twins.md) egy másolatot a feltétel és a konfigurációs beállítások fizikai eszközök IoT-központban. A fizikai eszköz a konfiguráció kezeléséhez egy eszköz kettős is használhatja.

## <a name="direct-method"></a>Közvetlen módszer

[Közvetlen módszer](iot-hub-devguide-direct-methods.md) módja a szeretné elindítani a módszer szerint meghívása egy API-t, kattintson a IoT központi eszközön végrehajtani.

## <a name="event-hub-compatible-endpoint"></a>Esemény központi-kompatibilis végpont

A IoT központi küldött eszköz a felhőbe üzenetek olvasása, a központi zárólap szeretne kapcsolódni, és minden olyan központi-kompatibilis esemény módszer használatával e-mailek olvasása. Esemény központi-kompatibilis módszerek közé tartozik, az esemény hubok SDK és Azure Értékáram-elemzés használata.

## <a name="field-gateway"></a>A mező átjáró

Mező az átjárók lehetővé teszi, hogy a csatlakozási eszközökhöz, amely nem tud kapcsolódni közvetlenül IoT központi és a szokásos telepítik helyileg eszközén. További tudnivalókért lásd: [Mi az Azure IoT központi?](iot-hub-what-is-iot-hub.md).

## <a name="job"></a>Feladat

A megoldás vissza vége feladatok segítségével ütemezése, és a tevékenységek nyomon követheti a IoT központi regisztrált eszközök csoportja. Tevékenységek eszköz kívánt kettős tulajdonságainak módosítása eszköz kettős címkék frissítése és közvetlen módszerek meghívása tartalmazzák.

## <a name="protocol-gateway"></a>Protocol (protokoll) átjáró

Protocol (protokoll) az átjárók általában telepíti a felhőben, és bemutat protocol fordítási szolgáltatások eszközök IoT-hubhoz csatlakozik. További tudnivalókért lásd: [Mi az Azure IoT központi?](iot-hub-what-is-iot-hub.md).

## <a name="interactive-message"></a>Interaktív üzenet

Egy interaktív üzenet jelenik meg egy felhőalapú-eszköz üzenet, amely elindítja a alkalmazás háttér azonnali művelet. Egy eszközt küldhet például egy riasztás kapcsolatos hiba, amely a naplózandó automatikusan CRM rendszerben.

## <a name="iot-hub"></a>IoT központi

IoT központi teljes körű felügyelt Azure szolgáltatás, amely lehetővé teszi, hogy a megbízható és nem biztonságos kétirányú kommunikációt IoT eszközök milliónyi és a megoldás vissza vége között. További tudnivalókért lásd: [Mi az Azure IoT központi?](iot-hub-what-is-iot-hub.md).

## <a name="iot-suite"></a>IoT programcsomagban

Azure IoT csomagja több Azure szolgáltatás előre beállított megoldásokkal együtt csomagok. Ezek a előre definiált megoldások lehetővé teszi – első lépések gyorsan esetei IoT végpont példányait. További tudnivalókért lásd: [Mi az Azure IoT csomagja?](../iot-suite/iot-suite-overview.md).

## <a name="job"></a>Feladat

Egy [feladat](iot-hub-devguide-jobs.md) IoT-központban lehetővé teszi, hogy a műveleteket, például egy belső vezérlőprogram frissítési minden több eszközön a hubhoz csatlakozik.

## <a name="mqtt"></a>MQTT

[MQTT](http://mqtt.org/) IoT központi támogatja az eszközök kidolgozása üzenetben protokollok egyike. Többet szeretne tudni az üzenetek protokollok IoT központi támogatja olvassa el a [IoT központi az üzenetek küldése és fogadása](iot-hub-devguide-messaging.md)című témakört.

## <a name="reported-properties"></a>A bejelentett tulajdonságai

Az eszköz twins környezetben jelentett tulajdonságok szinkronizálása az eszköz konfigurációs vagy feltétel használják *a kívánt tulajdonságokat* együtt. Bejelentett tulajdonságok csak az eszköz alkalmazást beállíthatja és olvasható, majd a háttér alkalmazás által lekérdezett.

## <a name="tags"></a>Címkék

Címkék devcie twins környezetében eszköz metaadatok tárolja, és a alkalmazás háttér JSON dokumentum formájában által beolvasott is. Címkék nem láthatók az eszközön-alkalmazásokhoz.

## <a name="system-properties"></a>A Rendszertulajdonságok

Az eszköz twins környezetben Rendszertulajdonságok akkor írásvédettek, és írja be az eszköz használatát, például a tevékenység utolsó időt és a kapcsolati állapot vonatkozó információkat.