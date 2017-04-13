<properties
    pageTitle="Hitelesítés és Azure mobilalkalmazások az engedélyezés |} Microsoft Azure"
    description="Elvi hivatkozás és a hitelesítési áttekintése és a Mobile-appokról Azure szolgáltatás engedélyezési"
    services="app-service\mobile"
    documentationCenter=""
    authors="mattchenderson"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="mahender"/>

# <a name="authentication-and-authorization-in-azure-mobile-apps"></a>Hitelesítés és Azure mobilalkalmazások az engedélyezés

## <a name="what-is-app-service-authentication--authorization"></a>Mi az alkalmazás szolgáltatás hitelesítési / engedély?

> [AZURE.NOTE] Ez a témakör áttelepítendőként szeretne egy összesített [alkalmazás szolgáltatás hitelesítési és engedélyezési](../app-service/app-service-authentication-overview.md) témakört, amely magában foglalja a webes, Mobile és az API-alkalmazások.

Alkalmazás szolgáltatás hitelesítési / engedély egy funkció, amely lehetővé teszi, hogy az alkalmazás bejelentkezni a felhasználók az alkalmazás kódmentes szükséges változtatás nélkül kódot. Ugyanígy védelme az alkalmazás és a felhasználó adatokkal végzett munkához biztosít.

Alkalmazás szolgáltatást használja a szövetséges identitást 3rd gyártók **identitásszolgáltató** ("IDP") tárolja a fiókok és hitelesíti a felhasználókat, valamint az alkalmazás Ez az identitás helyett a saját. Alkalmazás szolgáltatás lehetővé teszi a beépített öt Identitásszolgáltatók: _Azure Active Directory_, a _Facebook_, a _Google_, a _Microsoft-fiók_és a _Twitter_. Ez a támogatás az alkalmazások az integrálja a másik identitásszolgáltató vagy a saját egyéni megoldást is elemre.

Az alkalmazás is kihasználhatja a következő Identitásszolgáltatók tetszőleges számú, hogyan bejelentkezési beállításokkal biztosíthat a felhasználóknak.

Ha szeretne azonnal használatba, című alábbi oktatóanyagok közül:

- [Hitelesítés hozzáadása az iOS-alkalmazás]
- [Hitelesítés felvétele a Xamarin.iOS alkalmazásba]
- [Hitelesítés felvétele a Xamarin.Android alkalmazásba]
- [Hitelesítés hozzáadása a Windows-alkalmazás]

## <a name="how-authentication-works"></a>Hogyan működik a hitelesítés

Annak érdekében, hogy a hitelesíteni a Identitásszolgáltatók egyikének használatával, először kell tudni az alkalmazás a identitásszolgáltató konfigurálása. A identitásszolgáltató majd biztosítja az azonosítók és titkos kulcsok vissza az alkalmazást az Ön által. A meghatalmazás befejeződik, és a nyújtott identitások érvényesítéséhez alkalmazás szolgáltatás lehetővé teszi.

Ezeket a lépéseket részletezi az alábbi témaköröket:

- [Az alkalmazás használatához az Azure Active Directory bejelentkezési konfigurálása]
- [Az alkalmazás használatához a Facebook-bejelentkezési konfigurálása]
- [Az alkalmazás használatához a Google bejelentkezési konfigurálása]
- [Az alkalmazás használatához Microsoft Account bejelentkezni konfigurálása]
- [Az alkalmazás használatához a Twitteren login konfigurálása]

Miután minden van beállítva a kódmentes, jelentkezzen be az ügyfél módosíthatók. Íme néhány az alábbi két módszer:

- Tudassa az egysoros kód ügyfél SDK jelentkezzen be a felhasználók Mobile-alkalmazások.
- Kihasználhatja az identitás létrehozásához, és kattintson az alkalmazás szolgáltatás eléréséhez szükséges a megadott identitásnak szolgáltató által közzétett SDK.

>[AZURE.TIP] Legtöbb alkalmazással kell használni a szolgáltató SDK, egy további natív feeling bejelentkezési élmény és frissítés támogatási és az egyéb szolgáltatófüggő juttatások kihasználhatja.

### <a name="how-authentication-without-a-provider-sdk-works"></a>Hogyan működik a hitelesítés nélkül SDK szolgáltató

Ha nem szeretne egy szolgáltatót SDK beállítása, engedélyezheti a bejelentkezési végrehajtásához Mobile-alkalmazások. A Mobile-alkalmazások ügyfél SDK fog nyissa meg a szolgáltató a webes nézet, és fejezze be a bejelentkezési. Időnként blogok és fórumok, látni fogja a továbbiakban az "kiszolgáló forgalom" vagy "kiszolgáló utasította forgalom" óta a kiszolgáló a bejelentkezési kezeli, és az ügyfél SDK soha nem kap a szolgáltató jogkivonat.

Ez a folyamat indításához szükséges kódot a hitelesítési oktatóprogram során platformokon vonatkozik. A folyamat végén az ügyfél SDK-szolgáltatási alkalmazás jogkivonat, és rendelkezik a token automatikusan csatolva van a kódmentes összes kérelmet.

### <a name="how-authentication-with-a-provider-sdk-works"></a>Hogyan működik a egy szolgáltatót SDK hitelesítés

