<properties
    pageTitle="2.0-s verzió végpont korlátai és korlátozásokat |} Microsoft Azure"
    description="Korlátozások és korlátozásokat az Azure Active Directory 2.0-s verzió végponttal listája."
    services="active-directory"
    documentationCenter=""
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/30/2016"
    ms.author="dastrock"/>

# <a name="should-i-use-the-v20-endpoint"></a>A 2.0-s verzió végpontot kell használni?

Integrálása az Azure Active Directory-alkalmazások készítésekor kell döntse el, hogy a 2.0-s verzió végpontot, és a hitelesítéshez protokollok az igényeknek megfelelően.  Az eredeti Azure AD-végpontot továbbra is teljesen támogatott és bizonyos területeken 2.0-s-nél több funkció rich.  Azonban a 2.0-s verzió végpont [jelentős előnyt](active-directory-v2-compare.md) érnie az új programozási modellt használja a fejlesztők számára.

Ezen a ponton a egyszerre a javaslatok használata a 2.0-s verzió végpont a következő módon:

- Ha személyes Microsoft-fiókok esetén az alkalmazás használatát támogatja, a 2.0-s verzió végpont kell használnia.  De ügyeljen arra, hogy a jelen cikkben felsorolt, különösen a kifejezetten működik, és iskolai fiók kapcsolatos korlátozások megértéséhez.
- Ha az alkalmazás csak akkor van szüksége, támogató munkahelyi és iskolai fiókjából, [az eredeti Azure AD-végpontok](active-directory-developers-guide.md)kell használnia.

Az idő múlásával a 2.0-s verzió végpont kiküszöböléséhez az itt látható a korlátozás, így fog minden eddiginél csak akkor használja a 2.0-s verzió végpont fognak nőni.  Ez a cikk időközben célja, hogy ha a 2.0-s verzió végpont meg.  Ez a cikk frissítése a 2.0-s verzió végpont aktuális állapotára, ezért érdemes visszatérhet az igényeknek megfelelően alakíthatja a 2.0-s verzió funkciók ellen úrjaértékelheti időbeli folytatjuk.

Ha egy meglévő alkalmazást az Azure Active Directory, mely nem használja az 2.0-s verzió végpontot, nincs szükség van a nulláról.  A jövőben azt fogja kell biztosítva, hogy engedélyezése a meglévő Azure AD-alkalmazások használata a 2.0-s verzió végponttal.

## <a name="restrictions-on-apps"></a>Alkalmazások korlátozásai
A következő típusú alkalmazásokat jelenleg által nem támogatott a 2.0-s verzió végpontot.  A támogatott típusú alkalmazásokat leírását olvassa el az [Ebben](active-directory-v2-flows.md)a cikkben.

