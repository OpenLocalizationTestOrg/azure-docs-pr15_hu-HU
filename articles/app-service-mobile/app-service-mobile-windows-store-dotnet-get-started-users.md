<properties
    pageTitle="Hitelesítés hozzáadása a egyetemes Windows platformon (UWP) alkalmazás |} Azure mobilalkalmazások"
    description="Azure alkalmazás szolgáltatás Mobile-alkalmazások használata az univerzális Windows platformon (UWP)-alkalmazást, több módon is Identitásszolgáltatók, beleértve a felhasználói hitelesítés: AAD, a Google, a Facebook, a Twitteren és a Microsoft."
    services="app-service\mobile"
    documentationCenter="windows"
    authors="adrianhall"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="add-authentication-to-your-windows-app"></a>Hitelesítés hozzáadása a Windows-alkalmazás

[AZURE.INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

Ez a témakör bemutatja, hogyan felhőalapú hitelesítési hozzáadása a mobilalkalmazásban. Ebben az oktatóanyagban vesz fel hitelesítési a univerzális Windows platformon (UWP) quickstart útmutató projekt Mobile-appokról az Azure alkalmazás szolgáltatás által támogatott identitásszolgáltató használata. Miután sikeresen alatt hitelesített és a mobilalkalmazás kódmentes által engedélyezett, jelenik meg a felhasználói azonosító értékét.

Ebben az oktatóanyagban a Mobile-alkalmazások quickstart útmutató alapul. Az oktatóprogram [Mobile-alkalmazások – első lépések](app-service-mobile-windows-store-dotnet-get-started.md)először be kell fejezni.

##<a name="register"></a>Regisztrálni a hitelesítést az alkalmazást, és az alkalmazás szolgáltatás konfigurálása

[AZURE.INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

##<a name="permissions"></a>Hitelesített felhasználói engedélyek korlátozása

[AZURE.INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

Most ellenőrizheti, hogy a kódmentes névtelen hozzáférés le van tiltva. A UWP app a project alkalmazással az indítási projekt beállítása telepítése, és futtassa az alkalmazást; Győződjön meg arról, hogy az alkalmazás indítása után következik (nem engedélyezett) 401 állapotkódot a esetén nem kezelt kivételt. Ennek oka az, hogy az alkalmazás nem hitelesített felhasználóként a Mobile-alkalmazás kód csatlakozna, de a *TodoItem* táblázat most hitelesítést igényel lehetőséget.

Ezután a felhasználók hitelesítését előtt igénylő erőforrások a alkalmazás szolgáltatásból alkalmazás frissíti.

##<a name="add-authentication"></a>Hitelesítés felvétele az alkalmazásba

1. A UWP alkalmazás fájl MainPage.cs a project és a következő kódrészletet hozzáadása a MainPage osztály:
    
        // Define a member variable for storing the signed-in user. 
        private MobileServiceUser user;

        // Define a method that performs the authentication process
        // using a Facebook sign-in. 
        private async System.Threading.Tasks.Task<bool> AuthenticateAsync()
        {
            string message;
            bool success = false;
            try
            {
                // Change 'MobileService' to the name of your MobileServiceClient instance.
                // Sign-in using Facebook authentication.
                user = await App.MobileService
                    .LoginAsync(MobileServiceAuthenticationProvider.Facebook);
                message =
                    string.Format("You are now signed in - {0}", user.UserId);

                success = true;
            }
            catch (InvalidOperationException)
            {
                message = "You must log in. Login Required";
            }

            var dialog = new MessageDialog(message);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
            return success;
        }

    Ez a kód egy Facebook jelentkezzen be az adott felhasználó hitelesíti. Ha nem a Facebook-identitásszolgáltató használata esetén értékének módosítása **MobileServiceAuthenticationProvider** feletti értékre tartozó.

3. Megjegyzés meg vagy a meglévő **OnNavigatedTo** többszörösen a hívást a **ButtonRefresh_Click** módszer (vagy a **InitLocalStoreAsync** módszer) törlése. Ez megakadályozhatja, hogy az adatok betöltésük előtt a felhasználó hitelesítése. Ezután szeretne hozzáadni a **Bejelentkezés** gombot az alkalmazás, amely elindítja a hitelesítési.

4. A következő kódrészletet hozzáadása a MainPage osztály:

        private async void ButtonLogin_Click(object sender, RoutedEventArgs e)
        {
            // Login the user and then load data from the mobile app.
            if (await AuthenticateAsync())
            {
                // Switch the buttons and load items from the mobile app.
                ButtonLogin.Visibility = Visibility.Collapsed;
                ButtonSave.Visibility = Visibility.Visible;
                //await InitLocalStoreAsync(); //offline sync support.
                await RefreshTodoItems();
            }
        }
        
5. Nyissa meg a MainPage.xaml projektfájl, keresse meg az elemet, amely meghatározza a **Mentés** gombra, és cserélhet le a következő kódot:

        <Button Name="ButtonSave" Visibility="Collapsed" Margin="0,8,8,0" 
                Click="ButtonSave_Click">
            <StackPanel Orientation="Horizontal">
                <SymbolIcon Symbol="Add"/>
                <TextBlock Margin="5">Save</TextBlock>
            </StackPanel>
        </Button>
        <Button Name="ButtonLogin" Visibility="Visible" Margin="0,8,8,0" 
                Click="ButtonLogin_Click" TabIndex="0">
            <StackPanel Orientation="Horizontal">
                <SymbolIcon Symbol="Permissions"/>
                <TextBlock Margin="5">Sign in</TextBlock> 
            </StackPanel>
        </Button>

9. Nyomja le az F5 billentyűt, futtassa az alkalmazást, kattintson a **Bejelentkezés** gombra, és jelentkezzen be az alkalmazásba a választott identitásszolgáltató. Miután a bejelentkezés sikeres, az alkalmazás nem talál hibát, és nézheti meg a lekérdezés a kódmentes és frissítések az adatok.


##<a name="tokens"></a>A hitelesítési jogkivonat tárolni az ügyfélszámítógépen

Az előző példában mutatott szabványos bejelentkezés, forduljon az identitásszolgáltató és az alkalmazás szolgáltatást is minden alkalommal elindítja az alkalmazást, az ügyfél igénylő. Nem, csak akkor ezt a módszert hatékony futtatását is lehetővé teszi az használatát-vonatkozik problémák kell vevőknek megpróbálja elindítani az Ön alkalmazás egyszerre. Jobb megoldást jelenthet, hogy a jóváhagyási jogkivonat, az alkalmazás szolgáltatás által visszaadott gyorsítótár, és próbálja meg az első használni a szolgáltató alapú bejelentkezés használata előtt.

>[AZURE.NOTE]A jogkivonat, függetlenül attól, hogy hitelesítéssel ügyfél által kezelt vagy a szolgáltatás által kezelt szolgáltatások alkalmazás által kibocsátott is gyorsítótár. Ebben az oktatóanyagban szolgáltatás felügyelt hitelesítést használ.

[AZURE.INCLUDE [mobile-windows-universal-dotnet-authenticate-app-with-token](../../includes/mobile-windows-universal-dotnet-authenticate-app-with-token.md)]

##<a name="next-steps"></a>Következő lépések

Alapszintű hitelesítés oktatóprogram befejezte, érdemes megfontolni a Folytatás megjelenítheti az alábbi oktatóanyagok egyikét:

+ [Leküldéses értesítések felvétele az alkalmazásba](app-service-mobile-windows-store-dotnet-get-started-push.md)  
  Megtudhatja, hogy miként vehet fel a leküldéses értesítések támogatása az alkalmazás és a mobilalkalmazás kódmentes Azure értesítés hubok segítségével küldje el a leküldéses értesítések konfigurálása.

+ [Az alkalmazás offline szinkronizálás engedélyezése](app-service-mobile-windows-store-dotnet-get-started-offline-data.md)  
  Megtudhatja, hogy miként hozzáadása az offline munka támogatása az alkalmazás-mobilalkalmazás kódmentes használatával. Kapcsolat nélküli szinkronizálás lehetővé teszi, hogy a végfelhasználók vezérléséhez mobilalkalmazásban&mdash;megtekintése, hozzáadása vagy módosítása, adatok&mdash;akkor is, ha nincs hálózati kapcsolat.


<!-- URLs. -->
[Get started with your mobile app]: app-service-mobile-windows-store-dotnet-get-started.md

