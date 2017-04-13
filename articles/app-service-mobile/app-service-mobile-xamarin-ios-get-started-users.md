<properties
    pageTitle="Első lépések a hitelesítés Xamarin iOS Mobile-appokról"
    description="Megtudhatja, hogy miként Mobile-alkalmazások használata a hitelesítést végezni a felhasználók a Xamarin iOS-alkalmazás Identitásszolgáltatók, beleértve a AAD, a Google, a Facebook, a Twitteren és a Microsoft számos keresztül."
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="na"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="add-authentication-to-your-xamarinios-app"></a>Hitelesítés felvétele a Xamarin.iOS alkalmazásba

[AZURE.INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

Ez a témakör bemutatja, hogyan alkalmazás szolgáltatás Mobile alkalmazás az ügyfélalkalmazás felhasználója hitelesítést végezni. Ebben az oktatóanyagban vesz fel hitelesítési a Xamarin.iOS quickstart útmutató a projekt-szolgáltatási alkalmazás által támogatott identitásszolgáltató használata. Miután sikeresen éppen hitelesített és a Mobile-alkalmazás által engedélyezett, megjelenik a felhasználói azonosító értékét, és nem fogja korlátozott Táblázatadatok elérheti.

Az oktatóprogram [Xamarin.iOS alkalmazás létrehozása]először be kell fejezni. Ha nem használja a letöltött rövid útmutató az első kiszolgáló projekt, a hitelesítési bővítmény csomag a projekt hozzá kell adnia. További információt a kiszolgáló bővítmény csomagok [a .NET kódmentes server Azure Mobile-appokról SDK használata](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md)című rész tartalmaz.

##<a name="register-your-app-for-authentication-and-configure-app-services"></a>Regisztráljon az alkalmazás hitelesítéshez és alkalmazás szolgáltatások beállítása:

[AZURE.INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

##<a name="restrict-permissions-to-authenticated-users"></a>Hitelesített felhasználói engedélyek korlátozása

[AZURE.INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

&nbsp;&nbsp;4. a Visual Studio vagy Xamarin Studio futtassa az ügyfél projekt eszközt használ vagy irányító. Győződjön meg arról, hogy az alkalmazás indítása után következik (nem engedélyezett) 401 állapotkódot a esetén nem kezelt kivételt. A hiba be van jelentkezve a debugger konzolon. Ezért a Visual Studióban, meg kell jelennie a hibát, a kimeneti ablakban.

&nbsp;&nbsp;Ezzel az illetéktelen Ez a hiba oka az, hogy az alkalmazás kísérel meg hozzáférni a mobilalkalmazás kódmentes nem hitelesített felhasználóként. A *TodoItem* táblázat most hitelesítést igényel lehetőséget.

Ezután, frissíti az ügyfél alkalmazás kérelem erőforrásokhoz a mobilalkalmazás kódmentes a hitelesített felhasználó.

##<a name="add-authentication-to-the-app"></a>Hitelesítés felvétele az alkalmazásba

Ebben a szakaszban a bejelentkezési képernyő előtt adatok megjelenítése az alkalmazás módosítja. Az alkalmazás indításakor nem nem kapcsolódik az alkalmazás szolgáltatás, és nem jelennek meg az adatokat. Az első alkalommal, amikor a felhasználó elvégez a frissítés kézmozdulat a bejelentkezési képernyője jelenik meg; sikeres bejelentkezés után jelennek meg a teendők elemek listáját.

1. Az ügyfél projektben nyissa meg a fájlt **QSTodoService.cs** , és adja hozzá a következő utasítással és `MobileServiceUser` hozzáférő a QSTodoService osztály a:

    ```
        using UIKit;
    ```

        // Logged in user
        private MobileServiceUser user;
        public MobileServiceUser User { get { return user; } }

2. Adja hozzá a új módszer **hitelesítés** való **QSTodoService** nevű alábbi meghatározása:


        public async Task Authenticate(UIViewController view)
        {
            try
            {
                user = await client.LoginAsync(view, MobileServiceAuthenticationProvider.Facebook);
            }
            catch (Exception ex)
            {
                Console.Error.WriteLine (@"ERROR - AUTHENTICATION FAILED {0}", ex.Message);
            }
        }

    >[AZURE.NOTE] Ha nem a Facebook-identitásszolgáltató használata esetén módosítsa az értéket, **LoginAsync** feletti az alábbiak egyikét a átadott: _Microsoftaccountja_, _Twitter_, _a Google_vagy _WindowsAzureActiveDirectory_.

3. Nyissa meg a **QSTodoListViewController.cs**. Módosítsa a **ViewDidLoad** eltávolítása a hívást **RefreshAsync()** közelében vége módszer meghatározása:

        public override async void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            todoService = QSTodoService.DefaultService;
           await todoService.InitializeStoreAsync ();

           RefreshControl.ValueChanged += async (sender, e) => {
                await RefreshAsync ();
           }

            // Comment out the call to RefreshAsync
            // await RefreshAsync ();
        }


4. Módosítsa a módszerrel **RefreshAsync** hitelesítést végezni, ha a **felhasználó** tulajdonság értéke null. Adja hozzá a következő kódot tetején látható módszer meghatározása:

        // start of RefreshAsync method
        if (todoService.User == null) {
            await QSTodoService.DefaultService.Authenticate (this);
            if (todoService.User == null) {
                Console.WriteLine ("couldn't login!!");
                return;
            }
        }
        // rest of RefreshAsync method

5. A Visual Studio vagy Xamarin Studio csatlakozik a Xamarin összeállítása szolgáltató macen, futtassa az ügyfél projekt eszköz vagy irányító kiválasztásával. Győződjön meg arról, hogy az alkalmazás nem jelenít meg adatokat.

    Hajtsa végre a frissítés kézmozdulat lefelé a listában az elemek, amelyek hatására a bejelentkezési képernyője jelenik meg az adatok alapján. Miután megadta az érvényes hitelesítő adatokkal sikeresen, teendők elemek listáját jeleníti meg az alkalmazást, és az adatokat is frissítenie.


<!-- URLs. -->
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Xamarin.iOS alkalmazás létrehozása]: app-service-mobile-xamarin-ios-get-started.md
