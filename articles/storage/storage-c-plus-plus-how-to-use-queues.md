<properties
    pageTitle="Várólista-tároló (C++) használata |} Microsoft Azure"
    description="Megtudhatja, hogy miként használja a várólista tárhelyszolgáltatáshoz Azure-ban. A minták c++ kerülnek."
    services="storage"
    documentationCenter=".net"
    authors="dineshmurthy"
    manager="jahogg"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="dineshm"/>

# <a name="how-to-use-queue-storage-from-c"></a>A C++ várólista-tároló használata  

[AZURE.INCLUDE [storage-selector-queue-include](../../includes/storage-selector-queue-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>– Áttekintés
Ez az útmutató megtudhatja, miként végezheti el a gyakori alkalmazási területek, az Azure várólista tároló szolgáltatással. A minták C++ írt, és a [Azure tároló ügyfél-tár C++](http://github.com/Azure/azure-storage-cpp/blob/master/README.md). Az érintett esetek **beszúrása**, **Bepillantás**, **az első**és **Törlés,** üzenetek, valamint **létrehozásáról és a sorok törlése**.

>[AZURE.NOTE] Ez az útmutató az Azure tároló ügyfél tár célként, C++ verzió 1.0.0 és felett. A javasolt verziója tároló ügyfél tár 2.2.0, melyiket érdemes [NuGet](http://www.nuget.org/packages/wastorage) vagy [GitHub](http://github.com/Azure/azure-storage-cpp/)keresztül érhető el.

[AZURE.INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]
[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-c-application"></a>C++-alkalmazás létrehozása  
Ebből az útmutatóból tárolási lehetőségek, amely egy C++ alkalmazáson belül futtathatja fogja használni.

Ehhez, meg kell telepítse az Azure tároló ügyfél tár C++, és Azure tároló fiók létrehozása az Azure-előfizetésben.

Az Azure tároló ügyfél tár telepítése C++, az alábbi módszerek használhatja:

-   **Linux:** Kövesse az [Azure tároló ügyfél-tár C++ fontos](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) lapon utasításait.
-   **Windows:** Kattintson a Visual Studióban, **Eszközök > NuGet csomag kezelő > csomag kezelője konzol**. A következő parancsot írja be a [NuGet csomag Manager konzolhoz](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) , és nyomja le az **ENTER BILLENTYŰT**.

        Install-Package wastorage

## <a name="configure-your-application-to-access-queue-storage"></a>Az alkalmazás várólista-tároló eléréséhez konfigurálása
Adja meg a következő utasítások tetején a C++ fájlt, amelyre az Azure tároló API-khoz sorban várakozó eléréséhez használni a tartalmazza:  

    #include "was/storage_account.h"
    #include "was/queue.h"

## <a name="set-up-an-azure-storage-connection-string"></a>Az Azure tároló kapcsolati karakterlánc beállítása

Az Azure tároló ügyfél tároló kapcsolati karakterlánc használja a végpontok és adatok kezelése szolgáltatások eléréséhez szükséges hitelesítő adatok tárolására szolgáló. Ügyfélalkalmazás fut, meg kell adnia a tárhely kapcsolati karakterláncot, a következő formátumban *fióknév* és *AccountKey* az értékek az [Azure-portálon](https://portal.azure.com) szereplő tároló partner nevét a tárterület-fiók és a tárhely hívóbetű használata. A tároló partnereket és a hívóbetűk további tudnivalókért lásd [Azure tároló fiókokat kapcsolatban](storage-create-storage-account.md). Ez a példa mutatja, hogy hogyan tartsa lenyomva az ujját a kapcsolati karakterlánc statikus mező is deklarálhatnak:  

    // Define the connection-string with your values.
    const utility::string_t storage_connection_string(U("DefaultEndpointsProtocol=https;AccountName=your_storage_account;AccountKey=your_storage_account_key"));

Az alkalmazás tesztelése a Windows helyi számítógépen, a Microsoft Azure [tároló irányító](storage-use-emulator.md) , hogy telepítve van az [Azure SDK](https://azure.microsoft.com/downloads/)is használhatja. A tároló irányító egy, amely a helyi fejlesztési számítógépen elérhető az Azure Blob várólista és táblázat szolgáltatások segédprogramot. A következő példa bemutatja, hogyan szeretné a helyi tároló irányító tartsa lenyomva az ujját a kapcsolati karakterlánc statikus mező is deklarálhatnak:  

    // Define the connection-string with Azure Storage Emulator.
    const utility::string_t storage_connection_string(U("UseDevelopmentStorage=true;"));  

Az Azure tároló irányító elindításához kattintson a **Start** gombra, vagy nyomja le a **Windows** billentyűt. Írja be az **Azure tároló irányító**, és jelölje ki **A Microsoft Azure tároló irányító** -alkalmazások listájából.

Az alábbi példa feltételezi, hogy már használta az alábbi két módszerek egyikét a tárhely kapcsolati karakterláncát.

## <a name="retrieve-your-connection-string"></a>A kapcsolati karakterlánc beolvasása
A tároló fiókadatok ábrázolásához **cloud_storage_account** osztály is használhatja. A tároló fiókadatok lekérése a tárterület-kapcsolati karakterláncot, a **elemzési** módszert is használhatja.

    // Retrieve storage account from connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

## <a name="how-to-create-a-queue"></a>Útmutató: várólista létrehozása
Egy **cloud_queue_client** objektum lehetővé teszi a hivatkozási objektumok beszerzése sorban várakozó. A következő kódot létrehoz egy **cloud_queue_client** -objektumot.

    // Retrieve storage account from connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create a queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

A **cloud_queue_client** objektum segítségével a használni kívánt sor hivatkozást. A várakozási sorban található hozhat létre, ha még nem létezik.

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // Create the queue if it doesn't already exist.
    queue.create_if_not_exists();  

## <a name="how-to-insert-a-message-into-a-queue"></a>Útmutató: várólista üzenet beillesztése
Üzenet beszúrni egy meglévő várólista, először létre kell hoznia egy új **cloud_queue_message**. Ezután hívja fel a **add_message** módszert. Egy karakterlánc vagy egy **bájt** tömböt egy **cloud_queue_message** létrehozhatók. Az alábbiakban kódot, amely egy várólista hoz létre, (Ha még nem létezik), és beszúrja a "Helló, világ" üzenet:

    // Retrieve storage account from connection-string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // Create the queue if it doesn't already exist.
    queue.create_if_not_exists();

    // Create a message and add it to the queue.
    azure::storage::cloud_queue_message message1(U("Hello, World"));
    queue.add_message(message1);  

## <a name="how-to-peek-at-the-next-message"></a>Útmutató: a következő üzenet megtekintése
Az üzenet levettük várólista-e a anélkül, hogy eltávolítja a sor hívja fel a **peek_message** módszer Bepillantás.

    // Retrieve storage account from connection-string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // Peek at the next message.
    azure::storage::cloud_queue_message peeked_message = queue.peek_message();

    // Output the message content.
    std::wcout << U("Peeked message content: ") << peeked_message.content_as_string() << std::endl;

## <a name="how-to-change-the-contents-of-a-queued-message"></a>Útmutató: aszinkron üzenet tartalmának módosítása
Egy üzenet helyi a várakozási sorban található tartalmát módosíthatja. Ha az üzenet egy munkamennyiségű tevékenység jelöli, ez a szolgáltatás segítségével a munka feladat állapotának módosítása. A következő kódot a várólista üzenet frissíti az új tartalom, és a láthatóság időkorlátot, ha ki szeretné terjeszteni a másik 60 másodperc állít be. Az üzenet társított munka állapotának menti, és adja vissza az ügyfél, folytassa a munkát az üzenet egy másik perc. Ez a módszer segítségével nyomon követheti a több elem lépés munkafolyamatok üzenetek, a anélkül, hogy kezdje az elejétől, ha egy feldolgozás lépés hardveres és szoftveres hiba miatt nem sikerül. Általában szeretné megtartani, valamint a kísérletek száma, és az üzenet több, mint n számú alkalommal megismétlése, szeretné-e törölje azt. Ez az üzenet, amely elindítja az alkalmazás hibát minden alkalommal, amikor a program dolgozza fel védelmet.

    // Retrieve storage account from connection-string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_conection_string);

    // Create the queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // Get the message from the queue and update the message contents.
    // The visibility timeout "0" means make it visible immediately.
    // The visibility timeout "60" means the client can get another minute to continue
    // working on the message.
    azure::storage::cloud_queue_message changed_message = queue.get_message();

    changed_message.set_content(U("Changed message"));
    queue.update_message(changed_message, std::chrono::seconds(60), true);

    // Output the message content.
    std::wcout << U("Changed message content: ") << changed_message.content_as_string() << std::endl;  

## <a name="how-to-de-queue-the-next-message"></a>Útmutató: a következő üzenetre vonja sorba
A kód vonja két lépést az egy sorból üzenet várakozási sorba. **Get_message**hívásakor hibaüzenet a következő egy sorban. **Get_message** – által eredményül adott üzenet lesz nem látható az üzenetek olvasása a sorból bármely más kódot. Az üzenet eltávolítása a várólista befejezéséhez **delete_message**is meg kell hívni. Az üzenet eltávolítása a két lépésből álló folyamat biztosítja, hogy a kód feldolgozása üzenet hardveres és szoftveres hiba miatt sikertelen, ha egy másik példányát a kódot is ugyanazon üzenet jelenik meg, és próbálkozzon újra. A kód hívások **delete_message** jobbra, után az üzenet feldolgozása megtörtént.

    // Retrieve storage account from connection-string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // Get the next message.
    azure::storage::cloud_queue_message dequeued_message = queue.get_message();
    std::wcout << U("Dequeued message: ") << dequeued_message.content_as_string() << std::endl;

    // Delete the message.
    queue.delete_message(dequeued_message);

## <a name="how-to-leverage-additional-options-for-de-queuing-messages"></a>Útmutató: kihasználhatja a további beállítások vonja várólista üzenetek
Két módon testre szabhatja az üzenet lekérés a sorból. Első lépésként elérheti a köteget, az üzenetek (legfeljebb 32). Második beállíthatja, hogy vagy lerövidítéséhez invisibility időtúllépés, több vagy kevesebb ideig teljesen feldolgozása minden üzenet, amely lehetővé teszi, hogy a kódot. Az alábbi példa a **get_messages** módszerrel 20 üzenetet kérhet egy hívásban vesz részt. Kattintson **a** ismétlődve az egyes üzenetek feldolgozásával. Minden üzenet öt perc értékre állítja a invisibility időtúllépés is. Figyelje meg, hogy az 5 perc indítása egyszerre, az összes üzenetre vonatkozóan, miután 5 perccel van óta eltelt **get_messages**és az esetleges üzeneteket, amelyek nem törli a hívást ismét láthatóvá válnak.

    // Retrieve storage account from connection-string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // Dequeue some queue messages (maximum 32 at a time) and set their visibility timeout to
    // 5 minutes (300 seconds).
    azure::storage::queue_request_options options;
    azure::storage::operation_context context;

    // Retrieve 20 messages from the queue with a visibility timeout of 300 seconds.
    std::vector<azure::storage::cloud_queue_message> messages = queue.get_messages(20, std::chrono::seconds(300), options, context);

    for (auto it = messages.cbegin(); it != messages.cend(); ++it)
    {
        // Display the contents of the message.
        std::wcout << U("Get: ") << it->content_as_string() << std::endl;
    }

## <a name="how-to-get-the-queue-length"></a>Útmutató: a sor hossza beszerzése
Az üzenetek számát becslése várólista elérheti. A **download_attributes** módszer megkérdezi, hogy a várólista szolgáltatást beolvasni a várólista tulajdonságait, például az üzenetek száma. A **approximate_message_count** módszer megkapja a várólista közelítő az üzenetek számát.

    // Retrieve storage account from connection-string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // Fetch the queue attributes.
    queue.download_attributes();

    // Retrieve the cached approximate message count.
    int cachedMessageCount = queue.approximate_message_count();

    // Display number of messages.
    std::wcout << U("Number of messages in queue: ") << cachedMessageCount << std::endl;  

## <a name="how-to-delete-a-queue"></a>Útmutató: a várólista törlése
Várólista és a benne lévő összes üzenetet törléséhez hívja fel a **delete_queue_if_exists** módszer a várakozási sorban található objektum.

    // Retrieve storage account from connection-string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // If the queue exists and delete it.
    queue.delete_queue_if_exists();  

## <a name="next-steps"></a>Következő lépések

Most, hogy megtanulta várólista-tároló alapjait, kövesse ezeket a hivatkozásokat, ha többet szeretne tudni az Azure-tárterületet.

-   [Használatáról a C++ Blob-tárolóhoz](storage-c-plus-plus-how-to-use-blobs.md)
-   [A C++ Táblatároló használata](storage-c-plus-plus-how-to-use-tables.md)
-   [Lista Azure tároló C++-erőforrások](storage-c-plus-plus-enumeration.md)
-   [Tárterület ügyfél tár C++ hivatkozás](http://azure.github.io/azure-storage-cpp)
-   [Azure tároló dokumentáció](https://azure.microsoft.com/documentation/services/storage/)

