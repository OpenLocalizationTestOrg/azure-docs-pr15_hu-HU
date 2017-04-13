<properties
    pageTitle="Azure Active Directory – első lépések a bejelentkezés és kijelentkezés node.js használatával"
    description="Egy node.js Express MVC Web App alkalmazásban, amely integrálódik az Azure Active Directory való bejelentkezéshez létrehozásának módját."
    services="active-directory"
    documentationCenter="nodejs"
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
  ms.tgt_pltfrm="na"
    ms.devlang="javascript"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="brandwe"/>

# <a name="nodejs-web-app-sign-in--sign-out-with-azure-ad"></a>A Web App bejelentkezési NodeJS, és jelentkezzen be az Azure AD meg


Itt Passport be:

- A felhasználó jelentkezzen be az Azure Active Directory az alkalmazást.
- A felhasználó információkat megjeleníteni.
- Jelentkezzen be a felhasználó ki az alkalmazást.

**A Passport** hitelesítési köztes Node.js számára is. Rendkívül rugalmas és elemes, Passport unobtrusively húzható minden Express alapú vagy webalkalmazás Resitify. Stratégiák a felhasználónevet és jelszót, Facebook, Twitteren és az egyéb használatával hitelesítés támogatása a teljes készletével. Microsoft Azure Active Directory stratégia kialakítása azt. Azt a modul telepítése és vegye fel a Microsoft Azure Active Directory `passport-azure-ad` beépülő modult.

Annak érdekében, hogy ehhez kell:

1. Regisztráljon az alkalmazás.
2. Állítsa be az alkalmazás az ADFS-azure-Passport stratégia.
3. Azure Active Directory bejelentkezési és kijelentkezési kérések ki Passport használatára.
4. Nyomtassa ki a felhasználó adatait.

Ebben az oktatóanyagban kódját [a GitHub](https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS)karbantartása.  Követésre, [Töltse le az alkalmazás skeleton, mint egy .zip](https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS/archive/skeleton.zip) is vagy a skeleton klónozhatja:

```git clone --branch skeleton https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS.git```

A kész alkalmazást is ez az oktatóanyag végén megadva.

## <a name="1-register-an-app"></a>1. a regisztráció alkalmazás
- Jelentkezzen be az Azure adatkezelési portálra.
- A bal oldali navigációs sávon kattintson **Az Active Directory**.
- Jelölje ki a bérlő, ha ki szeretne regisztrálni az alkalmazást.
- Kattintson az **alkalmazások** fülre, és kattintson a Hozzáadás a alsó fiókban.
- Az útmutatást követve hozzon létre egy új **webalkalmazás és/vagy WebAPI**.
    - Az alkalmazás **neve** leírja a végfelhasználók alkalmazás
    -   A **Bejelentkezési URL** az alkalmazás alap URL-CÍMÉT.  A skeleton alapértelmezés "visszatérési http://localhost:3000/auth/openid".
    - Az **Alkalmazás azonosítója URI** az alkalmazás egy egyedi azonosítót.  A kiállítás környezetbe `https://<tenant-domain>/<app-name>`, például`https://contoso.onmicrosoft.com/my-first-aad-app`
- Ha befejezte a regisztrációs, AAD rendel az alkalmazás egy egyedi ügyfél-azonosító.  Ez az érték van szüksége a következő szakaszok, így másolja a vágólapra a beállítás lapon.

## <a name="2-add-pre-requisities-to-your-directory"></a>2. előtti-requisities hozzáadása a címtárban

A parancssorból könyvtárak a legfelső szintű mappához Ha még nem létezik módosítása, és futtassa az alábbi parancsokat:

- `npm install express`
- `npm install ejs`
- `npm install ejs-locals`
- `npm install restify`
- `npm install mongoose`
- `npm install bunyan`
- `npm install assert-plus`
- `npm install passport`

- Ezeken kívül szüksége lesz a `passport-azure-ad` is látható:

- `npm install passport-azure-ad`

Ez a telepíti a tárakban attól függenek, hogy passport azure-ADFS-.

## <a name="3-set-up-your-app-to-use-the-passport-node-js-strategy"></a>3. Állítsa be az alkalmazás a passport-csomópont-js stratégia
Ebben az esetben azt fogja állítsa be a Express köztes a csatlakozás OpenID hitelesítési protokoll használatára.  Passport bejelentkezési és kijelentkezési kérések hiba kezelése munkamenetet a felhasználó és a felhasználó számára, többek között adatainak használatával.

