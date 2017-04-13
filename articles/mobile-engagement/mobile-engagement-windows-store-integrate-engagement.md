<properties 
    pageTitle="A Windows univerzális alkalmazások tetszés szerint elmélyedhet SDK integrációja" 
    description="Univerzális alkalmazásokat a Windows Azure mobil tetszés szerint elmélyedhet integrálása"                  
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

# <a name="windows-universal-apps-engagement-sdk-integration"></a>A Windows univerzális alkalmazások tetszés szerint elmélyedhet SDK integrációja

> [AZURE.SELECTOR] 
- [Univerzális Windows](mobile-engagement-windows-store-integrate-engagement.md) 
- [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md) 
- [iOS](mobile-engagement-ios-integrate-engagement.md) 
- [Android-alapú](mobile-engagement-android-integrate-engagement.md) 

Ez az eljárás a legegyszerűbb módszer tetszés szerint elmélyedhet a analitikai és a Windows univerzális alkalmazásban függvények figyelése aktiválása ismerteti.

A következő lépések nem elég a jelentést a naplók szükség számítja ki a felhasználókat, munkamenetek, tevékenységek, összeomlik és Technicals kapcsolatos összes statisztika aktiválása. A jelentés naplók szükség más statisztikai adatokat, például eseményeket, a hibák és a feladatok számítja ki kell végezni, manuálisan segítségével tetszés szerint elmélyedhet API-val (lásd: [a speciális Mobile tetszés szerint elmélyedhet a Windows univerzális alkalmazásban API címkézés használatáról](mobile-engagement-windows-store-use-engagement-api.md) , mivel ezeket a statisztikákat függő alkalmazás.

## <a name="supported-versions"></a>Verziók

A mobil tetszés szerint elmélyedhet SDK a Windows univerzális alkalmazások csak integrálhatók Windows Runtime és univerzális Windows platformra alkalmazások kiválasztásával:

-   Windows 8-ban
-   Windows 8.1
-   Windows Phone 8.1
-   A Windows 10 (asztali és mobil családok)

> [AZURE.NOTE] Ha a Windows Phone Silverlight vannak célba juttatása, majd olvassa el a [Windows Phone Silverlight integráció az eljárást](mobile-engagement-windows-phone-integrate-engagement.md).

## <a name="install-the-mobile-engagement-universal-apps-sdk"></a>A mobil tetszés szerint elmélyedhet univerzális alkalmazások SDK telepítése

### <a name="all-platforms"></a>Minden platform

A tevékenységek SDK a Windows univerzális mobilalkalmazás *MicrosoftAzure.MobileEngagement*nevű Nuget csomag érhető el. Telepítheti a Visual Studio Nuget csomag Manager alkalmazásból.

### <a name="windows-8x-and-windows-phone-81"></a>A Windows 8.x és a Windows Phone 8.1

NuGet automatikusan a SDK erőforrások üzembe helyezése a `Resources` a alkalmazás projekt gyökérmappája.

### <a name="windows-10-universal-windows-platform-applications"></a>A Windows 10 univerzális Windows platformra alkalmazások

NuGet automatikusan telepítse a SDK erőforrások még a UWP alkalmazásban. Kézi végrehajtás módja mindaddig, amíg az erőforrások telepítése újból be vezetni a NuGet van:

1.  Nyissa meg a Fájlkezelőt.
2.  Nyissa meg azt a következő helyen (a**x.x.x** tetszés szerint elmélyedhet telepíti az verzióját is): *használata %\\.nuget\packages\MicrosoftAzure.MobileEngagement\\**x.x.x**\\content\win81*
3.  Húzással a **források** mappát a Fájlkezelőben a projekt a Visual Studióban legfelső szintű.
4.  A Visual Studióban válassza ki azt a projektet, és aktiválja a **Megoldást Explorer**fölött a **teljes fájlok** ikonra.
5.  Egyes fájlok nem szerepelnek a projektben. Importálhatja őket egyszerre kattintson a jobb gombbal az **erőforrások** mappára, a **project kizárása** , majd egy másik kattintson a jobb gombbal az **erőforrások** mappára, **felvétele a Project alkalmazásban** az egész mappa újbóli felvenni. Az **erőforrások** mappában lévő összes fájl a project most már szerepelnek.

## <a name="add-the-capabilities"></a>A Funkciók hozzáadása

A tevékenységek SDK csomagjában talál van szüksége annak érdekében, hogy működik megfelelően a Windows SDK néhány lehetőségeit.

Nyissa meg a `Package.appxmanifest` fájl, és ügyeljen arra, hogy deklarálva vannak-e az alábbi funkciókat:

-   `Internet (Client)`

## <a name="initialize-the-engagement-sdk"></a>A tevékenységek SDK inicializálni

### <a name="engagement-configuration"></a>Tetszés szerint elmélyedhet konfigurálása

A tevékenységek konfiguráció van központi a a `Resources\EngagementConfiguration.xml` a projekt fájlt.

Adja meg a fájlt szerkeszteni:

-   Az alkalmazás kapcsolati karakterlánc között címkék `<connectionString>` és `<\connectionString>`.

Szeretné inkább megadása futásidőben, ha felhívhatja a tetszés szerint elmélyedhet ügynök inicializálni előtt az alábbi módon:
          
          /* Engagement configuration. */
          EngagementConfiguration engagementConfiguration = new EngagementConfiguration();

          /* Set the Engagement connection string. */
          engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

          /* Initialize Engagement angent with above configuration. */
          EngagementAgent.Instance.Init(e, engagementConfiguration);

A kapcsolati karakterláncot, az alkalmazás megjelenik az Azure klasszikus portálon.

### <a name="engagement-initialization"></a>Tetszés szerint elmélyedhet inicializálni

Amikor egy új projektet hoz létre egy `App.xaml.cs` fájl jön létre. Az osztály örökli `Application` és számos fontos módszer tartalmaz. Is lesz a tetszés szerint elmélyedhet SDK inicializálni.

Módosítsa a `App.xaml.cs`:

-   Hozzáadása a `using` kimutatások:

        using Microsoft.Azure.Engagement;

-   Adja meg a módszer a tetszés szerint elmélyedhet inicializálni egyszer összes hívásokhoz megosztása:

        private void InitEngagement(IActivatedEventArgs e)
        {
          EngagementAgent.Instance.Init(e);
        
          // or
        
          EngagementAgent.Instance.Init(e, engagementConfiguration);
        }
        
-   Hívás `InitEngagement` a a `OnLaunched` módszer:

        protected override void OnLaunched(LaunchActivatedEventArgs e)
        {
          InitEngagement(e);
        }

-   Amikor az alkalmazás elindításakor egy egyéni színsémát, egy másik alkalmazás vagy a parancssorból, majd a `OnActivated` módszer neve. Meg kell kezdeményezhet a a tetszés szerint elmélyedhet SDK csomagjában talál, amikor az alkalmazás aktív. Ehhez felülbírálása `OnActivated` módszer:

        protected override void OnActivated(IActivatedEventArgs args)
        {
          InitEngagement(args);
        }

> [AZURE.IMPORTANT] Kifejezetten nem ajánlott a tetszés szerint elmélyedhet inicializálni felvenni az alkalmazás egy másik helyre.

## <a name="basic-reporting"></a>Egyszerű jelentés

### <a name="recommended-method-overload-your-page-classes"></a>Ajánlott módszer: túlterhelés a `Page` osztályok

A jelentés tetszés szerint elmélyedhet számítja ki a felhasználókat, munkamenetek, tevékenységek, összeomlik és technikai statisztika által igényelt összes naplók aktiválásához egyszerűen teheti az összes a `Page` alszint osztályok örökölhet a `EngagementPage` osztályok.

Íme egy példa ennek módjáról az alkalmazás lap. Az alkalmazás minden oldalhoz ugyanazt műveleteket hajthat végre.

#### <a name="c-source-file"></a>C# forrásfájl

A lap módosítása `.xaml.cs` fájl:

-   Hozzáadása a `using` kimutatások:

        using Microsoft.Azure.Engagement;

-   Csere `Page` a `EngagementPage`:

**Tetszés szerint elmélyedhet: nélkül**
    
        namespace Example
        {
          public sealed partial class ExamplePage : Page
          {
            [...]
          }
        }

**A tevékenységek:**

        using Microsoft.Azure.Engagement;
        
        namespace Example
        {
          public sealed partial class ExamplePage : EngagementPage 
          {
            [...]
          }
        }

> [AZURE.IMPORTANT] Ha a lap felülírja a `OnNavigatedTo` módszer, ne felejtse el, hívja fel `base.OnNavigatedTo(e)`. Egyéb esetben a tevékenység nem lehet jelenteni (a `EngagementPage` hívások `StartActivity` belül a `OnNavigatedTo` módszer).

#### <a name="xaml-file"></a>XAML-fájl

A lap módosítása `.xaml` fájl:

-   A névtér deklarációs hozzáadása:

        xmlns:engagement="using:Microsoft.Azure.Engagement"

-   Csere `Page` a `engagement:EngagementPage`:

**Tetszés szerint elmélyedhet: nélkül**

        <Page>
            <!-- layout -->
            ...
        </Page>

**A tevékenységek:**

        <engagement:EngagementPage 
            xmlns:engagement="using:Microsoft.Azure.Engagement">
            <!-- layout -->
            ...
        </engagement:EngagementPage >

#### <a name="override-the-default-behaviour"></a>Az alapértelmezett viselkedésének felülbírálása

Alapértelmezés szerint az osztálynév, oldal készként a tevékenység neve, és nincs extra. Ha az osztály a "Lap" utótag használja, tetszés szerint elmélyedhet is törlődik.

Ha szeretne a nevét az alapértelmezett viselkedésének felülbírálása, egyszerűen hozzáadása Ez a kód:

        // in the .xaml.cs file
        protected override string GetEngagementPageName()
        {
          /* your code */
          return "new name";
        }

Ha szeretne jelentést néhány további informations a tevékenységhez, ez a kód is felveheti:

        // in the .xaml.cs file
        protected override Dictionary<object,object> GetEngagementPageExtra()
        {
          /* your code */
          return extra;
        }

Ezeket a módszereket úgynevezett magából a `OnNavigatedTo` módszer a lapon.

### <a name="alternate-method-call-startactivity-manually"></a>Alternatív módszer: hívás `StartActivity()` manuálisan

Ha nem szeretne a többszörös vagy nem a `Page` osztályok, hívja fel a tevékenységek inkább elindíthatja `EngagementAgent` módszerek közvetlenül.

Azt javasoljuk, ha fel szeretne hívni `StartActivity` belül a `OnNavigatedTo` a lap metódusát.

            protected override void OnNavigatedTo(NavigationEventArgs e)
            {
              base.OnNavigatedTo(e);
              EngagementAgent.Instance.StartActivity("MyPage");
            }

> [AZURE.IMPORTANT]  Gondoskodjon arról, hogy helyesen befejezheti a munkamenetet.
> 
> A Windows univerzális SDK automatikusan felhívja a `EndActivity` módszer, ha az alkalmazás be van zárva. Így célszerű **erősen** ajánlott, ha fel szeretne hívni a `StartActivity` módszer, amikor a tevékenység annak a felhasználónak módosítása, és **Soha** hívja a `EndActivity` módszer, ez a módszer küld tetszés szerint elmélyedhet kiszolgálóhoz, hogy az aktuális felhasználó rendelkezik hagyja az alkalmazást, ez hatással van az összes alkalmazás naplók.

## <a name="advanced-reporting"></a>Speciális jelentése

Tetszés szerint érdemes jelentés alkalmazás bizonyos eseményeket, a hibák és a feladatok, így használja a többi módszerek található a `EngagementAgent` osztály. A tevékenységek API lehetővé teszi, hogy az összes tetszés szerint elmélyedhet a speciális funkciókat használja.

További tudnivalókért lásd [használatáról a speciális Mobile tetszés szerint elmélyedhet címkézés API-t a Windows univerzális alkalmazásban](mobile-engagement-windows-store-use-engagement-api.md).

##<a name="advanced-configuration"></a>Speciális beállítások

### <a name="disable-automatic-crash-reporting"></a>Automatikus összeomlik jelentéskészítés letiltása

Az automatikus összeomlást jelentési funkció felvételi tiltható le. Ezután, esetén nem kezelt kivételt akkor fordul elő, amikor tetszés szerint elmélyedhet semmi nem fogja elvégezni.

> [AZURE.WARNING] Ha ez a szolgáltatás letiltásához, ügyeljen arra, hogy egy esetén nem kezelt összeomlik akkor fordul elő, az alkalmazásban, ha tetszés szerint elmélyedhet nem küld az összeomlást, **és** nem zárja be a munkamenetet, és a feladatokat.

Automatikus összeomlik jelentéskészítés letiltásához testreszabása a deklarálva, akkor módja attól függően, hogy Ön konfigurációjának:

#### <a name="from-engagementconfigurationxml-file"></a>A `EngagementConfiguration.xml` fájl

Jelentés összeomlik beállítása `false` közötti `<reportCrash>` és `</reportCrash>` címkék.

#### <a name="from-engagementconfiguration-object-at-run-time"></a>A `EngagementConfiguration` futásidőben objektum

Állítsa a EngagementConfiguration objektuma segítségével hamis jelentés összeomlik.

        /* Engagement configuration. */
        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";
        
        /* Disable Engagement crash reporting. */
        engagementConfiguration.Agent.ReportCrash = false;

### <a name="burst-mode"></a>Burst mód

Alapértelmezés szerint a tetszés szerint elmélyedhet szolgáltatás jelentések naplózza valós időben. Ha az alkalmazás jelentések naplók gyakran, akkor célszerűbb puffer a naplókat, és a jelentést őket egyszerre egy rendszeres időközönként alap (azaz "burst mód").

Ehhez hívás módszer:

        EngagementAgent.Instance.SetBurstThreshold(int everyMs);

Az argumentum értéke **ezredmásodpercben**. Bármikor szeretne a valós idejű naplózás újraaktiválása, ha csak hívja fel a módszerrel bármely paraméter nélkül, vagy a 0 értéket.

A burst mód kissé növelése az elem leírási_idő, de hatással van az tetszés szerint elmélyedhet monitoron: minden munkamenetek és a feladatok időtartamának (így a munkamenetek és a dolgozók rövidebb, mint a burst küszöbértékét nem láthatják a feladatok) burst küszöbérték lesz kerekítve. Egy mint 30000 (30s) burst küszöbérték használata ajánlott. Ügyeljen arra, hogy a naplók mentése korlátozódik 300 tételt van. Ha a Küldés túl hosszú elvesznek a néhány naplók.

> [AZURE.WARNING] Kisebb, mint 1s pontra a burst küszöbértékét nem állíthatók be. Próbál teheti, ha a SDK jelennek meg a nyomkövetési a hibával, és automatikusan visszaáll az alapértelmezett érték, azaz 0s. Ez elindítja a jelentés a naplókat valós idejű SDK csomagjában talál.

[here]:http://www.nuget.org/packages/Capptain.WindowsCS
[NuGet website]:http://docs.nuget.org/docs/start-here/overview
 
