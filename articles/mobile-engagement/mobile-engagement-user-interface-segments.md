<properties 
   pageTitle="Azure mobil tetszés szerint elmélyedhet felhasználói felület - szakaszokat" 
   description="Megtudhatja, hogy miként hozhat létre és kezelhet a felhasználók azonosítása használatát mintát Azure Mobile tetszés szerint elmélyedhet szegmensek" 
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

# <a name="how-to-create-and-manage-segments-of-users-to-identify-usage-patterns"></a>Hogyan hozhat létre, és a felhasználók azonosításához használatát szegmensek kezelése

Ez a cikk ismerteti a **Mobile tetszés szerint elmélyedhet** portál **SZEGMENSEK** lapja. A **Mobil tetszés szerint elmélyedhet** portal segítségével figyelheti és kezelheti a mobilalkalmazások.

A felhasználói felület szegmensek szakasza lehetővé teszi a felhasználóknak a különböző viselkedéséről és érheti el az alkalmazás és a szegmensek API keresztül is elérheti analytics alapján példájaként használata. Szegmensek először vannak számított 24 óra után a létrehozásuk, és azok is recomputed 24 óránként legújabb analytics adatok alapján. Miután egy szakasz kiszámítása, megjeleníti "Day nap előzmények" diagram minden nap.


>[AZURE.NOTE] A **Mobil tetszés szerint elmélyedhet** portál felhasználói felület sok szakaszt tartalmaz a **SÚGÓ megjelenítése** gombra. Nyomja le az erre a gombra kattintva egy szakasz környezetfüggő ismertetése.

## <a name="create-segments"></a>Szegmensek létrehozása
A szakasz legfeljebb 10 feltételek alapján meghatározott időtartamra a felfelé az elmúlt 60 napig analytics szakaszából hozhat létre. Ha például a személyeket, akik megtekinthetők az egyes lapokat, vagy az adott tartalmat az alkalmazás az utolsó 10 napon belül szeretne keresni egy szegmensre is létrehozhat. Ezt az információt a analytics szakasz érhető el. Igen akkor vele hozzon létre egy szegmenst, és állítsa be ezen felhasználók részhalmazát célba leküldéses értesítést, hogy az illető visszatér az alkalmazás. 
 
> Megjegyzés: Ha egy szakasz kiszámításához nem lehet szerkeszteni; Ez csak klónozva (másolt) vagy semmisíteni (törölt). A szakasz is klónozható (ha az azonos AppID) ugyanarra alkalmazásból, és azt is is klónozható más alkalmazásokba (a különböző AppID). 
 
 ![segments1][35] 

## <a name="examples-segments"></a>Példák szegmensek
 ![segments2][36]

Szegmensek oszthatja fel az alkalmazás a végfelhasználók teszi lehetővé.
A felhasználók példájaként egy fontos marketingtevékenység stratégia. Azure Mobile tetszés szerint elmélyedhet lehetővé teszi a visszamenőleges adatok beolvasása és egyéni szegmensek létrehozása. A hatékony eszköz lehetővé teszi, hogy az alkalmazás ügyfelei számára. Egyszerűen elemzése a szakaszokat, és a szakaszok használhatók leküldéses célként.
Gyakori használatieset, hogy a leküldéses értesítést ösztönzése az alkalmazás az áruházban értékeljék a végfelhasználók elküldeni kívánt. Értesítést küld a végfelhasználók számára, hanem csak a felhasználók, hogy az alkalmazás minden nap használtak az elmúlt egy hónapban és kellett volna egy nagy felhasználói felület adja meg, az szegmens hozhat létre. Kevesebb, jól célzott leküldéses értesítések küldésére kap egy jobb Megtérülési.
 
 ![segments3][37]

### <a name="segments-you-can-create-based-on-the-major-azure-mobile-engagement-elements"></a>Szakaszokat is létrehozhat fő Azure Mobile tetszés szerint elmélyedhet elemek alapján:
- Esemény: hozzon létre egy szakasz célokat egy adott esemény az alkalmazás, amely kétszer több, mint a hét történt. 
- Munkamenet: hozzon létre egy még az alkalmazás a múlt héten több, mint 5 megszorzása használt felhasználói szegmens.
- Tevékenység: hozzon létre egy szakaszában használt egy lap vagy a tartalom több vagy kevesebb mint 10 idő múlt hónapban felhasználók.
- Feladat: hozzon létre egy szakaszában, amely több mint kétszer nap befejeződött egy feladat felhasználók.
- Összeomlik: hozzon létre egy szakasz összes a felhasználók e az összeomlást a múlt héten legfeljebb 10 hányszor volt. (Ez a szakasz-Bocsánatkérő vagy akár egy szelvényidőszak sikerült leküldéses!)
- Hiba: hozzon létre egy szakaszában kellett volna hiba legfeljebb 100 időpontok utóbbi 3 napból összes felhasználót.
- App útmutatók: hozzon létre egy szakasz függvénynél egy egyéni alkalmazás információ, amely az utolsó 25 napban történt.
 
 ![segments4][38]

