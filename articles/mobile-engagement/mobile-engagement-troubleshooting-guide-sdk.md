<properties 
   pageTitle="Azure mobil tetszés szerint elmélyedhet hibaelhárítási útmutatójának - SDK" 
   description="SDK integrációs problémáinak megoldása az Azure Mobile tetszés szerint elmélyedhet" 
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

# <a name="troubleshooting-guide-for-sdk-integration-issues"></a>Hibaelhárítási útmutatójának SDK integrációs problémák

Az alábbiakban az Azure Mobile tetszés szerint elmélyedhet hogyan integrálja az alkalmazás előfordulhatnak lehetséges problémákat.

## <a name="sdk-issues-discovered-by-a-failure-in-another-area-of-your-application"></a>Az alkalmazás egy másik területére hibája felmerült SDK problémák

### <a name="issue"></a>A probléma
- Felhasználói felület adatok webhelycsoport hiba (a Analytics, figyelés, szegmens vagy irányítópultok).
- Leküldéses hibák (veremmutatót nem működnek az app alkalmazásban, illetve mindkettő ki).
- Speciális szolgáltatás hibák (a nyomon követés, feloldás földrajzi helye vagy adott veremmutatót nem működnek platform).
- API-hibák (API-khoz fail gyakran csendes hibaüzenetek nélkül).
- Szolgáltatás-hibák (Azure Mobile tetszés szerint elmélyedhet közül egyik sem működik, az alkalmazás).

### <a name="causes"></a>Okok

- El kell hárítani a Azure Mobile tetszés szerint elmélyedhet SDK csomagjában talál a legtöbb hibát fog feltárással, az alkalmazás (például a felhasználói felület adatok webhelycsoport hiba, leküldéses hiba, speciális szolgáltatás hiba, API hiba, alkalmazás összeomlik vagy látható szolgáltatás üzemszünetek) hibája.  
- Azure Mobile felvételi egy adott funkció soha nem dolgozott előtt az alkalmazásban, ha szüksége lesz az integráció befejezéséhez. 
- Ha egy adott funkció az Azure Mobile tetszés szerint elmélyedhet dolgozott és leállítása, szükség lehet az Azure Mobile tetszés szerint elmélyedhet SDK tartalmazó utolsó frissítés. Ne feledje, hogy van-e az egyes Azure Mobile tetszés szerint elmélyedhet (Android, az iOS, a Windows és Windows Phone) által támogatott platformok az Azure Mobile tetszés szerint elmélyedhet SDK egy másik verzióját.

#### <a name="sdk-integration"></a>SDK-integráció

- Azure Mobile tetszés szerint elmélyedhet nincs megfelelően integrált SDK (Analytics).
- Érhető el, nem megfelelően integrált SDK (az alkalmazást, és ezt meg az alkalmazás nyújtja).
- A tanúsítvány lejárt vagy helytelen TERMÉKKATALÓGUS összehasonlítása Fejlesztők (csak iOS esetén).
- GCM vagy ADM nincs megfelelően az SDK integrált (Android csak - szolgáltatás adott verembe küldi).
- Nincs megfelelően követés integrált SDK (telepítés áruház nyomon követése).
- Lusta hely vagy GPS helye nincs megfelelően integrált SDK (célcsoportkezelést geo-elhelyezkedés szerint).


**Lásd még:**

- [SDK dokumentáció - integráció segédvonalak][Link 5] 
- [Hibaelhárítási útmutatójának - leküldéses][Link 23]

#### <a name="sdk-upgrade"></a>SDK frissítés

- A SDK (gyakran kapcsolódik az eszközön, operációs rendszer újabb verziója) régebbi verziói kapcsolatos problémák megoldásához SDK frissítenie kell.
- Az alkalmazás minden korábbi verziójának eltávolítása az eszközről, és telepítse újra az alkalmazást, a újraregisztrálása legújabb verziójában az Azure Mobile tetszés szerint elmélyedhet a felhasználói felület győződjön meg arról, hogy az eszközön használja-e az alkalmazás legújabb verzióját, hogy a Eszközazonosítót.

**Lásd még:**

