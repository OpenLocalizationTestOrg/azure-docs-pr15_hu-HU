## <a name="receive-messages-with-eventprocessorhost"></a>EventProcessorHost üzenetek fogadása

[EventProcessorHost][] .NET osztály esemény hubok által irányító állandó pontjainak fogadó eseményeinek megkönnyítő és párhuzamos azokat események hubok kap. [EventProcessorHost][]használ, szétoszthatja események keresztül több vevők, akkor is, ha a különböző csomópontokat is. Ez a példa bemutatja, hogyan [EventProcessorHost][] használata egy egyetlen címzett. Az [esemény ki méretezett feldolgozása][] minta [EventProcessorHost][] használata több vevők mutatja.

[EventProcessorHost][]használatához a [Azure tárterület-fiókkal][]kell rendelkeznie:

1. Jelentkezzen be az [Azure-portálra][], és a képernyő tetején kattintson az **Új** balra a képernyő.

2. Kattintson az **adatok + tárhely**, majd kattintson a **tárterület-fiókot**.

    ![](./media/service-bus-event-hubs-getstarted-receive-ephcs/create-storage1.png)

3. Írja be a tárterület-fiók nevét a **tárterület-fiók létrehozása** lap. Válassza az Azure előfizetés erőforráscsoport és helyet, ahová az erőforrás létrehozásához. Kattintson a **Létrehozás**gombra.

    ![](./media/service-bus-event-hubs-getstarted-receive-ephcs/create-storage2.png)

4. Tároló fiókok listájában kattintson az újonnan létrehozott tárterület-fiókot.

5. Kattintson a tárterület a fiók lap, a **hívóbillentyűk**. Másolja a **azonosító1** később az oktatóprogram használandó értékét.

    ![](./media/service-bus-event-hubs-getstarted-receive-ephcs/create-storage3.png)

4. A Visual Studióban vizuális C# asztali alkalmazás a **New** project-sablon használata új projekt létrehozása A projekt **címzett**neve.

    ![](./media/service-bus-event-hubs-getstarted-receive-ephcs/create-receiver-csharp1.png)

5. A megoldás Intézőben kattintson a jobb gombbal a megoldást, és kattintson a **Megoldás NuGet csomagok kezelése**.

6. Kattintson a **Tallózás** fülre, majd keressen `Microsoft Azure Service Bus Event Hub - EventProcessorHost`. Győződjön meg arról, hogy a projekt nevét (**címzett**) a **változatokat** mezőben megadott értékkel. Kattintson a **telepítés**gombra, és fogadja el a használati feltételeket.

    ![](./media/service-bus-event-hubs-getstarted-receive-ephcs/create-eph-csharp1.png)

    Visual Studio letöltések, telepítése, és hozzáadja a az [Azure Service Bus esemény központi - EventProcessorHost NuGet csomag](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost), az összes függőség hivatkozást.

7. Kattintson a jobb gombbal a **címzett** projektet, kattintson a **Hozzáadás**gombra, és kattintson az **Osztályjegyzetfüzet**. Nevezze el az új osztály **SimpleEventProcessor**, és kattintson a **Hozzáadás** az osztályjegyzetfüzet létrehozása gombra.

    ![](./media/service-bus-event-hubs-getstarted-receive-ephcs/create-receiver-csharp2.png)