A szakasz optimálisan használatához kell elvégezte a egy testre szabott integráció a SDK az alkalmazásban a címkézési csomaggal "App útmutatók" címkék.
Kattintson a kezdőlap lapra, a felület, jelölje ki azt az alkalmazást, majd kattintson a "Szegmensek" szakasz a.

1. Válassza ki a "Szegmensek" szakaszt.
2. Kattintás az "új szakasz" a gombra kattintva hozzon létre egy új szegmenst.

## <a name="real-life-example-create-a-simple-segment-based-on-session-information"></a>Valós leírási_idő példa: Hozzon létre egy egyszerű szakasz "Munkamenet" adatai alapján
Hozzon létre egy szakasz összes a végfelhasználók használt van az alkalmazás legalább 50 alkalommal a múlt héten. Itt találja a csak a végfelhasználók számára, amely legalább 30 másodperc minden munkamenetben az alkalmazásban eltöltött. Ez az alkalmazás egy pozitív élmény rendelkező összes végfelhasználók jelennek meg. Ezután a létrehozott szakasz használatával leküldéses értesítést e kérje meg, hogy az alkalmazás az áruházban értékelése a végfelhasználók számára.
 
 ![segments5][39]

1. Nevezze el a szakasz annak érdekében, hogy gyorsan keresse meg a "Szakasz" listában.
2. Kattintson a "Létrehozása" gombra.
 
 ![segments6][40]

Jelölje ki a munkamenetet.
 
 ![segments7][41]

1. Jelölje be a "Múlt héten" időtartamát.
2. Kattintson a Tovább gombra.
 
 ![segments8][42]

1. Jelölje ki a kapcsolódó között a listában szereplő: =; ≥, ≤.
2. Adja meg a kívánt számát.
3. Jelölje ki a kívánt előfordulás. 
4. Kattintson a Tovább gombra.
Ez a példa van állítva, a múlt héten fölé, hogy megfeleljen legalább 50 munkamenetek végzett felhasználók.
 
 ![segments9][43]

A munkamenet szegmens választhat a hossz minden feltételként munkamenetben.

1. Az operátornak a kiválasztása a listából.
2. Adja meg az időtartam minden munkamenetben.
3. Kattintson a Tovább gombra.
Ebben a példában azt észleli, hogy az összes munkamenet fölé, hogy van már szegmentált előfordulás szakasznál, jelölje ki csak azokat a felhasználókat, hogy minden munkamenetben legfeljebb 30 másodperc eltöltött.
 
 ![segments10][44]

Nevezze el a kritérium annak érdekében, hogy megtalálja a a teljes feltérképezése, és kattintson a Befejezés gombra.
 
 ![segments11][45]

Amikor befejezte a beállítás a kritériumnak, meg kíván jeleníteni a szakasz feltérképezése.
A szakasz analytics-adatok alapján, mert szegmensek naponta egyszer vannak számított.
Ebben a példában 47,7 %-át a teljes végfelhasználók egyezik a feltétel. A felhasználók, akik egy jó élmény kellett volna, és valószínűsíthetően nyújtanak az értékelheti a alkalmazást az áruházban érdeklődje meg értesítés leküldéses őket egy újabb minősítést kell őket.


## <a name="see-also"></a>Lásd még:

- [Fogalmak][Link 6]
- [Útmutató a szolgáltatás hibáinak elhárítása][Link 24]

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
[Link 27]: ../mobile-engagement-how-tos-first-push.md
[Link 28]: ../mobile-engagement-how-tos-test-campaign.md
[Link 29]: ../mobile-engagement-how-tos-personalize-push.md
[Link 30]: ../mobile-engagement-how-tos-differentiate-push.md
[Link 31]: ../mobile-engagement-how-tos-schedule-campaign.md
[Link 32]: ../mobile-engagement-how-tos-text-view.md
[Link 33]: ../mobile-engagement-how-tos-web-view.md
 
