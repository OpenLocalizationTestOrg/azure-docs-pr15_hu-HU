<properties
 pageTitle="Első lépések a mobileszköz-kezelés |} Microsoft Azure"
 description="Ebből az oktatóanyagból megtudhatja, hogy miként Azure IoT központi a mobileszköz-kezelés – első lépések"
 services="iot-hub"
 documentationCenter=".net"
 authors="juanjperez"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/30/2016" 
 ms.author="juanpere"/>

# <a name="tutorial-get-started-with-device-management-preview"></a>Oktatóprogram: Első lépések a mobileszköz-kezelés (előzetes verzió)

## <a name="introduction"></a>– Bevezetés
IoT felhő alkalmazások segítségével primitívek az Azure-IoT központban nevezetesen a eszköz kettős és közvetlen módszerek, távolról elindítása és figyelése eszköz adatkezelési műveletek eszközökön.  Ez a cikk nyújt a útmutatást és kód együtt szeretne kezdeményezni és figyelheti a távoli eszköz újraindítása IoT elosztót használ egy IoT felhő alkalmazást és az eszköz működéséről.

Távolról indítása és figyelése eszköz adatkezelési műveletek az eszközein, felhőalapú, a háttéradatbázist alkalmazásból használhatja például [eszköz kettős] Azure IoT központi primitívek[ lnk-devtwin] és a [felhő-eszköz (C2D) módszerek][lnk-c2dmethod]. Ebben az oktatóanyagban jeleníti meg, hogyan a háttéradatbázist alkalmazás egy eszközt is működik együtt ahhoz, hogy kezdeményezzen, és figyelemmel követheti a távoli eszköz újraindítása IoT központból.

Egy C2D módszerrel eszköz adatkezelési műveletek (például indítsa újra a rendszert, gyári alaphelyzetbe állítása és belső vezérlőprogram frissítésének) háttéradatbázist alkalmazásból a felhőbe kezdeményezhet. Az eszköz a felelős a:

- A módszer kérelem IoT központból küld kezelése.
- A megfelelő eszköz adott művelet az eszközön kezdeményezése.
- Tulajdonságok állapotfrissítések – az eszköz kettős nyújtó jelenteni IoT-hubon keresztül csatlakozott.

A felhőben a háttéradatbázist alkalmazás segítségével is jelenteni az eszköz adatkezelési műveletek végrehajtásának eszköz kettős-lekérdezések futtatása.

Ebből az oktatóanyagból megtudhatja, hogyan szeretné:

- Az Azure portal segítségével hozzon létre egy IoT hubhoz, és hozzon létre egy eszköz identitás a IoT-központját.
- Hozzon létre egy szimulált eszközön, amelyen a közvetlen módszer, amely lehetővé teszi a indítsa újra a rendszert, amely szerint az a felhő hívható.
- Indítsa újra a rendszert közvetlen módszer felhívja a IoT központi keresztül szimulált eszközön console-alkalmazás létrehozása.

Ez az oktatóanyag végén alkalmazások vannak telepítve a két Node.js konzol:

**dmpatterns_getstarted_device.js**, amely a korábban létrehozott eszköz identitású kapcsolódik a IoT hubon keresztül csatlakozott, indítsa újra a rendszert a közvetlen módszer kap, keltő szűrő megőrzi a fizikai kell indítani a számítógépet, és az utolsó újraindítás ideje jelentések.

a jelentett tulajdonságok **dmpatterns_getstarted_service.js**, amely felhívja a közvetlen módszer szimulált az eszközre, a válasz, valamint a frissített eszköz kettős jeleníti meg.

Ebben az oktatóanyagban a következőkre lesz szüksége:

NODE.js verzió 0.12.x vagy újabb <br/>  [Felkészülés a fejlesztői környezet] [ lnk-dev-setup] ismerteti, hogyan lehet az ebben az oktatóanyagban Node.js telepítése Windows vagy Linux rendszerhez.

Azure active fiók. (Nem rendelkeznek fiókkal, ha egy [ingyenes fiókra] hozhat létre[ lnk-free-trial] a mindössze néhány perc.)

[AZURE.INCLUDE [iot-hub-get-started-create-hub-pp](../../includes/iot-hub-get-started-create-hub-pp.md)]

## <a name="create-a-simulated-device-app"></a>Szimulált eszközön alkalmazás létrehozása

Ebben a részben hoz létre hívja meg a felhőben, amely elindítja a szimulált eszközön újra kell indítani a közvetlen módszer válaszoló Node.js konzol alkalmazást, és használja az eszköz kettős jelenteni tulajdonságok ahhoz, hogy eszköz kettős lekérdezések azonosítja az eszközén, és amikor azok az utolsó újraindítása után.

1. Hozzon létre egy új üres nevű **manageddevice**.  A **manageddevice** mappába az alábbi parancs használatával a parancssorban package.json fájl létrehozása.  Fogadja el az alapértelmezett beállításokat:

    ```
    npm init
    ```
    
