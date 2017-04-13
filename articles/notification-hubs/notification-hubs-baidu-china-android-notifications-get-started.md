<properties
    pageTitle="Első lépések az Azure értesítés hubok Baidu használatával |} Microsoft Azure"
    description="Ebből az oktatóanyagból megismerheti, hogyan Azure értesítés hubok használatával Baidu használata Android-eszközön való leküldéses értesítéseket."
    services="notification-hubs"
    documentationCenter="android"
    authors="ysxu"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.devlang="java"
    ms.topic="hero-article"
    ms.tgt_pltfrm="mobile-baidu"
    ms.workload="mobile"
    ms.date="08/19/2016"
    ms.author="yuaxu"/>

# <a name="get-started-with-notification-hubs-using-baidu"></a>Értesítés hubok Baidu használata – első lépések

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

##<a name="overview"></a>– Áttekintés

Baidu felhő leküldéses egy kínai felhőalapú szolgáltatásba, hogy a leküldéses értesítéseket küldeni mobileszközök is használhatja. Ez a szolgáltatás különösen fontos Kínában, androidon előadásához a leküldéses értesítések esetén összetett másik alkalmazás-áruház és a leküldéses szolgáltatások mellett az Android-eszközök, amelyek nem csatlakozik a szokásos (a Google Cloud üzenetküldés) GCM elérhető jelenléte miatt.

##<a name="prerequisites"></a>Előfeltételek

Ebben az oktatóprogramban az alábbiakra van szükség:

+ Android SDK (feltételezzük, hogy Holdas használandó), amely letölthető az <a href="http://go.microsoft.com/fwlink/?LinkId=389797">Android-webhely</a>
+ [Android SDK mobil szolgáltatások]
+ [Android SDK baidu leküldéses]

>[AZURE.NOTE] Ebben az oktatóanyagban befejezéséhez, az aktív Azure fiókkal kell rendelkezniük. Nem rendelkeznek fiókkal, ha mindössze néhány perc is létrehozhat ingyenes próba-fiók. A részletekért lásd: [Azure ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-baidu-get-started%2F).


##<a name="create-a-baidu-account"></a>Baidu-fiók létrehozása

Baidu használatához Baidu fiókkal kell rendelkeznie. Ha már van egy, jelentkezzen be az [Baidu portálra] , és ugorja át a következő lépéssel. Egyéb esetben nézze meg az alábbi utasításokat Baidu fiók létrehozása.  

1. Nyissa meg a [Baidu portálra] , és a**登录**(**Bejelentkezés**) hivatkozásra. Kattintson a**立即注册**a fiók regisztrációs folyamat megkezdéséhez.

    ![][1]

2. Adja meg a szükséges adatait – telefon vagy e-mail címet, a jelszót és az ellenőrzés kód –, és kattintson az **előfizetés**gombra.

    ![][2]

3. Akkor küld e-mailben, hogy aktiválja fiókját Baidu mutató hivatkozást tartalmazó megadott e-mail címre.

    ![][3]

4. Jelentkezzen be az e-mail fiókhoz, nyissa meg a Baidu aktiválási mail, és kattintson az aktiváláshoz szükséges hivatkozást, hogy aktiválja fiókját Baidu.

    ![][4]

Ha már aktiválva Baidu fiókkal, jelentkezzen be a [Baidu portálon].

##<a name="register-as-a-baidu-developer"></a>Regisztráció Baidu fejlesztői

1. Ha be van jelentkezve a [Baidu portálon], kattintson az**更多 >>** (**További**).

    ![][5]

2. **站长与开发者服务 (Webmesteri és fejlesztői szolgáltatások)** szakaszában görgessen le, és kattintson a**百度开放云平台**(**Baidu nyissa meg a felhőben platform**).

    ![][6]

3. A következő lapon kattintson a:**开发者服务**(**Fejlesztőeszközök szolgáltatások**)-jobb felső sarokban.

    ![][7]

4. A következő lapon kattintson a**注册开发者**(**Regisztrálva fejlesztők**) kattintson a jobb felső sarokban a menüből.

    ![][8]

5. Írja be a neve, leírás és a mobiltelefonszám ellenőrzési szöveges üzenetet, és kattintson a**送验证码**(**Ellenőrzőkódot küldése**). Fontos tudni, hogy a nemzetközi telefonszámokat, akkor az országkódot tegye zárójelbe. Ha például az Amerikai Egyesült Államok számot, akkor **(1) 1234567890**.

    ![][9]

