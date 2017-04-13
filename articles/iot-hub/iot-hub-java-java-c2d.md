<properties
    pageTitle="IoT központi üzenetek cloud-eszköz |} Microsoft Azure"
    description="Ebből az oktatóanyagból megtudhatja, hogy miként Azure IoT elosztót használ Java cloud-eszköz üzenetek küldéséhez kövesse."
    services="iot-hub"
    documentationCenter="java"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="java"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/13/2016"
     ms.author="dobett"/>

# <a name="tutorial-how-to-send-cloud-to-device-messages-with-iot-hub-and-java"></a>Oktatóprogram: Hogyan IoT központi és Java cloud-eszköz üzenet küldése

[AZURE.INCLUDE [iot-hub-selector-c2d](../../includes/iot-hub-selector-c2d.md)]

## <a name="introduction"></a>– Bevezetés

Azure IoT központi, amely segít teljes körű felügyelt szolgáltatás engedélyezése a megbízható és nem biztonságos kétirányú kommunikációt milliónyi IoT eszközök között, és a Befejezés vissza egy alkalmazást. Az [első lépések a IoT központi] szabhatja hozzon létre egy IoT hubhoz, rajta egy eszköz identitás kiépítése és kód eszköz a felhőbe üzeneteket küld szimulált eszközt.

Ez az oktatóanyag [első]lépések a IoT központi hoz létre. Hogyan azt mutatja, hogy:

- Végéről az alkalmazás felhő vissza üzenetküldés cloud-eszköz IoT hubon keresztül egyetlen eszközre.
- Egy eszközön cloud-eszköz üzeneteket fogadni.
- Az alkalmazás felhőből vége biztonsági, (*Visszajelzés*) kézbesítési visszaigazolás kérése egy eszközt IoT központból küldött üzenetek.

Talál további információt a felhőben-eszköz üzenetek a [IoT központi Developer Guide][IoT Hub Developer Guide - C2D].

Ez az oktatóanyag végén két Java konzol alkalmazások futtatása:

* **szimulált eszköz**, az alkalmazás [IoT központi – első lépések], amelyek a IoT hubhoz csatlakozik, és felhőalapú-eszköz üzenetek fogadása létrehozott módosított verzióját.
* az **üzenetek küldése c2d**cloud-eszköz üzenetet küld a szimulált eszközön IoT hubon keresztül, és ezután kap a kézbesítési visszaigazolás.

> [AZURE.NOTE] IoT hubhoz számos eszközt platformokon és nyelvek (többek között, C, Java és Javascript) keresztül Azure IoT eszköz SDK SDK támogatása. Ebben az oktatóanyagban kód csatlakoztatni az eszközt, és általában Azure IoT hubon keresztül csatlakozott, lásd: az [Azure IoT Developer Center]részletes útmutatást.

Ebben az oktatóanyagban a következőkre lesz szüksége:

+ Java-bA 8. <br/> [Felkészülés a fejlesztői környezet] [ lnk-dev-setup] ismerteti, hogyan lehet az ebben az oktatóanyagban Java telepítése Windows vagy Linux rendszerhez.

+ 3 maven tesztelése.  <br/> [Felkészülés a fejlesztői környezet] [ lnk-dev-setup] ismerteti, hogyan lehet az ebben az oktatóanyagban maven tesztelése telepítése Windows vagy Linux rendszerhez.

+ Azure active fiók. (Nem rendelkeznek fiókkal, ha egy [ingyenes fiókra] hozhat létre[ lnk-free-trial] a mindössze néhány perc.)

## <a name="receive-messages-on-the-simulated-device"></a>Hibaüzenetek szimulált az eszközre

Ebben a részben szimulált eszköz alkalmazása módosíthatja az a felhő-eszköz üzenetek fogadására a IoT központból [IoT központi – első lépések] létrehozott.

1. Szövegszerkesztővel, nyissa meg a simulated-device\src\main\java\com\mycompany\app\App.java fájlt.

2. Vegye fel az alábbi **MessageCallback** osztály kezdése az **App** osztály beágyazott osztály. A **végrehajtás** módszer nyílik meg, ha az eszköz üzenetet kap IoT központból. Ebben a példában az eszköz minden esetben értesítést küld a központi, hogy befejeződött-e az üzenet:

    ```
    private static class MessageCallback implements
    com.microsoft.azure.iothub.MessageCallback {
      public IotHubMessageResult execute(Message msg, Object context) {
        System.out.println("Received message from hub: "
          + new String(msg.getBytes(), Message.DEFAULT_IOTHUB_MESSAGE_CHARSET));

        return IotHubMessageResult.COMPLETE;
      }
    }
    ```

3. Hozzon létre egy **MessageCallback** példányt, és hívja fel a **setMessageCallback** módszer, mielőtt a következőképpen nyílik meg az ügyfél **fő** módszer módosítása:

    ```
    client = new DeviceClient(connString, protocol);

    MessageCallback callback = new MessageCallback();
    client.setMessageCallback(callback, null);
    client.open();
    ```

    > [AZURE.NOTE] HTTP MQTT vagy AMQP helyett a szállítás használja, ha a **DeviceClient** példány központból IoT ritkán (legfeljebb 25 percenként) üzeneteket keres. MQTT, a AMQP és a HTTP-támogatási és IoT központi szabályozásának közötti különbségekről olvashat, lásd: a [IoT központi Fejlesztőeszközök útmutató][IoT Hub Developer Guide - C2D].

## <a name="send-a-cloud-to-device-message"></a>Üzenet küldése egy felhőalapú-eszköz

