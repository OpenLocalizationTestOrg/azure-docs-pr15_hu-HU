<properties
 pageTitle="Feladatok ütemezéséről |} Microsoft Azure"
 description="Ebből az oktatóanyagból megtudhatja, hogy miként feladatok ütemezése"
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

# <a name="tutorial-schedule-and-broadcast-jobs-preview"></a>Oktatóprogram: Ütemezése, és közvetítése feladatok (előzetes verzió)

## <a name="introduction"></a>– Bevezetés
Azure IoT központi teljes körű felügyelt szolgáltatás, amely lehetővé teszi, hogy a hozhat létre és nyomon követheti a feladatok ütemezése, és frissítse a eszközök milliónyi alkalmazás vissza vége.  Feladatok használható a következő műveleteket:

- Eszköz kívánt kettős tulajdonságainak módosítása
- Eszköz kettős címkék frissítése
- Felhőalapú-eszköz módszerek meghívása

A feladat ebben az értelemben fussa az alábbi műveletek egyikét, és összehasonlítja az eszközök, a kettős lekérdezés által megadott végrehajtási végrehajtásának nyomon követi.  Például feladat használatával alkalmazás vissza vége hívhatja indítsa újra a rendszert a módszer kettős lekérdezés által megadott és a későbbi időpontban ütemezett 10 000 eszközökön.  Az alkalmazás is majd nyomon kövesse ezeket eszközökhöz fogadása, és hajtsa végre az újraindítás módszer.

További e ezekre a lehetőségekre, az alábbi cikkekben talál:

- Eszköz kettős és tulajdonságok: [első lépések a kettős] [ lnk-get-started-twin] és [oktatóprogram: kettős tulajdonságok használata][lnk-twin-props]
- Felhőalapú-eszköz módszerek: [Fejlesztőeszközök útmutató – a közvetlen módszerek] [ lnk-dev-methods] és [oktatóprogram: C2D módszerek][lnk-c2d-methods]

Ebből az oktatóanyagból megtudhatja, hogyan szeretné:

- Hozzon létre egy szimulált eszközt, egy közvetlen módszer, amely lehetővé teszi a lockDoor, amely hívható vissza az alkalmazás által befejezési amelynek.
- Amely felhívja a lockDoor közvetlen módszer feladat használatával szimulált eszközön, és a feladat eszköz használatával kívánt kettős tulajdonságok frissítése console-alkalmazás létrehozása.

Ez az oktatóanyag végén alkalmazások vannak telepítve a két Node.js konzol:

**simDevice.js**, amelyben az eszköz identitással IoT-hubhoz csatlakozik, és kap egy lockDoor közvetlen módszer.

**scheduleJobService.js**, amely felhívja a közvetlen módszer szimulált az eszközre, és frissítse a kettős disired tulajdonságait feladat használatával.

Ebben az oktatóanyagban a következőkre lesz szüksége:

NODE.js verzió 0.12.x vagy újabb <br/>  [Felkészülés a fejlesztői környezet] [ lnk-dev-setup] ismerteti, hogyan lehet az ebben az oktatóanyagban Node.js telepítése Windows vagy Linux rendszerhez.

Azure active fiók. (Nem rendelkeznek fiókkal, ha egy [ingyenes fiókra] hozhat létre[ lnk-free-trial] a mindössze néhány perc.)

[AZURE.INCLUDE [iot-hub-get-started-create-hub-pp](../../includes/iot-hub-get-started-create-hub-pp.md)]

## <a name="create-a-simulated-device-app"></a>Szimulált eszközön alkalmazás létrehozása

Ebben a részben hoz létre hívja meg a felhőben, amely elindítja a szimulált eszközön újra kell indítani a közvetlen módszer válaszoló Node.js konzol alkalmazást, és használja az eszköz kettős jelenteni tulajdonságok ahhoz, hogy eszköz kettős lekérdezések azonosítja az eszközén, és amikor azok az utolsó újraindítása után.

