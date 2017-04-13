<properties
 pageTitle="Fejlesztőeszközök útmutató - IoT központi SDK |} Microsoft Azure"
 description="Azure IoT központi Fejlesztőeszközök útmutató - információkat és a különböző Azure IoT központi eszközök és szolgáltatások SDK mutató hivatkozásokat."
 services="iot-hub"
 documentationCenter=""
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/30/2016"
 ms.author="dobett"/>

# <a name="iot-hub-sdks"></a>IoT központi SDK

## <a name="iot-hub-device-sdks"></a>SDK IoT központi eszköz

A Microsoft Azure IoT eszköz SDK kódot tartalmaznak, amely elősegíti épület eszközöket és alkalmazásokat, hogy csatlakozzon, és Azure IoT központi szolgáltatásai kezelhetők.

A következő IoT eszköz SDK érhetők el az GitHub letöltése:

- [Azure IoT eszköz SDK C] [ lnk-c-device-sdk] hordozhatóságát és a fő platform kompatibilitási ANSI C (C99) nyelven írt.
- [Azure IoT eszköz SDK a .NET rendszerhez][lnk-dotnet-device-sdk]
- [Azure IoT eszköz SDK Java][lnk-java-device-sdk]
- [Azure IoT eszköz SDK Node.js][lnk-node-device-sdk]
- [Microsoft Azure IoT eszköz SDK Python 2.7][lnk-python-device-sdk]

> [AZURE.NOTE] Lásd: a fontos fájlok bináris és függőségek telepítéséhez fejlesztési számítógépén a nyelvet és a menedzserek platform-specifikus csomag használatával kapcsolatos további tudnivalókért a GitHub tárházakban található.

## <a name="os-platforms-and-hardware-compatibility"></a>Operációs rendszerek és a hardver kompatibilitás

Az adott hardvereszközöket SDK kompatibilitással kapcsolatos további tudnivalókért lásd: [Azure tanúsítvánnyal IoT eszköz katalógus][lnk-certified].

## <a name="iot-hub-service-sdks"></a>IoT központi szolgáltatás SDK

A Microsoft Azure IoT szolgáltatás SDK, amely elősegíti épület alkalmazásokat közvetlenül a IoT központi kezelésére, eszközök és a biztonsági kódot tartalmaznak.

A következő IoT szolgáltatás SDK GitHub letöltését érhetők el:

- [Azure IoT szolgáltatás SDK a .NET rendszerhez][lnk-dotnet-service-sdk]
- [Azure IoT szolgáltatás SDK Node.js][lnk-node-service-sdk]
- [Azure IoT szolgáltatás SDK Java][lnk-java-service-sdk]

> [AZURE.NOTE] Lásd: a fontos fájlok bináris és függőségek telepítéséhez fejlesztési számítógépén a nyelvet és a menedzserek platform-specifikus csomag használatával kapcsolatos további tudnivalókért a GitHub tárházakban található.

## <a name="azure-iot-gateway-sdk"></a>Azure IoT átjáró SDK

Ez az Azure IoT átjáró SDK infrastruktúra és modulokat IoT átjáró megoldások létrehozása tartalmazza. Az átjárók bármely végpontok közötti forgatókönyv szabott létrehozása SDK bővítése

Letöltheti az [Azure IoT átjáró SDK] [ lnk-gateway-sdk] a GitHub.

## <a name="online-api-reference-documentation"></a>API-hivatkozás online dokumentációt

Az alábbiakban látható egy online API dokumentációját Azure IoT eszközt, a szolgáltatás és az átjáró tárak mutató hivatkozások listája:

- [Internetes dolgot (IoT) .NET][lnk-dotnet-ref]
- [IoT központi többi][lnk-rest-ref]
- [Azure IoT eszköz SDK C][lnk-c-ref]
- [Azure IoT eszköz SDK Java][lnk-java-ref]
- [Azure IoT szolgáltatás SDK Java][lnk-java-service-ref]
- [Azure IoT eszköz SDK Node.js][lnk-node-ref]
- [Azure IoT szolgáltatás SDK Node.js][lnk-node-service-ref]
- [Azure IoT átjáró SDK][lnk-gateway-ref]

## <a name="next-steps"></a>Következő lépések

Hivatkozás témakörök IoT központi Fejlesztőeszközök tartalom a következők:

- [IoT központi végpontok][lnk-devguide-endpoints]
- [Lekérdezési nyelv twins, módszerek és feladatok][lnk-devguide-query]
- [Kvóták és szabályozása][lnk-devguide-quotas]
- [IoT központi MQTT támogatás][lnk-devguide-mqtt]

<!-- Links and images -->

[lnk-c-device-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/c/readme.md
[lnk-dotnet-device-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/csharp/device/readme.md
[lnk-java-device-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/java/device/readme.md
[lnk-dotnet-service-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/csharp/service/README.md
[lnk-java-service-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/java/service/readme.md
[lnk-node-device-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/node/device/readme.md
[lnk-node-service-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/node/service/README.md
[lnk-python-device-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/python/device/readme.md
[lnk-certified]: https://catalog.azureiotsuite.com/
[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/README.md

[lnk-dotnet-ref]: https://msdn.microsoft.com/library/mt488521.aspx
[lnk-c-ref]: http://azure.github.io/azure-iot-sdks/c/api_reference/index.html
[lnk-java-ref]: http://azure.github.io/azure-iot-sdks/java/device/api_reference/index.html
[lnk-node-ref]: http://azure.github.io/azure-iot-sdks/node/api_reference/azure-iot-device/1.0.15/index.html
[lnk-rest-ref]: https://msdn.microsoft.com/library/mt548492.aspx
[lnk-java-service-ref]: http://azure.github.io/azure-iot-sdks/java/service/api_reference/index.html
[lnk-node-service-ref]: http://azure.github.io/azure-iot-sdks/node/api_reference/azure-iothub/1.0.17/index.html
[lnk-gateway-ref]: http://azure.github.io/azure-iot-gateway-sdk/api_reference/c/html/

[lnk-devguide-endpoints]: iot-hub-devguide-endpoints.md
[lnk-devguide-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-devguide-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md