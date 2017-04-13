<properties
    pageTitle="Azure Active Directory B2C: Használja a diagram API |} Microsoft Azure"
    description="Hogyan lehet, hogy hívja fel a Graph API B2C bérlő alkalmazás identitás használatával automatizálhatja a folyamat."
    services="active-directory-b2c"
    documentationCenter=".net"
    authors="dstrockis"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/30/2016"
    ms.author="dastrock"/>

# <a name="azure-ad-b2c-use-the-graph-api"></a>Azure Active Directory B2C: Használja a diagram API

Azure Active Directory (Azure Active Directory) B2C bérlők gyakran igen nagy. Ez azt jelenti, hogy az sok bérlő kezelése művelet kell elvégezni programozás útján. Egy elsődleges példája felhasználókezelés. Szükség lehet egy meglévő felhasználó tárolóban áttelepítése B2C bérlői webhelyre. Előfordulhat, hogy kívánt üzemeltetni felhasználói regisztráció saját lapon és a felhasználói fiókokat létrehozni az Azure Active Directory, a háttérben. Következő típusú feladatokat kell az azt jelenti, hogy a létrehozás, Olvasás, frissítés, és törölje a felhasználói fiókok. Az alábbi műveleteket végezheti el az Azure Active Directory Graph API segítségével.

B2C bérlők a diagram API-val való kommunikáció két elsődleges módban vannak.

