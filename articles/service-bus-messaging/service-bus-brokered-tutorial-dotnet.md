<properties 
    pageTitle="Szolgáltatás Bus brokered üzenetben .NET oktatóprogram |} Microsoft Azure"
    description="Üzenetek .NET oktatóprogram brokered."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="09/27/2016"
    ms.author="sethm" />

# <a name="service-bus-brokered-messaging-net-tutorial"></a>Szolgáltatás Bus brokered üzenetben .NET oktatóprogram

Azure Service Bus megoldásokat két teljes üzenetben – egy, a felhőben, számos különböző protokollokat és webes támogató operációs rendszert futtató központi "továbbítási" szolgáltatáson keresztül szolgáltatások szabványok, beleértve a SOAP WS-*, és a többi. Az ügyfél, a helyszíni szolgáltatás közvetlen kapcsolat nem szükséges, sem a felhasználónak meg kell, hogy hol található a szolgáltatást, és a helyszíni szolgáltatás nem kell minden bejövő portokat nyitva a tűzfalon keresztül.

A második üzenetben megoldást lehetővé teszi, hogy a "brokered" üzenetkezelő lehetőségeit. Ezek az aszinkron olyan, vagy leválasztott támogatási közzététel előfizetés üzenetben funkciói időbeli szétválasztás és terheléselosztási esetek a szolgáltatás Bus üzenetkezelési infrastruktúra használatával. Leválasztott kommunikációs előnyökkel jár sok; például ügyfelek és -kiszolgálók is szükség szerint csatlakozás, és hajtsa végre a műveletek aszinkron módon.

Ebben az oktatóprogramban az célja, hogy egy – áttekintés és a sorok rutinszerzéshez ad, szolgáltatás Bus fő összetevői közül brokered üzenetküldés. Ebben az oktatóanyagban keresztül témakörök sorozatában dolgozik, után lesz üzenetek feltölti létrehoz egy várólista és üzeneteket küld az adott várólista alkalmazást. Végül az alkalmazás megkapja a sorból üzeneteket jeleníti meg, majd törli a köteggel az erőforrások és a Kilépés. A szolgáltatás Bus továbbítási használó alkalmazás létrehozásának ismerteti, hogy megfelelő oktatóanyagban [szolgáltatás Bus továbbítását üzenetben oktatóprogram](../service-bus-relay/service-bus-relay-tutorial.md)című témakör tartalmaz.

## <a name="introduction-and-prerequisites"></a>– Bevezetés és a vonatkozó követelmények

Sorok tud nyújtani első, első ki (FIFO) üzenetkézbesítés egy vagy több versengő felhasználóknak. FIFO, az azt jelenti, hogy üzeneteket általában várhatóan kapott, és a címzett, a időbeli sorrendben, amelyben sorbaállítva voltak, és minden üzenet fogad és csak egy üzenet fogyasztói által feldolgozott által feldolgozott. Egy sorban várakozó használatával előnye *időbeli szétválasztás* alkalmazás összetevők eléréséhez: Ez azt jelenti, a gyártók és a fogyasztói nem kell kell üzenetek küldése és fogadása egyszerre, mivel az üzenetek tartósan kerülnek a várakozási sorban található. A kapcsolódó előnye *terheléselosztás*, amely lehetővé teszi a gyártók és fogyasztói küldhet és fogadhat üzeneteket különböző kamatlába mellett.

Az alábbiakban néhány az oktatóprogram megkezdése előtt célszerű követnie igazgatási és előzetesen szükséges lépéseket. Az első szolgáltatás névtér létrehozása, de egy megosztott aláírás (Társítások) hívóbetű juthat. Névtér az alkalmazás oszlopazonosító nyújt a minden alkalmazás szolgáltatás Bus keresztül elérhetővé tett. Társítások kulcs automatikusan generált, szolgáltatás névteret létrehozásakor. Társítások kulcs szolgáltatás névtere, ha a hitelesítő adatok, amellyel szolgáltatás Bus hitelesíti az alkalmazás hozzáférést biztosít.

### <a name="create-a-service-namespace-and-obtain-a-sas-key"></a>Szolgáltatás névtér létrehozása és Társítások kulcs beszerzése

