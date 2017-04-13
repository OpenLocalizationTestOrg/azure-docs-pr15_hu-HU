<properties
    pageTitle="Listaszerű folyamatábra IoT központi eszköz a felhőbe üzenetek (Java) |} Microsoft Azure"
    description="Kövesse a Java oktatóanyag további hasznos mintázatok IoT központi eszköz a felhőbe üzenetek feldolgozása."
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
     ms.date="09/01/2016"
     ms.author="dobett"/>

# <a name="tutorial-how-to-process-iot-hub-device-to-cloud-messages-using-java"></a>Oktatóprogram: Hogyan IoT központi eszköz a felhőbe az üzenetek Java feldolgozása

[AZURE.INCLUDE [iot-hub-selector-process-d2c](../../includes/iot-hub-selector-process-d2c.md)]

## <a name="introduction"></a>– Bevezetés

Azure IoT központi egy teljes körű felügyelt szolgáltatás, amely lehetővé teszi a megbízható és biztonságos kétirányú kommunikációt, több millió IoT eszközök és az alkalmazások között biztonsági célból. További oktatóanyagok ([IoT központi – első lépések] és [IoT központi cloud-eszköz üzenet küldése][lnk-c2d]) megtudhatja, hogy miként alapvető eszköz a felhőbe, és a felhő-eszköz üzenetben funkcióit IoT központi használni.

Ebben az oktatóanyagban épül az [első lépések a IoT központi] oktatóprogram kódot, és azt mutatja, hogy két méretezhető mintázatok eszköz a felhőbe üzenetek feldolgozása használható:

- Az eszköz a felhőbe üzenetek [Azure blob-tárolóban]lévő megbízható tárhely. A leggyakoribb helyzet *hideg elérési út* analytics, telemetriai adatokat a bevitt analytics folyamatok használandó BLOB tárolására szolgáló. Ezek a folyamatok [Azure Data Factory] vagy a [HDInsight (Hadoop)] Papírhalom powerview eszközzel alapú is lehet.

- *Interaktív* eszköz a felhőbe üzenetek megbízható feldolgozása. Eszköz a felhőbe üzenetek interaktívak azonnali indítók a beállított a alkalmazás háttér-műveletek közül. Például egy eszközt előfordulhat, hogy küldése riasztás kiváltó jegy beszúrásával egy CRM rendszerrel. Ezzel ellentétben *adatpont* üzenetek egyszerűen-hírcsatorna-elemző motor. Például a hőmérsékleti telemetriai újabb elemzéshez tárolni eszközről egy adatpontra üzenetet is.

Mivel a IoT központi közzététele egy [Központi esemény][lnk-event-hubs]-kompatibilis eszköz a felhőbe-üzenetek fogadására alkalmas, ez az oktatóanyag használ egy [EventProcessorHost] példány végpontot. Ebben az esetben:

* Biztos, hogy tárolja *az adatpont* üzenetek Azure blob-tárolóhoz.
* *Interaktív* eszköz a felhőbe üzeneteket továbbít az Azure [Service Bus várólista] azonnali feldolgozásra.

Szolgáltatás Bus biztosítja megbízható interaktív az üzenetek feldolgozásával üzenet pontok és idő ablak alapú megszüntetése ismétlődések biztosít.

> [AZURE.NOTE] Egy **EventProcessorHost** példány feldolgozni interaktív üzenetek csak egy lehetőség. Egyéb lehetőségek [Azure Service háló] [ lnk-service-fabric] és [Azure Értékáram-elemzés][lnk-stream-analytics].

Ez az oktatóanyag végén Java-konzol három alkalmazást futtatja:

* **szimulált eszköz**, az alkalmazás az [első lépések a IoT központi] oktatóprogram során létre módosított verzióját adatpont eszköz a felhőbe üzeneteket küld minden második és interaktív eszköz a felhőbe üzenetek 10 másodpercenként. Az alkalmazás a IoT központi kommunikálni az AMQP protokollt használja.
* **Folyamatábra-d2c-üzenetek** használja a [EventProcessorHost] osztály üzenetek lekérése az esemény központi-kompatibilis végpontot. Azt, majd biztos, hogy tárolja az adatpont üzenetek Azure blob-tárolóban lévő, és továbbítja szolgáltatás Bus várólista interaktív üzeneteket.
* **Folyamatábra-interaktív üzenetek** vonja vissza a szolgáltatás Bus sorból az interaktív üzenetek várakozási sorba.

> [AZURE.NOTE] IoT hubhoz számos eszközt platformokon és nyelvek, többek között a C, Java és JavaScript SDK támogatása. Hogyan kell a szimulált eszközön ebben az oktatóanyagban cserélje a fizikai eszközök, és hogyan csatlakoztassa az eszközöket IoT-hubon keresztül csatlakozott, tanulmányozza az [Azure IoT Developer Center].

Ebben az oktatóanyagban olyan közvetlenül alkalmazható más módon használhatnak esemény központi-kompatibilis üzeneteket, például a projektek [HDInsight (Hadoop)] . További információ a [Azure IoT központi Fejlesztőeszközök útmutató - eszköz cloud]című témakörben.

Ebben az oktatóanyagban a következőkre lesz szüksége:

+ Az [első lépések a IoT központi] oktatóprogram teljes munkaidő verziója.

+ Java-bA 8. <br/> [Felkészülés a fejlesztői környezet] [ lnk-dev-setup] ismerteti, hogyan lehet az ebben az oktatóanyagban Java telepítése Windows vagy Linux rendszerhez.

+ 3 maven tesztelése.  <br/> [Felkészülés a fejlesztői környezet] [ lnk-dev-setup] ismerteti, hogyan lehet az ebben az oktatóanyagban maven tesztelése telepítése Windows vagy Linux rendszerhez.

