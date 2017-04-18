> [AZURE.SELECTOR]
- [NODE.js](../articles/iot-hub/iot-hub-node-node-twin-getstarted.md)
- [C#](../articles/iot-hub/iot-hub-csharp-node-twin-getstarted.md)

## <a name="introduction"></a>– Bevezetés

Eszköz twins JSON dokumentumokat tároló eszköz állam információk (metaadatok konfigurációk és feltételek). IoT központi továbbra is fennáll, egy eszköz kettős az egyes IoT elosztóhoz csatlakoztatott eszközt.

Az eszköz twins használja:

* A visszahívási végéről eszköz-metaadatok tárolni.
* Az eszköz-alkalmazás jelentése a jelenlegi állapot adatai, például a rendelkezésre álló lehetőségek és a feltételt (például a csatlakozási használt módszer).
* (Például a belső vezérlőprogram és konfigurációs frissítések) hosszabb ideig futó munkafolyamatok állapotának szinkronizálása az eszköz alkalmazás és a hátsó vége között.
* A metaadatok eszköz, konfigurációs vagy állapot lekérdezés.

> [AZURE.NOTE] Eszköz twins készült szinkronizálás és eszköz beállításokat és a feltételek lekérdezésére. Használható [eszköz a felhőbe üzenetek] [ lnk-d2c] sorozatok rajta időbélyegző események (például telemetriai adatfolyam megjelenítését időalapú szenzoradatokat) és a [felhő-eszköz módszerek] az[ lnk-methods] az eszközök, például a felhasználó által felügyelt alkalmazásból egy ventilátorral bekapcsolásával interaktív vezérlőre.

Eszköz twins IoT-központban találhatók, és tartalmazza:

* *címkék*és az eszköz metaadatok csak a háttér; számára elérhető
* *a kívánt tulajdonságokat*, módosítható a vissza befejezése és az eszközhöz alkalmazással vált JSON-objektumok és
* *bejelentett tulajdonságait*, JSON objektumok módosítható eszközhöz alkalmazással és a vissza is olvashatják végén. Címkék és tulajdonságok tömbök nem tartalmazhat, de az objektumok ágyazhatók be.

![][img-twin]

Emellett a alkalmazás háttér is lekérdezhetők eszköz twins a fenti adatok alapján.
[Ismertetése eszköz twins] hivatkozni[ lnk-twins] további információt az eszköz twins és [IoT központi lekérdezési nyelv] [ lnk-query] lekérdezése hivatkozását.

> [AZURE.NOTE] Ekkor eszköz twins elérhetők csak a MQTT protokollal IoT központi csatlakozó eszközök. Olvassa el a [támogatási MQTT] [ lnk-devguide-mqtt] cikk utasításait a meglévő eszköz alkalmazás konverziójának MQTT.

Ebből az oktatóanyagból megtudhatja, hogyan szeretné:

- Hozzon létre az alkalmazás a háttéradatbázist, a *címkék* hozzáadja egy eszköz kettős és a saját csatlakozási csatorna jelentések, a *tulajdonság jelenteni* az eszköz kettős szimulált eszközt.
- A lekérdezés eszközök a szűrők használata a címkéket, és a korábban létrehozott tulajdonságok vissza vége alkalmazásból.


<!-- images -->
[img-twin]: media/iot-hub-selector-twin-get-started/twin.png

<!-- links -->
[lnk-query]: ../articles/iot-hub/iot-hub-devguide-query-language.md
[lnk-twins]: ../articles/iot-hub/iot-hub-devguide-device-twins.md
[lnk-d2c]: ../articles/iot-hub/iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-methods]: ../articles/iot-hub/iot-hub-devguide-direct-methods.md
[lnk-devguide-mqtt]: ../articles/iot-hub/iot-hub-mqtt-support.md