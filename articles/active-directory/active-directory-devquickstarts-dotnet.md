<properties
    pageTitle="Első lépések Azure AD-.NET |} Microsoft Azure"
    description="A .NET-Windows asztali alkalmazás, amely integrálódik való bejelentkezéshez Azure Active Directory és hívások Azure Active Directory létrehozásának API-k használata OAuth védett."
    services="active-directory"
    documentationCenter=".net"
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="dastrock"/>


# <a name="integrate-azure-ad-into-a-windows-desktop-wpf-app"></a>Azure Active Directory integráció a Windows asztali WPF alkalmazásba

[AZURE.INCLUDE [active-directory-devquickstarts-switcher](../../includes/active-directory-devquickstarts-switcher.md)]

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Ha egy asztali alkalmazás fejleszt, Azure Active Directory teszi egyszerű és a felhasználók az Active Directory-fiókokkal azonosítania kell magát az egyszerű.  Azt is lehetővé teszi, hogy az alkalmazás bármely webes API védi az Azure Active Directory, például az Office 365 API-k és az Azure API biztonságos módon használhatnak.

.NET natív ügyfélprogramokat sorolja fel kell védett forrásokat Azure AD az Active Directory Authentication Library vagy ADAL biztosít.  ADAL-féle egyetlen életben célja megkönnyítik az alkalmazás, hogy a hozzáférési jogkivonat.  Annak bemutatják, hogy milyen egyszerűen egyszerű, itt azt fogja hozza létre a .NET WPF teendőlista alkalmazás, amely:

-   Keresése access-tokenek a hívásához az Azure Active Directory Graph API [OAuth 2.0-s hitelesítési protokoll](https://msdn.microsoft.com/library/azure/dn645545.aspx)használatával.
-   A megadott alias rendelkező felhasználóknak könyvtárában keres.
-   Jelek ki a felhasználókat.

A teljes munkaidő-alkalmazás létrehozására kell:

2. Az alkalmazás regisztrálása Azure AD.
3. Telepítse & konfigurálása ADAL.
5. ADAL segítségével tokenek lekérése az Azure Active Directory.

Az első lépések, [Töltse le az alkalmazás skeleton](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/skeleton.zip) vagy [a kész minta letöltése](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/complete.zip).  Az Azure Active Directory bérlői, ahol felhasználók létrehozása és az alkalmazások regisztráljon is szüksége lesz.  Ha még nem rendelkezik a bérlői, [megtudhatja, hogy miként beszerzéséhez](active-directory-howto-tenant.md).

## <a name="1-register-the-directorysearcher-application"></a>*1. a DirectorySearcher alkalmazás regisztrálása*
Ahhoz, hogy az alkalmazás tokenek megszerezni, először kell regisztrálni a Azure AD-bérlő, és adja meg azt az Azure Active Directory Graph API eléréséhez szükséges engedély:

-   Jelentkezzen be a Azure Kezelőportálja segítségével
-   A bal oldali navigációs sávon kattintson **Az Active** Directory
-   Jelölje ki a bérlői webhelyet, ahol az alkalmazás regisztrálni.
-   Kattintson az **alkalmazások** fülre, és kattintson a **Hozzáadás** gombra az alsó fiókban.
-   Az útmutatást követve hozzon létre egy új **Natív ügyfélalkalmazás**.
    -   Az alkalmazás **neve** leírja a végfelhasználók alkalmazás
    -   Az **Átirányítás Uri** , amelyekkel a Azure AD vissza jogkivonat válaszok az Office-téma és karakterlánc együttes.  Például az alkalmazás, adjon meg egy adott értéket `http://DirectorySearcher`.
-   Regisztráció végrehajtása után, AAD rendel az alkalmazás az ügyfél egyedi azonosítót.  Ez az érték van szüksége a következő szakaszok, így másolja a vágólapra a **beállítás** lapon.
- Is **konfigurálása** lapon keresse meg a "Engedélyeket szeretne egyéb alkalmazások" szakaszt.  A "Azure Active Directory" alkalmazás hozzáadása a **hozzáférést a szervezet címtárára** engedélyt a **Meghatalmazott engedélyeit**.  Ezzel engedélyezi az alkalmazás a Graph API-felhasználóknak lekérdezéséhez.

## <a name="2-install--configure-adal"></a>*2. a telepítés & konfigurálása ADAL*
Most, hogy az alkalmazások az Azure Active Directory, ADAL telepítése, és írja be a kapcsolódó azonosító kódot.  Az Azure Active Directory kommunikálni tudjanak ADAL sorrendben kell, küldje el az alkalmazás regisztrációs információkat.
-   Kezdje el a csomag Manager konzolon DirectorySearcher projekthez ADAL hozzáadásával.

```
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
```

-   Nyissa meg a DirectorySearcher projekt `app.config`.  Az elemek az értékek lecserélése a `<appSettings>` szakasz meg őket a Azure portálra bemeneti értékek megfelelően.  A kód minden alkalommal ADAL hivatkoznak ezeket az értékeket.
    -   A `ida:Tenant` a Azure AD-bérlő, például contoso.onmicrosoft.com tartománya
    -   A `ida:ClientId` van az alkalmazás a portálról másolta a clientId.
    -   A `ida:RedirectUri` van az átirányítás regisztrálta a portál URL-címét.

## <a name="3--use-adal-to-get-tokens-from-aad"></a>*3. Kattintson ADAL úgy juthat az AAD tokenek*
A alapelve ADAL mögött, hogy amikor az alkalmazás szüksége van egy hozzáférési jogkivonat, egyszerűen meghívja `authContext.AcquireTokenAsync(...)`, és az ADAL jelent, a többi.  

-   Az a `DirectorySearcher` megnyitott projekt `MainWindow.xaml.cs` , és keresse meg a `MainWindow()` módot.  Első lépésként az alkalmazás inicializálni `AuthenticationContext` -ADAL adatait a elsődleges osztály.  Most, hogy hol ADAL adják át a koordináták kommunikálni az Azure Active Directory és a gyorsítótár tokenek erről szüksége van.

```C#
public MainWindow()
{
    InitializeComponent();

    authContext = new AuthenticationContext(authority, new FileCache());

    CheckForCachedToken();
}
```

- Keresse meg a `Search(...)` módszer, amikor a felhasználó cliks a "Keresés" gombot az alkalmazás felhasználói felületén hívható.  Ez a módszer Query az Azure Active Directory Graph API-nak GET kérésekben teszi a felhasználóknak, akiknek (UPN) kezdődik a megadott keresési kifejezést.  Annak érdekében, hogy a diagram API lekérdezéséhez is használni kell a egy access_token, de a `Authorization` élőfej a kérelem – Ez az ADAL honnan.

```C#
private async void Search(object sender, RoutedEventArgs e)
{
    // Validate the Input String
    if (string.IsNullOrEmpty(SearchText.Text))
    {
        MessageBox.Show("Please enter a value for the To Do item name");
        return;
    }

    // Get an Access Token for the Graph API
    AuthenticationResult result = null;
    try
    {
        result = await authContext.AcquireTokenAsync(graphResourceId, clientId, redirectUri, new PlatformParameters(PromptBehavior.Auto));
        UserNameLabel.Content = result.UserInfo.DisplayableId;
        SignOutButton.Visibility = Visibility.Visible;
    }
    catch (AdalException ex)
    {
        // An unexpected error occurred, or user canceled the sign in.
        if (ex.ErrorCode != "access_denied")
            MessageBox.Show(ex.Message);

        return;
    }

    ...
}
```
- Amikor az alkalmazás kér hívja fel a jogkivonat `AcquireTokenAsync(...)`, ADAL megpróbálja jogkivonat vissza anélkül, hogy a felhasználó hitelesítő adatait kérő.  ADAL határozza meg, hogy a felhasználónak kell jelentkezzen be az első jogkivonat, ha azt fog egy bejelentkezési párbeszédpanel megjelenítése, a felhasználó hitelesítő adatok összegyűjtése és jogkivonat sikeres hitelesítés esetén vissza.  Ha ADAL kérdezhetők le egy token valamilyen okból, akkor throw egy `AdalException`.
- Figyelje meg, hogy a `AuthenticationResult` objektum tartalmaz egy `UserInfo` használható az alkalmazás szükséges információk összegyűjtése az objektumot.  A DirectorySearcher, a `UserInfo` testre szabhatja az alkalmazás felhasználói felület és a felhasználói azonosító segítségével.

- Amikor a felhasználó a "Kijelentkezés" gombra kattint, a következő hívást ahhoz, hogy szeretnénk `AcquireTokenAsync(...)` felkéri a felhasználót, hogy jelentkezzen be.  Ez az ADAL, az ugyanolyan egyszerű, mint a token gyorsítótár kiürítése:

```C#
private void SignOut(object sender = null, RoutedEventArgs args = null)
{
    // Clear the token cache
    authContext.TokenCache.Clear();

    ...
}
```

- Azonban a felhasználók ne kattintson a "Kijelentkezés" gombra, ha érdemes megőrzéséhez a felhasználó munkamenet a következő alkalommal, amikor a DirectorySearcher futnak.  Amikor elindítja az alkalmazást, jelölje be a token gyorsítótár ADAL meg egy meglévő jogkivonat, és a felhasználói felület árnyalatait.  Az a `CheckForCachedToken()` módszer, egy másik hívás `AcquireTokenAsync(...)`, ez esetben az átadás a `PromptBehavior.Never` paraméter.  `PromptBehavior.Never`ADAL jelzi, hogy a felhasználónak bejelentkezés nem kéri, és az ADAL érdemes inkább kivételhibát kérdezhetők le egy token esetén.

```C#
public async void CheckForCachedToken() 
{
    // As the application starts, try to get an access token without prompting the user.  If one exists, show the user as signed in.
    AuthenticationResult result = null;
    try
    {
        result = await authContext.AcquireTokenAsync(graphResourceId, clientId, redirectUri, new PlatformParameters(PromptBehavior.Never));
    }
    catch (AdalException ex)
    {
        if (ex.ErrorCode != "user_interaction_required")
        {
            // An unexpected error occurred.
            MessageBox.Show(ex.Message);
        }

        // If user interaction is required, proceed to main page without singing the user in.
        return;
    }

    // A valid token is in the cache
    SignOutButton.Visibility = Visibility.Visible;
    UserNameLabel.Content = result.UserInfo.DisplayableId;
}
```

Gratulálok! Most már rendelkezik működő .NET WPF alkalmazás, amely az azt jelenti, hogy a felhasználók hitelesítő, biztonságos hívja a webes API-khoz OAuth 2.0-s verzióját használja, és a felhasználó alapvető adatainak.  Ha még nem tette meg, ezt követően az idő az egyes felhasználók a bérlő kitöltéséhez.  Futtassa a DirectorySearcher alkalmazást, és jelentkezzen be egy azoknak a felhasználóknak.  Más felhasználók UPN név alapján kereshet.  Zárja be az alkalmazást, és futtassa újra.  Figyelje meg, hogy a felhasználó munkamenet sértetlen marad.  Jelentkezzen ki, és jelentkezzen be újra más felhasználó.

ADAL megkönnyíti az alábbi közös identitás funkciók részévé tenni az alkalmazást.  Azt gondoskodik a dirty munkáját vonatkozó - gyorsítótár-kezelés, a OAuth protokoll támogatása, a felhasználó bemutató tartása a bejelentkezési felhasználói felülete frissítése lejárt tokenek, és így tovább.  Valójában kell tudnia, mindössze egyetlen API-hívás `authContext.AcquireTokenAsync(...)`.

A kész minta (nélkül a konfigurációs értékek) megadva hivatkozás, az [alábbi](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/complete.zip).  Most már áthelyezheti további forgatókönyvek.  Előfordulhat, hogy szeretne kipróbálni:

[.NET webes API az Azure Active Directory biztonságos >>](active-directory-devquickstarts-webapi-dotnet.md)

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
