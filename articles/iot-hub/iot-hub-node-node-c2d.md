<properties
    pageTitle="IoT központi üzenetek cloud-eszköz |} Microsoft Azure"
    description="Ebből az oktatóanyagból megtudhatja, hogy miként Azure IoT elosztót használ Java cloud-eszköz üzenetek küldéséhez kövesse."
    services="iot-hub"
    documentationCenter="nodejs"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="javascript"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/23/2016"
     ms.author="dobett"/>

# <a name="tutorial-how-to-send-cloud-to-device-messages-with-iot-hub-and-nodejs"></a>Oktatóprogram: Hogyan IoT központi és Node.js cloud-eszköz üzenet küldése

[AZURE.INCLUDE [iot-hub-selector-c2d](../../includes/iot-hub-selector-c2d.md)]

## <a name="introduction"></a>– Bevezetés

Azure IoT központi, amely segít teljes körű felügyelt szolgáltatás engedélyezése a megbízható és nem biztonságos kétirányú kommunikációt milliónyi IoT eszközök között, és a Befejezés vissza egy alkalmazást. Az [első lépések a IoT központi] szabhatja hozzon létre egy IoT hubhoz, rajta egy eszköz identitás kiépítése és kód eszköz a felhőbe üzeneteket küld szimulált eszközt.

Ez az oktatóanyag [első]lépések a IoT központi hoz létre. Hogyan azt mutatja, hogy:

- Végéről az alkalmazás felhő vissza üzenetküldés cloud-eszköz IoT hubon keresztül egyetlen eszközre.
- Egy eszközön cloud-eszköz üzeneteket fogadni.
- Az alkalmazás felhőből vége biztonsági, (*Visszajelzés*) kézbesítési visszaigazolás kérése egy eszközt IoT központból küldött üzenetek.

Talál további információt a felhőben-eszköz üzenetek a [IoT központi Developer Guide][IoT Hub Developer Guide - C2D].

Ez az oktatóanyag végén két Node.js konzol alkalmazás futtatásához:

* **SimulatedDevice**, az alkalmazás [IoT központi – első lépések], amelyek a IoT hubhoz csatlakozik, és felhőalapú-eszköz üzenetek fogadása létrehozott módosított verzióját.
* **SendCloudToDeviceMessage**, mely cloud-eszköz üzenetet küld a szimulált eszközön IoT hubon keresztül, és ezután kap a kézbesítési visszaigazolás.

> [AZURE.NOTE] IoT hubhoz számos eszközt platformokon és nyelvek (többek között, C, Java és Javascript) keresztül Azure IoT eszköz SDK SDK támogatása. Ebben az oktatóanyagban kód csatlakoztatni az eszközt, és általában Azure IoT hubon keresztül csatlakozott, lásd: az [Azure IoT Developer Center]részletes útmutatást.

Ebben az oktatóanyagban a következőkre lesz szüksége:

+ NODE.js verzió 0.10.x vagy újabb verziójában.

+ Azure active fiók. (Nem rendelkeznek fiókkal, ha egy [ingyenes fiókra] hozhat létre[ lnk-free-trial] a mindössze néhány perc.)

## <a name="receive-messages-on-the-simulated-device"></a>Hibaüzenetek szimulált az eszközre

Ebben a részben szimulált eszköz alkalmazása módosíthatja az a felhő-eszköz üzenetek fogadására a IoT központból [IoT központi – első lépések] létrehozott.

1. Szövegszerkesztővel, nyissa meg a SimulatedDevice.js fájlt.

