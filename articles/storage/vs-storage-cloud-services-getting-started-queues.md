<properties
    pageTitle="Első lépések várólista tárhely és a Visual Studio a csatlakoztatott szolgáltatások (felhőalapú szolgáltatások) |} Microsoft Azure"
    description="Ismerkedés az Azure várólista-tároló használata egy felhőalapú szolgáltatás project a Visual Studióban, miután részletesen a tárterület a Visual Studio segítségével a csatlakoztatott szolgáltatások"
    services="storage"
    documentationCenter=""
    authors="TomArcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="storage"
    ms.workload="web"
    ms.tgt_pltfrm="vs-getting-started" 
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/18/2016"
    ms.author="tarcher"/>

# <a name="getting-started-with-azure-queue-storage-and-visual-studio-connected-services-cloud-services-projects"></a>Azure várólista tárhely és a Visual Studio első lépések a csatlakoztatott szolgáltatások (cloud services projektek)

[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>– Áttekintés

Ez a cikk ismerteti, hogyan kezdjek hozzá az Azure várólista-tároló a Visual Studióban után a korábban létrehozott vagy cloud services projekt Azure tároló fiók hivatkozott a **Csatlakoztatott szolgáltatások hozzáadása** a Visual Studio párbeszédpanel segítségével.

Bemutatjuk, hogyan hozhat létre egy várólista kódot. Is bemutatjuk, hogyan végezhetők el egyszerű várólista műveletek, például hozzáadása, módosítása, olvasásához és üzenetek eltávolítása. A minták C# kód írt, és a [Microsoft Azure tároló ügyfél tár a .NET rendszerhez](https://msdn.microsoft.com/library/azure/dn261237.aspx).

A **Csatlakoztatott szolgáltatások hozzáadása** a művelet a projekt Azure tároló eléréséhez a megfelelő NuGet csomagok telepíti, és a kapcsolati karakterláncot, a tárterület-fiók felvétele a project konfigurációs fájlok.

 - Lásd: az [Azure várólista-tároló .NET használata – első lépések](storage-dotnet-how-to-use-queues.md) a kód testreszabhatóvá sorban várakozó további információt.
 - [Tárterület](https://azure.microsoft.com/documentation/services/storage/) dokumentációjában Azure tároló kapcsolatos általános tudnivalókat.
 - [Cloud Services](https://azure.microsoft.com/documentation/services/cloud-services/) dokumentációjában Azure felhőszolgáltatások kapcsolatos általános tudnivalókat.
 - Lásd: [az ASP.NET](http://www.asp.net) programozási ASP.NET-alkalmazásokkal kapcsolatban további tudnivalókat.


Azure várólista-tároló olyan szolgáltatás, a nagyszámú bárhol is elérhető a világ keresztül hitelesített hívások HTTP vagy HTTPS üzenetek tárolásához. Egyetlen várólista üzenet mérete legfeljebb 64 KB, és várólista üzenetek, a teljes kapacitásának által tároló fiók felfelé milliónyi is tartalmazhat.


## <a name="access-queues-in-code"></a>Az Access sorban várakozó a kódot.

A Visual Studio Cloud Services projektek sorban várakozó eléréséhez kell az alábbiakat tartalmazza bármelyik C# forrásfájlhoz Azure várólista-tároló elérésére.

1. Ellenőrizze, hogy a névtér nyilatkozatokat tetején látható a C# fájlt tartalmazza e **használata** kimutatásokban.

        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Queue;

2. Egy **CloudStorageAccount** objektum, amely a tárhely fiókadatok beolvasása. Az alábbi kód használatával beszerzése a a tárhely kapcsolati karakterlánc és a tárhely fiók adatait a Azure szolgáltatás beállításai.

         CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));

3. Ismerkedés a tárterület-fiókjában várólista-objektumok hivatkozni szeretne egy **CloudQueueClient** objektum.  

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

4. Ismerkedés a **CloudQueue** objektum egy adott várólista hivatkozni szeretne.

        // Get a reference to a queue named "messageQueue"
        CloudQueue messageQueue = queueClient.GetQueueReference("messageQueue");


**Megjegyzés:** Az összes elé a kódot a fenti kóddal használja az alábbi példák.

## <a name="create-a-queue-in-code"></a>A kód várólista létrehozása

A sor létrehozása a kódot, hozzáadása a hívást kezdeményez **CreateIfNotExists**.

    // Create the CloudQueue if it does not exist
    messageQueue.CreateIfNotExists();

## <a name="add-a-message-to-a-queue"></a>Üzenet várólista hozzáadása

Üzenet beszúrni egy meglévő várólista, hozzon létre egy új **CloudQueueMessage** objektumot, majd a **AddMessage** módszer hívja.

Egy karakterlánc (a formátum UTF-8) vagy egy bájt tömböt egy **CloudQueueMessage** objektum hozhat létre.

Íme egy példa, amely szúrja be a "Helló, világ" üzenet.

    // Create a message and add it to the queue.
    CloudQueueMessage message = new CloudQueueMessage("Hello, World");
    messageQueue.AddMessage(message);

## <a name="read-a-message-in-a-queue"></a>Olvassa el egy üzenetet, egy sorban

Az üzenet levettük várólista-e a anélkül, hogy eltávolítja a sor hívja fel a **PeekMessage** módszer Bepillantás.

    // Peek at the next message
    CloudQueueMessage peekedMessage = messageQueue.PeekMessage();

## <a name="read-and-remove-a-message-in-a-queue"></a>Olvassa el, és távolítsa el az üzenetet egy sorban

A kód eltávolíthatja (majd a sorból) üzenet két lépést az egy sorból.

1. Hívja fel a **GetMessage** a következő üzenet jelenik meg egy sorban. **GetMessage** – által eredményül adott üzenet lesz nem látható az üzenetek olvasása a sorból bármely más kódot. Alapértelmezés szerint ez az üzenet maradjon láthatatlan 30 másodperces.
2.  A befejezéshez az üzenet eltávolítása a sor hívja fel a **DeleteMessage**.

Az üzenet eltávolítása a két lépésből álló folyamat biztosítja, hogy a kód feldolgozása üzenet hardveres és szoftveres hiba miatt sikertelen, ha egy másik példányát a kódot is ugyanazon üzenet jelenik meg, és próbálkozzon újra. A következő kódot a hívások **DeleteMessage** jobbra után az üzenet feldolgozása megtörtént.

    // Get the next message in the queue.
    CloudQueueMessage retrievedMessage = messageQueue.GetMessage();

    // Process the message in less than 30 seconds

    // Then delete the message.
    await messageQueue.DeleteMessage(retrievedMessage);


## <a name="use-additional-options-to-process-and-remove-queue-messages"></a>További beállítások segítségével folyamat, és távolítsa el az üzenetek

Két módon testre szabhatja az üzenet lekérés a sorból.

 - Az üzenetek (legfeljebb 32) egy köteg elérheti.
 - Beállíthatja, hogy vagy lerövidítéséhez invisibility időtúllépés, több vagy kevesebb ideig teljesen feldolgozása minden üzenet, amely lehetővé teszi, hogy a kódot. Az alábbi példa a **GetMessages** módszerrel 20 üzenetet kérhet egy hívásban vesz részt. Ezután **foreach** ismétlődve az egyes üzenetek feldolgozásával. Minden üzenet öt perc értékre állítja a invisibility időtúllépés is. Figyelje meg, hogy az 5 perc indítása egyszerre, az összes üzenetre vonatkozóan, miután 5 perccel van óta eltelt **GetMessages**és az esetleges üzeneteket, amelyek nem törli a hívást ismét láthatóvá válnak.

Lássunk egy példát:

    foreach (CloudQueueMessage message in messageQueue.GetMessages(20, TimeSpan.FromMinutes(5)))
    {
        // Process all messages in less than 5 minutes, deleting each message after processing.

        // Then delete the message after processing
        messageQueue.DeleteMessage(message);

    }

## <a name="get-the-queue-length"></a>Ismerkedés a sor hossza

Az üzenetek számát becslése várólista elérheti. A **FetchAttributes** módszer megkérdezi, hogy a várólista szolgáltatást beolvasni a várólista tulajdonságait, például az üzenetek száma. A **ApproximateMethodCount** tulajdonság anélkül, hogy hívja fel a várólista szolgáltatást a **FetchAttributes** módszer által beolvasott utolsó értékét adja eredményül.

    // Fetch the queue attributes.
    messageQueue.FetchAttributes();

    // Retrieve the cached approximate message count.
    int? cachedMessageCount = messageQueue.ApproximateMessageCount;

    // Display number of messages.
    Console.WriteLine("Number of messages in queue: " + cachedMessageCount);

## <a name="use-the-async-await-pattern-with-common-azure-queue-apis"></a>Az aszinkron kerülve mintát használata a közös Azure várólista API-hoz

Ez a példa bemutatja, hogyan a aszinkron kerülve minta használata a közös Azure várólista API-khoz. A minta felhívja az adott módszerek aszinkron verziója, ez a **aszinkron** utáni fix minden módszer is láthatja. Amikor egy aszinkron módszert a aszinkron-kerülve mintát felfüggeszti a helyi végrehajtás, mindaddig, amíg a hívás befejezése. Ez a jelenség lehetővé teszi, hogy az aktuális szál más munkavégzés, amely segít a teljesítmény szűk elkerülése érdekében, és az alkalmazás általános növeléséhez javítja. Az aszinkron-Await mintát használatáról a .NET [aszinkron és Await (C# és a Visual Basic)] című (https://msdn.microsoft.com/library/hh191443.aspx)

    // Create a message to put in the queue
    CloudQueueMessage cloudQueueMessage = new CloudQueueMessage("My message");

    // Add the message asynchronously
    await messageQueue.AddMessageAsync(cloudQueueMessage);
    Console.WriteLine("Message added");

    // Async dequeue the message
    CloudQueueMessage retrievedMessage = await messageQueue.GetMessageAsync();
    Console.WriteLine("Retrieved message with content '{0}'", retrievedMessage.AsString);

    // Delete the message asynchronously
    await messageQueue.DeleteMessageAsync(retrievedMessage);
    Console.WriteLine("Deleted message");

## <a name="delete-a-queue"></a>Egy várólista törlése

Várólista és a benne lévő összes üzenetet törléséhez hívja fel a várakozási sorban található objektum **törlése** módszer.

    // Delete the queue.
    messageQueue.Delete();

## <a name="next-steps"></a>Következő lépések

[AZURE.INCLUDE [vs-storage-dotnet-queues-next-steps](../../includes/vs-storage-dotnet-queues-next-steps.md)]
