<properties
    pageTitle="Azure Mobile tetszés szerint elmélyedhet a bemutató alkalmazás |} Microsoft Azure"
    description="Ismerteti, honnan töltheti le, használatáról és a Azure Mobile tetszés szerint elmélyedhet a bemutató alkalmazás előnyei"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/10/2016"
    ms.author="piyushjo" />

# <a name="azure-mobile-engagement-demo-app"></a>Azure Mobile tetszés szerint elmélyedhet a bemutató alkalmazás

Azt korábban közzétett Azure Mobile tetszés szerint elmélyedhet a bemutató alkalmazás **iOS**, **Android**és **Windows** platformon segít hasznos források keresése és többet szeretne tudni a Mobile részvételét.

Az alkalmazás nyújt segítséget:

- Egyszerűen a Mobile tetszés szerint elmélyedhet erőforrások, mint a videók, dokumentáció, a támogatási fórum és frissítéseiről tipp információforrás hasznos témakörökben talál.
- Tapasztalat minta értesítések Mobile tetszés szerint elmélyedhet a saját mobilalkalmazások ötleteket által támogatott.
- A hivatkozás megvalósítás segítségével tanulmányozása megvalósításáról Mobile tetszés szerint elmélyedhet a saját alkalmazásba. Azt is megtudhatja, hogy miként:

    - Analytics adatainak összegyűjtése.
    - Speciális típusú, például a *teljes képernyős közbeszúrt* vagy *az előugró*értesítést esetek megvalósítása.
    - Felmérések és szavazásokra végrehajtása.
    - Adatok és a leküldéses esetek csendes leküldéses végrehajtása.   

## <a name="app-installation"></a>Alkalmazás telepítése
Az alkalmazás a következő alkalmazás-áruház érhető el:

- **A Windows univerzális bemutató alkalmazás**:

    - Töltse le az alkalmazást a [Windows-alkalmazás tárolni](https://www.microsoft.com/en-us/store/apps/azure-mobile-engagement/9nblggh4qmh2).
    - Az alkalmazás Windows 10 univerzális alkalmazásként kifejlesztett. A forráskód [GitHub](https://github.com/Azure/azure-mobile-engagement-app-windows)érhető el.

- **iOS demó alkalmazás**:

    - Töltse le az alkalmazást az [Apple tárolni](https://itunes.apple.com/us/app/azure%20mobile%20engagement/id1105090090).
    - Az alkalmazás az iOS Swift kifejlesztett. A forráskódot [GitHub](https://github.com/Azure/azure-mobile-engagement-app-ios)érhető el.

- **A bemutató android alkalmazást**:

    - Töltse le az alkalmazást a [Google Play tárolni](https://play.google.com/store/apps/details?id=com.microsoft.azure.engagement).
    - A forráskód [GitHub](https://github.com/Azure/azure-mobile-engagement-app-android)érhető el.

![A Windows univerzális bemutató alkalmazás][1]

![iOS demó alkalmazás][2]
![alkalmazás Android bemutatása][3]


## <a name="usage"></a>Használat

Az alkalmazás is használhatja az alábbi módon:

**Töltse le az alkalmazás a készülékén az alkalmazás áruházból hivatkozások (korábban megadott):**

>[AZURE.IMPORTANT] Az Azure-fiók vagy segítségre van szüksége az alkalmazás csatlakozhat egy vissza vége nem szükséges. Az alkalmazás függetlenül működik.

- Után az alkalmazás a készülékén, majd nyissa meg a bal oldali menüben a Mobile tetszés szerint elmélyedhet hasznos információforrásokért-kapcsolaton keresztül.
- Hozzáadtunk az [RSS-hírcsatorna szolgáltatás](https://aka.ms/azmerssfeed) a alkalmazásba, így esetén mindig frissülnek a legújabb frissítéseket kapcsolatos.
- Is beléphet az minta értesítés jelenik meg értesítés platformokon Mobile tetszés szerint elmélyedhet által támogatott típusú tapasztal keresztül. Ezeket az értesítéseket is tapasztalható helyben – vagyis, a gombra a mutatja, hogy az értesítések változat, amely megegyezik az értesítést küld a Mobile tetszés szerint elmélyedhet platform képernyőjén.

![A Windows App menüje][4]

![Az iOS alkalmazásmenüre][5]
![alkalmazásmenüre androidra][6]

**Töltse le a forráskód (korábban megadott) GitHub hivatkozások:**

- Miután letöltötte a forráskód, nyissa meg a megfelelő fejlesztői környezet – IOS, Android és Visual Studio Windows Android Studio XCode.
- Ezután, kövesse a [SDK integrációs alaplépései](mobile-engagement-windows-store-dotnet-get-started.md) , hogy az alkalmazás csatlakoztatása a saját Mobile tetszés szerint elmélyedhet háttéradatbázist példány tudja.
    - Kapcsolati karakterlánc konfigurálása a alkalmazásban van szükség.
    - Meg kell állítsa be a leküldéses értesítést platform az alkalmazás.
- Észre fogja venni, hogy az alkalmazás magát rendszereken-e a Mobile részvételét. Tehát ahogy azt a háttér összekötő után nyissa meg az alkalmazást, is láthatja a felhasználó munkamenet, tevékenységek, események és egyebekkel, **Monitor** lapján.
- Akkor is küldhet értesítést el ehhez az alkalmazáshoz a saját Mobile tetszés szerint elmélyedhet példányából helyi értesítések használata helyett.
    - Itt felveheti az eszköz próba eszköz **az eszköz ID azonosító beszerzése** menüpont az alkalmazás használatával. Ezzel kap olyan, majd rögzítheti a próba eszközként a platform háttér-példányt tartson Eszközazonosítót.

    ![A Windows Eszközazonosítót][7]

    ![Az iOS operációs rendszeren Eszközazonosítót][8]
    ![Eszközazonosítót Android-eszközön][9]

## <a name="key-features-of-the-demo-app"></a>A bemutató alkalmazás fontosabb funkciói

- Amint már említettük, az alkalmazás, akkor a fő erőforrások a Mobile tetszés szerint elmélyedhet a kéz. Nyissa meg a kapcsolaton keresztül a bal oldali menüben.

- Ki az alkalmazást az értesítések platformokon sorolunk. **Csak az értesítés**, amikor megnyitása gombjára kattintva az értesítés egyszerűen az alkalmazás a natív képernyővel felfelé (használatával **mély csatolása**) – vagy a **webes bejelentése**, ahol tartana további HTML-tartalmat a Mobile tetszés szerint elmélyedhet háttér megjeleníteni az értesítés kattintáskor a is kézbesítése ezeket az értesítéseket.

    ![Ki az alkalmazást az értesítések][29]

- IOS rendelkezik a ki az alkalmazást, vagy a rendszer push értesítés jelenik meg, az alkalmazás bezárásához. Megnézheti hozzáadásának **Akciógombok**, mint a hozzáadott az ki az alkalmazást az értesítésre *visszajelzési* és *megosztása* (úgy, hogy a felhasználó eltarthat magát értesítési művelet jobbra) az alábbi végrehajtása.

    ![Ki az alkalmazást az értesítések iOS operációs rendszeren][11] ![IOS ki az alkalmazást az értesítés megjelenítése][14]

- Android-eszközön a támogatott beállításokat hozzáadni többsoros szöveg (**Nagy szöveg**) vagy egy értesítés kép (**Áttekintés**) az értesítésre, a **művelet gombok** együtt (mint az iOS által támogatott).

    ![Ki az alkalmazást az értesítések Android-eszközön][12] ![Android-eszközön ki az alkalmazást az értesítés megjelenítése][15]

- A Windows 10 megtekintheti, hogyan jelennek meg az értesítéseket a PC-re. Ez az értesítés is megjelenik a Windows 10-es **Center értesítési**. Nem támogatja a **művelet gombok** hozzáadása a Windows SDK jelenleg nem.

    ![A Windows ki az alkalmazást az értesítések][10] ![A Windows ki az alkalmazást a képernyő][13]

- Alapértelmezett "a-" alkalmazásértesítések platformokon sorolunk. Itt jelenik meg egy két lépésből álló élmény **egy értesítéseken** az első. Amikor kattint, megnyitja hozzá be a teljes képernyős **bejelentése**, az alábbi képernyőképen látható módon. A tartalom a jelen közleményt a Mobile tetszés szerint elmélyedhet háttéradatbázist példány származik. A SDK csomagjában talál az értesítéseket és a hirdetmények sablonokat tartalmaz. Egyszerűen testre szabhatja velük, az embléma és színezés hozzáadásával bemutató alkalmazás látható módon.  

    ![A Windows-app értesítések][16]

    ![Az alkalmazás-értesítések az iOS operációs rendszeren][17]  ![Az alkalmazás értesítések Android-eszközön][18]

    **iOS**, **Android**

- A mobil tetszés szerint elmélyedhet **kategória** funkciót a alapértelmezett felhasználói felület testreszabása is használhatja. A bemutató alkalmazásban azt módosíthatja az értesítés a felület közös kétféleképpen már mutatni. Figyelje meg, hogy a kategória szolgáltatás még nem támogatott a Windows SDK.

    **Teljes képernyős közbeszúrt:**

    ![Az alkalmazás értesítést - közbeszúrt kategória][30]

    ![IOS közbeszúrt kategóriára][21]  ![Android-eszközön közbeszúrt kategória][22]

    **Előugró ablakokat:**

    ![Az alkalmazás értesítés – előugró kategória][31]

    ![Az iOS operációs rendszeren értesítési buboréka][19]   ![Az előugró értesítést, Android-eszközön][20]

**iOS**, **Android**

- Mobil tetszés szerint elmélyedhet támogatja az alkalmazás értesítést **szavazásokra**nevű egy speciális típusú is. Lehetővé teszi, hogy a szegmentált alkalmazás felhasználók rövid felmérést küldhet. Kérdések és a beállítások minden kérdés, ahogy az alábbi képernyőképen a is hozzáadhat. Ez majd első jelenik meg, az alkalmazás értesítjük a alkalmazás felhasználó számára.   

    ![A szavazás értesítések][32]

    ![Felmérés Windows rendszeren][26]

    ![IOS-felmérés][27]   ![Felmérés Android-eszközön][28]

**iOS**, **Android**

- Mobil tetszés szerint elmélyedhet is támogat, csendes **Adatok leküldéses** értesítések küldése. Ezeket az értesítéseket, küldhet adatokat a szolgáltatásból (például a következő példa JSON adatainak), amely kezelheti az alkalmazást, és néhány művelet végrehajtása. Példa, hogy hogyan azt módosítani szeretné egy elem árát szelektív adatok leküldéses értesítéseket használatával.

    ![Adatok leküldéses értesítést][33]

    ![Adatok leküldéses értesítést Windows rendszeren][23]

    ![Adatok leküldéses értesítést iOS operációs rendszeren][24]  ![Adatok leküldéses értesítést Android-eszközön][25]

**iOS**, **Android**

> [AZURE.NOTE] Az értesítések valamelyike részletes ismertetését egy minta értesítés képernyőt **kapcsolatos tudnivalókat a Mobile tetszés szerint elmélyedhet platform ezeket az értesítéseket küldeni, kattintson ide** gombra kattintva tekinthető meg.


[1]: ./media/mobile-engagement-demo-apps/home-windows.png
[2]: ./media/mobile-engagement-demo-apps/home-ios.png
[3]: ./media/mobile-engagement-demo-apps/home-android.png
[4]: ./media/mobile-engagement-demo-apps/menu-windows.png
[5]: ./media/mobile-engagement-demo-apps/menu-ios.png
[6]: ./media/mobile-engagement-demo-apps/menu-android.png
[7]: ./media/mobile-engagement-demo-apps/device-id-windows.png
[8]: ./media/mobile-engagement-demo-apps/device-id-ios.png
[9]: ./media/mobile-engagement-demo-apps/device-id-android.png
[10]: ./media/mobile-engagement-demo-apps/out-of-app-windows.png
[11]: ./media/mobile-engagement-demo-apps/out-of-app-ios.png
[12]: ./media/mobile-engagement-demo-apps/out-of-app-android.png
[13]: ./media/mobile-engagement-demo-apps/out-of-app-display-windows.png
[14]: ./media/mobile-engagement-demo-apps/out-of-app-display-ios.png
[15]: ./media/mobile-engagement-demo-apps/out-of-app-display-android.png
[16]: ./media/mobile-engagement-demo-apps/in-app-windows.png
[17]: ./media/mobile-engagement-demo-apps/in-app-ios.png
[18]: ./media/mobile-engagement-demo-apps/in-app-android.png
[19]: ./media/mobile-engagement-demo-apps/pop-up-ios.png
[20]: ./media/mobile-engagement-demo-apps/pop-up-android.png
[21]: ./media/mobile-engagement-demo-apps/interstitial-ios.png
[22]: ./media/mobile-engagement-demo-apps/interstitial-android.png
[23]: ./media/mobile-engagement-demo-apps/data-push-windows.png
[24]: ./media/mobile-engagement-demo-apps/data-push-ios.png
[25]: ./media/mobile-engagement-demo-apps/data-push-android.png
[26]: ./media/mobile-engagement-demo-apps/survey-windows.png
[27]: ./media/mobile-engagement-demo-apps/survey-ios.png
[28]: ./media/mobile-engagement-demo-apps/survey-android.png
[29]: ./media/mobile-engagement-demo-apps/out-of-app.png
[30]: ./media/mobile-engagement-demo-apps/in-app-interstitial.png
[31]: ./media/mobile-engagement-demo-apps/in-app-pop-up.png
[32]: ./media/mobile-engagement-demo-apps/notification-poll.png
[33]: ./media/mobile-engagement-demo-apps/notification-data-push.png
