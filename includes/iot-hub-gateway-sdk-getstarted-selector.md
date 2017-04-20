> [AZURE.SELECTOR]
- [Linux](../articles/iot-hub/iot-hub-linux-gateway-sdk-get-started.md)
- [A Windows](../articles/iot-hub/iot-hub-windows-gateway-sdk-get-started.md)

Ez a cikk tartalmaz egy részletes forgatókönyv a [Helló, világ példakód] [ lnk-helloworld-sample] mutatja be az alapvető összetevők [Azure IoT átjáró SDK] [ lnk-gateway-sdk] architektúrája. A mintát a IoT Hub átjáró SDK egy egyszerű fájl "Helló, világ" üzenet jelentkezik ötpercenként átjáró létrehozásához használ.

A forgatókönyv leírja:

- **Fogalmak**: bármely hoz létre a IoT átjáró SDK átjáró alkotó összetevők elméleti áttekintése.  
- **Helló, világ minta architektúra**: ismerteti, hogyan fogalmak a Helló, világ minta vonatkoznak, és hogyan az összetevők illeszkednek.
- **Hogyan kell a minta**: a minta létrehozásához szükséges lépéseket.
- **Hogyan lehet futtatni a minta**: a minta futtatásához szükséges lépéseket. 
- **Kimenete**: Példa a minta futtatásakor várt kimenet.
- **Kódtöredék**: hogyan valósítja meg a Helló, világ minta kulcs átjáró alkatrészek megjelenítése kódtöredékek gyűjteménye.

## <a name="azure-iot-gateway-sdk-concepts"></a>Borzas IoT átjáró SDK fogalmak

Előtt vizsgálja meg a minta kódot, vagy hozzon létre saját mező gateway a Gateway IoT SDK használatával, meg kell ismernie a kulcsfogalmakat, amelyek az SDK architektúrája.

### <a name="modules"></a>Modulok

Az Azure IoT átjáró SDK és az átjáró létrehozásával és összeszerelés *modulok*hoz létre. Modulok *üzeneteket* használnak az egymással való adatcsere céljából. A modul üzenetet kap, valamilyen műveletet végez, tetszés szerint értelmezhetjük új üzenet és majd közzéteszi azt más modulok feldolgozásához. Egyes modulok lehet, hogy csak az új üzenetek létrehozására és soha nem dolgozza fel a bejövő üzeneteket. Modulok láncolata adatokat feldolgozó csővezeték, az egyes modulok az átalakítás végrehajtása az adatokat, hogy a csővezeték egy pontján hoz létre.

![Az átjáró az Azure IoT átjáró SDK segítségével készített modulok láncolata][1]
 
Az SDK tartalmazza a következőket:

- Előzetes írásbeli modulok, amelyek általános átjáró funkciók használatához.
- A kapcsolódási pontok a fejlesztő egyéni modulokat használja.
- Az infrastruktúra telepítése és futtatása az modulok szükséges.

Az SDK tartalmaz a hardverabsztrakciós réteg, amely futtatni a különböző operációs rendszerek és platformok átjárók létrehozását teszi lehetővé.

![Borzas IoT átjáró SDK hardverabsztrakciós réteg][2]

### <a name="messages"></a>Üzenetek

Bár egyes üzenetek továbbítása modulok erőfeszítés hogyan egy átjáró funkciók conceptualize kényelmesen, azt nem pontosan tükrözi mi történik. Modulok ügynök segítségével kommunikálnak egymással, a munkamenet-átvitelszervező (busz, pubsub vagy bármely más üzenetkezelő minta) üzenetek közzététele és engedélyezze a munkamenet-átvitelszervező továbbítja az üzenetet a hozzá kapcsolódó modulok.

A modulok a **Broker_Publish** függvény segítségével egy üzenet közzététele a broker. A munkamenet-átvitelszervező kézbesíti az üzeneteket egy modul által visszahívási függvény meghívása. A közleménynek kulcs/érték tulajdonságai és át egy memóriablokk tartalmát.

![Az Azure IoT átjáró SDK közvetítő szerepe][3]

### <a name="message-routing-and-filtering"></a>Üzenet-útválasztást és szűrés

A megfelelő modulokban üzenetek irányítása két módja van. Hivatkozásokat tartalmaz a munkamenet-átvitelszervező továbbíthatók, így a munkamenet-átvitelszervező tudja, hogy a forrás- és gyűjtő modulokhoz tartozó, vagy a modul végezhet az üzenet tulajdonságait. Egy modult kell csak jár el egy üzenetet, ha azt az üzenetet szánják. A hivatkozások és az üzenet szűrés mi hatékonyan csővezeték üzenetet hoz létre.

## <a name="hello-world-sample-architecture"></a>Helló világ minta architektúra

Helló, világ minta szemlélteti az előző részben ismertetett fogalmak. Helló, világ mintát egy átjárót, amelyen áll két modulok a csővezeték valósítja meg:

-   A *Helló, világ* modul öt másodpercenként egy üzenetet hoz létre, és átadja a naplózó modul.
-   A *naplózó* modul fogadott üzeneteket fájlba írja.

![Helló, világ minta az Azure IoT átjáró SDK segítségével készített architektúra][4]

Az előző részben leírtak a Helló, világ modul nem átadni üzeneteket közvetlenül a naplózó modul öt másodpercenként. Ehelyett azt közzéteszi egy üzenetet a munkamenet-átvitelszervező öt másodpercenként.

A naplózó modul az üzenetet kap a broker, és műveleteket végez azok, az üzenet tartalmát egy fájlba írása.

A naplózó modul csak közvetítő üzeneteket dolgoz fel, azt soha nem teszi közzé az új üzenetek a broker.

![Hogyan közvetítő továbbítja az üzeneteket az Azure IoT átjáró SDK modulok között][5]

A fenti ábrán látható a Helló, világ mintát és a relatív elérési utak a forrásfájlok különböző részeit a minta a [tárház]megvalósító architektúra[lnk-gateway-sdk]. Felfedezhető a kódot, vagy a kódtöredékek alatt útmutatóként használja.

<!-- Images -->
[1]: media/iot-hub-gateway-sdk-getstarted-selector/modules.png
[2]: media/iot-hub-gateway-sdk-getstarted-selector/modules_2.png
[3]: media/iot-hub-gateway-sdk-getstarted-selector/messages_1.png
[4]: media/iot-hub-gateway-sdk-getstarted-selector/high_level_architecture.png
[5]: media/iot-hub-gateway-sdk-getstarted-selector/detailed_architecture.png

<!-- Links -->
[lnk-helloworld-sample]: https://github.com/Azure/azure-iot-gateway-sdk/tree/master/samples/hello_world
[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk