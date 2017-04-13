<properties
    pageTitle="Hogyan használható a .NET kódmentes kiszolgálóval SDK, a Mobile-alkalmazások |} Azure alkalmazás szolgáltatás"
    description="Megtudhatja, hogy miként működik a .NET kódmentes kiszolgálóval SDK Azure alkalmazás szolgáltatás Mobile-alkalmazások."
    keywords="alkalmazás szolgáltatás, azure alkalmazás, mobilalkalmazás, a mobil szolgáltatást, a méretarány méretezhető, alkalmazás telepítéshez azure alkalmazások telepítésének"
    services="app-service\mobile"
    documentationCenter=""
    authors="adrianhall"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-multiple"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="work-with-the-net-backend-server-sdk-for-azure-mobile-apps"></a>A .NET kódmentes kiszolgáló SDK Azure Mobile-alkalmazások használata

[AZURE.INCLUDE [app-service-mobile-selector-server-sdk](../../includes/app-service-mobile-selector-server-sdk.md)]

Ez a témakör bemutatja, hogyan a .NET kódmentes kiszolgáló SDK esetek Azure alkalmazás szolgáltatás Mobile-alkalmazások használatát. Az Azure Mobile alkalmazások SDK segít a mobilügyfelek az ASP.NET-alkalmazásokból használata.

>[AZURE.TIP] A [.NET server Azure Mobile-appokról SDK] [ 2] GitHub a Megnyitás van. A tár összes forráskódot, beleértve az egész kiszolgálóra SDK egység próba csomagot, és néhány példa projektek tartalmazza.

## <a name="reference-documentation"></a>Hivatkozási dokumentáció

A kiszolgáló SDK dokumentációját található itt: [Azure Mobile alkalmazások .NET hivatkozás][1].

## <a name="create-app"></a>Útmutató: a .NET mobilalkalmazás kódmentes létrehozása

Az [Azure portálon] vagy a Visual Studio alkalmazás szolgáltatásalkalmazás új projekt indulva is létrehozhat. Az szolgáltatásalkalmazás helyben futtatni, vagy a projekt közzététele a felhőalapú alkalmazás szolgáltatás mobilalkalmazásban.  

Mobil funkciók egy meglévő projekten szeretne hozzáadni, [Töltse le és a SDK inicializálni](#install-sdk) szakaszában olvashat.

### <a name="create-a-net-backend-using-the-azure-portal"></a>Hozzon létre egy .NET kódmentes az Azure portál használatával

Hozzon létre egy alkalmazás szolgáltatás mobil kódmentes, vagy hajtsa végre az [oktatóprogram quickstart útmutató] [ 3] , vagy kövesse az alábbi lépéseket:

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service-classic](../../includes/app-service-mobile-dotnet-backend-create-new-service-classic.md)]

Vissza az _első lépések_ lap **API a tábla létrehozása** **C#** , válassza a **Kódmentes nyelvet**. Kattintson a **Letöltés**gombra, és bontsa ki a tömörített project-fájlokat a helyi számítógépre a Visual Studio alkalmazásban nyissa meg azt a megoldást.

### <a name="create-a-net-backend-using-visual-studio-2013-and-visual-studio-2015"></a>A Visual Studio 2013 és a Visual Studio 2015 .NET kódmentes létrehozása

Telepítse az [Azure SDK a .NET rendszerhez] [ 4] (verzió 2.9.0 vagy újabb) az Azure Mobile-alkalmazások projekteket létrehozni a Visual Studio. Miután telepítette a SDK csomagjában talál, az alábbi lépésekkel ASP.NET alkalmazás létrehozása:

1. Nyissa meg az **Új projekt** párbeszédpanelt ( *fájlból* > **Új** > **Project...**).
2. Bontsa ki a **sablonok** > **Visual C#**, és válassza **webhely**.
3. Jelölje ki a **ASP.NET webalkalmazást**.
4. Adja meg a projekt nevét. Kattintson **az OK**gombra.
5. A _ASP.NET 4.5.2 sablonok_, jelölje be az **Azure Mobile alkalmazásban**. Jelölje be **a felhőben Host** egy mobil kódmentes létrehozása a felhőben, amelyhez tegye közzé a projektet.
6. Kattintson az **OK gombra**.

## <a name="install-sdk"></a>Útmutató: Töltse le és a SDK inicializálni

A SDK [NuGet.org]érhető el. A csomag magában foglalja a első lépésiről a SDK szükséges alapvető funkcionalitást. A SDK inicializálni kell a **HttpConfiguration** objektum műveleteket.

### <a name="install-the-sdk"></a>A SDK telepítése

A SDK telepítéséhez kattintson a jobb gombbal a kiszolgáló projekten, a Visual Studióban, válassza a **NuGet csomagok kezelése**, keresse meg a [Microsoft.Azure.Mobile.Server] csomagot, majd kattintson a **telepítés**gombra.

