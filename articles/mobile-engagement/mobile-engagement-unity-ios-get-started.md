<properties
    pageTitle="Azure Mobile tetszés szerint elmélyedhet egységét iOS telepítési használatának első lépései"
    description="Megtudhatja, hogy miként analitikai és a leküldéses értesítések Azure Mobile tetszés szerint elmélyedhet használata egységét alkalmazás telepítése iOS-eszközök."
    services="mobile-engagement"
    documentationCenter="unity"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-unity-ios"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="08/19/2016"
    ms.author="piyushjo" />

# <a name="get-started-with-azure-mobile-engagement-for-unity-ios-deployment"></a>Azure Mobile tetszés szerint elmélyedhet egységét iOS telepítési használatának első lépései

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

Ez a témakör bemutatja, hogyan Azure Mobile tetszés szerint elmélyedhet használja az alkalmazás használatát megértéséhez, és küldje el a leküldéses értesítéseket, hogy miként szegmentált felhasználók egységét alkalmazás iOS-eszközökön való telepítésekor.
Ebben az oktatóanyagban használja a klasszikus egységét Vetítődnek megtalálhatja a kos oktatóanyagot kezdőpontjának. Kövesse a lépéseket, ebben az [oktatóanyagban](mobile-engagement-unity-roll-a-ball.md) azt az oktatóprogram során dinamikus Mobile tetszés szerint elmélyedhet-integrációval a folytatás előtt. 

Ebben az oktatóprogramban az alábbiakra van szükség:

+ [Egységét szerkesztő](http://unity3d.com/get-unity)
+ [Mobil tetszés szerint elmélyedhet egységét SDK](https://aka.ms/azmeunitysdk)
+ XCode szerkesztő

> [AZURE.NOTE] Ebben az oktatóanyagban befejezéséhez, az aktív Azure fiókkal kell rendelkezniük. Nem rendelkeznek fiókkal, ha mindössze néhány perc is létrehozhat ingyenes próba-fiók. A részletekért lásd: [Azure ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-unity-ios-get-started).

##<a id="setup-azme"></a>Mobil tetszés szerint elmélyedhet beállítása az iOS-alkalmazás

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

1. Nyisson meg a **EngagementConfiguration** parancsfájl a SDK mappát, és a frissítést a **IOS\_kapcsolat\_karakterlánc** a kapcsolattal az Azure portálról korábban kapott.  

    ![][73]

2. Mentse a fájlt. 

###<a name="configure-the-app-for-basic-tracking"></a>Állítsa be az alkalmazás egyszerű nyomon követés céljából

1. Nyisson meg szerkesztésre a lejátszó objektumhoz **PlayerController** parancsfájl. 

2. Adja hozzá a következő utasítással:

        using Microsoft.Azure.Engagement.Unity;

3. Az alábbi módon adja hozzá a `Start()` módszer
    
        EngagementAgent.Initialize();
        EngagementAgent.StartActivity("Home");

###<a name="deploy-and-run-the-app"></a>Futtassa az alkalmazást, és terjesztése

1. IOS-eszközökön kapcsolódni a számítógépen. 

2. **Fájl -> Szerkesztés Beállítások** megnyitása 

    ![][40]

3. Jelölje ki az **iOS** , és válassza a **Váltás** a Platform

    ![][41]

    ![][42]

4. Kattintson a **lejátszó beállítások** elemre, és adja meg az érvénytelen az első lépésekhez azonosító. 

    ![][53]

5. Végül kattintson a **Szerkesztés és futtatása**

    ![][54]

6. A mappa nevét tárolja az iOS-csomag megadására kérheti. 

    ![][43]

7. Ha mindent rendben, majd a projekt össze kell állítani, és nem kell megnyitni a a XCode alkalmazást. 

8. Ellenőrizze, hogy a projekt megfelelő **foglalja csomagba azonosító** .  

    ![][75]

10. Most már futtassa az alkalmazást XCode, hogy a csomag telepítve van az összekapcsolt eszközre, és meg kell jelennie a egységét játék telefonon! 

##<a id="monitor"></a>Alkalmazás kapcsolatba valós idejű ellenőrzése

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a id="integrate-push"></a>Leküldéses értesítések és az alkalmazás üzenetküldés engedélyezése

Mobil tetszés szerint elmélyedhet lehetővé teszi, hogy a felhasználók kommunikálni, és a leküldéses értesítések és az-app kampányok környezetében üzenetküldés is ELÉRHETI. Ez a modul neve vannak a Mobile tetszés szerint elmélyedhet portálon.
A teendő, ha értesítést szeretne kapni az alkalmazás bármely további konfigurációja nincs, és már be állítva.

[AZURE.INCLUDE [mobile-engagement-ios-send-push-push](../../includes/mobile-engagement-ios-send-push.md)]

<!-- Images. -->
[40]: ./media/mobile-engagement-unity-ios-get-started/40.png
[41]: ./media/mobile-engagement-unity-ios-get-started/41.png
[42]: ./media/mobile-engagement-unity-ios-get-started/42.png
[43]: ./media/mobile-engagement-unity-ios-get-started/43.png
[53]: ./media/mobile-engagement-unity-ios-get-started/53.png
[54]: ./media/mobile-engagement-unity-ios-get-started/54.png
[70]: ./media/mobile-engagement-unity-ios-get-started/70.png
[71]: ./media/mobile-engagement-unity-ios-get-started/71.png
[72]: ./media/mobile-engagement-unity-ios-get-started/72.png
[73]: ./media/mobile-engagement-unity-ios-get-started/73.png
[74]: ./media/mobile-engagement-unity-ios-get-started/74.png
[75]: ./media/mobile-engagement-unity-ios-get-started/75.png
