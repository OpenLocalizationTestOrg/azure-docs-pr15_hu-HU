<properties
    pageTitle="Első lépések az Android alkalmazások Azure Mobile tetszés szerint elmélyedhet"
    description="Megtudhatja, hogy miként analitikai és a leküldéses értesítések Azure Mobile tetszés szerint elmélyedhet használata Android-alkalmazással."
    services="mobile-engagement"
    documentationCenter="android"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="Java"
    ms.topic="hero-article"
    ms.date="08/10/2016"
    ms.author="piyushjo;ricksal" />

# <a name="get-started-with-azure-mobile-engagement-for-android-apps"></a>Azure Mobile tetszés szerint elmélyedhet Android alkalmazások használatának első lépései

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

Ez a témakör bemutatja az Azure Mobile tetszés szerint elmélyedhet használatáról az alkalmazás használatát megértéséhez és a leküldéses értesítések üzenetküldés szegmentált felhasználói egy Android alkalmazást.
Ebben az oktatóprogramban az egyszerű közvetítési forgatókönyv Mobile tetszés szerint elmélyedhet használatával mutatja be. Hoz létre, amely egyszerű adatait gyűjti össze, és a Google Cloud üzenetküldés (GCM) használata a leküldéses értesítések fogadása üres Android alkalmazást.

## <a name="prerequisites"></a>Előfeltételek

