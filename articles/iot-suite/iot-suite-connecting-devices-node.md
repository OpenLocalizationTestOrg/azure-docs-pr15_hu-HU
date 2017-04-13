<properties
   pageTitle="Csatlakoztassa a telefont használ Node.js |} Microsoft Azure"
   description="Csatlakoztassa a telefont a Azure IoT csomagja előre távoli felügyeleti megoldás az alkalmazás Node.js nyelven íródott ismerteti."
   services=""
   suite="iot-suite"
   documentationCenter="na"
   authors="dominicbetts"
   manager="timlt"
   editor=""/>

<tags
   ms.service="iot-suite"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/05/2016"
   ms.author="dobett"/>


# <a name="connect-your-device-to-the-remote-monitoring-preconfigured-solution-nodejs"></a>Csatlakoztassa az eszközt a távoli felügyeleti előre beállított megoldást (Node.js)

[AZURE.INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

## <a name="build-and-run-the-nodejs-sample-solution"></a>Össze, és futtassa a node.js minta megoldás

1. A *Microsoft Azure IoT SDK* GitHub tárházba klónozhatja, és telepítse a *Microsoft Azure IoT eszköz SDK Node.js* az asztali környezetben, hajtsa végre a [Felkészülés a fejlesztői környezet] [ lnk-github-prepare] utasításokat.

2. A helyi másolatát az [azure-iot-SDK] [ lnk-github-repo] tárházba, a következő két fájlok másolása a csomópontot, a eszköz és a minták mappából egy mappát az eszközön:

  - Packages.JSON
  - remote_monitoring.js

3. Nyissa meg a remote_monitoring.js fájlt, és keresse meg a következő változó:

    ```
    var connectionString = "[IoT Hub device connection string]";
    ```

4. Az eszköz kapcsolati karakterláncot cserélje le **[IoT központi eszköz kapcsolati karakterlánc]** . Találhat meg az értékeket a IoT központi hostname (állomásnév), eszközazonosítót és eszköz billentyűt a távoli felügyeleti megoldás irányítópulton. Kapcsolati karakterlánc eszköz formátuma a következő:

    ```
    HostName={your IoT Hub hostname};DeviceId={your device id};SharedAccessKey={your device key}
    ```

    Ha a IoT központi hostname (állomásnév) **contoso** , és a eszközazonosítót **mydevice**, a kapcsolati karakterlánc néz ki:

    ```
    var connectionString = "HostName=contoso.azure-devices.net;DeviceId=mydevice;SharedAccessKey=2s ... =="
    ```

5. Mentse a fájlt. A következő parancsokat a mappában, amely tartalmazza ezeket a fájlokat a szükséges csomagok telepítése, és kattintson a minta alkalmazásnak a futtatására parancssorba:

    ```
    npm install --save
    node remote_monitoring.js
    ```

[AZURE.INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]

[lnk-github-repo]: https://github.com/azure/azure-iot-sdks
[lnk-github-prepare]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md