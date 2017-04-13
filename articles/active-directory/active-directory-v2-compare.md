<properties
    pageTitle="Az Azure Active Directory 2.0-s verzió végpontot |} Microsoft Azure"
    description="Az eredeti Azure AD-összehasonlítására és a 2.0-s verzió végpontok."
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
    ms.date="09/16/2016"
    ms.author="dastrock"/>

# <a name="whats-different-about-the-v20-endpoint"></a>Mi a különbség a 2.0-s verzió végpont kapcsolatban?

Ha jártas Azure Active Directory vagy integrálva van alkalmazások múltbeli Azure AD, lehetnek eltérések a az 2.0-s verzió végpontot, amely nem a várt érték.  A dokumentum hívja meg azokat a különbségeket a megértéséhez.

> [AZURE.NOTE]
    Nem minden Azure Active Directory esetek és szolgáltatások 2.0-s verzió végpont által támogatott.  Annak megállapításához, ha a 2.0-s verzió végpont kell használnia, [2.0-s verzió korlátozások](active-directory-v2-limitations.md)olvashat.


## <a name="microsoft-accounts-and-azure-ad-accounts"></a>Microsoft-fiókok és Azure Active Directory-fiókok
a 2.0-s verzió végpont lehetővé teszi a fejlesztők fogadja el a Bejelentkezés Microsoft Accounts és Azure Active Directory fiókokból, használja a egyetlen auth végpont alkalmazásai írni.  Ez lehetővé teszi az azt jelenti, hogy az alkalmazás teljesen fiók-felismerése nélkül; írása milyen típusú fiókot, amely a felhasználó bejelentkezik az-vizsgálatot lehet.  Természetesen akkor *is* , hogy az alkalmazás egy adott munkamenetben használt fiók típusától tudatában, de nem kell.

