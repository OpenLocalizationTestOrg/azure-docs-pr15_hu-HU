
1. A **Project Explorer** Android Studio alkalmazásban nyissa meg a ToDoActivity.java fájlt, és adja hozzá a következő importálása utasításokat.

        import java.util.concurrent.ExecutionException;
        import java.util.concurrent.atomic.AtomicBoolean;

        import android.content.Context;
        import android.content.SharedPreferences;
        import android.content.SharedPreferences.Editor;

        import com.microsoft.windowsazure.mobileservices.authentication.MobileServiceAuthenticationProvider;
        import com.microsoft.windowsazure.mobileservices.authentication.MobileServiceUser;

2. A következő módszerrel hozzáadása a **ToDoActivity** osztály: 
    
        private void authenticate() {
            // Login using the Google provider.
            
            ListenableFuture<MobileServiceUser> mLogin = mClient.login(MobileServiceAuthenticationProvider.Google);
    
            Futures.addCallback(mLogin, new FutureCallback<MobileServiceUser>() {
                @Override
                public void onFailure(Throwable exc) {
                    createAndShowDialog((Exception) exc, "Error");
                }           
                @Override
                public void onSuccess(MobileServiceUser user) {
                    createAndShowDialog(String.format(
                            "You are now logged in - %1$2s",
                            user.getUserId()), "Success");
                    createTable();  
                }
            });     
        }


    Ezzel létrehoz egy új módszer a hitelesítési eljárás kezelésére. A felhasználó hitelesítése a Google bejelentkezési használatával. Egy párbeszédpanel jelenik meg, amelyek a hitelesített felhasználó azonosítója jeleníti meg. Nem folytatható, egy pozitív hitelesítés nélkül.

    > [AZURE.NOTE] Ha nem a Google-identitásszolgáltató használata esetén módosítsa az értéket, átadni a **bejelentkezési** módszer feletti az alábbiak egyikét: _Microsoftaccountja_, _Facebook_, _Twitter_vagy _windowsazureactivedirectory_.

3. Az a **onCreate** módot, adja meg a következő sort a kód után a kódot, amely elindítja a `MobileServiceClient` objektumot.

        authenticate();

    A hívás indítása a hitelesítési folyamat.

4. Helyezze át a hátralévő kód után `authenticate();` egy új **createTable** módszerrel **onCreate** módszer, amely így néz ki:

        private void createTable() {
    
            // Get the table instance to use.
            mToDoTable = mClient.getTable(ToDoItem.class);
    
            mTextNewToDo = (EditText) findViewById(R.id.textNewToDo);
    
            // Create an adapter to bind the items with the view.
            mAdapter = new ToDoItemAdapter(this, R.layout.row_list_to_do);
            ListView listViewToDo = (ListView) findViewById(R.id.listViewToDo);
            listViewToDo.setAdapter(mAdapter);
    
            // Load the items from Azure.
            refreshItemsFromTable();
        }

9. A **Futtatás** menüből válassza a **Futtatás alkalmazás** indítsa el az alkalmazást, és jelentkezzen be a választott identitásszolgáltató. 

    Miután sikeresen bejelentkezett beépülő modul, hibátlan futtassa az alkalmazást, és látnia kell a kódmentes szolgáltatás lekérdezés és frissítések az adatok.