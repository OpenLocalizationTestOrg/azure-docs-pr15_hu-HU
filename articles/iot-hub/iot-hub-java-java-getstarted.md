<properties
    pageTitle="Első lépések Java Azure IoT központi |} Microsoft Azure"
    description="Az első Java Azure IoT központi oktatóprogram lépések. Használjon Azure IoT központi és Java a Microsoft Azure IoT SDK az Internet a dolog, amit megoldás végrehajtásához."
    services="iot-hub"
    documentationCenter="java"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="java"
     ms.topic="hero-article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="08/11/2016"
     ms.author="dobett"/>

# <a name="get-started-with-azure-iot-hub-for-java"></a>Azure IoT központi Java – első lépések

[AZURE.INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

Ez az oktatóanyag végén három Java konzol alkalmazások vannak telepítve:

* **Hozzon létre eszköz-azonosító**, amely hoz létre egy eszköz identitás- és a szimulált eszköze társított biztonsági kulcs.
* **üzenetek olvasása d2c**, amely a szimulált eszköze által küldött telemetriai jeleníti meg.
* **szimulált eszköz**, amely a korábban létrehozott eszköz identitású a IoT-hubhoz csatlakozik, és telemetriai üzenetet küld, minden második használata a AMQP Protocol (protokoll).

> [AZURE.NOTE] A cikk [IoT központi SDK] [ lnk-hub-sdks] a különböző SDK mindkét alkalmazás futtatásához az eszközén, és a megoldás vissza vége létrehozásához használható információt tartalmaz.

Ebben az oktatóanyagban a következőkre lesz szüksége:

+ Java-bA 8. <br/> [Felkészülés a fejlesztői környezet] [ lnk-dev-setup] ismerteti, hogyan lehet az ebben az oktatóanyagban Java telepítése Windows vagy Linux rendszerhez.

+ 3 maven tesztelése.  <br/> [Felkészülés a fejlesztői környezet] [ lnk-dev-setup] ismerteti, hogyan lehet az ebben az oktatóanyagban maven tesztelése telepítése Windows vagy Linux rendszerhez.

+ Azure active fiók. (Nem rendelkeznek fiókkal, ha egy [ingyenes fiókra] hozhat létre[ lnk-free-trial] a mindössze néhány perc.)

[AZURE.INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

Utolsó lépésként jegyezze fel az **elsődleges kulcs** értékét, és kattintson az **üzenetkezelés**. Az **üzenetkezelés** lap jegyezze fel egy **esemény központi-kompatibilis nevét** és az **esemény központi-kompatibilis végpontot**. Az **üzenetek olvasása d2c** alkalmazás létrehozásakor szüksége ezt a három értéket.

![Azure portál IoT központi üzenetküldés lap][6]

Most már létrehozott a IoT-központját, és rendelkezik IoT központi hostname (állomásnév), IoT központi kapcsolati karakterláncot, IoT központi elsődleges kulcs, esemény központi-kompatibilis nevét, és ebben az oktatóanyagban szükséges esemény központi-kompatibilis végpontot.

## <a name="create-a-device-identity"></a>Egy eszköz azonosító létrehozása

Ebben a részben hoz létre, amely a IoT-központban identitás beállításjegyzékbeli hoz létre egy új eszköz identitás Java konzol alkalmazást. Egy eszközt IoT központi eszköz identitás beállításjegyzékbeli bejegyzés mindaddig nem tud csatlakozni. A **Eszköz identitás beállításjegyzék** szakaszban a [IoT központi Fejlesztőeszközök útmutató] [ lnk-devguide-identity] további információt. A New futtatásakor egy egyedi Eszközazonosítót és az eszköz segítségével azonosítja magát, amikor eszköz a felhőbe üzeneteket küld IoT központi kulcsot hoz létre.

1. Hozzon létre egy új üres nevű iot java-get-indított. Iot java-get-indított mappában maven tesztelése **létrehozása eszköz-Identitáskezelés** az alábbi parancs használatával a parancssorban nevű új projekt létrehozása Megjegyzés: Ez egy egyetlen, hosszú parancsot:

    ```
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=create-device-identity -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. A parancssorablakban nyissa meg az új mappa létrehozása eszköz-identitás.

3. Szövegszerkesztőben használja, nyissa meg a pom.xml fájlt a létrehozás-eszköz-identitás mappát, és a következő típusú függés hozzáadása a **függőségeket** csomópontot. Ez lehetővé teszi, hogy az alkalmazás az iothub-szolgáltatás – sdk csomag használata:

    ```
    <dependency>
      <groupId>com.microsoft.azure.iothub-java-client</groupId>
      <artifactId>iothub-java-service-client</artifactId>
      <version>1.0.9</version>
    </dependency>
    ```
    
4. Mentse és zárja be a pom.xml fájlt.

5. Szövegszerkesztővel, nyissa meg a create-device-identity\src\main\java\com\mycompany\app\App.java fájlt.

6. A következő **importálása** kimutatások hozzáadása a fájlhoz:

    ```
    import com.microsoft.azure.iot.service.exceptions.IotHubException;
    import com.microsoft.azure.iot.service.sdk.Device;
    import com.microsoft.azure.iot.service.sdk.RegistryManager;

    import java.io.IOException;
    import java.net.URISyntaxException;
    ```

7. A következő osztály szintű változók hozzáadása az **alkalmazás** osztály, helyettesítve a **{yourhubconnectionstring}** az érték a feljegyzett régebbi verzióiban:

    ```
    private static final String connectionString = "{yourhubconnectionstring}";
    private static final String deviceId = "myFirstJavaDevice";
    
    ```
    
8. Módosíthatja az aláírását, a **fő** módszer kivételek megadása az alábbiak szerint:

    ```
    public static void main( String[] args ) throws IOException, URISyntaxException, Exception
    ```
    
9. A következő kód hozzáadása a **fő** módszer szövegtörzseként. Ez a kód létrehoz egy eszközt a IoT központi identitás beállításjegyzékben nevű *javadevice* , ha még nem létezik. Ezt követően megjeleníti a eszközazonosítót és billentyűt, hogy később szükség lenne:

    ```
    RegistryManager registryManager = RegistryManager.createFromConnectionString(connectionString);

    Device device = Device.createFromId(deviceId, null, null);
    try {
      device = registryManager.addDevice(device);
    } catch (IotHubException iote) {
      try {
        device = registryManager.getDevice(deviceId);
      } catch (IotHubException iotf) {
        iotf.printStackTrace();
      }
    }
    System.out.println("Device id: " + device.getDeviceId());
    System.out.println("Device key: " + device.getPrimaryKey());
    ```

10. Mentse és zárja be a App.java fájlt.

11. Összeállítása maven tesztelése **létrehozása-eszköz-identitás** alkalmazást, hajtsa végre a következő parancsot a parancssorablakban-létrehozása-eszköz-identitás mappában:

    ```
    mvn clean package -DskipTests
    ```

12. Maven tesztelése használó **létrehozása-eszköz-identitás** alkalmazás futtatásához hajtsa végre a következő parancsot a parancssorablakban-létrehozása-eszköz-identitás mappában:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

13. Jegyezze le a **eszközazonosítót** és **eszköz billentyűt**. Szüksége lesz a későbbi eszközként IoT-hubhoz csatlakozik, az alkalmazások létrehozásakor.

> [AZURE.NOTE] A IoT központi identitás beállításjegyzék csak a központi biztonságos hozzáférés engedélyezése eszköz identitások tárolja. Tárolja az azonosítók eszköztől és a hitelesítő adatokat, és engedélyezett/letiltott jelző is használhatja az egyes eszköz hozzáférés letiltása használható billentyűparancsokról. Ha az alkalmazás más eszköz-specifikus metaadatokat tárolhatnak, akkor az alkalmazás-specifikus tárolót kell használni. [IoT központi Fejlesztőeszközök] útmutató[ lnk-devguide-identity] további információt.

## <a name="receive-device-to-cloud-messages"></a>Eszköz a felhőbe hibaüzenetek

Ebben a részben hoz létre, amely beolvassa az eszköz a felhőbe üzenetek IoT központból Java konzol alkalmazást. Egy IoT központi közzététele egy [Központi esemény][lnk-event-hubs-overview]-kompatibilis végpont ahhoz, hogy az eszköz a felhőbe üzenetek olvasása. Ahhoz, hogy egyszerű dolgot, ebben az oktatóprogramban egy egyszerű olvasó, amely nem megfelelő magas átviteli telepítés hoz létre. A [folyamat eszköz a felhőbe üzenetek] [ lnk-process-d2c-tutorial] testre szabhatja a Méretezés eszköz a felhőbe üzenetek feldolgozása. Az [Első lépések az esemény hubok] [ lnk-eventhubs-tutorial] oktatóanyag további információt biztosít folyamat során az esemény hubok, és a IoT központi esemény központi-kompatibilis végpontok vonatkoznak.

> [AZURE.NOTE] Az esemény központi-kompatibilis végpontot, az eszköz a felhőbe üzenetek olvasása mindig a AMQP protokollt használja.

1. A mappában iot java-get-indított *létrehozása egy eszköz identitás* szakaszban létrehozott az **üzenetek olvasása d2c** a következő parancsot a parancssorban nevű új maven tesztelése projektet létrehozni. Megjegyzés: Ez egy egyetlen, hosszú parancsot:

    ```
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=read-d2c-messages -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. A parancssorablakban nyissa meg az új üzenetek olvasása d2c mappát.

3. Egy egyszerű szövegszerkesztő programban használ, a pom.xml az üzenetek olvasása d2c mappában található fájlra, és a következő típusú függés hozzáadása a **függőségeket** csomópontot. Ez lehetővé teszi, hogy az alkalmazás eventhubs ügyfélcsomag segítségével az esemény központi-kompatibilis végpontot olvassák:

    ```
    <dependency> 
        <groupId>com.microsoft.azure</groupId> 
        <artifactId>azure-eventhubs</artifactId> 
        <version>0.8.2</version> 
    </dependency>
    ```

4. Mentse és zárja be a pom.xml fájlt.

5. Szövegszerkesztővel, nyissa meg a read-d2c-messages\src\main\java\com\mycompany\app\App.java fájlt.

6. A következő **importálása** kimutatások hozzáadása a fájlhoz:

    ```
    import java.io.IOException;
    import com.microsoft.azure.eventhubs.*;
    import com.microsoft.azure.servicebus.*;
    
    import java.io.IOException;
    import java.nio.charset.Charset;
    import java.time.*;
    import java.util.Collection;
    import java.util.concurrent.ExecutionException;
    import java.util.function.*;
    import java.util.logging.*;
    ```

7. Az **alkalmazás** osztály adja hozzá a következő osztály szintű változók. **{Youriothubkey}**, **{youreventhubcompatibleendpoint}**és **{youreventhubcompatiblename}** cserélje ki a korábban említettük értékeket:

    ```
    private static String connStr = "Endpoint={youreventhubcompatibleendpoint};EntityPath={youreventhubcompatiblename};SharedAccessKeyName=iothubowner;SharedAccessKey={youriothubkey}";
    ```

8. A következő **receiveMessages** módszer hozzáadása az **alkalmazás** osztály. Ez a módszer létrehoz egy **EventHubClient** példány csatlakozhat az esemény központi-kompatibilis végpontot, és aszinkron létrehoz egy **PartitionReceiver** példány egy esemény központi partíciót olvasásakor. Folyamatosan hurkok azt, és az üzenet részletei nyomtat, mindaddig, amíg az alkalmazás leállása.

    ```
    private static EventHubClient receiveMessages(final String partitionId)
    {
      EventHubClient client = null;
      try {
        client = EventHubClient.createFromConnectionStringSync(connStr);
      }
      catch(Exception e) {
        System.out.println("Failed to create client: " + e.getMessage());
        System.exit(1);
      }
      try {
        client.createReceiver( 
          EventHubClient.DEFAULT_CONSUMER_GROUP_NAME,  
          partitionId,  
          Instant.now()).thenAccept(new Consumer<PartitionReceiver>()
        {
          public void accept(PartitionReceiver receiver)
          {
            System.out.println("** Created receiver on partition " + partitionId);
            try {
              while (true) {
                Iterable<EventData> receivedEvents = receiver.receive(100).get();
                int batchSize = 0;
                if (receivedEvents != null)
                {
                  for(EventData receivedEvent: receivedEvents)
                  {
                    System.out.println(String.format("Offset: %s, SeqNo: %s, EnqueueTime: %s", 
                      receivedEvent.getSystemProperties().getOffset(), 
                      receivedEvent.getSystemProperties().getSequenceNumber(), 
                      receivedEvent.getSystemProperties().getEnqueuedTime()));
                    System.out.println(String.format("| Device ID: %s", receivedEvent.getProperties().get("iothub-connection-device-id")));
                    System.out.println(String.format("| Message Payload: %s", new String(receivedEvent.getBody(),
                      Charset.defaultCharset())));
                    batchSize++;
                  }
                }
                System.out.println(String.format("Partition: %s, ReceivedBatch Size: %s", partitionId,batchSize));
              }
            }
            catch (Exception e)
            {
              System.out.println("Failed to receive messages: " + e.getMessage());
            }
          }
        });
      }
      catch (Exception e)
      {
        System.out.println("Failed to create receiver: " + e.getMessage());
      }
      return client;
    }
    ```

    > [AZURE.NOTE] Ez a módszer szűrő használ, amikor létrehozza a címzett, hogy a címzett csak olvassa be a címzett indítása futtatása után IoT központi küldött üzeneteket. Ez akkor hasznos, tesztkörnyezetben, így láthatja, hogy az üzenetek aktuális készlete. A kód győződjön meg arról, hogy üzemi környezetben, hogy minden az üzenetek feldolgozásával – lásd: a [IoT központi eszköz a felhőbe üzenetek feldolgozási módjának] [ lnk-process-d2c-tutorial] oktatóanyag további információt.

9. Módosítsa az aláírást a **fő** módszer a kivétel felvenni az alábbi képlettel történik:

    ```
    public static void main( String[] args ) throws IOException
    ```

10. A következő kód hozzáadása a **fő** módszer az **alkalmazás** osztály. Ez a kód hoz létre a két **EventHubClient** és **PartitionReceiver** -példányok, és lehetővé teszi, hogy az alkalmazás bezárásához, ha befejezte az üzenetek feldolgozásával:

    ```
    EventHubClient client0 = receiveMessages("0");
    EventHubClient client1 = receiveMessages("1");
    System.out.println("Press ENTER to exit.");
    System.in.read();
    try
    {
      client0.closeSync();
      client1.closeSync();
      System.exit(0);
    }
    catch (ServiceBusException sbe)
    {
      System.exit(1);
    }
    ```

    > [AZURE.NOTE] Ez a kód feltételezi, hogy az F1 (ingyenes) réteg hozta létre a IoT-központját. Egy ingyenes IoT hubhoz két partíciók, "0" és "1" nevű fájlt.

11. Mentse és zárja be a App.java fájlt.

12. Az **üzenetek olvasása d2c** alkalmazást maven tesztelése össze, hajtsa végre a következő parancsot a parancssorablakban-az üzenetek olvasása d2c mappában:

    ```
    mvn clean package -DskipTests
    ```

## <a name="create-a-simulated-device-app"></a>Szimulált eszközön alkalmazás létrehozása

Ebben a részben hozzon létre egy konzol Java-alkalmazást, amely eszköz a felhőbe üzeneteket küld egy IoT központi eszközt.

1. A mappában iot java-get-indított *létrehozása egy eszköz identitás* szakaszban létrehozott **szimulált eszköz** a következő parancs használatával a parancssorban nevű új maven tesztelése projektet létrehozni. Megjegyzés: Ez egy egyetlen, hosszú parancsot:

    ```
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=simulated-device -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. A parancssorablakban nyissa meg az új szimulált eszköz mappára.

3. Szövegszerkesztővel, nyissa meg a pom.xml fájlt a szimulált eszköz mappára, és a következő függőségeket hozzáadása a **függőségeket** csomópontot. Ez lehetővé teszi, hogy az alkalmazás iothub-java-ügyfélcsomag használatával kommunikál a IoT-központját, és szerializálni JSON Java objektumok használata:

    ```
    <dependency>
      <groupId>com.microsoft.azure.iothub-java-client</groupId>
      <artifactId>iothub-java-device-client</artifactId>
      <version>1.0.14</version>
    </dependency>
    <dependency>
      <groupId>com.google.code.gson</groupId>
      <artifactId>gson</artifactId>
      <version>2.3.1</version>
    </dependency>
    ```

4. Mentse és zárja be a pom.xml fájlt.

5. Szövegszerkesztővel, nyissa meg a simulated-device\src\main\java\com\mycompany\app\App.java fájlt.

6. A következő **importálása** kimutatások hozzáadása a fájlhoz:

    ```
    import com.microsoft.azure.iothub.DeviceClient;
    import com.microsoft.azure.iothub.IotHubClientProtocol;
    import com.microsoft.azure.iothub.Message;
    import com.microsoft.azure.iothub.IotHubStatusCode;
    import com.microsoft.azure.iothub.IotHubEventCallback;
    import com.microsoft.azure.iothub.IotHubMessageResult;
    import com.google.gson.Gson;
    import java.io.IOException;
    import java.net.URISyntaxException;
    import java.util.Random;
    import java.util.concurrent.Executors;
    import java.util.concurrent.ExecutorService;
    ```

7. A következő osztály szintű változók hozzáadása az **alkalmazás** osztály, **{youriothubname}** helyettesítve a IoT központi nevét, és a **{yourdevicekey}** hozza létre, ha az *eszköz megjelenést alakíthat* szakasz eszköz kulcs értékét:

    ```
    private static String connString = "HostName={youriothubname}.azure-devices.net;DeviceId=myFirstJavaDevice;SharedAccessKey={yourdevicekey}";
    private static IotHubClientProtocol protocol = IotHubClientProtocol.AMQP;
    private static String deviceId = "myFirstJavaDevice";
    private static DeviceClient client;
    ```

    Ez elindítja a **DeviceClient** objektum minta alkalmazás használja a **protokoll** változó. HTTP vagy AMQP protokoll használatával kommunikál a IoT központi is használhatja.

8. Adja meg a következő egymásba ágyazott **TelemetryDataPoint** osztály kezdése az **App** osztály adja meg az eszköz küld a IoT központi telemetriai adatokat:

    ```
    private static class TelemetryDataPoint {
      public String deviceId;
      public double windSpeed;
        
      public String serialize() {
        Gson gson = new Gson();
        return gson.toJson(this);
      }
    }
    ```

9. Adja hozzá a következő beágyazott **EventCallback** osztály kezdése az **App** osztály nyugtázó állapota, amely a IoT központi ad vissza, ha egy üzenet az szimulált eszközről feldolgozásával. Ezt a módszert is értesíti az alkalmazásban a fő szál, amikor az üzenet feldolgozása megtörtént:

    ```
    private static class EventCallback implements IotHubEventCallback
    {
      public void execute(IotHubStatusCode status, Object context) {
        System.out.println("IoT Hub responded to message with status: " + status.name());
      
        if (context != null) {
          synchronized (context) {
            context.notify();
          }
        }
      }
    }
    ```

10. Adja hozzá a következő beágyazott **MessageSender** osztály kezdése az **App** osztály. Az osztály **futtatása** módszer minta telemetriai adatokat szeretne küldeni a IoT központi hoz létre, és megvárja, amíg a következő üzenet elküldése előtt visszaigazolást:

    ```
    private static class MessageSender implements Runnable {
      public volatile boolean stopThread = false;
      
      public void run()  {
        try {
          double avgWindSpeed = 10; // m/s
          Random rand = new Random();
          
          while (!stopThread) {
            double currentWindSpeed = avgWindSpeed + rand.nextDouble() * 4 - 2;
            TelemetryDataPoint telemetryDataPoint = new TelemetryDataPoint();
            telemetryDataPoint.deviceId = deviceId;
            telemetryDataPoint.windSpeed = currentWindSpeed;
            
            String msgStr = telemetryDataPoint.serialize();
            Message msg = new Message(msgStr);
            System.out.println("Sending: " + msgStr);
            
            Object lockobj = new Object();
            EventCallback callback = new EventCallback();
            client.sendEventAsync(msg, callback, lockobj);
            
            synchronized (lockobj) {
              lockobj.wait();
            }
            Thread.sleep(1000);
          }
        } catch (InterruptedException e) {
          System.out.println("Finished.");
        }
      }
    }
    ```

    Ez a módszer új eszköz a felhőbe üzenetet küld egy másodperc után a IoT központi nyugtázza az előző üzenetre. Az üzenet a deviceId és a szél sebesség érzékelő hasonlóan tízjegyű szám JSON szerializáltként objektumot tartalmaz.

11. A **fő** módszer cserélje ki az alábbi kódot, amely egy eszközt a felhőbe üzeneteket küldeni a IoT központi szál hoz létre:

    ```
    public static void main( String[] args ) throws IOException, URISyntaxException {
      client = new DeviceClient(connString, protocol);
      client.open();

      MessageSender sender = new MessageSender();

      ExecutorService executor = Executors.newFixedThreadPool(1);
      executor.execute(sender);

      System.out.println("Press ENTER to exit.");
      System.in.read();
      executor.shutdownNow();
      client.close();
    }
    ```

12. Mentse és zárja be a App.java fájlt.

13. Összeállítása maven tesztelése **szimulált eszköz** alkalmazást, hajtsa végre a következő parancsot a parancssorablakban – a szimulált eszköz mappában:

    ```
    mvn clean package -DskipTests
    ```

> [AZURE.NOTE] Ahhoz, hogy egyszerű dolgot, ebben az oktatóanyagban nem valósítja bármely újrapróbálkozási házirend. Gyártási kódban végre kell hajtania újrapróbálkozási házirendek (például egy exponenciális visszalépési), az MSDN-cikk [Tranziens hibafa kezelése]a javasolt[lnk-transient-faults].

## <a name="run-the-applications"></a>Az alkalmazások futtatása

Most már készen áll az alkalmazások futtatásához.

1. A parancssorban az olvasás-d2c mappában a következő parancsot a kezdéshez figyelése a első partíciót a IoT-központban:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![Java IoT központi szolgáltatás ügyfélalkalmazás eszköz a felhőbe üzenetek figyelése][7]

2. Parancssorba – a szimulált eszköz mappában a következő parancsot a kezdéshez telemetriai adatokat küld a IoT központi:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App" 
    ```

    ![Java IoT központi eszköz ügyfélalkalmazás eszköz a felhőbe üzenetek küldéséhez][8]

3. A **használati** csempére az [Azure portál] [ lnk-portal] a központi küldött üzenetek számát jeleníti meg:

    ![Azure portál használatát csempe megjelenítő küldött üzenetek száma IoT hubhoz][43]

## <a name="next-steps"></a>Következő lépések

Ebben az oktatóprogramban egy új IoT hubhoz konfigurálni az Azure-portálon, és hozza létre a eszköz identitás a központi identitás beállításjegyzékben. Az eszköz identitás használt ahhoz, hogy az eszköz a felhőbe üzeneteket küldeni a központi szimulált eszközön alkalmazás. Az alkalmazás, amely megjeleníti az üzenetek, a központi megkapta is létrehozott. 

Továbbra is az első lépések – IoT központi és feltárása más IoT esetben talál:

- [Csatlakozás az eszközön][lnk-connect-device]
- [Első lépések a mobileszköz-kezelés][lnk-device-management]
- [Az átjáró IoT SDK – első lépések][lnk-gateway-SDK]

A IoT megoldás kiterjesztése és dolgozza eszköz a felhőbe üzeneteket a méretezés című témakörben talál a [folyamat eszköz a felhőbe üzenetek] [ lnk-process-d2c-tutorial] oktatóprogram.

<!-- Images. -->
[6]: ./media/iot-hub-java-java-getstarted/create-iot-hub6.png
[7]: ./media/iot-hub-java-java-getstarted/runapp1.png
[8]: ./media/iot-hub-java-java-getstarted/runapp2.png
[43]: ./media/iot-hub-java-java-getstarted/usage.png

<!-- Links -->
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-eventhubs-tutorial]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[lnk-devguide-identity]: iot-hub-devguide-identity-registry.md
[lnk-event-hubs-overview]: ../event-hubs/event-hubs-overview.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/java-devbox-setup.md
[lnk-process-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

[lnk-device-management]: iot-hub-device-management-get-started.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/