2. Parancssorba a- **manageddevice** mappában, a következő parancsot telepítheti a **azure-iot-device@dtpreview** eszköz SDK csomag és **azure-iot-device-mqtt@dtpreview** csomag:

    ```
    npm install azure-iot-device@dtpreview azure-iot-device-mqtt@dtpreview --save
    ```

3. Egy egyszerű szövegszerkesztő programban, hoz létre új **dmpatterns_getstarted_device.js** fájl **manageddevice** mappában.

4. Adja meg a következő 'szükséges' kimutatások **dmpatterns_getstarted_device.js** fájl elején:

    ```
    'use strict';

    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    ```

5. **ConnectionString** változó hozzáadása, és hozzon létre egy eszköz ügyfél használatával.  A kapcsolati karakterláncot cserélje le a eszköz kapcsolati karakterláncot.  

    ```
    var connectionString = 'HostName={youriothostname};DeviceId=myDeviceId;SharedAccessKey={yourdevicekey}';
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```

6. Adja hozzá a következő függvény a direkt módszer végrehajtásához az eszközön

    ```
    var onReboot = function(request, response) {
        
        // Respond the cloud app for the direct method
        response.send(200, 'Reboot started', function(err) {
            if (!err) {
                console.error('An error occured when sending a method response:\n' + err.toString());
            } else {
                console.log('Response to method \'' + request.methodName + '\' sent successfully.');
            }
        });
        
        // Report the reboot before the physical restart
        var date = new Date();
        var patch = {
            iothubDM : {
                reboot : {
                    lastReboot : date.toISOString(),
                }
            }
        };

        // Get device Twin
        client.getTwin(function(err, twin) {
            if (err) {
                console.error('could not get twin');
            } else {
                console.log('twin acquired');
                twin.properties.reported.update(patch, function(err) {
                    if (err) throw err;
                    console.log('Device reboot twin state reported')
                });  
            }
        });
        
        // Add your device's reboot API for physical restart.
        console.log('Rebooting!');
    };
    ```

7. Nyissa meg a kapcsolatot a IoT hubon keresztül csatlakozott, és indítsa el a közvetlen módszer figyelő:

    ```
    client.open(function(err) {
        if (err) {
            console.error('Could not open IotHub client');
        }  else {
            console.log('Client opened.  Waiting for reboot method.');
            client.onDeviceMethod('reboot', onReboot);
        }
    });
    ```
    
8. Mentse és zárja be a **dmpatterns_getstarted_device.js** fájlt.

 [AZURE.NOTE] Ahhoz, hogy egyszerű dolgot, ebben az oktatóanyagban nem valósítja bármely újrapróbálkozási házirend. Gyártási kódban végre kell hajtania újrapróbálkozási házirendek (például egy exponenciális visszalépési), az MSDN-cikk [Tranziens hibafa kezelése]a javasolt[lnk-transient-faults].

## <a name="trigger-a-remote-reboot-on-the-device-using-a-direct-method"></a>Az elindítani a távoli újra kell indítani az eszközre a közvetlen módszerének alkalmazásával. 

Ebben a részben hoz létre, amely elindítja a távoli számítógép újraindítása közvetlen módszerrel eszközön, és keresse meg az, hogy az eszköz újraindítása legutóbb eszköz kettős lekérdezések használ Node.js konzol alkalmazást.

1. Hozzon létre egy új üres nevű **triggerrebootondevice**.  A **triggerrebootondevice** mappába az alábbi parancs használatával a parancssorban package.json fájl létrehozása.  Fogadja el az alapértelmezett beállításokat:

    ```
    npm init
    ```
    
2. Parancssorba a- **triggerrebootondevice** mappában, a következő parancsot telepítheti a **azure-iothub@dtpreview** eszköz SDK csomag és **azure-iot-device-mqtt@dtpreview** csomag:

    ```
    npm install azure-iothub@dtpreview --save
    ```
    
3. Egy egyszerű szövegszerkesztő programban, hoz létre új **dmpatterns_getstarted_service.js** fájl **triggerrebootondevice** mappában.

4. Adja meg a következő 'szükséges' kimutatások **dmpatterns_getstarted_service.js** fájl elején:

    ```
    'use strict';

    var Registry = require('azure-iothub').Registry;
    var Client = require('azure-iothub').Client;
    ```

5. Adja hozzá a következő változó nyilatkozatokat, és cserélje le a helyőrző értékeket:

    ```
    var connectionString = '{iothubconnectionstring}';
    var registry = Registry.fromConnectionString(connectionString);
    var client = Client.fromConnectionString(connectionString);
    var deviceToReboot = 'myDeviceId';
    ```
    
6. Adja hozzá a következő függvény az eszköz módszert, indítsa újra a célként megadott eszköz meghívásához:

    ```
    var startRebootDevice = function(twin) {

        var methodName = "reboot";
        
        var methodParams = {
            methodName: methodName,
            payload: null,
            timeoutInSeconds: 30
        };
        
        client.invokeDeviceMethod(deviceToReboot, methodParams, function(err, result) {
            if (err) { 
                console.error("Direct method error: "+err.message);
            } else {
                console.log("Successfully invoked the device to reboot.");  
            }
        });
    };
    ```

