<properties 
   pageTitle="Azure mobil tetszés szerint elmélyedhet felhasználói felület - vannak tartalom" 
   description="Útmutató: a leküldéses értesítést kampányok Azure Mobile tetszés szerint elmélyedhet a különböző típusú egyedi tartalom kezelése" 
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

# <a name="how-to-manage-the-unique-content-of-the-different-types-of-push-notification-campaigns"></a>A leküldéses értesítést kampányok különböző típusú egyedi tartalom kezelése
 
A tartalom szakasz új vannak kampányok segítségével módosíthatja a hirdetmények, felmérésekre, adatok verembe küldi és Csempék (csak a Windows Phone) tartalma. A tartalom leküldéses kampányok értéke adott marketingkampány típusára. 
 
### <a name="content-types"></a>Tartalomtípusok:
- Hirdetmények
- Szavazásokra
- Adatok veremmutatót
- Csempék (csak a Windows Phone)
 
## <a name="content-of-announcements"></a>Hirdetmények tartalma
 ![Vannak-Content1][30] 

### <a name="choose-the-type-of-your-announcement"></a>Válassza ki a bejelentése típusát:
-    Csak az értesítés: egyszerű szokásos értesítés. Azt jelenti, hogy ha a felhasználó kattint, nincs további nézet jelenik meg, de csak a hozzá tartozó művelet akkor fordul elő.
-    Szöveg bejelentése: értesítést, amely a felhasználót, hogy a szöveg nézet figyelmébe folytat.
-    Webes bejelentése: értesítést, amely a felhasználónak szeretne adni egy pillantást webes nézet folytat.

### <a name="see-also"></a>Lásd még:
- [Elérjen – hogyan távközlési - hirdetmények][Link 3] 

### <a name="about-web-view-announcements"></a>Tudnivalók a webes közlemények megtekintése:
A HTML-kód vagy JavaScript-kód, itt a "{deviceid}" mintát előfordulását automatikusan váltja fel az azonosító az eszköz a közlemény megjelenítéséhez. Az egyszerűen lekérése az Azure Mobile tetszés szerint elmélyedhet eszköz azonosítók egy külső webszolgáltatás az irodában is.
Ha szeretne (nélkül az alapértelmezett művelet- és kilépési gombok elvégezheti a szükséges) web teljes képernyős nézet létrehozása a JavaScript-kód a webes megtekintése bejelentése a következő függvények használhatók: 

-    hirdetmények művelet végrehajtása: ReachContent.actionContent()
-    az értesítés onnan: ReachContent.exitContent()
 
### <a name="choose-your-action"></a>Választási lehetőségek:

### <a name="about-action-urls"></a>A művelet URL-címek:
A célzott eszköz operációs rendszer által értelmezhető kívánt URL-cím is alkalmazható egy művelet URL-címe.
Bármelyik kijelölt URL-címet, az alkalmazás előfordulhat, hogy támogató (például, hogy a felhasználók adott képernyőre ugorhat) is is alkalmazható egy művelet URL-címe.
Az {deviceid} szabályt minden előfordulását automatikusan helyettesíti be az eszközt, a művelet végrehajtása azonosítója. Ez egyszerűen lekérése az Azure Mobile tetszés szerint elmélyedhet eszköz azonosítók egy külső web Service az irodában is használható.

- **Android-alapú + iOS műveletek**
    - Nyissa meg az weblapon
    - http://\[web-webhely-tartomány\] 
    - Példa: http://www.azure.com
    - E-mail küldése
    - mailto:\[– címzett\]?subject =\[Tárgy\]& szervezet =\[üzenet\] 
    - Example:mailto:foo@example.com?subject=Greetings%20from%20Azure%20Mobile%20Engagement!&body=Good%20stuff!
    - Egy Szöveges üzenet beállítása
    - az SMS:\[telefonszám\] 
    - Példa: sms:2125551212
    - Telefonszám tárcsázása
    - Tel:\[telefonszám\] 
    - Példa: tel:2125551212
- **Android csak műveletek**
    - A Play áruház-alkalmazás letöltése
    - Market://details?id=\[alkalmazáscsomag\] 
    - Példa: market://details?id=com.microsoft.office.word
    - Geo honosított keresés indítása
    - GEO:0, 0?q =\[keresési lekérdezés\] 
    - Példa: geo:0, 0? a kérdések starbucks, Párizs =
