<properties
    pageTitle="Azure alkalmazás szolgáltatáshoz – Node.js frissítési mobil szolgáltatásból"
    description="Megtudhatja, hogy miként egyszerűen frissítse a Mobile Services alkalmazás egy alkalmazás Mobile-alkalmazás"
    services="app-service\mobile"
    documentationCenter=""
    authors="adrianhall"
    manager="yochayk"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile"
    ms.devlang="node"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="upgrade-your-existing-nodejs-azure-mobile-service-to-app-service"></a>A meglévő Node.js Azure mobilszolgáltatási alkalmazás szolgáltatás frissítése

Alkalmazás szolgáltatás Mobile új módja a mobil alkalmazások használata a Microsoft Azure össze. További tudnivalókért lásd: [Mik azok a Mobile-Appok?].

Ez a témakör ismerteti, hogyan frissíthet Azure Mobile Services egy létező Node.js kódmentes alkalmazás új alkalmazás szolgáltatás Mobile-alkalmazások. Közben végrehajthatja a frissítést, a meglévő Mobile Services-alkalmazás továbbra is működnek.  Ha egy Node.js kódmentes alkalmazás frissítése van szüksége, olvassa el [a .NET Mobile szolgáltatások frissítése](./app-service-mobile-net-upgrading-from-mobile-services.md).

A mobil kódmentes Azure alkalmazás szolgáltatás frissítésekor fér hozzá az összes alkalmazás szolgáltatása, és megfelelően [alkalmazás szolgáltatás árak], nem árak Mobile szolgáltatások vannak a számlát kapni.

## <a name="migrate-vs-upgrade"></a>Frissítés összehasonlítása áttelepítése

[AZURE.INCLUDE [app-service-mobile-migrate-vs-upgrade](../../includes/app-service-mobile-migrate-vs-upgrade.md)]

>[AZURE.TIP] Azt ajánljuk, hogy [áttelepítés](app-service-mobile-migrating-from-mobile-services.md) előtt frissítésének keresztül. Ezzel a módszerrel is a két verzió, az alkalmazás elhelyezése a azonos alkalmazás szolgáltatás tervezése és merülnek fel külön költség nélkül.

### <a name="improvements-in-mobile-apps-nodejs-server-sdk"></a>A Mobile alkalmazások Node.js server SDK javítása

