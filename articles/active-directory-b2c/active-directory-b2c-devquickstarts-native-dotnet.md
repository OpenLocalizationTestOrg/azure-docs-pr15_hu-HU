<properties
    pageTitle="Azure Active Directory B2C |} Microsoft Azure"
    description="Windows asztali alkalmazás, amely tartalmazza a bejelentkezés,-előfizetés, létrehozásának és Azure Active Directory B2C használatával profilok kezelése."
    services="active-directory-b2c"
    documentationCenter=".net"
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="07/22/2016"
    ms.author="dastrock"/>

# <a name="azure-ad-b2c-build-a-windows-desktop-app"></a>Azure Active Directory B2C: A Windows asztali alkalmazás összeállítása

Azure Active Directory (Azure Active Directory) B2C használatával hatékony önkiszolgáló identitás-felügyeleti funkciókat is adhat az asztali alkalmazásban néhány rövid lépéseket. Ez a cikk bemutatja, hogyan, amely tartalmazza a felhasználó-előfizetési, a bejelentkezési és profilok kezelése .NET Windows Presentation Foundation (WPF) "Feladatlista" alkalmazás létrehozása céljából. Az alkalmazás támogatni fogja előfizetési és a bejelentkezési felhasználónév vagy e-mailek használatával. Azt is támogatni fogja előfizetési és a bejelentkezési közösségi fiókból, például a Facebook- és a Google használatával.

## <a name="get-an-azure-ad-b2c-directory"></a>Ismerkedés az Azure Active Directory B2C címtár

Azure Active Directory B2C használata előtt hozzon létre egy könyvtárat, vagy az bérlői.  A könyvtár (az összes a felhasználók, alkalmazások, csoportok és az egyéb tároló). Ha egy már nem rendelkezik, a [B2C könyvtár létrehozása](active-directory-b2c-get-started.md) előtt ebből az útmutatóból továbbra is.

## <a name="create-an-application"></a>Alkalmazás létrehozása

Ezután kell B2C címtárában-alkalmazás létrehozása. Ezzel kap, hogy szüksége van az alkalmazás biztonságos kommunikáció Azure AD-információkat. Az alkalmazás létrehozásához kövesse az [alábbi utasításokat](active-directory-b2c-app-registration.md).  Győződjön meg arról, hogy:

- A **natív ügyfele** bele az alkalmazást.
- Másolja a vágólapra az **átirányítás URI** `urn:ietf:wg:oauth:2.0:oob`. Célszerű a kódot a következő példában az alapértelmezett URL-CÍMÉT.
- Másolja az alkalmazás rendelt **Azonosítója** . Később lesz szüksége.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>A házirendek létrehozása