+ Azure active fiók. <br/>Nem rendelkeznek fiókkal, ha mindössze néhány perc is létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free/) .

Rendelkeznie kell néhány [Azure-tárhely] és [Azure Service Bus]alapismeretei.


## <a name="send-interactive-messages-from-a-simulated-device"></a>Interaktív küldésére a szimulált eszközről

Ebben a részben módosítsa az interaktív eszköz a felhőbe üzeneteket küldeni a IoT központi [IoT központi – első lépések] oktatóanyagban hoztunk szimulált eszközön alkalmazást.

1. Egy egyszerű szövegszerkesztő programban segítségével a simulated-device\src\main\java\com\mycompany\app\App.java fájlra. Ez a fájl az [első lépések a IoT központi] oktatóanyagban hoztunk **szimulált eszköz** alkalmazás kódot tartalmazza.

2. Az alábbi beágyazott osztály hozzáadása az **alkalmazás** osztály:

    ```
    private static class InteractiveMessageSender implements Runnable {
      public void run() {
        try {
          while (true) {
            String msgStr = "Alert message!";
            Message msg = new Message(msgStr);
            msg.setMessageId(java.util.UUID.randomUUID().toString());
            msg.setProperty("messageType", "interactive");
            System.out.println("Sending interactive message: " + msgStr);

            Object lockobj = new Object();
            EventCallback callback = new EventCallback();
            client.sendEventAsync(msg, callback, lockobj);

            synchronized (lockobj) {
              lockobj.wait();
            }
            Thread.sleep(10000);
          }
        } catch (InterruptedException e) {
          System.out.println("Finished sending interactive messages.");
        }
      }
    }
    ```

    Az osztály hasonlít a **szimulált eszköz** projekt **MessageSender** osztály. Csak az, hogy most már a tulajdonság akkor **MessageId** rendszer, és az egyéni tulajdonság neve **messageType**.
    A kód rendel a **MessageId** tulajdonság egy univerzális egyedi azonosító (UUID). A szolgáltatás Bus az azonosító segítségével vonja ismétlődő fogadott üzeneteket. A példa a interaktív adatpont mailekből megkülönböztetni a **messageType** tulajdonság használja. A alkalmazástól kapott üzenet tulajdonságai, ezt az információt helyett az üzenet törzsébe, hogy az esemény processzor nem szükséges, az üzenet végrehajtani üzenet továbbítása deszerializálni.

    > [AZURE.NOTE] Fontos, hogy a használt eszköz kód interaktív üzenetek vonja ismétlődő **MessageId** létrehozása. Szakaszos hálózati kommunikáció vagy más hibák, ugyanazt az üzenetet, az adott eszközről több Újraküldés eredményezhet. Szemantikai Üzenetazonosító, például a megfelelő üzenet adatmezők helyett UUID kivonat is használhatja.

3. Módosítsa a **fő** módszert mindkét interaktív üzenetek küldése és adatpont üzeneteket a következő kódrészletet látható módon:

    ````
    MessageSender sender = new MessageSender();
    InteractiveMessageSender interactiveSender = new InteractiveMessageSender();

    ExecutorService executor = Executors.newFixedThreadPool(2);
    executor.execute(sender);
    executor.execute(interactiveSender);
    ````

4. Mentse és zárja be a simulated-device\src\main\java\com\mycompany\app\App.java fájlt.

    > [AZURE.NOTE] Ebben az oktatóprogramban az egyszerűség kedvéért nem hajtja végre bármely újrapróbálkozási házirend. A gyártási kódot végre kell hajtania a exponenciális visszalépési, [Ideiglenes (tranziens) hiba kezelésének]MSDN című témakörben javasolt, például egy újrapróbálkozási házirend.

5. Összeállítása maven tesztelése **szimulált eszköz** alkalmazást, hajtsa végre a következő parancsot a parancssorablakban – a szimulált eszköz mappában:

    ```
    mvn clean package -DskipTests
    ```

## <a name="process-device-to-cloud-messages"></a>A folyamat eszköz a felhőbe üzenetek

Ebben a részben hoz létre, amely IoT központból eszköz a felhőbe üzenetek feldolgozásával Java konzol alkalmazást. IOT központi közzététele [esemény központi]-kompatibilis végpont ahhoz, hogy az eszköz a felhőbe üzenetek olvasása alkalmazások. Ebben az oktatóanyagban használja a [EventProcessorHost] osztály folyamat az alábbi üzenetek console-alkalmazásban. Az esemény hubok folyamat során használatáról további tudnivalókért lásd: az [Első lépések az esemény hubok] oktatóprogram.

A fő beavatkozás igazolására szolgáló eljárás megvalósítása adatpont üzenetek megbízható tárolására és interaktív az üzenetek továbbítása, hogy az esemény feldolgozása támaszkodik az üzenet fogyasztói pontjainak nyújt a meghívási folyamat. Ezenkívül egy nagy teljesítmény eléréséhez, amikor, olvassa el az esemény hubok meg kell adnia a nagy kötegekben pontjainak. Ezzel a módszerrel hoz létre ismétlődő feldolgozásban egy nagy mennyiségű üzenetet, ha a hibát, és visszaállítja az előző ellenőrzés lehetőségét. Ebben az oktatóanyagban, megtudhatja, hogy miként Azure tároló írások és szolgáltatás Bus megszüntetése ismétlődések windows szinkronizálása **EventProcessorHost** pontjainak.

Biztos, hogy Azure tárolóhoz írhat üzeneteket, a mintában a [Továbbfejlesztett fájlblokkolás BLOB]egyes blokk jóváhagyás szolgáltatását használja[Azure Block Blobs]. Az esemény processzor összegzi az üzeneteket a memóriában mindaddig, amíg az ideje egy ellenőrzés megadását. Ha például a halmozott puffer üzenetek eléri 4 MB-os maximális méretét, illetve után a szolgáltatás Bus megszüntetése ismétlődések időkeret telik. Ezután az ellenőrzés, mielőtt a kód véglegesíti egy új szövegrészt a blob szeretne.

