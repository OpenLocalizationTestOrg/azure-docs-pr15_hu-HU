<properties
    pageTitle="Küldje el a leküldéses értesítések Azure értesítés hubok a Chrome-alkalmazás |} Microsoft Azure"
    description="Megtudhatja, hogy miként küldhet Azure értesítés hubok leküldéses értesítéseket a a Chrome-at."
    services="notification-hubs"
    keywords="leküldéses értesítései mobilon, leküldéses értesítéseket, leküldéses értesítést, a chrome leküldéses értesítések"
    documentationCenter=""
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-chrome"
    ms.devlang="JavaScript"
    ms.topic="hero-article"
    ms.date="10/03/2016"
    ms.author="yuaxu"/>

# <a name="send-push-notifications-to-chrome-apps-with-azure-notification-hubs"></a>Küldje el a leküldéses értesítések Azure értesítés hubok a Chrome-alkalmazás

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

Ez a témakör bemutatja, hogyan Azure értesítés hubok segítségével küldje el a leküldéses értesítéseket a Chrome-alkalmazásra, amely jelenik meg a Google Chrome környezetén belül. Ebben az oktatóanyagban azt alkalmazás a Chrome-ban, a leküldéses értesítések fogadása a [Google Cloud üzenetküldés (GCM)](https://developers.google.com/cloud-messaging/)hoz létre. 

>[AZURE.NOTE] Ebben az oktatóanyagban befejezéséhez, az aktív Azure fiókkal kell rendelkezniük. Nem rendelkeznek fiókkal, ha mindössze néhány perc is létrehozhat ingyenes próba-fiók. A részletekért lásd: [Azure ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%notification-hubs-chrome-get-started%2F).

Az oktatóprogram végigvezeti az alapvető lépések leküldéses értesítések engedélyezése:

* [A Google Cloud üzenetküldés engedélyezése](#register)
* [Az értesítés központi konfigurálása](#configure-hub)
* [A Chrome alkalmazás csatlakoztatása az értesítési-központban](#connect-app)
* [Leküldéses értesítést küld, a Chrome-alkalmazás](#send)
* [További szolgáltatások és funkciók](#next-steps)

>[AZURE.NOTE] A Chrome alkalmazás leküldéses értesítéseket nem általános böngésző az értesítést – a böngésző bővítési jellemző modell (lásd: további információt [A Chrome-alkalmazások áttekintése] ). Az asztali böngésző mellett a Chrome-alkalmazások futtathatja Mobile (Android és iOS) Apache Cordova. Lásd: a [Chrome alkalmazások Mobile] további információt.

GCM és Azure értesítés hubok konfigurálása megegyezik beállítása Android-alapú a [Google Chrome üzenetküldés felhő] elavult, és az azonos GCM támogatja az Android-eszközökhöz és a Chrome-példányok.

##<a id="register"></a>A Google Cloud üzenetküldés engedélyezése

1. Keresse meg a [Google Cloud konzol] webhelyet, jelentkezzen be a Google-fiók hitelesítő adatait, és kattintson a **Projekt létrehozása** gombra. Adja meg a **Projekt nevét**, és kattintson a **Létrehozás** gombra.

    ![A Google Cloud Console - projekt létrehozása][1]

2. Jegyezze le a **Projekt száma** a **projektek** lap projekt éppen létrehozott. De kell használni ezt a Chrome alkalmazásban a **GCM feladó azonosítója** GCM regisztrálása.

    ![A Google Cloud Console - projekt száma][2]

3. A bal oldali ablaktáblában **API-khoz & auth**, kattintson, majd görgessen le, és kattintson a kapcsoló, amellyel a **Google Cloud az Android-alapú üzenetküldés**engedélyezése. Nem kell **A Google Chrome felhő üzenetküldés**engedélyezése.

    ![A Google Cloud Console - kiszolgáló billentyűt][3]

4. A bal oldali ablaktáblában kattintson a **hitelesítő adatok** > **Hozzon létre új kulcs** > **Server kulcs** > **létrehozása**.

    ![A Google Cloud Console - hitelesítő adatok][4]

5. Jegyezze fel a kiszolgáló **API billentyűt**. Fog konfigurálnia Ez az értesítés-központban ezután ahhoz, hogy a leküldéses értesítéseket küldeni GCM.

    ![A Google Cloud Console - API-ja kulcs][5]

##<a id="configure-hub"></a>Az értesítés központi konfigurálása

[AZURE.INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]


&emsp;&emsp;6. Kattintson a **Beállítások** lap jelölje ki a **Értesítési szolgáltatások** és **Google (GCM)**. Adja meg az API-billentyűt, és mentse.

&emsp;&emsp;![Azure értesítés hubok - Google (GCM)](./media/notification-hubs-android-get-started/notification-hubs-gcm-api.png)

##<a id="connect-app"></a>A Chrome alkalmazás csatlakoztatása az értesítési-központban

Az értesítési központi GCM végezhető most már van beállítva, és az alkalmazás is küldhet és fogadhat leküldéses értesítéseket regisztrálhatja a kapcsolat karakterláncok van. LK

###<a name="create-a-new-chrome-app"></a>A Chrome új alkalmazás létrehozása

Az alábbi példa a [Chrome alkalmazás GCM minta] alapján, és használja a Chrome-alkalmazás létrehozása céljából az ajánlott módszereket. Azt kiemeli az Azure értesítés hubok kifejezetten kapcsolódó lépéseket. 

>[AZURE.NOTE] Azt javasoljuk, hogy a [Chrome alkalmazás értesítést központi minta]Chrome alkalmazás letöltése a forrás.

A Chrome alkalmazás keresztül JavaScript hoz létre, és hozhat létre, akkor használhatja a használni kívánt word szerkesztők közül. Az alábbi képen a Chrome az alkalmazás hasonlóan fog kinézni.

![Google Chrome alkalmazás][15]

1. Hozzon létre egy mappát, és nevezze el `ChromePushApp`. Természetesen a nevem tetszőleges –, adjon meg egy különböző, ellenőrizze, hogy az elérési utat a szükséges kódrészleteket le.

2. Töltse le a [crypto-js tár] a mappában, a második lépésben létrehozott. A könyvtár mappát fogja tartalmazni két almappák: `components` és `rollups`.

3. Hozzon létre egy `manifest.json` fájlt. Mind a Chrome-alkalmazás, amely tartalmazza az alkalmazás-metaadatok és a legtöbb nyilvánvalóan fájl által készül fontosabb, az összes engedélyekkel rendelkeznek a alkalmazásba, amikor a felhasználó telepíti.

        {
          "name": "NH-GCM Notifications",
          "description": "Chrome platform app.",
          "manifest_version": 2,
          "version": "0.1",
          "app": {
            "background": {
              "scripts": ["background.js"]
            }
          },
          "permissions": ["gcm", "storage", "notifications", "https://*.servicebus.windows.net/*"],
          "icons": { "128": "gcm_128.png" }
        }

    Értesítés a `permissions` elemet, amely meghatározza, hogy az alkalmazás a Chrome tudják GCM a leküldéses értesítéseket. Ha a Chrome alkalmazás fog kezdeményezése többi regisztrálhatja Azure értesítés hubok URI is meg kell határoznia.
    A minta alkalmazás is használja az ikon fájl `gcm_128.png`, látni fogja a forrás, amely az eredeti GCM mintából újra. Bármilyen kép, amely megfelel a [ikon feltétel](https://developer.chrome.com/apps/manifest/icons)helyettesítheti.

4. Hozzon létre egy nevű fájlt `background.js` kódot a következő:

        // Returns a new notification ID used in the notification.
        function getNotificationId() {
          var id = Math.floor(Math.random() * 9007199254740992) + 1;
          return id.toString();
        }

        function messageReceived(message) {
          // A message is an object with a data property that
          // consists of key-value pairs.

          // Concatenate all key-value pairs to form a display string.
          var messageString = "";
          for (var key in message.data) {
            if (messageString != "")
              messageString += ", "
            messageString += key + ":" + message.data[key];
          }
          console.log("Message received: " + messageString);

          // Pop up a notification to show the GCM message.
          chrome.notifications.create(getNotificationId(), {
            title: 'GCM Message',
            iconUrl: 'gcm_128.png',
            type: 'basic',
            message: messageString
          }, function() {});
        }

        var registerWindowCreated = false;

        function firstTimeRegistration() {
          chrome.storage.local.get("registered", function(result) {

            registerWindowCreated = true;
            chrome.app.window.create(
              "register.html",
              {  width: 520,
                 height: 500,
                 frame: 'chrome'
              },
              function(appWin) {}
            );
          });
        }

        // Set up a listener for GCM message event.
        chrome.gcm.onMessage.addListener(messageReceived);

        // Set up listeners to trigger the first-time registration.
        chrome.runtime.onInstalled.addListener(firstTimeRegistration);
        chrome.runtime.onStartup.addListener(firstTimeRegistration);

    Ez az a fájlt, megjelenik a Chrome App ablakának (**register.html**) HTML és is határozza meg, hogy miként kezelje a bejövő leküldéses értesítést kezelő **messageReceived** .

5. Hozzon létre egy nevű fájlt `register.html` – Ez határozza meg, hogy a felhasználói felület Chrome alkalmazásra. 

   >[AZURE.NOTE] Ez a példa a **CryptoJS v3.1.2**használja. A tár egy másik verzióját töltötte le, ellenőrizze, hogy a verzió megfelelően le a `src` elérési útját.

        <html>

        <head>
        <title>GCM Registration</title>
        <script src="register.js"></script>
        <script src="CryptoJS v3.1.2/rollups/hmac-sha256.js"></script>
        <script src="CryptoJS v3.1.2/components/enc-base64-min.js"></script>
        </head>

        <body>

        Sender ID:<br/><input id="senderId" type="TEXT" size="20"><br/>
        <button id="registerWithGCM">Register with GCM</button>
        <br/>
        <br/>
        <br/>

        Notification Hub Name:<br/><input id="hubName" type="TEXT" style="width:400px"><br/><br/>
        Connection String:<br/><textarea id="connectionString" type="TEXT" style="width:400px;height:60px"></textarea>

        <br/>

        <button id="registerWithNH" disabled="true">Register with Azure Notification Hubs</button>

        <br/>
        <br/>

        <textarea id="console" type="TEXT" readonly style="width:500px;height:200px;background-color:#e5e5e5;padding:5px"></textarea>

        </body>

        </html>

6. Hozzon létre egy nevű fájlt `register.js` az alábbi kód. Ezt a fájlt, adja meg a parancsfájlt mögött `register.html`. A Chrome-alkalmazások nem engedélyezik a beágyazott végrehajtás, így kell a felhasználói felület hozzon létre egy külön biztonsági parancsfájl.

        var registrationId = "";
        var hubName        = "", connectionString = "";
        var originalUri    = "", targetUri = "", endpoint = "", sasKeyName = "", sasKeyValue = "", sasToken = "";

        window.onload = function() {
           document.getElementById("registerWithGCM").onclick = registerWithGCM;  
           document.getElementById("registerWithNH").onclick = registerWithNH;
           updateLog("You have not registered yet. Please provider sender ID and register with GCM and then with  Notification Hubs.");
        }

        function updateLog(status) {
          currentStatus = document.getElementById("console").innerHTML;
          if (currentStatus != "") {
            currentStatus = currentStatus + "\n\n";
          }

          document.getElementById("console").innerHTML = currentStatus  + status;
        }

        function registerWithGCM() {
          var senderId = document.getElementById("senderId").value.trim();
          chrome.gcm.register([senderId], registerCallback);

          // Prevent register button from being clicked again before the registration finishes.
          document.getElementById("registerWithGCM").disabled = true;
        }

        function registerCallback(regId) {
          registrationId = regId;
          document.getElementById("registerWithGCM").disabled = false;

          if (chrome.runtime.lastError) {
            // When the registration fails, handle the error and retry the
            // registration later.
            updateLog("Registration failed: " + chrome.runtime.lastError.message);
            return;
          }

          updateLog("Registration with GCM succeeded.");
          document.getElementById("registerWithNH").disabled = false;

          // Mark that the first-time registration is done.
          chrome.storage.local.set({registered: true});
        }

        function registerWithNH() {
          hubName = document.getElementById("hubName").value.trim();
          connectionString = document.getElementById("connectionString").value.trim();
          splitConnectionString();
          generateSaSToken();
          sendNHRegistrationRequest();
        }

        // From http://msdn.microsoft.com/library/dn495627.aspx
        function splitConnectionString()
        {
          var parts = connectionString.split(';');
          if (parts.length != 3)
          throw "Error parsing connection string";

          parts.forEach(function(part) {
            if (part.indexOf('Endpoint') == 0) {
            endpoint = 'https' + part.substring(11);
            } else if (part.indexOf('SharedAccessKeyName') == 0) {
            sasKeyName = part.substring(20);
            } else if (part.indexOf('SharedAccessKey') == 0) {
            sasKeyValue = part.substring(16);
            }
          });

          originalUri = endpoint + hubName;
        }

        function generateSaSToken()
        {
          targetUri = encodeURIComponent(originalUri.toLowerCase()).toLowerCase();
          var expiresInMins = 10; // 10 minute expiration

          // Set expiration in seconds.
          var expireOnDate = new Date();
          expireOnDate.setMinutes(expireOnDate.getMinutes() + expiresInMins);
          var expires = Date.UTC(expireOnDate.getUTCFullYear(), expireOnDate
            .getUTCMonth(), expireOnDate.getUTCDate(), expireOnDate
            .getUTCHours(), expireOnDate.getUTCMinutes(), expireOnDate
            .getUTCSeconds()) / 1000;
          var tosign = targetUri + '\n' + expires;

          // Using CryptoJS.
          var signature = CryptoJS.HmacSHA256(tosign, sasKeyValue);
          var base64signature = signature.toString(CryptoJS.enc.Base64);
          var base64UriEncoded = encodeURIComponent(base64signature);

          // Construct authorization string.
          sasToken = "SharedAccessSignature sr=" + targetUri + "&sig="
                          + base64UriEncoded + "&se=" + expires + "&skn=" + sasKeyName;
        }

        function sendNHRegistrationRequest()
        {
          var registrationPayload =
          "<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
          "<entry xmlns=\"http://www.w3.org/2005/Atom\">" +
              "<content type=\"application/xml\">" +
                  "<GcmRegistrationDescription xmlns:i=\"http://www.w3.org/2001/XMLSchema-instance\" xmlns=\"http://schemas.microsoft.com/netservices/2010/10/servicebus/connect\">" +
                      "<GcmRegistrationId>{GCMRegistrationId}</GcmRegistrationId>" +
                  "</GcmRegistrationDescription>" +
              "</content>" +
          "</entry>";

          // Update the payload with the registration ID obtained earlier.
          registrationPayload = registrationPayload.replace("{GCMRegistrationId}", registrationId);

          var url = originalUri + "/registrations/?api-version=2014-09";
          var client = new XMLHttpRequest();

          client.onload = function () {
            if (client.readyState == 4) {
              if (client.status == 200) {
                updateLog("Notification Hub Registration succesful!");
                updateLog(client.responseText);
              } else {
                updateLog("Notification Hub Registration did not succeed!");
                updateLog("HTTP Status: " + client.status + " : " + client.statusText);
                updateLog("HTTP Response: " + "\n" + client.responseText);
              }
            }
          };

          client.onerror = function () {
                updateLog("ERROR - Notification Hub Registration did not succeed!");
          }

          client.open("POST", url, true);
          client.setRequestHeader("Content-Type", "application/atom+xml;type=entry;charset=utf-8");
          client.setRequestHeader("Authorization", sasToken);
          client.setRequestHeader("x-ms-version", "2014-09");

          try {
              client.send(registrationPayload);
          }
          catch(err) {
              updateLog(err.message);
          }
        }

    A fenti parancsfájlt az alábbi főbb paramétereket foglalja magában:
    - **window.onload** a két gombok gombra kattintás eseményeket a felhasználói felület határozza meg. Egy GCM regisztrálja, és a többi használ, a függvény által visszaadott GCM az Azure értesítés hubok regisztrálhatja a regisztráció után regisztrációs Azonosítót.
    - **updateLog** , akkor a függvény, amely lehetővé teszi, hogy us egyszerű naplózás funkciókat kezelheti.
    - **registerWithGCM** az első gombra kattintás kezelő, amely lehetővé teszi a `chrome.gcm.register` GCM az aktuális Chrome alkalmazás példány regisztrálni szeretne hívni.
    - **registerCallback** , akkor a visszahívás függvény, amely kap hívja meg a GCM regisztrációs hívás adja eredményül.
    - **registerWithNH** értesítés hubok regisztrálja második gombra kattintás kezelő. Kap `hubName` és `connectionString` (amely a felhasználó által megadott van), és a értesítés hubok regisztrációs REST API-hívás crafts.
    - **splitConnectionString** és **generateSaSToken** segítők megjelenítő társítások jogkivonat létrehozási folyamat, amely az összes REST API-hívások kell használni a JavaScript végrehajtását. További tudnivalókért olvassa el a [Közös fogalmak](http://msdn.microsoft.com/library/dn495627.aspx)című témakört.
    - **sendNHRegistrationRequest** , akkor a függvény, amely módosít egy hívás HTTP többi Azure értesítés hubok.
    - **registrationPayload** a regisztrációs XML-tartalom határozza meg. További tudnivalókért lásd: [Hozzon létre regisztrációs NH REST API -t]. A regisztrációs Azonosítót, akkor a mi azt kapott GCM frissítjük.
    - **ügyfél** , akkor ahhoz, hogy a HTTP-bejegyzés kérelmet **XMLHttpRequest** egy példánya. Megjegyzendő, hogy elérhetők, frissítjük a `Authorization` élőfej `sasToken`. A hívás sikeres befejezésétől Chrome alkalmazás példány regisztrálása Azure értesítés hubok.


A teljes mappaszerkezet ehhez a projekthez kell hasonló ez:     ![Google Chrome alkalmazás – mappastruktúra][21]

###<a name="set-up-and-test-your-chrome-app"></a>Állítsa be és tesztelje a Chrome-alkalmazás

1. Nyissa meg a Chrome böngészőt. Nyissa meg a **Chrome-bővítmények** , és engedélyezze a **fejlesztői üzemmód**.

    ![Google Chrome - Fejlesztőeszközök mód engedélyezése][16]

2. Kattintson a **kicsomagolt bővítmény betöltése** , és nyissa meg azt a mappát, amelyen létrehozta a fájlokat. A **Chrome-alkalmazások és a bővítmények Fejlesztőeszközök eszköz**tetszés szerint is használhatja. Ez az eszköz önmagában (telepítve van a Chrome webes áruházból) alkalmazás a Chrome-ban, és lehetővé teszi speciális hibakeresési lehetőségeket a Chrome-alkalmazások fejlesztéséhez.

    ![Google Chrome - kicsomagolt bővítmény betöltése][17]

3. Ha a Chrome-alkalmazás létrehozása hibák nélkül, majd megjelenik a Chrome-alkalmazás jelenik meg.

    ![Google Chrome - Chrome alkalmazás megjelenítése][18]

4. Írja be a **Projekt száma** a **Google Cloud konzol** a feladó ID korábbi használ, és kattintson a **GCM regisztrálása**. Olvassa el az üzenetet **sikeres GCM bejegyzésre.**

    ![Google Chrome - Chrome alkalmazás testreszabása][19]

5. Adja meg az **Értesítési központi neve** és a korábban kapott a portálról **DefaultListenSharedAccessSignature** , majd kattintson a **Azure értesítés központi regisztrálása**. Olvassa el az üzenetet **értesítés központi regisztrációs sikeres!** és a regisztrációs választ, amely tartalmazza az Azure értesítés hubok regisztrációs részleteit azonosítóval.

    ![Google Chrome - értesítés központi részletek megadása][20]  

##<a name="send"></a>Értesítés küldése a Chrome-alkalmazásba

Tesztelési célú, azt küld, a Chrome leküldéses értesítéseket a .NET használatával konzol alkalmazást. 

>[AZURE.NOTE] Értesítés hubok a leküldéses értesítések származó minden kódmentes saját nyilvános <a href="http://msdn.microsoft.com/library/windowsazure/dn223264.aspx">többi felületén</a>keresztül is küldhet. Nézze meg a [dokumentáció portál](https://azure.microsoft.com/documentation/services/notification-hubs/) további platformok példákat.

1. A Visual Studióban a **fájl** menüben válassza az **Új** , majd a **Projekt**elemre. A **vizuális C#**kattintson a **Windows** és a **New**, és kattintson **az OK**gombra.  Ez a konzol alkalmazás új projektet hoz létre.

2. Kattintson az **eszközök** menü kattintson a **Tár csomag Manager** , majd a **Csomag kezelője konzol**elemre. A csomag kezelője konzol jeleníti meg.

3. Konzol ablakában végre az alábbi parancsot:

        Install-Package Microsoft.Azure.NotificationHubs

    Ez a lehetőség az Azure Service Bus SDK a <a href="http://nuget.org/packages/  WindowsAzure.ServiceBus/">WindowsAzure.ServiceBus NuGet csomag</a>mutató hivatkozás.

4. Nyissa meg `Program.cs` , és adja hozzá a következő `using` utasítás:

        using Microsoft.Azure.NotificationHubs;

5. Az a `Program` osztály, és adja meg az alábbi módon:

        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            String message = "{\"data\":{\"message\":\"Hello Chrome from Azure Notification Hubs\"}}";
            await hub.SendGcmNativeNotificationAsync(message);
        }

    Győződjön meg arról, hogy le szeretné cserélni a `<hub name>` helyőrző az értesítési-központban, amely a [portálon](https://portal.azure.com) a értesítés központi lap jelenik meg a nevet. A kapcsolati karakterlánc helyőrző is helyettesítése a kapcsolati karakterlánc nevű `DefaultFullSharedAccessSignature` , amely a értesítés központi konfigurálása szakaszban szerezte be.

    >[AZURE.NOTE] Győződjön meg arról, hogy használjon **teljes** hozzáférést, az access nem **meghallgatása** a kapcsolati karakterlánc. **Figyeljen** access kapcsolati karakterlánc nem engedélyeket küldje el a leküldéses értesítéseket.

5. Adja hozzá a következő hívásai a `Main` módszer:

         SendNotificationAsync();
         Console.ReadLine();
         
6. Győződjön meg arról, hogy fut-e a Chrome és a konzol alkalmazásnak a futtatására.

7. Meg kell jelennie a következő értesítő emlékeztető az asztalon.

    ![Google Chrome - értesítés][13]

8. Minden értesítést használatával a Chrome-értesítések ablakban a tálcán (Windows) is megjelenik a Chrome működik, ha.

    ![Google Chrome - értesítések lista][14]

>[AZURE.NOTE] A Chrome alkalmazás haladási van, vagy nyissa meg a böngészőben (bár a Chrome magát rendszernek kell futnia) nem kell. Minden értesítést összevont nézetének is megnyithatja a Chrome-értesítések ablakban.

## <a name="next-steps"> </a>Következő lépések

További tudnivalók az értesítési hubok az [Értesítési hubok áttekintése].

A célként megadott felhasználók, tanulmányozza az [Azure értesítés hubok értesíti a felhasználók] oktatóprogram. 

Ha szeretne kamat csoportok a felhasználói szegmens, kövesse az [Azure értesítés hubok hírek megszakítása] oktatóprogram.

<!-- Images. -->
[1]: ./media/notification-hubs-chrome-get-started/GoogleConsoleCreateProject.PNG
[2]: ./media/notification-hubs-chrome-get-started/GoogleProjectNumber.png
[3]: ./media/notification-hubs-chrome-get-started/EnableGCM.png
[4]: ./media/notification-hubs-chrome-get-started/CreateServerKey.png
[5]: ./media/notification-hubs-chrome-get-started/ServerKey.png
[6]: ./media/notification-hubs-chrome-get-started/CreateNH.png
[7]: ./media/notification-hubs-chrome-get-started/NHNamespace.png
[8]: ./media/notification-hubs-chrome-get-started/NamespaceConfigure.png
[9]: ./media/notification-hubs-chrome-get-started/NHConfigure.png
[10]: ./media/notification-hubs-chrome-get-started/NHConfigureGCM.png
[11]: ./media/notification-hubs-chrome-get-started/NHDashboard.png
[12]: ./media/notification-hubs-chrome-get-started/NHConnString.png
[13]: ./media/notification-hubs-chrome-get-started/ChromeNotification.png
[14]: ./media/notification-hubs-chrome-get-started/ChromeNotificationWindow.png
[15]: ./media/notification-hubs-chrome-get-started/ChromeApp.png
[16]: ./media/notification-hubs-chrome-get-started/ChromeExtensions.png
[17]: ./media/notification-hubs-chrome-get-started/ChromeLoadExtension.png
[18]: ./media/notification-hubs-chrome-get-started/ChromeAppLoaded.png
[19]: ./media/notification-hubs-chrome-get-started/ChromeAppGCM.png
[20]: ./media/notification-hubs-chrome-get-started/ChromeAppNH.png
[21]: ./media/notification-hubs-chrome-get-started/FinalFolderView.png

<!-- URLs. -->
[A Chrome alkalmazás értesítést központi minta]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/PushToChromeApps
[A Google Cloud konzolon]: http://cloud.google.com/console
[Azure Classic Portal]: https://manage.windowsazure.com/
[Értesítés hubok – áttekintés]: notification-hubs-push-notification-overview.md
[A Chrome alkalmazások – áttekintés]: https://developer.chrome.com/apps/about_apps
[A Chrome alkalmazás GCM minta]: https://github.com/GoogleChrome/chrome-app-samples/tree/master/samples/gcm-notifications
[Installable Web Apps]: https://developers.google.com/chrome/apps/docs/
[A Chrome Mobile-alkalmazásokat]: https://developer.chrome.com/apps/chrome_apps_on_mobile
[Regisztráció NH REST API létrehozása]: http://msdn.microsoft.com/library/azure/dn223265.aspx
[Crypto-js-tárban]: http://code.google.com/p/crypto-js/
[GCM with Chrome Apps]: https://developer.chrome.com/apps/cloudMessaging
[A Google Cloud a Chrome üzenetküldés]: https://developer.chrome.com/apps/cloudMessagingV1
[Azure értesítés hubok felhasználók értesítése]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md
[Azure értesítés hubok legfrissebb híreket.]: notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md
