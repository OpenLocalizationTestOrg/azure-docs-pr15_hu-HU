<properties
    pageTitle="Védelmét a webes API kódmentes az Azure Active Directory és az API-kezelés"
    description="Megtudhatja, hogy miként védelmét a webes API kódmentes az Azure Active Directory és az API-kezelés." 
    services="api-management"
    documentationCenter=""
    authors="steved0x"
    manager="erikre"
    editor=""/>

<tags
    ms.service="api-management"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="sdanie"/>

# <a name="how-to-protect-a-web-api-backend-with-azure-active-directory-and-api-management"></a>Védelmét a webes API kódmentes Azure Active Directory és a API kezelése

Az alábbi videó bemutatja, hogyan egy webes API kódmentes összeállítása és védelme OAuth 2.0-s protokoll használatával az Azure Active Directory és az API-kezelés.  Ebben a cikkben áttekintést és további információk a videó című témakör lépéseit. 24 perc videóból megtudhatja, hogyan szeretné:

-   A webes API kódmentes összeállítása és biztonságossá tenni a AAD - 1:30 kezdve
-   API-kezelés – 7:10 kezdve az API importálása
-   Ha fel szeretne hívni az API - 9:09 karaktertől kezdve a Developer Portal segítségével konfigurálása
-   Asztali alkalmazás, hívja fel a API - kezdve a 18:08 konfigurálása
-   Előre engedélyezheti a kérések – 20:47 kezdve JWT érvényességi házirend konfigurálása

>[AZURE.VIDEO protecting-web-api-backend-with-azure-active-directory-and-api-management]

## <a name="create-an-azure-ad-directory"></a>Hozzon létre egy Azure Active directory

