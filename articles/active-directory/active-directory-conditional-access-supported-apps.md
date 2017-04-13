<properties
    pageTitle="Az Azure Active Directory feltételes hozzáférési szabályokat használó alkalmazások |} Microsoft Azure"
    description="Feltételes hozzáférés-vezérlés az Azure Active Directory ellenőrzi a megadott feltételekkel, amikor a felhasználó számára, és lehetővé teszi az alkalmazás hozzáférést."
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="10/26/2016"
    ms.author="markvi"/>

# <a name="applications-that-use-conditional-access-rules-in-azure-active-directory"></a>Az Azure Active Directory feltételes hozzáférési szabályokat használó alkalmazások

Az Azure Active Directory (Azure Active Directory) támogatott feltételes hozzáférési szabályok-alkalmazások csatlakozik, előre integrált szövetséges szoftver, a (szoftver) szolgáltatásalkalmazások, így jelszó egyszeri bejelentkezés (SSO), a vállalati verzió-alkalmazásainak és Azure Active Directory szolgáltatásalkalmazás-Proxy használó alkalmazások. Amelynek feltételes access használható alkalmazások részletes listájáért olvassa el a [szolgáltatások engedélyezve feltételes hozzáféréssel rendelkező](active-directory-conditional-access-technical-reference.md#Services-enabled-with-conditional-access)című témakört. Feltételes access mindkét modern hitelesítési használó mobil és az asztali alkalmazásokkal működik. Ez a cikk foglalkozunk mobil és az asztali alkalmazások hogyan feltételes access működik.

Azure Active Directory bejelentkezési oldal modern hitelesítési használó alkalmazásokban is használhatja. A bejelentkezési lapján, a felhasználó kér többtényezős hitelesítés. Üzenet jelenik meg, ha a felhasználói hozzáférés zárolva van. Modern hitelesítési hitelesítést végezni az Azure Active Directory, az eszköz szükség, hogy az eszköz alapuló feltételes hozzáférési értékeli ki.

Fontos, hogy mely alkalmazások feltételes hozzáférési szabályokat és a lépéseket, hogy szükség lehet egyéb alkalmazások belépési pontról biztonságos is használható.

## <a name="applications-that-use-modern-authentication"></a>Modern hitelesítési használó alkalmazások

> [AZURE.NOTE] Az Azure Active Directory megfelelője az Office 365-ben, ha egy feltételes hozzáférési házirendet, mindkét feltételes hozzáférési házirend beállítása A szeretné alkalmazni, például feltételes access házirendek az Exchange online-ban vagy a SharePoint online-ban.

A következő alkalmazásokat támogatja a feltételes hozzáférést az Office 365- és egyéb Azure Active Directory-kompatibilis szolgáltatásalkalmazások:

| Tároló szolgáltatás  | Platform  | Alkalmazás                                                  |
|--------------|-----------------|----------------------------------------------------------------|
|Az Office 365 Exchange Online | Windows 10-ben|Levelek, a naptár és a személyek alkalmazást, az Outlook 2016-ban, az Outlook 2013 (a modern hitelesítési), a Skype vállalati verzió (a modern hitelesítési)|
|Az Office 365 Exchange Online| A Windows 8.1, Windows 7 rendszerben |Az Outlook 2016-ban, az Outlook 2013 (a modern hitelesítési), a Skype vállalati verzió (modern hitelesítési)|
|Az Office 365 Exchange Online|iOS, Android|  Az Outlook mobile-alkalmazás|
|Az Office 365 Exchange Online|Mac OS X| Többtényezős hitelesítés, és csak; a hely az Outlook 2016-ban a tervek, a Skype vállalati verzió támogatási szolgálatával a jövőben tervezett tervezett eszköz-alapú házirend-támogatás|
|Az Office 365 SharePoint online-ban|Windows 10-ben| Office 2016-alkalmazásokat, univerzális Office-alkalmazások, az Office 2013-ban (a modern hitelesítési), a OneDrive vállalati verzió alkalmazással (következő generációs szinkronizálási ügyfélprogramot vagy NGSC) támogatási tervezett a tervek, a tervek, a SharePoint-alkalmazás támogatása a jövőben tervezett tervezett Office csoportok támogatás|
|Az Office 365 SharePoint online-ban|A Windows 8.1, Windows 7 rendszerben|Office 2016-alkalmazásokat, (a modern hitelesítési) Office 2013-ban, a OneDrive vállalati verzió alkalmazással (Groove szinkronizálási ügyfélprogramot)|
|Az Office 365 SharePoint online-ban|iOS, Android|  Office mobile-alkalmazások |
|Az Office 365 SharePoint online-ban|Mac OS X| Többtényezős hitelesítés, és csak; hely az Office 2016-alkalmazások a jövőben tervezett eszköz-alapú házirend-támogatás|
|Az Office 365 Yammerbe|A Windows 10, az iOS és az Android | Az Office a Yammer alkalmazás|
|Dynamics CRM-hez|A Windows 10, Windows 8.1, Windows 7-ben, az iOS és Android | Dynamics CRM-alkalmazás|
|PowerBI szolgáltatás|A Windows 10, Windows 8.1, Windows 7-ben, az iOS és Android | PowerBI-alkalmazás|
|Azure távoli alkalmazás szolgáltatás|A Windows 10, Windows 8.1, Windows 7, iOS, Android és Mac OS X |Azure Remote alkalmazáshoz|
|Bármely Office-alkalmazások alkalmazás szolgáltatás|Android és iOS|Bármely Office-alkalmazások alkalmazás szolgáltatás |


## <a name="applications-that-do-not-use-modern-authentication"></a>Modern hitelesítési nem használó alkalmazások

Jelenleg alkalmazásokat modern hitelesítési nem használó elérésének letiltása más módszerekkel kell használnia. Modern hitelesítési nem használó alkalmazásai hozzáférési szabályok feltételes az access nem lép életbe. Ez az Exchange és a SharePoint-hozzáférési szempont elsősorban. Alkalmazások a legtöbb korábbi verzióiban a régebbi access vezérlő protokollok használja.

### <a name="control-access-in-office-365-sharepoint-online"></a>Az Office 365 SharePoint Online hozzáférés szabályozása
A Set-SPOTenant parancsmaggal SharePoint access régebbi protokollok letilthatja. Ezzel a parancsmaggal segítségével megakadályozhatja, hogy nem modern hitelesítési protokollokkal hozzáférjenek a SharePoint Online erőforrásokhoz Office-ügyfélprogramok.

**Példa parancsot**:    `Set-SPOTenant -LegacyAuthProtocolsEnabled $false`

### <a name="control-access-in-office-365-exchange-online"></a>Az Office 365 Exchange Online hozzáférés szabályozása

Exchange felajánlja a protokollok két fő kategóriába. Tekintse át a következő beállításokat, és válassza ki a házirendet, amely után mindig a szervezet számára.

-   **Az Exchange ActiveSync**. Többtényezős hitelesítés és a hely feltételes access házirendek alapértelmezés szerint nem kényszerített az Exchange ActiveSync. Közvetlenül az Exchange ActiveSync házirendek konfigurálásával és az Exchange ActiveSync letiltása az Active Directory összevonási szolgáltatások (AD FS) szabályok használatával védelme az alábbi szolgáltatások hozzáférést szeretne.
-   A **régi protokollok**. Az AD FS régebbi protokollok zárolhatja. A régebbi Office-ügyfélprogramok, például az Office 2013-ban engedélyezett modern hitelesítési és az Office korábbi verziói nem blokkolja a hozzáférést.


### <a name="use-ad-fs-to-block-legacy-protocol"></a>Active Directory összevonási szolgáltatások használatával örökölt protokoll letiltása

A következő példa szabályok segítségével az Active Directory összevonási szolgáltatások szintjén örökölt protokoll hozzáférésének letiltása az. Két közös beállítások közül választhat.

#### <a name="option-1-allow-exchange-activesync-and-allow-legacy-apps-but-only-on-the-intranet"></a>1 beállítást: Lehetővé teszik az Exchange ActiveSync és régi alkalmazás, de csak az intraneten letiltása

Az alábbi három szabályt alkalmazza az Active Directory összevonási szolgáltatások a Microsoft Office 365-ben identitás Platform, az Exchange ActiveSync-alapú forgalmat, és a böngészőben és a modern hitelesítési forgalom felek adatvédelmi használna, a hozzáférést. Régebbi alkalmazások programfájltípusok a extranetes.

##### <a name="rule-1"></a>Szabály 1

    @RuleName = “Allow all intranet traffic”
    c1:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "true"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");

##### <a name="rule-2"></a>Szabály 2

    @RuleName = “Allow Exchange ActiveSync ”
    c1:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application", Value == "Microsoft.Exchange.ActiveSync"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");

