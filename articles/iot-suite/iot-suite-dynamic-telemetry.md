<properties
    pageTitle="Dinamikus telemetriai használata |} Microsoft Azure"
    description="Hajtsa végre az ebből az oktatóanyagból megtudhatja, hogy miként dinamikus telemetriai használata a távoli felügyeleti előre beállított megoldás."
    services=""
    suite="iot-suite"
    documentationCenter=""
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-suite"
     ms.devlang="na"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="08/25/2016"
     ms.author="dobett"/>

# <a name="use-dynamic-telemetry-with-the-remote-monitoring-preconfigured-solution"></a>A távoli felügyeleti előre beállított megoldást dinamikus telemetriai használata

## <a name="introduction"></a>– Bevezetés

Dinamikus telemetriai lehetővé teszi, hogy minden küld a távoli felügyeleti előre beállított megoldást telemetriai ábrázolása. Az előre beállított megoldást üzembe helyezéséhez szimulált eszközök küldése hőmérséklet és páratartalom telemetriai, amely akkor is megjelenítése az irányítópulton. Ha a meglévő szimulált eszközök testreszabása, hozzon létre új szimulált eszközök vagy fizikai eszközök csatlakoztatása az előre beállított megoldás más telemetriai érték, például a külső hőmérsékleti, RPM vagy szélsebesség is küldhet. Képi megjelenítés a az irányítópulton a további telemetriai majd.

Ebben az oktatóprogramban egy egyszerű olyan Node.js szimulált eszközt, amely egyszerűen módosítható kísérletezés a dinamikus telemetriai használ.

Oktatóprogram elvégzéséhez az alábbiakra lesz szüksége:

- Azure active előfizetés. Nem rendelkeznek fiókkal, ha mindössze néhány perc is létrehozhat ingyenes próba-fiók. A részletekért olvassa [Azure ingyenes próbaverziót][lnk_free_trial].
- [NODE.js] [ lnk-node] verzió 0.12.x vagy újabb verziójában.

Ebben az oktatóanyagban minden operációs rendszeren, például a Windows vagy Linux rendszerhez, honnan telepítheti Node.js hajthatja végre.