Első lépésként szolgáltatás névtér létrehozása, és szerezze be a [Megosztott Access-aláírás](service-bus-sas-overview.md) (Társítások) billentyűt. Névtér az alkalmazás oszlopazonosító nyújt a minden alkalmazás szolgáltatás Bus keresztül elérhetővé tett. Társítások kulcs automatikusan generált, szolgáltatás névteret létrehozásakor. Társítások kulcs szolgáltatás névtere, ha egy hitelesítőadat-szolgáltatás Bus hitelesítést végezni az alkalmazás hozzáférést biztosít.

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

A következő lépésben, hogy a Visual Studio projekt létrehozása, és írja be az üzenetek vesszővel tagolt listáját betöltése erősen beírta [BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx) .NET [lista](https://msdn.microsoft.com/library/6sh2ey19.aspx) objektum két segítő függvényeket.

### <a name="create-a-visual-studio-project"></a>Visual Studio projekt létrehozása

1. Nyissa meg a Visual Studio rendszergazdaként jobb gombbal a program a Start menüben kattintson a **Futtatás rendszergazdaként**parancsra.

1. Konzol alkalmazás új projekt létrehozása. Kattintson a **fájl** menüre, majd válassza az **Új** **Projekt**lehetőségre. Kattintson az **Új projekt** párbeszédpanel **Visual C#** (Ha **Visual C#** nem jelenik meg, keresse a **Más nyelven**), kattintson a **New** sablonra, és nevezze el **QueueSample**. Használja az alapértelmezett **helyre**. Kattintson az **OK gombra** a projekt létrehozása.

1. A NuGet csomag kezelő segítségével a szolgáltatás Bus tárak hozzáadása a projekthez:
    1. A megoldás Intézőben kattintson a jobb gombbal a **QueueSample** projekt, és kattintson a **NuGet csomagok kezelése**gombra.
    2. **Nuget csomagok kezelése** párbeszédpanelen kattintson a **Tallózás** lap **Azure Service Bus**keres, majd kattintson a **telepítés**gombra.
<br />
1. A megoldás Intézőben és a Program.cs fájlra duplán kattintva nyissa meg a Visual Studio-szerkesztőben. A névtér neve átállítása az alapértelmezett nevének `QueueSample` való `Microsoft.ServiceBus.Samples`.

    ```
    Microsoft.ServiceBus.Samples
    {
        ...
    ```

1. Módosítsa a `using` kimutatások, ahogy az alábbi kódot.

    ```
    using System;
    using System.Collections.Generic;
    using System.Data;
    using System.IO;
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.ServiceBus.Messaging;
    ```

1. Hozzon létre egy szövegfájlt, nevű Data.csv, és a következő vesszővel tagolt szöveg másolása.

    ```
    IssueID,IssueTitle,CustomerID,CategoryID,SupportPackage,Priority,Severity,Resolved
    1,Package lost,1,1,Basic,5,1,FALSE
    2,Package damaged,1,1,Basic,5,1,FALSE
    3,Product defective,1,2,Premium,5,2,FALSE
    4,Product damaged,2,2,Premium,5,2,FALSE
    5,Package lost,2,2,Basic,5,2,TRUE
    6,Package lost,3,2,Basic,5,2,FALSE
    7,Package damaged,3,7,Premium,5,3,FALSE
    8,Product defective,3,2,Premium,5,3,FALSE
    9,Product damaged,4,6,Premium,5,3,TRUE
    10,Package lost,4,8,Basic,5,3,FALSE
    11,Package damaged,5,4,Basic,5,4,FALSE
    12,Product defective,5,4,Basic,5,4,FALSE
    13,Package lost,6,8,Basic,5,4,FALSE
    14,Package damaged,6,7,Premium,5,5,FALSE
    15,Product defective,6,2,Premium,5,5,FALSE
    ```

    Mentse és zárja be a Data.csv fájlt, és ne feledje, hogy a helyet, amelybe mentette azt.

1. A megoldás Intézőben kattintson a jobb gombbal (a példában a **QueueSample**), a projekt nevére kattintson a **Hozzáadás**gombra, majd kattintson a **Meglévő elemet**.

1. Keresse meg az Ön által létrehozott a 6 Data.csv fájlt. Kattintson a Fájl fülre, majd kattintson a **Hozzáadás**gombra. Győződjön meg arról, hogy * *az összes fájl (*.*) ** van jelölve, a fájltípusok listájában.

### <a name="create-a-method-that-parses-a-list-of-messages"></a>Hozzon létre egy mód, elemzi a üzenetek

1. Az a `Program` előtt osztály a `Main()` módszer két változó deklarálása: egyik típus **adattábla**Data.csv az üzenetek listáját tartalmazza. Objektumtípus listában, való [BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx)erősen beírta a másik kell lenniük. Az utóbbi az üzenetlista brokered, amelyekkel a későbbi az oktatóprogram lépéseit.

    ```
    namespace Microsoft.ServiceBus.Samples
    {
        class Program
        {
    
            private static DataTable issues;
            private static List<BrokeredMessage> MessageList;
    ```

1. Külső `Main()`, megadása egy `ParseCSV()` megadott elemzi a Data.csv üzenetek listáját, és betölti az üzenetek [adattábla](https://msdn.microsoft.com/library/azure/system.data.datatable.aspx) táblába, mint az ábrán. A módszer egy **adattábla** objektum adja eredményül.

    ```
    static DataTable ParseCSVFile()
    {
        DataTable tableIssues = new DataTable("Issues");
        string path = @"..\..\data.csv";
        try
        {
            using (StreamReader readFile = new StreamReader(path))
            {
                string line;
                string[] row;
    
                // create the columns
                line = readFile.ReadLine();
                foreach (string columnTitle in line.Split(','))
                {
                    tableIssues.Columns.Add(columnTitle);
                }
    
                while ((line = readFile.ReadLine()) != null)
                {
                    row = line.Split(',');
                    tableIssues.Rows.Add(row);
                }
            }
        }
        catch (Exception e)
        {
            Console.WriteLine("Error:" + e.ToString());
        }
    
        return tableIssues;
    }
    ```

1. Az a `Main()` módszer felvétele meghívó kimutatást a `ParseCSVFile()` módszer:

    ```
    public static void Main(string[] args)
    {
    
        // Populate test data
        issues = ParseCSVFile();
    
    }
    ```

### <a name="create-a-method-that-loads-the-list-of-messages"></a>Az üzenetlista betöltő módszer létrehozása

1. Külső `Main()`, megadása egy `GenerateMessages()` módszer, hogy mi az **adattábla** objektum által visszaadott `ParseCSVFile()` és a táblázat betölti erősen beírta brokered üzenetek listába. A módszer a **lista** -objektumra, ahogy az alábbi példa adja vissza. 

    ```
    static List<BrokeredMessage> GenerateMessages(DataTable issues)
    {
        // Instantiate the brokered list object
        List<BrokeredMessage> result = new List<BrokeredMessage>();
    
        // Iterate through the table and create a brokered message for each row
        foreach (DataRow item in issues.Rows)
        {
            BrokeredMessage message = new BrokeredMessage();
            foreach (DataColumn property in issues.Columns)
            {
                message.Properties.Add(property.ColumnName, item[property]);
            }
            result.Add(message);
        }
        return result;
    }
    ```

1. A `Main()`, közvetlenül a hívást után `ParseCSVFile()`, felvétele meghívó kimutatást a `GenerateMessages()` módszer a feladó mezőbe írja be a `ParseCSVFile()` argumentumaként:

    ```
    public static void Main(string[] args)
    {
    
        // Populate test data
        issues = ParseCSVFile();
        MessageList = GenerateMessages(issues);
    }
    ```

### <a name="obtain-user-credentials"></a>Felhasználói hitelesítő adatok beszerzése

1. Első lépésként hozzon létre három globális karakterlánc változót is tartsa lenyomva az ujját ezeket az értékeket. Ezek a változók deklarálhatnak közvetlenül az előző változó deklarációs; után Példa:

    ```
    namespace Microsoft.ServiceBus.Samples
    {
        public class Program
        {
    
            private static DataTable issues;
            private static List<BrokeredMessage> MessageList; 

            // Add these variables
            private static string ServiceNamespace;
            private static string sasKeyName = "RootManageSharedAccessKey";
            private static string sasKeyValue;
            …
    ```

1. Ezután hozzon létre egy funkció, amely elfogad és tárolja a szolgáltatás névtere és Társítások billentyűt. Ez a módszer kívüli hozzáadása `Main()`. Példa: 

    ```
    static void CollectUserInput()
    {
        // User service namespace
        Console.Write("Please enter the namespace to use: ");
        ServiceNamespace = Console.ReadLine();
    
        // Issuer key
        Console.Write("Enter the SAS key to use: ");
        sasKeyValue = Console.ReadLine();
    }
    ```

1. A `Main()`, közvetlenül a hívást után `GenerateMessages()`, felvétele meghívó kimutatást a `CollectUserInput()` módszer:

    ```
    public static void Main(string[] args)
    {
    
        // Populate test data
        issues = ParseCSVFile();
        MessageList = GenerateMessages(issues);
        
        // Collect user input
        CollectUserInput();
    }
    ```

### <a name="build-the-solution"></a>A megoldás összeállítása

A Visual Studio **Build** menüben kattintson a **Szerkesztés megoldást** , vagy nyomja le a **Ctrl + Shift + B** kattintva erősítse meg, amennyiben a munka pontosságát.

## <a name="create-management-credentials"></a>Adatkezelési hitelesítő adatok létrehozása

Ebben a lépésben az adatkezelési műveletek átengedése aláírás (Társítások) hitelesítő adatait, amelyeknek a az alkalmazás engedélyezni kell létrehozásához használandó határozza meg.

1. Áttekinthetőség érdekében a elvégezze ebben az oktatóprogramban egy külön módszer helyezi. Hozzon létre egy aszinkron `Queue()` módszer a `Program` osztály után a `Main()` módot. Példa:
 
    ```
    public static void Main(string[] args)
    {
    …
    }
    static async Task Queue()
    {
    }
    ```

1. A következő lépésként hozzon létre egy Társítások hitelesítő adatok használata egy [TokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.tokenprovider.aspx) objektumot. A létrehozás módszer veszi a Társítások kulcs nevét, és a kapott értéket a `CollectUserInput()` módot. Adja hozzá a következő kódot a `Queue()` módszer:

    ```
    static async Task Queue()
    {
        // Create management credentials
        TokenProvider credentials = TokenProvider.CreateSharedAccessSignatureTokenProvider(sasKeyName,sasKeyValue);
    }
    ```

2. Hozzon létre egy új névtér management objektumot, amelyben a névtér neve és az előző lépésben argumentumként kapott hitelesítő adatok kezelése URI. Ez a kód hozzáadása a közvetlenül a kódot hozzáadni az előző lépésben után. Győződjön meg arról, hogy le szeretné cserélni `<yourNamespace>` a szolgáltatás névtere a nevet:
    
    ```
    NamespaceManager namespaceClient = new NamespaceManager(ServiceBusEnvironment.CreateServiceUri("sb", "<yourNamespace>", string.Empty), credentials);
    ```

### <a name="example"></a>Példa

A kód ezen a ponton a következőhöz hasonlóan kell kinéznie:

```
using System;
using System.Collections.Generic;
using System.Data;
using System.IO;
using System.Threading;
using System.Threading.Tasks;
using Microsoft.ServiceBus.Messaging;

namespace Microsoft.ServiceBus.Samples
{
  public class Program
  {
    private static DataTable issues;
    private static List<BrokeredMessage> MessageList;
    private static string ServiceNamespace;
    private static string sasKeyName = "RootManageSharedAccessKey";
    private static string sasKeyValue;

    static void Main(string[] args)
    {
      // Populate test data
      issues = ParseCSVFile();
      MessageList = GenerateMessages(issues);

      // Collect user input
      CollectUserInput();

      // Add this call
      Task.WaitAll(Queue());
    }

    static async Task Queue()
    {
      // Create management credentials
      TokenProvider credentials = TokenProvider.CreateSharedAccessSignatureTokenProvider(sasKeyName, sasKeyValue);
      NamespaceManager namespaceClient = new NamespaceManager(ServiceBusEnvironment.CreateServiceUri("sb", "<yourNamespace>", string.Empty), credentials);
    }

    static DataTable ParseCSVFile()
    {
      DataTable tableIssues = new DataTable("Issues");
      string path = @"..\..\data.csv";
      try
      {
        using (StreamReader readFile = new StreamReader(path))
        {
          string line;
          string[] row;

          // create the columns
          line = readFile.ReadLine();
          foreach (string columnTitle in line.Split(','))
          {
            tableIssues.Columns.Add(columnTitle);
          }

          while ((line = readFile.ReadLine()) != null)
          {
            row = line.Split(',');
            tableIssues.Rows.Add(row);
          }
        }
      }
      catch (Exception e)
      {
        Console.WriteLine("Error:" + e.ToString());
      }

      return tableIssues;
    }

    static List<BrokeredMessage> GenerateMessages(DataTable issues)
    {
      // Instantiate the brokered list object
      List<BrokeredMessage> result = new List<BrokeredMessage>();

      // Iterate through the table and create a brokered message for each row
      foreach (DataRow item in issues.Rows)
      {
        BrokeredMessage message = new BrokeredMessage();
        foreach (DataColumn property in issues.Columns)
        {
          message.Properties.Add(property.ColumnName, item[property]);
        }
        result.Add(message);
      }
      return result;
    }

    static void CollectUserInput()
    {
      // User service namespace
      Console.Write("Please enter the service namespace to use: ");
      ServiceNamespace = Console.ReadLine();

      // Issuer key
      Console.Write("Please enter the issuer key to use: ");
      sasKeyValue = Console.ReadLine();
    }
  }
}
```

## <a name="send-messages-to-the-queue"></a>A várakozási sorban található üzenetek küldése

Ebben a lépésben várólista létrehozása, majd küldje el az üzeneteket az adott sorba brokered üzenetek listában szereplő.

### <a name="create-queue-and-send-messages-to-the-queue"></a>Várólista létrehozása és üzeneteket küldeni a sor

1. Első lépésként hozzon létre a várakozási sorban található. Ha például hívja meg `myQueue`, és bejelenteni, közvetlenül az adatkezelési műveletek felvett után a `Queue()` módszer az utolsó lépésben:

    ```
    QueueDescription myQueue;

    if (namespaceClient.QueueExists("IssueTrackingQueue"))
    {
        namespaceClient.DeleteQueue("IssueTrackingQueue");
    }

    myQueue = namespaceClient.CreateQueue("IssueTrackingQueue");
    ```

1. Az a `Queue()` módszer, egy üzenetben factory-objektum létrehozása az újonnan létrehozott szolgáltatás Bus URI argumentumaként. Adja hozzá a következő kódot közvetlenül az adatkezelési műveletek az utolsó lépésben felvett után. Győződjön meg arról, hogy le szeretné cserélni `<yourNamespace>` a szolgáltatás névtere a nevet:

    ```
    MessagingFactory factory = MessagingFactory.Create(ServiceBusEnvironment.CreateServiceUri("sb", "<yourNamespace>", string.Empty), credentials);
    ```

1. Ezután hozzon létre a [QueueClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx) osztály használatával várólista-objektum. Adja hozzá a következő kódot közvetlenül után a hozzáadott az utolsó lépésben kódot:

    ```
    QueueClient myQueueClient = factory.CreateQueueClient("IssueTrackingQueue");
    ```

1. Ezután adja hozzá a kódot, amely a korábban létrehozott, Küldés a sorba minden brokered üzenetek között hurkok. Adja hozzá a következő kódot közvetlenül után a `CreateQueueClient()` utasítás az előző lépésben:
    
    ```
    // Send messages
    Console.WriteLine("Now sending messages to the queue.");
    for (int count = 0; count < 6; count++)
    {
        var issue = MessageList[count];
        issue.Label = issue.Properties["IssueTitle"].ToString();
        await myQueueClient.SendAsync(issue);
        Console.WriteLine(string.Format("Message sent: {0}, {1}", issue.Label, issue.MessageId));
    }
    ```

## <a name="receive-messages-from-the-queue"></a>A várakozási sorban található érkező üzenetek fogadására

Ebben a lépésben a sorból az előző lépésben létrehozott fog kapni az üzenetek listáját.

### <a name="create-a-receiver-and-receive-messages-from-the-queue"></a>Hozzon létre egy címzett és a várólista érkező üzenetek fogadására

Az a `Queue()` módszer, a sor Végigléptetés, és a jelennek meg a nyomtatás, a konzol minden üzenet [QueueClient.ReceiveAsync](https://msdn.microsoft.com/library/azure/dn130423.aspx) módszerrel. Adja hozzá a következő kódot közvetlenül után a hozzáadott az előző lépésben kódot:

```
Console.WriteLine("Now receiving messages from Queue.");
BrokeredMessage message;
while ((message = await myQueueClient.ReceiveAsync(new TimeSpan(hours: 0, minutes: 1, seconds: 5))) != null)
    {
        Console.WriteLine(string.Format("Message received: {0}, {1}, {2}", message.SequenceNumber, message.Label, message.MessageId));
        message.Complete();
    
        Console.WriteLine("Processing message (sleeping...)");
        Thread.Sleep(1000);
    }
```

Megjegyzendő, hogy a `Thread.Sleep` az üzenet hasonlóan használható feldolgozása, és valószínűleg nem kell egy valós üzenetkezelés alkalmazásban.

### <a name="end-the-queue-method-and-clean-up-resources"></a>A várakozási módszer befejezéséhez, és erőforrások karbantartása

Közvetlenül az előző kód után adja hozzá a következő kódot karbantartása: az üzenet gyári és várólista erőforrások:

```
factory.Close();
myQueueClient.Close();
namespaceClient.DeleteQueue("IssueTrackingQueue");
```

### <a name="call-the-queue-method"></a>Hívja fel a várólista módszer

Az utolsó lépésként felvétele meghívó kimutatást a `Queue()` származó mód `Main()`. Adja hozzá a következő kijelölt sor kód Main() végén:
    
```
public static void Main(string[] args)
{
    // Collect user input
    CollectUserInput();
    
    // Populate test data
    issues = ParseCSVFile();
    MessageList = GenerateMessages(issues);
    
    // Add this call
    Queue();
}
```

### <a name="example"></a>Példa

A következő kódot a teljes **QueueSample** alkalmazást tartalmazza.

```
using System;
using System.Collections.Generic;
using System.Data;
using System.IO;
using System.Threading;
using System.Threading.Tasks;
using Microsoft.ServiceBus.Messaging;

namespace Microsoft.ServiceBus.Samples
{
    public class Program
    {
        private static DataTable issues;
        private static List<BrokeredMessage> MessageList;

        // Add these variables
        private static string ServiceNamespace;
        private static string sasKeyName = "RootManageSharedAccessKey";
        private static string sasKeyValue;

        static void Main(string[] args)
        {

            // Populate test data
            issues = ParseCSVFile();
            MessageList = GenerateMessages(issues);

            // Collect user input
            CollectUserInput();

            Queue();

        }

        static async Task Queue()
        {
            // Create management credentials
            TokenProvider credentials = TokenProvider.CreateSharedAccessSignatureTokenProvider(sasKeyName, sasKeyValue);
            NamespaceManager namespaceClient = new NamespaceManager(ServiceBusEnvironment.CreateServiceUri("sb", "<yourNamespace>", string.Empty), credentials);

            QueueDescription myQueue;

            if (namespaceClient.QueueExists("IssueTrackingQueue"))
            {
                namespaceClient.DeleteQueue("IssueTrackingQueue");
            }
            
            myQueue = namespaceClient.CreateQueue("IssueTrackingQueue");
            
            MessagingFactory factory = MessagingFactory.Create(ServiceBusEnvironment.CreateServiceUri("sb", "<yourNamespace>", string.Empty), credentials);

            QueueClient myQueueClient = factory.CreateQueueClient("IssueTrackingQueue");

            // Send messages
            Console.WriteLine("Now sending messages to the queue.");
            for (int count = 0; count < 6; count++)
            {
                var issue = MessageList[count];
                issue.Label = issue.Properties["IssueTitle"].ToString();
                await myQueueClient.SendAsync(issue);
                Console.WriteLine(string.Format("Message sent: {0}, {1}", issue.Label, issue.MessageId));
            }

            Console.WriteLine("Now receiving messages from Queue.");
            BrokeredMessage message;
            while ((message = await myQueueClient.ReceiveAsync(new TimeSpan(hours: 0, minutes: 1, seconds: 5))) != null)
            {
                Console.WriteLine(string.Format("Message received: {0}, {1}, {2}", message.SequenceNumber, message.Label, message.MessageId));
                message.Complete();

                Console.WriteLine("Processing message (sleeping...)");
                Thread.Sleep(1000);
            }

            factory.Close();
            myQueueClient.Close();
            namespaceClient.DeleteQueue("IssueTrackingQueue");


        }

        static void CollectUserInput()
        {
            // User service namespace
            Console.Write("Please enter the namespace to use: ");
            ServiceNamespace = Console.ReadLine();

            // Issuer key
            Console.Write("Enter the SAS key to use: ");
            sasKeyValue = Console.ReadLine();
        }

        static List<BrokeredMessage> GenerateMessages(DataTable issues)
        {
            // Instantiate the brokered list object
            List<BrokeredMessage> result = new List<BrokeredMessage>();

            // Iterate through the table and create a brokered message for each row
            foreach (DataRow item in issues.Rows)
            {
                BrokeredMessage message = new BrokeredMessage();
                foreach (DataColumn property in issues.Columns)
                {
                    message.Properties.Add(property.ColumnName, item[property]);
                }
                result.Add(message);
            }
            return result;
        }

        static DataTable ParseCSVFile()
        {
            DataTable tableIssues = new DataTable("Issues");
            string path = @"..\..\data.csv";
            try
            {
                using (StreamReader readFile = new StreamReader(path))
                {
                    string line;
                    string[] row;

                    // create the columns
                    line = readFile.ReadLine();
                    foreach (string columnTitle in line.Split(','))
                    {
                        tableIssues.Columns.Add(columnTitle);
                    }

                    while ((line = readFile.ReadLine()) != null)
                    {
                        row = line.Split(',');
                        tableIssues.Rows.Add(row);
                    }
                }
            }
            catch (Exception e)
            {
                Console.WriteLine("Error:" + e.ToString());
            }

            return tableIssues;
        }
    }
}
```

## <a name="build-and-run-the-queuesample-application"></a>Szerkesztés és a QueueSample alkalmazásnak a futtatására

Most, hogy a fenti lépések befejezése össze, és a **QueueSample** alkalmazásnak a futtatására.

### <a name="build-the-queuesample-application"></a>A QueueSample alkalmazás összeállítása

A Visual Studióban, kattintson a **Szerkesztés** menü **Összeállítása megoldást**, vagy nyomja le a **Ctrl + Shift + B**. Ha a hibákat tapasztal, győződjön meg arról, hogy helyesek-e a kód a kész példa az előző lépésben végén bemutatott alapján.

## <a name="next-steps"></a>Következő lépések

Ebben az oktatóanyagban mutatott létrehozásának szolgáltatás Bus ügyfél, alkalmazás vagy szolgáltatás a szolgáltatás Bus brokered üzenetben lehetőségeivel. Egy hasonló oktatóanyagban, amelyet használ szolgáltatási Bus [továbbítási](service-bus-messaging-overview.md#Relayed-messaging) [szolgáltatás Bus továbbítását üzenetben oktatóprogram](../service-bus-relay/service-bus-relay-tutorial.md)című témakör tartalmaz.

További tudnivalók a [Szolgáltatás Bus](https://azure.microsoft.com/services/service-bus/), a következő témakörökben talál.

- [Szolgáltatás Bus üzenetküldés – áttekintés](service-bus-messaging-overview.md)
- [Szolgáltatás Bus alapjai](service-bus-fundamentals-hybrid-solutions.md)
- [Szolgáltatás Bus architektúra](service-bus-architecture.md)

