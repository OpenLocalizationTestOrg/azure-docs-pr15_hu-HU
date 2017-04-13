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

Ez az oktatóanyag végén választania kell a .NET és Node.js konzol alkalmazást:

* **AddTagsAndQuery.sln**, a .NET-at végéről vissza, amely felveszi a címkéket, és eszköz twins lekérdezések futtatásához szólnak.
* **TwinSimulatedDevice.js**, egy Node.js app mely keltő a szűrő megőrzi a IoT hubhoz csatlakozik, a korábban létrehozott eszköz identitású eszközt, és a jelentések kapcsolat állapotát.

> [AZURE.NOTE] A cikk [IoT központi SDK] [ lnk-hub-sdks] használható eszköz és a háttér-alkalmazások létrehozásához a különböző SDK információt tartalmaz.

Oktatóprogram elvégzéséhez az alábbiakra van szükség:

+ Microsoft Visual Studio 2015.

+ NODE.js verzió 0.10.x vagy újabb verziójában.

+ Azure active fiók. (Nem rendelkeznek fiókkal, ha egy [ingyenes fiókra] hozhat létre[ lnk-free-trial] a mindössze néhány perc.)

[AZURE.INCLUDE [iot-hub-get-started-create-hub-pp](../../includes/iot-hub-get-started-create-hub-pp.md)]

## <a name="create-the-service-app"></a>A szolgáltatás-alkalmazás létrehozása

Ebben a részben hoz létre a kettős **myDeviceId**társított metaadatok hely ellátó Node.js konzol alkalmazást. Azt, majd az eszközök kijelölése a központi tárolt twins található az Amerikai Egyesült Államok, majd a mobilhálózati kapcsolatot jelentéskészítés lekérdezések.

1. A Visual Studióban hozzáadása vizuális C# klasszikus Windows asztal projekt az aktuális megoldás a **New** project-sablon segítségével. A projekt **AddTagsAndQuery**neve.

    ![Új képi C# klasszikus Windows asztal projekt][img-createapp]

2. Kattintson a jobb gombbal a **AddTagsAndQuery** projekt megoldás Explorerben, és válassza a **Nuget csomagok kezelése**.

3. **Nuget csomag kezelő** ablakban győződjön meg arról, hogy be van-e jelölve **Belefoglalás előzetes** , **microsoft.azure.devices**keresni, jelölje ki a **telepíteni** telepítése a a **Microsoft.Azure.Devices** *előzetes* verziójának csomagolása, és fogadja el a használati feltételeket. Ez az eljárás letöltések, telepíti, és hozzáadja a [Microsoft Azure IoT szolgáltatás SDK] hivatkozás[ lnk-nuget-service-sdk] Nuget csomag és függőségét.

    ![Nuget csomag kezelőjének ablaka][img-servicenuget]

4. Adja hozzá a következő `using` kimutatások **Program.cs** fájl tetején:

        using Microsoft.Azure.Devices;

5. Az alábbi mezők hozzáadása a **Program** osztály. Helyőrző értékét cserélje le a kapcsolati karakterláncot az IoT-központban, az előző részben létrehozott az.

        static RegistryManager registryManager;
        static string connectionString = "{iot hub connection string}";

6. Az alábbi módon adja hozzá a **Program** osztály:

        public static async Task AddTagsAndQuery()
        {
            var twin = await registryManager.GetTwinAsync("myDeviceId");
            var patch =
                @"{
                    tags: {
                        location: {
                            region: 'US',
                            plant: 'Redmond43'
                        }
                    }
                }";
            await registryManager.UpdateTwinAsync(twin.DeviceId, patch, twin.ETag);

            var query = registryManager.CreateQuery("SELECT * FROM devices WHERE tags.location.plant = 'Redmond43'", 100);
            var twinsInRedmond43 = await query.GetNextAsTwinAsync();
            Console.WriteLine("Devices in Redmond43: {0}", string.Join(", ", twinsInRedmond43.Select(t => t.DeviceId)));

            query = registryManager.CreateQuery("SELECT * FROM devices WHERE tags.location.plant = 'Redmond43' AND properties.reported.connectivity.type = 'cellular'", 100);
            var twinsInRedmond43UsingCellular = await query.GetNextAsTwinAsync();
            Console.WriteLine("Devices in Redmond43 using cellular network: {0}", string.Join(", ", twinsInRedmond43UsingCellular.Select(t => t.DeviceId)));
        }

    A **RegistryManager** osztály a szolgáltatásból eszköz twins vezérléséhez szükséges összes módszerek közzététele. Az előző kódot először előkészíti a **registryManager** objektumra, majd olvassa be a kettős **myDeviceId**az, és végül frissíti a címkék a kívánt helyre adatokkal.

    A frissítés után végrehajtja a két lekérdezések: az első csak a **Redmond43** növény található eszközök eszköz twins kijelöli, és a második finomítja a lekérdezésre, jelölje be a csak a mobil hálózaton keresztül is csatlakoztatott eszközökön.

    Figyelje meg, hogy az előző kódot, amikor létrehozza a **lekérdezési** objektum Itt adhatja meg a visszaadott dokumentumok maximális száma. A **lekérdezési** objektum összes eredmények beolvasásához többször az **GetNextAsTwinAsync** módszerek meghívásához használható logikai **HasMoreResults** tulajdonság tartalmazza. A módszer **GetNextAsJson** nevű, amelyek nem eszköz twins például összesítési lekérdezés eredményének eredmények érhető el.