##### <a name="standalone-web-apis"></a>Különálló webes API-hoz
Az a 2.0-s verzió végpontot az azt jelenti, hogy [egy webes API -val, amely biztonságos OAuth 2.0 használatával generál](active-directory-v2-flows.md#web-apis), ha van.  Azonban, hogy a webes API csak akkor tudja tokenek fogadjanak az alkalmazások megosztó ugyanazt az alkalmazás azonosítót.  Nem támogatott felépíteni a webes API-val, amely egy másik alkalmazás azonosítóval ügyfélről érhető el.  Az ügyfél által fog nem tudja kérjen vagy a webes API engedélyért.

Hogyan hozhat létre, amely az azonos alkalmazás azonosító ügyfélről tokenek fogad el egy webes API című témakör tartalmazza az [Első lépések](active-directory-appmodel-v2-overview.md#getting-started)a 2.0-s verzió végpont webes API minták.

##### <a name="web-api-on-behalf-of-flow"></a>Webes API a nevében – az továbbításához
Sok architektúrákban egy másik lefelé irányuló webes API-val, mind a 2.0-s verzió végpont által biztosított hívás kell, hogy a webes API tartalmazzák.  Ebben az esetben a webes API kódmentes, amelyik meghívja a Microsoft online szolgáltatás vagy egy másik beépített egyéni webes API-val, amely támogatja az Azure Active Directory rendelkező natív ügyfelek a közös.

Ebben az esetben nem használható a OAuth 2.0-s Jwt Bearer hitelesítőadat-támogatás, más néven a On-Behalf-Of Flow használatával.  Azonban a nevében – az áramlás jelenleg nem támogatott a 2.0-s verzió végpontot.  Ez a munkafolyamat működése a általában elérhető Azure Active Directory megtekintéséhez szolgáltatás, olvassa el a [kódot a-nevében-: minta GitHub](https://github.com/AzureADSamples/WebAPI-OnBehalfOf-DotNet).

## <a name="restrictions-on-app-registrations"></a>Alkalmazás regisztrációk korlátozásai
Ezen a ponton a egyszerre integrálása a 2.0-s verzió végpont kívánt összes alkalmazás létre kell hoznia egy új alkalmazás regisztráció [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)címen.  Bármelyik meglévő Azure Active Directory vagy a Microsoft Account alkalmazások nem lesz kompatibilis a 2.0-s verzió végpontot, sem bármely portálon mellett az alkalmazás új bejegyzés portál regisztrált alkalmazásokat fogja.  Azt kívánja engedélyezése a meglévő alkalmazást a 2.0-s verzió alkalmazásként használatra ad lehetőséget. Adott időben azonban nem nincs áttelepítési útvonalat egy alkalmazást a 2.0-s verzió végpontra.

Hasonlóképpen az alkalmazás új bejegyzés portál regisztrált alkalmazások nem működik az eredeti Azure Active Directory authentication végpontot ellen.  Azonban használhatja az alkalmazás regisztrációs portálon létrehozott alkalmazások sikeresen integrálása a Microsoft fiók hitelesítési végpont `https://login.live.com`.

Emellett a [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) létrehozott alkalmazás regisztrációk van az alábbiak szerint:

- A **Kezdőlap** tulajdonság, más néven **URL bejelentkezés** nem támogatott.  Nélkül kezdőlap ezeket az alkalmazásokat fogja nem jeleníthető meg az Office-alkalmazások parancsot panelen.
- Az alkalmazás azonosítója jelenleg csak két alkalmazás titkos kulcsok vannak hozható.
- Az alkalmazás regisztrációs csak tekinthetők meg és egy egyetlen Fejlesztőeszközök fiók kezeli.  Több fejlesztők között nem lehet megosztani.
- Vannak engedélyezve redirect_uri a formátumot több korlátozásai.  További részleteket a következő szakaszban olvashat.

## <a name="restrictions-on-redirect-uris"></a>Az átirányítási URL-címe korlátozásai
Az alkalmazás új bejegyzés portálon regisztrált is jelenleg csak korlátozott értékhalmaz redirect_uri.  A web Apps alkalmazások és szolgáltatások redirect_uri lehet a rendszer `https`, és az összes redirect_uri értékét meg kell osztania a DNS tartományon.  Ha például még nem lehetséges redirect_uris tartalmazó webalkalmazást regisztrálhatja:

`https://login-east.contoso.com`  
`https://login-west.contoso.com`

A regisztrációs rendszer hasonlítja össze a teljes DNS nevet a meglévő redirect_uri a redirect_uri hozzáadni a DNS nevet. Az értekezlet-összehívást a tartománynév hozzáadása meghiúsul, ha az alábbi feltételek bármelyike teljesül:  

- Ha a teljes DNS nevet az új redirect_uri nem egyezik meg a meglévő redirect_uri DNS-neve
- Ha a teljes DNS nevet az új redirect_uri nem, a meglévő redirect_uri altartományt

Ha például az alkalmazás által jelzett redirect_uri:

`https://login.contoso.com`

Ezután érdemes lehet hozzáadni:

`https://login.contoso.com/new`

amely pontosan egyezik a tartománynév vagy:

`https://new.login.contoso.com`

Ez a login.contoso.com DNS altartományt.  Ha azt szeretné, hogy az alkalmazás bejelentkezési-east.contoso.com van, és bejelentkezés-west.contoso.com mint redirect_uris, majd hozzá kell adnia az alábbi redirect_uris, sorrendben:

`https://contoso.com`  
`https://login-east.contoso.com`  
`https://login-west.contoso.com`  

Az utóbbi két leendő, mert azok altartományokat az első redirect_uri, akkor a contoso.com. A korlátozás a egy rövidesen törlődnek.

Megtudhatja, hogyan lehet rögzíteni az alkalmazás új bejegyzés portál alkalmazást, olvassa el [Ebben](active-directory-v2-app-registration.md)a cikkben.

## <a name="restrictions-on-services--apis"></a>A szolgáltatások és az API-khoz korlátozások
A 2.0-s verzió végpont jelenleg támogatja való bejelentkezés bármely alkalmazásban, az új alkalmazás regisztrációs portálon, feltéve hogy regisztrált esik [által támogatott hitelesítéstípusok](active-directory-v2-flows.md)flow a listába.  Az alkalmazások azonban csak akkor tud OAuth 2.0-s hozzáférési jogkivonat az erőforrások nagyon korlátozott számú szerezheti be.  A 2.0-s verzió végpont csak adnak a access_tokens:

- Az alkalmazás, amely a token kéri.  Az alkalmazás, hogy töltsenek egy access_token magának, ha a logikai alkalmazás számos különböző összetevőit vagy rétegek áll.  Ebben az esetben a művelet megtekintéséhez tanulmányozása: az [Első lépések](active-directory-appmodel-v2-overview.md#getting-started) oktatóanyagok
- Az Outlook posta, a naptár és a névjegyek REST API-hoz, ami a https://outlook.office.com webhelyen találhatók.  Megtudhatja, hogy miként éri el a következő API-khoz alkalmazás írni, olvassa el a [Bevezetés Office](https://www.msdn.com/office/office365/howto/authenticate-Office-365-APIs-using-v2) oktatóanyagokban.
- A Microsoft Graph API-khoz.  A Microsoft Graph és az elérhető adatok kapcsolatos további tudnivalókért látogassa meg a [https://graph.microsoft.io](https://graph.microsoft.io).

Nincs más szolgáltatások támogatottak, adott időben.  További Microsoft Online services és az egyéni beépített webes API-hoz és a szolgáltatások támogatása a jövőben belekerül.

## <a name="restrictions-on-libraries--sdks"></a>A tárak és SDK korlátozások
Ekkor a 2.0-s verzió végpont tár támogatása, meglehetősen korlátozott.  Ha egy gyártási alkalmazást a 2.0-s verzió végpont használni szeretne, a következő lehetőségei vannak:

- Webalkalmazás készítésekor, nyugodtan használhatja a kiszolgálóoldali általában elérhető köztes végezze el a bejelentkezés és érvényességi token.  Ezek közé tartozik a OWIN megnyitott azonosító csatlakozás köztes ASP.NET és a NodeJS Passport bővítményt.  Az [Első lépések](active-directory-appmodel-v2-overview.md#getting-started) szakaszában, valamint a köztes használatával mintakódok érhetők el.
- A más platformokhoz készült Worddel és a natív és mobile alkalmazások integrálhatja a 2.0-s verzió végponttal közvetlenül küldése & protokoll üzenetek fogadására alkalmazás kódban.  A 2.0-s verzió OpenID csatlakozás és OAuth protokollok [indításakor lépnek életbe](active-directory-v2-protocols.md) ilyen integrációs elvégzéséhez.
- Végül a 2.0-s verzió végpont integrálása Megnyitás azonosító csatlakozás megnyitása és OAuth tárak is használhatja.  A 2.0-s verzió protokoll sok megnyitás nélkül megjelent fontosabb változásokról protokoll tárak kompatibilisnek kell lennie.  Az ilyen tárak elérhetősége egy nyelvet és a platform, és a [megnyitott azonosító](http://openid.net/connect/) , és [OAuth 2.0-s](http://oauth.net/2/) webhelyek kezelése népszerű megvalósítás listáját. [Azure Active Directory (ADFS) 2.0-s verziója és a hitelesítéshez tárak](active-directory-v2-libraries.md) találhat további információra kíváncsi, és a Megnyitás ügyfél tárak és a 2.0-s verzió végponttal tesztelt minták listáját.

Azt is kiadott egy kezdeti előnézetét a [Microsoft Authentication Library (MSAL)](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) a .NET rendszerhez csak.  Üdvözli a próbálja ki a .NET-ügyfél- és -alkalmazások tárban, de nem fog Georgia minőségű mellékelni előzetes tárról, támogatja áll.

## <a name="restrictions-on-protocols"></a>Protokollok korlátozásai
A 2.0-s verzió végpont csak a megnyitott azonosító csatlakozás és OAuth 2.0-s támogatja.  Azonban nem az összes funkciójára és lehetőségére, az egyes protokollok van már szóló az 2.0-s verzió végpontot, például:

- A csatlakozás OpenID `end_session_endpoint`, amely lehetővé teszi, hogy egy alkalmazást a 2.0-s verzió végponttal a felhasználó befejezéséhez.
- a 2.0-s verzió végpont által kibocsátott id_tokens csak egy páros a felhasználói azonosító tartalmazhat.  Ez azt jelenti, hogy két különböző alkalmazások fog kapni az adott felhasználó eltérő azonosítót.  Megjegyzendő, hogy a Microsoft Graph lekérdezésével `/me` végpontot, akkor fogja tudni correlatable ID azonosító beszerzése a felhasználó számára használható.
- a 2.0-s verzió végpont által kibocsátott id_tokens nem tartalmaz egy `email` még akkor is, ha a felhasználótól megtekintésére jogosultak az e-mailekhez finomítása ekkor a felhasználó formál.
- Az intézők OpenID csatlakozás végpontot. Az intézők végpontot nem áll rendelkezésre a 2.0-s verzió végpont adott időben.  Jó helyen jár, az összes felhasználó profiljának adatait szeretné esetleg kap a végpont érhető el a a Microsoft Graph `/me` végpontot.
- Szerepkör & csoport követelések.  Ekkor a 2.0-s verzió végpont nem támogatja a kibocsátó szerepkör vagy csoport követelések id_tokens a.

Jobban megértheti támogatja a 2.0-s verzió végpont protokoll funkciók körét, olvassa el a [Csatlakozás OpenID és OAuth 2.0-s protokoll hivatkozás](active-directory-v2-protocols.md)keresztül.

## <a name="restrictions-for-work--school-accounts"></a>Munkahelyi és iskolai fiókokat vonatkozó korlátozások
Vannak bizonyos Microsoft szervezeti/vállalati felhasználóknak, amely még nem támogatott a 2.0-s verzió végpont néhány funkciók.

##### <a name="device-based-conditional-access-native-and-mobile-apps-and-the-microsoft-graph"></a>Feltételes access eszköz-alapú belső és a mobile-alkalmazások és a Microsoft Graph
A 2.0-s verzió végpont egyelőre nem támogatják a eszköz hitelesítési mobil- és natív alkalmazások, például az iOS és az Android rendszeren futó natív alkalmazások.  Ez lehet letiltása a natív alkalmazás hívja fel a Microsoft Graph bizonyos szervezetek számára.  Eszköz hitelesítés szükség, ha a rendszergazda állítja be egy eszköz-alapú feltételes hozzáférési házirend-alkalmazás.  A 2.0-s verzió végpont a legvalószínűbb feltételes Access eszköz-alapú rendszergazda házirend beállítása a Microsoft Graph, például az Outlook API erőforrás.  Ha a rendszergazda állítja be ezt a házirendet, és a natív alkalmazás kéri a Microsoft Graph jogkivonat, a kérelem végül meghiúsul, mivel a eszköz hitelesítési még nem támogatott.  A Microsoft Graph tokenek kérő webalkalmazások azonban támogatottak, amikor eszköz-alapú házirend konfigurálva van.  A webhely alkalmazás forgatókönyv eszköz hitelesítési történik, a felhasználó egyszerű böngészőn keresztül.

Fejlesztői, akkor valószínűleg nincs hozzáféréssel rendelkeznek fölé házirendek vannak beállítva, Microsoft Graph-erőforrások, vagy akár szem előtt, amikor ez történik, amikor.  A munkahelyi és iskolai felhasználóknak az alkalmazások hoz létre, ha [az eredeti Azure AD-végpontot](active-directory-developers-guide.md) mindaddig, amíg a 2.0-s verzió végpont támogató eszköz hitelesítési kell használni.  További információt a feltételes access eszköz-alapú tanulmányozása: [Ez a cikk](active-directory-conditional-access.md#device-based-conditional-access)

##### <a name="windows-integrated-authentication-for-federated-tenants"></a>A szövetséges bérlők a Windows-hitelesítés
Ha a Windows-alkalmazást használta az ADAL (és ezért az eredeti Azure AD-végpontot), előfordulhat, hogy léptek a SAML állítás támogatás úgynevezett előnyeit.  A támogatás lehetővé teszi, hogy a felhasználók: szövetséges Azure Active Directory bérlők csendes hitelesíteni a helyszíni Active Directory-példány anélkül, hogy adja meg a hitelesítő adatait.  A SAML állítás támogatás jelenleg nem támogatja a 2.0-s verzió végpont.
