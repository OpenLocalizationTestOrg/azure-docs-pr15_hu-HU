<properties
     pageTitle="Az Azure portal segítségével a többi fájlfeltöltéshez beállítása |} Microsoft Azure"
     description="Megtudhatja, hogy miként fájlfeltöltéshez az Azure portálon konfigurálása"
     services="iot-hub"
     documentationCenter=""
     authors="dominicbetts"
     manager="timlt"
     editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="na"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/30/2016"
     ms.author="dobett"/>

# <a name="configure-file-uploads-using-the-azure-portal"></a>Az Azure portálon fájlfeltöltések konfigurálása

## <a name="file-upload"></a>A fájl feltöltése

[Fájl feltöltése a funkciók a IoT központi]használandó[lnk-upload], először társítania kell Azure tárterület-fiók a-központját. Kattintson a **fájl feltöltése** beállítások az éppen módosított IoT-központban feltöltés fájltulajdonságok listájának megjelenítéséhez.

![][13]

**Tárterület tároló**: az Azure portal segítségével blob-tároló jelölje ki az aktuális előfizetése Azure társítani a IoT központi Azure tárterület-fiókkal. Ha szükséges, létrehozhat Azure tárterület-fiók a **tárolók** lap **tároló fiókok** lap és a blob tároló a. IoT központi Társítások URL-címe automatikusan létrehozza az írási engedélye ehhez a blob-tárolóhoz eszközök: Ha az azok a fájlok feltöltése.

![][14]

**Feltöltött fájlokat fogadás értesítésekhez**: engedélyezése vagy letiltása a fájl feltöltésekről tájékoztató értesítések keresztül a kapcsolót.

**Társítások TTL (élettartam)**: Ez a beállítás nem a time-to-live, az eszközhöz kapott IoT központi Társítások URL-címe. Alapértelmezés szerint egy óra állítva, de testre szabható egyéb értékek a csúszka használatával.

**Fájl értesítési beállítások default TTL (élettartam)**: A time-to-live fájl feltöltése értesítést, mielőtt lejárt. Alapértelmezés szerint egy nap beállítása, de testre szabható egyéb értékek a csúszka használatával.

**Fájlok értesítés kézbesítési maximális száma**: hányszor a IoT központi megpróbál egy fájl feltöltése értesítés. Alapértelmezés szerint 10 beállítva, de testre szabható egyéb értékek a csúszka használatával.

![][15]

## <a name="next-steps"></a>Következő lépések

További információt IoT központi a fájl feltöltése képességeiről című [tölthet fel fájlokat a eszközről] [ lnk-upload] a fejlesztői útmutatóban olvashat.

Kövesse ezeket a hivatkozásokat, ha többet szeretne tudni az Azure IoT központi kezelése:

- [Tömeges IoT eszközök kezelése][lnk-bulk]
- [Mértékek használata][lnk-metrics]
- [Műveletek figyelése][lnk-monitor]

További feltárása a IoT központi funkcióinak, olvassa el:

- [Fejlesztőeszközök útmutató][lnk-devguide]
- [Egy eszközt a IoT átjáró SDK csomagjában talál, amelyek][lnk-gateway]
- [A IoT megoldás az alapoktól kezdve biztonságos mentése][lnk-securing]


  [13]: ./media/iot-hub-configure-file-upload/file-upload-settings.png
  [14]: ./media/iot-hub-configure-file-upload/file-upload-container-selection.png
  [15]: ./media/iot-hub-configure-file-upload/file-upload-selected-container.png

[lnk-upload]: iot-hub-devguide-file-upload.md

[lnk-bulk]: iot-hub-bulk-identity-mgmt.md
[lnk-metrics]: iot-hub-metrics.md
[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
[lnk-securing]: iot-hub-security-ground-up.md