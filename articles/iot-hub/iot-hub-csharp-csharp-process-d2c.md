<properties
    pageTitle="Listaszerű folyamatábra IoT központi eszköz a felhőbe üzenetek (.Net) |} Microsoft Azure"
    description="Hajtsa végre az ebből az oktatóanyagból megtudhatja hasznos mintázatok IoT központi eszköz a felhőbe üzenetek feldolgozása."
    services="iot-hub"
    documentationCenter=".net"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="csharp"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="10/05/2016"
     ms.author="dobett"/>

# <a name="tutorial-how-to-process-iot-hub-device-to-cloud-messages-using-net"></a>Oktatóprogram: Hogyan IoT központi eszköz a felhőbe az üzenetek .net feldolgozása

[AZURE.INCLUDE [iot-hub-selector-process-d2c](../../includes/iot-hub-selector-process-d2c.md)]

## <a name="introduction"></a>– Bevezetés

Azure IoT központi egy teljes körű felügyelt szolgáltatás, amely lehetővé teszi a megbízható és biztonságos kétirányú kommunikációt, több millió IoT eszközök és az alkalmazások között biztonsági célból. További oktatóanyagok ([IoT központi – első lépések] és [IoT központi cloud-eszköz üzenet küldése][lnk-c2d]) megtudhatja, hogy miként alapvető eszköz a felhőbe, és a felhő-eszköz üzenetben funkcióit IoT központi használni.

Ebben az oktatóanyagban épül az [első lépések a IoT központi] oktatóprogram kódot, és azt mutatja, hogy két méretezhető mintázatok eszköz a felhőbe üzenetek feldolgozása használható:

- Az eszköz a felhőbe üzenetek [Azure blob-tárolóban]lévő megbízható tárhely. A leggyakoribb helyzet telemetriai adatokat a bevitt analytics folyamatok használandó BLOB tároljuk, *hideg elérési út* analytics. Ezek a folyamatok [Azure Data Factory] vagy a [HDInsight (Hadoop)] Papírhalom powerview eszközzel alapú is lehet.

- *Interaktív* eszköz a felhőbe üzenetek megbízható feldolgozása. Eszköz a felhőbe üzenetek interaktívak azonnali indítók a beállított a alkalmazás háttér-műveletek közül. Például egy eszközt előfordulhat, hogy küldése riasztás kiváltó jegy beszúrásával egy CRM rendszerrel. Ezzel ellentétben *az adatpont* üzenetek egyszerűen-hírcsatorna-elemző motor. Például a későbbi elemzéshez tárolni eszközt a hőmérsékleti telemetriai adatokat pont üzenet is.

Mivel a IoT központi közzététele egy [Központi esemény][lnk-event-hubs]-kompatibilis eszköz a felhőbe-üzenetek fogadására alkalmas, ez az oktatóanyag használ egy [EventProcessorHost] példány végpontot. Ebben az esetben:

* Azure blob-tárolóhoz biztos, hogy tárolja *az adatpont* üzeneteket.
* *Interaktív* eszköz a felhőbe üzeneteket továbbít az Azure [Service Bus várólista] azonnali feldolgozásra.

Szolgáltatás Bus biztosítja megbízható interaktív az üzenetek feldolgozásával üzenet pontok és idő ablak alapú megszüntetése ismétlődések biztosít.

> [AZURE.NOTE] Egy **EventProcessorHost** példány feldolgozni interaktív üzenetek csak egy lehetőség. Egyéb lehetőségek [Azure Service háló] [ lnk-service-fabric] és [Azure Értékáram-elemzés][lnk-stream-analytics].

Ez az oktatóanyag végén futtatja a Windows-konzol három alkalmazást:

* **SimulatedDevice**, az alkalmazás a módosított verziójával létrehozott az [első lépések a IoT központi] oktatóprogram adatokat küld a pont eszköz a felhőbe üzenetek minden második és interaktív eszköz a felhőbe üzenetek 10 másodpercenként. Az alkalmazás a IoT központi kommunikálni az AMQP protokollt használja.
* **ProcessDeviceToCloudMessages** használja a [EventProcessorHost] osztály üzenetek lekérése az esemény központi-kompatibilis végpontot. Azt, majd biztos, hogy tárolja adatok pont üzenetek Azure blob-tárolóban lévő, és továbbítja szolgáltatás Bus várólista interaktív üzeneteket.
* **ProcessD2CInteractiveMessages** vonja vissza a szolgáltatás Bus sorból az interaktív üzenetek várakozási sorba.

