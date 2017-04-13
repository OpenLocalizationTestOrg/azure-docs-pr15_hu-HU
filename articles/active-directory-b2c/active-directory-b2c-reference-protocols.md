<properties
    pageTitle="Azure Active Directory B2C |} Microsoft Azure"
    description="Hogyan lehet alkalmazások készíthet közvetlenül az Azure Active Directory B2C által támogatott protokollok használatával."
    services="active-directory-b2c"
    documentationCenter=""
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/22/2016"
    ms.author="dastrock"/>

# <a name="azure-ad-b2c-authentication-protocols"></a>Azure Active Directory B2C: Hitelesítési protokollok

Azure Active Directory (Azure Active Directory) B2C funkció identitás-alkalmazások szolgáltatásként, két iparágban szabványos protokollt támogató: OpenID kapcsolatot teremthetnek OAuth 2.0-s verziója. A szolgáltatás szabványok-kompatibilis, de ezek a protokollok bármely két példányait beállíthatja, hogy finom különbségek.  Az adatokat a tartalom hasznos lehet ahhoz, hogy lesz, ha a írhatja a kódot közvetlenül küldése, és a HTTP-kérések kezelése, hogy használata helyett egy megnyitott adatforrás beállításai párbeszédpanelen. Azt javasoljuk, olvassa el a lapon, az egyes adott protokoll részleteit kördiagramjaira előtt. De ha már ismeri a Azure Active Directory B2C, láthatja el közvetlenül [a protokoll útmutatók](#protocols).

<!-- TODO: Need link to libraries above -->

## <a name="the-basics"></a>Az alapvető műveletei
Azure Active Directory B2C használó minden app regisztrálva kell lennie az [Azure portál](https://portal.azure.com)B2C címtárában. Az alkalmazás regisztrációs folyamat gyűjti össze, és több érték rendel az alkalmazást:

- **Azonosítója** egyedileg azonosító az alkalmazást.
- **Átirányítás URI** vagy **csomag azonosító** irányítsa át a válaszokat vissza az alkalmazás a használható.
- Néhány egyéb forgatókönyv kötött értékek. További tudnivalók [az alkalmazás regisztrálása](active-directory-b2c-app-registration.md).

Miután regisztrálta az alkalmazást, úgy kommunikál az Azure Active Directory kérelem küld a 2.0-s verzió végpont:

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize
https://login.microsoftonline.com/common/oauth2/v2.0/token
```

Szinte minden OAuth és csatlakozás OpenID flow négy fél is részt az exchange:

![OAuth 2.0-s szerepkörök](./media/active-directory-b2c-reference-protocols/protocols_roles.png)

- A **kiszolgálói hitelesítési** az Azure Active Directory 2.0-s verzió végpontot. Biztonságos kezeli bármi felhasználói adatok és a hozzáféréssel kapcsolatban. A folyamat felek adatvédelmi kapcsolatának is kezeli. A felhasználó személyazonosság ellenőrzésének, megadásának és általános erőforrások hozzáférésének visszavonása és tokenek kiállító felelős. Más néven a identitásszolgáltató.
- Az **erőforrás-tulajdonos** rendszerint a végfelhasználó. A fél, amely a tulajdonosa az adatokat, és rendelkezik, hogy adatok vagy az erőforrás eléréséhez harmadik felek engedélyezése a power.
- Az **ügyfél OAuth** az alkalmazás. Azt azonosítják az alkalmazás-azonosítóval. Érdemes általában a fél, amely a végfelhasználók használata. A hitelesítés kiszolgálóról tokenek is kér. Az erőforrás tulajdonosa engedélyt kell beállítania az ügyfél az erőforrás eléréséhez.
- Az **erőforrás-kiszolgáló** , amely az erőforrás-adatokat tartalmazza. A engedélyezési kiszolgáló biztonságos hitelesítést végezni, és engedélyezheti az OAuth ügyfél megbízhatónak. Annak érdekében, hogy egy erőforrás is hozzáférést bearer hozzáférési jogkivonat is használja.

## <a name="policies"></a>Házirendek
Arguably az Azure Active Directory B2C házirendek a szolgáltatás legfontosabb jellemzőit. Azure Active Directory B2C a szabványos OAuth 2.0-s és csatlakozás OpenID protokollok házirendek bevezetésével terjed ki. Ezek lehetővé teszik az Azure Active Directory B2C sokkal több, mint az egyszerű hitelesítési és engedélyezési végrehajtásához. Házirendek teljesen ismertetik a fogyasztói identitás élményt,-előfizetés, beleértve a bejelentkezési és a profil szerkesztése. Házirend-rendszergazdai felhasználói felület definiálható. Speciális lekérdezési paraméter használatával a HTTP-hitelesítési kérések végrehajthatók. Házirendek sem szokványos funkciók OAuth 2.0-s és OpenID csatlakozni, így kell tennie az idő megértéséhez őket. További tudnivalókért lásd: az [Azure Active Directory B2C házirend megfelelőire](active-directory-b2c-reference-policies.md).

## <a name="tokens"></a>Tokenek
Az Azure Active Directory B2C végrehajtásának OAuth 2.0-s és OpenID csatlakozás helykihasználást eredményez teljes körű bearer tokenek, többek között a bearer tokenek, amely van ábrázolva JSON webes tokenek (JWTs). Egy bearer jogkivonathoz egy lightweight biztonsági jogkivonat, amely a "bearer" hozzáférést biztosít a védett erőforrás. A bearer bármely fél, amely a token bemutathatja. Azure Active Directory kell először hitelesítést végezni a féltől megkaphatja a bearer jogkivonathoz előtt. De ha a szükséges lépéseket nem vesznek biztonságossá tétele a token továbbítása, tárterület, azt is hozzá és egy várt fél által használt.

Néhány biztonsági tokenek van egy beépített mechanizmusa, hogy megakadályozza, hogy a jogosulatlan felek használja őket, de nincs bearer tokenek Ez az eljárás. Azok a biztonságos csatornát, például átviteli réteg biztonsági (HTTPS) kell szállított. Ha egy bearer jogkivonathoz kívüli biztonságos csatornát, a kártékony féltől segítségével egy férfi középső homonimaszerű webcímmel szerezheti be a token, és ezzel az illetéktelen eléréséhez védett erőforrás használni. Azonos biztonsági elvek alkalmazni, a rendszer bearer tokenek tárolt vagy gyorsítótárazott későbbi felhasználás céljából. Mindig győződjön meg arról, hogy az alkalmazás továbbítja, és biztonságos módon bearer tokenek tárolja.

További bearer jogkivonat biztonsági megfontolások [RFC 6750 szakasz 5](http://tools.ietf.org/html/rfc6750)című témakör tartalmaz.

További részletek az Azure Active Directory B2C használt tokenek különböző típusú [jogkivonat Azure AD-hivatkozást](active-directory-b2c-reference-tokens.md)érhetők el.

## <a name="protocols"></a>Protokollok

Ha készen áll, tekintse át a néhány példa kérelmeket, kiindulhat egy az alábbi oktatóanyagok. Példa az adott hitelesítési egyenként megfelel. Ha a meghatározásában mely folyamat szüksége segítségre van szüksége, olvassa el [az alkalmazások típusú nyújthat Azure Active Directory B2C használatával](active-directory-b2c-apps.md).

- [Mobil- és natív alkalmazások készíthet OAuth 2.0 használatával](active-directory-b2c-reference-oauth-code.md)
- [Web Apps alkalmazások készítése OpenID kapcsolódás használatával](active-directory-b2c-reference-oidc.md)
