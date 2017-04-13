<properties
 pageTitle="Azure IoT központi méretezés |} Microsoft Azure"
 description="Ha át kívánja méretezni Azure IoT központi ismerteti."
 services="iot-hub"
 documentationCenter=""
 authors="fsautomata"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/19/2016"
 ms.author="elioda"/>

# <a name="scaling-iot-hub"></a>Méretezési IoT központi

Azure IoT központi egymillió akár egyidejű csatlakoztatott eszközök támogatják. További tudnivalókért lásd: a [IoT központi árak][lnk-pricing]. Minden egyes IoT központi egység lehetővé teszi, hogy a megadott számú napi üzeneteket.

A megoldás megfelelően méretezéséhez fontolja meg az adott használt IoT központi. Mindenekelőtt fontolja meg a szükséges csúcs átviteli a következő kategóriák műveletek:

* Üzenetek eszköz a felhőbe
* Üzenetek cloud-eszköz
* Identitás beállításjegyzék műveletek

Mellett az átvitel létrehozójától [IoT központi kvóták][] és szabályozás, és ennek megfelelően tervezése a megoldás.

## <a name="device-to-cloud-and-cloud-to-device-message-throughput"></a>Eszköz a felhőbe, és a felhő-eszköz üzenet átviteli

A legjobb módszer a méretezés-IoT központi megoldás a forgalom egység alapon ki szeretné számítani.

Eszköz a felhőbe üzeneteket a következő tartós átviteli szabályokat.

| Réteg | Folyamatos átviteli | Folyamatos küldés ráta |
| ---- | -------------------- | ------------------- |
| S1 | Felfelé egység 1111 KB/percenként<br/>(1,5 GB/nap/egység) | Átlaga egység 278 üzenetek/percenként<br/>(400 000 üzenetek/nap / egység) |
| S2 | Legfeljebb 16 MB/percenként egység<br/>(22.8 GB/nap/egység) | Átlaga egység 4167 üzenetek/percenként<br/>(6 millió üzenetek/nap egység) |
| S3 | Felfelé egység 814 MB/percenként<br/>(1144.4 GB/nap/egység) | Átlaga egység 208,333 üzenetek/percenként<br/>(300 millió üzenetek/nap / egység) |

## <a name="identity-registry-operation-throughput"></a>Identitás beállításjegyzék művelet átviteli

IoT központi identitás beállításjegyzék műveletek vannak nem kellene futtatókörnyezet műveletek, főleg kapcsolódnak eszköz kiépítési.

Adott burst teljesítmény számok olvassa el a [IoT központi kvótáinak és a szabályozás][]című témakört.

## <a name="sharding"></a>Sharding

Egy egyetlen IoT hubhoz eszközök milliónyi is méretezhető, miközben előfordul, hogy a megoldás, amely egyetlen IoT hubon nem garantálja az adott teljesítmény jellemzők szükséges. Ebben az esetben javasolt, hogy több IoT hubra partition eszközén. Több IoT hubok forgalom felszakadásáig sima, és szerezze be a szükséges átviteli vagy művelet díjak szükséges.

## <a name="next-steps"></a>Következő lépések

További feltárása a IoT központi funkcióinak, olvassa el:

- [Fejlesztőeszközök útmutató][lnk-devguide]
- [Egy eszközt a IoT átjáró SDK csomagjában talál, amelyek][lnk-gateway]

[lnk-pricing]: https://azure.microsoft.com/pricing/details/iot-hub
[IoT központi kvótáinak és a szabályozás]: iot-hub-devguide-quotas-throttling.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
