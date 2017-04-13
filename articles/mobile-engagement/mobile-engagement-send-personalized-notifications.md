<properties 
    pageTitle="Azure Mobile tetszés szerint elmélyedhet a személyre szabott értesítés küldése" 
    description="Személyre szabott értesítések üzenetküldés az értesítéseket, például a nevüket a felhasználóiprofil-adatok felvételével"        
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="all" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="personalize-notifications-by-including-user-name"></a>Értesítések testreszabása felhasználónév felvételével

Az, hogy a értesítések nagyszerű megjelenést biztosíthat neki a alkalmazás felhasználók hívása érdemes megfontolni, személyre szabása őket. Egy hatékony megközelítés magában foglalja a szelektív módon használnak az alkalmazás felhasználók nevét, hogy további személyes értesítések megoldására. Egy szót, amire érdemes ügyelni itt - meg kell megközelíthető hozzáadása felhasználónevek az értesítések gondosan mert ha, a stratégia túlhasználatnak majd azt sikerült származnak, creepy alkalmazás felhasználók. Biztosítania kell, hogy vannak-e engedélyezem a felhasználó ahhoz, hogy a személyes adatok, a mobilalkalmazásban teljes jóváhagyásával: Ezzel a megkezdése előtt csatlakozás. 

Értelemben az Azure-Mobile Előjegyzéssel elvégezhető az értesítések testreszabása, amelyben fogjuk használni az alkalmazási példát, beleértve a felhasználó nevét az értesítés az alábbi lépéseket követve. Amely App – útmutatók a használni kívánt, vagy a címkéket, amelynek értékeit vagy átadandó által a SDK integrált API-hoz vagy a Mobile alkalmazásban. E alkalmazás-folyamat elejéig vagy a címkék akkor kell használni:

1. az értesítések az adott felhasználókhoz az App – útmutatók értékein alapuló kiválasztásával vagy 
2. az értesítés, amely cseréli a eszköz felhasználó adott értékek közben értesítést küld, hogy az eszköz a helyőrzőként. 

> [AZURE.IMPORTANT] Figyelje meg, hogy értesítést küld sebességének miatt egyes értesítések app – útmutatók értékek cseréje a további feldolgozása fogják látni csökkentése. 

##<a name="register-app-info-in-the-mobile-engagement-portal"></a>App – útmutatók regisztrálhatja a mobil tetszés szerint elmélyedhet portálon

1) Indítsa el az alkalmazás információ vagy a címkék regisztrálása az Azure-portálon. Nyissa meg a **Beállítások** -> a**címke (App – útmutatók)** .  

![][1]  

2) Kattintson az **Új** címke (app – útmutatók), és adja meg a név, *felhasználónév* és a *karakterlánc* típusú, és kattintson a **Küldés gombra**. 

![][2]

3) Végül jelennek meg a app – útmutatók regisztrált a következőhöz hasonló:

![][3]

##<a name="send-app-info-from-the-client-sdk"></a>Alkalmazás-adatok küldése a ügyfélprogramból SDK

Itt azt használja a Windows univerzális alkalmazás példa, de a egyenértékű módszerek állnak rendelkezésre a többi SDK is. 

Feltételezve, hogy hol, letölthető a profiladatokat a felhasználót, például a nevük valószínűleg őket hitelesítését követően a mobilalkalmazásban módszer van, akkor visszahívja `SendAppInfo` módszer és feltöltése értékét a `user_name` be a mobil tetszés szerint elmélyedhet szolgáltatás kódmentes korábbi regisztrált app útmutatók. 

    Dictionary<object, object> appInfo = new Dictionary<object, object>();
    appInfo.Add("user_name", str);
    EngagementAgent.Instance.SendAppInfo(appInfo); 

##<a name="send-personalized-notifications"></a>Személyre szabott értesítések küldésére

Most már minden beállítása a **felhasználónév**használatával értesítések küldésére áll. 

1) Mobile tetszés szerint elmélyedhet portálon lépjen értesítés létrehozása **elérje a** lapon, és használhatja a helyőrző bárhol az értesítés címét vagy a szervezet a következő formátumban. 

![][4]  

> [AZURE.NOTE] Minden olyan felhasználók, amelynek a felhasználónév app útmutatók értéke nem, nem kap értesítést. Ha az értesítési marketingkampány teszt módban futtatja, és nem rendelkezik app – útmutatók küldeni fogunk, majd állítsa "?" karakter le szeretné cserélni a helyőrzőt. 

2) Amikor a Mobile tetszés szerint elmélyedhet fog válasszon egy eszközt, ez az értesítés küldése, akkor tekintse meg a app – útmutatók, és a helyőrző értéket.  
Tegyük fel például, hogy állított `str = "Scott"` egy felhasználóhoz, mint az eszköz regisztrációs fog kapni az alkalmazás adatainak társított **felhasználónév = SCOTT** esetében a felhasználó és a felhasználó megjelenik a "Házon kívül" alkalmazás leküldéses értesítést a következő formátumban. 

![][5]  

<!-- Images. -->
[1]: ./media/mobile-engagement-send-personalized-notifications/app-info.png
[2]: ./media/mobile-engagement-send-personalized-notifications/create-app-info.png
[3]: ./media/mobile-engagement-send-personalized-notifications/app-info-user-name.png
[4]: ./media/mobile-engagement-send-personalized-notifications/personal-notification.png
[5]: ./media/mobile-engagement-send-personalized-notifications/notification.png

