<properties 
   pageTitle="Azure mobil tetszés szerint elmélyedhet felhasználói felület - vannak hogyan"
   description="Azure mobil tetszés szerint elmélyedhet a felhasználói felület áttekintése" 
   services="mobile-engagement" 
   documentationCenter="" 
   authors="piyushjo" 
   manager="dwrede" 
   editor=""/>

<tags
   ms.service="mobile-engagement"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="mobile-multiple"
   ms.workload="mobile" 
   ms.date="08/19/2016"
   ms.author="piyushjo"/>

# <a name="how-to-get-started-using-and-managing-pushes-to-reach-out-to-your-end-users"></a>Ismerkedés a használata és kezelése verembe küldi kattintva keresse meg a végfelhasználók

A SDK teljes mértékben integrálva van az alkalmazás, miután használatának megkezdéséhez használatával az a felhasználói felület, hogy az alkalmazás a felhasználóknak a leküldéses értesítések vannak szakaszát.  

## <a name="do-your-first-push-notification-campaign"></a>Végezze el az első leküldéses értesítést marketingkampány
-    Győződjön meg arról, hogy a gyermekektől integrálva van-e az alkalmazást, és a SDK csomagjában talál. 
-    Jelölje ki azt az alkalmazást.
 
![First1][1]

-    Nyissa meg a "Vannak" szakaszt, és kattintson a "születéséről"
 
![First2][2]

-    Hozzon létre egy új marketingkampány, és nevezze el
 
 ![First3][3]

-    Jelölje ki, hogyan az értesítés kézbesítési, mint a-app csak
 
![First4][4]

-    A leküldéses kívánt üzenet létrehozása
 
![First5][5]

-    Az értesítés (nem kötelező) a címet is írhat.
-    Leküldéses üzenet tartalom megírása.
-    Kép feltöltése Ne feledje, hogy a fájl mérete nem haladhatja meg 32 768 bájt.
-    Válassza a további beállítások lehetősége is van, de a ebben az oktatóanyagban fókuszt, akkor jelenik meg, hogy később.

-    Csak az értesítés, a tartalomtípus kiválasztása
 
![First6][6]

-    A leküldéses marketingkampány létrehozása, és meg kíván jeleníteni a marketingkampány listában.
 
![First7][7]

## <a name="test-your-push-notification-campaign"></a>A leküldéses értesítést marketingkampány tesztelése
![Teszt1][8]

-    Regisztráljon az eszközön.
-    Kattintson az eszköz szeretné küldeni a jelölőnégyzetet.
-    Kattintson a a leküldéses küldeni az eszköz "Próba" gombra.
 
![Teszt2][9]

-    A marketingkampány aktiválása
 
![Teszt3][10]

-    Most, hogy a marketingkampány létrehozott csak az értesítésben, ahol a felhasználók kell nyomni az aktiválni kell.
 
## <a name="send-personalized-pushes"></a>Személyre szabott veremmutatót küldése
-    Ez a példa létrehoz egy leküldéses, ahol egy egyéni visszatérítési kódja bekerül a leküldéses értesítést.
 
![Personalize1][11]

Személyre szabási works szerint-app információ címkét a jelölőt így cserélésével, is, hogy a felhasználó rendelkezik-e definiált először megfelelő app útmutatók. Ebben a példában a célként megadott felhasználók egy alkalmazás információ címke definiált rebate_code nevű rendelkeznek.
Felett látható módon a a leküldéses értesítést tartalom tartalmaz, amely jelzi, hogy az a tényleges tartalmat az alkalmazás információ címke helyébe jelölő ${rebate_code}.

> Figyelmeztetés: Ha az alkalmazás információ címke a felhasználónak nincs megadva, a felhasználó nem kap a leküldéses.

-    Eredmény
 
![Personalize2][12]

### <a name="you-can-further-personalize-the-text-your-notification"></a>További személyre szabhatja a szöveget az értesítésben
![Personalize3][13]

-    A cím, az értesítés, többek között
-    És az üzenet tartalmát.
-    Válassza ki a bejelentése (szöveg vagy webes nézetben)
 
![Personalize4][14]

### <a name="the-body-of-an-announcement-may-also-be-personalized-with"></a>A meghívó szövegében tartalmat előfordulhat, hogy is személyre:
-    A művelet URL-címe kell szeretne testre szabhatja a céloldal
-    A cím,
-    Az üzenet törzsébe.
 
 
## <a name="differentiate-your-push-notification-in-or-out-of-app"></a>A leküldéses értesítést például megkülönböztetheti egymástól a (vagy kicsinyítése alkalmazás)
-    Válassza ki a értesítés használni leküldéses, jelölje ki azt az alkalmazást, nyissa meg a "Elérje a" szakaszt, jelölje be vagy a leküldéses marketingkampány létrehozása és az "Értesítési" szakasz a típusát.
 
