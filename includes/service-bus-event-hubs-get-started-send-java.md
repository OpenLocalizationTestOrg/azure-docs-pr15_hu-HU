## <a name="send-messages-to-event-hubs"></a>Esemény hubok üzeneteket küldeni

Az esemény hubok Java ügyfél tárat a [Központi adattárban maven tesztelése](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22)a maven tesztelése projektekben használható, és használja a következő típusú függés nyilatkozat belül a maven tesztelése projektfájlba hivatkozhat:    

``` XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventhubs</artifactId>
    <version>{VERSION}</version>
</dependency>
```
 
Különböző típusú build környezetekben kifejezetten szerezhet be a legújabb üveg fájlokat megjelent a [Maven tesztelése központi adattárban](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22) vagy [GitHub megjelenés terjesztési](https://github.com/Azure/azure-event-hubs/releases)pontról.  

Egy egyszerű esemény közzétevő importálja a *com.microsoft.azure.eventhubs* esemény hubok ügyfél osztályok és a *com.microsoft.azure.servicebus* csomagja segédprogram osztályok, például a közös kivételeket az Azure Service Bus üzenetben ügyfél megosztott. 

Az alábbi példa az előbb létre új maven tesztelése projekt konzol/rendszerhéj alkalmazáshoz a kedvenc Java fejlesztői környezet. Az osztály neve lehet ```Send```.     

``` Java

import java.io.IOException;
import java.nio.charset.*;
import java.util.*;
import java.util.concurrent.ExecutionException;

import com.microsoft.azure.eventhubs.*;
import com.microsoft.azure.servicebus.*;

public class Send
{
    public static void main(String[] args) 
            throws ServiceBusException, ExecutionException, InterruptedException, IOException
    {
```

Cserélje ki az értékeket az esemény-központban létrehozásakor használja a névtér és az esemény központi neve.

``` Java
    final String namespaceName = "----ServiceBusNamespaceName-----";
    final String eventHubName = "----EventHubName-----";
    final String sasKeyName = "-----SharedAccessSignatureKeyName-----";
    final String sasKey = "---SharedAccessSignatureKey----";
    ConnectionStringBuilder connStr = new ConnectionStringBuilder(namespaceName, eventHubName, sasKeyName, sasKey);
```

Ezt követően létrehozása a egyes esemény alakítja egy karakterláncban a UTF-8 bájtos kódolását. Hogy a kapcsolati karakterláncot az új esemény hubok ügyfél példány létrehozása és küldje el az üzenetet.   

``` Java 
                
    byte[] payloadBytes = "Test AMQP message from JMS".getBytes("UTF-8");
    EventData sendEvent = new EventData(payloadBytes);
    
    EventHubClient ehClient = EventHubClient.createFromConnectionStringSync(connStr.toString());
    ehClient.sendSync(sendEvent);
    }
}

``` 
