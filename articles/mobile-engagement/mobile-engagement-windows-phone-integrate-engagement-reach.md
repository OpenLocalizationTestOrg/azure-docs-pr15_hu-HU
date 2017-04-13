<properties 
    pageTitle="Windows Phone Silverlight vannak SDK integrációja" 
    description="Azure mobil tetszés szerint elmélyedhet vannak integrálása a Windows Phone a Silverlight-alkalmazások"                    
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-windows-phone" 
    ms.devlang="na" 
    ms.topic="article"
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="windows-phone-silverlight-reach-sdk-integration"></a>Windows Phone Silverlight vannak SDK integrációja

Követniük kell a előtt ez az útmutató a [Windows Phone Silverlight tetszés szerint elmélyedhet SDK integrációs](mobile-engagement-windows-phone-integrate-engagement.md) leírt integrációs eljárással.

##<a name="embed-the-engagement-reach-sdk-into-your-windows-phone-silverlight-project"></a>A tevékenységek elérjen SDK beágyazása egy a Windows Phone-Silverlight-projekt

Nem kell semmit hozzáadása. `EngagementReach`hivatkozások és források már rendelkezik a projektben.

> [AZURE.TIP]  Testre szabhatja a található képek a `Resources` mappát a projekt, különösen a arculatának ikonra (hogy tetszés szerint elmélyedhet ikonra az alapértelmezett).

##<a name="add-the-capabilities"></a>A Funkciók hozzáadása

A tevékenységek elérjen SDK kell néhány további lehetőségeket.

Nyissa meg a `WMAppManifest.xml` fájl, és ügyeljen arra, hogy deklarálva vannak-e az alábbi funkciókat:

-   `ID_CAP_PUSH_NOTIFICATION`
-   `ID_CAP_WEBBROWSERCOMPONENT`

A legelső értesítőjére megjelenítésének engedélyezése a MPNS szolgáltatást használják. A második egy böngészőben tevékenység beágyazása a SDK szolgál.

Szerkesztés a `WMAppManifest.xml` fájlt, és adja hozzá a belül a `<Capabilities />` címke:

    <Capability Name="ID_CAP_PUSH_NOTIFICATION" />
    <Capability Name="ID_CAP_WEBBROWSERCOMPONENT" />

##<a name="enable-the-microsoft-push-notification-service"></a>A Microsoft leküldéses értesítéseket kezelő szolgáltatás engedélyezése

A **Microsoft leküldéses értesítéseket kezelő szolgáltatása** (MPNS néven) használatához a `WMAppManifest.xml` kell rendelkeznie a fájl egy `<App />` a címke egy `Publisher` attribútum állítsuk be a projekt nevére.

##<a name="initialize-the-engagement-reach-sdk"></a>A tevékenységek vannak SDK inicializálni

### <a name="engagement-configuration"></a>Tetszés szerint elmélyedhet konfigurálása

A tevékenységek konfiguráció van központi a a `Resources\EngagementConfiguration.xml` a projekt fájlt.

Adja meg a gyermekektől konfigurációs fájl szerkesztése:

-   *Nem kötelező*, jelzik, hogy aktiválva van, a natív leküldéses (MPNS), vagy a nem közötti `<enableNativePush>` és `</enableNativePush>` címkék (`true` alapértelmezés szerint).
-   *Nem kötelező*, azt jelzik között a leküldéses csatorna nevét `<channelName>` és `</channelName>` címkék, adja meg, hogy jelenleg is-e használni az alkalmazást, vagy hagyja üresen.

Szeretné inkább megadása futásidőben, ha felhívhatja a tetszés szerint elmélyedhet ügynök inicializálni előtt az alábbi módon:

    /* Engagement configuration. */
    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    
    engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";
    
    engagementConfiguration.Reach.EnableNativePush = true;                  
    /* [Optional] whether the native push (MPNS) is activated or not. */
    
    engagementConfiguration.Reach.ChannelName = "YOUR_PUSH_CHANNEL_NAME";   
    /* [Optional] Provide the same channel name that your application may currently use. */
    
    /* Initialize Engagement agent with above configuration. */
    EngagementAgent.Instance.Init(engagementConfiguration);