> [AZURE.NOTE] IoT hubhoz számos eszközt platformokon és nyelvek, többek között a C, Java és JavaScript SDK támogatása. Útmutató: a cserélje ki a szimulált eszközön ebben az oktatóprogramban egy fizikai eszközt, és csatlakoztassa az eszközöket IoT-hubon keresztül csatlakozott, lásd: az [Azure IoT Developer Center].

Ebben az oktatóanyagban olyan közvetlenül alkalmazható más módon használhatnak esemény központi-kompatibilis üzeneteket, például a projektek [HDInsight (Hadoop)] . További információ a [Azure IoT központi Fejlesztőeszközök útmutató - eszköz cloud]című témakörben.

Ebben az oktatóanyagban a következőkre lesz szüksége:

+ Microsoft Visual Studio 2015.

+ Azure active fiók. <br/>Nem rendelkeznek fiókkal, ha mindössze néhány perc is létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free/) .

Rendelkeznie kell néhány [Azure-tárhely] és [Azure Service Bus]alapismeretei.


## <a name="send-interactive-messages-from-a-simulated-device"></a>Interaktív küldésére a szimulált eszközről

Ebben a részben módosítsa az interaktív eszköz a felhőbe üzeneteket küldeni a IoT központi [IoT központi – első lépések] oktatóanyagban hoztunk szimulált eszközön alkalmazást.

1. A Visual Studióban a **SimulatedDevice** projekt adja hozzá a **Program** osztály a következő módszerrel.

    ```
    private static async void SendDeviceToCloudInteractiveMessagesAsync()
    {
      while (true)
      {
        var interactiveMessageString = "Alert message!";
        var interactiveMessage = new Message(Encoding.ASCII.GetBytes(interactiveMessageString));
        interactiveMessage.Properties["messageType"] = "interactive";
        interactiveMessage.MessageId = Guid.NewGuid().ToString();

        await deviceClient.SendEventAsync(interactiveMessage);
        Console.WriteLine("{0} > Sending interactive message: {1}", DateTime.Now, interactiveMessageString);

        Task.Delay(10000).Wait();
      }
    }
    ```

    Ez a módszer hasonlít a **SendDeviceToCloudMessagesAsync** módszer a **SimulatedDevice** projekt. Csak különbségek, akkor most a tulajdonság **MessageId** rendszer, és egy tulajdonság neve **messageType**.
    A kód rendel a **MessageId** tulajdonság egy globálisan egyedi azonosítója (GUID). A szolgáltatás Bus az azonosító segítségével vonja ismétlődő fogadott üzeneteket. A példa a pont üzenetekből interaktív megkülönböztetni a **messageType** tulajdonság használja. A alkalmazástól kapott üzenet tulajdonságai, ezt az információt helyett az üzenet törzsébe, hogy az esemény processzor nem szükséges, az üzenet végrehajtani üzenet továbbítása deszerializálni.

    > [AZURE.NOTE] Fontos, hogy a használt eszköz kód interaktív üzenetek vonja ismétlődő **MessageId** létrehozása. Szakaszos hálózati kommunikáció vagy más hibák, ugyanazt az üzenetet, az adott eszközről több Újraküldés eredményezhet. Szemantikai Üzenetazonosító, például a megfelelő üzenet adatmezők helyett egy globálisan egyedi azonosítója kivonat is használhatja.

2. A **fő** módszer esetén az alábbit hozzáadása előtt jobbra a `Console.ReadLine()` sor:

    ````
    SendDeviceToCloudInteractiveMessagesAsync();
    ````

    > [AZURE.NOTE] Ebben az oktatóprogramban az egyszerűség kedvéért nem hajtja végre bármely újrapróbálkozási házirend. A gyártási kódot végre kell hajtania a exponenciális visszalépési, [Ideiglenes (tranziens) hiba kezelésének]MSDN című témakörben javasolt, például egy újrapróbálkozási házirend.

## <a name="process-device-to-cloud-messages"></a>A folyamat eszköz a felhőbe üzenetek