- [SDK dokumentáció – kibocsátási megjegyzések](http://go.microsoft.com/fwlink/?LinkId= 525554) 
- [SDK dokumentáció - frissítési segédvonalak](http://go.microsoft.com/fwlink/?LinkId= 525554)

#### <a name="sdk-other"></a>Más SDK

- Alkalmazás cikkét "AndroidManifest.xml" hibákat okozhatnak Azure Mobile tetszés szerint elmélyedhet, hogy nem működnek (csak Android).
- SDK integráció és az API felülettel közös problémája a SDK és az API kulcs tévessze össze.

**Lásd még:**

- [Fogalmak – szószedet][Link 6]

## <a name="advanced-coding-issues"></a>Speciális coding kapcsolatos problémák

### <a name="issue"></a>A probléma
-  Platform kódját közvetlenül nem kapcsolódó Azure Mobile tetszés szerint elmélyedhet problémákat okozhatják iOS, Android és Windows Phone.

### <a name="causes"></a>Okok

- Helytelenül írt platform kódját közvetlenül nem kapcsolódó Azure Mobile tetszés szerint elmélyedhet számos Azure Mobile tetszés szerint elmélyedhet coding problémáinak speciális okozza. Szüksége lesz az Azure Mobile tetszés szerint elmélyedhet dokumentáció (Android, az iOS, webes, a Windows és Windows Phone) mellett fejlesztésének platformra adott dokumentációjában.
- Nincs megfelelően beállítása "kategória", megakadályozza, hogy értesítést csatolása belüli vagy kívüli az alkalmazás (csak Android) másik helyre. 
- Nem ezt a beállítást "UIKit.framework" "nem" kötelező, a kódban iOS "Szimbólum a hiba nem található" jeleníti meg, illetve lefagy a régebbi az iOS-eszközök (csak iOS esetén).
- Lejárt tanúsítványok, vagy nem megfelelően Fejlesztők vagy termékkatalógus a verziójával tanúsítvány, okok leküldéses kapcsolatos problémákat (csak iOS esetén).
- Vannak bizonyos korlátozások belső az Azure Mobile tetszés szerint elmélyedhet nem szabályozható, (például a system center működése-ki letölthető verembe küldi, Android és iOS) platformot.
- Azure Mobile tetszés szerint elmélyedhet teszi közzé a belső csomagok által használt Azure Mobile tetszés szerint Elmélyedhet az iOS és az Android hivatkozás teljes listáját. Ne feledje, hogy bizonyos Azure Mobile tetszés szerint elmélyedhet szolgáltatásai adott a platformra (Android, az iOS, webes, a Windows és Windows Phone).

### <a name="see-also"></a>Lásd még:

 - [Hibaelhárítási útmutatójának - leküldéses][Link 23] 
 - [SDK dokumentáció – kibocsátási megjegyzések][Link 5]
 - [SDK dokumentáció - frissítési segédvonalak][Link 5]

## <a name="application-crashes"></a>Alkalmazás összeomlik

### <a name="issue"></a>A probléma
- Az alkalmazás összeomlik a végfelhasználók eszközön.

### <a name="causes"></a>Okok

- Összeomlik információk megtekintheti a *Analytics felhasználói felület* vagy a *Analytics API*
- Keresse meg a vizsgált eszközök eszköz azonosítója, és az azonos művelet végrehajtása, az alkalmazás összeomlik a végfelhasználó a összeomlását okozza azonosításához okozó.
- A SDK a legújabb verzióra történő frissítés néha szünteti meg az Azure Mobile tetszés szerint elmélyedhet SDK ismert problémái hatására alkalmazás összeomlik. Ellenőrizze, hogy a kibocsátási megjegyzések az platform kapcsolatos összeomlik vizsgálatakor.

### <a name="see-also"></a>Lásd még:

- [SDK dokumentáció – kibocsátási megjegyzések][Link 5]
- [SDK dokumentáció - frissítési segédvonalak][Link 5]

## <a name="app-store-upload-failures"></a>App store feltöltés hibák

### <a name="issue"></a>A probléma
- Az alkalmazás legújabb verzióját feltöltése Apple, Google vagy a Windows-alkalmazás-áruházból kapcsolatos hibákat.

### <a name="causes"></a>Okok

- Alkalmazás tárolja néha blokk alkalmazások az egyes engedélyezett funkciók (például az Apple Store megakadályozza, hogy a IDFV használatban az áruház alkalmazásait és az GooglePlay tároló megakadályozza, hogy az alkalmazások között alkalmazás megosztása). 
- Győződjön meg arról, hogy a kibocsátási megjegyzések a platform és a SDK aktuális verziójának ellenőrzése, ha az alkalmazás feltöltése az üzlet nehézséget.

<!--Link references-->
[Link 1]: mobile-engagement-user-interface.md
[Link 2]: mobile-engagement-troubleshooting-guide.md
[Link 3]: mobile-engagement-how-tos.md
[Link 4]: http://go.microsoft.com/fwlink/?LinkID=525553
[Link 5]: http://go.microsoft.com/fwlink/?LinkID=525554
[Link 6]: http://go.microsoft.com/fwlink/?LinkId=525555
[Link 7]: https://account.windowsazure.com/PreviewFeatures
[Link 8]: https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=azuremobileengagement
[Link 9]: http://azure.microsoft.com/en-us/services/mobile-engagement/
[Link 10]: http://azure.microsoft.com/en-us/documentation/services/mobile-engagement/
[Link 11]: http://azure.microsoft.com/en-us/pricing/details/mobile-engagement/
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
 
