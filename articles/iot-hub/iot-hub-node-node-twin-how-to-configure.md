<properties
    pageTitle="Kettős tulajdonságainak használata |} Microsoft Azure"
    description="Ebből az oktatóanyagból megtudhatja, hogy miként kettős tulajdonságainak használata"
    services="iot-hub"
    documentationCenter=".net"
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

# <a name="tutorial-use-desired-properties-to-configure-devices-preview"></a>Oktatóprogram: Használni kívánt tulajdonságok eszközök (előzetes verzió) konfigurálása

[AZURE.INCLUDE [iot-hub-selector-twin-how-to-configure](../../includes/iot-hub-selector-twin-how-to-configure.md)]

Ez az oktatóanyag végén a két Node.js konzol alkalmazások vannak telepítve:

* **SimulateDeviceConfiguration.js**, alkalmazás a szimulált eszközön, megvárja, amíg a kívánt konfiguráció frissítése, és a jelentések egy szimulált konfigurációs frissítési folyamat állapotát.
* **SetDesiredConfigurationAndQuery.js**, egy Node.js alkalmazás végéről vissza, amely állítja be a kívánt konfiguráció eszközön, és a konfigurációs frissítési folyamat lekérdezések futtatásához szólnak.

> [AZURE.NOTE] A cikk [IoT központi SDK] [ lnk-hub-sdks] használható eszköz és a háttér-alkalmazások létrehozásához a különböző SDK információt tartalmaz.

Oktatóprogram elvégzéséhez az alábbiakra van szükség:

+ NODE.js verzió 0.10.x vagy újabb verziójában.

+ Azure active fiók. (Nem rendelkeznek fiókkal, ha egy [ingyenes fiókra] hozhat létre[ lnk-free-trial] a mindössze néhány perc.)

Ha követni, az [első lépések az eszköz twins] [ lnk-twin-tutorial] oktatóanyagban már van egy eszköz engedélyezett felügyeleti központban, és egy eszközt identitás nevű **myDeviceId**; és [a szimulált eszköz-alkalmazás létrehozása] kihagyhatja[ lnk-how-to-configure-createapp] szakaszban.

[AZURE.INCLUDE [iot-hub-get-started-create-hub-pp](../../includes/iot-hub-get-started-create-hub-pp.md)]

## <a name="create-the-simulated-device-app"></a>A szimulált eszköz-alkalmazás létrehozása

Ebben a részben hoz létre, amelyek másként **myDeviceId**a hubhoz csatlakozik, megvárja, amíg a kívánt konfiguráció frissítése és frissítések majd jelentéseket a szimulált konfigurációs frissítési folyamat Node.js konzol alkalmazást.

1. Hozzon létre egy új üres nevű **simulatedeviceconfiguration**. Hozzon létre egy új, az alábbi parancs használatával a parancssorban package.json fájlt a **simulatedeviceconfiguration** mappában. Fogadja el az alapértelmezett beállításokat:

    ```
    npm init
    ```

2. A-parancssorban a **simulatedeviceconfiguration** mappában, telepítse az **azure-iot-eszköz**és **azure-iot-eszköz-mqtt** csomag a következő parancs futtatásával:

    ```
    npm install azure-iot-device@dtpreview azure-iot-device-mqtt@dtpreview --save
    ```

3. Egy egyszerű szövegszerkesztő programban, hoz létre új **SimulateDeviceConfiguration.js** fájl **simulatedeviceconfiguration** mappában.

