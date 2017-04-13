## <a name="receive-messages-with-eventprocessorhost-in-java"></a>Java EventProcessorHost és hibaüzenetek

EventProcessorHost Java osztály esemény hubok által irányító állandó pontjainak fogadó eseményeinek megkönnyítő és párhuzamosan kap e esemény hubok. EventProcessorHost használatával, feloszthatja események keresztül több vevők, akkor is, ha a csomópontjai is. Ez a példa bemutatja, hogyan EventProcessorHost használata egy egyetlen címzett.

###<a name="create-a-storage-account"></a>Tárterület-fiók létrehozása

EventProcessorHost használatához, az [Azure tárterület-fiókkal][]kell rendelkeznie:

1. Jelentkezzen be az [Azure klasszikus portálra][], és kattintson az **Új** elemre a képernyő alján.

2. Kattintson a **Data Services**, **tárterület**, majd a **Gyors létrehozása**, és írjon be egy nevet a tárterület-fiók. Válassza ki a kívánt területet, és kattintson a **Tárterület-fiók létrehozása**gombra.

    ![][11]

3. Jelölje ki az újonnan létrehozott tárterület-fiókját, és kattintson a **Hívóbetűk kezelése**:

    ![][12]

    Másolja az elsődleges hívóbetű később az oktatóprogram használni.

###<a name="create-a-java-project-using-the-eventprocessor-host"></a>Használja a EventProcessor Host Java projekt létrehozása

Az esemény hubok Java ügyfél tárat érhető el az a [Központi adattárban maven tesztelése]maven tesztelése projektekben használható[Maven Package], és használja a következő típusú függés nyilatkozat belül a maven tesztelése projektfájlba hivatkozhat:    

``` XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventhubs</artifactId>
    <version>{VERSION}</version>
</dependency>
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventhubs-eph</artifactId>
    <version>{VERSION}</version>
</dependency>
```
 
