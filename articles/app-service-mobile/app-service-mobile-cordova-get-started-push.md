<properties
    pageTitle="Leküldéses értesítések hozzáadása Apache Cordova alkalmazás Azure mobilalkalmazásokkal |} Azure alkalmazás szolgáltatás"
    description="Megtudhatja, hogy miként Azure Mobile-alkalmazások használata a leküldéses értesítéseket küldeni az Apache Cordova alkalmazást."
    services="app-service\mobile"
    documentationCenter="javascript"
    manager="erikre"
    editor=""
    authors="ysxu"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-html"
    ms.devlang="javascript"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="yuaxu"/>

# <a name="add-push-notifications-to-your-apache-cordova-app"></a>Leküldéses értesítések felvétele a Apache Cordova alkalmazásba

[AZURE.INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>– Áttekintés

Ebben az oktatóanyagban ad hozzá leküldéses értesítéseket a [Apache Cordova gyors indítása] projekthez, hogy a leküldéses értesítést küld az eszköz minden alkalommal, amikor a program beszúrja a rekordot.

Ha nem használja a letöltött rövid útmutató az első kiszolgáló projekt, szüksége lesz a leküldéses értesítést bővítmény csomag. [Munka a .NET kódmentes kiszolgálóval Azure Mobile-appokról SDK csomagjában talál](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) további információt talál.

##<a name="prerequisites"></a>Előfeltételek

Ebben az oktatóprogramban egy Apache Cordova alkalmazást a Google Android irányító, Android-eszközön, Windows-eszközön vagy iOS-eszközökön futó Visual Studio-2015 kidolgozni foglalkozik.

Ebben az oktatóanyagban befejezéséhez szükséges:

* PC-n a [Visual Studio közösségi 2015] vagy újabb verziók.
* [Visual Studio Tools for Apache Cordova].
* [Aktív Azure-fiók](https://azure.microsoft.com/pricing/free-trial/).
* [Rövid útmutató Apache Cordova] befejezett projekt.
* (Android) A [Google-fiók] igazolt e-mail címmel rendelkező.
* (iOS) Az Apple Fejlesztőeszközök Program tagság és az iOS-eszközökön (iOS simulatort használja nem támogatja a leküldéses).
* (Windows) A Windows áruházból Fejlesztőeszközök fiók és a Windows 10-es eszköz.

##<a name="configure-hub"></a>Értesítés hubon konfigurálása

[AZURE.INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

[Lépések megjelenítése ebben a részben videó megtekintése](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-3-Create-azure-notification-hub)

##<a name="update-the-server-project-to-send-push-notifications"></a>Leküldéses értesítések küldésére a kiszolgáló projekt frissítése

[AZURE.INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]

##<a name="add-push-to-app"></a>A leküldéses értesítést szeretne kapni a Cordova-alkalmazás módosítása

Győződjön meg róla, hogy a Apache Cordova alkalmazás projekt készen áll a leküldéses értesítések kezelése a Cordova leküldéses beépülő modul plusz platform-specifikus leküldéses szolgáltatások telepítésével.

#### <a name="update-the-cordova-version-in-your-project"></a>Frissítés a projekt Cordova verziója.

Ajánlott frissíteni az ügyfél projekt Cordova 6.1.1 Ha a projekt be van állítva egy régebbi verzióját használja. A projekt frissítéséhez kattintson a jobb gombbal kattintva nyissa meg a konfigurációs Tervező config.xml. Jelölje ki a platformokon fülre, és válassza a 6.1.1 a **Cordova CLI** szövegmezőbe.

Válassza a **Szerkesztés**, majd **Megoldás létrehozása** a projekt frissítése.

#### <a name="install-the-push-plugin"></a>A leküldéses beépülő modul telepítése

Apache Cordova alkalmazások natív módon nem kezeli eszköz vagy a hálózati képességeit.  Ezek a funkciók [npm](https://www.npmjs.com/) vagy GitHub közzétett bővítmények által nyújtott.  A `phonegap-plugin-push` beépülő modul segítségével kezelhető a hálózati leküldéses értesítéseket.

Telepítheti a leküldéses beépülő modul között az alábbi lehetőségek állnak rendelkezésére:

**A parancssorból:**

Hajtsa végre az alábbi parancsot:

    cordova plugin add phonegap-plugin-push

**A Visual Studio:**

1.  A megoldás Intézőben, nyissa meg a `config.xml` fájl **bővítmények**elemre > **egyéni**, **mely számjegy** jelöli ki a telepítési forrást, majd adja meg `https://github.com/phonegap/phonegap-plugin-push` forrásaként.

    ![](./media/app-service-mobile-cordova-get-started-push/add-push-plugin.png)

2.  Kattintson a telepítési forrást melletti nyílra.

3. A **SENDER_ID**Ha már van egy numerikus projekt azonosítója, a Google Fejlesztőeszközök konzol projekt megadhatja azt az alábbi. Ellenkező esetben a helyőrző érték, például 777777, adja meg, és ha Android-alapú céloz ezt az értéket a később config.xml frissítheti.

4. Kattintson a **hozzáadása**gombra.

A leküldéses beépülő modul telepítve van.

####<a name="install-the-device-plugin"></a>Az eszköz beépülő modul telepítése

Kövesse ugyanazt az eljárást a leküldéses beépülő modul telepítéséhez használt, de a Core bővítmények listában megtalálja a beépülő modul eszköz (kattintson a **bővítmények** > **Core** , hogy meglássa). Van szüksége a bővítmény beszerzése a platform nevét (`device.platform`).

#### <a name="register-your-device-for-push-on-start-up"></a>Regisztráljon az eszközt a leküldéses indításkor

Első lépésként is minimális kód Android. Később azt fogja módosításokat néhány kis IOS rendszerben, vagy a Windows 10-es futtatásához.

1. Adja hozzá a **registerForPushNotifications** hívás közben a visszahívás, a bejelentkezési folyamatot, illetve a képernyő alján az **onDeviceReady** módszer:

        // Login to the service.
        client.login('google')
            .then(function () {
                // Create a table reference
                todoItemTable = client.getTable('todoitem');

                // Refresh the todoItems
                refreshDisplay();

                // Wire up the UI Event Handler for the Add Item
                $('#add-item').submit(addItemHandler);
                $('#refresh').on('click', refreshDisplay);

                    // Added to register for push notifications.
                registerForPushNotifications();

            }, handleError);

    Ez a példa **registerForPushNotifications** hívása, miután létrejött a hitelesítési mutatja, amely ajánlott az alkalmazásban a leküldéses értesítéseket és a hitelesítéshez használata esetén.

2. Adja meg az új **registerForPushNotifications** módszer az alábbi képlettel történik:

        // Register for Push Notifications. Requires that phonegap-plugin-push be installed.
        var pushRegistration = null;
        function registerForPushNotifications() {
          pushRegistration = PushNotification.init({
              android: { senderID: 'Your_Project_ID' },
              ios: { alert: 'true', badge: 'true', sound: 'true' },
              wns: {}
          });

        // Handle the registration event.
        pushRegistration.on('registration', function (data) {
          // Get the native platform of the device.
          var platform = device.platform;
          // Get the handle returned during registration.
          var handle = data.registrationId;
          // Set the device-specific message template.
          if (platform == 'android' || platform == 'Android') {
              // Register for GCM notifications.
              client.push.register('gcm', handle, {
                  mytemplate: { body: { data: { message: "{$(messageParam)}" } } }
              });
          } else if (device.platform === 'iOS') {
              // Register for notifications.            
              client.push.register('apns', handle, {
                  mytemplate: { body: { aps: { alert: "{$(messageParam)}" } } }
              });
          } else if (device.platform === 'windows') {
              // Register for WNS notifications.
              client.push.register('wns', handle, {
                  myTemplate: {
                      body: '<toast><visual><binding template="ToastText01"><text id="1">$(messageParam)</text></binding></visual></toast>',
                      headers: { 'X-WNS-Type': 'wns/toast' } }
              });
          }
        });

        pushRegistration.on('notification', function (data, d2) {
          alert('Push Received: ' + data.message);
        });

        pushRegistration.on('error', handleError);
        }

3. (Android) A fenti kóddal cserélje le `Your_Project_ID` numerikus a project azonosító az alkalmazás a [Google Fejlesztőeszközök konzolt].

## <a name="optional-configure-and-run-the-app-on-android"></a>(Nem kötelező) Állítsa be, és futtassa az alkalmazást Android-eszközön

Töltse ki az Android-alapú a leküldéses értesítések engedélyezése ebben a szakaszban.

####<a name="enable-gcm"></a>Firebase engedélyezése a felhő üzenetküldés

Mivel kezdetben vannak kiválasztásával a Google Android platform, engedélyeznie kell a Firebase felhő üzenetküldés. Ha a Microsoft Windows eszközök voltak célba juttatása, hasonlóan szeretné WNS támogatás engedélyezése.

[AZURE.INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

####<a name="configure-backend"></a>A leküldéses kéréseket FCM használatával küldhet mobilalkalmazás kódmentes konfigurálása

[AZURE.INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push.md)]

####<a name="configure-your-cordova-app-for-android"></a>A Cordova alkalmazás beállítása Android-alapú

Az Cordova alkalmazást, nyissa meg a config.xml és cseréje `Your_Project_ID` numerikus a project azonosító az alkalmazás a [Google Fejlesztőeszközök konzolt].

        <plugin name="phonegap-plugin-push" version="1.7.1" src="https://github.com/phonegap/phonegap-plugin-push.git">
            <variable name="SENDER_ID" value="Your_Project_ID" />
        </plugin>

Nyissa meg a index.js, és frissítse a kódot, és használja a numerikus projekt azonosítójával.

        pushRegistration = PushNotification.init({
            android: { senderID: 'Your_Project_ID' },
            ios: { alert: 'true', badge: 'true', sound: 'true' },
            wns: {}
        });

####<a name="configure-device"></a>Az Android-eszközzel USB-hibakereséshez konfigurálása

Mielőtt telepítheti az alkalmazást az Android-eszközzel, hogy engedélyeznie kell USB-hibakeresése során.  Android-telefonra, hajtsa végre az alábbi lépéseket:

1. Nyissa meg a **Beállítások** > **Telefon kapcsolatban**, majd koppintson a **szám összeállítása** addig, amíg a fejlesztői üzemmód engedélyezve van (körülbelül 7 alkalommal).

2. Vissza a **Beállítások** > **Fejlesztői beállítások** **USB-hibakeresés**engedélyezése, majd a csatlakozás a számítógép USB-kábellel fejlesztési Android-telefonon.

Ellenőrizte a rendszert futtató Android 6.0-s (Almaspite) Google Nexus 5 X eszközt használ.  Azonban a technikákat olyan gyakori modern Android kibocsátás keresztül.

#### <a name="install-google-play-services"></a>Telepítse a Google Play szolgáltatásokat.

A leküldéses bővítményt a leküldéses értesítések Android a Google Play szolgáltatások támaszkodik.  

1.  A **Visual Studióban**, kattintson az **eszközök** > **Android** > **Android SDK kezelőben**bontsa ki a **Extrák** mappát, és a jelölőnégyzet bejelölésével, győződjön meg arról, hogy a következő SDK mindegyike telepítve van.
    * Android 2.3-as vagy újabb
    * A Google tárházba verzió 27 vagy újabb verzió
    * A Google Play szolgáltatások 9.0.2 vagy újabb

2.  Kattintson a **Csomagok telepítése** , és várja meg a telepítés befejezéséhez.

Az aktuális szükséges tárak [phonegap-bővítmény-leküldéses dokumentáció]jelennek meg.

#### <a name="test-push-notifications-in-the-app-on-android"></a>Teszt leküldéses értesítéseket a alkalmazásban az Android-eszközön

Most próba leküldéses értesítéseket megteheti az alkalmazást futtató és elemek beszúrása az TodoItem táblázatban. Ezt megteheti ugyanarra az eszközre, vagy egy második eszközről, feltéve, hogy az azonos kódmentes használja. Az alábbi módszerek egyikével tesztelje a Cordova alkalmazás az Android platformon:

- **Fizikai eszközön:**  
A fejlesztői számítógép USB-kábellel csatolhat az Android rendszerű eszközén.  **A Google Android irányító**helyett válassza ki az **eszközt**. Visual Studio fog az eszközön az alkalmazás telepítéséhez, futtatásával.  Kattintson az eszközre az alkalmazás használata.  
A fejlesztői felhasználói élmény fokozása.  Képernyő-megosztási alkalmazások, például [Mobizen] az Android-alkalmazások fejlesztése által egy webböngészőben megjelenítheti a Android képernyő kivetítése PC-n nyújt segítséget.

- **Kattintson egy Android irányító:**  
Vannak olyan irányító forgalmi további beállítási lépéseket.

    Ellenőrizze, hogy bevezetéshez vagy egy virtuális eszközön, amelyen a Google API-hoz a célként beállítása az Android virtuális eszköz (AVD) manager alább látható módon hibakeresési vannak.

    ![](./media/app-service-mobile-cordova-get-started-push/google-apis-avd-settings.png)

    Ha egy gyorsabb x86 használni kívánt irányító, [a HAXM illesztőprogram telepítése](https://taco.visualstudio.com/en-us/docs/run-app-apache/#HAXM) és konfigurálása a irányító vele.

    Google-fiók hozzáadása az Android-készülék **alkalmazások**kattintva > **Beállítások** > **fiók hozzáadása**, majd kövesse az útmutatást követve adja hozzá egy meglévő Google fiókot az eszköz (javasolt meglévő fiókot használ, ahelyett hogy újat hozna létre).

    ![](./media/app-service-mobile-cordova-get-started-push/add-google-account.png)

    Futtassa az listájába alkalmazást, mielőtt, és szúrja be a Teendők új elemet. Ebben az esetben egy értesítési ikon az értesítési területre jelölőnégyzetet. Az értesítés teljes szövegének megjelenítése az értesítési fiókban is megnyithatja.

    ![](./media/app-service-mobile-cordova-get-started-push/android-notifications.png)

##<a name="optional-configure-and-run-on-ios"></a>(Nem kötelező) Állítsa be, és futtassa az iOS

Ez a szakasz nem fut a Cordova projekt iOS rendszerű eszközökön. Ez a szakasz átugorhatja, ha nem működik az iOS-eszközök.

####<a name="install-and-run-the-ios-remotebuild-agent-on-a-mac-or-cloud-service"></a>Telepíteni és futtatni az iOS remotebuild ügynök Mac vagy felhőalapú szolgáltatás

Egy Cordova alkalmazás futtatásához a Visual Studio segítségével iOS, nyissa meg az [iOS beállítási útmutatója](http://taco.visualstudio.com/en-us/docs/ios-guide/) telepítheti és futtathatja a remotebuild agent a lépéseit.

Győződjön meg arról, hogy az iOS-alkalmazás készíthet. A beállítási útmutatója lépéseit az iOS Visual Studio build van szükség. Ha nem rendelkezik a Macen, a remotebuild agent használata egy szolgáltatásba, például MacInCloud IOS hozhat létre. További információt című témakörben talál [a felhőben az iOS-alkalmazás futtatásához](http://taco.visualstudio.com/en-us/docs/build_ios_cloud/).

>[AZURE.NOTE] A leküldéses beépülő modul használata iOS 7-es vagy újabb XCode van szükség.

####<a name="find-the-id-to-use-as-your-app-id"></a>Keresse meg az App ID használni kívánt azonosító

Mielőtt rögzítheti az alkalmazás a leküldéses értesítések, a Cordova alkalmazásban megnyitott config.xml keresse meg a `id` attribútum listáját megjelenítő vezérlővel elemének értékét, és másolja a vágólapra későbbi felhasználás céljából. Az alábbi XML a Azonosítóra van `io.cordova.myapp7777777`.

        <widget defaultlocale="en-US" id="io.cordova.myapp7777777"
        version="1.0.0" windows-packageVersion="1.1.0.0" xmlns="http://www.w3.org/ns/widgets"
            xmlns:cdv="http://cordova.apache.org/ns/1.0" xmlns:vs="http://schemas.microsoft.com/appx/2014/htmlapps">

Később ez az azonosító létrehozásához kövesse az alkalmazás azonosítója Apple developer Portal. (Ha szeretne használni, és hozzon létre egy másik alkalmazás azonosítója developer Portal, szüksége lesz csak néhány további lépés ebben az oktatóanyagban ez az azonosító config.xml módosítása a. A vezérlőmenü elem azonosítója egyeznie kell az alkalmazás azonosítója developer Portal.)

####<a name="register-the-app-for-push-notifications-on-apples-developer-portal"></a>Regisztráljon az alkalmazást az Apple developer Portal segítségével a leküldéses értesítések

[AZURE.INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]

[Hasonló lépésekkel megjelenítő videó megtekintése](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-5-Set-up-apns-for-push)

####<a name="configure-azure-to-send-push-notifications"></a>Azure küldése a leküldéses értesítések beállítása

[AZURE.INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

####<a name="verify-that-your-app-id-matches-your-cordova-app"></a>Ellenőrizze, hogy az alkalmazás azonosítója megfelel-e a Cordova alkalmazás

Ha az alkalmazás azonosítója már létrehozott Apple Fejlesztőeszközök fiókban megfelelő config.xml a vezérlőmenü elem azonosítója, kihagyhatja ezt a lépést. Ha nem egyezik meg az azonosítók, kövesse az alábbi lépéseket:

1. A platformokon mappa töröl a projektből.

2. A bővítmények mappa töröl a projektből.

3. A node_modules mappa töröl a projektből.

4. Frissítse az azonosító attribútum listáját megjelenítő vezérlővel elemének config.xml használni az Ön által létrehozott Apple Fejlesztőeszközök fiókban alkalmazás azonosítója.

5. A projekt újraépítéséhez.

#####<a name="test-push-notifications-in-your-ios-app"></a>Teszt leküldéses értesítéseket, az iOS-alkalmazásban

1. A Visual Studióban győződjön meg arról, hogy **iOS** választotta a telepítés helyének, és válassza az **eszköz** futtatása csatlakoztatott iOS-eszközén.

    A számítógéphez csatlakoztatott iOS-eszközökön futtathatja iTunes használatával. Leküldéses értesítések nem támogatja az iOS simulatort használja.

2. Nyomja le a Visual Studio készítése a projekthez, és indítsa el az alkalmazást az iOS-eszközökön a **F5** vagy a **Futtatás** gombra, majd kattintson **az OK gombra** koppintva fogadja el a leküldéses értesítéseket.

    >[AZURE.NOTE] Az alkalmazás kifejezetten el kell fogadnia leküldéses értesítéseket. A kérés csak az első alkalommal a alkalmazást futtató fordul elő.

3. Abban az alkalmazásban, írja be a feladat, és kattintson a pluszjelre (+) ikonra.

4. Győződjön meg arról, hogy a értesítés érkezik, majd kattintson az OK gombra kattintva zárja be az értesítésre.

##<a name="optional-configure-and-run-on-windows"></a>(Nem kötelező) Állítsa be, és futtassa a Windows

Ez a szakasz nem fut a Apache Cordova app a project (PhoneGap leküldéses a beépülő modul támogatja a Windows 10) a Windows 10-eszközökön. Ez a szakasz kihagyhatja, ha nem működik Windows eszközökkel.

####<a name="register-your-windows-app-for-push-notifications-with-wns"></a>A leküldéses értesítések Windows-alkalmazás regisztrálása WNS

Használni szeretné a tár beállításai a Visual Studióban, kattintson egy Windows célt a megoldást platformokon listából, például a **Windows-x64** vagy **Windows-x86** (elkerülése érdekében a leküldéses értesítések **Windows-AnyCPU** ).

[AZURE.INCLUDE [app-service-mobile-register-wns](../../includes/app-service-mobile-register-wns.md)]

[Hasonló lépésekkel megjelenítő videó megtekintése](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-6-Set-up-wns-for-push)

####<a name="configure-the-notification-hub-for-wns"></a>Az értesítés-központban WNS konfigurálása

[AZURE.INCLUDE [app-service-mobile-configure-wns](../../includes/app-service-mobile-configure-wns.md)]

####<a name="configure-your-cordova-app-to-support-windows-push-notifications"></a>A Cordova alkalmazás támogatja a Windows leküldéses értesítések beállítása

Nyissa meg a konfigurációs Tervező (kattintson a jobb gombbal a config.xml, és válassza **A Tervező nézet**), jelölje be a **Windows** fülre, és válassza a **Windows 10** , **Windows céltábladiagram**verzióban.

>[AZURE.NOTE] Az 5.1.1-es Cordova előtt (javasolt 6.1.1) egy Cordova verzióját használja, ha is be kell az értesítőben szereplő alkalmas jelző config.xml az igaz.

A támogatási leküldéses értesítéseket, az alapértelmezett (hibakeresési) hoz létre, Megnyitás build.json fájl. Másolja a hibakeresési konfigurációs "megjelenés" beállításait.

        "windows": {
            "release": {
                "packageCertificateKeyFile": "res\\native\\windows\\CordovaApp.pfx",
                "publisherId": "CN=yourpublisherID"
            }
        }

A frissítés után a megelőző kód így néz ki.

    "windows": {
        "release": {
            "packageCertificateKeyFile": "res\\native\\windows\\CordovaApp.pfx",
            "publisherId": "CN=yourpublisherID"
            },
        "debug": {
            "packageCertificateKeyFile": "res\\native\\windows\\CordovaApp.pfx",
            "publisherId": "CN=yourpublisherID"
            }
        }

Az alkalmazás összeállítása és ellenőrizheti, hogy nem hibákat. Ügyfél app most regisztráljanak a mobilalkalmazás kódmentes az értesítésekhez. Ismételje meg ebben a szakaszban a megoldás minden Windows projektjére.

####<a name="test-push-notifications-in-your-windows-app"></a>Teszt leküldéses értesítéseket, a Windows-alkalmazásban

A Visual Studióban ellenőrizze, hogy a Windows platformon a telepítés helyének, például a **Windows-x64** vagy **Windows-x86**van kiválasztva. Az alkalmazás futtatásához a Visual Studio szolgáltatója Windows 10 rendszerben válassza a **Helyi számítógép zónában**.

A Futtatás gombra kattintva hozza létre a projekt, és indítsa el az alkalmazást.

Abban az alkalmazásban, írja be egy új todoitem nevét, és kattintson a pluszjelre (+) ikonra koppintva vegye fel.

Győződjön meg arról, hogy értesítést kapott, ha az elem felvétele.

##<a name="next-steps"></a>Következő lépések

* További információ: [Értesítés hubok] , ha többet szeretne tudni a leküldéses értesítéseket.
* Ha még nem tette meg, folytassa az oktatóprogram hitelesítés [Hozzáadása] a Apache Cordova alkalmazásba.

Megtudhatja, hogy miként használhatja a SDK.

* [Apache Cordova SDK]
* [ASP.NET-kiszolgáló SDK]
* [NODE.js Server SDK]

<!-- URLs -->
[Hitelesítés hozzáadása]: app-service-mobile-cordova-get-started-users.md
[Rövid útmutató az Apache Cordova]: app-service-mobile-cordova-get-started.md
[authentication]: app-service-mobile-cordova-get-started-users.md
[Work with the .NET backend server SDK for Azure Mobile Apps]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Google-fiók]: http://go.microsoft.com/fwlink/p/?LinkId=268302
[A Google Fejlesztőeszközök konzolon]: https://console.developers.google.com/home/dashboard
[phonegap-bővítmény-leküldéses dokumentáció]: https://github.com/phonegap/phonegap-plugin-push/blob/master/docs/INSTALLATION.md
[Mobizen]: https://www.mobizen.com/
[Visual Studio közösségi 2015]: http://www.visualstudio.com/
[Visual Studio Tools for Apache Cordova]: https://www.visualstudio.com/en-us/features/cordova-vs.aspx
[Értesítés hubok]: ../notification-hubs/notification-hubs-push-notification-overview.md
[Apache Cordova SDK]: app-service-mobile-cordova-how-to-use-client-library.md
[ASP.NET-kiszolgáló SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[NODE.js Server SDK]: app-service-mobile-node-backend-how-to-use-server-sdk.md
