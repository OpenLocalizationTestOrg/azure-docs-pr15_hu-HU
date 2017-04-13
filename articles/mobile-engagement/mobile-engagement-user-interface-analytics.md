<properties
   pageTitle="Azure mobil tetszés szerint elmélyedhet felhasználói felület - elemzés"
   description="Megtudhatja, hogy miként az Azure Mobile tetszés szerint elmélyedhet használatával alkalmazásokkal kapcsolatos korábbi adatok elemzése céljából"
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

# <a name="how-to-analyze-historical-data-about-your-application"></a>Hogyan lehet a alkalmazásról korábbi adatok elemzése céljából

Ez a cikk ismerteti a **Mobile tetszés szerint elmélyedhet** portál **ANALYTICS** lapja. A **Mobil tetszés szerint elmélyedhet** portal segítségével figyelheti és kezelheti a mobilalkalmazások. Fontos tudni, hogy a portálon használatának megkezdéséhez, először hozzon létre egy **Azure Mobile tetszés szerint elmélyedhet** fiókot.


A felhasználói felület Analytics szakasza az alkalmazás, amely frissül 24 óránként visszamenőleges adatok alapján összesített információt tartalmaz. A feltüntetett adatok célja a különböző irányítópultok, térképek, kör-vonal-sáv diagramok és rácsok tevődik össze. Az adatokat is lehet letölteni csv-fájlként. Ezek az információk ugyanazt a legtöbb érhető el a felhasználói felület Monitor részében valós idejű, és azt is elérheti az analitikai API-t.

>[AZURE.NOTE] A **Mobil tetszés szerint elmélyedhet** portál felhasználói felület sok szakaszt tartalmaz a **SÚGÓ megjelenítése** gombra. Nyomja le az erre a gombra kattintva egy szakasz környezetfüggő ismertetése.

## <a name="standard-and-custom-analytics"></a>Normál és egyéni Analytics

Azure Mobile tetszés szerint elmélyedhet egyszerű, szabványos az alkalmazásokat, amint az app integrálása a SDK TénylegesKöltség analitikus információt biztosít. Azure Mobile tetszés szerint elmélyedhet is lehetővé teszi a egyéni analytics további információkat gyűjthet, amelyet a végfelhasználók számára viselkedését. Ehhez az egyéni "címkék (alkalmazás adatai)", létrehozott **beállításokat** , hogy az Azure Mobile tetszés szerint elmélyedhet a további adatokat gyűjt meg címke terv létrehozása.



## <a name="analytics"></a>Elemző
- Irányítópult: Általános információk az új és beillesztené felhasználók és a trendek jeleníti meg.
- Felhasználók: Felhasználók azonosítjuk a eszközazonosítót: Ez az azonosító az egyes egyedi (egy új felhasználót is valójában egy új eszköz). A felhasználó tekintendő új a megadott időintervallum ha ő elvégezte a első munkamenet közben a időszakot. A felhasználó tekintendő, ha ő végrehajtotta legalább egy munkamenet közben az utóbbi 7 napból tartja meg. Az aktív felhasználók olyan felhasználók, legalább egy munkamenet egy adott időszak során végzett. Rendezheti, havi, heti, napi vagy óránkénti időszakhoz. Az összes a diagramok hasonló, de más funkciókat, például az alkalmazás verziójának szerinti szűrés, majd egy idő szerinti rendezés engedélyezi. A szokásos adatok integrálása a SDK által gyűjtött tartalmazza a következőt: vevők az aktív felhasználók, új felhasználó, a munkamenetek számát, hossza minden munkamenet, műszaki információkat szeretne kapni a ország helyi változók, hely, nyelvi carrier, eszközök, belső vezérlőprogram, hálózati (WIFI), az alkalmazás és a SDK verzióját használják. Ez az információ megtekintheti a monitor szakaszából valós időben.

> Megjegyzés: Az időszak alapuló a felhasználói beállításokat, a dátumot, egy felhasználóhoz, akinek a telefonhoz helytelenül megadott dátumot sikerült jelenjenek meg a megfelelő időszakon belül.

