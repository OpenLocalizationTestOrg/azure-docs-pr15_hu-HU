<properties
 pageTitle="Futtassa az minta alkalmazást a felhőalapú-eszköz üzenetek fogadására |} Microsoft Azure"
 description="A minta lecke 4-alkalmazás a Pi fut, és figyeli a bejövő üzenetek a IoT központból. Az új feladat gulp üzeneteket küld a IoT központból szeretne a LED villogó a a Pi."
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

# <a name="41-run-the-sample-application-to-receive-cloud-to-device-messages"></a>4.1-es a minta az alkalmazás cloud-eszköz üzenetek fogadására futtatása

Ebben a szakaszban a málna Pi 3 egy minta alkalmazás üzembe azt. A minta alkalmazás figyelése a bejövő üzenetek a IoT központból. Akkor is gulp feladat futtatása a Pi a IoT központból üzeneteket küldeni a számítógépen. Az üzenetek fogadásakor a minta alkalmazás villogó a LED. Ha bármilyen problémát eleget tesz, keresés, [oldal hibaelhárítási](iot-hub-raspberry-pi-kit-node-troubleshooting.md)megoldások.

## <a name="411-what-you-will-do"></a>4.1.1 mit érhetek

- Csatlakozás a minta alkalmazás a IoT-központját.
- Üzembe helyezéséhez és a minta alkalmazásnak a futtatására.
- Üzenetek küldése a IoT központból a pi értékét a LED villogó.

## <a name="412-what-you-will-learn"></a>4.1.2 mi ismerkedhet meg

- Hogyan lehet figyelni a bejövő üzenetek a IoT központból.
- Hogyan lehet a IoT központból cloud-eszköz üzeneteket küldeni a Pi. 

## <a name="413-what-do-you-need"></a>4.1.3 mi van szüksége

- Málna Pi 3 használatára van beállítva. Megtudhatja, hogy miként állíthatja be a Pi, lásd: [lecke 1: első lépések a málna Pi 3 eszközzel](iot-hub-raspberry-pi-kit-node-get-started.md)
- Egy IoT hubhoz, amely az Azure előfizetés jön létre. Megtudhatja, hogy miként hozhat létre az Azure IoT központi, lásd: [lecke 2: létrehozása az Azure IoT központi](iot-hub-raspberry-pi-kit-node-get-started.md)

## <a name="414-connect-the-sample-application-to-your-iot-hub"></a>4.1.4 csatlakoztatása a IoT központi minta alkalmazása

1. Győződjön meg róla, hogy a repó mappában `iot-hub-node-raspberrypi-getting-started`. Nyissa meg a minta alkalmazás Visual Studio-kódot a következő parancs futtatásával:

    ```bash
    cd Lesson4
    code .
    ```

    Értesítés a `app.js` a fájl a `app` almappát. A `app.js` a fő forrás fájl, amely tartalmazza a Lync-IoT központból bejövő üzenetek a kódot. A `blinkLED` függvény a LED villogó.

    ![Repó struktúra](media/iot-hub-raspberry-pi-lessons/lesson4/repo_structure.png)

2. Az alábbi parancsok a konfigurációs fájl inicializálni:

    ```bash
    npm install
    gulp init
    ```

    Ha befejezte a lecke 3 ezen a számítógépen, az összes konfiguráció öröklik, kihagyhatja 4.1.5 lépésre. Ha egy másik számítógépen elvégezte a lecke 3, ki kell cserélni a helyőrzőket, a a `config-raspberrypi.json` fájlt. A `config-raspberrypi.json` fájl megtalálható az almappa az otthoni mappáját.

    ![Beállítások](media/iot-hub-raspberry-pi-lessons/lesson4/config_raspberrypi.png)

- A Pi IP-cím vagy hostname (állomásnév), amely a parancs futtatásával el **[eszköz hostname (állomásnév) vagy IP-cím]** cseréje`devdisco list --eth`
- **[IoT eszköz kapcsolati karakterlánc]** cserélje ki az eszközt kapcsolati karakterlánc, amely a parancs futtatásával el `az iot hub show-connection-string --name {my hub name} --resource-group {resource group name}`.
- **[IoT központi kapcsolati karakterlánc]** cserélje ki a IoT központi kapcsolati karakterlánc, amely a parancs futtatásával el `az iot device show-connection-string --hub {my hub name} --device-id {device id} --resource-group {resource group name}`.

## <a name="415-deploy-and-run-the-sample-application"></a>4.1.5 üzembe helyezéséhez, és a minta alkalmazásnak a futtatására

Üzembe helyezéséhez és a Pi a minta az alkalmazás futtatása a következő parancs futtatásával:
  
```
gulp
```

A gulp parancs először futtatja a telepítés-eszközök tevékenység. Kattintson rá üzembe helyezése a Pi minta alkalmazása. Az alkalmazás végül a Pi és a állomáson külön tevékenység 20 pislogás üzeneteket küldeni a Pi a IoT központból fusson.

A minta alkalmazás elindul, amint a IoT központból üzenetek figyel kezdődik. Eközben a gulp tevékenység küld "pislogás" üzenetek számos a IoT központból a Pi. Minden egyes a kapott pislogás üzenetet a minta alkalmazás a hívások a LED villogó blinkLED függvény.

Meg kell jelennie a két másodpercenként villogó, mint a gulp tevékenység 20 üzeneteket küld a IoT központból a Pi LED. Az utolsó egyik "befejezése" üzenet, amely arra utasítja az alkalmazás futásának leállítása lesz.

![Gulp](media/iot-hub-raspberry-pi-lessons/lesson4/gulp_blink.png)

## <a name="416-summary"></a>4.1.6 összegzése

A pi értékét a LED villogó szeretne a IoT központból sikeresen küldött üzeneteket. Következő szakasz választható szakasz megtudhatja, hogyan módosíthatja a be- és kikapcsolása a LED viselkedését.

## <a name="next-steps"></a>Következő lépések

[Választható szakasz: be- és kikapcsolása a LED viselkedésének módosítása](iot-hub-raspberry-pi-kit-node-lesson4-change-led-behavior.md)