1. Hozzon létre egy új üres nevű **simDevice**.  A **simDevice** mappába az alábbi parancs használatával a parancssorban package.json fájl létrehozása.  Fogadja el az alapértelmezett beállításokat:

    ```
    npm init
    ```
    
2. A-parancssorban a **simDevice** mappában, telepítse az **azure-iot-eszköz** eszköz SDK és **azure-iot-eszköz-mqtt** csomagja a következő parancs futtatásával:

    ```
    npm install azure-iot-device@dtpreview azure-iot-device-mqtt@dtpreview --save
    ```

3. Egy egyszerű szövegszerkesztő programban, hoz létre új **simDevice.js** fájl **simDevice** mappában.

4. Adja meg a következő 'szükséges' kimutatások **simDevice.js** fájl elején:

    ```
    'use strict';

    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    ```

5. **ConnectionString** változó hozzáadása, és hozzon létre egy eszköz ügyfél használatával.  

    ```
    var connectionString = 'HostName={youriothostname};DeviceId={yourdeviceid};SharedAccessKey={yourdevicekey}';
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```

6. Adja hozzá a következő függvény a lockDoor módszer kezelése.

    ```
    var onLockDoor = function(request, response) {
        
        // Respond the cloud app for the direct method
        response.send(200, function(err) {
            if (!err) {
                console.error('An error occured when sending a method response:\n' + err.toString());
            } else {
                console.log('Response to method \'' + request.methodName + '\' sent successfully.');
            }
        });
        
        console.log('Locking Door!');
    };
    ```

7. Adja hozzá a következő kódot regisztrálhatja a kezelő lockDoor mód.

    ```
    client.open(function(err) {
        if (err) {
            console.error('Could not connect to IotHub client.');
        }  else {
            console.log('Client connected to IoT Hub.  Waiting for reboot direct method.');
            client.onDeviceMethod('lockDoor', onLockDoor);
        }
    });
    ```
    
8. Mentse és zárja be a **simDevice.js** fájlt.

> [AZURE.NOTE] Ahhoz, hogy egyszerű dolgot, ebben az oktatóanyagban nem valósítja bármely újrapróbálkozási házirend. Gyártási kódban végre kell hajtania újrapróbálkozási házirendek (például egy exponenciális visszalépési), az MSDN-cikk [Tranziens hibafa kezelése]a javasolt[lnk-transient-faults].

## <a name="schedule-jobs-for-calling-a-direct-method-and-updating-a-twins-properties"></a>Hívja fel a közvetlen módszer, és a tulajdonságok egy kettős frissítésére feladatok ütemezése

Ebben a részben egy távoli lockDoor közvetlen módszerrel eszközön kezdeményező Node.js konzol alkalmazás létrehozása, és frissítse a kettős tulajdonságait.

1. Hozzon létre egy új üres nevű **scheduleJobService**.  A **scheduleJobService** mappába az alábbi parancs használatával a parancssorban package.json fájl létrehozása.  Fogadja el az alapértelmezett beállításokat:

    ```
    npm init
    ```
    
2. A-parancssorban a **scheduleJobService** mappában, telepítse az **azure-iothub** eszköz SDK és **azure-iot-eszköz-mqtt** csomagja a következő parancs futtatásával:

    ```
    npm install azure-iothub@dtpreview uuid --save
    ```
    
3. Egy egyszerű szövegszerkesztő programban, hoz létre új **scheduleJobService.js** fájl **scheduleJobService** mappában.

4. Adja meg a következő 'szükséges' kimutatások **dmpatterns_gscheduleJobServiceetstarted_service.js** fájl elején:

    ```
    'use strict';

    var uuid = require('uuid');
    var JobClient = require('azure-iothub').JobClient;
    ```

5. Adja hozzá a következő változó nyilatkozatokat, és cserélje le a helyőrző értékeket:

    ```
    var connectionString = '{iothubconnectionstring}';
    var deviceArray = ['myDeviceId'];
    var startTime = new Date();
    var maxExecutionTimeInSeconds =  3600;
    var jobClient = JobClient.fromConnectionString(connectionString);
    ```
    