##### <a name="rule-3"></a>Szabály 3

    @RuleName = “Allow extranet browser and browser dialog traffic”
    c1:[Type == " http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] &&
    c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value =~ "(/adfs/ls)|(/adfs/oauth2)"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");

#### <a name="option-2-allow-exchange-activesync-and-block-legacy-apps"></a>2 lehetőség: Lehetővé teszi az Exchange ActiveSync és régebbi alkalmazások letiltása

Az alábbi három szabályt alkalmazza az Active Directory összevonási szolgáltatások a Microsoft Office 365-ben identitás Platform, az Exchange ActiveSync-alapú forgalmat, és a böngészőben és a modern hitelesítési forgalom felek adatvédelmi használna, a hozzáférést. Régebbi alkalmazások programfájltípusok tetszés szerinti helyen tárolhatja.

##### <a name="rule-1"></a>Szabály 1

    @RuleName = “Allow all intranet traffic only for browser and modern authentication clients”
    c1:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "true"] &&
    c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value =~ "(/adfs/ls)|(/adfs/oauth2)"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");


##### <a name="rule-2"></a>Szabály 2

    @RuleName = “Allow Exchange ActiveSync”
    c1:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application", Value == "Microsoft.Exchange.ActiveSync"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");


##### <a name="rule-3"></a>Szabály 3

    @RuleName = “Allow extranet browser and browser dialog traffic”
    c1:[Type == " http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] &&
    c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value =~ "(/adfs/ls)|(/adfs/oauth2)"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");
