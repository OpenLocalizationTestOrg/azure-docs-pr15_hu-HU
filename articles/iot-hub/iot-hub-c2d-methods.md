<properties
 pageTitle="Közvetlen módszerekkel |} Microsoft Azure"
 description="Ebből az oktatóanyagból megtudhatja, hogy miként közvetlen módszerekkel"
 services="iot-hub"
 documentationCenter=""
 authors="nberdy"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="10/05/2016"
 ms.author="nberdy"/>

# <a name="tutorial-use-direct-methods"></a>Oktatóprogram: Közvetlen módszerekkel

## <a name="introduction"></a>– Bevezetés

Azure IoT központi egy teljes körű felügyelt szolgáltatás, amely lehetővé teszi a megbízható és biztonságos kétirányú kommunikációt, több millió IoT eszközök és az alkalmazások között biztonsági célból. Előző oktatóanyagok ([IoT központi – első lépések] és [IoT központi Cloud-eszköz üzenet küldése]) alapvető eszköz a felhőbe, és a felhő-eszköz üzenetben funkcióit IoT központi szemléltetése IoT központi is biztosít az azt jelenti, hogy nem tartós módszerek az eszközökön a felhőből meghívásához. Módszerek a kérés / válasz kapcsolati hasonlóan, mint a HTTP-hívás eszközön jelenítik meg abban a sikeresek, vagy azonnal (a felhasználó által megadott időtúllépése) után, hogy a felhasználónak, hogy a hívás állapotának sikertelen lesz. [Közvetlen metódus eszközön] [ lnk-devguide-methods] módszerek részletesen ismerteti, és mikor érdemes használni, és a felhő-eszköz üzenetek módszerek útmutató nyújt.

Ebből az oktatóanyagból megtudhatja, hogyan szeretné:

- Az Azure portal segítségével hozzon létre egy IoT hubhoz, és hozzon létre egy eszköz identitás a IoT-központját.
- Hozzon létre, amely szerint az a felhő hívható közvetlen metódus szimulált eszközt.
- Közvetlen módszer felhívja a IoT központi keresztül szimulált eszközön console-alkalmazás létrehozása.

> [AZURE.NOTE] Ebben az esetben a közvetlen módszerek elérhetők csak a MQTT protokollal IoT központi csatlakozó eszközök. Olvassa el a [támogatási MQTT] [ lnk-devguide-mqtt] miként meglévő eszköz alkalmazás konverziójának MQTT használni.

Ez az oktatóanyag végén alkalmazások vannak telepítve a két Node.js konzol:

* **CallMethodOnDevice.js**, amely felhívja a módszer szimulált az eszközre, és megjeleníti a kérdésre adott választ.
* **SimulatedDevice.js**, amely a korábban létrehozott eszköz identitású a IoT-hubhoz csatlakozik, és válaszoljon a módszerrel hívja meg a felhőben.

> [AZURE.NOTE] A cikk [IoT központi SDK] [ lnk-hub-sdks] a különböző SDK az eszközén, és a megoldás vissza vége futtatandó mindkét alkalmazások készítéséhez használható információt tartalmaz.

Ebben az oktatóanyagban a következőkre lesz szüksége:

+ NODE.js verzió 0.10.x vagy újabb verziójában.

+ Azure active fiók. (Nem rendelkeznek fiókkal, ha egy [ingyenes fiókra] hozhat létre[ lnk-free-trial] a mindössze néhány perc.)

[AZURE.INCLUDE [iot-hub-get-started-create-hub-pp](../../includes/iot-hub-get-started-create-hub-pp.md)]

## <a name="create-a-simulated-device-app"></a>Szimulált eszközön alkalmazás létrehozása

Ebben a részben hoz létre a módszer hívja meg a felhőben válaszoló Node.js konzol alkalmazást.

1. Hozzon létre egy új üres nevű **simulateddevice**. A **simulateddevice** mappába az alábbi parancs használatával a parancssorban package.json fájl létrehozása. Fogadja el az alapértelmezett beállításokat:

    ```
    npm init
    ```

2. A-parancssorban a **simulateddevice** mappában, telepítse az **azure-iot-eszköz** eszköz SDK és **azure-iot-eszköz-mqtt** csomagja a következő parancs futtatásával:

    ```
    npm install azure-iot-device@dtpreview azure-iot-device-mqtt@dtpreview --save
    ```

3. Egy egyszerű szövegszerkesztő programban, hoz létre új **SimulatedDevice.js** fájl **simulateddevice** mappában.

4. Adja hozzá a következő `require` kimutatások **SimulatedDevice.js** fájl elején:

    ```
    'use strict';

    var Mqtt = require('azure-iot-device-mqtt').Mqtt;
    var DeviceClient = require('azure-iot-device').Client;
    ```

5. **ConnectionString** változó hozzáadása, és hozzon létre egy eszköz ügyfél használatával. **{Eszköz kapcsolati karakterlánc}** cserélje ki a kapcsolati karakterlánc, *egy eszköz identitás létrehozása* szakaszban létrehozott:

    ```
    var connectionString = '{device connection string}';
    var client = DeviceClient.fromConnectionString(connectionString, Mqtt);
    ```

6. Adja hozzá a következő függvény a módszerrel végrehajtásához az eszközön:

    ```
    function onWriteLine(request, response) {
        console.log(request.payload);

        response.send(200, 'Input was written to log.', function(err) {
            if(err) {
                console.error('An error ocurred when sending a method response:\n' + err.toString());
            } else {
                console.log('Response to method \'' + request.methodName + '\' sent successfully.' );
            }
        });
    }
    ```

