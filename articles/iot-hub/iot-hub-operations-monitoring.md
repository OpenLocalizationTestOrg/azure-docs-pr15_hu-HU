<properties
 pageTitle="Figyelés IoT központi műveletek"
 description="Azure IoT központi műveletek figyelés, amely lehetővé teszi a Lync-műveletek a valós idejű IoT-központját állapotának áttekintése"
 services="iot-hub"
 documentationCenter=""
 authors="nberdy"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="08/11/2016"
 ms.author="nberdy"/>

# <a name="introduction-to-operations-monitoring"></a>Műveletek figyelése – bevezetés

Figyelés IoT központi műveletek lehetővé teszi, hogy figyelheti a valós idejű IoT-központját műveletek állapotát. IoT központi az események több kategória műveletek nyomon követi, és választhatja az egy vagy több kategóriát események küld, a IoT központi feldolgozásra zárólap. Lync-hibák adatait, vagy adatok mintázatok alapján összetettebb feldolgozásának beállítása.

IoT központi öt kategóriák a teljes figyeli:

- Eszköz identitás műveletek
- Eszköz telemetriai
- Felhőalapú-eszköz parancsok
- Kapcsolatok
- Fájlfeltöltések

## <a name="how-to-enable-operations-monitoring"></a>Hogyan engedélyezhető a műveletek figyelése

1. Hozzon létre egy IoT hubhoz. Hogyan hozhat létre egy IoT központ az [Első lépések] útmutató megtalálhatja[ lnk-get-started] útmutató.

2. Nyissa meg a lap, a IoT-központját. Itt kattintson a **Műveletek figyelése**.

    ![][1]

3. Jelölje ki a megfigyeléssel kategóriákat, figyelni, és kattintson a **Mentés**kívánt. Az események az esemény központi-kompatibilis végpontot **ellenőrzési beállítások**szerepel az olvasási érhetők el. A központi IoT végpont nevezik `messages/operationsmonitoringevents`.

    ![][2]

## <a name="event-categories-and-how-to-use-them"></a>Esemény kategóriák és használatuk

Minden egyes kategória számok figyelése műveleteket egy másik típusú IoT központi és felügyeleti kategórián interakció rendelkezik, amely definiálja, hogyan kategóriákba tartozó események szerkezete séma.

### <a name="device-identity-operations"></a>Eszköz identitás műveletek

Az eszköz identitás műveletek kategória fordulhat elő, amikor megpróbálja létrehozása, módosítása és törlése egy bejegyzést a IoT központi identitás beállításjegyzékben hibákról nyomon követi. Követés ebbe a kategóriába esetén hasznos esetek kiépítési.

    {
        "time": "UTC timestamp",
         "operationName": "create",
         "category": "DeviceIdentityOperations",
         "level": "Error",
         "statusCode": 4XX,
         "statusDescription": "MessageDescription",
         "deviceId": "device-ID",
         "durationMs": 1234,
         "userAgent": "userAgent",
         "sharedAccessPolicy": "accessPolicy"
    }

### <a name="device-telemetry"></a>Eszköz telemetriai

Az eszköz telemetriai kategória nyomon követi a IoT központi előforduló, és a telemetriai folyamat kapcsolatos hibák. Ez a kategória tartalmazza (például szabályozásának) telemetriai események küldésekor előforduló hibák és fogadása telemetriai események (például jogosulatlan olvasó). Figyelje meg, hogy a ebbe a kategóriába nem elfog magán az eszközön futó kód okozta hibákat.

    {
         "messageSizeInBytes": 1234,
         "batching": 0,
         "protocol": "Amqp",
         "authType": "{\"scope\":\"device\",\"type\":\"sas\",\"issuer\":\"iothub\"}",
         "time": "UTC timestamp",
         "operationName": "ingress",
         "category": "DeviceTelemetry",
         "level": "Error",
         "statusCode": 4XX,
         "statusType": 4XX001,
         "statusDescription": "MessageDescription",
         "deviceId": "device-ID",
         "EventProcessedUtcTime": "UTC timestamp",
         "PartitionId": 1,
         "EventEnqueuedUtcTime": "UTC timestamp"
    }