Az esemény processzor üzenet eltolja esemény hubok használja a azonosítók parancsot. Ez az eljárás lehetővé teszi, hogy az esemény processzor azt az új blokk véglegesíti tárolóhoz, ügyelve között egy szövegrészt, és az ellenőrzés elvégzése a lehetséges összeomlik, mielőtt a megszüntetése ismétlődés ellenőrzése végrehajtásához.

> [AZURE.NOTE] Ebben az oktatóanyagban IoT központból beolvasott összes üzenetet írhat egyetlen Azure tároló fiókot használ. Szükségességének használni a megoldás több Azure tárterület-fiókot, olvassa el az [Azure tároló méretezhetőség irányelveket].

Az alkalmazás elkerülése érdekében az ismétlődő elemeket, amikor interaktív üzenetek feldolgozásával szolgáltatás Bus megszüntetése ismétlődések szolgáltatását használja. A szimulált eszközön minden interaktív üzenet az egy egyedi **MessageId**ellátja időbélyeggel. Az azonosító szolgáltatás Bus annak érdekében, hogy a megadott megszüntetése ismétlődések idő ablakában nem két üzenetek az azonos **MessageId** a kézbesítési a vevők. A megszüntetése másolást, az üzenet teljesítése szemantikáját szolgáltatás Bus sorok, által biztosított együtt egyszerűen interaktív üzenetek megbízható feldolgozásának végrehajtásához.

Győződjön meg arról, hogy nincs üzenet kívül a megszüntetése ismétlődések ablakban van-e küldeni, a kód szinkronizálja a **EventProcessorHost** ellenőrzés mechanizmusa a szolgáltatás Bus várakozási sor megszüntetése ismétlődések ablak. A szinkronizálás végzi a ellenőrzés legalább egyszer kényszerítése, minden alkalommal, amikor a program a megszüntetése ismétlődések ablak (ebben az oktatóprogramban az egy órával).

> [AZURE.NOTE] Ebben az oktatóprogramban egy adott particionált szolgáltatás Bus sorba az interaktív üzenetek beolvasott IoT központból folyamat használja. A megoldás méretezhetőség követelményeknek szolgáltatás Bus sorban várakozó használatáról további információt az [Azure Service Bus] dokumentációjában olvasható.

### <a name="provision-an-azure-storage-account-and-a-service-bus-queue"></a>Azure tárterület-fiók és egy szolgáltatás Bus várólista kiépítése

A [EventProcessorHost] osztály használatához rendelkeznie ahhoz, hogy a **EventProcessorHost** ellenőrzés az adatok rögzítéséhez Azure tároló fiók. Egy meglévő Azure tárterület-fiókot használ, vagy kövesse az utasításokat [Azure-tárolóban] lévő hozzon létre egy újat. Jegyezze fel az Azure tároló fiók kapcsolati karakterláncot.

> [AZURE.NOTE] Másolja a vágólapra, és illessze be az Azure tároló fiók kapcsolati karakterláncot, ügyeljen az nem tartalmaz szóközöket.

A szolgáltatás Bus várólista interaktív üzenetek megbízható feldolgozásának engedélyezése is szükséges. Létrehozhat egy várólista programozás útján, egy órával megszüntetése ismétlődések ablakot,[szolgáltatás Bus várólista] [szolgáltatást Bus sorban várakozó használata]című cikkben ismertetett módon. Másik lehetőségként használhatja az [Azure klasszikus portál][lnk-classic-portal], ezeket a lépéseket követve:

1. A bal alsó sarkában kattintson az **Új** gombra. Kattintson a **Szolgáltatások alkalmazás** > **Szolgáltatás Bus** > **várólista** > **Egyéni létrehozása**. Írja be a név **d2ctutorial**, jelöljön ki egy területet, és egy meglévő névtér használata, vagy hozzon létre egy újat. Jegyezze le a névtér neve, később az oktatóprogram szükséges. A következő lapon jelölje be **a duplikált elemek észlelése engedélyezése**jelölőnégyzetet, és a **Duplikálás észlelési előzmények időkeret** beállítása egy óra. Kattintson a jobb alsó sarokban a várólista konfiguráció mentése a pipa ikonra.

    ![Várólista létrehozása az Azure-portálon][30]

2. A szolgáltatás Bus sorok listájában kattintson a **d2ctutorial**, és kattintson a **beállítás**. Hozzon létre két megosztott access-házirendek, egy úgynevezett **küldése** **küldése** engedélyekkel rendelkező, és egy úgynevezett **meghallgatása** **meghallgatása** engedélyekkel. Jegyezze fel mindkét házirendek az **elsődleges kulcs** értékeit, az oktatóprogram később szükség szerint csoportokat. Ha végzett, kattintson a **Mentés** gombra a képernyő alján.

    ![Az Azure-portálon várólista konfigurálása][31]

### <a name="create-the-event-processor"></a>Az esemény processzor létrehozása

Ebben a részben az esemény központi-kompatibilis végpontot a folyamat során Java-alkalmazás létrehozása.

Az első tevékenység hozzáadása a nevű **folyamat-d2c-üzenetek** eszköz a felhőbe üzenetet kap a IoT központi esemény központi-kompatibilis végpont és más háttéradatbázist szolgáltatások továbbítja azokat az üzeneteket maven tesztelése projekt.

