<properties
    pageTitle="Java várólista tárhelyet használata |} Microsoft Azure"
    description="Megtudhatja, hogy miként használhatja az Azure várólista szolgáltatást létrehozása és törlése a sorok, beszúrása, beszerzése és üzeneteket törölheti. A minta Java nyelven íródott."
    services="storage"
    documentationCenter="java"
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robinsh"/>

# <a name="how-to-use-queue-storage-from-java"></a>Java várólista tárhelyet használata

[AZURE.INCLUDE [storage-selector-queue-include](../../includes/storage-selector-queue-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>– Áttekintés

Ez az útmutató megtudhatja, miként végezheti el a gyakori alkalmazási területek, az Azure várólista tároló szolgáltatással. A minták Java írt, és az [Azure tároló SDK Java][]használja. Az érintett esetek **beszúrását**, **Bepillantás**, **az első**és **Törlés,** üzenetek, valamint **létrehozásáról** és **törléséről** sorban várakozó. Sorban várakozó a további tudnivalókért lásd: a [következő lépésekkel](#Next-Steps) szakaszban.

Megjegyzés: Az SDK érhető el a fejlesztők számára, akik a Azure tároló Android-eszközökön. További tudnivalókért lásd: az [Azure tároló SDK csomagjában talál az Android-alapú][].

[AZURE.INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-java-application"></a>Java-alkalmazás létrehozása

Ebből az útmutatóból tárolási lehetőségek, amely a másolni helyileg vagy a webes szerepkör vagy dolgozó szerepkör Azure-ban futó kód egy Java alkalmazáson belül futtathatja fogja használni.

Ehhez, meg kell Java fejlesztési Kit (JDK) telepítése, és Azure tároló fiók létrehozása az Azure-előfizetésben. Miután elvégezte a úgy, szüksége lesz annak ellenőrzése, hogy a fejlesztői rendszer megfelel-e a minimális követelmények és függőségek, amelyek jelennek meg a GitHub [Java Azure tároló SDK][] tárházából. Ha a rendszer megfelel ezeknek a követelményeknek, kövesse az utasításokat a letöltése és telepítése az Azure tároló tárakat Java, hogy a tárházba a számítógépre. Miután elvégezte ezeket a feladatokat, lesz a példák a jelen cikkben használó Java-alkalmazás létrehozása.

## <a name="configure-your-application-to-access-queue-storage"></a>Az alkalmazás várólista-tároló eléréséhez konfigurálása

Adja hozzá a következő importálása kimutatások felső részén a Java-fájlt, amelyhez Azure tároló API-khoz sorban várakozó eléréséhez használni:

    // Include the following imports to use queue APIs.
    import com.microsoft.azure.storage.*;
    import com.microsoft.azure.storage.queue.*;

## <a name="setup-an-azure-storage-connection-string"></a>Az Azure tároló kapcsolati karakterlánc beállítása

Az Azure tároló ügyfél tároló kapcsolati karakterlánc használja a végpontok és adatok kezelése szolgáltatások eléréséhez szükséges hitelesítő adatok tárolására szolgáló. Ügyfélalkalmazás fut, meg kell adnia a tárhely kapcsolati karakterláncot, a következő formátumban *fióknév* és *AccountKey* az értékek az [Azure-portálon](https://portal.azure.com) szereplő tároló partner nevét a tárterület-fiók és az access elsődleges kulcs használ. Ez a példa mutatja, hogy hogyan tartsa lenyomva az ujját a kapcsolati karakterlánc statikus mező is deklarálhatnak:

    // Define the connection-string with your values.
    public static final String storageConnectionString =
        "DefaultEndpointsProtocol=http;" +
        "AccountName=your_storage_account;" +
        "AccountKey=your_storage_account_key";

Az operációs rendszert futtató belül a Microsoft Azure szerepkörbe alkalmazásokban a karakterlánc szolgáltatás konfigurációs fájl *ServiceConfiguration.cscfg*, tárolhatók és a hívást a **RoleEnvironment.getConfigurationSettings** módszerrel is elérhető. Íme egy példa a kapcsolati karakterlánc ahhoz, hogy a **beállítás** elem nevű *StorageConnectionString* a szolgáltatás konfigurációs fájl:

    // Retrieve storage account from connection-string.
    String storageConnectionString =
        RoleEnvironment.getConfigurationSettings().get("StorageConnectionString");

Az alábbi példa feltételezi, hogy már használta az alábbi két módszerek egyikét a tárhely kapcsolati karakterláncát.

## <a name="how-to-create-a-queue"></a>Útmutató: várólista létrehozása

Egy **CloudQueueClient** objektum lehetővé teszi a hivatkozási objektumok beszerzése sorban várakozó. A következő kódot létrehoz egy **CloudQueueClient** -objektumot. (Megjegyzés: más módokon **CloudStorageAccount** objektumok; létrehozásához további tudnivalókért lásd: **CloudStorageAccount** az [Azure tároló ügyfél SDK hivatkozást].)

A **CloudQueueClient** objektum segítségével a használni kívánt sor hivatkozást. A várakozási sorban található hozhat létre, ha még nem létezik.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

       // Create the queue client.
       CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

       // Retrieve a reference to a queue.
       CloudQueue queue = queueClient.getQueueReference("myqueue");

       // Create the queue if it doesn't already exist.
       queue.createIfNotExists();
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-add-a-message-to-a-queue"></a>Útmutató: várólista üzenet hozzáadása

Üzenet beszúrni egy meglévő várólista, először létre kell hoznia egy új **CloudQueueMessage**. Ezután hívja fel a **addMessage** módszert. Egy karakterlánc (a formátum UTF-8) vagy egy bájt tömböt egy **CloudQueueMessage** hozhat létre. Az alábbiakban kódot, amely egy várólista hoz létre, (Ha még nem létezik), és beszúrja a "Helló, világ" üzenet.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

        // Create the queue if it doesn't already exist.
        queue.createIfNotExists();

        // Create a message and add it to the queue.
        CloudQueueMessage message = new CloudQueueMessage("Hello, World");
        queue.addMessage(message);
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-peek-at-the-next-message"></a>Útmutató: a következő üzenet megtekintése

Az üzenet levettük várólista-e a anélkül, hogy eltávolítja a sor hívja fel a **peekMessage**Bepillantás.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

        // Peek at the next message.
        CloudQueueMessage peekedMessage = queue.peekMessage();

        // Output the message value.
        if (peekedMessage != null)
        {
          System.out.println(peekedMessage.getMessageContentAsString());
       }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-change-the-contents-of-a-queued-message"></a>Útmutató: aszinkron üzenet tartalmának módosítása

Egy üzenet helyi a várakozási sorban található tartalmát módosíthatja. Ha az üzenet egy munkamennyiségű tevékenység jelöli, ez a szolgáltatás segítségével a munka feladat állapotának módosítása. A következő kódot a várólista üzenet frissíti az új tartalom, és a láthatóság időkorlátot, ha ki szeretné terjeszteni a másik 60 másodperc állít be. Az üzenet társított munka állapotának menti, és adja vissza az ügyfél, folytassa a munkát az üzenet egy másik perc. Ez a módszer segítségével nyomon követheti a több elem lépés munkafolyamatok üzenetek, a anélkül, hogy kezdje az elejétől, ha egy feldolgozás lépés hardveres és szoftveres hiba miatt nem sikerül. Általában szeretné megtartani, valamint a kísérletek száma, és az üzenet több, mint *n* időpontok megismétlése, szeretné-e törölje azt. Ez az üzenet, amely elindítja az alkalmazás hibát minden alkalommal, amikor a program dolgozza fel védelmet.

A következő kódot minta keres keresztül listájához az üzenetek megkeresi az első üzenet, amely megfelel a "Helló, világ" tartalmát, megváltoztatja a tartalom üzenet és, majd a Kilépés.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

        // The maximum number of messages that can be retrieved is 32.
        final int MAX_NUMBER_OF_MESSAGES_TO_PEEK = 32;

        // Loop through the messages in the queue.
        for (CloudQueueMessage message : queue.retrieveMessages(MAX_NUMBER_OF_MESSAGES_TO_PEEK,1,null,null))
        {
            // Check for a specific string.
            if (message.getMessageContentAsString().equals("Hello, World"))
            {
                // Modify the content of the first matching message.
                message.setMessageContent("Updated contents.");
                // Set it to be visible in 30 seconds.
                EnumSet<MessageUpdateFields> updateFields =
                    EnumSet.of(MessageUpdateFields.CONTENT,
                    MessageUpdateFields.VISIBILITY);
                // Update the message.
                queue.updateMessage(message, 30, updateFields, null, null);
                break;
            }
        }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

Azt is megteheti a következő kódot minta frissíti a csak a sor első látható üzenetének.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

        // Retrieve the first visible message in the queue.
        CloudQueueMessage message = queue.retrieveMessage();

        if (message != null)
        {
            // Modify the message content.
            message.setMessageContent("Updated contents.");
            // Set it to be visible in 60 seconds.
            EnumSet<MessageUpdateFields> updateFields =
                EnumSet.of(MessageUpdateFields.CONTENT,
                MessageUpdateFields.VISIBILITY);
            // Update the message.
            queue.updateMessage(message, 60, updateFields, null, null);
        }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-get-the-queue-length"></a>Útmutató: a sor hossza beszerzése

Az üzenetek számát becslése várólista elérheti. A **downloadAttributes** módszer megkérdezi, hogy a több jelenlegi értékeket, beleértve a száma, hogy hány üzenet várólista várólista szolgáltatást. A számláló oka csak közelítő üzeneteket, illetve is eltűnik, miután a kérelem válaszol a várólista szolgáltatást. A **getApproximateMessageCount** módszer anélkül, hogy hívja fel a várólista szolgáltatást **downloadAttributes**, hívás által beolvasott utolsó értékét adja eredményül.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

       // Download the approximate message count from the server.
        queue.downloadAttributes();

        // Retrieve the newly cached approximate message count.
        long cachedMessageCount = queue.getApproximateMessageCount();

        // Display the queue length.
        System.out.println(String.format("Queue length: %d", cachedMessageCount));
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-dequeue-the-next-message"></a>Útmutató: a következő üzenetre feldolgozásához

A kód dequeues két lépést az egy sorból üzenet. **RetrieveMessage**hívásakor hibaüzenet a következő egy sorban. **RetrieveMessage** – által eredményül adott üzenet lesz nem látható az üzenetek olvasása a sorból bármely más kódot. Alapértelmezés szerint ez az üzenet maradjon láthatatlan 30 másodperces. Az üzenet eltávolítása a várólista befejezéséhez **deleteMessage**is meg kell hívni. Az üzenet eltávolítása a két lépésből álló folyamat biztosítja, hogy a kód feldolgozása üzenet hardveres és szoftveres hiba miatt sikertelen, ha egy másik példányát a kódot is ugyanazon üzenet jelenik meg, és próbálkozzon újra. A kód hívások **deleteMessage** jobbra, után az üzenet feldolgozása megtörtént.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

        // Retrieve the first visible message in the queue.
        CloudQueueMessage retrievedMessage = queue.retrieveMessage();

        if (retrievedMessage != null)
        {
            // Process the message in less than 30 seconds, and then delete the message.
            queue.deleteMessage(retrievedMessage);
        }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }


## <a name="additional-options-for-dequeuing-messages"></a>Az üzenetek üzenetmozgatót további beállítások

Két módon testre szabhatja az üzenet lekérés a sorból. Első lépésként elérheti a köteget, az üzenetek (legfeljebb 32). Második beállíthatja, hogy vagy lerövidítéséhez invisibility időtúllépés, több vagy kevesebb ideig teljesen feldolgozása minden üzenet, amely lehetővé teszi, hogy a kódot.

Az alábbi példa a **retrieveMessages** módszerrel 20 üzenetet kérhet egy hívásban vesz részt. Kattintson **a** ismétlődve az egyes üzenetek feldolgozásával. Minden üzenet öt perc (300 másodperc) értékre állítja a invisibility időtúllépés is. Látható, hogy az öt perc elindítja az összes üzenetre vonatkozóan egyszerre, így ha öt perc telt **retrieveMessages**a hívást, az esetleges üzeneteket, amelyek nem törli újra láthatóvá válnak.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

        // Retrieve 20 messages from the queue with a visibility timeout of 300 seconds.
        for (CloudQueueMessage message : queue.retrieveMessages(20, 300, null, null)) {
            // Do processing for all messages in less than 5 minutes,
            // deleting each message after processing.
            queue.deleteMessage(message);
        }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-list-the-queues"></a>Útmutató: a sorok lista

Szerezze be az aktuális sorban várakozó listáját, hogy hívja fel a **CloudQueueClient.listQueues()** módszer **CloudQueue** objektumok gyűjteménye ad vissza.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient =
            storageAccount.createCloudQueueClient();

        // Loop through the collection of queues.
        for (CloudQueue queue : queueClient.listQueues())
        {
            // Output each queue name.
            System.out.println(queue.getName());
        }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-delete-a-queue"></a>Útmutató: a várólista törlése

Várólista és a benne lévő összes üzenetet törléséhez hívja fel a **deleteIfExists** módszer a **CloudQueue** objektum.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

        // Delete the queue if it exists.
        queue.deleteIfExists();
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="next-steps"></a>Következő lépések

Most, hogy megtanulta várólista-tároló alapjait, kövesse ezeket a hivatkozásokat, ha többet szeretne tudni az összetettebb tároló tevékenységek.

- [Azure tároló Java SDK][]
- [Azure tároló ügyfél SDK hivatkozás][]
- [Azure tárolása REST API-val][]
- [Azure tároló csapatának blogja][]

[Azure SDK for Java]: http://go.microsoft.com/fwlink/?LinkID=525671
[Azure tároló Java SDK]: https://github.com/azure/azure-storage-java
[Azure tároló SDK androidra]: https://github.com/azure/azure-storage-android
[Azure tároló ügyfél SDK hivatkozás]: http://dl.windowsazure.com/storage/javadoc/
[Azure tárolása REST API-val]: https://msdn.microsoft.com/library/azure/dd179355.aspx
[Azure tároló csapatának blogja]: http://blogs.msdn.com/b/windowsazurestorage/