4. A következő kód hozzáadása a **SimulateDeviceConfiguration.js** fájlt, és a kapcsolati karakterlánc másolta a **myDeviceId** eszköz identitás létrehozásakor **{eszköz kapcsolati karakterlánc}** helyőrzőt helyettesítő:

        'use strict';
        var Client = require('azure-iot-device').Client;
        var Protocol = require('azure-iot-device-mqtt').Mqtt;

        var connectionString = '{device connection string}';
        var client = Client.fromConnectionString(connectionString, Protocol);

        client.open(function(err) {
            if (err) {
                console.error('could not open IotHub client');
            } else {
                client.getTwin(function(err, twin) {
                    if (err) {
                        console.error('could not get twin');
                    } else {
                        console.log('retrieved device twin');
                        twin.properties.reported.telemetryConfig = {
                            configId: "0",
                            sendFrequency: "24h"
                        }
                        twin.on('properties.desired', function(desiredChange) {
                            console.log("received change: "+JSON.stringify(desiredChange));
                            var currentTelemetryConfig = twin.properties.reported.telemetryConfig;
                            if (desiredChange.telemetryConfig &&desiredChange.telemetryConfig.configId !== currentTelemetryConfig.configId) {
                                initConfigChange(twin);
                            }
                        });
                    }
                });
            }
        });

    Az **ügyfél** vezérlőnek vezérléséhez az eszközről eszköz twins szükséges összes módszereket. Az előző kód után az **ügyfél** objektum állíthatja olvassa be a kettős **myDeviceId**az, és csatolja a kívánt tulajdonságokat frissítés leíró. A kezelő ellenőrzi, hogy van egy tényleges konfigurációs módosítás-kérési összehasonlíthatja a configIds, akkor egy módszer, amely elindítja a konfigurációs módosítást elindítja.

    Ne feledje, hogy az egyszerűség kedvéért az előző kód használja egy csomagolásukkor alapértelmezett a kiindulási konfiguráció. A valós alkalmazás valószínűleg töltenének be, hogy a konfigurációs egy helyi tárból.
    
    > [AZURE.IMPORTANT] A kívánt tulajdonság módosítása események vannak mindig kibocsátott egyszer eszköz csatlakozáskor, ügyeljen arra, hogy ellenőrizze, hogy van egy tényleges módosítást kívánt tulajdonságok bármely művelet végrehajtása előtt.

5. Az alábbi módszerek előtt hozzáadása a `client.open()` meghívása:

        var initConfigChange = function(twin) {
            var currentTelemetryConfig = twin.properties.reported.telemetryConfig;
            currentTelemetryConfig.pendingConfig = twin.properties.desired.telemetryConfig;
            currentTelemetryConfig.status = "Pending";

            var patch = {
            telemetryConfig: currentTelemetryConfig
            };
            twin.properties.reported.update(patch, function(err) {
                if (err) {
                    console.log('Could not report properties');
                } else {
                    console.log('Reported pending config change: ' + JSON.stringify(patch));
                    setTimeout(function() {completeConfigChange(twin);}, 60000);
                }
            });
        }

        var completeConfigChange =  function(twin) {
            var currentTelemetryConfig = twin.properties.reported.telemetryConfig;
            currentTelemetryConfig.configId = currentTelemetryConfig.pendingConfig.configId;
            currentTelemetryConfig.sendFrequency = currentTelemetryConfig.pendingConfig.sendFrequency;
            currentTelemetryConfig.status = "Success";
            delete currentTelemetryConfig.pendingConfig;
            
            var patch = {
                telemetryConfig: currentTelemetryConfig
            };
            patch.telemetryConfig.pendingConfig = null;

            twin.properties.reported.update(patch, function(err) {
                if (err) {
                    console.error('Error reporting properties: ' + err);
                } else {
                    console.log('Reported completed config change: ' + JSON.stringify(patch));
                }
            });
        };

    A **initConfigChange** módszer a helyi kettős objektum jelentett tulajdonságai frissíti a config frissítési kérelem állítja az állapot **függőben lévő**, majd frissíti az eszköz kettős a szolgáltatás. Miután sikeresen frissítette a kettős, szimulál, amely a **completeConfigChange**végrehajtását megszakítja hosszú futó folyamatot. Ez a módszer frissíti a helyi kettős jelentett tulajdonságok **sikeres** az állapot beállítása és eltávolítása a **pendingConfig** objektumot. A szolgáltatás a kettős majd frissíti.

    Megjegyzés: sávszélességet, mentéséhez, bejelenteni tulajdonságait módosítani kell cserélni a teljes dokumentum helyett (elnevezett **javítás** a fenti kód), csak a tulajdonságok megadásával frissülnek.

    > [AZURE.NOTE] Ebben az oktatóanyagban nem szimulálhatja bármely viselkedés egyidejű konfigurációs frissítéseit. Néhány konfigurációs frissítési folyamat cél konfiguráció módosítások igazodik, miközben fut-e a frissítés, mások kell kattintania a sorból vagy mások is hibát elvetése lehet. Ügyeljen arra, hogy fontolja meg a kívánt a konkrét beállítási folyamat működését, és a megfelelő logika hozzáadása a konfigurációs módosítást kezdeményezése előtt.

