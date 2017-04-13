<properties
    pageTitle="Az Azure Active Directory 2.0-s verzió végpontot módosításaira |} Microsoft Azure"
    description="Az alkalmazás modell 2.0-s verzió nyilvános előzetes protokollhoz elvégzett módosításokat leírását."
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

# <a name="important-updates-to-the-v20-authentication-protocols"></a>Fontos frissítéseket szeretne a 2.0-s verzió hitelesítési protokollok
Beavatkozást fejlesztők! A következő két hétben azt végez néhány frissítéseket a 2.0-s verzió jelenthetik, az összes alkalmazás, a betekintő időszakban írt változások megszakítása hitelesítési protokollok szeretne.  

## <a name="who-does-this-affect"></a>Kik ez hatással van?
Összes alkalmazás használatához a 2.0-s verzió írt átszervezve hitelesítési végpontot,

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize
```

További információt a 2.0-s verzió végponton található [Itt](active-directory-appmodel-v2-overview.md).

Ha egy alkalmazást a 2.0-s verzió végpont segítségével közvetlenül az a 2.0-s verzió protokoll coding készített van, a OpenID csatlakozás vagy OAuth webes middlewares bármelyikét használja, vagy 3 fél tárakat való hitelesítés, hajtsa végre, kell készíteni a projektek tesztelésére, és végezze el a módosításokat, ha szükséges.

## <a name="who-doesnt-this-affect"></a>Akik nem ez van?
A gyártási Azure Active Directory authentication végpont ellen írt összes alkalmazás

```
https://login.microsoftonline.com/common/oauth2/authorize
```

A protokoll követ van beállítva, és fog nem valamilyen el a módosításokat.

Továbbá ha az alkalmazás **csak** a ADAL tár való hitelesítéshez, akkor nem kell módosítható.  ADAL van árnyékolt az alkalmazás a módosításokat.  

## <a name="what-are-the-changes"></a>Mik azok a módosítások?
### <a name="removing-the-x5t-value-from-jwt-headers"></a>A x5t érték eltávolítása JWT fejlécek
A 2.0-s verzió végpont használja JWT tokenek öröklődést, megfelelő metaadatokkal kapcsolatos a token paraméterek fejlécszakasz tartalmazó.  A fejléc, az aktuális JWTs egyikének vissza, ha szeretné keresése hasonló:

```
{ 
    "type": "JWT",
    "alg": "RS256",
    "x5t": "MnC_VZcATfM5pOYiJHMba9goEKY",
    "kid": "MnC_VZcATfM5pOYiJHMba9goEKY"
}
```

A "x5t" és a "kid" tulajdonságok hol azonosítása: a nyilvános kulcs, amellyel a token aláírást, érvényesíteni a metaadat-alapú OpenID csatlakozás végpont beolvasása.

A változás azt az alábbi készít, hogy távolítsa el a "x5t" tulajdonságot.  Továbbra is használhatják az azonos mechanizmusok tokenek érvényesítéséhez, de érdemes támaszkodhat csak a "kid" tulajdonság a helyes nyilvános kulcs OpenID csatlakozás protokollt meghatározott beolvasásához. 

> [AZURE.IMPORTANT] **A feladat: Győződjön meg arról, hogy az alkalmazás nem a x5t érték megléte függ.**

### <a name="removing-profileinfo"></a>Profile_info eltávolítása
Korábban, a 2.0-s verzió végpont van már visszaadni egy kódolt base64 JSON objektum nevű jogkivonat válaszok `profile_info`.  Amikor a 2.0-s verzió végpont egy kérést küld a egy hozzáférési jogkivonat kérnek:

```
https://login.microsoftonline.com/common/oauth2/v2.0/token
```

A válasz a következő JSON objektum lenne:
```
{ 
    "token_type": "Bearer",
    "expires_in": 3599,
    "scope": "https://outlook.office.com/mail.read",
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
    "refresh_token": "OAAABAAAAiL9Kn2Z27UubvWFPbm0gL...",
    "profile_info": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
}
```

A `profile_info` érték található információt a felhasználó, aki jelentkezett be az alkalmazás - a megjelenítendő név, keresztnév, Vezetéknév, e-mail címét, azonosító, és így tovább.  Célozza a `profile_info` jogkivonat gyorsítótárazás használt és megjelenítése céljából.

Azt most eltávolítja a `profile_info` érték – de ne aggódjon, azt is továbbra is tájékoztatása a fejlesztők némileg eltérő helyen.  Helyett `profile_info`, a 2.0-s verzió végpont most ad vissza egy `id_token` minden jogkivonat válaszként:

```
{ 
    "token_type": "Bearer",
    "expires_in": 3599,
    "scope": "https://outlook.office.com/mail.read",
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
    "refresh_token": "OAAABAAAAiL9Kn2Z27UubvWFPbm0gL...",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
}
```

Előfordulhat, hogy dekódolását és elemezni az id_token beolvasásához, amelyek forrása profile_info ugyanazokat az adatokat.  A id_token egy JSON webes jogkivonat (JWT), OpenID csatlakozás által meghatározott tartalmával.  Ennek a kódot úgy kell nagyon hasonlít – egyszerűen csak meg kell a id_token középső (a szervezet) szakaszában bontsa ki és base64 dekódolását azt a JSON-objektummal eléréséhez.

A következő két hétben kód kell az alkalmazást, vagy a felhasználói adatok lekérése a `id_token` vagy `profile_info`; amelyik szerepel.  Ebben az esetben, amikor módosítja az alkalmazás zökkenőmentes kezelhető való áttérés `profile_info` való `id_token` megszakítás nélkül.

> [AZURE.IMPORTANT] **A feladat: Győződjön meg arról, hogy az alkalmazás nem függő megléte a `profile_info` értéket.**

### <a name="removing-idtokenexpiresin"></a>Id_token_expires_in eltávolítása
Hasonló `profile_info`, azt is eltávolítása a `id_token_expires_in` válaszok paramétert.  A 2.0-s verzió végpont korábban adja vissza az érték `id_token_expires_in` együtt minden egyes id_token válasz, például a választ engedélyezése:

```
https://myapp.com?id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...&id_token_expires_in=3599...
```

Vagy jogkivonat választ:

```
{ 
    "token_type": "Bearer",
    "id_token_expires_in": 3599,
    "scope": "openid",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
    "refresh_token": "OAAABAAAAiL9Kn2Z27UubvWFPbm0gL...",
    "profile_info": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
}
```

A `id_token_expires_in` érték jelezné, hogy hány másodperc a id_token továbbra is érvényes.  Most, hogy eltávolítása a `id_token_expires_in` teljesen érték.  Ehelyett használhatja a csatlakozás OpenID standard `nbf` és `exp` követelések szemügyre veszi a egy id_token érvényességét.  További információt a [2.0-s verzió jogkivonat hivatkozás](active-directory-v2-tokens.md) nézze meg ezeket a követelések.

> [AZURE.IMPORTANT] **A feladat: Győződjön meg arról, hogy az alkalmazás nem függő megléte a `id_token_expires_in` értéket.**


### <a name="changing-the-claims-returned-by-scopeopenid"></a>A hatókör által visszaadott jogcímalapú módosítása = openid
Ez a változás valójában a legjelentősebb – lesz, szinte minden alkalmazást a 2.0-s verzió végpont használó befolyásolja.  Sok alkalmazás kérést küld a 2.0-s verzió végpont használata a `openid` hatókör, például:

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=...
&redirect_uri=...
&response_mode=form_post
&response_type=id_token
&scope=openid offline_access https://outlook.office.com/mail.read
```

