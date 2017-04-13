<properties
    pageTitle="Hitelesítési és az API-alkalmazások Azure alkalmazás szolgáltatás engedélyezési |} Microsoft Azure"
    description="Tudjon meg többet az Azure alkalmazás szolgáltatás API-alkalmazások biztosítja hitelesítési és engedélyezési szolgáltatásokat."
    services="app-service\api"
    documentationCenter=".net"
    authors="tdykstra"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-api"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/23/2016"
    ms.author="rachelap"/>

# <a name="authentication-and-authorization-for-api-apps-in-azure-app-service"></a>Hitelesítési és az API-alkalmazások Azure alkalmazás szolgáltatás engedélyezési

## <a name="overview"></a>– Áttekintés 

> [AZURE.NOTE] Ez a témakör áttelepítendőként szeretne egy összesített [alkalmazás szolgáltatás hitelesítési és engedélyezési](../app-service/app-service-authentication-overview.md) témakört, amely magában foglalja a webes, Mobile és az API-alkalmazások.

Azure alkalmazás szolgáltatás, amely az [OAuth 2.0-s](#oauth) és [OpenID csatlakozás](#oauth)végrehajtása beépített hitelesítési és engedélyezési szolgáltatások kínál. Ez a cikk ismerteti a szolgáltatások és az API-alkalmazások Azure App szolgáltatásban használható beállításokat.

Az alábbi ábra mutatja be, néhány alkalmazás szolgáltatás hitelesítési fő jellemzői:

* Azt preprocesses API bejövő felkérést, ami azt jelenti, hogy nyelvi vagy szolgáltatás alkalmazás által támogatott keretrendszer működik.
* A program felajánlja mennyi hitelesítési működik, több lehetőség közül választhat műveletet szeretne végezni a saját kód.
* Végfelhasználói és a szolgáltatás fiók hitelesítési működik. 
* Öt Identitásszolgáltatók támogat: Azure Active Directory, a Facebook, a Google, a Twitteren és a Microsoft-Account.
* Ugyanaz a API-alkalmazások, a Web Apps alkalmazások és a Mobile-alkalmazások működik.

![](./media/app-service-api-authentication/api-apps-overview.png)

## <a name="language-agnostic"></a>Nyelvi felismerése nélkül

Alkalmazás szolgáltatás hitelesítési feldolgozás kérések az API-alkalmazást, amely az API-alkalmazások bármelyik nyelvi vagy keretrendszer írt jelenti, hogy működik-e a hitelesítési szolgáltatások elérése előtt történik.  A API ASP.NET, Java, Node.js vagy szolgáltatás alkalmazás által támogatott bármely keretrendszer alapulhatnak.

Alkalmazás szolgáltatás továbbítja a HTTP-kérelem engedélyezési fejlécében JSON-webes jogkivonat (JWT), és bármelyik nyelvi vagy keretrendszer megírt kód a token is beolvashat a szükséges adatokat. Ezeken kívül alkalmazás szolgáltatás könnyebben hozzáférést biztosít a leggyakrabban használt jogcímeken beállításával, hogy egyes speciális fejlécek, például a következő:

* X-MS-ÜGYFÉL-EGYSZERŰ – NEVE
* X – AZ MS-ÜGYFÉL-EGYSZERŰ-AZONOSÍTÓ
* X-MS-TOKEN-FACEBOOK-ACCESS-TOKEN
* X-MS-TOKEN-FACEBOOK-EXPIRES-ON
 
A .NET API is használhatja a `Authorize` attribútum és könnyen írhat külön engedélyezési kód jogcímeken alapuló, mert követelések információk meg van töltve .NET osztályok.

## <a name="multiple-protection-options"></a>Több védelem beállításai

Alkalmazás szolgáltatás megakadályozhatja, hogy névtelen HTTP kérések alkalmazáson keresztül az API-alkalmazást, adja át az összes kérés és iránti kérelmek őket tartalmazó tokenek ellenőrzése vagy azt hagyhatja keresztül összes kérelmet rajtuk művelet végrehajtása nélkül:

1. Csak hitelesített kérelmek elérje az API-alkalmazás engedélyezése.

    Ha névtelen kérelem érkezik a böngészőben, alkalmazás szolgáltatás úgy irányítja át a bejelentkezési oldal a hitelesítési szolgáltató (Azure Active Directory, a Google, Twitter, stb.) választott. 

    Ezt a beállítást választja akkor nem kell kódírás bármely hitelesítési egyáltalán az alkalmazásban, és engedélyezési kód egyszerűsített, mivel a legfontosabb jogcímalapú a HTTP-fejlécekben találhatók.

2. Elérni az API-alkalmazást, de a hitelesített kérések érvényesítéséhez és a hitelesítési adatot a HTTP-fejlécekben mentén adják át az összes kérések engedélyezése

    Ezt a lehetőséget akkor nagyobb rugalmasság a névtelen kérések kezelésével, de ki kell kódírás Ha meg szeretné akadályozni, hogy a névtelen felhasználók számára a API-t használja. Mivel a leggyakrabban használt jogcímalapú át a HTTP-kérések fejlécekben, engedélyezési kód viszonylag egyszerű.
    
3. Lehetővé teszi az API elérni, nincs művelet végrehajtása a hitelesítési adatot a összehívásokban összes kérelmet.

    Ezt a beállítást hitelesítési és engedélyezési teljesen felfelé az alkalmazás kódja feladatainak elhagyja.

Az [Azure portál](https://portal.azure.com/)választja ki a kívánt beállítást a a **hitelesítési / engedélyezési** lap.

![](./media/app-service-api-authentication/authblade.png)

1 és 2 lehetőségekkel **Alkalmazás szolgáltatás hitelesítési**be- és a **műveleteket, ha nem lehet kérelem hitelesíteni** legördülő listában válassza a **Bejelentkezés** vagy a **kérés engedélyezése (nem tesz)**.  Ha úgy dönt, hogy **Jelentkezzen be**, akkor válassza egy hitelesítési-szolgáltatóval, és beállíthatja, hogy a szolgáltatót.

![](./media/app-service-api-authentication/actiontotake.png)

Információ arról, hogy miként hitelesítés megtudhatja, [hogy miként konfigurálása, az alkalmazás Azure Active Directory login használata](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md). A cikk a API-alkalmazásokat, valamint a mobilalkalmazások vonatkozik, és a többi hitelesítésszolgáltatók hivatkozások, más cikkekre mutató.
 
## <a id="internal"></a>Szolgáltatási számla hitelesítés

Alkalmazás szolgáltatás hitelesítési belső felhasználási területei például egy másik API-alkalmazás egyik API-alkalmazásból hívásra feljogosító működik. Ebben az esetben a jogkivonat használt hitelesítő adataival helyett a felhasználó hitelesítő adatait egy szolgáltatásfiókhoz kap. Szolgáltatásfiók más néven egy *egyszerű szolgáltatás* az Azure Active Directory, és ilyen fiókot használ hitelesítési néven is ismert szolgáltatás – példa. 

A szolgáltatás felhasználási területei a hívott API-alkalmazás védhetők Azure Active Directory, és adja meg egy AAD szolgáltatás egyszerű jóváhagyási jogkivonat, amikor felhívja az API-alkalmazást. Jogkivonat, mert az ügyfél, azonosító és a titkos AAD alkalmazásból ügyfél kap. Nem csak Azure kódot szükség, például a mobil szolgáltatások Zumo jogkivonat-kezeléshez igaz használt. Példa ebben az esetben az ASP.NET API-alkalmazások használata az oktatóprogram [szolgáltatás fő hitelesítési az API-alkalmazások](app-service-api-dotnet-service-principal-auth.md)hatálya alá.

Ha szeretné kezelni a szolgáltatás példa az alkalmazás szolgáltatás hitelesítés nélkül, ügyfél tanúsítványok vagy alapszintű hitelesítés is használhatja. Azure-ban ügyfél tanúsítványokkal kapcsolatos további tudnivalókért lásd [Hogyan szeretné konfigurálni TLS kölcsönös hitelesítés Web Apps alkalmazások](../app-service-web/app-service-web-configure-tls-mutual-auth.md). Alapszintű hitelesítés ASP.NET tudni lásd: a [Hitelesítési szűrők ASP.NET webes API-2](http://www.asp.net/web-api/overview/security/authentication-filters).

Szolgáltatási fiók hitelesítés-szolgáltatási alkalmazás logika alkalmazásban az API-alkalmazásokban helyzet egy speciális, hogy [az egyéni API is logika alkalmazással alkalmazás szolgáltatás használata](../app-service-logic/app-service-logic-custom-hosted-api.md)magyarázza.

## <a name="mobile-client-authentication"></a>Mobil ügyfélprogram-hitelesítés

Információ arról, hogy miként kezelheti a mobil ügyfélalkalmazások hitelesítés, akkor olvassa el a [dokumentáció mobilalkalmazásai-hitelesítést](../app-service-mobile/app-service-mobile-ios-get-started-users.md). Alkalmazás szolgáltatás hitelesítési mobilalkalmazások és az API-alkalmazások ugyanígy működik.
  
## <a name="more-information"></a>További információ

Hitelesítési és Azure alkalmazás szolgáltatás engedélyezési további információt az alábbi forrásokban talál:

* [Növekvő alkalmazás szolgáltatás hitelesítési / engedély](/blog/announcing-app-service-authentication-authorization/)
* [Az alkalmazás szolgáltatásalkalmazás használatához az Azure Active Directory bejelentkezési konfigurálása](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md) (Hivatkozásokat tartalmaz az egyéb hitelesítésszolgáltatók elemre a lap tetején.) 

Az alábbi forrásokban talál további információt a OAuth 2.0-s OpenID csatlakozás és JSON webes tokenek (JWT).

* [Első lépések – OAuth 2.0-s verziója] (http://shop.oreilly.com/product/0636920021810.do "Első lépések – OAuth 2.0-s verziója") 
* [Csatlakozás oauth2 hitelesítési mód, OpenID – bevezetés és a JSON webes jogkivonatok (JWT) – a tanfolyam PluralSight](http://www.pluralsight.com/courses/oauth2-json-web-tokens-openid-connect-introduction) 
* [Építési és biztonságossá tétele RESTful API ASP.NET - PluralSight tanfolyam több ügyfél](http://www.pluralsight.com/courses/building-securing-restful-api-aspdotnet)

Többet szeretne tudni az Azure Active Directory a következő forrásokban talál.

* [Azure Active Directory-esetek](http://aka.ms/aadscenarios)
* [Azure Active Directory fejlesztői útmutatója](http://aka.ms/aaddev)
* [Azure Active Directory-minta](http://aka.ms/aadsamples)

## <a name="next-steps"></a>Következő lépések

Ez a cikk van magyarázata hitelesítési és engedélyezési funkciók, amelyek segítségével használhatja az API-alkalmazások alkalmazás szolgáltatás. A következő oktatóanyagra a keresztüli lépések sorozat megvalósításáról [az alkalmazás szolgáltatás API-alkalmazásokban a felhasználói hitelesítés](app-service-api-dotnet-user-principal-auth.md)jeleníti meg.