Például ha az alkalmazás felhívja a [Microsoft Graph](https://graph.microsoft.io), néhány további funkciókat és az adatok lesz elérhető vállalati felhasználóknak, például a SharePoint-webhelyek vagy a címtár-adatok.  De sok műveletek, például [a felhasználó levelek olvasása](https://graph.microsoft.io/docs/api-reference/v1.0/resources/message), a kód írhatók pontosan azonos a Microsoft Accounts és Azure Active Directory-fiókok.  

Az alkalmazás integrálása az Azure Active Directory és a Microsoft Accounts már egy egyszerű folyamat.  Használhatja a végpontok, egyetlen tárban és egyetlen alkalmazás regisztrációkor egyetlen csoportja a világon a fogyasztói és vállalati eléréséhez szükséges.  A 2.0-s verzió végpont kapcsolatos további információért olvassa el [a](active-directory-appmodel-v2-overview.md).


## <a name="new-app-registration-portal"></a>Új alkalmazás regisztrációs portál
a 2.0-s verzió végpont csak egy új helyre regisztrálását: [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList).  A portál, ahol fog kapni az alkalmazás azonosítója, ez az alkalmazás bejelentkezési lapján, és így tovább megjelenését.  Elérheti a portált kell kapcsolva Microsoft-fiók – személyes vagy a munkahelyi vagy iskolai fiókjával.  

További és több funkciót hozzáadása a alkalmazás regisztrációs portál időbeli folytatjuk.  Célja, hogy a portál lesz-e az új helyen, ahol látogathat bármely és minden, hogy a Microsoft-alkalmazásokat a kezelésére.


## <a name="one-app-id-for-all-platforms"></a>Az összes platformokon egy alkalmazás azonosítója
Az eredeti Azure Active Directory-szolgáltatás előfordulhat, hogy regisztrálta számos különböző alkalmazások egyetlen projektre vonatkozóan.  A natív ügyfelek és web Apps alkalmazások használata a különálló regisztrációk kellett meg:

![Régi alkalmazás Regisztrálás a felhasználói felület](../media/active-directory-v2-flows/old_app_registration.PNG)

Például ha egy webhelyet, mind az iOS-alkalmazás készített, kellett regisztrálni őket külön-külön, két különböző Alkalmazásazonosítók használatával.  Ha egy webhelyet, és a webes api kódmentes, előfordulhat, hogy regisztrálta minden, különálló alkalmazásként az Azure Active Directory.  Ha az iOS-alkalmazásokban és az Android-alkalmazásokban, is előfordulhat, hogy regisztrálta két különböző alkalmazás.  

<!-- You may have even registered different apps for each of your build environments - one for dev, one for test, and one for production. -->

Most szükség, egyetlen alkalmazás regisztráció és az egyes projektjeit egyetlen alkalmazás azonosítót.  Több "platformokon" hozzáadása minden projekt, és adja meg a megfelelő adatokat platformokon hozzáadása.  Természetesen hozhat létre, amely tetszik, attól függően, hogy az igényeknek megfelelően alakíthatja, de a legtöbb esetben csak egy azonosítója kell, hogy minél több alkalmazásokat.

<!-- You can also label a particular platform as "production-ready" when it is ready to be published to the outside world, and use that same Application Id safely in your development environments. -->

Az célja, hogy ezzel vezethetnek egy további egyszerűsített alkalmazás kezelése és a fejlesztői funkcióit, és egy projekt, amely akkor működik a további összevont nézetének létrehozása.


## <a name="scopes-not-resources"></a>Keresési tartományok, nem erőforrások
Az eredeti Azure AD-szolgáltatás alkalmazás **erőforrás**, vagy egy címzett tokenek módon működnek.  Erőforrás definiálhatók **hatókörök** és értelmezésére képes, **oAuth2Permissions** számos lehetővé tevő ügyfél alkalmazások részhalmazát hatókörök erőforráshoz tokenek kérhet.  Fontolja meg egy erőforrás példaként az Azure Active Directory Graph API:

- Erőforrás-azonosító vagy `AppID URI`:`https://graph.windows.net/`
- Keresési tartományok, vagy `OAuth2Permissions`: `Directory.Read`, `Directory.Write`stb.  

Mindezek igaznak számára a 2.0-s verzió végpontot.  Az alkalmazás továbbra is működnek, erőforrásként keresési tartományok definiálása és egy URI azonosítania kell magát.  Ügyfélalkalmazások továbbra is e hatókörök hozzáférést kérhet.  Azonban megváltozott módja, amelyben egy ügyfél kér ezeket az engedélyeket.  OAuth 2.0-s engedélyezheti az elmúlt, előfordulhat, hogy rendelkezik hasonlított Azure Active Directory kérelmet:

```
GET https://login.microsoftonline.com/common/oauth2/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&resource=https%3A%2F%2Fgraph.windows.net%2F
...
```

Ha az **erőforrás** paraméterrel megadott erőforrás-az ügyfél alkalmazás engedélyt kér.  Azure Active Directory számítja ki az alkalmazást, az Azure-portálon statikus konfigurációjának, és ennek megfelelően kiadott tokenek szükséges engedélyeket.  Most az azonos OAuth 2.0 engedélyezze a kérelem néz ki:

```
GET https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&scope=https%3A%2F%2Fgraph.windows.net%2Fdirectory.read%20https%3A%2F%2Fgraph.windows.net%2Fdirectory.write
...
```

Ha a **hatókör** paraméter azt jelzi, amelyben az erőforrások és engedélyeket az alkalmazás engedélyt kér. A kívánt erőforrás nagyon szerepel a kérelem - azt egyszerűen a hatókör paraméter az értékek minden egyes vonatkozik.  A hatókör paraméter használatával ily módon lehetővé teszi, hogy a további felel meg az OAuth 2.0 szabványnak 2.0-s verzió végpontot, és jobban közös ipari gyakorlatok igazítja.  Is lehetővé teszi a alkalmazások [növekményes jóváhagyását adja](#incremental-and-dynamic-consent), amely a következő szakaszban ismertetett végrehajtásához.

## <a name="incremental-and-dynamic-consent"></a>Növekményes és dinamikus hozzájárulása
Alkalmazások regisztrált a általában elérhető Azure Active Directory szolgáltatás adja meg a szükséges OAuth 2.0-s engedélyeket az Azure-portálon alkalmazás létrehozáskor szükséges:

![Engedélyek regisztrációs felhasználói felület](../media/active-directory-v2-flows/app_reg_permissions.PNG)

Az engedélyek volt szükség alkalmazás **statikus**beállítva.  Bár ez engedélyezett az alkalmazás az Azure-portálon olyan konfigurációjának és tárolni a kód szép egyszerű, így a fejlesztők számára megjelenített néhány hibát:

- Az alkalmazás kellett tudnivalók az összes alkalmazás létrehozáskor minden eddiginél kellene engedélyt.  Engedélyek hozzáadása időbeli volt túl bonyolult folyamat.
- Az alkalmazás az összes erőforrás időszakokra a volna bármikor elérheti, hogy kellett.  Nehezen is készíthet alkalmazást, amely hozzáférhet az erőforrások tetszőleges számú volt.
- Az alkalmazás a felhasználó első bejelentkezés után minden eddiginél kellene összes engedély kérése volt.  Bizonyos esetekben ez vezetett engedélyek, az alkalmazás hozzáférést a kezdeti bejelentkezés jóváhagyásáért a végfelhasználók javasolt nagyon sok listáját.

A 2.0-s verzió végponttal adhatja meg az engedélyeket az alkalmazás **dinamikusan**, futásidőben, szüksége van az alkalmazás használatát normál során.  Kéri, adja meg a tartományok, az alkalmazás igényeinek adott bármikor idő a csoportnaptárba a `scope` paramétere: a hitelesítési kérelmet:

```
GET https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&scope=https%3A%2F%2Fgraph.windows.net%2Fdirectory.read%20https%3A%2F%2Fgraph.windows.net%2Fdirectory.write
...
```

A fenti kérések engedéllyel egy Azure AD-felhasználó directory adataira, valamint adatokat írni a címtár olvasható az alkalmazást.  Ha a felhasználó számára az adott alkalmazás múltbeli hozzájárult ezeket az engedélyeket, egyszerűen írja be a saját hitelesítő adatok, és, az alkalmazás be kell jelentkeznie.  Ha a felhasználó nem járult hozzá bármilyen ezeket az engedélyeket, a 2.0-s verzió végpont kéri a felhasználó felhasználó hozzájárul ahhoz, hogy ezeket az engedélyeket.  További információért erről [engedélyeket,](active-directory-v2-scopes.md)a felhasználó hozzájárul ahhoz, és a keresési tartományok.

Dinamikusan keresztül engedélyért alkalmazás lehetővé teszi a `scope` paramétert ad a felhasználói felület teljes hozzáféréssel.  Ha kívánja, megadhatja a felhasználó hozzájárul ahhoz egy első engedélyt összehívásban összes engedélyt kér, és megoldást frontload.  Vagy ha az alkalmazás engedélyek sok van szüksége, választhatja azt gyűjtése ezeket az engedélyeket a felhasználótól fokozatosan, mint tudják használni az alkalmazást az egyes szolgáltatások adott idő alatt.

## <a name="well-known-scopes"></a>Az ismert hatókörök

#### <a name="offline-access"></a>Az offline hozzáférés
a 2.0-s verzió végpont alkalmazások – új jól ismert jogosultsági e szükség lehet a `offline_access` hatókörét.  Minden alkalmazás kell ezzel az engedéllyel kérése, ha egy hosszabb ideig, még akkor is, amikor a felhasználó előfordulhat, hogy nem kell aktívan használja az alkalmazás a felhasználó nevében erőforrások eléréséhez szükséges.  A `offline_access` hatókör fog megjelenni a felhasználó a felhasználó hozzájárul ahhoz párbeszédpanelek, mint "Az adatok offline hozzáférés", amely a felhasználónak kell fogadnia.  Kérése a `offline_access` jogosultsági lehetővé teszi a webalkalmazás az OAuth 2.0-s refresh_tokens fogadjanak az 2.0-s verzió végpontot.  Refresh_tokens élettartamú, és átviheti az új OAuth 2.0-s access_tokens hosszabb ideig, az access.  

Ha az alkalmazás nem kér a `offline_access` hatókörét, nem fog kapni refresh_tokens.  Ez azt jelenti, hogy az [OAuth 2.0-s engedélyezési kód adatfolyam](active-directory-v2-protocols.md#oauth2-authorization-code-flow)-authorization_code beváltása meg, amikor csak kap vissza egy, a access_token a `/token` végpontot.  Adott access_token egy rövid ideig (általában egy órával) érvényes marad, de végül lejár.  At pont időben, az alkalmazás a felhasználó átirányítása kell biztonsági másolatot a `/authorize` egy új authorization_code beolvasásához végpontot.  Ez az átirányítási során a felhasználó előfordulhat, hogy, vagy előfordulhat, hogy nem kell újra megadnia a hitelesítő adatait, vagy újra hozzájárul engedélyek, attól függően, az alkalmazás típusát.

Ha többet szeretne megtudni a OAuth 2.0-s, refresh_tokens és access_tokens, kivétele a [2.0-s verzió Protocol (protokoll) hivatkozást](active-directory-v2-protocols.md).

#### <a name="openid-profile--email"></a>OpenID és a profilját, és az e-mailben

Az eredeti Azure Active Directory-szolgáltatás a legalapvetőbb OpenID csatlakozás bejelentkezési folyamat volna nyújtanak különböző az eredményül kapott id_token a felhasználó adatai.  Az egy id_token követelések is elhelyezhet, a felhasználó nevét, előnyben részesített felhasználónevét, e-mail címét, Objektumazonosító és további.

Az információk korlátozása most azt is, amely a `openid` hatókör számára biztosítja az alkalmazás hozzáférést.  A "openid" hatókör csak lehetővé teszi az alkalmazását a felhasználónak bejelentkezés, és az alkalmazás-specifikus azonosító a felhasználó kap.  Ha azt szeretné, az alkalmazás a felhasználó személyes azonosításra (adat) juthat, az alkalmazás kell további engedélyek kérése a felhasználótól.  Vannak bemutatása, hogy két új hatókörök – a `email` és `profile` hatókörök – ezt engedélyezi.

A `email` rendkívül egyszerű hatókör – az alkalmazás hozzáférést a felhasználók elsődleges e-mail címét keresztül lehetővé teszi a `email` a id_token lévő igényt.  A `profile` hatókör számára biztosítja az alkalmazás hozzáférést a felhasználó – a nevét, az előnyben részesített felhasználónevét, minden egyéb alapvető adatainak objektumazonosító, és így tovább.

Ez lehetővé teszi, hogy az alkalmazás minimális közzétételi módon kód – csak a felhasználó megkérheti a fontos, hogy az alkalmazás a feladat végrehajtásához szükséges információkat.  A tartományok kapcsolatos további tudnivalókért tekintse át [a 2.0-s verzió hatókör hivatkozás](active-directory-v2-scopes.md). 

## <a name="token-claims"></a>Jogkivonat követelések

A tokenek az 2.0-s verzió végpontot által kibocsátott követelések nem lesz azonos-e a rendelkezésre álló általában által kibocsátott tokenek végpontok Azure AD - alkalmazások áttelepítését az új szolgáltatás kell nem tegyük fel, egy adott állítás id_tokens vagy access_tokens megtalálható lesz.   A 2.0-s verzió végpont által kibocsátott tokenek az OAuth 2.0-s és a csatlakozás OpenID jellemzői kompatibilis, de mint általában elérhető különböző szemantikáját előfordulhat, hogy hajtsa végre az Azure Active Directory-szolgáltatás.

Az adott jogcímeken 2.0-s verzió tokenek kibocsátott kapcsolatos további tudnivalókért lásd: a [2.0-s verzió jogkivonat-hivatkozást](active-directory-v2-tokens.md).

## <a name="limitations"></a>Korlátozások
Létezik néhány korlátozás tudatában kell lennie a 2.0-s verzió pont használata esetén.  Olvassa el a [2.0-s verzió korlátozások dokumentum](active-directory-v2-limitations.md) megtekintéséhez, ha bármelyik alábbi korlátozások vonatkoznak az adott esetben.