-   Első lépésként nyissa meg a `config.js` a legfelső szintű a projekt fájlt, és adja meg az alkalmazás konfigurációs értékét a `exports.creds` szakaszban.
    -   A `clientID:` az alkalmazás a regisztrációs portálon rendelt **Azonosítója** .
    -   A `returnURL` a portálon megadott **Átirányítás Uri** van.
    - A `clientSecret` van a titkos hozza létre, ha a portálon

- Ezután nyissa meg a `app.js` a projektben való legfelső szintű a fájl, és adja hozzá a feloldását kérte hívás meghívásához a `OIDCStrategy` épített stratégia`passport-azure-ad`


```JavaScript
var OIDCStrategy = require('passport-azure-ad').OIDCStrategy;

// add a logger

var log = bunyan.createLogger({
    name: 'Microsoft OIDC Example Web Application'
});
```

- Ezután az azt közvetlenül a bejelentkezési kérések kezelésére hivatkozott stratégia használata

```JavaScript
// Use the OIDCStrategy within Passport. (Section 2) 
// 
//   Strategies in passport require a `validate` function, which accept
//   credentials (in this case, an OpenID identifier), and invoke a callback
//   with a user object.
passport.use(new OIDCStrategy({
    callbackURL: config.creds.returnURL,
    realm: config.creds.realm,
    clientID: config.creds.clientID,
    clientSecret: config.creds.clientSecret,
    oidcIssuer: config.creds.issuer,
    identityMetadata: config.creds.identityMetadata,
    skipUserProfile: config.creds.skipUserProfile,
    responseType: config.creds.responseType,
    responseMode: config.creds.responseMode
  },
  function(iss, sub, profile, accessToken, refreshToken, done) {
    if (!profile.email) {
      return done(new Error("No email found"), null);
    }
    // asynchronous verification, for effect...
    process.nextTick(function () {
      findByEmail(profile.email, function(err, user) {
        if (err) {
          return done(err);
        }
        if (!user) {
          // "Auto-registration"
          users.push(profile);
          return done(null, profile);
        }
        return done(null, user);
      });
    });
  }
));
```
Passport hasonló mintát használ, az összes stratégiák (Twitter, Facebook stb.), amely az összes stratégia szövegírói igazodjon. Megjeleníti a stratégia látni azt adja át egy function() jogkivonat, és a paraméterek kész. A stratégia fog megfelelő gondossággal térjen vissza az us Ha így tesz, az összes meg adatait a munkát. Ha így tesz szeretnénk fog tárolni a felhasználó és a token Panelcsoport, így nem kell a kérdezzen rá.


> [AZURE.IMPORTANT] 
A fenti kódot, amely a kiszolgáló hitelesítést végezni történik bármelyik felhasználó megnyitja. Automatikus regisztrációs nevezik. Gyártás-kiszolgálók nem szeretne engedélyez nélkül első őket a regisztrációs folyamat döntse el, hogy feldolgozzuk. Ez általában az a mintázat látható fogyasztói alkalmazások, akiknek engedélyezi a Facebook regisztrálása, de majd kérjük, hogy adja meg a további információt. Ha ez nem lett egy minta alkalmazást, azt is van csak kiolvasott az e-mailt a token objektum, amely adja vissza, és ezután ismételt őket töltheti ki további információt. Mivel ez a próba-kiszolgáló azt egyszerűen felveheti a memóriában adatbázist.

- Ezután a módszereket, amely lehetővé teszi számunkra, nyomon követheti a a bejelentkezett a felhasználók által Passport szükség szerint helyezzen el. Ide tartoznak a szerializálása és a felhasználói adatok deszerializálása:

```JavaScript

// Passport session setup. (Section 2)

//   To support persistent login sessions, Passport needs to be able to
//   serialize users into and deserialize users out of the session.  Typically,
//   this will be as simple as storing the user ID when serializing, and finding
//   the user by ID when deserializing.
passport.serializeUser(function(user, done) {
  done(null, user.email);
});

passport.deserializeUser(function(id, done) {
  findByEmail(id, function (err, user) {
    done(err, user);
  });
});

// array to hold logged in users
var users = [];

var findByEmail = function(email, fn) {
  for (var i = 0, len = users.length; i < len; i++) {
    var user = users[i];
   log.info('we are using user: ', user);
    if (user.email === email) {
      return fn(null, user);
    }
  }
  return fn(null, null);
};
```

- Ezután a kódot, és töltse be a sürgős motor hozzáadása. Itt látható, használja az alapértelmezett /views és /routes minta, amely Express biztosít.

