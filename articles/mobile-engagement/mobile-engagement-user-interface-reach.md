<properties 
   pageTitle="Azure mobil tetszés szerint elmélyedhet felhasználói felület - vannak" 
   description="Megtudhatja, hogy miként keresse meg a felhasználók az alkalmazás a leküldéses értesítések Azure Mobile tetszés szerint elmélyedhet használatával" 
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


# <a name="how-to-reach-out-to-the-users-of-your-application-with-push-notifications"></a>Keresse meg a leküldéses értesítéseket, az alkalmazás a felhasználók hogyan

Ez a cikk ismerteti a **Mobile tetszés szerint elmélyedhet** portál **ELÉRÉSE** lapja. A **Mobil tetszés szerint elmélyedhet** portal segítségével figyelheti és kezelheti a mobilalkalmazások. Fontos tudni, hogy a portálon használatának megkezdéséhez, először hozzon létre egy **Azure Mobile tetszés szerint elmélyedhet** fiókot. További tudnivalókért olvassa el a [Azure Mobile tetszés szerint elmélyedhet fiók létrehozása](mobile-engagement-create.md)című témakört.

A felhasználói felület vannak szakasza itt végezhető el létrehozása és szerkesztése/aktiválása/Befejezés/monitor leküldéses marketingkampány-kezelés eszköz, és elérheti a leküldéses értesítést kampányok és a elérjen API-val (és az alacsony szinten leküldéses API elemeket) keresztül is elérhető szolgáltatások statisztikai. Ne feledje, hogy használja az API-hoz vagy a felhasználói felület, hogy meg kell integráció Azure Mobile tetszés szerint elmélyedhet és a gyermekektől platformokon az alkalmazásba, mielőtt elkezdhetné használni a SDK elérjen kampányok.

>[AZURE.NOTE] A **Mobil tetszés szerint elmélyedhet** portál felhasználói felület sok szakaszt tartalmaz a **SÚGÓ megjelenítése** gombra. Nyomja le az erre a gombra kattintva egy szakasz környezetfüggő ismertetése.

## <a name="four-types-of-push-notifications"></a>Leküldéses értesítések négyféle
1.    Hirdetmények - teszi lehetővé hirdetési üzeneteket küldeni a felhasználóknak, hogy az alkalmazás belül másik helyre irányítja őket, és küldje el nekik egy weblapra vagy a tár nem az alkalmazás. 
2.    Lekérdezések – végfelhasználó által kérdések felvetése információk összegyűjtése teszi lehetővé.
3.    Adatok veremmutatót - fájlküldés bináris vagy base64 adatokat teszi lehetővé. Adatok leküldéses tárolt adatok módosítására a felhasználói jelenlegi felület az alkalmazásban az alkalmazás küldi. Az alkalmazás kell tudja adatok leküldéses adatainak feldolgozása.

## <a name="campaign-details"></a>Marketingkampány részletei

Szerkesztése, klónozhatja, törlése, vagy nincs aktiválva még az illető neve fölé elemekre kampányok aktiválása, és kattintással nyissa meg őket. Átmásolhatja kampányokkal, hogy az illető neve fölé elemekre már aktiválva, vagy kattintással nyissa meg őket. Jó helyen jár Ha aktiválva van egy marketingkampány nem módosítható.
 
![Reach1][18]

## <a name="reach-feedback"></a>Visszajelzés elérése

Kattintson a **Statisztika** vannak kampányok részleteinek megtekintéséhez. Az **egyszerű** nézet vizuálisan ábrázolhatók arról, mi történt a marketingkampány lett aktiválása után oszlop oszlopdiagramon formájában. A **Speciális** nézet ismerteti a leküldéses marketingkampány finomabb részleteit. Ezek az adatok csak akkor érhető el, ha szeretné elküldeni a próba marketingkampány tehát küld egy próba eszköz leküldéses. Itt látható, miként kell értelmezni ezeket az adatokat:

1. **Pushed** - azt adja meg, hogy az eszközök tolódik üzenetek számát. Ez a szám a megadott a leküldéses marketingkampány létrehozása során célközönség függ. Ha nem ad meg bármely célközönséget, majd a leküldéses küld a bejegyzett eszközök. Minden más leküldéses szolgáltatások, például azt az értesítést nem a közvetlenül az eszközök, de inkább leküldéses őket a megfelelő platformra adott leküldéses értesítést szolgáltatások (PNS - APN/GCM/WNS), hogy ezeket az értesítéseket tartana az eszközökön. 

