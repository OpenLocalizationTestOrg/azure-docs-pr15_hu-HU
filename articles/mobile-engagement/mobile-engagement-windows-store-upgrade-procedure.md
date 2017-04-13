<properties 
    pageTitle="A Windows univerzális alkalmazások SDK frissítési eljárások" 
    description="A Windows univerzális alkalmazások SDK frissítési eljárások az Azure mobil tetszés szerint elmélyedhet"                     
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

#<a name="windows-universal-apps-sdk-upgrade-procedures"></a>A Windows univerzális alkalmazások SDK frissítési eljárások

Ha Ön már rendelkezik integrált tetszés szerint elmélyedhet a régebbi verzióját az alkalmazásba, akkor a SDK frissítéskor, vegye figyelembe az alábbiakat.

Előfordulhat, hogy végre kell hajtani több eljárás, ha Ön kihagyott a SDK különböző verzióiban. Ha például telepít át 0.10.1 0.11.0 először menete a "feladó 0.9.0 való 0.10.1" kell majd a "from 0.10.1 való 0.11.0" eljárást.

##<a name="from-330-to-340"></a>A 3.3.0 való 3.4.0

### <a name="test-logs"></a>Naplók tesztelése

A SDK készített konzol naplók lehet most engedélyezett/letiltott/szűrt. Testreszabásához ez a tulajdonság frissítéséhez `EngagementAgent.Instance.TestLogEnabled` az egyik elérhető értékét a `EngagementTestLogLevel` számbavétel, például:

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();

### <a name="resources"></a>Erőforrások

Továbbfejlesztettük a gyermekektől átfedő. A SDK NuGet csomag forrásai része.

Amíg az új verzió SDK áttérni megadhatja, hogy szeretné-e meg, hogy a meglévő fájlok erőforrásait a átfedő mappát vagy sem.

* Ha az előző átfedés dolgoznak, vagy integrálni kívánja-e a `WebView` elemek manuális majd úgy is dönt, hogy a kilépő fájlok, azt is működnek. 
* Ha szeretné frissíteni az új átfedés, csak cserélje ki az egész `overlay` az erőforrások és a SDK csomagból az új egy mappájából (UWP alkalmazások: a frissítés után, amely letölthető az új átfedő mappa használata %\\.nuget\packages\MicrosoftAzure.MobileEngagement\3.4.0\content\win81\Resources).

> [AZURE.WARNING] Az új átfedés használatával felülírja a tartalomtípusokon végzett az előző verzióhoz képest.

##<a name="from-320-to-330"></a>A 3.2.0 való 3.3.0

### <a name="resources"></a>Erőforrások
Ebben a lépésben csak a testre szabott erőforrások vonatkozik. Ha testre szabott az SDK (html, képek, átfedő) által biztosított források majd akkor biztonsági őket a frissítés előtt, és alkalmazza újra a frissített erőforrások testreszabási.

##<a name="from-310-to-320"></a>A 3.2.0 való 3.1.0

### <a name="resources"></a>Erőforrások
Ebben a lépésben csak a testre szabott erőforrások vonatkozik. Ha testre szabott az SDK (html, képek, átfedő) által biztosított források majd akkor biztonsági őket a frissítés előtt, és alkalmazza újra a frissített erőforrások testreszabási.

### <a name="webview-integration"></a>Webnézet-integráció
Ez a verzió bevezetett néhány javítása másik eszközt űrlap tényezők megfelelően. Győződjön meg arról, hogy a integrálása a webnézet felel meg a következő:

Az a XAML lap ():

            <WebView x:Name="engagement_notification_content" Visibility="Collapsed" Height="80" HorizontalAlignment="Right" VerticalAlignment="Top"/>
            <WebView x:Name="engagement_announcement_content" Visibility="Collapsed" HorizontalAlignment="Right" VerticalAlignment="Top"/> 