A különböző típusú build környezetekben, explicit módon letöltheti a legújabb üveg fájlokat megjelent a [Központi adattárban maven tesztelése] [ Maven Package] vagy [GitHub megjelenés terjesztési](https://github.com/Azure/azure-event-hubs/releases)pontról.  

1. Az alábbi példa az előbb létre új maven tesztelése projekt konzol/rendszerhéj alkalmazáshoz a kedvenc Java fejlesztői környezet. Az osztály neve lehet ```ErrorNotificationHandler```.     

    ``` Java
    import java.util.function.Consumer;
    import com.microsoft.azure.eventprocessorhost.ExceptionReceivedEventArgs;

    public class ErrorNotificationHandler implements Consumer<ExceptionReceivedEventArgs>
    {
        @Override
        public void accept(ExceptionReceivedEventArgs t)
        {
            System.out.println("SAMPLE: Host " + t.getHostname() + " received general error notification during " + t.getAction() + ": " + t.getException().toString());
        }
    }
    ```

2. Az alábbi kód használatával hozzon létre egy új nevű osztály ```EventProcessor```.

    ```Java
    import com.microsoft.azure.eventhubs.EventData;
    import com.microsoft.azure.eventprocessorhost.CloseReason;
    import com.microsoft.azure.eventprocessorhost.IEventProcessor;
    import com.microsoft.azure.eventprocessorhost.PartitionContext;

    public class EventProcessor implements IEventProcessor
    {
        private int checkpointBatchingCount = 0;

        @Override
        public void onOpen(PartitionContext context) throws Exception
        {
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " is opening");
        }

        @Override
        public void onClose(PartitionContext context, CloseReason reason) throws Exception
        {
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " is closing for reason " + reason.toString());
        }
        
        @Override
        public void onError(PartitionContext context, Throwable error)
        {
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " onError: " + error.toString());
        }

        @Override
        public void onEvents(PartitionContext context, Iterable<EventData> messages) throws Exception
        {
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " got message batch");
            int messageCount = 0;
            for (EventData data : messages)
            {
                System.out.println("SAMPLE (" + context.getPartitionId() + "," + data.getSystemProperties().getOffset() + "," +
                        data.getSystemProperties().getSequenceNumber() + "): " + new String(data.getBody(), "UTF8"));
                messageCount++;
                
                this.checkpointBatchingCount++;
                if ((checkpointBatchingCount % 5) == 0)
                {
                    System.out.println("SAMPLE: Partition " + context.getPartitionId() + " checkpointing at " +
                        data.getSystemProperties().getOffset() + "," + data.getSystemProperties().getSequenceNumber());
                    context.checkpoint(data);
                }
            }
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " batch size was " + messageCount + " for host " + context.getOwner());
        }
    }
    ```

3. Hozzon létre egy újat végleges osztály meghívása ```EventProcessorSample```, a következő kód használatával.

    ```Java
    import com.microsoft.azure.eventprocessorhost.*;
    import com.microsoft.azure.servicebus.ConnectionStringBuilder;
    import com.microsoft.azure.eventhubs.EventData;

    public class EventProcessorSample
    {
        public static void main(String args[])
        {
            final String consumerGroupName = "$Default";
            final String namespaceName = "----ServiceBusNamespaceName-----";
            final String eventHubName = "----EventHubName-----";
            final String sasKeyName = "-----SharedAccessSignatureKeyName-----";
            final String sasKey = "---SharedAccessSignatureKey----";

            final String storageAccountName = "---StorageAccountName----";
            final String storageAccountKey = "---StorageAccountKey----";
            final String storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=" + storageAccountName + ";AccountKey=" + storageAccountKey;
            
            ConnectionStringBuilder eventHubConnectionString = new ConnectionStringBuilder(namespaceName, eventHubName, sasKeyName, sasKey);
            
            EventProcessorHost host = new EventProcessorHost(eventHubName, consumerGroupName, eventHubConnectionString.toString(), storageConnectionString);
            
            System.out.println("Registering host named " + host.getHostName());
            EventProcessorOptions options = new EventProcessorOptions();
            options.setExceptionNotification(new ErrorNotificationHandler());
            try
            {
                host.registerEventProcessor(EventProcessor.class, options).get();
            }
            catch (Exception e)
            {
                System.out.print("Failure while registering: ");
                if (e instanceof ExecutionException)
                {
                    Throwable inner = e.getCause();
                    System.out.println(inner.toString());
                }
                else
                {
                    System.out.println(e.toString());
                }
            }

            System.out.println("Press enter to stop");
            try
            {
                System.in.read();
                host.unregisterEventProcessor();
                
                System.out.println("Calling forceExecutorShutdown");
                EventProcessorHost.forceExecutorShutdown(120);
            }
            catch(Exception e)
            {
                System.out.println(e.toString());
                e.printStackTrace();
            }
            
            System.out.println("End of sample");
        }
    }
    ```

4. Cserélje ki az értékeket az esemény központi és a tárterület-fiók létrehozásakor használja az alábbi mezőket.

    ``` Java
    final String namespaceName = "----ServiceBusNamespaceName-----";
    final String eventHubName = "----EventHubName-----";

    final String sasKeyName = "-----SharedAccessSignatureKeyName-----";
    final String sasKey = "---SharedAccessSignatureKey----";

    final String storageAccountName = "---StorageAccountName----"
    final String storageAccountKey = "---StorageAccountKey----";
    ```

> [AZURE.NOTE] Ebben az oktatóanyagban EventProcessorHost egyetlen példányát használja. Átviteli növeléséhez ajánlott EventProcessorHost több példányával futtatott. Ezekben az esetekben az egyes példányok automatikusan a többi terheléselosztó érdekében a kapott rendezvények koordinálása. Ha több vevők minden folyamat *minden* az események, amely **ConsumerGroup** kell használnia. A különböző számítógépeken események fogadásakor gépek (vagy szerepkör) alapján EventProcessorHost előfordulását nevének megadása az általuk rendszerbe célszerű lehet.

<!-- Links -->
[Event Hubs overview]: ../articles/event-hubs/event-hubs-overview.md
[Azure tárterület-fiók]: ../articles/storage/storage-create-storage-account.md
[Azure klasszikus portál]: http://manage.windowsazure.com
[Maven Package]: https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs-eph%22

<!-- Images -->
[11]: ./media/service-bus-event-hubs-get-started-receive-ephjava/create-eph-csharp2.png
[12]: ./media/service-bus-event-hubs-get-started-receive-ephjava/create-eph-csharp3.png