### <a name="cloud-to-device-commands"></a>Felhőalapú-eszköz parancsok

A felhő-eszköz menüparancsok kategóriájában nyomon követi a IoT központi előforduló és az eszköz parancs folyamat kapcsolatos hibák. Ez a kategória (például jogosulatlan feladó) parancsokat, fogad (például a kézbesítési túllépve) parancsokat, válna parancs visszajelzés (mint például a visszajelzés járt le) használatakor előforduló hibák tartalmazza. Ez a kategória foghat hibák, amelyek nem megfelelően kezeli a parancsot, ha a parancs sikeresen kézbesítve lett a eszközről.

    {
         "messageSizeInBytes": 1234,
         "authType": "{\"scope\":\"hub\",\"type\":\"sas\",\"issuer\":\"iothub\"}",
         "deliveryAcknowledgement": 0,
         "protocol": "Amqp",
         "time": " UTC timestamp",
         "operationName": "ingress",
         "category": "C2DCommands",
         "level": "Error",
         "statusCode": 4XX,
         "statusType": 4XX001,
         "statusDescription": "MessageDescription",
         "deviceId": "device-ID",
         "EventProcessedUtcTime": "UTC timestamp",
         "PartitionId": 1,
         "EventEnqueuedUtcTime": “UTC timestamp"
    }

### <a name="connections"></a>Kapcsolatok

A kapcsolatok kategóriára nyomon követi a hibáról, amikor eszközök csatlakoztatása vagy leválasztása IoT-központban. Követés ebbe a kategóriába akkor lehet hasznos, ezzel az illetéktelen kapcsolatfelvételi azonosítása és nyomon követése, ha egy megszakad a kapcsolat gyenge kapcsolódási részeinek készülékén.

    {
         "durationMs": 1234,
         "authType": "{\"scope\":\"hub\",\"type\":\"sas\",\"issuer\":\"iothub\"}",
         "protocol": "Amqp",
         "time": " UTC timestamp",
         "operationName": "deviceConnect",
         "category": "Connections",
         "level": "Error",
         "statusCode": 4XX,
         "statusType": 4XX001,
         "statusDescription": "MessageDescription",
         "deviceId": "device-ID"
    }

### <a name="file-uploads"></a>Fájlfeltöltések

A fájl feltöltése kategória nyomon követi a hibák, amelyek a IoT központi előforduló és kapcsolódó funkciók a fájl feltöltése. Ez a kategória tartalmazza (például amikor lejárta előtt eszköz értesíti a kezdőképernyő központjában egy befejezett feltöltött) a Társítások URI előforduló hibák, a sikertelen feltöltést jelenteni az eszköz, és amikor a fájl nem található tároló IoT központi értesítő üzenet létrehozása során. Figyelje meg, hogy az ebbe a kategóriába jelentkező hibák közvetlenül az eszköz fájl feltöltésekor tárolóhoz nem elfog.

    {
         "authType": "{\"scope\":\"hub\",\"type\":\"sas\",\"issuer\":\"iothub\"}",
         "protocol": "HTTP",
         "time": " UTC timestamp",
         "operationName": "ingress",
         "category": "fileUpload",
         "level": "Error",
         "statusCode": 4XX,
         "statusType": 4XX001,
         "statusDescription": "MessageDescription",
         "deviceId": "device-ID",
         "blobUri": "http//bloburi.com",
         "durationMs": 1234
    }

## <a name="next-steps"></a>Következő lépések

További feltárása a IoT központi funkcióinak, olvassa el:

- [Fejlesztőeszközök útmutató][lnk-devguide]
- [Egy eszközt a IoT átjáró SDK csomagjában talál, amelyek][lnk-gateway]

<!-- Links and images -->
[1]: media/iot-hub-operations-monitoring/enable-OM-1.png
[2]: media/iot-hub-operations-monitoring/enable-OM-2.png

[lnk-get-started]: iot-hub-csharp-csharp-getstarted.md
[lnk-diagnostic-metrics]: iot-hub-metrics.md
[lnk-scaling]: iot-hub-scaling.md
[lnk-dr]: iot-hub-ha-dr.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md