1. A mappában iot java-get-indított az [első lépések a IoT központi] oktatóanyagban hoztunk nevű **folyamat-d2c-üzenetek** , a következő parancs használatával a parancssorban maven tesztelése projektet létrehozni. Megjegyzés: Ez egy egyetlen, hosszú parancsot:

    ```
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=process-d2c-messages -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. A parancssorablakban nyissa meg az új folyamat-d2c-üzenetek mappát.

3. Szövegszerkesztővel, nyissa meg a pom.xml fájlt a folyamat-d2c-üzenetek mappát, és a következő függőségeket hozzáadása a **függőségeket** csomópontot. Ezeket a függőségeket segítségével az azure-eventhubs, azure-eventhubs-eph és azure-servicebus csomagok az alkalmazás kapcsolatba léphet a IoT központi és szolgáltatás Bus várólista teszik lehetővé:

    ```
    <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-eventhubs</artifactId>
      <version>0.8.0</version>
    </dependency>
    <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-eventhubs-eph</artifactId>
      <version>0.8.0</version>
    </dependency>
    <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-servicebus</artifactId>
      <version>0.9.4</version>
    </dependency>
    ```

4. Mentse és zárja be a pom.xml fájlt.

A következő tevékenység egy **ErrorNotificationHandler** osztály hozzáadása a projekthez.

1. Egy egyszerű szövegszerkesztő programban használatával hozzon létre egy process-d2c-messages\src\main\java\com\mycompany\app\ErrorNotificationHandler.java fájlt. A következő kód hozzáadása a fájlhoz, a **EventProcesssorHost** példányból hibaüzenetek megjelenítéséhez:

    ```
    package com.mycompany.app;

    import java.util.function.Consumer;
    import com.microsoft.azure.eventprocessorhost.ExceptionReceivedEventArgs;

    public class ErrorNotificationHandler implements
        Consumer<ExceptionReceivedEventArgs> {
      @Override
      public void accept(ExceptionReceivedEventArgs t) {
        System.out.println("EventProcessorHost: Host " + t.getHostname()
            + " received general error notification during " + t.getAction() + ": "
            + t.getException().toString());
      }
    }
    ```

2. Mentse és zárja be a ErrorNotificationHandler.java fájlt.

Most, hogy a **IEventProcessor** felületet osztály is hozzáadhat. A **EventProcessorHost** osztály hívja fel az osztály IoT központból fogadott eszköz a felhőbe üzenetek feldolgozása. Az osztály a kód blob-tároló megbízható tárolják az üzeneteket, és a szolgáltatás Bus várólista interaktív üzenetek továbbítása a logika hajtja végre.

A **onEvents** módszer a esemény processzor értelmezése a legújabb üzenet az eltolás és a sorozat számlálja **latestEventData** változó állítja be. Ne feledje, hogy minden processzor egyetlen partíciót felelős. A **onEvents** módszer az üzenetek egy köteg kap IoT központból, majd következőképpen dolgozza fel: interaktív üzeneteket küld a szolgáltatás Bus várakozási sorban található, és adatok pont üzenetek hozzáfűzi a **toAppend** memória puffer. Ha a memória puffer eléri a blokk 4 MB-os korlát, vagy a windows megszüntetése ismétlődések indításakor a program (ebben az oktatóprogramban az utolsó ellenőrzés után egy órával), a módszerrel a ellenőrzés elindítja.

A **AppendAndCheckPoint** módszer először létrehoz egy **blockId** Tiltás hozzáfűzése a blob. Azure tárhely szükséges az összes blokkolása azonosítók azonos hosszúságúak, így a módszerrel pads az eltolás, bevezető nullákkal. Ezt követően egy szövegrészt a azonosítójú már szerepel a blob, ha a módszerrel felülírja azt az aktuális pufferelési tartalom.

> [AZURE.NOTE] Egyszerűsítése érdekében a kódot, ebben az oktatóanyagban partíciót egy egyetlen blob használja az üzenetek tárolásához. Valós megoldást bizonyos után, illetve ha egy bizonyos méretet elérik további fájlokat létrehozásával közbeni fájlt szeretne végrehajtása. Ne feledje, hogy egy blokk Azure blob tartalmazhatnak legfeljebb 195 GB adatot.

A következő tevékenységek végrehajtása a **IEventProcessor** felület:

1. Egy egyszerű szövegszerkesztő programban használatával hozzon létre egy process-d2c-messages\src\main\java\com\mycompany\app\EventProcessor.java fájlt.

2. Adja hozzá a következő import és osztály meghatározása a EventProcessor.java fájlt. A **EventProcessor** osztály a **IEventProcessor** felülettel, amely definiálja az esemény hubok ügyfél viselkedését hajtja végre:

    ```
    package com.mycompany.app;

    import java.io.ByteArrayInputStream;
    import java.io.ByteArrayOutputStream;
    import java.io.IOException;
    import java.net.URISyntaxException;
    import java.nio.charset.StandardCharsets;
    import java.time.Duration;
    import java.time.Instant;
    import java.util.ArrayList;
    import java.util.Base64;
    import java.util.concurrent.ExecutionException;

    import com.microsoft.azure.eventhubs.EventData;
    import com.microsoft.azure.eventprocessorhost.*;
    import com.microsoft.azure.storage.*;
    import com.microsoft.azure.storage.blob.*;
    import com.microsoft.windowsazure.services.servicebus.*;
    import com.microsoft.windowsazure.services.servicebus.models.BrokeredMessage;

    public class EventProcessor implements IEventProcessor {

    }
    ```

3. Az alábbi módszerek hozzáadása a **EventProcessor** osztály a **IEventProcessor** felület végrehajtásához:

    ```
    @Override
    public void onOpen(PartitionContext context) throws Exception {
      System.out.println("EventProcessorHost: Partition "
          + context.getPartitionId() + " is opening");
    }

    @Override
    public void onClose(PartitionContext context, CloseReason reason)
        throws Exception {
      System.out.println("EventProcessorHost: Partition "
          + context.getPartitionId() + " is closing for reason "
          + reason.toString());
    }

    @Override
    public void onError(PartitionContext context, Throwable error) {
      System.out.println("EventProcessorHost: Partition "
          + context.getPartitionId() + " onError: " + error.toString());
    }

    @Override
    public void onEvents(PartitionContext context, Iterable<EventData> messages)
        throws Exception {
    }
    ```

4. A következő osztály szintű változók hozzáadása a **EventProcessor** osztály:

    ```
    public static CloudBlobContainer blobContainer;
    public static ServiceBusContract serviceBusContract;

    // Use a smaller MAX_BLOCK_SIZE value to test.
    final private int MAX_BLOCK_SIZE = 4 * 1024 * 1024;
    final private Duration MAX_CHECKPOINT_TIME = Duration.ofHours(1);

    private ByteArrayOutputStream toAppend = new ByteArrayOutputStream(
        MAX_BLOCK_SIZE);
    private Instant start = Instant.now();
    private EventData latestEventData;
    ```

5. Egy **AppendAndCheckPoint** módszer a következő aláírás hozzáadása a **EventProcessor** osztály:

    ```
    private void AppendAndCheckPoint(PartitionContext context)
      throws URISyntaxException, StorageException, IOException,
      IllegalArgumentException, InterruptedException, ExecutionException {
    }
    ```

6. Az aktuális üzenet az eltolás és a sorozat száma a partíciót beolvasásához a **AppendAndCheckPoint** módszerrel hozzá a következő kódot:

    ```
    String currentOffset = latestEventData.getSystemProperties().getOffset();
    Long currentSequence = latestEventData.getSystemProperties().getSequenceNumber();
    System.out
        .printf(
            "\nAppendAndCheckPoint using partition: %s, offset: %s, sequence: %s\n",
            context.getPartitionId(), currentOffset, currentSequence);
    ```

7. A **AppendAndCheckPoint** módszer használja a jelenlegi eltolás értéket a következő időtartomány mentése a blob- **BlockEntry** példány létrehozása:

    ```
    Long blockId = Long.parseLong(currentOffset);
    String blockIdString = String.format("startSeq:%1$025d", blockId);
    String encodedBlockId = Base64.getEncoder().encodeToString(
        blockIdString.getBytes(StandardCharsets.US_ASCII));
    BlockEntry block = new BlockEntry(encodedBlockId);
    ```

8. Töltse fel a legújabb készlete üzeneteket a továbbfejlesztett fájlblokkolás blob a **AppendAndCheckPoint** módszer, és megakadályozza az aktuális lista beolvasása:

    ```
    String blobName = String.format("iothubd2c_%s", context.getPartitionId());
    CloudBlockBlob currentBlob = blobContainer.getBlockBlobReference(blobName);

    currentBlob.uploadBlock(block.getId(),
        new ByteArrayInputStream(toAppend.toByteArray()), toAppend.size());
    ArrayList<BlockEntry> blockList = currentBlob.downloadBlockList();
    ```

9. A **AppendAndCheckPoint** módszer a kezdeti építőelem létrehozása az új blob, vagy a továbbfejlesztett fájlblokkolás fűznie a meglévő blob:

    ```
    if (currentBlob.exists()) {
      // Check if we should append new block or overwrite existing block
      BlockEntry last = blockList.get(blockList.size() - 1);
      if (blockList.size() > 0 && !last.getId().equals(block.getId())) {
        System.out.printf("Appending block %s to blob %s\n", blockId, blobName);
        blockList.add(block);
      } else {
        System.out.printf("Overwriting block %s in blob %s\n", blockId,
            blobName);
      }
    } else {
      System.out.printf("Creating initial block %s in new blob: %s\n", blockId,
          blobName);
      blockList.add(block);
    }
    currentBlob.commitBlockList(blockList);
    ```

10. Végül a **AppendAndCheckPoint** módszer, hozzon létre egy ellenőrzés partíciót és előkészítése a következő időtartomány az üzenetek mentése:

    ```
    context.checkpoint(latestEventData);

    // Reset everything after the checkpoint.
    toAppend.reset();
    start = Instant.now();
    System.out.printf("Checkpointed on partition id: %s\n",
        context.getPartitionId());
    ```

11. Az a **onEvents** módot adja meg a következő kódot üzenetek fogadására a IoT központi végpontot, és interaktív üzenetek továbbítása a szolgáltatás Bus sorba. A továbbfejlesztett fájlblokkolás megtelt és a határidő lejárta, hívhatja **AppendAndCheckPoint** módszer:

    ```
    if (messages != null) {
      for (EventData eventData : messages) {
        latestEventData = eventData;
        byte[] data = eventData.getBody();
        if (eventData.getProperties().containsKey("messageType")
            && eventData.getProperties().get("messageType")
                .equals("interactive")) {
          String messageId = (String) eventData.getSystemProperties().get(
              "message-id");
          BrokeredMessage message = new BrokeredMessage(data);
          message.setMessageId(messageId);
          serviceBusContract.sendQueueMessage("d2ctutorial", message);
          continue;
        }
        if (toAppend.size() + data.length > MAX_BLOCK_SIZE
            || Duration.between(start, Instant.now()).compareTo(
                MAX_CHECKPOINT_TIME) > 0) {
          AppendAndCheckPoint(context);
        }
        toAppend.write(data);
      }
    }
    ```

12. A **onEvents** módszer, végül adja hozzá az "else if" záradék hívja fel a **AppendAndCheckPoint** , ha az időkorlát lejár, miközben nincsenek IoT központi érkező üzenetek:

    ```
    else if ((toAppend.size() > 0)
        && Duration.between(start, Instant.now())
            .compareTo(MAX_CHECKPOINT_TIME) > 0) {
      AppendAndCheckPoint(context);
    }
    ```

13. Mentse a módosításokat a EventProcessor.java fájlt.

Az utolsó művelet a **folyamat-d2c-üzenetek** projekt kód hozzáadása a **fő** módszer, hogy elindítja- **EventProcessorHost** példány.

1. Egy egyszerű szövegszerkesztő programban segítségével a process-d2c-messages\src\main\java\com\mycompany\app\App.java fájlra.

2. A következő **importálása** kimutatások hozzáadása a fájlhoz:

    ```
    import com.microsoft.azure.eventprocessorhost.*;
    import com.microsoft.azure.servicebus.ConnectionStringBuilder;
    import com.microsoft.azure.storage.CloudStorageAccount;
    import com.microsoft.azure.storage.StorageException;
    import com.microsoft.azure.storage.blob.CloudBlobClient;
    import com.microsoft.windowsazure.Configuration;
    import com.microsoft.windowsazure.services.servicebus.ServiceBusConfiguration;
    import com.microsoft.windowsazure.services.servicebus.ServiceBusService;

    import java.net.URISyntaxException;
    import java.security.InvalidKeyException;
    import java.util.concurrent.*;
    ```

3. Az **alkalmazás** osztály adja hozzá a következő osztály szintű változó. **{Yourstorageaccountconnectionstring}** cserélje okozó egy korábban [Azure tárterület-fiók és egy szolgáltatás Bus várólista kiépítése](#provision-an-azure-storage-account-and-a-service-bus-queue) szakaszában Azure tároló fiók kapcsolati karakterláncot:

    ```
    private final static String storageConnectionString = "{yourstorageaccountconnectionstring}";
    ```

4. A következő osztály szintű változók hozzáadása az **alkalmazás** osztály, és cserélje le **{yourservicebusnamespace}** **{yourservicebussendkey}** és szolgáltatás Bus névtér a várólista **küldése** billentyűt. Korábban elvégzett névtér és **meghallgatásához** kulcs értékeit a Megjegyzés [Azure tárterület-fiók és egy szolgáltatás Bus várólista kiépítése](#provision-an-azure-storage-account-and-a-service-bus-queue) szakaszában:

    ```
    private final static String serviceBusNamespace = "{yourservicebusnamespace}";
    private final static String serviceBusSasKeyName = "send";
    private final static String serviceBusSASKey = "{yourservicebussendkey}";
    private final static String serviceBusRootUri = ".servicebus.windows.net";
    ```

5. Az **alkalmazás** osztály adja hozzá a következő osztály szintű változók. **{Youreventhubcompatibleendpoint}** cserélje le az esemény központi-kompatibilis végpont értéket. Az endpoint értéket a következőképpen néz **annak... névtér** , el kell távolítania a **sb: / /** előtag és az **.servicebus.windows.net/** utótag. **{Youreventhubcompatiblename}** cserélje le az esemény központi-kompatibilis nevére. **{Youriothubkey}** cserélje le a **iothubowner** billentyűt. Ezeket az értékeket a feljegyzés [létrehozása egy IoT központi] elvégzett[ lnk-create-an-iot-hub] szakaszban az *első lépések az Azure IoT központi Java* oktatóprogram:

    ```
    private final static String consumerGroupName = "$Default";
    private final static String namespaceName = "{youreventhubcompatibleendpoint}";
    private final static String eventHubName = "{youreventhubcompatiblename}";
    private final static String sasKeyName = "iothubowner";
    private final static String sasKey = "{youriothubkey}";
    ```

6. Módosítsa az aláírást a **fő** módszer a következőképpen:

    ```
    public static void main(String args[]) throws InvalidKeyException,
      URISyntaxException, StorageException {
    }
    ```

7. A következő kód hozzáadása a **fő** módszert tárolja az üzeneteket a blob-tárolóhoz hivatkozás:

    ```
    System.out.println("Process D2C messages using EventProcessorHost");
    CloudStorageAccount account = CloudStorageAccount
        .parse(storageConnectionString);
    CloudBlobClient client = account.createCloudBlobClient();
    EventProcessor.blobContainer = client
        .getContainerReference("d2cjavatutorial");
    EventProcessor.blobContainer.createIfNotExists();
    ```

8. A következő kód hozzáadása a **fő** módszert a szolgáltatás Bus szolgáltatás hivatkozás:

    ```
    Configuration config = ServiceBusConfiguration
        .configureWithSASAuthentication(serviceBusNamespace,
            serviceBusSasKeyName, serviceBusSASKey, serviceBusRootUri);
    EventProcessor.serviceBusContract = ServiceBusService.create(config);
    ```

9. A **fő** módszer állítsa be és hozza létre a **EventProcessorHost** példány. A **setInvokeProcessorAfterReceiveTimeout** beállítás biztosítja, hogy az **EventProcessorHost** példány elindítja **IEventProcessor** felületén a **onEvents** módszer akkor is, ha nincs üzenetek feldolgozása. A **onEvents** módszer majd mindig elindítja a **AppendAndCheckPoint** módszer az időkorlát lejártakor.

    ```
    ConnectionStringBuilder eventHubConnectionString = new ConnectionStringBuilder(
        namespaceName, eventHubName, sasKeyName, sasKey);
    EventProcessorHost host = new EventProcessorHost(eventHubName,
        consumerGroupName, eventHubConnectionString.toString(),
        storageConnectionString);
    EventProcessorOptions options = new EventProcessorOptions();
    options.setExceptionNotification(new ErrorNotificationHandler());
    options.setInvokeProcessorAfterReceiveTimeout(true);
    ```

10. A **fő** módszer rögzítése **IEventProcessor** végrehajtása **EventProcessorHost** használata:

    ```
    try {
      System.out.println("Registering host named " + host.getHostName());
      host.registerEventProcessor(EventProcessor.class, options).get();
    } catch (Exception e) {
      System.out.print("Failure while registering: ");
      if (e instanceof ExecutionException) {
        Throwable inner = e.getCause();
        System.out.println(inner.toString());
      } else {
        System.out.println(e.toString());
      }
      System.out.println(e.toString());
    }
    ```

11. Végezetül logika hozzáadása a **fő** módszer a **EventProcessorHost** példány leállítása:

    ```
    System.out.println("Press enter to stop");
    try {
      System.in.read();
      host.unregisterEventProcessor();

      System.out.println("Calling forceExecutorShutdown");
      EventProcessorHost.forceExecutorShutdown(120);
    } catch (Exception e) {
      System.out.println(e.toString());
      e.printStackTrace();
    }

    System.out.println("End of sample");
    ```

12. Mentse és zárja be a process-d2c-messages\src\main\java\com\mycompany\app\App.java fájlt.

13. Összeállítása maven tesztelése **folyamat-d2c-üzenetek** alkalmazást, hajtsa végre a következő parancsot a parancssorablakban – a folyamat-d2c-üzenetek mappában:

    ```
    mvn clean package -DskipTests
    ```

## <a name="receive-interactive-messages"></a>Interaktív hibaüzenetek

Ebben a részben egy Java konzol alkalmazás, amely az interaktív üzenetek kapja a szolgáltatás Bus sorból írni.

Az első tevékenység hozzáadása a nevű **folyamat interaktív üzenetet** kapja meg a szolgáltatás Bus várólista **EventProcessor** példányok küldött üzenetek maven tesztelése projekt.

1. A mappában iot java-get-indított az [első lépések a IoT központi] oktatóanyagban hoztunk nevű **folyamat interaktív üzenetek** , a következő parancs használatával a parancssorban maven tesztelése projektet létrehozni. Megjegyzés: Ez egy egyetlen, hosszú parancsot:

    ```
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=process-interactive-messages -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. A parancssorablakban nyissa meg az új folyamat interaktív üzenetek mappát.

