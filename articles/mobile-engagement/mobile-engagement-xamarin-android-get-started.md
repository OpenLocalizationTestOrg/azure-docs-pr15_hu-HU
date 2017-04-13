<properties
    pageTitle="Azure mobil részvételét Xamarin.Android az első lépések"
    description="Megtudhatja, hogy miként analitikai és a leküldéses értesítések Azure Mobile tetszés szerint elmélyedhet használata Xamarin.Android alkalmazások."
    services="mobile-engagement"
    documentationCenter="xamarin"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-android"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="06/16/2016"
    ms.author="piyushjo" />

# <a name="get-started-with-azure-mobile-engagement-for-xamarinandroid-apps"></a>Azure mobil részvételét-Xamarin.Android alkalmazások – első lépések

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

Ez a témakör bemutatja az Azure Mobile tetszés szerint elmélyedhet használatáról az alkalmazás használatát megértéséhez és a leküldéses értesítések Xamarin.Android alkalmazás szegmentált felhasználóknak elküldendő.
Ebben az oktatóprogramban az egyszerű közvetítési forgatókönyv Mobile tetszés szerint elmélyedhet használatával mutatja be. Hozzon létre egy üres Xamarin.Android alkalmazás által gyűjtött adatokat, és a Google Cloud üzenetküldés (GCM) használata a leküldéses értesítések fogadása.

Ebben az oktatóprogramban az alábbiakra van szükség:

+ [Xamarin Studio](http://xamarin.com/studio). Visual Studio az Xamarin is használhatja, de ebben az oktatóanyagban Xamarin Studio használja. Telepítési című cikkben olvashat [a telepítő, és telepítse az Visual Studio és Xamarin](https://msdn.microsoft.com/library/mt613162.aspx).
+ [Mobil tetszés szerint elmélyedhet Xamarin SDK](https://www.nuget.org/packages/Microsoft.Azure.Engagement.Xamarin/)

> [AZURE.NOTE] Ebben az oktatóanyagban befejezéséhez, az aktív Azure fiókkal kell rendelkezniük. Nem rendelkeznek fiókkal, ha mindössze néhány perc is létrehozhat ingyenes próba-fiók. A részletekért lásd: [Azure ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-xamarin-android-get-started).

##<a id="setup-azme"></a>A telepítő mobil tetszés szerint Elmélyedhet az Android számára

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

##<a id="connecting-app"></a>Az alkalmazás csatlakoztatása a Mobile tetszés szerint elmélyedhet kódmentes

Ebben az oktatóprogramban egy "egyszerű integráció", amely a minimálisan szükséges adatok összegyűjtése és küldje el a leküldéses értesítést jeleníti meg. 

Egy egyszerű app integrációja keresztül mutatjuk Xamarin Studio azt hoz létre.

###<a name="create-a-new-xamarinandroid-project"></a>Xamarin.Android új projekt létrehozása

1. Kattintson a Launch **Xamarin Studio** **fájl** -> **Új** -> **megoldás** 

    ![][1]

2. Jelölje ki **Az Android alkalmazást** , majd győződjön meg arról, hogy a kijelölt nyelven **C#** , és kattintson a **Tovább**gombra.

    ![][2]

3. Írja be az **Alkalmazás neve** és a **szervezet azonosítója**. Győződjön meg arról, hogy be van jelölve **A Google Play szolgáltatások** , és kattintson a **Tovább**gombra. 

    ![][3]
    
4. Szükség esetén frissítse az a **Projekt nevét**, **Megoldás nevét** és **helyét** , és kattintson a **Létrehozás**gombra.

    ![][4]
 
Xamarin Studio hoz létre az alkalmazást, amelyben Mobile tetszés szerint elmélyedhet integrálja azt. 

###<a name="connect-your-app-to-mobile-engagement-backend"></a>Az alkalmazás csatlakoztatása Mobile tetszés szerint elmélyedhet kódmentes

1. Kattintson a jobb gombbal a megoldás windows- **csomagok** mappa, és válassza a **Csomagok hozzáadása**

    ![][5]

2. A **Microsoft Azure Mobile tetszés szerint elmélyedhet Xamarin SDK** kereshet, és vegye fel a megoldás.  

    ![][6]
   
3. Nyissa meg a **MainActivity.cs** , és adja hozzá a következő utasítások segítségével:

        using Microsoft.Azure.Engagement;
        using Microsoft.Azure.Engagement.Activity;

4. Az a `OnCreate` módot, adja hozzá a következő inicializálni Mobile tetszés szerint elmélyedhet kódmentes a kapcsolatot. Ellenőrizze, hogy a **ConnectionString**hozzáadni. 

        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.ConnectionString = "YourConnectionStringFromAzurePortal";
        EngagementAgent.Init(engagementConfiguration);

###<a name="add-permissions-and-a-service-declaration"></a>Engedélyek és szolgáltatás nyilatkozatot hozzáadása

1. Nyissa meg a Tulajdonságok mappán **Manifest.xml** másolatot. Jelölje be a forrás lapon, hogy közvetlenül az XML-forrás frissíteni.
 
2. Az engedélyek hozzáadása (amely megtalálható a **Tulajdonságok** mappán) a Manifest.xml a projekt azonnal előtti vagy utáni a `<application>` címke:

        <uses-permission android:name="android.permission.INTERNET"/>
        <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
        <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
        <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
        <uses-permission android:name="android.permission.VIBRATE" />
        <uses-permission android:name="android.permission.DOWNLOAD_WITHOUT_NOTIFICATION"/>

3. Adja hozzá a következő között a `<application>` és `</application>` címkék deklarálása a ügynök szolgáltatás:

        <service
            android:name="com.microsoft.azure.engagement.service.EngagementService"
            android:exported="false"
            android:label="<Your application name>"
            android:process=":Engagement"/>

4. Csak beillesztett kódban cseréje `"<Your application name>"` a címke. Ezzel jelenik meg a **Beállítások** menüben, ahol a felhasználók megtekinthetik a szolgáltatások fut az eszközön. Adott címke például felveheti a "Szolgáltatás" szót.

###<a name="send-a-screen-to-mobile-engagement"></a>A képernyő Mobile tetszés szerint elmélyedhet küldése

Annak érdekében, hogy adatokat küldi és annak biztosítására, a felhasználók aktív elindításához legalább egy képernyővel kell küldenie a Mobile tetszés szerint elmélyedhet kódmentes. Mindezt-győződjön meg arról, hogy a `MainActivity` örökli `EngagementActivity` helyett `Activity`.

    public class MainActivity : EngagementActivity
    
Azt is megteheti Ha nem örökölhet `EngagementActivity` majd hozzá kell adnia `.StartActivity` és `.EndActivity` módszerek `OnResume` és `OnPause` rendre.  

        protected override void OnResume()
            {
                EngagementAgent.StartActivity(EngagementAgentUtils.BuildEngagementActivityName(Java.Lang.Class.FromType(this.GetType())), null);
                base.OnResume();             
            }
    
            protected override void OnPause()
            {
                EngagementAgent.EndActivity();
                base.OnPause();            
            }

##<a id="monitor"></a>Alkalmazás kapcsolatba valós idejű ellenőrzése

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a id="integrate-push"></a>Leküldéses értesítések és az alkalmazás üzenetküldés engedélyezése

Mobil tetszés szerint elmélyedhet lehetővé teszi másokkal, és a felhasználók a leküldéses értesítések és az-app kampányok környezetében üzenetküldés is ELÉRHETI. Ez a modul neve vannak a Mobile tetszés szerint elmélyedhet portálon.
Az alábbi szakaszok is kapni az alkalmazás beállítása.

[AZURE.INCLUDE [Enable Google Cloud Messaging](../../includes/mobile-engagement-enable-google-cloud-messaging.md)]

[AZURE.INCLUDE [Enable in-app messaging](../../includes/mobile-engagement-android-send-push.md)]

[AZURE.INCLUDE [Send notification from portal](../../includes/mobile-engagement-android-send-push-from-portal.md)]

<!-- Images -->
[1]: ./media/mobile-engagement-xamarin-android-get-started/1.png
[2]: ./media/mobile-engagement-xamarin-android-get-started/2.png
[3]: ./media/mobile-engagement-xamarin-android-get-started/3.png
[4]: ./media/mobile-engagement-xamarin-android-get-started/4.png
[5]: ./media/mobile-engagement-xamarin-android-get-started/5.png
[6]: ./media/mobile-engagement-xamarin-android-get-started/6.png