Azure Active Directory B2C minden felhasználói felület [házirend](active-directory-b2c-reference-policies.md)van megadva. A kód minta tartalmaz három identitás szolgáltatások: regisztráció, jelentkezzen be, és a profil szerkesztése elemre. Meg kell minden típushoz házirend létrehozása a [házirend összefoglaló cikkben](active-directory-b2c-reference-policies.md#how-to-create-a-sign-up-policy)ismertetett módon. Amikor a három házirendek hoz létre, ne felejtse el:

- Válassza az identitás-szolgáltatók lap **Előfizetési felhasználói azonosító** és a **regisztrációs e-mailben** .
- Válassza ki a **megjelenítendő név** és a más előfizetési attribútumok a regisztrációs házirend.
- Minden kötvény alkalmazás követelések, válassza a **megjelenítendő név** és a **Objektumazonosító** követelések. Megadhatja, valamint egyéb követelések.
- Létrehozásuk után, másolja a vágólapra a minden szabály **nevét** . Az előtag rendelkeznie kell `b2c_1_`.  A házirend nevek újabb mobiltelefon szükséges.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Miután létrehozta a három házirendek sikeresen, készen áll az alkalmazás összeállítása.

## <a name="download-the-code"></a>Töltse le a kódot.

Ezen oktatóprogram [a GitHub karbantartása](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet)kódját. A minta készítéséhez menet közben is [.zip fájlként skeleton projekt letöltése](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet/archive/skeleton.zip). Átmásolhatja a skeleton is:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet.git
```

A kész alkalmazás [.zip fájlként](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet/archive/complete.zip) is érhető el vagy a `complete` részlege a azonos tárat.

Miután letöltötte a példakódot, nyissa meg a Visual Studio .sln fájlt a kezdéshez. A `TaskClient` projekt a WPF asztali alkalmazás, amely a felhasználó kommunikáljon. Ebben az oktatóanyagban alkalmazásában hívások háttéradatbázist tevékenység webes API-val, az Azure-tároló feladatlista minden felhasználó is.  Nem kell a webes API összeállítása, már meg futni.

Megtudhatja, hogyan webes API biztonságosan hitelesíti kérések Azure Active Directory B2C használatával, kivétele a [webes API első lépések a cikk](active-directory-b2c-devquickstarts-api-dotnet.md).

## <a name="execute-policies"></a>Házirendek végrehajtása
Az alkalmazás kommunikál Azure Active Directory B2C, üzenetküldés hitelesítés, adja meg a házirend szeretnének részeként a HTTP-kérés végrehajtása. .NET asztali alkalmazások OAuth 2.0-s hitelesítési küldésére, hajtsa végre a házirendek az előzetes verzió Microsoft Authentication Library (MSAL) használatával, és elérheti, hogy hívja fel a webes API-khoz tokenek.

### <a name="install-msal"></a>Telepítse a MSAL
MSAL szeretne hozzáadni a `TaskClient` project a Visual Studio csomag Manager konzol használatával.

```
PM> Install-Package Microsoft.Identity.Client -IncludePrerelease
```

### <a name="enter-your-b2c-details"></a>Adja meg a B2C adatait
Nyissa meg a fájlt `Globals.cs` és cserélje le a tulajdonság értékeit saját. Ez az osztály összes használható `TaskClient` gyakran használt hivatkozás értékekre.

```C#
public static class Globals
{
    ...

    // TODO: Replace these five default with your own configuration values
    public static string tenant = "fabrikamb2c.onmicrosoft.com";
    public static string clientId = "90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6";
    public static string signInPolicy = "b2c_1_sign_in";
    public static string signUpPolicy = "b2c_1_sign_up";
    public static string editProfilePolicy = "b2c_1_edit_profile";

    ...
}
```

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]


### <a name="create-the-publicclientapplication"></a>A PublicClientApplication létrehozása
Az elsődleges osztály a MSAL `PublicClientApplication`. Ez az osztály a Azure Active Directory B2C rendszer jelöli meg az alkalmazást. Amikor az alkalmazás initalizes példányának létrehozása `PublicClientApplication` a `MainWindow.xaml.cs`. Ez is felhasználható a ablakot.

```C#
protected async override void OnInitialized(EventArgs e)
{
    base.OnInitialized(e);

    pca = new PublicClientApplication(Globals.clientId)
    {
        // MSAL implements an in-memory cache by default.  Since we want tokens to persist when the user closes the app, 
        // we've extended the MSAL TokenCache and created a simple FileCache in this app.
        UserTokenCache = new FileCache(),
    };
    
    ...
```

### <a name="initiate-a-sign-up-flow"></a>A regisztrációs folyamat indítása
Ha egy felhasználó a jelek be mellett dönt, érdemes a regisztrációs folyamat a létrehozott előfizetési házirend használó kezdeményezhet. MSAL használatával egyszerűen hívja `pca.AcquireTokenAsync(...)`. Adja meg a paraméterek `AcquireTokenAsync(...)` határozza meg, mely jogkivonat jelenik meg, a házirendet, használja a hitelesítési kérést, és így tovább.

```C#
private async void SignUp(object sender, RoutedEventArgs e)
{
    AuthenticationResult result = null;
    try
    {
        // Use the app's clientId here as the scope parameter, indicating that
        // you want a token to the your app's backend web API (represented by
        // the cloud hosted task API).  Use the UiOptions.ForceLogin flag to
        // indicate to MSAL that it should show a sign-up UI no matter what.
        result = await pca.AcquireTokenAsync(new string[] { Globals.clientId },
                string.Empty, UiOptions.ForceLogin, null, null, Globals.authority,
                Globals.signUpPolicy);

        // Upon success, indicate in the app that the user is signed in.
        SignInButton.Visibility = Visibility.Collapsed;
        SignUpButton.Visibility = Visibility.Collapsed;
        EditProfileButton.Visibility = Visibility.Visible;
        SignOutButton.Visibility = Visibility.Visible;

        // When the request completes successfully, you can get user 
        // information from the AuthenticationResult
        UsernameLabel.Content = result.User.Name;

        // After the sign up successfully completes, display the user's To-Do List
        GetTodoList();
    }

    // Handle any exeptions that occurred during execution of the policy.
    catch (MsalException ex)
    {
        if (ex.ErrorCode != "authentication_canceled")
        {
            // An unexpected error occurred.
            string message = ex.Message;
            if (ex.InnerException != null)
            {
                message += "Inner Exception : " + ex.InnerException.Message;
            }

            MessageBox.Show(message);
        }

        return;
    }
}
```

### <a name="initiate-a-sign-in-flow"></a>A bejelentkezési folyamat indítása
A bejelentkezési folyamat, hogy a regisztrációs folyamat kezdeményez ugyanúgy kezdeményezhet. Amikor a felhasználó bejelentkezik, a híváshoz azonos MSAL, hogy ez esetben a bejelentkezési házirend használatával:

```C#
private async void SignIn(object sender = null, RoutedEventArgs args = null)
{
    AuthenticationResult result = null;
    try
    {
        result = await pca.AcquireTokenAsync(new string[] { Globals.clientId },
                    string.Empty, UiOptions.ForceLogin, null, null, Globals.authority,
                    Globals.signInPolicy);
        ...
```

### <a name="initiate-an-edit-profile-flow"></a>A profil szerkesztése folyamat kezdeményezése
Ismét a azonos módon-profil szerkesztése házirend hajthat végre:

```C#
private async void EditProfile(object sender, RoutedEventArgs e)
{
    AuthenticationResult result = null;
    try
    {
        result = await pca.AcquireTokenAsync(new string[] { Globals.clientId },
                    string.Empty, UiOptions.ForceLogin, null, null, Globals.authority,
                    Globals.editProfilePolicy);
```

Az összes ezekben az esetekben MSAL vagy ad vissza egy token `AuthenticationResult` vagy kivételt. Minden alkalommal, amikor egy jogkivonat, letölthető MSAL, használhatja a `AuthenticationResult.User` a felhasználói adatok az alkalmazásban, például a felhasználói felület frissítése objektumot. ADAL is gyorsítótárát a jogkivonat, a többi az alkalmazás részei való használatra.


### <a name="check-for-tokens-on-app-start"></a>Alkalmazás indításkor a tokenek ellenőrzése
MSAL a felhasználó bejelentkezési állapot nyomon követheti a is használhatja.  Ez az alkalmazás a szeretnénk maradjon bejelentkezve, akkor is, miután bezárja az alkalmazást, és nyissa meg újra a felhasználó.  Vissza belül a `OnInitialized` felülbírálása, akkor használja a cég MSAL `AcquireTokenSilent` módszer a ellenőrzéséhez gyorsítótárazott tokenek:

```C#
AuthenticationResult result = null;
try
{
    // If the user has has a token cached with any policy, we'll display them as signed-in.
    TokenCacheItem tci = pca.UserTokenCache.ReadItems(Globals.clientId).Where(i => i.Scope.Contains(Globals.clientId) && !string.IsNullOrEmpty(i.Policy)).FirstOrDefault();
    string existingPolicy = tci == null ? null : tci.Policy;
    result = await pca.AcquireTokenSilentAsync(new string[] { Globals.clientId }, string.Empty, Globals.authority, existingPolicy, false);

    SignInButton.Visibility = Visibility.Collapsed;
    SignUpButton.Visibility = Visibility.Collapsed;
    EditProfileButton.Visibility = Visibility.Visible;
    SignOutButton.Visibility = Visibility.Visible;
    UsernameLabel.Content = result.User.Name;
    GetTodoList();
}
catch (MsalException ex)
{
    if (ex.ErrorCode == "failed_to_acquire_token_silently")
    {
        // There are no tokens in the cache.  Proceed without calling the To Do list service.
    }
    else
    {
        // An unexpected error occurred.
        string message = ex.Message;
        if (ex.InnerException != null)
        {
            message += "Inner Exception : " + ex.InnerException.Message;
        }
        MessageBox.Show(message);
    }
    return;
}
```

## <a name="call-the-task-api"></a>Hívja fel a tevékenység API
Most már használta MSAL végrehajtása házirendek és tokenek.  Hívja fel a tevékenység API tokenek használni szeretné, ha MSAL meg ismét használható `AcquireTokenSilent` módszer a ellenőrzéséhez gyorsítótárazott tokenek:

```C#
private async void GetTodoList()
{
    AuthenticationResult result = null;
    try
    {
        // Here we want to check for a cached token, independent of whatever policy was used to acquire it.
        TokenCacheItem tci = pca.UserTokenCache.ReadItems(Globals.clientId).Where(i => i.Scope.Contains(Globals.clientId) && !string.IsNullOrEmpty(i.Policy)).FirstOrDefault();
        string existingPolicy = tci == null ? null : tci.Policy;

        // Use AcquireTokenSilent to indicate that MSAL should throw an exception if a token cannot be acquired
        result = await pca.AcquireTokenSilentAsync(new string[] { Globals.clientId }, string.Empty, Globals.authority, existingPolicy, false);

    }
    // If a token could not be acquired silently, we'll catch the exception and show the user a message.
    catch (MsalException ex)
    {
        // There is no access token in the cache, so prompt the user to sign-in.
        if (ex.ErrorCode == "failed_to_acquire_token_silently")
        {
            MessageBox.Show("Please sign up or sign in first");
            SignInButton.Visibility = Visibility.Visible;
            SignUpButton.Visibility = Visibility.Visible;
            EditProfileButton.Visibility = Visibility.Collapsed;
            SignOutButton.Visibility = Visibility.Collapsed;
            UsernameLabel.Content = string.Empty;
        }
        else
        {
            // An unexpected error occurred.
            string message = ex.Message;
            if (ex.InnerException != null)
            {
                message += "Inner Exception : " + ex.InnerException.Message;
            }
            MessageBox.Show(message);
        }

        return;
    }
    ...
```

Ha a hívást `AcquireTokenSilentAsync(...)` sikerül és jogkivonat megtalálható a gyorsítótár, felveheti a jogkivonat a `Authorization` élőfej a HTTP-kérés. A tevékenység webes API ezt a fejlécet segítségével az értekezlet-összehívást olvassa el a feladatlistát a felhasználó hitelesítő:

```C#
    ...
    // Once the token has been returned by MSAL, add it to the http authorization header, before making the call to access the To Do list service.
    httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.Token);

    // Call the To Do list service.
    HttpResponseMessage response = await httpClient.GetAsync(Globals.taskServiceUrl + "/api/tasks");
    ...
```

## <a name="sign-the-user-out"></a>Kijelentkezés a felhasználó
Végezetül MSAL is használhatja az alkalmazásban a felhasználó munkamenet befejezése, amikor egy felhasználó kijelöli **Jelentkezzen ki**.  MSAL használata esetén ez végezhető el a tokenek a jogkivonat gyorsítótárból az összes törlése:

```C#
private void SignOut(object sender, RoutedEventArgs e)
{
    // Clear any remnants of the user's session.
    pca.UserTokenCache.Clear(Globals.clientId);

    // This is a helper method that clears browser cookies in the browser control that MSAL uses, it is not part of MSAL.
    ClearCookies();

    // Update the UI to show the user as signed out.
    TaskList.ItemsSource = string.Empty;
    SignInButton.Visibility = Visibility.Visible;
    SignUpButton.Visibility = Visibility.Visible;
    EditProfileButton.Visibility = Visibility.Collapsed;
    SignOutButton.Visibility = Visibility.Collapsed;
    return;
}
```

## <a name="run-the-sample-app"></a>A minta alkalmazás futtatása

Végül össze, és futtassa a minta.  Fizessen elő az alkalmazás használatával egy e-mail címet vagy a felhasználó nevére. Jelentkezzen ki, és jelentkezzen be újra az azonos felhasználóként. A felhasználói profil szerkesztése elemre. Jelentkezzen ki, és jelentkezzen egy másik felhasználó.

## <a name="add-social-idps"></a>Közösségi IDPs hozzáadása

Az alkalmazás jelenleg csak a felhasználó-előfizetési és a bejelentkezési **helyi fiókok**használó. Ezek a fiók B2C címtárában tárolt egy felhasználónevet és jelszót. Azure Active Directory B2C segítségével más identitásszolgáltatók (IDPs) támogatása bármelyik a kód megváltoztatása nélkül is hozzáadhat.

Közösségi IDPs hozzáadása az alkalmazáshoz, kezdje el az alábbi cikkekben részletes utasításait követve. Minden egyes IDP támogatja a kívánt frissítenie kell, hogy a rendszer egy alkalmazás regisztrálása, és egy ügyfél-azonosító beszerzése

- [Facebook-IDP beállítása](active-directory-b2c-setup-fb-app.md)
- [Google-IDP beállítása](active-directory-b2c-setup-goog-app.md)
- [Egy IDP Amazon beállítása](active-directory-b2c-setup-amzn-app.md)
- [LinkedIn-IDP beállítása](active-directory-b2c-setup-li-app.md)

Miután felvette a Identitásszolgáltatók a B2C Directoryhoz, kell szerkesztése a három házirendek, amelyet fel szeretne venni az új IDPs mindegyike a [házirend összefoglaló cikkben](active-directory-b2c-reference-policies.md)leírt módon. Miután mentette a házirendek, futtassa újra az alkalmazást. Meg kell jelennie a bejelentkezési összegként új IDPs, és az egyes az identitás-előfizetési beállítások találkozik.

A házirendek kísérletezhet, és tekintse meg az effektusok a minta alkalmazásba. Hozzáadása vagy eltávolítása IDPs, alkalmazás követelések módosítására és -előfizetési attribútumok módosítása. Kísérletezés, amíg nem láthatja, hogy milyen házirendek, hitelesítési kérések és MSAL kötik össze.

Kész példa [.zip fájlként megadva](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet/archive/complete.zip)hivatkozás. Is átmásolhatja azt a GitHub:

```git clone --branch complete https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet.git```
