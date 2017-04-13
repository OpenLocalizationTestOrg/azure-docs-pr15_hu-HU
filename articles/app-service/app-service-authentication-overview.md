<properties
    pageTitle="Hitelesítési és Azure alkalmazás szolgáltatás engedélyezési |} Microsoft Azure"
    description="Elvi hivatkozás és a hitelesítési áttekintése és engedélyezési Azure alkalmazás szolgáltatás funkciója"
    services="app-service"
    documentationCenter=""
    authors="mattchenderson"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="08/29/2016"
    ms.author="mahender"/>

# <a name="authentication-and-authorization-in-azure-app-service"></a>Hitelesítési és Azure alkalmazás szolgáltatás engedélyezési

## <a name="what-is-app-service-authentication--authorization"></a>Mi az alkalmazás szolgáltatás hitelesítési / engedély?

Alkalmazás szolgáltatás hitelesítési / engedély szolgáltatás az alkalmazás bejelentkezni a felhasználóknak, hogy nem kell az alkalmazás kódmentes kódot módosítása úgy, hogy. Ugyanígy védelme az alkalmazás és a felhasználó adatokkal végzett munkához biztosít.

Alkalmazás szolgáltatás használja az összevont identitás egy harmadik fél identitásszolgáltató tárolja a fiókok és hitelesíti a felhasználókat. Az alkalmazás, hogy az alkalmazás nem tartozik, hogy magát adatokat tároló identitás szolgáltatója támaszkodik. Alkalmazás szolgáltatás lehetővé teszi a beépített öt Identitásszolgáltatók: Azure Active Directory, a Facebook, a Google, a Microsoft-Account és a Twitteren. Az alkalmazás ezek Identitásszolgáltatók tetszőleges számú segítségével hogyan bejelentkeznek beállításokkal felhasználóinak. A beépített támogatás kibontásához integrálhatja a másik identitásszolgáltató vagy [a saját egyéni identitás megoldás][custom-auth].

Ha szeretné az első lépések azonnal, olvassa el az alábbi oktatóanyagok:

- [Hitelesítés hozzáadása az iOS-alkalmazás] [ iOS] (vagy [Android], [a Windows], [Xamarin.iOS], [Xamarin.Android], [Xamarin.Forms]vagy [Cordova])
- [Az Azure-App szolgáltatásban API-alkalmazások a felhasználói hitelesítés][apia-user]
- [Azure alkalmazás szolgáltatás – rész 2 – első lépések][web-getstarted]

## <a name="how-authentication-works-in-app-service"></a>Hogyan működik a hitelesítési App szolgáltatásban

Annak érdekében, hogy a hitelesíteni a Identitásszolgáltatók valamelyikével, először kell tudni az alkalmazás a identitásszolgáltató konfigurálása. A identitásszolgáltató majd nyújt azonosítókhoz és titkos kulcsok, hogy az alkalmazás szolgáltatás kiszámlázásakor. Ezzel a meghatalmazás befejeződött, hogy az alkalmazás szolgáltatás ellenőrizheti a felhasználó Előfeltételek, például a hitelesítési tokenek az identitás-szolgáltatótól.

Jelentkezzen be a felhasználó a fenti szolgáltatók egyikéhez használatával, a felhasználó átvált lehet, hogy a felhasználó bejelentkezik az adott szolgáltató zárólap. Ha a felhasználók, akik használják egy webböngészőben, beállíthatja, hogy alkalmazás szolgáltatás automatikusan irányítsa át az összes hitelesített felhasználó az végpontot, a felhasználó bejelentkezik. Egyéb esetben meg kell irányítsa át az ügyfelek `{your App Service base URL}/.auth/login/<provider>`, ahol `<provider>` a következő értékek egyike: aad, facebook, google, microsoft vagy twitter. Mobil és az API felhasználási területei a szakaszok, a jelen cikk című cikkben ismertetett.