-    Kattintson a "szállítási mód" szeretné.
-    Kattintson a "Tevékenységek korlátozása" jelölőnégyzetet, ha azt szeretné, hogy az értesítés az adott tevékenység (képernyő) fordul elő.

![Differentiate1][15]

### <a name="out-of-app-only-delivery-mode"></a>"Ki csak letölthető" kézbesítési mód
![Differentiate2][16]

"Ki csak letölthető" kézbesítési módot nyújt a leküldéses értesítést, ha az alkalmazás meg van nyitva. Ez az a szabványos leküldéses értesítést.
"Ki csak letölthető" kiválasztásakor meg előzetesen a platform, hogy az alkalmazás (APNS vagy GCM) támaszkodva tanúsítványai.

### <a name="see-also"></a>Lásd még:
-  [Az Apple leküldéses értesítéseket kezelő szolgáltatás – tanúsítványok](http://developer.apple.com/library/mac/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/ApplePushService/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW9), a Google Cloud üzenetküldés – tanúsítvány] (http://developer.android.com/google/gcm/index.html) 

### <a name="in-app-only-delivery-mode"></a>"a-App csak" kézbesítési mód
![Differentiate3][17]

"A-App csak" kézbesítési módot nyújt a leküldéses értesítést, amikor az alkalmazást futtató.
Ez az értesítés, nem kell az APN és GCM rendszer folyamatát.
Az alkalmazás a kézbesítési rendszer elérje a végfelhasználók számára is használhatja.
Teljesen testre szabható az értesítés és döntse el, amelyben a tevékenység (képernyő) az értesítés jelenik meg.

### <a name="anytime-delivery-mode"></a>"Bármikor" kézbesítési mód
Választhat egy "Bármikor" kézbesítési módot, biztosíthatja, hogy a végfelhasználói elérni, hogy az alkalmazás fut-e.
Ha "Bármikor" lehetőséget választja, akkor előzetesen a platform, hogy az alkalmazás (APNS vagy GCM) épít tanúsítványai. 
 
## <a name="schedule-a-push-campaign"></a>A leküldéses marketingkampány ütemezése
### <a name="plan-to-start-a-campaign"></a>Tervezi, hogy egy marketingkampány indítása
![Shedule1][18]

A 21, március és van, hogy tartalmat, és tervezett esetében a 22.és, március a éjfél. A felület végezze el a leküldéses elé kapcsolatának nincs! Megtervezheti előre a pontos percet értesítést küld.
-    Bejelölését a "nincs" jelölőnégyzetet, és válassza a kezdő időpontot 
-    Válassza a dátum és időpont szeretne kezdeni a leküldéses marketingkampány.

### <a name="plan-to-end-a-campaign"></a>Befejezheti a marketingkampány tervezése
![Shedule2][19]

Azt szeretné, hogy le szeretné állítani a a 25, március 3:00 du.: a marketingkampány, de nem tudja, nincs mód.
A felület leküldéses elé kapcsolatának nincs! Előre a marketingkampány leáll pontos perc megtervezése
-    Kattintson a "nincs" jelölőnégyzetet, vagy jelöljön ki egy befejezési időpontot
-    Válassza a dátum és időpont és a leküldéses marketingkampány befejezése.

### <a name="end-a-campaign-manually"></a>Egy marketingkampány manuálisan befejezése
![Shedule3][20]

Alapértelmezés szerint a "nincs" jelölőnégyzet-be van jelölve.
Ez azt jelenti, hogy a marketingkampány elindul, amint aktiválja azt a gyermekektől szakaszban, és amikor elér szakasznál szinkronizálásuk megszűnik.
 
> Megjegyzés: A befejezési dátumának nélkül létrehozott kampányok a leküldéses helyben tárolni az eszközön, és akkor megjeleníthető a következő alkalommal, amikor az alkalmazás nyitja meg, ha a marketingkampány manuálisan véget ér.

## <a name="enhance-a-push-notification-with-a-text-view"></a>Szöveg megtekintésre leküldéses értesítést növelése
### <a name="what-is-a-text-view"></a>Mi az szöveg nézet?
![TextView1][21]

Szöveg nézet szöveg tartalommal előugró ablakot. A helyi menüje a végfelhasználói rákattint a leküldéses értesítést jelenik meg.
Szöveg nézet lehetővé teszi a végfelhasználói további tartalmakat. Ez akkor is a lehetőség a hívást kezdeményez, például az alkalmazást, és egy áruházból, egy weblap megnyitása küldése e-mailt, a geo honosított keresés indítása átirányítása az weblapon Ugrás művelet stb...

### <a name="example-text-view"></a>Példa: Szöveg megtekintése
-    A leküldéses értesítést marketingkampány létrehozása "Vannak" szakaszában, és adjon meg egy nevet a marketingkampány
 
![TextView2][22]

-    Az értesítés, amely megjelenik az üzenet írása
-    Jelölje ki a bejelentése tartalomtípust "szöveg"
 
![TextView3][23]

> Megjegyzés: szöveg nézet megnyomása, mindig megtalálható értesítést először. 

- A szöveg megadása (kiválasztását követően bejelentése szövegtartalom a szakasz jelenik meg, így adhatja meg a megjeleníteni kívánt szöveget.)
 
![TextView4][24]

-    Írja be a címet, amely az üzenet tetején jelenik meg.
-    Írja be a szöveg nézet fő tartalmát.
-    Írja le a tartalmat, amely megjelenik a (Akciógomb lehetővé teszi, hogy az alkalmazás, hogy egy adott műveletet, például az alkalmazás átirányítása az App Store áruházban vagy bármilyen típusú adatforrások, akkor megadhatja az weblapon megnyitása) gombra.
-    Írja be a tartalom, amely megjelenik a Kilépés gombra (a Kilépés gombra kattintva a szöveg nézet eltűnnek.)
 
-    A leküldéses értesítést marketingkampány létrehozása, és meg kíván jeleníteni a marketingkampány listából.
 
![TextView5][25]

-    Bekapcsolhatja a leküldéses értesítést marketingkampány küldése a szöveg megjelenítése a felhasználóknak.
 
![TextView6][26]

-    Eredmény
 
![TextView7][27]

-    A felhasználó megkapja az értesítés, és kattintson rá.
-    A szöveg nézet engedélyezése a felhasználók kezelhetik előugró formában jelenik meg.

## <a name="enhance-a-push-notification-with-a-web-view"></a>A webes nézet leküldéses értesítést növelése
### <a name="what-is-a-web-view"></a>Mi az webes nézet?
![WebView1][28]

Webes nézet a webes tartalmak előugró ablakot. Ez az előugró jelenik meg, ha a végfelhasználói rákattint a leküldéses értesítést.
Webes nézet teszi lehetővé, hogy a végfelhasználói további interakció.
Ez akkor is a lehetőség a hívás átirányítása például Action App Store webhelyet, hogy egy weblap megnyitása, e-mailt küld, a geo honosított keresés indítása stb...

### <a name="example-web-view"></a>Példa: Webes nézet
-    A leküldéses marketingkampány létrehozása "Vannak" szakaszában, és adjon meg egy nevet a marketingkampány.
 
![WebView2][29]

-    Az értesítés, amely megjelenik az üzenet írása
-    Válassza a bejelentése tartalomtípus "web"
 
![WebView3][30]

### <a name="about-announcement-types"></a>Tudnivalók a bejelentése típusok:
- Csak az értesítés: egyszerű szokásos értesítés. Azt jelenti, hogy ha a felhasználó kattint, nincs további nézet jelenik meg, de csak a hozzá tartozó művelet akkor fordul elő.
- Szöveg bejelentése: értesítést, amely a felhasználót, hogy a szöveg nézet figyelmébe folytat.
- Webes küldésről: értesítést, amely a felhasználónak szeretne adni egy pillantást webes nézet folytat.
Jelölje ki a "Webhely küldésről" tartalmat.

> Megjegyzés: Amikor webes nézet, akkor mindig megtalálható értesítést először.

- A webes tartalom meghatározása (kiválasztását követően küldésről webtartalom a alszakasz fog megjelenni, lehetővé téve a megjeleníteni kívánt webtartalom nézet definiálása.)

 
![WebView4][31]

-    Írja be a címet, amely (nem kötelező) az üzenet tetején jelenik meg.
-    Írja be ide a HTML-kódot.
-    Kattintson a forrás szerkesztése edition váltás és jelenik meg hogyan mód gombra.
-    Írja le a tartalmat, amely megjelenik a (Akciógomb lehetővé teszi, hogy az alkalmazás, hogy egy adott műveletet, például az alkalmazás átirányítása egy áruházban vagy bármilyen típusú adatforrások, akkor megadhatja az weblapon megnyitása) gombra.
-    Írja be a tartalom, amely megjelenik a Kilépés gombra (a Kilépés gombra kattintva a webes nézet fog tűnni).
 
-    Eredmény
 
![WebView5][32]

-    A felhasználó megkapja az értesítést, és kattintson rá.
-    A szöveg nézet engedélyezése a felhasználók kezelhetik előugró formában jelenik meg.

<!--Image references-->
[1]: ./media/mobile-engagement-how-tos/First1.png
[2]: ./media/mobile-engagement-how-tos/First2.png
[3]: ./media/mobile-engagement-how-tos/First3.png
[4]: ./media/mobile-engagement-how-tos/First4.png
[5]: ./media/mobile-engagement-how-tos/First5.png
[6]: ./media/mobile-engagement-how-tos/First6.png
[7]: ./media/mobile-engagement-how-tos/First7.png
[8]: ./media/mobile-engagement-how-tos/Test1.png
[9]: ./media/mobile-engagement-how-tos/Test2.png
[10]: ./media/mobile-engagement-how-tos/Test3.png
[11]: ./media/mobile-engagement-how-tos/Personalize1.png
[12]: ./media/mobile-engagement-how-tos/Personalize2.png
[13]: ./media/mobile-engagement-how-tos/Personalize3.png
[14]: ./media/mobile-engagement-how-tos/Personalize4.png
[15]: ./media/mobile-engagement-how-tos/Differentiate1.png
[16]: ./media/mobile-engagement-how-tos/Differentiate2.png
[17]: ./media/mobile-engagement-how-tos/Differentiate3.png
[18]: ./media/mobile-engagement-how-tos/Schedule1.png
[19]: ./media/mobile-engagement-how-tos/Schedule2.png
[20]: ./media/mobile-engagement-how-tos/Schedule3.png
[21]: ./media/mobile-engagement-how-tos/TextView1.png
[22]: ./media/mobile-engagement-how-tos/TextView2.png
[23]: ./media/mobile-engagement-how-tos/TextView3.png
[24]: ./media/mobile-engagement-how-tos/TextView4.png
[25]: ./media/mobile-engagement-how-tos/TextView5.png
[26]: ./media/mobile-engagement-how-tos/TextView6.png
[27]: ./media/mobile-engagement-how-tos/TextView7.png
[28]: ./media/mobile-engagement-how-tos/WebView1.png
[29]: ./media/mobile-engagement-how-tos/WebView2.png
[30]: ./media/mobile-engagement-how-tos/WebView3.png
[31]: ./media/mobile-engagement-how-tos/WebView4.png
[32]: ./media/mobile-engagement-how-tos/WebView5.png

<!--Link references-->
[Link 1]: mobile-engagement-user-interface.md
[Link 2]: mobile-engagement-troubleshooting-guide.md
[Link 3]: mobile-engagement-how-tos.md
[Link 4]: http://go.microsoft.com/fwlink/?LinkID=525553
[Link 5]: http://go.microsoft.com/fwlink/?LinkID=525554
[Link 6]: http://go.microsoft.com/fwlink/?LinkId=525555
[Link 7]: https://account.windowsazure.com/PreviewFeatures
[Link 8]: https://social.msdn.microsoft.com/Forums/azure/home?forum=azuremobileengagement
[Link 9]: http://azure.microsoft.com/services/mobile-engagement/
[Link 10]: http://azure.microsoft.com/documentation/services/mobile-engagement/
[Link 11]: http://azure.microsoft.com/pricing/details/mobile-engagement/
[Link 12]: mobile-engagement-user-interface-navigation.md
[Link 13]: mobile-engagement-user-interface-home.md
[Link 14]: mobile-engagement-user-interface-my-account.md
[Link 15]: mobile-engagement-user-interface-analytics.md
[Link 16]: mobile-engagement-user-interface-monitor.md
[Link 17]: mobile-engagement-user-interface-reach.md
[Link 18]: mobile-engagement-user-interface-segments.md
[Link 19]: mobile-engagement-user-interface-dashboard.md
[Link 20]: mobile-engagement-user-interface-settings.md
[Link 21]: mobile-engagement-troubleshooting-guide-analytics.md
[Link 22]: mobile-engagement-troubleshooting-guide-apis.md
[Link 23]: mobile-engagement-troubleshooting-guide-push-reach.md
[Link 24]: mobile-engagement-troubleshooting-guide-service.md
[Link 25]: mobile-engagement-troubleshooting-guide-sdk.md
[Link 26]: mobile-engagement-troubleshooting-guide-sr-info.md
[Link 27]: ../mobile-engagement-how-tos-first-push.md
[Link 28]: ../mobile-engagement-how-tos-test-campaign.md
[Link 29]: ../mobile-engagement-how-tos-personalize-push.md
[Link 30]: ../mobile-engagement-how-tos-differentiate-push.md
[Link 31]: ../mobile-engagement-how-tos-schedule-campaign.md
[Link 32]: ../mobile-engagement-how-tos-text-view.md
[Link 33]: ../mobile-engagement-how-tos-web-view.md
 
