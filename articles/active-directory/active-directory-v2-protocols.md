<properties
    pageTitle="Azure Active Directory 2.0-s verzió protokollok |} Microsoft Azure"
    description="Az Azure Active Directory 2.0-s verzió végpontot által támogatott protokollok útmutatóját."
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

# <a name="v20-protocols---oauth-20--openid-connect"></a>2.0-s verzió protokollok - OAuth 2.0-s & OpenID csatlakoztatása

A 2.0-s verzió végpont Azure Active Directory identitás-mint-a-szolgáltatás ipari szabványos protokollok, OpenID csatlakozás és a OAuth 2.0 használhatja.  A szolgáltatás ugyan szabványok-kompatibilis, ezek a protokollok bármely két példányait finom eltérések lehet.  Az adatok itt fog lehet hasznos, ha úgy dönt, hogy közvetlenül küldésével írja be a kódot, és a HTTP-kérések vagy használata egy 3rd fél kezelése nyissa meg a kép helyett használja a Megnyitás tárak közül.
<!-- TODO: Need link to libraries above -->

> [AZURE.NOTE]
    Nem minden Azure Active Directory esetek és szolgáltatások 2.0-s verzió végpont által támogatott.  Annak megállapításához, ha a 2.0-s verzió végpont kell használnia, [2.0-s verzió korlátozások](active-directory-v2-limitations.md)olvashat.

## <a name="the-basics"></a>Az alapvető műveletei
Szinte minden OAuth & OpenID csatlakozás flow vannak négy fél részt az exchange:

![OAuth 2.0-s szerepkörök](../media/active-directory-v2-flows/protocols_roles.png)

- A **Kiszolgáló engedélyezési** az 2.0-s verzió végpontot.  Ez a felelős a biztosítja a felhasználói azonosító, megadásának és általános erőforrások hozzáférésének visszavonása és tokenek kiállító.  Más néven a identitásszolgáltató – azért, végezze el a felhasználói adatok, a hozzáférés és egy folyamat felek adatvédelmi kapcsolatának biztonságosan kezeli.
- Az **Erőforrás-tulajdonos** általában a végfelhasználói áll.  A fél, amely a tulajdonosa az adatokat, és az adott adatok vagy az erőforrás eléréséhez harmadik felek engedélyezése a power.
- Az **Ügyfél OAuth** az alkalmazás jelölt az alkalmazás azonosítója.  Általában az, hogy hogyan kommunikáljon a végfelhasználói fél, és tokenek kér a engedélyezési kiszolgáló.  Az ügyfél jogosultsággal kell rendelkeznie az erőforrás eléréséhez szükséges engedély az erőforrás tulajdonosa.
- Az **Erőforrás-kiszolgáló** , amely az erőforrás-adatokat tartalmazza.  Bizalmi kapcsolatok a engedélyezési kiszolgáló biztonságos hitelesítheti, és az OAuth ügyfél engedélyezheti, és Bearer access_tokens használja annak érdekében, hogy egy erőforrás eléréséhez lehet adni.


## <a name="app-registration"></a>Alkalmazás regisztráció
Minden alkalmazást a 2.0-s verzió végpont használó kell regisztrált a [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) , használhatják a OAuth vagy OpenID csatlakozás előtt.  Az alkalmazás regisztrációs folyamat fog összegyűjtése és több érték hozzárendelése az alkalmazást:

- Egy egyedi módon azonosító az alkalmazás **Azonosítója**
- Egy **Átirányítás URI** vagy irányítsa át a válaszokat vissza az alkalmazás a használható **Csomag azonosító**
- Néhány egyéb forgatókönyv kötött értékek.

További részletekért megtudhatja, hogyan lehet [rögzíteni az alkalmazás](active-directory-v2-app-registration.md).

## <a name="endpoints"></a>Végpontok
Miután regisztrálta, az alkalmazás kommunikál Azure Active Directory kérelem küld a a 2.0-s verzió végpont:

```
https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize
https://login.microsoftonline.com/{tenant}/oauth2/v2.0/token
```

Ha a `{tenant}` is eltarthat, négy különböző értékek közül:

