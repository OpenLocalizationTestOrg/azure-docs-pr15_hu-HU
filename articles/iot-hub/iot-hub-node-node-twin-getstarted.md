<properties
    pageTitle="Első lépések a twins |} Microsoft Azure"
    description="Ebből az oktatóanyagból megtudhatja, hogy miként twins használata"
    services="iot-hub"
    documentationCenter="node"
    authors="fsautomata"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="node"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/13/2016"
     ms.author="elioda"/>

# <a name="tutorial-get-started-with-device-twins-preview"></a>Oktatóprogram: Első lépések a eszköz twins (előzetes verzió)

[AZURE.INCLUDE [iot-hub-selector-twin-get-started](../../includes/iot-hub-selector-twin-get-started.md)]

Ez az oktatóanyag végén a két Node.js konzol alkalmazások vannak telepítve:

* **AddTagsAndQuery.js**, egy Node.js alkalmazás végéről vissza, amely felveszi a címkéket, és eszköz twins lekérdezések futtatásához szólnak.
* **TwinSimulatedDevice.js**, egy Node.js app mely keltő a szűrő megőrzi a IoT hubhoz csatlakozik, a korábban létrehozott eszköz identitású eszközt, és a jelentések kapcsolat állapotát.

> [AZURE.NOTE] A cikk [IoT központi SDK] [ lnk-hub-sdks] használható eszköz és a háttér-alkalmazások létrehozásához a különböző SDK információt tartalmaz.

Oktatóprogram elvégzéséhez az alábbiakra van szükség:

+ NODE.js verzió 0.10.x vagy újabb verziójában.

+ Azure active fiók. (Nem rendelkeznek fiókkal, ha egy [ingyenes fiókra] hozhat létre[ lnk-free-trial] a mindössze néhány perc.)

[AZURE.INCLUDE [iot-hub-get-started-create-hub-pp](../../includes/iot-hub-get-started-create-hub-pp.md)]

## <a name="create-the-service-app"></a>A szolgáltatás-alkalmazás létrehozása

Ebben a részben hoz létre a kettős **myDeviceId**társított metaadatok hely ellátó Node.js konzol alkalmazást. Azt, majd az eszközök kijelölése a központi tárolt twins található az Amerikai Egyesült Államok, majd a mobilhálózati kapcsolatot jelentéskészítés lekérdezések.

1. Hozzon létre egy új üres nevű **addtagsandqueryapp**. Hozzon létre egy új, az alábbi parancs használatával a parancssorban package.json fájlt a **addtagsandqueryapp** mappában. Fogadja el az alapértelmezett beállításokat:

    ```
    npm init
    ```

2. A-parancssorban a **addtagsandqueryapp** mappában, telepítse az **azure-iothub** csomagot a következő parancs futtatásával:

    ```
    npm install azure-iothub@dtpreview --save
    ```

3. Egy egyszerű szövegszerkesztő programban, hoz létre új **AddTagsAndQuery.js** fájl **addtagsandqueryapp** mappában.

4. A következő kód hozzáadása a **AddTagsAndQuery.js** fájlt, és a kapcsolati karakterlánc másolta a központi létrehozásakor **{szolgáltatás kapcsolati karakterlánc}** helyőrzőt helyettesítő:

        'use strict';
        var iothub = require('azure-iothub');
        var connectionString = '{service hub connection string}';
        var registry = iothub.Registry.fromConnectionString(connectionString);

        registry.getTwin('myDeviceId', function(err, twin){
            if (err) {
                console.error(err.constructor.name + ': ' + err.message);
            } else {
                var patch = {
                    tags: {
                        location: {
                            region: 'US',
                            plant: 'Redmond43'
                      }
                    }
                };
             
                twin.update(patch, function(err) {
                  if (err) {
                    console.error('Could not update twin: ' + err.constructor.name + ': ' + err.message);
                  } else {
                    console.log(twin.deviceId + ' twin updated successfully');
                    queryTwins();
                  }
                });
            }
        });

    A **beállításjegyzék** vezérlőnek eszköz twins a szolgáltatásból vezérléséhez szükséges összes módszereket. Az előző kód először előkészíti a **beállításjegyzék** -objektum, majd olvassa be az **myDeviceId**a kettős, és végül frissíti a címkék a kívánt helyre adatokkal.

    A címkék frissítése után felhívja a **queryTwins** függvényt.

7. Adja hozzá a következő kódot **AddTagsAndQuery.js** a **queryTwins** függvény végrehajtásához végén:

        var queryTwins = function() {
            var query = registry.createQuery("SELECT * FROM devices WHERE tags.location.plant = 'Redmond43'", 100);
            query.nextAsTwin(function(err, results) {
                if (err) {
                    console.error('Failed to fetch the results: ' + err.message);
                } else {
                    console.log("Devices in Redmond43: " + results.map(function(twin) {return twin.deviceId}).join(','));
                }
            });
            
            query = registry.createQuery("SELECT * FROM devices WHERE tags.location.plant = 'Redmond43' AND properties.reported.connectivity.type = 'cellular'", 100);
            query.nextAsTwin(function(err, results) {
                if (err) {
                    console.error('Failed to fetch the results: ' + err.message);
                } else {
                    console.log("Devices in Redmond43 using cellular network: " + results.map(function(twin) {return twin.deviceId}).join(','));
                }
            });
        };

    Az előző kód végrehajtja a két lekérdezések: az első csak a **Redmond43** növény található eszközök eszköz twins kijelöli, és a második finomítja a lekérdezésre, jelölje be a csak a mobil hálózaton keresztül is csatlakoztatott eszközökön.

    Figyelje meg, hogy az előző kódot, amikor létrehozza a **lekérdezési** objektum Itt adhatja meg a visszaadott dokumentumok maximális száma. A **lekérdezési** objektum összes eredmények beolvasásához többször az **nextAsTwin** módszerek meghívásához használható logikai **hasMoreResults** tulajdonság tartalmazza. A **következő** nevű mód, amelyek nem eszköz twins például összesítési lekérdezés eredményének eredmények érhető el.