Ebben az oktatóanyagban befejezése van szükség az [Android Fejlesztőeszközök](https://developer.android.com/sdk/index.html), amelyek tartalmazzák még a Android Studio integrált fejlesztői környezet és a legújabb Android platform.

A [Mobil tetszés szerint elmélyedhet Android SDK](https://aka.ms/vq9mfn)is igényel.

> [AZURE.IMPORTANT] Ebben az oktatóanyagban befejezéséhez aktív Azure-fiók szükséges. Nem rendelkeznek fiókkal, ha mindössze néhány perc is létrehozhat ingyenes próba-fiók. A részletekért lásd: [Azure ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-android-get-started).

## <a name="set-up-mobile-engagement-for-your-android-app"></a>Az Android alkalmazás Mobile tetszés szerint elmélyedhet beállítása

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <a name="connect-your-app-to-the-mobile-engagement-backend"></a>Az alkalmazás csatlakoztatása a Mobile tetszés szerint elmélyedhet kódmentes

Ebben az oktatóprogramban egy "egyszerű integráció", amely a minimálisan szükséges adatok összegyűjtése és küldje el a leküldéses értesítést jeleníti meg. Android Studio keresztül mutatjuk integrációja hoz létre egy egyszerű alkalmazást.

A teljes integrációs dokumentációt a [Mobile tetszés szerint elmélyedhet Android SDK integrációs](mobile-engagement-android-sdk-overview.md)találhatók.

### <a name="create-an-android-project"></a>Android projekt létrehozása

1. Indítsa el **Az Android Studio**, és az előugró ablakban, jelölje be a **Android Studio új projekt indítása**.

    ![][1]

2. Adja meg az alkalmazás nevét és a vállalat tartományhoz. Jegyezze le mi kitöltés, mivel később szüksége. Kattintson a **Tovább**gombra.

    ![][2]

3. Jelölje be a cél helyigény és az API szintű, és kattintson a **Tovább**gombra.

    >[AZURE.NOTE] Mobil tetszés szerint elmélyedhet szükséges API szintű legalább 10 (Android 2.3.3).

    ![][3]

4. Jelölje ki az **Üres tevékenység** ebben az esetben ez az alkalmazás csak a képernyőn, és kattintson a **Tovább**gombra.

    ![][4]

5. Végezetül hagyja az alapértelmezett beállításokat, és kattintson a **Befejezés gombra**.

    ![][5]

Android Studio most hoz létre, a bemutató alkalmazást, amelyikbe azt integráció Mobile részvételét.

### <a name="include-the-sdk-library-in-your-project"></a>A SDK tár tartalmazza a projektben

1. Töltse le a [mobil tetszés szerint elmélyedhet Android SDK csomagjában talál](https://aka.ms/vq9mfn).
2. Bontsa ki az archív mappák mappába a számítógépen.
3. Az aktuális verziójának a SDK .jar tárat azonosítása, és másolja a vágólapra.

      ![][6]

4. Nyissa meg a **projekthez** szakaszt (1), és illessze be a .jar a könyvtárral lenne mappába (2).

      ![][7]

5. A tár betöltéséhez, a projekt szinkronizálása.

      ![][8]

### <a name="connect-your-app-to-mobile-engagement-backend-with-the-connection-string"></a>Az alkalmazás csatlakoztatása a kapcsolattal Mobile tetszés szerint elmélyedhet kódmentes

1. A következő kódsorokat másolja a tevékenység létrehozása (kell elvégezni csak az alkalmazás, a fő tevékenység általában egy helyen). A minta alkalmazás meg a MainActivity csoportban a forrás-fő > -> java mappát, és adja hozzá a következő:

        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
        EngagementAgent.getInstance(this).init(engagementConfiguration);

2. A hivatkozások megoldható nyomja le az Alt + Enter vagy a következő importálása kimutatások hozzáadása:

        import com.microsoft.azure.engagement.EngagementAgent;
        import com.microsoft.azure.engagement.EngagementConfiguration;

3. Térjen vissza az alkalmazást **a kapcsolat adatai** lapon az Azure klasszikus portált, és másolja a vágólapra a **Kapcsolati karakterlánc**.

      ![][9]

4. Illessze be a `setConnectionString` paraméter, a teljes szöveget a következő kódot látható cseréjével kapcsolatban:

        engagementConfiguration.setConnectionString("Endpoint=my-company-name.device.mobileengagement.windows.net;SdkKey=********************;AppId=*********");

### <a name="add-permissions-and-a-service-declaration"></a>Engedélyek és szolgáltatás nyilatkozatot hozzáadása

1. Az engedélyek hozzáadása a projekt a Manifest.xml azonnal előtti vagy utáni a `<application>` címke:

        <uses-permission android:name="android.permission.INTERNET"/>
        <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
        <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
        <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
        <uses-permission android:name="android.permission.VIBRATE" />
        <uses-permission android:name="android.permission.DOWNLOAD_WITHOUT_NOTIFICATION"/>

2. A ügynök szolgáltatás deklarálása, közötti kód hozzáadása a `<application>` és `</application>` címkék:

        <service
            android:name="com.microsoft.azure.engagement.service.EngagementService"
            android:exported="false"
            android:label="<Your application name>"
            android:process=":Engagement"/>

3. A beillesztett kódban cseréje `"<Your application name>"` a címke, amely jelenik meg a **Beállítások** menüben, ahol megtekintheti az eszközön futó szolgáltatások. Adott címke például felveheti a "Szolgáltatás" szót.

### <a name="send-a-screen-to-mobile-engagement"></a>A képernyő Mobile tetszés szerint elmélyedhet küldése

Indítsa el az adatok küldése, és győződjön meg arról, hogy a felhasználók aktívak, legalább egy képernyővel (a tevékenység) kell küldenie a Mobile tetszés szerint elmélyedhet kódmentes.

Nyissa meg a **MainActivity.java** , és adja hozzá a következő **MainActivity** **EngagementActivity**az alap osztálya cseréje:

    public class MainActivity extends EngagementActivity {

> [AZURE.NOTE] Ha az alap osztályához nem *tevékenységet*, olvassa el az [Android jelentéskészítés speciális](mobile-engagement-android-advanced-reporting.md#modifying-your-codeactivitycode-classes) , hogy hogyan különböző osztályok örökli.


Megjegyzések hozzáfűzése a következő sort a minta egyszerű forgatókönyv meg:

    // setSupportActionBar(toolbar);

Ha meg szeretné őrizni a `ActionBar` az alkalmazást, olvassa el [Az Android jelentéskészítés speciális](mobile-engagement-android-advanced-reporting.md#modifying-your-codeactivitycode-classes).

## <a name="connect-app-with-real-time-monitoring"></a>Alkalmazás kapcsolatba valós idejű ellenőrzése

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <a name="enable-push-notifications-and-in-app-messaging"></a>Leküldéses értesítések és az alkalmazás üzenetküldés engedélyezése

Akció során Mobile tetszés szerint elmélyedhet segítségével együttműködhet, és a felhasználók a leküldéses értesítéseket, és az alkalmazás üzenetküldés is ELÉRHETI. Ez a modul neve vannak a Mobile tetszés szerint elmélyedhet portálon.
A következő szakasz beállítja az alkalmazás fogadásához őket.

### <a name="copy-sdk-resources-in-your-project"></a>Másolás SDK az erőforrásokhoz

1. Lépjen vissza a SDK tartalom letöltése, és másolja a **felbontás** mappát.

    ![][10]

2. Lépjen vissza az Android Studio, jelölje be a **fő** könyvtár a project-fájlokat, és illessze be az erőforrások hozzáadása a projekthez.

    ![][11]

[AZURE.INCLUDE [Enable Google Cloud Messaging](../../includes/mobile-engagement-enable-google-cloud-messaging.md)]

[AZURE.INCLUDE [Enable in-app messaging](../../includes/mobile-engagement-android-send-push.md)]

[AZURE.INCLUDE [Send notification from portal](../../includes/mobile-engagement-android-send-push-from-portal.md)]

## <a name="next-steps"></a>Következő lépések

Nyissa meg az [Android SDK csomagjában talál](mobile-engagement-android-sdk-overview.md) részletes ismereteket SDK integrációja eléréséhez.

<!-- Images. -->
[1]: ./media/mobile-engagement-android-get-started/android-studio-new-project.png
[2]: ./media/mobile-engagement-android-get-started/android-studio-project-props.png
[3]: ./media/mobile-engagement-android-get-started/android-studio-project-props2.png
[4]: ./media/mobile-engagement-android-get-started/android-studio-add-activity.png
[5]: ./media/mobile-engagement-android-get-started/android-studio-activity-name.png
[6]: ./media/mobile-engagement-android-get-started/sdk-content.png
[7]: ./media/mobile-engagement-android-get-started/paste-jar.png
[8]: ./media/mobile-engagement-android-get-started/sync-project.png
[9]: ./media/mobile-engagement-android-get-started/app-connection-info-page.png
[10]: ./media/mobile-engagement-android-get-started/copy-resources.png
[11]: ./media/mobile-engagement-android-get-started/paste-resources.png