A webes API biztonságos biztonsági Azure Active Directory ahhoz először kötelezővé kell használata egy egy AAD-ös bérlői webhelyet. Ez a videó **APIMDemo** nevű bérlő használják. Hozzon létre egy AAD-ös bérlői webhelyet, bejelentkezés az [Azure klasszikus portál](https://manage.windowsazure.com) , és kattintson az **Új**->**Alkalmazás szolgáltatások**->**Az Active Directory**->**címtár**->**Egyéni létrehozása**. 

![Azure Active Directory][api-management-create-aad-menu]

Ebben a példában a **APIMDemo** nevű könyvtárában létrejön **DemoAPIM.onmicrosoft.com**nevű alapértelmezett tartománynevet. Ezt a címtárat a videó egész használják.

![Azure Active Directory][api-management-create-aad]

## <a name="create-a-web-api-service-secured-by-azure-active-directory"></a>Hozzon létre egy webes API-szolgáltatás védi az Azure Active Directory

Ebben a lépésben a webes API kódmentes jön létre, a Visual Studio 2013-ban. Ebben a lépésben a videó 1:30 kezdődik. A Visual Studióban kódmentes project webes API létrehozásához kattintson a **fájl**->**Új**->**Project**és a **webes** sablonok listából választhatja ki a **ASP.NET webalkalmazást** . Ez a videó a projekt neve **APIMAADDemo**. Kattintson az **OK gombra** a projekt létrehozása. 

![Visual Studio][api-management-new-web-app]

Kattintson a **Webes API** **Jelöljön ki egy sablont listát** hozhat létre egy webes API-projektet. Azure Directory Authentication beállításához kattintson a **Módosítás hitelesítési**.

![Új projekt][api-management-new-project]

Kattintson a **Szervezeti fiókok**, majd adja meg a AAD bérlő **tartományt** . Ebben a példában a tartománya **DemoAPIM.onmicrosoft.com**. A tartomány a könyvtár szerezhető be a **tartományok** lapon, a könyvtár.

![A tartományok][api-management-aad-domains]

A **Hitelesítés módosítása** párbeszédpanelen állítsa be a kívánt beállításokat, és kattintson az **OK gombra**.

![Hitelesítés módosítása][api-management-change-authentication]

Kattintson az **OK gombra** a Visual Studio megkísérli az alkalmazás regisztrálása az Azure Active directory és, jelentkezzen be a Visual Studio kérheti. Jelentkezzen be egy rendszergazdai fiókkal a könyvtár.

![Jelentkezzen be a Visual Studio][api-management-sign-in-vidual-studio]

A projekt beállítása Azure webes API jelölje be a jelölőnégyzetet, a szolgáltatója **a felhőben** , és kattintson az **OK gombra**.

![Új projekt][api-management-new-project-cloud]

Azure bejelentkezni kérheti olyan, és ezután beállíthatja a Web App alkalmazásban.

![Konfigurálása][api-management-configure-web-app]

Ebben a példában a **APIMAADDemo** nevű új **alkalmazás szolgáltatáscsomagja** van megadva.

Kattintson az **OK gombra** a Web App és a projekt létrehozása.

## <a name="add-the-code-to-the-web-api-project"></a>A kód hozzáadása a webes API-projekt

A következő lépés a videóban a kód hozzáadása a webes API-projektet. Ebben a lépésben 4:35 kezdődik.

Ebben a példában a webes API egy egyszerű számológép szolgáltatás modell és a vezérlő hajtja végre. A modell a szolgáltatás hozzáadása, kattintson a jobb gombbal a **Megoldást Explorer** **modellek** , és válassza a **Hozzáadás**, **osztály**. Nevezze el az osztály `CalcInput` , és kattintson a **Hozzáadás**gombra.

Adja hozzá a következő `using` utasítás tetején a `CalcInput.cs` fájlt.

    using Newtonsoft.Json;

 A létrehozott osztály cserélje le a következő kódot.

    public class CalcInput
    {
        [JsonProperty(PropertyName = "a")]
        public int a;

        [JsonProperty(PropertyName = "b")]
        public int b;
    }

Kattintson a jobb gombbal a **Megoldást Explorer** **vezérlők** , és válassza az **Add**->**vezérlő**. Válassza a **Webes API-2 vezérlő - üres** , és kattintson a **Hozzáadás**gombra. Írja be a **CalcController** a vezérlő nevére, és kattintson a **Hozzáadás**gombra.

![Vezérlő hozzáadása][api-management-add-controller]

Adja hozzá a következő `using` utasítás tetején a `CalcController.cs` fájlt.

    using System.IO;
    using System.Web;
    using APIMAADDemo.Models;

A létrehozott vezérlő osztály cserélje le a következő kódot. Ez a kód hajtja végre a `Add`, `Subtract`, `Multiply`, és `Divide` az egyszerű számológép API a rajtuk végzett adatműveleteket.

    [Authorize]
    public class CalcController : ApiController
    {
        [Route("api/add")]
        [HttpGet]
        public HttpResponseMessage GetSum([FromUri]int a, [FromUri]int b)
        {
            string xml = string.Format("<result><value>{0}</value><broughtToYouBy>Azure API Management - http://azure.microsoft.com/apim/ </broughtToYouBy></result>", a + b);
            HttpResponseMessage response = Request.CreateResponse();
            response.Content = new StringContent(xml, System.Text.Encoding.UTF8, "application/xml");
            return response;
        }

        [Route("api/sub")]
        [HttpGet]
        public HttpResponseMessage GetDiff([FromUri]int a, [FromUri]int b)
        {
            string xml = string.Format("<result><value>{0}</value><broughtToYouBy>Azure API Management - http://azure.microsoft.com/apim/ </broughtToYouBy></result>", a - b);
            HttpResponseMessage response = Request.CreateResponse();
            response.Content = new StringContent(xml, System.Text.Encoding.UTF8, "application/xml");
            return response;
        }

        [Route("api/mul")]
        [HttpGet]
        public HttpResponseMessage GetProduct([FromUri]int a, [FromUri]int b)
        {
            string xml = string.Format("<result><value>{0}</value><broughtToYouBy>Azure API Management - http://azure.microsoft.com/apim/ </broughtToYouBy></result>", a * b);
            HttpResponseMessage response = Request.CreateResponse();
            response.Content = new StringContent(xml, System.Text.Encoding.UTF8, "application/xml");
            return response;
        }

        [Route("api/div")]
        [HttpGet]
        public HttpResponseMessage GetDiv([FromUri]int a, [FromUri]int b)
        {
            string xml = string.Format("<result><value>{0}</value><broughtToYouBy>Azure API Management - http://azure.microsoft.com/apim/ </broughtToYouBy></result>", a / b);
            HttpResponseMessage response = Request.CreateResponse();
            response.Content = new StringContent(xml, System.Text.Encoding.UTF8, "application/xml");
            return response;
        }
    }

Nyomja le az **F6** létre, és ellenőrizze a megoldást.

## <a name="publish-the-project-to-azure"></a>Azure a projekt közzététele

Ebben a lépésben a Visual Studio projekt Azure van közzétéve. Ebben a lépésben a videó 5:45 kezdődik.

A projekt közzététele Azure, kattintson a jobb gombbal a **APIMAADDemo** projekt a Visual Studióban, és válassza a **Közzététel**lehetőséget. A **Webhely közzététele** párbeszédpanelen az alapértelmezett beállításokat őrizze, és kattintson a **Közzététel**gombra.

![Webes közzététele][api-management-web-publish]

## <a name="grant-permissions-to-the-azure-ad-backend-service-application"></a>Engedélyek az Azure Active Directory kódmentes szolgáltatásalkalmazáshoz

Az új alkalmazás a kódmentes szolgáltatás a Azure Active directory a webes API-projekt beállítása és a közzétételi folyamat részeként jön létre. Ebben a lépésben a videó 6:13-mal, kezdve a webes API kódmentes megadott engedélyekkel.

![Alkalmazás][api-management-aad-backend-app]

Kattintson a nevére, az alkalmazás beállítása az ehhez szükséges engedélyekkel. Nyissa meg azt a **beállítás** fülre, majd görgessen le az **engedélyek egyéb alkalmazások** csoportban. Kattintson a lefelé mutató nyíllal **a Windows** **Azure Active Directory** **Alkalmazásengedélyek** , jelölje be a jelölőnégyzetet **a címtár-adatok olvasása**, és kattintson a **Mentés**gombra.

![Engedélyek hozzáadása][api-management-aad-add-permissions]

>[AZURE.NOTE] Ha a **Windows** **Azure Active Directory** nem szerepel az engedélyek más alkalmazások, kattintson az **alkalmazás hozzáadása** parancsra, és vegye fel a listából.

Jegyezze le az **Alkalmazás azonosítója URI** használatra egy későbbi lépésben az Azure Active Directory-alkalmazások a API-kezelés developer Portal beállításakor.

![Alkalmazás azonosítója URI][api-management-aad-sso-uri]

## <a name="import-the-web-api-into-api-management"></a>A webes API importálása API-kezelés

A publisher az Azure klasszikus portálon keresztül érhető el API portálon megtörténik az API-khoz. A publisher portál elérjen, kattintson a **kezelés** az Azure klasszikus portálon a API-kezelési szolgáltatáshoz elemre. Ha még nem hozott létre API Management szolgáltatás példány, olvassa el a [API Management szolgáltatás példány létrehozása][] az [kezelése az első API][] oktatóprogram során.

![Publisher-portál][api-management-management-console]

Műveletek [kézzel hozzáadott API-khoz](api-management-howto-add-operations.md)is lehet, vagy lehet importálni. Ebből a videóból műveletek importálásának Swagger formátumban 6:40 karaktertől kezdve.

Hozzon létre egy nevű fájlt `calcapi.json` a következő tartalmát, és mentse a számítógépre. Győződjön meg arról, hogy a `host` pontok attribútum a webes API kódmentes. Ebben a példában `"host": "apimaaddemo.azurewebsites.net"` használják.

{"swagger": "2.0-s", "adatok": {"cím": "Számológép", "leírás": "Arithmetics http!", "verzió": "1.0"}, "host": "apimaaddemo.azurewebsites.net", "alapelérési": "/ api", "rendszerek": ["http"], "elérési út": {"hozzáadása /? egy = {} és b = {b}": {"beolvasása": {"leírás": "Válasza a két számok. összege", "operationId": "Hozzáadása két változása", "paraméterek": [{"név": "a", "a": "lekérdezés-", "leírás": "első operandus. Alapértelmezett értéke <code>51</code>. ","szükséges": igaz,"alapértelmezés":"51","felsorolás": ["51"]}, {"név":"b";"a":"lekérdezés-","leírás":"második operandus. Alapértelmezett értéke <code>49</code>. ","szükséges": igaz,"alapértelmezés":"49","felsorolás": ["49"]}],"választ": {}}}," / sub?a = {a} & b = {b} ": {"beolvasása": {"leírás":"Megválaszolja közti különbség között két számok.","operationId":"Kivonás két változása","paraméterek": [{"név":"a","a":"lekérdezés-","leírás":"első operandus. Alapértelmezett értéke <code>100</code>. ","szükséges": igaz,"alapértelmezés":"100","felsorolás": ["100"]}, {"név":"b";"a":"lekérdezés-","leírás":"második operandus. Alapértelmezett értéke <code>50</code>. ","szükséges": igaz,"alapértelmezés":"50","felsorolás": ["50"]}],"választ": {}}}," / div?a = {a} & b = {b} ": {"beolvasása": {"leírás":"Válasza a két számok. hányadosát","operationId":"A két egész számok osztás","paraméterek": [{"név":"a","a":"lekérdezés-","leírás":"első operandus. Alapértelmezett értéke <code>100</code>. ","szükséges": igaz,"alapértelmezés":"100","felsorolás": ["100"]}, {"név":"b";"a":"lekérdezés-","leírás":"második operandus. Alapértelmezett értéke <code>20</code>. ","szükséges": igaz,"alapértelmezés":"20","felsorolás": ["20"]}],"választ": {}}}," / mul?a = {a} & b = {b} ": {"beolvasása": {"leírás":"Megválaszolja termékkel két számok.","operationId":"Szendvicspozitív két egész számok","paraméterek": [{"név":"a","a":"lekérdezés-","leírás":"első operandus. Alapértelmezett értéke <code>20</code>. ","szükséges": igaz,"alapértelmezés":"20","felsorolás": ["20"]}, {"név":"b";"a":"lekérdezés-","leírás":"második operandus. Alapértelmezett értéke <code>5</code>. ","szükséges": igaz,"alapértelmezés":"5","felsorolás": ["5"]}],"választ": {}}}}}

A Számológép API importálásához **API-khoz** kattintson a bal oldali menüjében **API-kezelés** , és kattintson az **Importálás API -val**.

![Importálás API gomb][api-management-import-api]

A következő lépésekkel állítsa be a Számológép API-val.

1. Kattintson a **fájlból**, keresse meg azt a `calculator.json` mentette a fájlt, és kattintson a **Swagger** választógombra.
2. Írja be a **webes API URL-címe utótag** szövegdoboz **calc** .
3. Kattintson a **termékek (nem kötelező)** mezőbe, és válassza az **alapszintű**.
4. Az API importálása a **Mentés** gombra.

![Adja hozzá az új API][api-management-import-new-api]

Az API importálása után az API Összegzés lapon megjelenik a publisher-portálon.

## <a name="call-the-api-unsuccessfully-from-the-developer-portal"></a>Hívás a API nem tud a fejlesztői portáljáról

Ezen a ponton a API importálta az API-kezelés, de nem még hívható sikeresen a fejlesztői portálról, mert az Azure Active Directory authentication a kódmentes szolgáltatás védi. Ez a videó, az alábbi lépésekkel 7:40 karaktertől kezdve a van mutatni.

Kattintson a **Developer Portal segítségével** a publisher portál jobb felső szélén a.

![Developer Portal segítségével][api-management-developer-portal-menu]

Kattintson az **API-k** , majd a **Számológép** API parancsra.

![Developer Portal segítségével][api-management-dev-portal-apis]

Kattintson a **próbálja ki**.

![További információk][api-management-dev-portal-try-it]

Kattintson a **Küldés** gombra, és jegyezze fel a válaszállapota **401 nem engedélyezett**.

![Küldés][api-management-dev-portal-send-401]

A kérés nincs, mert a kódmentes API Azure Active Directory védi. Az API-t a fejlesztői hívása sikeres előtt portál kell beállítania, hogy a fejlesztők OAuth 2.0 használatával engedélyezhetik. Ez a folyamat az alábbi szakaszok leírása.

## <a name="register-the-developer-portal-as-an-aad-application"></a>A developer Portal segítségével regisztrálhatja AAD alkalmazásként

Az első lépés a developer Portal segítségével OAuth 2.0-s verzióját használó fejlesztők engedélyezése konfigurálása a developer Portal segítségével regisztrálhatja AAD alkalmazásként. Ez igazolni, 8:27 látható a videóban karaktertől kezdve.

Keresse fel az Azure AD-bérlő **APIMDemo** példánkban ez a videó az első lépésében, és keresse meg az **alkalmazások** fülre.

![Új alkalmazás][api-management-aad-new-application-devportal]

Kattintson a **Hozzáadás** gombra kattintva hozzon létre egy új Azure Active Directory-alkalmazást, és válassza a **szervezeten elkészítésének az alkalmazás hozzáadása**.

![Új alkalmazás][api-management-new-aad-application-menu]

Válassza a **webalkalmazás és/vagy a webes API**, írjon be egy nevet, és kattintson a következő nyílra. Ebben a példában **APIMDeveloperPortal** használják.

![Új alkalmazás][api-management-aad-new-application-devportal-1]

**Bejelentkezési** URL-adja meg a API alkalmazáskezelési szolgáltatás URL-CÍMÉT, és a Hozzáfűzés `/signin`. Ebben a példában **https://contoso5.portal.azure-api.net/signin **használják.

Az **Alkalmazás azonosítója URL-** adja meg a API alkalmazáskezelési szolgáltatás URL-CÍMÉT, és a Hozzáfűzés egyedi karakter. Ezek a kívánt karakterek lehetnek, és ebben a példában **https://contoso5.portal.azure-api.net/dp** szolgál. Ha a kívánt **alkalmazás Tulajdonságok** van konfigurálva, kattintson a jelölőnégyzet be van jelölve az alkalmazás létrehozása.

![Új alkalmazás][api-management-aad-new-application-devportal-2]

## <a name="configure-an-api-management-oauth-20-authorization-server"></a>Egy API Management OAuth 2.0-s engedélyezési kiszolgáló konfigurálása

A következő lépésként OAuth 2.0-s engedélyezési kiszolgáló API-kezelés beállítása. Ebben a lépésben a 9:43 karaktertől kezdve a videó az igazolni.

Kattintson a **Biztonság** gombra a bal oldalon a API-kezelés menüben kattintson a **OAuth 2.0-s**, és kattintson a **Hozzáadás engedélyezési** kiszolgáló.

![Engedély kiszolgáló hozzáadása][api-management-add-authorization-server]

Írja be a nevét és leírását a **név** és **Leírás** mezőkbe. Ezeket a mezőket az API Management szolgáltatás példány OAuth 2.0-s engedélyezési kiszolgálóján azonosítására szolgálnak. Ebben a példában **engedélyezési kiszolgáló bemutató** használják. Később az API hitelesítési használni egy OAuth 2.0-s kiszolgáló megadása esetén, ez a név jelölje ki.

Az **ügyfél regisztrációs lap URL-címet** adjon meg egy helyőrző értéket például `http://localhost`.  Az **ügyfél regisztrációs lap URL-címe** mutat, a lapot, amelyet a felhasználók létrehozása és a saját fiók konfigurálása a felhasználói fiókok kezelése támogató szolgáltatók OAuth 2.0-s segítségével. Ebben a példában a felhasználók létrehozása és nem a saját fiók konfigurálása, hogy a helyőrző használják.

![Engedély kiszolgáló hozzáadása][api-management-add-authorization-server-1]

Ezután adja meg a **engedélyezési végpont URL-cím** és a **Token végpont URL-CÍMÉT**.

![Engedély kiszolgáló][api-management-add-authorization-server-1a]

Ezeket az értékeket az AAD alkalmazás, a developer Portal segítségével létrehozott **Alkalmazás végpontok** oldaláról tudja visszaszerezni. Az access a végpontok nyissa meg azt a **beállítása** lapon az AAD alkalmazáshoz, és kattintson a **Nézet végpontok**.

![Alkalmazás][api-management-aad-devportal-application]

![Nézet végpontok][api-management-aad-view-endpoints]

Másolja az **OAuth 2.0-s engedélyezési végpontot** , és illessze be a **engedélyezési végpont URL-címe** mezőben lévő értéket.

![Engedély kiszolgáló hozzáadása][api-management-add-authorization-server-2]

Másolja az **OAuth 2.0-s jogkivonat végpontot** , és illessze be a **Token végpont URL-címe** mezőben lévő értéket.

![Engedély kiszolgáló hozzáadása][api-management-add-authorization-server-2a]

Nemcsak a illeszti be a token végpontot, egy további szervezet paraméter nevű **erőforrás** , és a kódmentes szolgáltatás mikor létrehozott érték használható az **Alkalmazás azonosítója URI** AAD alkalmazásból a Visual Studio projekt közzétett hozzáadása.

![Alkalmazás azonosítója URI][api-management-aad-sso-uri]

Ezután adja meg az ügyfél hitelesítő adatait. Ezek az erőforrás szeretne elérni, ebben az esetben a kódmentes szolgáltatás hitelesítő adatait.

![Ügyfél hitelesítő adatok][api-management-client-credentials]

Az **Ügyfél-azonosító**beszerzése, nyissa meg azt a **beállítása** lapon az AAD alkalmazás a kódmentes szolgáltatás és az **Ügyfél-azonosító**másolja.

Kattintson az **Ügyfél titkos** **Jelölje be az időtartamot** legördülő **billentyűk** szakaszában és intervallum megadása. Ebben a példában az 1 év használják.

![Ügyfél-azonosító][api-management-aad-client-id]

Mentse a konfigurációt, és a kulcs megjelenítéséhez a **Mentés** gombra. 

>[AZURE.IMPORTANT] Jegyezze fel a következő kulcsot. Miután bezárja Azure Active Directory konfigurációs ablakában, a kulcs nem jelenik meg újból.

Másolja a vágólapra a kulcs, lépjen vissza a publisher-portálon, a kulcs illessze be az **Ügyfél titkos** szövegdoboz, és kattintson a **Mentés**gombra.

![Engedély kiszolgáló hozzáadása][api-management-add-authorization-server-3]

Közvetlenül a következő az ügyfél hitelesítő adatait egy engedélyezési kód megadása. Engedély kód másolása, és lépjen vissza az Azure Active Directory developer portal alkalmazás a állítsa be, oldal és a Beillesztés az engedély megadása a **Válasz URL-cím** mezőbe, és kattintson a **Mentés** .

![Válasz URL-címe][api-management-aad-reply-url]

A következő lépésként az developer portal AAD alkalmazások engedélyeinek beállítása. Kattintson az **Alkalmazás engedélyek** parancsra, és jelölje be a jelölőnégyzetet **a címtár-adatok olvasása**. Kattintson a **Mentés** a módosítás mentése gombra, és válassza az **alkalmazás hozzáadása**parancsra.

![Engedélyek hozzáadása][api-management-add-devportal-permissions]

Kattintson a keresés ikonra, **APIM** írja be a mezőbe a kezdő **APIMAADDemo**jelölje ki, és kattintson a jelölőnégyzet be van jelölve a Mentés gombra.

![Engedélyek hozzáadása][api-management-aad-add-app-permissions]

Kattintson a **Meghatalmazott engedélyeit** **APIMAADDemo** és **Az Access APIMAADDemo**jelölje be a jelölőnégyzetet, és kattintson a **Mentés**gombra. A Fejlesztőeszközök így portál alkalmazás a kódmentes szolgáltatás eléréséhez.

![Engedélyek hozzáadása][api-management-aad-add-delegated-permissions]

## <a name="enable-oauth-20-user-authorization-for-the-calculator-api"></a>Az Számológép API OAuth 2.0 felhasználói engedélyezése

Most, hogy az OAuth 2.0-kiszolgálót, megadhatja az API a biztonsági beállításait. Ezt a lépést az igazolni 14:30 karaktertől kezdve a videóban.

Kattintson a bal oldali menü **API-k** , majd kattintson a **Számológép** megtekintése és a beállítások megadásához.

![Számológép API][api-management-calc-api]

Nyissa meg azt a **Biztonság** fülre, a **OAuth 2.0-s** jelölőnégyzetet, és jelölje ki a kívánt engedélyt kiszolgáló **engedélyezési kiszolgáló** legördülő, kattintson a **Mentés**gombra.

![Számológép API][api-management-enable-aad-calculator]

## <a name="successfully-call-the-calculator-api-from-the-developer-portal"></a>A Számológép API sikeresen hívhat a developer Portal segítségével

Most, hogy az OAuth 2.0-s engedélyt a API van beállítva, developer Center sikeresen hívható tevékenységét. Ebben a lépésben a 15:00 karaktertől kezdve a videó az igazolni.

Lépjen vissza a developer Portal segítségével a Számológép szolgáltatás **hozzáadása két egész számok** működését, és kattintson a **próbálja ki**. Megjegyzés: a megfelelő közvetlenül a felvétele után engedélyezési kiszolgálóra **engedélyezési** szakaszban az új elemet.

![Számológép API][api-management-calc-authorization-server]

Jelölje ki a **engedélyezési kód** a engedélyezési legördülő listából, és írja be a használata a fiók hitelesítő adatait. Ha már be van jelentkezve a fiók, előfordulhat, hogy nem kéri.

![Számológép API][api-management-devportal-authorization-code]

Kattintson a **Küldés** gombra, és jegyezze fel a **válaszállapota** **200 OK** és a válasz a tartalom a művelet eredménye.

![Számológép API][api-management-devportal-response]

## <a name="configure-a-desktop-application-to-call-the-api"></a>Asztali alkalmazás, hívja fel az API konfigurálása

A videó a következő eljárással 16:30 kezdődik, és egy egyszerű asztali alkalmazás, hívja fel az API beállítja. Az első lépésként az asztali alkalmazás regisztrálhatja az Azure Active Directory, és adjon neki hozzáférést a címtárhoz és a kódmentes szolgáltatás. A 18:25 csak az asztali alkalmazás, hívja fel a Számológép API műveletet szemléltetés.

## <a name="configure-a-jwt-validation-policy-to-pre-authorize-requests"></a>Előre a kérések engedélyezése JWT érvényességi házirend konfigurálása

A videó a végleges eljárás 20:48 kezdődik, és megtudhatja, hogy miként használja a [JWT ellenőrzése](https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#ValidateJWT) házirendjének előre ellenőrzi a hozzáférési jogkivonat, az egyes bejövő kérelmek a kérések engedélyezése. A érvényesítése JWT házirend nem érvényesítése a kérelmet, ha a kérelem API-kezeléssel zárolva van, és nem a kódmentes mentén átadott.

    <validate-jwt header-name="Authorization" failed-validation-httpcode="401" failed-validation-error-message="Unauthorized. Access token is missing or invalid.">
        <openid-config url="https://login.windows.net/DemoAPIM.onmicrosoft.com/.well-known/openid-configuration" />
        <required-claims>
            <claim name="aud">
                <value>https://DemoAPIM.NOTonmicrosoft.com/APIMAADDemo</value>
            </claim>
        </required-claims>
    </validate-jwt>

Ezzel a házirenddel és konfigurálása, egy másik bemutatása című [felhő terjed ki rész 177: további API-felügyeleti funkciókat](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) és 13:50 előretekerés. Előretekerése 15:00 a csoportházirend-szerkesztőben házirendeket megjelenítéséhez, majd a 18:50 szemléltető hívásakor egy műveletet a developer Portal segítségével a és a szükséges engedély jogkivonat nélkül is.

## <a name="next-steps"></a>Következő lépések
-   Nézze meg az API-kezelésével kapcsolatos további [Videók](https://azure.microsoft.com/documentation/videos/index/?services=api-management) .
-   Más módszerek a kódmentes szolgáltatás biztonságos olvassa el a [kölcsönös hitelesítési](api-management-howto-mutual-certificates.md) és [Csatlakozás a VPN Használatát, illetve készült ExpressRoute](api-management-howto-setup-vpn.md)című témakört.

[api-management-management-console]: ./media/api-management-howto-protect-backend-with-aad/api-management-management-console.png

[api-management-import-api]: ./media/api-management-howto-protect-backend-with-aad/api-management-import-api.png
[api-management-import-new-api]: ./media/api-management-howto-protect-backend-with-aad/api-management-import-new-api.png
[api-management-create-aad-menu]: ./media/api-management-howto-protect-backend-with-aad/api-management-create-aad-menu.png
[api-management-create-aad]: ./media/api-management-howto-protect-backend-with-aad/api-management-create-aad.png
[api-management-new-web-app]: ./media/api-management-howto-protect-backend-with-aad/api-management-new-web-app.png
[api-management-new-project]: ./media/api-management-howto-protect-backend-with-aad/api-management-new-project.png
[api-management-new-project-cloud]: ./media/api-management-howto-protect-backend-with-aad/api-management-new-project-cloud.png
[api-management-change-authentication]: ./media/api-management-howto-protect-backend-with-aad/api-management-change-authentication.png
[api-management-sign-in-vidual-studio]: ./media/api-management-howto-protect-backend-with-aad/api-management-sign-in-vidual-studio.png
[api-management-configure-web-app]: ./media/api-management-howto-protect-backend-with-aad/api-management-configure-web-app.png
[api-management-aad-domains]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-domains.png
[api-management-add-controller]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-controller.png
[api-management-web-publish]: ./media/api-management-howto-protect-backend-with-aad/api-management-web-publish.png
[api-management-aad-backend-app]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-backend-app.png
[api-management-aad-add-permissions]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-add-permissions.png
[api-management-developer-portal-menu]: ./media/api-management-howto-protect-backend-with-aad/api-management-developer-portal-menu.png
[api-management-dev-portal-apis]: ./media/api-management-howto-protect-backend-with-aad/api-management-dev-portal-apis.png
[api-management-dev-portal-try-it]: ./media/api-management-howto-protect-backend-with-aad/api-management-dev-portal-try-it.png
[api-management-dev-portal-send-401]: ./media/api-management-howto-protect-backend-with-aad/api-management-dev-portal-send-401.png
[api-management-aad-new-application-devportal]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-new-application-devportal.png
[api-management-aad-new-application-devportal-1]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-new-application-devportal-1.png
[api-management-aad-new-application-devportal-2]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-new-application-devportal-2.png
[api-management-aad-devportal-application]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-devportal-application.png
[api-management-add-authorization-server]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server.png
[api-management-aad-sso-uri]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-sso-uri.png
[api-management-aad-view-endpoints]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-view-endpoints.png
[api-management-aad-client-id]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-client-id.png
[api-management-add-authorization-server-1]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-1.png
[api-management-add-authorization-server-2]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-2.png
[api-management-add-authorization-server-2a]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-2a.png
[api-management-add-authorization-server-3]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-3.png
[api-management-aad-reply-url]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-reply-url.png
[api-management-add-devportal-permissions]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-devportal-permissions.png
[api-management-aad-add-app-permissions]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-add-app-permissions.png
[api-management-aad-add-delegated-permissions]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-add-delegated-permissions.png
[api-management-calc-api]: ./media/api-management-howto-protect-backend-with-aad/api-management-calc-api.png
[api-management-enable-aad-calculator]: ./media/api-management-howto-protect-backend-with-aad/api-management-enable-aad-calculator.png
[api-management-devportal-authorization-code]: ./media/api-management-howto-protect-backend-with-aad/api-management-devportal-authorization-code.png
[api-management-devportal-response]: ./media/api-management-howto-protect-backend-with-aad/api-management-devportal-response.png
[api-management-calc-authorization-server]: ./media/api-management-howto-protect-backend-with-aad/api-management-calc-authorization-server.png
[api-management-add-authorization-server-1a]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-1a.png
[api-management-client-credentials]: ./media/api-management-howto-protect-backend-with-aad/api-management-client-credentials.png
[api-management-new-aad-application-menu]: ./media/api-management-howto-protect-backend-with-aad/api-management-new-aad-application-menu.png

[Hozza létre az API Management szolgáltatás]: api-management-get-started.md#create-service-instance
[Az első API kezelése]: api-management-get-started.md
