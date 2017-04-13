<properties
pageTitle="Azure Active Directory 2.0-s verzió .NET natív alkalmazás |} Microsoft Azure"
description="Hogyan hozhat létre, amely a felhasználók be mindkét személyes Microsoft-Account bejelentkezik .NET natív alkalmazás és a munkahelyi vagy iskolai fiókokat."
services="active-directory"
documentationCenter=""
authors="dstrockis"
manager="mbaldwin"
editor=""/>

<tags
ms.service="active-directory"
ms.workload="identity"
ms.tgt_pltfrm="na"
ms.devlang="dotnet"
ms.topic="article"
ms.date="07/30/2016"
ms.author="dastrock; vittorib"/>

# <a name="add-sign-in-to-a-windows-desktop-app"></a>A Windows asztali alkalmazás bejelentkezési hozzáadása

Az a a 2.0-s verzió végpont gyorsan adhat hozzá hitelesítés támogatása mindkét személyes Microsoft-fiókok esetén az asztali alkalmazásait és a munkahelyi vagy iskolai fiókokat.  Is lehetővé teszi az alkalmazás biztonságos kommunikáció kódmentes webes API-val, valamint [A Microsoft Graph](https://graph.microsoft.io) és az [Office 365 egyesített API -khoz](https://www.msdn.com/office/office365/howto/authenticate-Office-365-APIs-using-v2)néhány.

> [AZURE.NOTE] Nem minden Azure Active Directory (AD) esetek és szolgáltatások 2.0-s verzió végpont által támogatott.  Annak megállapításához, ha a 2.0-s verzió végpont kell használnia, [2.0-s verzió korlátozások](active-directory-v2-limitations.md)olvashat.

[.NET natív alkalmazások eszközön futó](active-directory-v2-flows.md#mobile-and-native-apps)Azure AD a Microsoft identitás Authentication Library vagy MSAL biztosít.  MSAL meg egyetlen életben célja megkönnyítik az alkalmazás tokenek beszerzése webszolgáltatásokhoz hívásához.  Annak bemutatják, hogy milyen egyszerűen egyszerű, itt azt fogja hozza létre a .NET WPF feladatlista-at, amely:

- A felhasználó bejelentkezik és keresése az [OAuth 2.0-s hitelesítési protokoll](active-directory-v2-protocols.md#oauth2-authorization-code-flow)használatával tokenek eléréséhez.
- Egy feladatlista webes szolgáltatása, amelynek OAuth 2.0-s verziója is védett kódmentes biztonságosan hívások.
- A felhasználó bejelentkezik.

## <a name="download-sample-code"></a>Példakódot letöltése

Ebben az oktatóanyagban kódját [a GitHub](https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet)karbantartása.  Követésre, [Töltse le az alkalmazás skeleton, mint egy .zip](https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet/archive/skeleton.zip) is vagy a skeleton klónozhatja:

    git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet.git

A kész alkalmazást is ez az oktatóanyag végén megadva.

## <a name="register-an-app"></a>Regisztráljon az alkalmazás
Új alkalmazás létrehozása a [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), vagy kövesse az alábbi [lépések részletes leírását](active-directory-v2-app-registration.md).  Győződjön meg arról, hogy:

- Másolás lefelé **Azonosítója** rendelt az alkalmazást, szüksége van rá hamarosan.
- A **mobil** platform az alkalmazás hozzáadása

## <a name="install--configure-msal"></a>Telepítse és állítsa be a MSAL
Most, hogy regisztrált Microsoft alkalmazás, MSAL telepítése, és írja be a kapcsolódó azonosító kódot.  Ahhoz, hogy engedélyezni szeretné a 2.0-s verzió végpont kommunikáció MSAL meg kell adnia azt néhány információkkal, az alkalmazás regisztrációs.

-   Kezdje el a csomag Manager konzolon TodoListClient projekthez MSAL hozzáadásával.

```
PM> Install-Package Microsoft.Identity.Client -ProjectName TodoListClient -IncludePrerelease
```

-   Nyissa meg a TodoListClient projekt `app.config`.  Az elemek az értékek lecserélése a `<appSettings>` szakasz meg őket az alkalmazás regisztrációs portálra bemeneti értékek megfelelően.  A kód minden alkalommal MSAL hivatkoznak ezeket az értékeket.
    -   A `ida:ClientId` másolta a portálról alkalmazás **Azonosítója** .

- Nyissa meg a listájába szolgáltatási projekt `web.config` a legfelső szintű a projekt.  
    - Cserélje le a `ida:Audience` a portálról **Azonosítója** egy értéket.

## <a name="use-msal-to-get-tokens"></a>MSAL tokenek használata
A alapelve MSAL mögött, hogy amikor az alkalmazás szüksége van egy hozzáférési jogkivonat, egyszerűen hívja `app.AcquireToken(...)`, és a többi MSAL.  

-   Az a `TodoListClient` megnyitott projekt `MainWindow.xaml.cs` , és keresse meg a `OnInitialized(...)` módot.  Első lépésként az alkalmazás inicializálni `PublicClientApplication` -MSAL meg, amely natív alkalmazások elsődleges osztály.  Ez a hol a sikeres MSAL a koordináták kommunikálni az Azure Active Directory és a gyorsítótár-tokenek erről szüksége van.

```C#
protected override async void OnInitialized(EventArgs e)
{
        base.OnInitialized(e);

        app = new PublicClientApplication(new FileCache());
        AuthenticationResult result = null;
        ...
}
```

- Az alkalmazás indításakor szeretnénk ellenőrizze, és ha a felhasználó már bejelentkezett az alkalmazást.  Azonban azt nem szeretné, hogy csak a még meghívása bejelentkezési felhasználói Felületet – a felhasználó "kattintson a Bejelentkezés gombra" ezt fogja VÁLLALUNK.  A is a `OnInitialized(...)` módszer:

```C#
// As the app starts, we want to check to see if the user is already signed in.
// You can do so by trying to get a token from MSAL, using the method
// AcquireTokenSilent.  This forces MSAL to throw an exception if it cannot
// get a token for the user without showing a UI.
try
{
    result = await app.AcquireTokenSilentAsync(new string[] { clientId });
    // If we got here, a valid token is in the cache - or MSAL was able to get a new oen via refresh token.
    // Proceed to fetch the user's tasks from the TodoListService via the GetTodoList() method.
    
    SignInButton.Content = "Clear Cache";
    GetTodoList();
}
catch (MsalException ex)
{
    if (ex.ErrorCode == "user_interaction_required")
    {
        // If user interaction is required, the app should take no action,
        // and simply show the user the sign in button.
    }
    else
    {
        // Here, we catch all other MsalExceptions
        string message = ex.Message;
        if (ex.InnerException != null)
        {
            message += "Inner Exception : " + ex.InnerException.Message;
        }
        MessageBox.Show(message);
    }
}

```

- Ha a felhasználó nincs bejelentkezve, és a "Bejelentkezés" gombra kattintanak, szeretnénk meghívása egy bejelentkezési felhasználói felület és a felhasználót, adja meg a hitelesítő adatait.  A bejelentkezési gomb kezelő végrehajtása:

```C#
private async void SignIn(object sender = null, RoutedEventArgs args = null)
{
        // TODO: Sign the user out if they clicked the "Clear Cache" button

// If the user clicked the 'Sign-In' button, force
// MSAL to prompt the user for credentials by using
// AcquireTokenAsync, a method that is guaranteed to show a prompt to the user.
// MSAL will get a token for the TodoListService and cache it for you.

AuthenticationResult result = null;
try
{
    result = await app.AcquireTokenAsync(new string[] { clientId });
    SignInButton.Content = "Clear Cache";
    GetTodoList();
}
catch (MsalException ex)
{
    // If MSAL cannot get a token, it will throw an exception.
    // If the user canceled the login, it will result in the
    // error code 'authentication_canceled'.

    if (ex.ErrorCode == "authentication_canceled")
    {
        MessageBox.Show("Sign in was canceled by the user");
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


}
```

- Ha a felhasználó sikeresen jelei beépülő modul, MSAL fogadja és gyorsítótár jogkivonat Önnél, és folytassa hívja fel a `GetTodoList()` biztonságos módszer.  Lépjen egy felhasználó feladatai hagyott mindössze végrehajtásához a `GetTodoList()` módot.

```C#
private async void GetTodoList()
{

AuthenticationResult result = null;
try
{
    // Here, we try to get an access token to call the TodoListService
    // without invoking any UI prompt.  AcquireTokenSilentAsync forces
    // MSAL to throw an exception if it cannot get a token silently.


    result = await app.AcquireTokenSilentAsync(new string[] { clientId });
}
catch (MsalException ex)
{
    // MSAL couldn't get a token silently, so show the user a message
    // and let them click the Sign-In button.

    if (ex.ErrorCode == "user_interaction_required")
    {
        MessageBox.Show("Please sign in first");
        SignInButton.Content = "Sign In";
    }
    else
    {
        // In any other case, an unexpected error occurred.

        string message = ex.Message;
        if (ex.InnerException != null)
        {
            message += "Inner Exception : " + ex.InnerException.Message;
        }
        MessageBox.Show(message);
    }

    return;
}

// Once the token has been returned by MSAL,
// add it to the http authorization header,
// before making the call to access the To Do list service.

httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.Token);


        ...
...


- When the user is done managing their To-Do List, they may finally sign out of the app by clicking the "Clear Cache" button.

```C#
private async void SignIn(object sender = null, RoutedEventArgs args = null)
{
        // If the user clicked the 'clear cache' button,
        // clear the MSAL token cache and show the user as signed out.
        // It's also necessary to clear the cookies from the browser
        // control so the next user has a chance to sign in.

        if (SignInButton.Content.ToString() == "Clear Cache")
        {
                TodoList.ItemsSource = string.Empty;
                app.UserTokenCache.Clear(app.ClientId);
                ClearCookies();
                SignInButton.Content = "Sign In";
                return;
        }

        ...
```

## <a name="run"></a>Futtatása

Gratulálok! Ekkor a munka .NET WPF alkalmazás, amely tartalmazza az azt jelenti, hogy a felhasználók hitelesítő és biztonságos módon hívja fel a webes API-khoz OAuth 2.0-s verzióját használja.  Futtassa a mindkét projektet, és jelentkezzen be egy személyes Microsoft-fiókkal vagy egy munkahelyi vagy iskolai fiókjával.  Tevékenységek hozzáadása az adott felhasználó feladatlista.  Jelentkezzen ki, és jelentkezzen be újra a másik felhasználó megtekintheti a feladatlista.  Zárja be az alkalmazást, és futtassa újra.  Figyelje meg, hogy a felhasználó munkamenet sértetlen marad – ennek az oka az alkalmazás gyorsítótárát helyi fájl a tokenek.

MSAL egyszerűen való közös identitás szolgáltatások részévé tenni az alkalmazást, személyes és a munkahelyi fiók használatával.  Azt gondoskodik a dirty munkáját vonatkozó - gyorsítótár-kezelés, a OAuth protokoll támogatása, a felhasználó bemutató tartása a bejelentkezési felhasználói felülete frissítése lejárt tokenek, és így tovább.  Valójában kell tudnia, mindössze egyetlen API-hívás `app.AcquireTokenAsync(...)`.

Hivatkozások [formájában érhető el az alábbi egy .zip](https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet/archive/complete.zip)kész példa (nélkül a konfigurációs értékek), illetve klónozhatja, a GitHub:

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet.git```

## <a name="next-steps"></a>Következő lépések

Most már áthelyezheti Speciális témakörök alakzatot.  Előfordulhat, hogy szeretne kipróbálni:

- [A TodoListService webes API-t a 2.0-s verzió végponttal biztonságossá tétele](active-directory-v2-devquickstarts-dotnet-api.md)

További források olvassa el:  

- [A 2.0-s verzió Fejlesztőeszközök útmutató >>](active-directory-appmodel-v2-overview.md)
- [StackOverflow "msal" címke >>](http://stackoverflow.com/questions/tagged/msal)

## <a name="get-security-updates-for-our-products"></a>Biztonsági frissítések beszerzése termékei

Javasoljuk, hogy mikor védelmi események előfordulásainak megtalálhatók az [erre a lapra](https://technet.microsoft.com/security/dd252948) , és a feliratkozás biztonsági figyelmeztető riasztás, értesítést kap.