6. Az eszköz app futtatásával:

        node SimulateDeviceConfiguration.js

    Meg kell jelennie a üzenet `retrieved device twin`. Hagyja meg az alkalmazást futtató.

## <a name="create-the-service-app"></a>A szolgáltatás-alkalmazás létrehozása

Ez a szakasz létrehoz egy Node.js konzol alkalmazás, amely a *kívánt tulajdonságokat* a kettős **myDeviceId** új telemetriai konfigurációs objektummal társított frissíti. Ezután az eszköz twins a központi tárolt lekérdezések és a kívánt és jelenteni az eszköz konfigurációk közötti különbséget jeleníti meg.

1. Hozzon létre egy új üres nevű **setdesiredandqueryapp**. Hozzon létre egy új, az alábbi parancs használatával a parancssorban package.json fájlt a **setdesiredandqueryapp** mappában. Fogadja el az alapértelmezett beállításokat:

    ```
    npm init
    ```

2. A-parancssorban a **setdesiredandqueryapp** mappában, telepítse az **azure-iothub** csomagot a következő parancs futtatásával:

    ```
    npm install azure-iothub@dtpreview node-uuid --save
    ```

3. Egy egyszerű szövegszerkesztő programban, hoz létre új **SetDesiredAndQuery.js** fájl **addtagsandqueryapp** mappában.

4. A következő kód hozzáadása a **SetDesiredAndQuery.js** fájlt, és a kapcsolati karakterlánc másolta a központi létrehozásakor **{szolgáltatás kapcsolati karakterlánc}** helyőrzőt helyettesítő:

        'use strict';
        var iothub = require('azure-iothub');
        var uuid = require('node-uuid');
        var connectionString = '{service connection string}';
        var registry = iothub.Registry.fromConnectionString(connectionString);
         
        registry.getTwin('myDeviceId', function(err, twin){
            if (err) {
                console.error(err.constructor.name + ': ' + err.message);
            } else {
                var newConfigId = uuid.v4();
                var newFrequency = process.argv[2] || "5m";
                var patch = {
                    properties: {
                        desired: {
                            telemetryConfig: {
                                configId: newConfigId,
                                sendFrequency: newFrequency
                            }
                        }
                    }
                }
                twin.update(patch, function(err) {
                    if (err) {
                        console.error('Could not update twin: ' + err.constructor.name + ': ' + err.message);
                    } else {
                        console.log(twin.deviceId + ' twin updated successfully');
                    }
                });
                setInterval(queryTwins, 10000);
            }
        });
            

    A **beállításjegyzék** vezérlőnek eszköz twins a szolgáltatásból vezérléséhez szükséges összes módszereket. Az előző kódot, után a **beállításjegyzék** objektum állíthatja olvassa be az **myDeviceId**a kettős, és a kívánt tulajdonságait frissíti egy új telemetriai konfigurációs objektummal. Ezt követően meghívja a **queryTwins** függvény esemény 10 másodperc.

    > [AZURE.IMPORTANT] Ez az alkalmazás minden 10 másodperc szemléltető célokra IoT központi kérdezi le. Lekérdezések használata a felhasználó elérésű jelentések készítése minden több eszközön, és nem észleli azokat a módosításokat. Ha a megoldás igényel eszköz események valós idejű értesítések használata [eszköz a felhőbe üzenetek][lnk-d2c].