Ebben a részben hoz létre, amely IoT központból eszköz a felhőbe üzenetek feldolgozásával Windows konzol alkalmazást. IOT központi közzététele [esemény központi]-kompatibilis végpont ahhoz, hogy az eszköz a felhőbe üzenetek olvasása alkalmazások. Ebben az oktatóanyagban használja a [EventProcessorHost] osztály folyamat az alábbi üzenetek console-alkalmazásban. Az esemény hubok folyamat során használatáról további tudnivalókért lásd: az [Első lépések az esemény hubok] oktatóprogram.

A kérdés, ha megbízható tárterület-adatpont üzenetek alkalmazhat, vagy interaktív üzenetek továbbítása, hogy esemény feldolgozása az üzenet fogyasztói pontjainak nyújt a meghívási folyamat támaszkodik. Ezenkívül egy nagy teljesítmény eléréséhez, amikor, olvassa el az esemény hubok meg kell adnia a nagy kötegekben pontjainak. Ezzel a módszerrel hoz létre ismétlődő feldolgozásban nagy mennyiségű üzenetet, ha egy hiba, és visszaállítja az előző ellenőrzés lehetőségét. Ebben az oktatóanyagban, megtudhatja, hogy miként Azure tároló írások és szolgáltatás Bus megszüntetése ismétlődések windows szinkronizálása **EventProcessorHost** pontjainak.

Biztos, hogy Azure tárolóhoz írhat üzeneteket, a mintában a [Továbbfejlesztett fájlblokkolás BLOB]egyes blokk jóváhagyás szolgáltatását használja[Azure Block Blobs]. Az esemény processzor összegzi az üzeneteket a memóriában mindaddig, amíg az ideje egy ellenőrzés megadását. Ha például a halmozott puffer üzenetek eléri 4 MB-os maximális méretét, illetve után a szolgáltatás Bus megszüntetése ismétlődések időkeret telik. Ezután az ellenőrzés, mielőtt a kód véglegesíti egy új szövegrészt a blob szeretne.

Az esemény processzor üzenet eltolja esemény hubok használja a azonosítók parancsot. Ez az eljárás lehetővé teszi, hogy az esemény processzor azt az új blokk véglegesíti tárolóhoz, ügyelve között egy szövegrészt, és az ellenőrzés elvégzése a lehetséges összeomlik, mielőtt a megszüntetése ismétlődés ellenőrzése végrehajtásához.

> [AZURE.NOTE] Ebben az oktatóanyagban IoT központból beolvasott összes üzenetet írhat egyetlen Azure tároló fiókot használ. Szükségességének használni a megoldás több Azure tárterület-fiókot, olvassa el az [Azure tároló méretezhetőség irányelveket].

Az alkalmazás elkerülése érdekében az ismétlődő elemeket, amikor interaktív üzenetek feldolgozásával szolgáltatás Bus megszüntetése ismétlődések szolgáltatását használja. A szimulált eszközön minden interaktív üzenet az egy egyedi **MessageId**ellátja időbélyeggel. Ezek lehetővé tevő dokumentumazonosítók engedélyezése szolgáltatás Bus annak érdekében, hogy a megadott megszüntetése ismétlődések idő ablakában nem két üzenetek az azonos **MessageId** a kézbesítési a vevők. A megszüntetése másolást, az üzenet teljesítése szemantikáját szolgáltatás Bus sorok, által biztosított együtt egyszerűen interaktív üzenetek megbízható feldolgozásának végrehajtásához.

Győződjön meg arról, hogy nincs üzenet kívül a megszüntetése ismétlődések ablakban van-e küldeni, a kód szinkronizálja a **EventProcessorHost** ellenőrzés mechanizmusa a szolgáltatás Bus várakozási sor megszüntetése ismétlődések ablak. A szinkronizálás végzi a ellenőrzés legalább egyszer kényszerítése, minden alkalommal, amikor a program a megszüntetése ismétlődések ablak (ebben az oktatóprogramban az egy órával).

> [AZURE.NOTE] Ebben az oktatóprogramban egy adott particionált szolgáltatás Bus sorba az interaktív üzenetek beolvasott IoT központból folyamat használja. A megoldás méretezhetőség követelményeknek szolgáltatás Bus sorban várakozó használatáról további információt az [Azure Service Bus] dokumentációjában olvasható.

