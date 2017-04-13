<properties
    pageTitle="Azure Mobile tetszés szerint elmélyedhet egységét Android telepítési használatának első lépései"
    description="Megtudhatja, hogy miként analitikai és a leküldéses értesítések Azure Mobile tetszés szerint elmélyedhet használata egységét alkalmazás telepítése iOS-eszközök."
    services="mobile-engagement"
    documentationCenter="unity"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-unity-android"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="08/19/2016"
    ms.author="piyushjo" />

# <a name="get-started-with-azure-mobile-engagement-for-unity-android-deployment"></a>Azure Mobile tetszés szerint elmélyedhet egységét Android telepítési használatának első lépései

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

Ez a témakör bemutatja, hogyan Azure Mobile tetszés szerint elmélyedhet használja az alkalmazás használatát megértéséhez, és küldje el a leküldéses értesítéseket, hogy miként szegmentált felhasználók egységét alkalmazás az Android-eszközökre való telepítésekor.
Ebben az oktatóanyagban használja a klasszikus egységét Vetítődnek megtalálhatja a kos oktatóanyagot kezdőpontjának. Kövesse a lépéseket, ebben az [oktatóanyagban](mobile-engagement-unity-roll-a-ball.md) azt az oktatóprogram során dinamikus Mobile tetszés szerint elmélyedhet-integrációval a folytatás előtt. 

Ebben az oktatóprogramban az alábbiakra van szükség:

