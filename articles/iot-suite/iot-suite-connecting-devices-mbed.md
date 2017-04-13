<properties
   pageTitle="Csatlakoztassa a telefont használ C mbed |} Microsoft Azure"
   description="Csatlakoztassa a telefont a Azure IoT csomagja előre távoli felügyeleti megoldás az alkalmazás, a C mbed-eszközökön futó ismerteti."
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


# <a name="connect-your-device-to-the-remote-monitoring-preconfigured-solution-mbed"></a>Csatlakoztassa az eszközt a távoli felügyeleti előre beállított megoldást (mbed)

[AZURE.INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

## <a name="build-and-run-the-c-sample-solution"></a>Készítése és a C minta megoldást futtatása

Az alábbi lépéseket ismertetik történő csatlakozás- [Freescale FRDM-K64F mbed engedélyező] lépései[ lnk-mbed-home] eszközt, hogy a távoli felügyeleti megoldás.

### <a name="connect-the-mbed-device-to-your-network-and-desktop-machine"></a>Csatlakoztassa az mbed eszközt a hálózati és az asztali számítógépre

1. Csatlakoztassa mbed egy Ethernet-kábelt a hálózathoz. Ez a lépés nem szükséges, mert a minta alkalmazást van internetkapcsolat.

2. Lásd: az [Első lépések – mbed] [ lnk-mbed-getstarted] asztali számítógépen mbed eszköze csatlakozni.

3. Ha az asztali számítógépen a Windows rendszer fut, olvassa el a [PC -re konfigurációs] [ lnk-mbed-pcconnect] konfigurálása a soros access mbed eszközére.

### <a name="create-an-mbed-project-and-import-the-sample-code"></a>Mbed projekteket létrehozni, és importálja a példakódot

1. A webböngészőben nyissa meg a mbed.org [fejlesztői webhely](https://developer.mbed.org/). Ha még nem jelentkezett, megjelenik egy hozhat létre fiókot (Ez az ingyenes) lehetőséget. Egyéb esetben jelentkezzen be a fiók hitelesítő adatait. Ezután kattintson a lap jobb felső sarkában **fordító** . Ez a művelet megjeleníti a *munkaterület* -felület.

2. Ellenőrizze, hogy a hardver platform használ megjelenik az ablak jobb felső sarkában, vagy kattintson a ikonra, jelölje be a hardver platform sarokban.

3. A fő menüben kattintson az **Importálás** gombra. Kattintson a **kattintson ide** importálása mellett a mbed földgömb embléma URL-címe hivatkozásra.

    ![][6]

4. Az előugró ablakban írja be a hivatkozást a minta kód https://developer.mbed.org/users/AzureIoTClient/code/remote_monitoring/, majd kattintson az **Importálás**gombra.

    ![][7]

5. Megjelenik a mbed fordító ablakban, hogy a projekt importálása is importálja a tárak különféle. Néhány megadott és karbantartani az Azure IoT csoport ([azureiot_common](https://developer.mbed.org/users/AzureIoTClient/code/azureiot_common/), [iothub_client](https://developer.mbed.org/users/AzureIoTClient/code/iothub_client/), [iothub_amqp_transport](https://developer.mbed.org/users/AzureIoTClient/code/iothub_amqp_transport/), [azure_uamqp](https://developer.mbed.org/users/AzureIoTClient/code/azure_uamqp/)), míg mások külső tárak be a mbed tárak érhető el.

    ![][8]

6. Nyissa meg a remote_monitoring\remote_monitoring.c fájlt, és keresse meg a fájlt a következő kódot:

    ```
    static const char* deviceId = "[Device Id]";
    static const char* deviceKey = "[Device Key]";
    static const char* hubName = "[IoTHub Name]";
    static const char* hubSuffix = "[IoTHub Suffix, i.e. azure-devices.net]";
    ```

7. Csere [eszközazonosítót] és [eszköz billentyű], az eszköz adatok ahhoz, hogy a program a minta a IoT központban összekapcsolása. A IoT központi hostname (állomásnév) segítségével [IoTHub név] és [IoTHub utótag, azaz az azure-devices.net] helyőrző. Például ha a IoT központi hostname (állomásnév) **contoso.azure-devices.net**, **contoso** is a **hubName** és minden után a **hubSuffix**:

    ```
    static const char* deviceId = "mydevice";
    static const char* deviceKey = "mykey";
    static const char* hubName = "contoso";
    static const char* hubSuffix = "azure-devices.net";
    ```

    ![][9]

### <a name="walk-through-the-code"></a>Haladjon végig, és a kódot.

Ha érdeklik a program működésének, az alábbi Csatornakezelési néhány főbb elemeinek a minta kódot. Ha csak a kódra, ugorjon a következő [össze, és futtassa a programot](#buildandrun).

#### <a name="defining-the-model"></a>Az adatmodell meghatározása

Ez a példa használja a [szerializáló] [ lnk-serializer] tárban, amely meghatározza az üzeneteket, az eszköz IoT hubhoz küldhet és fogadhat IoT központból modell meghatározásához. Ez a példa a **Contoso** névtér **termosztát** modell, amely megadja a **hőmérsékleti** **ExternalTemperature**és **páratartalom** telemetriai adatokat metaadatokat, például az eszközazonosítót, az eszköz tulajdonságainak és a parancsok az eszköz válaszoló együtt határozza meg:

```
BEGIN_NAMESPACE(Contoso);

DECLARE_STRUCT(SystemProperties,
    ascii_char_ptr, DeviceID,
    _Bool, Enabled
);

DECLARE_STRUCT(DeviceProperties,
ascii_char_ptr, DeviceID,
_Bool, HubEnabledState
);

DECLARE_MODEL(Thermostat,

    /* Event data (temperature, external temperature and humidity) */
    WITH_DATA(int, Temperature),
    WITH_DATA(int, ExternalTemperature),
    WITH_DATA(int, Humidity),
    WITH_DATA(ascii_char_ptr, DeviceId),

    /* Device Info - This is command metadata + some extra fields */
    WITH_DATA(ascii_char_ptr, ObjectType),
    WITH_DATA(_Bool, IsSimulatedDevice),
    WITH_DATA(ascii_char_ptr, Version),
    WITH_DATA(DeviceProperties, DeviceProperties),
    WITH_DATA(ascii_char_ptr_no_quotes, Commands),

    /* Commands implemented by the device */
    WITH_ACTION(SetTemperature, int, temperature),
    WITH_ACTION(SetHumidity, int, humidity)
);

END_NAMESPACE(Contoso);
```

A modell definíció kapcsolódó definiálják az a **SetTemperature** és **SetHumidity** parancsok az eszköz válaszoló:

```
EXECUTE_COMMAND_RESULT SetTemperature(Thermostat* thermostat, int temperature)
{
    (void)printf("Received temperature %d\r\n", temperature);
    thermostat->Temperature = temperature;
    return EXECUTE_COMMAND_SUCCESS;
}

EXECUTE_COMMAND_RESULT SetHumidity(Thermostat* thermostat, int humidity)
{
    (void)printf("Received humidity %d\r\n", humidity);
    thermostat->Humidity = humidity;
    return EXECUTE_COMMAND_SUCCESS;
}
```

#### <a name="connecting-the-model-to-the-library"></a>Csatlakozás a modell a tárban

A függvények **sendMessage** és **IoTHubMessage** telemetriai küldése az eszközről és csatlakoztatása az üzenetek IoT központból a parancs kezelők újrafelhasználható kódját.

#### <a name="the-remotemonitoringrun-function"></a>A remote_monitoring_run függvény

A program **fő** függvény elindítja a **remote_monitoring_run** függvény, az eszköz viselkedés egy központi IoT eszköz ügyfél végrehajtásához az alkalmazás indításakor. Függvények egymásba ágyazott pár **remote_monitoring_run** függvény főleg áll:

- **Platform\_init** és **platform\_deinit** platform-specifikus inicializálni és leállítási műveletek hajthatók végre.
- **szerializáló\_init** és **szerializáló\_deinit** inicializálni és vonja inicializálni a szerializáló tárat.
- **IoTHubClient\_létrehozása** és **IoTHubClient\_Destroy** hozzon létre egy ügyfél fogópontot, **iotHubClientHandle**, a IoT-hubhoz csatlakozik, az eszköz hitelesítő adataival.

Fő szakaszában a **remote_monitoring_run** függvény a program a **iotHubClientHandle** fogópont használata az alábbi műveleteket hajtja végre:

- Létrehoz egy példány a Contoso termosztát modell, és beállítja az üzenet visszahívást a két parancsok.
- Az eszköz magát, beleértve a parancsok támogat, a IoT hubon keresztül csatlakozott, használja a szerializáló tár információt küld. A központi a következő üzenetet kapja, ha az eszköz állapotát módosítja az irányítópult a **függőben** **operációs rendszert futtató**.
- **Miközben** ismétlődve küld hőmérsékleti, külső hőmérsékleti és páratartalom értékek IoT központi minden második kezdődik.

Hivatkozás Íme egy minta **DeviceInfo** üzenetet IoT központi indításakor:

```
{
  "ObjectType":"DeviceInfo",
  "Version":"1.0",
  "IsSimulatedDevice":false,
  "DeviceProperties":
  {
    "DeviceID":"mydevice01", "HubEnabledState":true
  }, 
  "Commands":
  [
    {"Name":"SetHumidity", "Parameters":[{"Name":"humidity","Type":"double"}]},
    { "Name":"SetTemperature", "Parameters":[{"Name":"temperature","Type":"double"}]}
  ]
}
```

Hivatkozások a következő példa **Telemetriai** IoT központi küldött üzenet:

```
{"DeviceId":"mydevice01", "Temperature":50, "Humidity":50, "ExternalTemperature":55}
```

A hivatkozást a következő **parancs** kapott IoT központból minta:

```
{
  "Name":"SetHumidity",
  "MessageId":"2f3d3c75-3b77-4832-80ed-a5bb3e233391",
  "CreatedTime":"2016-03-11T15:09:44.2231295Z",
  "Parameters":{"humidity":23}
}
```

<a id="buildandrun"/>
### <a name="build-and-run-the-program"></a>Készítése és a program futtatása

1. Kattintson a **fordítási** létrehozásához a program. Biztonságos bármely figyelmeztetések mellőzése, de ha összeállítása hoz létre a hibák, oldja meg a folytatás előtt.

2. Ha összeállítása sikeres, a mbed fordító webhely létrehoz egy .bin fájlt, írja be az a projekt nevét, és letölti a helyi számítógépre. Másolja a .bin fájl az eszközön. Az eszköz újraindítása és a program a .bin fájlban lévő futtatása a .bin fájl az eszközön való mentése okoz. A program bármikor mbed az eszközre az Alaphelyzet gombra billentyűkombinációval manuálisan indítani.

3. Csatlakozás az eszköz SSH ügyfélprogram, például gitt alkalmazás segítségével. Meghatározhatja, hogy az eszközön, jelölje be a Windows-kezelő eszközt használ, a soros portot.

    ![][11]

4. GITT kattintson a **soros** kapcsolat típusát. Az eszköz a szokásos csatlakozik a 9600 átviteli, így adnia 9600 a **sebesség** mezőbe. Ezután kattintson a **Megnyitás**gombra.

5. Elindul a program végrehajtása. Előfordulhat, hogy állítsa alaphelyzetbe a fal (nyomja le a CTRL + oldaltörés, vagy nyomja le a fal Alaphelyzet gombra) Ha a program nem indul el automatikusan kapcsolódáskor.

    ![][10]

[AZURE.INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]


[6]: ./media/iot-suite-connecting-devices-mbed/mbed1.png
[7]: ./media/iot-suite-connecting-devices-mbed/mbed2a.png
[8]: ./media/iot-suite-connecting-devices-mbed/mbed3a.png
[9]: ./media/iot-suite-connecting-devices-mbed/suite6.png
[10]: ./media/iot-suite-connecting-devices-mbed/putty.png
[11]: ./media/iot-suite-connecting-devices-mbed/mbed6.png

[lnk-mbed-home]: https://developer.mbed.org/platforms/FRDM-K64F/
[lnk-mbed-getstarted]: https://developer.mbed.org/platforms/FRDM-K64F/#getting-started-with-mbed
[lnk-mbed-pcconnect]: https://developer.mbed.org/platforms/FRDM-K64F/#pc-configuration
[lnk-serializer]: https://azure.microsoft.com/documentation/articles/iot-hub-device-sdk-c-intro/#serializer
