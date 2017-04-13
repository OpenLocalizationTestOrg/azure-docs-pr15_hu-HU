<properties
    pageTitle="Első lépések az Azure várólista tárterület .NET |} Microsoft Azure"
    description="Azure sorban várakozó adja meg, hogy megbízható, aszinkron üzenetküldés alkalmazásösszetevők között. A felhő üzenetben lehetővé teszi, hogy az alkalmazás-összetevők, ha át kívánja méretezni, függetlenül."
    services="storage"
    documentationCenter=".net"
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/12/2016"
    ms.author="robinsh"/>

# <a name="get-started-with-azure-queue-storage-using-net"></a>Azure várólista-tároló .NET használata – első lépések

[AZURE.INCLUDE [storage-selector-queue-include](../../includes/storage-selector-queue-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>– Áttekintés

Azure várólista-tároló felhő között alkalmazásösszetevők üzenetküldés biztosít. Az alkalmazások skála tervez, alkalmazásösszetevők is gyakran leválasztott, úgy, hogy azok a független méretezheti. Várólista-tároló biztosítja, hogy aszinkron üzenetküldés alkalmazásösszetevők, közötti kommunikációhoz, hogy a felhőben, az asztalon, a helyszíni kiszolgálón vagy mobileszközön futnak. Várólista-tároló támogatja a aszinkron feladatok kezelése és a folyamat munkafolyamatok fejlesztésére is.

### <a name="about-this-tutorial"></a>Ebben az oktatóanyagban kapcsolatban

Ebből az oktatóanyagból megtudhatja, hogy miként kódírás .NET-néhány gyakori alkalmazási területek Azure várólista tároló használatával. Érintett esetek létrehozása és törlése a sorok és hozzáadásával, olvasási és üzenetek törlése.

**Becsült időt vesz igénybe:** 45 perc

**Prerequisities:**

- [Microsoft Visual Studio](https://www.visualstudio.com/en-us/visual-studio-homepage-vs.aspx)
- [Azure tároló ügyfél tár a .NET rendszerhez](https://www.nuget.org/packages/WindowsAzure.Storage/)
- [A .NET rendszerhez Azure Konfigurációkezelő](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/)
- [Azure tárterület-fiók](storage-create-storage-account.md#create-a-storage-account)


[AZURE.INCLUDE [storage-dotnet-client-library-version-include](../../includes/storage-dotnet-client-library-version-include.md)]

[AZURE.INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

[AZURE.INCLUDE [storage-development-environment-include](../../includes/storage-development-environment-include.md)]

### <a name="add-namespace-declarations"></a>Névtér deklarációs hozzáadása

Adja hozzá a következő `using` kimutatások tetején a `program.cs` fájl:

    using Microsoft.Azure; // Namespace for CloudConfigurationManager
    using Microsoft.WindowsAzure.Storage; // Namespace for CloudStorageAccount
    using Microsoft.WindowsAzure.Storage.Queue; // Namespace for Queue storage types

### <a name="parse-the-connection-string"></a>A kapcsolati karakterlánc elemzése

[AZURE.INCLUDE [storage-cloud-configuration-manager-include](../../includes/storage-cloud-configuration-manager-include.md)]

### <a name="create-the-queue-service-client"></a>A várólista szolgáltatást ügyfél létrehozása

A **CloudQueueClient** osztály lehetővé teszi, hogy várólista tárolóban lévő sorok beolvasásához. Az alábbiakban az egyik módja a szolgáltatási ügyfelet létrehozása:

    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

Most már készen áll, amely beolvassa az adatokat, és adatot ír várólista-tároló kódírás.

## <a name="create-a-queue"></a>Várólista létrehozása

Ez a példa bemutatja, hogyan várólista létrehozására, ha még nem létezik:

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a container.
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    // Create the queue if it doesn't already exist
    queue.CreateIfNotExists();

## <a name="insert-a-message-into-a-queue"></a>Üzenet beillesztése egy várólista

Üzenet beszúrni egy meglévő várólista, először létre kell hoznia egy új **CloudQueueMessage**. Ezután hívja fel a **AddMessage** módszert. Egy karakterlánc (a formátum UTF-8) vagy egy **bájt** tömböt egy **CloudQueueMessage** hozhat létre. Az alábbiakban kódot, amely egy várólista hoz létre, (Ha még nem létezik), és beszúrja a "Helló, világ" üzenet:

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    // Create the queue if it doesn't already exist.
    queue.CreateIfNotExists();

    // Create a message and add it to the queue.
    CloudQueueMessage message = new CloudQueueMessage("Hello, World");
    queue.AddMessage(message);

## <a name="peek-at-the-next-message"></a>Betekintés a a következő üzenetre

Az üzenet levettük várólista-e a anélkül, hogy eltávolítja a sor hívja fel a **PeekMessage** módszer Bepillantás.

    // Retrieve storage account from connection string
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the queue client
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a queue
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    // Peek at the next message
    CloudQueueMessage peekedMessage = queue.PeekMessage();

    // Display message.
    Console.WriteLine(peekedMessage.AsString);

## <a name="change-the-contents-of-a-queued-message"></a>Aszinkron üzenet tartalmának módosítása

Egy üzenet helyi a várakozási sorban található tartalmát módosíthatja. Ha az üzenet egy munkamennyiségű tevékenység jelöli, ez a szolgáltatás segítségével a munka feladat állapotának módosítása. A következő kódot a várólista üzenet frissíti az új tartalom, és a láthatóság időkorlátot, ha ki szeretné terjeszteni a másik 60 másodperc állít be. Az üzenet társított munka állapotának menti, és adja vissza az ügyfél, folytassa a munkát az üzenet egy másik perc. Ez a módszer segítségével nyomon követheti a több elem lépés munkafolyamatok üzenetek, a anélkül, hogy kezdje az elejétől, ha egy feldolgozás lépés hardveres és szoftveres hiba miatt nem sikerül. Általában szeretné megtartani, valamint a kísérletek száma, és az üzenet több, mint *n* időpontok megismétlése, szeretné-e törölje azt. Ez az üzenet, amely elindítja az alkalmazás hibát minden alkalommal, amikor a program dolgozza fel védelmet.

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    // Get the message from the queue and update the message contents.
    CloudQueueMessage message = queue.GetMessage();
    message.SetMessageContent("Updated contents.");
    queue.UpdateMessage(message,
        TimeSpan.FromSeconds(60.0),  // Make it visible for another 60 seconds.
        MessageUpdateFields.Content | MessageUpdateFields.Visibility);

## <a name="de-queue-the-next-message"></a>A következő üzenetre vonja sorba

A kód vonja két lépést az egy sorból üzenet várakozási sorba. **GetMessage**hívásakor hibaüzenet a következő egy sorban. **GetMessage** – által eredményül adott üzenet lesz nem látható az üzenetek olvasása a sorból bármely más kódot. Alapértelmezés szerint ez az üzenet maradjon láthatatlan 30 másodperces. Az üzenet eltávolítása a várólista befejezéséhez **DeleteMessage**is meg kell hívni. Az üzenet eltávolítása a két lépésből álló folyamat biztosítja, hogy a kód feldolgozása üzenet hardveres és szoftveres hiba miatt sikertelen, ha egy másik példányát a kódot is ugyanazon üzenet jelenik meg, és próbálkozzon újra. A kód hívások **DeleteMessage** jobbra, után az üzenet feldolgozása megtörtént.

    // Retrieve storage account from connection string
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the queue client
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a queue
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    // Get the next message
    CloudQueueMessage retrievedMessage = queue.GetMessage();

    //Process the message in less than 30 seconds, and then delete the message
    queue.DeleteMessage(retrievedMessage);

## <a name="use-async-await-pattern-with-common-queue-storage-apis"></a>Aszinkron kerülve mintát használata a közös várólista-tároló API-hoz

Ez a példa megtudhatja, hogy hogyan szeretné használni a aszinkron kerülve minta közös várólista tároló API-khoz. A minta a hívások a aszinkron verziót az adott módszerek az *aszinkron* utótag minden módszer szerint. Amikor egy aszinkron módszert, a aszinkron-kerülve mintát mindaddig, amíg a hívás befejezése felfüggeszti a helyi végrehajtása. Ez a jelenség lehetővé teszi, hogy az aktuális szál más munkavégzés, amely segít a teljesítmény szűk elkerülése érdekében, és az alkalmazás általános növeléséhez javítja. Az aszinkron-Await mintát használatáról a .NET rendszerhez című [aszinkron és Await (C# és a Visual Basic)](https://msdn.microsoft.com/library/hh191443.aspx)

    // Create the queue if it doesn't already exist
    if(await queue.CreateIfNotExistsAsync())
    {
        Console.WriteLine("Queue '{0}' Created", queue.Name);
    }
    else
    {
        Console.WriteLine("Queue '{0}' Exists", queue.Name);
    }

    // Create a message to put in the queue
    CloudQueueMessage cloudQueueMessage = new CloudQueueMessage("My message");

    // Async enqueue the message
    await queue.AddMessageAsync(cloudQueueMessage);
    Console.WriteLine("Message added");

    // Async dequeue the message
    CloudQueueMessage retrievedMessage = await queue.GetMessageAsync();
    Console.WriteLine("Retrieved message with content '{0}'", retrievedMessage.AsString);

    // Async delete the message
    await queue.DeleteMessageAsync(retrievedMessage);
    Console.WriteLine("Deleted message");

## <a name="leverage-additional-options-for-de-queuing-messages"></a>Kihasználhatja a további beállítások vonja várólista üzenetek

Két módon testre szabhatja az üzenet lekérés a sorból.
Első lépésként elérheti a köteget, az üzenetek (legfeljebb 32). Második beállíthatja, hogy vagy lerövidítéséhez invisibility időtúllépés, több vagy kevesebb ideig teljesen feldolgozása minden üzenet, amely lehetővé teszi, hogy a kódot. Az alábbi példa a **GetMessages** módszerrel 20 üzenetet kérhet egy hívásban vesz részt. Ezután **foreach** ismétlődve az egyes üzenetek feldolgozásával. Minden üzenet öt perc értékre állítja a invisibility időtúllépés is. Figyelje meg, hogy az 5 perc indítása egyszerre, az összes üzenetre vonatkozóan, miután 5 perccel van óta eltelt **GetMessages**és az esetleges üzeneteket, amelyek nem törli a hívást ismét láthatóvá válnak.

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    foreach (CloudQueueMessage message in queue.GetMessages(20, TimeSpan.FromMinutes(5)))
    {
        // Process all messages in less than 5 minutes, deleting each message after processing.
        queue.DeleteMessage(message);
    }

## <a name="get-the-queue-length"></a>Ismerkedés a sor hossza

Az üzenetek számát becslése várólista elérheti. A **FetchAttributes** módszer megkérdezi, hogy a várólista szolgáltatást beolvasni a várólista tulajdonságait, például az üzenetek száma. A **ApproximateMessageCount** tulajdonság anélkül, hogy hívja fel a várólista szolgáltatást a **FetchAttributes** módszer által beolvasott utolsó értékét adja eredményül.

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    // Fetch the queue attributes.
    queue.FetchAttributes();

    // Retrieve the cached approximate message count.
    int? cachedMessageCount = queue.ApproximateMessageCount;

    // Display number of messages.
    Console.WriteLine("Number of messages in queue: " + cachedMessageCount);

## <a name="delete-a-queue"></a>Egy várólista törlése

Várólista és a benne lévő összes üzenetet törléséhez hívja fel a várakozási sorban található objektum **törlése** módszer.

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    // Delete the queue.
    queue.Delete();

## <a name="next-steps"></a>Következő lépések

Most, hogy megtanulta várólista-tároló alapjait, kövesse ezeket a hivatkozásokat, ha többet szeretne tudni az összetettebb tároló tevékenységek.

- Várólista szolgáltatást hivatkozás dokumentációjában találhat részletes adatait a rendelkezésre álló API-khoz megtekintése:
    - [Tárterület ügyfél dokumentumtár a .NET-hivatkozás](http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409)
    - [REST API-hivatkozás](http://msdn.microsoft.com/library/azure/dd179355)
- Megtudhatja, hogyan egyszerűsíti a kódot az Azure-tároló kezelése az [Azure WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk.md)használatával írhat.
- További tudnivalók: további lehetőségeket adattárolásra Azure-ban több funkció útmutatók megtekintése.
    - [Azure táblatárolóhoz .NET használata – első lépések](storage-dotnet-how-to-use-tables.md) a strukturált adatokat tárolja.
    - [Első lépések az Azure Blob-tárolóhoz .NET használatával](storage-dotnet-how-to-use-blobs.md) strukturálatlan adatokat tároló.
    - [Csatlakozás SQL-adatbázis .NET (C#) használatával](../sql-database/sql-database-develop-dotnet-simple.md) relációs adatok tárolására.

  [Download and install the Azure SDK for .NET]: /develop/net/
  [.NET client library reference]: http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409
  [Creating a Azure Project in Visual Studio]: http://msdn.microsoft.com/library/azure/ee405487.aspx
  [Azure Storage Team Blog]: http://blogs.msdn.com/b/windowsazurestorage/
  [OData]: http://nuget.org/packages/Microsoft.Data.OData/5.0.2
  [Edm]: http://nuget.org/packages/Microsoft.Data.Edm/5.0.2
  [Spatial]: http://nuget.org/packages/System.Spatial/5.0.2
