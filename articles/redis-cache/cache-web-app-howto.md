<properties 
    pageTitle="Egy webalkalmazás létrehozása vgx.dll gyorsítótár |} Microsoft Azure" 
    description="Megtudhatja, hogy miként készíthet webalkalmazást vgx.dll gyorsítótár" 
    services="redis-cache" 
    documentationCenter="" 
    authors="steved0x" 
    manager="douge" 
    editor=""/>

<tags 
    ms.service="cache" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="cache-redis" 
    ms.devlang="na" 
    ms.topic="hero-article" 
    ms.date="10/11/2016" 
    ms.author="sdanie"/>

# <a name="how-to-create-a-web-app-with-redis-cache"></a>Gyorsítótár vgx.dll webalkalmazást létrehozása

> [AZURE.SELECTOR]
- [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
- [ASP.NET](cache-web-app-howto.md)
- [NODE.js](cache-nodejs-get-started.md)
- [Java](cache-java-get-started.md)
- [Python](cache-python-get-started.md)

Ebben az oktatóanyagban mutatja készíthetnek és helyezhetnek üzembe a web App alkalmazás szolgáltatásban Azure ASP.NET-webalkalmazások Visual Studio 2015 használatával. A minta alkalmazás csapat statisztika adatbázisból listáját jeleníti meg, és megjeleníti az Azure vgx.dll gyorsítótár használatának tárolására és az adatok beolvasásához a gyorsítótárból különböző módjai. Az oktatóprogram befejezésekor is, amely beolvassa, és -adatbázishoz, az Azure vgx.dll gyorsítótár optimalizált, és a Azure-ban futó webalkalmazást.

Dióhéjban:

-   Hogyan lehet hozzon létre egy ASP.NET MVC 5 webalkalmazást a Visual Studio.
-   Hogyan lehet access-adatok használata a szervezet keretrendszer adatbázisból.
-   Hogyan javítható fájlmegosztásra tárolásához és Azure vgx.dll gyorsítótár használatával adatok visszakeresése adatbázis betöltés csökkentése.
-   Egy vgx.dll használata beállítása a felső 5 csapatok beolvasásához vannak rendezve.
-   Hogyan hozhatók létre az erőforrás-kezelő sablon használatával alkalmazás Azure erőforrásokat.
-   Hogy miként teheti közzé a Visual Studio segítségével Azure az alkalmazást.

## <a name="prerequisites"></a>Előfeltételek

Az oktatóprogram elvégzéséhez az alábbi előfeltételek kell rendelkeznie.

-   [Azure-fiók](#azure-account)
-   [A .NET rendszerhez az Azure SDK a Visual Studio 2015](#visual-studio-2015-with-the-azure-sdk-for-net)

### <a name="azure-account"></a>Azure-fiók

Az oktatóprogram elvégzéséhez az Azure-fiók szükséges. képes vagy:

* [Nyissa meg az ingyenes Azure-fiók](/pricing/free-trial/?WT.mc_id=redis_cache_hero). Próbálja ki az Azure szolgáltatások fizetett használható jóváírások kap. Után a credits használnak, megtarthatja a fiókot, és ingyenes Azure szolgáltatások és funkciók.
* [Visual Studio aktiválása előfizetői előnyeit](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=redis_cache_hero). Az MSDN-előfizetés lépve credits havonta fizetett Azure szolgáltatások használható.

### <a name="visual-studio-2015-with-the-azure-sdk-for-net"></a>A .NET rendszerhez az Azure SDK a Visual Studio 2015

Az oktatóprogram az [Azure SDK a .NET rendszerhez](../dotnet-sdk.md) 2.8.2 a Visual Studio 2015 írásos vagy újabb verziója. [Töltse le a legújabb Visual Studio 2015 itt Azure SDK](http://go.microsoft.com/fwlink/?linkid=518003). Ha még nincs Visual Studio automatikusan települ a SDK csomagjában talál.

Ha a Visual Studio 2013, azt is megteheti, [Töltse le a legújabb Visual Studio 2013 Azure SDK](http://go.microsoft.com/fwlink/?LinkID=324322). Néhány képernyő eltér az ábrák, ebben az oktatóanyagban látható jelenhet meg.

>[AZURE.NOTE] Attól függően, hogy hány SDK függőség már van a számítógépen a SDK telepítése eltarthat néhány percet fél órán vagy több hosszú ideig.

## <a name="create-the-visual-studio-project"></a>A Visual Studio projekt létrehozása

1. Nyissa meg a Visual Studióban, és kattintson a **fájl**és az **Új** **Projekt**.

2. Bontsa ki a **képi C#** csomópontot a **sablonok** listából, jelölje be a **felhőben**elemre, majd kattintson a **ASP.NET webalkalmazás**lehetőséget. Győződjön meg arról, hogy be van jelölve **a .NET-keretrendszer 4.5.2.** .  **ContosoTeamStats** írja be a **név** mezőben lévő értéket, és kattintson az **OK gombra**.
 
    ![Projekt létrehozása][cache-create-project]

3. Jelölje be a projekt típusa **MVC** . Törölje a jelet **a felhőben Host** jelölőnégyzetből. Az oktatóprogram fogja [az Azure erőforrások kiépítése](#provision-the-azure-resources) , majd a [Közzététel Azure az alkalmazás](#publish-the-application-to-azure) a következő lépéseket. Példa egy alkalmazás szolgáltatás web App alkalmazásban a Visual Studio kiépítési hagyja bejelölve **a felhőben Host** olvassa el a [Azure App szolgáltatásban ASP.NET és a Visual Studio segítségével a Web Apps alkalmazások – első lépések](../app-service-web/web-sites-dotnet-get-started.md)című témakört.

    ![Jelölje ki a project-sablon][cache-select-template]

4. Kattintson az **OK gombra** a projekt létrehozása.

## <a name="create-the-aspnet-mvc-application"></a>ASP.NET MVC-alkalmazás létrehozása

Az oktatóprogram ebben a részben kell létrehoznia az egyszerű alkalmazás, amely beolvassa, és megjeleníti az adatbázisból csapat statisztikát.

-   [A modell hozzáadása](#add-the-model)
-   [A vezérlő hozzáadása](#add-the-controller)
-   [A nézetek konfigurálása](#configure-the-views)

### <a name="add-the-model"></a>A modell hozzáadása

1. Kattintson a jobb gombbal a **Megoldást Explorer** **modellek** , és válassza a **Hozzáadás**, **osztály**. 

    ![Modell hozzáadása][cache-model-add-class]

2. Adja meg `Team` az osztály nevet, és kattintson a **Hozzáadás**gombra.

    ![Adja hozzá a modell osztályához][cache-model-add-class-dialog]

3. Cserélje le a `using` kimutatások tetején a `Team.cs` fájl az alábbi utasítások segítségével.


        using System;
        using System.Collections.Generic;
        using System.Data.Entity;
        using System.Data.Entity.SqlServer;


4. Csere meghatározása a `Team` a következő kódrészletet, amely tartalmazza a frissített osztály `Team` definícióját, valamint bizonyos egyéb entitás keretrendszer segítő osztályok osztály. Ebben az oktatóanyagban használt entitás keretrendszer kód első megoldása a további tudnivalókért lásd: a [kód először egy új adatbázist](https://msdn.microsoft.com/data/jj193542).


        public class Team
        {
            public int ID { get; set; }
            public string Name { get; set; }
            public int Wins { get; set; }
            public int Losses { get; set; }
            public int Ties { get; set; }
        
            static public void PlayGames(IEnumerable<Team> teams)
            {
                // Simple random generation of statistics.
                Random r = new Random();
        
                foreach (var t in teams)
                {
                    t.Wins = r.Next(33);
                    t.Losses = r.Next(33);
                    t.Ties = r.Next(0, 5);
                }
            }
        }
        
        public class TeamContext : DbContext
        {
            public TeamContext()
                : base("TeamContext")
            {
            }
        
            public DbSet<Team> Teams { get; set; }
        }
        
        public class TeamInitializer : CreateDatabaseIfNotExists<TeamContext>
        {
            protected override void Seed(TeamContext context)
            {
                var teams = new List<Team>
                {
                    new Team{Name="Adventure Works Cycles"},
                    new Team{Name="Alpine Ski House"},
                    new Team{Name="Blue Yonder Airlines"},
                    new Team{Name="Coho Vineyard"},
                    new Team{Name="Contoso, Ltd."},
                    new Team{Name="Fabrikam, Inc."},
                    new Team{Name="Lucerne Publishing"},
                    new Team{Name="Northwind Traders"},
                    new Team{Name="Consolidated Messenger"},
                    new Team{Name="Fourth Coffee"},
                    new Team{Name="Graphic Design Institute"},
                    new Team{Name="Nod Publishers"}
                };
        
                Team.PlayGames(teams);
        
                teams.ForEach(t => context.Teams.Add(t));
                context.SaveChanges();
            }
        }
        
        public class TeamConfiguration : DbConfiguration
        {
            public TeamConfiguration()
            {
                SetExecutionStrategy("System.Data.SqlClient", () => new SqlAzureExecutionStrategy());
            }
        }


2. Kattintson duplán a **Megoldást Explorer** **web.config** a megnyitáshoz.

    ![Web.config][cache-web-config]

3.  Adja hozzá a következő kapcsolati karakterláncot az `connectionStrings` szakaszban. A kapcsolati karakterlánc nevét kell egyezzen a entitás keretrendszer adatbázis helyi osztály, amely `TeamContext`.

        <add name="TeamContext" connectionString="Data Source=(LocalDB)\v11.0;AttachDbFilename=|DataDirectory|\Teams.mdf;Integrated Security=True" providerName="System.Data.SqlClient" />


    Miután megadta ezt a `connectionStrings` szakaszban a következő példához hasonlóan kell kinéznie.


        <connectionStrings>
            <add name="DefaultConnection" connectionString="Data Source=(LocalDb)\MSSQLLocalDB;AttachDbFilename=|DataDirectory|\aspnet-ContosoTeamStats-20160216120918.mdf;Initial Catalog=aspnet-ContosoTeamStats-20160216120918;Integrated Security=True"
                providerName="System.Data.SqlClient" />
            <add name="TeamContext" connectionString="Data Source=(LocalDB)\v11.0;AttachDbFilename=|DataDirectory|\Teams.mdf;Integrated Security=True"  providerName="System.Data.SqlClient" />
        </connectionStrings>

### <a name="add-the-controller"></a>A vezérlő hozzáadása

1. Nyomja le az **F6** össze a projekt. 
2. A **Megoldás Explorer**kattintson a jobb gombbal a **vezérlők** mappát, és válassza a **Hozzáadás**, **vezérlő**.

    ![Vezérlő hozzáadása][cache-add-controller]

3. Válassza a **MVC 5 vezérlőhöz személy keretrendszer használatával nézetekkel**, és kattintson a **Hozzáadás**gombra. Ha egy hiba után kattintson a **Hozzáadás**gombra, győződjön meg arról, hogy a projekt először rendelkezik beépített.

    ![Adja hozzá a vezérlő osztály][cache-add-controller-class]

5. A **modell osztályához** legördülő listából válassza ki a **csapat (ContosoTeamStats.Models)** . Az **adatok helyi osztály** legördülő listából válassza ki a **TeamContext (ContosoTeamStats.Models)** . Típus `TeamsController` a a **vezérlő** neve mezőben lévő értéket (Ha ez nem automatikusan kitölti a rendszer). Kattintson a **Hozzáadás** a vezérlő osztályjegyzetfüzet létrehozásához és az alapértelmezett nézeteket.

    ![Vezérlő beállítása][cache-configure-controller]

4. A **Megoldás Explorer**bontsa ki a **Global.asax** , és **Global.asax.cs** a megnyitáshoz kattintson duplán.

    ![Global.asax.cs][cache-global-asax]

5. Adja hozzá a következő két kimutatások használatával az egyéb csoportjában a fájl tetején kimutatások használatával.


        using System.Data.Entity;
        using ContosoTeamStats.Models;


6. A következő sort a kód hozzáadása a végén a `Application_Start` módot.


        Database.SetInitializer<TeamContext>(new TeamInitializer());


7. A **Megoldás Explorer**bontsa ki a `App_Start` kattintson duplán a `RouteConfig.cs`.

    ![RouteConfig.cs][cache-RouteConfig-cs]

8. Csere `controller = "Home"` az a következő kódrészlet a `RegisterRoutes` metódussal `controller = "Teams"` az alábbi példában látható módon.


        routes.MapRoute(
            name: "Default",
            url: "{controller}/{action}/{id}",
            defaults: new { controller = "Teams", action = "Index", id = UrlParameter.Optional }
        );


### <a name="configure-the-views"></a>A nézetek konfigurálása

1. A **Megoldás Explorer**bontsa ki a **nézetek** mappára, majd a **megosztott** mappára, és kattintson duplán a **_Layout.cshtml**. 

    ![_Layout.cshtml][cache-layout-cshtml]

2. Tartalmának módosítása a `title` elem és a csere `My ASP.NET Application` a `Contoso Team Stats` az alábbi példában látható módon.


        <title>@ViewBag.Title - Contoso Team Stats</title>


3. Az a `body` szakaszban, frissítse az első `Html.ActionLink` utasítás és a csere `Application name` a `Contoso Team Stats` és cseréje `Home` a `Teams`.
    -   Előtt:`@Html.ActionLink("Application name", "Index", "Home", new { area = "" }, new { @class = "navbar-brand" })`
    -   Miután:`@Html.ActionLink("Contoso Team Stats", "Index", "Teams", new { area = "" }, new { @class = "navbar-brand" })`

    ![Kód módosításai][cache-layout-cshtml-code]

4. Nyomja le a **Ctrl + F5** létre, és futtassa az alkalmazást. Ez az alkalmazás verziójának beolvassa az eredmények közvetlenül az adatbázist. Megjegyzés: az alkalmazás a **MVC 5 vezérlőhöz személy keretrendszer használatával nézetekkel** scaffold által automatikusan hozzáadott **Új létrehozása**, **szerkesztése**, **Részletek**és **törlése** műveletek. A következő szakaszban az oktatóprogram vgx.dll-gyorsítótár optimalizálása az adatokhoz való hozzáférést, és adja meg a Továbbiak az alkalmazás hozzáadása fogja.

![Starter alkalmazásban][cache-starter-application]

## <a name="configure-the-application-to-use-redis-cache"></a>Az alkalmazás használatához vgx.dll gyorsítótár beállítása

Az oktatóprogram ebben a részben konfigurálnia kell a minta alkalmazás tárolására és a Contoso csapat statisztika lekérése az Azure vgx.dll gyorsítótár-példány a [StackExchange.Redis](https://github.com/StackExchange/StackExchange.Redis) gyorsítótár ügyfélprogram használatával.

-   [Az alkalmazás használatához StackExchange.Redis konfigurálása](#configure-the-application-to-use-stackexchangeredis)
-   [A TeamsController osztály jelenjen meg eredmény a gyorsítótár vagy az adatbázis frissítése](#update-the-teamscontroller-class-to-return-results-from-the-cache-or-the-database)
-   [A gyorsítótár dolgozhat az létrehozása, szerkesztése és törlése módszerek módosítása](#update-the-create-edit-and-delete-methods-to-work-with-the-cache)
-   [A csoportok Index nézete a gyorsítótár frissítése](#update-the-teams-index-view-to-work-with-the-cache)


### <a name="configure-the-application-to-use-stackexchangeredis"></a>Az alkalmazás használatához StackExchange.Redis konfigurálása

1. Visual Studio segítségével a StackExchange.Redis NuGet csomag ügyfélalkalmazás beállításához kattintson a jobb gombbal a projekt a **Megoldást Intézőben** , és válassza a **NuGet csomagok kezelése**. 

    ![NuGet csomagok kezelése][redis-cache-manage-nuget-menu]

2. **StackExchange.Redis** írja be a keresett szöveg mezőbe, és válassza ki a kívánt verziót az eredmények között kattintson a **telepítés**gombra.

    ![StackExchange.Redis NuGet csomag][redis-cache-stack-exchange-nuget]

    A NuGet csomag tölthető le, és hozzáadja az ügyfélalkalmazás a StackExchange.Redis gyorsítótár ügyfélprogrammal Azure vgx.dll gyorsítótár eléréséhez szükséges összeállítás hivatkozását. Ha inkább a **StackExchange.Redis** ügyfél tár erős nevű verzióját szeretné használni, válassza a **StackExchange.Redis.StrongName**; egyéb válassza a **StackExchange.Redis**.

3. A **Megoldás Explorer**bontsa ki a **vezérlők** mappát, és kattintson duplán a **TeamsController.cs** a megnyitáshoz.

    ![A csoportok vezérlő][cache-teamscontroller]

4. Adja hozzá a következő két **TeamsController.cs**kimutatások használatával.

        using System.Configuration;
        using StackExchange.Redis;

5. A következő két tulajdonságok hozzáadása a `TeamsController` osztály.

        // Redis Connection string info
        private static Lazy<ConnectionMultiplexer> lazyConnection = new Lazy<ConnectionMultiplexer>(() =>
        {
            string cacheConnection = ConfigurationManager.AppSettings["CacheConnection"].ToString();
            return ConnectionMultiplexer.Connect(cacheConnection);
        });
    
        public static ConnectionMultiplexer Connection
        {
            get
            {
                return lazyConnection.Value;
            }
        }
  
1. Hozzon létre egy fájlt a számítógépen nevű `WebAppPlusCacheAppSecrets.config` és olyan helyre, amely nem lehet ellenőrizni a minta alkalmazás forráskódot be kell úgy dönt, hogy beadása azon elemére. Ebben a példában a `AppSettingsSecrets.config` fájl található a `C:\AppSecrets\WebAppPlusCacheAppSecrets.config`.

    Szerkesztés a `WebAppPlusCacheAppSecrets.config` fájlt, és adja hozzá a következő tartalmát. Ha az alkalmazás helyileg ezt az információt az Azure vgx.dll gyorsítótár példányához szolgál. Az oktatóprogram a fogja Azure vgx.dll gyorsítótár-példány kiépítése, és frissítse a gyorsítótár nevét és jelszavát. Ha nem szeretné a minta alkalmazást helyileg futtatni létrehozása a fájlt, és a következő lépéseket hivatkozó a fájlt, kihagyhatja, mert amikor rendszerbe állítják Azure az alkalmazás olvassa be a gyorsítótár kapcsolati adatokat az app beállítása a webes alkalmazáshoz, és nem a fájlból. Mivel a `WebAppPlusCacheAppSecrets.config` nincs telepítve az Azure az alkalmazással, nincs szüksége, kivéve, ha az alkalmazás futtatásához helyileg fogja.


        <appSettings>
          <add key="CacheConnection" value="MyCache.redis.cache.windows.net,abortConnect=false,ssl=true,password=..."/>
        </appSettings>


2. Kattintson duplán a **Megoldást Explorer** **web.config** a megnyitáshoz.

    ![Web.config][cache-web-config]

3. Adja hozzá a következő `file` attribútum a `appSettings` elemet. Ha korábban a másik fájl nevét vagy helyét, helyettesítő ezeket az értékeket az példában szerint a lehetőségekből.
    -   Előtt:`<appSettings>`
    -   Miután:` <appSettings file="C:\AppSecrets\WebAppPlusCacheAppSecrets.config">`

    Az ASP.NET futásidőben egyesíti a külső a korrektúrát a fájl tartalmát a `<appSettings>` elemet. A futtatókörnyezet figyelmen kívül hagyja a attribútumot, ha a megadott fájl nem található. A titkos kulcsok (a kapcsolati karakterlánc a gyorsítótár) nem szerepelnek az részeként a forráskód alkalmazásához. Amikor rendszerbe állítják a web app Azure, a `WebAppPlusCacheAppSecrests.config` fájl nem telepíthető (vagyis kapcsolódó). Többféle módon titkos adatok megadhatja az Azure-ban, és ebben az oktatóanyagban konfigurálhatók automatikusan meg, amikor Ön [az Azure erőforrások kiépítése](#provision-the-azure-resources) egy későbbi oktatóanyagban lépés. Titkos kulcsok Azure-ban használata kapcsolatos további tudnivalókért olvassa el a [Gyakorlati tanácsok a központi telepítés jelszavakat és más bizalmas adatokat ASP.NET és Azure alkalmazás szolgáltatás](http://www.asp.net/identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure)című témakört.


### <a name="update-the-teamscontroller-class-to-return-results-from-the-cache-or-the-database"></a>A TeamsController osztály jelenjen meg eredmény a gyorsítótár vagy az adatbázis frissítése

Ez a példa a csapat statisztika tudja visszaszerezni az adatbázisból vagy a gyorsítótárból. Csoportwebhely statisztikai adatokat tárolja a gyorsítótár, mint egy szerializált `List<Team>`, és adattípusa vgx.dll rendezett meg is. Amikor egy rendezett csoportjából elemekre, beolvasásához néhány – az összes, vagy lekérdezése az egyes elemek. A mintában a a rendezett beállítása a felső 5 csapatok száma wins rangsorol fogja lekérdezés.

>[AZURE.NOTE] Még nem a csapat statisztikai adatok tárolására a gyorsítótárban lévő több formátumokban Azure vgx.dll gyorsítótár használatához szükséges. Ebben az oktatóanyagban több formátumok segítségével mutatja be, néhány különböző módokon és különböző adattípusú gyorsítótár adatokat is használhatja.



1. Adja hozzá a következő kimutatások használatával az `TeamsController.cs` tetején az egyéb fájl kimutatások használatával.

        using System.Diagnostics;
        using Newtonsoft.Json;

2. Az aktuális cseréje `public ActionResult Index()` módszer a következő végrehajtása.


        // GET: Teams
        public ActionResult Index(string actionType, string resultType)
        {
            List<Team> teams = null;
        
            switch(actionType)
            {
                case "playGames": // Play a new season of games.
                    PlayGames();
                    break;
        
                case "clearCache": // Clear the results from the cache.
                    ClearCachedTeams();
                    break;
        
                case "rebuildDB": // Rebuild the database with sample data.
                    RebuildDB();
                    break;
            }
        
            // Measure the time it takes to retrieve the results.
            Stopwatch sw = Stopwatch.StartNew();
        
            switch(resultType)
            {
                case "teamsSortedSet": // Retrieve teams from sorted set.
                    teams = GetFromSortedSet();
                    break;
        
                case "teamsSortedSetTop5": // Retrieve the top 5 teams from the sorted set.
                    teams = GetFromSortedSetTop5();
                    break;
        
                case "teamsList": // Retrieve teams from the cached List<Team>.
                    teams = GetFromList();
                    break;
        
                case "fromDB": // Retrieve results from the database.
                default:
                    teams = GetFromDB();
                    break;
            }
        
            sw.Stop();
            double ms = sw.ElapsedTicks / (Stopwatch.Frequency / (1000.0));

            // Add the elapsed time of the operation to the ViewBag.msg.
            ViewBag.msg += " MS: " + ms.ToString();
        
            return View(teams);
        }


3. A következő három módszerekkel hozzáadása a `TeamsController` osztály végrehajtásához a `playGames`, `clearCache`, és `rebuildDB` az adott az előző kódtöredék állandót a tevékenységtípusok.

    A `PlayGames` módot a csapat statisztika frissíti egy időszakán játékok amelyek az eredmények mentése az adatbázis és a gyorsítótár most elavult adatainak törlése.


        void PlayGames()
        {
            ViewBag.msg += "Updating team statistics. ";
            // Play a "season" of games.
            var teams = from t in db.Teams
                        select t;
    
            Team.PlayGames(teams);
    
            db.SaveChanges();
    
            // Clear any cached results
            ClearCachedTeams();
        }


    A `RebuildDB` módszer inicializálja alapértelmezés szerint a csoportoknak az adatbázist, statisztika létrehoz őket, és a gyorsítótár most elavult adatainak törlése.
    
        void RebuildDB()
        {
            ViewBag.msg += "Rebuilding DB. ";
            // Delete and re-initialize the database with sample data.
            db.Database.Delete();
            db.Database.Initialize(true);

            // Clear any cached results
            ClearCachedTeams();
        }


    A `ClearCachedTeams` módszer gyorsítótárazott csapat statisztikát eltávolítása a gyorsítótárból.

    
        void ClearCachedTeams()
        {
            IDatabase cache = Connection.GetDatabase();
            cache.KeyDelete("teamsList");
            cache.KeyDelete("teamsSortedSet");
            ViewBag.msg += "Team data removed from cache. ";
        } 


4. A következő négy módszerekkel hozzáadása a `TeamsController` osztály a különböző módon is, hogy a csapat statisztikai adatok lekérése a gyorsítótár és az adatbázis végrehajtásához. Minden, az alábbi módszerek egyikét adja vissza egy `List<Team>` amelyen ekkor megjelenik a nézetet.

    A `GetFromDB` módszer a csapat statisztika beolvassa az adatbázist.

        List<Team> GetFromDB()
        {
            ViewBag.msg += "Results read from DB. ";
            var results = from t in db.Teams
                orderby t.Wins descending
                select t; 
    
            return results.ToList<Team>();
        }


    A `GetFromList` módszer beolvassa a csapat statisztika gyorsítótárból, mint egy szerializált `List<Team>`. Ha a gyorsítótár mért sikertelen találatok, a csapat statisztika olvasni az adatbázist, és ezt követően tárolni a gyorsítótárban lévő következő alkalommal. Ez a példa a programmal mutatjuk be JSON.NET szerializálási szerializálni a .NET-objektumok szeretne, és a gyorsítótárból. További információért megtudhatja, [hogy miként Azure vgx.dll gyorsítótárban .NET-objektumok használata](cache-dotnet-how-to-use-azure-redis-cache.md#work-with-net-objects-in-the-cache).

        List<Team> GetFromList()
        {
            List<Team> teams = null;

            IDatabase cache = Connection.GetDatabase();
            string serializedTeams = cache.StringGet("teamsList");
            if (!String.IsNullOrEmpty(serializedTeams))
            {
                teams = JsonConvert.DeserializeObject<List<Team>>(serializedTeams);

                ViewBag.msg += "List read from cache. ";
            }
            else
            {
                ViewBag.msg += "Teams list cache miss. ";
                // Get from database and store in cache
                teams = GetFromDB();

                ViewBag.msg += "Storing results to cache. ";
                cache.StringSet("teamsList", JsonConvert.SerializeObject(teams));
            }
            return teams;
        }


    A `GetFromSortedSet` módszer a csapat statisztika beolvassa a gyorsítótárban tárolt rendezett meg. Ha a gyorsítótár mért sikertelen találatok, a csapat statisztika olvassa el az adatbázisból, és egy rendezett beállított a gyorsítótárban tárolt.


        List<Team> GetFromSortedSet()
        {
            List<Team> teams = null;
            IDatabase cache = Connection.GetDatabase();
            // If the key teamsSortedSet is not present, this method returns a 0 length collection.
            var teamsSortedSet = cache.SortedSetRangeByRankWithScores("teamsSortedSet", order: Order.Descending);
            if (teamsSortedSet.Count() > 0)
            {
                ViewBag.msg += "Reading sorted set from cache. ";
                teams = new List<Team>();
                foreach (var t in teamsSortedSet)
                {
                    Team tt = JsonConvert.DeserializeObject<Team>(t.Element);
                    teams.Add(tt);
                }
            }
            else
            {
                ViewBag.msg += "Teams sorted set cache miss. ";
    
                // Read from DB
                teams = GetFromDB();
    
                ViewBag.msg += "Storing results to cache. ";
                foreach (var t in teams)
                {
                    Console.WriteLine("Adding to sorted set: {0} - {1}", t.Name, t.Wins);
                    cache.SortedSetAdd("teamsSortedSet", JsonConvert.SerializeObject(t), t.Wins);
                }
            }
            return teams;
        }


    A `GetFromSortedSetTop5` módszer beolvassa az 5 legfontosabb a csoportoknak a gyorsítótárban tárolt rendezett beállítása. Jelölje be a gyorsítótár meglétének kezdődjön a `teamsSortedSet` billentyűt. Ha a kulcs nem szerepel, a `GetFromSortedSet` módszer neve a csapat statisztika olvashatók, és a gyorsítótár tárolja őket. Ezután a gyorsítótárban tárolt rendezett beállítása lekérik a visszaadott felső 5 csapatokat.


        List<Team> GetFromSortedSetTop5()
        {
            List<Team> teams = null;
            IDatabase cache = Connection.GetDatabase();

            // If the key teamsSortedSet is not present, this method returns a 0 length collection.
            var teamsSortedSet = cache.SortedSetRangeByRankWithScores("teamsSortedSet", stop: 4, order: Order.Descending);
            if(teamsSortedSet.Count() == 0)
            {
                // Load the entire sorted set into the cache.
                GetFromSortedSet();

                // Retrieve the top 5 teams.
                teamsSortedSet = cache.SortedSetRangeByRankWithScores("teamsSortedSet", stop: 4, order: Order.Descending);
            }

            ViewBag.msg += "Retrieving top 5 teams from cache. ";
            // Get the top 5 teams from the sorted set
            teams = new List<Team>();
            foreach (var team in teamsSortedSet)
            {
                teams.Add(JsonConvert.DeserializeObject<Team>(team.Element));
            }
            return teams;
        }


### <a name="update-the-create-edit-and-delete-methods-to-work-with-the-cache"></a>A gyorsítótár dolgozhat az létrehozása, szerkesztése és törlése módszerek módosítása

A állványon kód részeként ebben a minta létrehozott módszereket, amelyekkel hozzáadása, szerkesztése és törlése a csoportokat tartalmazza. Bármikor csapatnak hozzáadott, szerkeszteni vagy eltávolítani, a gyorsítótárban lévő adatok elavult lesz. Ebben a szakaszban törölje a jelet a gyorsítótárban tárolt csapatok, hogy a gyorsítótár nincs szinkronban az adatbázis nem lesz a fenti három módszert fogja módosíthatja.

1. Tallózással keresse meg a `Create(Team team)` módszer a `TeamsController` osztály. Adja hozzá a hívásokat a `ClearCachedTeams` módszer, az alábbi példában látható módon.


        // POST: Teams/Create
        // To protect from overposting attacks, please enable the specific properties you want to bind to, for 
        // more details see http://go.microsoft.com/fwlink/?LinkId=317598.
        [HttpPost]
        [ValidateAntiForgeryToken]
        public ActionResult Create([Bind(Include = "ID,Name,Wins,Losses,Ties")] Team team)
        {
            if (ModelState.IsValid)
            {
                db.Teams.Add(team);
                db.SaveChanges();
                // When a team is added, the cache is out of date.
                // Clear the cached teams.
                ClearCachedTeams();
                return RedirectToAction("Index");
            }
    
            return View(team);
        }


2. Tallózással keresse meg a `Edit(Team team)` módszer a `TeamsController` osztály. Adja hozzá a hívásokat a `ClearCachedTeams` módszer, az alábbi példában látható módon.


        // POST: Teams/Edit/5
        // To protect from overposting attacks, please enable the specific properties you want to bind to, for 
        // more details see http://go.microsoft.com/fwlink/?LinkId=317598.
        [HttpPost]
        [ValidateAntiForgeryToken]
        public ActionResult Edit([Bind(Include = "ID,Name,Wins,Losses,Ties")] Team team)
        {
            if (ModelState.IsValid)
            {
                db.Entry(team).State = EntityState.Modified;
                db.SaveChanges();
                // When a team is edited, the cache is out of date.
                // Clear the cached teams.
                ClearCachedTeams();
                return RedirectToAction("Index");
            }
            return View(team);
        }


3. Tallózással keresse meg a `DeleteConfirmed(int id)` módszer a `TeamsController` osztály. Adja hozzá a hívásokat a `ClearCachedTeams` módszer, az alábbi példában látható módon.


        // POST: Teams/Delete/5
        [HttpPost, ActionName("Delete")]
        [ValidateAntiForgeryToken]
        public ActionResult DeleteConfirmed(int id)
        {
            Team team = db.Teams.Find(id);
            db.Teams.Remove(team);
            db.SaveChanges();
            // When a team is deleted, the cache is out of date.
            // Clear the cached teams.
            ClearCachedTeams();
            return RedirectToAction("Index");
        }


### <a name="update-the-teams-index-view-to-work-with-the-cache"></a>A csoportok Index nézete a gyorsítótár frissítése

1. A **Megoldás Explorer**bontsa ki a **nézetek** mappát, majd a **csoportok** mappát, és kattintson duplán a **Index.cshtml**.

    ![Index.cshtml][cache-views-teams-index-cshtml]

2. A fájl tetején keresse meg a következő bekezdés elemet.

    ![Művelet táblázat][cache-teams-index-table]

    Az új csoport létrehozása a hivatkozásra. Az alábbi táblázatban cserélje ki a bekezdés elemet. Ez a táblázat egy új csoport létrehozása, egy új időszakán játékok lejátszás, a gyorsítótár kiürítése, beolvasása a csoportoknak a különböző formátumokban a gyorsítótárból, beolvasása a csoportoknak az adatbázisból és az adatbázis friss mintaadatokkal újbóli létrehozása művelet hivatkozásokat tartalmaz.


        <table class="table">
            <tr>
                <td>
                    @Html.ActionLink("Create New", "Create")
                </td>
                <td>
                    @Html.ActionLink("Play Season", "Index", new { actionType = "playGames" })
                </td>
                <td>
                    @Html.ActionLink("Clear Cache", "Index", new { actionType = "clearCache" })
                </td>
                <td>
                    @Html.ActionLink("List from Cache", "Index", new { resultType = "teamsList" })
                </td>
                <td>
                    @Html.ActionLink("Sorted Set from Cache", "Index", new { resultType = "teamsSortedSet" })
                </td>
                <td>
                    @Html.ActionLink("Top 5 Teams from Cache", "Index", new { resultType = "teamsSortedSetTop5" })
                </td>
                <td>
                    @Html.ActionLink("Load from DB", "Index", new { resultType = "fromDB" })
                </td>
                <td>
                    @Html.ActionLink("Rebuild DB", "Index", new { actionType = "rebuildDB" })
                </td>
            </tr>    
        </table>


3. Görgetéssel keresse meg a **Index.cshtml** fájl aljára, és adja hozzá a következő `tr` elem, hogy az utolsó sorban az utolsó táblázat a fájlban.

        <tr><td colspan="5">@ViewBag.Msg</td></tr>

    A sor értékének megjelenítése `ViewBag.Msg` , amely tartalmazza az aktuális művelet, amelyet az előző lépésben a művelet hivatkozások egyikére kattintva kapcsolatos állapotjelentés.   

    ![Állapotüzenet][cache-status-message]

4. Nyomja le az **F6** össze a projekt.

## <a name="provision-the-azure-resources"></a>Az Azure erőforrások kiépítése

Tárolni az Azure-alkalmazás, akkor először engedélyeznie kell az Azure szolgáltatásokat az alkalmazáshoz szükséges. A minta alkalmazás ebben az oktatóanyagban használja az alábbi Azure-szolgáltatásokat.

-   Azure vgx.dll gyorsítótár
-   Alkalmazás szolgáltatás Web App alkalmazásban
-   SQL-adatbázis

Egy új vagy meglévő erőforráscsoport lehetőség az alábbi szolgáltatások telepítéséhez kattintson a következő **Azure a központi telepítés** gombra.

[! [Azure telepítéséhez] [deploybutton]](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-web-app-redis-cache-sql-database%2Fazuredeploy.json)

A **központi telepítés az Azure** gomb az alábbi szolgáltatások kiépítése és a kapcsolati karakterláncot az SQL-adatbázissal, és az alkalmazás beállítása az Azure vgx.dll gyorsítótár kapcsolati karakterláncot a [Létrehozás webalkalmazást plusz vgx.dll gyorsítótár plusz SQL-adatbázis](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-redis-cache-sql-database) [Azure quickstart útmutató](https://github.com/Azure/azure-quickstart-templates) sablont használja.

>[AZURE.NOTE] Azure-fiók esetén nem hozhat [létre egy ingyenes Azure-fiók](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=redis_cache_hero) a mindössze néhány perc.

A **központi telepítés az Azure** gombra kattintva megnyitja az Azure-portálra, és az erőforrások által a sablon leírt létrehozásának folyamata kezdeményez.

![Azure telepítése][cache-deploy-to-azure-step-1]

1. Kattintson az **Egyéni telepítés** lap válassza az Azure előfizetést használ, és válassza a meglévő erőforráscsoport, vagy hozzon létre egy újat, és az erőforrás csoport helyének megadása.
2. Adja meg a **Paraméterek** lap a rendszergazda partnernévvel (**ADMINISTRATORLOGIN** - nem használja a **rendszergazdai**), rendszergazda Adatbázisjelszó (**ADMINISTRATORLOGINPASSWORD**), és az adatbázis nevét (**ADATBÁZISNÉV**). A többi paraméter egy ingyenes alkalmazás szolgáltatás tervet, és az SQL-adatbázis és Azure vgx.dll gyorsítótár, amelyhez nem tartozik egy ingyenes réteg alsó költség beállítások szolgáltatója megtörténik.
3. Módosítsa a beállításokat tetszés szerint vagy tartani az alapértelmezett beállításokat, és kattintson az **OK gombra**.


![Azure telepítése][cache-deploy-to-azure-step-2]

1. Kattintson a **Véleményezés jogi feltételeket**.
2. Olvassa el a feltételeket, kattintson a **vásárlás** lap, és kattintson a **vásárlás**gombra.
3. Az erőforrások kiépítési indításához kattintson az **egyéni telepítési** lap kattintson a **Létrehozás** gombra.

A telepítés állapotának megtekintése, kattintson az értesítési ikonra, és kattintson a **telepítés megkezdődött**.

![Telepítési lépések][cache-deployment-started]

A telepítés állapotának is megtekintheti a **Microsoft.Template** lap.

![Azure telepítése][cache-deploy-to-azure-step-3]

Kiépítési befejeződése után közzéteheti a Visual Studio Azure az alkalmazást.

>[AZURE.NOTE] A hibák, a kiépítési folyamat során fellépő jelenik meg a **Microsoft.Template** lap. A gyakori hibák túl sok SQL-kiszolgálók vagy a tervek előfizetésenként szolgáltatója túl sok ingyenes alkalmazás szolgáltatás. A hibák megoldásához, és indítsa újra a a folyamat parancsával **telepítsen újra** a **Microsoft.Template** lap vagy a **Deploy az Azure** gombra, ebben az oktatóanyagban.

## <a name="publish-the-application-to-azure"></a>Az alkalmazás Azure közzététele

Ebben a lépésben az oktatóprogram fogja közzététele az Azure alkalmazást, és indítsa el a felhőben.

1. Kattintson a jobb gombbal a **ContosoTeamStats** projekt a Visual Studióban, és válassza a **Közzététel**lehetőséget.

    ![Közzététel][cache-publish-app]

2. Kattintson a **Microsoft Azure alkalmazás szolgáltatás**.

    ![Közzététel][cache-publish-to-app-service]

3. Jelölje ki azt az előfizetést, az Azure erőforrások létrehozásánál használt, bontsa ki a tartalmazó az erőforrások erőforráscsoport, jelölje ki a kívánt Web App, és kattintson az **OK gombra**. Ha a **webhely** további karakter követ kezdődik a Web App neve **Azure a központi telepítés** gombra.

    ![Jelölje ki a Web App alkalmazásban][cache-select-web-app]

4. Kattintson **Az érvényesítés kapcsolat** , a beállítások ellenőrzéséhez, és kattintson a **Közzététel**gombra.

    ![Közzététel][cache-publish]

    Néhány másodperc múlva a közzétételi folyamat befejeződik, és a böngésző elindul a a futtatása minta alkalmazást. Ha ellenőrzéskor vagy közzétételi DNS-hibaüzenetet kap, és a kiépítési az Azure erőforrások az alkalmazás csak a legutóbb befejeződött, várjon egy kicsit, majd próbálkozzon újra.

    ![Gyorsítótár hozzáadott][cache-added-to-application]

Az alábbi táblázat ismerteti az egyes művelet hivatkozások minta alkalmazásból.

| Művelet                  | Leírás                                                                                                                                                      |
|-------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Új gyorsművelet létrehozása              | Új csoport létrehozása.                                                                                                                                               |
| Lejátszás season mezővel             | Játszhat le egy időszakán játékok, frissítse a csapat stat és bármely elavult törölje a jelet a gyorsítótár adatainak csapatának.                                                                          |
| Gyorsítótár ürítése             | Törölje a jelet a csapat stat a gyorsítótárból.                                                                                                                             |
| Lista gyorsítótárból         | A csoportwebhely stat beolvashatja a gyorsítótárból. Ha a gyorsítótár mért sikertelen találatok, töltse be a statisztika az adatbázisból, és a gyorsítótár mentése későbbi használatra.                                        |
| Gyorsítótárból rendezett beállítása   | A csoportwebhely stat beolvashatja a gyorsítótárból egy rendezett készlet segítségével. Ha a gyorsítótár mért sikertelen találatok, töltse be a statisztika az adatbázisból, és a gyorsítótár egy rendezett készlet segítségével mentheti.  |
| 5 legfontosabb a csoportoknak a gyorsítótárból  | A felső 5 csapatok beolvashatja a gyorsítótárból egy rendezett készlet segítségével. Ha a gyorsítótár mért sikertelen találatok, töltse be a statisztika az adatbázisból, és a gyorsítótár egy rendezett készlet segítségével mentheti. |
| KCS2 betöltése            | A csoportwebhely stat beolvashatja az adatbázisból.                                                                                                                       |
| KCS2 újraépítéséhez              | Az adatbázis újjáépítése, és töltse be újra a csapat mintaadatokat tartalmazó.                                                                                                        |
| Szerkesztés / részletek / törlése | Csoport szerkesztése, egy csoport részleteinek megtekintése, törölheti a csoportot.                                                                                                             |


Kattintson a műveletek néhány és kísérletezhet a különböző forrásokból származó adatok beolvasása. Nem közötti különbségek az adatok beolvasása az adatbázisból és a gyorsítótár különféle módjait befejezéséhez szükséges időt.

## <a name="delete-the-resources-when-you-are-finished-with-the-application"></a>Az erőforrások törlése, ha végzett az alkalmazással

Ha befejezte a minta oktatóanyag alkalmazással, törölheti az Azure forrásokban használt költség és erőforrások megőrzése érdekében. Ha [kiépítése az Azure források](#provision-the-azure-resources) szakaszában használja a **Deploy az Azure** gombra, és az azonos erőforráscsoport szereplő összes az erőforrások, törölheti őket együtt egy lépésben az erőforráscsoport törlésével.

1. Jelentkezzen be az [Azure-portálra](https://portal.azure.com) , és kattintson **az erőforrás csoportok**.
2. A csoport nevét, az erőforrás beírja a **... elemek szűrése** mezőben lévő értéket.
3. Kattintson a **...** az erőforráscsoport jobbra.
4. Kattintson a **Törlés**gombra.

    ![Törlése][cache-delete-resource-group]

5. Írja be az erőforráscsoport a nevét, és kattintson a **Törlés**gombra.

    ![Törlés jóváhagyása][cache-delete-confirm]

Néhány másodperc múlva erőforráscsoport és az összes tárolt erőforrásait törlődnek.

>[AZURE.IMPORTANT] Ne feledje, hogy az erőforrás a csoport törlése nem vissza nem vonható, hogy az erőforrás csoport és a benne lévő összes erőforrás véglegesen törlődik. Győződjön meg arról, hogy nem véletlenül törli a nem megfelelő erőforráscsoport vagy erőforrásokat. Ha létrehozott az erőforrások szolgáltatója meglévő erőforráscsoport belül a következő példában, törölhet egyes erőforrásokhoz egyenként a a megfelelő pengéit.

## <a name="run-the-sample-application-on-your-local-machine"></a>A minta alkalmazás futtatásához a helyi számítógépre

Az alkalmazás helyileg fut a számítógépen, egy Azure vgx.dll gyorsítótár-példányt, amelyben az adatok gyorsítótárba szüksége van. 

-   Ha az alkalmazás Azure van közzétéve, az előző szakaszban leírtak szerint, az Azure vgx.dll gyorsítótár-példányt, hogy lépés során kiépített is használhatja.
-   Ha egy másik meglévő Azure vgx.dll gyorsítótár-példányt, is használhatja, amely a következő példában helyi futtatásához.
-   Azure vgx.dll gyorsítótár-példány létrehozásához szükséges, ha a lépésekkel [létrehozása gyorsítótárat](cache-dotnet-how-to-use-azure-redis-cache.md#create-a-cache).

Ha már kijelölt vagy a gyorsítótár használandó létrehozott, tallózással keresse meg a gyorsítótár az Azure-portálon, és beolvasni a [host Name mezőbe](cache-configure.md#properties) , és a [hívóbetűk](cache-configure.md#access-keys) a gyorsítótár. Útmutatásért lásd: a [gyorsítótár beállításainak konfigurálása vgx.dll](cache-configure.md#configure-redis-cache-settings).

1. Nyissa meg a `WebAppPlusCacheAppSecrets.config` [konfigurálása a gyorsítótár vgx.dll használni kívánt alkalmazást](#configure-the-application-to-use-redis-cache) lépés e lehetőség a szerkesztőben az oktatóprogram során létrehozott fájlra.

2. Szerkesztés a `value` attribútum és a csere `MyCache.redis.cache.windows.net` a gyorsítótár, a [host Name mezőbe](cache-configure.md#properties) , és adja meg a jelszó vagy a [elsődleges és másodlagos kulcsa](cache-configure.md#access-keys) a gyorsítótár.


        <appSettings>
          <add key="CacheConnection" value="MyCache.redis.cache.windows.net,abortConnect=false,ssl=true,password=..."/>
        </appSettings>


3. Nyomja le a **Ctrl + F5 billentyűparancs hatására** az alkalmazás futtatásához.

>[AZURE.NOTE] Figyelje meg, hogy az alkalmazás, beleértve az adatbázist, helyben fut, és a vgx.dll gyorsítótár üzemelteti Azure-ban, mert a gyorsítótár jelenhetnek meg csoportban hajtsa végre az adatbázist. A legjobb teljesítmény elérése érdekében az ügyfélalkalmazás és Azure vgx.dll gyorsítótár-példány ugyanazon a helyen kell lennie. 

## <a name="next-steps"></a>Következő lépések

-   További tudnivalók a [ASP.NET MVC 5 – első lépések](http://www.asp.net/mvc/overview/getting-started/introduction/getting-started) a [ASP.NET](http://asp.net/) -webhelyen.
-   További példák létrehozása az ASP.NET Web App alkalmazás szolgáltatásban című témakörben talál [létrehozása és telepítése az ASP.NET web app alkalmazás Azure szolgáltatásban](https://github.com/Microsoft/HealthClinic.biz/wiki/Create-and-deploy-an-ASP.NET-web-app-in-Azure-App-Service) a [bemutató](https://blogs.msdn.microsoft.com/visualstudio/2015/12/08/connectdemos-2015-healthclinic-biz/) [HealthClinic.biz](https://github.com/Microsoft/HealthClinic.biz) 2015 csatlakozás.
    -   További QuickStarts HealthClinic.biz bemutatója a csomagban a [Azure fejlesztői eszközökkel QuickStarts csomagban](https://github.com/Microsoft/HealthClinic.biz/wiki/Azure-Developer-Tools-Quickstarts)című témakör tartalmaz.
-   További tudnivalók ebben az oktatóanyagban használt entitás keretrendszer [kódot először az új adatbázis](https://msdn.microsoft.com/data/jj193542) megközelítése.
-   További tudnivalók a [web Apps alkalmazások Azure App szolgáltatásban](../app-service-web/app-service-web-overview.md).
-   Megtudhatja, hogyan [monitor](cache-how-to-monitor.md) a gyorsítótár az Azure-portálon.

-   Azure vgx.dll gyorsítótár prémium szolgáltatásaihoz feltárása
    -   [Adatmegőrzési prémium Azure vgx.dll gyorsítótár beállítása](cache-how-to-premium-persistence.md)
    -   [Prémium Azure vgx.dll gyorsítótár fürtözőszoftverét konfigurálása](cache-how-to-premium-clustering.md)
    -   [Virtuális hálózat támogatása prémium Azure vgx.dll gyorsítótár beállítása](cache-how-to-premium-vnet.md)
    -   Olvassa el az [Azure-gyorsítótár Gyakori vgx.dll](cache-faq.md#what-redis-cache-offering-and-size-should-i-use) méretét, átviteli és a prémium gyorsítótárát sávszélesség olvashat bővebben.



<!-- IMAGES -->
[cache-starter-application]: ./media/cache-web-app-howto/cache-starter-application.png
[cache-added-to-application]: ./media/cache-web-app-howto/cache-added-to-application.png
[cache-create-project]: ./media/cache-web-app-howto/cache-create-project.png
[cache-select-template]: ./media/cache-web-app-howto/cache-select-template.png
[cache-model-add-class]: ./media/cache-web-app-howto/cache-model-add-class.png
[cache-model-add-class-dialog]: ./media/cache-web-app-howto/cache-model-add-class-dialog.png
[cache-add-controller]: ./media/cache-web-app-howto/cache-add-controller.png
[cache-add-controller-class]: ./media/cache-web-app-howto/cache-add-controller-class.png
[cache-configure-controller]: ./media/cache-web-app-howto/cache-configure-controller.png
[cache-global-asax]: ./media/cache-web-app-howto/cache-global-asax.png
[cache-RouteConfig-cs]: ./media/cache-web-app-howto/cache-RouteConfig-cs.png
[cache-layout-cshtml]: ./media/cache-web-app-howto/cache-layout-cshtml.png
[cache-layout-cshtml-code]: ./media/cache-web-app-howto/cache-layout-cshtml-code.png
[redis-cache-manage-nuget-menu]: ./media/cache-web-app-howto/redis-cache-manage-nuget-menu.png
[redis-cache-stack-exchange-nuget]: ./media/cache-web-app-howto/redis-cache-stack-exchange-nuget.png
[cache-teamscontroller]: ./media/cache-web-app-howto/cache-teamscontroller.png
[cache-web-config]: ./media/cache-web-app-howto/cache-web-config.png
[cache-views-teams-index-cshtml]: ./media/cache-web-app-howto/cache-views-teams-index-cshtml.png
[cache-teams-index-table]: ./media/cache-web-app-howto/cache-teams-index-table.png
[cache-status-message]: ./media/cache-web-app-howto/cache-status-message.png
[deploybutton]: ./media/cache-web-app-howto/deploybutton.png
[cache-deploy-to-azure-step-1]: ./media/cache-web-app-howto/cache-deploy-to-azure-step-1.png
[cache-deploy-to-azure-step-2]: ./media/cache-web-app-howto/cache-deploy-to-azure-step-2.png
[cache-deploy-to-azure-step-3]: ./media/cache-web-app-howto/cache-deploy-to-azure-step-3.png
[cache-deployment-started]: ./media/cache-web-app-howto/cache-deployment-started.png
[cache-publish-app]: ./media/cache-web-app-howto/cache-publish-app.png
[cache-publish-to-app-service]: ./media/cache-web-app-howto/cache-publish-to-app-service.png
[cache-select-web-app]: ./media/cache-web-app-howto/cache-select-web-app.png
[cache-publish]: ./media/cache-web-app-howto/cache-publish.png
[cache-delete-resource-group]: ./media/cache-web-app-howto/cache-delete-resource-group.png
[cache-delete-confirm]: ./media/cache-web-app-howto/cache-delete-confirm.png