###<a name="server-project-setup"></a>A kiszolgáló projekt inicializálni

.NET kódmentes kiszolgáló projekt más ASP.NET projektek hasonló van inicializálni egy OWIN indítási osztály felvételével. Győződjön meg arról, hogy hivatkozik-e van a NuGet csomag `Microsoft.Owin.Host.SystemWeb`. Az osztály a Visual Studióban hozzáadásához kattintson a jobb gombbal a kiszolgáló projekten, és válassza a **Hozzáadás** > 
**Új elemet**, majd a **webhely** > **Általános** > **OWIN indítási osztály**.  A következő attribútummal osztály jönnek létre:

    [assembly: OwinStartup(typeof(YourServiceName.YourStartupClassName))]

Az a `Configuration()` módszer a OWIN indítási osztály **HttpConfiguration** objektum segítségével a Azure Mobile-alkalmazások környezet beállításához.
A következő példa előkészíti a kiszolgáló projekt nincsenek hozzáadott funkciók:

    // in OWIN startup class
    public void Configuration(IAppBuilder app)
    {
        HttpConfiguration config = new HttpConfiguration();

        new MobileAppConfiguration()
            // no added features
            .ApplyTo(config);

        app.UseWebApi(config);
    }

Ahhoz, hogy egyes szolgáltatások, meg kell hívni bővítmény módszerek a **MobileAppConfiguration** objektum **ApplyTo**hívása előtt. Ha például a következő kódot ad az alapértelmezett útvonalak attribútuma API-vezérlő `[MobileAppController]` inicializálni során:

    new MobileAppConfiguration()
        .MapApiControllers()
        .ApplyTo(config);

A kiszolgáló quickstart útmutató az Azure portálról **UseDefaultConfiguration()**hívások. Ez az alábbi beállítások egyenértékű:

        new MobileAppConfiguration()
            .AddMobileAppHomeController()             // from the Home package
            .MapApiControllers()
            .AddTables(                               // from the Tables package
                new MobileAppTableConfiguration()
                    .MapTableControllers()
                    .AddEntityFramework()             // from the Entity package
                )
            .AddPushNotifications()                   // from the Notifications package
            .MapLegacyCrossDomainController()         // from the CrossDomain package
            .ApplyTo(config);

A bővítmény használt módszereket használhatja:

* `AddMobileAppHomeController()`Azure Mobile-alkalmazások alapértelmezett kezdőlapja biztosít.
* `MapApiControllers()`WebAPI Vezérlők díszítve egyéni API-funkciókat kínál a `[MobileAppController]` attribútum.
* `AddTables()`a megfeleltetés tartalmaz a `/tables` táblázat vezérlők végpontok.
* `AddTablesWithEntityFramework()`rövid a leképezéshez kéz alakúra van a `/tables` használó személy keretrendszer végpontok vezérlők alapján.
* `AddPushNotifications()`az eszközök regisztrálása az értesítési hubok egyszerű módszert kínál.
* `MapLegacyCrossDomainController()`szabványos CORS fejlécek nyújt a helyi fejlesztését.

### <a name="sdk-extensions"></a>SDK bővítmények

A következő NuGet-alapú bővítmény csomagokban adja meg az alkalmazás által használt különféle mobil funkciókat. Bővítmények engedélyezzen inicializálni a **MobileAppConfiguration** objektum használatával.

- [Microsoft.Azure.Mobile.Server.Quickstart] Mobile-alkalmazások alapbeállítása támogatja. A konfiguráció hozzá a **UseDefaultConfiguration** bővítmény metódus meghívása inicializálni során. Erre a mellékre tartalmazza a következő bővítmények: értesítések, a hitelesítés, a entitás, a táblázatok,-tartományok és az otthoni csomagok. A csomag a Mobile alkalmazások quickstart útmutató érhető el az Azure Portal által használt.

- [Microsoft.Azure.Mobile.Server.Home](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Home/) 
   alkalmazza az alapértelmezett *mobile-alkalmazás már működik lap* a legfelső szintű webhelyén. Hívja fel a   **AddMobileAppHomeController** kiterjesztése mód hozzáadása a konfiguráción.

- [Microsoft.Azure.Mobile.Server.Tables](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Tables/) 
   adatkezelés az osztályok tartalmaz, és a kifejezéskészletek felfelé az adatok folyamat. Hívja fel a **AddTables** kiterjesztése mód hozzáadása a konfiguráción.

- [Microsoft.Azure.Mobile.Server.Entity](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Entity/) 
   lehetővé teszi, hogy a szervezet keretrendszer az SQL-adatbázis az access-adatok. Hívja fel a **AddTablesWithEntityFramework** kiterjesztése mód hozzáadása a konfiguráción.

