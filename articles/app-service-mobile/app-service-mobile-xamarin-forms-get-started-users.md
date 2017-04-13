<properties
    pageTitle="Első lépések a hitelesítési Mobile-appokról Xamarin.Forms alkalmazásban |} Microsoft Azure"
    description="Megtudhatja, hogy miként Mobile-alkalmazások használata a Xamarin űrlapok alkalmazását Identitásszolgáltatók, beleértve a AAD, a Google, a Facebook, a Twitteren és a Microsoft számos felhasználói hitelesítést végezni."
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="add-authentication-to-your-xamarinforms-app"></a>Hitelesítés felvétele a Xamarin.Forms alkalmazásba

[AZURE.INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

##<a name="overview"></a>– Áttekintés

Ez a témakör bemutatja, hogyan alkalmazás szolgáltatás Mobile alkalmazás az ügyfélalkalmazás felhasználója hitelesítést végezni. Ebben az oktatóanyagban vesz fel hitelesítési a Xamarin.Forms quickstart útmutató a projekt-szolgáltatási alkalmazás által támogatott identitásszolgáltató használata. Miután sikeresen éppen hitelesített és a Mobile-alkalmazás által engedélyezett, megjelenik a felhasználói azonosító értékét, és elérheti korlátozott Táblázatadatok lesz.

##<a name="prerequisites"></a>Előfeltételek

A legjobb eredmény az ebben az oktatóanyagban javasoljuk, először a [Xamarin.Forms alkalmazás létrehozása](app-service-mobile-xamarin-forms-get-started.md) oktatóprogram elvégzéséhez. Ebben az oktatóanyagban elvégzése után lesz egy Xamarin.Forms projekt alkalmazás a több elem platform listájába.

Ha nem használja a letöltött rövid útmutató az első kiszolgáló projekt, a hitelesítési bővítmény csomag a projekt hozzá kell adnia. További információt a kiszolgáló bővítmény csomagok [a .NET kódmentes server Azure Mobile-appokról SDK használata](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md)című rész tartalmaz.

##<a name="register-your-app-for-authentication-and-configure-app-services"></a>Regisztráljon az alkalmazás hitelesítéshez és alkalmazás szolgáltatások beállítása:

[AZURE.INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

##<a name="restrict-permissions-to-authenticated-users"></a>Hitelesített felhasználói engedélyek korlátozása

[AZURE.INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]


##<a name="add-authentication-to-the-portable-class-library"></a>Hitelesítés hozzáadása a hordozható osztálytár

Mobile-alkalmazások [LoginAsync] bővítmény módszer a [MobileServiceClient] való bejelentkezéshez alkalmazás szolgáltatás hitelesítési rendelkező felhasználó használja. Ez a példa egy kiszolgáló által kezelt hitelesítési folyamat, amely a szolgáltató bejelentkezési felület jeleníti meg az alkalmazást használ. További tudnivalókért lásd: a [hitelesítési kiszolgáló által kezelt](app-service-mobile-dotnet-how-to-use-client-library.md#serverflow). Ahhoz, hogy a hatékonyabb felhasználói élmény a termelési alkalmazásban, akkor fontolja meg inkább a [ügyfél által kezelt hitelesítés](app-service-mobile-dotnet-how-to-use-client-library.md#clientflow)használatával. 

Hitelesítés Xamarin.Forms projekt, a hordozható osztálytár alkalmazás határozhatja meg **IAuthenticate** felületet. Is frissítheti a felhasználói felület hozzáadása egy **bejelentkezési** gombjára, amelynek a felhasználói hitelesítés indítására gombra kattint a hordozható osztálytár definiálva. Adatok sikeres hitelesítést követően be van töltve a mobilalkalmazásban kódmentes.

Az egyes az alkalmazás által támogatott platformok **IAuthenticate** felület be kell állítani.


1. A Visual Studio vagy Xamarin Studio, nyissa meg a **hordozható** a nevében, amely hordozható osztálytár projekt, a projektből App.cs majd adja hozzá a következő `using` utasítás:

        using System.Threading.Tasks;

2. Az App.cs, adja meg a következő `IAuthenticate` előtt közvetlenül a definíció kapcsolat a `App` definition osztály.

        public interface IAuthenticate
        {
            Task<bool> Authenticate();
        }

3. A következő statikus tagok hozzáadása az **alkalmazás** osztály inicializálni platform adott megvalósítása felülettel.

        public static IAuthenticate Authenticator { get; private set; }

        public static void Init(IAuthenticate authenticator)
        {
            Authenticator = authenticator;
        }

4. Nyissa meg a TodoList.xaml a hordozható osztálytár projektből, és a következő **gomb** elem hozzáadása a *buttonsPanel* elrendezés elemben után a meglévő gombra: 

        <Button x:Name="loginButton" Text="Sign-in" MinimumHeightRequest="30" 
            Clicked="loginButton_Clicked"/>

    Ez a gomb a mobilalkalmazásban kódmentes kiszolgáló által kezelt hitelesítéssel elindítja.

5. Nyissa meg a TodoList.xaml.cs a hordozható osztálytár projektből, majd adja hozzá a következő mezőt a `TodoList` osztály:

        // Track whether the user has authenticated. 
        bool authenticated = false;


6. A **OnAppearing** módszer cserélje ki a következő kódot:

        protected override async void OnAppearing()
        {
            base.OnAppearing();
    
            // Refresh items only when authenticated.
            if (authenticated == true)
            {
                // Set syncItems to true in order to synchronize the data 
                // on startup when running in offline mode.
                await RefreshItems(true, syncItems: false);

                // Hide the Sign-in button.
                this.loginButton.IsVisible = false;
            }
        }

    Ezzel biztosíthatja, hogy az adatok csak frissülnek a szolgáltatásból a felhasználó hitelesítése után.

7. A következő kezelő az **Clicked** esemény hozzáadása a **listájába** osztály:

        async void loginButton_Clicked(object sender, EventArgs e)
        {
            if (App.Authenticator != null)
                authenticated = await App.Authenticator.Authenticate();

            // Set syncItems to true to synchronize the data on startup when offline is enabled.
            if (authenticated == true)
                await RefreshItems(true, syncItems: false);
        }

8. A módosítások mentéséhez, és hozza létre újra a hordozható osztálytár projektet hibátlan ellenőrzéséhez.


##<a name="add-authentication-to-the-android-app"></a>Az Android alkalmazást hitelesítési hozzáadása

Ebben a részben a **IAuthenticate** felület megvalósításáról az Android alkalmazás projektben. Ez a szakasz átugrása, ha nem támogatja az Android-eszközökhöz.

1. Visual Studio vagy Xamarin Studio kattintson a jobb gombbal a **droid** projektet, majd az **Indítás projektként beállítása**.

2. Nyomja le az F5 billentyűt a projekt indítása a hibakeresőben, majd ellenőrizze, hogy az alkalmazás indítása után következik (nem engedélyezett) 401 állapotkódot a esetén nem kezelt kivételt. Ennek oka az, hogy az access a kódmentes korlátozódik csak jogosult felhasználók.

3. Nyissa meg a MainActivity.cs az Android projektben, és adja hozzá a következő `using` kimutatások:

        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;

4. Frissítse a **MainActivity** osztály az **IAuthenticate** felület végrehajtásához az alábbi képlettel történik:

        public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsApplicationActivity, IAuthenticate


5. Frissítse a **MainActivity** osztály hozzáadásával **MobileServiceUser** mező és a **hitelesítés** módszer van szüksége a **IAuthenticate** felülettel, az alábbi képlettel történik:

        // Define a authenticated user.
        private MobileServiceUser user;

        public async Task<bool> Authenticate()
        {
            var success = false;
            var message = string.Empty;
            try
            {
                // Sign in with Facebook login using a server-managed flow.
                user = await TodoItemManager.DefaultManager.CurrentClient.LoginAsync(this,
                    MobileServiceAuthenticationProvider.Facebook);
                if (user != null)
                {
                    message = string.Format("you are now signed-in as {0}.",
                        user.UserId);
                    success = true;
                }
            }
            catch (Exception ex)
            {
                message = ex.Message;
            }

            // Display the success or failure message.
            AlertDialog.Builder builder = new AlertDialog.Builder(this);
            builder.SetMessage(message);
            builder.SetTitle("Sign-in result");
            builder.Create().Show();

            return success;
        }


    Ha nem a Facebook-identitásszolgáltató használata esetén választhat egy másik értéket az [MobileServiceAuthenticationProvider].

6. A következő kód hozzáadása a hívás előtt **MainActivity** osztály **OnCreate** metódusát `LoadApplication()`:

        // Initialize the authenticator before loading the app.
        App.Init((IAuthenticate)this);

    Ezzel biztosíthatja, hogy a hitelesítő inicializált-e az alkalmazás terhelések előtt.

7. Az alkalmazás újjáépítése, indítsa el, majd jelentkezzen be a hitelesítési szolgáltató választotta, és győződjön meg róla, zavartalanul hozzáférhetnek az adataikhoz hitelesített felhasználóként.

##<a name="add-authentication-to-the-ios-app"></a>Hitelesítés hozzáadása az iOS-alkalmazás

Ebben a részben a **IAuthenticate** felület megvalósításáról az iOS-alkalmazás projektben. Ez a szakasz átugrása, ha nem támogatja az iOS-eszközök.

1. Visual Studio vagy Xamarin Studio kattintson a jobb gombbal az **iOS** projektet, majd az **Indítás projektként beállítása**.

2. Nyomja le az F5 billentyűt a projekt indítása a hibakeresőben, majd ellenőrizze, hogy az alkalmazás indítása után következik (nem engedélyezett) 401 állapotkódot a esetén nem kezelt kivételt. Ennek oka az, hogy az access a kódmentes korlátozódik csak jogosult felhasználók.

4. Nyissa meg az iOS projekt AppDelegate.cs, és adja hozzá a következő `using` kimutatások:

        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;

4. Frissítse a **AppDelegate** osztály az **IAuthenticate** felület végrehajtásához az alábbi képlettel történik:

        public partial class AppDelegate : global::Xamarin.Forms.Platform.iOS.FormsApplicationDelegate, IAuthenticate

5. Frissítse a **AppDelegate** osztály hozzáadásával **MobileServiceUser** mező és a **hitelesítés** módszer van szüksége a **IAuthenticate** felülettel, az alábbi képlettel történik:

        // Define a authenticated user.
        private MobileServiceUser user;

        public async Task<bool> Authenticate()
        {
            var success = false;
            var message = string.Empty;
            try
            {
                // Sign in with Facebook login using a server-managed flow.
                if (user == null)
                {
                    user = await TodoItemManager.DefaultManager.CurrentClient
                        .LoginAsync(UIApplication.SharedApplication.KeyWindow.RootViewController,
                        MobileServiceAuthenticationProvider.Facebook);
                    if (user != null)
                    {
                        message = string.Format("You are now signed-in as {0}.", user.UserId);
                        success = true;                        
                    }
                }        
            }
            catch (Exception ex)
            {
               message = ex.Message;
            }

            // Display the success or failure message.
            UIAlertView avAlert = new UIAlertView("Sign-in result", message, null, "OK", null);
            avAlert.Show();         

            return success;
        }

    Ha nem a Facebook-identitásszolgáltató használata esetén választhat egy másik értéket az [MobileServiceAuthenticationProvider].

6. A következő sort a kód hozzáadása a **FinishedLaunching** módszer a hívás előtt `LoadApplication()`: 

        App.Init(this);

    Ezzel biztosíthatja, hogy a hitelesítő inicializálni, mielőtt az alkalmazás betöltött.

7. Az alkalmazás újjáépítése, indítsa el, majd jelentkezzen be a hitelesítési szolgáltató választotta, és győződjön meg róla, zavartalanul hozzáférhetnek az adataikhoz hitelesített felhasználóként.


##<a name="add-authentication-to-windows-app-projects"></a>Hitelesítés hozzáadása a Windows-alkalmazás projektekhez

Ebben a részben a **IAuthenticate** felület megvalósításáról a Windows 8.1 és Windows Phone 8.1 alkalmazás projektek. Ezeket a lépéseket a projektekhez univerzális Windows platformon (UWP) vonatkoznak. Ez a szakasz átugrása, ha nem támogatja a Windows-eszközökön.

1. A Visual Studióban kattintson jobb gombbal a **WinApp** vagy a **WinPhone81** projektet, majd az **Indítás projektként beállítása**.

2. Nyomja le az F5 billentyűt a projekt indítása a hibakeresőben, majd ellenőrizze, hogy az alkalmazás indítása után következik (nem engedélyezett) 401 állapotkódot a esetén nem kezelt kivételt. Ennek oka az, hogy az access a kódmentes korlátozódik csak jogosult felhasználók.

3. Nyissa meg a Windows-alkalmazás projekt MainPage.xaml.cs, és adja hozzá a következő `using` kimutatások:

        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;
        using Windows.UI.Popups;
        using <your_Portable_Class_Library_namespace>;

    Csere `<your_Portable_Class_Library_namespace>` a hordozható osztálytár névtere együtt.

4. Frissítse a **MainPage** osztály az **IAuthenticate** felület végrehajtásához az alábbi képlettel történik:

        public sealed partial class MainPage : IAuthenticate


5. Frissítse a **MainPage** osztály hozzáadásával **MobileServiceUser** mező és a **hitelesítés** módszer van szüksége a **IAuthenticate** felülettel, az alábbi képlettel történik:

        // Define a authenticated user.
        private MobileServiceUser user;

        public async Task<bool> Authenticate()
        {
            string message = string.Empty;
            var success = false;

            try
            {
                // Sign in with Facebook login using a server-managed flow.
                if (user == null)
                {
                    user = await TodoItemManager.DefaultManager.CurrentClient
                        .LoginAsync(MobileServiceAuthenticationProvider.Facebook);
                    if (user != null)
                    {
                        success = true;
                        message = string.Format("You are now signed-in as {0}.", user.UserId);
                    }
                }
                
            }
            catch (Exception ex)
            {
                message = string.Format("Authentication Failed: {0}", ex.Message);
            }

            // Display the success or failure message.
            await new MessageDialog(message, "Sign-in result").ShowAsync();

            return success;
        }


    Ha nem a Facebook-identitásszolgáltató használata esetén kiválasztása értékét az [MobileServiceAuthenticationProvider]

6. A következő sort a kód hozzáadása a hívás előtt az **MainPage** osztály konstruktorban `LoadApplication()`:

        // Initialize the authenticator before loading the app.
        <your_Portable_Class_Library_namespace>.App.Init(this);
 
    Csere `<your_Portable_Class_Library_namespace>` a hordozható osztálytár névtere együtt.  
    Ha a WinApp projekt, kihagyhatja le a 8. A következő lépésben csak a WinPhone81 projekt, ahol a bejelentkezési visszahívás befejezéséhez szükséges vonatkozik.

7. (Nem kötelező) A **WinPhone81** alkalmazás projekt, nyissa meg a App.xaml.cs, és adja hozzá a következő `using` kimutatások:

        using Microsoft.WindowsAzure.MobileServices;
        using <your_Portable_Class_Library_namespace>;

    Csere `<your_Portable_Class_Library_namespace>` a hordozható osztálytár névtere együtt.

8.  A következő **OnActivated** többszörösen hozzáadása az **alkalmazás** osztály:

        protected override void OnActivated(IActivatedEventArgs args)
        {
            base.OnActivated(args);

            // We just need to handle activation that occurs after web authentication. 
            if (args.Kind == ActivationKind.WebAuthenticationBrokerContinuation)
            {
                // Get the client and call the LoginComplete method to complete authentication.
                var client = TodoItemManager.DefaultManager.CurrentClient as MobileServiceClient;
                client.LoginComplete(args as WebAuthenticationBrokerContinuationEventArgs);
            }
        }

    Már létezik egy többszörösen, ha csak a feltételes kód hozzáadása a fenti kódtöredékének.

7. Az alkalmazás újjáépítése, indítsa el, majd jelentkezzen be a hitelesítési szolgáltató választotta, és győződjön meg róla, zavartalanul hozzáférhetnek az adataikhoz hitelesített felhasználóként.

##<a name="next-steps"></a>Következő lépések

Alapszintű hitelesítés oktatóprogram kitöltötte, érdemes megfontolni a Folytatás megjelenítheti az alábbi oktatóanyagok egyikét:

+ [Leküldéses értesítések felvétele az alkalmazásba](app-service-mobile-xamarin-forms-get-started-push.md)  
  Megtudhatja, hogy miként vehet fel a leküldéses értesítések támogatása az alkalmazás és a Mobile alkalmazásban kódmentes Azure értesítés hubok segítségével küldje el a leküldéses értesítések konfigurálása.

+ [Az alkalmazás offline szinkronizálás engedélyezése](app-service-mobile-xamarin-forms-get-started-offline-data.md)  
  Megtudhatja, hogy miként hozzáadása az offline munka támogatása az alkalmazás-mobilalkalmazás kódmentes használatával. Kapcsolat nélküli szinkronizálás lehetővé teszi, hogy a végfelhasználók vezérléséhez mobilalkalmazásban&mdash;megtekintése, hozzáadása vagy módosítása, adatok&mdash;akkor is, ha nincs hálózati kapcsolat.

<!-- Images. -->

<!-- URLs. -->
[apns object]: http://go.microsoft.com/fwlink/p/?LinkId=272333
[LoginAsync]: https://msdn.microsoft.com/library/azure/dn268341(v=azure.10).aspx
[MobileServiceClient]: https://msdn.microsoft.com/library/azure/JJ553674(v=azure.10).aspx
[MobileServiceAuthenticationProvider]: https://msdn.microsoft.com/library/azure/jj730936(v=azure.10).aspx

