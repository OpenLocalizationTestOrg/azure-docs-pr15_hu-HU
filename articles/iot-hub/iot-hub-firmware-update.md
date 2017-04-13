<properties
 pageTitle="A belső vezérlőprogram frissítésének módjáról |} Microsoft Azure"
 description="Ebből az oktatóanyagból megtudhatja, hogy miként végezze el a belső vezérlőprogram frissítése"
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

# <a name="tutorial-how-to-do-a-firmware-update-preview"></a>Oktatóprogram: Hogyan lehet egy belső vezérlőprogram frissítés (előzetes verzió)

## <a name="introduction"></a>– Bevezetés
Az [első lépések a mobileszköz-kezelés] [ lnk-dm-getstarted] oktatóanyag használatáról az [eszközhöz kettős] látta[ lnk-devtwin] és a [felhő-eszköz (C2D) módszerek] [ lnk-c2dmethod] primitívek való távolról indítsa újra az eszközt. Ebben az oktatóanyagban az ugyanazon IoT központi primitívek használ, és nyújt útmutatást, és megtudhatja, hogy miként végezze el a végpontok közötti szimulált belső vezérlőprogram frissítés.  A minta belső vezérlőprogram frissítés végrehajtása során az Intel Edison eszköz minta használatos.

Ebből az oktatóanyagból megtudhatja, hogyan szeretné:

- A firmwareUpdate közvetlen módszer felhívja a IoT központi keresztül szimulált eszközön console-alkalmazás létrehozása.
- Hozzon létre egy firmwareUpdate közvetlen módszer, amely egy többszakaszos folyamatát, töltse le a belső vezérlőprogram képet vár, letölti a belső vezérlőprogram képet hajtja végre, és végül alkalmazza az adik belső vezérlőprogram kép szimulált eszközt.  Minden egyes szakaszban az eszközt használ, a kettős jelentett tulajdonságok frissítése végrehajtása során haladnia.

Ez az oktatóanyag végén alkalmazások vannak telepítve a két Node.js konzol:

**dmpatterns_fwupdate_service.js**, amely felhívja a közvetlen módszer szimulált az eszközre, jeleníti meg a választ, és rendszeres időközönként (minden 500ms) jeleníti meg a frissített eszköz kettős jelenteni tulajdonságait.

**dmpatterns_fwupdate_device.js**, amely a korábban létrehozott eszköz identitású kapcsolódik a IoT hubon keresztül csatlakozott, kap egy firmwareUpdate közvetlen módszer, hasonlóan egy belső vezérlőprogram frissítés többek között a több államot eljárással fut: a kép letöltés vár, az új kép letöltéséről és végül alkalmazása a képre.


Ebben az oktatóanyagban a következőkre lesz szüksége:

NODE.js verzió 0.12.x vagy újabb <br/>  [Felkészülés a fejlesztői környezet] [ lnk-dev-setup] ismerteti, hogyan lehet az ebben az oktatóanyagban Node.js telepítése Windows vagy Linux rendszerhez.

Azure active fiók. (Nem rendelkeznek fiókkal, ha egy [ingyenes fiókra] hozhat létre[ lnk-free-trial] a mindössze néhány perc.)

Kövesse a [mobileszköz-kezelés – első lépések](iot-hub-device-management-get-started.md) a cikkben a IoT központi létrehozása és a kapcsolati karakterlánc.

[AZURE.INCLUDE [iot-hub-get-started-create-hub-pp](../../includes/iot-hub-get-started-create-hub-pp.md)]

## <a name="create-a-simulated-device-app"></a>Szimulált eszközön alkalmazás létrehozása

Ebben a részben hoz létre egy közvetlen módszer hívja meg a felhőben, amely elindítja a szimulált eszközön belső vezérlőprogram frissítésének válaszoló Node.js konzol alkalmazást, és használja az eszköz kettős jelenteni tulajdonságok ahhoz, hogy eszköz kettős lekérdezések azonosítja az eszközén, és amikor azok az utolsó újraindítása után.

