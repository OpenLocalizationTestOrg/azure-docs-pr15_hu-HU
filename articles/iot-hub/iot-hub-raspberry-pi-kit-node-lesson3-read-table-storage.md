<properties
 pageTitle="Üzenetek olvasása állandó Azure-tárolóban lévő |} Microsoft Azure"
 description="Szerint az Azure táblatárolóhoz írták őket figyelje meg az eszköz a felhőbe üzeneteket."
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

# <a name="33-read-messages-persisted-in-azure-storage"></a>3.3 Azure-tárolóban lévő állandó olvas üzenetek

## <a name="331-what-will-you-do"></a>3.3.1 fog mire

Figyelje meg az eszköz a felhőbe küldött üzenetek a málna Pi 3 a IoT hubon keresztül csatlakozott, az üzenetek az Azure táblatárolóhoz kerülnek. Ha bármilyen problémát eleget tesz, keresés, [oldal hibaelhárítási](iot-hub-raspberry-pi-kit-node-troubleshooting.md)megoldások.

## <a name="332-what-will-you-learn"></a>3.3.2 mi ismerkedhet meg

- A gulp üzenet olvasása tevékenység használata üzenetek olvasása a Azure táblatárolóhoz az állandó.

## <a name="333-what-do-you-need"></a>3.3.3 mi van szüksége

- Meg kell sikeresen befejeződött az előző szakaszban [az Azure pislogás a málna Pi 3 minta az alkalmazásnak a futtatására](iot-hub-raspberry-pi-kit-node-lesson3-run-azure-blink.md).

## <a name="334-read-new-messages-from-your-storage-account"></a>3.3.4 új üzenetek olvasása a tárterület-fiókból

Az előző szakaszban egy minta alkalmazást a Pi a futtatta. A minta alkalmazás küldött üzenetek az Azure IoT hubon keresztül csatlakozott. Az üzeneteket a IoT hubon keresztül csatlakozott az Azure függvény alkalmazásából az Azure táblatárolóhoz tárolja. Az Azure tároló kapcsolati karakterláncot az Azure táblatárolóhoz az üzenetek olvasása van szüksége.

Az Azure táblatárolóban lévő üzenetek olvasása, kövesse az alábbi lépéseket:

1. A kapcsolati karakterlánc első a következő parancs futtatásával:

    ```bash
    az storage account list -g iot-sample --query [].name
    az storage account show-connection-string -g iot-sample -n {storage name}
    ```

    Az első parancs beolvassa a `storage name` használt a második parancs a kapcsolati karakterláncát. `iot-sample`az alapértelmezett érték `{resource group name}` Ha lecke 2 értéke nem módosítja.

2. Nyissa meg a konfigurációs fájl `config-raspberrypi.json` fájl Visual Studio-kódot a következő parancs futtatásával:

    ```bash
    # For Windows command prompt
    code %USERPROFILE%\.iot-hub-getting-started\config-raspberrypi.json

    # For macOS or Ubuntu
    code ~/.iot-hub-getting-started/config-raspberrypi.json
    ```

3. Csere `[Azure Storage connection string]` a kapcsolattal szerezte be az 1.
4. Mentse a `config-raspberrypi.json` fájlt.
5. Küldje el újra az üzeneteket, és olvasnak őket az Azure táblatárolóhoz a következő parancs futtatásával:

    ```bash
    gulp run --read-storage
    ```

    Azure táblatárolója olvasása a logikája szerepel a `azure-table.js` fájlt.

    ![gulp futásra – olvasható-tárhely](media/iot-hub-raspberry-pi-lessons/lesson3/gulp_read_message.png)

## <a name="335-summary"></a>3.3.5 összegzése

Már sikeresen a Pi a felhőben a IoT-hubhoz csatlakozik, és a pislogás minta alkalmazás használt eszköz a felhőbe üzeneteket küldhet. Bejövő IoT központi üzenetek tárolásához az Azure táblatárolóhoz használja az Azure függvény alkalmazást is. Áthelyezhetők a következő lecke cloud-eszköz üzeneteket küldeni a IoT központból a Pi hogyan szeretné.

## <a name="next-steps"></a>Következő lépések

[4 a lecke: Cloud-eszköz az üzenetek küldése](iot-hub-raspberry-pi-kit-node-lesson4-send-cloud-to-device-messages.md)