### <a name="provision-an-azure-storage-account-and-a-service-bus-queue"></a>Azure tárterület-fiók és egy szolgáltatás Bus várólista kiépítése
A [EventProcessorHost] osztály használatához rendelkeznie ahhoz, hogy a **EventProcessorHost** ellenőrzés az adatok rögzítéséhez Azure tároló fiók. Egy meglévő Azure tárterület-fiókot használ, vagy kövesse az utasításokat [Azure-tárolóban] lévő hozzon létre egy újat. Jegyezze fel az Azure tároló fiók kapcsolati karakterláncot.

> [AZURE.NOTE] Másolja a vágólapra, és illessze be az Azure tároló fiók kapcsolati karakterláncot, ügyeljen az nem tartalmaz szóközöket.

A szolgáltatás Bus várólista interaktív üzenetek megbízható feldolgozásának engedélyezése is szükséges. Létrehozhat egy várólista programozás útján, egy órával megszüntetése ismétlődések ablakot,[szolgáltatás Bus várólista] [szolgáltatást Bus sorban várakozó használata]című cikkben ismertetett módon. Másik lehetőségként használhatja az [Azure klasszikus portál][lnk-classic-portal], ezeket a lépéseket követve:

1. A bal alsó sarkában kattintson az **Új** gombra. Kattintson a **Szolgáltatások alkalmazás** > **Szolgáltatás Bus** > **várólista** > **Egyéni létrehozása**. Írja be a név **d2ctutorial**, jelöljön ki egy területet, és egy meglévő névtér használata, vagy hozzon létre egy újat. A következő lapon jelölje be **a duplikált elemek észlelése engedélyezése**jelölőnégyzetet, és a **Duplikálás észlelési előzmények időkeret** beállítása egy óra. Kattintson a jobb alsó sarokban a várólista konfiguráció mentése a pipa ikonra.

    ![Várólista létrehozása az Azure-portálon][30]

2. A szolgáltatás Bus sorok listájában kattintson a **d2ctutorial**, és kattintson a **beállítás**. Hozzon létre két megosztott access-házirendek, egy úgynevezett **küldése** **küldése** engedélyekkel rendelkező, és egy úgynevezett **meghallgatása** **meghallgatása** engedélyekkel. Ha végzett, kattintson a **Mentés** gombra a képernyő alján.

    ![Az Azure-portálon várólista konfigurálása][31]

3. A képernyő alján kattintson az **Irányítópult** tetején, és kattintson a **kapcsolat adatait** . Jegyezze fel a két kapcsolati karakterláncot.

    ![Az Azure-portálon várólista irányítópult][32]

### <a name="create-the-event-processor"></a>Az esemény processzor létrehozása

1. Az aktuális Visual Studio megoldás vizuális Windows C# projekt létrehozása a **New** project-sablon használatával, kattintson a **fájl** > **hozzáadása** > **Új projektet**. Győződjön meg arról, hogy a .NET-keretrendszer verzió 4.5.1 vagy újabb verziója. A projekt **ProcessDeviceToCloudMessages**nevet, és kattintson az **OK gombra**.

    ![A Visual Studio alkalmazásban új projektet][10]

2. Kattintson a jobb gombbal a **ProcessDeviceToCloudMessages** projekt megoldás Explorerben, és válassza a **NuGet csomagok kezelése**. A **NuGet csomag kezelő** párbeszédpanel jelenik meg.

