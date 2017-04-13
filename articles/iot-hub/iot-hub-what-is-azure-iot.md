<properties
 pageTitle="Az Internet a elhárításukhoz Azure megoldások |} Microsoft Azure"
 description="A minta megoldási, és hogyan vonatkozik Azure IoT központi eszköz SDK és előre definiált megoldások Azure IoT egy áttekintése"
 services="iot-hub"
 documentationCenter=""
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="get-started-article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="10/05/2016"
 ms.author="dobett"/>

[AZURE.INCLUDE [iot-azure-and-iot](../../includes/iot-azure-and-iot.md)]

## <a name="next-steps"></a>Következő lépések

Azure IoT központi egy Azure szolgáltatás, amely lehetővé teszi, hogy a biztonságos és az alkalmazás közötti megbízható kétirányú kommunikációt biztonsági vége és eszközök milliónyi. Lehetővé teszi a alkalmazás vissza végén:

- A méretezés telemetriai kap eszközén.
- Adatok készülékekről adatfolyam esemény processzort irányítja.
- Kap a fájlfeltöltések eszközökről.
- Küldje el parancsok cloud-eszköz adott eszközök.

A saját megoldás vissza vége végrehajtásához IoT központi is használhatja. Ezeken kívül az IoT központi eszközök, a hitelesítő adatokat és a központi csatlakozhat jogainak kiépítése használt eszköz identitás beállításjegyzék tartalmazza. IoT központi kapcsolatos további tudnivalókért lásd: [Mi az központi IoT?] [lnk-iot-hub].

Hogyan Azure IoT központi lehetővé teszi, hogy szabványos-alapú IoT az mobileszköz-kezelés távolról kezelheti, beállítása és frissítése az eszközén című témakörben talál [mobileszköz-kezelés Áttekinté az Azure IoT központi][lnk-device-management].

Számos különböző eszköz hardver platformokon és operációs rendszerek ügyfélalkalmazásokban végrehajtásához az IoT eszköz SDK is használhatja. A IoT eszköz SDK tárakra, amelyek megkönnyítik a küldő telemetriai egy IoT központi és a felhő-eszköz parancsokat fogadó tartalmazzák. A SDK használata esetén választhat számos hálózati protokollokat IoT központi kommunikálni. További tudnivalókért lásd: az [eszköz SDK adatai][lnk-device-sdks].

Első lépésként néhány kódírás és néhány minták futtatása a [IoT központi – első lépések] című[ lnk-getstarted] oktatóprogram.

Is [Azure IoT csomagja]érdekli[lnk-iot-suite], amely olyan, előre definiált megoldások gyűjteménye. IoT csomagja lehetővé teszi, hogy a szoftver használatának gyors megkezdése és IoT projektek gyakori helyzetekben IoT – például a távoli figyelés, az eszközök kezelése és a prediktív karbantartási méretezni.

[lnk-getstarted]: iot-hub-csharp-csharp-getstarted.md
[lnk-device-sdks]: https://github.com/Azure/azure-iot-sdks/blob/master/readme.md
[lnk-iot-hub]: iot-hub-what-is-iot-hub.md
[lnk-iot-suite]: https://azure.microsoft.com/documentation/suites/iot-suite/
[lnk-iotdev]: https://azure.microsoft.com/develop/iot/
[lnk-device-management]: iot-hub-device-management-overview.md