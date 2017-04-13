<properties
    pageTitle="Frissítés a mobil szolgáltatások Azure alkalmazás szolgáltatás"
    description="Megtudhatja, hogy miként egyszerűen frissítse a Mobile Services alkalmazás egy alkalmazás Mobile-alkalmazás"
    services="app-service\mobile"
    documentationCenter=""
    authors="adrianhall"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="upgrade-your-existing-net-azure-mobile-service-to-app-service"></a>A meglévő .NET Azure mobilszolgáltatási alkalmazás szolgáltatás frissítése

Alkalmazás szolgáltatás Mobile új módja a mobil alkalmazások használata a Microsoft Azure össze. További tudnivalókért lásd: [Mik azok a Mobile-Appok?].

Ez a témakör ismerteti, hogyan frissíthet Azure Mobile Services egy létező .NET kódmentes alkalmazás új alkalmazás szolgáltatás Mobile-alkalmazások. Közben végrehajthatja a frissítést, a meglévő Mobile Services-alkalmazás továbbra is működnek.   Ha egy Node.js kódmentes alkalmazás frissítése van szüksége, olvassa el [a Node.js Mobile szolgáltatások frissítése](./app-service-mobile-node-backend-upgrading-from-mobile-services.md).

A mobil kódmentes Azure alkalmazás szolgáltatás frissítésekor fér hozzá az összes alkalmazás szolgáltatása, és megfelelően [alkalmazás szolgáltatás árak], nem árak Mobile szolgáltatások vannak a számlát kapni.

##<a name="migrate-vs-upgrade"></a>Frissítés összehasonlítása áttelepítése

[AZURE.INCLUDE [app-service-mobile-migrate-vs-upgrade](../../includes/app-service-mobile-migrate-vs-upgrade.md)]

>[AZURE.TIP] Azt ajánljuk, hogy [áttelepítés](app-service-mobile-migrating-from-mobile-services.md) előtt frissítésének keresztül. Ezzel a módszerrel is a két verzió, az alkalmazás elhelyezése a azonos alkalmazás szolgáltatás tervezése és merülnek fel külön költség nélkül.

###<a name="improvements-in-mobile-apps-net-server-sdk"></a>A Mobile alkalmazások .NET server SDK javítása

Az új [Mobile alkalmazások SDK](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/) frissítése a következő előnyökkel jár:

- Nagyobb rugalmasság NuGet függőségeket. Az üzemeltetési környezet már nem ez a saját NuGet csomagok, ezért alternatív kompatibilis verziókkal is használhatja. Jó helyen jár Ha új kritikus bugfixes vagy a Mobile Server SDK vagy függőségek biztonsági frissítések, kézzel kell frissítenie a szolgáltatásban.