5. Adja hozzá a következő kódot jobb előtt a `registry.getDeviceTwin()` függvénymeghívás a **queryTwins** függvény végrehajtása:

        var queryTwins = function() {
            var query = registry.createQuery("SELECT * FROM devices WHERE deviceId = 'myDeviceId'", 100);
            query.nextAsTwin(function(err, results) {
                if (err) {
                    console.error('Failed to fetch the results: ' + err.message);
                } else {
                    console.log();
                    results.forEach(function(twin) {
                        var desiredConfig = twin.properties.desired.telemetryConfig;
                        var reportedConfig = twin.properties.reported.telemetryConfig;
                        console.log("Config report for: " + twin.deviceId);
                        console.log("Desired: ");
                        console.log(JSON.stringify(desiredConfig, null, 2));
                        console.log("Reported: ");
                        console.log(JSON.stringify(reportedConfig, null, 2));
                    });
                }
            });
        };

    Az előző kód lekérdezések a twins a központi tárolja, és nyomtatja ki a kívánt és jelentett telemetriai beállításokat. Olvassa el a [IoT központi lekérdezési nyelv] [ lnk-query] akiket minden eszközén sokoldalú jelentések készítése című témakörből.


8. A **SimulateDeviceConfiguration.js** futtatása, futtassa az alkalmazást:

        node SetDesiredAndQuery.js 5m

    Meg kell jelennie a jelentett konfigurációs módosítást a **munka sikerét** **függőben** **sikeres** ismét az új aktív küldés gyakoriság öt perc 24 óra helyett.

    > [AZURE.IMPORTANT] Nincs a felfelé az eszköz jelentés művelet és a lekérdezés eredménye között egy perc késés. Ez a ahhoz, hogy a lekérdezés infrastruktúra egyidejű nagyon magas skála. Beolvashatja egy egyetlen kettős egységes nézeteinek használja a **getDeviceTwin** módszert a **beállításjegyzék** osztály.

## <a name="next-steps"></a>Következő lépések

Ebben az oktatóanyagban háttéradatbázist alkalmazásból *a kívánt tulajdonságokat* a kívánt konfiguráció beállítása és a program észleli, hogy módosítása és a több elem lépés frissítési folyamat, jelentés állapota *Tulajdonságok jelenteni* , mint a kettős hasonlóan egy szimulált eszközön alkalmazást.

Az alábbi források segítségével megtudhatja, hogy miként:

- telemetriai küldése eszközökről való az [első lépések a IoT központi] [ lnk-iothub-getstarted] oktatóanyagban
- ütemezése vagy nagy adathalmazok eszközök el műveletek [használata feladatok ütemezése és közvetítése eszköz műveletek] hajthatók végre[ lnk-schedule-jobs] oktatóprogram.
- szabályozhatja az eszközök interaktív (például bekapcsolása a felhasználó által felügyelt alkalmazásból ventilátor), és a [használat közvetlen módszerek] [ lnk-methods-tutorial] oktatóprogram.


<!-- links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-d2c]: iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-dm-overview]: iot-hub-device-management-overview.md
[lnk-twin-tutorial]: iot-hub-node-node-twin-getstarted.md
[lnk-schedule-jobs]: iot-hub-schedule-jobs.md
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/
[lnk-device-management]: iot-hub-device-management-get-started.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md
[lnk-iothub-getstarted]: iot-hub-node-node-getstarted.md
[lnk-methods-tutorial]: iot-hub-c2d-methods.md

[lnk-guid]: https://en.wikipedia.org/wiki/Globally_unique_identifier

[lnk-how-to-configure-createapp]: iot-hub-node-node-twin-how-to-configure.md#create-the-simulated-device-app