- Interaktív, egyszeres futtatás tevékenységek meg kell használni kívánt B2C-ös bérlői rendszergazdai fiókkal az műveletek végrehajtásakor. Ebben az üzemmódban van szükség rendszergazdai hitelesítő adataival jelentkezzen be, hogy a rendszergazda a Graph API bármely hívások végrehajtása előtt.
- Az automatikus, folytonos tevékenységhez adja meg a felügyeleti feladatok elvégzéséhez szükséges jogosultságokkal rendelkező szolgáltatásfiók bizonyos típusú kell használnia. Az Azure Active Directory akkor ehhez regisztrálása az alkalmazások és az Azure Active Directory hitelesítő. Ez történik, [Adja meg az OAuth 2.0 ügyfél hitelesítő adatait](../active-directory/active-directory-authentication-scenarios.md#daemon-or-server-application-to-web-api)használó **Azonosítója** segítségével. Ebben az esetben az alkalmazás végpontként magát nem felhasználóként, hívja fel a Graph API-t.

Ebben a cikkben mutatjuk be az automatikus használatieset módjáról. Bemutatják, hogy fogja összeállítása a .NET 4.5 `B2CGraphClient` , amely a felhasználó hajt végre létrehozása, olvasása, módosítása és törlési (CRUD) műveletek. Az ügyfél lesz a Windows parancssori kezelőfelületről, amely lehetővé teszi, hogy a különböző módszereket meghívásához. Interaktív, automatizált módon működnek azonban a kódot kell írni.

## <a name="get-an-azure-ad-b2c-tenant"></a>Ismerkedés az Azure Active Directory B2C bérlői

Mielőtt alkalmazások és a felhasználók létrehozása, vagy egyáltalán együttműködhet az Azure Active Directory, szüksége lesz az Azure Active Directory B2C bérlői és bérlőhöz globális rendszergazdai fiókkal. Ha nem rendelkezik a bérlői már, [első lépések az Azure Active Directory B2C](active-directory-b2c-get-started.md).

## <a name="register-a-service-application-in-your-tenant"></a>Szolgáltatásalkalmazás regisztrálhatja a bérlői webhelyén

Után B2C bérlői webhelyre, kell a szolgáltatásalkalmazás létrehozása az Azure Active Directory PowerShell-parancsmagok használatával.
Először töltse le és telepítse a [Microsoft Online Services – bejelentkezési segéd](http://go.microsoft.com/fwlink/?LinkID=286152). Töltse le és telepítheti a [64 bites Azure Active Directory modul Windows Powershellhez készült](http://go.microsoft.com/fwlink/p/?linkid=236297).

> [AZURE.IMPORTANT]
A diagram API használata a B2C bérlő, szüksége lesz egy dedikált alkalmazás rögzítése a PowerShell használatával. Kövesse az ebben a cikkben az utasítás megtennie. Az Azure-portálon regisztrálta már meglévő B2C az alkalmazások nem tudja használni.

Miután telepítette a PowerShell-modult, nyissa meg a PowerShell, és csatlakozzon az B2C bérlőhöz. Futtatása után `Get-Credential`, felkéri a program egy felhasználónevet és jelszót, írja be a felhasználónevét és jelszavát a B2C bérlői rendszergazdai fiókkal.

```
> $msolcred = Get-Credential
> Connect-MsolService -credential $msolcred
```

Az alkalmazás hoz létre, mielőtt szeretne létrehozni egy új **ügyfél titkos**.  Azure Active Directory hitelesíteni, és szerezheti be a hozzáférési jogkivonat, az alkalmazás az ügyfél titkos fogja használni. A PowerShell egy érvényes titkos hozhat létre:

```
> $bytes = New-Object Byte[] 32
> $rand = [System.Security.Cryptography.RandomNumberGenerator]::Create()
> $rand.GetBytes($bytes)
> $rand.Dispose()
> $newClientSecret = [System.Convert]::ToBase64String($bytes)
> $newClientSecret
```

A véglegesként parancsot kell nyomtassa ki az új ügyfél titkos. Másolja a vágólapra biztonságos helyen. Újabb mobiltelefon szükséges. Most már elkészítheti az alkalmazást, mert az új ügyfél titkos, a hitelesítő adatok, az alkalmazás:

```
> New-MsolServicePrincipal -DisplayName "My New B2C Graph API App" -Type password -Value $newClientSecret

DisplayName           : My New B2C Graph API App
ServicePrincipalNames : {dd02c40f-1325-46c2-a118-4659db8a55d5}
ObjectId              : e2bde258-6691-410b-879c-b1f88d9ef664
AppPrincipalId        : dd02c40f-1325-46c2-a118-4659db8a55d5
TrustedForDelegation  : False
AccountEnabled        : True
Addresses             : {}
KeyType               : Password
KeyId                 : a261e39d-953e-4d6a-8d70-1f915e054ef9
StartDate             : 9/2/2015 1:33:09 AM
EndDate               : 9/2/2016 1:33:09 AM
Usage                 : Verify
```

Ha sikeresen létre az alkalmazást, meg kell nyomtassa ki az alkalmazásról, mint a fenti tulajdonságok is. Mindkét kell `ObjectId` és `AppPrincipalId`, így másolása túl ezeket az értékeket.

Miután létrehozott egy alkalmazást a B2C bérlői webhelyén, kell a felhasználói CRUD műveletek elvégzéséhez szükséges engedélyek kiosztása. Az alkalmazás három szerepköröket rendelhet hozzájuk: címtár-olvasók (számára, olvassa el a felhasználók), a címtár szövegírói (hozhat létre, és a felhasználók frissítése), és a felhasználói fiók rendszergazdája (a felhasználó törlése). Ezeket a szerepköröket tartalmaz a jól ismert azonosítók, ezért lecserélheti a `-RoleMemberObjectId` paraméter `ObjectId` felülről és a következő parancsokat. Az összes címtár szerepkörök, ismételt futtatása listájának megjelenítéséhez `Get-MsolRole`.

```
> Add-MsolRoleMember -RoleObjectId 88d8e3e3-8f55-4a1e-953a-9b9898b8876b -RoleMemberObjectId <Your-ObjectId> -RoleMemberType servicePrincipal
> Add-MsolRoleMember -RoleObjectId 9360feb5-f418-4baa-8175-e2a00bac4301 -RoleMemberObjectId <Your-ObjectId> -RoleMemberType servicePrincipal
> Add-MsolRoleMember -RoleObjectId fe930be7-5e62-47db-91af-98c3a49a38b1 -RoleMemberObjectId <Your-ObjectId> -RoleMemberType servicePrincipal
```

Most már olyan alkalmazás, amely a létrehozás, Olvasás, frissítés engedéllyel rendelkezik, és a B2C bérlői felhasználók törlése.

## <a name="download-configure-and-build-the-sample-code"></a>Töltse le, beállítása és a példakódot összeállítása

Először töltse le a példakódot, és a munkavégzés Bá operációs rendszert futtató. Tart majd azt meg részletesebben.  Azt is megteheti, [Töltse le a példakódot .zip fájlként](https://github.com/AzureADQuickStarts/B2C-GraphAPI-DotNet/archive/master.zip). Is átmásolhatja azt egy tetszés szerinti könyvtárban:

```
git clone https://github.com/AzureADQuickStarts/B2C-GraphAPI-DotNet.git
```

Nyissa meg a `B2CGraphClient\B2CGraphClient.sln` a Visual Studio alkalmazásban a Visual Studio megoldás. Az a `B2CGraphClient` projekt, nyissa meg a fájlt `App.config`. A három alkalmazás beállításainak cserélje ki a kívánt értékét:

```
<appSettings>
    <add key="b2c:Tenant" value="contosob2c.onmicrosoft.com" />
    <add key="b2c:ClientId" value="{The AppPrincipalId from above}" />
    <add key="b2c:ClientSecret" value="{The client secret you generated above}" />
</appSettings>
```

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

Ezután kattintson a jobb gombbal a `B2CGraphClient` megoldást, és a minta újraépítő. Ha a sikeresek, célszerű most már rendelkezik egy `B2C.exe` található végrehajtható fájl `B2CGraphClient\bin\Debug`.

## <a name="build-user-crud-operations-by-using-the-graph-api"></a>Felhasználói CRUD műveletek összeállítása a Graph API segítségével

A B2CGraphClient használja, nyissa meg a `cmd` Windows parancsot a kérdésre, és módosítsa a Directoryval, hogy a `Debug` címtár. Futtassa a `B2C Help` parancsot.

```
> cd B2CGraphClient\bin\Debug
> B2C Help
```

Ekkor megjelenik egy rövid leírást az egyes parancsok. Minden alkalommal, amikor indítható el ezek a parancsok egyikét `B2CGraphClient` az Azure Active Directory Graph API-nak megkeresést.

### <a name="get-an-access-token"></a>Egy hozzáférési jogkivonat beszerzése

A diagram API kérésének egy hozzáférési jogkivonat hitelesítés van szükség. `B2CGraphClient`a hozzáférési jogkivonat szerezheti be a Megnyitás-forrás az Active Directory hitelesítési tár (ADAL) használja. ADAL megkönnyíti a token WIA nyújtó egyszerű API-val és a néhány fontos részletekről olvashat, például a gyorsítótárazás hozzáférési jogkivonat ügyelve. Használata ADAL tokenek, de nincs. HTTP-kérések létrehozásával tokenek is megnyithatja.

> [AZURE.NOTE]
    A kód példa ADAL v2 használja, a diagram API-val kommunikáció érdekében.  Ahhoz, hogy az Azure Active Directory Graph API-val használt hozzáférési jogkivonat ADAL v2 vagy v3 kell használnia.

Ha `B2CGraphClient` fut, ezzel létrehoz egy példányát a `B2CGraphClient` osztály. Az osztály konstruktorának beállítja az ADAL hitelesítési állványon:

```C#
public B2CGraphClient(string clientId, string clientSecret, string tenant)
{
    // The client_id, client_secret, and tenant are provided in Program.cs, which pulls the values from App.config
    this.clientId = clientId;
    this.clientSecret = clientSecret;
    this.tenant = tenant;

    // The AuthenticationContext is ADAL's primary class, in which you indicate the tenant to use.
    this.authContext = new AuthenticationContext("https://login.microsoftonline.com/" + tenant);

    // The ClientCredential is where you pass in your client_id and client_secret, which are
    // provided to Azure AD in order to receive an access_token by using the app's identity.
    this.credential = new ClientCredential(clientId, clientSecret);
}
```

Használjuk a `B2C Get-User` példaként parancsot. Ha `B2C Get-User` anélkül, hogy minden további bemeneti adatok alapján, a CLI hívások meghívott a `B2CGraphClient.GetAllUsers(...)` módot. Ez a módszer felhívja `B2CGraphClient.SendGraphGetRequest(...)`, amely elküld egy HTTP GET kérés, a Graph API-hoz. Mielőtt `B2CGraphClient.SendGraphGetRequest(...)` küld a GET kérés, először kap egy hozzáférési jogkivonat ADAL használatával:

```C#
public async Task<string> SendGraphGetRequest(string api, string query)
{
    // First, use ADAL to acquire a token by using the app's identity (the credential)
    // The first parameter is the resource we want an access_token for; in this case, the Graph API.
    AuthenticationResult result = authContext.AcquireToken("https://graph.windows.net", credential);

    ...

```

Hozzáférhet az jogkivonat az Graph API hívja fel az ADAL `AuthenticationContext.AcquireToken(...)` módot. ADAL adja vissza egy `access_token` , amely az alkalmazás azonosítóját.

### <a name="read-users"></a>Olvassa el a felhasználók

Ha meg szeretné beszerzése a felhasználók listáját, vagy kérjen egy felhasználó a Graph API, elküldheti HTTP `GET` kérése a `/users` végpontot. A felhasználók a bérlő összes kérelmet néz ki:

```
GET https://graph.windows.net/contosob2c.onmicrosoft.com/users?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
```

Ha látni szeretné a kérést, futtatása:

 ```
 > B2C Get-User
 ```

Két fontos dolgot jegyezze fel:

- A via ADAL szerezte be jogkivonat bekerül a `Authorization` élőfej használatával a `Bearer` séma.
- A bérlők B2C, kell használnia a lekérdezési paraméter `api-version=1.6`.

Ezek az adatok mindkét történő a `B2CGraphClient.SendGraphGetRequest(...)` módszer:

```C#
public async Task<string> SendGraphGetRequest(string api, string query)
{
    ...

    // For B2C user management, be sure to use the 1.6 Graph API version.
    HttpClient http = new HttpClient();
    string url = "https://graph.windows.net/" + tenant + api + "?" + "api-version=1.6";
    if (!string.IsNullOrEmpty(query))
    {
        url += "&" + query;
    }

    // Append the access token for the Graph API to the Authorization header of the request by using the Bearer scheme.
    HttpRequestMessage request = new HttpRequestMessage(HttpMethod.Get, url);
    request.Headers.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);
    HttpResponseMessage response = await http.SendAsync(request);

    ...
```

### <a name="create-consumer-user-accounts"></a>Fogyasztói felhasználói fiókok létrehozása

Felhasználói fiókok létrehozásakor a B2C bérlőhöz elküldheti HTTP `POST` kérése a `/users` végpont:

```
POST https://graph.windows.net/contosob2c.onmicrosoft.com/users?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
Content-Type: application/json
Content-Length: 338

{
    // All of these properties are required to create consumer users.

    "accountEnabled": true,
    "signInNames": [                            // controls which identifier the user uses to sign in to the account
        {
            "type": "emailAddress",             // can be 'emailAddress' or 'userName'
            "value": "joeconsumer@gmail.com"
        }
    ],
    "creationType": "LocalAccount",            // always set to 'LocalAccount'
    "displayName": "Joe Consumer",              // a value that can be used for displaying to the end user
    "mailNickname": "joec",                     // an email alias for the user
    "passwordProfile": {
        "password": "P@ssword!",
        "forceChangePasswordNextLogin": false   // always set to false
    },
    "passwordPolicies": "DisablePasswordExpiration"
}
```

A legtöbb e összehívásban tulajdonságokból fogyasztói felhasználók létrehozása szükséges. Ha többet szeretne, kattintson [ide](https://msdn.microsoft.com/library/azure/ad/graph/api/users-operations#CreateLocalAccountUser). Megjegyzendő, hogy a `//` úgy, hogy a megjegyzések szerepelnek az ábrán. Ne kerüljön bele őket a tényleges kérelmet.

Ha látni szeretné a kérést, futtassa az alábbi parancsok egyikére:

```
> B2C Create-User ..\..\..\usertemplate-email.json
> B2C Create-User ..\..\..\usertemplate-username.json
```

A `Create-User` parancs megnyitja a bemeneti paraméterre .json fájlként. Ez a user objektumban JSON képét is tartalmazza. Két .json mintafájlok szerepelnek a példakódot: `usertemplate-email.json` és `usertemplate-username.json`. Ezeket a fájlokat az igényeinek megfelelően módosíthatja. A szükséges mezőket a fenti mellett több választható mezőket, amelyek segítségével használhatja ezeket a fájlokat szerepelnek. Részletes tudnivalókat a választható mezők találhatók az [Azure Active Directory Graph API entitás hivatkozást](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#user-entity).

Megjelenik a bejegyzés kérelem kialakítani hogyan `B2CGraphClient.SendGraphPostRequest(...)`.

- Hozzáférési jogkivonat csatolja a `Authorization` a kérelem fejlécében.
- Állítja a `api-version=1.6`.
- A JSON user objektumban tartalmazza az összehívás törzsében.

> [AZURE.NOTE]
Ha egy meglévő felhasználó áruházból áttelepítendő fiókoknak alsó jelszó erőssége [vannak érvényben az Azure Active Directory B2C erős jelszavak erőssége](https://msdn.microsoft.com/library/azure/jj943764.aspx)-nél, a erős jelszavak követelmény használatával letilthatja a `DisableStrongPassword` az az érték a `passwordPolicies` tulajdonság. Például a Létrehozás felhasználói kérelem feletti az alábbiak szerint szolgáltatni módosíthatók: `"passwordPolicies": "DisablePasswordExpiration, DisableStrongPassword"`.

### <a name="update-consumer-user-accounts"></a>Frissítse a fogyasztói felhasználói fiókok

Ha felhasználói objektumok, akkor a folyamat hasonlít a felhasználó-objektumok létrehozása. Ez a folyamat használja a HTTP, de `PATCH` módszer:

```
PATCH https://graph.windows.net/contosob2c.onmicrosoft.com/users/<user-object-id>?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
Content-Type: application/json
Content-Length: 37

{
    "displayName": "Joe Consumer",              // this request updates only the user's displayName
}
```

Próbálja meg frissíteni a felhasználó a JSON-fájlok frissítésével új adatokkal. Ezután felhasználhatja `B2CGraphClient` futtatható az alábbi parancsok egyikére:

```
> B2C Update-User <user-object-id> ..\..\..\usertemplate-email.json
> B2C Update-User <user-object-id> ..\..\..\usertemplate-username.json
```

Nézze meg a `B2CGraphClient.SendGraphPatchRequest(...)` módszer a kérelem küldése olvashat.

### <a name="search-users"></a>A keresés felhasználói

A B2C bérlői webhelyén, a probléma több módon is kereshet felhasználókat. Egy, a felhasználó használatával az objektum azonosító vagy két, használja a felhasználó bejelentkezési azonosítót (tehát a `signInNames` tulajdonság).

Futtatni szeretne keresni egy adott felhasználói az alábbi parancsok egyikére:

```
> B2C Get-User <user-object-id>
> B2C Get-User <filter-query-expression>
```

Íme néhány példa:

```
> B2C Get-User 2bcf1067-90b6-4253-9991-7f16449c2d91
> B2C Get-User $filter=signInNames/any(x:x/value%20eq%20%27joeconsumer@gmail.com%27)
```

### <a name="delete-users"></a>Felhasználó törlése

Felhasználó törlése a folyamat nem túl bonyolult feladat. Használja a HTTP `DELETE` módszer, és az URL-cím helyes szerkezet objektumazonosító:

```
DELETE https://graph.windows.net/contosob2c.onmicrosoft.com/users/<user-object-id>?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
```

Példa megtekintéséhez adja meg ezt a parancsot, és a törlési kérés, hogy a konzolhoz nyomtatott megtekintése:

```
> B2C Delete-User <object-id-of-user>
```

Nézze meg a `B2CGraphClient.SendGraphDeleteRequest(...)` módszer a kérelem küldése olvashat.

Nemcsak a felhasználók kezelése az Azure Active Directory Graph API-val számos egyéb műveleteket végezhet. Az [Azure Active Directory Graph API hivatkozás](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog) minden művelettel együtt minta kérések részletesen.

## <a name="use-custom-attributes"></a>Egyéni attribútumok használata

A legtöbb fogyasztói alkalmazás kell bizonyos típusú egyéni felhasználói profil adatait tárolja. Ehhez a egyik módja egy egyéni attribútum meghatározása a B2C bérlőhöz. Attribútum, amely bármely más tulajdonság felhasználói objektum kezelik ugyanúgy majd kezeli. Frissítse az attribútum, törölje az attribútum, az attribútum a lekérdezés, a attribútum küldeni a bejelentkezési tokenek és az egyéb igény szerint.

A B2C bérlő egy egyéni attribútum definiálásához [B2C egyéni attribútum hivatkozást](active-directory-b2c-reference-custom-attr.md)talál.

Megtekintheti az egyéni attribútumok definiált a B2C bérlői webhelyén `B2CGraphClient`:

```
> B2C Get-B2C-Application
> B2C Get-Extension-Attribute <object-id-in-the-output-of-the-above-command>
```

A kimenet ezeket a funkciókat például megjelennek az egyes egyéni attribútumok, a részletek:

```JSON
{
      "odata.type": "Microsoft.DirectoryServices.ExtensionProperty",
      "objectType": "ExtensionProperty",
      "objectId": "cec6391b-204d-42fe-8f7c-89c2b1964fca",
      "deletionTimestamp": null,
      "appDisplayName": "",
      "name": "extension_55dc0861f9a44eb999e0a8a872204adb_Jersey_Number",
      "dataType": "Integer",
      "isSyncedFromOnPremises": false,
      "targetObjects": [
        "User"
      ]
}
```

A teljes nevét, például: használható `extension_55dc0861f9a44eb999e0a8a872204adb_Jersey_Number`, a felhasználói objektumok tulajdonság.  Frissítse a .json fájlt az új tulajdonság és a tulajdonság értékét, és futtassa:

```
> B2C Update-User <object-id-of-user> <path-to-json-file>
```

A `B2CGraphClient`, programozás útján is kezelheti a B2C bérlői felhasználók szolgáltatásalkalmazás van. `B2CGraphClient`a saját alkalmazás azonosítója használja az Azure Active Directory Graph API-nak hitelesítést végezni. Egy ügyfél titkos használatával tokenek is megszerzi. Mivel ez a funkció részévé az alkalmazást, ne feledje, hogy néhány főbb pontjaira B2C alkalmazásai:

- Szükséges engedélyeket szeretne adni az alkalmazást a megfelelő bérlőhöz.
- Most frissítenie kell hozzáférési jogkivonat ADAL v2 segítségével. (Is küldhet Protocol (protokoll) üzeneteket, közvetlenül a tár használata nélkül.)
- A diagram API hívásakor használata `api-version=1.6`.
- Amikor létrehozása és frissítése a fogyasztói felhasználók, néhány tulajdonságok szükségesek, fent leírt módon.

Kérdéseit vagy szeretné, hogy a diagram API a B2C bérlői a rendszergazdák által végzett műveletek kérelem, ez a cikk a Kilépés a Megjegyzés vagy probléma a GitHub kód minta tárházba a fájl.
