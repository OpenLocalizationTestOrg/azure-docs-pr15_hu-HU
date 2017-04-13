<properties
    pageTitle="Regisztráció kezelése"
    description="Ebből a témakörből megtudhatja, hogyan lehet rögzíteni az eszközöket az értesítési hubok annak érdekében, hogy a leküldéses értesítéseket."
    services="notification-hubs"
    documentationCenter=".net"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-multiple"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

# <a name="registration-management"></a>Regisztráció kezelése

##<a name="overview"></a>– Áttekintés

Ebből a témakörből megtudhatja, hogyan lehet rögzíteni az eszközöket az értesítési hubok annak érdekében, hogy a leküldéses értesítéseket. A témakör ismerteti a magas szintű regisztrációk, majd a bemutatja, hogy regisztrált eszközök két fő mintázatok: az eszközről közvetlenül az értesítési-központban való regisztrálás, és az alkalmazás kódmentes keresztül regisztráló. 


##<a name="what-is-device-registration"></a>Mi az eszköz regisztrációs

Regisztráció eszköz értesítési hubon a **regisztráció** vagy a **telepítés**hajtható végre.

#### <a name="registrations"></a>Regisztráció
Regisztráció a Platform értesítési szolgáltatás (PNS) leíró eszköz címkék és esetleg a sablon társít. A PNS leíró lehet egy ChannelURI, eszköz jogkivonat vagy GCM regisztrációs azonosítót. Címkék és értesítések irányítja a megfelelő színkészletet eszköz fogópontok használják. További tudnivalókért lásd: [Útválasztás és a címke kifejezések](notification-hubs-tags-segment-push-message.md). Sablonok segítségével regisztrációs transzformáció végrehajtása. További tudnivalókért lásd: [sablonok](notification-hubs-templates-cross-platform-push-messages.md).