Ma, amikor a felhasználó adja meg a felhasználó hozzájárul ahhoz a `openid` hatókörét, az alkalmazás különböző információkat a felhasználó megkapja az eredményül kapott id_token.  Ezek a jogcímalapú is elhelyezhet, a nevére, előnyben részesített felhasználónevét, e-mail címét, Objektumazonosító és az egyéb.

A frissítés, az adatok módosításával azt, hogy a `openid` hatókör számára biztosítja az alkalmazás hozzáférést, jobban comform való megfelelése OpenID csatlakozni.  A `openid` hatókör fog csak lehetővé teszi az alkalmazását a felhasználónak bejelentkezés, és az alkalmazás-specifikus azonosító esetében a felhasználó kap a `sub` formál, a id_token.  A csak egy id_token követelések a `openid` nyújtott hatókör lesz személyazonosításra alkalmas adatokat nem tartalmaz.  Példa id_token követelések a következők:

```
{ 
    "aud": "580e250c-8f26-49d0-bee8-1c078add1609",
    "iss": "https://login.microsoftonline.com/b9410318-09af-49c2-b0c3-653adc1f376e/v2.0 ",
    "iat": 1449520283,
    "nbf": 1449520283,
    "exp": 1449524183,
    "nonce": "12345",
    "sub": "MF4f-ggWMEji12KynJUNQZphaUTvLcQug5jdF2nl01Q",
    "tid": "b9410318-09af-49c2-b0c3-653adc1f376e",
    "ver": "2.0",
}
```

Ha azt szeretné, az alkalmazás a felhasználó személyes azonosításra (adat) juthat, az alkalmazás kell további engedélyek kérése a felhasználótól.  Azt is OpenID csatlakozás specifikáció – a két új hatókörök támogatása bemutatása a `email` és `profile` hatókörök – ezt engedélyezi.

- A `email` rendkívül egyszerű hatókör – az alkalmazás hozzáférést a felhasználók elsődleges e-mail címét keresztül lehetővé teszi a `email` a id_token lévő igényt.  Megjegyzendő, hogy a `email` állítást nem mindig lesznek id_tokens jelen – csak akkor lesz része Ha a felhasználói profil érhető el.
- A `profile` hatókör számára biztosítja az alkalmazás hozzáférést a felhasználó – a nevét, az előnyben részesített felhasználónevét, minden egyéb alapvető adatainak objektumazonosító, és így tovább.