6. Kell majd kap szöveg ellenőrzési szám, a következő példában látható módon:

    ![][10]

7. Adja meg az ellenőrzési száma**验证码**(**jóváhagyási kódot**) az üzenetből.

8. Végezetül fejezze be a Fejlesztőeszközök regisztráció a Baidu szerződést, és kattintson a**提交**(**Küldés**). A következő oldal regisztráció sikeres befejezésétől jelennek meg:

    ![][11]

##<a name="create-a-baidu-cloud-push-project"></a>Baidu felhő leküldéses projekt létrehozása

Baidu felhő leküldéses projektet hoz létre, amikor az alkalmazás azonosítója kap API-billentyűt, és a titkos kulcs.

1. Ha be van jelentkezve a [Baidu portálon], kattintson az**更多 >>** (**További**).

    ![][5]

2. **站长与开发者服务**(**Webmesteri és a fejlesztői szolgáltatások**) szakaszában görgessen le, és kattintson a**百度开放云平台**(**Baidu nyissa meg a felhőben platform**).

    ![][6]

3. A következő lapon kattintson a:**开发者服务**(**Fejlesztőeszközök szolgáltatások**)-jobb felső sarokban.

    ![][7]

4. A következő lapon kattintson a**云推送**(a**Felhőben leküldéses**)**云服务**(**Cloud Services**) szakaszában.

    ![][12]

5. Regisztrált a fejlesztők vannak, látni fogja a:**管理控制台**(**Kezelőkonzol**)-a felső menüből. Kattintson a**开发者服务管理**(a**fejlesztők Szolgáltatáskezelés**).

    ![][13]

6. A következő lapon kattintson a**创建工程**(**Projekt létrehozása**) lehetőséget.

    ![][14]

7. Írja be az alkalmazás nevét, és**创建**(**Létrehozás**) gombra.

    ![][15]

8. Baidu felhő leküldéses projekt sikeres létrehozás után megjelenik a lapon **AppID**, **API-billentyűt**, és a **Titkos kulcs**van. Jegyezze le a API-billentyűt, és a titkos kulcs, hogy később fogja használni.

    ![][16]

9. A projekt a leküldéses értesítések konfigurálása:**云推送**(**Felhő leküldéses**)-bal oldali ablaktáblában a gombra kattintva.

    ![][31]

10. A következő lapon kattintson a**推送设置**(**leküldéses beállítások**) gombra.

    ![][32]  

11. Az beállítása lapon adja meg, hogy használni kell a Android projektben, a**应用包名**(**Alkalmazáscsomag**) mezőben használja, és válassza a (**Mentés**)**保存设置**csomag nevét.  

    ![][33]

Ekkor megjelenik a**保存成功!** (**sikeresen mentett!**) üzenet.

##<a name="configure-your-notification-hub"></a>Az értesítés központi konfigurálása

1. Jelentkezzen be a [Klasszikus Azure-portálra], és kattintson az **+ Új** a képernyő alján.

2. Kattintson az **Alkalmazás szolgáltatások**, kattintson a **Szolgáltatás Bus**, **Értesítés központi**, gombmenü **Gyors létrehozása**.

3. Nevezze el a **Központi értesítést**, jelölje be a **terület** és a **Namespace** , ha ez az értesítés központi létrejön, és kattintson az **Új értesítés hubon létrehozása**.  

    ![][17]

4. Kattintson a névtér, amelyben a értesítés központi létrehozása, majd a **Értesítés hubok** tetején.

    ![][18]

5. Jelölje ki az Ön által létrehozott értesítés-központban, és kattintson a **beállítás** a felső menüből.

    ![][19]

6. Görgessen le a **baidu értesítési beállítások** szakaszban, és írja be az API-ja kulcs és a titkos kulcs szerezte be a Baidu konzolról korábban Baidu felhő leküldéses munkaütemezésre. Kattintson a **Mentés**gombra.

    ![][20]

7. Kattintson az **Irányítópult** lap tetején a az értesítési-központban, és kattintson a **Nézet kapcsolati karakterláncot**.

    ![][21]

8. Jegyezze le a **DefaultListenSharedAccessSignature** és **DefaultFullSharedAccessSignature** az **Access kapcsolatadatainak** ablakból.

    ![][22]

##<a name="connect-your-app-to-the-notification-hub"></a>Az alkalmazás csatlakoztatása az értesítési-központban