1. Hozzon létre egy új üres nevű **manageddevice**.  A **manageddevice** mappába az alábbi parancs használatával a parancssorban package.json fájl létrehozása.  Fogadja el az alapértelmezett beállításokat:

    ```
    npm init
    ```
    
2. Parancssorba a- **manageddevice** mappában, a következő parancsot telepítheti a **azure-iot-device@dtpreview** eszköz SDK csomag és **azure-iot-device-mqtt@dtpreview** csomag:

    ```
    npm install azure-iot-device@dtpreview azure-iot-device-mqtt@dtpreview --save
    ```

3. Egy egyszerű szövegszerkesztő programban, hoz létre új **dmpatterns_fwupdate_device.js** fájl **manageddevice** mappában.

4. Adja meg a következő 'szükséges' kimutatások **dmpatterns_fwupdate_device.js** fájl elején:

    ```
    'use strict';

    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    ```

5. **ConnectionString** változó hozzáadása, és hozzon létre egy eszköz ügyfél használatával.  

    ```
    var connectionString = 'HostName={youriothostname};DeviceId=myDeviceId;SharedAccessKey={yourdevicekey}';
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```

6. Adja hozzá a következő kettős frissítése használt függvény jelentett tulajdonságai

    ```
    var reportFWUpdateThroughTwin = function(twin, firmwareUpdateValue) {
      var patch = {
          iothubDM : {
            firmwareUpdate : firmwareUpdateValue
          }
      };

      twin.properties.reported.update(patch, function(err) {
        if (err) throw err;
        console.log('twin state reported')
      });
    };
    ```
    
7. Adja hozzá a következő függvények, amely a letöltés szimulálhatja és a belső vezérlőprogram kép alkalmazása.

    ```
    var simulateDownloadImage = function(imageUrl, callback) {
      var error = null;
      var image = "[fake image data]";
      
      console.log("Downloading image from " + imageUrl);
      
      callback(error, image);
    }

    var simulateApplyImage = function(imageData, callback) {
      var error = null;
      
      if (!imageData) {
        error = {message: 'Apply image failed because of missing image data.'};
      }
      
      callback(error);
    }
    ```

8. Adja hozzá a következő függvény, amely frissíti a belső vezérlőprogram frissítési állapotát a kettős keresztül tulajdonságok jelentett kell letölteni.  Általában eszközök kapjanak avaiable frissítés, és a rendszergazda definiált házirend okok az eszköz letöltéséről és alkalmazásáról a frissítés indításához.  Ez az, hogy hol kell ahhoz, hogy házirend logika futtatni.  Az egyszerűség kedvéért azt használja a 4 másodpercig késleltetése, és töltse le a belső vezérlőprogram képet irányuló eljárás. 

    ```
    var waitToDownload = function(twin, fwPackageUriVal, callback) {
      var now = new Date();
      
      reportFWUpdateThroughTwin(twin, {
        fwPackageUri: fwPackageUriVal,
        status: 'waiting',
        error : null,
        startedWaitingTime : now.toISOString()
      });
      setTimeout(callback, 4000);
    };
    ```
    
9. Adja hozzá a következő függvény, amely frissíti a belső vezérlőprogram frissítési állapotát a kettős keresztül tulajdonságok jelentett a belső vezérlőprogram kép letöltése.  Azt, amelyek egy belső vezérlőprogram letöltési követi, és végül frissíti a letöltés sikeres vagy sikertelen tájékoztatja belső vezérlőprogram frissítési állapotát.

    ```
    var downloadImage = function(twin, fwPackageUriVal, callback) {
      var now = new Date();   
      
      reportFWUpdateThroughTwin(twin, {
        status: 'downloading',
      });
      
      setTimeout(function() {
        // Simulate download
        simulateDownloadImage(fwPackageUriVal, function(err, image) {
          
          if (err)
          {
            reportFWUpdateThroughTwin(twin, {
              status: 'downloadfailed',
              error: {
                code: error_code,
                message: error_message,
              }
            });
          }
          else {        
            reportFWUpdateThroughTwin(twin, {
              status: 'downloadComplete',
              downloadCompleteTime: now.toISOString(),
            });
            
            setTimeout(function() { callback(image); }, 4000);   
          }
        });
        
      }, 4000);
    }
    ```
    
