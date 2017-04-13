<properties
 pageTitle="Hozzon létre egy Azure függvény és Azure tároló fiók |} Microsoft Azure"
 description="Az Azure függvény alkalmazás Azure IoT központi események figyeli, a bejövő üzenetek feldolgozásával és Azure táblatárolóhoz beírja őket."
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

# <a name="31-create-an-azure-function-app-and-azure-storage-account"></a>3.1 egy Azure függvény és Azure tárterület-fiók létrehozása

[Azure függvények](../../articles/azure-functions/functions-overview.md) a kis hardvereszközöket kódot, úgynevezett "funkciók", a felhőben egyszerűen futó megoldást. Az alkalmazás Azure függvény végrehajtása során az Azure-ban függvények tárolja.

## <a name="311-what-will-you-do"></a>3.1.1 fog mire

Azure függvény alkalmazás és Azure tároló fiók létrehozása az Azure erőforrás-kezelő sablonnal. Az Azure függvény alkalmazás Azure IoT központi események figyeli, a bejövő üzenetek feldolgozásával és Azure táblatárolóhoz beírja őket. Ha bármilyen problémát eleget tesz, keresés, [oldal hibaelhárítási](iot-hub-raspberry-pi-kit-node-troubleshooting.md)megoldások.

## <a name="312-what-will-you-learn"></a>3.1.2 mi ismerkedhet meg

- Hogyan lehet Azure erőforrások [Azure erőforrás-kezelő](../../articles/azure-resource-manager/resource-group-overview.md) használatával.
- Azure függvény alkalmazás használatához IoT központi feldolgozása üzeneteket, és írja be őket a táblázatokat a Azure táblatárolóhoz.

## <a name="313-what-do-you-need"></a>3.1.3 mi van szüksége

- Meg kell sikeresen befejeződött az előző órákhoz: [első lépések a málna Pi 3](iot-hub-raspberry-pi-kit-node-get-started.md) és [létrehozása az Azure IoT központi](iot-hub-raspberry-pi-kit-node-get-started.md).

## <a name="314-open-the-sample-app"></a>3.1.4, nyissa meg a minta alkalmazást

Nyissa meg a minta projekt Visual Studio-kódot a következő parancs futtatásával:

```bash
cd Lesson3
code .
```

![Repó struktúra](media/iot-hub-raspberry-pi-lessons/lesson3/repo_structure.png)

- A `app.js` a fájl a `app` almappa a fő forrásfájlban is. A forrásfájl 20 alkalommal üzenet küldése a IoT hubon keresztül csatlakozott, és az egyes üzenetet küld, a LED villogó kódot tartalmazza.
- A `arm-template.json` fájl, az erőforrás-kezelő Azure-sablon, amely tartalmazza az Azure függvény alkalmazás és Azure tároló fiók.
- A `arm-template-param.json` a konfigurációs fájl az erőforrás-kezelő Azure-sablon segítségével.
- A `ReceiveDeviceMessages` almappa az Azure függvény Node.js kódot tartalmazza.

## <a name="315-configure-azure-resource-manager-templates-and-create-resources-in-azure"></a>3.1.5 konfigurálása Azure erőforrás-kezelő sablonok és erőforrások létrehozása az Azure-ban

Frissítés a `arm-template-param.json` fájl Visual Studio kódot.

![Azure erőforrás-kezelő sablon paraméterei](media/iot-hub-raspberry-pi-lessons/lesson3/arm_para.png)

- Cserélje le a **[IoT központi neve]** [lecke 2](iot-hub-raspberry-pi-kit-node-lesson2-prepare-azure-iot-hub.md)megadott **{a központi neve}** .
- Cserélje ki bármelyik előtagot, amelyet **[előtag karakterlánc új erőforrások]** . Az előtag biztosítja, hogy az erőforrás neve globálisan egyedi ütközések elkerülése érdekében. Ne használjon kötőjel vagy kezdeti az előtag a számot.

> [AZURE.NOTE] Fölösleges `azure_storage_connection_string` ebben a szakaszban. Megtartása a jelenlegi állapotában.

Miután frissítette a `arm-template-param.json` fájlt, az erőforrások Azure telepíteni a következő parancs futtatásával:

```bash
az resource group deployment create --template-file-path arm-template.json --parameters-file-path arm-template-param.json -g iot-sample -n mydeployment
```

Ezek az erőforrások létrehozása öt perc szükséges időt. Az erőforrás létrehozási folyamat során, az alábbi szakasszal áthelyezhetők.

## <a name="316-summary"></a>3.1.6 összegzése

Létrehozta az Azure függvény alkalmazás feldolgozása IoT központi üzenetek és az Azure tárterület-fiókkal az alábbi üzenetek tárolásához. Áthelyezhetők üzembe helyezéséhez és a minta futtassa az alábbi szakasszal eszköz a felhőbe üzenetek küldéséhez kattintson a Pi.

## <a name="next-steps"></a>Következő lépések

[A 3,2 eszköz a felhőbe üzenetek küldéséhez kattintson a málna Pi 3 minta alkalmazás futtatása](iot-hub-raspberry-pi-kit-node-lesson3-run-azure-blink.md)