- [Microsoft.Azure.Mobile.Server.Authentication] lehetővé teszi, hogy a hitelesítési és a tokenek érvényesítéséhez használt OWIN köztes készletek felfelé. A konfiguráció hozzáadása hívja fel a **AddAppServiceAuthentication**  
   és **IAppBuilder**. **UseAppServiceAuthentication** bővítmény módszereket.

- [Microsoft.Azure.Mobile.Server.Notifications] lehetővé teszi a leküldéses értesítések, és a leküldéses regisztrációs végpont határozza meg. Hívja fel a **AddPushNotifications** kiterjesztése mód hozzáadása a konfiguráción.

- [Microsoft.Azure.Mobile.Server.CrossDomain](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.CrossDomain/) 
   létrehoz egy vezérlő, amely a régi böngészők adatok szolgál a Mobile alkalmazásból. Hívja fel a   **MapLegacyCrossDomainController** kiterjesztése mód hozzáadása a konfiguráción.

- [Microsoft.Azure.Mobile.Server.Login] a AppServiceLoginHandler.CreateToken() módszert, amely során az egyéni hitelesítési esetek statikus módszer biztosít.   

## <a name="publish-server-project"></a>Útmutató: a projekt közzététele

Ez a szakasz bemutatja, hogyan teheti közzé a Visual Studio .NET kódmentes projektjét. A kódmentes project mely számjegy vagy [Azure alkalmazás szolgáltatás üzembe helyezési dokumentációja](../app-service-web/web-sites-deploy.md)szabályozott az más módszerekkel is telepítheti.

1. A Visual Studióban újbóli létrehozása a projekt NuGet csomagok visszaállításához.

2. A megoldás Intézőben kattintson a jobb gombbal a projektet, kattintson a **Közzététel**gombra. Először teszi közzé kell közzétételi profil határozza meg. Ha már van egy definiált profilt, jelölje ki azt, és kattintson a **Közzététel**gombra.

2. Ha szükséges, jelölje be a cél közzététel, kattintson **A Microsoft Azure alkalmazás szolgáltatás** > **következő**, (ha szükséges), majd jelentkezzen be a Azure hitelesítő adatait. 
   Visual Studio letöltések és biztonságos módon tárolja a közzétételi beállításokat közvetlenül az Azure.

    ![](./media/app-service-mobile-dotnet-backend-how-to-use-server-sdk/publish-wizard-1.png)

3. Válassza az **előfizetés**lehetőséget, jelölje ki az **Erőforrás típusa** **nézetből**, bontsa ki a **Mobile alkalmazást**, kattintson a mobilalkalmazás kódmentes, majd kattintson az **OK gombra**.

    ![](./media/app-service-mobile-dotnet-backend-how-to-use-server-sdk/publish-wizard-2.png)

4. Ellenőrizze a közzététel profil adatait, majd kattintson a **Közzététel**gombra.

    ![](./media/app-service-mobile-dotnet-backend-how-to-use-server-sdk/publish-wizard-3.png)

    A mobilalkalmazás kódmentes közzétételét sikeresen jelenik a céloldal sikeres jelző.

    ![](./media/app-service-mobile-dotnet-backend-how-to-use-server-sdk/publish-success.png)

##<a name="define-table-controller"></a>Útmutató: a táblázat vezérlő meghatározása

Adja meg egy táblázat vezérlő kattintva jelenítse meg az SQL táblázattá mobiltelefonos ügyfélalkalmazások.  Egy táblázat vezérlő konfigurálása három lépésből áll:

1. Hozzon létre egy adatok átvitele objektum (DTO) osztály.
2. Állítsa be a mobil DbContext osztály táblázathivatkozásokban.
3. Hozzon létre egy táblázatot vezérlő.

Egy adatok átvitele objektum (DTO) egy egyszerű C# objektum öröklő `EntityData`.  Példa:

    public class TodoItem : EntityData
    {
        public string Text { get; set; }
        public bool Complete {get; set;}
    }

Az SQL-adatbázis táblázatát meghatározása a DTO szolgál.  Az adatbázis-bejegyzés létrehozása, vegyen fel egy `DbSet<>` tulajdonságot szeretné használni a DbContext.  Az Azure Mobile-alkalmazások alapértelmezett projektsablon a DbContext neve `Models\MobileServiceContext.cs`:

    public class MobileServiceContext : DbContext
    {
        private const string connectionStringName = "Name=MS_TableConnectionString";

        public MobileServiceContext() : base(connectionStringName)
        {

        }

        public DbSet<TodoItem> TodoItems { get; set; }

        protected override void OnModelCreating(DbModelBuilder modelBuilder)
        {
            modelBuilder.Conventions.Add(
                new AttributeToColumnAnnotationConvention<TableColumnAttribute, string>(
                    "ServiceColumnTable", (property, attributes) => attributes.Single().ColumnType.ToString()));
        }
    }

