<properties
 pageTitle="Választható szakasz - be- és kikapcsolása a LED viselkedésének módosítása |} Microsoft Azure"
 description="Testre szabhatja az üzeneteket, a LED be- és kikapcsolása viselkedés módosítása."
 services="iot-hub"
 documentationCenter=""
 authors="shizn"
 manager="timlt"
 tags=""
 keywords=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="10/21/2016"
 ms.author="xshi"/>

# <a name="42-optional-section-change-the-on-and-off-behavior-of-the-led"></a>4.2 választható szakasz: be- és kikapcsolása a LED viselkedésének módosítása

## <a name="421-what-you-will-do"></a>4.2.1 mit érhetek

Testre szabhatja az üzeneteket, a LED be- és kikapcsolása viselkedés módosítása. Ha bármilyen problémát eleget tesz, keresés, [oldal hibaelhárítási](iot-hub-raspberry-pi-kit-node-troubleshooting.md)megoldások.

## <a name="422-what-you-will-learn"></a>4.2.2 mi ismerkedhet meg

További Node.js függvények használatával módosíthatja a a LED be- és kikapcsolása a működését.

## <a name="423-what-you-need"></a>4.2.3 szükséges

Meg kell sikeresen befejeződött [4.1-es egy minta alkalmazást a felhőalapú eszköz üzenetek fogadására a málna Pi kell futtatnia](iot-hub-raspberry-pi-kit-node-lesson4-send-cloud-to-device-messages.md).

## <a name="424-add-nodejs-functions"></a>4.2.4 Node.js funkciók hozzáadása

1. Nyissa meg a minta alkalmazás Visual Studio-kódot a következő parancs futtatásával:

    ```bash
    cd Lesson4
    code .
    ```

2. Nyissa meg a `app.js` fájlt, és a végén a következő függvények fel:

    ```javascript
    function turnOnLED() {
      wpi.digitalWrite(CONFIG_PIN, 1);
    }

    function turnOffLED() {
      wpi.digitalWrite(CONFIG_PIN, 0);
    }
    ```

    ![app.js frissítése](media/iot-hub-raspberry-pi-lessons/lesson4/updated_app_js.png)

3. Az alábbi feltételek, mielőtt az alapértelmezett egyet a kapcsoló blokk a hozzá a `receiveMessageCallback` függvény:

    ```javascript
    case 'on':
      turnOnLED();
      break;
    case 'off':
      turnOffLED();
      break;
    ```

    Most, hogy beállított a minta alkalmazás további útmutatást az üzenetekben válaszolni. A "a" utasítás aktívvá válik a LED, és a "Kikapcsolva" utasítás, azzal kikapcsolja azokat a LED.

4. Nyissa meg a gulpfile.js fájlt, és adja hozzá az új függvényt a függvény előtt `sendMessage`:

    ```javascript
    var buildCustomMessage = function (messageId) {
      if ((messageId & 1) && (messageId < MAX_MESSAGE_COUNT)) {
        return new Message(JSON.stringify({ command: 'on', messageId: messageId }));
      } else if (messageId < MAX_MESSAGE_COUNT) {
        return new Message(JSON.stringify({ command: 'off', messageId: messageId }));
      } else {
        return new Message(JSON.stringify({ command: 'stop', messageId: messageId }));
      }
    }
    ```

    ![gulpfile frissítése](media/iot-hub-raspberry-pi-lessons/lesson4/updated_gulpfile.png)

5. Az a `sendMessage` működik, a sor cseréje `var message = buildMessage(sentMessageCount);` a következő kódtöredékének látható új vonallal:

    ```javascript
    var message = buildCustomMessage(sentMessageCount);
    ```

6. Az összes a módosítások mentéséhez.

### <a name="425-deploy-and-run-the-sample-application"></a>4.2.5 üzembe helyezéséhez, és a minta alkalmazásnak a futtatására

Üzembe helyezéséhez és a Pi a minta az alkalmazás futtatása a következő parancs futtatásával:

```bash
gulp
```

Meg kell jelennie a LED két másodpercig kapcsolva, és ezután ki van kapcsolva a másik két másodperc. Az utolsó "befejezése" üzenet leállítja futtatását a minta alkalmazást.

![be- és kikapcsolása](media/iot-hub-raspberry-pi-lessons/lesson4/gulp_on_and_off.png)

Gratulálok! A testre szabott, amely a Pi a IoT központból elküldi az üzenetek sikeresen.

### <a name="427-summary"></a>4.2.7 összegzése

A választható szakasz demos az üzenetek testreszabása, úgy, hogy egy másik módszert a minta alkalmazás kezelhetik be- és kikapcsolása a LED viselkedését.

