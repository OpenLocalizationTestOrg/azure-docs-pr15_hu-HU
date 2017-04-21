
1. Nyissa meg a megosztott projektfájl MainPage.cs, és a következő kódrészletet hozzáadása a MainPage osztály:
    
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

3. Megjegyzés meg, vagy törölje a meglévő **OnNavigatedTo** többszörösen **RefreshTodoItems** módszer a hívást.

    Ez megakadályozhatja, hogy az adatok betöltésük előtt a felhasználó hitelesítése. Ezután szeretne hozzáadni a **Bejelentkezés** gombot az alkalmazás, amely elindítja a hitelesítési.

4. A következő kódrészletet hozzáadása a MainPage osztály:

        private async void ButtonLogin_Click(object sender, RoutedEventArgs e)
        {
            // Login the user and then load data from the mobile app.
            if (await AuthenticateAsync())
            {
                // Hide the login button and load items from the mobile app.
                ButtonLogin.Visibility = Windows.UI.Xaml.Visibility.Collapsed;
                //await InitLocalStoreAsync(); //offline sync support.
                await RefreshTodoItems();
            }
        }
        
5. A Windows Áruházbeli alkalmazás projekt nyissa meg a MainPage.xaml projektfájlnak, és közvetlenül előtt az elemet, amely meghatározza a **Mentés** gombra a következő **gomb** elem hozzáadása:

        <Button Name="ButtonLogin" Click="ButtonLogin_Click" 
                        Visibility="Visible">Sign in</Button>

6. A Windows Phone áruház alkalmazás projekt adja hozzá a következő **gomb** elem **ContentPanel**, a a **Szövegdoboz** elem után:

        <Button Grid.Row ="1" Grid.Column="1" Name="ButtonLogin" Click="ButtonLogin_Click" 
            Margin="10, 0, 0, 0" Visibility="Visible">Sign in</Button>

8. Nyissa meg a megosztott App.xaml.cs projekt-fájlját, és adja hozzá a következő kódot:

        protected override void OnActivated(IActivatedEventArgs args)
        {
            // Windows Phone 8.1 requires you to handle the respose from the WebAuthenticationBroker.
            #if WINDOWS_PHONE_APP
            if (args.Kind == ActivationKind.WebAuthenticationBrokerContinuation)
            {
                // Completes the sign-in process started by LoginAsync.
                // Change 'MobileService' to the name of your MobileServiceClient instance. 
                App.MobileService.LoginComplete(args as WebAuthenticationBrokerContinuationEventArgs);
            }
            #endif

            base.OnActivated(args);
        }

    Ha már létezik a **OnActivated** módszer, csak adja meg a `#if...#endif` kód blokkolása.

9. Nyomja le az F5 billentyűt, futtassa a Windows áruházból letölthető, kattintson a **Bejelentkezés** gombra, és jelentkezzen be az alkalmazásba a választott identitásszolgáltató. 

    Miután sikeresen bejelentkezett beépülő modul, hibátlan futtassa az alkalmazást, és látnia kell a kódmentes lekérdezés és frissítések az adatokat.

10. Kattintson a jobb gombbal a Windows Phone áruház alkalmazás projekt, **projekt indítása**parancsra, majd ismételje az előző lépést, annak ellenőrzésére, hogy a Windows Phone-alkalmazás is megfelelően fut áruházból.  

 