2. Módosítsa a **connectCallback** függvény IoT központból küldött üzenetek kezelésére. Ebben a példában az eszköz nem dolgozott az üzenet IoT központi értesíti a **teljes** függvény mindig elindítja. A **connectCallback** függvény verzióját néz ki:

    ```
    var connectCallback = function (err) {
      if (err) {
        console.log('Could not connect: ' + err);
      } else {
        console.log('Client connected');
        client.on('message', function (msg) {
          console.log('Id: ' + msg.messageId + ' Body: ' + msg.data);
          client.complete(msg, printResultFor('completed'));
        });
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

    > [AZURE.NOTE] HTTP MQTT vagy AMQP helyett a szállítás használja, ha a **DeviceClient** példány központból IoT ritkán (legfeljebb 25 percenként) üzeneteket keres. MQTT, a AMQP és a HTTP-támogatási és IoT központi szabályozásának közötti különbségekről olvashat, lásd: a [IoT központi Fejlesztőeszközök útmutató][IoT Hub Developer Guide - C2D].

## <a name="send-a-cloud-to-device-message"></a>Üzenet küldése egy felhőalapú-eszköz

Ebben a részben hoz létre, amely cloud-eszköz üzeneteket küld a szimulált eszközön alkalmazás Node.js konzol alkalmazást. Az eszközre az eszköz az [első lépések a IoT központi] oktatóprogram során felvett Id szükséges. A IoT központi megtalálható az [Azure portálon]is kell a kapcsolati karakterlánc.

1. Hozzon létre egy üres nevű **sendcloudtodevicemessage**. A **sendcloudtodevicemessage** mappába az alábbi parancs használatával a parancssorban package.json fájl létrehozása. Fogadja el az alapértelmezett beállításokat:

    ```
    npm init
    ```

2. A-parancssorban a **sendcloudtodevicemessage** mappában, telepítse az **azure-iothub** csomagot a következő parancs futtatásával:

    ```
    npm install azure-iothub --save
    ```

3. Egy egyszerű szövegszerkesztő programban, hoz létre új **SendCloudToDeviceMessage.js** fájl **sendcloudtodevicemessage** mappában.

4. Adja hozzá a következő `require` kimutatások **SendCloudToDeviceMessage.js** fájl elején:

    ```
    'use strict';
    
    var Client = require('azure-iothub').Client;
    var Message = require('azure-iot-common').Message;
    ```

5. A következő kód hozzáadása a **SendCloudToDeviceMessage.js** fájlhoz. Helyőrző a kapcsolati karakterláncot cserélje a kapcsolati karakterláncot az IoT-központban, az [első lépések a IoT központi] oktatóanyagban hoztunk számára. A cél eszköz helyőrző cserélje ki az eszközt, az [első lépések a IoT központi] oktatóprogram során felvett eszköz azonosítója:

    ```
    var connectionString = '{iot hub connection string}';
    var targetDevice = '{device id}';

    var serviceClient = Client.fromConnectionString(connectionString);
    ```

6. Adja hozzá szeretné kinyomtatni a konzolhoz művelet eredménye a következő függvénynek:

    ```
    function printResultFor(op) {
      return function printResult(err, res) {
        if (err) console.log(op + ' error: ' + err.toString());
        if (res) console.log(op + ' status: ' + res.constructor.name);
      };
    }
    ```

7. Adja hozzá a következő függvény a konzol visszajelzés üzenetek kézbesítési nyomtatása:

    ```
    function receiveFeedback(err, receiver){
      receiver.on('message', function (msg) {
        console.log('Feedback message:')
        console.log(msg.getData().toString('utf-8'));
      });
    }
    ```

8. Adja hozzá a következő kódot üzenetet küldeni az eszközre, és kezelje a a visszajelzés üzenetet, ha az eszköz nyugtázza a felhőben-eszköz üzenet:

    ```
    serviceClient.open(function (err) {
      if (err) {
        console.error('Could not connect: ' + err.message);
      } else {
        console.log('Service client connected');
        serviceClient.getFeedbackReceiver(receiveFeedback);
        var message = new Message('Cloud to device message.');
        message.ack = 'full';
        message.messageId = "My Message ID";
        console.log('Sending message: ' + message.getData());
        serviceClient.send(targetDevice, message, printResultFor('send'));
      }
    });
    ```

7. Mentse, és zárja be a **SendCloudToDeviceMessage.js** fájlt.

## <a name="run-the-applications"></a>Az alkalmazások futtatása

Most már készen áll az alkalmazások futtatásához.

1. A parancssorba- **simulateddevice** mappában a következő parancsot telemetriai küldése IoT-hubon keresztül csatlakozott, és felhőalapú-eszköz üzenetek meghallgatása:

    ```
    node SimulatedDevice.js 
    ```

    ![Futtassa az szimulált eszközön alkalmazást][img-simulated-device]

2. A parancssorban az **sendcloudtodevicemessage** mappában futtassa a következő üzenet küldése egy felhőalapú-eszközt, és várja meg a visszaigazoló visszajelzés parancsot:

    ```
    node SendCloudToDeviceMessage.js 
    ```

    ![Futtassa az alkalmazást a c2d parancs küldése][img-send-command]

    > [AZURE.NOTE] Ebben az oktatóprogramban az egyszerűség 's rizspálinkát, nem hajtja végre bármely újrapróbálkozási házirend. A gyártási kódot végre kell hajtania a újrapróbálkozási házirendek (például exponenciális visszalépési), az [Ideiglenes (tranziens) hiba kezelésének]MSDN-cikk javasolt.

## <a name="next-steps"></a>Következő lépések

Ebben az oktatóanyagban cloud-eszköz üzenetek küldése és fogadása hogyan megtanulta azt. 

Példák a teljes végpont megoldások, amelyek IoT központi talál további [Azure IoT csomagot].

IoT központi megoldások fejlesztésével kapcsolatos további információért lásd: a [IoT központi Fejlesztőeszközök útmutató].

<!-- Images -->
[img-simulated-device]: media/iot-hub-node-node-c2d/receivec2d.png
[img-send-command]:  media/iot-hub-node-node-c2d/sendc2d.png

<!-- Links -->

[Első lépések a IoT központi]: iot-hub-node-node-getstarted.md
[IoT Hub Developer Guide - C2D]: iot-hub-devguide-messaging.md
[IoT központi Fejlesztőeszközök útmutató]: iot-hub-devguide.md
[Azure IoT Developer Center]: http://www.azure.com/develop/iot
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md
[Ideiglenes (tranziens) hibafa kezelése]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[Azure portál]: https://portal.azure.com
[Azure IoT programcsomagban]: https://azure.microsoft.com/documentation/suites/iot-suite/