3. Szövegszerkesztőben használja, nyissa meg a pom.xml fájlt a folyamat interaktív üzenetek mappát, és a következő típusú függés hozzáadása a **függőségeket** csomópontot. Ez a függés lehetővé teszi, hogy az alkalmazás az azure-servicebus csomag segítségével kapcsolatba léphet a szolgáltatás Bus várólista:

    ```
    <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-servicebus</artifactId>
      <version>0.9.4</version>
    </dependency>
    ```

4. Mentse és zárja be a pom.xml fájlt.

A következő feladatra a szolgáltatás Bus sorból üzeneteihez kódot hozzáadni.

1. Egy egyszerű szövegszerkesztő programban segítségével a process-interactive-messages\src\main\java\com\mycompany\app\App.java fájlra.

2. Adja hozzá a következő `import` utasítások a fájlt:

    ```
    import java.io.IOException;
    import java.util.concurrent.ExecutorService;
    import java.util.concurrent.Executors;

    import com.microsoft.windowsazure.Configuration;
    import com.microsoft.windowsazure.exception.ServiceException;
    import com.microsoft.windowsazure.services.servicebus.*;
    import com.microsoft.windowsazure.services.servicebus.models.*;
    ```

3. A következő osztály szintű változók hozzáadása az **alkalmazás** osztály, és cserélje le **{yourservicebusnamespace}** **{yourservicebuslistenkey}** és szolgáltatás Bus névtér a várólista **meghallgatása** billentyűt. Korábban elvégzett névtér és **meghallgatásához** kulcs értékeit a Megjegyzés [Azure tárterület-fiók és egy szolgáltatás Bus várólista kiépítése](#provision-an-azure-storage-account-and-a-service-bus-queue) szakaszában:

    ```
    private final static String serviceBusNamespace = "{yourservicebusnamespace}";
    private final static String serviceBusSasKeyName = "listen";
    private final static String serviceBusSASKey = "{yourservicebuslistenkey}";
    private final static String serviceBusRootUri = ".servicebus.windows.net";
    private final static String queueName = "d2ctutorial";
    private static ServiceBusContract service = null;
    ```

4. Az alábbi beágyazott osztály hozzáadása az **alkalmazás** osztály a várólista érkező üzenetek fogadására:

    ```
    private static class MessageReceiver implements Runnable {
      public void run() {
        ReceiveMessageOptions opts = ReceiveMessageOptions.DEFAULT;
        try {
          while (true) {
            ReceiveQueueMessageResult resultQM = service.receiveQueueMessage(
                queueName, opts);
            BrokeredMessage message = resultQM.getValue();
            if (message != null && message.getMessageId() != null) {
              System.out.println("MessageID: " + message.getMessageId());
              System.out.print("From queue: ");
              byte[] b = new byte[200];
              String s = null;
              int numRead = message.getBody().read(b);
              while (-1 != numRead) {
                s = new String(b);
                s = s.trim();
                System.out.print(s);
                numRead = message.getBody().read(b);
              }
              System.out.println();
            } else {
              Thread.sleep(1000);
            }
          }
        } catch (InterruptedException e) {
          System.out.println("Finished.");
        } catch (ServiceException e) {
          System.out.println("ServiceException: " + e.getMessage());
        } catch (IOException e) {
          System.out.println("IOException: " + e.getMessage());
        }
      }
    }
    ```

5. Módosítsa az aláírást a **fő** módszer a következőképpen:

    ```
    public static void main(String args[]) throws ServiceException, IOException {
    }
    ```

6. A **fő** módszer adja hozzá az új üzenetek elkezdeni a következő kódot:

    ```
    System.out.println("Process interactive messages");

    Configuration config = ServiceBusConfiguration
        .configureWithSASAuthentication(serviceBusNamespace,
            serviceBusSasKeyName, serviceBusSASKey, serviceBusRootUri);
    service = ServiceBusService.create(config);

    MessageReceiver receiver = new MessageReceiver();

    ExecutorService executor = Executors.newFixedThreadPool(2);
    executor.execute(receiver);

    System.out.println("Press ENTER to exit.");
    System.in.read();
    executor.shutdownNow();
    ```

7. Mentse és zárja be a process-interactive-messages\src\main\java\com\mycompany\app\App.java fájlt.

8. Összeállítása maven tesztelése **folyamat interaktív üzenetek** alkalmazást, hajtsa végre a következő parancsot a parancssorablakban – a folyamat interaktív üzenetek mappában:

    ```
    mvn clean package -DskipTests
    ```

## <a name="run-the-applications"></a>Az alkalmazások futtatása

Most már készen áll a három alkalmazások futtatásához.

1.  A **folyamat interaktív üzenetek** alkalmazás futtatásához a parancssor vagy rendszerhéj keresse meg a folyamat interaktív üzenetek mappát, és hajtsa végre az alábbi parancsot:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![Folyamatábra-interaktív üzenetek futtatása][processinteractive]

2.  A **folyamat-d2c-üzenetek** alkalmazás futtatásához a parancssor vagy rendszerhéj keresse meg a folyamat-d2c-üzenetek mappát, és hajtsa végre a következő parancsot:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![Folyamatábra-d2c-üzenetek futtatása][processd2c]

3.  Az **eszköz szimulált** alkalmazás futtatásához parancssor vagy rendszerhéj keresse meg a szimulált eszköz mappát, és hajtsa végre a következő parancsot:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![Szimulált eszköz futtatása][simulateddevice]

> [AZURE.NOTE] A blob a módosítások megjelenítéséhez szükség lehet a **StoreEventProcessor** osztály **MAX_BLOCK_SIZE** állandó csökkentése egy kisebb értéket, például **1024**. Ez a változás akkor lehet hasznos, mert egy kis időt, a továbbfejlesztett fájlblokkolás méretkorlátok vonatkoznak azokra a szimulált eszköz által küldött adatok eléréséhez szükséges időt. A továbbfejlesztett fájlblokkolás kisebb méretű megjelenítéséhez, nem kell olyan sokáig várja meg, amíg a blob létrehozott és a frissített. Jó helyen jár nagyobb méretű blokkokból álló révén az alkalmazás további méretezhető.

## <a name="next-steps"></a>Következő lépések

Ebben az oktatóanyagban adatpont és interaktív eszköz a felhőbe üzenetek megbízható dolgozhat a [EventProcessorHost] osztály használatával hogyan megtanulta azt.

A [felhő-eszköz üzenetek IoT központi üzenetküldés] [ lnk-c2d] megtudhatja, hogy miként eszközén a hátsó végéről üzeneteket küldeni.

Példák a teljes végpont megoldások, amelyek IoT központi megtekintéséhez kattintson a [Azure IoT csomagja][lnk-suite].

IoT központi megoldások fejlesztésével kapcsolatos további információért lásd: a [IoT központi Fejlesztőeszközök útmutató].

<!-- Images. -->
[simulateddevice]: ./media/iot-hub-java-java-process-d2c/runsimulateddevice.png
[processinteractive]: ./media/iot-hub-java-java-process-d2c/runprocessinteractive.png
[processd2c]: ./media/iot-hub-java-java-process-d2c/runprocessd2c.png

[30]: ./media/iot-hub-java-java-process-d2c/createqueue2.png
[31]: ./media/iot-hub-java-java-process-d2c/createqueue3.png

<!-- Links -->

[Azure blob-tárolóhoz]: ../storage/storage-dotnet-how-to-use-blobs.md
[Azure Data Factory]: https://azure.microsoft.com/documentation/services/data-factory/
[HDInsight (Hadoop)]: https://azure.microsoft.com/documentation/services/hdinsight/
[Szolgáltatás Bus várólista]: ../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md

[Azure IoT központi Fejlesztőeszközök útmutató - eszköz a felhőbe]: iot-hub-devguide-messaging.md

[Azure tárhely]: https://azure.microsoft.com/documentation/services/storage/
[Azure Service Bus]: https://azure.microsoft.com/documentation/services/service-bus/

[IoT központi Fejlesztőeszközök útmutató]: iot-hub-devguide.md
[Első lépések a IoT központi]: iot-hub-java-java-getstarted.md
[Azure IoT Developer Center]: https://azure.microsoft.com/develop/iot
[lnk-service-fabric]: https://azure.microsoft.com/documentation/services/service-fabric/
[lnk-stream-analytics]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-event-hubs]: https://azure.microsoft.com/documentation/services/event-hubs/
[Ideiglenes (tranziens) hibafa kezelése]: https://msdn.microsoft.com/library/hh675232.aspx

<!-- Links -->
[Azure tároló kapcsolatban]: ../storage/storage-create-storage-account.md#create-a-storage-account
[Esemény hubok – első lépések]: ../event-hubs/event-hubs-java-ephjava-getstarted.md
[Azure tároló méretezhetőség útmutató]: ../storage/storage-scalability-targets.md
[Azure Block Blobs]: https://msdn.microsoft.com/library/azure/ee691964.aspx
[Event Hubs]: ../event-hubs/event-hubs-overview.md
[EventProcessorHost]: https://github.com/Azure/azure-event-hubs/tree/master/java/azure-eventhubs-eph
[Ideiglenes (tranziens) hibafa kezelése]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-classic-portal]: https://manage.windowsazure.com
[lnk-c2d]: iot-hub-java-java-process-d2c.md
[lnk-suite]: https://azure.microsoft.com/documentation/suites/iot-suite/

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/java-devbox-setup.md
[lnk-create-an-iot-hub]: iot-hub-java-java-getstarted.md#create-an-iot-hub