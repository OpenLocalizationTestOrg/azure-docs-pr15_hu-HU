<properties 
   pageTitle="Azure mobil tetszés szerint elmélyedhet hibaelhárítási útmutatójának - API-hoz" 
   description="Hibaelhárítási útmutatók az Azure mobil tetszés szerint elmélyedhet - API-hoz" 
   services="mobile-engagement" 
   documentationCenter="" 
   authors="piyushjo" 
   manager="erikre" 
   editor=""/>

<tags
   ms.service="mobile-engagement"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="mobile-multiple"
   ms.workload="mobile" 
   ms.date="10/04/2016"
   ms.author="piyushjo"/>

# <a name="troubleshooting-guide-for-api-issues"></a>Útmutató az API-val kapcsolatos problémák elhárítása

Az alábbi táblázat a lehetséges problémák előfordulhatnak a rendszergazdák Azure Mobile tetszés szerint elmélyedhet keresztül az API-k és együttműködését.

## <a name="syntax-issues"></a>Szintaxis kapcsolatos problémák

### <a name="issue"></a>A probléma
- Szintaktikai hibák API (vagy szokatlan viselkedést tapasztal) segítségével.

### <a name="causes"></a>Okok

- Szintaktikai hibák:
    - Ellenőrizze, hogy az adott API segítségével győződjön meg arról, hogy érhető el az alábbi.
    - A gyakori problémák, az API felülettel is tévessze össze a elérjen API-val és a leküldéses API (a legtöbb feladatokat kell elvégezni a helyett a leküldéses API elérjen API-val). 
    - Egy másik közös probléma SDK integráció és az API felülettel tévessze össze a SDK és az API kulcs.
    - Az API-k csatlakoztatása parancsprogramok kell legalább 10 percenként adatok küldése vagy a kapcsolatot a (különösen Monitor API parancsfájlok figyeli az adatokat a közös) időtúllépést okoz. Időtúllépései elkerülése érdekében a parancsprogram-XMPP ping 10 percenként küldése tartani a munkamenet életben kommunikálni a kiszolgálóval rendelkezik.

### <a name="see-also"></a>Lásd még:
 
- [API dokumentációja][Link 4]
- [XMPP protokoll információ]( http://xmpp.org/extensions/xep-0199.html)
 
## <a name="unable-to-use-the-api-to-perform-the-same-action-available-in-the-azure-mobile-engagement-ui"></a>Nem lehet, hogy ugyanazt a műveletet az Azure Mobile tetszés szerint elmélyedhet a felhasználói felület érhető el az API segítségével

### <a name="issue"></a>A probléma
- Olyan művelet, amelyet az Azure Mobile tetszés szerint elmélyedhet a felhasználói felület működik az Azure Mobile kapcsolódó tetszés szerint elmélyedhet API nem működik.

### <a name="causes"></a>Okok

- Igazolja, hogy az azonos műveleteket végezhet az Azure Mobile tetszés szerint elmélyedhet a felhasználói felület jeleníti meg, hogy helyesen integrálva van ez a funkció az Azure Mobile tetszés szerint elmélyedhet a SDK csomagjában talál.

### <a name="see-also"></a>Lásd még:
 
- [Felhasználói felület dokumentáció][Link 1]
 
## <a name="error-messages"></a>Hibaüzenetek jelennek meg

### <a name="issue"></a>A probléma
- A megjelenített futásidőben vagy naplókban API hibakódok.

### <a name="causes"></a>Okok

- Az alábbiakban a gyakori API állapot kódok számok hivatkozást és az előzetes hibaelhárítási összetett listájának:

        200        Success.
        200        Account updated: device registered, associated, updated, or removed from the current account.
        200        Returns a list of projects as a JSON object or an authentication token generated and returned in the response’s body.
        201        Account created.
        400        Invalid parameter or validation exception (check payload for details). The parameters provided to the API or service are invalid. In this case, the HTTP response will embed more details. Make sure to test for the MIME type of the response as the payload can either be plain text or a JSON object.
        401        Authentication error. No user is currently authenticated or connected (check the AppID and SDK key).
        402        Billing lock. The application is either off its quotas or is currently in a bad billing state.
        403        The application is not enabled or the specific API is disabled for this application.
        403        Unauthorized access to the project or application, invalid access key (the key must match the one provided when created).
        403        Campaign specific errors: campaign must be finished (or has already been activated), the suspend action can only be performed on an scheduled campaign, cannot finish a campaign that is not currently “in progress”, campaign must be “in progress” and the campaign’s property named, manual Push must be set to true.
        403        The email address is already associated to another account (a super user for instance). No authentication token will be generated.
        404        Application, device, campaign, or project identifier not found.
        404        Query parameter is invalid JSON or has a field with an unexpected value.
        404        The email address is not associated with an account. Please create or update the account first.
        405        Invalid HTTP method (GET, POST, etc.) or trying to edit a read only segment (i.e. add or update or delete a criterion). A segment becomes read only after it has been computed for the first time.
        409        Name already associated to a different device ID or campaign.
        413        Too many device identifiers (current limit is 1,000), POST URL encoded entity is over 2MB, or the period is too large to be displayed (the server didn’t retrieve the analytics because the user request is for a period that is too large).
        503        Analytics not available yet (the requested information is not computed yet for an application).
        504        The server was not able to handle your request in a reasonable time (if you make multiple calls to an API very quickly, try to make one call at a time and spread the calls out over time).

### <a name="see-also"></a>Lásd még:

- [Részletes hibák minden adott API - API dokumentációja][Link 4]
 
## <a name="silent-failures"></a>Csendes-hibák

### <a name="issue"></a>A probléma
- API művelet sikertelen lesz a nincs futásidőben vagy naplókban jelenik meg hibaüzenet.

### <a name="causes"></a>Okok

- Sok elemet Ha megfelelően, hogy nem integrált Azure Mobile tetszés szerint elmélyedhet a felhasználói felület letiltjuk lesz, de az API-t az értesítés nélkül sikertelen lesz, így ne feledje, hogy tesztelje a felhasználói felületen, ha működésével ugyanazokat a szolgáltatásokat is.
- Azure Mobile tetszés szerint elmélyedhet és speciális jellemzői Azure Mobile tetszés szerint elmélyedhet a használni kívánt kell egyenként integrálni kell az alkalmazás és a SDK külön lépések előtt használhatja őket.

### <a name="see-also"></a>Lásd még:

- [Hibaelhárítási útmutatójának - SDK][Link 25]
 
<!--Link references-->
[Link 1]: mobile-engagement-user-interface-home.md
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
 
