<properties
 pageTitle="Eszköz a felhőbe üzenetek küldéséhez minta alkalmazásnak a futtatására |} Microsoft Azure"
 description="Üzembe helyezéséhez és a málna Pi 3, amely üzeneteket küld IoT-hubon keresztül csatlakozott, és a LED villogó egy minta alkalmazásnak a futtatására."
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

# <a name="32-run-sample-application-to-send-device-to-cloud-messages"></a>A 3,2 futtatása minta alkalmazás eszköz a felhőbe üzenetek küldéséhez

## <a name="321-what-you-will-do"></a>3.2.1 mit érhetek

Üzembe helyezéséhez és egy minta alkalmazás futtatásához a málna Pi 3, amely üzeneteket küld a IoT-központját. Ha bármilyen problémát eleget tesz, keresés, [oldal hibaelhárítási](iot-hub-raspberry-pi-kit-node-troubleshooting.md)megoldások.

## <a name="322-what-you-will-learn"></a>3.2.2 mi ismerkedhet meg

- Hogyan lehet telepíteni és futtatni a minta Node.js alkalmazás a Pi gulp eszközzel.

## <a name="323-what-you-need"></a>3.2.3 szükséges

- Meg kell sikeresen befejeződött az előző szakaszban a lecke: [Azure függvény alkalmazás és a folyamat és IoT központi üzenetek tárolásához Azure tároló fiók létrehozása](iot-hub-raspberry-pi-kit-node-lesson3-deploy-resource-manager-template.md).

## <a name="324-get-your-iot-hub-and-device-connection-strings"></a>3.2.4 a IoT központi és eszköz a csatlakozási_karakterlánc beszerzése

Az eszköz kapcsolati karakterlánc használják a Pi csatlakozni a IoT-központját. A IoT központi csatlakozási karakterlánc használják a IoT központi csatlakozhat az eszköz identitás, amely a Pi IoT-központban.

- A központi IoT kapcsolati karakterlánc első az Azure CLI következő parancs futtatásával:

```bash
az iot hub show-connection-string --name {my hub name} --resource-group iot-sample
```

`{my hub name}`a megadott névvel, akkor a 2 van. Használat `iot-sample` értékeként `{resource group name}` Ha lecke 2 értéke nem módosítja.

- Az eszköz kapcsolati karakterlánc első a következő parancs futtatásával:

```bash
az iot device show-connection-string --hub {my hub name} --device-id myraspberrypi --resource-group iot-sample
```

`{my hub name}`értéke megegyezik az előző paranccsal használtat tart. Használata `iot-sample` értékeként `{resource group name}` , és `myraspberrypi` értékeként `{device id}` Ha lecke 2 értéke nem módosítja.

## <a name="325-configure-the-device-connection"></a>3.2.5 eszköz kapcsolat beállítása

1. A beállítási fájl inicializálni a következő parancs futtatásával:

    ```bash
    npm install
    gulp init
    ```

2. Nyissa meg az eszköz konfigurációs fájl `config-raspberrypi.json` a Visual Studio-kódot a következő parancs futtatásával:

    ```bash
    # For Windows command prompt
    code %USERPROFILE%\.iot-hub-getting-started\config-raspberrypi.json
  
    # For macOS or Ubuntu
    code ~/.iot-hub-getting-started/config-raspberrypi.json
    ```

    ![Config.JSON](media/iot-hub-raspberry-pi-lessons/lesson3/config.png)

3. A következő javaslatok a ellenőrizze a `config-raspberrypi.json` fájl:

  - **[Eszköz hostname (állomásnév) vagy IP-cím]** lecserélése az eszköz IP-cím vagy szerezte be az állomásnév `device-discovery-cli` vagy a lecke 1 konfigurált öröklődik értéket.
  - **[IoT eszköz kapcsolati karakterlánc]** cseréje a `device connection string` , kapott.
  - **[IoT központi kapcsolati karakterlánc]** cseréje a `iot hub connection string` , kapott.

Frissíti a `config-raspberrypi.json` , hogy a számítógépről a minta alkalmazást telepítheti a fájlt.

## <a name="326-deploy-and-run-the-sample-application"></a>3.2.6 üzembe helyezéséhez, és a minta alkalmazásnak a futtatására

Üzembe helyezéséhez és a Pi a minta az alkalmazás futtatása a következő parancs futtatásával:

```bash
gulp
```

> [AZURE.NOTE] Az alapértelmezett gulp feladat fut `install-tools`, `deploy`, és `run` feladatok egymás után. [Lecketervező 1](iot-hub-raspberry-pi-kit-node-lesson1-deploy-blink-app.md), a futtatta az alábbi műveleteket egymás után külön-külön.

## <a name="327-verify-the-sample-application-works"></a>3.2.7 ellenőrizze a minta alkalmazás működése

Meg kell jelennie a LED, amely a Pi két másodpercenként villogó csatlakozik. Minden alkalommal, amikor a LED villogó, a minta alkalmazás üzenetet küld a IoT-központját, és ellenőrzi, hogy az üzenet sikeresen küldte a IoT hubon keresztül csatlakozott. Ezenkívül minden egyes az üzenetet megkapta az IoT-központban, az üzenet nyomtatása a konzol ablakban. A minta alkalmazás 20 üzenetek elküldése után automatikusan véget ér.

![](media/iot-hub-raspberry-pi-lessons/lesson3/gulp_run.png)

## <a name="328-summary"></a>3.2.8 összegzése

Hogy rendszerbe, és az új pislogás minta alkalmazás futtatását a Pi eszköz a felhőbe üzeneteket küldeni a IoT-központját. Áthelyezheti a következő szakaszban az üzenetek Lync-, az Azure tároló számlára írták őket.

## <a name="next-steps"></a>Következő lépések

[3.3 Azure-tárolóban lévő állandó olvas üzenetek](iot-hub-raspberry-pi-kit-node-lesson3-read-table-storage.md)