+ [Egységét szerkesztő](http://unity3d.com/get-unity)
+ [Mobil tetszés szerint elmélyedhet egységét SDK](https://aka.ms/azmeunitysdk)
+ A Google Android SDK

> [AZURE.NOTE] Ebben az oktatóanyagban befejezéséhez, az aktív Azure fiókkal kell rendelkezniük. Nem rendelkeznek fiókkal, ha mindössze néhány perc is létrehozhat ingyenes próba-fiók. A részletekért lásd: [Azure ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-unity-android-get-started).

##<a id="setup-azme"></a>A telepítő mobil tetszés szerint Elmélyedhet az Android számára

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

##<a id="connecting-app"></a>Az alkalmazás csatlakoztatása a Mobile tetszés szerint elmélyedhet kódmentes

###<a name="import-the-unity-package"></a>Importálás a egységét csomag

1. A [mobil tetszés szerint elmélyedhet egységét csomag](https://aka.ms/azmeunitysdk) letöltése, és mentse a helyi számítógépre. 

2. Nyissa meg a **csomag importálása-eszközök > egyéni csomag ->** , és válassza ki a letöltött csomag a fenti lépést. 

    ![][70] 

3. Győződjön meg arról, hogy az összes fájl legyen kijelölve, és kattintson az **Importálás** gombra. 

    ![][71] 

4. Miután importálása sikeres, megjelenik az importált SDK fájlok a projektben.  

    ![][72] 

###<a name="update-the-engagementconfiguration"></a>A EngagementConfiguration frissítése

1. Nyisson meg a **EngagementConfiguration** parancsfájl a SDK mappát, és a frissítést a **ANDROID\_kapcsolat\_karakterlánc** a kapcsolattal az Azure portálról korábban kapott.  

    ![][73]

2. Mentse a fájlt 

3. Végrehajtás **tetszés szerint elmélyedhet-fájl > -> Android jegyzék készítése**. Ez a beépülő modul a Mobile tetszés szerint elmélyedhet SDK hozzá, és rákattintva automatikus frissítése a project beállításai. 

    ![][74]

> [AZURE.IMPORTANT] Ellenőrizze, hogy ez végrehajtani, minden alkalommal, amikor frissíti a **EngagementConfiguration** fájl különben a program a módosítások nem jelennek meg az alkalmazást. 

###<a name="configure-the-app-for-basic-tracking"></a>Állítsa be az alkalmazás egyszerű nyomon követés céljából

1. Nyisson meg szerkesztésre a lejátszó objektumhoz **PlayerController** parancsfájl. 

2. Adja hozzá a következő utasítással:

        using Microsoft.Azure.Engagement.Unity;

3. Az alábbi módon adja hozzá a `Start()` módszer
    
        EngagementAgent.Initialize();
        EngagementAgent.StartActivity("Home");

###<a name="deploy-and-run-the-app"></a>Futtassa az alkalmazást, és terjesztése
Győződjön meg arról, hogy rendelkezik-e telepítve a számítógépre, mielőtt megnyitná a egységét alkalmazások terjesztése az eszközre az Android SDK csomagjában talál. 

1. Az Android-eszközökre kapcsolódni a számítógépen. 

2. **Fájl -> Szerkesztés Beállítások** megnyitása 

    ![][40]

3. Jelölje be az **Android** , és válassza a **Váltás** a Platform

    ![][51]

    ![][52]

4. Kattintson a **lejátszó beállítások** elemre, és adja meg az érvénytelen az első lépésekhez azonosító. 

    ![][53]

5. Végül kattintson a **Szerkesztés és futtatása**

    ![][54]

6. A mappa nevét tárolja az Android-csomag megadására kérheti. 

7. Ha mindent rendben, a csomag telepítve lesz a csatlakoztatott eszközre, és meg kell jelennie a egységét játék telefonon! 

##<a id="monitor"></a>Alkalmazás kapcsolatba valós idejű ellenőrzése

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a id="integrate-push"></a>Leküldéses értesítések és az alkalmazás üzenetküldés engedélyezése

[AZURE.INCLUDE [Enable Google Cloud Messaging](../../includes/mobile-engagement-enable-google-cloud-messaging.md)]

###<a name="update-the-engagementconfiguration"></a>A EngagementConfiguration frissítése

1. Nyisson meg a **EngagementConfiguration** parancsfájl a SDK mappát, és a frissítést a **ANDROID\_GOOGLE\_szám** és a Google Cloud Developer Portal segítségével a korábban kapott a **Google projektszámot** . Ez a karakterlánc értéket, ellenőrizze, hogy idézőjelek közé kell tenni, dupla idézőjelek közé. 

    ![][75]

2. Mentse a fájlt. 

3. Végrehajtás **tetszés szerint elmélyedhet-fájl > -> Android jegyzék készítése**. Ez a beépülő modul a Mobile tetszés szerint elmélyedhet SDK hozzá, és rákattintva automatikus frissítése a project beállításai. 

    ![][74]

###<a name="configure-the-app-to-receive-notifications"></a>Ha értesítést szeretne kapni az alkalmazás beállítása

1. Nyisson meg szerkesztésre a lejátszó objektumhoz **PlayerController** parancsfájl. 

2. Az alábbi módon adja hozzá a `Start()` módszer

        EngagementReachAgent.Initialize();

3. Most, hogy az alkalmazás frissül, telepítése, és futtassa az alkalmazást, az alábbi utasításokat egy eszközön. 

[AZURE.INCLUDE [Send notification from portal](../../includes/mobile-engagement-android-send-push-from-portal.md)]

<!-- Images -->
[40]: ./media/mobile-engagement-unity-android-get-started/40.png
[70]: ./media/mobile-engagement-unity-android-get-started/70.png
[71]: ./media/mobile-engagement-unity-android-get-started/71.png
[72]: ./media/mobile-engagement-unity-android-get-started/72.png
[73]: ./media/mobile-engagement-unity-android-get-started/73.png
[74]: ./media/mobile-engagement-unity-android-get-started/74.png
[75]: ./media/mobile-engagement-unity-android-get-started/75.png
[51]: ./media/mobile-engagement-unity-android-get-started/51.png
[52]: ./media/mobile-engagement-unity-android-get-started/52.png
[53]: ./media/mobile-engagement-unity-android-get-started/53.png
[54]: ./media/mobile-engagement-unity-android-get-started/54.png
