<properties
 pageTitle="Készíthetnek és helyezhetnek üzembe a pislogás alkalmazás |} Microsoft Azure"
 description="A minta Node.js alkalmazás Github klónozhatja, és a 3-as Pi málna falra e alkalmazás telepítéséhez gulp. Minta alkalmazás villogó a LED két másodpercenként a fal csatlakozik."
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

# <a name="13-create-and-deploy-the-blink-application"></a>Az 1.3 létrehozása és az pislogás alkalmazás telepítése

## <a name="131-what-you-will-do"></a>1.3.1 mit érhetek

A minta Node.js alkalmazás Github klónozhatja, és a gulp eszközzel az a málna Pi 3 minta-alkalmazás telepítéséhez. A minta alkalmazás villogó a LED két másodpercenként a fal csatlakozik. Ha bármilyen problémát eleget tesz, keresés, [oldal hibaelhárítási](iot-hub-raspberry-pi-kit-node-troubleshooting.md)megoldások.

## <a name="132-what-you-will-learn"></a>1.3.2 mi ismerkedhet meg

- Hogyan kell használni a `device-discover-cli` eszköz beolvasni a Pi hálózati adatait.
- Hogyan lehet telepíteni és futtatni a minta alkalmazás a Pi.
- Hogyan telepíthető, és a Pi távolról futó alkalmazások hibáinak.

## <a name="133-what-you-need"></a>1.3.3 szükséges

Meg kell sikeresen befejeződött a követés szakaszok lecke 1:

- [Az eszköz beállítása](iot-hub-raspberry-pi-kit-node-lesson1-configure-your-device.md)
- [Eszközökhöz](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-win32.md)

## <a name="134-obtain-the-ip-address-and-host-name-of-your-pi"></a>1.3.4 beszerzése a Pi IP címeket és neve

Nyisson meg egy parancssorablakot Windows vagy az macOS vagy Ubuntu Terminálszolgáltatások ablak, és futtassa az alábbi parancsot:

```bash
devdisco list --eth
```

Meg kell jelennie a kibocsátás, az alábbihoz hasonló:

![eszköz felderítési](media/iot-hub-raspberry-pi-lessons/lesson1/device_discovery.png)

Vegye figyelembe a `IP address` és `hostname` a pi matematikai. Ezek az információk ebben a szakaszban később szüksége.

> [AZURE.NOTE] Győződjön meg arról, hogy a Pi ugyanabba a hálózatba, mint a számítógép csatlakozik-e. Például a számítógép vezeték nélküli hálózathoz kapcsolódik, a Pi vezetékes hálózatot használ csatlakozva, ha nem megjelenhet devdisco kimeneti IP-címét.

## <a name="135-clone-the-sample-application"></a>1.3.5 klónozhatja minta alkalmazása

Nyissa meg a minta kódot, kövesse az alábbi lépéseket:

1. A minta tárat a Github klónozhatja a következő parancs futtatásával:

    ```bash
    git clone https://github.com/Azure-Samples/iot-hub-node-raspberrypi-getting-started.git
    ```

2. Nyissa meg a minta alkalmazás Visual Studio-kódot a következő parancs futtatásával:

    ```bash
    cd iot-hub-node-raspberrypi-getting-started
    cd Lesson1
    code .
    ```

![Repó struktúra](media/iot-hub-raspberry-pi-lessons/lesson1/vscode-blink-mac.png)

A `app.js` a fájl a `app` almappa is szabályozhatja a LED a kódot tartalmazó kulcs forrásfájl.

### <a name="136-install-application-dependencies"></a>1.3.6 függőségek alkalmazás telepítése

Telepítse a tárak és egyéb modulok van szüksége a minta alkalmazás a következő parancs futtatásával:

```bash
npm install
```

## <a name="137-configure-the-device-connection"></a>1.3.7 eszköz kapcsolat beállítása

Adja meg az eszköz kapcsolata, kövesse az alábbi lépéseket:

1. Az eszköz konfigurációs fájl létrehozása a következő parancs futtatásával:

    ```bash
    gulp init
    ```

    A beállítási fájl `config-raspberrypi.json` használatával jelentkezzen be a Pi, a felhasználó hitelesítő adatait tartalmazza. A fejlesztő felhasználói hitelesítő adatokat elkerülése érdekében a konfigurációs fájl jön létre az almappát `.iot-hub-getting-started` az otthoni mappa a számítógépen.

2. Nyissa meg az eszköz konfigurációs fájl Visual Studio-kódot a következő parancs futtatásával:

    ```bash
    # For Windows command prompt
    code %USERPROFILE%\.iot-hub-getting-started\config-raspberrypi.json

    # For macOS or Ubuntu
    code ~/.iot-hub-getting-started/config-raspberrypi.json
    ```

3. Cserélje le a helyőrző `[device hostname or IP address]` az IP-cím vagy az állomásnév, amely szakasz 1.3.4 el.

    ![Config.JSON](media/iot-hub-raspberry-pi-lessons/lesson1/vscode-config-mac.png)

Gratulálok! A Pi első minta kérelem sikeresen létrehozott.

## <a name="138-deploy-and-run-the-sample-application"></a>1.3.8 üzembe helyezéséhez, és a minta alkalmazásnak a futtatására

### <a name="1381-install-nodejs-and-npm-on-your-pi"></a>1.3.8.1 Node.js és NPM telepítése a Pi

Telepítése Node.js és NPM a Pi a következő parancs futtatásával:

```bash
gulp install-tools
```

Az első alkalommal futtatja a feladat befejezéséhez tíz percig is eltarthat.

### <a name="1382-deploy-and-run-the-sample-app"></a>1.3.8.2 telepítése és futtatása a minta alkalmazás

Üzembe helyezéséhez és a minta alkalmazás futtatásához a következő parancs futtatásával:

```bash
gulp deploy && gulp run
```

### <a name="1383-verify-the-app-works"></a>1.3.8.3 Ellenőrizze az alkalmazás működése

Ekkor megjelenik a Pi villogó két másodpercenként a LED.  Ha nem látja a LED villogó, olvassa el a [hibaelhárítási útmutatójának](iot-hub-raspberry-pi-kit-node-troubleshooting.md) gyakori problémák megoldásainak című témakört.
![LED villogó](media/iot-hub-raspberry-pi-lessons/lesson1/led_blinking.jpg)

> [AZURE.NOTE] Használat `Ctrl + C` az alkalmazásból való kilépéshez.

## <a name="139-summary"></a>1.3.9 összegzése

Már telepítve van a Pi készült szükséges eszközök, és a LED villogó egy minta alkalmazást a Pi rendszerbe. Most már áthelyezheti következő lecke létrehozása, üzembe helyezését és Azure IoT elosztót üzenetek küldése és fogadása a Pi kapcsolódó egy másik példa az alkalmazásnak a futtatására.

## <a name="next-steps"></a>Következő lépések

Most már készen lecke 2 kezdetének Kiindulás [Azure eszközökhöz](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-win32.md)