> [AZURE.TIP] Megadhatja, hogy az alkalmazás a MPNS leküldéses csatorna nevét. Alapértelmezés szerint tetszés szerint elmélyedhet létrehoz egy nevet a appId alapján. Ha nincs szükség a nevét adja meg, kivéve ha a leküldéses csatorna tetszés szerint elmélyedhet kívüli használni kívánja.

### <a name="engagement-initialization"></a>Tetszés szerint elmélyedhet inicializálni

Módosítsa a `App.xaml.cs`:

-   Hozzáadása a `using` kimutatások:

        using Microsoft.Azure.Engagement;

-   Beszúrás `EngagementReach.Instance.Init` csak utána `EngagementAgent.Instance.Init` a `Application_Launching` :

        private void Application_Launching(object sender, LaunchingEventArgs e)
        {
           EngagementAgent.Instance.Init();
           EngagementReach.Instance.Init();
        }

-   Beszúrás `EngagementReach.Instance.OnActivated` a a `Application_Activated` módszer:

        private void Application_Activated(object sender, ActivatedEventArgs e)
        {
           EngagementAgent.Instance.OnActivated(e);
           EngagementReach.Instance.OnActivated(e);
        }

> [AZURE.IMPORTANT] A `EngagementReach.Instance.Init` egy dedikált szálon futó. Nincs teendő saját magának.

##<a name="app-store-submission-considerations"></a>App store Beküldési kapcsolatos szempontok

A Microsoft ró egyes szabályokat, a leküldéses értesítések használata esetén:

A Microsoft [Házirendek alkalmazás] dokumentációjában 2.9 szakasz:

1) Kérje meg a felhasználót, hogy fogadja el a leküldéses értesítéseket. A beállítások, adja hozzá a leküldéses értesítések letiltása lehetőséget.

A EngagementReach objektum kezelése a funkcióról – a/lemondási, két módszert kínál `EnableNativePush()` és `DisableNativePush()`. Például létrehozhat egy beállítást a átállítása beállításainak MPNS engedélyezése vagy letiltása.

Úgy is dönt, hogy inaktiválja MPNS tetszés szerint elmélyedhet beállítása révén\<windows phone-sdk-vannak-konfiguráció\>.

> 2.9.1) az alkalmazás kell először írja le az értesítések kell adni, és **szerezze be a felhasználó engedélye (kiválaszthatják – a)**és **kell mechanizmusa keresztül, amely a felhasználó dönthetek ki a leküldéses értesítések fogadása**. Minden értesítést, feltéve, hogy a Microsoft leküldéses értesítéseket kezelő szolgáltatás használatával követik a leírás, a felhasználónak kell lennie, és minden vonatkozó [Alkalmazás házirendek] meg kell felelnie [ Content Policies] és az [Adott alkalmazás típusú további követelményeknek].

2) Túl sok leküldéses értesítéseket, ne használja. Tetszés szerint elmélyedhet értesítéseket fogja kezelni.

> 2.9.2) az alkalmazás és a Microsoft leküldéses értesítéseket kezelő szolgáltatás használatát kell túlzottan nem a hálózati kapacitás vagy Microsoft leküldéses értesítéseket kezelő szolgáltatása sávszélesség vagy egyéb jogosulatlanul a Windows Phone Microsoft eszköz, illetve más szolgáltatás, amelynek a fölösleges leküldéses értesítéseket, az elfogadható tetszése szerint a Microsoft által meghatározott terhet és kell nem kárt vagy zavarja bármely Microsoft hálózatok vagy kiszolgálók vagy bármely harmadik fél kiszolgálók és hálózatok csatlakozik a Microsoft leküldéses értesítéseket kezelő szolgáltatása.

3) Nem támaszkodik MPNS criticals információkat. Tetszés szerint elmélyedhet MPNS, használ, így a ezt a szabályt az előtér-a tevékenységek belül létrehozott kampányok is érinti.