A felhasználók, akik a böngészőn keresztül alkalmazás használata a cookie-k beállítani, hogy azok maradhat hitelesített, az alkalmazás tallózzák lesz. Az egyéb lekérdezésiügyfél-típusok, például a mobil, JSON webes jogkivonat (JWT), amely kell megadni a `X-ZUMO-AUTH` fejléc, az ügyfél indítandó. A Mobile-alkalmazások ügyfél SDK fogja kezelni ezt meg. Másik lehetőségként az Azure Active Directory identitás token vagy a hozzáférési jogkivonat közvetlenül szerepelni fog a `Authorization` az élőfej [bearer jogkivonathoz](https://tools.ietf.org/html/rfc6750).

Ellenőrzi, hogy a szolgáltatási alkalmazás bármely cookie-k vagy jogkivonat, hogy az alkalmazás problémáinak felhasználók hitelesítést végezni. Szeretné korlátozni, hogy ki tud hozzáférni az alkalmazás, című [engedélyt](#authorization) a jelen cikk.

### <a name="mobile-authentication-with-a-provider-sdk"></a>Egy szolgáltatót SDK mobil hitelesítés

A szolgáltatás beállítása után kattintson a kódmentes mobilügyfelek, hogy jelentkezzen be az alkalmazás szolgáltatás módosíthatók. Íme néhány az alábbi két módszer:

- Egy SDK csomagjában talál, amelyek egy adott identitásszolgáltató közzé, identitás létrehozásához, és kattintson az alkalmazás szolgáltatás eléréséhez szükséges használja.
- Kód egyetlen sor, hogy a Mobile-alkalmazások ügyfél SDK jelentkezzen be a felhasználók használata

>[AZURE.TIP] Legtöbb alkalmazással használja a szolgáltató SDK egységesebb kapnak, amikor a felhasználók jelentkezzen be, a frissítés támogatás használata, valamint más előnyökkel jár, amely meghatározza a szolgáltató.

Ha használja a szolgáltató SDK, felhasználók is jelentkezzen be az eszköz, amely több szorosan integrálódik az operációs rendszert futtató az alkalmazást az. Ezzel is kap egy szolgáltató token és az egyes felhasználói adatok az ügyfélszámítógépen, amely lehetővé teszi a sokkal egyszerűbb felhasználása graph API-hoz és a felhasználói felület testreszabását. Időnként blogok és fórumok, látni fogja ezt nevezzük a "ügyfél folyamat" vagy "ügyfél irányuló forgalom", mert az ügyfél kód bejelentkezik a felhasználók és az ügyfél kód fér hozzá a szolgáltató jogkivonat.

Miután a szolgáltató jogkivonat származik, kell el lehet küldeni a alkalmazás szolgáltatás adatérvényesítéshez. Miután alkalmazás szolgáltatás ellenőrzi a jogkivonat, a alkalmazás szolgáltatás létrehoz egy új alkalmazás szolgáltatás jogkivonat, a függvény által visszaadott az ügyfél számára. A Mobile-alkalmazások ügyfél SDK rendelkezik-e exchange kezeléséhez, és az alkalmazás kódmentes összes kérelmet automatikusan csatolása a token helper módszerek. A fejlesztők is tarthatja a szolgáltató jogkivonat mutató hivatkozás, ha úgy dönt.

### <a name="mobile-authentication-without-a-provider-sdk"></a>Mobil hitelesítés nélkül SDK szolgáltató

Ha nem szeretné, hogy állíthatja be a szolgáltató SDK, engedélyezheti a Mobile-alkalmazások funkció bejelentkezés az Azure-alkalmazás szolgáltatás. A Mobile-alkalmazások ügyfél SDK a szolgáltató a webes nézet megnyitásához, és a jelentkezzen be a felhasználó. Időnként blogok és fórumok, látni fogja ezt nevezzük a "kiszolgáló forgalom" vagy "kiszolgáló utasította forgalom", mert a kiszolgáló, amely a felhasználó bejelentkezik a folyamat kezeli, és az ügyfél SDK soha nem kap a szolgáltató jogkivonat.

Az authentication oktatóprogram platformokon kódot a folyamat indításához szerepel. A folyamat végén az ügyfél SDK-szolgáltatási alkalmazás jogkivonat, és rendelkezik a token automatikusan csatolva van az alkalmazás kódmentes összes kérelmet.

### <a name="service-to-service-authentication"></a>Szolgáltatás hitelesítés

Bár a felhasználó hozzáférést biztosíthat az alkalmazás, hívja fel a saját API egy másik alkalmazás is megbízhat. Egy webalkalmazás betárcsázni egy API-t egy másik webalkalmazás például tartozhat. Ebben az esetben használatával hitelesítő adatok-szolgáltatási fiók helyett felhasználói hitelesítő adatokat a jogkivonat első. Szolgáltatásfiók más néven egy *egyszerű szolgáltatás* az Azure Active Directory nyelvén mondják, és egy ilyen fiókot használó hitelesítés néven is ismert szolgáltatás – példa.

>[AZURE.IMPORTANT] Mobilalkalmazások ügyfél eszközön futtatni, mert mobilalkalmazások _nem_ megbízható alkalmazást, és ne használja a szolgáltatás fő folyamat darab teheti meg. Ehelyett használják a felhasználói folyamat, amely a korábban részletes volt.

Ha a szolgáltatás esetben alkalmazás szolgáltatás is védhetők az alkalmazás Azure Active Directory. A hívó alkalmazás ugyanúgy kell az Azure Active Directory szolgáltatás egyszerű jóváhagyási jogkivonat, azáltal, hogy az ügyfél, Azonosítóját és Azure Active Directoryból titkos ügyfél előállított. Ebben az esetben ASP.NET API-alkalmazások használó példa az oktatóanyagot, [szolgáltatás fő hitelesítési az API-alkalmazások]magyarázható[apia-service].

Ha szeretné kezelni szolgáltatás – példa alkalmazás szolgáltatás-hitelesítés használatával, ügyfél tanúsítványok vagy alapszintű hitelesítés is használhatja. Azure-ban ügyfél tanúsítványokkal kapcsolatos további tudnivalókért lásd [Hogyan szeretné konfigurálni TLS kölcsönös hitelesítés Web Apps alkalmazások](../app-service-web/app-service-web-configure-tls-mutual-auth.md). Alapszintű hitelesítés ASP.NET tudni lásd: a [Hitelesítési szűrők ASP.NET webes API-2](http://www.asp.net/web-api/overview/security/authentication-filters).

Szolgáltatási fiók hitelesítés-szolgáltatási alkalmazás logika alkalmazásban az API-alkalmazásokban helyzet egy speciális, hogy [az egyéni API is logika alkalmazással alkalmazás szolgáltatás használata](../app-service-logic/app-service-logic-custom-hosted-api.md)részletesen.

## <a name="authorization"></a>Az alkalmazás szolgáltatás engedélyezési működése

Ha a teljes hozzáféréssel a kérelmeket, az alkalmazás is rendelkeznek hozzáféréssel. Alkalmazás szolgáltatás hitelesítési / engedélyezési konfigurálható közül az alábbiak egyike:

- Az alkalmazás eléréséhez csak hitelesített kérelmek engedélyezése.

    Ha böngészőben névtelen kérelem érkezik, alkalmazás szolgáltatás átirányítja lapra az identitás-szolgáltatóhoz, amelyek úgy dönt, hogy a felhasználók jelentkezhetnek be. A kérelem érkezik, a mobileszközről, a visszaadott választ esetén egy _401 nem engedélyezett_ HTTP választ.

    Ezt a beállítást választja akkor nem kell a kódírás bármely hitelesítési egyáltalán az alkalmazásban. Ha finomabb engedély szükséges, a kód a felhasználó adatai érhető el.

- Elérni az alkalmazás, de hitelesített kérések ellenőrzése és hitelesítési adatot a HTTP-fejlécekben mentén adják át az összes kérések engedélyezése

    Ez a beállítás eltér az alkalmazás kódja engedélyezési döntéseket. Nagyobb rugalmasság a névtelen kérések kezelésével biztosít, de ki kell kódírás.

- Elérje az alkalmazás az összes kérések engedélyezése, és nincs művelet végrehajtása a hitelesítési adatot a összehívásokban.

    Az ez utóbbi esetben a hitelesítés és a hitelesítési szolgáltatás ki van kapcsolva. Hitelesítés és az engedélyezés feladatai vannak teljesen felfelé az alkalmazás kódját.

Az előző viselkedések vezérel a **műveleteket, ha nem lehet kérelem hitelesíteni** beállítást az Azure-portálon. * *Jelentkezzen be a *szolgáltató neve* választhat **, az összes kérelem kell hitelesíteni.** Kérés (semmi) engedélyezése ** eltér az engedélyezés döntés a kódot, de továbbra is biztosít a hitelesítési adatait. Ha a kód minden kezelni szeretné használni, letilthatja a hitelesítés / engedélyezési szolgáltatás.

## <a name="working-with-user-identities-in-your-application"></a>Felhasználói identitások az alkalmazás használata

Alkalmazás szolgáltatás egyes felhasználói adatok továbbítja az alkalmazás speciális fejlécek használatával. Külső kérések tilthatják ezeket a fejléceket, és csak ha bemutató állítja be alkalmazás szolgáltatás hitelesítés / engedélyt. Néhány példa fejlécek a következők:

* X-MS-ÜGYFÉL-EGYSZERŰ – NEVE
* X – AZ MS-ÜGYFÉL-EGYSZERŰ-AZONOSÍTÓ
* X-MS-TOKEN-FACEBOOK-ACCESS-TOKEN
* X-MS-TOKEN-FACEBOOK-EXPIRES-ON

Kód, amely bármely nyelvi vagy keretrendszer írott szükséges adatokat beolvashat ezeket a fejléceket. ASP.NET 4.6 alkalmazásai a **ClaimsPrincipal** automatikusan beírja a megfelelő értékekkel.

Az alkalmazás további felhasználó adatai HTTP GET keresztül is szerezhet a a `/.auth/me` az alkalmazás végpontot. Érvényes jogkivonat, amely tartalmaz a kérést, használja a szolgáltató részleteit, az alapul szolgáló szolgáltató jogkivonathoz és egyes felhasználói adatok JSON hasznos ad eredményül. A Mobile-alkalmazások kiszolgáló SDK a segítő módszerek a adatokkal végzett munkához szükséges. További tudnivalókért lásd: [az Azure Mobile alkalmazások Node.js SDK használatáról](../app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#howto-tables-getidentity)és [a .NET kódmentes server Azure Mobile-appokról SDK használata](../app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#user-info).

## <a name="documentation-and-additional-resources"></a>Dokumentáció és további források

### <a name="identity-providers"></a>Identitásszolgáltatók
Az alábbi oktatóanyagok bemutatják, hogyan alkalmazás szolgáltatás használata különböző hitelesítésszolgáltatók konfigurálása:

- [Az alkalmazás használatához az Azure Active Directory bejelentkezési konfigurálása][AAD]
- [Az alkalmazás használatához a Facebook-bejelentkezési konfigurálása][Facebook]
- [Az alkalmazás használatához a Google bejelentkezési konfigurálása][Google]
- [Az alkalmazás használatához Microsoft Account bejelentkezni konfigurálása][MSA]
- [Az alkalmazás használatához a Twitteren bejelentkezési konfigurálása][Twitter]

Ha szeretne egy személyes rendszer kívül a Itt adhat, is használhatja az [egyéni hitelesítési támogatása a Mobile alkalmazások .NET-kiszolgáló SDK megtekintése][custom-auth], a web Apps alkalmazások, mobilalkalmazások vagy API-alkalmazások is használható.

### <a name="web-applications"></a>Webalkalmazások
Az alábbi oktatóanyagok hitelesítési hozzáadása egy webalkalmazáshoz megjelenítése:

- [Azure alkalmazás szolgáltatás – rész 2 – első lépések][web-getstarted]

### <a name="mobile-applications"></a>Mobilalkalmazások
Az alábbi oktatóanyagok bemutatják, hogyan hitelesítési hozzáadása a mobil ügyfélalkalmazások a kiszolgáló utasította folyamat használatával:

- [Hitelesítés hozzáadása az iOS-alkalmazás][iOS]
- [Hitelesítés az Android-alkalmazás hozzáadása] [Android]
- [A Windows-alkalmazás hitelesítés hozzáadása] [A Windows]
- [A Xamarin.iOS alkalmazás hozzáadása a hitelesítés] [Xamarin.iOS]
- [A Xamarin.Android alkalmazás hozzáadása a hitelesítés] [Xamarin.Android]
- [A Xamarin.Forms alkalmazás hozzáadása a hitelesítés] [Xamarin.Forms]
- [Adja hozzá a Cordova alkalmazásba hitelesítés] [Cordova]

Használja az alábbi források, ha azt szeretné, az ügyfél irányított áramlás Azure Active Directory használni:

- [Használja az Active Directory Authentication Library IOS][ADAL-iOS]
- [Az Active Directory Authentication Library használata Android-alapú][ADAL-Android]
- [Az Active Directory Authentication Library for Windows és a Xamarin használatához][ADAL-dotnet]

Használja az alábbi források, ha azt szeretné, az ügyfél irányított áramlás Facebook használni:

- [A Facebook-SDK iOS használata](../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#facebook-sdk)

Használja az alábbi források, ha azt szeretné, az ügyfél irányított áramlás Twitter használni:

- [IOS Twitter háló használata](../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#twitter-fabric)

Használja az alábbi források, ha azt szeretné, az ügyfél irányított áramlás Google használni:

- [A Google bejelentkezési SDK iOS használata](../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#google-sdk)

### <a name="api-applications"></a>API-alkalmazások
Az alábbi oktatóanyagok az API-alkalmazások védelmét megjelenítése:

- [Az Azure-App szolgáltatásban API-alkalmazások a felhasználói hitelesítés][apia-user]
- [Szolgáltatás fő hitelesítési az Azure-App szolgáltatásban API-alkalmazások][apia-service]









[apia-user]: ../app-service-api/app-service-api-dotnet-user-principal-auth.md
[apia-service]: ../app-service-api/app-service-api-dotnet-service-principal-auth.md

[web-getstarted]: ../app-service-web/app-service-web-get-started-2.md#authenticate-your-users

[iOS]: ../app-service-mobile/app-service-mobile-ios-get-started-users.md
[Android-alapú]: ../app-service-mobile/app-service-mobile-android-get-started-users.md
[Xamarin.iOS]: ../app-service-mobile/app-service-mobile-xamarin-ios-get-started-users.md
[Xamarin.Android]: ../app-service-mobile/app-service-mobile-xamarin-android-get-started-users.md
[Xamarin.Forms]: ../app-service-mobile/app-service-mobile-xamarin-forms-get-started-users.md
[A Windows]: ../app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-users.md
[Cordova]: ../app-service-mobile/app-service-mobile-cordova-get-started-users.md

[AAD]: ../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md
[Facebook]: ../app-service-mobile/app-service-mobile-how-to-configure-facebook-authentication.md
[Google]: ../app-service-mobile/app-service-mobile-how-to-configure-google-authentication.md
[MSA]: ../app-service-mobile/app-service-mobile-how-to-configure-microsoft-authentication.md
[Twitter]: ../app-service-mobile/app-service-mobile-how-to-configure-twitter-authentication.md

[custom-auth]: ../app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#custom-auth

[ADAL-Android]: ../app-service-mobile/app-service-mobile-android-how-to-use-client-library.md#adal
[ADAL-iOS]: ../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#adal
[ADAL-dotnet]: ../app-service-mobile/app-service-mobile-dotnet-how-to-use-client-library.md#adal