És a kapcsolódó .cs fájl:

    using Microsoft.Azure.Engagement;
    using System;
    using Windows.ApplicationModel.Core;
    using Windows.UI.ViewManagement;
    using Windows.UI.Xaml;
    using Windows.UI.Xaml.Navigation;

    namespace My.Namespace.Example
    {
            /// <summary>
            /// An empty page that can be used on its own or navigated to within a Frame.
            /// </summary>
            public sealed partial class ExampleEngagementReachPage : EngagementPage
            {
              public ExampleEngagementReachPage()
              {
                this.InitializeComponent();
            
                /* Set your webview elements to the correct size. */
                SetWebView(width, height);
              }
            
              #region to implement
              /* Attach events when page is navigated. */
              protected override void OnNavigatedTo(NavigationEventArgs e)
              {
                /* Update the webview when the app window is resized. */
                Window.Current.SizeChanged += DisplayProperties_OrientationChanged;

                /* Update the webview when the app/status bar is resized. */
    #if WINDOWS_PHONE_APP || WINDOWS_UWP
                ApplicationView.GetForCurrentView().VisibleBoundsChanged += DisplayProperties_VisibleBoundsChanged; 
    #endif
                base.OnNavigatedTo(e);
              }

              /* When page is left ensure to detach SizeChanged handler. */
              protected override void OnNavigatedFrom(NavigationEventArgs e)
              {
                Window.Current.SizeChanged -= DisplayProperties_OrientationChanged;
    #if WINDOWS_PHONE_APP || WINDOWS_UWP
                ApplicationView.GetForCurrentView().VisibleBoundsChanged -= DisplayProperties_VisibleBoundsChanged;
    #endif
                base.OnNavigatedFrom(e);
              }
              
              /* "width" and "height" are the current size of your application display. */
    #if WINDOWS_PHONE_APP || WINDOWS_UWP
              double width = ApplicationView.GetForCurrentView().VisibleBounds.Width;
              double height = ApplicationView.GetForCurrentView().VisibleBounds.Height;
    #else
              double width =  Window.Current.Bounds.Width;
              double height =  Window.Current.Bounds.Height;
    #endif
            
              /// <summary>
              /// Set your webview elements to the correct size.
              /// </summary>
              /// <param name="width">The width of your current display.</param>
              /// <param name="height">The height of your current display.</param>
              private void SetWebView(double width, double height)
              {
                #pragma warning disable 4014
                CoreApplication.MainView.CoreWindow.Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal,
                        () =>
                        {
                          this.engagement_notification_content.Width = width;
                          this.engagement_announcement_content.Width = width;
                          this.engagement_announcement_content.Height = height;
                        });
              }
            
              /// <summary>
              /// Handler that takes the Windows.Current.SizeChanged and indicates that webviews have to be resized.
              /// </summary>
              /// <param name="sender">Original event trigger.</param>
              /// <param name="e">Window Size Changed Event arguments.</param>
              private void DisplayProperties_OrientationChanged(object sender, Windows.UI.Core.WindowSizeChangedEventArgs e)
              {
                double width = e.Size.Width;
                double height = e.Size.Height;
            
                /* Set your webview elements to the correct size. */
                SetWebView(width, height);
              }

    #if WINDOWS_PHONE_APP || WINDOWS_UWP              
              /// <summary>
              /// Handler that takes the ApplicationView.VisibleBoundsChanged and indicates that webviews have to be resized
              /// </summary>
              /// <param name="sender">The related application view.</param>
              /// <param name="e">Related event arguments.</param>
              private void DisplayProperties_VisibleBoundsChanged(ApplicationView sender, Object e)
              {
                double width = sender.VisibleBounds.Width;
                double height = sender.VisibleBounds.Height;
            
                /* Set your webview elements to the correct size. */
                SetWebView(width, height);
              }
    #endif
              #endregion
            }
    }

##<a name="from-200-to-300"></a>A 2.0.0 való 3.0.0

### <a name="resources"></a>Erőforrások
Ebben a lépésben csak a testre szabott erőforrások vonatkozik. Ha testre szabott az SDK (html, képek, átfedő) által biztosított források majd akkor biztonsági őket a frissítés előtt, és alkalmazza újra a frissített erőforrások testreszabási.

##<a name="from-111-to-200"></a>A 2.0.0 való 1.1.1

Az alábbi leírja, hogy miként telepítheti át SDK integrációs-alkalmazásba, Azure Mobile tetszés szerint elmélyedhet hajtott Capptain Társítások által kínált Capptain szolgáltatás. 

> [Azure.IMPORTANT] Capptain és Mobile tetszés szerint elmélyedhet nem ugyanazok a szolgáltatások és az alábbi eljárás csak kiemeli az ügyfél alkalmazás áttelepítése. Az alkalmazásban a SDK áttelepítése nem áttelepíti az adatok a Capptain kiszolgálókról Mobile tetszés szerint elmélyedhet kiszolgálókhoz

Ha egy korábbi verzióról, olvassa el a Capptain webhely 1.1.1 először áttelepítése, majd az alábbi eljárást alkalmazni

### <a name="nuget-package"></a>Nuget csomag

**Capptain.WindowsPhone** felülírásához **MicrosoftAzure.MobileEngagement** Nuget csomagot.

### <a name="applying-mobile-engagement"></a>Mobil tetszés szerint elmélyedhet alkalmazása

A SDK csomagjában talál a kifejezést használja `Engagement`. Módosítania kell a projekthez, ezt a módosítást megfelelően.

Akkor el kell távolítania az aktuális Capptain nuget csomagolásán látható termékszámmal. Fontolja meg, hogy Capptain erőforrások mappában található összes módosítás törlődik. Ha meg szeretné őrizni a azokat a fájlokat végezze el egy példányát.

Ezt követően telepítse a Microsoft Azure Engagement nuget új csomag a projekt. Közvetlenül a [nuget webhelyen] megtalálhatja. vagy itt index. Ez a művelet felülírja az összes erőforrások fájlokat tetszés szerint elmélyedhet által használt, és felveszi az új tetszés szerint elmélyedhet DLL a projekt hivatkozásait.