Sok rendelkezik, például az új [Mobile alkalmazások SDK](https://www.npmjs.com/package/azure-mobile-apps) való frissítés nyújtja:

- [Express keretrendszer](http://expressjs.com/en/index.html)alapján, az új csomópont SDK egyszerűsített és érdemes új csomópont-verzióra, és miközben új funkciók hamarosan megtervezni. Testre szabhatja az alkalmazás működésének Express köztes együtt.

- Teljesítménybeli fejlesztések a mobil szolgáltatások SDK képest.

- A mobil kódmentes; együtt most üzemeltetheti webhelye Hasonlóképpen Gyerekjáték adja hozzá az Azure Mobile SDK bármelyik meglévő express.v4 alkalmazáshoz.

- Beépített platformok és a helyi fejlesztési, a Mobile alkalmazások SDK csomagjában talál lehetnek fejlesztett és helyileg futtassa a Windows, Linux és OSX platformon futó verziók. Célszerű most könnyen használható gyakori csomópont fejlesztési technikákat például az [galuskánál](https://mochajs.org/) teszteket a telepítés előtt.

## <a name="overview"></a>Egyszerű frissítési – áttekintés

Azure alkalmazás szolgáltatás történő egy Node.js kódmentes frissítés megkönnyítése érdekében a kompatibilitási csomag nyújtott.  Frissítés után lehetősége van egy új alkalmazás szolgáltatás webhelyen telepített niew webhelyen.

A mobil ügyfélprogram SDK **nem** kompatibilis a új Mobile-alkalmazások kiszolgálóval SDK is. Annak érdekében, hogy az alkalmazás szolgáltatás folyamatosságát, meg kell nem módosításainak közzététele jelenleg az ügyfeleknek közzétett szolgáló webhely. Ehelyett hozzunk létre egy új mobilalkalmazás másolataként kiszolgáló. Ez az alkalmazás további pénzügyi költség felmerülése elkerülése ugyanazon alkalmazás szolgáltatás terven elhelyezheti.

Ezután meg kell az alkalmazás két verziója: a helyettesítő, és a többi, amelyek ezután frissítheti, illetve egy új ügyfél verziójának megjelenése cél az alkalmazások változatlan marad, és szolgál egyik közzé. Áthelyezheti, és tesztelje a kódot a azt szeretné, de gondoskodnia kell arról, hogy bármely hibajavítás, akkor alkalmazza is. Ha úgy érzi, hogy a helyettesítő alkalmazásainak a legújabb verzióra frissítette ügyfél táblázatcellák kívánt számát, ha, törölheti az eredeti áttelepített alkalmazás kívánt. További pénzbeli költségek, azt nem merülnek, ha az alkalmazás szolgáltatás megegyező csomagot a Mobile alkalmazásban is.

A frissítési folyamat esetében a teljes vázlat van az alábbi képlettel történik:

1. Töltse le a meglévő (áttelepített) Azure mobilszolgáltatási.
2. A projekt átalakítása az Azure Mobile alkalmazásban a kompatibilitási csomag használata.
3. Javítsa ki (például a hitelesítési beállítások) közötti különbség.
4. A konvertált Azure mobilalkalmazás projekt új alkalmazás szolgáltatás telepítése.
4. Engedje fel az ügyfélalkalmazás az új Mobile alkalmazást használó új verziója.
5. (Nem kötelező) Törölje az eredeti áttelepített mobilszolgáltatás alkalmazást.

Törlés akkor fordulhat elő, ha nem látható minden forgalom az eredeti áttelepített mobilszolgáltatás.

## <a name="install-npm-package"></a>Telepítse a előtti követelmények

A helyi számítógépen telepíteni kell [csomópont].  A kompatibilitási csomag kell telepíteni.  Csomópont telepítése után a új cmd vagy PowerShell-parancssorában futtathatja az alábbi parancsot:

```npm i -g azure-mobile-apps-compatibility```

## <a name="obtain-ams-scripts"></a>Szerezze be a mobil szolgáltatások Azure parancsfájlok

- Jelentkezzen be az [Azure-portálon].
- **Összes erőforrás** - vagy **Alkalmazás szolgáltatások**használata esetén keresse meg a mobil szolgáltatások webhelyen.
- A webhely, kattintson az **eszközök** -> **Kudu** -> **lépjen** a Kudu-webhely megnyitása.
- **Hibakeresési**konzolban kattintson -> **PowerShell** a hibakeresési konzol megnyitása.
- Nyissa meg azt `site/wwwroot/App_Data/config` az egyes címtár parancsával
- Kattintson a letöltés ikonra a Tovább gombra a `scripts` címtár.

A parancsfájlok ZIP-formátumban letöltése.  Hozzon létre egy új könyvtárat a helyi számítógépen, és csomagolja ki az `scripts.ZIP` fájl a címtáron belül.  Ez a művelet létrehoz egy `scripts` címtár.

## <a name="scaffold-app"></a>Az új Azure Mobile-alkalmazások kódmentes scaffold

A következő parancsot a címtárból parancsfájlok könyvtárat tartalmazó:

```scaffold-mobile-app scripts out```

Ez a művelet létrehoz egy scaffolded Azure Mobile-alkalmazások kódmentes a a `out` címtár.  Nem kötelező, bár célszerű ellenőrizni a `out` könyvtár történő egy adatforrás kód tárházba lehetőség.

## <a name="deploy-ama-app"></a>Az Azure Mobile-alkalmazások kódmentes terjesztése

A telepítés során meg kell kövesse az alábbi lépéseket:

1. Hozzon létre egy új mobilalkalmazás az [Azure-portálon].
2. Futtassa a `createViews.sql` parancsfájl a csatlakoztatott adatbázisban.
3. Hivatkozás az adatbázist, amely a mobilszolgáltatási az új alkalmazás szolgáltatásba van csatolva.
4. Hivatkozás más erőforrások (például értesítés hubok) az új alkalmazás szolgáltatás.
5. A létrehozott kód telepítse az új webhely.

### <a name="create-a-new-mobile-app"></a>Hozzon létre egy új Mobile alkalmazásban

1. Jelentkezzen be a az [Azure-portálon].

2. Kattintson a **+ Új** > **webes + Mobile** > **Mobile alkalmazást**, majd nevezze el a mobilalkalmazás kódmentes.

3. Az **Erőforráscsoport**jelölje be a meglévő erőforráscsoport, vagy hozzon létre egy újat (ugyanazt a nevet a az alkalmazást használják.) 
 
    Válasszon egy másik alkalmazás szolgáltatás tervet, vagy hozzon létre egy újat. További információt az alkalmazás szolgáltatások tervek és az új csomag létrehozása az, hogy egy másik árak első csoportba tartozó, és a kívánt helyre a [áttekintése](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)című témakörben találhat Azure alkalmazás szolgáltatás csomagok mélyreható.

4. Az **alkalmazás szolgáltatáscsomagja**az alapértelmezett csomag (a [szabványos réteg](https://azure.microsoft.com/pricing/details/app-service/)) be van jelölve. Emellett bejelölheti a másik csomagra, vagy [Hozzon létre egy újat](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md#create-an-app-service-plan). Az alkalmazás szolgáltatáscsomagja beállításai határozzák meg a [helyet, funkcióiról, költség, és erőforrások kiszámítására](https://azure.microsoft.com/pricing/details/app-service/) az alkalmazással társított. 

    Miután úgy dönt, kattintson a csomagot, kattintson a **Létrehozás**gombra. Ezzel létrehoz, hogy a mobilalkalmazás kódmentes. 


### <a name="run-createviewssql"></a>CreateViews.SQL futtatása

A scaffolded alkalmazást tartalmaz egy nevű fájlt `createViews.sql`.  A céladatbázisban kell futtatható parancsfájl.  A kapcsolati karakterláncot, a céladatbázis szerezheti be a **Beállítások** lap a **Kapcsolati karakterláncot**az az áttelepített mobilszolgáltatás.  Névvel `MS_TableConnectionString`.

Ezt a parancsfájlt az SQL Server Management Studio vagy Visual Studio belül futtathatja.

### <a name="link-the-database-to-your-app-service"></a>Az adatbázis csatolása az alkalmazás szolgáltatás

A meglévő adatbázis csatolása az alkalmazás szolgáltatás:

- Az [Azure-portálon]nyissa meg az App szolgáltatás.
- Válassza a **minden beállítások** -> **adatkapcsolatokat**.
- Kattintson a **+ Hozzáadás**.
- A legördülő listában jelölje ki **Az SQL-adatbázis**
- **SQL-adatbázissal**csoportban jelölje be a meglévő adatbázist, majd kattintson a **Jelölje ki**.
- A **kapcsolati karakterláncot**az adatbázis írja be a felhasználónevét és jelszavát, majd kattintson az **OK gombra**.
- A **Hozzáadás adatkapcsolatokat** a lap kattintson **az OK gombra**.

A felhasználónév és jelszó megtekintésével, a céladatbázis a kapcsolati karakterláncot az áttelepített mobilszolgáltatási találhatók.


### <a name="set-up-authentication"></a>Hitelesítés beállítása

Azure Mobile-alkalmazások lehetővé teszi, hogy Azure Active Directory, a Facebook, a Google, a Microsoft és a Twitteren hitelesítés belül a szolgáltatás konfigurálását.  Egyéni hitelesítési készüljön külön-külön kell.  Olvassa el a [Hitelesítési fogalmak] dokumentáció és a [Hitelesítési quickstart útmutató] dokumentáció további információt.  

## <a name="updating-clients"></a>Frissítés mobilügyfelek

Ha már egy műveleti mobilalkalmazás kódmentes, az ügyfélalkalmazás, amely fogyasztása azt az új verziójában dolgozhat. Mobile-alkalmazások is tartalmazza az ügyfél SDK új verziót, és a kiszolgáló frissítés feletti hasonló, meg kell a Mobile-alkalmazások verzió telepítése előtt távolítsa el a a mobil szolgáltatások SDK mutató összes hivatkozást.

A legfontosabb változások a verziói között egyik, hogy a konstruktorok már nem szükséges egy helyi menüt megjelenítő billentyűt. Most már egyszerűen adja meg a mobilalkalmazás URL-címét. Például a a .NET-ügyfelek a `MobileServiceClient` konstruktor ettől kezdve:

        public static MobileServiceClient MobileService = new MobileServiceClient(
            "https://contoso.azurewebsites.net", // URL of the Mobile App
        );

Az új SDK telepítésével és használatával a keresztül az alábbi hivatkozásokat új struktúráját olvashat:

- [Android 2.2 vagy újabb verzió](app-service-mobile-android-how-to-use-client-library.md)
- [iOS verzió 3.0.0 vagy újabb verzió](app-service-mobile-ios-how-to-use-client-library.md)
- [.NET (Windows/Xamarin) verzió 2.0.0 vagy újabb verzió](app-service-mobile-dotnet-how-to-use-client-library.md)
- [Apache Cordova 2.0-s vagy újabb verzió](app-service-mobile-cordova-how-to-use-client-library.md)

Ha az alkalmazás használata a leküldéses értesítéseket, jegyezze fel a regisztráció utasítás platformokon készíti, az eddig néhány változtatásokat, valamint.

Ha készen áll az új ügyfél verziót, próbálja ki a frissített kiszolgáló projekthez ellen. Után ellenőrzése, hogy működik-e, engedje fel az ügyfelek alkalmazás új verziója. Végül után az ügyfelekkel megkapja ezeket a frissítéseket a névjegyadatok kellett volna, törölheti az alkalmazás Mobile szolgáltatások verzióját. Ezen a ponton teljesen frissítette-kiszolgálóval a legújabb Mobile-alkalmazások SDK alkalmazás szolgáltatás Mobile alkalmazásban.

<!-- URLs. -->

[Azure portál]: https://portal.azure.com/
[Azure classic portal]: https://manage.windowsazure.com/
[Mik azok a Mobile-alkalmazások?]: app-service-mobile-value-prop.md
[I already use web sites and mobile services – how does App Service help me?]: /en-us/documentation/articles/app-service-mobile-value-prop-migration-from-mobile-services
[Mobile App Server SDK]: https://www.npmjs.com/package/azure-mobile-apps
[Create a Mobile App]: app-service-mobile-xamarin-ios-get-started.md
[Add push notifications to your mobile app]: app-service-mobile-xamarin-ios-get-started-push.md
[Add authentication to your mobile app]: app-service-mobile-xamarin-ios-get-started-users.md
[Azure Scheduler]: /en-us/documentation/services/scheduler/
[Web Job]: ../app-service-web/websites-webjobs-resources.md
[How to use the .NET server SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Migrate from Mobile Services to an App Service Mobile App]: app-service-mobile-migrating-from-mobile-services.md
[Migrate your existing Mobile Service to App Service]: app-service-mobile-migrating-from-mobile-services.md
[Árak alkalmazás szolgáltatás]: https://azure.microsoft.com/en-us/pricing/details/app-service/
[.NET server SDK overview]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Hitelesítés fogalmak]: ../app-service/app-service-authentication-overview.md
[Hitelesítés quickstart útmutató]: app-service-mobile-auth.md

[Azure portál]: https://portal.azure.com/
[OData]: http://www.odata.org
[Promise]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise
[basicapp sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/basic-app
[todo sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/todo
[samples directory on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples
[static-schema sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/static-schema
[QueryJS]: https://github.com/Azure/queryjs
[Node.js Tools 1.1 for Visual Studio]: https://github.com/Microsoft/nodejstools/releases/tag/v1.1-RC.2.1
[mssql Node.js package]: https://www.npmjs.com/package/mssql
[Microsoft SQL Server 2014 Express]: http://www.microsoft.com/en-us/server-cloud/Products/sql-server-editions/sql-server-express.aspx
[ExpressJS Middleware]: http://expressjs.com/guide/using-middleware.html
[Winston]: https://github.com/winstonjs/winston