> 2.9.3) a Microsoft leküldéses értesítéseket kezelő szolgáltatás nem feltétlenül értesítéseket, amelyek a kritikus vagy más módon misszió hatással lehet a élet vagy halál ügyekben küldésére használt, beleértve a kritikus értesítések korlátozás nélkül egy kapcsolódó orvosi eszköz vagy a feltétel. A MICROSOFT KIFEJEZETTEN ELHÁRÍT SEMMIFÉLE, HOGY A MICROSOFT LEKÜLDÉSES ÉRTESÍTÉSEKET KEZELŐ SZOLGÁLTATÁS HASZNÁLATÁT VAGY A MICROSOFT LEKÜLDÉSES ÉRTESÍTÉST SZOLGÁLTATÁS ÉRTESÍTÉSEK KÉZBESÍTÉSÉNEK FOLYAMATOS LESZ, INGYENES VAGY MÁS MÓDON HIBA GARANTÁLT VALÓS IDEJŰ ALAPON TÖRTÉNIK.

**Azt nem garantálja, hogy az alkalmazás továbbítja az ellenőrzési folyamat, ha azt veszi figyelembe a fenti ajánlást.**

##<a name="handle-data-push-optional"></a>Leíró adat leküldéses (nem kötelező)

Ha engedélyezni szeretné a vannak adatok veremmutatót kapni az alkalmazás, a EngagementReach osztály két esemény végrehajtásához van:

    EngagementReach.Instance.DataPushStringReceived += (body) =>
    {
       Debug.WriteLine("String data push message received: " + body);
       return true;
    };
    
    EngagementReach.Instance.DataPushBase64Received += (decodedBody, encodedBody) =>
    {
       Debug.WriteLine("Base64 data push message received: " + encodedBody);
       // Do something useful with decodedBody like updating an image view
       return true;
    };

Láthatja, hogy az egyes módszerek a visszahívás logikai érték beolvasása. Tetszés szerint elmélyedhet után az adatok leküldéses elküldési a visszajelzés küldése a háttéradatbázist. A visszahívási hamis értéket ad vissza, ha a `exit` visszajelzés küldése lesz. Egyéb esetben legyen `action`. Ha nincs visszahívás van állítva, az események, a `drop` visszajelzés visszakerül részvételét.

> [AZURE.WARNING] Nem érkeznek meg az adatok leküldéses Többszörösök visszajelzések részvételét. Ha több kezelők meg egy eseményt, tartsa szem előtt, hogy az utolsó felel meg a visszajelzést szeretne egy küldi. Ebben az esetben javasoljuk, hogy mindig a áttekinthetőbb legyen visszajelzés gyakorló az előtér-elkerülése ugyanazon értékét adja eredményül.

##<a name="customize-ui-optional"></a>(Nem kötelező) felhasználói felület testreszabása

### <a name="first-step"></a>Első lépés

Azt lehetővé teszi, hogy a gyermekektől felhasználói felület testreszabását.

Ehhez hozhat létre egy van a `EngagementReachHandler` osztály.

**Példakódot:**

    using Microsoft.Azure.Engagement;
    
    namespace Example
    {
       internal class ExampleReachHandler : EngagementReachHandler
       {
          // Override EngagementReachHandler methods depending on your needs
       }
    }

Megadhatja a tartalmát a `EngagementReach.Instance.Handler` egyéni objektumának mezőbe a `App.xaml.cs` belül osztály a `Application_Launching` módot.

**Példakódot:**

    private void Application_Launching(object sender, LaunchingEventArgs e)
    {
       // your app initialization 
       EngagementReach.Instance.Handler = new ExampleReachHandler();
       // Engagement Agent and Reach initialization
    }

> [AZURE.NOTE] Alapértelmezés szerint tetszés szerint elmélyedhet használja a saját végrehajtásának `EngagementReachHandler`. Hozzon létre saját nincs, és ha így tesz, nem kell minden módszer felülbírálása. Az alapértelmezett működés található a tetszés szerint elmélyedhet alap objektum kijelöléséhez.

### <a name="layouts"></a>Elrendezések

Alapértelmezés szerint vannak fogja használni a beágyazott erőforrásait a DLL-szolgáltatásfájlját az értesítéseket és a lapok megjelenítése.

Jó helyen jár eldöntheti, a saját erőforrások használatához az alábbi összetevőket a arculatának megfelelően.

