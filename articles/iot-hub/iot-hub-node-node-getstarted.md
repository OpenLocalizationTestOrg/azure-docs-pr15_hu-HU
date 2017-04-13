<properties
    pageTitle="Az első lépések Node.js Azure IoT központi |} Microsoft Azure"
    description="Az első Node.js Azure IoT központi oktatóprogram lépések. Használjon Azure IoT központi és Node.js a Microsoft Azure IoT SDK az Internet a dolog, amit megoldás végrehajtásához."
    services="iot-hub"
    documentationCenter="nodejs"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="javascript"
     ms.topic="hero-article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/12/2016"
     ms.author="dobett"/>

# <a name="get-started-with-azure-iot-hub-for-nodejs"></a>Azure IoT központi Node.js használatának első lépései

[AZURE.INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

Ez az oktatóanyag végén három Node.js konzol alkalmazások vannak telepítve:

* **CreateDeviceIdentity.js**, amely egy eszköz identitás- és a szimulált eszköze társított biztonsági kulcs hoz létre.
* **ReadDeviceToCloudMessages.js**, amely a szimulált eszköze által küldött telemetriai jeleníti meg.
* **SimulatedDevice.js**, amely a korábban létrehozott eszköz identitású a IoT-hubhoz csatlakozik, és telemetriai üzenetet küld, minden második használata a AMQP Protocol (protokoll).

> [AZURE.NOTE] A cikk [IoT központi SDK] [ lnk-hub-sdks] a különböző SDK mindkét alkalmazás futtatásához az eszközén, és a megoldás vissza vége létrehozásához használható információt tartalmaz.

Ebben az oktatóanyagban a következőkre lesz szüksége:

+ NODE.js verzió 0.10.x vagy újabb verziójában.

+ Azure active fiók. (Nem rendelkeznek fiókkal, ha egy [ingyenes fiókra] hozhat létre[ lnk-free-trial] a mindössze néhány perc.)

[AZURE.INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

Most már hozott létre a IoT-központját. Ha a IoT központi hostname (állomásnév) és a IoT központi kapcsolati karakterlánc, amely az oktatóprogram hátralevő részében szükséges.

## <a name="create-a-device-identity"></a>Egy eszköz azonosító létrehozása

Ebben a részben hoz létre, amely a IoT-központban identitás beállításjegyzékbeli hoz létre egy eszköz identitás Node.js konzol alkalmazást. Egy eszközt IoT központi eszköz identitás beállításjegyzékbeli bejegyzés mindaddig nem tud csatlakozni. A **Eszköz identitás beállításjegyzék** szakaszban a [IoT központi Fejlesztőeszközök útmutató] [ lnk-devguide-identity] további információt. A New futtatásakor egy egyedi Eszközazonosítót és az eszköz segítségével azonosítja magát, amikor eszköz a felhőbe üzeneteket küld IoT központi kulcsot hoz létre.

1. Hozzon létre egy új üres nevű **createdeviceidentity**. A **createdeviceidentity** mappába az alábbi parancs használatával a parancssorban package.json fájl létrehozása. Fogadja el az alapértelmezett beállításokat:

    ```
    npm init
    ```

2. Parancssorba a- **createdeviceidentity** mappában futtassa az **azure-iothub** szolgáltatás SDK csomag telepítéséhez a következő parancsot:

    ```
    npm install azure-iothub --save
    ```

3. Egy szövegszerkesztővel **CreateDeviceIdentity.js** fájl létrehozása a **createdeviceidentity** mappában.

4. Adja hozzá a következő `require` utasítás **CreateDeviceIdentity.js** fájl elején:

    ```
    'use strict';
    
    var iothub = require('azure-iothub');
    ```

5. A következő kód hozzáadása a **CreateDeviceIdentity.js** fájlt, és cserélje ki a helyőrző értéket a kapcsolati karakterláncot az IoT-központban, az előző részben létrehozott az: 

    ```
    var connectionString = '{iothub connection string}';
    
    var registry = iothub.Registry.fromConnectionString(connectionString);
    ```

6. Adja hozzá a következő kódot a IoT központi eszköz identitás beállításjegyzékben létre kell hoznia egy eszköz-definíciót. Kód egy eszközt hoz létre, ha az eszközazonosítót nem létezik a beállításjegyzékben, akkor eredménye a kulcsot a meglévő eszköz:

    ```
    var device = new iothub.Device(null);
    device.deviceId = 'myFirstNodeDevice';
    registry.create(device, function(err, deviceInfo, res) {
      if (err) {
        registry.get(device.deviceId, printDeviceInfo);
      }
      if (deviceInfo) {
        printDeviceInfo(err, deviceInfo, res)
      }
    });

    function printDeviceInfo(err, deviceInfo, res) {
      if (deviceInfo) {
        console.log('Device id: ' + deviceInfo.deviceId);
        console.log('Device key: ' + deviceInfo.authentication.SymmetricKey.primaryKey);
      }
    }
    ```

7. Mentse, és zárja be a **CreateDeviceIdentity.js** fájlt.

8. A **createdeviceidentity** alkalmazásnak a futtatására, hajtsa végre a következő parancsot a parancssorablakban-createdeviceidentity mappában:

    ```
    node CreateDeviceIdentity.js 
    ```

9. Jegyezze le a **eszközazonosítót** és **eszköz billentyűt**. Szükséges ezeket az értékeket később eszközként IoT-hubhoz csatlakozik, az alkalmazások létrehozásakor.

> [AZURE.NOTE] A IoT központi identitás beállításjegyzék csak a központi biztonságos hozzáférés engedélyezése eszköz identitások tárolja. Tárolja az azonosítók eszköztől és a hitelesítő adatokat, és engedélyezett/letiltott jelző is használhatja az egyes eszköz hozzáférés letiltása használható billentyűparancsokról. Ha az alkalmazás más eszköz-specifikus metaadatokat tárolhatnak, akkor az alkalmazás-specifikus tárolót kell használni. [IoT központi Fejlesztőeszközök] útmutató[ lnk-devguide-identity] további információt.

## <a name="receive-device-to-cloud-messages"></a>Eszköz a felhőbe hibaüzenetek

Ebben a részben hoz létre, amely beolvassa az eszköz a felhőbe üzenetek IoT központból Node.js konzol alkalmazást. Egy IoT központi közzététele egy [Esemény hubok][lnk-event-hubs-overview]-kompatibilis végpont ahhoz, hogy az eszköz a felhőbe üzenetek olvasása. Ahhoz, hogy egyszerű dolgot, ebben az oktatóprogramban egy egyszerű olvasó, amely nem megfelelő magas átviteli telepítés hoz létre. A [folyamat eszköz a felhőbe üzenetek] [ lnk-process-d2c-tutorial] testre szabhatja a Méretezés eszköz a felhőbe üzenetek feldolgozása. Az [Első lépések az esemény hubok] [ lnk-eventhubs-tutorial] oktatóanyag további információt biztosít folyamat során az esemény hubok, és a IoT központi esemény központi-kompatibilis végpontok vonatkoznak.

> [AZURE.NOTE] Az esemény központi-kompatibilis végpontot, az eszköz a felhőbe üzenetek olvasása mindig a AMQP protokollt használja.

1. Hozzon létre egy új üres nevű **readdevicetocloudmessages**. A **readdevicetocloudmessages** mappába az alábbi parancs használatával a parancssorban package.json fájl létrehozása. Fogadja el az alapértelmezett beállításokat:

    ```
    npm init
    ```

2. A-parancssorban a **readdevicetocloudmessages** mappában, telepítse az **azure-esemény-hubok** csomagot a következő parancs futtatásával:

    ```
    npm install azure-event-hubs --save
    ```

3. Egy szövegszerkesztővel **ReadDeviceToCloudMessages.js** fájl létrehozása a **readdevicetocloudmessages** mappában.

4. Adja hozzá a következő `require` kimutatások **ReadDeviceToCloudMessages.js** fájl elején:

    ```
    'use strict';

    var EventHubClient = require('azure-event-hubs').Client;
    ```

5. Adja hozzá a következő változó nyilatkozat, és cserélje le a helyőrző értéket a IoT központi kapcsolati karakterláncát:

    ```
    var connectionString = '{iothub connection string}';
    ```

6. Adja hozzá a következő két függvényeket nyomat a konzolhoz:

    ```
    var printError = function (err) {
      console.log(err.message);
    };

    var printMessage = function (message) {
      console.log('Message received: ');
      console.log(JSON.stringify(message.body));
      console.log('');
    };
    ```

7. Adja hozzá a következő kódot létrehozása a **EventHubClient**, nyissa meg a kapcsolatot a IoT hubhoz és létrehozása az összes partíciót a címzettnek. Ez az alkalmazás használja a szűrő, amikor létrehoz egy telefonkagylót, hogy a címzett csak olvassa be a címzett indítása futtatása után IoT központi küldött üzeneteket. Ez a szűrő akkor lehet hasznos, tesztkörnyezetben, megtekintheti, hogy csak az aktuális készlete üzeneteket. Az munkakörnyezetben, a kód kell győződjön meg arról, hogy az üzenetek - feldolgozásával lásd: a [IoT központi eszköz a felhőbe üzenetek feldolgozási módjának] [ lnk-process-d2c-tutorial] oktatóanyag további információt:

    ```
    var client = EventHubClient.fromConnectionString(connectionString);
    client.open()
        .then(client.getPartitionIds.bind(client))
        .then(function (partitionIds) {
            return partitionIds.map(function (partitionId) {
                return client.createReceiver('$Default', partitionId, { 'startAfterTime' : Date.now()}).then(function(receiver) {
                    console.log('Created partition receiver: ' + partitionId)
                    receiver.on('errorReceived', printError);
                    receiver.on('message', printMessage);
                });
            });
        })
        .catch(printError);
    ```

8. Mentse és zárja be a **ReadDeviceToCloudMessages.js** fájlt.

## <a name="create-a-simulated-device-app"></a>Szimulált eszközön alkalmazás létrehozása

Ebben a részben hoz létre egy Node.js konzol alkalmazást, amely eszköz a felhőbe üzeneteket küld egy IoT központi eszközt.

1. Hozzon létre egy új üres nevű **simulateddevice**. A **simulateddevice** mappába az alábbi parancs használatával a parancssorban package.json fájl létrehozása. Fogadja el az alapértelmezett beállításokat:

    ```
    npm init
    ```

2. A-parancssorban a **simulateddevice** mappában, telepítse az **azure-iot-eszköz** eszköz SDK és **azure-iot-eszköz-amqp** csomagja a következő parancs futtatásával:

    ```
    npm install azure-iot-device azure-iot-device-amqp --save
    ```

3. Egy egyszerű szövegszerkesztő programban, hoz létre új **SimulatedDevice.js** fájl **simulateddevice** mappában.

4. Adja hozzá a következő `require` kimutatások **SimulatedDevice.js** fájl elején:

    ```
    'use strict';

    var clientFromConnectionString = require('azure-iot-device-amqp').clientFromConnectionString;
    var Message = require('azure-iot-device').Message;
    ```

5. **ConnectionString** változó hozzáadása, és hozzon létre egy eszköz ügyfél használatával. **{Youriothostname}** cseréje a IoT központi nevű létrehozott *egy IoT központi létrehozása* szakaszban. **{Yourdevicekey}** cserélje ki az eszköz hozza létre, ha *egy eszköz identitás létrehozása* szakaszban kulcs értékét:

    ```
    var connectionString = 'HostName={youriothostname};DeviceId=myFirstNodeDevice;SharedAccessKey={yourdevicekey}';
    
    var client = clientFromConnectionString(connectionString);
    ```

6. Adja hozzá a következő függvény az alkalmazás kimenetét megjelenítéséhez:

    ```
    function printResultFor(op) {
      return function printResult(err, res) {
        if (err) console.log(op + ' error: ' + err.toString());
        if (res) console.log(op + ' status: ' + res.constructor.name);
      };
    }
    ```

7. A visszahívási létrehozása, és egy új üzenetet küldeni a IoT központi másodpercenként a **setInterval** függvénnyel:

    ```
    var connectCallback = function (err) {
      if (err) {
        console.log('Could not connect: ' + err);
      } else {
        console.log('Client connected');

        // Create a message and send it to the IoT Hub every second
        setInterval(function(){
            var windSpeed = 10 + (Math.random() * 4);
            var data = JSON.stringify({ deviceId: 'myFirstNodeDevice', windSpeed: windSpeed });
            var message = new Message(data);
            console.log("Sending message: " + message.getData());
            client.sendEvent(message, printResultFor('send'));
        }, 1000);
      }
    };
    ```

8. Nyissa meg a kapcsolatot a IoT hubon keresztül csatlakozott, és indítsa el az üzenetek küldése:

    ```
    client.open(connectCallback);
    ```

9. Mentse és zárja be a **SimulatedDevice.js** fájlt.

> [AZURE.NOTE] Ahhoz, hogy egyszerű dolgot, ebben az oktatóanyagban nem valósítja bármely újrapróbálkozási házirend. Gyártási kódban végre kell hajtania újrapróbálkozási házirendek (például egy exponenciális visszalépési), az MSDN-cikk [Tranziens hibafa kezelése]a javasolt[lnk-transient-faults].


## <a name="run-the-applications"></a>Az alkalmazások futtatása

Most már készen áll az alkalmazások futtatásához.

1. A parancssorban az **readdevicetocloudmessages** mappában a következő parancsot a IoT központi figyelése megkezdéséhez:

    ```
    node ReadDeviceToCloudMessages.js 
    ```

    ![NODE.js IoT központi szolgáltatás ügyfélalkalmazás eszköz a felhőbe üzenetek figyelése][7]

2. A parancssorban az **simulateddevice** mappában a következő parancsot a kezdéshez telemetriai adatokat küld a IoT központi:

    ```
    node SimulatedDevice.js
    ```

    ![NODE.js IoT központi eszköz ügyfélalkalmazás eszköz a felhőbe üzenetek küldéséhez][8]

3. A **használati** csempére az [Azure portál] [ lnk-portal] a központi küldött üzenetek számát jeleníti meg:

    ![Azure portál használatát csempe megjelenítő küldött üzenetek száma IoT hubhoz][43]

## <a name="next-steps"></a>Következő lépések

Ebben az oktatóprogramban egy új IoT hubhoz konfigurálni az Azure-portálon, és hozza létre a eszköz identitás a központi identitás beállításjegyzékben. Az eszköz identitás használt ahhoz, hogy az eszköz a felhőbe üzeneteket küldeni a központi szimulált eszközön alkalmazás. Az alkalmazás, amely megjeleníti az üzenetek, a központi megkapta is létrehozott. 

Továbbra is az első lépések – IoT központi vagy más IoT esetben feltárása, olvassa el a:

- [Csatlakozás az eszközön][lnk-connect-device]
- [Első lépések a mobileszköz-kezelés][lnk-device-management]
- [Az átjáró IoT SDK – első lépések][lnk-gateway-SDK]

A IoT megoldás kiterjesztése és dolgozza eszköz a felhőbe üzeneteket a méretezés című témakörben talál a [folyamat eszköz a felhőbe üzenetek] [ lnk-process-d2c-tutorial] oktatóprogram.

<!-- Images. -->
[7]: ./media/iot-hub-node-node-getstarted/runapp1.png
[8]: ./media/iot-hub-node-node-getstarted/runapp2.png
[43]: ./media/iot-hub-csharp-csharp-getstarted/usage.png

<!-- Links -->
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-eventhubs-tutorial]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[lnk-devguide-identity]: iot-hub-devguide-identity-registry.md
[lnk-event-hubs-overview]: ../event-hubs/event-hubs-overview.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md
[lnk-process-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

[lnk-device-management]: iot-hub-device-management-get-started.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/