7. Végül adja hozzá a következő sort a **fő** módszer:

        registryManager = RegistryManager.CreateFromConnectionString(connectionString);
        AddTagsAndQuery().Wait();
        Console.WriteLine("Press Enter to exit.");
        Console.ReadLine();

8. Futtassa az alkalmazást, és meg kell jelennie a lekérdezés megkérdezi, hogy az összes eszközön, olvassa el a **Redmond43** , és nincs a lekérdezést, amely az eredmények korlátozza eszközök mobil hálózatot használ az eredményeket egy eszközt.

    ![Lekérdezés eredményének ablak][img-addtagapp]

A következő szakaszban hoz létre, amely a jelentéseket a csatlakozási adatok és az előző szakaszban a lekérdezés eredménye változik eszköz alkalmazást.

## <a name="create-the-device-app"></a>Az eszköz-alkalmazás létrehozása

Ebben a részben hozzon létre egy Node.js **myDeviceId**, a hubhoz csatlakozik, és frissíti a kettős konzol alkalmazással jelentett tulajdonságok az, hogy csatlakoztatva van a mobil hálózaton keresztül információkat tartalmazza.

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

6. Most, hogy az eszköz a csatlakozási adatok jelenteni, meg kell jelennie a két lekérdezések. A lekérdezések futtassa újra a.NET **AddTagsAndQuery** app futtatásával. Ez idő **myDeviceId** jelenjen meg mindkét lekérdezés eredményét.

    ![][img-addtagapp2]

## <a name="next-steps"></a>Következő lépések
Ebben az oktatóprogramban egy új IoT hubhoz konfigurálni az Azure-portálon, és hozza létre a eszköz identitás a központi identitás beállításjegyzékben. Eszköz metaadatai összegként címkék háttéradatbázist alkalmazásból, és egy szimulált eszközön alkalmazás jelentés eszköz kapcsolódási információk az eszköz kettős beírva. Emellett megtanulta azt a keresés ezt az információt az SQL-szerű lekérdezési nyelv IoT-elosztót használ.

Az alábbi források segítségével megtudhatja, hogy miként:

- telemetriai küldése eszközökről való az [első lépések a IoT központi] [ lnk-iothub-getstarted] oktatóanyagban
- kettős meg a kívánt tulajdonságokat használata a [használni kívánt tulajdonságok eszközök konfigurálása] eszközök konfigurálása[ lnk-twin-how-to-configure] oktatóanyagban
- interaktív (például a felhasználó által felügyelt alkalmazásból egy ventilátorral bekapcsolásával) eszközökön szabályozhatja a [használata közvetlen módszerek] [ lnk-methods-tutorial] oktatóprogram.

<!-- images -->
[img-servicenuget]: media/iot-hub-csharp-node-twin-getstarted/servicesdknuget.png
[img-createapp]: media/iot-hub-csharp-node-twin-getstarted/createnetapp.png
[img-addtagapp]: media/iot-hub-csharp-node-twin-getstarted/addtagapp.png
[img-addtagapp2]: media/iot-hub-csharp-node-twin-getstarted/addtagapp2.png

<!-- links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/1.1.0-preview-004

[lnk-d2c]: iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-identity]: iot-hub-devguide-identity-registry.md

[lnk-iothub-getstarted]: iot-hub-node-node-getstarted.md
[lnk-methods-tutorial]: iot-hub-c2d-methods.md
[lnk-twin-how-to-configure]: iot-hub-csharp-node-twin-how-to-configure.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md

