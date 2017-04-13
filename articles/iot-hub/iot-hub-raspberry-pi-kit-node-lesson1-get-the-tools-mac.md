<properties
 pageTitle="(MacOS 10.10) eszközökhöz |} Microsoft Azure"
 description="Töltse le és telepítse a szükséges eszközök és a szoftver a Pi első minta alkalmazáshoz macOS."
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

# <a name="12-get-the-tools-macos-1010"></a>Az 1.2 beszerzése a tools (macOS 10.10)

> [AZURE.SELECTOR]
- [Windows 7 +](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-win32.md)
- [Ubuntu 16.04](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-ubuntu.md)
- [macOS 10.10](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-mac.md)

## <a name="121-what-you-will-do"></a>1.2.1 mit érhetek

Töltse le a Fejlesztőeszközök és az első, a málna Pi 3 minta alkalmazást a szoftvert. Ha bármilyen problémát eleget tesz, keresés, [oldal hibaelhárítási](iot-hub-raspberry-pi-kit-node-troubleshooting.md)megoldások.

## <a name="122-what-you-will-learn"></a>1.2.2 mi ismerkedhet meg
Ebben a szakaszban megismerheti:

- Mely számjegy és Node.js telepítése
    - [Mely számjegy](https://git-scm.com) rendszer egy elosztott Megnyitás verzió vezérlő. Ez a lecke minta kérelmezése mely számjegy tárolja.
    - [Node.js](https://nodejs.org/en/) a JavaScript futtatókörnyezet és a multimédiás csomag ökológiai.
- Bemutatja, hogyan telepítheti NPM további Node.js Fejlesztőeszközök.
  - A minimálisan szükséges Node.js verziója 4,5 LTS.
  - [NPM](https://www.npmjs.com) az egyik Node.js csomag felettesét.

## <a name="123-what-you-need"></a>1.2.3 szükséges

- A Fejlesztőeszközök és a szoftver letöltése internetkapcsolat
- Mac rendszerű macOS Yosemite (10.10) vagy újabb verzió

## <a name="124-install-git-and-nodejs"></a>1.2.4 mely számjegy és Node.js telepítése

Mely számjegy és Node.js telepítéséhez segédprogrammal [Homebrew](http://brew.sh) csomag management ezeket a lépéseket követve:

1. Telepítse a Homebrew. Ha már telepítette az Homebrew, ugorjon a 2.
  1. Nyomja le a `Cmd + Space` , és adja meg `Terminal` Terminálszolgáltatások ablak megnyitásához.
  2. A következő parancsot:

    ```bash
    /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
    ```
2. Mely számjegy és Node.js telepítse a következő parancs futtatásával:

    ```bash
    brew install node git
    ```

## <a name="125-install-additional-nodejs-development-tools"></a>1.2.5 további Node.js Fejlesztőeszközök telepítése

Automatizálható a Pi minta alkalmazása a [gulp.js](http://gulpjs.com) segítségével. Az [eszköz-feltárás-cli](https://github.com/Azure/device-discovery-cli) hálózati adatok beolvasásához kapcsolatos IoT eszközén is használhatja.

Telepítse `gulp` és `device-discovery-cli` Terminálszolgáltatások ablakban a következő parancs futtatásával:

```bash
sudo npm install -g device-discovery-cli gulp
```

Ha Node.js és a következő további Fejlesztőeszközök telepít macOS problémákat tapasztal, olvassa el a [hibaelhárítási útmutatójának](iot-hub-raspberry-pi-kit-node-troubleshooting.md) gyakori problémák megoldásainak című témakört.

## <a name="126-install-visual-studio-code"></a>1.2.6 telepítse az Visual Studio kódot.

[Töltse le](https://code.visualstudio.com/docs/setup/osx) és telepítse Visual Studio kódot. Visual Studio kód egy hozza létre, de hatékony kód-Forrásszerkesztő a Windows, Linux és macOS. A szerkesztő belül az oktatóprogram szerkesztéséhez használhatja a minta kódot.

## <a name="127-summary"></a>1.2.7 összegzése

A szükséges Fejlesztőeszközök és a szoftver első minta alkalmazásához telepítette. A következő szakaszban létrehozása, telepítése, és a minta az alkalmazás futtatása a Pi.

## <a name="next-steps"></a>Következő lépések

[Az 1.3 létrehozása és az pislogás minta alkalmazás telepítése](iot-hub-raspberry-pi-kit-node-lesson1-deploy-blink-app.md)