A szolgáltató használata SDK lehetővé teszi, hogy a bejelentkezési felület további szorosan együttműködhet platform operációs rendszer fut az alkalmazást. Ezzel is kap egy szolgáltató token és az egyes felhasználói adatok az ügyfélszámítógépen, amely lehetővé teszi a sokkal egyszerűbb felhasználása graph API-hoz és a felhasználói felület testreszabását. Időnként blogok és fórumok jelennek meg ezt nevezzük a "ügyfél folyamat" vagy "ügyfél irányított továbbításához" óta kódot az ügyfél kezeli a bejelentkezés, és az ügyfél-kódot a szolgáltató jogkivonat hozzáférése van.

Miután a szolgáltató jogkivonat származik, kell el lehet küldeni a alkalmazás szolgáltatás adatérvényesítéshez. A folyamat végén az ügyfél SDK-szolgáltatási alkalmazás jogkivonat, és rendelkezik a token automatikusan csatolva van a kódmentes összes kérelmet. A Fejlesztőeszközök is tarthatja a szolgáltató jogkivonat hivatkozást, ha úgy dönt.

## <a name="how-authorization-works"></a>Hogyan működik a hitelesítés

Alkalmazás szolgáltatás hitelesítési / engedélyezési közzététele számos beállítást a **műveleteket, ha nem lehet kérelem hitelesíteni**. A kód egy adott kérelem érkezik, mielőtt lehet alkalmazás szolgáltatás ellenőrizze, hogy ha a kérelem hitelesítése és a Ha nem, el kell utasítania, és próbálja meg a felhasználót, mielőtt megpróbálja újra bejelentkezni.

Egy, hogy rendelkezik nem hitelesített kérések átirányítása a Identitásszolgáltatók egyikére. Egy webböngészőben ez volna ténylegesen igénybe vehet a felhasználó számára új oldalt. Azonban a mobil ügyfélprogram nem lehet átirányítani, ily módon, és nem hitelesített válaszokat kap egy _401 nem engedélyezett_ HTTP választ. Adott meg, az első kérés teszi az ügyfélnek kell a bejelentkezési végpont, és kattintson a bármely más API-khoz hívásokat is folytathat. Ha egy másik API hívja bejelentkezés előtt megpróbálja, az ügyfélnek hibaüzenetet kap.

Ha azt szeretné, hogy pontosabban szabályozhatja, hogy milyen végpontokat fölé hitelesítést igényel, is kiválaszthat "nincs további teendő (kérés engedélyezése)" nem hitelesített kérelmekhez. Ebben az esetben az összes hitelesítési döntéseket elhalasztott az alkalmazás kód. Ez lehetővé teszi a megadott felhasználók egyéni engedélyezési szabályok alapján való hozzáférés engedélyezése.

## <a name="documentation"></a>Dokumentáció

Az alábbi oktatóanyagok hitelesítési hozzáadása az alkalmazás szolgáltatással mobilügyfelek megjelenítése:

- [Hitelesítés hozzáadása az iOS-alkalmazás]
- [Hitelesítés felvétele a Xamarin.iOS alkalmazásba]
- [Hitelesítés felvétele a Xamarin.Android alkalmazásba]
- [Hitelesítés hozzáadása a Windows-alkalmazás]

Az alábbi oktatóanyagok bemutatják, hogyan használhatja a különböző hitelesítésszolgáltatók alkalmazás szolgáltatás beállítása:

- [Az alkalmazás használatához az Azure Active Directory login konfigurálása]
- [Az alkalmazás használata a Facebook-bejelentkezési konfigurálása]
- [Az alkalmazás használata Google bejelentkezési konfigurálása]
- [Az alkalmazás használatához Microsoft Account bejelentkezni konfigurálása]
- [Az alkalmazás használatához a Twitteren bejelentkezési konfigurálása]

Ha szeretne egy személyes rendszer kívül a Itt adhat, is is élvezheti [Egyéni hitelesítés támogatása a .NET-kiszolgáló SDK előnézete](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#custom-auth).

[Hitelesítés hozzáadása az iOS-alkalmazás]: app-service-mobile-ios-get-started-users.md
[Hitelesítés felvétele a Xamarin.iOS alkalmazásba]: app-service-mobile-xamarin-ios-get-started-users.md
[Hitelesítés felvétele a Xamarin.Android alkalmazásba]: app-service-mobile-xamarin-android-get-started-users.md
[Hitelesítés hozzáadása a Windows-alkalmazás]: app-service-mobile-windows-store-dotnet-get-started-users.md

[Az alkalmazás használatához az Azure Active Directory login konfigurálása]: app-service-mobile-how-to-configure-active-directory-authentication.md
[Az alkalmazás használata a Facebook-bejelentkezési konfigurálása]: app-service-mobile-how-to-configure-facebook-authentication.md
[Az alkalmazás használatához a Google bejelentkezési konfigurálása]: app-service-mobile-how-to-configure-google-authentication.md
[Az alkalmazás használatához Microsoft Account bejelentkezni konfigurálása]: app-service-mobile-how-to-configure-microsoft-authentication.md
[Az alkalmazás használatához a Twitteren bejelentkezési konfigurálása]: app-service-mobile-how-to-configure-twitter-authentication.md