10. Adja hozzá a következő függvény, amely frissíti a belső vezérlőprogram frissítési állapotát a kettős keresztül tulajdonságok jelentett alkalmazása a belső vezérlőprogram képet.  Azt, amelyek egy alkalmazása a belső vezérlőprogram kép követi, és végül frissíti az alkalmazás sikeres vagy sikertelen tájékoztatja belső vezérlőprogram frissítési állapotát.

    ```
    var applyImage = function(twin, imageData, callback) {
      var now = new Date();   
      
      reportFWUpdateThroughTwin(twin, {
        status: 'applying',
        startedApplyingImage : now.toISOString()
      });
      
      setTimeout(function() {
        
        // Simulate apply firmware image
        simulateApplyImage(imageData, function(err) {
          if (err) {
            reportFWUpdateThroughTwin(twin, {
              status: 'applyFailed',
              error: {
                code: err.error_code,
                message: err.error_message,
              }
            });
          } else { 
            reportFWUpdateThroughTwin(twin, {
              status: 'applyComplete',
              lastFirmwareUpdate: now.toISOString()
            });    
            
          }
        });
        
        setTimeout(callback, 4000);
        
      }, 4000);
    }
    ```

11. Adja hozzá a következő functoin firmwareUpdate módszer kezelje, és a többszakaszos belső vezérlőprogram frissítés megkezdéséhez.

    ```
    var onFirmwareUpdate = function(request, response) {
            
      // Respond the cloud app for the direct method
      response.send(200, 'FirmwareUpdate started', function(err) {
        if (!err) {
          console.error('An error occured when sending a method response:\n' + err.toString());
        } else {
          console.log('Response to method \'' + request.methodName + '\' sent successfully.');
        }
      });

      // Get the parameter from the body of the method request
      var fwPackageUri = JSON.parse(request.payload).fwPackageUri;

      // Obtain the device twin
      client.getTwin(function(err, twin) {
        if (err) {
          console.error('Could not get device twin.');
        } else {
          console.log('Device twin acquired.');
          
          // Start the multi-stage firmware update
          waitToDownload(twin, fwPackageUri, function() {
            downloadImage(twin, fwPackageUri, function(imageData) {
              applyImage(twin, imageData, function() {});    
            });  
          });

        }
      });
    }
    ```
    
12. Végül adja hozzá a következő kódot, amely egy eszközként IoT-hubhoz csatlakozik 

    ```
    client.open(function(err) {
      if (err) {
        console.error('Could not connect to IotHub client');
      }  else {
        console.log('Client connected to IoT Hub.  Waiting for firmwareUpdate direct method.');
      }
      
      client.onDeviceMethod('firmwareUpdate', onFirmwareUpdate(request, response));
    });
    ```

> [AZURE.NOTE] Ahhoz, hogy egyszerű dolgot, ebben az oktatóanyagban nem valósítja bármely újrapróbálkozási házirend. Gyártási kódban végre kell hajtania újrapróbálkozási házirendek (például egy exponenciális visszalépési), az MSDN-cikk [Tranziens hibafa kezelése]a javasolt[lnk-transient-faults].

## <a name="trigger-a-remote-firmware-update-on-the-device-using-a-direct-method"></a>Az elindítani a távoli vezérlőprogram-frissítést az eszközre a közvetlen módszerének alkalmazásával. 

Ebben a részben hoz létre, amely elindítja a távoli vezérlőprogram-frissítést közvetlen módszerrel eszközön eszköz kettős lekérdezések használó rendszeres állapotuk az aktív belső vezérlőprogram frissítésének eszközön futó Node.js konzol alkalmazást.


1. Hozzon létre egy új üres nevű **triggerfwupdateondevice**.  A **triggerfwupdateondevice** mappába az alábbi parancs használatával a parancssorban package.json fájl létrehozása.  Fogadja el az alapértelmezett beállításokat:

    ```
    npm init
    ```
    
2. A-parancssorban a **triggerfwupdateondevice** mappában, telepítse az **azure-iothub** eszköz SDK és **azure-iot-eszköz-mqtt** csomagja a következő parancs futtatásával:

    ```
    npm install azure-iot-hub@dtpreview --save
    ```
    