Felülbírálhatja `EngagementReachHandler` módszerek a alosztály állapítható meg, hogy tetszés szerint elmélyedhet a elrendezések használata:

**Példakódot:**

    // In your subclass of EngagementReachHandler
    
    public override string GetTextViewAnnouncementUri()
    {
       // return the path of your own xaml
    }
    
    public override string GetWebViewAnnouncementUri()
    {
       // return the path of your own xaml
    }
    
    public override string GetPollUri()
    {
       // return the path of your own xaml
    }
    
    public override EngagementNotificationView CreateNotification(EngagementNotificationViewModel viewModel)
    {
       // return a new instance of your own notification
    }

> [AZURE.TIP] A `CreateNotification` módszer null térhet vissza. Az értesítés nem jelenik meg, és a gyermekektől marketingkampány el lesz távolítva.

Elrendezés példányába leegyszerűsítése kínálunk is, amely saját xaml, a kód alapjául szolgáló. Ezek a tetszés szerint elmélyedhet SDK archívumba találhatók (/ src/vannak /).

> [AZURE.WARNING] Elvégezheti a szükséges forrásainak lépnek pontos azonos használjuk. Ha szeretne közvetlenül módosítható, ne felejtse el módosítani a névtér és nevét.

### <a name="notification-position"></a>Értesítés pozíció

Alapértelmezés szerint a képernyő alján bal oldalán látható az alkalmazás-alkalmazás az értesítés jelenik meg. Ez a jelenség felülbírálása módosíthatja a `GetNotificationPosition` módszer a `EngagementReachHandler` objektum.

    // In your subclass of EngagementReachHandler
    
    public override EngagementReachHandler.NotificationPosition GetNotificationPosition(EngagementNotificationViewModel viewModel)
    {
       // return a value of the EngagementReachHandler.NotificationPosition enum (TOP or BOTTOM)
    }

Jelenleg, választhat a `BOTTOM` (alapértelmezett) és `TOP` beosztások.

### <a name="launch-message"></a>Indítsa el az üzenetben

Kattintáskor a rendszer értesítést (egy bejelentési), tetszés szerint elmélyedhet elindítja az alkalmazást, és töltse be a leküldéses üzenetek tartalma a megfelelő marketingkampány-lap megjelenítéséhez.

A menü az alkalmazás és a lapot (attól függően, hogy a hálózat sebességétől) megjelenítésének között van késleltetést.

Hogy a felhasználó számára, valamit, amit betöltése, egy vizuális információkat, például egy állapotsáv vagy állapotjelző kell megadnia. Tetszés szerint elmélyedhet nem tudják kezelni, hogy magát, de itt néhány kezelők meg.

A visszahívási alkalmazza, hajtsa végre:

    /* The application has launched and the content is loading.
     * You should display an indicator here.
     */
    EngagementReach.Instance.RetrieveLaunchMessageStarted += () => { [...] };
    
    /* The application has finished loading the content and the page
     * is about to be displayed.
     * You should hide the indicator here.
     */
    EngagementReach.Instance.RetrieveLaunchMessageCompleted += () => { [...] };
    
    /* The content has been loaded, but an error has occurred.
     * You can provide an information to the user.
     * You should hide the indicator here.
     */
    EngagementReach.Instance.RetrieveLaunchMessageFailed += () => { [...] };

Beállíthatja, hogy a visszahívás a `Application_Launching` módszer a `App.xaml.cs` lehetőleg, mielőtt a fájl a `EngagementReach.Instance.Init()` hívja.

> [AZURE.TIP] Minden egyes kezelő a felhasználói felület szál hívja meg. Nem kell aggódnia MessageBox vagy valami felhasználói felület kapcsolatos használata esetén.

[Alkalmazás-házirendek]:http://msdn.microsoft.com/library/windows/apps/hh184841(v=vs.105).aspx
[Content Policies]:http://msdn.microsoft.com/library/windows/apps/hh184842(v=vs.105).aspx
[További követelmények az adott alkalmazás típusai]:http://msdn.microsoft.com/library/windows/apps/hh184838(v=vs.105).aspx
 