Ha az Azure SDK telepítve van, most már létrehozhat egy sablon táblázat vezérlő az alábbi képlettel történik:

1. Kattintson a jobb gombbal a vezérlők mappára, és válassza a **Hozzáadás** > **... vezérlő**.
2. Válassza az **Azure Mobile alkalmazások táblázat vezérlő** lehetőséget, majd kattintson a **Hozzáadás**gombra.
3. A **Vezérlő hozzáadása** párbeszédpanelen:
    * A **modell osztályához** tartalmazó legördülő listára válassza az új DTO.
    * A **DbContext** tartalmazó legördülő listára válassza a Mobile Service DbContext osztály.
    * A vezérlő neve akkor jön létre.
4. Kattintson a **hozzáadása**gombra.

A quickstart útmutató kiszolgáló projekt példa egy egyszerű **TodoItemController**tartalmazza.

### <a name="how-to-adjust-the-table-paging-size"></a>Útmutató: a táblázat lapozási méretének módosítása

Alapértelmezés szerint az Azure Mobile-alkalmazások egy kérelemre 50 rekordokat adja eredményül.  Lapozás biztosítja, hogy az ügyfél nem kötik a felhasználói felület szál, sem a kiszolgálón túl sokáig be egy jó felhasználói felület biztosítása. A táblázat lapozási méretének módosításához növelje a kiszolgálóoldalon "lekérdezés méret engedélyezett" és az ügyféloldali oldalméret kiszolgálóoldali "lekérdezés méret engedélyezett" módosul, használja a `EnableQuery` attribútum:

    [EnableQuery(PageSize = 500)]

Győződjön meg róla a pageSize értékének azonos vagy nagyobb, mint az ügyfél által kért méretét.  Keresse meg az adott ügyfél útmutató dokumentációjában az ügyfél Oldalméret módosítása.

## <a name="how-to-define-a-custom-api-controller"></a>Útmutató: egy egyéni API-vezérlő meghatározása

Az egyéni API-vezérlő biztosít a mobilalkalmazás kódmentes legalapvetőbb funkciók zárólap ki. Rögzítheti egy, a [MobileAppController] attribútummal mobilspecifikus API-vezérlő. A `MobileAppController` attribútum regisztrál az útvonal, beállítja a Mobile alkalmazások JSON szerializáló, és bekapcsolja az [ügyfél verzió ellenőrzése](app-service-mobile-client-and-server-versioning.md).

1. A Visual Studióban, kattintson a jobb gombbal a vezérlők mappára, majd kattintson a **Hozzáadás** > **vezérlő**, a kijelölés **webes API 2 vezérlő&mdash;üres** , és kattintson a **Hozzáadás**gombra.

2. **Vezérlő neve**, például: ellátási `CustomController`, és kattintson a **Hozzáadás**gombra.

3. A vezérlő új osztályfájl adja hozzá a következő utasítással:

        using Microsoft.Azure.Mobile.Server.Config;

4. A API vezérlő osztály-definíciót, ahogy az alábbi példa a **[MobileAppController]** attribútum vonatkoznak:

        [MobileAppController]
        public class CustomController : ApiController
        {
              //...
        }

4. App_Start/Startup.MobileApp.cs fájlban adja hozzá a hívás a **MapApiControllers** kiterjesztés módszerrel, ahogy a következő példában:

        new MobileAppConfiguration()
            .MapApiControllers()
            .ApplyTo(config);

Is használhatja a `UseDefaultConfiguration()` bővítmény módszer `MapApiControllers()`. Bármely vezérlő nincs telepítve a **MobileAppControllerAttribute** alkalmazott továbbra is elérhető az ügyfelek, de azt nem megfelelően felhasználhatók bármely mobilalkalmazás ügyfélprogram SDK ügyfelek.

## <a name="how-to-work-with-authentication"></a>Útmutató: hitelesítés használata

Azure Mobile-alkalmazások alkalmazás szolgáltatás hitelesítést használ / biztonságossá tétele a mobil kódmentes engedélyt.  Ez a szakasz mutatja be a következő feladatokat hitelesítési kapcsolatos a .NET kódmentes kiszolgáló projektben:

+ [Útmutató: hitelesítés hozzáadása kiszolgáló projekthez](#add-auth)
+ [Útmutató: az alkalmazás egyéni hitelesítés használata](#custom-auth)
+ [Útmutató: lekérés hitelesített felhasználói adatok](#user-info)
+ [Útmutató: adatok elérése a jogosult felhasználók korlátozása](#authorize)

### <a name="add-auth"></a>Útmutató: hitelesítés hozzáadása kiszolgáló projekthez

A kiszolgáló projekt hitelesítési vehet bővítése a **MobileAppConfiguration** objektumra, és OWIN köztes konfigurálásával. Ha a [Microsoft.Azure.Mobile.Server.Quickstart] csomag telepítéséhez, és hívja fel a **UseDefaultConfiguration** kiterjesztése mód, kihagyhatja a 3.

1. A Visual Studióban a [Microsoft.Azure.Mobile.Server.Authentication] csomag telepítéséhez.

2. A Startup.cs projektfájlba adja hozzá a következő sort a kód a **konfigurációs** módszer az elején:

        app.UseAppServiceAuthentication(config);

    Köztes OWIN összetevő ellenőrzi a társított átjáró alkalmazás szolgáltatás által kibocsátott tokenek.

3. Adja hozzá a `[Authorize]` attribútumot minden vezérlő vagy megadott hitelesítést igényel lehetőséget. 

Kapcsolatban további tudnivalókat a Mobile-alkalmazások kódmentes ügyfelek hitelesítést végezni, lásd: [az alkalmazás hozzáadása a hitelesítés](app-service-mobile-ios-get-started-users.md).

### <a name="custom-auth"></a>Útmutató: az alkalmazás egyéni hitelesítés használata

Ha nem szeretné használni egy alkalmazás szolgáltatás hitelesítési és engedélyezési szolgáltatónál, saját bejelentkezési rendszert alkalmazhat. Telepítse a hitelesítési jogkivonat előállítása megkönnyítése [Microsoft.Azure.Mobile.Server.Login] csomagot.  A saját kód szükséges felhasználói hitelesítő adatok érvényesítése. Érdemes lehet például sózott és kivonatolt jelszavak adatbázisban szemben. Az alábbi példában a `isValidAssertion()` (definiált máshol) módszer e ellenőrzésért felelős.

Az egyéni hitelesítési van téve egy ApiController létrehozásával, és ki `register` és `login` műveletek. Az ügyfél az adatokat gyűjt a felhasználó egy egyéni felhasználói felületi kell használnia.  A szokásos HTTP POST hívást az API-nak majd elküldése az adatokat. A kiszolgáló ellenőrzi a állítás, miután egy token kibocsátott használata a `AppServiceLoginHandler.CreateToken()` módot.  A ApiController **nem** használja az `[MobileAppController]` attribútum. 

Példa `login` művelet:

        public IHttpActionResult Post([FromBody] JObject assertion)
        {
            if (isValidAssertion(assertion)) // user-defined function, checks against a database
            {
                JwtSecurityToken token = AppServiceLoginHandler.CreateToken(new Claim[] { new Claim(JwtRegisteredClaimNames.Sub, assertion["username"]) },
                    mySigningKey,
                    myAppURL,
                    myAppURL,
                    TimeSpan.FromHours(24) );
                return Ok(new LoginResult()
                {
                    AuthenticationToken = token.RawData,
                    User = new LoginResultUser() { UserId = userName.ToString() }
                });
            }
            else // user assertion was not valid
            {
                return this.Request.CreateUnauthorizedResponse();
            }
        }

Az előző példában LoginResult és LoginResultUser is ki a kötelező tulajdonságok szerializálható objektumokat. Az ügyfél JSON objektumként az űrlap eredményül adott válaszokat vár:

        {
            "authenticationToken": "<token>",
            "user": {
                "userId": "<userId>"
            }
        }

A `AppServiceLoginHandler.CreateToken()` módszer foglal magában a _közönség_ és egy _kibocsátó_ paramétert. Mindkét paraméter URL-CÍMÉT az alkalmazás legfelső szintű, a HTTPS-séma vannak beállítva. Hasonlóképpen _secretKey_ lesz, az érték az alkalmazás által aláírási kulcs beállíthatja. Az aláírási kulcs az ügyfél nem osztja fel, Mentaízű kulcsok és a felhasználók megszemélyesítés szolgál. Az aláírási billentyűt, miközben App szolgáltatásban arra hivatkozó által üzemeltetett szerezhet a _webhely\_AUTH\_aláírási\_kulcs_ környezeti. Ha szükséges, a helyi hibakeresési környezetben, kövesse a beolvasni a kulcsot, és képfájlba egy alkalmazás beállítása a [helyi hitelesítéssel hibakeresési](#local-debug) című szakaszt.

A kibocsátott jogkivonat is tartalmazhat, más igények és a lejárati ideje.  A kibocsátott jogkivonat minimálisan, tárgy (**sub**) igényt kell tartalmaznia.

Akkor támogatniuk kell a szabványos ügyfél `loginAsync()` módszer a hitelesítési útvonal túlterhelés.  Ha az ügyfél felhívja `client.loginAsync('custom');` bejelentkezhetne, az útvonal kell `/.auth/login/custom`.  Beállíthatja, hogy az egyéni hitelesítési vezérlő használatához az útvonal `MapHttpRoute()`:

    config.Routes.MapHttpRoute("custom", ".auth/login/custom", new { controller = "CustomAuth" });

>[AZURE.TIP] Használja a `loginAsync()` megközelítés biztosítja, hogy a hitelesítési jogkivonat csatolva van a szolgáltatás minden későbbi hívás.

###<a name="user-info"></a>Útmutató: lekérés hitelesített felhasználói adatok

Amikor egy felhasználó alkalmazás szolgáltatás által hitelesítése, tudja elérni a hozzárendelt felhasználói Azonosítójával és egyéb adatok megadása a .NET kódmentes kódot. A felhasználói adatok a kódmentes engedélyezési döntések használható. A következő kódot beolvassa a kérést társított Felhasználóazonosító:

    // Get the SID of the current user.
    var claimsPrincipal = this.User as ClaimsPrincipal;
    string sid = claimsPrincipal.FindFirst(ClaimTypes.NameIdentifier).Value;

A biztonsági AZONOSÍTÓK szolgáltatófüggő Felhasználóazonosító származik, és egy adott felhasználó és a bejelentkezés-szolgáltató statikus.  A biztonsági azonosító értéke null érvénytelen hitelesítési tokenek.

Alkalmazás szolgáltatás is lehetővé teszi az adott követelések kérése a bejelentkezési szolgáltató. Minden egyes identitásszolgáltató használata az identitásszolgáltató SDK csomagjában talál további információt tartalmaznak.  Ha például barátok információt a Facebook-grafikon API is használhatja.  A szolgáltató lap az Azure-portálon is megadhat igényelt követelések. Néhány követelések az identitásszolgáltatójával további beállításokat igényel.

A következő kódot hívások beszerzése a bejelentkezési hitelesítő adatait, amelyek tartalmazzák a szükséges kéréseket szemben a Facebook-grafikon API jogkivonat **GetAppServiceIdentityAsync** bővítmény módszer:

    // Get the credentials for the logged-in user.
    var credentials =
        await this.User
        .GetAppServiceIdentityAsync<FacebookCredentials>(this.Request);

    if (credentials.Provider == "Facebook")
    {
        // Create a query string with the Facebook access token.
        var fbRequestUrl = "https://graph.facebook.com/me/feed?access_token="
            + credentials.AccessToken;

        // Create an HttpClient request.
        var client = new System.Net.Http.HttpClient();

        // Request the current user info from Facebook.
        var resp = await client.GetAsync(fbRequestUrl);
        resp.EnsureSuccessStatusCode();

        // Do something here with the Facebook user information.
        var fbInfo = await resp.Content.ReadAsStringAsync();
    }

Hozzá egy nyilatkozata `System.Security.Principal` a **GetAppServiceIdentityAsync** kiterjesztése mód számára.

### <a name="authorize"></a>Útmutató: adatok elérése a jogosult felhasználók korlátozása

Az előző szakaszban beolvasásához a felhasználói Azonosítójával egy hitelesített felhasználó hogyan mutatott azt. Elérésének korlátozásához adatok és más erőforrások: Ez az érték alapján. Például a felhasználói azonosítóval oszlop hozzáadása táblázatokhoz és -szűrés a lekérdezés eredményében a felhasználói azonosító is egyszerűen csak az engedélyezett felhasználók visszaadott adatok körének korlátozását. A következő kódot adatok sorát adja vissza, csak akkor, ha a biztonsági AZONOSÍTÓK megegyezik a TodoItem táblázatban a felhasználóazonosító oszlop értéke:

    // Get the SID of the current user.
    var claimsPrincipal = this.User as ClaimsPrincipal;
    string sid = claimsPrincipal.FindFirst(ClaimTypes.NameIdentifier).Value;

    // Only return data rows that belong to the current user.
    return Query().Where(t => t.UserId == sid);

A `Query()` módszer eredménye egy `IQueryable` , hogy miként kezelje a szűrés LINQ kezelhetők.

## <a name="how-to-add-push-notifications-to-a-server-project"></a>Útmutató: Adja hozzá a leküldéses értesítések kiszolgáló projektté

Leküldéses értesítések hozzáadása a kiszolgáló projekt bővítése a **MobileAppConfiguration** objektumra, és egy értesítés hubok ügyfél létrehozása.

1. A Visual Studióban, kattintson a jobb gombbal a kiszolgáló projekt, és kattintson a **NuGet csomagok kezelése**, kereshet `Microsoft.Azure.Mobile.Server.Notifications`, majd kattintson a **telepítés**gombra. 

2. Ismételje meg ezt a lépést, telepítheti az `Microsoft.Azure.NotificationHubs` csomag, amelyek tartalmazzák még a értesítés hubok ügyfél tárat.

3. A App_Start/Startup.MobileApp.cs és vegyen fel a **AddPushNotifications()** bővítmény módszerrel hívás közben inicializálni:

        new MobileAppConfiguration()
            // other features...
            .AddPushNotifications()
            .ApplyTo(config);

4. Adja hozzá a következő kódot, amely egy értesítés hubok ügyfél hoz létre:

        // Get the settings for the server project.
        HttpConfiguration config = this.Configuration;
        MobileAppSettingsDictionary settings =
            config.GetMobileAppSettingsProvider().GetMobileAppSettings();

        // Get the Notification Hubs credentials for the Mobile App.
        string notificationHubName = settings.NotificationHubName;
        string notificationHubConnection = settings
            .Connections[MobileAppSettingsKeys.NotificationHubConnectionString].ConnectionString;

        // Create a new Notification Hub client.
        NotificationHubClient hub = NotificationHubClient
            .CreateClientFromConnectionString(notificationHubConnection, notificationHubName);

Most már használhatja az értesítési hubok ügyfél leküldéses értesítéseket küldeni bejegyzett eszközök. További tudnivalókért olvassa el a [Hozzáadás leküldéses értesítéseket, hogy az alkalmazás](app-service-mobile-ios-get-started-push.md)című témakört. Többet szeretne tudni az értesítési hubok, [Értesítés hubok – áttekintés](../notification-hubs/notification-hubs-push-notification-overview.md)című témakör.

##<a name="tags"></a>Útmutató: engedélyezése célzott leküldéses-címkék használata

Értesítés hubok célzott értesítéseket küldeni adott regisztrációk címkék használatával teszi lehetővé. Több címkék automatikusan jönnek létre:

* A Telepítésazonosítót azonosítja egy bizonyos eszközt.
* A felhasználói azonosító, a hitelesített biztonsági AZONOSÍTÓK alapján egy adott felhasználó azonosítja.

A Telepítésazonosítót webböngészőn keresztül elérhetők a **MobileServiceClient** **végrehajtott** tulajdonsága.  A következő példa bemutatja, hogyan telepítési azonosító segítségével címke felvétele egy adott telepítése az értesítési hubok:

    hub.PatchInstallation("my-installation-id", new[]
    {
        new PartialUpdateOperation
        {
            Operation = UpdateOperationType.Add,
            Path = "/tags",
            Value = "{my-tag}"
        }
    });

A telepítés létrehozásakor a kódmentes figyelmen kívül hagyja a be leküldéses értesítést regisztráció során az ügyfél által megadott címkéket. Ahhoz, hogy az ügyfél megcímkézése a telepítést, létre kell hoznia egy egyéni API-val, hogy hozzáadja a fenti minta használatával. 

Lásd: [ügyfél hozzáadott leküldéses értesítést címkék] [ 5] a alkalmazás szolgáltatás Mobile-alkalmazások bejegyzett quickstart útmutató minta példát.

##<a name="push-user"></a>Útmutató: küldés leküldéses értesítéseket egy hitelesített felhasználó számára

Ha egy hitelesített felhasználó regisztrálja a leküldéses értesítéseket, felhasználói azonosító címke automatikusan felkerül a regisztráció. Címke használatával az adott személy által regisztrált összes eszközök elküldheti a leküldéses értesítéseket. A következő kódot a kérés felhasználó biztonsági azonosítója kap, és minden eszköz regisztrációs az illető személyhez sablon leküldéses értesítést küld:

    // Get the current user SID and create a tag for the current user.
    var claimsPrincipal = this.User as ClaimsPrincipal;
    string sid = claimsPrincipal.FindFirst(ClaimTypes.NameIdentifier).Value;
    string userTag = "_UserId:" + sid;

    // Build a dictionary for the template with the item message text.
    var notification = new Dictionary<string, string> { { "message", item.Text } };

    // Send a template notification to the user ID.
    await hub.SendTemplateNotificationAsync(notification, userTag);

Leküldéses értesítések egy hitelesített ügyfélprogramból regisztrálásakor győződjön meg róla, hogy a hitelesítés teljes regisztrációs előtt. További tudnivalókért lásd: a [leküldéses felhasználók] [ 6] .NET kódmentes alkalmazás szolgáltatás Mobile-alkalmazások bejegyzett quickstart útmutató mintában szereplő.

## <a name="how-to-debug-and-troubleshoot-the-net-server-sdk"></a>Útmutató: hibakeresési és a .NET-kiszolgáló SDK – problémamegoldás

Azure alkalmazás szolgáltatása több hibakeresési és hibaelhárítási ASP.NET-alkalmazások technikák:

- [Az alkalmazás Azure szolgáltatás figyelése](../app-service-web/web-sites-monitor.md)
- [Azure alkalmazás szolgáltatás a diagnosztikai naplózás engedélyezéséhez](../app-service-web/web-sites-enable-diagnostic-log.md)
- [A Visual Studióban az Azure alkalmazás szolgáltatásainak – problémamegoldás](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md)

### <a name="logging"></a>Naplózás

A diagnosztikai naplók alkalmazás szolgáltatás írni használja a szabványos ASP.NET nyomkövetési írni. A naplók megírása előtt engedélyeznie kell diagnosztika a mobilalkalmazás kódmentes a.

Diagnosztikai engedélyezheti, és a naplókat írni:

1. Kövesse a [Diagnosztika engedélyezése](../app-service-web/web-sites-enable-diagnostic-log.md#enablediag).

2. Adja hozzá a következő utasítással a kód fájl:

        using System.Web.Http.Tracing;

3. Hozzon létre egy nyomkövetési író írják a .NET kódmentes a diagnosztikai naplók, hogy az alábbi képlettel történik:

        ITraceWriter traceWriter = this.Configuration.Services.GetTraceWriter();
        traceWriter.Info("Hello, World");

4. Ismételt közzététele a kiszolgálón projekt, és elérheti a mobilalkalmazás kódmentes végrehajtani a kód elérési utat a naplózás.

5. Töltse le és a naplókat, felmérése, ismertetett módon [hogyan: Töltse le a naplók](../app-service-web/web-sites-enable-diagnostic-log.md#download).

### <a name="local-debug"></a>Helyi hitelesítéssel hibakeresése

Az alkalmazás helyileg módosítások ellenőrzéséhez a felhőbe közzététel előtt futtathatók. A legtöbb Azure Mobile-alkalmazások háttérkiszolgálókon nyomja le az *F5 billentyűt* a Visual Studióban. Vannak azonban olyan néhány további szempontokat, amikor a hitelesítés használatával.

Felhőalapú mobilalkalmazásban az alkalmazás szolgáltatás hitelesítési/engedélyezési konfigurálni kell rendelkeznie, és az ügyfelek a felhőben végpontot, a bejelentkezési alternatív állomáswebhelyén megadott kell rendelkeznie. A megadott lépéseket az ügyfél platform dokumentációjában.

Győződjön meg arról, hogy a mobil kódmentes [Microsoft.Azure.Mobile.Server.Authentication] telepítve. Ezután az alkalmazás OWIN indítási osztály, helyezzen el a következő után `MobileAppConfiguration` alkalmaz a `HttpConfiguration`:

        app.UseAppServiceAuthentication(new AppServiceAuthenticationOptions()
        {
            SigningKey = ConfigurationManager.AppSettings["authSigningKey"],
            ValidAudiences = new[] { ConfigurationManager.AppSettings["authAudience"] },
            ValidIssuers = new[] { ConfigurationManager.AppSettings["authIssuer"] },
            TokenHandler = config.GetAppServiceTokenHandler()
        });

Az előző példában konfigurálnia kell a _authAudience_ és _authIssuer_ Alkalmazásbeállítások a Web.config fájlban minden egyes URL-CÍMÉT az alkalmazás legfelső szintű, a HTTPS-séma kell. Hasonlóképpen _authSigningKey_ lesz, az érték az alkalmazás által aláírási kulcs beállíthatja. Az aláírási kulcs beszerzése:

1. Nyissa meg az alkalmazást az [Azure portál] belül 
2. Kattintson az **eszközök**menü **Kudu**, **lépjen**.
3. A Kudu Management-webhelyen kattintson a **környezetben**.
4. Keresse meg az érték _webhely\_AUTH\_aláírási\_kulcs_. 

Az aláírási kulcs használata a helyi alkalmazás config _authSigningKey_ paraméter.  A mobil kódmentes most fel van szerelve tokenek fut helyi meghajtóra, amely az ügyfél beolvassa a token át a felhőalapú végpont érvényesítéséhez.

[1]: https://msdn.microsoft.com/library/azure/dn961176.aspx
[2]: https://github.com/Azure/azure-mobile-apps-net-server
[3]: app-service-mobile-ios-get-started.md
[4]: https://azure.microsoft.com/downloads/
[5]: https://github.com/Azure-Samples/app-service-mobile-dotnet-backend-quickstart/blob/master/README.md#client-added-push-notification-tags
[6]: https://github.com/Azure-Samples/app-service-mobile-dotnet-backend-quickstart/blob/master/README.md#push-to-users
[Azure portál]: https://portal.azure.com
[NuGet.org]: http://www.nuget.org/
[Microsoft.Azure.Mobile.Server]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/
[Microsoft.Azure.Mobile.Server.Quickstart]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Quickstart/
[Microsoft.Azure.Mobile.Server.Authentication]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Authentication/
[Microsoft.Azure.Mobile.Server.Login]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Login/
[Microsoft.Azure.Mobile.Server.Notifications]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Notifications/
[MapHttpAttributeRoutes]: https://msdn.microsoft.com/library/dn479134(v=vs.118).aspx

