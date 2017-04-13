<properties
    pageTitle="Azure Active Directory B2C: Áttekintés |} Microsoft Azure"
    description="Az Azure Active Directory B2C fogyasztói elérésű alkalmazások fejlesztéséhez"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="07/24/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-sign-up-and-sign-in-consumers-in-your-applications"></a>Azure Active Directory B2C: Jelentkezzen, és jelentkezzen be az alkalmazások fogyasztói

Azure Active Directory B2C egy teljes felhő identitáskezelési megoldást a fogyasztói elérésű webhely és a mobilalkalmazások. Egy könnyen hozzáférhető globális-szolgáltatás, méretezze át a fogyasztói identitások milliónyi több száz. Egy vállalati szintű biztonságos platformon épül, Azure Active Directory B2C őrzi meg az alkalmazások, a vállalkozás és a fogyasztói védett.

Az elmúlt alkalmazások fejlesztői számára, akik jelentkezhetnek, és bejelentkezés az alkalmazásokba fogyasztói szeretett volna volna írt a saját kód. És azok ugyanúgy használható a helyszíni adatbázisok vagy rendszerek felhasználóneveket és jelszavakat tárolja. Azure Active Directory B2C: egy biztonságos, előírások platform és egy formázott extensible házirendeket segítségével kérelmeiket fogyasztói Identitáskezelés integrálni szeretné a hatékonyabb módszerre felajánlja a fejlesztők számára. Azure Active Directory B2C használata esetén a fogyasztói regisztrálhatnak-alkalmazás létrehozása új hitelesítő adatok (e-mail címét és jelszavát, vagy a felhasználónév és jelszó); vagy a meglévő közösségi fiókjuk (Facebook, a Google, Amazon, LinkedIn) használatával az utóbbi "helyi fiókok." hívása

## <a name="get-started"></a>Első lépések

Össze egy alkalmazást, amely fogyasztói feliratkozás, és jelentkezzen be, akkor fogad el kell először meg kell regisztrálni az alkalmazást az Azure Active Directory B2C a bérlői. A saját bérlői Ismerkedés [az Azure Active Directory B2C bérlő létrehozása](active-directory-b2c-get-started.md)című témakörben ismertetett lépésekkel.

Az alkalmazás az Azure Active Directory B2C szolgáltatás ellen írhat választva Protocol (protokoll) üzeneteket küldhet, közvetlenül a [OAuth 2.0-s](active-directory-b2c-reference-protocols.md#oauth2-authorization-code-flow) vagy a [Megnyitott azonosító csatlakozni](active-directory-b2c-reference-protocols.md#openid-connect-sign-in-flow), vagy a tárak használata a munka az Önnek. A kedvenc platform válasszon az alábbi táblázatban, és vágjon bele.

[AZURE.INCLUDE [active-directory-b2c-quickstart-table](../../includes/active-directory-b2c-quickstart-table.md)]

## <a name="whats-new"></a>Mi újság

Itt vissza gyakran az, ha többet szeretne tudni az Azure Active Directory B2C jövőbeli módosításait. Azt fogja is tweet bármely frissítéseiről használatával @AzureAD.

- További tudnivalók a [bővíthető keretet](active-directory-b2c-reference-policies.md) házirendek létrehozása és az alkalmazások használata a típusú kapcsolatban, illetve.
- A könyvjelző a [blogjának](https://blogs.msdn.microsoft.com/azureadb2c/) a kis szolgáltatásproblémák, frissítések, állapot és megoldásokkal kapcsolatban az értesítésekhez. Továbbra is az [Azure állapot irányítópult](https://azure.microsoft.com/status/) , valamint figyelheti.
- Aktuális [szolgáltatáselérhetőség, korlátozások, és korlátozásokat](active-directory-b2c-limitations.md).
- Végezetül [kód minta](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect-aspnetcore-b2c) Azure Active Directory B2C & ASP.NET Core használatával.

## <a name="how-to-articles"></a>Útmutatók

Megtudhatja, hogy miként adott Azure Active Directory B2C szolgáltatások használata:

- Használatra [Facebook](active-directory-b2c-setup-fb-app.md), [a Google +](active-directory-b2c-setup-goog-app.md), [Microsoft-fiókkal](active-directory-b2c-setup-msa-app.md), [Amazon](active-directory-b2c-setup-amzn-app.md)és [LinkedIn](active-directory-b2c-setup-li-app.md) fiókok konfigurálása elemre a fogyasztói elérésű alkalmazásokban.
- [A fogyasztói vonatkozó információk gyűjtését egyéni attribútumok használatát](active-directory-b2c-reference-custom-attr.md).
- [Azure többtényezős hitelesítés engedélyezése a fogyasztói elérésű alkalmazásokban](active-directory-b2c-reference-mfa.md).
- [Állítsa be az önkiszolgáló jelszó alaphelyzetbe állítása az a vonzóbbak lehetnek](active-directory-b2c-reference-sspr.md).
- [A Megjelenés és működés azoknak, bejelentkezés, és más fogyasztói elérésű lapok testreszabása](active-directory-b2c-reference-ui-customization.md) Azure Active Directory B2C kombinációja.
- [Az Azure Active Directory Graph API programozás útján létrehozása, olvasása, frissítéséhez és törlése a fogyasztói használata](active-directory-b2c-devquickstarts-graph-dotnet.md) az Azure Active Directory B2C bérlői webhelyen.

## <a name="next-steps"></a>Következő lépések

Ezeket a hivatkozásokat lesz felfedezése a mélység szolgáltatás esetében hasznos lehet:

- Olvassa el az [Azure Active Directory B2C árak információkat](https://azure.microsoft.com/pricing/details/active-directory-b2c/).
- Segítség a túlcsordulás Papírhalom az [azure active Directoryval](http://stackoverflow.com/questions/tagged/azure-active-directory) segítségével vagy [adal](http://stackoverflow.com/questions/tagged/adal) címkék.
- Közölje velünk a [Felhasználó hangposta](https://feedback.azure.com/forums/169401-azure-active-directory/)gondolatait – szeretnénk hallhatók őket! Kifejezés használata "AzureADB2C:" a bejegyzésre, hogy megtalálhassuk azt címére.
- Tekintse át az [Azure Active Directory B2C Protocol (protokoll) hivatkozást](active-directory-b2c-reference-protocols.md).
- Tekintse át az [Azure Active Directory B2C jogkivonat-hivatkozást](active-directory-b2c-reference-tokens.md).
- Olvassa el az [Azure Active Directory B2C gyakori kérdések](active-directory-b2c-faqs.md).
- [Támogatási kérelem Azure Active Directory B2C fájlt](active-directory-b2c-support.md).

## <a name="get-security-updates-for-our-products"></a>Biztonsági frissítések beszerzése termékei

Javasoljuk, hogy mikor védelmi események előfordulásainak megtalálhatók az [erre a lapra](https://technet.microsoft.com/security/dd252948) , és a feliratkozás biztonsági figyelmeztető riasztás, értesítést kap.
