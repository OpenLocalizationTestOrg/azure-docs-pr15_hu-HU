<properties
    pageTitle="GEO kapcsolódó leküldéses értesítéseket Azure értesítés hubok és térbeli adatok Bing |} Microsoft Azure"
    description="Ebből az oktatóanyagból megtanulhatja helyfüggő leküldéses értesítéseket Azure értesítés hubok és térbeli adatok Bing ad."
    services="notification-hubs"
    documentationCenter="windows"
    keywords="leküldéses értesítést, leküldéses értesítést"
    authors="dend"
    manager="yuaxu"
    editor="dend"/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows-phone"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="05/31/2016"
    ms.author="dendeli"/>
    
# <a name="geo-fenced-push-notifications-with-azure-notification-hubs-and-bing-spatial-data"></a>GEO kapcsolódó leküldéses értesítéseket Azure értesítés hubok és a Bing térbeli adatokkal
 
 > [AZURE.NOTE] Ebben az oktatóanyagban befejezéséhez, az aktív Azure fiókkal kell rendelkezniük. Nem rendelkeznek fiókkal, ha mindössze néhány perc is létrehozhat ingyenes próba-fiók. A részletekért lásd: [Azure ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02).

Ebből az oktatóanyagból megtanulhatja helyfüggő leküldéses értesítéseket Azure értesítés hubok és a Bing térbeli adatokkal, a egy univerzális Windows platformra alkalmazáson belül kapcsolatos károkozásra ad.

##<a name="prerequisites"></a>Előfeltételek
Mindenekelőtt és elsősorban győződjön meg arról, hogy minden a szoftver- és szolgáltatáslista előtti követelmény van szükség:

* [Visual Studio 2015 frissítés 1](https://www.visualstudio.com/en-us/downloads/download-visual-studio-vs.aspx) vagy újabb (a[Közösségi Edition](https://go.microsoft.com/fwlink/?LinkId=691978&clcid=0x409) , valamint hajt végre). 
* Az [Azure SDK](https://azure.microsoft.com/downloads/)legújabb verzióját. 
* [A Bing Maps fejlesztői központ fiók](https://www.bingmapsportal.com/) (hozzon létre egyet ingyen, és azt társítása a Microsoft-fiók). 

##<a name="getting-started"></a>Első lépések

Első lépésként a projekt létrehozása. A Visual Studióban egy új projekt típusú **Üres alkalmazás (Universal Windows)**.

![](./media/notification-hubs-geofence/notification-hubs-create-blank-app.png)

Miután befejeződött a projekt létrehozása, rendelkeznie kell a kábelezés magát az alkalmazás. Most vegyük beállítása a geo-kerítés infrastruktúrájának mindent. A Bing szolgáltatások használatához fogjuk, mert nem egy nyilvános REST API-végpontot, amely lehetővé teszi, hogy számunkra, hogy a lekérdezés adott pontjára keretek:

    http://spatial.virtualearth.net/REST/v1/data/
    
Meg kell adja meg a következő paraméterek kell megtenni:

* **Adatforrás-azonosító** , és az **Adatforrás neve** – a Bing Maps API, adatforrásokat tartalmaz különböző bucketed metaadatokat, például a helyek és a tevékenység munkaidei. Bővebb információ ezeket itt. 
* **Szervezet neve** – a szervezet bejelentésére szolgáló kiindulópontként használni kívánt. 
* **A Bing Maps API -ja kulcs** – Ez az a billentyűt, amely a korábban kapott a Bing fejlesztői központ fiók létrehozásakor.
 
Vegyük végezze el a mély-merülési a beállítása az egyes a fenti elemeket.

##<a name="setting-up-the-data-source"></a>Az adatforrás beállítása

A Bing Maps fejlesztői központ a műveleteket hajthat végre. Egyszerűen **adatforrások** parancsára a felső navigációs sávon, és válassza ki az **Adatforrások kezelése**.

![](./media/notification-hubs-geofence/bing-maps-manage-data.png)

Ha nem dolgozott előtt a Bing Maps API-val, valószínűleg nincs nem bemutató, adatforrások, csak hozzon létre egy új adatforrás adatokon Feltöltés gombra kattintva. Győződjön meg arról, hogy adja meg az összes szükséges mezőt:

![](./media/notification-hubs-geofence/bing-maps-create-data.png)

Felmerülhet – Mi az, hogy az adatokat tartalmazó fájl, és mit kell meg kell feltöltése? A tesztet alkalmazásában csak ábrázolásakor a minta függőleges vonás-alapú, amely a San Francisco waterfront terület kereteket:

    Bing Spatial Data Services, 1.0, TestBoundaries
    EntityID(Edm.String,primaryKey)|Name(Edm.String)|Longitude(Edm.Double)|Latitude(Edm.Double)|Boundary(Edm.Geography)
    1|SanFranciscoPier|||POLYGON ((-122.389825 37.776598,-122.389438 37.773087,-122.381885 37.771849,-122.382186 37.777022,-122.389825 37.776598))
    
A fenti entitás jelöli meg:

![](./media/notification-hubs-geofence/bing-maps-geofence.png)

Egyszerűen másolja és illessze be egy új fájlt a fenti szövegben és **NotificationHubsGeofence.pipe**mentheti, és töltse fel a Bing fejlesztői központ.

>[AZURE.NOTE]Kérheti adja meg az új kulcsot a **fő kulcs** eltér a **Lekérdezés billentyűt**. Egyszerűen hozzon létre egy új kulcsot az irányítópulton keresztül, és frissítse a adatforrás feltöltés lapon.

Miután az adatokat tartalmazó fájl feltöltése, szüksége lesz, hogy az adatforrás közzé. 

Nyissa meg **Az adatforrások kezelése**, ugyanúgy, mint azt fent elvégzett, keresse meg a adatforrást a listában, és kattintson a **Közzététel** az **Műveletek** oszlop. Az egy kicsit, meg kell jelennie a **Közzétett adatforrások** lapon az adatforrás:

![](./media/notification-hubs-geofence/bing-maps-published-data.png)

Ha **szerkesztése**gombra kattint, lesz egy pillantással milyen azt jelent meg a benne lévő helyek:

![](./media/notification-hubs-geofence/bing-maps-data-details.png)

Ezen a ponton a portálon Ön nem látható a határokat esetében a geofence, hogy a létrehozott – szükség egy megerősítést kérő, amely a megadott helyre a megfelelő közelében.

Most már van az adatforrásban a követelményei. Tekintheti meg a részleteket a kérelem URL-CÍMÉT az API-híváshoz a Bing Maps fejlesztői központ, kattintson az **adatforrások** elemre, és válassza az **Adatforrás adatait**.

![](./media/notification-hubs-geofence/bing-maps-data-info.png)

A **Lekérdezés URL-címe** , mit után itt vagyunk. Ez a szemben, amely azt ellenőrizze, hogy az eszköz nem jelenleg található hely határértékei-e a lekérdezés végrehajtása végpontot. Ez az ellenőrzés végrehajtásához egyszerűen szükség szemben a lekérdezés URL-címe GET hívás végrehajtása hozzáfűzi az alábbi paraméterekkel:

    ?spatialFilter=intersects(%27POINT%20LONGITUDE%20LATITUDE)%27)&$format=json&key=QUERY_KEY

Úgy, hogy a célhely ad meg, amely azt az eszközről beszerzése mutasson, és a Bing Maps automatikusan végez a számítások megtudhatja, hogy a geofence belül. Miután a böngészőben (vagy cURL) keresztül kérés végrehajtása, jelenik meg szabványos JSON választ:

![](./media/notification-hubs-geofence/bing-maps-json.png)

A válasz csak akkor történik, ha a pont a kijelölt határai om, pedig van. Ha nem, egy üres **eredményt** időszakot jelenik meg:

![](./media/notification-hubs-geofence/bing-maps-nores.png)

##<a name="setting-up-the-uwp-application"></a>A UWP alkalmazás beállítása

Most, hogy elkészült az adatforrás készen áll arra, hogy megkezdheti a munkát azt korábbi bootstrapped UWP alkalmazásáról.

Először és elsősorban azt engedélyeznie kell az alkalmazás hely szolgáltatásokat. Ehhez kattintson duplán a `Package.appxmanifest` fájl **Megoldás Explorerben**.

![](./media/notification-hubs-geofence/vs-package-manifest.png)

A csomag Tulajdonságok lapon, amely éppen megnyitott **Funkciók** parancsára, és győződjön meg arról, hogy a **hely**kiválasztása:

![](./media/notification-hubs-geofence/vs-package-location.png)

A hely képesség deklarálva van, mint új mappa létrehozása a megoldás nevű `Core`, és vegyen fel egy új fájlt benne nevű `LocationHelper.cs`:

![](./media/notification-hubs-geofence/vs-location-helper.png)

A `LocationHelper` magát osztály meglehetősen egyszerű ezen a ponton – hajtja végre mindössze lehetővé számunkra, hogy a felhasználói hely, a rendszer API keresztül beszerzése:

    using System;
    using System.Threading.Tasks;
    using Windows.Devices.Geolocation;

    namespace NotificationHubs.Geofence.Core
    {
        public class LocationHelper
        {
            private static readonly uint AppDesiredAccuracyInMeters = 10;

            public async static Task<Geoposition> GetCurrentLocation()
            {
                var accessStatus = await Geolocator.RequestAccessAsync();
                switch (accessStatus)
                {
                    case GeolocationAccessStatus.Allowed:
                        {
                            Geolocator geolocator = new Geolocator { DesiredAccuracyInMeters = AppDesiredAccuracyInMeters };

                            return await geolocator.GetGeopositionAsync();
                        }
                    default:
                        {
                            return null;
                        }
                }
            }

        }
    }

Erről további tudnivalók a felhasználói hely a hivatalos [MSDN-dokumentum](https://msdn.microsoft.com/library/windows/apps/mt219698.aspx)UWP alkalmazás első.

Ellenőrizze, hogy a hely beszerzése valójában jól működik, nyissa meg a fő lap szélén lévő kódot (`MainPage.xaml.cs`). Hozzon létre egy új eseménykezelő a `Loaded` esemény a `MainPage` konstruktor:

    public MainPage()
    {
        this.InitializeComponent();
        this.Loaded += MainPage_Loaded;
    }

Eseménykezelő végrehajtásának van az alábbi képlettel történik:

    private async void MainPage_Loaded(object sender, RoutedEventArgs e)
    {
        var location = await LocationHelper.GetCurrentLocation();

        if (location != null)
        {
            Debug.WriteLine(string.Concat(location.Coordinate.Longitude,
                " ", location.Coordinate.Latitude));
        }
    }

Figyelje meg, hogy azt deklarálva a kezelő aszinkron, mert `GetCurrentLocation` awaitable, és ezért csak egy aszinkron környezetben hajtja végre. Is mert bizonyos körülmények között azt előfordulhat, hogy a végeredmény egy üres helyre (például a hely szolgáltatások le vannak tiltva, vagy az alkalmazás megtagadva helyre hozzáférési engedélyek), szükség győződjön meg arról, hogy ez megfelelően kezelése egy null ellenőrzéssel.

Futtassa az alkalmazást. Ügyeljen arra, hogy hely hozzáférést:

![](./media/notification-hubs-geofence/notification-hubs-location-access.png)

Egyszer az alkalmazás indítást, akkor láthatja lásd: a koordináták a **kimeneti** ablakban:

![](./media/notification-hubs-geofence/notification-hubs-location-output.png)

Most már tudja, hogy hely WIA works – nyugodtan az próba eseménykezelő eltávolítása a betöltve, mert azt nem lehet használja eltűnt.

A következő lépésként rögzítési helye módosításokat. Az adott, térjen vissza a a `LocationHelper` osztály, és adja hozzá a eseménykezelő `PositionChanged`:

    geolocator.PositionChanged += Geolocator_PositionChanged;

Végrehajtása a hely koordináták megjelennek a **kimeneti** ablakban:

    private static async void Geolocator_PositionChanged(Geolocator sender, PositionChangedEventArgs args)
    {
        await CoreApplication.MainView.CoreWindow.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
        {
            Debug.WriteLine(string.Concat(args.Position.Coordinate.Longitude, " ", args.Position.Coordinate.Latitude));
        });
    }

##<a name="setting-up-the-backend"></a>A kódmentes beállítása

Töltse le a [.NET Kódmentes mintája GitHub](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/NotifyUsers). A letöltés befejeződése után nyissa meg a `NotifyUsers` mappát, és ezt követően – a `NotifyUsers.sln` fájlt.

Állítsa a `AppBackend` project a **Projekt indítása** , és indítsa el.

![](./media/notification-hubs-geofence/vs-startup-project.png)

A projekt már be van állítva leküldéses értesítéseket küldeni cél eszközök, így azt kell csak két dolgot kell tennie – cserélheti a megfelelő kapcsolatot karakterlánc-az értesítési-központban, és adja hozzá az értesítés küldése, csak akkor, amikor a felhasználó a geofence esik oszlopazonosító azonosító.

A kapcsolati karakterlánc konfigurálása a `Models` mappa megnyitása `Notifications.cs`. A `NotificationHubClient.CreateClientFromConnectionString` függvény kell tartalmaznia az értesítési központi, amely az [Azure-portálon](https://portal.azure.com) (keresse meg a **Hozzáférési** **Beállítások**fel belül) elérheti vonatkozó információkat. A frissített konfigurációs fájl mentéséhez.

Most már szükség egy modellt, a Bing Maps API eredményhez. A legegyszerűbb módja, kattintson a jobb gombbal a `Models` mappát, **Hozzáadás** > **osztály**. Nevezze el `GeofenceBoundary.cs`. Miután elvégezte az eltávolítást, másolja a JSON azt tárgyalja, hogy az első szakaszban és **szerkesztése**a Visual Studio használja az API-válasz > **Beillesztés** > **Beillesztés JSON osztályok szerint**. 

Ezzel a módszerrel akkor győződjön meg arról, hogy az objektum pontosan a várt módon fog deszerializálható. Az osztály eredő meg kell hasonló:

    namespace AppBackend.Models
    {
        public class Rootobject
        {
            public D d { get; set; }
        }

        public class D
        {
            public string __copyright { get; set; }
            public Result[] results { get; set; }
        }

        public class Result
        {
            public __Metadata __metadata { get; set; }
            public string EntityID { get; set; }
            public string Name { get; set; }
            public float Longitude { get; set; }
            public float Latitude { get; set; }
            public string Boundary { get; set; }
            public string Confidence { get; set; }
            public string Locality { get; set; }
            public string AddressLine { get; set; }
            public string AdminDistrict { get; set; }
            public string CountryRegion { get; set; }
            public string PostalCode { get; set; }
        }

        public class __Metadata
        {
            public string uri { get; set; }
        }
    }

Ezután nyissa meg a `Controllers`  >  `NotificationsController.cs`. Az animáció működését a bejegyzés hívást a cél hosszúság és a szélesség-fiók szükséges. Az adott, egyszerűen hozzáadhatja a két karakterláncot az függvény aláírás – `latitude` és `longitude`.

    public async Task<HttpResponseMessage> Post(string pns, [FromBody]string message, string to_tag, string latitude, string longitude)

Hozzon létre egy új osztály a projekt neve `ApiHelper.cs` – használjuk csatlakozni szeretne, jelölje be a Bing oszlopazonosító útkereszteződéseket mutassanak. Megvalósítása egy `IsPointWithinBounds` függvény jelennek meg:

    public class ApiHelper
    {
        public static readonly string ApiEndpoint = "{YOUR_QUERY_ENDPOINT}?spatialFilter=intersects(%27POINT%20({0}%20{1})%27)&$format=json&key={2}";
        public static readonly string ApiKey = "{YOUR_API_KEY}";

        public static bool IsPointWithinBounds(string longitude,string latitude)
        {
            var json = new WebClient().DownloadString(string.Format(ApiEndpoint, longitude, latitude, ApiKey));
            var result = JsonConvert.DeserializeObject<Rootobject>(json);
            if (result.d.results != null && result.d.results.Count() > 0)
            {
                return true;
            }
            else
            {
                return false;
            }
        }
    }

>[AZURE.NOTE] Ellenőrizze, hogy való helyettesítéséhez az API-végpontot, a lekérdezés URL-címmel kapott korábbi a Bing fejlesztői központ (Ugyanez igaz a API billentyűt). 

Ha a lekérdezés eredményeit, amely azt jelenti, hogy a megadott pont az geofence határain belül, akkor térjen `true`. Ha nincs találat, a Bing azt közli velünk, hogy a pont a keresési keret kívül esik, így azt visszatérési `false`.

Vissza a `NotificationsController.cs`, közvetlenül az állandót előtt ellenőrzés létrehozása:

    if (ApiHelper.IsPointWithinBounds(longitude, latitude))
    {
        switch (pns.ToLower())
        {
            case "wns":
                //// Windows 8.1 / Windows Phone 8.1
                var toast = @"<toast><visual><binding template=""ToastText01""><text id=""1"">" +
                            "From " + user + ": " + message + "</text></binding></visual></toast>";
                outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, userTag);

                // Windows 10 specific Action Center support
                toast = @"<toast><visual><binding template=""ToastGeneric""><text id=""1"">" +
                            "From " + user + ": " + message + "</text></binding></visual></toast>";
                outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, userTag);

                break;
        }
    }

Úgy, hogy az értesítés csak küldi, amikor a pont a határokat esik.

##<a name="testing-push-notifications-in-the-uwp-app"></a>A UWP alkalmazásban tesztelje a leküldéses értesítések

Visszalépés a UWP alkalmazást, akkor most már értesítések tesztelése. Belül a `LocationHelper` osztály, hozzon létre egy új függvény – `SendLocationToBackend`:

    public static async Task SendLocationToBackend(string pns, string userTag, string message, string latitude, string longitude)
    {
        var POST_URL = "http://localhost:8741/api/notifications?pns=" +
            pns + "&to_tag=" + userTag + "&latitude=" + latitude + "&longitude=" + longitude;

        using (var httpClient = new HttpClient())
        {
            try
            {
                await httpClient.PostAsync(POST_URL, new StringContent("\"" + message + "\"",
                    System.Text.Encoding.UTF8, "application/json"));
            }
            catch (Exception ex)
            {
                Debug.WriteLine(ex.Message);
            }
        }
    }

>[AZURE.NOTE] Cserélje a `POST_URL` azt a helyet, a telepített webalkalmazás az előző részben létrehozott. Most figyelmeztetésében helyben futtatni, de a munka közben egy nyilvános verzió telepítéséről, szüksége lesz egy külső szolgáltatójának kell üzemeltetnie azt.

Vegyük most győződjön meg arról, hogy azt regisztrálhatja a leküldéses értesítések UWP alkalmazás. A Visual Studióban, kattintson a **Projekt** > **tárolása** > **társítani a áruházzal alkalmazást**.

![](./media/notification-hubs-geofence/vs-associate-with-store.png)

Jelentkezzen be a Fejlesztőeszközök fiókjába, amint ki a meglévő alkalmazás kijelölése, vagy hozzon létre egy újat és a csomag társítani. 

Nyissa meg a fejlesztői központ, és nyissa meg az imént létrehozott alkalmazást. Kattintson a **szolgáltatások** > **Leküldéses értesítések** > **Live szolgáltatások webhelyet**.

![](./media/notification-hubs-geofence/ms-live-services.png)

A webhelyen a **Titkos alkalmazás** és a **Csomag biztonsági AZONOSÍTÓK**vegye figyelembe. Szüksége lesz a mind az Azure-portálon – nyissa meg a értesítés-központját, és kattintson a **Beállítások**elemre > **Értesítési szolgáltatások** > **Windows (WNS)** , és írja be az adatokat a kötelező mezőket.

![](./media/notification-hubs-geofence/notification-hubs-wns.png)

Kattintson a **Mentés**.

A **hivatkozások** a **Megoldást Intézőben** kattintson a jobb gombbal, és válassza a **NuGet csomagok kezelése**. A **Microsoft Azure Service Bus tár felügyelt** mutató hivatkozás hozzáadása: egyszerűen kereshet lesz szükség `WindowsAzure.Messaging.Managed` , és adja hozzá a projekthez.

![](./media/notification-hubs-geofence/vs-nuget.png)

Tesztelési célú, létrehozhatunk a `MainPage_Loaded` eseménykezelő még egyszer és a kódtöredék hozzáadása:

    var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

    var hub = new NotificationHub("HUB_NAME", "HUB_LISTEN_CONNECTION_STRING");
    var result = await hub.RegisterNativeAsync(channel.Uri);

    // Displays the registration ID so you know it was successful
    if (result.RegistrationId != null)
    {
        Debug.WriteLine("Reg successful.");
    }

Az értesítés-központban a fenti regisztrálja az alkalmazást. Készen áll a küldésre! 

A `LocationHelper`, belső a `Geolocator_PositionChanged` -kezelő is hozzáadhat a helyet a geofence belül kényszerítéssel elhelyezett próba-kód valamilyen:

    await LocationHelper.SendLocationToBackend("wns", "TEST_USER", "TEST", "37.7746", "-122.3858");

Azt nem haladnak a valós koordináták (ez nem lehet a határokat belül pillanatában), és használja az előre definiált vizsgált értékek, mert azt jelennek meg a frissítés értesítés jelenik meg:

![](./media/notification-hubs-geofence/notification-hubs-test-notification.png)

##<a name="whats-next"></a>Mi az következő?

Néhány lépést, kövesse a fenti győződjön meg arról, hogy a megoldás gyártási kész kívül kell.

Mindenekelőtt és elsősorban szükség lehet annak érdekében, hogy geofences dinamikus. Annak érdekében, hogy töltse fel a meglévő adatforrásból belül új határai ehhez néhány további munka, a Bing API-val. További információt a témában, olvassa el a [Bing térbeli adatok szolgáltatások API dokumentációt](https://msdn.microsoft.com/library/ff701734.aspx) .

Második amikor annak érdekében, hogy a kézbesítés történik-e a megfelelő résztvevőnek dolgozik, érdemes lehet keresztül [címkézés](notification-hubs-tags-segment-push-message.md)célba őket.

A fent látható megoldást, amelyben lehet, hogy számos különböző típusú célplatformok, így azt did nem korlátozza a geofencing és a rendszer-specifikus képességek példa mutatja be. Említett, a univerzális Windows platformon [geofences jobb ki az-kész](https://msdn.microsoft.com/windows/uwp/maps-and-location/set-up-a-geofence)feltárása funkciókat kínál.

Kapcsolatos értesítési hubok funkciók további információra kíváncsi tanulmányozza a [dokumentáció portálon](https://azure.microsoft.com/documentation/services/notification-hubs/).
