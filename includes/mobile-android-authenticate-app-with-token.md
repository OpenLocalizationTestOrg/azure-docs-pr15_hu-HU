
Az előző példában mutatott szabványos bejelentkezés, forduljon az identitásszolgáltató, mind az Azure service kódmentes minden alkalommal elindítja az alkalmazást, az ügyfél igénylő. Nem csak akkor is hiba lép fel használatát kapcsolatos kell vevőknek megpróbálja elindítani az Ön alkalmazás egyszerre hatékony az ezt a módszert. Jobb megoldást jelenthet, hogy a jóváhagyási jogkivonat, a Azure szolgáltatás által visszaadott gyorsítótárban, és próbálja meg az első használni a szolgáltató alapú bejelentkezés használata előtt. 

>[AZURE.NOTE]A jogkivonat, a kódmentes függetlenül attól, hogy hitelesítéssel ügyfél által kezelt vagy a szolgáltatás által kezelt Azure szolgáltatás által kibocsátott is gyorsítótár. Ebben az oktatóanyagban szolgáltatás felügyelt hitelesítést használ.


1. Nyissa meg a ToDoActivity.java fájlt, és adja hozzá a következő importálása utasításokat:

        import android.content.Context;
        import android.content.SharedPreferences;
        import android.content.SharedPreferences.Editor;

2. Adja hozzá a az alábbi tagokat, hogy a `ToDoActivity` osztály.

        public static final String SHAREDPREFFILE = "temp"; 
        public static final String USERIDPREF = "uid";  
        public static final String TOKENPREF = "tkn";   


3. A ToDoActivity.java fájlt az adja meg a következő definíciója a `cacheUserToken` módszert.
 
        private void cacheUserToken(MobileServiceUser user)
        {
            SharedPreferences prefs = getSharedPreferences(SHAREDPREFFILE, Context.MODE_PRIVATE);
            Editor editor = prefs.edit();
            editor.putString(USERIDPREF, user.getUserId());
            editor.putString(TOKENPREF, user.getAuthenticationToken());
            editor.commit();
        }   
  
    Ez a módszer a felhasználói azonosító, és a token egy privát megjelölt preferencia-sorrend fájl tárol. A gyorsítótárban való hozzáférés ki kell védeni, hogy az eszközön egyéb alkalmazások nincs hozzáférése a jogkivonat, mert a beállítás ki az alkalmazás védőfal mögé helyezett. Jó helyen jár Ha valaki hozzáfér az eszközre, akkor lehet, hogy a token gyorsítótár más módon hozzáférést szerezhetnek. 

    >[AZURE.NOTE]A token szereplő adatoknak titkosítással további védelmét, ha számít, hogy nagyon fontos jogkivonat az adatokhoz való hozzáférés, és valaki lehet hozzáférni az eszközt. Teljesen biztonságos megoldást azonban nem túlra ebben az oktatóanyagban és a biztonsági követelményeknek függ.


4. A ToDoActivity.java fájlt az adja meg a következő definíciója a `loadUserTokenCache` módszert.

        private boolean loadUserTokenCache(MobileServiceClient client)
        {
            SharedPreferences prefs = getSharedPreferences(SHAREDPREFFILE, Context.MODE_PRIVATE);
            String userId = prefs.getString(USERIDPREF, null); 
            if (userId == null)
                return false;
            String token = prefs.getString(TOKENPREF, null); 
            if (token == null)
                return false;
                
            MobileServiceUser user = new MobileServiceUser(userId);
            user.setAuthenticationToken(token);
            client.setCurrentUser(user);
                
            return true;
        }



5. A *ToDoActivity.java* fájl cserélje le a `authenticate` módszer a következő módszerrel használó jogkivonat gyorsítótárat. A bejelentkezés-szolgáltató módosítása, ha nem a Google-fiókot használ.

        private void authenticate() {
            // We first try to load a token cache if one exists.
            if (loadUserTokenCache(mClient))
            {
                createTable();
            }
            // If we failed to load a token cache, login and create a token cache
            else
            {
                // Login using the Google provider.    
                ListenableFuture<MobileServiceUser> mLogin = mClient.login(MobileServiceAuthenticationProvider.Google);
        
                Futures.addCallback(mLogin, new FutureCallback<MobileServiceUser>() {
                    @Override
                    public void onFailure(Throwable exc) {
                        createAndShowDialog("You must log in. Login Required", "Error");
                    }           
                    @Override
                    public void onSuccess(MobileServiceUser user) {
                        createAndShowDialog(String.format(
                                "You are now logged in - %1$2s",
                                user.getUserId()), "Success");
                        cacheUserToken(mClient.getCurrentUser());
                        createTable();  
                    }
                });
            }
        }

6. Az alkalmazás és a vizsgálat hitelesítéssel érvényes fiók összeállítása. Legalább kétszer futtatni. Az első futtatása során egy üzenet kéri, jelentkezzen be, és hozzon létre a token gyorsítótár kell kapnia. Ezt követően minden futtatásakor megpróbálja töltse be a token gyorsítótár hitelesítéshez, és nem kell a bejelentkezés szükséges.



