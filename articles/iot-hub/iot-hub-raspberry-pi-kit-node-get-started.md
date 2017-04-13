<properties
 pageTitle="Első lépések a Raspberry Pi 3 |} Microsoft Azure"
 description="Málna Pi 3 – első lépések, létrehozása az Azure IoT központi és csatlakozás a Pi IoT-hubon keresztül csatlakozott."
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

# <a name="get-started-with-raspberry-pi-3"></a>A Pi Raspberry 3 – első lépések

Ebben az oktatóanyagban, kezdje azzal, hogy futó Raspbian málna Pi 3 használata alapjait tanulási. Majd megtudhatja, hogy miként csatlakozhat zökkenőmentes eszközén az [Azure IoT központi](iot-hub-what-is-iot-hub.md)a felhőben. Windows 10 IoT Core mintáknál látogasson el a [windowsondevices.com](http://www.windowsondevices.com/).

## <a name="lesson-1-configure-your-device"></a>1 a lecke: Az eszköz beállítása

![Lecketervező 1 E2E Diagram](media/iot-hub-raspberry-pi-lessons/e2e-lesson1.png)

Ez a lecke málna Pi 3 eszköze állítson be egy operációs rendszer, a fejlesztői környezet beállítása, és a Pi alkalmazás telepítéséhez.

### <a name="configure-your-device"></a>Az eszköz beállítása

Állítsa be a málna Pi 3 első használatra, és telepítse a Raspbian-OS, ingyenes operációs rendszerű málna Pi hardver optimalizált.

*Becsült időt vesz igénybe: 30 percben* 

[Nyissa meg a "Állítsa be az eszközt"](iot-hub-raspberry-pi-kit-node-lesson1-configure-your-device.md)

### <a name="get-the-tools"></a>Eszközökhöz
Töltse le az eszközök és a szoftver létre és helyezhetnek üzembe a málna Pi 3 az első alkalmazást.

*Becsült időt vesz igénybe: 20 perc* 

[Nyissa meg a "Get az eszközök"](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-win32.md)

### <a name="create-and-deploy-the-blink-application"></a>Hozzon létre, és az pislogás alkalmazás telepítéséhez

A minta Node.js alkalmazás Github klónozhatja, és a 3-as Pi málna falra e alkalmazás telepítéséhez gulp. Minta alkalmazás villogó a LED két másodpercenként a fal csatlakozik.

*Becsült időt vesz igénybe: 5 percig tart* 

[Nyissa meg a "létrehozása és az pislogás alkalmazás telepítéséhez"](iot-hub-raspberry-pi-kit-node-lesson1-deploy-blink-app.md)

## <a name="lesson-2-create-your-iot-hub"></a>2 a lecke: A IoT központi létrehozása

![Lecketervező 2 E2E Diagram](media/iot-hub-raspberry-pi-lessons/e2e-lesson2.png)

Ez a lecke létrehozhat ingyenes Azure fiókja, az Azure IoT központi kiépítése és az első eszköz létrehozása az Azure IoT-központban.

Töltse ki a lecke 1, ez a lecke megkezdése előtt.

### <a name="get-the-azure-tools"></a>Azure eszközökhöz

Telepítse az Azure parancssori kezelőfelületről Azure.

*Becsült időt vesz igénybe: 10 perc* 

[Ugrás az első Azure-eszközök](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-win32.md)

### <a name="create-your-iot-hub-and-register-your-raspberry-pi-3"></a>A IoT központi létrehozása, és regisztráljon a málna Pi 3

Az erőforráscsoport létrehozása, az első Azure IoT központi kiépítése és az első eszköz hozzáadása az Azure-IoT központban Azure CLI használatával. 

*Becsült időt vesz igénybe: 10 perc* 

[Nyissa meg a IoT központi létrehozása és regisztrálhatja a málna Pi 3](iot-hub-raspberry-pi-kit-node-lesson2-prepare-azure-iot-hub.md)


## <a name="lesson-3-send-device-to-cloud-messages"></a>3 a lecke: Az eszköz a felhőbe üzenetek küldése

![Lecketervező 3 E2E Diagram](media/iot-hub-raspberry-pi-lessons/e2e-lesson3.png)

Ez a lecke üzeneteket küld a a Pi a IoT-központját. Akkor is, hogy felveszi a IoT központból bejövő üzeneteket, és beírja őket a Azure táblatárolóhoz Azure függvény alkalmazás létre.

Töltse ki a órákhoz 1 és 2, ez a lecke megkezdése előtt.

### <a name="create-an-azure-function-app-and-azure-storage-account"></a>Hozzon létre egy Azure függvény és Azure tárterület-fiók

Azure függvény alkalmazás és Azure tároló fiók létrehozása az Azure erőforrás-kezelő sablonnal.

*Becsült időt vesz igénybe: 10 perc* 

[Nyissa meg a "Hozzon létre egy Azure függvény és Azure tároló fiók"](iot-hub-raspberry-pi-kit-node-lesson3-deploy-resource-manager-template.md)

### <a name="run-sample-application-to-send-device-to-cloud-messages"></a>Eszköz a felhőbe üzenetek küldéséhez minta alkalmazásnak a futtatására

Üzembe helyezéséhez és üzeneteket küld IoT központi málna Pi 3 eszközére egy minta alkalmazásnak a futtatására.

*Becsült időt vesz igénybe: 10 perc* 

[Nyissa meg a "Eszköz a felhőbe üzenetek küldéséhez minta alkalmazásnak a futtatására"](iot-hub-raspberry-pi-kit-node-lesson3-run-azure-blink.md)

### <a name="read-messages-persisted-in-azure-storage"></a>Azure-tárolóban lévő állandó üzenetek olvasása
Szerint írták őket a Azure-tárolóban figyelje meg az eszköz a felhőbe üzeneteket.

*Becsült időt vesz igénybe: 5 percig tart* 

[Nyissa meg a "üzenetek olvasása állandó Azure-tárolóban lévő"](iot-hub-raspberry-pi-kit-node-lesson3-read-table-storage.md)


## <a name="lesson-4-send-cloud-to-device-messages"></a>4 a lecke: Cloud-eszköz az üzenetek küldése

![Lecketervező 4 E2E Diagram](media/iot-hub-raspberry-pi-lessons/e2e-lesson4.png)

Ez a lecke demos üzenetek küldése az Azure IoT központból a málna Pi 3. Az üzenetek ellenőrzés be- és kikapcsolása, amely a Pi csatlakozik a LED viselkedését. Egy minta alkalmazás készül, hogy a feladat elérése.

Töltse ki az órákhoz 1, 2 és 3, mielőtt megkezdené a lecke.

### <a name="run-the-sample-application-to-receive-cloud-to-device-messages"></a>A minta alkalmazás cloud-eszköz üzenetek fogadására futtatása

A minta lecke 4-alkalmazás a Pi fut, és figyeli a bejövő üzenetek a IoT központból. Az új feladat gulp üzeneteket küld a IoT központból szeretne a LED villogó a a Pi.

*Becsült időt vesz igénybe: 10 perc* 

[Nyissa meg a "Felhő-eszköz üzenetek fogadására a minta alkalmazásnak a futtatására"](iot-hub-raspberry-pi-kit-node-lesson4-send-cloud-to-device-messages.md)

### <a name="optional-section-change-the-on-and-off-behavior-of-the-led"></a>Választható szakasz: be- és kikapcsolása a LED viselkedésének módosítása

Testre szabhatja az üzeneteket, a LED be- és kikapcsolása viselkedés módosítása.

*Becsült időt vesz igénybe: 10 perc* 

[Nyissa meg a "választható szakasz: be- és kikapcsolása a LED viselkedésének módosítása"](iot-hub-raspberry-pi-kit-node-lesson4-change-led-behavior.md)


## <a name="troubleshooting"></a>Hibaelhárítás

Ha bármelyik során fellépő problémák során az órákhoz felel meg, kér a megoldások ezen a lapon.

[Nyissa meg a "hibaelhárítása](iot-hub-raspberry-pi-kit-node-troubleshooting.md)