Ebben a részben hoz létre, amely cloud-eszköz üzeneteket küld a szimulált eszközön alkalmazás Java konzol alkalmazást. Az eszközre az eszköz az [első lépések a IoT központi] oktatóprogram során felvett Id szükséges. A IoT központi megtalálható az [Azure portálon]is kell a kapcsolati karakterlánc.

1. Az **üzenetek küldése c2d** a következő parancsot a parancssorban nevű maven tesztelése projektet létrehozni. Megjegyzés: Ez egy egyetlen, hosszú parancsot:

    ```
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=send-c2d-messages -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. A parancssorablakban nyissa meg az új üzenetek küldése c2d mappát.

3. Egy egyszerű szövegszerkesztő programban használ, a pom.xml az üzenetek küldése c2d mappában található fájlra, és a következő típusú függés hozzáadása a **függőségeket** csomópontot. A függőséget hozzáadásával lehetővé teszi, hogy az alkalmazás **iothub-java-szolgáltatás -** ügyfélcsomag használva kommunikálhat a IoT központi szolgáltatásban:

    ```
    <dependency>
      <groupId>com.microsoft.azure.iothub-java-client</groupId>
      <artifactId>iothub-java-service-client</artifactId>
      <version>1.0.7</version>
    </dependency>
    ```

4. Mentse és zárja be a pom.xml fájlt.

5. Szövegszerkesztővel, nyissa meg a send-c2d-messages\src\main\java\com\mycompany\app\App.java fájlt.

6. A következő **importálása** kimutatások hozzáadása a fájlhoz:

    ```
    import com.microsoft.azure.iot.service.sdk.*;
    import java.io.IOException;
    import java.net.URISyntaxException;
    ```

7. A következő osztály szintű változók hozzáadása az **alkalmazás** osztály, helyettesítve a **{yourhubconnectionstring}** , **{yourdeviceid}** és az értékeket a feljegyzett régebbi verzióiban:

    ```
    private static final String connectionString = "{yourhubconnectionstring}";
    private static final String deviceId = "{yourdeviceid}";
    private static final IotHubServiceClientProtocol protocol = IotHubServiceClientProtocol.AMQP;
    ```
    
8. A **fő** módszer cserélje ki az alábbi kódot, amely a IoT-hubhoz csatlakozik, üzenetet küld az eszköz és várakozik, hogy az eszköz kapott, és az üzenet feldolgozott nyugtát:

    ```
    public static void main(String[] args) throws IOException,
        URISyntaxException, Exception {
      ServiceClient serviceClient = ServiceClient.createFromConnectionString(
        connectionString, protocol);
      
      if (serviceClient != null) {
        serviceClient.open();
        FeedbackReceiver feedbackReceiver = serviceClient
          .getFeedbackReceiver(deviceId);
        if (feedbackReceiver != null) feedbackReceiver.open();

        Message messageToSend = new Message("Cloud to device message.");
        messageToSend.setDeliveryAcknowledgement(DeliveryAcknowledgement.Full);

        serviceClient.send(deviceId, messageToSend);
        System.out.println("Message sent to device");

        FeedbackBatch feedbackBatch = feedbackReceiver.receive(10000);
        if (feedbackBatch != null) {
          System.out.println("Message feedback received, feedback time: "
            + feedbackBatch.getEnqueuedTimeUtc().toString());
        }

        if (feedbackReceiver != null) feedbackReceiver.close();
        serviceClient.close();
      }
    }
    ```

    > [AZURE.NOTE] Ebben az oktatóprogramban az egyszerűség 's rizspálinkát, nem hajtja végre bármely újrapróbálkozási házirend. A gyártási kódot végre kell hajtania a újrapróbálkozási házirendek (például exponenciális visszalépési), az [Ideiglenes (tranziens) hiba kezelésének]MSDN-cikk javasolt.

## <a name="run-the-applications"></a>Az alkalmazások futtatása

Most már készen áll az alkalmazások futtatásához.

1. Parancssorba – a szimulált eszköz mappában a következő parancsot a kezdéshez telemetriai küld a IoT-központját, és a központból küldött cloud-eszköz üzenetek meghallgatása:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App" 
    ```

    ![Futtassa az szimulált eszközön alkalmazást][img-simulated-device]

2. A parancssorban az üzenetek küldése c2d mappában futtassa a felhőben-eszköz üzenetet küldeni, és várja meg a visszajelzési nyugta a következő parancsot:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![Futtassa a parancsot a felhőben-eszköz üzenet küldése][img-send-command]

## <a name="next-steps"></a>Következő lépések

Ebben az oktatóanyagban cloud-eszköz üzenetek küldése és fogadása hogyan megtanulta azt. 

Példák a teljes végpont megoldások, amelyek IoT központi talál további [Azure IoT csomagot].

IoT központi megoldások fejlesztésével kapcsolatos további információért lásd: a [IoT központi Fejlesztőeszközök útmutató].


<!-- Images -->
[img-simulated-device]: media/iot-hub-java-java-c2d/receivec2d.png
[img-send-command]:  media/iot-hub-java-java-c2d/sendc2d.png
<!-- Links -->

[Első lépések a IoT központi]: iot-hub-java-java-getstarted.md
[IoT Hub Developer Guide - C2D]: iot-hub-devguide-messaging.md
[IoT központi Fejlesztőeszközök útmutató]: iot-hub-devguide.md
[Azure IoT Developer Center]: http://www.azure.com/develop/iot
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/java-devbox-setup.md
[Ideiglenes (tranziens) hibafa kezelése]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[Azure portál]: https://portal.azure.com
[Azure IoT programcsomagban]: https://azure.microsoft.com/documentation/suites/iot-suite/