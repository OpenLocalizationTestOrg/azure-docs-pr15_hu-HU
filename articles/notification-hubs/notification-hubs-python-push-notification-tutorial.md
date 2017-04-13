<properties 
    pageTitle="Értesítés hubok használata Python" 
    description="Megtudhatja, hogy miként Python háttéradatbázist Azure értesítés hubok használja." 
    services="notification-hubs" 
    documentationCenter="" 
    authors="ysxu"
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="notification-hubs" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="python" 
    ms.devlang="php" 
    ms.topic="article" 
    ms.date="06/29/2016" 
    ms.author="yuaxu"/>

# <a name="how-to-use-notification-hubs-from-python"></a>Értesítés hubok a Python használata
[AZURE.INCLUDE [notification-hubs-backend-how-to-selector](../../includes/notification-hubs-backend-how-to-selector.md)]
        
Minden értesítést hubok funkció a fonetikus Java/PHP/Python háttéradatbázist a értesítés központi többi felületén a [Értesítés hubok REST API -khoz](http://msdn.microsoft.com/library/dn223264.aspx)MSDN-témakörben ismertetett módon érheti el.

> [AZURE.NOTE] Ez egy minta hivatkozás végrehajtása az értesítés küldése végrehajtására Python és nem a támogatott hivatalos értesítés központi Python SDK csomagjában talál.
>
> Ez a példa Python 3.4 íródott.

Ez a témakör bemutatjuk hogyan:

* Az értesítési hubok funkcióinak Python a többi ügyfél összeállítása.
* Küldje el a Python felületén a értesítés központi REST API-k értesítéseket. 
* Egy kiírása a HTTP többi kérés/válasz első hibakeresési/oktatási célra. 

Kövesse az [első lépések oktatóprogram](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) a mobil platform a választási lehetőségek, a háttéradatbázist rész Python végrehajtása.

> [AZURE.NOTE] A minta hatókörének csak korlátozott küldhet értesítést, és azt nem minden regisztrációs kezelése.

## <a name="client-interface"></a>Ügyfél-kapcsolat
A fő ügyfél felületén megadhatja az elérhető a [.NET értesítés hubok SDK](http://msdn.microsoft.com/library/jj933431.aspx)ugyanazokat a módszereket. Lehetővé teszi, hogy közvetlenül a a oktatóanyagok és a webhely jelenleg elérhető minták lefordítása, és az interneten a Közösség járult.

Az összes rendelkezésre álló a [többi Python csomagolópapír minta]kód megkeresése

Ha például egy ügyfél létrehozása:

    isDebug = True
    hub = NotificationHub("myConnectionString", "myNotificationHubName", isDebug)
    
A Windows bejelentési értesítés küldése:
    
    wns_payload = """<toast><visual><binding template=\"ToastText01\"><text id=\"1\">Hello world!</text></binding></visual></toast>"""
    hub.send_windows_notification(wns_payload)
    
## <a name="implementation"></a>Végrehajtása
Ha még nem jelent, kövesse az [első lépések oktatóprogram] felfelé az utolsó szakasz hol van a háttéradatbázist végrehajtásához.

Egy teljes többi csomagolópapír végrehajtásához a részletek [az MSDN webhelyen](http://msdn.microsoft.com/library/dn530746.aspx)található. Ebben a részben azt leírja a fő lépéseit, értesítés hubok többi végpontok és értesítések küldésére Python végrehajtása

1. A kapcsolati karakterlánc elemzése
2. A jóváhagyási jogkivonat készítése
3. HTTP REST API segítségével értesítés küldése

### <a name="parse-the-connection-string"></a>A kapcsolati karakterlánc elemzése

Az alábbiakban a fő osztály végrehajtása az ügyfél, amelynek konstruktor elemzi a kapcsolati karakterláncot:

    class NotificationHub:
        API_VERSION = "?api-version=2013-10"
        DEBUG_SEND = "&test"
    
        def __init__(self, connection_string=None, hub_name=None, debug=0):
            self.HubName = hub_name
            self.Debug = debug
    
            # Parse connection string
            parts = connection_string.split(';')
            if len(parts) != 3:
                raise Exception("Invalid ConnectionString.")
    
            for part in parts:
                if part.startswith('Endpoint'):
                    self.Endpoint = 'https' + part[11:]
                if part.startswith('SharedAccessKeyName'):
                    self.SasKeyName = part[20:]
                if part.startswith('SharedAccessKey'):
                    self.SasKeyValue = part[16:]


### <a name="create-security-token"></a>Biztonsági jogkivonat létrehozása
A részletek, a biztonsági jogkivonat létrehozásának érhetők el [az alábbi](http://msdn.microsoft.com/library/dn495627.aspx).
Az alábbi módszerekkel kell adható hozzá a **NotificationHub** osztály hozhat létre a jogkivonat, a URI az aktuális kérelem és a kapcsolati karakterlánc kiolvasott hitelesítő adatok alapján.

    @staticmethod
    def get_expiry():
        # By default returns an expiration of 5 minutes (=300 seconds) from now
        return int(round(time.time() + 300))

    @staticmethod
    def encode_base64(data):
        return base64.b64encode(data)

    def sign_string(self, to_sign):
        key = self.SasKeyValue.encode('utf-8')
        to_sign = to_sign.encode('utf-8')
        signed_hmac_sha256 = hmac.HMAC(key, to_sign, hashlib.sha256)
        digest = signed_hmac_sha256.digest()
        encoded_digest = self.encode_base64(digest)
        return encoded_digest

    def generate_sas_token(self):
        target_uri = self.Endpoint + self.HubName
        my_uri = urllib.parse.quote(target_uri, '').lower()
        expiry = str(self.get_expiry())
        to_sign = my_uri + '\n' + expiry
        signature = urllib.parse.quote(self.sign_string(to_sign))
        auth_format = 'SharedAccessSignature sig={0}&se={1}&skn={2}&sr={3}'
        sas_token = auth_format.format(signature, expiry, self.SasKeyName, my_uri)
        return sas_token

### <a name="send-a-notification-using-http-rest-api"></a>HTTP REST API segítségével értesítés küldése
Először is, hogy használata egy értesítést, amely osztály határozza meg.

    class Notification:
        def __init__(self, notification_format=None, payload=None, debug=0):
            valid_formats = ['template', 'apple', 'gcm', 'windows', 'windowsphone', "adm", "baidu"]
            if not any(x in notification_format for x in valid_formats):
                raise Exception(
                    "Invalid Notification format. " +
                    "Must be one of the following - 'template', 'apple', 'gcm', 'windows', 'windowsphone', 'adm', 'baidu'")
    
            self.format = notification_format
            self.payload = payload
    
            # array with keynames for headers
            # Note: Some headers are mandatory: Windows: X-WNS-Type, WindowsPhone: X-NotificationType
            # Note: For Apple you can set Expiry with header: ServiceBusNotification-ApnsExpiry
            # in W3C DTF, YYYY-MM-DDThh:mmTZD (for example, 1997-07-16T19:20+01:00).
            self.headers = None

Az osztály a natív értesítés szervezet vagy abban az esetben, ha sablon értesítést, élőfejek format (belső platform vagy sablon) és a platform-specifikus tulajdonságai (például az Apple lejárati tulajdonság és WNS fejlécek) tartalmazó halmazának tulajdonságait tároló.

Olvassa el a [értesítés hubok REST API -khoz dokumentáció](http://msdn.microsoft.com/library/dn495827.aspx) , illetve az összes rendelkezésre álló beállítások a meghatározott értesítési platformon formátumok.

Most már az Ez az osztály azt írhat a Küldés értesítés módszerek az **NotificationHub** osztály belül.

    def make_http_request(self, url, payload, headers):
        parsed_url = urllib.parse.urlparse(url)
        connection = http.client.HTTPSConnection(parsed_url.hostname, parsed_url.port)

        if self.Debug > 0:
            connection.set_debuglevel(self.Debug)
            # adding this querystring parameter gets detailed information about the PNS send notification outcome
            url += self.DEBUG_SEND
            print("--- REQUEST ---")
            print("URI: " + url)
            print("Headers: " + json.dumps(headers, sort_keys=True, indent=4, separators=(' ', ': ')))
            print("--- END REQUEST ---\n")

        connection.request('POST', url, payload, headers)
        response = connection.getresponse()

        if self.Debug > 0:
            # print out detailed response information for debugging purpose
            print("\n\n--- RESPONSE ---")
            print(str(response.status) + " " + response.reason)
            print(response.msg)
            print(response.read())
            print("--- END RESPONSE ---")

        elif response.status != 201:
            # Successful outcome of send message is HTTP 201 - Created
            raise Exception(
                "Error sending notification. Received HTTP code " + str(response.status) + " " + response.reason)

        connection.close()

    def send_notification(self, notification, tag_or_tag_expression=None):
        url = self.Endpoint + self.HubName + '/messages' + self.API_VERSION

        json_platforms = ['template', 'apple', 'gcm', 'adm', 'baidu']

        if any(x in notification.format for x in json_platforms):
            content_type = "application/json"
            payload_to_send = json.dumps(notification.payload)
        else:
            content_type = "application/xml"
            payload_to_send = notification.payload

        headers = {
            'Content-type': content_type,
            'Authorization': self.generate_sas_token(),
            'ServiceBusNotification-Format': notification.format
        }

        if isinstance(tag_or_tag_expression, set):
            tag_list = ' || '.join(tag_or_tag_expression)
        else:
            tag_list = tag_or_tag_expression

        # add the tags/tag expressions to the headers collection
        if tag_list != "":
            headers.update({'ServiceBusNotification-Tags': tag_list})

        # add any custom headers to the headers collection that the user may have added
        if notification.headers is not None:
            headers.update(notification.headers)

        self.make_http_request(url, payload_to_send, headers)

    def send_apple_notification(self, payload, tags=""):
        nh = Notification("apple", payload)
        self.send_notification(nh, tags)

    def send_gcm_notification(self, payload, tags=""):
        nh = Notification("gcm", payload)
        self.send_notification(nh, tags)

    def send_adm_notification(self, payload, tags=""):
        nh = Notification("adm", payload)
        self.send_notification(nh, tags)

    def send_baidu_notification(self, payload, tags=""):
        nh = Notification("baidu", payload)
        self.send_notification(nh, tags)

    def send_mpns_notification(self, payload, tags=""):
        nh = Notification("windowsphone", payload)

        if "<wp:Toast>" in payload:
            nh.headers = {'X-WindowsPhone-Target': 'toast', 'X-NotificationClass': '2'}
        elif "<wp:Tile>" in payload:
            nh.headers = {'X-WindowsPhone-Target': 'tile', 'X-NotificationClass': '1'}

        self.send_notification(nh, tags)

    def send_windows_notification(self, payload, tags=""):
        nh = Notification("windows", payload)

        if "<toast>" in payload:
            nh.headers = {'X-WNS-Type': 'wns/toast'}
        elif "<tile>" in payload:
            nh.headers = {'X-WNS-Type': 'wns/tile'}
        elif "<badge>" in payload:
            nh.headers = {'X-WNS-Type': 'wns/badge'}

        self.send_notification(nh, tags)

    def send_template_notification(self, properties, tags=""):
        nh = Notification("template", properties)
        self.send_notification(nh, tags)

A fenti módszerek HTTP POST felkérés küldése az értesítési központi a helyes-e a szervezet és az értesítés küldése a fejlécek /messages végpontját.

### <a name="using-debug-property-to-enable-detailed-logging"></a>Hibakeresési tulajdonát felhasználni a részletes naplózás engedélyezése
Naplózás részletes információt a HTTP-kérés hibakeresési tulajdonság engedélyezése az értesítési-központban inicializálásakor ír, és a válasz kiírása, valamint részletes értesítő üzenet küldése az eredményeket. A legutóbb jelöltük a tulajdonság értékét adja vissza az értesítés küldése így információt az [értesítés hubok TestSend tulajdonság](http://msdn.microsoft.com/library/microsoft.servicebus.notifications.notificationhubclient.enabletestsend.aspx) neve. Ahhoz, hogy használhassa - inicializálni, használja az alábbi:

    hub = NotificationHub("myConnectionString", "myNotificationHubName", isDebug)

Az értesítési központi kérés küldése HTTP URL-cím kap ellátva egy "próba" lekérdezési karakterlánc az eredményt adja. 

##<a name="complete-tutorial"></a>Az oktatóprogram elvégzéséhez.
Az első lépések oktatóprogram letöltése az értesítés küldése a Python háttéradatbázist hajthatja végre.

Az értesítés hubok ügyfelek (helyette a csatlakozási karakterlánc és a központi nevét az [első lépések oktatóprogram]utasításai) inicializálni:

    hub = NotificationHub("myConnectionString", "myNotificationHubName")

Akkor adja hozzá a Küldés kódot, attól függően, hogy a célhely mobil platform. Ez a példa a magasabb szintű módszereket, amelyekkel a platform, például a windows; send_windows_notification alapján küldő értesítések engedélyezése is hozzáadása send_apple_notification (az apple) stb. 

### <a name="windows-store-and-windows-phone-81-non-silverlight"></a>A Windows áruházból, és a Windows Phone 8.1 (nem Silverlight)

    wns_payload = """<toast><visual><binding template=\"ToastText01\"><text id=\"1\">Test</text></binding></visual></toast>"""
    hub.send_windows_notification(wns_payload)

### <a name="windows-phone-80-and-81-silverlight"></a>Windows Phone 8.0-ban és 8.1 Silverlight

    hub.send_mpns_notification(toast)

### <a name="ios"></a>iOS

    alert_payload = {
        'data':
            {
                'msg': 'Hello!'
            }
    }
    hub.send_apple_notification(alert_payload)

### <a name="android"></a>Android-alapú
    gcm_payload = {
        'data':
            {
                'msg': 'Hello!'
            }
    }
    hub.send_gcm_notification(gcm_payload)

### <a name="kindle-fire"></a>Kindle Fire táblagéphez
    adm_payload = {
        'data':
            {
                'msg': 'Hello!'
            }
    }
    hub.send_adm_notification(adm_payload)

### <a name="baidu"></a>Baidu
    baidu_payload = {
        'data':
            {
                'msg': 'Hello!'
            }
    }
    hub.send_baidu_notification(baidu_payload)

A Python kód futtatását jelenjen meg a célként megadott eszköz értesítést kell hozhat létre.

## <a name="examples"></a>Példa:

### <a name="enabling-debug-property"></a>Hibakeresési tulajdonság engedélyezése
Ha engedélyezi az hibakeresési jelző a NotificationHub inicializálásakor, akkor látni fogja részletes HTTP-kérés és válasz kiírása, valamint NotificationOutcome az alábbiakhoz hasonlóan hol megismerheti, milyen HTTP-fejlécek át az-összehívásban, és milyen HTTP-válasz érkezett az értesítési-központban:    ![][1]

Jelenik meg értesítés központi eredménye például részletes 

- Amikor az üzenet sikeresen küldi a leküldéses értesítéseket kezelő szolgáltatás. 
    
        <Outcome>The Notification was successfully sent to the Push Notification System</Outcome>

- Ha nem voltak a leküldéses értesítések található célok majd valószínűleg fogja a válaszban (Ez jelzi, hogy volt-e az értesítés valószínűleg ad, mert a regisztrációk néhány egyező címkék található regisztrációt) a következő témakörben találhat

        '<NotificationOutcome xmlns="http://schemas.microsoft.com/netservices/2010/10/servicebus/connect" xmlns:i="http://www.w3.org/2001/XMLSchema-instance"><Success>0</Success><Failure>0</Failure><Results i:nil="true"/></NotificationOutcome>'

### <a name="broadcast-toast-notification-to-windows"></a>A Windows értesítőjére közvetítése 

Figyelje meg, hogy ki sikerül elküldeni, ha a szórás értesítőjére Windows ügyfélnek küld a fejléceket. 

    hub.send_windows_notification(wns_payload)

![][2]

### <a name="send-notification-specifying-a-tag-or-tag-expression"></a>Címke (vagy kifejezés címke) tartalmazó értesítés küldése

Figyelje meg, a címkék HTTP-fejléc, amelyeket a rendszer hozzáadja a HTTP-kérés (az alábbi példában azt küld az értesítés csak az "a sports tartalom regisztrációk)

    hub.send_windows_notification(wns_payload, "sports")

![][3]

### <a name="send-notification-specifying-multiple-tags"></a>Több címkét tartalmazó értesítés küldése

Figyelje meg, hogyan változik a címkék HTTP-fejléc, több címkék elküldésekor. 
    
    tags = {'sports', 'politics'}
    hub.send_windows_notification(wns_payload, tags)

![][4]

### <a name="templated-notification"></a>Sablon értesítés

Figyelje meg, hogy a formátum HTTP-fejléc változik, és a tartalom szervezet HTTP összehívás törzsébe részeként küldhetők el:

**Ügyféloldali - regisztrált sablon**

        var template =
                        @"<toast><visual><binding template=""ToastText01""><text id=""1"">$(greeting_en)</text></binding></visual></toast>";
            
**Kiszolgálóoldali – a tartalom küldése**
        
        template_payload = {'greeting_en': 'Hello', 'greeting_fr': 'Salut'}
        hub.send_template_notification(template_payload)

![][5]


## <a name="next-steps"></a>Következő lépések
Ez a témakör azt hogyan hozhat létre egy egyszerű Python többi ügyfél értesítés hubok mutatott. További lehetőségek közül választhat:

* Töltse le a teljes [Python többi csomagolópapír minta], mely tartalmazza az összes fenti kódot.
* Továbbra is tanulási kapcsolatos értesítési hubok címkézés funkció az [oktatóprogram hírek megszakítása]
* Az [oktatóprogram honosítása során hírek] továbbra is tanulási értesítés hubok-sablonok szolgáltatásról

<!-- URLs -->
[Python többi csomagolópapír minta]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/notificationhubs-rest-python
[Első lépések oktatóprogram]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-get-started/
[Hírek oktatóprogram megszakítása]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-send-breaking-news/
[Honosítása hírek oktatóprogram során]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-send-localized-breaking-news/

<!-- Images. -->
[1]: ./media/notification-hubs-python-backend-how-to/DetailedLoggingInfo.png
[2]: ./media/notification-hubs-python-backend-how-to/BroadcastScenario.png
[3]: ./media/notification-hubs-python-backend-how-to/SendWithOneTag.png
[4]: ./media/notification-hubs-python-backend-how-to/SendWithMultipleTags.png
[5]: ./media/notification-hubs-python-backend-how-to/TemplatedNotification.png
 
