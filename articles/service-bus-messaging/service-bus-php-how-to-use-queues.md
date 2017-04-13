<properties 
    pageTitle="Szolgáltatás Bus sorban várakozó használata PHP |} Microsoft Azure" 
    description="Megtudhatja, hogy miként használja a szolgáltatás Bus sorban várakozó Azure-ban. A mintakódok PHP nyelven íródott." 
    services="service-bus" 
    documentationCenter="php" 
    authors="sethmanheim" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="service-bus" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="PHP" 
    ms.topic="article" 
    ms.date="10/04/2016" 
    ms.author="sethm"/>

# <a name="how-to-use-service-bus-queues"></a>Szolgáltatás Bus sorban várakozó használata

[AZURE.INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

Ez az útmutató bemutatja, hogyan szolgáltatás Bus sorban várakozó használni. A minták PHP írt, és az [Azure SDK PHP](../php-download-sdk.md)használja. Az érintett esetek **sorban várakozó létrehozása**, **üzenetek küldése és fogadása**, és **Sorok törlése**.

[AZURE.INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

## <a name="create-a-php-application"></a>PHP-alkalmazás létrehozása

Csak egy az Azure Blob-szolgáltatás hozzáférő PHP-alkalmazás létrehozása kötelező, az arra hivatkozó az osztályok az [Azure SDK PHP](../php-download-sdk.md) a belül a kód. Bármely Fejlesztőeszközök hozhat létre a alkalmazást vagy a Jegyzettömbben.

> [AZURE.NOTE] PHP telepítésének is rendelkeznie kell a [OpenSSL bővítmény](http://php.net/openssl) telepítve és engedélyezve van.

Ebből az útmutatóból szolgáltatása, amely hívható a helyi meghajtóra PHP alkalmazásból, illetve az Azure webes szerepkör, a dolgozó szerepkör és a webhely futó kód fogja használni.

## <a name="get-the-azure-client-libraries"></a>Ismerkedés az Azure ügyfél tárak

[AZURE.INCLUDE [get-client-libraries](../../includes/get-client-libraries.md)]

## <a name="configure-your-application-to-use-service-bus"></a>Az alkalmazás használatához szolgáltatás Bus konfigurálása

A szolgáltatás Bus várólista API-khoz használni, tegye a következőket:

1. Hivatkozás az automatikus betöltő fájlt a [require_once] [ require_once] utasítás.
2. Hivatkozás bármelyik osztályok használhat.

A következő példa bemutatja, hogyan lehet az automatikus betöltő fájl és hivatkozást a **ServicesBuilder** osztály.

> [AZURE.NOTE] Ez a példa (és más, a jelen cikkben szereplő példák) tartalma feltételezi, a PHP ügyfél képtárak Azure zeneszerző keresztül telepítette. Ha manuálisan vagy KÖRTEFÁK csomag telepítette a tárak, hivatkoznia kell a **WindowsAzure.php** automatikus betöltő fájlt.

```
require_once 'vendor/autoload.php';
use WindowsAzure\Common\ServicesBuilder;
```

Az alábbi példában a `require_once` utasítás mindig megjelenik, de csak az osztályok a példa végrehajtásához szükséges hivatkozunk.

## <a name="set-up-a-service-bus-connection"></a>A szolgáltatás Bus kapcsolat beállítása

Szolgáltatás Bus ügyfél elindítását, először van érvényes kapcsolat-karakterlánc ebben a formátumban:

```
Endpoint=[yourEndpoint];SharedSecretIssuer=[Default Issuer];SharedSecretValue=[Default Key]
```

**Végpont** esetén általában a formátum `[yourNamespace].servicebus.windows.net`.

Azure service jegyzetlapon létrehozása a **ServicesBuilder** osztály kell használnia. képes vagy:

* Átadni a kapcsolati karakterlánc közvetlenül.
* Jelölje be a kapcsolati karakterlánc több külső forrásból használja a **CloudConfigurationManager (CCM)** :
    * Alapértelmezés szerint előtelepített egy külső adatforrás - környezeti változók támogatása
    * A **ConnectionStringSource** osztály időtartamának felvehet új források

Az itt ismertetett példák a kapcsolati karakterlánc fog átadandó közvetlenül.

```
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;

$connectionString = "Endpoint=[yourEndpoint];SharedSecretIssuer=[Default Issuer];SharedSecretValue=[Default Key]";

$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
```

## <a name="how-to-create-a-queue"></a>Útmutató: várólista létrehozása

A szolgáltatás Bus sorban várakozó keresztül az **ServiceBusRestProxy** osztály felügyeleti műveleteket végezheti el. Egy **ServiceBusRestProxy** objektum összeállítás keresztül a **ServicesBuilder::createServiceBusService** gyári módszer a token engedélyek kezelése, hogy beágyazza megfelelő kapcsolati karakterlánccal.

A következő példa bemutatja, hogyan hozható létre egy **ServiceBusRestProxy** és nevű várólista létrehozásához **ServiceBusRestProxy -> createQueue** -hívás `myqueue` belül egy `MySBNamespace` szolgáltatás névtere:

```
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\QueueInfo;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
    
try {
    $queueInfo = new QueueInfo("myqueue");
        
    // Create queue.
    $serviceBusRestProxy->createQueue($queueInfo);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here: 
    // http://msdn.microsoft.com/library/windowsazure/dd179357
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

> [AZURE.NOTE] Használhatja a `listQueues` módszer `ServiceBusRestProxy` objektumok ellenőrzéséhez, ha a megadott nevű már létezik várólista névteret belül.

## <a name="how-to-send-messages-to-a-queue"></a>Útmutató: üzeneteket küldeni egy várólista

A szolgáltatás Bus várólista üzenet elküldéséhez az alkalmazás hívások **ServiceBusRestProxy -> sendQueueMessage** módszer. A következő kódrészlet szemlélteti, hogyan lehet küldeni az üzenetet a `myqueue` belül a korábban létrehozott várólista a `MySBNamespace` szolgáltatás névtere.

```
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\BrokeredMessage;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
        
try {
    // Create message.
    $message = new BrokeredMessage();
    $message->setBody("my message");
    
    // Send message.
    $serviceBusRestProxy->sendQueueMessage("myqueue", $message);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here: 
    // http://msdn.microsoft.com/library/windowsazure/hh780775
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

Az üzenetek küldenek (és a kapott) szolgáltatás Bus sorban várakozó **BrokeredMessage** osztály példánya is. **BrokeredMessage** objektumhoz tartozik-e egy sor olyan szabványos módszerek (például **getLabel**, **getTimeToLive**, **setLabel**és **setTimeToLive**) és egyéni az alkalmazás-specifikus tulajdonságok és a szervezet tetszőleges alkalmazás adatok tárolására használt tulajdonságait.

Szolgáltatás Bus sorban várakozó támogatja a [Normál réteg](service-bus-premium-messaging.md) 256 KB és 1 MB üzenetek maximális mérete a [prémium réteg](service-bus-premium-messaging.md). A fejléc, amelyek tartalmazzák még a szabványos és egyéni alkalmazás tulajdonságait, beállíthatja, hogy mérete legfeljebb 64 KB. Nincs várólista tartott üzenetek száma korlátozott, de van egy vonalvég az üzenetek várólista birtokában összesített méretét. A felső várólista méretét a korlát 5 GB.

## <a name="how-to-receive-messages-from-a-queue"></a>Hogyan várólista érkező üzenetek fogadására

A várólista érkező üzenetek fogadására legkönnyebben **ServiceBusRestProxy -> receiveQueueMessage** metódus használatára. Lehet üzeneteket fogadni két különböző módokban: **ReceiveAndDelete** (alapértelmezett) és **PeekLock**.

A **ReceiveAndDelete** móddal fogadott egy olyan egyetlen-ikon művelet, – ez azt jelenti, hogy szolgáltatás Bus olvasási vonatkozóan kap megkeresést üzenet egy sorban, amikor azt felhasználás alatt álló minősíti az üzenetet, és visszatér azt az alkalmazás. **ReceiveAndDelete** mód a legegyszerűbb modellt, és a legjobban, amelyben az alkalmazás nem a hiba esetén üzenet feldolgozása elviseli esetek. Ez megértéséhez, fontolja meg, amelyben a fogyasztói a fogadás kérést, és azt feldolgozása előtt összeomlik, hogy példa. Mivel a szolgáltatás Bus fog megjelölte alatt az elfogyasztott mennyiség megjelenítésére, az üzenetet, és amikor az alkalmazás újraindítása, és megkezdi az üzenetek ismét fogyasztása, akkor fogja hagyott-e az üzenetet, amelyet az összeomlást előtt felhasznált.

**PeekLock** módban üzenetet lesz egy két szakasz műveletet, amely lehetővé teszi, hogy nem elviselni hiányzó üzenetek támogatási alkalmazásokat. Bus szolgáltatási kérelem érkezik, amikor azt talál a következő üzenetre felhasználandó, zárolja megakadályozhatja, hogy a többi fogyasztói fejeződött be, és visszatér az alkalmazás. Az alkalmazás az üzenet befejezése (vagy tárolja megbízható jövőbeli feldolgozásra) követően a beérkezett üzenetekben- **ServiceBusRestProxy > deleteMessage**megkerülhetők befejezése a másodszintű fogadási folyamat. Szolgáltatás Bus hívás **deleteMessage** láthatja, amikor azt fog megjelölni az üzenetet az éppen, és távolítsa el azt a várakozási sorban található.

A következő példa bemutatja, hogyan üzenet fogadható és feldolgozása **PeekLock** mód (nem az alapértelmezett mód).

```
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\ReceiveMessageOptions;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
        
try {
    // Set the receive mode to PeekLock (default is ReceiveAndDelete).
    $options = new ReceiveMessageOptions();
    $options->setPeekLock();
        
    // Receive message.
    $message = $serviceBusRestProxy->receiveQueueMessage("myqueue", $options);
    echo "Body: ".$message->getBody()."<br />";
    echo "MessageID: ".$message->getMessageId()."<br />";
        
    /*---------------------------
        Process message here.
    ----------------------------*/
        
    // Delete message. Not necessary if peek lock is not set.
    echo "Message deleted.<br />";
    $serviceBusRestProxy->deleteMessage($message);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/windowsazure/hh780735
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Útmutató: alkalmazás összeomlik, és nem olvasható üzenetek kezelése

Szolgáltatás Bus segít biztonságosan helyreállítása az alkalmazás vagy egy üzenet feldolgozása nehézségeket hibák funkciókat tartalmaz. Ha a címzett alkalmazások nem lehet feldolgozni az üzenetet valamilyen okból, majd azt felhívhatja a **unlockMessage** módszer (helyett a **deleteMessage** módszer) kapott üzenet a. Ennek hatására a szolgáltatás Bus zárolásának feloldása a várólista belül az üzenetet, és ismét kapott igénybe alkalmazásával ugyanazon vagy másik igénybe alkalmazás elérhetővé.

Is van zárolva van, a sor belül üzenet társított időtúllépés, és ha az alkalmazás nem sikerül egy feldolgozása előtt az üzenetet a zárolási időkorlát lejár (például ha az alkalmazás összeomlik), akkor szolgáltatás Bus zárolásának feloldásához automatikusan az üzenetet, és azt elérhetővé kapott újra.

Abban az esetben, ha az alkalmazás összeomlik, az üzenet végrehajtása után, de a **deleteMessage** kérelem kibocsátása előtt, majd az üzenet fog kell újra az alkalmazást az újraindításkor. Gyakran Link **Legalább egyszer feldolgozása**; Ez azt jelenti, hogy minden üzenet legalább egyszer feldolgozása, de bizonyos esetekben a előfordulhat, hogy újra ugyanezt a hibaüzenetet. Ha az eset ismétlődő feldolgozása nem elviselni, töltse fel további logika kezelni az ismétlődő üzenetkézbesítés alkalmazások ajánlott. Ez gyakran érhető el az üzenetet küld, amely a kézbesítési kísérletek keresztül változatlan marad **getMessageId** módszerrel.

## <a name="next-steps"></a>Következő lépések

Most, hogy megtanulta szolgáltatás Bus sorban várakozó alapjait, további információt talál [sorban várakozó, témák, és előfizetések][] .

További információt is megjelenik a [PHP Developer Center](/develop/php/).

[Sorok, témák és előfizetések]: service-bus-queues-topics-subscriptions.md
[require_once]: http://php.net/require_once

 
