<properties
    pageTitle="Első lépések az Android-alapú Xamarin Mobile-appokról hitelesítési"
    description="Megtudhatja, hogy miként Mobile-alkalmazások használata a Xamarin Android-alapú Identitásszolgáltatók, beleértve a AAD, a Google, a Facebook, a Twitteren és a Microsoft számos alkalmazását felhasználói hitelesítést végezni."
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-android"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="add-authentication-to-your-xamarinandroid-app"></a>Hitelesítés felvétele a Xamarin.Android alkalmazásba

[AZURE.INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

Ez a témakör bemutatja, hogyan egy mobil alkalmazás az ügyfélalkalmazás felhasználója hitelesítést végezni. Ebben az oktatóanyagban ad hozzá hitelesítési a quickstart útmutató projekthez az Azure Mobile-alkalmazások által támogatott identitásszolgáltató használata. Miután sikeresen alatt hitelesített és engedélyezett a Mobile alkalmazásban, a felhasználói azonosító értékét jelenik meg.

Ebben az oktatóanyagban a mobilalkalmazás quickstart útmutató alapul. Először is végre kell hajtania az oktatóprogram [Xamarin.Android alkalmazás létrehozása]. Ha nem használja a letöltött rövid útmutató az első kiszolgáló projekt, a hitelesítési bővítmény csomag a projekt hozzá kell adnia. További információt a kiszolgáló kiterjesztés csomagok [a .NET kódmentes server Azure Mobile-appokról SDK használata](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md)című rész tartalmaz.

##<a name="register"></a>Regisztráljon az alkalmazás hitelesítéshez és alkalmazás szolgáltatások beállítása:

[AZURE.INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

##<a name="permissions"></a>Hitelesített felhasználói engedélyek korlátozása

[AZURE.INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

Visual Studio vagy Xamarin Studio futtassa az ügyfél projekt eszközt használ vagy irányító. Győződjön meg arról, hogy az alkalmazás indítása után következik (nem engedélyezett) 401 állapotkódot a esetén nem kezelt kivételt. Ennek oka az, hogy az alkalmazás kísérel meg hozzáférni a mobilalkalmazás kódmentes nem hitelesített felhasználóként. A *TodoItem* táblázat most hitelesítést igényel lehetőséget.

Ezután, frissíti az ügyfél alkalmazás kérelem erőforrásokhoz a mobilalkalmazás kódmentes a hitelesített felhasználó.

##<a name="add-authentication"></a>Hitelesítés felvétele az alkalmazásba

Az alkalmazás frissül, koppintson a **Bejelentkezés** gombra, és a hitelesítési előtt adatok jelennek meg a felhasználóknak.

1. A következő kód hozzáadása a **TodoActivity** osztály:

        // Define a authenticated user.
        private MobileServiceUser user;
        private async Task<bool> Authenticate()
        {
                var success = false;
                try
                {
                    // Sign in with Facebook login using a server-managed flow.
                    user = await client.LoginAsync(this,
                        MobileServiceAuthenticationProvider.Facebook);
                    CreateAndShowDialog(string.Format("you are now logged in - {0}",
                        user.UserId), "Logged in!");

                    success = true;
                }
                catch (Exception ex)
                {
                    CreateAndShowDialog(ex, "Authentication failed");
                }
                return success;
        }

        [Java.Interop.Export()]
        public async void LoginUser(View view)
        {
            // Load data only after authentication succeeds.
            if (await Authenticate())
            {
                //Hide the button after authentication succeeds.
                FindViewById<Button>(Resource.Id.buttonLoginUser).Visibility = ViewStates.Gone;

                // Load the data.
                OnRefreshItemsSelected();
            }
        }

    Ezzel létrehoz egy új módszer a felhasználónak, és **Jelentkezzen be az** új gomb módszer leíró hitelesítést végezni. A felhasználót a fenti példa kód hitelesíteni a Facebook-bejelentkezés használatáról. A párbeszédpanel a felhasználói azonosító, amint megtörténik a hitelesítése megjelenítésére szolgál.

    > [AZURE.NOTE] Ha eltérő Facebook-identitásszolgáltató használata esetén módosítsa az értéket, átadott **LoginAsync** feletti az alábbiak egyikére: _Microsoftaccountja_, _Twitter_, _a Google_vagy _WindowsAzureActiveDirectory_.

3. A **OnCreate** módszer törlése vagy a következő sort a kód Megjegyzés meg:

        OnRefreshItemsSelected ();

4. A Activity_To_Do.axml fájlt az adja meg a következő *LoginUser* gomb definícióját, mielőtt a meglévő *AddItem* gombra:

        <Button
            android:id="@+id/buttonLoginUser"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:onClick="LoginUser"
            android:text="@string/login_button_text" />

5. A következő elem hozzáadása a Strings.xml erőforrások fájlt:

        <string name="login_button_text">Sign in</string>

6. A Visual Studio vagy Xamarin Studio az ügyfél projekt futtatása irányító vagy eszközön, és jelentkezzen be a választott identitásszolgáltató.

    Miután sikeresen bejelentkezett beépülő modul, az alkalmazás jelennek meg a bejelentkezési azonosító és a teendők elemek listáját, és az adatokat is frissítenie.


<!-- URLs. -->
[Xamarin.Android alkalmazás létrehozása]: app-service-mobile-xamarin-android-get-started.md