Ez lehetővé teszi, hogy az alkalmazás minimális közzétételi módon kód – megkérheti a felhasználó csak az számára, hogy az alkalmazás a feladat végrehajtásához szükséges információkat.  Ha szeretné folytatni a felhasználói adatok kapó az alkalmazás jelenleg a skype_for_businesshoz első, kell szerepeltetni kívánt összes három hatókör a hitelesítési kérések:

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=...
&redirect_uri=...
&response_mode=form_post
&response_type=id_token
&scope=openid profile email offline_access https://outlook.office.com/mail.read
```

Az alkalmazás előkészületek nélkül használhatja a Küldés a `email` és `profile` azonnal hatókörök és a 2.0-s verzió végpont fogadja el a következő két hatókörök, és kezdje el az engedélyeket igénylő a felhasználó szükség szerint.  Azonban értelmezésében módosítása a `openid` hatókör akkor lépnek életbe, pár hétre.

> [AZURE.IMPORTANT] **A feladat: Adja hozzá a `profile` és `email` hatókörök, ha az alkalmazás csak a felhasználó adatai.**  Figyelje meg, hogy ADAL fog tartalmazni a következő engedélyeket összehívásokban alapértelmezés szerint. 

### <a name="removing-the-issuer-trailing-slash"></a>A záró perjel kibocsátó eltávolítása.
A korábban a kibocsátó értéket, amely kitűnik a 2.0-s verzió végpont tokenek került-e az űrlap

```
https://login.microsoftonline.com/{some-guid}/v2.0/
```

Ha a globálisan egyedi azonosítója volt a tenantId a az Azure AD-bérlő a token kiadó.  Ezek a változások a kibocsátó érték válik.

```
https://login.microsoftonline.com/{some-guid}/v2.0 
```

mindkét tokenek, és a csatlakozás OpenID felderítési dokumentum.

> [AZURE.IMPORTANT] **A feladat: Győződjön meg arról, hogy az alkalmazás kibocsátó ellenőrzés során fogadja el a kibocsátó értéket tartalmazó és perjellel nélkül is.**

## <a name="why-change"></a>Miért módosítani?
Az elsődleges motivációját bemutatása a módosítások is felel meg a csatlakozás OpenID szabvány.  OpenID csatlakozás kompatibilis azt azt kívánom lekicsinyítheti a személyes Microsoft-szolgáltatásokkal, és a iparágban más identitás szolgáltatásokkal való integrálásának közötti különbségeket.  Ahhoz, hogy a fejlesztők anélkül, hogy módosítja a tárakat a kedvenc Megnyitás hitelesítési tárak használata Microsoft különbségeinek szeretnénk.

## <a name="what-can-you-do"></a>Mit tud tehát tenni?
A mai megkezdheti végez az összes módosítás fentebb ismertetett.  Azonnal kell:

1.  **Függőségek eltávolítani a `x5t` élőfej paraméter.**
2.  **Biztonságos kezelését átállás `profile_info` való `id_token` jogkivonat válaszaiban.**
3.  **Függőségek eltávolítani a `id_token_expires_in` válasz paraméter.**
3.  **Adja hozzá a `profile` és `email` hatókörök a alkalmazásba, ha az alkalmazás alapszintű felhasználói információra van szüksége.**
4.  **Fogadja el a tokenek és anélkül perjellel kibocsátó értékeket.**

A [2.0-s verzió protocol dokumentációja](active-directory-v2-protocols.md) már frissítette a módosítások megfelelően, így a kód segítségével referenciaként használhatja.

Ha a módosításokat a hatókör bármilyen további kérdései vannak, kérjük, nyugodtan keresse meg a kapcsolatfelvételi a Twitteren, a @AzureAD.

## <a name="how-often-will-protocol-changes-occur"></a>Milyen gyakran történik a protokoll módosítások?
A hitelesítési protokollok bármilyen további megszakítása módosításaira nem előre.  Ezek a változások be egy megjelenés szándékosan azt is hozzákapcsolása, hogy nem kell újra bármikor hamarosan frissítési folyamat a következő típusú folyamatát.  Természetesen folytatjuk szolgáltatások hozzáadása a minősítőben 2.0-s verziója, amely használatba veheti a hitelesítési szolgáltatás, de ezekhez a változtatásokhoz kell adalékanyag, és nem létező kód oldaltörés.

Végezetül dolog, amit ki szeretné próbálni az előnézeti időszakban Köszönjük, azaz szeretné azt.  A háttérismeretek és szolgáltatások: a korai bevezetés lett különösen idáig, és azt a projektet eddigieknek megfelelően a vélemények és elképzelések megosztására.

Boldog coding!

A Microsoft identitás osztás