```JavaScript

// configure Express (Section 2)

var app = express();


app.configure(function() {
  app.set('views', __dirname + '/views');
  app.set('view engine', 'ejs');
  app.use(express.logger());
  app.use(express.methodOverride());
  app.use(cookieParser());
  app.use(expressSession({ secret: 'keyboard cat', resave: true, saveUninitialized: false }));
  app.use(bodyParser.urlencoded({ extended : true }));
  // Initialize Passport!  Also use passport.session() middleware, to support
  // persistent login sessions (recommended).
  app.use(passport.initialize());
  app.use(passport.session());
  app.use(app.router);
  app.use(express.static(__dirname + '/../../public'));
});

```

- Végezetül hozzáadása a leveleket, hogy a tényleges bejelentkezési kérések leadandó fog a `passport-azure-ad` motor:

```JavaScript

// Our Auth routes (Section 3)

// GET /auth/openid
//   Use passport.authenticate() as route middleware to authenticate the
//   request.  The first step in OpenID authentication will involve redirecting
//   the user to their OpenID provider.  After authenticating, the OpenID
//   provider will redirect the user back to this application at
//   /auth/openid/return
app.get('/auth/openid',
  passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
  function(req, res) {
    log.info('Authentication was called in the Sample');
    res.redirect('/');
  });

// GET /auth/openid/return
//   Use passport.authenticate() as route middleware to authenticate the
//   request.  If authentication fails, the user will be redirected back to the
//   login page.  Otherwise, the primary route function function will be called,
//   which, in this example, will redirect the user to the home page.
app.get('/auth/openid/return',
  passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
  function(req, res) {
    log.info('We received a return from AzureAD.');
    res.redirect('/');
  });

// POST /auth/openid/return
//   Use passport.authenticate() as route middleware to authenticate the
//   request.  If authentication fails, the user will be redirected back to the
//   login page.  Otherwise, the primary route function function will be called,
//   which, in this example, will redirect the user to the home page.
app.post('/auth/openid/return',
  passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
  function(req, res) {
    log.info('We received a return from AzureAD.');
    res.redirect('/');
  });
  ```

## <a name="4-use-passport-to-issue-sign-in-and-sign-out-requests-to-azure-ad"></a>4. Azure Active Directory számára a bejelentkezési és kijelentkezési kérések állít Passport használata

Az alkalmazás letöltése megfelelően konfigurálva a hitelesítési OpenID csatlakozás protokollal 2.0-s verzió végpont kommunikálni.  `passport-azure-ad`van készített érdekli, az összes hitelesítési üzeneteket létrehozásával, a érvényesítése token származhat Azure Active Directory és a felhasználói munkamenet fenntartása szép részleteit.  Továbbra is, hogy adjon a felhasználóknak, jelentkezzen be, jelentkezzen ki, és további információk gyűjtése a bejelentkezett felhasználó a lehetőség.

- Első lépésként lehetővé teszi, hogy az alapértelmezett, bejelentkezés, fiók és kijelentkezés módszereket, amelyekkel hozzáadása a `app.js` fájl:

```JavaScript

//Routes (Section 4)

app.get('/', function(req, res){
  res.render('index', { user: req.user });
});

app.get('/account', ensureAuthenticated, function(req, res){
  res.render('account', { user: req.user });
});

app.get('/login',
  passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
  function(req, res) {
    log.info('Login was called in the Sample');
    res.redirect('/');
});

app.get('/logout', function(req, res){
  req.logout();
  res.redirect('/');
});

```

-   Tanulmányozzuk ezek részletesen:
    -   A `/` útvonalon átirányítja a index.ejs nézetre átadása a felhasználó a kérelem (ha van ilyen)
    - A `/account` útvonal az első, ***Győződjön meg arról, hogy hitelesített*** lesz (megvalósítása azt, amely alatt) adják majd a felhasználó a kérelem, hogy azt elérheti, hogy a felhasználó további információt.
    - A `/login` útvonal visszahívja a saját azuread-openidconnect hitelesítő `passport-azuread` , és ha, amely nem fog sikerülni átirányítása a felhasználó újra Publication
    - A `/logout` fog egyszerűen hívja fel a logout.ejs (és átirányítása) amelyek cookie-k törlése, és térjen vissza az index.ejs a felhasználó


- Az utolsó részét `app.js`, helyezzen el a EnsureAuthenticated módszer használt `/account` felett.

```JavaScript

// Simple route middleware to ensure user is authenticated. (Section 4)

//   Use this route middleware on any resource that needs to be protected.  If
//   the request is authenticated (typically via a persistent login session),
//   the request will proceed.  Otherwise, the user will be redirected to the
//   login page.
function ensureAuthenticated(req, res, next) {
  if (req.isAuthenticated()) { return next(); }
  res.redirect('/login')
}
```

- Végezetül ténylegesen létrehozása a kiszolgáló magát a `app.js`:

```JavaScript

app.listen(3000);

```


## <a name="5-create-the-views-and-routes-in-express-to-display-our-user-in-the-website"></a>5. hozza létre a nézetek és útvonalak Express megjelenítése a felhasználó a webhelyen

Van még a `app.js` teljes. Most egyszerűen szükség adja hozzá a útvonalak és a nézetek, amely azt a felhasználót, valamint a fogópont eléréséhez az adatokat jelenít meg a `/logout` és `/login` korábban létrehozott útvonalak.

- Hozzon létre a `/routes/index.js` a legfelső szintű könyvtárában továbbításához.

```JavaScript
/*
 * GET home page.
 */

exports.index = function(req, res){
  res.render('index', { title: 'Express' });
};
```

- Hozzon létre a `/routes/user.js` a legfelső szintű könyvtárában továbbítására

```JavaScript
/*
 * GET users listing.
 */

exports.list = function(req, res){
  res.send("respond with a resource");
};
```

Ezek az egyszerű útvonalak fog csak átadni a kérelem mentén a nézeteket, például a felhasználó, ha jelen van.

- Hozzon létre a `/views/index.ejs` megtekintése a legfelső szintű könyvtárban. Ez az egyszerű lap, amely a bejelentkezés és kijelentkezés módszerek felhívni és engedélyezése us ragadni a fiók adatait. Értesítés, hogy a feltételes ábrázolásakor `if (!user)` felhasználóként a kérelem átadott keresztül van még a bejelentkezett felhasználó bizonyítékokra.

```JavaScript
<% if (!user) { %>
    <h2>Welcome! Please log in.</h2>
    <a href="/login">Log In</a>
<% } else { %>
    <h2>Hello, <%= user.displayName %>.</h2>
    <a href="/account">Account Info</a></br>
    <a href="/logout">Log Out</a>
<% } %>
```

- Hozzon létre a `/views/account.ejs` a legfelső szintű könyvtárban megtekintheti, hogy akkor is megtekintheti a további információt, amely `passport-azuread` van helyezi el a felhasználói kérelem.

```Javascript
<% if (!user) { %>
    <h2>Welcome! Please log in.</h2>
    <a href="/login">Log In</a>
<% } else { %>
<p>displayName: <%= user.displayName %></p>
<p>givenName: <%= user.name.givenName %></p>
<p>familyName: <%= user.name.familyName %></p>
<p>UPN: <%= user._json.upn %></p>
<p>Profile ID: <%= user.id %></p>
<p>Full Claimes</p>
<%- JSON.stringify(user) %>
<p></p>
<a href="/logout">Log Out</a>
<% } %>
```

- Végezetül vegyük teheti rendben talált közérthető elrendezést. Hozzon létre a "vagy a legfelső szintű könyvtárban views/layout.ejs' nézet

```HTML

<!DOCTYPE html>
<html>
    <head>
        <title>Passport-OpenID Example</title>
    </head>
    <body>
        <% if (!user) { %>
            <p>
            <a href="/">Home</a> | 
            <a href="/login">Log In</a>
            </p>
        <% } else { %>
            <p>
            <a href="/">Home</a> | 
            <a href="/account">Account</a> | 
            <a href="/logout">Log Out</a>
            </p>
        <% } %>
        <%- body %>
    </body>
</html>
```

Végezetül továbbépítése és az alkalmazás futtatásához! 

Futtatása `node app.js` , és keresse meg`http://localhost:3000`


Jelentkezzen be akár személyes Microsoft-Account, akár egy munkahelyi vagy iskolai fiókjával, és figyelje meg, hogyan a felhasználói azonosító tükröződik a /account listában.  Most már védett iparágban szabványos protokollokkal hitelesíthet a személyes és munkahelyi vagy iskolai fiókkal rendelkező felhasználók webalkalmazást.

Hivatkozások [formájában érhető el az alábbi egy .zip](https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS/archive/complete.zip)kész példa (nélkül a konfigurációs értékeinek), illetve klónozhatja, a GitHub:

```git clone --branch complete https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS.git```


Most már áthelyezheti Speciális témakörök alakzatot.  Előfordulhat, hogy szeretne kipróbálni:

[A webes API az Azure Active Directory biztonságos >>](active-directory-devquickstarts-webapi-nodejs.md)

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
