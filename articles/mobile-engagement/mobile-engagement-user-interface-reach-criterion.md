<properties 
   pageTitle="Azure mobil tetszés szerint elmélyedhet felhasználói felület - vannak feltétel" 
   description="Megtudhatja, hogy miként küldhet célkritériumokat leküldéses kampányok a egy kijelölt részét a felhasználók Azure Mobile tetszés szerint elmélyedhet használatával" 
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


# <a name="how-to-use-targeting-criteria-to-send-push-campaigns-to-a-select-subset-of-your-users"></a>Küldje el a felhasználók egy kijelölt részét leküldéses kampányok célkritériumokat használata

A célközönség kiválasztásával által megadott feltételek az "Új feltételeket" gombbal az egyik leghatékonyabb fogalmak Azure Mobile tetszés szerint elmélyedhet a megfelelő elküldött segít értesítést, hogy a vevők válaszol levélszemét mindenki helyett. A közönség normál feltételek alapján korlátozza, és szimulálhatja veremmutatót határozza meg, hogy hányan a értesítést kap.

**Lásd még:**

- [Felhasználóifelület - vannak - dokumentáció új leküldéses marketingkampány][Link 27]

## <a name="audience-criteria-can-include"></a>A célközönség feltételek a következők lehetnek:
- **Technicals:** Megcélozhat megjelenik az analitikai és Monitor szakaszok azonos technikai adatok alapján. **Lásd még:** [Felhasználói felület dokumentáció - elemzés] [ Link 15], [Felhasználói felület dokumentáció – képernyő][Link 16]
- **Helye:** Alkalmazások használható "valós idejű hely jelentéskészítés" Geo-kerítés használhatják Geo helyfüggő feltételként a GPS helyről célközönség célba. "Lusta hely jelentési" hívást a mobiltelefonján helyről célközönség célba is használható ("valós idejű hely jelentés" és "Lusta terület hely jelentéskészítés" aktiválnia kell az a SDK). **Lásd még:** [SDK dokumentáció – iOS - integráció] [ Link 5], [SDK dokumentáció - Android - integráció][Link 5]
- **Elérje a visszajelzés:** Megcélozhat a közönség az előző vannak értesítések keresztül hirdetmények, felmérésekre és adatok verembe küldi vannak visszajelzései visszajelzései alapján. Ez lehetővé teszi jobb célba a közönség után két vagy három kampányok elérni, mint azt is, hogy az első alkalommal. Is használható a felhasználók, akik már nem küldi el a felhasználók, akik már megkapta az egy adott előző marketingkampány szeretne egy marketingkampány megadásával a hasonló tartalommal értesítést kapott kiszűrésére. Felhasználók is kizárhat ki tartalmaz egy adott marketingkampány továbbra is aktív új veremmutatót sorából. **Lásd még:** [Felhasználói felület dokumentáció - elérjen - leküldéses tartalom][Link 29]
- **Telepítése a nyomon követés:** Nyomon követheti a felhasználók az alkalmazást futtató alapuló információk. **Lásd még:** [Felhasználói felület dokumentáció - beállítások][Link 20]
- **Felhasználói profil:** Megcélozhat alapuló, szabványos felhasználói adatok és a létrehozott egyéni alkalmazások adatai alapján cél is. Ide tartoznak a felhasználók, akik jelentkezett be, és a felhasználóknál, akik fogadta nekik a magát az alkalmazás csak hogyan azok már megválaszolt előző kampányokkal helyett kérték problémája van. Az alkalmazás megjelenjenek a lista összes az alkalmazás információ definiálva.
- Szegmensek: Is alapján létrehozott szegmensek cél több feltételt tartalmazó adott viselkedés alapján. Az összes a szakaszokat, az alkalmazás definiált jelennek meg a listát. **Lásd még:** [Felhasználói felület dokumentáció - szakaszokat][Link 18]
- **App útmutatók:** Egyéni alkalmazás információ címkéket hozhat létre beállításokból"" felhasználó viselkedés nyomon követéséhez. **Lásd még:** [Felhasználói felület dokumentáció - beállítások][Link 20]