| Érték | Leírás |
| ----------------------- | ------------------------------- |
| `common` | Lehetővé teszi, hogy a felhasználók a személyes Microsoft-fiók és a munkahelyi vagy iskolai fiókokat az Azure Active Directory jelentkezhet be az alkalmazás. |
| `organizations` | Az Azure Active Directory jelentkezhet be az alkalmazás lehetővé teszi, hogy csak a munkahelyi vagy iskolai fiókkal rendelkező felhasználók. |
| `consumers` | Lehetővé teszi, hogy csak azok a felhasználók, a személyes Microsoft-fiókok (MSA) jelentkezhet be az alkalmazás. |
| `8eaef023-2b34-4da1-9baa-8bc8c9d6a490`vagy`contoso.onmicrosoft.com` | Az egy adott Azure Active Directory-bérlőből jelentkezhet be az alkalmazás lehetővé teszi, hogy csak a munkahelyi vagy iskolai fiókkal rendelkező felhasználók.  A rövid tartománynevét az Azure AD-bérlő vagy a bérlő guid azonosítója használható.  |

További tájékoztatást a vezérléséhez ezeket a végpontokat válassza az adott alkalmazás írja be az alábbi.

## <a name="tokens"></a>Tokenek
A 2.0-s verzió végrehajtásának OAuth 2.0-s és OpenID csatlakozás kell használniuk teljes körű bearer tokenek, többek között a bearer tokenek JWTs ábrázolva. Egy bearer jogkivonathoz a könnyű biztonsági jogkivonat, amely a "bearer" hozzáférést biztosít a védett erőforrás. Ebben az értelemben "bearer" bármely fél, amely a token bemutathatja. Bár kell fél Azure AD az bearer jogkivonathoz fogadásához először hitelesíteni, ha a szükséges lépéseket nem vesznek biztonságossá tétele a token továbbítása, tárterület, hozzá, és egy várt fél használják. Néhány biztonsági tokenek van a beépített kisalkalmazások megakadályozza, hogy a jogosulatlan felek használja őket, miközben a bearer tokenek Ez az eljárás nem rendelkezik, és egy biztonságos csatornát, például átviteli réteg biztonsági (HTTPS) kell lennie szállított. Egy bearer jogkivonathoz törlése küldött a rendszer, egy férfi a a középső név kezdőbetűjét homonimaszerű webcímmel használható rosszindulatú fél szerezheti be a token és használni szeretné az illetéktelen hozzáférést egy védett erőforrásra. Azonos biztonsági elvek alkalmazása tárolásáról vagy gyorsítótárazásának bearer tokenek későbbi felhasználás céljából. Mindig győződjön meg arról, hogy az alkalmazás továbbítja, és biztonságos módon bearer tokenek tárolja. További biztonsági megfontolások a bearer tokenek [RFC 6750 szakasz 5](http://tools.ietf.org/html/rfc6750)című témakör tartalmaz.

A különböző típusú a 2.0-s verzió végponton használt tokenek további részleteket [a 2.0-s verzió végpont jogkivonat](active-directory-v2-tokens.md)hivatkozás érhető el.

## <a name="protocols"></a>Protokollok

Ha készen áll a talál néhány példa kérések, – első lépések közül az alábbi oktatóanyagok.  Példa az adott hitelesítési egyenként megfelel.  Ha segítségre van szüksége annak megállapítása, hogy melyiket érdemes-e meg a megfelelő folyamat, olvassa el [az alkalmazások és a 2.0-s vagy típusú](active-directory-v2-flows.md).

- [Mobil és OAuth 2.0-s natív alkalmazás összeállítása](active-directory-v2-protocols-oauth-code.md)
- [Webes összeállítása megnyitott azonosítójú alkalmazások csatlakoztatása](active-directory-v2-protocols-oidc.md)
- [A 2.0-s OAuth Implicit folyamat az egy oldalra alkalmazások készítése](active-directory-v2-protocols-implicit.md)
- [Szerkesztés démonok vagy Server egymás folyamatok OAuth 2.0 ügyfél hitelesítő Flow](active-directory-v2-protocols-oauth-client-creds.md)
- Kapcsolatfelvétel egy webes API az OAuth 2.0-s más nevében Flow (hamarosan) a tokenek

<!-- - Get tokens using a username & password with the OAuth 2.0 Resource Owner Password Credentials Flow (coming soon) --> 