8. Futtassa az alkalmazást:

        node AddTagsAndQuery.js

    Meg kell jelennie a lekérdezés megkérdezi, hogy az összes eszközön, olvassa el a **Redmond43** , és nincs a lekérdezést, amely az eredmények korlátozza eszközök mobil hálózatot használ az eredményeket egy eszközt.

    ![][1]

A következő szakaszban hoz létre, amely a jelentéseket a csatlakozási adatok és az előző szakaszban a lekérdezés eredménye változik eszköz alkalmazást.

## <a name="create-the-device-app"></a>Az eszköz-alkalmazás létrehozása

Ebben a részben hozzon létre egy Node.js **myDeviceId**, a hubhoz csatlakozik, és frissíti a kettős konzol alkalmazással jelentett tulajdonságok az, hogy csatlakoztatva van a mobil hálózaton keresztül információkat tartalmazza.

> [AZURE.NOTE] Ekkor eszköz twins elérhetők csak a MQTT protokollal IoT központi csatlakozó eszközök. Olvassa el a [támogatási MQTT] [ lnk-devguide-mqtt] miként meglévő eszköz alkalmazás konverziójának MQTT használni.

1. Hozzon létre egy új üres nevű **reportconnectivity**. Hozzon létre egy új, az alábbi parancs használatával a parancssorban package.json fájlt a **reportconnectivity** mappában. Fogadja el az alapértelmezett beállításokat:

    ```
    npm init
    ```

2. A-parancssorban a **reportconnectivity** mappában, telepítse az **azure-iot-eszköz**és **azure-iot-eszköz-mqtt** csomag a következő parancs futtatásával:

    ```
    npm install azure-iot-device@dtpreview azure-iot-device-mqtt@dtpreview --save
    ```

3. Egy egyszerű szövegszerkesztő programban, hoz létre új **ReportConnectivity.js** fájl **reportconnectivity** mappában.

4. A következő kód hozzáadása a **ReportConnectivity.js** fájlt, és a kapcsolati karakterlánc másolta a **myDeviceId** eszköz identitás létrehozásakor **{eszköz kapcsolati karakterlánc}** helyőrzőt helyettesítő:

        'use strict';
        var Client = require('azure-iot-device').Client;
        var Protocol = require('azure-iot-device-mqtt').Mqtt;

        var connectionString = '{device connection string}';
        var client = Client.fromConnectionString(connectionString, Protocol);

        client.open(function(err) {
        if (err) {
            console.error('could not open IotHub client');
        }  else {
            console.log('client opened');

            client.getTwin(function(err, twin) {
            if (err) {
                console.error('could not get twin');
            } else {
                var patch = {
                    connectivity: {
                        type: 'cellular'
                    }
                };

                twin.properties.reported.update(patch, function(err) {
                    if (err) {
                        console.error('could not update twin');
                    } else {
                        console.log('twin state reported');
                        process.exit();
                    }
                });
            }
            });
        }
        });

    Az **ügyfél** vezérlőnek van szüksége az interaktív eszköz twins az eszközről a minden módszert. Az előző kód után az **ügyfél** objektum állíthatja a kettős **myDeviceId** az, és az frissíti a jelentett tulajdonság a csatlakozási adatokkal.

5. Az eszköz alkalmazás futtatása

        node ReportConnectivity.js

    Meg kell jelennie a üzenet `twin state reported`.

6. Most, hogy az eszköz a csatlakozási adatok jelenteni, meg kell jelennie a két lekérdezések. Térjen vissza a **addtagsandqueryapp** mappában, és futtassa újra a lekérdezések:

        node AddTagsAndQuery.js

    Ez idő **myDeviceId** jelenjen meg mindkét lekérdezés eredményét.

    ![][3]

## <a name="next-steps"></a>Következő lépések
Ebben az oktatóprogramban egy új IoT hubhoz konfigurálni az Azure-portálon, és hozza létre a eszköz identitás a központi identitás beállításjegyzékben. Eszköz metaadatok összegként címkék háttéradatbázist alkalmazásból, és egy szimulált eszközön alkalmazás jelentés eszköz kapcsolódási információk az eszköz kettős beírva. Emellett megtanulta azt a keresés ezt az információt az SQL-szerű lekérdezési nyelv IoT-elosztót használ.

Az alábbi források segítségével megtudhatja, hogy miként:

- telemetriai küldése eszközökről való az [első lépések a IoT központi] [ lnk-iothub-getstarted] oktatóanyagban
- kettős meg a kívánt tulajdonságokat használata a [használni kívánt tulajdonságok eszközök konfigurálása] eszközök konfigurálása[ lnk-twin-how-to-configure] oktatóanyagban
- szabályozhatja az eszközök interaktív (például bekapcsolása a felhasználó által felügyelt alkalmazásból ventilátor), és a [használat közvetlen módszerek] [ lnk-methods-tutorial] oktatóprogram.

<!-- images -->
[1]: media/iot-hub-node-node-twin-getstarted/service1.png
[3]: media/iot-hub-node-node-twin-getstarted/service2.png

<!-- links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[lnk-d2c]: iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-identity]: iot-hub-devguide-identity-registry.md

[lnk-iothub-getstarted]: iot-hub-node-node-getstarted.md
[lnk-device-management]: iot-hub-device-management-get-started.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/

[lnk-twin-how-to-configure]: iot-hub-node-node-twin-how-to-configure.md
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md

[lnk-methods-tutorial]: iot-hub-c2d-methods.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md