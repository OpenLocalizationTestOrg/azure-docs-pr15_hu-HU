
1. A MainPage.xaml.cs projektfájlba adja hozzá a következő **használata** kimutatásokban:

        using System.Linq;      
        using Windows.Security.Credentials;

2. A **AuthenticateAsync** módszer cserélje ki a következő kódot:

        private async System.Threading.Tasks.Task<bool> AuthenticateAsync()
        {
            string message;
            bool success = false;

            // This sample uses the Facebook provider.
            var provider = MobileServiceAuthenticationProvider.Facebook;

            // Use the PasswordVault to securely store and access credentials.
            PasswordVault vault = new PasswordVault();
            PasswordCredential credential = null;

            try
            {
                // Try to get an existing credential from the vault.
                credential = vault.FindAllByResource(provider.ToString()).FirstOrDefault();
            }
            catch (Exception)
            {
                // When there is no matching resource an error occurs, which we ignore.
            }

            if (credential != null)
            {
                // Create a user from the stored credentials.
                user = new MobileServiceUser(credential.UserName);
                credential.RetrievePassword();
                user.MobileServiceAuthenticationToken = credential.Password;

                // Set the user from the stored credentials.
                App.MobileService.CurrentUser = user;

                // Consider adding a check to determine if the token is 
                // expired, as shown in this post: http://aka.ms/jww5vp.

                success = true;
                message = string.Format("Cached credentials for user - {0}", user.UserId);
            }
            else
            {
                try
                {
                    // Login with the identity provider.
                    user = await App.MobileService
                        .LoginAsync(provider);

                    // Create and store the user credentials.
                    credential = new PasswordCredential(provider.ToString(),
                        user.UserId, user.MobileServiceAuthenticationToken);
                    vault.Add(credential);

                    success = true;
                    message = string.Format("You are now logged in - {0}", user.UserId);
                }
                catch (MobileServiceInvalidOperationException)
                {
                    message = "You must log in. Login Required";
                }
            }
            
            var dialog = new MessageDialog(message);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();

            return success;
        }

    **AuthenticateAsync**e verziójában az alkalmazás próbálja használni a **PasswordVault** tárolt hitelesítő adatokhoz elérni a szolgáltatást. Normál bejelentkezés is történik, amikor nincs tárolt hitelesítő adatokat nem.

    >[AZURE.NOTE]Egy gyorsítótárazott token lehet lejárt, és az alkalmazás használata közben a következő is jelentkezhet jogkivonat lejárati hitelesítés után. Megtudhatja, hogyan állapíthatja meg, ha egy token lejárt, olvassa el a [lejárt hitelesítési tokenek ellenőrzése](http://aka.ms/jww5vp)című témakört. Megoldásának elévülési tokenek engedélyezési hibáinak kezelése olvassa el a a bejegyzést, [a felügyelt SDK gyorsítótár és a lejárt tokenek Azure Mobile szolgáltatások kezelése](http://blogs.msdn.com/b/carlosfigueira/archive/2014/03/13/caching-and-handling-expired-tokens-in-azure-mobile-services-managed-sdk.aspx)című témakört. 

3. Kétszer indítsa újra az alkalmazást.

    Figyelje meg, hogy az első indításkor, jelentkezzen be a szolgáltató újra meg kell adni. Azonban a második indítsa újra a gyorsítótárban tárolt hitelesítő használja, és a bejelentkezési elmarad. 