8. Adja hozzá a következő utasítások a SimpleEventProcessor.cs fájl tetején:

    ```
    using Microsoft.ServiceBus.Messaging;
    using System.Diagnostics;
    ```

    Ezután helyettesítő osztály a szervezet számára a következő kódot:

    ```
    class SimpleEventProcessor : IEventProcessor
    {
        Stopwatch checkpointStopWatch;

        async Task IEventProcessor.CloseAsync(PartitionContext context, CloseReason reason)
        {
            Console.WriteLine("Processor Shutting Down. Partition '{0}', Reason: '{1}'.", context.Lease.PartitionId, reason);
            if (reason == CloseReason.Shutdown)
            {
                await context.CheckpointAsync();
            }
        }

        Task IEventProcessor.OpenAsync(PartitionContext context)
        {
            Console.WriteLine("SimpleEventProcessor initialized.  Partition: '{0}', Offset: '{1}'", context.Lease.PartitionId, context.Lease.Offset);
            this.checkpointStopWatch = new Stopwatch();
            this.checkpointStopWatch.Start();
            return Task.FromResult<object>(null);
        }

        async Task IEventProcessor.ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages)
        {
            foreach (EventData eventData in messages)
            {
                string data = Encoding.UTF8.GetString(eventData.GetBytes());

                Console.WriteLine(string.Format("Message received.  Partition: '{0}', Data: '{1}'",
                    context.Lease.PartitionId, data));
            }

            //Call checkpoint every 5 minutes, so that worker can resume processing from 5 minutes back if it restarts.
            if (this.checkpointStopWatch.Elapsed > TimeSpan.FromMinutes(5))
            {
                await context.CheckpointAsync();
                this.checkpointStopWatch.Restart();
            }
        }
    }
    ```

    Az osztály által a **EventProcessorHost** az esemény-központban kapott folyamat események neve. Megjegyzendő, hogy a `SimpleEventProcessor` osztály használ egy időmérője rendszeresen hívja fel az ellenőrzési módszer a **EventProcessorHost** környezetben. Ezzel biztosíthatja, hogy ha a címzett újraindítása után megszűnik a munka feldolgozási ágyazásának legfeljebb öt perc.

9. A **Program** osztály adja hozzá a következő `using` utasítás a fájl tetején:

    ```
    using Microsoft.ServiceBus.Messaging;
    ```

    Ezután cseréje a `Main` módszer a `Program` osztály esemény központi nevét és a névtér szintű kapcsolati karakterlánc, amely a korábban mentett helyettesítése, a következő kódot és a tárterület-fiók és az előző szakaszokban kimásolt billentyűt. 

    ```
    static void Main(string[] args)
    {
      string eventHubConnectionString = "{Event Hub connection string}";
      string eventHubName = "{Event Hub name}";
      string storageAccountName = "{storage account name}";
      string storageAccountKey = "{storage account key}";
      string storageConnectionString = string.Format("DefaultEndpointsProtocol=https;AccountName={0};AccountKey={1}", storageAccountName, storageAccountKey);

      string eventProcessorHostName = Guid.NewGuid().ToString();
      EventProcessorHost eventProcessorHost = new EventProcessorHost(eventProcessorHostName, eventHubName, EventHubConsumerGroup.DefaultGroupName, eventHubConnectionString, storageConnectionString);
      Console.WriteLine("Registering EventProcessor...");
      var options = new EventProcessorOptions();
      options.ExceptionReceived += (sender, e) => { Console.WriteLine(e.Exception); };
      eventProcessorHost.RegisterEventProcessorAsync<SimpleEventProcessor>(options).Wait();

      Console.WriteLine("Receiving. Press enter key to stop worker.");
      Console.ReadLine();
      eventProcessorHost.UnregisterEventProcessorAsync().Wait();
    }
    ```

> [AZURE.NOTE] Ebben az oktatóanyagban [EventProcessorHost][]egyetlen példányát használja. Növelheti a teljesítményt, ajánlott [EventProcessorHost][], több példányával, hogy futtassa a [méretezett meg az esemény feldolgozása][] minta látható módon. Ezekben az esetekben az egyes példányok automatikusan a többi terhelését szeretné a kapott rendezvények koordinálása. Ha több vevők minden folyamat *minden* az események, amely **ConsumerGroup** kell használnia. A különböző számítógépeken események fogadásakor gépek (vagy szerepkör) alapján [EventProcessorHost][] előfordulását nevének megadása az általuk rendszerbe célszerű lehet. További információt a következő témaköröket témakörökben az [Esemény hubok – áttekintés][] és [Esemény hubok programozási útmutatója][] .

<!-- Links -->
[Esemény hubok – áttekintés]: ../articles/event-hubs/event-hubs-overview.md
[Esemény hubok programozási útmutatója]: ../articles/event-hubs/event-hubs-programming-guide.md
[Esemény feldolgozása, amelyekkel méretarányos]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
[Azure tárterület-fiók]: ../articles/storage/storage-create-storage-account.md
[EventProcessorHost]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventprocessorhost(v=azure.95).aspx
[Azure portál]: https://portal.azure.com