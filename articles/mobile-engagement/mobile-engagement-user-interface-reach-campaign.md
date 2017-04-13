<properties 
   pageTitle="Azure mobil tetszés szerint elmélyedhet felhasználói felület - vannak marketingkampány" 
   description="Laern hogyan hozhat létre és kezelhet leküldéses értesítést kampányokra Azure Mobile tetszés szerint elmélyedhet használatával" 
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


# <a name="how-to-create-and-manage-push-notification-campaigns"></a>Hogyan lehet létrehozni és kezelni a leküldéses értesítést kampányok
A felhasználói felület vannak szakasza használatával hozzon létre egy új leküldéses marketingkampány egy összetett képletet tartalmazó azáltal, hogy a leküldéses értesítést küldjön kell minden információt. A beállítások leküldéses kampányok kissé eltérő lehet attól függően, hogy a négy marketingkampány-típusok: hirdetmények, felmérésekre, adatok verembe küldi és Csempék (csak a Windows Phone).

### <a name="option-applies-to"></a>A beállítás vonatkozik:
- Nyelv: Az összes (hirdetmények, felmérésekre, adatok veremmutatót, Csempék)
- Marketingkampány: Az összes (hirdetmények, felmérésekre, adatok veremmutatót, Csempék)
- Értesítés: Hirdetmények, szavazásokra
- Tartalom: Egyedi az egyes marketingkampány:
- Célközönség: Az összes (hirdetmények, felmérésekre, adatok veremmutatót, Csempék)
- Időkereten: hirdetmények, felmérésekre, Csempék
- Teszt: Az összes (hirdetmények, felmérésekre, adatok veremmutatót, Csempék)
 
![Vannak-Campaign1][20]

## <a name="languages"></a>Nyelvek
A nyelv legördülő menü segítségével küldje el a leküldéses egy másik verzióját eszközök, amelyek több nyelv használata. Alapértelmezés szerint minden eszköz kap az azonos leküldéses, függetlenül attól, mely nyelveken megfelelőek-e használni. Az eszköz beállítása más nyelven rendelkező felhasználók kap az alapértelmezett nyelve a leküldéses. A leküldéses marketingkampány beállítások közül számos lehetővé teszi, hogy az egyes lehetőséget választja, a további nyelvek alternatív tartalom meghatározása. 
 
![Vannak-Campaign2][21]

### <a name="language-differences-apply-to"></a>Nyelvi különbségek vonatkoznak:
- Nyelv: Egyedi nyelvek jelölhető ki az alapértelmezett nyelv mellett
- Marketingkampány: Az összes nyelvhez azonos
- Értesítés: Egyedi az alapértelmezett nyelv mellett az egyes nyelvekhez
- Tartalom: Egyedi az alapértelmezett nyelv mellett az egyes nyelvekhez
- Célközönség: Egy külön nyelvi feltétel szerint szűrve lehetnek
- Időkereten: ugyanazt az összes nyelvhez
- Teszt: Előfordulhat, hogy el lehet küldeni a minden nyelv egyszerre
 
### <a name="supported-languages"></a>A támogatott nyelvek:
- Arab (ar) 
- Bolgár (bg) 
- Katalán (ca) 
- Kínai (zh) 
- Horvát (ó) 
- Czech (CS) 
- Dán (da) 
- Holland (Hollandia) 
- Angol (en) 
- Finn (fi) 
- Francia (fr) 
- Német (de) 
- Görög (el) 
- Héber (automatikus) 
- Hindi (elrejtése) 
- A magyar (hu) 
- Indonéz (azonosító) 
- Olasz () 
- Japán (ja) 
- Koreai (ko) 
- Lett (lv) 
- Litván (lt) 
- Maláj (macrolanguage) (ms) 
- Norvég (Bokmål) (Megjegyzés) 
- Lengyel (pl) 
- Portugál (pt) 
- Román (ro) 
- Orosz (licencelési) 
- Szerb (sr) 
- Szlovák (sk) 
- Szlovén (sl) 
- Spanyol (es) 
- Svéd (ÜKtgE) 
- Tagalog (tl) 
- Thai (s) 
- Török (tr) 
- Ukrán (Egyesült Királyság) 
- Vietnami (vi) 
 
## <a name="campaign"></a>Marketingkampány
A marketingkampány szakasz segítségével a tár nevét és a marketingkampány, valamint a kategória, mintha figyelmen kívül hagyja a leküldéses kampányok a célközönség csoportban, és a elérjen API-val (és az alacsony szinten leküldéses API elemeket) keresztül a marketingkampány inkább küldeni szeretné. A kategóriák használható az egyéni értesítés sablonnal a vezérlőelem-app értesítések előre megadott beállítások alapján. A kategóriák listájában, a meglévő"" a elérjen API keresztül is elérheti.

