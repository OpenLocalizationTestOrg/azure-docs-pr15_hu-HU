<properties
    pageTitle="Az átjáró IoT SDK valós eszközzel |} Microsoft Azure"
    description="Azure IoT átjáró SDK forgatókönyv Texas eszközök SensorTag eszközt használ IoT központi adatokat küldeni az Intel Edison kiszámítania modulon rendszert futtató átjárón keresztül"
    services="iot-hub"
    documentationCenter=""
    authors="chipalost"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="cpp"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="08/29/2016"
     ms.author="andbuc"/>


# <a name="azure-iot-gateway-sdk-beta--send-device-to-cloud-messages-with-a-real-device-using-linux"></a>Azure IoT átjáró SDK (beta) – segítségével Linux valós eszközön eszköz a felhőbe az üzenetek küldése

Ez a forgatókönyv a [Bluetooth alacsony energy minta] [ lnk-ble-samplecode] megtudhatja, hogy miként használhatja az [Azure IoT átjáró SDK] [ lnk-sdk] szeretne egy fizikai eszközök és parancsok IoT központból fizikai eszközök irányítása hogyan IoT hubhoz előre eszköz a felhőbe telemetriai.

Ez a forgatókönyv foglalja magában:

* **Architektúra**: a Bluetooth alacsony energy minta építészeti fontos információkat.

* **Szerkesztés és a Futtatás**: a össze, és a minta használatához szükséges lépéseket.

## <a name="architecture"></a>Architektúra

Az útmutató bemutatja, hogyan létre, és az Intel Edison kiszámítania modul Linux futtató egy IoT átjáró futtatni. Az átjáró épül a IoT átjáró SDK használatával. A minta a hőmérsékleti adatok gyűjtése a Texas eszközök SensorTag Bluetooth alacsony Energy (táblázat) eszközt használ.

Az átjáró futtatásakor azt:

- A Bluetooth alacsony Energy (táblázat) protokoll használatával SensorTag eszköz csatlakozik.
- A HTTP protokollal IoT-hubhoz csatlakozik.
- Telemetriai továbbítja az SensorTag eszközről IoT-hubon keresztül csatlakozott.
- Parancsok az SensorTag eszköz IoT központból irányítja.

Az átjáró a következő modulokat tartalmazza:

- A *táblázat modul* , amely az eszköz és a Küldés parancs az eszközre hőmérsékleti adatok fogadni táblázat eszközön felületek.
- *Táblázat felhő eszköz modul* , amely a táblázat útmutatást a *táblázat modul*az a felhő érkező JSON üzeneteket.
- A *naplózási modul* , amely naplózza az összes átjárót üzenet.
- Az *identitás megfeleltetése modul* , amely a táblázat eszköz MAC-címek és Azure IoT központi eszköz azonosítók között.
- Egy *központi IoT modul* , amely telemetriai adatokat feltöltések IoT-hubon keresztül csatlakozott, és eszköz-parancsok kap egy IoT központból.
- A *táblázat nyomtató modul* , amely a táblázat eszközről telemetriai értelmezi és nyomtatása a hibaelhárítás és hibakeresés engedélyezése konzolhoz adatok formázva.

### <a name="how-data-flows-through-the-gateway"></a>Hogyan adatok halad-e az átjáró keresztül

A következő Blokkdiagram bemutatja a telemetriai feltöltés adatok folyamat során:

![](media/iot-hub-gateway-sdk-physical-device/gateway_ble_upload_data_flow.png)

Telemetriai, ha megnyitja a táblázat eszközről IoT hubhoz út közben lépések a következők:

1. A táblázat eszköz hőmérsékleti minta hoz létre, és Bluetooth keresztül küld az átjáró táblázat moduljának.
2. A táblázat modul a minta fogadása, és azt közzé, és a MAC-cím, az eszköz közvetítő.
3. Az identitás megfeleltetés modul felveszi a ezt az üzenetet, és egy belső tábla használja a MAC-cím, az eszköz lefordítása IoT központi eszköz identitás (Eszközazonosítót és eszköz billentyűt). Kattintson egy új üzenetet, amely tartalmazza a hőmérsékleti mintaadatokat, a MAC-az eszközt, az Eszközazonosítót és a eszköz billentyűt cím teszi közzé.
4. A központi IoT modul (az identitás-megfeleltetés modul által generált), az új üzenet fogadása, és információkat tesz közzé IoT-hubon keresztül csatlakozott.
5. A naplózási modul bejelentkezik összes üzenet közvetítő egy fájl.

A következő Blokkdiagram bemutatja a eszköz parancs adatok folyamat során:

![](media/iot-hub-gateway-sdk-physical-device/gateway_ble_command_data_flow.png)

1. A központi IoT modul rendszeres lekérdezi a IoT központi parancs az új üzenetekhez.
2. A központi IoT modul parancs új üzenetet kap, amikor azt közzé, akkor közvetítő.
3. Az identitás megfeleltetés modul felveszi a parancs üzenetet, és a MAC-cím eszközre IoT központi Eszközazonosítót fordítása egy belső tábla használ. Kattintson egy új üzenetet, amely tartalmazza a célként megadott eszköz MAC-címét az üzenet tulajdonságok térképe teszi közzé.
4. A táblázat felhő eszköz modulhoz felveszi a ezt az üzenetet, és a megfelelő táblázat utasítást a táblázat modulhoz átalakítja azt. Kattintson egy új üzenet teszi közzé.
5. A táblázat modul felveszi a ezt az üzenetet, és végrehajtja a I/O utasítás által a táblázat lejátszóval való kommunikáció.
6. A naplózási modul bejelentkezik összes üzenet közvetítő egy fájl.

## <a name="prepare-your-hardware"></a>A hardver előkészítése