7. Adja hozzá az eszköz a lekérdezés és a legutóbbi indítsa újra a rendszert a következő függvénynek:

    ```
    var queryTwinLastReboot = function() {

        registry.getTwin(deviceToReboot, function(err, twin){

            if (twin.properties.reported.iothubDM != null)
            {
                if (err) {
                    console.error('Could not query twins: ' + err.constructor.name + ': ' + err.message);
                } else {
                    var lastRebootTime = twin.properties.reported.iothubDM.reboot.lastReboot;
                    console.log('Last reboot time: ' + JSON.stringify(lastRebootTime, null, 2));
                }
            } else 
                console.log('Waiting for device to report last reboot time.');
        });
    };
    ```
    
8. Adja hozzá a következő kódot elindítani a legutóbbi indítsa újra a rendszert a újraindítás közvetlen módszer és a lekérdezés függvényeket:

    ```
    startRebootDevice();
    setInterval(queryTwinLastReboot, 2000);
    ```
    
9. Mentse és zárja be a **dmpatterns_getstarted_service.js** fájlt.

## <a name="run-the-applications"></a>Az alkalmazások futtatása

Most már készen áll az alkalmazások futtatásához.

1. A parancssorablakban – a **manageddevice** mappában, futtassa a következő parancsot, indítsa újra a rendszert közvetlen mód figyel megkezdéséhez.

    ```
    node dmpatterns_getstarted_device.js
    ```

2. A parancssorba- **triggerrebootondevice** mappában futtassa a következő parancsot a távoli újraindítás és lekérdezése az eszköz kettős keresése az utolsó indítsa újra az idő eseményindító.

    ```
    node dmpatterns_getstarted_service.js
    ```

3. Ekkor megjelenik a közvetlen módszerrel reagálásra kinyomtatásával meg az üzenet

## <a name="customize-and-extend-the-device-management-actions"></a>Az eszköz adatkezelési műveletek kiterjesztése és testreszabása

A IoT megoldások eszköz management mintázatok meghatározott kibonthatja és engedélyezése az eszköz kettős és C2D módszer primitívek való egyéni minták. Egyéb eszköz adatkezelési műveletek többek között gyári alaphelyzetbe állítása, belső vezérlőprogram frissítése, szoftver frissítése, power kezelése, hálózati és a kapcsolatok kezelése és adatok titkosítása.

## <a name="device-maintenance-windows"></a>Eszköz karbantartási windows

A szokásos beállíthatja a műveletek végrehajtása a lehető legkevesebb kiküszöbölése és legrövidebb leállás egyszerre eszközök.  Eszköz karbantartási windows olyan gyakran használt mintázatot adja meg a időt, amikor egy eszközt kell frissíteni a konfigurációját. A háttér-megoldások a eszköz kettős kívánt tulajdonságainak segítségével definiálása és aktiválása az eszközön, amely lehetővé teszi a karbantartás ablak házirend. Amikor egy eszközt a karbantartás ablak Házirend kap, akkor a jelentett tulajdonsága az eszköz kettős segítségével a házirend állapotának jelentése. A háttéradatbázist alkalmazás segítségével eszköz kettős lekérdezések eszközök és a házirend-megfelelőséggel igazolnia.

## <a name="next-steps"></a>Következő lépések

Ebben az oktatóanyagban használt közvetlen módszer szeretne elindítani egy eszközön, használja a távoli újra kell indítani az eszköz kettős jelenteni az eszköz újraindítása legutóbb tulajdonságok jelenteni, és az eszköz kettős fel a felhőből az eszköz újraindítása legutóbb lekérdezni.

Továbbra is, és első lépések IoT központi eszköz management mintázatok, például a távoli fölé a air frissítést, olvassa el:

[Oktatóprogram: Hogyan lehet a belső vezérlőprogram frissítése][lnk-fwupdate]

Megtudhatja, hogy miként, ha ki szeretné terjeszteni a IoT megoldás és az ütemezés módja a hívások több eszközön, olvassa el a [ütemezése és közvetítési feladatok] [ lnk-tutorial-jobs] oktatóprogram.

Első lépések – IoT központi folytatásához lásd: [a IoT átjáró SDK – első lépések][lnk-gateway-SDK].


<!-- images and links -->
[img-output]: media/iot-hub-get-started-with-dm/image6.png
[img-dm-ui]: media/iot-hub-get-started-with-dm/dmui.png

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md

[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-fwupdate]: iot-hub-firmware-update.md
[Azure portal]: https://portal.azure.com/
[Using resource groups to manage your Azure resources]: ../azure-portal/resource-group-portal.md
[lnk-dm-github]: https://github.com/Azure/azure-iot-device-management
[lnk-tutorial-jobs]: iot-hub-schedule-jobs.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md

[lnk-devtwin]: iot-hub-devguide-device-twins.md
[lnk-c2dmethod]: iot-hub-devguide-direct-methods.md
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx