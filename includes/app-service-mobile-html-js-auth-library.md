###<a name="server-auth"></a>Útmutató: hitelesítést végezni egy szolgáltatót (kiszolgáló áramlás)

Telepítve van a Mobile-alkalmazások kezelése az alkalmazásban a hitelesítési folyamat, regisztrálnia kell az alkalmazás a identitásszolgáltatójával. Az Azure App szolgáltatásban, akkor kell azonosítója és a szolgáltató által biztosított titkos konfigurálása.
További tudnivalókért lásd: az oktatóprogram [az alkalmazás hozzáadása hitelesítés].

Miután regisztrálta a identitásszolgáltató, egyszerűen hívja fel a .login() módszer a szolgáltató nevét. Ha például Facebook használja az alábbi kódot bejelentkezni.

```
client.login("facebook").done(function (results) {
     alert("You are now logged in as: " + results.userId);
}, function (err) {
     alert("Error: " + err);
});
```

Ha nem a Facebook-identitásszolgáltató használata esetén módosítsa az értéket, átadni a bejelentkezési módszer feletti az alábbiak egyikét: `microsoftaccount`, `facebook`, `twitter`, `google`, vagy `aad`.

Ebben az esetben Azure alkalmazás szolgáltatás kezeli a OAuth 2.0-s hitelesítési folyamat megjelenítése a kijelölt szolgáltató bejelentkezési lapját, és egy alkalmazás szolgáltatás hitelesítési jogkivonat létrehozása az identitásszolgáltatójával sikeres bejelentkezés után. A bejelentkezési függvény, amikor elkészült, adja vissza egy JSON objektum (felhasználói), amely a felhasználói Azonosítóját és az alkalmazás szolgáltatás közzététele hitelesítési jogkivonat a felhasználóazonosító és authenticationToken mezőket kurzor. Ez a token is gyorsítótáras, és újra felhasználni mindaddig, amíg lejárna.

###<a name="client-auth"></a>Útmutató: hitelesítést végezni egy szolgáltatót (ügyfél-adatfolyam)

Az alkalmazás egymástól függetlenül is fordulhat a identitásszolgáltató, és majd meg kell adnia a visszaadott jogkivonat a App szolgáltatásban hitelesítéshez. Ebben az ügyfél adatfolyamban lehetővé teszi, hogy felhasználók egy egyszeri bejelentkezés módja a szükséges vagy további felhasználói adatok visszakeresése a identitásszolgáltató.

#### <a name="social-authentication-basic-example"></a>Közösségi hitelesítési egyszerű példa

Ez a példa Facebook-ügyfél SDK hitelesítés:

```
client.login(
     "facebook",
     {"access_token": token})
.done(function (results) {
     alert("You are now logged in as: " + results.userId);
}, function (err) {
     alert("Error: " + err);
});
```
Ez a példa feltételezi, hogy a megfelelő szolgáltató SDK által biztosított jogkivonat a token változóban tárolja.

#### <a name="microsoft-account-example"></a>Microsoft-Account példa

Az alábbi példában a Live SDK csomagjában talál, amelyek egyetlen-bejelentkezés a Windows áruházból alkalmazásokat támogatja a Microsoft-Account:

```
WL.login({ scope: "wl.basic"}).then(function (result) {
      client.login(
            "microsoftaccount",
            {"authenticationToken": result.session.authentication_token})
      .done(function(results){
            alert("You are now logged in as: " + results.userId);
      },
      function(error){
            alert("Error: " + err);
      });
});
```

Ez a példa egy token megszerzi Live kapcsolatba, amelyeket az alkalmazás szolgáltatás hívja fel a bejelentkezési függvény.

###<a name="auth-getinfo"></a>Útmutató: a hitelesített felhasználó információhoz juthat

A hitelesítési adatait az aktuális felhasználó számára beolvasása is a `/.auth/me` AJAX módszerrel végpontot.  Győződjön meg róla, beállíthatja a `X-ZUMO-AUTH` a hitelesítési jogkivonat fejléc.  A hitelesítési jogkivonat tárolt `client.currentUser.mobileServiceAuthenticationToken`.  Ha például a fájllehívási API használata:

```
var url = client.applicationUrl + '/.auth/me';
var headers = new Headers();
headers.append('X-ZUMO-AUTH', client.currentUser.mobileServiceAuthenticationToken);
fetch(url, { headers: headers })
    .then(function (data) {
        return data.json()
    }).then(function (user) {
        // The user object contains the claims for the authenticated user
    });
```

Fájllehívási egy npm csomagot vagy CDNJS böngésző letölthető érhető el. JQuery vagy más AJAX API-ból az információkat is használhatja.  Adatok JSON objektumként fogadja.
