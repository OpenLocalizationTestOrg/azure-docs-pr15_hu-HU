<properties
    pageTitle="2.0-s verzió végpont áttekintése |} Microsoft Azure"
    description="A Microsoft-Account és Azure Active Directory is bejelentkezési alkalmazásfejlesztésről bemutatása."
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
    ms.date="09/27/2016"
    ms.author="dastrock"/>

# <a name="sign-in-microsoft-account--azure-ad-users-in-a-single-app"></a>Bejelentkezés a Microsoft Account és Azure Active Directory-felhasználók egy egyetlen alkalmazásban

Múltbeli az alkalmazás fejlesztője ki szeretett volna támogatja a Microsoft-fiókok és Azure Active Directory is volt szükség integrálása a két külön formában.  Azt már most bevezetett új hitelesítési API-verziót, amely lehetővé teszi, hogy jelentkezzen be a felhasználók be mindkét fióktípus az Azure Active Directory rendszert használ.  Ez minősítőben hitelesítési a rendszer **a 2.0-s verzió végpont**nevezik.  A 2.0-s verzió végponttal egy egyszerű integráció teszi elérje a személyes és munkahelyi vagy iskolai fiókkal rendelkező felhasználók millióit nyúló közönségnek.

A 2.0-s verzió végpont használó alkalmazások is igénybe vehet, a [Microsoft Graph](https://graph.microsoft.io) és bármely típusú fiókot használ [az Office 365](https://msdn.microsoft.com/office/office365/howto/authenticate-Office-365-APIs-using-v2) REST API-khoz.

<!-- For a quick introduction to the v2.0 endpoint, please view the [Getting Started with Microsoft Identities: Enterprise Grade Sign In For Your Apps](https://azure.microsoft.com/documentation/videos/build-2016-getting-started-with-microsoft-identities-enterprise-grade-sign-in-for-your-apps/) video. -->

## <a name="getting-started"></a>Első lépések
[AZURE.VIDEO build-2016-getting-started-with-microsoft-identities-enterprise-grade-sign-in-for-your-apps]

A kedvenc platform válasszon az alábbi lista használatával, a Megnyitás tárak és keretek alkalmazás létrehozására.  Azt is megteheti is használhatja az OAuth 2.0-s & OpenID csatlakozás protocol dokumentációja küldése & auth tárról használata nélkül, közvetlenül a protokoll üzeneteket fogadni.

<!-- TODO: Finalize this table  -->
[AZURE.INCLUDE [active-directory-v2-quickstart-table](../../includes/active-directory-v2-quickstart-table.md)]

## <a name="whats-new"></a>Mi újság
Az általános információkat mi, és mi nem lehetséges a 2.0-s verzió végponttal megértéséhez hasznos lesz.

- Tudjon meg többet a [típusú alkalmazásokat a 2.0-s verzió végponttal készíthet](active-directory-v2-flows.md).
- Ismerje meg a [korlátozások, korlátozások, és korlátozások](active-directory-v2-limitations.md) a 2.0-s verzió végponttal.
- A legutóbb hozzáadtunk [felügyeleti tiltott tartományok](active-directory-v2-scopes.md) és az [oauth2 hitelesítési mód ügyfél adni a hitelesítő adatok](active-directory-v2-protocols-oauth-client-creds.md)támogatása.  Próbálja ki láthatóságukat!

## <a name="reference"></a>Hivatkozás
Ezeket a hivatkozásokat hasznos, ha a platformot mély felfedezése lesz:

- Szerkesztés 2016: [Microsoft identitások – első lépések: vállalati minőségű bejelentkezés az alkalmazások az](https://azure.microsoft.com/documentation/videos/build-2016-getting-started-with-microsoft-identities-enterprise-grade-sign-in-for-your-apps/)
- Segítség kérése a Papírhalom túlcsordulás az [azure active Directoryval](http://stackoverflow.com/questions/tagged/azure-active-directory) vagy [adal](http://stackoverflow.com/questions/tagged/adal) címkék.
- [Hivatkozás a 2.0-s verzió Protocol (protokoll)](active-directory-v2-protocols.md)
- [Jogkivonat-hivatkozást 2.0-s verzió](active-directory-v2-tokens.md)
- [Tár-hivatkozás 2.0-s verzió](active-directory-v2-libraries.md)
- [Keresési tartományok és a 2.0-s verzió végpont jóváhagyása](active-directory-v2-scopes.md)
- [A Microsoft alkalmazás regisztrációs portál](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)
- [Az Office 365 REST API-hivatkozás](https://msdn.microsoft.com/office/office365/howto/authenticate-Office-365-APIs-using-v2)
- [A Microsoft Graph-hoz](https://graph.microsoft.io)