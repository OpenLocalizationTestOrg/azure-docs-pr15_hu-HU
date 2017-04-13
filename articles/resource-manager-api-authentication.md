<properties 
   pageTitle="Active Directory-hitelesítés és erőforrás-kezelő |} Microsoft Azure"
   description="A Fejlesztőeszközök útmutató a többi Azure előfizetésben alkalmazás integrálása az Azure erőforrás-kezelő API-val és az Active Directory hitelesítés."
   services="azure-resource-manager,active-directory"
   documentationCenter="na"
   authors="dushyantgill"
   manager="timlt"
   editor="tysonn" />
<tags 
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/31/2016"
   ms.author="dugill;tomfitz" />

# <a name="how-to-use-azure-active-directory-and-resource-manager-to-manage-a-customers-resources"></a>Azure Active Directory- és erőforrás-kezelő használata egy ügyfél erőforrások kezelése

## <a name="introduction"></a>– Bevezetés

Ha egy szoftverfejlesztő, akinek van szüksége az ügyfél Azure erőforrások kezelő-alkalmazás létrehozása, a Ez a témakör bemutatja, hogyan hitelesíteni a Azure erőforrás-kezelő API-k és más előfizetések erőforrásainak eléréséhez szükséges. 

Az alkalmazás az erőforrás-kezelő API-k probléma több módon érheti el:

1. **Felhasználói + access alkalmazás**: az erőforrások bejelentkezett felhasználó nevében elérésére alkalmazások. Ezt a megközelítést működik, az alkalmazások, többek között a web Apps alkalmazások és a parancssori eszközök, amely csak "interaktív" Azure források kezelése foglalkozik.
1. **Csak alkalmazás hozzáférést**: például démon servicesben és az ütemezett feladat futtatása. Az alkalmazás identitás közvetlen hozzáférést forrásokat. Ezt a módszert akkor működik, például szükségük a hosszú távú "offline" Azure.

Ez a témakör részletes útmutatás mindkét hitelesítési módszer ellátott-alkalmazás létrehozása. Azt mutatja REST API-t a és C# minden egyes lépés végrehajtása. A teljes ASP.NET MVC kérelem [https://github.com/dushyantgill/VipSwapper/tree/master/CloudSense](https://github.com/dushyantgill/VipSwapper/tree/master/CloudSense)címen érhető el.