1. A Holdas ADT, Android új projekt létrehozása (**fájl** > **Új** > **Android alkalmazás projekt**).

    ![][23]

2. Írja be az **Alkalmazás nevét** , és ellenőrizze, hogy a **Minimálisan szükséges SDK** verzió beállítása **API-16: Android 4.1-es**.

    ![][24]

3. Kattintson a **Tovább** gombra, és továbbra is a varázsló következő, amíg meg nem jelenik a **Tevékenység létrehozása** ablakban. Győződjön meg arról, hogy **Üres tevékenység** be van jelölve, és végül válassza a **Befejezés gombra** kattintva hozzon létre egy új Android alkalmazást.

    ![][25]

4. Győződjön meg arról, hogy a **Projekt összeállítása cél** megfelelően van-e beállítva.

    ![][26]

5. A **fájlok** fülre, a [Értesítés-hubok-Android-SDK Bintray a](https://bintray.com/microsoftazuremobile/SDK/Notification-Hubs-Android-SDK/0.4)töltse le az értesítési-hubok-0.4.jar fájlt. Adja hozzá a fájlt a Holdas projekt **könyvtárral lenne** mappát, és frissítse a *könyvtárral lenne* mappát.

6. Töltse le és csomagolja ki az [Android SDK Baidu leküldéses], nyissa meg a **könyvtárral lenne** mappát, és másolja a **pushservice-x.y.z** üveg fájlt, és a **armeabi** & **mips** mappák **könyvtárral lenne** mappájában található az Android alkalmazást.

7. Nyissa meg a **AndroidManifest.xml** fájlt az Android projekt, és adja hozzá az Baidu SDK által szükséges engedélyekről.

        <uses-permission android:name="android.permission.INTERNET" />
        <uses-permission android:name="android.permission.READ_PHONE_STATE" />
        <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
        <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
        <uses-permission android:name="android.permission.WRITE_SETTINGS" />
        <uses-permission android:name="android.permission.VIBRATE" />
        <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
        <uses-permission android:name="android.permission.DISABLE_KEYGUARD" />
        <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
        <uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
        <uses-permission android:name="android.permission.ACCESS_DOWNLOAD_MANAGER" />
        <uses-permission android:name="android.permission.DOWNLOAD_WITHOUT_NOTIFICATION" />

8. Az **Android-alapú: name** tulajdonság hozzáadása **AndroidManifest.xml**, *yourprojectname* (például a **com.example.BaiduTest**) cseréje az **alkalmazás** elemet. Győződjön meg arról, hogy a projekt neve megegyezik az egyik a Baidu konzolon beállított.

        <application android:name="yourprojectname.DemoApplication"

9. Adja hozzá a **után az alábbi beállításokkal belül az alkalmazás elemet. MainActivity** tevékenység elem cseréje *yourprojectname* (például a **com.example.BaiduTest**):

        <receiver android:name="yourprojectname.MyPushMessageReceiver">
            <intent-filter>
                <action android:name="com.baidu.android.pushservice.action.MESSAGE" />
                <action android:name="com.baidu.android.pushservice.action.RECEIVE" />
                <action android:name="com.baidu.android.pushservice.action.notification.CLICK" />
            </intent-filter>
        </receiver>

        <receiver android:name="com.baidu.android.pushservice.PushServiceReceiver"
            android:process=":bdservice_v1">
            <intent-filter>
                <action android:name="android.intent.action.BOOT_COMPLETED" />
                <action android:name="android.net.conn.CONNECTIVITY_CHANGE" />
                <action android:name="com.baidu.android.pushservice.action.notification.SHOW" />
            </intent-filter>
        </receiver>

        <receiver android:name="com.baidu.android.pushservice.RegistrationReceiver"
            android:process=":bdservice_v1">
            <intent-filter>
                <action android:name="com.baidu.android.pushservice.action.METHOD" />
                <action android:name="com.baidu.android.pushservice.action.BIND_SYNC" />
            </intent-filter>
            <intent-filter>
                <action android:name="android.intent.action.PACKAGE_REMOVED"/>
                <data android:scheme="package" />
            </intent-filter>
        </receiver>

        <service
            android:name="com.baidu.android.pushservice.PushService"
            android:exported="true"
            android:process=":bdservice_v1"  >
            <intent-filter>
                <action android:name="com.baidu.android.pushservice.action.PUSH_SERVICE" />
            </intent-filter>
        </service>

9. Adja hozzá a projekthez a **ConfigurationSettings.java** nevű új osztály.

    ![][28]

    ![][29]

10. Hozzáadni a következő kódot:

        public class ConfigurationSettings {
                public static String API_KEY = "...";
                public static String NotificationHubName = "...";
                public static String NotificationHubConnectionString = "...";
            }

    A projektből Baidu felhő korábbi **NotificationHubName** az értesítési központi nevű az Azure klasszikus portálról és **NotificationHubConnectionString** DefaultListenSharedAccessSignature az Azure klasszikus portálról a beolvasott **API_KEY** értékének beállítása.

11. Adja hozzá a **DemoApplication.java**nevű új osztály, és a következő kód hozzáadása:

        import com.baidu.frontia.FrontiaApplication;

        public class DemoApplication extends FrontiaApplication {
            @Override
            public void onCreate() {
                super.onCreate();
            }
        }

12. Egy másik osztályú új néven **MyPushMessageReceiver.java**hozzáadása és alatt a kódot hozzáadni. Ez az a Baidu leküldéses kiszolgálótól kapott a leküldéses értesítéseket kezelő osztály.

        import java.util.List;
        import android.content.Context;
        import android.os.AsyncTask;
        import android.util.Log;
        import com.baidu.frontia.api.FrontiaPushMessageReceiver;
        import com.microsoft.windowsazure.messaging.NotificationHub;

        public class MyPushMessageReceiver extends FrontiaPushMessageReceiver {
            /** TAG to Log */
            public static NotificationHub hub = null;
            public static String mChannelId, mUserId;
            public static final String TAG = MyPushMessageReceiver.class
                    .getSimpleName();

            @Override
            public void onBind(Context context, int errorCode, String appid,
                    String userId, String channelId, String requestId) {
                String responseString = "onBind errorCode=" + errorCode + " appid="
                        + appid + " userId=" + userId + " channelId=" + channelId
                        + " requestId=" + requestId;
                Log.d(TAG, responseString);
                mChannelId = channelId;
                mUserId = userId;

                try {
                 if (hub == null) {
                        hub = new NotificationHub(
                                ConfigurationSettings.NotificationHubName,
                                ConfigurationSettings.NotificationHubConnectionString,
                                context);
                        Log.i(TAG, "Notification hub initialized");
                    }
                } catch (Exception e) {
                   Log.e(TAG, e.getMessage());
                }

                registerWithNotificationHubs();
            }

            private void registerWithNotificationHubs() {
               new AsyncTask<Void, Void, Void>() {
                  @Override
                  protected Void doInBackground(Void... params) {
                     try {
                         hub.registerBaidu(mUserId, mChannelId);
                         Log.i(TAG, "Registered with Notification Hub - '"
                                + ConfigurationSettings.NotificationHubName + "'"
                                + " with UserId - '"
                                + mUserId + "' and Channel Id - '"
                                + mChannelId + "'");
                     } catch (Exception e) {
                         Log.e(TAG, e.getMessage());
                     }
                     return null;
                 }
               }.execute(null, null, null);
            }

            @Override
            public void onSetTags(Context context, int errorCode,
                    List<String> sucessTags, List<String> failTags, String requestId) {
                String responseString = "onSetTags errorCode=" + errorCode
                        + " sucessTags=" + sucessTags + " failTags=" + failTags
                        + " requestId=" + requestId;
                Log.d(TAG, responseString);
            }

            @Override
            public void onDelTags(Context context, int errorCode,
                    List<String> sucessTags, List<String> failTags, String requestId) {
                String responseString = "onDelTags errorCode=" + errorCode
                        + " sucessTags=" + sucessTags + " failTags=" + failTags
                        + " requestId=" + requestId;
                Log.d(TAG, responseString);
            }

            @Override
            public void onListTags(Context context, int errorCode, List<String> tags,
                    String requestId) {
                String responseString = "onListTags errorCode=" + errorCode + " tags="
                        + tags;
                Log.d(TAG, responseString);
            }

            @Override
            public void onUnbind(Context context, int errorCode, String requestId) {
                String responseString = "onUnbind errorCode=" + errorCode
                        + " requestId = " + requestId;
                Log.d(TAG, responseString);
            }

            @Override
            public void onNotificationClicked(Context context, String title,
                    String description, String customContentString) {
                String notifyString = "title=\"" + title + "\" description=\""
                        + description + "\" customContent=" + customContentString;
                Log.d(TAG, notifyString);
            }

            @Override
            public void onMessage(Context context, String message,
                    String customContentString) {
                String messageString = "message=\"" + message + "\" customContentString=" + customContentString;
                Log.d(TAG, messageString);
            }
        }

13. Nyissa meg a **MainActivity.java**, és adja hozzá a **onCreate** módszer a következőket:

            PushManager.startWork(getApplicationContext(),
                    PushConstants.LOGIN_TYPE_API_KEY, ConfigurationSettings.API_KEY);

14. Nyissa meg a következő importálása kimutatások tetején:

            import com.baidu.android.pushservice.PushConstants;
            import com.baidu.android.pushservice.PushManager;

##<a name="send-notifications-to-your-app"></a>Értesítés küldése az alkalmazás


Lehetősége van gyorsan tesztelje az alkalmazás értesítésekkel értesítést küld a az [Azure portál](https://portal.azure.com/) használata a **Küldés tesztelése** gombra az értesítési-központban az alábbi képernyőképen látható módon.

![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-test-send-wns.png)

Leküldéses értesítések általában küldjük a háttéradatbázist szolgáltatásba, például a mobil szolgáltatások vagy ASP.NET-kompatibilis tár használatával. A REST API közvetlenül az értesítési küldésére, ha egy tárban nem érhető el a háttéradatbázist is használhatja.

Ebben az oktatóanyagban azt fogja legyen egyszerű, és csak bemutatják, az ügyfél-alkalmazás teszteli a .NET SDK használata helyett kódmentes szolgáltatás konzol alkalmazásban értesítés hubok értesítést küld. A következő lépés az értesítéseket küldeni egy ASP.NET kódmentes javasoljuk az [Használata értesítés hubok leküldéses értesítéseket, hogy a felhasználók az](notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md) oktatóprogram. Az alábbi módszerekkel azonban küldött értesítések használható:

* **Többi Interface**: bármely, a [többi felület](http://msdn.microsoft.com/library/windowsazure/dn223264.aspx)kódmentes platformon támogatja az új értesítés.

* **Microsoft Azure értesítés hubok .NET SDK**: A a Nuget csomag Manager for Visual Studio, futtassa a [Telepítés-csomag Microsoft.Azure.NotificationHubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).

* **Node.js** : [Node.js az értesítési hubok használatáról](notification-hubs-nodejs-push-notification-tutorial.md).

* **Mobile Appjai**:, hogyan küldhet értesítést a az Azure alkalmazás szolgáltatás Mobile-alkalmazások kódmentes értesítés hubok integrálva van, például egy [értesítés](../app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-push.md)jelenik meg, Hozzáadás leküldéses a mobil alkalmazásba.

* **Java / PHP**: egy példa, hogyan küldhet értesítést a REST API-k használatával című témakörben "Hogyan használhatja az értesítési hubok Java/PHP a" ([Java](notification-hubs-java-push-notification-tutorial.md) | [PHP](notification-hubs-php-push-notification-tutorial.md)).

##<a name="optional-send-notifications-from-a-net-console-app"></a>(Nem kötelező) Értesítés küldése .NET konzol alkalmazásból.

Ez a szakasz bemutatjuk .NET konzol alkalmazással értesítést küld.

1. Új képi C# console-alkalmazás létrehozása:

    ![][30]

2. Csomag kezelője konzol ablakában a **projekt alapértelmezett** beállítása a konzol alkalmazás új projekt, és majd konzol ablakában végre az alábbi parancsot:

        Install-Package Microsoft.Azure.NotificationHubs

    Ez a lehetőség a <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification hubok NuGet csomag</a>használata Azure-értesítés hubok SDK mutató hivatkozás.

    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-package-manager.png)

3. Nyissa meg a fájlt **Program.cs** , és adja hozzá a következő utasítással:

        using Microsoft.Azure.NotificationHubs;

4. Az a `Program` osztály, adja hozzá a következő módszerrel és *DefaultFullSharedAccessSignatureSASConnectionString* és *NotificationHubName* cserélje ki az értékeket, ha rendelkezik.

        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("DefaultFullSharedAccessSignatureSASConnectionString", "NotificationHubName");
            string message = "{\"title\":\"((Notification title))\",\"description\":\"Hello from Azure\"}";
            var result = await hub.SendBaiduNativeNotificationAsync(message);
        }

5. Adja meg a következő sort a **fő** mód:

         SendNotificationAsync();
         Console.ReadLine();

##<a name="test-your-app"></a>Az alkalmazás tesztelése

Ha tesztelni szeretné az alkalmazás-tényleges telefonszámmal, csak csatlakoztassa a telefont a számítógéphez egy USB-kábel használatával. Az alkalmazás, a csatolt telefon alakzatot betöltése.

Az alkalmazás és a irányító eszköztárán Holdas felső ellenőrzéséhez kattintson a **Futtatás**parancsra, és válassza az alkalmazás. Ez elindítja a irányító, és betölt, és az alkalmazás elindul.

Az alkalmazás a "felhasználói azonosítót" és "channelId" a Baidu leküldéses értesítéseket kezelő szolgáltatása és az regisztrálja az értesítési-központban.

Teszt értesítés küldése a hibakeresési lapján az Azure klasszikus portálon is használhatja. Ha készített a Visual Studio .NET konzol alkalmazást, csak nyomja le az F5 billentyűt Visual Studio az alkalmazásnak a futtatására. Az alkalmazás az eszközön vagy irányító felső értesítési területén megjelenő értesítést küld.


<!-- Images. -->
[1]: ./media/notification-hubs-baidu-get-started/BaiduRegistration.png
[2]: ./media/notification-hubs-baidu-get-started/BaiduRegistrationInput.png
[3]: ./media/notification-hubs-baidu-get-started/BaiduConfirmation.png
[4]: ./media/notification-hubs-baidu-get-started/BaiduActivationEmail.png
[5]: ./media/notification-hubs-baidu-get-started/BaiduRegistrationMore.png
[6]: ./media/notification-hubs-baidu-get-started/BaiduOpenCloudPlatform.png
[7]: ./media/notification-hubs-baidu-get-started/BaiduDeveloperServices.png
[8]: ./media/notification-hubs-baidu-get-started/BaiduDeveloperRegistration.png
[9]: ./media/notification-hubs-baidu-get-started/BaiduDevRegistrationInput.png
[10]: ./media/notification-hubs-baidu-get-started/BaiduDevRegistrationConfirmation.png
[11]: ./media/notification-hubs-baidu-get-started/BaiduDevConfirmationFinal.png
[12]: ./media/notification-hubs-baidu-get-started/BaiduCloudPush.png
[13]: ./media/notification-hubs-baidu-get-started/BaiduDevSvcMgmt.png
[14]: ./media/notification-hubs-baidu-get-started/BaiduCreateProject.png
[15]: ./media/notification-hubs-baidu-get-started/BaiduCreateProjectInput.png
[16]: ./media/notification-hubs-baidu-get-started/BaiduProjectKeys.png
[17]: ./media/notification-hubs-baidu-get-started/AzureNHCreation.png
[18]: ./media/notification-hubs-baidu-get-started/NotificationHubs.png
[19]: ./media/notification-hubs-baidu-get-started/NotificationHubsConfigure.png
[20]: ./media/notification-hubs-baidu-get-started/NotificationHubBaiduConfigure.png
[21]: ./media/notification-hubs-baidu-get-started/NotificationHubsConnectionStringView.png
[22]: ./media/notification-hubs-baidu-get-started/NotificationHubsConnectionString.png
[23]: ./media/notification-hubs-baidu-get-started/EclipseNewProject.png
[24]: ./media/notification-hubs-baidu-get-started/EclipseProjectCreation.png
[25]: ./media/notification-hubs-baidu-get-started/EclipseBlankActivity.png
[26]: ./media/notification-hubs-baidu-get-started/EclipseProjectBuildProperty.png
[27]: ./media/notification-hubs-baidu-get-started/EclipseBaiduReferences.png
[28]: ./media/notification-hubs-baidu-get-started/EclipseNewClass.png
[29]: ./media/notification-hubs-baidu-get-started/EclipseConfigSettingsClass.png
[30]: ./media/notification-hubs-baidu-get-started/ConsoleProject.png
[31]: ./media/notification-hubs-baidu-get-started/BaiduPushConfig1.png
[32]: ./media/notification-hubs-baidu-get-started/BaiduPushConfig2.png
[33]: ./media/notification-hubs-baidu-get-started/BaiduPushConfig3.png

<!-- URLs. -->
[Android SDK mobil szolgáltatások]: https://go.microsoft.com/fwLink/?LinkID=280126&clcid=0x409
[Android SDK baidu leküldéses]: http://developer.baidu.com/wiki/index.php?title=docs/cplat/push/sdk/clientsdk
[Azure klasszikus portál]: https://manage.windowsazure.com/
[Baidu portál]: http://www.baidu.com/