> Figyelmeztetés: Ha a "Figyelmen kívül hagyása célközönséget, leküldéses küld az API-t a felhasználók" beállítás a gyermekektől marketingkampány "Marketingkampány" szakaszában a marketingkampány automatikusan nem küld, szüksége lesz manuális küldése a elérjen API keresztül.
 
![Vannak-Campaign3][22]
 
### <a name="option-applies-to"></a>A beállítás vonatkozik:
- Név: az összes
- Kategória: Hirdetmények, szavazásokra
- Figyelmen kívül hagyja a közönségnek, leküldéses küld az API-t a felhasználók: minden
 
## <a name="notification"></a>Értesítés
Az értesítés szakasz használatával a leküldéses többek között az alapvető beállítások megadása: a leküldéses, az üzenetet, az alkalmazás képet, a cím, vagy ha dismissible. A platform, az eszköz a sok értesítési beállítások vonatkoznak. Választhat, hogy a leküldéses küld "alkalmazás" vagy "kicsinyítése alkalmazás" vagy mindkettőt. (Ne feledje, hogy a felhasználók lehet "részvétel" vagy "letiltásra", "kívül alkalmazás" verembe küldi, az operációs rendszer szintű az eszközön, és Azure Mobile tetszés szerint elmélyedhet nem tudnak felülbírálása ezt a beállítást. Ne feledje, hogy a elérjen API "alkalmazásban" kezeli, és ezt "alkalmazás out" nyújtja. A leküldéses API használható "ki letölthető" veremmutatót is kezelheti.) Veremmutatót testre szabható a képek vagy HTML-tartalmat, például az alkalmazás-Ön kívül vagy az alkalmazás (Android SDK 2.1.0 vagy újabb leképezési kategóriák kötelező) másik helyére csatolható mély mutató hivatkozásokat. A ikonra, és az iOS jelvény módosíthatja, és küldje el a szöveg vagy a webhely tartalma (HTML-tartalom URL-címe hivatkozásra egy másik helyre belüli vagy kívüli az alkalmazás előugró). Győződjön meg az Android-eszközökhöz csengetés vagy a leküldéses rezgés is. (Ne feledje, hogy szüksége lesz a megfelelő SDK az engedélyek az Android-fájl gyűrű vagy egy eszközt rezgés cikkét.) Jelenleg nem iparágban szokásos méretű Android "Áttekintés", mivel képernyő méretű különböző minden eszközön, de 400-x 100 képek munkát szinte bármilyen képernyő méretét a.

### <a name="delivery-types"></a>Kézbesítési típusok:
-    Ki csak letölthető: az értesítés, amikor a felhasználó nem használja az alkalmazás kerülnek.
- Az alkalmazás csak értesítés elévült igényel egy tanúsítványt az Apple vagy a Google (APNS vagy GCM tanúsítvány).
- A-app csak: az értesítés jelenik meg, csak ha az alkalmazást futtató.
- Az értesítés Capptain kézbesítési rendszert használ, a felhasználó eléréséhez. Teljesen testre szabhatja a leküldéses vizuális elrendezés/megjelenítésének.
- Bármikor: Ezt a beállítást biztosítja, vagy az alkalmazás fut-e értesítést küldött.

 
![Vannak-Campaign4][23]

### <a name="option-applies-to"></a>A beállítás vonatkozik:
- Értesítés: Hirdetmények, szavazásokra
 
## <a name="content"></a>Tartalom
A tartalomtípusok szakasz segítségével módosíthatja a hirdetmények, felmérésekre, adatok verembe küldi és Csempék (csak a Windows Phone) tartalma. A tartalom leküldéses kampányok értéke adott marketingkampány típusára. 

### <a name="see-also"></a>Lásd még:
- [Felhasználói felület dokumentáció - elérjen - tartalom leküldéses][Link 29]
 
![Vannak-Campaign5][24]

## <a name="audience"></a>A célközönség
A célközönség csoportban segítségével szabványos elemlistán korlátozhatja a marketingkampány vagy a testre szabott feltétel alapján a marketingkampány korlátozások megadása. A szokásos beállítási lehetőség, korlátozhatja a közönség lehetővé teszi, hogy új vagy a régi vagy csak a natív leküldéses felhasználóknak leküldéses. Is beállíthatja, hogy a felhasználók, akik a leküldéses fogadása számának korlátozása kvóta. A kifejezés hogyan a marketingkampány szűrt tartalmaz egy vagy több feltétel cél felhasználók kézzel szerkeszthető. Írja be a közönség kifejezés manuálisan. Ilyen kifejezés explicit módon meg kell határoznia feltétel közötti kapcsolatot. Az azonosító nagybetűvel kell kezdődnie, és nem tartalmazhat szóközt írja le egy adott határértéknél. A feltétel közötti kapcsolatot használ kell leírni "és", "vagy", "nem" operátorok, valamint a '(",")'. Példa: "Criterion1 vagy (Criterion1 és nem Criterion2)".

> Megjegyzés: Nagyobb közönség számára kampányok szerepel, a beolvasás kiválasztásával kiszolgálóoldali lassú lehet, különösen akkor, ha egyszerre több kampányok indításához megpróbálja.

- Ha lehetséges csak indítsa el a több marketingkampány egyszerre.
- A legtöbb csak indítsa el a négy kampányok egyszerre.
- Csak az aktív felhasználók a leküldéses (jelölőnégyzet "folytasson csak olyan felhasználók, akik is natív leküldéses érhető el" és "csak az aktív felhasználók"), hogy csak a felhasználók továbbra is telepítve van az alkalmazás és használni kell ellenőrizni.
Miután a közönség van megadva, a simulate gomb segítségével megtudhatja, hogy hány felhasználója fog kapni a leküldéses. Ez lesz (Ez a felhasználók véletlenszerűen minta alapján becslést) célközönség esetleg célként ismert felhasználók számát számítja ki. Ügyeljen arra, hogy a felhasználók, akik az alkalmazás eltávolítása után is célközönség részét képezik, de nem érhető el.

### <a name="see-also"></a>Lásd még:
- [Felhasználóifelület - vannak - dokumentáció új leküldéses feltétel][Link 28]

![Vannak-Campaign6][25]

### <a name="edit-expression"></a>Kifejezés szerkesztése
![Vannak-Campaign7][26]
 
### <a name="limit-your-audience-option-applies-to"></a>A célközönség beállítás vonatkozik korlát:
- Felhasználók csak egy részhalmazát folytatni: az összes (hirdetmények, felmérésekre, adatok verembe küldi, Csempék)
- Csak a régi felhasználók folytatni: az összes (hirdetmények, felmérésekre, adatok verembe küldi, Csempék)
- Csak az új felhasználók folytatni: az összes (hirdetmények, felmérésekre, adatok verembe küldi, Csempék)
- Csak az üresjárati felhasználók folytatni: hirdetmények, felmérésekre, Csempék
- Csak az aktív felhasználók folytatni: az összes (hirdetmények, felmérésekre, adatok verembe küldi, Csempék)
- Csak olyan felhasználók, akik a belső leküldéses használatával érhető el folytatni: hirdetmények, szavazásokra
 
## <a name="time-frame"></a>Időkereten
Az időkeret szakasz használatával állítja be, amikor a leküldéses küld vagy üresen is hagyhat időkereten a marketingkampány azonnali indításához. Ne feledje, hogy használata a végfelhasználók számára időzóna kezdheti el a marketingkampány nap, mint a végfelhasználók Ázsiában várhatók a rendszergazda, és küldje el a kisebb kötegekben veremmutatót mindaddig, amíg az ügyfélgép összes időzóna megadása a marketingkampány időkereten felel meg egyszerre a korábbi verziójában. A végfelhasználók időzóna használatával is okozhatják késések kampányok mivel azt az időt kérése a telefonról a leküldéses megkezdése előtt.

> Megjegyzés: Kampányokra nélkül is gyorsítótár-befejezési dátumának veremmutatót helyi meghajtóra, és továbbra is megjelenítheti őket Miután manuálisan teljes kampányok. Ennek elkerülése érdekében, adott kampányok záró időpontja.

### <a name="see-also"></a>Lásd még:
- [Elérjen – hogyan távközlési – ütemezése][Link 3] 
 
![Vannak-Campaign8][27]

### <a name="settings-apply-to"></a>A beállítások vonatkoznak:
- Időkereten: hirdetmények, felmérésekre, Csempék
 
