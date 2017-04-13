<properties 
    pageTitle="Univerzális alkalmazások Windows SDK integrációs elérése" 
    description="Univerzális alkalmazásokat a Windows Azure mobil tetszés szerint elmélyedhet vannak integrálása"
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-windows-store" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

# <a name="windows-universal-apps-reach-sdk-integration"></a>Univerzális alkalmazások Windows SDK integrációs elérése

Meg kell követnie az integrációs előtt ez az útmutató a [Windows univerzális tetszés szerint elmélyedhet SDK integrációs](mobile-engagement-windows-store-integrate-engagement.md) ismertetett.

## <a name="embed-the-engagement-reach-sdk-into-your-windows-universal-project"></a>A tevékenységek elérjen SDK beágyazása a Windows univerzális projekt

Nem kell semmit hozzáadása. `EngagementReach`hivatkozások és források már rendelkezik a projektben.

> [AZURE.TIP] Testre szabhatja a található képek a `Resources` mappát a projekt, különösen a arculatának ikonra (hogy tetszés szerint elmélyedhet ikonra az alapértelmezett). Univerzális alkalmazások mozgathatja a `Resources` mappába a megosztott projekt között alkalmazások, de a tartalmat szeretne megosztani kell tartani a `Resources\EngagementConfiguration.xml` az alapértelmezett helyre a fájl, mert függő platform.

## <a name="enable-the-windows-notification-service"></a>A Windows értesítési szolgáltatás engedélyezése

### <a name="windows-8x-and-windows-phone-81-only"></a>A Windows 8.x és a Windows Phone 8.1 csak

A **Windows értesítési szolgáltatás** (WNS néven) használatához a a `Package.appxmanifest` fájl a következő `Application UI` kattintson a `All Image Assets` a bal oldali bot mezőbe. A párbeszédpanel jobb oldali `Notifications`, módosítása `toast capable` a `(not set)` való `(Yes)`.

### <a name="all-platforms"></a>Minden platform

