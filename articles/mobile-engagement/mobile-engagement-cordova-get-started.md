<properties
    pageTitle="Azure mobil részvételét Cordova/Phonegap az első lépések"
    description="Megtudhatja, hogy miként analitikai és a leküldéses értesítések Azure Mobile tetszés szerint elmélyedhet használata Cordova/Phonegap alkalmazások."
    services="mobile-engagement"
    documentationCenter="Mobile"
    authors="piyushjo"
    manager="dwrede"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-phonegap"
    ms.devlang="js"
    ms.topic="hero-article" 
    ms.date="08/19/2016"
    ms.author="piyushjo" />

# <a name="get-started-with-azure-mobile-engagement-for-cordovaphonegap"></a>Azure mobil részvételét Cordova/Phonegap az első lépések

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

Ez a témakör bemutatja, hogyan Azure Mobile tetszés szerint elmélyedhet használatával az alkalmazás használatát megértéséhez, és küldje el a leküldéses értesítések szegmentált felhasználók Cordova kidolgozni mobile alkalmazáshoz.

Ebben az oktatóanyagban azt létrehoz egy üres Cordova alkalmazást, Mac és integrálhatja a Mobile tetszés szerint elmélyedhet SDK csomagjában talál. Egyszerű analytics adatainak gyűjti és az Apple leküldéses értesítést rendszer (APNS) használ az iOS és a Google Cloud üzenetküldés (GCM) az Android-alapú leküldéses értesítések fogadása. Azt telepítésének ez egy iOS és Android-készülék teszteléshez. 

> [AZURE.NOTE] Ebben az oktatóanyagban befejezéséhez, az aktív Azure fiókkal kell rendelkezniük. Nem rendelkeznek fiókkal, ha mindössze néhány perc is létrehozhat ingyenes próba-fiók. A részletekért lásd: [Azure ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-cordova-get-started).

Ebben az oktatóprogramban az alábbiakra van szükség:

+ XCode, amely is telepítheti a Mac App Store (telepítése IOS-en)
+ [Android SDK & irányító](http://developer.android.com/sdk/installing/index.html) (az Android-alapú bevezetéshez)
+ Leküldéses értesítést tanúsítvány (.p12), letöltheti az Apple fejlesztői központ az APN
+ Szerezhetők be a Google Fejlesztőeszközök konzolról GCM GCM projektszámot
+ [Mobil tetszés szerint elmélyedhet Cordova beépülő modul](https://www.npmjs.com/package/cordova-plugin-ms-azure-mobile-engagement)

> [AZURE.NOTE] A [Github](https://github.com/Azure/azure-mobile-engagement-cordova) Cordova beépülő modul megtalálhatja a Forráskód és a fontos fájl

##<a id="setup-azme"></a>A telepítő mobil tetszés szerint elmélyedhet a Cordova alkalmazáshoz

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

##<a id="connecting-app"></a>Kapcsolódás az alkalmazás a Mobile tetszés szerint elmélyedhet kódmentes

Ebben az oktatóprogramban egy "egyszerű integráció" van-e a minimális beállítva szükséges adatok összegyűjtése és küldje el a leküldéses értesítést, amely mutatja be. 

Egy egyszerű alkalmazás Cordova keresztül mutatjuk integrálása a hozunk létre:

###<a name="create-a-new-cordova-project"></a>Cordova új projekt létrehozása

1. Indítsa el a Mac gépen *Terminálszolgáltatások* ablakban, és írja be a következő, amely Cordova új projektet hoz létre az alapértelmezett sablon alapján. Győződjön meg arról, hogy a közzétételi profil ahányat használatával az iOS-alkalmazások terjesztése "com.mycompany.myapp" használ, az alkalmazás azonosítója. 

        $ cordova create azme-cordova com.mycompany.myapp
        $ cd azme-cordova

2. Hajtsa végre a következő beállítása az **iOS** a projekt, és indítsa el az iOS simulatort használja a:

        $ cordova platform add ios 
        $ cordova run ios

3. Hajtsa végre a következő, a projekt beállítása az **Android** , és indítsa el az Android irányító a. Győződjön meg arról, hogy a Android SDK irányító beállításainak rendelkezik a cél megjelölése Google API-khoz (Google Inc.) a Processzor / ABI, mint a Google API-khoz ARM.  

        $ cordova platform add android
        $ cordova run android

4.  Adja hozzá a Cordova Console beépülő modul. 

        $ cordova plugin add cordova-plugin-console 

###<a name="connect-your-app-to-mobile-engagement-backend"></a>Az alkalmazás csatlakoztatása Mobile tetszés szerint elmélyedhet kódmentes

1. Az Azure Mobile tetszés szerint elmélyedhet Cordova beépülő modul telepítése során kezeléséről a változók értékein konfigurálása a beépülő modul:

        cordova plugin add cordova-plugin-ms-azure-mobile-engagement    
            --variable AZME_IOS_CONNECTION_STRING=<iOS Connection String> 
            --variable AZME_IOS_REACH_ICON=... (icon name WITH extension) 
            --variable AZME_ANDROID_CONNECTION_STRING=<Android Connection String> 
            --variable AZME_ANDROID_REACH_ICON=... (icon name WITHOUT extension)       
            --variable AZME_ANDROID_GOOGLE_PROJECT_NUMBER=... (From your Google Cloud console for sending push notifications) 
            --variable AZME_ACTION_URL =... (URL scheme which triggers the app for deep linking)
            --variable AZME_ENABLE_NATIVE_LOG=true|false
            --variable AZME_ENABLE_PLUGIN_LOG=true|false

*Android elérjen ikonra* : kell lennie a bármelyik mellék, sem drawable előtag nélkül az erőforrás neve (ex: mynotificationicon), és a ikon fájlt az android (platformokon/android/felbontás/drawable) projektbe kell másolni

*iOS elérjen ikonra* : kell lenniük, amit a kiterjesztése az erőforrás neve (ex: mynotificationicon.png), és az ikon fájl hozzá kell adnia az iOS projektbe a XCode (használata a fájlok hozzáadása menü)

##<a id="monitor"></a>Valós idejű ellenőrzése engedélyezése

1. A Cordova projekt - szerkesztése **www/js/index.js** hozzáadása a Mobile tetszés szerint elmélyedhet egyszer deklarálása új tevékenységet a *deviceReady* esemény-hívás érkezik.

         onDeviceReady: function() {
                Engagement.startActivity("myPage",{});
            }

2. Futtassa az alkalmazást:
        
    - **Az iOS**
    
        A `Terminal` ablak indítsa el az alkalmazás új példányát simulatort használja a hajtja végre a következőket:

            cordova run ios

    - **Az Android-alapú**
        
        A `Terminal` ablak indítsa el az alkalmazást az új irányító példány hajtja végre a következő:

            cordova run android

3. Megjelenik a konzol naplókban a következő:

        [Engagement] Agent: Session started
        [Engagement] Agent: Activity 'myPage' started
        [Engagement] Connection: Established
        [Engagement] Connection: Sent: appInfo
        [Engagement] Connection: Sent: startSession
        [Engagement] Connection: Sent: activity name='myPage'

##<a id="monitor"></a>Alkalmazás kapcsolatba valós idejű ellenőrzése

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a id="integrate-push"></a>Leküldéses értesítések és az alkalmazás üzenetküldés engedélyezése

Mobil tetszés szerint elmélyedhet lehetővé teszi, hogy a felhasználók a leküldéses értesítések és az-app kampányok környezetében üzenetküldés használata. Ez a modul neve vannak a Mobile tetszés szerint elmélyedhet portálon.
A következőkben lesz beállítási is kapni az alkalmazás.

###<a name="configure-push-credentials-for-mobile-engagement"></a>Mobil tetszés szerint elmélyedhet leküldéses hitelesítő adatok beállítása

Engedélyezi a mobil tetszés szerint elmélyedhet leküldéses értesítéseket küldeni az Ön nevében, szüksége az Apple iOS tanúsítvány és a GCM kiszolgáló API-ja kulcs hozzáférés megadását. 
    
1. Nyissa meg a Mobile tetszés szerint elmélyedhet portált. Győződjön meg róla, akkor ehhez a projekthez használ, és válassza a **Engage** gombra a képernyő alján az alkalmazás a:
    
    ![][1]
    
2. A tevékenységek portálon land lesz a beállítások lapon. Az ott kattintson erre a **Natív leküldéses** területen:
    
    ![][2]

3. IOS tanúsítvány/GCM kiszolgáló API-ja kulcs konfigurálása

    **[iOS]**

    egy. Jelölje ki a .p12, töltse fel, és írja be a jelszavát:
    
    ![][3]

    **[Android]**

    egy. Kattintson a Szerkesztés ikonra elé **API -ja kulcs** GCM beállításai csoportban, és a helyi menü, mely azt mutatja be, illessze be a GCM kiszolgáló billentyűt, és kattintson az **OK gombra**. 
        
    ![][4]

###<a name="enable-push-notifications-in-the-cordova-app"></a>A Cordova alkalmazásban a leküldéses értesítések engedélyezése

**Www/js/index.js** hozzáadása a hívást a Mobile tetszés szerint elmélyedhet, kérje a leküldéses értesítéseket, és egy kezelő deklarálhatnak szerkesztése:

     onDeviceReady: function() {
           Engagement.initializeReach(  
                // on OpenUrl  
                function(_url) {   
                alert(_url);   
                });  
            Engagement.startActivity("myPage",{});  
        }

###<a name="run-the-app"></a>Futtassa az alkalmazást

**[iOS]**

1. Építse fel és telepítse az alkalmazást az eszközön, tesztelje a leküldéses értesítéseket, mivel az iOS csak lehetővé teszi, hogy az aktuális eszközre a leküldéses értesítések XCode használja azt. Nyissa meg a helyet, ahol a Cordova projekt hoz létre, és keresse meg a beszúrandó **...\platforms\ios** . A natív .xcodeproj XCode fájl megnyitása. 
    
2. Készítése és a Cordova alkalmazások terjesztése az iOS-eszközön fiókkal, amely a kiépítési profil tartalmazó a tanúsítvány imént feltöltött a Mobile tetszés szerint elmélyedhet portál és az alkalmazás azonosítója mellett a Cordova-alkalmazás létrehozása során a megadott mely találat az adott. Az *első lépésekhez azonosító* érdemes a **erőforrások\*-info.plist** XCode egyezik meg a fájlt. 

3. A szokásos iOS előugró közli, hogy az alkalmazás kér értesítést küldhet jogosultsági eszközén jelenik meg. Engedélyt. 

**[Android]**

A irányító segítségével egyszerűen GCM értesítések támogatottak az Android irányító futtatni az Android alkalmazást. 

    cordova run android

##<a id="send"></a>Értesítés küldése az alkalmazásba

Most már hozunk létre egy egyszerű leküldéses értesítést marketingkampány, hogy a leküldéses elküldi az alkalmazás fut az eszközön:

1. Nyissa meg azt a Mobile tetszés szerint elmélyedhet portálon **elérje a** lapon

2. Kattintson a **Születéséről** a leküldéses marketingkampány létrehozása

    ![][6]

3. Adja meg a bemeneti adatok alapján a **[Android]** marketingkampány létrehozása
    
    - Adjon **nevet** a marketingkampány. 
    - Jelölje be a *rendszer értesítést* *egyszerű* **Kézbesítési típusa**
    - Jelölje be a **kézbesítési idő** *"bármely idő"*
    - Az értesítésben, amely a leküldéses első sora lesz a **cím** szükséges.
    - A szükséges **üzenet** az értesítésben, amelyek szerint a szövegtörzs szolgálnak. 

    ![][11]

4. Adja meg a bemeneti adatok alapján hozhat létre a marketingkampány **[iOS]**

    - Adjon **nevet** a marketingkampány. 
    - Jelölje be a **kézbesítési idő** *"ki csak letölthető"*
    - Az értesítésben, amely a leküldéses első sora lesz a **cím** szükséges.
    - A szükséges **üzenet** az értesítésben, amelyek szerint a szövegtörzs szolgálnak. 
 
    ![][12]

5. Görgessen lefelé, és a tartalom szakaszban jelölje be a **csak az értesítés**

    ![][8]

6. [Választható] Akkor is megadhatja egy művelet URL-címe. Győződjön meg arról, hogy használja-e a beépülő modul beállításakor megadott URL-cím séma **AZME\_ÁTIRÁNYÍTÁS\_URL-cím** változó például *myapp://test*.  

7. Ezzel elkészült a legalapvetőbb marketingkampány lehetséges beállítást. Most görgessen lefelé, és kattintson a **Létrehozás** gombra a marketingkampány mentéséhez.

8. Végül **aktiválja** a marketingkampány
    
    ![][10]

9. Most már látnia kell a leküldéses értesítést az eszközön vagy irányító részeként ebben marketingkampány. 

##<a id="next-steps"></a>Következő lépések
[Minden módszer Cordova Mobile tetszés szerint elmélyedhet SDK elérhető áttekintése](https://github.com/Azure/azure-mobile-engagement-cordova)

<!-- Images. -->

[1]: ./media/mobile-engagement-cordova-get-started/engage-button.png
[2]: ./media/mobile-engagement-cordova-get-started/engagement-portal.png
[3]: ./media/mobile-engagement-cordova-get-started/native-push-settings.png
[4]: ./media/mobile-engagement-cordova-get-started/api-key.png
[6]: ./media/mobile-engagement-cordova-get-started/new-announcement.png
[8]: ./media/mobile-engagement-cordova-get-started/campaign-content.png
[10]: ./media/mobile-engagement-cordova-get-started/campaign-activate.png
[11]: ./media/mobile-engagement-cordova-get-started/campaign-first-params-android.png
[12]: ./media/mobile-engagement-cordova-get-started/campaign-first-params-ios.png

