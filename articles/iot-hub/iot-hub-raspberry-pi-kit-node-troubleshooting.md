<properties
 pageTitle="Elhárítása |} Microsoft Azure"
 description="Hibaelhárítás az málna Pi Node.js használat lap"
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

# <a name="troubleshooting"></a>Hibaelhárítás

## <a name="hardware-issues"></a>Hardveres kapcsolatos problémák

### <a name="the-application-runs-well-but-the-led-is-not-blinking"></a>Az alkalmazás jól fut, de a LED nem villogó.

Ez a probléma mindig kapcsolódó hardver áramkör kapcsolatot. Kövesse az alábbi lépéseket a probléma azonosításához.

1. Ellenőrizze, hogy ha a táblán válassza ki a megfelelő **GPIO** . Ez a lecke a két portokat **GPIO GND (PIN-kód 6)** és **GPIO 04 (PIN-kód 7)**kell lennie.
2. Ellenőrizze, hogy a LED polaritásának helyes-e. A hosszabb láb jelezze a **pozitív**, anód PIN-kódot.
3. Használja a **3.3V PIN-kód** és a **GND PIN-kódot** a Raspberry Pi 3. A Pi tekinti a Adatközpont Power. Annak ellenőrzése, ha a LED jól működik-e.

![LED specifikáció](media/iot-hub-raspberry-pi-lessons/troubleshooting/led_spec.png)

### <a name="other-hardware-issues"></a>Más hardver kapcsolatos problémák

Málna Pi 3-as a gyakori problémák megoldásával kapcsolatos tudnivalókért olvassa el a [hivatalos hibaelhárítási lap](http://elinux.org/R-Pi_Troubleshooting).

## <a name="nodejs-package-issues"></a>NODE.js csomag kapcsolatos problémák

### <a name="no-response-during-gulp-tasks"></a>Válasz gulp feladatok során

Ha problémába ütközik futó gulp feladatok, felveheti a `--verbose` hibakereséshez lehetőséget. Próbálja ki az aktuális gulp tevékenységek befejezéséhez `Ctrl + C` hibakeresési üzenetek majd futtassa a következő parancsot a konzol ablakban látható. Előfordulhat, hogy részletes hibaüzenetek jelennek meg a konzol kimeneti kinyomtatni. 

```bash
gulp --verbose
```

### <a name="device-discovery-issues"></a>Eszköz feltárás kapcsolatos problémák

A gyakori problémák a `devdisco` parancsot, jelölje be a [fontos](https://github.com/Azure/device-discovery-cli/blob/develop/readme.md).

### <a name="other-npm-issues"></a>Más NPM kapcsolatos problémák

Próbálja meg a NPM csomag frissítse a következő parancsot:

```bash
npm install -g npm
```

Ha a probléma továbbra is fennáll, hagyja a megjegyzéseit, ez a cikk végén, vagy hozzon létre egy Github probléma a [Minta tárházba](https://github.com/Azure-Samples/iot-hub-node-raspberrypi-getting-started)

## <a name="remote-debugging"></a>Távoli hibakeresés

### <a name="run-the-sample-application-in-debug-mode"></a>A minta alkalmazás futtatásához hibakeresési módban

```bash
gulp run --debug
```

Amikor készen áll a hibakeresési motor, meg kell jelennie ```Debugger listening on port 5858``` konzol kimeneti.

### <a name="configure-vs-code-to-connect-to-the-remote-device"></a>A távoli eszköz való kapcsolódáshoz és kód beállítása

Nyissa meg a bal oldalon a **hibakeresési** panelen.

Kattintson a zöld **Indítása hibakeresési** (F5) gombra. VIEWBEN kód megnyílik a **launch.json** fájlok frissítenie kell.

Frissítse a **launch.json** fájlt a következő tartalmat. Csere `[device hostname or IP address]` a tényleges eszköz IP-címmel vagy a hostname (állomásnév).   

```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Attach",
            "type": "node",
            "request": "attach",
            "port": 5858,
            "address": "[device hostname or IP address]",
            "restart": false,
            "sourceMaps": false,
            "outDir": null,
            "localRoot": "${workspaceRoot}",
            "remoteRoot": null
        }
    ]
}
```

![Távoli hibakeresési beállítása](media/iot-hub-raspberry-pi-lessons/troubleshooting/remote_debugging_configuration.png)

### <a name="attach-to-the-remote-application"></a>A távoli alkalmazás csatolása

Kattintson a hibakeresés zöld **Indítása hibakeresési** (F5) gombra. 

Olvassa el a [JavaScript-kód VIEWBEN](https://code.visualstudio.com/docs/languages/javascript#_debugging) , ha többet szeretne tudni a hibakeresőt.

![Távoli interaktív hibakeresése](media/iot-hub-raspberry-pi-lessons/troubleshooting/remote_debugging_interactive.png)

## <a name="azure-cli-issues"></a>Azure CLI kapcsolatos problémák

Azure CLI előzetes építés. [Előzetes verzió telepítése útmutató](https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md) keresési megoldások is hivatkozhat.

Ha az eszközzel hibáit, a [probléma](https://github.com/Azure/azure-cli/issues) fájl a GitHub repó **problémák** szakaszában.

Gyakori problémák hibaelhárítási segítséget jelölje be a [fontos](https://github.com/Azure/azure-cli/blob/master/README.rst).

## <a name="python-installation-issues"></a>Python telepítési problémákra

### <a name="legacy-installation-issues-macos"></a>Régebbi telepítési problémák (macOS)

**Mezőpont**telepítésekor engedélyekkel hibába **SZU** engedélyekkel rendelkező telepített régebbi csomagok esetén elő. Ez a helyzet az oka, hogy az brew (macOS) használatával Python korábbi telepítése nem távolítja el teljesen. Néhány **mezőpont** csomagok egy korábbi telepítése a jogosultsági hibát okoz a legfelső szintű hozta létre. A megoldás, ha telepítve van a legfelső szintű azokat a csomagokat eltávolítása. A feladat végrehajtásához kövesse az alábbi lépéseket:

1. Nyissa meg a /usr/local/lib/python2.7/site-packages
2. Legfelső szintű csomagok hozhat létre:`ls -l | grep root`
3. Távolítsa el a csomagok a 2.lépésben:`sudo rm -rf {package name}`
4. Telepítse újra az Python.

## <a name="azure-iot-hub-issues"></a>Azure IoT központi kapcsolatos problémák

Ha sikeresen, már kiépítve az Azure IoT központi `azure-cli`, és kezelheti a a IoT-hubhoz csatlakozik, próbálkozzon a következő eszközök eszköz van szüksége:

### <a name="device-explorer"></a>Eszköz Explorer

[Eszköz Explorer](https://github.com/Azure/azure-iot-sdks/blob/master/tools/DeviceExplorer/doc/how_to_use_device_explorer.md) futtatja a Windows helyi számítógépen, és az Azure-ban IoT-hubhoz csatlakozik. A következő [IoT központi végpontok](iot-hub-devguide.md)kommunikál:

- *Eszköz Identitáskezelés* kiépítése és eszközkezelés regisztrált a IoT-központját.
- *Fogadási eszköz felhő* ahhoz, hogy a Lync-az eszközről a IoT központi küldött üzeneteket.
- A *felhő-eszköz küldése* lehetővé teszi a IoT központi eszközén küldésére.

Állítsa be a `IoT hub connection string` belül ez az eszköz összes funkcióját használni.

### <a name="iot-hub-explorer"></a>IoT központi Explorer

[IoT központi Explorer](https://github.com/Azure/azure-iot-sdks/blob/master/tools/iothub-explorer/readme.md) egy olyan minta többplatformos CLI eszköz az eszköz ügyfél felügyeletét látja el. Az eszköz lehetővé teszi a identitás beállításjegyzékben eszközök kezelése, eszköz a felhőbe üzenetek figyelésére és elküldése cloud-eszköz parancsai.

A iothub-explorer eszköz (az előzetes verziójú) legújabb verziójának telepítéséhez futtassa a következő parancsot a parancssori környezetben:

```
npm install -g iothub-explorer@latest
```

A következő paranccsal minden iothub-Intéző parancs és a paraméterek kapcsolatban további segítséget:

```bash
iothub-explorer help
```

### <a name="use-azure-portal-to-manage-your-resources"></a>Azure portal segítségével az erőforrások kezelése

A következő órákhoz egy teljes CLI élmény hozhat létre és kezelhet az Azure erőforrások kapja. Célszerű az [Azure portal](../azure-portal-overview.md) segítségével súgó rendelkezni, kezelése és az Azure erőforrások hibakeresési is.

## <a name="azure-storage-issues"></a>Azure tároló kapcsolatos problémák

[Microsoft Azure tároló Explorer (előzetes verzió)](http://storageexplorer.com) egy különálló alkalmazás, a Microsofttól, amely lehetővé teszi, hogy Windows macOS és Linux Azure tároló adatainak feldolgozása. Ez az eszköz lehetővé teszi a táblázat csatlakozzon, és azt az adatokat. Ez az eszköz az Azure tároló problémák megoldásához használható.
