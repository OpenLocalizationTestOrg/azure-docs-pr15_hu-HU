<properties
    pageTitle="Első lépések Azure AD a Windows áruházból |} Microsoft Azure"
    description="Hogyan hozhat létre egy Windows áruház-alkalmazás, amely a együttműködik a bejelentkezés az Azure Active Directory és Azure Active Directory felhívja API-k használata OAuth védett."
    services="active-directory"
    documentationCenter="windows"
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="mobile-windows-store"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="dastrock"/>


# <a name="integrate-azure-ad-with-a-windows-store-app"></a>Azure Active Directory integráció a Windows áruház-alkalmazás

[AZURE.INCLUDE [active-directory-devquickstarts-switcher](../../includes/active-directory-devquickstarts-switcher.md)]

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Ha a Windows áruház-alkalmazás fejleszt, Azure AD teszi egyszerű és egyszerű, az Active Directory-fiókokat azok a felhasználók azonosítania kell magát.  Azt is lehetővé teszi, hogy az alkalmazás bármely webes API védi az Azure Active Directory, például az Office 365 API-k és az Azure API biztonságos módon használhatnak.

A Windows áruházból asztali alkalmazások kell a védett forrásokat Azure AD az Active Directory Authentication Library vagy ADAL biztosít.  ADAL-féle kizárólagos életben célja, hogy az alkalmazás, hogy a hozzáférési jogkivonat könnyen.  Annak bemutatják, hogy milyen egyszerűen egyszerű, itt azt fogja hozza létre a "Címtár elutasíthat" Windows áruház-alkalmazás, amely:

-   Keresése access-tokenek a hívásához az Azure Active Directory Graph API [OAuth 2.0-s hitelesítési protokoll](https://msdn.microsoft.com/library/azure/dn645545.aspx)használatával.
-   Egy adott UPN rendelkező felhasználóknak könyvtárában keres.
-   Jelek ki a felhasználókat.

A teljes munkaidő-alkalmazás létrehozására kell:

2. Az alkalmazás regisztrálása Azure AD.
3. Telepítse & konfigurálása ADAL.
5. ADAL segítségével tokenek lekérése az Azure Active Directory.

Az első lépések, [skeleton projekt letölthetők](https://github.com/AzureADQuickStarts/NativeClient-WindowsStore/archive/skeleton.zip) , és [Töltse le a bejegyzett minta](https://github.com/AzureADQuickStarts/NativeClient-WindowsStore/archive/complete.zip).  Minden egyes a Visual Studio 2015 megoldást.  Az Azure Active Directory bérlői, ahol felhasználók létrehozása és az alkalmazások regisztráljon is szüksége lesz.  Ha még nem rendelkezik a bérlői, [megtudhatja, hogy miként beszerzéséhez](active-directory-howto-tenant.md).

## <a name="1-register-the-directory-searcher-application"></a>*1. a címtár-elutasíthat alkalmazás regisztrálása*
Ahhoz, hogy az alkalmazás tokenek megszerezni, először kell regisztrálni a Azure AD-bérlő, és adja meg azt az Azure Active Directory Graph API eléréséhez szükséges engedély:

-   Jelentkezzen be az [Azure Kezelőportálja segítségével](https://manage.windowsazure.com)
-   A bal oldali navigációs sávon kattintson **Az Active** Directory
-   Jelölje ki a bérlői webhelyet, ahol az alkalmazás regisztrálni.
-   Kattintson az **alkalmazások** fülre, és kattintson a **Hozzáadás** gombra az alsó fiókban.
-   Az útmutatást követve hozzon létre egy új **Natív ügyfélalkalmazás**.
    -   Az alkalmazás **neve** leírja a végfelhasználók alkalmazás
    -   Az **Átirányítás Uri** , amelyekkel a Azure AD vissza jogkivonat válaszok az Office-téma és karakterlánc együttes.  Adjon meg egy helyőrző értéket a fontos, például `http://DirectorySearcher`.  Később fogja helyére ezt az értéket.
-   Ha befejezte a regisztrációs, AAD rendel az alkalmazás egy egyedi ügyfél-azonosító.  Ez az érték van szüksége a következő szakaszok, így másolja a vágólapra a **beállítás** lapon.
- Is **konfigurálása** lapon keresse meg a "Engedélyeket szeretne egyéb alkalmazások" szakaszt.  A "Azure Active Directory" alkalmazás hozzáadása a **hozzáférést a bejelentkezett felhasználó a címtárban** engedélyt a **Meghatalmazott engedélyeit**.  Ezzel engedélyezi az alkalmazás a Graph API-felhasználóknak lekérdezéséhez.

## <a name="2-install--configure-adal"></a>*2. a telepítés & konfigurálása ADAL*
Most, hogy az alkalmazások az Azure Active Directory, ADAL telepítése, és írja be a kapcsolódó azonosító kódot.  Az Azure Active Directory kommunikálni tudjanak ADAL sorrendben kell, küldje el az alkalmazás regisztrációs információkat.
-   Kezdje el a csomag Manager konzolon DirectorySearcher projekthez ADAL hozzáadásával.

```
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
```

-   Nyissa meg a DirectorySearcher projekt `MainPage.xaml.cs`.  Az értékek lecserélése a `Config Values` régió az Azure-portálra a bevitt értékeket megfelelően.  A kód minden alkalommal ADAL hivatkoznak ezeket az értékeket.
    -   A `tenant` a Azure AD-bérlő, például contoso.onmicrosoft.com tartománya
    -   A `clientId` van az alkalmazás a portálról másolta a clientId.
-   Most már kell a Windows áruházból letölthető a visszahívási uri felfedezése.  Töréspont beállítása az ebben a sorban a `MainPage` módszer:

```
redirectURI = Windows.Security.Authentication.Web.WebAuthenticationBroker.GetCurrentApplicationCallbackUri();
```
- Gondoskodhat arról, hogy az összes csomag hivatkozások szolgáltatáscsomagot a megoldás létrehozható.  Csomagok fognak hiányozni, ha nyissa meg a Nuget csomag kezelő felfelé, és a csomagok visszaállítása.
- Futtassa az alkalmazást, és másolja a következő értékének `redirectUri` esetén a töréspont megérintve-e.  Következőhöz hasonlóan kell kinéznie

```
ms-app://s-1-15-2-1352796503-54529114-405753024-3540103335-3203256200-511895534-1429095407/
```

- Vissza a **beállítás** lapon az alkalmazás az adatkezelési portálon Azure ezt az értéket a **RedirectUri** értékének cserélje.  

## <a name="3--use-adal-to-get-tokens-from-aad"></a>*3. Kattintson ADAL úgy juthat az AAD tokenek*
A alapelve ADAL mögött, hogy amikor az alkalmazás szüksége van egy hozzáférési jogkivonat, egyszerűen meghívja `authContext.AcquireToken(…)`, és az ADAL jelent, a többi.  

-   Első lépésként az alkalmazás inicializálni `AuthenticationContext` -ADAL adatait a elsődleges osztály.  Most, hogy hol ADAL adják át a koordináták kommunikálni az Azure Active Directory és a gyorsítótár tokenek erről szüksége van.

```C#
public MainPage()
{
    ...

    authContext = new AuthenticationContext(authority);
}
```

- Keresse meg a `Search(...)` módszer, hívható, amikor a felhasználó az alkalmazás felhasználói felületén a "Keresés" gombra kattint.  Ez a módszer Query az Azure Active Directory Graph API-nak GET kérésekben teszi a felhasználóknak, akiknek (UPN) kezdődik a megadott keresési kifejezést.  Annak érdekében, hogy a diagram API lekérdezéséhez is használni kell a egy access_token, de a `Authorization` élőfej a kérelem – Ez az ADAL honnan.

```C#
private async void Search(object sender, RoutedEventArgs e)
{
    ...
    AuthenticationResult result = null;
    try
    {
        result = await authContext.AcquireTokenAsync(graphResourceId, clientId, redirectURI, new PlatformParameters(PromptBehavior.Auto, false));
    }
    catch (AdalException ex)
    {
        if (ex.ErrorCode != "authentication_canceled")
        {
            ShowAuthError(string.Format("If the error continues, please contact your administrator.\n\nError: {0}\n\nError Description:\n\n{1}", ex.ErrorCode, ex.Message));
        }
        return;
    }
    ...
}
```
- Amikor az alkalmazás kér hívja fel a jogkivonat `AcquireTokenAsync(...)`, ADAL megpróbálja jogkivonat vissza anélkül, hogy a felhasználó hitelesítő adatait kérő.  ADAL határozza meg, hogy a felhasználónak kell jelentkezzen be az első jogkivonat, ha azt fog egy bejelentkezési párbeszédpanel megjelenítése, a felhasználó hitelesítő adatok összegyűjtése és jogkivonat sikeres hitelesítés esetén vissza.  Ha a ADAL kérdezhetők le egy token valamilyen okból a `AuthenticationResult` állapota hiba lesz.

- Most pedig használja az imént szerezte be a access_token.  A is a `Search(...)` módszer, a token csatolhat engedélyezési fejlécében Graph API GET kérés:

```C#
// Add the access token to the Authorization Header of the call to the Graph API, and call the Graph API.
httpClient.DefaultRequestHeaders.Authorization = new HttpCredentialsHeaderValue("Bearer", result.AccessToken);

```
- Is használhatja a `AuthenticationResult` objektum a felhasználó adatai jelennek meg az alkalmazást, például a felhasználói azonosítójával:

```C#
// Update the Page UI to represent the signed in user
ActiveUser.Text = result.UserInfo.DisplayableId;
```
- Végezetül ADAL segítségével jelentkezzen ki az alkalmazásból, valamint a felhasználó.  Amikor a felhasználó a "Kijelentkezés" gombra kattint, a következő hívást ahhoz, hogy szeretnénk `AcquireTokenAsync(...)` nézetben jelenít meg.  Ez az ADAL, az ugyanolyan egyszerű, mint a token gyorsítótár kiürítése:

```C#
private void SignOut()
{
    // Clear session state from the token cache.
    authContext.TokenCache.Clear();

    ...
}
```

Gratulálok! Most már rendelkezik, amelynek az azt jelenti, hogy a felhasználók hitelesítő, biztonságos hívja a webes API-khoz OAuth 2.0 használata Windows áruház-alkalmazás működő, és a felhasználó alapvető adatainak.  Ha még nem tette meg, ezt követően az idő az egyes felhasználók a bérlő kitöltéséhez.  Futtassa a DirectorySearcher alkalmazást, és jelentkezzen be egy azoknak a felhasználóknak.  Más felhasználók UPN név alapján kereshet.  Zárja be az alkalmazást, és futtassa újra.  Figyelje meg, hogy a felhasználó munkamenet sértetlen marad.  Jelentkezzen ki (a jobb gombbal kattintva jelenítse meg az alsó sávján) kattintva, és jelentkezzen be újra más felhasználó.

ADAL megkönnyíti az alábbi közös identitás funkciók részévé tenni az alkalmazást.  Azt gondoskodik a dirty munkáját vonatkozó - gyorsítótár-kezelés, a OAuth protokoll támogatása, a felhasználó bemutató tartása a bejelentkezési felhasználói felülete frissítése lejárt tokenek, és így tovább.  Valójában kell tudnia, mindössze egyetlen API-hívás `authContext.AcquireToken*(…)`.

A kész minta (nélkül a konfigurációs értékek) megadva hivatkozás, az [alábbi](https://github.com/AzureADQuickStarts/NativeClient-WindowsStore/archive/complete.zip).  Most már áthelyezheti további identitás esetek.  Előfordulhat, hogy szeretne kipróbálni:

[.NET webes API az Azure Active Directory biztonságos >>](active-directory-devquickstarts-webapi-dotnet.md)

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
