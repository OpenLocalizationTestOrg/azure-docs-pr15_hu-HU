<properties 
    pageTitle="Értesítés hubok használata PHP" 
    description="Megtudhatja, hogy miként PHP háttéradatbázist Azure értesítés hubok használja." 
    services="notification-hubs" 
    documentationCenter="" 
    authors="ysxu" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="notification-hubs" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="php" 
    ms.devlang="php" 
    ms.topic="article" 
    ms.date="06/07/2016" 
    ms.author="yuaxu"/>

# <a name="how-to-use-notification-hubs-from-php"></a>Értesítés hubok a PHP használata
[AZURE.INCLUDE [notification-hubs-backend-how-to-selector](../../includes/notification-hubs-backend-how-to-selector.md)]

Minden értesítést hubok funkció egy Java/PHP/fonetikus háttéradatbázist a értesítés központi többi felületén a [Értesítés hubok REST API -khoz](http://msdn.microsoft.com/library/dn223264.aspx)MSDN-témakörben ismertetett módon érheti el.

Ez a témakör bemutatjuk hogyan:

* Az értesítés hubok funkcióinak PHP; a többi ügyfél összeállítása
* Kövesse az [első lépések oktatóprogram](notification-hubs-ios-apple-push-notification-apns-get-started.md) a mobil platform a választási lehetőségek, a háttéradatbázist rész PHP végrehajtása.

## <a name="client-interface"></a>Ügyfél-kapcsolat
A fő ügyfél felületén biztosíthat a [.NET értesítés hubok SDK](http://msdn.microsoft.com/library/jj933431.aspx)elérhető ugyanazokat a módszereket, ez lehetővé teszi, hogy közvetlenül fordítása, a oktatóanyagok és a minták jelenleg elérhető ezen a webhelyen, és az interneten a Közösség járult.

Az összes rendelkezésre álló a [többi PHP-csomagolópapír minta]kód megkeresése

Ha például egy ügyfél létrehozása:

    $hub = new NotificationHub("connection string", "hubname"); 

Az iOS natív értesítés küldése:
    
    $notification = new Notification("apple", '{"aps":{"alert": "Hello!"}}');
    $hub->sendNotification($notification, null);

## <a name="implementation"></a>Végrehajtása
Ha még nem jelent, kövesse az [első lépések oktatóprogram] felfelé az utolsó szakasz hol van a háttéradatbázist végrehajtásához.
Ha azt szeretné, a kód, a [többi PHP-csomagolópapír minta] használható, és lépjen közvetlenül a [teljes az oktatóprogram](#complete-tutorial) szakaszban.

Egy teljes többi csomagolópapír végrehajtásához a részletek [az MSDN webhelyen](http://msdn.microsoft.com/library/dn530746.aspx)található. Ebben a részben azt leírja a fő lépéseit, értesítés hubok többi végpontok eléréséhez szükséges PHP végrehajtása:

1. A kapcsolati karakterlánc elemzése
2. A jóváhagyási jogkivonat készítése
3. Végezze el a HTTP-híváshoz

### <a name="parse-the-connection-string"></a>A kapcsolati karakterlánc elemzése

Az alábbiakban a fő osztály végrehajtása az ügyfél, amelynek konstruktor, amely elemzi a kapcsolati karakterláncot:

    class NotificationHub {
        const API_VERSION = "?api-version=2013-10";
    
        private $endpoint;
        private $hubPath;
        private $sasKeyName;
        private $sasKeyValue;
    
        function __construct($connectionString, $hubPath) {
            $this->hubPath = $hubPath;
    
            $this->parseConnectionString($connectionString);
        }
    
        private function parseConnectionString($connectionString) {
            $parts = explode(";", $connectionString);
            if (sizeof($parts) != 3) {
                throw new Exception("Error parsing connection string: " . $connectionString);
            }
    
            foreach ($parts as $part) {
                if (strpos($part, "Endpoint") === 0) {
                    $this->endpoint = "https" . substr($part, 11);
                } else if (strpos($part, "SharedAccessKeyName") === 0) {
                    $this->sasKeyName = substr($part, 20);
                } else if (strpos($part, "SharedAccessKey") === 0) {
                    $this->sasKeyValue = substr($part, 16);
                }
            }
        }
    }


### <a name="create-security-token"></a>Biztonsági jogkivonat létrehozása
A részletek, a biztonsági jogkivonat létrehozásának érhetők el [az alábbi](http://msdn.microsoft.com/library/dn495627.aspx).
Az alábbi módon adhatók hozzá a jogkivonat, a URI az aktuális kérelem és a kapcsolati karakterlánc kiolvasott hitelesítő adatok alapján létrehozni kívánt **NotificationHub** osztály rendelkezik.

    private function generateSasToken($uri) {
        $targetUri = strtolower(rawurlencode(strtolower($uri)));

        $expires = time();
        $expiresInMins = 60;
        $expires = $expires + $expiresInMins * 60;
        $toSign = $targetUri . "\n" . $expires;

        $signature = rawurlencode(base64_encode(hash_hmac('sha256', $toSign, $this->sasKeyValue, TRUE)));

        $token = "SharedAccessSignature sr=" . $targetUri . "&sig="
                    . $signature . "&se=" . $expires . "&skn=" . $this->sasKeyName;

        return $token;
    }

### <a name="send-a-notification"></a>Értesítés küldése
Első lépésként tudassa velünk megadása egy értesítést, amely osztály.

    class Notification {
        public $format;
        public $payload;
    
        # array with keynames for headers
        # Note: Some headers are mandatory: Windows: X-WNS-Type, WindowsPhone: X-NotificationType
        # Note: For Apple you can set Expiry with header: ServiceBusNotification-ApnsExpiry in W3C DTF, YYYY-MM-DDThh:mmTZD (for example, 1997-07-16T19:20+01:00).
        public $headers;
    
        function __construct($format, $payload) {
            if (!in_array($format, ["template", "apple", "windows", "gcm", "windowsphone"])) {
                throw new Exception('Invalid format: ' . $format);
            }
    
            $this->format = $format;
            $this->payload = $payload;
        }
    }

Az osztály a natív értesítés szervezet vagy a sablon értesítést írásmódját tulajdonságait, és beállított egy fejlécek format (belső platform vagy sablon) és a platform-specifikus tulajdonságai (például az Apple lejárati tulajdonság és WNS fejlécek) tartalmazó tároló.

Olvassa el a [értesítés hubok REST API -khoz dokumentáció](http://msdn.microsoft.com/library/dn495827.aspx) , illetve az összes rendelkezésre álló beállítások a meghatározott értesítési platformon formátumok.

Az osztály fegyveres, azt most írhat a Küldés értesítés módszerek az **NotificationHub** osztály belül.

    public function sendNotification($notification, $tagsOrTagExpression="") {
        if (is_array($tagsOrTagExpression)) {
            $tagExpression = implode(" || ", $tagsOrTagExpression);
        } else {
            $tagExpression = $tagsOrTagExpression;
        }

        # build uri
        $uri = $this->endpoint . $this->hubPath . "/messages" . NotificationHub::API_VERSION;
        $ch = curl_init($uri);

        if (in_array($notification->format, ["template", "apple", "gcm"])) {
            $contentType = "application/json";
        } else {
            $contentType = "application/xml";
        }

        $token = $this->generateSasToken($uri);

        $headers = [
            'Authorization: '.$token,
            'Content-Type: '.$contentType,
            'ServiceBusNotification-Format: '.$notification->format
        ];

        if ("" !== $tagExpression) {
            $headers[] = 'ServiceBusNotification-Tags: '.$tagExpression;
        }

        # add headers for other platforms
        if (is_array($notification->headers)) {
            $headers = array_merge($headers, $notification->headers);
        }
        
        curl_setopt_array($ch, array(
            CURLOPT_POST => TRUE,
            CURLOPT_RETURNTRANSFER => TRUE,
            CURLOPT_SSL_VERIFYPEER => FALSE,
            CURLOPT_HTTPHEADER => $headers,
            CURLOPT_POSTFIELDS => $notification->payload
        ));

        // Send the request
        $response = curl_exec($ch);

        // Check for errors
        if($response === FALSE){
            throw new Exception(curl_error($ch));
        }

        $info = curl_getinfo($ch);

        if ($info['http_code'] <> 201) {
            throw new Exception('Error sending notificaiton: '. $info['http_code'] . ' msg: ' . $response);
        }
    } 

A fenti módszerek HTTP POST felkérés küldése az értesítési központi a helyes-e a szervezet és az értesítés küldése a fejlécek /messages végpontját.

##<a name="complete-tutorial"></a>Az oktatóprogram elvégzéséhez.
Az első lépések oktatóprogram letöltése az értesítés küldése a PHP háttéradatbázist hajthatja végre.

Az értesítés hubok ügyfelek (helyette a csatlakozási karakterlánc és a központi nevét az [első lépések oktatóanyagból]utasításai) inicializálni:

    $hub = new NotificationHub("connection string", "hubname"); 

Akkor adja hozzá a Küldés kódot, attól függően, hogy a célhely mobil platform.

### <a name="windows-store-and-windows-phone-81-non-silverlight"></a>A Windows áruházból, és a Windows Phone 8.1 (nem Silverlight)

    $toast = '<toast><visual><binding template="ToastText01"><text id="1">Hello from PHP!</text></binding></visual></toast>';
    $notification = new Notification("windows", $toast);
    $notification->headers[] = 'X-WNS-Type: wns/toast';
    $hub->sendNotification($notification, null);

### <a name="ios"></a>iOS

    $alert = '{"aps":{"alert":"Hello from PHP!"}}';
    $notification = new Notification("apple", $alert);
    $hub->sendNotification($notification, null);

### <a name="android"></a>Android-alapú
    $message = '{"data":{"msg":"Hello from PHP!"}}';
    $notification = new Notification("gcm", $message);
    $hub->sendNotification($notification, null);

### <a name="windows-phone-80-and-81-silverlight"></a>Windows Phone 8.0-ban és 8.1 Silverlight

    $toast = '<?xml version="1.0" encoding="utf-8"?>' .
                '<wp:Notification xmlns:wp="WPNotification">' .
                   '<wp:Toast>' .
                        '<wp:Text1>Hello from PHP!</wp:Text1>' .
                   '</wp:Toast> ' .
                '</wp:Notification>';
    $notification = new Notification("windowsphone", $toast);
    $notification->headers[] = 'X-WindowsPhone-Target : toast';
    $notification->headers[] = 'X-NotificationClass : 2';
    $hub->sendNotification($notification, null);


### <a name="kindle-fire"></a>Kindle Fire táblagéphez
    $message = '{"data":{"msg":"Hello from PHP!"}}';
    $notification = new Notification("adm", $message);
    $hub->sendNotification($notification, null);

A PHP kód futtatását kell konzerv most jelenjen meg a célként megadott eszköz értesítést.


## <a name="next-steps"></a>Következő lépések
Ez a témakör azt hogyan hozhat létre egy egyszerű Java többi ügyfél értesítés hubok mutatott. További lehetőségek közül választhat:

* Töltse le a teljes [PHP többi csomagolópapír minta], mely tartalmazza az összes fenti kódot.
* Továbbra is tanulási kapcsolatos értesítési hubok címkézés funkció az [megszakítása hírek oktatóprogram]
* További tudnivalók: az értesítések terjesztése az egyes felhasználók [értesíti a felhasználók oktatóprogram]

További tudnivalókért lásd még: a [PHP Developer Center](/develop/php/).

[PHP többi csomagolópapír minta]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/notificationhubs-rest-php
[Első lépések oktatóanyagból]: http://azure.microsoft.com/documentation/articles/notification-hubs-ios-get-started/
 