Ebben az oktatóanyagban feltételezi, hogy kapcsolódik az Intel Edison fal [Texas eszközök SensorTag](http://www.ti.com/ww/en/wireless_connectivity/sensortag2015/index.html) eszköz használata esetén.

### <a name="set-up-the-edison-board"></a>A Edison fal beállítása

Mielőtt hozzákezdene, győződjön meg arról, hogy tud csatlakozni az Edison eszköz a vezeték nélküli hálózathoz. Állítsa be a Edison eszközt, szüksége egy gazdaszámítógép csatlakozni. Intel tartalmaz, az alábbi operációs rendszerek az útmutatók az első lépésekhez:

- [Első lépések a Windows 64 bites Intel Edison fejlesztési falra][lnk-setup-win64].
- [Első lépések a Windows 32 bites Intel Edison fejlesztési falra][lnk-setup-win32].
- [Első lépések az Intel Edison fejlesztési fal Mac OS X rendszeren][lnk-setup-osx].
- [Első lépések a Intel® Edison fal Linux][lnk-setup-linux].

A Edison eszköz beállítása és ismerkedjen meg vele, kell végrehajtania, az összes ezeket a lépéseket "első lépések" cikkek kivételével az utolsó lépésben "Kattintson IDE", amely nincs szükség a az aktuális tanulásra. A Edison telepítési folyamatot végén van:

- A legújabb belső vezérlőprogram a Edison fellobban.
- Létesített a Edison állomásról a soros kapcsolatot.
- Futtassa a **configure_edison** jelszó beállítása, és kattintson a Edison WiFi engedélyezése.

### <a name="enable-connectivity-to-the-sensortag-device-from-your-edison-board"></a>Kapcsolatok engedélyezése a SensorTag eszközre a Edison tábláról

A minta futtatásához ellenőrizni szeretné, hogy a Edison fal SensorTag eszköz csatlakozhat.

Először meg kell ellenőrizze, hogy a Edison SensorTag eszköz csatlakozhat.

1. Kattintson a Edison bluetooth tiltásának feloldása, és ellenőrizze, hogy a verziószám **5.37**.
    
    ```
    rfkill unblock bluetooth
    bluetoothctl --version
    ```

2. A **bluetoothctl** parancs végrehajtása. Most már áll egy interaktív bluetooth rendszerhéj. 

3. Írja be a parancsot **a power** power be a Bluetooth-vezérlő. Kimeneti hasonlóan kell megjelennie:
    
    ```
    [NEW] Controller 98:4F:EE:04:1F:DF edison [default]
    ```

4. Miközben továbbra is a interaktív bluetooth rendszerhéj adja meg a parancsot **a beolvasás** szeretne képet beolvasni a bluetooth-eszközt. Kimeneti hasonlóan kell megjelennie:
    
    ```
    Discovery started
    [CHG] Controller 98:4F:EE:04:1F:DF Discovering: yes
    ```

5. Áttekinthetővé tételére SensorTag eszköz a kis gombra (a zöld LED kell flash) billentyűkombinációval. A Edison kell felfedezése az SensorTag eszköz:
    
    ```
    [NEW] Device A0:E6:F8:B5:F6:00 CC2650 SensorTag
    [CHG] Device A0:E6:F8:B5:F6:00 TxPower: 0
    [CHG] Device A0:E6:F8:B5:F6:00 RSSI: -43
    ```
    
    Ebben a példában látható, hogy a MAC-cím az SensorTag eszköz **A0:E6:F8:B5:F6:00**-e.

6. Kapcsolja ki a **képet beolvasni a kikapcsolása** parancs megadásával végzett vizsgálatot.
    
    ```
    [CHG] Controller 98:4F:EE:04:1F:DF Discovering: no
    Discovery stopped
    ```

7. Csatlakozás az SensorTag eszköz használatával a MAC-cím beírásával **Csatlakozás \<MAC cím >**. Tartsa szem előtt, hogy rövidítve-e a az alábbi példa eredménye:
    
    ```
    Attempting to connect to A0:E6:F8:B5:F6:00
    [CHG] Device A0:E6:F8:B5:F6:00 Connected: yes
    Connection successful
    [CHG] Device A0:E6:F8:B5:F6:00 UUIDs: 00001800-0000-1000-8000-00805f9b34fb
    ...
    [NEW] Primary Service
            /org/bluez/hci0/dev_A0_E6_F8_B5_F6_00/service000c
            Device Information
    ...
    [CHG] Device A0:E6:F8:B5:F6:00 GattServices: /org/bluez/hci0/dev_A0_E6_F8_B5_F6_00/service000c
    ...
    [CHG] Device A0:E6:F8:B5:F6:00 Name: SensorTag 2.0
    [CHG] Device A0:E6:F8:B5:F6:00 Alias: SensorTag 2.0
    [CHG] Device A0:E6:F8:B5:F6:00 Modalias: bluetooth:v000Dp0000d0110
    ```
    
    Megjegyzés: Az eszköz ismét a **lista-attribútumok** paranccsal GATT jellemzői jeleníthet meg.

8. Az eszköz **leválasztása** paranccsal most választani, és lépjen ki az bluetooth rendszerhéj a **Kilépés a** parancs használatával:
    
    ```
    Attempting to disconnect from A0:E6:F8:B5:F6:00
    Successful disconnected
    [CHG] Device A0:E6:F8:B5:F6:00 Connected: no
    ```

Most már készen áll a táblázat átjáró minta Edison eszközén futtatásához.

## <a name="run-the-ble-gateway-sample"></a>A táblázat átjáró minta futtatása

A táblázat minta futtatásához a Edison kell három feladatok elvégzésére:

- Két példa eszköz beállítása a IoT-központját.
- Az átjáró IoT SDK épülnek Edison eszközére.
- Állítsa be, és futtassa a táblázat minta Edison eszközén.

Az írás időpontjában a IoT átjáró SDK csomagjában talál csak Linux táblázat modulok használó átjárók használatát támogatja.

### <a name="configure-two-sample-devices-in-your-iot-hub"></a>Két példa eszköz beállítása a IoT-központban

- [Hozzon létre egy IoT központi] [ lnk-create-hub] az Azure-előfizetése, szüksége lesz az útmutató elvégzéséhez a központi nevét. Ha nem rendelkeznek fiókkal, létrehozhat egy [ingyenes fiókot] [ lnk-free-trial] a mindössze néhány perc.
- Hozzá egy eszközt **SensorTag_01** nevű a IoT hubon keresztül csatlakozott, és jegyezze fel az azonosító és eszköz kulcsának. A használható [eszköz Intéző vagy iothub-explorer] [ lnk-explorer-tools] eszközöket, ez az eszköz hozzáadása a IoT hubon keresztül csatlakozott, akkor az előző lépésben létrehozott beolvasni a kulcsot. Az átjáró konfigurálásakor ez az eszköz fog megfeleltetése a SensorTag eszközt.

### <a name="build-the-iot-gateway-sdk-on-your-edison-device"></a>Az átjáró IoT SDK épülnek Edison eszköze

**Mely számjegy** a Edsion a verziója nem támogatja a submodules. A teljes forrás töltheti le a IoT átjáró SDK csomagjában talál a Edison való lehetőségei vannak:

- #1 beállítás: Az [Azure IoT átjáró SDK] klónozhatja[ lnk-sdk] a Edison tárházából és manuálisan klónozhatja minden submodule tárháza.
- #2 beállítás: Az [Azure IoT átjáró SDK] klónozhatja[ lnk-sdk] tárházba, ahol **mely számjegy** submodules támogatja, és másolja a teljes tárházba az alakzatot a Edison submodules asztali eszközön.

Ha a #2 lehetőséget választja, az alábbi parancsokkal **mely számjegy** a IoT átjáró SDK és annak a submodules klónozhatja:

```
git clone --recursive https://github.com/Azure/azure-iot-gateway-sdk.git 
git submodule update --init --recursive
```

Meg kell majd zip-a teljes helyi tárházba egyetlen archív fájlba előtt másolja a Edison. Használhatja például megtalálható a **gitt** szeretné másolni az archív mappák a Edison **pscp** segédprogramot. Példa:

```
pscp .\gatewaysdk.zip root@192.168.0.45:/home/root
```

Amikor a Edison a teljes másolatát a IoT átjáró SDK tárat, vagy azt a mappát, amely tartalmazza a SDK csomagjában talál a következő paranccsal:

```
./tools/build.sh
```

### <a name="configure-and-run-the-ble-sample-on-your-edison-device"></a>Állítsa be, és a táblázat minta futtatása Edison mobileszközön

Bootstrap, és futtassa a minta, meg kell minden modulban, amely vesz az átjáró beállítása. Ez a beállítás van megadva, JSON fájl, és minden öt részt vevő modul konfigurálnia kell. A tárat, használhatja a saját konfigurációs fájl készítéséhez kezdőpontjának **gateway_sample.json** nevű leírt JSON mintafájl van. Ezt a fájlt a **minták/ble_gateway_hl/src** mappában van, a helyi másolatát a IoT átjáró SDK tárat.

Az alábbi szakaszok ismertetik, hogyan lehet ez a táblázat minta konfigurációs fájl szerkesztése és feltételezik, hogy a IoT átjáró SDK tárat a a **/home/root/azure-iot-gateway-sdk /** Edison eszközén mappát. Ha a tárházba máshol, az elérési út megfelelően kell beállításához:

#### <a name="logger-configuration"></a>Naplózó konfigurálása

Feltételezve, hogy az átjáró tárházba a mappában található **/home/root/azure-iot-gateway-sdk /**, állítsa be az alábbi képlettel történik a naplózó modul:

```json
{
    "module name": "logger",
    "module path": "/home/root/azure-iot-gateway-sdk/build/modules/logger/liblogger_hl.so",
    "args":
    {
        "filename":"/home/root/gw_logger.log"
    }
}
```

#### <a name="ble-module-configuration"></a>Táblázat modul konfigurálása

A minta konfiguráció a táblázat eszköz feltételezi, hogy egy Texas eszközök SensorTag eszközt. Minden olyan normál táblázat eszköz, amely a külső GATT kell munka közben is működik, de kell frissíteni a GATT jellemző azonosítókhoz és az adatok (az írási utasítások). Adja hozzá a MAC-címét az SensorTag eszköz: 

```json
{
  "module name": "SensorTag",
  "module path": "/home/root/azure-iot-gateway-sdk/build/modules/ble/libble_hl.so",
  "args": {
    "controller_index": 0,
    "device_mac_address": "<<AA:BB:CC:DD:EE:FF>>",
    "instructions": [
      {
        "type": "read_once",
        "characteristic_uuid": "00002A24-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A25-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A26-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A27-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A28-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A29-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "write_at_init",
        "characteristic_uuid": "F000AA02-0451-4000-B000-000000000000",
        "data": "AQ=="
      },
      {
        "type": "read_periodic",
        "characteristic_uuid": "F000AA01-0451-4000-B000-000000000000",
        "interval_in_ms": 1000
      },
      {
        "type": "write_at_exit",
        "characteristic_uuid": "F000AA02-0451-4000-B000-000000000000",
        "data": "AA=="
      }
    ]
  }
}
```

#### <a name="iot-hub-module"></a>IoT központi modul

Adja meg a IoT központi nevét. Utótag érték általában **azure-devices.net**:

```json
{
  "module name": "IoTHub",
  "module path": "/home/root/azure-iot-gateway-sdk/build/modules/iothub/libiothub_hl.so",
  "args": {
    "IoTHubName": "<<Azure IoT Hub Name>>",
    "IoTHubSuffix": "<<Azure IoT Hub Suffix>>",
    "Transport": "HTTP"
  }
}
```

#### <a name="identity-mapping-module-configuration"></a>Identitás megfeleltetésének modul konfigurációja

A MAC-címét a SensorTag és az eszköz azonosító és a hozzá a IoT központi **SensorTag_01** eszköz kulcs felvétele:

```json
{
  "module name": "mapping",
  "module path": "/home/root/azure-iot-gateway-sdk/build/modules/identitymap/libidentity_map_hl.so",
  "args": [
    {
      "macAddress": "<<AA:BB:CC:DD:EE:FF>>",
      "deviceId": "<<Azure IoT Hub Device ID>>",
      "deviceKey": "<<Azure IoT Hub Device Key>>"
    }
  ]
}
```

#### <a name="ble-printer-module-configuration"></a>Táblázat nyomtatási modul konfigurálása

```json
{
    "module name": "BLE Printer",
    "module path": "/home/root/azure-iot-gateway-sdk/build/samples/ble_gateway_hl/ble_printer/libble_printer.so",
    "args": null
}
```

#### <a name="routing-configuration"></a>Útválasztási konfiguráció

Az alábbi beállításokkal biztosíthatja az alábbiakat:
- A **naplózási** modul kap, és jelentkezzen be az összes üzenetre.
- A **SensorTag** modul üzeneteket küld a **hozzárendelés** és a **Táblázat nyomtató** modulokat.
- A **leképezési** modul üzeneteket küld a **IoTHub** modult, és elküldését a IoT hubon keresztül csatlakozott.
- A **IoTHub** modul elküldi üzeneteket a **hozzárendelés** modulra.
- A **leképezési** modul elküldi üzeneteket a **SensorTag** modulra.

```json
"links" : [
    {"source" : "*", "sink" : "Logger" },
    {"source" : "SensorTag", "sink" : "mapping" },
    {"source" : "SensorTag", "sink" : "BLE Printer" },
    {"source" : "mapping", "sink" : "IoTHub" },
    {"source" : "IoTHub", "sink" : "mapping" },
    {"source" : "mapping", "sink" : "SensorTag" }
  ]
```

A minta futtatásához a **ble_gateway_hl** bináris a JSON konfigurációs fájl elérési átadása futtatnia. Ha korábban a **gateway_sample.json** fájlt, akkor a végrehajtandó parancs néz ki:

```
./build/samples/ble_gateway_hl/ble_gateway_hl ./samples/ble_gateway_hl/src/gateway_sample.json
```

Szükség lehet a tételére ezt a minta futtatása előtt SensorTag eszközön nyomja le az ENTER a kis gombra.

A minta futtatásakor a használható [eszköz Intéző vagy iothub-explorer] [ lnk-explorer-tools] eszköz figyelemmel az üzeneteket, az átjáró továbbítja az SensorTag eszközről.

## <a name="send-cloud-to-device-messages"></a>Felhőalapú-eszköz küldésére

A táblázat modul is támogatja az eszközre az Azure IoT központból küldő utasításokat. Az [Azure IoT központi eszköz Explorer](https://github.com/Azure/azure-iot-sdks/blob/master/tools/DeviceExplorer/doc/how_to_use_device_explorer.md) vagy a [IoT központi Intéző](https://github.com/Azure/azure-iot-sdks/tree/master/tools/iothub-explorer) használatával, amely a táblázat eszköz továbbítja a táblázat átjáró modul JSON üzenetek küldése. Például ha a Texas eszközök SensorTag eszközt használ majd elküldheti a következő JSON üzenetek az eszközre IoT központból.

- Az összes LED és a berregő alaphelyzetbe (kikapcsolhatja ezt a funkciót)

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "AA=="
    }
    ```

- Állítsa be I/O "távoli"

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA66-0451-4000-B000-000000000000",
      "data": "AQ=="
    }
    ```

- A piros LED bekapcsolása

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "AQ=="
    }
    ```

- A zöld LED bekapcsolása

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "Ag=="
    }
    ```

- A berregő bekapcsolása

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "BA=="
    }
    ```

Az alapértelmezett működés egy eszköz IoT központban összekapcsolása a HTTP protokollal is jelölje be a 25 percenként az új parancsot. Ezért ha több külön parancsok küldi el szeretne várja meg a 25 percig, amíg az eszközre a minden parancs fogadása.

> [AZURE.NOTE] Az átjáró is ellenőrzi, hogy új parancsok minden indításakor úgy, hogy az átjáró indítása és leállítása parancs feldolgozásához kényszerítheti.

## <a name="next-steps"></a>Következő lépések

Ha szerezhet a IoT átjáró SDK csomagjában talál egy speciális megértését és kód példák kísérletezni szeretne, látogasson el a következő Fejlesztőeszközök oktatóanyagok és erőforrások:

- [Azure IoT átjáró SDK][lnk-sdk]

További feltárása a IoT központi funkcióinak, olvassa el:

- [Fejlesztőeszközök útmutató][lnk-devguide]

<!-- Links -->
[lnk-ble-samplecode]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/samples/ble_gateway_hl
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-explorer-tools]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/manage_iot_hub.md
[lnk-setup-win64]: https://software.intel.com/get-started-edison-windows
[lnk-setup-win32]: https://software.intel.com/get-started-edison-windows-32
[lnk-setup-osx]: https://software.intel.com/get-started-edison-osx
[lnk-setup-linux]: https://software.intel.com/get-started-edison-linux
[lnk-sdk]: https://github.com/Azure/azure-iot-gateway-sdk/


[lnk-devguide]: iot-hub-devguide.md
[lnk-create-hub]: iot-hub-create-through-portal.md 
