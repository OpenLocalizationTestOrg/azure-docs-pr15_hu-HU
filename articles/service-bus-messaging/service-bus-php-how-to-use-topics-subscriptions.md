<properties 
    pageTitle="Szolgáltatás Bus témakörök használata PHP |} Microsoft Azure" 
    description="Megtudhatja, hogy miként szolgáltatás Bus témakörök használata PHP Azure-ban." 
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
    ms.date="10/14/2016" 
    ms.author="sethm"/>


# <a name="how-to-use-service-bus-topics-and-subscriptions"></a>Szolgáltatás Bus témakörök és előfizetések használatával

[AZURE.INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

Ez a cikk bemutatja, hogyan szolgáltatás Bus témakörök és előfizetések használatára. A minták PHP írt, és az [Azure SDK PHP](../php-download-sdk.md)használja. Érintett esetek **létrehozása, témák és az előfizetéseket**, **előfizetés szűrők létrehozása**, **üzenetküldés témakörben**, **előfizetésből üzenetek fogadására**és **témakörök és az előfizetés törlése**.

[AZURE.INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

## <a name="create-a-php-application"></a>PHP-alkalmazás létrehozása

Csak kötelező egy az Azure Blob-szolgáltatás hozzáférő PHP-alkalmazás létrehozása az osztályok az [Azure SDK PHP](../php-download-sdk.md) a belül a kód hivatkozni szeretne. Bármely Fejlesztőeszközök hozhat létre a alkalmazást vagy a Jegyzettömbben.

> [AZURE.NOTE] PHP telepítésének is rendelkeznie kell a [OpenSSL bővítmény](http://php.net/openssl) telepítve és engedélyezve van.

Ez a cikk ismerteti, hogyan helyileg PHP alkalmazásból, illetve az Azure webes szerepkör, a dolgozó szerepkör és a webhely futó kód hívható szolgáltatás szolgáltatásokat.

## <a name="get-the-azure-client-libraries"></a>Ismerkedés az Azure ügyfél tárak

[AZURE.INCLUDE [get-client-libraries](../../includes/get-client-libraries.md)]

## <a name="configure-your-application-to-use-service-bus"></a>Az alkalmazás használatához szolgáltatás Bus konfigurálása

A szolgáltatás Bus API-k használata:

1. Hivatkozás az automatikus betöltő fájlt a [require_once] [ require-once] utasítás.
2. Hivatkozás bármelyik osztályok használhat.

A következő példa bemutatja, hogyan lehet az automatikus betöltő fájl és hivatkozást a **ServiceBusService** osztály.

> [AZURE.NOTE] Ez a példa (és más, a jelen cikkben szereplő példák) tartalma feltételezi, a PHP ügyfél képtárak Azure zeneszerző keresztül telepítette. Ha manuálisan vagy KÖRTEFÁK csomag telepítette a tárak, hivatkoznia kell a **WindowsAzure.php** automatikus betöltő fájlt.

```
require_once 'vendor\autoload.php';
use WindowsAzure\Common\ServicesBuilder;
```

Az alábbi példákban a `require_once` utasítás mindig megjelenik, de csak az osztályok a példa végrehajtásához szükséges hivatkozunk.

## <a name="set-up-a-service-bus-connection"></a>A szolgáltatás Bus kapcsolat beállítása

Szolgáltatás Bus ügyfél elindítását először van érvényes kapcsolat-karakterlánc ebben a formátumban:

```
Endpoint=[yourEndpoint];SharedSecretIssuer=[Default Issuer];SharedSecretValue=[Default Key]
```

Ha `Endpoint` van általában a formátum `https://[yourNamespace].servicebus.windows.net`.

Azure service jegyzetlapon létrehozása a **ServicesBuilder** osztály kell használnia. képes vagy:

* Átadni a kapcsolati karakterlánc közvetlenül.
* Jelölje be a kapcsolati karakterlánc több külső forrásból használja a **CloudConfigurationManager (CCM)** :
    * Alapértelmezés szerint akkor megtalálható egy külső adatforrás - környezeti változók támogatása.
    * Új források a **ConnectionStringSource** osztály időtartamának is hozzáadhat.

Az itt ismertetett példák a kapcsolati karakterlánc közvetlenül át.

```
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
    
$connectionString = "Endpoint=[yourEndpoint];SharedSecretIssuer=[Default Issuer];SharedSecretValue=[Default Key]";

$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
```

## <a name="create-a-topic"></a>Témakör létrehozása

A **ServiceBusRestProxy** osztály keresztül szolgáltatás Bus témaköröket felügyeleti műveleteket végezheti el. Egy **ServiceBusRestProxy** objektum összeállítás keresztül a **ServicesBuilder::createServiceBusService** gyári módszer a token engedélyek kezelése, hogy beágyazza megfelelő kapcsolati karakterlánccal.

A következő példa bemutatja, hogyan hozható létre egy **ServiceBusRestProxy** és nevű témakör létrehozása **ServiceBusRestProxy -> createTopic** -hívás `mytopic` belül egy `MySBNamespace` névtér:

```
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\TopicInfo;
    
// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
    
try {       
    // Create topic.
    $topicInfo = new TopicInfo("mytopic");
    $serviceBusRestProxy->createTopic($topicInfo);
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

> [AZURE.NOTE] Használhatja a `listTopics` módszer `ServiceBusRestProxy` objektumok ellenőrzéséhez, ha a témakör a megadott nevű már létezik egy szolgáltatás névtere belül.

## <a name="create-a-subscription"></a>Előfizetés létrehozása

A témakör előfizetések a **ServiceBusRestProxy -> createSubscription** módszerrel is létre. Az előfizetések neve, és beállíthatja, hogy a választható szűrő, amely a kapott az előfizetés virtuális várólista üzenetek készlete korlátozza.

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Az alapértelmezett (MatchAll) szűrővel előfizetés létrehozása

A **MatchAll** szűrő az alapértelmezett szűrő, amely akkor használható, ha nincs szűrő van megadva, amikor új előfizetést jön létre. A **MatchAll** szűrő használatakor a témakör közzétett összes üzenet az előfizetés virtuális várólista kerülnek. A következő példa létrehoz egy "mysubscription" nevű előfizetést, és használja az alapértelmezett **MatchAll** szűrő.

```
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\SubscriptionInfo;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
    
try {
    // Create subscription.
    $subscriptionInfo = new SubscriptionInfo("mysubscription");
    $serviceBusRestProxy->createSubscription("mytopic", $subscriptionInfo);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here: 
    // http://msdn.microsoft.com/library/azure/dd179357
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

### <a name="create-subscriptions-with-filters"></a>Előfizetések szűrőkkel létrehozása

Is állíthat be, amelyek lehetővé teszik, hogy adja meg, mely a témakör küldött üzenetek kell szűrők belül egy adott témakör előfizetés jelennek meg. A legtöbb rugalmas típusú előfizetések által támogatott szűrő a **SqlFilter**, amely SQL92 egy részét. Az üzenetek, a témakör közzétett tulajdonságainak SQL szűrők vonatkozni fognak. SqlFilters kapcsolatos további tudnivalókért lásd: [SqlFilter.SqlExpression tulajdonság][sqlfilter].

> [AZURE.NOTE] Előfizetés a szabályok dolgozza fel a bejövő üzenetek egymástól függetlenül, a eredmény üzenetek hozzáadása az előfizetéshez. Ezeken kívül minden új előfizetést rendelkezik egy alapértelmezett **szabály** objektum szűrő, amely a témakör összes üzenet hozzáadása az előfizetéshez. Csak a szűrőnek megfelelő üzeneteket fogadni, el kell távolítania az alapértelmezés szerinti szabályt. Az alapértelmezett szabály használatával eltávolíthatja a `ServiceBusRestProxy->deleteRule` módot.

Az alábbi példa létrehoz egy névvel ellátott **HighMessages** **SqlFilter** , amely csak a 3-as (lásd: [küldésére témakörben](#send-messages-to-a-topic) foglalkozó egyéni tulajdonságok üzenetek) nagyobb egyéni **LastMessage elemet** tulajdonság üzeneteket kiválasztja az előfizetés:

```
$subscriptionInfo = new SubscriptionInfo("HighMessages");
$serviceBusRestProxy->createSubscription("mytopic", $subscriptionInfo);

$serviceBusRestProxy->deleteRule("mytopic", "HighMessages", '$Default');

$ruleInfo = new RuleInfo("HighMessagesRule");
$ruleInfo->withSqlFilter("MessageNumber > 3");
$ruleResult = $serviceBusRestProxy->createRule("mytopic", "HighMessages", $ruleInfo);
```

Jegyzet, hogy ez a kód van szükség egy további névtér: `WindowsAzure\ServiceBus\Models\SubscriptionInfo`.

Hasonlóképpen az alábbi példa létrehoz egy nevű **LowMessages** **SqlFilter** , amely csak a **LastMessage elemet** tulajdonság kisebb vagy egyenlő 3 üzeneteket kiválasztja az előfizetés:

```
$subscriptionInfo = new SubscriptionInfo("LowMessages");
$serviceBusRestProxy->createSubscription("mytopic", $subscriptionInfo);

$serviceBusRestProxy->deleteRule("mytopic", "LowMessages", '$Default');

$ruleInfo = new RuleInfo("LowMessagesRule");
$ruleInfo->withSqlFilter("MessageNumber <= 3");
$ruleResult = $serviceBusRestProxy->createRule("mytopic", "LowMessages", $ruleInfo);
```

Most, amikor egy üzenetet küld a `mytopic` témakör, mindig átadása vevők fizetett elő, a `mysubscription` előfizetését, és szelektív kézbesítve vevők fizetett elő, hogy a `HighMessages` és `LowMessages` előfizetések (az üzenet tartalmának függően).

## <a name="send-messages-to-a-topic"></a>A tematikus üzeneteket küldeni

Üzenet küldése szolgáltatás Bus témakörben, az alkalmazás hívások **ServiceBusRestProxy -> sendTopicMessage** módszer. A következő kódrészlet szemlélteti, hogyan lehet küldeni az üzenetet a `mytopic` a témakör a korábban létrehozott belül a `MySBNamespace` szolgáltatás névtere.

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
    $serviceBusRestProxy->sendTopicMessage("mytopic", $message);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here: 
    // http://msdn.microsoft.com/library/azure/hh780775
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

Szolgáltatás Bus témakörök küldött üzeneteket a **BrokeredMessage** osztály példánya is. **BrokeredMessage** objektumhoz tartozik-e általános tulajdonságok és módszerek (például **getLabel**, **getTimeToLive**, **setLabel**és **setTimeToLive**), és egyéni az alkalmazás-specifikus tulajdonságok tartása használható tulajdonságait. A következő példa bemutatja, hogyan 5 próba üzeneteket küld a `mytopic` korábban létrehozott témakört. A **tulajdonságbeállítása** módszert egyéni tulajdonság felvétele (`MessageNumber`) minden üzenethez. Megjegyzendő, hogy a `MessageNumber` tulajdonságérték változó minden üzenet (használható ez az érték megállapítása a előfizetésekről kap, az [előfizetés létrehozása](#create-a-subscription) című szakaszban leírt módon):

```
for($i = 0; $i < 5; $i++){
    // Create message.
    $message = new BrokeredMessage();
    $message->setBody("my message ".$i);
            
    // Set custom property.
    $message->setProperty("MessageNumber", $i);
            
    // Send message.
    $serviceBusRestProxy->sendTopicMessage("mytopic", $message);
}
```

Szolgáltatás Bus témakörök a [szabványos réteg](service-bus-premium-messaging.md) 256 KB és 1 MB üzenetek maximális mérete a [prémium réteg](service-bus-premium-messaging.md)használatát támogatja. A fejléc, amelyek tartalmazzák még a szabványos és egyéni alkalmazás tulajdonságait, beállíthatja, hogy mérete legfeljebb 64 KB. Nincs korlátozott tartani a témakör az üzenetek számát, de van egy vonalvég az üzenetek, a témakör birtokában összesített méretét. Ez a témakör méretének felső határ 5 GB. Kvóták kapcsolatos további tudnivalókért olvassa el a [szolgáltatás Bus kvóták][]című témakört.

## <a name="receive-messages-from-a-subscription"></a>Hibaüzenetek előfizetésből

Az előfizetés érkező üzenetek fogadására legkönnyebben **ServiceBusRestProxy -> receiveSubscriptionMessage** metódus használatára. A Beérkezett üzenetek is dolgozhat, két különböző módokban: **ReceiveAndDelete** (alapértelmezett) és **PeekLock**.

Fogadott **ReceiveAndDelete** módban van egyetlen-kép; a művelet Ez azt jelenti, hogy szolgáltatás Bus olvasási vonatkozóan kap megkeresést egy üzenetet az előfizetés, amikor azt az üzenetet az éppen, és adja vissza azt az alkalmazás. **ReceiveAndDelete** mód a legegyszerűbb modellt, és a legjobban, amelyben az alkalmazás nem a hiba esetén üzenet feldolgozása elviseli esetek. Ez megértéséhez, fontolja meg, amelyben a fogyasztói a fogadás kérést, és azt feldolgozása előtt összeomlik, hogy példa. Mivel a szolgáltatás Bus fog megjelölte alatt az elfogyasztott mennyiség megjelenítésére, az üzenetet, és amikor az alkalmazás újraindítása, és megkezdi az üzenetek ismét fogyasztása, akkor fogja hagyott-e az üzenetet, amelyet az összeomlást előtt felhasznált.

**PeekLock** módban üzenetet lesz egy két szakasz műveletet, amely lehetővé teszi, hogy nem elviselni hiányzó üzenetek támogatási alkalmazásokat. Bus szolgáltatási kérelem érkezik, amikor azt talál a következő üzenetre felhasználandó, zárolja megakadályozhatja, hogy más fogyasztói fejeződött be, és visszatér az alkalmazás. Az alkalmazás az üzenet befejezése (vagy tárolja megbízható jövőbeli feldolgozásra) követően a beérkezett üzenetekben- **ServiceBusRestProxy > deleteMessage**megkerülhetők befejezése a másodszintű fogadási folyamat. Szolgáltatás Bus hívás **deleteMessage** láthatja, amikor azt fog megjelölni az üzenetet az éppen, és távolítsa el azt a várakozási sorban található.

A következő példa bemutatja, hogyan fogadása és feldolgozása az üzenetek **PeekLock** mód (nem az alapértelmezett mód). 

```
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\ReceiveMessageOptions;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
        
try {
    // Set receive mode to PeekLock (default is ReceiveAndDelete)
    $options = new ReceiveMessageOptions();
    $options->setPeekLock();
    
    // Get message.
    $message = $serviceBusRestProxy->receiveSubscriptionMessage("mytopic", "mysubscription", $options);

    echo "Body: ".$message->getBody()."<br />";
    echo "MessageID: ".$message->getMessageId()."<br />";
        
    /*---------------------------
        Process message here.
    ----------------------------*/
        
    // Delete message. Not necessary if peek lock is not set.
    echo "Deleting message...<br />";
    $serviceBusRestProxy->deleteMessage($message);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/hh780735
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Útmutató: alkalmazás összeomlik, és nem olvasható üzenetek kezelése

Szolgáltatás Bus segít biztonságosan helyreállítása az alkalmazás vagy egy üzenet feldolgozása nehézségeket hibák funkciókat tartalmaz. Ha a címzett alkalmazások nem lehet feldolgozni az üzenetet valamilyen okból, majd azt felhívhatja a **unlockMessage** módszer (helyett a **deleteMessage** módszer) kapott üzenet a. Ennek hatására a szolgáltatás Bus zárolásának feloldása a várólista belül az üzenetet, és ismét kapott igénybe alkalmazásával ugyanazon vagy másik igénybe alkalmazás elérhetővé.

Is van zárolva van, a sor belül üzenet társított időtúllépés, és ha az alkalmazás nem sikerül egy feldolgozása előtt az üzenetet a zárolási időkorlát lejár (például ha az alkalmazás összeomlik), akkor szolgáltatás Bus zárolásának feloldásához automatikusan az üzenetet, és azt elérhetővé kapott újra.

Abban az esetben, ha az alkalmazás összeomlik, az üzenet végrehajtása után, de a **deleteMessage** kérelem kibocsátása előtt, majd az üzenet fog kell újra az alkalmazást az újraindításkor. Gyakran Link **Legalább egyszer feldolgozása**; Ez azt jelenti, hogy minden üzenet legalább egyszer feldolgozása, de bizonyos esetekben a előfordulhat, hogy újra ugyanezt a hibaüzenetet. Ha az eset ismétlődő feldolgozása nem elviselni, majd alkalmazások fejlesztői számára kell további logika hozzáadása kezelni az ismétlődő üzenetkézbesítés alkalmazások. Ez gyakran érhető el az üzenetet küld, amely a kézbesítési kísérletek keresztül változatlan marad **getMessageId** módszerrel.

## <a name="delete-topics-and-subscriptions"></a>Témák és az előfizetés törlése

A témakör vagy az előfizetés törlése, használja a- **ServiceBusRestProxy -> deleteTopic** vagy **ServiceBusRestProxy -> deleteSubscripton** módszerek a kurzor. Figyelje meg, hogy témakör törlése is törli a témakör a regisztrált előfizetése.

A következő példa bemutatja a nevű témakör törlése `mytopic` és annak regisztrált előfizetések.

```
require_once 'vendor/autoload.php';

use WindowsAzure\ServiceBus\ServiceBusService;
use WindowsAzure\ServiceBus\ServiceBusSettings;
use WindowsAzure\Common\ServiceException;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
    
try {       
    // Delete topic.
    $serviceBusRestProxy->deleteTopic("mytopic");
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here: 
    // http://msdn.microsoft.com/library/azure/dd179357
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

A **deleteSubscription** módszerrel törölheti előfizetés független:

```
$serviceBusRestProxy->deleteSubscription("mytopic", "mysubscription");
```

## <a name="next-steps"></a>Következő lépések

Most, hogy megtanulta szolgáltatás Bus sorban várakozó alapjait, további információt talál [sorban várakozó, témák, és előfizetések][] .

[Sorok, témák és előfizetések]: service-bus-queues-topics-subscriptions.md
[sqlfilter]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx
[require-once]: http://php.net/require_once
[Szolgáltatás Bus kvóták]: service-bus-quotas.md