- Nagyobb rugalmasság a mobil SDK csomagjában talál. Pontosan megadhatja, hogy mely funkciók és útvonalak be van állítva, például hitelesítés, a táblázat API-hoz és a leküldéses regisztrációs végpont. További tudnivalókért lásd: [a .NET-kiszolgáló SDK Azure Mobile-alkalmazások használata](app-service-mobile-net-upgrading-from-mobile-services.md#server-project-setup).

- Más ASP.NET projekttípusok és útvonalak támogatása. Most már üzemeltetheti MVC és a webes API vezérlők a mobil kódmentes projektként ugyanabban a projektben.

- Új alkalmazás hitelesítési szolgáltatások, amelyek lehetővé teszik, hogy egy közös hitelesítési konfiguráció használata a webes és mobilalkalmazások támogatása.

##<a name="overview"></a>Egyszerű frissítési – áttekintés

Sok esetben frissítése lesz, egyszerűen az új Mobile-alkalmazások kiszolgálóra SDK váltás és közzétehető Mobile-alkalmazás új példányát az alakzatot a kódot. Vannak azonban néhány olyan esetek, amelyek esetén kell néhány további konfigurációja, például a speciális hitelesítési esetek és használata ütemezett a feladatokat. Ezeket az újabb szakaszok vonatkozik.

>[AZURE.TIP] Tanácsos, hogy Ön és a többi Ez a témakör teljesen érthetőbbé frissítés megkezdése előtt. Jegyezze fel funkcióit használhatja alatti beállításokkal.

A mobil ügyfélprogram SDK **nem** kompatibilis a új Mobile-alkalmazások kiszolgálóval SDK is. Annak érdekében, hogy az alkalmazás szolgáltatás folyamatosságát, meg kell nem módosításainak közzététele jelenleg az ügyfeleknek közzétett szolgáló webhely. Ehelyett hozzunk létre egy új mobilalkalmazás másolataként kiszolgáló. Ez az alkalmazás további pénzügyi költség felmerülése elkerülése ugyanazon alkalmazás szolgáltatás terven elhelyezheti.

Ezután meg kell az alkalmazás két verziója: a helyettesítő, és a többi, amelyek ezután frissítheti, illetve egy új ügyfél verziójának megjelenése cél az alkalmazások változatlan marad, és szolgál egyik közzé. Áthelyezheti, és tesztelje a kódot a azt szeretné, de gondoskodnia kell arról, hogy bármely hibajavítás, akkor alkalmazza is. Ha úgy érzi, hogy a helyettesítő alkalmazásainak a legújabb verzióra frissítette ügyfél táblázatcellák kívánt számát, ha, törölheti az eredeti áttelepített alkalmazás kívánt.

A frissítési folyamat esetében a teljes vázlat van az alábbi képlettel történik:

1. Hozzon létre egy új Mobile alkalmazásban
2. Az új kiszolgáló SDK használni a projekt frissítése
3. Az ügyfél alkalmazás új kiadását
4. (Nem kötelező) Az eredeti áttelepített példány törlése

##<a name="mobile-app-version"></a>A másodfokú alkalmazás létrehozása
Az első a frissítés lépésként hozzon létre a mobilalkalmazás erőforrást, amely az alkalmazás új verziója fog tárolni. Ha egy meglévő mobilszolgáltatás már áttelepítette, fog szeretne létrehozni verziójában szolgáltatója ugyanarra a csomagra szóló. Nyissa meg az [Azure-portálra] , és nyissa meg az áttelepített alkalmazás. Jegyezze fel az alkalmazás szolgáltatás terv működik.

Ezután hozzon létre az alkalmazás a másodfokú [.NET kódmentes létrehozásának utasításait](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#create-app)követve. Amikor a rendszer kéri, válassza a alkalmazás szolgáltatáscsomagja vagy a "szolgáltatója terv" Válassza ki a az áttelepített alkalmazás a csomagot.

Valószínűleg kívánt ugyanazt az adatbázis és értesítés központi használja, mint a mobil szolgáltatások. [Azure portál] megnyitása és Navigálás az eredeti alkalmazás másolásához ezeket az értékeket, majd kattintson a **Beállítások** > **alkalmazás beállításait**. A **Kapcsolati karakterláncot**, másolja a vágólapra `MS_NotificationHubConnectionString` és `MS_TableConnectionString`. Nyissa meg az új frissítési webhelyet, és illessze be őket, bármelyik meglévő értékeinek felülírása. Ismételje meg ezt a folyamatot minden más Alkalmazásbeállítások alkalmazás igényeinek megfelelően. Ha nem az áttelepített szolgáltatás, erről a csatlakozási_karakterlánc és az alkalmazás beállításainak a **beállítás** lapon az [Azure klasszikus portál]Mobile szolgáltatások része.

Az alkalmazás az ASP.NET projekt másolatot készíteni, és közzéteheti az új webhelyre. Egy példányát a ügyfélalkalmazás frissíteni az új URL-címmel használata esetén ellenőrizze, hogy minden a várt módon működik.

## <a name="updating-the-server-project"></a>A kiszolgáló projekt frissítése

Mobile-alkalmazások tartalmaz egy új [Mobile alkalmazást Server SDK] nagy része megegyezik a Mobile futtatókörnyezetének funkcióval nyújt. Első lépésként el kell távolítania a Mobile Services csomagok mutató összes hivatkozást. A NuGet csomag manager, keressen rá a `WindowsAzure.MobileServices.Backend`. A legtöbb alkalmazások jelenik több csomagok ebben az esetben, beleértve a `WindowsAzure.MobileServices.Backend.Tables` és `WindowsAzure.MobileServices.Backend.Entity`. Ebben az esetben, indítsa el a függőség fában legalacsonyabb csomaggal például `Entity`, és távolítsa el azt. Amikor a rendszer kéri, az összes függő csomagok nem távolítja el. Ismételje meg ezt a folyamatot, amíg eltávolítása `WindowsAzure.MobileServices.Backend` magát.

Ezen a ponton, amely már nem hivatkozik Mobile szolgáltatások SDK projekt lesz.

Ezután ad hozzá hivatkozást a Mobile alkalmazások SDK számára. A frissítés, a legtöbb fejlesztők is szükség van töltse le és telepítse a `Microsoft.Azure.Mobile.Server.Quickstart` csomag, mint ez lesz lekérje a teljes szükséges megadása.

A SDK közötti különbségek eredő igazán néhány fordítási hibák lesz, de ezek egyszerűen címet, és ebben a szakaszban a többi tartoznak.

### <a name="base-configuration"></a>Alapkonfiguráció

Ezután a WebApiConfig.cs, cserélheti le:

        // Use this class to set configuration options for your mobile service
        ConfigOptions options = new ConfigOptions();

        // Use this class to set WebAPI configuration options
        HttpConfiguration config = ServiceConfig.Initialize(new ConfigBuilder(options));

a

        HttpConfiguration config = new HttpConfiguration();
        new MobileAppConfiguration()
            .UseDefaultConfiguration()
        .ApplyTo(config);

>[AZURE.NOTE] Ha szeretne többet szeretne tudni az új .NET-kiszolgáló SDK és a Hozzáadás vagy eltávolítás az alkalmazás hogyan, kérjük, témakört [használatáról a .NET-kiszolgáló SDK csomagjában talál] .

Ha az alkalmazás használatát a hitelesítési szolgáltatások, is szüksége lesz egy OWIN köztes regisztrálni. Ebben az esetben a fenti konfiguráció kódot kell áthelyezése egy új OWIN indítási osztály.

1. Adja hozzá a NuGet csomag `Microsoft.Owin.Host.SystemWeb` Ha ez nem már szerepel a projekt.
2. A Visual Studióban, kattintson a projekt a jobb gombbal, és válassza a **Hozzáadás** -> **Új elemet**. Jelölje be a **webes** -> **Általános** -> **OWIN indítási osztály**.
3. A fenti kóddal áthelyezése a MobileAppConfiguration `WebApiConfig.Register()` szeretne a `Configuration()` az új indítási osztályához metódusát.

Győződjön meg arról, hogy a `Configuration()` módszer, vége pedig:

        app.UseWebApi(config)
        app.UseAppServiceAuthentication(config);

Nincsenek hitelesítéssel kapcsolatos további módosításokat, amelyek szerepelnek a teljes hitelesítési szakaszban.

### <a name="working-with-data"></a>Adatkezelés

A mobil szolgáltatások mobile-alkalmazás nevét a entitás keretrendszer beállítása alapértelmezett séma neveként fel.

Annak érdekében, hogy rendelkezik-e ugyanarra a sémára hivatkozott előtti, beállíthatja a séma a DbContext az alkalmazás a következő használata:

        string schema = System.Configuration.ConfigurationManager.AppSettings.Get("MS_MobileServiceName");

Győződjön meg arról, hogy beállítása, ha nem tesz a fenti MS_MobileServiceName. Is megadhat egy másik séma neve, ha az alkalmazás egyéni a korábban.

### <a name="system-properties"></a>A Rendszertulajdonságok

#### <a name="naming"></a>Elnevezése

Azure Mobile szolgáltatások kiszolgálón SDK Rendszertulajdonságok mindig tartalmazza a dupla aláhúzás (`__`) előtag az a tulajdonságokat:

- __createdAt
- __updatedAt
- __deleted
- __version

A mobil ügyfélprogram SDK speciális logikát elemzéséhez Rendszertulajdonságok ebben a formátumban van.

Az Azure Mobile-alkalmazások Rendszertulajdonságok már nem egy speciális formátum pedig a következő nevek:

- createdAt
- updatedAt
- törölt
- verzió

A Mobile-alkalmazások ügyfél SDK használatával az új rendszer tulajdonságok nevek, hogy a módosítások szükségesek ügyfél kódot. Jó helyen jár Ha közvetlenül a szolgáltatásban többi hívások majd módosítania kell a lekérdezések megfelelően.

#### <a name="local-store"></a>Helyi tárolóba

A Rendszertulajdonságok azoknak a módosítások jelenti, hogy egy kapcsolat nélküli szinkronizálás helyi adatbázis Mobile szolgáltatások nem kompatibilis a Mobile-alkalmazások. Lehetőleg ne alkalmazások Mobile szolgáltatásokból után addig, amíg a Mobile alkalmazások függőben lévő változásokat a kiszolgálóra küldött ügyfélprogram frissítése. Ezután a frissített alkalmazás egy új adatbázis fájlneve kell használni.

Ha egy ügyfél app van frissített Mobile szolgáltatásokból a Mobile-alkalmazások közben függő offline módosítások vannak a művelet várakozási sorban található, majd a rendszer adatbázis kell frissíteni az új oszlop neveket használják. A iOS Ez lehet elérni könnyű áttelepítések használatával az oszlopnevek módosítása. Android-alapú és a .NET felügyelt ügyfél az adattáblák objektum oszlopainak átnevezése egyéni SQL kell írhat.

A iOS módosítania kell a Core adatszerkezet az adatok entitás megfelelően a következőket. Megjegyzendő, hogy az tulajdonságokat `createdAt`, `updatedAt` és `version` nincs már egy `ms_` előtag:

| Attribútum |  Típus   | Megjegyzés:                                                 |
|---------- |  ------ | -----------------------------------------------------|
| azonosító        | Karakterlánc, a kötelezőként megjelölt  | elsődleges kulcs a távoli áruházból         |
| createdAt | Dátum    | (nem kötelező) térképek createdAt rendszer tulajdonság         |
| updatedAt | Dátum    | (nem kötelező) térképek updatedAt rendszer tulajdonság         |
| verzió   | Karakterlánc  | (nem kötelező) használt feltárása ütközéseket, térképek verzióra |

#### <a name="querying-system-properties"></a>A Rendszertulajdonságok lekérdezése

Azure Mobile szolgáltatások – Rendszertulajdonságok küldi el a program alapértelmezés szerint, de csak akkor, ha azt kérték a lekérdezés karakterlánccal `__systemProperties`. Viszont az Azure Mobile-alkalmazások rendszer tulajdonságok nem **mindig legyen kijelölve** , mert a kiszolgáló SDK objektummodell részét képezik.

Ez a változás főként hatással van a tartomány menedzserek, például a bővítmények egyéni példányait `MappedEntityDomainManager`. A mobil szolgáltatások, ha egy ügyfél soha nem kéri a bármely Rendszertulajdonságok ajánlatos használata egy `MappedEntityDomainManager` , amely nem ténylegesen összes-tulajdonságok megfeleltetése. Azonban az Azure Mobile-alkalmazások, a nem megfeleltetett tulajdonságokból hibát okoz a GET-lekérdezések.

A probléma megoldásához legkönnyebben, hogy azok örökölhet a DTOs módosítására `ITableData` helyett `EntityData`. Ezután adja hozzá a `[NotMapped]` attribútum azon mezői, amelyek megadni.

Ha például a következő határozza meg `TodoItem` nincs rendszer tulajdonságok:

    using System.ComponentModel.DataAnnotations.Schema;

    public class TodoItem : ITableData
    {
        public string Text { get; set; }

        public bool Complete { get; set; }

        public string Id { get; set; }

        [NotMapped]
        public DateTimeOffset? CreatedAt { get; set; }

        [NotMapped]
        public DateTimeOffset? UpdatedAt { get; set; }

        [NotMapped]
        public bool Deleted { get; set; }

        [NotMapped]
        public byte[] Version { get; set; }
    }

Megjegyzés: Ha hibák `NotMapped`, az összeállítás mutató hivatkozás hozzáadása `System.ComponentModel.DataAnnotations`.

### <a name="cors"></a>CORS

Mobil szolgáltatások néhány CORS támogatása kategóriákat: az ASP.NET CORS megoldás Körbefuttatás. A sortörés réteghez adhat az fejlesztője több szabályozási lehetőségért így közvetlenül is élvezheti [ASP.NET CORS támogatása](http://www.asp.net/web-api/overview/security/enabling-cross-origin-requests-in-web-api)megszűnt.

A főbb területek CORS használata, amelyek a `eTag` és `Location` fejlécek kell tenni ahhoz, hogy az ügyfél SDK működik megfelelően.

### <a name="push-notifications"></a>Leküldéses értesítések
A leküldéses a fő, akkor azt tapasztalhatja hiányzik a kiszolgáló SDK csomagjában talál az elem a PushRegistrationHandler osztály. Regisztráció kissé másképp történő Mobile-alkalmazások, és tagless regisztrációk alapértelmezés szerint engedélyezve van. Címkék kezelése végezhető egyéni API-khoz használatával. Kérjük, olvassa el a további információt [a címkék regisztrálása](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#tags) útmutatót.

### <a name="scheduled-jobs"></a>Ütemezett feladat
Ütemezett feladat nem beépített Mobile-alkalmazások, így minden meglévő feladatok a .NET kódmentes rendelkező külön-külön kell frissíteni kell. Egy, hogy egy ütemezett [Webes feladat] létrehozása a mobilalkalmazás kód webhelyen. Képes egy vezérlő, amely rendelkezik a feladat kód beállítása, és állítsa be az [Azure ütemező] találati végpontra a várt ütemtervben.

### <a name="miscellaneous-changes"></a>Egyéb változások
Az összes ApiControllers, amely a mobil ügyfélalkalmazásba fogyni fog most már rendelkeznie kell a `[MobileAppController]` attribútum. Most már nem szeretne lépni a mobil formatters érinti, hogy más ApiControllers alapértelmezés szerint látható.

A `ApiServices` objektum már nem része a SDK csomagjában talál. Ha hozzá szeretne férni Mobile alkalmazásban, az alábbi használhatja:

    MobileAppSettingsDictionary settings = this.Configuration.GetMobileAppSettingsProvider().GetMobileAppSettings();

Hasonlóképpen naplózás most hajtható végre a szabványos ASP.NET nyomkövetési írási használata:

    ITraceWriter traceWriter = this.Configuration.Services.GetTraceWriter();
    traceWriter.Info("Hello, World");  

##<a name="authentication"></a>Hitelesítés kapcsolatos szempontok

A mobil szolgáltatások hitelesítési összetevői most át lett helyezve alkalmazás szolgáltatás hitelesítési és engedélyezési funkciójához. Ez a webhely engedélyezése a [mobile-alkalmazás hozzáadása a hitelesítés](app-service-mobile-ios-get-started-users.md) kapcsolódó témakörök megismerheti.

Egyes szolgáltatók AAD, a Facebook és a Google, például látnia kell a levelezőprogramból másolása a meglévő regisztráció használhatja. Egyszerűen kell nyissa meg a identitásszolgáltató portált, és új átirányítási URL-cím hozzáadása a regisztráció. Alkalmazás szolgáltatás hitelesítési és engedélyezési beállította az ügyfél-azonosító és a titkos.

### <a name="controller-action-authorization"></a>Vezérlő művelet engedélyezése
Minden előfordulását a `[AuthorizeLevel(AuthorizationLevel.User)]` attribútum most módosítható a szabványos ASP.NET használandó `[Authorize]` attribútum. Ezenkívül vezérlők most névtelen alapértelmezés szerint ki legyenek, ahogy más ASP.NET-alkalmazások.
Ha használta a többi AuthorizeLevel beállításokat, például felügyeleti vagy program irányítását, tartsa szem előtt, hogy ezek nincsenek benne. Ehelyett a következők AuthorizationFilters beállítása, vagy egy AAD szolgáltatás egyszerű ahhoz, hogy a szolgáltatás hívások biztonságosan konfigurálása.

### <a name="getting-additional-user-information"></a>További felhasználói adatok beolvasása

További felhasználói adatait, beleértve az hozzáférési jogkivonat keresztül elérheti a `GetAppServiceIdentityAsync()` módszer:

        FacebookCredentials creds = await this.User.GetAppServiceIdentityAsync<FacebookCredentials>();

Ezenkívül ha az alkalmazás függőségek tart a felhasználói azonosítók, például az őket tároló adatbázisban, fontos ügyeljen arra, hogy a felhasználói azonosítókhoz Mobile Services és az alkalmazás szolgáltatás Mobile-alkalmazások között különböző. A mobil szolgáltatások felhasználói Azonosítójával, azonban továbbra is elérheti. Valamennyi ProviderCredentials alosztályát van felhasználóazonosító tulajdonságait. Ezért a folytatás előtt példából:

        string mobileServicesUserId = creds.Provider + ":" + creds.UserId;

Ha az alkalmazás bármely függőségek végrehajthat felhasználói azonosítók, fontos, hogy akkor kihasználhatja ugyanazt a regisztrációt tel Ha lehetséges. Felhasználói azonosítók általában tükrözik az alkalmazás regisztrációs használt, így egy új bejegyzés bemutatása hozható létre felhasználói adataik egyező problémáit.

### <a name="custom-authentication"></a>Egyéni hitelesítési

Ha az alkalmazás az egyéni hitelesítési megoldást, kívánt győződjön meg róla, hogy a frissített webhely hozzáfér a rendszer. A megoldás integrálása a [.NET-kiszolgálói SDK áttekintése] egyéni hitelesítési új utasításokat követve. Felhívjuk a figyelmét arra, hogy az egyéni hitelesítési összetevői továbbra is a előzetes verzióban.

##<a name="updating-clients"></a>Ügyfelek frissítése
Ha már egy műveleti mobilalkalmazás kódmentes, az ügyfélalkalmazás, amely fogyasztása azt az új verziójában dolgozhat. Mobile-alkalmazások is tartalmazza az ügyfél SDK új verziót, és a kiszolgáló frissítés feletti hasonló, meg kell a Mobile-alkalmazások verzió telepítése előtt távolítsa el a a mobil szolgáltatások SDK mutató összes hivatkozást.

A legfontosabb változások a verziói között egyik, hogy a konstruktorok már nem szükséges egy helyi menüt megjelenítő billentyűt. Most már egyszerűen adja meg a mobilalkalmazás URL-címét. Például a a .NET-ügyfelek a `MobileServiceClient` konstruktor ettől kezdve:

        public static MobileServiceClient MobileService = new MobileServiceClient(
            "https://contoso.azurewebsites.net", // URL of the Mobile App
        );

Az új SDK telepítésével és használatával a keresztül az alábbi hivatkozásokat új struktúráját olvashat:

- [iOS verzió 3.0.0 vagy újabb verzió](app-service-mobile-ios-how-to-use-client-library.md)
- [.NET (Windows/Xamarin) verzió 2.0.0 vagy újabb verzió](app-service-mobile-dotnet-how-to-use-client-library.md)

Ha az alkalmazás használata a leküldéses értesítéseket, jegyezze fel a regisztráció utasítás platformokon készíti, az eddig néhány változtatásokat, valamint.

Ha készen áll az új ügyfél verziót, próbálja ki a frissített kiszolgáló projekthez ellen. Után ellenőrzése, hogy működik-e, engedje fel az ügyfelek alkalmazás új verziója. Végül után az ügyfelekkel megkapja ezeket a frissítéseket a névjegyadatok kellett volna, törölheti az alkalmazás Mobile szolgáltatások verzióját. Ezen a ponton teljesen frissítette-kiszolgálóval a legújabb Mobile-alkalmazások SDK alkalmazás szolgáltatás Mobile alkalmazásban.

<!-- URLs. -->

[Azure portál]: https://portal.azure.com/
[Azure klasszikus portál]: https://manage.windowsazure.com/
[Mik azok a Mobile-alkalmazások?]: app-service-mobile-value-prop.md
[I already use web sites and mobile services – how does App Service help me?]: /en-us/documentation/articles/app-service-mobile-value-prop-migration-from-mobile-services
[Mobil App kiszolgálója SDK]: http://www.nuget.org/packages/microsoft.azure.mobile.server
[Create a Mobile App]: app-service-mobile-xamarin-ios-get-started.md
[Add push notifications to your mobile app]: app-service-mobile-xamarin-ios-get-started-push.md
[Add authentication to your mobile app]: app-service-mobile-xamarin-ios-get-started-users.md
[Azure ütemező]: /en-us/documentation/services/scheduler/
[Webes feladat]: ../app-service-web/websites-webjobs-resources.md
[A .NET-kiszolgáló SDK használata]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Migrate from Mobile Services to an App Service Mobile App]: app-service-mobile-migrating-from-mobile-services.md
[Migrate your existing Mobile Service to App Service]: app-service-mobile-migrating-from-mobile-services.md
[Árak alkalmazás szolgáltatás]: https://azure.microsoft.com/en-us/pricing/details/app-service/
[.NET-kiszolgáló SDK – áttekintés]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