7. Nyissa meg a kapcsolatot a IoT központi és kezdő inicializálni a módszer figyelő:

    ```
    client.open(function(err) {
        if (err) {
            console.error('could not open IotHub client');
        }  else {
            console.log('client opened');
            client.onDeviceMethod('writeLine', onWriteLine);
        }
    });
    ```

8. Mentse és zárja be a **SimulatedDevice.js** fájlt.

> [AZURE.NOTE] Ahhoz, hogy egyszerű dolgot, ebben az oktatóanyagban nem valósítja bármely újrapróbálkozási házirend. Gyártási kódban végre kell hajtania újrapróbálkozási házirendek (például a kapcsolat újrapróbálkozási), az MSDN-cikk [Tranziens hibafa kezelése]a javasolt[lnk-transient-faults].

## <a name="call-a-method-on-a-device"></a>Hívja fel a módszer eszközön

Ebben a részben hoz létre, amely felhívja a módszer szimulált az eszközre, és akkor jeleníti meg a választ Node.js konzol alkalmazást.

1. Hozzon létre egy új üres nevű **callmethodondevice**. A **callmethodondevice** mappában az alábbi parancs használatával a parancssorban package.json fájl létrehozása. Fogadja el az alapértelmezett beállításokat:

    ```
    npm init
    ```

2. A-parancssorban a **callmethodondevice** mappában, telepítse az **azure-iothub** csomagot a következő parancs futtatásával:

    ```
    npm install azure-iothub@dtpreview --save
    ```

3. Egy szövegszerkesztővel **CallMethodOnDevice.js** fájl létrehozása a **callmethodondevice** mappában.

4. Adja hozzá a következő `require` **CallMethodOnDevice.js** fájl kezdetekor kimutatások:

    ```
    'use strict';

    var Client = require('azure-iothub').Client;
    ```

5. Adja hozzá a következő változó nyilatkozat, és cserélje le a helyőrző értéket a IoT központi kapcsolati karakterláncát:

    ```
    var connectionString = '{iothub connection string}';
    var methodName = 'writeLine';
    var deviceId = 'myDeviceId';
    ```

6. Az ügyfél kattintva nyissa meg a IoT hubon keresztül csatlakozott a kapcsolat létrehozása.

    ```
    var client = Client.fromConnectionString(connectionString);
    ```
    
7. Adja hozzá a következő függvény az eszköz módszer meghívása, és kinyomtathatja az eszköz válasza a konzolhoz:

    ```
    var methodParams = {
        methodName: methodName,
        payload: 'a line to be written',
        timeoutInSeconds: 30
    };

    client.invokeDeviceMethod(deviceId, methodParams, function (err, result) {
        if (err) {
            console.error('Failed to invoke method \'' + methodName + '\': ' + err.message);
        } else {
            console.log(methodName + ' on ' + deviceId + ':');
            console.log(JSON.stringify(result, null, 2));
        }
    });
    ```

7. Mentse és zárja be a **CallMethodOnDevice.js** fájlt.

## <a name="run-the-applications"></a>Az alkalmazások futtatása

Most már készen áll az alkalmazások futtatásához.

1. Parancssorba- **simulateddevice** mappában futtassa a következő parancsot a IoT központból módszer hívásokhoz figyelés indítása:

    ```
    node SimulatedDevice.js
    ```

    ![][7]
    
2. A parancssorban az **callmethodondevice** mappában a következő parancsot a IoT központi figyelése megkezdéséhez:

    ```
    node CallMethodOnDevice.js 
    ```

    ![][8]
    
3. Az eszköz kinyomtatásával meg az üzenetet, és az alkalmazás, a válasz módszer megjelenített néven az eszközről a módszerrel reagálásra jelennek meg:

    ![][9]
    
## <a name="next-steps"></a>Következő lépések

Ebben az oktatóprogramban egy új IoT hubhoz konfigurálni az Azure-portálon, és hozza létre a eszköz identitás a központi identitás beállításjegyzékben. Az eszköz identitás ahhoz, hogy a felhőben által indított módszerek reagálni a szimulált eszközön alkalmazást használta. Az alkalmazás, amely elindítja a módszerek az eszközre, és megjeleníti az eszközről a válasz is létrehozott. 

Továbbra is az első lépések – IoT központi vagy más IoT esetben feltárása, olvassa el a:

- [Első lépések a IoT központi]
- [A feladatok ütemezése több eszközön][lnk-devguide-jobs]

Megtudhatja, hogy miként, ha ki szeretné terjeszteni a IoT megoldás és az ütemezés módja a hívások több eszközön, olvassa el a [ütemezése és közvetítési feladatok] [ lnk-tutorial-jobs] oktatóprogram.

<!-- Images. -->
[7]: ./media/iot-hub-c2d-methods/run-simulated-device.png
[8]: ./media/iot-hub-c2d-methods/run-callmethodondevice.png
[9]: ./media/iot-hub-c2d-methods/methods-output.png

<!-- Links -->
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-tutorial-jobs]: iot-hub-schedule-jobs.md
[lnk-devguide-methods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md

[IoT központi Cloud-eszköz üzenet küldése]: iot-hub-csharp-csharp-c2d.md
[Process Device-to-Cloud messages]: iot-hub-csharp-csharp-process-d2c.md
[Első lépések a IoT központi]: iot-hub-node-node-getstarted.md