Az alkalmazás, a Microsoft-fiókjával, és a tevékenységek platformra szinkronizálása szüksége. Ahhoz, hogy ez kell hozzon létre egy fiókot vagy jelentkezzen be [windows fejlesztői központ](https://dev.windows.com). Ezután hozzon létre egy új alkalmazást, és keresse meg a biztonsági azonosító és a titkos kulcs. A tevékenységek frontend válassza az alkalmazás beállítása a `native push` , és illessze be a hitelesítő adatait. Ezt követően kattintson a jobb gombbal a projekt választó `store` és `Associate App with the Store...`. Csak kell jelölje be az alkalmazás való szinkronizálás előtt létrehozása van.

## <a name="initialize-the-engagement-reach-sdk"></a>A tevékenységek vannak SDK inicializálni

Módosítsa a `App.xaml.cs`:

-   Beszúrás `EngagementReach.Instance.Init` csak utána `EngagementAgent.Instance.Init` a a `InitEngagement` módszer:

        private void InitEngagement(IActivatedEventArgs e)
        {
          EngagementAgent.Instance.Init(e);
          EngagementReach.Instance.Init(e);
        }

    A `EngagementReach.Instance.Init` egy dedikált szálon futó. Nincs teendő saját magának.

> [AZURE.NOTE] Ha Ön használja a leküldéses értesítések máshol az alkalmazás majd kell [megosztani a leküldéses csatorna](#push-channel-sharing) tetszés szerint elmélyedhet elérje a.

## <a name="integration"></a>Integráció

Tetszés szerint elmélyedhet nyújt két módon is hozzáadhat a jutnak el az alkalmazás transzparensek és hirdetmények és szavazásokra közbeszúrt nézetek az alkalmazás: az átfedő integráció és a webes nézetek kézi integráció. Ki kell alakítania a két megközelítés ugyanazon az oldalon nem.

A két integráció a választást sikerült összegezni ily módon:

-   Előfordulhat, hogy választhat az átfedő integráció, ha a lapok már örökli az ügynök `EngagementPage`, csak cseréje kérdése `EngagementPage` által `EngagementPageOverlay` és `xmlns:engagement="using:Microsoft.Azure.Engagement"` által `xmlns:engagement="using:Microsoft.Azure.Engagement.Overlay"` a lapokon.
-   Választhat a webes nézetek kézi integrációs Ha pontosan a elérje a felhasználói felület a lapok belül helyezze el, vagy ha azt szeretné, ha egy másik öröklődés szintet a lapokon. 

### <a name="overlay-integration"></a>Átfedő-integráció

A tevékenységek átfedő dinamikusan hozzáadása vannak kampányok jelennek meg a lapon a felhasználói felület elemeket. Ha az átfedő nem megfeleljen a elrendezés vegye figyelembe a webes nézetek kézi integrációs helyette.

A .xaml fájl módosítása `EngagementPage` hivatkozás`EngagementPageOverlay`

-   A névtér deklarációs hozzáadása:

        xmlns:engagement="using:Microsoft.Azure.Engagement.Overlay"

-   Csere `engagement:EngagementPage` a `engagement:EngagementPageOverlay`:

**A EngagementPage:**

        <engagement:EngagementPage 
            xmlns:engagement="using:Microsoft.Azure.Engagement">
        
            <!-- Your layout -->
        </engagement:EngagementPage>

**A EngagementPageOverlay:**

        <engagement:EngagementPageOverlay 
            xmlns:engagement="using:Microsoft.Azure.Engagement.Overlay">
        
            <!-- Your layout -->
        </engagement:EngagementPageOverlay>

Kattintson a .cs fájl címkézéséhez az weblapon `EngagementPageOverlay` helyett `EngagementPage` , és importálja a `Microsoft.Azure.Engagement.Overlay`.

            using Microsoft.Azure.Engagement.Overlay;

-   Csere `EngagementPage` a `EngagementPageOverlay`:

**A EngagementPage:**

            using Microsoft.Azure.Engagement;
            
            namespace Example
            {
              public sealed partial class ExamplePage : EngagementPage
              {
                [...]
              }
            }

**A EngagementPageOverlay:**

            using Microsoft.Azure.Engagement.Overlay;
            
            namespace Example
            {
              public sealed partial class ExamplePage : EngagementPageOverlay 
              {
                [...]
              }
            }


A tetszés szerint elmélyedhet átfedő hozzáadása egy `Grid` elemet a lap felső részén tevődik össze az elrendezés és a két `WebView` elemeket egy a fejléc, a másik közbeszúrt megjelenítéséhez.

Testre szabhatja az átfedő közvetlenül az elemeket a `EngagementPageOverlay.cs` fájlt.

### <a name="web-views-manual-integration"></a>Webes nézetek kézi-integráció

A lapok a két vannak fog keresni `WebView` elemek felelős a fejléc és a közbeszúrt nézetben jeleníti meg. Beállítást kell elvégeznie Ez esetben adja hozzá ezeket a két `WebView` elemek valahol a lapján, Íme egy példa:

    <Grid x:Name="engagementGrid">

      <!-- Your layout -->

      <WebView x:Name="engagement_notification_content" Visibility="Collapsed" Height="80" HorizontalAlignment="Stretch" VerticalAlignment="Top"/>
      <WebView x:Name="engagement_announcement_content" Visibility="Collapsed"  HorizontalAlignment="Stretch"  VerticalAlignment="Stretch"/>
    </Grid>


Ebben a példában a `WebView` elemek úgy módosítja a program a tároló, amely automatikusan újra méretű őket képernyő Forgatás vagy az ablak méretének módosítása esetén igazítása.

> [AZURE.WARNING] Fontos, hogy az azonos névhasználati megtartása `engagement_notification_content` és `engagement_announcement_content` esetében a `WebView` elemeket. Vannak azonosító őket a nevük számára. 

## <a name="handle-datapush-optional"></a>Fogópont datapush (nem kötelező)

Ha engedélyezni szeretné a vannak adatok veremmutatót kapni az alkalmazás, a EngagementReach osztály két esemény végrehajtásához van:

A App() konstruktorban App.xaml.cs hozzáadása:

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

## <a name="customize-ui-optional"></a>(Nem kötelező) felhasználói felület testreszabása

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

Megadhatja a tartalmát a `EngagementReach.Instance.Handler` egyéni objektumának mezőbe a `App.xaml.cs` belül osztály a `App()` módot.

**Példakódot:**

            protected override void OnLaunched(LaunchActivatedEventArgs args)
            {
              // your app initialization 
              EngagementReach.Instance.Handler = new ExampleReachHandler();
              // Engagement Agent and Reach initialization
            }

> [AZURE.NOTE]Alapértelmezés szerint tetszés szerint elmélyedhet használja a saját végrehajtásának `EngagementReachHandler`.
> Hozzon létre saját nincs, és ha így tesz, nem kell minden módszer felülbírálása. Az alapértelmezett működés található a tetszés szerint elmélyedhet alap objektum kijelöléséhez.

### <a name="web-view"></a>Webes nézet

Alapértelmezés szerint vannak fogja használni a beágyazott erőforrásait a DLL-szolgáltatásfájlját az értesítéseket és a lapok megjelenítése.

Az adja meg a teljes testreszabás használjuk csak webes nézet lehetőséget. Elrendezések testreszabása szeretné, ha felülbírálása közvetlenül az erőforrások fájlok `EngagementAnnouncement.html` és `EngagementNotification.html`. Tetszés szerint elmélyedhet szüksége van az összes kód `<body></body>` megfelelő futtatásához. De hozzáadható a külső címke `engagement_webview_area`.

Jó helyen jár eldöntheti, használja a saját erőforrásait.

Felülbírálhatja `EngagementReachHandler` módszerek a alosztály állapítható meg, hogy az elrendezéseket használhatja, de ügyeljen arra, hogy tetszés szerint elmélyedhet a beágyazott a tetszés szerint elmélyedhet mechanizmusa:

**Példakódot:**
            
            // In your subclass of EngagementReachHandler
            
            public override string GetAnnouncementHTML()
            {
              return base.GetAnnouncementHTML();
            }
            public override string GetAnnouncementName()
            {
              return base.GetAnnouncementName();
            }
            public override string GetNotfificationHTML()
            {
              return base.GetNotfificationHTML();
            }
            public override string GetNotfificationName()
            {
              return base.GetNotfificationName();
            }


Alapértelmezés szerint ki van AnnouncementHTML `ms-appx-web:///Resources/EngagementAnnouncement.html`. A leküldéses üzenet (szöveg bejelentése, webes anoucement és a szavazás bejelentése) tartalma tervezhet html-fájl képviseli. AnnouncementName van `engagement_announcement_content`. Célszerű az xaml lapon a webnézet tervezés nevét.

NotfificationHTML van `ms-appx-web:///Resources/EngagementNotification.html`. A HTML-fájl, amely az üzenet leküldéses értesítést tervezése képviseli. NotfificationName van `engagement_notification_content`. Célszerű az xaml lapon a webnézet tervezés nevét.

### <a name="customization"></a>Testreszabás

Testre szabhatja, értesítés és webes nézetben van, akkor tetszés szerint elmélyedhet objektum megőrzése, ha bejelentése. Készítése a figyelmet, hogy webnézet objektum leírása háromszor – az első alkalommal a xaml-ben, a "setwebview()" módszerrel .cs fájljait a második alkalommal és harmadszor a HTML-fájlban.

-   A xaml a írja le az aktuális grafikus elrendezés webnézet összetevőt.
-   A .cs fájl határozhatja meg, amelyben a két webnézet (értesítést, bejelentése) dimenzió beállítása "setwebview()". Érdemes nagyon hatékony az alkalmazás átméretezésekor.
-   A tevékenységek HTML-fájl azt írja le a Webnézeti tartalom, a tervezés és az elemek pozíció egymás között.

### <a name="launch-message"></a>Indítsa el az üzenetben

Kattintáskor a rendszer értesítést (egy bejelentési), tetszés szerint elmélyedhet elindítja az alkalmazást, és töltse be a leküldéses üzenetek tartalma a megfelelő marketingkampány-lap megjelenítéséhez.

A menü az alkalmazás és a lapot (attól függően, hogy a hálózat sebességétől) megjelenítésének között van késleltetést.

Hogy a felhasználó számára, valamit, amit betöltése, egy vizuális információkat, például egy állapotsáv vagy állapotjelző kell megadnia. Tetszés szerint elmélyedhet nem tudják kezelni, hogy magát, de itt néhány kezelők meg.

A visszahívási végrehajtásához a "Nyilvános App() {}" App.xaml.cs adja:

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

Beállíthatja, hogy a visszahívás, a "Nyilvános App() {}" módszer a `App.xaml.cs` lehetőleg, mielőtt a fájl a `EngagementReach.Instance.Init()` hívja.

> [AZURE.TIP] Minden egyes kezelő a felhasználói felület szál hívja meg. Nem kell aggódnia MessageBox vagy valami felhasználói felület kapcsolatos használata esetén.

##<a id="push-channel-sharing"></a>Leküldéses csatorna megosztása

Ha Ön használja a leküldéses értesítések más célra az alkalmazás majd meg kell használnia a leküldéses csatorna megosztása tetszés szerint elmélyedhet SDK szolgáltatás. Ez a nem fogadott leküldéses elkerülése érdekében.

- A saját leküldéses csatorna a tetszés szerint elmélyedhet elérjen inicializálni lehet nyújtani. A SDK csomagjában talál egy új kérő helyett használja.

Frissítse a tetszés szerint elmélyedhet elérjen inicializálni a leküldéses csatorna a `InitEngagement` származó mód a `App.xaml.cs` fájl:
    
    /* Your own push channel logic... */
    var pushChannel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();
    
    /*...Engagement initialization */
    EngagementAgent.Instance.Init(e);
    EngagementReach.Instance.Init(e,pushChannel);

- Másik lehetőség Ha csak szeretne felhasználni a leküldéses csatornát, után a gyermekektől inicializálni, majd a visszahívás állíthat be tetszés szerint elmélyedhet a leküldéses csatorna egyszer megszerezni elérjen hozta a SDK csomagjában talál.

Állítsa a visszahívás minden olyan helyen **után** a gyermekektől inicializálni:

    /* Set action on the SDK push channel. */
    EngagementReach.Instance.SetActionOnPushChannel((PushNotificationChannel channel) => 
    {
      /* The forwarded channel can be null if its creation fails for any reason. */
      if (channel != null)
      {
        /* Your own push channel logic... */
      });
    }

## <a name="custom-scheme-tip"></a>Egyéni színséma tipp:

Elvégezheti a szükséges egyéni színséma használata. Tetszés szerint elmélyedhet frontend az tetszés szerint elmélyedhet alkalmazásban használható a különböző típusú URI is küldhet. Alapértelmezett séma például `http, ftp, ...` is kezelheti ablak lesz a Windows kérdés Ha még nincs telepítve az eszközön alapértelmezett alkalmazás. Az alkalmazás is létrehozhat saját egyéni színsémát.

Az egyszerű szeretne megadni egy egyéni színsémát az alkalmazás módja kattintva nyissa meg a `Package.appxmanifest` válassza a `Declarations` panelen. Válassza a `Protocol` görgessen a rendelkezésre álló nyilatkozatokat mezőbe, és adja hozzá. Szerkesztés a `Name` az új Protocol (protokoll) mezőbe a kívánt nevet.

Most a protokoll használatára, módosítsa a `App.xaml.cs` együtt a `OnActivated` módszer, és ne felejtse el az alábbi tetszés szerint elmélyedhet is inicializálni:

            /// <summary>
            /// Enter point when app his called by another way than user click
            /// </summary>
            /// <param name="args">Activation args</param>
            protected override void OnActivated(IActivatedEventArgs args)
            {
              /* Init engagement like it was launch by a custom uri scheme */
              EngagementAgent.Instance.Init(args);
              EngagementReach.Instance.Init(args);
            
              //TODO design action to do when app is launch
            
              #region Custom scheme use
              if (args.Kind == ActivationKind.Protocol)
              {
                ProtocolActivatedEventArgs myProtocol = (ProtocolActivatedEventArgs)args;
            
                if (myProtocol.Uri.Scheme.Equals("protocolName"))
                {
                  string path = myProtocol.Uri.AbsolutePath;
                }
              }
              #endregion
 