2.  **Szállítva** – ezzel adja meg az üzenetek sikeresen kézbesítve által a PNS, hogy az eszközön és nyugtázza számát, mint Mobile tetszés szerint elmélyedhet SDK szerint kapott. 
        
    *Okok szállítva megszámolása kisebb tolt száma:*
    
    1. Ha a felhasználó rendelkezik el lesz távolítva az alkalmazást az eszközt, de a PNS vele nem tudja a leküldéses elküldjük Önnek a PNS való időben az üzenetet a program eltávolítja.
    2. Ha az eszköz van az alkalmazás, de saját maguk eszközök Ez a kapcsolat nélküli hosszú ideig, a PNS meghiúsul az üzenet kézbesítése az eszközt. 
    3. Ha az üzenet kézbesítve az eszközre, de a Mobile tetszés szerint elmélyedhet SDK csomagjában talál az alkalmazás nem ismeri fel az üzenet tartalmát, majd esik ezt az üzenetet. Ha a testre szabott az értesítés az alkalmazásban hoz létre kivétel, amely azt elfog SDK és az üzenet legördülő ennek. Ez is előfordulhatnak, ha az alkalmazás az eszközre az a Mobile tetszés szerint elmélyedhet SDK, amely nem érti a leküldéses üzenet a platform küldünk újabb verziója verzióját használva, de ez csak akkor, ha az alkalmazás frissítették után az értesítés az a szolgáltatás platform elküldésének. A **Speciális** lapon megtudhatja, hogy hány üzenet megszakadt. 
    4. IOS-eszközökön üzenetek előfordul, hogy nem kézbesítve alacsony elem vagy az eszköz esetén, vagy ha az alkalmazás van használata power jelentős mennyiségű más, távoli értesítések feldolgozása közben. Az iOS-eszközökhöz korlátozása.   

3.  A **megjelenő** - azt adja meg, hogy az üzenetek sikeresen megjelenik az alkalmazás felhasználói az eszközre az értesítési központban rendszer push/out-a-alkalmazás értesítést vagy a mobilalkalmazás belül az alkalmazás értesítés formájában, amely számát.  A **Speciális** lapon megtudhatja, hány rendszerüzenetek és hány az alkalmazás. 
    
    *Okok megjelenő megszámolása kisebb szállítva száma (váró jelenik meg)*
    
    1. Ha az értesítési marketingkampány kellett egy záró dátumot rajta majd lehetőség, hogy az értesítés kézbesítése megtörtént, de az idő nyissa meg, és jelenítse meg az app felhasználónak jutott, amikor azt már lejárt, a program soha nem jelenik meg.   
    2. Ha az értesítés az alkalmazás az értesítést, majd az értesítés, csak akkor jelenik meg, amikor az alkalmazás felhasználó megnyitja az alkalmazás. Azokban az esetekben, ahol az alkalmazás felhasználói nem megnyitott, az alkalmazás a SDK jelentést, hogy az értesítés kézbesítve, de még nem jelenik meg, amíg meg nem nyitja meg az alkalmazást. 
    2. Ha az értesítés az alkalmazás az értesítést, és konfigurált ahhoz, hogy megjelenítse a egy adott tevékenység képernyőn, akkor is az értesítés is jelenteni szállított, de kézbesítve addig, amíg a felhasználó még nem nyílik meg az alkalmazást egy adott képernyőn. 
    
4.  **Felhasználói kommunikációt** – Ez az alkalmazás felhasználó rendelkezik kezelni üzenetek számát adja meg, és az üzeneteket, amelyek actioned vagy kilépett tartalmazni fogja. 

    - *Az alkalmazás felhasználói művelet értesítés az alábbi módszerek egyikével is:*
            
        1. Ha az értesítés rendszer/kimenő-a-alkalmazás értesítést, vagy csak értesítést küld, az értesítés az alkalmazás felhasználó kattint, majd az alkalmazás értesítés.
        2. Ha az értesítés az alkalmazás az értesítést a szöveg vagy más webes-nézet, vagy kattintson az alkalmazás felhasználói gombra kattint, a művelet gombra az értesítésben lekérdezi.
        3. Ha az értesítés az egy webes megtekintése az alkalmazás értesítést, majd az alkalmazás felhasználó kattint a webes [Android csak megtekintés] URL-cím
    
    - *Az alkalmazás felhasználói kiléphet értesítés az alábbi módszerek egyikével:*
    
        1. Az értesítés Bezárás gombjára kattintva közvetlenül. 
        2. Pöccintsen a nem vagyok a gépnél, vagy törlése az értesítésre. 
        3. Szöveg: webtartalom és szavazásokra az alkalmazás értesítések általában jelennek meg az alkalmazás felhasználó lépéssel. Először látják az értesítést, és amikor rákattintanak rajta, akkor fogja látni a későbbi szöveg, a webhely és a szavazás tartalmat. Az alkalmazás felhasználói kiléphet jelenít meg értesítést e, és a részletek, a Speciális nézetben ez rögzíti. 

5.  **Actioned** - azt adja meg, amelyek egyértelműen actioned az alkalmazás felhasználó által üzenetek számát. Ez azt jelenti, hogy hány app felhasználói szerint az üzenetet az értesítésben tolni lettek érdeklik, az a legérdekesebbek számot. 
 
> [AZURE.NOTE] Az iOS és az ablak platformok, ha a felhasználó rendelkezik-e az alkalmazást, nyissa meg és a marketingkampány "Bármikor" marketingkampány volt, és lehetséges, hogy mindkettő ki az alkalmazást, és az alkalmazás megjelenik egy időben. Ez a nagyobb, mint a szállítva megjelenő száma jelenhet meg. Ha a felhasználói vagy műveletek az értesítés, majd a felhasználói kapcsolati/Actioned számát is lehet nagyobb, mint szállítva. 


![Reach2][19]

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
[Link 27]: mobile-engagement-user-interface-reach-campaign.md
[Link 28]: mobile-engagement-user-interface-reach-criterion.md
[Link 29]: mobile-engagement-user-interface-reach-content.md
 