Ha a projekt hivatkozásait tisztítására Capptain DLL hivatkozás törlésével. Ha nem ez, Capptain verziójának ütköznek egymással, és hiba lép fel.

Ha testre szabott Capptain erőforrások, a régi fájlok tartalom másolása, és illessze be őket az új tetszés szerint elmélyedhet fájlok. Felhívjuk a figyelmét arra, hogy xaml-és cs frissíteni kell rendelkeznie.

Ezek a lépések végrehajtásakor csak akkor cserélje le a régi Capptain hivatkozás által az új tetszés szerint elmélyedhet hivatkozásokat.

1. Az összes Capptain névtér kell frissülnek.

    Áttelepítés: előtt
    
        using Capptain.Agent;
        using Capptain.Reach;
    
    Miután az áttelepítés:
    
        using Microsoft.Azure.Engagement;

2. Az összes Capptain osztályok, amelyek tartalmazzák a "Capptain" a "Tevékenységek" kell tartalmaznia.

    Áttelepítés: előtt
    
        public sealed partial class MainPage : CapptainPage
        {
          protected override string GetCapptainPageName()
          {
            return "Capptain Demo";
          }
          ...
        }
    
    Miután az áttelepítés:
    
        public sealed partial class MainPage : EngagementPage
        {
          protected override string GetEngagementPageName()
          {
            return "Engagement Demo";
          }
          ...
        }

3. Az xaml-fájlokat Capptain névtér és attribútumok is módosíthatja.

    Áttelepítés: előtt
    
        <capptain:CapptainPage
        ...
        xmlns:capptain="clr-namespace:Capptain.Agent;assembly=Capptain.Agent.WP"
        ...
        </capptain:CapptainPage>
    
    Miután az áttelepítés:
    
        <engagement:EngagementPage
        ...
        xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP"
        ...
        </engagement:EngagementPage>

4. Átfedő, oldal változik.
    > [AZURE.IMPORTANT] Egymást átfedő megjelenítés módosításokat. Az új névtér `Microsoft.Azure.Engagement.Overlay`. Az xaml-és cs használandó rendelkezik. Továbbá `CapptainGrid` , ha nevezhetők `EngagementGrid`, `capptain_notification_content` és `capptain_announcement_content` olyan nevesített `engagement_notification_content` és `engagement_announcement_content`.
    
    Az átfedő:
    
        <capptain:CapptainPageOverlay
          xmlns:capptain="using:Capptain.Overlay"
          ...
        </capptain:CapptainPageOverlay>
    
    Ez lesz:
    
        <EngagementPageOverlay
          engagement="using:Microsoft.Azure.Engagement.Overlay"
          ...
        </engagement:EngagementPageOverlay>

5. A más erőforrások például Capptain képek HTML-fájlokat ne feledje, hogy azok is átnevezett "Tevékenységek" használni.

### <a name="project-declaration"></a>Projekt deklaráció

A Package.appxmanifest `File Type Associations` frissült a:

 -   capptain\_elérjen\_tetszés szerint elmélyedhet tartalom\_elérjen\_tartalom
 -   capptain\_log\_tetszés szerint elmélyedhet fájl\_log\_fájl

### <a name="application-id--sdk-key"></a>Azonosítója / SDK billentyűt

Tetszés szerint elmélyedhet a kapcsolati karakterlánc használja. Nem kell az azonosítója, és egy SDK kulcs Mobile tetszés szerint elmélyedhet adja meg, csak akkor adjon meg egy kapcsolati karakterláncot. Beállíthatja azt a EngagementConfiguration a fájlon.

A tevékenységek konfiguráció állíthatók be a `Resources\EngagementConfiguration.xml` a projekt fájlt.

Adja meg a fájlt szerkeszteni:

-   Az alkalmazás kapcsolati karakterlánc között címkék `<connectionString>` és `<\connectionString>`.

Szeretné inkább megadása futásidőben, ha felhívhatja a tetszés szerint elmélyedhet ügynök inicializálni előtt az alábbi módon:

    /* Engagement configuration. */
    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";
    
    /* Initialize Engagement agent with above configuration. */
    EngagementAgent.Instance.Init(args, engagementConfiguration);

A kapcsolati karakterláncot, az alkalmazás megjelenik az Azure klasszikus portálon.

### <a name="items-name-change"></a>Elemek nevének módosítása

Minden elem *capptain* nevű megnevezett *részvételét*. Hasonlóképpen a *Capptain* *részvételét*.

Példák gyakran használt Capptain elemek:

-   Most már nevű EngagementConfiguration CapptainConfiguration
-   Most már nevű EngagementAgent CapptainAgent
-   Most már nevű EngagementReach CapptainReach
-   Most már nevű EngagementHttpConfig CapptainHttpConfig
-   Most már nevű GetEngagementPageName GetCapptainPageName

Megjegyzendő, hogy átnevezése is hatással van a módszerek túllépni.

 