6. Adja hozzá a következő függvénynek figyelheti a feladat végrehajtására használt:

    ```
    function monitorJob (jobId, callback) {
        var jobMonitorInterval = setInterval(function() {
            jobClient.getJob(jobId, function(err, result) {
            if (err) {
                console.error('Could not get job status: ' + err.message);
            } else {
                console.log('Job: ' + jobId + ' - status: ' + result.status);
                if (result.status === 'completed' || result.status === 'failed' || result.status === 'cancelled') {
                clearInterval(jobMonitorInterval);
                callback(null, result);
                }
            }
            });
        }, 5000);
    }
    ```

7. Adja hozzá a következő kódot, amely felhívja a eszköz módszer a feladat ütemezése:

    ```
    var methodParams = {
        methodName: 'lockDoor',
        payload: null,
        timeoutInSeconds: 45
    };

    var methodJobId = uuid.v4();
    console.log('scheduling Device Method job with id: ' + methodJobId);
    jobClient.scheduleDeviceMethod(methodJobId,
                                deviceArray,
                                methodParams,
                                startTime,
                                maxExecutionTimeInSeconds,
                                function(err) {
        if (err) {
            console.error('Could not schedule device method job: ' + err.message);
        } else {
            monitorJob(methodJobId, function(err, result) {
                if (err) {
                    console.error('Could not monitor device method job: ' + err.message);
                } else {
                    console.log(JSON.stringify(result, null, 2));
                }
            });
        }
    });
    ```
        
8. Adja hozzá a következő kódot a kettős frissítése a feladat ütemezése:

    ```
    var twinPatch = {
        etag: '*',
        desired: {
            building: '43',
            floor: 3
        }
    };

    var twinJobId = uuid.v4();

    console.log('scheduling Twin Update job with id: ' + twinJobId);
    jobClient.scheduleTwinUpdate(twinJobId,
                                deviceArray,
                                twinPatch,
                                startTime,
                                maxExecutionTimeInSeconds,
                                function(err) {
        if (err) {
            console.error('Could not schedule twin update job: ' + err.message);
        } else {
            monitorJob(twinJobId, function(err, result) {
                if (err) {
                    console.error('Could not monitor twin update job: ' + err.message);
                } else {
                    console.log(JSON.stringify(result, null, 2));
                }
            });
        }
    });
    ```
    
9. Mentse és zárja be a **scheduleJobService.js** fájlt.

## <a name="run-the-applications"></a>Az alkalmazások futtatása

Most már készen áll az alkalmazások futtatásához.

1. A parancssorablakban – a **simDevice** mappában, futtassa a következő parancsot, indítsa újra a rendszert közvetlen mód figyel megkezdéséhez.

    ```
    node simDevice.js
    ```

2. A parancssorba- **scheduleJobService** mappában futtassa a következő parancsot a távoli újraindítás és lekérdezése az eszköz kettős keresése az utolsó indítsa újra az idő eseményindító.

    ```
    node scheduleJobService.js
    ```

3. A kimenet eszköz és a hátsó vége alkalmazások jelenik meg.


## <a name="next-steps"></a>Következő lépések

Ebben az oktatóanyagban használva egy projekt ütemezése egy eszközt, és a frissítést az eszköz kettős tulajdonságok közvetlen módszert.

Továbbra is, és első lépések IoT központi eszköz management mintázatok, például a távoli fölé a air frissítést, olvassa el:

[Oktatóprogram: Hogyan lehet a belső vezérlőprogram frissítése][lnk-fwupdate]

Első lépések – IoT központi folytatásához lásd: [a IoT átjáró SDK – első lépések][lnk-gateway-SDK].

[lnk-get-started-twin]: iot-hub-node-node-twin-getstarted.md
[lnk-twin-props]: iot-hub-node-node-twin-how-to-configure.md
[lnk-c2d-methods]: iot-hub-c2d-methods.md
[lnk-dev-methods]: iot-hub-devguide-direct-methods.md
[lnk-fwupdate]: iot-hub-firmware-update.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx