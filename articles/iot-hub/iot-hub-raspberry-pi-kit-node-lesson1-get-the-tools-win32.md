<properties
 pageTitle="Eszközökhöz (Windows 7 +) |} Microsoft Azure"
 description="Töltse le és a szükséges eszközök és a Pi első minta alkalmazáshoz szoftver telepítése Windows 7 vagy újabb verziók."
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

# <a name="12-get-the-tools-windows-7-"></a>1.2-es beszerzése a tools (Windows 7 +) 

> [AZURE.SELECTOR]
- [Windows 7 +](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-win32.md)
- [Ubuntu 16.04](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-ubuntu.md)
- [macOS 10.10](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-mac.md)

## <a name="121-what-you-will-do"></a>1.2.1 mit érhetek

Töltse le a Fejlesztőeszközök és az első, a málna Pi 3 minta alkalmazást a szoftvert. Ha bármilyen problémát eleget tesz, keresés, [oldal hibaelhárítási](iot-hub-raspberry-pi-kit-node-troubleshooting.md)megoldások.

## <a name="122-what-you-will-learn"></a>1.2.2 mi ismerkedhet meg
- Mely számjegy és Node.js telepítése
  - [Mely számjegy](https://git-scm.com) rendszer egy elosztott Megnyitás verzió vezérlő. Ez a lecke minta kérelmezése mely számjegy tárolja.
  - [Node.js](https://nodejs.org/en/) a JavaScript futtatókörnyezet és a multimédiás csomag ökológiai.
- Bemutatja, hogyan telepítheti NPM további Node.js Fejlesztőeszközök.
  - A legkisebb verziószáma követelménye Node.js 4,5 LTS.
  - [NPM](https://www.npmjs.com) az egyik Node.js csomag felettesét.

## <a name="123-what-you-need"></a>1.2.3 szükséges

- A Fejlesztőeszközök és a szoftver letöltése internetkapcsolat
- Windows operációs rendszert futtató számítógépen

## <a name="124-install-git-and-nodejs"></a>1.2.4 mely számjegy és Node.js telepítése

Kattintson a az alábbi hivatkozásokra kattintva töltse le és telepítse a mely számjegy és a Windows Node.js LTS.

- [Mely számjegy letöltése a Windows rendszerhez](https://git-scm.com/download/win/)
- [Node.js LTS letöltése a Windows rendszerhez](https://nodejs.org/en/)

## <a name="125-install-additional-nodejs-development-tools"></a>1.2.5 további Node.js Fejlesztőeszközök telepítése

Automatizálható a Pi minta alkalmazása a [gulp.js](http://gulpjs.com) segítségével. Az [eszköz-feltárás-cli](https://github.com/Azure/device-discovery-cli) hálózati adatok beolvasásához kapcsolatos IoT eszközén is használhatja.

Indítsa el a egy parancssorablakot rendszergazdaként. Telepítse `gulp` és `device-discovery-cli` a következő parancs futtatásával:

```bash
npm install -g device-discovery-cli gulp
```
    
Ha problémák Node.js és a következő további Node.js Fejlesztőeszközök telepítése a számítógépen, olvassa el a [hibaelhárítási útmutató](iot-hub-raspberry-pi-kit-node-troubleshooting.md) a gyakori problémák megoldásainak című témakört.

## <a name="126-install-visual-studio-code"></a>1.2.6 telepítse az Visual Studio kódot.

[Töltse le](https://code.visualstudio.com/docs/setup/windows) és telepítse Visual Studio kódot. Visual Studio kód egy hozza létre, de hatékony kód-Forrásszerkesztő a Windows, Linux és macOS. A szerkesztő belül az oktatóprogram szerkesztéséhez használhatja a minta kódot.

## <a name="127-summary"></a>1.2.7 összegzése

A szükséges Fejlesztőeszközök és a szoftver első minta alkalmazásához telepítette. A következő szakaszban létrehozása, telepítése, és a minta az alkalmazás futtatása a Pi.

## <a name="next-steps"></a>Következő lépések

[Az 1.3 létrehozása és az pislogás minta alkalmazás telepítése](iot-hub-raspberry-pi-kit-node-lesson1-deploy-blink-app.md)