## <a name="test"></a>Teszt
A próba szakasz segítségével küldje el a leküldéses a marketingkampány mentése előtt próba eszközére. Ha minden egyéni nyelvek a marketingkampány állította be, az egyes nyelvek tesztelheti a leküldéses. "Saját fiókból" próba eszköz beállítása
> Megjegyzés: A nincsenek adatok be van jelentkezve a gomb "tesztelése" használatakor kiszolgálóoldali verembe küldi, az adatok csak be van jelentkezve valós leküldéses kampányok.

### <a name="see-also"></a>Lásd még:
- [Felhasználói felület dokumentáció - fiókom][Link 14]
 
![Vannak-Campaign9][28]

<!--Image references-->
[1]: ./media/mobile-engagement-user-interface-navigation/navigation1.png
[2]: ./media/mobile-engagement-user-interface-home/home1.png
[3]: ./media/mobile-engagement-user-interface-home/home2.png
[4]: ./media/mobile-engagement-user-interface-home/home3.png
[5]: ./media/mobile-engagement-user-interface-home/home4.png
[6]: ./media/mobile-engagement-user-interface-home/home5.png
[7]: ./media/mobile-engagement-user-interface-my-account/myaccount1.png
[8]: ./media/mobile-engagement-user-interface-my-account/myaccount2.png
[9]: ./media/mobile-engagement-user-interface-my-account/myaccount3.png
[10]: ./media/mobile-engagement-user-interface-analytics/analytics1.png
[11]: ./media/mobile-engagement-user-interface-analytics/analytics2.png
[12]: ./media/mobile-engagement-user-interface-analytics/analytics3.png
[13]: ./media/mobile-engagement-user-interface-analytics/analytics4.png
[14]: ./media/mobile-engagement-user-interface-monitor/monitor1.png
[15]: ./media/mobile-engagement-user-interface-monitor/monitor2.png
[16]: ./media/mobile-engagement-user-interface-monitor/monitor3.png
[17]: ./media/mobile-engagement-user-interface-monitor/monitor4.png
[18]: ./media/mobile-engagement-user-interface-reach/reach1.png
[19]: ./media/mobile-engagement-user-interface-reach/reach2.png
[20]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign1.png
[21]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign2.png
[22]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign3.png
[23]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign4.png
[24]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign5.png
[25]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign6.png
[26]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign7.png
[27]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign8.png
[28]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign9.png
[29]: ./media/mobile-engagement-user-interface-reach-criterion/Reach-Criterion1.png
[30]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content1.png
[31]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content2.png
[32]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content3.png
[33]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content4.png
[34]: ./media/mobile-engagement-user-interface-dashboard/dashboard1.png
[35]: ./media/mobile-engagement-user-interface-segments/segments1.png
[36]: ./media/mobile-engagement-user-interface-segments/segments2.png
[37]: ./media/mobile-engagement-user-interface-segments/segments3.png
[38]: ./media/mobile-engagement-user-interface-segments/segments4.png
[39]: ./media/mobile-engagement-user-interface-segments/segments5.png
[40]: ./media/mobile-engagement-user-interface-segments/segments6.png
[41]: ./media/mobile-engagement-user-interface-segments/segments7.png
[42]: ./media/mobile-engagement-user-interface-segments/segments8.png
[43]: ./media/mobile-engagement-user-interface-segments/segments9.png
[44]: ./media/mobile-engagement-user-interface-segments/segments10.png
[45]: ./media/mobile-engagement-user-interface-segments/segments11.png
[46]: ./media/mobile-engagement-user-interface-settings/settings1.png
[47]: ./media/mobile-engagement-user-interface-settings/settings2.png
[48]: ./media/mobile-engagement-user-interface-settings/settings3.png
[49]: ./media/mobile-engagement-user-interface-settings/settings4.png
[50]: ./media/mobile-engagement-user-interface-settings/settings5.png
[51]: ./media/mobile-engagement-user-interface-settings/settings6.png
[52]: ./media/mobile-engagement-user-interface-settings/settings7.png
[53]: ./media/mobile-engagement-user-interface-settings/settings8.png
[54]: ./media/mobile-engagement-user-interface-settings/settings9.png
[55]: ./media/mobile-engagement-user-interface-settings/settings10.png
[56]: ./media/mobile-engagement-user-interface-settings/settings11.png
[57]: ./media/mobile-engagement-user-interface-settings/settings12.png
[58]: ./media/mobile-engagement-user-interface-settings/settings13.png

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
[Link 27]: mobile-engagement-user-interface-reach-campaign.md
[Link 28]: mobile-engagement-user-interface-reach-criterion.md
[Link 29]: mobile-engagement-user-interface-reach-content.md
 