#### <a name="installations"></a>Telepítések
Egy másik telepítés egy továbbfejlesztett leküldéses összessége tartalmazó regisztrációs kapcsolódó tulajdonságok. A legújabb és a legjobb megközelítés regisztrálása az eszközökre. Azonban nem támogatja ügyféloldali .NET SDK ([Értesítési központi SDK kódmentes műveletekhez](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/)) még kezdve.  Ez azt jelenti, hogy regisztrál az ügyfél eszközről magát, ha azt szeretné, hogy a [Értesítés hubok REST API](https://msdn.microsoft.com/library/mt621153.aspx) -megközelítés használatával támogatják a telepítést. Ha kódmentes szolgáltatást használ, akkor használhatja az [Értesítési központi SDK kódmentes műveletekhez](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/)kell lennie.

Az alábbiakban néhány fontosabb előnyei telepítések:

* Teljesen idempotent létrehozásakor vagy frissítésekor telepítését. Igen, próbálkozás azt minden ismétlődő regisztrációk kétségei nélkül.
* A telepítés modell egyszerűen végezze el az egyes veremmutatót - célba juttatása eszközre. A rendszer címkét **"$InstallationId: [végrehajtott]"** automatikusan felkerül az egyes alapú telepítés regisztrációs. Egy adott eszköz célba anélkül, hogy végezze el a további coding címke egy Küldés így felhívhatja.
* Használata telepítések is lehetővé teszi a részleges regisztrációs frissítések teendő. A javítás módszerrel a [JSON-javítás standard](https://tools.ietf.org/html/rfc6902)szükséges, a részleges analitikafrissítés telepítés. Ez akkor különösen akkor hasznos, ha a címkét a regisztráció a frissíteni kívánt. Nem kell a teljes regisztráció lehúzható, és küldje el újra az előző címkéket.

Telepítésű tartalmazhat, az a következő tulajdonságokat. A telepítés teljes listája tulajdonságok című témakör [létrehozása és felülírása REST API -t tartalmazó telepítés](https://msdn.microsoft.com/library/azure/mt621153.aspx) vagy [Telepítési tulajdonságok](https://msdn.microsoft.com/library/azure/microsoft.azure.notificationhubs.installation_properties.aspx) a.

    // Example installation format to show some supported properties
    {
        installationId: "",
        expirationTime: "",
        tags: [],
        platform: "",
        pushChannel: "",
        ………
        templates: {
            "templateName1" : {
                body: "",
                tags: [] },
            "templateName2" : {
                body: "",
                // Headers are for Windows Store only
                headers: {
                    "X-WNS-Type": "wns/tile" }
                tags: [] }
        },
        secondaryTiles: {
            "tileId1": {
                pushChannel: "",
                tags: [],
                templates: {
                    "otherTemplate": {
                        bodyTemplate: "",
                        headers: {
                            ... }
                        tags: [] }
                }
            }
        }
    }

 

Fontos tudni, hogy regisztrációk és telepítések alapértelmezés szerint nincs lejár.

Regisztráció és telepítések érvényes PNS leíró a az egyes eszköz csatornák kell tartalmaznia. PNS fogópontok csak szerezhető be az eszközön ügyfél-alkalmazásban, mert egy mintája, közvetlenül az eszközön, hogy az ügyfél alkalmazásban regisztrálni. Kézzel biztonsági megfontolások és a címkék kapcsolódó üzleti logikai előfordulhat, hogy csak az alkalmazás háttéradatbázist az eszköz regisztrációs kezelése. 

#### <a name="templates"></a>Sablonok

Szeretne [sablonok](notification-hubs-templates-cross-platform-push-messages.md)használata, ha az eszköz telepítése is tartsa lenyomva az ujját a JSON az eszköz társított összes sablont formázása (lásd a fenti minta). A sablon nevét a cél különböző sablont ugyanarra az eszközre segítségével.

Megjegyzés: egyes sablonnév rendel egy sablon szervezet és a címkék nem kötelező megadni. Ezenkívül minden egyes platform további tulajdonságai van. (Használatával WNS) a Windows áruházból, és a Windows Phone 8 (használatával MPNS) egy további beállítása fejlécek lehet része a sablont. Az APN, esetében beállíthatja, hogy egy lejárati tulajdonság egy konstans vagy egy sablon kifejezést. A telepítés teljes listája tulajdonságok témakörben, [létrehozása és a többi telepítés felülírása](https://msdn.microsoft.com/library/azure/mt621153.aspx) .

#### <a name="secondary-tiles-for-windows-store-apps"></a>A Windows áruházból alkalmazásokat másodlagos Csempéje

A Windows áruházból ügyfélalkalmazásokban értesítést küld a másodlagos csempék megegyezik az elsődleges egy elküldeni. A telepített is támogatott. Ne feledje, hogy a másodlagos csempék egy másik ChannelUri, és az ügyfél alkalmazásba a SDK átlátszó kezeli.

A SecondaryTiles szótárt használja az azonos TileId, amely a SecondaryTiles-objektum létrehozása a Windows áruházból letölthető.
Az elsődleges ChannelUri együtt a ChannelUris másodlagos csempék közül bármikor módosíthatja. Annak érdekében, hogy a telepítés frissítése az értesítési-központban, az eszköz frissítenie kell őket, az aktuális ChannelUris a másodlagos csempék közül.


##<a name="registration-management-from-the-device"></a>Regisztráció kezelése az eszközről

Ügyfél-alkalmazásokból eszköz regisztrációs kezelésekor a kódmentes felelős csak a értesítést küld. Ügyfélalkalmazások PNS fogópontok naprakészen tartása, és a címkék regisztrálni. Az alábbi képen a minta mutatja be.

![](./media/notification-hubs-registration-management/notification-hubs-registering-on-device.png)

Az eszköz először a PNS leíró beolvassa a PNS, majd regisztrálja közvetlenül az értesítési-központban. Után a regisztráció sikeres, az alkalmazás kódmentes küldhet értesítést a bejegyzés kiválasztásával. További információ arról, hogy miként küldhet értesítést, akkor olvassa el a [Útválasztás és a címke kifejezések](notification-hubs-tags-segment-push-message.md).
Megjegyzés: Ebben az esetben használni kívánt csak figyelje az eszközről a értesítés hubok hozzáférjenek. További tudnivalókért olvassa el a [biztonsági](notification-hubs-push-notification-security.md)című témakört.

Regisztrálás az eszközről a legegyszerűbb módszer, de van néhány hátrányai.
Első hátránya az, hogy egy ügyfél app csak frissítheti a címkék amikor az alkalmazás aktív. Például ha egy felhasználó regisztrálhatja a sport csapatok, egy további címke (például Seahawks) és az első eszközön kapcsolatos címkék két eszközök, a második eszköz nem kap a Seahawks értesítéseket mindaddig, amíg az alkalmazás, a második eszközön másodszori hajtja végre. Általánosságban címkék alakzatokra is befolyással különféle eszközökön, ha a kódmentes címkék kezelni beállítás célszerű.
A második regisztrációs kezelése a ügyfél alkalmazásból hátránya, hogy alkalmazások megtámadott is lehet, mert adott címkékhez a regisztráció összefoglalva kell külön figyelmet, a "címke szintű biztonsági." szakaszban leírtak szerint



#### <a name="example-code-to-register-with-a-notification-hub-from-a-device-using-an-installation"></a>Példa kód egy értesítés hubhoz-telepítést használ, a eszközről regisztrálása 

Most ezt csak a támogatott a [Értesítés hubok REST API -t](https://msdn.microsoft.com/library/mt621153.aspx).

A javítás módszerrel a [JSON-javítás standard](https://tools.ietf.org/html/rfc6902) frissítése a telepítés is használhatja.

    class DeviceInstallation
    {
        public string installationId { get; set; }
        public string platform { get; set; }
        public string pushChannel { get; set; }
        public string[] tags { get; set; }
    }

    private async Task<HttpStatusCode> CreateOrUpdateInstallationAsync(DeviceInstallation deviceInstallation,
         string hubName, string listenConnectionString)
    {
        if (deviceInstallation.installationId == null)
            return HttpStatusCode.BadRequest;

        // Parse connection string (https://msdn.microsoft.com/library/azure/dn495627.aspx)
        ConnectionStringUtility connectionSaSUtil = new ConnectionStringUtility(listenConnectionString);
        string hubResource = "installations/" + deviceInstallation.installationId + "?";
        string apiVersion = "api-version=2015-04";

        // Determine the targetUri that we will sign
        string uri = connectionSaSUtil.Endpoint + hubName + "/" + hubResource + apiVersion;

        //=== Generate SaS Security Token for Authorization header ===
        // See, https://msdn.microsoft.com/library/azure/dn495627.aspx
        string SasToken = connectionSaSUtil.getSaSToken(uri, 60);

        using (var httpClient = new HttpClient())
        {
            string json = JsonConvert.SerializeObject(deviceInstallation);

            httpClient.DefaultRequestHeaders.Add("Authorization", SasToken);

            var response = await httpClient.PutAsync(uri, new StringContent(json, System.Text.Encoding.UTF8, "application/json"));
            return response.StatusCode;
        }
    }

    var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

    string installationId = null;
    var settings = ApplicationData.Current.LocalSettings.Values;

    // If we have not stored a installation id in application data, create and store as application data.
    if (!settings.ContainsKey("__NHInstallationId"))
    {
        installationId = Guid.NewGuid().ToString();
        settings.Add("__NHInstallationId", installationId);
    }

    installationId = (string)settings["__NHInstallationId"];

    var deviceInstallation = new DeviceInstallation
    {
        installationId = installationId,
        platform = "wns",
        pushChannel = channel.Uri,
        //tags = tags.ToArray<string>()
    };

    var statusCode = await CreateOrUpdateInstallationAsync(deviceInstallation, 
                        "<HUBNAME>", "<SHARED LISTEN CONNECTION STRING>");

    if (statusCode != HttpStatusCode.Accepted)
    {
        var dialog = new MessageDialog(statusCode.ToString(), "Registration failed. Installation Id : " + installationId);
        dialog.Commands.Add(new UICommand("OK"));
        await dialog.ShowAsync();
    }
    else
    {
        var dialog = new MessageDialog("Registration successful using installation Id : " + installationId);
        dialog.Commands.Add(new UICommand("OK"));
        await dialog.ShowAsync();
    }

   

#### <a name="example-code-to-register-with-a-notification-hub-from-a-device-using-a-registration"></a>Példa egy értesítés hubon keresztül csatlakozott a eszközről regisztráció segítségével regisztrálhatja kód


Ezeket a módszereket regisztráció létrehozása vagy módosítása egy az eszköz, amelyen neve. Ez azt jelenti, hogy annak érdekében, hogy a fogópont vagy a címkék frissítése, írja felül a teljes regisztráció. Ne feledje, hogy regisztrációk ideiglenes (tranziens), így mindig van, hogy az aktuális címkékkel, amely egy adott eszköz szüksége van egy megbízható áruházból.


    // Initialize the Notification Hub
    NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString(listenConnString, hubName);

    // The Device id from the PNS
    var pushChannel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

    // If you are registering from the client itself, then store this registration id in device
    // storage. Then when the app starts, you can check if a registration id already exists or not before
    // creating.
    var settings = ApplicationData.Current.LocalSettings.Values;

    // If we have not stored a registration id in application data, store in application data.
    if (!settings.ContainsKey("__NHRegistrationId"))
    {
        // make sure there are no existing registrations for this push handle (used for iOS and Android)    
        string newRegistrationId = null;
        var registrations = await hub.GetRegistrationsByChannelAsync(pushChannel.Uri, 100);
        foreach (RegistrationDescription registration in registrations)
        {
            if (newRegistrationId == null)
            {
                newRegistrationId = registration.RegistrationId;
            }
            else
            {
                await hub.DeleteRegistrationAsync(registration);
            }
        }

        newRegistrationId = await hub.CreateRegistrationIdAsync();

        settings.Add("__NHRegistrationId", newRegistrationId);
    }
     
    string regId = (string)settings["__NHRegistrationId"];

    RegistrationDescription registration = new WindowsRegistrationDescription(pushChannel.Uri);
    registration.RegistrationId = regId;
    registration.Tags = new HashSet<string>(YourTags);

    try
    {
        await hub.CreateOrUpdateRegistrationAsync(registration);
    }
    catch (Microsoft.WindowsAzure.Messaging.RegistrationGoneException e)
    {
        settings.Remove("__NHRegistrationId");
    }


## <a name="registration-management-from-a-backend"></a>Az egy kódmentes regisztrációs kezelése

További kódírás regisztrációk kezelni a kódmentes szükséges. Az alkalmazás az eszközről meg kell adnia a frissített PNS kezelni a kódmentes minden alkalommal (együtt a címkék és a sablonok) elindítja az alkalmazást, és a kódmentes Ez az értesítés-központban fogópont frissítenie kell. Az alábbi ábrán a tervezés mutatja be.

![](./media/notification-hubs-registration-management/notification-hubs-registering-on-backend.png)

Regisztrációk kezelni a kódmentes előnyei többek között címkék regisztrációk módosítani, akkor is, ha a megfelelő alkalmazást az eszközön nem aktív, és hitelesítést végezni az ügyfél-alkalmazást, mielőtt felvenne egy címkét a regisztráció lehetősége.


#### <a name="example-code-to-register-with-a-notification-hub-from-a-backend-using-an-installation"></a>Példa egy kódmentes-telepítést használ az értesítési hubon regisztrálása kód

Az ügyfél eszköz továbbra is kap a PNS kitöltőjel használatának és a megfelelő telepítési tulajdonságok előtt, és egy egyéni API felhívja a kódmentes, amely a regisztráció elvégezheti, majd engedélyezheti a címkék stb. A kódmentes is kihasználhatja az [Értesítési központi SDK kódmentes műveletekhez](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).

A javítás módszerrel a [JSON-javítás standard](https://tools.ietf.org/html/rfc6902) frissítése a telepítés is használhatja.
 

    // Initialize the Notification Hub
    NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString(listenConnString, hubName);

    // Custom API on the backend
    public async Task<HttpResponseMessage> Put(DeviceInstallation deviceUpdate)
    {

        Installation installation = new Installation();
        installation.InstallationId = deviceUpdate.InstallationId;
        installation.PushChannel = deviceUpdate.Handle;
        installation.Tags = deviceUpdate.Tags;

        switch (deviceUpdate.Platform)
        {
            case "mpns":
                installation.Platform = NotificationPlatform.Mpns;
                break;
            case "wns":
                installation.Platform = NotificationPlatform.Wns;
                break;
            case "apns":
                installation.Platform = NotificationPlatform.Apns;
                break;
            case "gcm":
                installation.Platform = NotificationPlatform.Gcm;
                break;
            default:
                throw new HttpResponseException(HttpStatusCode.BadRequest);
        }


        // In the backend we can control if a user is allowed to add tags
        //installation.Tags = new List<string>(deviceUpdate.Tags);
        //installation.Tags.Add("username:" + username);

        await hub.CreateOrUpdateInstallationAsync(installation);

        return Request.CreateResponse(HttpStatusCode.OK);
    }


#### <a name="example-code-to-register-with-a-notification-hub-from-a-device-using-a-registration-id"></a>Példa egy értesítés hubon keresztül csatlakozott a eszközről regisztrációs azonosító használatával regisztrálása kód

Ebből az alkalmazás kódmentes a regisztrációk alapműveletek CRUDS végezhetők el. Példa:

    var hub = NotificationHubClient.CreateClientFromConnectionString("{connectionString}", "hubName");
            
    // create a registration description object of the correct type, e.g.
    var reg = new WindowsRegistrationDescription(channelUri, tags);

    // Create
    await hub.CreateRegistrationAsync(reg);

    // Get by id
    var r = await hub.GetRegistrationAsync<RegistrationDescription>("id");

    // update
    r.Tags.Add("myTag");

    // update on hub
    await hub.UpdateRegistrationAsync(r);

    // delete
    await hub.DeleteRegistrationAsync(r);


A kódmentes regisztrációs frissítések közötti feldolgozási kell kezelni. Szolgáltatás Bus optimista feldolgozási vezérlő regisztrációs kezeléséhez nyújt. HTTP szintre, de ez történik, regisztrációs adatkezelési műveletek ETag használatával. Ez a szolgáltatás Microsoft SDKs, amely kivételhibát jelenít meg, ha frissítés elutasítják feldolgozási okokból átlátszó használják. Az alkalmazás kódmentes a felelős ezek a kivételek kezelésével, és újra próbálkozik a frissítést, ha szükséges.