## <a name="example"></a>Példa: 
Ha szeretne tartalmat leküldéses csak a felhasználót, hogy az alkalmazás vásárlása művelet végrehajtását követően alszint megadása.

1. Nyissa meg az alkalmazás beállításai lapon, a "App útmutatók" menüt, és válassza az "Új alkalmazás információ"
2. Egy új logikai alkalmazás info nevű "inAppPurchase" regisztráció
3. Győződjön meg az alkalmazás beállítása "igaz" Amikor a felhasználó sikeresen hajt végre-az alkalmazás vásárlása a app útmutatók (a sendAppInfo ("inAppPurchase",...) a függvény)
4. Ha azt szeretné, ehhez az alkalmazásból, megteheti azt az eszköz API segítségével a kódmentes)
5. Akkor csak kell a közleményt, hozzon létre egy feltétel, a közönségnek, ezáltal "inAppPurchase" értékűre rendelkező felhasználók korlátozása "igaz")
 
> Megjegyzés: Kiválasztásával alkalmazás információ címkék-től különböző feltételek alapján szükséges információk összegyűjtése felhasználói eszközökről a leküldéses elküldése előtt, és így lelassulhat az Azure Mobile tetszés szerint elmélyedhet. Összetett leküldéses konfigurációs beállítások (például a jelvények frissítése) is késleltetheti verembe küldi. A "több tartalma" marketingkampány a leküldéses API a használata Azure Mobile tetszés szerint elmélyedhet abszolút leggyorsabb leküldéses módot. A következő leggyorsabb módszerrel leküldéses feltételként csak az alkalmazás információ címkék használata a gyermekektől marketingkampány (vagy a elérjen API-t vagy a felhasználói felület), mert a alkalmazás információ címkék kiszolgálóoldali mappájában vannak tárolva. A leküldéses marketingkampány más célkritériumokat használata a rugalmas de a legkisebb leküldéses mód, mivel Azure Mobile tetszés szerint elmélyedhet eszközök lekérdezése a marketingkampány küldhet.
 
![Vannak-Criterion1][29] 

## <a name="criterion-options-apply-to"></a>A kritérium beállítások alkalmazása:
- **Technicals**     
- Belső vezérlőprogram neve: belső vezérlőprogram neve
- Belső vezérlőprogram verziója: belső vezérlőprogram verziója
- Eszköz modell: eszköz modell
- Eszközgyártó: számítógépgyártó
- Alkalmazás verziója: alkalmazás verziója
- Carrier neve: meghatározatlan Carrier neve
- Carrier ország: Carrier ország, nem definiált
- Hálózati típusa: hálózati típusa
- Területi beállítások: területi beállítások
- Képernyő méretéhez: képernyő méretéhez
- **Hely**      
- Legutolsó terület: ország, régió, településen
- Valós idejű geo-kerítés: listában a POIs (a név, a műveletek), kör alakú n (neve, szélesség, hosszúság, sugarú méter)
- **Visszajelzés elérése**     
- Hirdetmények visszajelzés: hirdetmény, visszajelzés
- Visszajelzés lekérdezik: szavazást, visszajelzés
- Válasz visszajelzés lekérdezik: lekérdezik az answer visszajelzés, a kérdést, a választási lehetőségek
- Adatok leküldéses visszajelzés: adatok leküldéses, visszajelzés
- **A nyomon követés telepítése**     
- Tároló: Tároló meghatározatlan
- Forrás: A forrás meghatározatlan
- **Felhasználói profil**     
- Gender: férfi vagy nő, nem definiált
- Dátum megellett: operátorral, nem definiált dátum
- Csatlakozás a: IGAZ vagy hamis, nem meghatározott
- **Alkalmazás-információ**      
- Karakterlánc: Karakterláncot, nem definiált
- : Operátor, dátuma meghatározatlan
- Egész szám: operátor, szám, nem definiált
- Logikai érték: IGAZ vagy hamis értéket, nem definiált
- **A szakasz**    
- Szegmensek nevét (legördülő listából) kizárás (cél felhasználóknak, akik nem része: Ez a szakasz).

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
 