3. Egy egyszerű szövegszerkesztő programban, hoz létre új **dmpatterns_getstarted_service.js** fájl **triggerfwupdateondevice** mappában.

4. Adja meg a következő 'szükséges' kimutatások **dmpatterns_getstarted_service.js** fájl elején:

    ```
    'use strict';

    var Registry = require('azure-iothub').Registry;
    var Client = require('azure-iothub').Client;
    ```

5. Adja hozzá a következő változó nyilatkozatokat, és cserélje le a helyőrző értékeket:

    ```
    var connectionString = '{device_connectionstring}';
    var registry = Registry.fromConnectionString(connectionString);
    var client = Client.fromConnectionString(connectionString);
    var deviceToUpdate = 'myDeviceId';
    ```
    
6. Adja hozzá a következő függvény keresése, és a firmwareUpdate értékének megjelenítési jelentett tulajdonság.

    ```
    var queryTwinFWUpdateReported = function() {
        registry.getTwin(deviceToUpdate, function(err, twin){
            if (err) {
              console.error('Could not query twins: ' + err.constructor.name + ': ' + err.message);
            } else {
              console.log((JSON.stringify(twin.properties.reported.iothubDM.firmwareUpdate)) + "\n");
            }
        });
    };
    ```

7. Adja hozzá a következő függvény az firmwareUpdate módszert, indítsa újra a célként megadott eszköz meghívásához:

    ```
    var startFirmwareUpdateDevice = function() {
      var params = {
          fwPackageUri: 'https://secureurl'
      };
      
      var methodName = "firmwareUpdate";
      var payloadData =  JSON.stringify(params);
      
      var methodParams = {
        methodName: methodName,
        payload: payloadData,
        timeoutInSeconds: 30
      };
      
      client.invokeDeviceMethod(deviceToUpdate, methodParams, function(err, result) {
        if (err) {
          console.error('Could not start the firmware update on the device: ' + err.message)
        } 
      });
    };
    ```

8. Végül vegye fel a következő kódot a belső vezérlőprogram indításához függvény sorrendjének módosítása, és indítsa el a rendszeres megjelenítő a kettős jelzett tulajdonságok:

    ```
    startFirmwareUpdateDevice();
    setInterval(queryTwinFWUpdateReported, 500);
    ```
    
9. Mentse és zárja be a **dmpatterns_fwupdate_service.js** fájlt.

## <a name="run-the-applications"></a>Az alkalmazások futtatása

Most már készen áll az alkalmazások futtatásához.

1. A parancssorablakban – a **manageddevice** mappában, futtassa a következő parancsot, indítsa újra a rendszert közvetlen mód figyel megkezdéséhez.

    ```
    node dmpatterns_fwupdate_device.js
    ```

2. A parancssorba- **triggerfwupdateondevice** mappában futtassa a következő parancsot a távoli újraindítás és lekérdezése az eszköz kettős keresése az utolsó indítsa újra az idő eseményindító.

    ```
    node dmpatterns_fwupdate_service.js
    ```

3. Ekkor megjelenik a közvetlen módszerrel reagálásra kinyomtatásával meg az üzenet

## <a name="next-steps"></a>Következő lépések

Ebben az oktatóanyagban használt közvetlen módszer szeretne elindítani egy távoli belső vezérlőprogram frissítés eszközön, és a kettős jelentett rendszeresen használja a belső vezérlőprogram végrehajtásának megértéséhez tulajdonságainak módosítása folyamat.  

Megtudhatja, hogy miként, ha ki szeretné terjeszteni a IoT megoldás és az ütemezés módja a hívások több eszközön, olvassa el a [ütemezése és közvetítési feladatok] [ lnk-tutorial-jobs] oktatóprogram.

[lnk-devtwin]: iot-hub-devguide-device-twins.md
[lnk-c2dmethod]: iot-hub-devguide-direct-methods.md
[lnk-dm-getstarted]: iot-hub-device-management-get-started.md
[lnk-tutorial-jobs]: iot-hub-schedule-jobs.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