- Adatmegőrzési: A felhasználó van tekinteni megmarad a megadott időtartamot, ha ő elvégezte a első munkamenet közben a időszakot. Az óra, nap, hét vagy hónap módosíthatja az időközöket, ameddig megőrzött felhasználók (és az új felhasználók) számítanak. A felhasználó adatmegőrzési analytics fölött cohorts épül. Egy kohortban az új felhasználók adott időszakra észlelt halmazára (tehát az első munkamenet ez idő alatt futtatnak a felhasználók beállítása). 1 nap, 2 nap, 4, a nap, 7 napig vagy 1 hónap cohorts használjuk. 1 – naponta, 2 nap, 4 nap, 7 napig, vagy 1-hónap és Azure mobil részvételét függvény ki az összes olyan felhasználó, aki a kohortban és a tagja megadása még egy kohortban tekintve aktív (vagyis a halmaz a felhasználók, akik legalább egy munkamenet az időszak során végzett). A felhasználók egy kohortban verzió nevezik. (Azure Mobile tetszés szerint elmélyedhet tekintheti meg, hány felhasználó még használ az alkalmazás, de csak a platform adott tár tudhatja meg, hány, a felhasználók eltávolítása a alkalmazás – például a GooglePlay iTunes, a Windows áruházból, stb.).
- Munkamenetek: A felhasználó által az alkalmazás használatát. Munkamenetek jönnek létre, a felhasználók által végzett tevékenységek sorozatának (egy tevékenységet a egy képernyővel az alkalmazás használatát a általában, de ez használatától függően eltérő lehet a SDK integrálva van az alkalmazás módon). A felhasználók egyszerre csak birtokában elvégezhetnek egy tevékenység: munkamenet elindul, amint a felhasználó elindítja az első tevékenységét, és leállítja a ő befejeződésekor utolsó tevékenységét. Ha egy felhasználó több, mint néhány másodpercet marad, minden tevékenység végrehajtása nélkül, a tevékenységek sorozatának feloszthatja van két különböző munkamenetek.
- Tevékenységek: Minden képernyő, az alkalmazás nevét és a felhasználók idő hosszának töltsön minden képernyőn. Tevékenységek egy egyéni analitikus beállítást, amely a "app útmutatók" címkék úgy állította be a saját alkalmazás felel meg a következők:
- Felhasználói elérési útja: Jeleníti meg, hogyan navigálnak a felhasználók az alkalmazás tevékenységek (képernyő). A csúszka húzásával állítsa be a részletek szintjét. Kék csomópontok jelenítik meg az alkalmazás tevékenységeket. Azok a mérete az idő arányos felhasználók töltött el benne. Fehér csomópontok munkamenet indítása és leállítása jelenítik meg. Piros csomópontok összeomlik jelenítik meg. Hivatkozások jelenítik meg az alkalmazás tevékenységek (vagy tevékenységek és lefagy a között) áttűnések. Kattintson a csomópont vagy az adatok további információt az Elemleírások megjelenítése hivatkozás: töltött idő egy adott képernyőn áttűnések számát, és a cél műveletre a forrás tevékenységből áttűnések. (A---60 %---> B azt jelenti, hogy a felhasználók jelentő tevékenységet A tevékenység B ugrik az idő 60 %.) A diagram átrendezéséhez van szüksége, egyértelművé; helyzetét menti a program minden alkalommal, amikor módosítása. Megjelenítheti és elrejtheti a lefagy a világosítása a diagramot.
- Események: A az alkalmazás a felhasználó által végzett műveletek adott. Az eloszlás események a munkamenet felhasználónkénti események száma formában jelenik meg. Esemény azonnali műveletet, például egy kattintással gomb vagy értesítést fogadását jelző. (Események szerinti függ, hogyan a SDK integrálva van az alkalmazás.) Az esemény akkor fordulhat elő, a munkamenet vagy a projekt során, vagy különálló is lehet.
- Feladatok: Kivéve azokat a művelet hossza koncentráljon események hasonló. Ha például feladatok lehet mondani, műszaki információkat szeretne kapni, mennyi ideig tart a tartalom betöltés vagy webszolgáltatás hívása. Azt is megjelenítése sikerült, mennyi ideig tart a felhasználót, hogy egy űrlapot kitöltő, hozzon létre egy fiókot vagy a vásárlás. Egy feladat jelöli a tevékenység időtartamát, például egy letöltési tevékenység vagy az idő hosszának transzparens jelenik meg a képernyőn. (Feladatok szerinti függ, hogyan a SDK integrálva van az alkalmazás.) Feladatok a háttérben futó feladatokat a munkamenet (tehát minden olyan felhasználó tevékenység) nélkül hatókörének kívüli végrehajtott általában.
- Technicals: Műszaki információkat szeretne kapni az eszközöket, hogy nyomon követi, például a területi, a Carrier, a hálózati, az eszköz, a belső vezérlőprogram, és a felhasználók eszközalapú méretét, és az alkalmazás verziójának és az alkalmazásban használt SDK verzió használata esetén az alkalmazás felhasználóinak.
- Hibák: Információk a hibákról technikai belül az alkalmazás, amely nem hiúsul meg az alkalmazás összeomlik. Hiba azonnali problémát, például hálózati hiba vagy egy hibás kezelő jelöli. (Események szerinti függ, hogyan a SDK integrálva van az alkalmazás.) Hiba akkor fordulhat elő, a munkamenet vagy a projekt során, vagy különálló is lehet.
- Összeomlik: Az alkalmazás összeomlik okozó hibák információt. E az összeomlást egy váratlan feltétel, ahol az alkalmazás megáll a várt ellátása, és le kell állítani. E az összeomlást az általában az alkalmazás egy hiba miatt.