[AZURE.INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

## <a name="configure-the-nodejs-simulated-device"></a>A Node.js szimulált eszköz beállítása

1. A távoli felügyeleti irányítópulton **+ eszköz hozzáadása** gombra, és adja hozzá az egyéni eszközt. Jegyezze le a IoT központi hostname (állomásnév), eszközazonosítót és eszköz billentyűt. Szükség szerint csoportokat később a ebben az oktatóprogramban az remote_monitoring.js eszköz ügyfélalkalmazás előkészítésekor.

2. Győződjön meg róla a Node.js verzióval 0.12.x vagy újabb számítógépre telepítve van a fejlesztés. Futtatása `node --version` parancssorba vagy egy rendszerhéj verziószámának. Kapcsolatos használatáról egy csomag telepítéséhez Node.js Linux további információért olvassa el a [Via csomag manager telepítésével Node.js][node-linux].

3. Ha Node.js telepítette, klónozhatja a legújabb az [azure-iot-SDK] [ lnk-github-repo] tárházba a fejlesztés géphez. A **fő** ág mindig a tárak és a minták legújabb verzióját használja.

4. A helyi másolatát az [azure-iot-SDK] [ lnk-github-repo] tárházba, a következő két fájlok másolása a csomópontot, a eszköz és a minták mappából mappa ürítése fejlesztési számítógépre:

  - Packages.JSON
  - remote_monitoring.js

5. Nyissa meg a remote_monitoring.js fájlt, és keresse meg a következő változó meghatározása:

    ```
    var connectionString = "[IoT Hub device connection string]";
    ```

6. Az eszköz kapcsolati karakterláncot cserélje le **[IoT központi eszköz kapcsolati karakterlánc]** . Az értékek használata a IoT központi hostname (állomásnév), eszközazonosítót és végrehajtott a Megjegyzés lépés: 1 eszköz billentyűt. Kapcsolati karakterlánc eszköz formátuma a következő:

    ```
    HostName={your IoT Hub hostname};DeviceId={your device id};SharedAccessKey={your device key}
    ```

    A központi IoT hostname (állomásnév) **contoso** , és a eszközazonosítót **mydevice**, ha a kapcsolati karakterlánc formátumban, például a következőket:

    ```
    var connectionString = "HostName=contoso.azure-devices.net;DeviceId=mydevice;SharedAccessKey=2s ... =="
    ```

7. Mentse a fájlt. A következő parancsokat rendszerhéj vagy a mappában, amely tartalmazza ezeket a fájlokat a szükséges csomagok telepítése, és kattintson a minta alkalmazásnak a futtatására parancssort:

    ```
    npm install
    node remote_monitoring.js
    ```

## <a name="observe-dynamic-telemetry-in-action"></a>A művelet dinamikus telemetriai tekintse meg

Az irányítópult a hőmérsékleti és páratartalom telemetriai a meglévő szimulált eszközökről jeleníti meg:

![Az alapértelmezett irányítópult][image1]

Ha bejelöli a az előző szakaszban futtatta Node.js szimulált eszközön, hőmérsékleti, a páratartalom és a külső hőmérsékleti telemetriai láthat:

![Külső hőmérsékleti hozzáadása az irányítópulton][image2]

A távoli felügyeleti megoldást automatikusan észleli a további külső hőmérsékleti telemetriai típusa és hozzáadja őket a diagramon az irányítópulton.

## <a name="add-a-telemetry-type"></a>Telemetriai-típus felvétele

Következő lépésként az új értékeket tartalmazó Node.js szimulált eszköz által generált telemetriai cseréje:

1. Állítsa le a Node.js szimulált eszközön a parancssor vagy rendszerhéj írja be a **Ctrl + C billentyűkombinációt** .

2. A meglévő hőmérsékleti páratartalom és külső hőmérsékleti telemetriai alapadatok értékének megjelenik a remote_monitoring.js fájlban. Adjon meg **rpm** alapadatok értéket az alábbi képlettel történik:

    ```
    // Sensors data
    var temperature = 50;
    var humidity = 50;
    var externalTemperature = 55;
    var rpm = 200;
    ```

3. A Node.js szimulált eszközön a remote_monitoring.js fájlt egy véletlen növelés hozzáadása alapadatok értékeket a **generateRandomIncrement** függvényt használja. A **rpm** érték véletlenszerű hozzáadásával egy sort a meglévő randomizations után az alábbi képlettel történik:

    ```
    temperature += generateRandomIncrement();
    externalTemperature += generateRandomIncrement();
    humidity += generateRandomIncrement();
    rpm += generateRandomIncrement();
    ```

4. Az új rpm érték hozzáadása a JSON-tartalom IoT központi küld az eszközön:

    ```
    var data = JSON.stringify({
      'DeviceID': deviceId,
      'Temperature': temperature,
      'Humidity': humidity,
      'ExternalTemperature': externalTemperature,
      'RPM': rpm
    });
    ```

5. A Node.js szimulált eszközön használja a következő parancs futtatásával:

    ```
    node remote_monitoring.js
    ```

6. Tekintse át az új RPM telemetriai típusra, amely a diagramon az irányítópult jeleníti meg:

![Az irányítópult RPM hozzáadása][image3]

> [AZURE.NOTE] Előfordulhat, hogy le szeretne tiltani engedélyeznie kell az **eszközök** lapon az irányítópulton a azonnal megtekintheti a változásokat a Node.js eszközt.

## <a name="customize-the-dashboard-display"></a>Irányítópult megjelenítésének testreszabása

Az **Eszköz-információ** üzenetet is elhelyezhet a az eszköz küldhet IoT központi telemetriai metaadatait. A metaadatok adhatja meg az eszköz küld telemetriai fájltípusok. Módosítsa a remote_monitoring.js fájl mentését követően a **parancsok** definíció **Telemetriai** definícióját **deviceMetaData** értéke. A következő kódrészletet **parancsok** definíciója látható (ne felejtse el hozzáadása egy `,` a **parancsok** megadása után):

```
'Commands': [{
  'Name': 'SetTemperature',
  'Parameters': [{
    'Name': 'Temperature',
    'Type': 'double'
  }]
},
{
  'Name': 'SetHumidity',
  'Parameters': [{
    'Name': 'Humidity',
    'Type': 'double'
  }]
}],
'Telemetry': [{
  'Name': 'Temperature',
  'Type': 'double'
},
{
  'Name': 'Humidity',
  'Type': 'double'
},
{
  'Name': 'ExternalTemperature',
  'Type': 'double'
}]
```

> [AZURE.NOTE] A távoli felügyeleti megoldást nagybetűk egyező használja a metaadat-adatok használata az telemetriai adatfolyam definíció összehasonlítása.

Hozzáadása **Telemetriai** definícióját, ahogy a fenti kódrészletet nem változtatja meg az irányítópult viselkedését. A metaadatok azonban is tartalmazhatnak olyan **DisplayName** attribútum az irányítópult megjelenítésének testreszabása. Frissítse a metaadat-alapú **Telemetriai** definícióját, a következő kódtöredékének látható módon:

```
'Telemetry': [
{
  'Name': 'Temperature',
  'Type': 'double',
  'DisplayName': 'Temperature (C*)'
},
{
  'Name': 'Humidity',
  'Type': 'double',
  'DisplayName': 'Humidity (relative)'
},
{
  'Name': 'ExternalTemperature',
  'Type': 'double',
  'DisplayName': 'Outdoor Temperature (C*)'
}
]
```

Az alábbi képernyőképen látható, hogyan módosítja Ez a változás a az irányítópulton a diagramjához:

![A diagram jelmagyarázatába testreszabása][image4]

> [AZURE.NOTE] Előfordulhat, hogy le szeretne tiltani engedélyeznie kell az **eszközök** lapon az irányítópulton a azonnal megtekintheti a változásokat a Node.js eszközt.

## <a name="filter-the-telemetry-types"></a>A telemetriai típusú szűrése

Alapértelmezés szerint az irányítópulton a diagram minden adatsor az telemetriai adatfolyam jeleníti meg. A telemetriai adott típusú a diagram megjelenítését a **Eszköz-információ** metaadatok is használhatja. 

A diagram csak hőmérséklet és páratartalom telemetriai megjelenítése érdekében kihagyása **ExternalTemperature** az **Eszköz-információ** **Telemetriai** metaadattárából az alábbi képlettel történik:

```
'Telemetry': [
{
  'Name': 'Temperature',
  'Type': 'double',
  'DisplayName': 'Temperature (C*)'
},
{
  'Name': 'Humidity',
  'Type': 'double',
  'DisplayName': 'Humidity (relative)'
},
//{
//  'Name': 'ExternalTemperature',
//  'Type': 'double',
//  'DisplayName': 'Outdoor Temperature (C*)'
//}
]
```

A diagram már nem látható a **Szabadtéri hőmérsékleti** :

![A telemetriai az irányítópulton szűrése][image5]

Ez a változás csak érinti a diagram megjelenését. A **ExternalTemperature** adatértékek továbbra is tárolja, és elérhetővé tett bármely kódmentes feldolgozása.

> [AZURE.NOTE] Előfordulhat, hogy le szeretne tiltani engedélyeznie kell az **eszközök** lapon az irányítópulton a azonnal megtekintheti a változásokat a Node.js eszközt.

## <a name="handle-errors"></a>Fogópont hibák

Adatfolyam jelenjen meg a diagramot, a megfelelő **típusú** a **Eszköz-információ** metaadatok egyeznie kell telemetriai értékeket adattípusát. Például a metaadatok Megadja, hogy páratartalom adatok **típusát** az **int** és a **dupla** megtalálható a telemetriai adatfolyam majd a páratartalom telemetriai nem jelennek meg a diagramon. **Páratartalom** értékeket azonban továbbra is tárolt és elérhetővé tett bármely kódmentes feldolgozása.

## <a name="next-steps"></a>Következő lépések

Most, hogy használata a dinamikus telemetriai láthatta, többet is megtudhat az előre definiált megoldások használatára eszköz információk: [a távoli eszköz információk metaadatok figyelése előre beállított megoldás][lnk-devinfo].

[lnk-devinfo]: iot-suite-remote-monitoring-device-info.md

[image1]: media/iot-suite-dynamic-telemetry/image1.png
[image2]: media/iot-suite-dynamic-telemetry/image2.png
[image3]: media/iot-suite-dynamic-telemetry/image3.png
[image4]: media/iot-suite-dynamic-telemetry/image4.png
[image5]: media/iot-suite-dynamic-telemetry/image5.png

[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-node]: http://nodejs.org
[node-linux]: https://github.com/nodejs/node-v0.x-archive/wiki/Installing-Node.js-via-package-manager
[lnk-github-repo]: https://github.com/Azure/azure-iot-sdks