Ez a témakör a kódját fut, mint a [http://vipswapper.azurewebsites.net/cloudsense](http://vipswapper.azurewebsites.net/cloudsense)megpróbálhatja webalkalmazást. 

## <a name="what-the-web-app-does"></a>Mit jelent a web App alkalmazásban

A web App alkalmazásban:

1. Jelek modul az Azure felhasználója.
2. A felhasználó hozzáférést a web app erőforrás-kezelő kéri.
3. Erőforrás-kezelő eléréséhez felhasználói + alkalmazás hozzáférési jogkivonat beolvasása
4. Jogkivonat (a 3-as lépés) segítségével hívja fel az erőforrás-kezelő, és az alkalmazás szolgáltatás egyszerű hozzárendelése az előfizetés, és az alkalmazás hosszú távú hozzáférést biztosít az előfizetés szerepkörhöz.
5. Csak alkalmazás hozzáférési jogkivonat kap.
6. (Ez a lépés 5) jogkivonat alapján az erőforrás-kezelő előfizetés erőforrások kezelésére.

Az alábbiakban a webalkalmazás a végpontok közötti folyamat.

![Erőforrás-kezelő hitelesítési folyamat](./media/resource-manager-api-authentication/Auth-Swim-Lane.png)

Olyan felhasználóként a használni kívánt előfizetés adja meg az előfizetés azonosítója:

![Adja meg az előfizetés azonosítója](./media/resource-manager-api-authentication/sample-ux-1.png)

Jelölje be a naplózás a használt fiókot.

![Jelölje ki fiókját](./media/resource-manager-api-authentication/sample-ux-2.png)

Adja meg a hitelesítő adatait.

![Adja meg a hitelesítő adatok](./media/resource-manager-api-authentication/sample-ux-3.png)

Az alkalmazás hozzáférést az Azure-előfizetésekhez:
 
![Engedélyek biztosítása](./media/resource-manager-api-authentication/sample-ux-4.png)
 
A csatlakoztatott előfizetések kezelése:

![Csatlakozás előfizetés](./media/resource-manager-api-authentication/sample-ux-7.png)


## <a name="register-application"></a>Alkalmazás regisztrálása

Mielőtt megkezdené a kódolás, regisztráljon az Azure Active Directory (AD) a web App alkalmazásban. Az alkalmazás regisztrációs egy központi identitás az alkalmazás az Azure Active Directory hoz létre. Az alkalmazást, például a OAuth ügyfél-azonosító, a válasz URL-EK és a hitelesítő adatokat, az alkalmazás által használt hitelesítő és Azure erőforrás-kezelő API-khoz elérése vonatkozó alapadatok tárolja azt. Az alkalmazás regisztrációs is rögzíti a különböző delegált engedélyeket, hogy az alkalmazás van szüksége, amikor a felhasználó nevében Microsoft APIs. 

Az alkalmazás fér hozzá a más típusú előfizetés esetén, mivel több bérlői alkalmazásként kell adnia. Adatérvényesítési átadni adja meg az Active Directory társított tartomány. Ha látni szeretné a tartományokat az Active Directory társított, jelentkezzen be a [Klasszikus portálon](https://manage.windowsazure.com). Jelölje ki az Active Directory, és válassza a **tartományok**.

A következő példa bemutatja, hogyan lehet rögzíteni az alkalmazás Azure PowerShell használatával. A legújabb (augusztus 2016) Ez a parancs használata az Azure PowerShell kell rendelkeznie. 

    $app = New-AzureRmADApplication -DisplayName "{app name}" -HomePage "https://{your domain}/{app name}" -IdentifierUris "https://{your domain}/{app name}" -Password "{your password}" -AvailableToOtherTenants $true
    
Jelentkezzen be az Active Directory-alkalmazást, szüksége van az alkalmazás azonosítóját és jelszavát. A függvény által visszaadott az előző parancs azonosítója megjelenítéséhez használja:

    $app.ApplicationId

A következő példa bemutatja, hogyan lehet rögzíteni az alkalmazás Azure CLI használatával. 

    azure ad app create --name {app name} --home-page https://{your domain}/{app name} --identifier-uris https://{your domain}/{app name} --password {your password} --available true

Az eredmények alkalmazásként hitelesítés szükséges AppId tartalmazza.

### <a name="optional-configuration---certificate-credential"></a>Választható beállítási - tanúsítvány hitelesítő adatok

Azure Active Directory tanúsítvány hitelesítő adatokat is támogatja az alkalmazások: egy önaláírt tanúsítvány létrehozása, a titkos kulcs megőrzése és a nyilvános kulcs hozzáadása az Azure Active Directory-alkalmazás regisztrációs. Hitelesítés esetén az alkalmazás egy kis tartalom küld az Azure Active Directory, a titkos kulccsal aláírt, és Azure Active Directory ellenőrzi az aláírást, a nyilvános kulccsal regisztrált.

Az Active Directory-alkalmazások létrehozását a tanúsítvánnyal kapcsolatos további tudnivalókért lásd: [Azure PowerShell használatá erőforrások elérése egyszerű szolgáltatás hozzon létre](resource-group-authenticate-service-principal.md#create-service-principal-with-certificate) vagy [Használata Azure CLI hozhat létre egy egyszerű erőforrások eléréséhez szolgáltatás](resource-group-authenticate-service-principal-cli.md#create-service-principal-with-certificate).

## <a name="get-tenant-id-from-subscription-id"></a>Bérlői azonosító beszerzése előfizetés azonosítója

Erőforrás-kezelő felhívására használható token kéréséhez a alkalmazást kell, hogy a Azure AD-bérlő az Azure előfizetés üzemeltető bérlői azonosítója. Nagy valószínűséggel, hogy a felhasználók előfizetés azonosítók, de esetleg nem tudják, hogy a bérlői azonosítók az Active Directory. Úgy juthat az bérlői felhasználóazonosító, kérje meg a felhasználót a előfizetés azonosító. Ha az előfizetéssel kapcsolatos kérelem küld, adja meg, hogy az előfizetés azonosítója:

    https://management.azure.com/subscriptions/{subscription-id}?api-version=2015-01-01

A kérelem sikertelen, mert a felhasználó nem kijelentkezett még, de a bérlői azonosító beolvashatja a kérdésre adott választ. Az, hogy a kivétel a válasz élőfej értékből a bérlői azonosító lekérése **WWW hitelesítést végezni**. Ez a megvalósítás [GetDirectoryForSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L20) módszer látni.

## <a name="get-user--app-access-token"></a>Felhasználói + alkalmazás hozzáférési jogkivonat beszerzése

Az alkalmazás képes a felhasználó Azure AD-OAuth 2.0-s engedélyezheti kérelem - hitelesítést végezni a felhasználó hitelesítő adatait, és térjen vissza az engedélyezési kód a. Az alkalmazás a engedélyezési kód segítségével get-hozzáférési jogkivonat az erőforrás-kezelő. A [ConnectSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/Controllers/HomeController.cs#L42) módszerrel hoz létre az engedélyezési kérelmet.

Ez a témakör bemutatja a REST API kérések csatlakozó felhasználók hitelesítését. Segítő tárak segítségével hajtsa végre a hitelesítést a kódot. A tárak kapcsolatos további tudnivalókért olvassa el az [Azure Active Directory Authentication tárak](./active-directory/active-directory-authentication-libraries.md)című témakört. A integrálása az Identitáskezelés az alkalmazásokban című témakörben olvashat [Fejlesztőeszközök Azure Active Directory-ban](./active-directory/active-directory-developers-guide.md).

### <a name="auth-request-oauth-20"></a>Auth kérelem (OAuth 2.0)

Egy megnyitott azonosító csatlakozás/OAuth2.0 engedélyezheti kérése ki az Azure Active Directory engedélyezése végpontot:

    https://login.microsoftonline.com/{tenant-id}/OAuth2/Authorize

A kérés a rendelkezésre álló karakterlánc lekérdezésparaméter megkötésekről a [kérést egy engedélyezési kódot](./active-directory/active-directory-protocols-oauth-code.md#request-an-authorization-code) .

A következő példa bemutatja, hogy hogyan kérhet OAuth2.0 engedélyezése:

    https://login.microsoftonline.com/{tenant-id}/OAuth2/Authorize?client_id=a0448380-c346-4f9f-b897-c18733de9394&response_mode=query&response_type=code&redirect_uri=http%3a%2f%2fwww.vipswapper.com%2fcloudsense%2fAccount%2fSignIn&resource=https%3a%2f%2fgraph.windows.net%2f&domain_hint=live.com

Azure AD hitelesíti a felhasználót, és, ha szükséges, a program rákérdez a felhasználót, hogy engedélyt adhat az alkalmazást. A engedélyezési kód visszatér az alkalmazás válasz URL-CÍMÉT. Függően vagy a kért response_mode Azure AD vissza az adatokat küld lekérdezési karakterláncban, vagy bejegyzés adatok.

    code=AAABAAAAiL****FDMZBUwZ8eCAA&session_state=2d16bbce-d5d1-443f-acdf-75f6b0ce8850

### <a name="auth-request-open-id-connect"></a>Auth kérelem (Megnyitás azonosító csatlakozás)

Ha nem csak szeretne hozzáférni az Azure erőforrás-kezelő a felhasználó nevében, de azt is, hogy a felhasználót, hogy az alkalmazás az Azure Active Directory-fiók használatával jelentkezzen be, ki egy megnyitott azonosító csatlakozás engedélyezése kérése. A megnyitott azonosító csatlakozni az alkalmazás is kap egy id_token az Azure Active Directory, a felhasználónak bejelentkezés használható az alkalmazás.

A kérés a rendelkezésre álló karakterlánc lekérdezésparaméter megkötésekről a [elküldeni a bejelentkezési kérelmet](./active-directory/active-directory-protocols-openid-connect-code.md#send-the-sign-in-request) .

A következő példa megnyitott azonosító csatlakozás kérelmet:

     https://login.microsoftonline.com/{tenant-id}/OAuth2/Authorize?client_id=a0448380-c346-4f9f-b897-c18733de9394&response_mode=form_post&response_type=code+id_token&redirect_uri=http%3a%2f%2fwww.vipswapper.com%2fcloudsense%2fAccount%2fSignIn&resource=https%3a%2f%2fgraph.windows.net%2f&scope=openid+profile&nonce=63567Dc4MDAw&domain_hint=live.com&state=M_12tMyKaM8

Azure AD hitelesíti a felhasználót, és, ha szükséges, a program rákérdez a felhasználót, hogy engedélyt adhat az alkalmazást. A engedélyezési kód visszatér az alkalmazás válasz URL-CÍMÉT. Függően vagy a kért response_mode Azure AD vissza az adatokat küld lekérdezési karakterláncban, vagy bejegyzés adatok.

Egy megnyitott azonosító csatlakozás válasz példája:

    code=AAABAAAAiL*****I4rDWd7zXsH6WUjlkIEQxIAA&id_token=eyJ0eXAiOiJKV1Q*****T3GrzzSFxg&state=M_12tMyKaM8&session_state=2d16bbce-d5d1-443f-acdf-75f6b0ce8850

### <a name="token-request-oauth20-code-grant-flow"></a>Jogkivonat kérelem (OAuth2.0 kód megadása Flow)

Most, hogy az alkalmazás a engedélyezési kód kapott Azure Active Directory, érdemes időt szánni a hozzáférési jogkivonat az Azure erőforrás parancsra.  Közzé egy OAuth2.0 kód megadása jogkivonat kérése az Azure Active Directory jogkivonat végpontot: 

    https://login.microsoftonline.com/{tenant-id}/OAuth2/Token

A kérés a rendelkezésre álló karakterlánc lekérdezésparaméter megkötésekről a [használja a engedélyt](./active-directory/active-directory-protocols-oauth-code.md#use-the-authorization-code-to-request-an-access-token) .

A következő példa bemutatja a jelszó hitelesítő kód megadása jogkivonat kér:

    POST https://login.microsoftonline.com/7fe877e6-a150-4992-bbfe-f517e304dfa0/oauth2/token HTTP/1.1

    Content-Type: application/x-www-form-urlencoded
    Content-Length: 1012

    grant_type=authorization_code&code=AAABAAAAiL9Kn2Z*****L1nVMH3Z5ESiAA&redirect_uri=http%3A%2F%2Flocalhost%3A62080%2FAccount%2FSignIn&client_id=a0448380-c346-4f9f-b897-c18733de9394&client_secret=olna84E8*****goScOg%3D

Tanúsítvány hitelesítő adatokkal használatakor hozzon létre egy JSON webes jogkivonat (JWT) és a bejelentkezési (RSA SHA256), az alkalmazás tanúsítvány hitelesítő adatok titkos kulccsal. A token állítást felsorolt [JWT jogkivonat követelések](./active-directory/active-directory-protocols-oauth-code.md#jwt-token-claims)jelennek meg. Hivatkozás az [Active Directory Auth tár (.NET) kód](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/blob/dev/src/ADAL.PCL.Desktop/CryptographyHelper.cs) jelentkezzen be az ügyfél állítás JWT tokenek talál.

További információ a [megnyitott azonosító csatlakozás specifikáció](http://openid.net/specs/openid-connect-core-1_0.html#ClientAuthentication) lássanak ügyfél-hitelesítést. 

A következő példa bemutatja a tanúsítvány hitelesítő kód megadása jogkivonat kér:

    POST https://login.microsoftonline.com/7fe877e6-a150-4992-bbfe-f517e304dfa0/oauth2/token HTTP/1.1
    
    Content-Type: application/x-www-form-urlencoded
    Content-Length: 1012
    
    grant_type=authorization_code&code=AAABAAAAiL9Kn2Z*****L1nVMH3Z5ESiAA&redirect_uri=http%3A%2F%2Flocalhost%3A62080%2FAccount%2FSignIn&client_id=a0448380-c346-4f9f-b897-c18733de9394&client_assertion_type=urn%3Aietf%3Aparams%3Aoauth%3Aclient-assertion-type%3Ajwt-bearer&client_assertion=eyJhbG*****Y9cYo8nEjMyA

A kód egy példa választ adni token: 

    HTTP/1.1 200 OK

    {"token_type":"Bearer","expires_in":"3599","expires_on":"1432039858","not_before":"1432035958","resource":"https://management.core.windows.net/","access_token":"eyJ0eXAiOiJKV1Q****M7Cw6JWtfY2lGc5A","refresh_token":"AAABAAAAiL9Kn2Z****55j-sjnyYgAA","scope":"user_impersonation","id_token":"eyJ0eXAiOiJKV*****-drP1J3P-HnHi9Rr46kGZnukEBH4dsg"}

#### <a name="handle-code-grant-token-response"></a>Kód megadása jogkivonat válasz kezelése

A sikeres jogkivonat válasz tartalmaz (felhasználói + az alkalmazás) hozzáférési jogkivonat az Azure erőforrás parancsra. Az alkalmazás Ez a jogkivonat használja az erőforrás-kezelő érhető el a felhasználó nevében. Azure Active Directory által kibocsátott hozzáférési jogkivonat élettartama egy órával. Valószínű, hogy a webalkalmazás kell-e a (felhasználói + alkalmazás) megújítása hozzáférési jogkivonat. Ha azt szeretné megújítani az jogkivonat, használja a frissítés jogkivonat, hogy az alkalmazás a token válasz kap. Közzé egy OAuth2.0 jogkivonat kérése az Azure Active Directory jogkivonat végpontot: 

    https://login.microsoftonline.com/{tenant-id}/OAuth2/Token

A paraméterek használata a frissítési kérést [frissítése az jogkivonat](./active-directory/active-directory-protocols-oauth-code.md#refreshing-the-access-tokens)témakörben olvashat.

A következő példa bemutatja a frissítés használata jogkivonat:

    POST https://login.microsoftonline.com/7fe877e6-a150-4992-bbfe-f517e304dfa0/oauth2/token HTTP/1.1

    Content-Type: application/x-www-form-urlencoded
    Content-Length: 1012

    grant_type=refresh_token&refresh_token=AAABAAAAiL9Kn2Z****55j-sjnyYgAA&client_id=a0448380-c346-4f9f-b897-c18733de9394&client_secret=olna84E8*****goScOg%3D

Frissítés tokenek kérjen új hozzáférési jogkivonat az Azure erőforrás-kezelő használható, bár azok nem alkalmasak a kapcsolat nélküli hozzáférés az alkalmazás. A frissítés tokenek élettartam korlátozódik, és a felhasználó frissítés tokenek kötött. Ha a felhasználó elhagyja a szervezetet, az alkalmazást a frissítés jogkivonat megszakad a hozzáférést. Ezt a megközelítést nem alkalmas az Azure erőforrások kezelésére csapatok által használt alkalmazásokat.

## <a name="check-if-user-can-assign-access-to-subscription"></a>Ha felhasználói hozzáférés hozzárendelheti előfizetés ellenőrzése

Az alkalmazás letöltése a felhasználó nevében Azure erőforrás-kezelő eléréséhez token rendelkezik. A következő lépésben csatlakozik, az alkalmazás az előfizetéséhez. A csatlakozás után az alkalmazás kezelhetik ezen előfizetések akkor is, ha a felhasználónak nincs jelen (hosszú távú az offline hozzáférés). 

Az egyes előfizetések csatlakozni hívja fel az [erőforrás-kezelő Listaengedélyek](https://msdn.microsoft.com/library/azure/dn906889.aspx) API határozza meg, hogy a felhasználó rendelkezik-e jogosultsága kezelése az előfizetés.

A [UserCanManagerAccessForSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L44) módszer az ASP.NET MVC minta alkalmazás végrehajtja a hívást.

A következő példa bemutatja, hogy hogyan kérhet a felhasználó engedélyeinek előfizetést. 83cfe939-2402-4581-b761-4f59b0a041e4 az előfizetés azonosítója.

    GET https://management.azure.com/subscriptions/83cfe939-2402-4581-b761-4f59b0a041e4/providers/microsoft.authorization/permissions?api-version=2015-07-01 HTTP/1.1

    Authorization: Bearer eyJ0eXAiOiJKV1QiLC***lwO1mM7Cw6JWtfY2lGc5A

Példa a választ az első felhasználó engedélyeit a előfizetés van:

    HTTP/1.1 200 OK

    {"value":[{"actions":["*"],"notActions":["Microsoft.Authorization/*/Write","Microsoft.Authorization/*/Delete"]},{"actions":["*/read"],"notActions":[]}]}

Az engedélyek API több engedélyek adja eredményül. Minden jogosultsági engedélyezett műveletek (műveletek) áll, és nem engedélyezett műveletek (notactions). Ha a művelet megtalálható minden jogosultsági engedélyezett műveletek listáját, és nem szerepel a ezt a jogosultságot notactions listáját, a felhasználó jogosult a művelet végrehajtásához. **Microsoft.Authorization/RoleAssignments/Write** egy, a művelet, hogy, hogy hozzáférést biztosít a management rights. Az alkalmazás kell értelmezni az engedélyek eredmény kereséséhez kattintson a műveletek és az egyes engedély notactions Ez a művelet karakterlánc regex egyező.

## <a name="get-app-only-access-token"></a>Csak alkalmazás hozzáférési jogkivonat beszerzése

Ezután tudja, hogy ha a felhasználó hozzáférést oszthatnak ki az Azure előfizetést. A következő lépések a következők:

1. A megfelelő RBAC szerepkör hozzárendelése az alkalmazás identitásának az előfizetést.
2. Az access-hozzárendelés lekérdezése az alkalmazás engedélyt az előfizetéshez, illetve csak alkalmazás token használatával erőforrás-kezelő elérése ellenőrzése
1. Rögzítse a kapcsolatot az alkalmazások "csatlakoztatott előfizetések" adatstruktúra - pályától tartós az előfizetés azonosítója.

Nézzük meg az első lépés a közelebb. A megfelelő RBAC szerepkör hozzárendelése az alkalmazás azonosítóját, meg kell határoznia:

- Az alkalmazás azonosítóját, a felhasználó Azure Active Directory objektum azonosítója
- A RBAC szerepkört, amely az előfizetéshez szükséges az alkalmazás azonosítója

Az alkalmazás hitelesíti egy felhasználó az Azure AD, amikor az adott Azure Active Directory létrehoz egy egyszerű szolgáltatás-objektum, az alkalmazás. Azure lehetővé teszi, hogy a RBAC szerepkörök rendelni kívánt szolgáltatás rendszerbiztonsági Azure erőforrások megfelelő alkalmazások közvetlen hozzáférés megadását. Ez a művelet pontosan, mit azt kíván. Lekérdezés az Azure Active Directory Graph API határozza meg az alkalmazást a bejelentkezett felhasználó egyszerű azonosítója Azure AD meg.

Csak van egy hozzáférési jogkivonat az Azure erőforrás-kezelő – szüksége van egy új jogkivonat, hívja fel az Azure Active Directory Graph API-t. Azure AD-minden alkalmazás saját szolgáltatás fő objektum lekérdezni, ezért csak alkalmazás jogkivonat megfelelő engedéllyel rendelkezik.

<a id="app-azure-ad-graph">
### <a name="get-app-only-access-token-for-azure-ad-graph-api"></a>Beszerzése csak alkalmazás hozzáférési jogkivonat Azure Active Directory Graph API

Az alkalmazás hitelesítő és Azure Active Directory Graph API jogkivonat megtekintéséhez ki egy ügyfél hitelesítőadat-támogatás OAuth2.0 folyamat jogkivonat kérelmet Azure Active Directory jogkivonat végpontra (**https://login.microsoftonline.com/ {directory_domain_name} / oauth2 hitelesítési mód/jogkivonat**).

Az ASP.net MVC minta alkalmazás [GetObjectIdOfServicePrincipalInOrganization](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureADGraphAPIUtil.cs) metódusát kap egy csak alkalmazás hozzáférést jogkivonat Graph API használja az Active Directory Authentication Library a .NET rendszerhez.

A kérés a rendelkezésre álló karakterlánc lekérdezésparaméter megkötésekről a [kérelem-hozzáférési jogkivonat](./active-directory/active-directory-protocols-oauth-service-to-service.md#request-an-access-token) .

Példa felkérés ügyfél hitelesítőadat-támogatás jogkivonat: 

    POST https://login.microsoftonline.com/62e173e9-301e-423e-bcd4-29121ec1aa24/oauth2/token HTTP/1.1
    Content-Type: application/x-www-form-urlencoded
    Content-Length: 187</pre>
    <pre>grant_type=client_credentials&client_id=a0448380-c346-4f9f-b897-c18733de9394&resource=https%3A%2F%2Fgraph.windows.net%2F &client_secret=olna8C*****Og%3D

Példa ügyfél hitelesítő választ adni token: 

    HTTP/1.1 200 OK

    {"token_type":"Bearer","expires_in":"3599","expires_on":"1432039862","not_before":"1432035962","resource":"https://graph.windows.net/","access_token":"eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNBVGZNNXBPWWlKSE1iYTlnb0VLWSIsImtpZCI6Ik1uQ19WWmNBVGZNNXBPWWlKSE1iYTlnb0VLWSJ9.eyJhdWQiOiJodHRwczovL2dyYXBoLndpbmRv****G5gUTV-kKorR-pg"}

### <a name="get-objectid-of-application-service-principal-in-user-azure-ad"></a>Az alkalmazás szolgáltatás egyszerű objektumazonosító egyben az Azure Active Directory felhasználói

Most az alkalmazás csak jogkivonat használatával lekérdezheti a az [Azure Active Directory Graph szolgáltatás rendszerbiztonsági](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity) határozza meg az alkalmazás szolgáltatás fő a címtárban objektumazonosító API.

Az ASP.net MVC minta alkalmazás [GetObjectIdOfServicePrincipalInOrganization](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureADGraphAPIUtil.cs#) metódusát végrehajtja a hívást.

A következő példa bemutatja, hogy hogyan kérhet egy alkalmazás szolgáltatás egyszerű. a0448380-c346-4f9f-b897-c18733de9394 az alkalmazás az ügyfél-azonosító.

    GET https://graph.windows.net/62e173e9-301e-423e-bcd4-29121ec1aa24/servicePrincipals?api-version=1.5&$filter=appId%20eq%20'a0448380-c346-4f9f-b897-c18733de9394' HTTP/1.1

    Authorization: Bearer eyJ0eXAiOiJK*****-kKorR-pg

A következő példa bemutatja az alkalmazások szolgáltatási kérelem adott válasz fő 

    HTTP/1.1 200 OK

    {"odata.metadata":"https://graph.windows.net/62e173e9-301e-423e-bcd4-29121ec1aa24/$metadata#directoryObjects/Microsoft.DirectoryServices.ServicePrincipal","value":[{"odata.type":"Microsoft.DirectoryServices.ServicePrincipal","objectType":"ServicePrincipal","objectId":"9b5018d4-6951-42ed-8a92-f11ec283ccec","deletionTimestamp":null,"accountEnabled":true,"appDisplayName":"CloudSense","appId":"a0448380-c346-4f9f-b897-c18733de9394","appOwnerTenantId":"62e173e9-301e-423e-bcd4-29121ec1aa24","appRoleAssignmentRequired":false,"appRoles":[],"displayName":"CloudSense","errorUrl":null,"homepage":"http://www.vipswapper.com/cloudsense","keyCredentials":[],"logoutUrl":null,"oauth2Permissions":[{"adminConsentDescription":"Allow the application to access CloudSense on behalf of the signed-in user.","adminConsentDisplayName":"Access CloudSense","id":"b7b7338e-683a-4796-b95e-60c10380de1c","isEnabled":true,"type":"User","userConsentDescription":"Allow the application to access CloudSense on your behalf.","userConsentDisplayName":"Access CloudSense","value":"user_impersonation"}],"passwordCredentials":[],"preferredTokenSigningKeyThumbprint":null,"publisherName":"vipswapper"quot;,"replyUrls":["http://www.vipswapper.com/cloudsense","http://www.vipswapper.com","http://vipswapper.com","http://vipswapper.azurewebsites.net","http://localhost:62080"],"samlMetadataUrl":null,"servicePrincipalNames":["http://www.vipswapper.com/cloudsense","a0448380-c346-4f9f-b897-c18733de9394"],"tags":["WindowsAzureActiveDirectoryIntegratedApp"]}]}

### <a name="get-azure-rbac-role-identifier"></a>Azure RBAC szerepkör-azonosító beszerzése

A megfelelő RBAC szerepkör hozzárendelése a legfontosabb szolgáltatásban, meg kell határoznia az Azure RBAC szerepkör azonosítója.

A jobb oldali RBAC szerepkör az alkalmazás:

- Ha az alkalmazás csak figyeli az előfizetését, anélkül, hogy a módosításokat, az előfizetés csak olvasó engedélyeinek van szükség. Rendelje hozzá az **olvasó** szerepkörhöz.
- Ha az alkalmazás kezeli Azure az előfizetést, létrehozása és módosítása és törlése szervezetek szükséges a munkatárs engedélyekkel közül.
  - Egy adott típusú erőforrás kezelése, rendelje hozzá az erőforrás-specifikus munkatársi szerepkörök (virtuális gép közreműködői, virtuális hálózati közreműködői, tárterület-fiók közös munka stb.)
  - Erőforrás bármilyen felügyeletéhez vonatkozó **munkatársi** szerepkörök hozzárendelése.

Az alkalmazás szerepkör-hozzárendelés a felhasználók számára, ezért válassza a legkisebb szükséges jogosultságot.

Hívja fel az [erőforrás-kezelő szerepkör-definíció API](https://msdn.microsoft.com/library/azure/dn906879.aspx) Azure RBAC szerepkörök és a Keresés listában, majd az eredmény a kívánt szerepkört definíció keresése név szerint ismétlés.

A [GetRoleId](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L246) módszer az ASP.net MVC minta alkalmazás végrehajtja a hívást.

Ez a példa kérelem Azure RBAC szerepkör-azonosító beszerzése jeleníti meg. 09cbd307-aa71-4aca-b346-5f253e6e3ebb az előfizetés azonosítója.

    GET https://management.azure.com/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleDefinitions?api-version=2015-07-01 HTTP/1.1

    Authorization: Bearer eyJ0eXAiOiJKV*****fY2lGc5

A válasz van, a következő formátumban: 

    HTTP/1.1 200 OK

    {"value":[{"properties":{"roleName":"API Management Service Contributor","type":"BuiltInRole","description":"Lets you manage API Management services, but not access to them.","scope":"/","permissions":[{"actions":["Microsoft.ApiManagement/Services/*","Microsoft.Authorization/*/read","Microsoft.Resources/subscriptions/resources/read","Microsoft.Resources/subscriptions/resourceGroups/read","Microsoft.Resources/subscriptions/resourceGroups/resources/read","Microsoft.Resources/subscriptions/resourceGroups/deployments/*","Microsoft.Insights/alertRules/*","Microsoft.Support/*"],"notActions":[]}]},"id":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleDefinitions/312a565d-c81f-4fd8-895a-4e21e48d571c","type":"Microsoft.Authorization/roleDefinitions","name":"312a565d-c81f-4fd8-895a-4e21e48d571c"},{"properties":{"roleName":"Application Insights Component Contributor","type":"BuiltInRole","description":"Lets you manage Application Insights components, but not access to them.","scope":"/","permissions":[{"actions":["Microsoft.Insights/components/*","Microsoft.Insights/webtests/*","Microsoft.Authorization/*/read","Microsoft.Resources/subscriptions/resources/read","Microsoft.Resources/subscriptions/resourceGroups/read","Microsoft.Resources/subscriptions/resourceGroups/resources/read","Microsoft.Resources/subscriptions/resourceGroups/deployments/*","Microsoft.Insights/alertRules/*","Microsoft.Support/*"],"notActions":[]}]},"id":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleDefinitions/ae349356-3a1b-4a5e-921d-050484c6347e","type":"Microsoft.Authorization/roleDefinitions","name":"ae349356-3a1b-4a5e-921d-050484c6347e"}]}

Nem kell fordulnia a Ez az API folyamatos kombinálásával. Miután megadta, már jól ismert GUID azonosítója szerepkör-definíció, mint szerepkör definition azonosítóját állíthat össze:

    /subscriptions/{subscription_id}/providers/Microsoft.Authorization/roleDefinitions/{well-known-role-guid}

Az alábbiakban a jól ismert GUID gyakran használt beépített szerepkörök:

| Szerepkör | Globálisan egyedi azonosítója |
| ----- | ------ |
| A képernyőolvasók | acdd72a7-3385-48EF-bd42-f606fba81ae7
| Közös munka | b24988ac-6180-42a0-ab88-20f7382dd24c
| Virtuális gép közös munka | d73bb868-a0df-4d4d-bd69-98a00b01fccb
| Virtuális hálózati közös munka | b34d265f-36f7-4a0d-a4d4-e158ca92e90f
| Tárterület-fiók közös munka | 86e8f5dc-a6e9-4c67-9d15-de283e8eac25
| Webhely közös munka | de139f84-1756-47ae-9be6-808fbbe84772
| Webes csomag közös munka | 2cc479cb-7b4d-49a8-b449-8c00fd0f0a4b
| Az SQL Server közös munka | 6d8ee4ec-f05a-4a1d-8b00-a9b17e38b437
| SQL-adatbázis közös munka | 9b7fa17d-e63e-47b0-bb0a-15c516ac86ec

### <a name="assign-rbac-role-to-application"></a>Alkalmazás RBAC szerepkör hozzárendelése

Ha mindent kell a megfelelő RBAC szerepkör hozzárendelése a legfontosabb szolgáltatásban az [erőforrás-kezelő létrehozása szerepkör-hozzárendelés](https://msdn.microsoft.com/library/azure/dn906887.aspx) API segítségével.

A [GrantRoleToServicePrincipalOnSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L170) módszer az ASP.net MVC minta alkalmazás végrehajtja a hívást.

Példa felkérés alkalmazás RBAC szerepkör hozzárendelése: 

    PUT https://management.azure.com/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/microsoft.authorization/roleassignments/4f87261d-2816-465d-8311-70a27558df4c?api-version=2015-07-01 HTTP/1.1

    Authorization: Bearer eyJ0eXAiOiJKV1QiL*****FlwO1mM7Cw6JWtfY2lGc5
    Content-Type: application/json
    Content-Length: 230

    {"properties": {"roleDefinitionId":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleDefinitions/acdd72a7-3385-48ef-bd42-f606fba81ae7","principalId":"c3097b31-7309-4c59-b4e3-770f8406bad2"}}

A kérelem az alábbi értékeket használatosak:

| Globálisan egyedi azonosítója | Leírás |
| ------ | --------- |
| 09cbd307-aa71-4aca-b346-5f253e6e3ebb | az előfizetés azonosítója
| c3097b31-7309-4c59-b4e3-770f8406bad2 | az alkalmazás a szolgáltatás egyszerű objektum azonosítója
| acdd72a7-3385-48EF-bd42-f606fba81ae7 | az Olvasó szerepkör azonosítója
| 4f87261d-2816-465d-8311-70a27558df4c | egy új szerepkör-hozzárendelés által létrehozott új globálisan egyedi azonosítója

A válasz van, a következő formátumban: 

    HTTP/1.1 201 Created

    {"properties":{"roleDefinitionId":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleDefinitions/acdd72a7-3385-48ef-bd42-f606fba81ae7","principalId":"c3097b31-7309-4c59-b4e3-770f8406bad2","scope":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb"},"id":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleAssignments/4f87261d-2816-465d-8311-70a27558df4c","type":"Microsoft.Authorization/roleAssignments","name":"4f87261d-2816-465d-8311-70a27558df4c"}

### <a name="get-app-only-access-token-for-azure-resource-manager"></a>Hozzáférés csak alkalmazás jogkivonat az Azure erőforrás parancsra

Ellenőrizze, hogy az alkalmazás rendelkezik a kívánt is elérheti az előfizetést, az alkalmazás csak token használatával előfizetés próba feladatokat.

Úgy juthat az alkalmazás csak access jogkivonat, kövesse az utasításokat a szakaszának [első jogkivonat az Azure Active Directory Graph API csak alkalmazás hozzáférést](#app-azure-ad-graph), az erőforrás paraméter értékét: 

    https://management.core.windows.net/

Az ASP.NET MVC minta alkalmazás [ServicePrincipalHasReadAccessToSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L110) metódusát kap egy csak alkalmazás hozzáférést jogkivonat az Azure erőforrás-kezelő használja az Active Directory Authentication Library a .NET rendszerhez.

#### <a name="get-applications-permissions-on-subscription"></a>Alkalmazás engedélyek beszerzése a előfizetés

Annak ellenőrzéséhez, hogy rendelkezik-e az alkalmazást a kívánt hozzáférés Azure-előfizetéshez, akkor is hívhat az [Erőforrás-kezelő engedélyek](https://msdn.microsoft.com/library/azure/dn906889.aspx) API-val. Ezt a megközelítést hasonlít hogyan megadta, hogy a felhasználó rendelkezik-e a hozzáférés-kezelés engedélyeit az előfizetést. Ebben az esetben azonban az engedélyeket az alkalmazás csak jogkivonat, amely az előző lépésben kapott API hívja fel.

A [ServicePrincipalHasReadAccessToSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L110) módszer az ASP.NET MVC minta alkalmazás végrehajtja a hívást.

## <a name="manage-connected-subscriptions"></a>Csatlakoztatott előfizetések kezelése

A megfelelő RBAC szerepkör az alkalmazás szolgáltatás fő az előfizetéshez van rendelve, amikor az alkalmazás is figyelése és kezelni a használatával csak alkalmazás hozzáférési jogkivonat az Azure erőforrás parancsra.

Ha egy előfizetés tulajdonosa a klasszikus portal vagy parancssori eszközök segítségével az alkalmazásokat szerepkör-hozzárendelés távolítja el, az alkalmazás már nem elérheti, hogy az előfizetés. Ebben az esetben érdemes értesíti a felhasználót, hogy az előfizetés a kapcsolatot a, az alkalmazás kívüli daraboltak és a nekik a kapcsolat "kijavítása" beállítást. "Javítás" egyszerűen volna hozza létre újból a szerepkör-hozzárendelés nélküli törölt.

Ahogyan engedélyezte a felhasználó előfizetések csatlakozhat az alkalmazást, engedélyeznie kell a felhasználó túl leválasztja az előfizetések. Az access kezelése szempontjából válassza az azt jelenti, hogy az előfizetése van az alkalmazás szolgáltatás egyszerű szerepkör-hozzárendelés eltávolítása le. Tetszés szerint az alkalmazás az előfizetéshez tartozó bármely állam előfordulhat, hogy távolítható el is. Csak az előfizetéshez tartozó adatkezelési jogosultsággal rendelkező felhasználók is tudják leválasztja az előfizetést.

A [RevokeRoleFromServicePrincipalOnSubscription módszer](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L200) az ASP.net MVC minta alkalmazás végrehajtja a hívást.

Ez - felhasználók most egyszerűen csatlakozás és a alkalmazással az Azure előfizetések kezelése.