3. **WindowsAzure.ServiceBus**keresni, kattintson a **telepítés**gombra, és fogadja el a használati feltételeket. Ezt a műveletet tölthető le, és a telepítést, és hozzáadja a az [Azure Service Bus NuGet csomag](https://www.nuget.org/packages/WindowsAzure.ServiceBus), az összes függőség mutató hivatkozás.

4. **Microsoft.Azure.ServiceBus.EventProcessorHost**keresni, kattintson a **telepítés**gombra, és fogadja el a használati feltételeket. Ez a művelet letöltések, telepíti, és hozzáadja a az [Azure Service Bus esemény központi - EventProcessorHost NuGet csomag](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost), az összes függőség hivatkozást.

5. Kattintson a jobb gombbal a **ProcessDeviceToCloudMessages** projektet, kattintson a **Hozzáadás**gombra, és kattintson az **Osztályjegyzetfüzet**. Nevezze el az új osztály **StoreEventProcessor**, és kattintson **az OK** gombra az osztályjegyzetfüzet létrehozásához.

6. Adja hozzá a következő utasítások a StoreEventProcessor.cs fájl tetején:

    ```
    using System.IO;
    using System.Diagnostics;
    using System.Security.Cryptography;
    using Microsoft.ServiceBus.Messaging;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;
    ```

7. Helyettesítő osztály a szervezet számára a következő kódot:

    ```
    class StoreEventProcessor : IEventProcessor
    {
      private const int MAX_BLOCK_SIZE = 4 * 1024 * 1024;
      public static string StorageConnectionString;
      public static string ServiceBusConnectionString;

      private CloudBlobClient blobClient;
      private CloudBlobContainer blobContainer;
      private QueueClient queueClient;

      private long currentBlockInitOffset;
      private MemoryStream toAppend = new MemoryStream(MAX_BLOCK_SIZE);

      private Stopwatch stopwatch;
      private TimeSpan MAX_CHECKPOINT_TIME = TimeSpan.FromHours(1);

      public StoreEventProcessor()
      {
        var storageAccount = CloudStorageAccount.Parse(StorageConnectionString);
        blobClient = storageAccount.CreateCloudBlobClient();
        blobContainer = blobClient.GetContainerReference("d2ctutorial");
        blobContainer.CreateIfNotExists();
        queueClient = QueueClient.CreateFromConnectionString(ServiceBusConnectionString);
      }

      Task IEventProcessor.CloseAsync(PartitionContext context, CloseReason reason)
      {
        Console.WriteLine("Processor Shutting Down. Partition '{0}', Reason: '{1}'.", context.Lease.PartitionId, reason);
        return Task.FromResult<object>(null);
      }

      Task IEventProcessor.OpenAsync(PartitionContext context)
      {
        Console.WriteLine("StoreEventProcessor initialized.  Partition: '{0}', Offset: '{1}'", context.Lease.PartitionId, context.Lease.Offset);

        if (!long.TryParse(context.Lease.Offset, out currentBlockInitOffset))
        {
          currentBlockInitOffset = 0;
        }
        stopwatch = new Stopwatch();
        stopwatch.Start();

        return Task.FromResult<object>(null);
      }

      async Task IEventProcessor.ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages)
      {
        foreach (EventData eventData in messages)
        {
          byte[] data = eventData.GetBytes();

          if (eventData.Properties.ContainsKey("messageType") && (string) eventData.Properties["messageType"] == "interactive")
          {
            var messageId = (string) eventData.SystemProperties["message-id"];

            var queueMessage = new BrokeredMessage(new MemoryStream(data));
            queueMessage.MessageId = messageId;
            queueMessage.Properties["messageType"] = "interactive";
            await queueClient.SendAsync(queueMessage);

            WriteHighlightedMessage(string.Format("Received interactive message: {0}", messageId));
            continue;
          }

          if (toAppend.Length + data.Length > MAX_BLOCK_SIZE || stopwatch.Elapsed > MAX_CHECKPOINT_TIME)
          {
            await AppendAndCheckpoint(context);
          }
          await toAppend.WriteAsync(data, 0, data.Length);

          Console.WriteLine(string.Format("Message received.  Partition: '{0}', Data: '{1}'",
            context.Lease.PartitionId, Encoding.UTF8.GetString(data)));
        }
      }

      private async Task AppendAndCheckpoint(PartitionContext context)
      {
        var blockIdString = String.Format("startSeq:{0}", currentBlockInitOffset.ToString("0000000000000000000000000"));
        var blockId = Convert.ToBase64String(ASCIIEncoding.ASCII.GetBytes(blockIdString));
        toAppend.Seek(0, SeekOrigin.Begin);
        byte[] md5 = MD5.Create().ComputeHash(toAppend);
        toAppend.Seek(0, SeekOrigin.Begin);

        var blobName = String.Format("iothubd2c_{0}", context.Lease.PartitionId);
        var currentBlob = blobContainer.GetBlockBlobReference(blobName);

        if (await currentBlob.ExistsAsync())
        {
          await currentBlob.PutBlockAsync(blockId, toAppend, Convert.ToBase64String(md5));
          var blockList = await currentBlob.DownloadBlockListAsync();
          var newBlockList = new List<string>(blockList.Select(b => b.Name));

          if (newBlockList.Count() > 0 && newBlockList.Last() != blockId)
          {
            newBlockList.Add(blockId);
            WriteHighlightedMessage(String.Format("Appending block id: {0} to blob: {1}", blockIdString, currentBlob.Name));
          }
          else
          {
            WriteHighlightedMessage(String.Format("Overwriting block id: {0}", blockIdString));
          }
          await currentBlob.PutBlockListAsync(newBlockList);
        }
        else
        {
          await currentBlob.PutBlockAsync(blockId, toAppend, Convert.ToBase64String(md5));
          var newBlockList = new List<string>();
          newBlockList.Add(blockId);
          await currentBlob.PutBlockListAsync(newBlockList);

          WriteHighlightedMessage(String.Format("Created new blob", currentBlob.Name));
        }

        toAppend.Dispose();
        toAppend = new MemoryStream(MAX_BLOCK_SIZE);

        // checkpoint.
        await context.CheckpointAsync();
        WriteHighlightedMessage(String.Format("Checkpointed partition: {0}", context.Lease.PartitionId));

        currentBlockInitOffset = long.Parse(context.Lease.Offset);
        stopwatch.Restart();
      }

      private void WriteHighlightedMessage(string message)
      {
        Console.ForegroundColor = ConsoleColor.Yellow;
        Console.WriteLine(message);
        Console.ResetColor();
      }
    }
    ```

    A **EventProcessorHost** osztály hívja fel az osztály IoT központból fogadott eszköz a felhőbe üzenetek feldolgozása. Az osztály a kód blob-tároló megbízható tárolják az üzeneteket, és a szolgáltatás Bus várólista interaktív üzenetek továbbítása a logika hajtja végre.

    A **OpenAsync** módszer előkészíti a **currentBlockInitOffset** változó, amely követi nyomon az aktuális eltolás az első üzenet a esemény processzor értelmezése. Ne feledje, hogy minden processzor egyetlen partíciót felelős.

    A **ProcessEventsAsync** módszer kap egy a köteget, az üzenetek IoT központból, és a következőképpen dolgozza fel: interaktív üzeneteket küld a szolgáltatás Bus várakozási sorban található, és adatok pont üzenetek hozzáfűzi a memória puffer **toAppend**neve. Ha a memória puffer eléri a 4 MB-os korlát, vagy a windows megszüntetése ismétlődések indításakor a program (ebben az oktatóprogramban egy ellenőrzés után egy órával), az alkalmazás egy ellenőrzés elindítja.

    A **AppendAndCheckpoint** módszer először egy blockId hozzáfűzése a blokk hoz létre. Azure tárhely szükséges összes blokkolása van a ugyanolyan hosszú, ezért a módszerrel pads az eltolás, bevezető nullákkal - azonosítók `currentBlockInitOffset.ToString("0000000000000000000000000")`. Ezután, ha egy szövegrészt a azonosítójú már a blob, a módszerrel felülírja a puffer aktuális tartalmával.

    > [AZURE.NOTE] Egyszerűsítése érdekében a kódot, ebben az oktatóanyagban partíciót egy egyetlen blob használja az üzenetek tárolásához. Valós megoldást bizonyos után, illetve ha egy bizonyos méretet elérik további fájlokat létrehozásával közbeni fájlt szeretne végrehajtása. Ne feledje, hogy egy blokk Azure blob tartalmazhatnak legfeljebb 195 GB adatot.

8. A **Program** osztály adja hozzá a következő **használatával** utasítás tetején:

    ```
    using Microsoft.ServiceBus.Messaging;
    ```

9. A **fő** módszer a **Program** osztály módosítsa a következőképpen. **{Iot központi kapcsolati karakterlánc}** cserélje le a az [első lépések a IoT központi] oktatóprogram **iothubowner** kapcsolati karakterlánccal. A tároló kapcsolati karakterláncot cserélje le a kapcsolati karakterlánc elején ebben a szakaszban a lejegyzett. A szolgáltatás Bus kapcsolati karakterláncot cserélje ki a sor elején ebben a szakaszban a lejegyzett **d2ctutorial** nevű engedélyeinek **küldése** :

    ```
    static void Main(string[] args)
    {
      string iotHubConnectionString = "{iot hub connection string}";
      string iotHubD2cEndpoint = "messages/events";
      StoreEventProcessor.StorageConnectionString = "{storage connection string}";
      StoreEventProcessor.ServiceBusConnectionString = "{service bus send connection string}";

      string eventProcessorHostName = Guid.NewGuid().ToString();
      EventProcessorHost eventProcessorHost = new EventProcessorHost(eventProcessorHostName, iotHubD2cEndpoint, EventHubConsumerGroup.DefaultGroupName, iotHubConnectionString, StoreEventProcessor.StorageConnectionString, "messages-events");
      Console.WriteLine("Registering EventProcessor...");
      eventProcessorHost.RegisterEventProcessorAsync<StoreEventProcessor>().Wait();

      Console.WriteLine("Receiving. Press enter key to stop worker.");
      Console.ReadLine();
      eventProcessorHost.UnregisterEventProcessorAsync().Wait();
    }
    ```

    > [AZURE.NOTE] Az egyszerűség kedvéért ebben az oktatóanyagban használja a [EventProcessorHost] osztály egyetlen példányát. További tudnivalókért lásd: az [Esemény hubok programozási útmutató].

## <a name="receive-interactive-messages"></a>Interaktív hibaüzenetek
Ebben a részben egy Windows konzol alkalmazás, amely az interaktív üzenetek kapja a szolgáltatás Bus sorból írni. Tervezővel használ szolgáltatási Bus megoldást kapcsolatos további tudnivalókért olvassa el a [szolgáltatás Bus a több szálon futó-alkalmazások készítése][]című témakört.

1. A jelenlegi Visual Studio megoldás vizuális C# Windows projektet létrehozni a **New** project-sablon segítségével. A projekt **ProcessD2CInteractiveMessages**neve.

2. Kattintson a jobb gombbal a **ProcessD2CInteractiveMessages** projekt megoldás Explorerben, és válassza a **NuGet csomagok kezelése**. Ez a művelet a **NuGet csomag kezelő** ablakban jeleníti meg.

3. **WindowsAzure.ServiceBus**keresni, kattintson a **telepítés**gombra, és fogadja el a használati feltételeket. Ez a művelet letöltések, telepíti, és hozzáadja a az [Azure Service Bus](https://www.nuget.org/packages/WindowsAzure.ServiceBus), az összes függőség mutató hivatkozás.

4. Adja hozzá a következő **használata** kimutatások **Program.cs** fájl tetején:

    ```
    using System.IO;
    using Microsoft.ServiceBus.Messaging;
    ```

5. Végül adja hozzá a következő sort a **fő** módszerrel. A kapcsolati karakterlánc engedélyekkel **meghallgatása** a **d2ctutorial**nevű várólista helyettesítő:

    ```
    Console.WriteLine("Process D2C Interactive Messages app\n");

    string connectionString = "{service bus listen connection string}";
    QueueClient Client = QueueClient.CreateFromConnectionString(connectionString);

    OnMessageOptions options = new OnMessageOptions();
    options.AutoComplete = false;
    options.AutoRenewTimeout = TimeSpan.FromMinutes(1);

    Client.OnMessage((message) =>
    {
      try
      {
        var bodyStream = message.GetBody<Stream>();
        bodyStream.Position = 0;
        var bodyAsString = new StreamReader(bodyStream, Encoding.ASCII).ReadToEnd();

        Console.WriteLine("Received message: {0} messageId: {1}", bodyAsString, message.MessageId);

        message.Complete();
      }
      catch (Exception)
      {
        message.Abandon();
      }
    }, options);

    Console.WriteLine("Receiving interactive messages from SB queue...");
    Console.WriteLine("Press any key to exit.");
    Console.ReadLine();
    ```

## <a name="run-the-applications"></a>Az alkalmazások futtatása

Most már készen áll az alkalmazások futtatásához.

1.  A Visual Studióban, a megoldást Intézőben kattintson a jobb gombbal a megoldás, és válassza az **Indítás projektek beállítása**. Jelöljön ki **több indítási projekt**, majd **Indítsa el** a **ProcessDeviceToCloudMessages** **SimulatedDevice**és **ProcessD2CInteractiveMessages** projektek műveletként.

2.  Nyomja le az **F5 billentyűparancs hatására** a három konzol alkalmazások indításához. A **ProcessD2CInteractiveMessages** alkalmazás interaktív **SimulatedDevice** alkalmazásból küldött üzenetek kell dolgoznia.

  ![Három kezelőkonzol-alkalmazások][50]

> [AZURE.NOTE] A blob a módosítások megjelenítéséhez szükség lehet a **StoreEventProcessor** osztály **MAX_BLOCK_SIZE** állandó csökkentése egy kisebb értéket, például **1024**. Ez a változás akkor lehet hasznos, mert egy kis időt, a továbbfejlesztett fájlblokkolás méretkorlátok vonatkoznak azokra a szimulált eszköz által küldött adatok eléréséhez szükséges időt. A továbbfejlesztett fájlblokkolás kisebb méretű megjelenítéséhez, nem kell olyan sokáig várja meg, amíg a blob létrehozott és a frissített. Jó helyen jár nagyobb méretű blokkokból álló révén az alkalmazás további méretezhető.

## <a name="next-steps"></a>Következő lépések

Ebben az oktatóanyagban adatpont és interaktív eszköz a felhőbe üzenetek megbízható dolgozhat a [EventProcessorHost] osztály használatával hogyan megtanulta azt.

A [felhő-eszköz üzenetek IoT központi üzenetküldés] [ lnk-c2d] megtudhatja, hogy miként eszközén a hátsó végéről üzeneteket küldeni.

Példák a teljes végpont megoldások, amelyek IoT központi megtekintéséhez kattintson a [Azure IoT csomagja][lnk-suite].

IoT központi megoldások fejlesztésével kapcsolatos további információért lásd: a [IoT központi Fejlesztőeszközök útmutató].

<!-- Images. -->
[50]: ./media/iot-hub-csharp-csharp-process-d2c/run1.png
[10]: ./media/iot-hub-csharp-csharp-process-d2c/create-identity-csharp1.png

[30]: ./media/iot-hub-csharp-csharp-process-d2c/createqueue2.png
[31]: ./media/iot-hub-csharp-csharp-process-d2c/createqueue3.png
[32]: ./media/iot-hub-csharp-csharp-process-d2c/createqueue4.png

<!-- Links -->

[Azure blob-tárolóhoz]: ../storage/storage-dotnet-how-to-use-blobs.md
[Azure Data Factory]: https://azure.microsoft.com/documentation/services/data-factory/
[HDInsight (Hadoop)]: https://azure.microsoft.com/documentation/services/hdinsight/
[Szolgáltatás Bus várólista]: ../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md

[Azure IoT központi Fejlesztőeszközök útmutató - eszköz a felhőbe]: iot-hub-devguide-messaging.md

[Azure tárhely]: https://azure.microsoft.com/documentation/services/storage/
[Azure Service Bus]: https://azure.microsoft.com/documentation/services/service-bus/

[IoT központi Fejlesztőeszközök útmutató]: iot-hub-devguide.md
[Első lépések a IoT központi]: iot-hub-csharp-csharp-getstarted.md
[Azure IoT Developer Center]: https://azure.microsoft.com/develop/iot
[lnk-service-fabric]: https://azure.microsoft.com/documentation/services/service-fabric/
[lnk-stream-analytics]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-event-hubs]: https://azure.microsoft.com/documentation/services/event-hubs/
[Ideiglenes (tranziens) hibafa kezelése]: https://msdn.microsoft.com/library/hh675232.aspx

<!-- Links -->
[Azure tároló kapcsolatban]: ../storage/storage-create-storage-account.md#create-a-storage-account
[Esemény hubok – első lépések]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[Azure tároló méretezhetőség útmutató]: ../storage/storage-scalability-targets.md
[Azure Block Blobs]: https://msdn.microsoft.com/library/azure/ee691964.aspx
[Event Hubs]: ../event-hubs/event-hubs-overview.md
[EventProcessorHost]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventprocessorhost(v=azure.95).aspx
[Esemény hubok programozási útmutató]: ../event-hubs/event-hubs-programming-guide.md
[Ideiglenes (tranziens) hibafa kezelése]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[A szolgáltatás Bus a több szálon futó alkalmazások készítése]: ../service-bus-messaging/service-bus-dotnet-multi-tier-app-using-service-bus-queues.md

[lnk-classic-portal]: https://manage.windowsazure.com
[lnk-c2d]: iot-hub-csharp-csharp-process-d2c.md
[lnk-suite]: https://azure.microsoft.com/documentation/suites/iot-suite/