![Analytics2][11]

## <a name="accessing-the-retention-overview"></a>Az adatmegőrzési áttekintés elérése
![Analytics3][12]

Az adatmegőrzési áttekintés lebontott be több üdvözlőlap, a középső egyes a áttekintést megjelenítő egyes adatmegőrzési időszakra vonatkozóan. A 2 napos az adatmegőrzési időszak példában látható. A többi kártya megjelenítése a 4 nap és 7 napig adatmegőrzési időszak.

## <a name="understanding-the-retention-overview-cards"></a>Az adatmegőrzési áttekintése kártyák ismertetése
![Analytics4][13]

### <a name="each-card-is-composed-of-3-main-parts"></a>Minden egyes kártyán megtalálható 3 fő részből áll:
1. 1: kohortba és az időszak
2. 2-4-es: az aktuális időszakra az adatmegőrzési
3. 5: a előzményeket értékgörbe

### <a name="here-is-detailed-information-about-each-element"></a>Az alábbiakban a minden elem részletes információt:
1.    Kohortban és időszak: A címsort kohortban típusú adja vissza. Az alábbi "2 napon" azt jelenti, hogy fog megnézi az működése a felhasználók 2 nap alatt, a 2 nappal az alábbi blokkokban csatlakozáskor felhasználóknál, akik érkezett egy adott időszakra vonatkozóan a 2 nappal és kell-e. A fenti példa úgy véli, hogy a felhasználók a 21 és november 22nd között a tevékenységet.
2.    Az adatmegőrzési ráta ezzel kap a 21 és november 22 azoknak a felhasználóknak, 19 november 20 érkezésekor. 1 az aktív felhasználót a 21 és 22nd, között az alábbi fölé, amely a 19th és 20 között új felhasználók volt a 3-as volt.
3.    A vizuális kijelző grafikusan fentivel azonos információt biztosít. (A harmadik a kör is 33 %-os szám) A szín nyújt további információkat: zöld jelzi, hogy az előző számítás egyre száma. Sárga azt jelenti, hogy a csökkenő stabil, és piros eszközöket.
4.    Ez azt jelzi, hogy az értékek kiszámításához használt.
5.    Ez az értékgörbe a előzményeket az adatmegőrzési értékek. Lehetővé teszi, hogy hogyan evolved széles nézetének múltbeli a értékek megjelenítése.


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