- **csak iOS-műveletek**
    - Az alkalmazás-áruházból alkalmazás letöltése
    - http://iTunes.apple.com/ [Ország] [alkalmazás neve] /app/ /id [alkalmazás azonosítója]? mt = 8 
    - Példa: http://itunes.apple.com/fr/app/briquet-virtuel/id430154748?mt=8
    - Windows-műveletek
    - Nyissa meg az weblapon
    - http://\[web-webhely-tartomány\] 
    - Példa: http://www.azure.com
    - E-mail küldése
    - mailto:\[– címzett\]?subject =\[Tárgy\]& szervezet =\[üzenet\] 
    - Example:mailto:foo@example.com?subject=Greetings%20from%20Azure%20Mobile%20Engagement!&body=Good%20stuff!
    - Egy Szöveges (Skype Áruházbeli alkalmazás szükséges) üzenet
    - az SMS:\[telefonszám\] 
    - Példa: sms:2125551212
    - Telefonszám (Skype Áruházbeli alkalmazás szükséges)
    - Tel:\[telefonszám\] 
    - Példa: tel:2125551212
    - A Play áruház-alkalmazás letöltése
    - MS-windows-áruházból: PDP?PFN =\[alkalmazás csomag azonosítója\] 
    - Példa: ms-windows-tár: a projektinformációs lapon? PFN = 4d91298a-07cb-40fb-aecc-4cb5615d53c1
    - Bingmaps keresés indítása
    - bingmaps:?q =\[keresési lekérdezés\] 
    - Példa: bingmaps:? a kérdések starbucks, Párizs =
    - Egy egyéni színsémát használata
    - \[Egyéni színséma\]://\[egyéni színsémát paraméterek\] 
    - Példa: myCustomProtocol://myCustomParams
    - A csomag adatok (Áruházbeli alkalmazás bővítmény, olvassa el a szükséges) használata
    - \[mappa\]\[adatok\]. \[bővítmény\] 
    - Example:myfolderdata.txt
 
### <a name="build-a-tracking-url"></a>A nyomon követés URL-cím létrehozása:
-    "Beállítások" című szakaszában talál a <UI Documentation> vonatkozó útmutatás épület nyomon követés URL-címet, amely lehetővé teszi a felhasználóknak, hogy egy másik alkalmazás letöltése.
 
### <a name="define-the-texts-of-your-announcement"></a>A bejelentése szövegei meghatározása
Töltse ki a cím, a tartalom és a gomb a bejelentése szövegét. Megcélozhat, hogy a felhasználók hogyan a marketingkampány válaszolt a gyermekektől visszajelzései alapján jövőbeli kampányok közönségnek. Célközönség-e a marketingkampány lett csak tolni, megválaszolt, actioned vagy kilép a visszajelzését alapulhatnak.

### <a name="see-also"></a>Lásd még:
- [Felhasználóifelület - vannak - dokumentáció új leküldéses feltétel][Link 28]

## <a name="content-of-polls"></a>Szavazásokra tartalma
![Vannak-Content2][31] töltse ki a cím, leírás és a bejelentése gomb szövegét. Ezután vegyen fel kérdéseket és a kérdésekre adott válaszok lehetőségeit.
Megcélozhat, hogy a felhasználók hogyan a marketingkampány válaszolt a gyermekektől visszajelzései alapján jövőbeli kampányok közönségnek. Célközönség-e a marketingkampány lett csak tolni, megválaszolt, actioned vagy kilépett is alapul. Célközönség kiválasztásával is szavazás answer visszajelzést, ahol a kérdés és válasz választási lehetőségek vannak feltételként alapul.

### <a name="see-also"></a>Lásd még:
- [Felhasználóifelület - vannak - dokumentáció új leküldéses feltétel][Link 28]
 
## <a name="content-of-data-pushes"></a>Adatok veremmutatót tartalma
![Vannak-Content3][32] 

### <a name="choose-the-type-of-your-data"></a>Válassza ki az adatok típusát:
- Szöveg
- Bináris adatokat
- Base64 adatok

### <a name="define-the-content-of-your-data"></a>A tartalom, az adatok megadása
- Ha bejelölte a leküldéses szöveg mellé, szöveg másolása és beillesztése a az "tartalom" mezőbe.
- Ha Ön által megadott adatok bináris vagy a base64 leküldéses, a "töltse fel a fájlt" gomb segítségével töltse fel a fájlt.
- Megcélozhat, hogy a felhasználók hogyan a marketingkampány válaszolt a gyermekektől visszajelzései alapján jövőbeli kampányok közönségnek. Célközönség-e a marketingkampány lett csak tolni, megválaszolt, actioned vagy kilépett is alapul.

### <a name="see-also"></a>Lásd még:
- [Felhasználóifelület - vannak - dokumentáció új leküldéses feltétel][Link 28]

## <a name="content-of-tiles-windows-phone-only"></a>A Csempék (csak a Windows Phone) tartalma
![Vannak-Content4][33]

### <a name="define-the-content-of-your-tile"></a>A mozaik tartalmának meghatározása
A mozaik tartalom az a szöveg, a mozaik, az alkalmazás Windows Phone-eszközök jelennek meg.
A mozaik leküldéses áll a Microsoft leküldéses értesítést szolgáltatás (MPNS) a Windows Phone natív leküldéses. A mozaik leküldéses típus csak akkor leküldéses nincs telepítve a választ, és így a közönség a jövőbeli kampányok nem hozható létre a csempe leküldéses marketingkampány eredmények. 

### <a name="see-also"></a>Lásd még:
- [API - vannak API - dokumentáció natív leküldéses][Link 4]

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
 
