<properties 
    pageTitle="Windows Phone Silverlight tetszés szerint elmélyedhet SDK integrációja" 
    description="Azure mobil tetszés szerint elmélyedhet integrálása a Windows Phone a Silverlight-alkalmazások"                  
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-windows-phone" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="windows-phone-silverlight-engagement-sdk-integration"></a>Windows Phone Silverlight tetszés szerint elmélyedhet SDK integrációja

> [AZURE.SELECTOR] 
- [A Windows univerzális](mobile-engagement-windows-store-integrate-engagement.md) 
- [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md) 
- [iOS](mobile-engagement-ios-integrate-engagement.md) 
- [Android-alapú](mobile-engagement-android-integrate-engagement.md) 

Ez az eljárás a legegyszerűbb módszer Azure Mobile részvételét analitikai és a függvények a Windows Phone-Silverlight-alkalmazás figyelése aktiválása ismerteti.

A következő lépések nem elég a jelentést a naplók szükség számítja ki a felhasználókat, munkamenetek, tevékenységek, összeomlik és Technicals kapcsolatos összes statisztika aktiválása. A jelentés naplók szükség más statisztikai adatokat, például eseményeket, a hibák és a feladatok számítja ki kell végezni, manuálisan segítségével tetszés szerint elmélyedhet API-val (lásd: [a speciális Mobile tetszés szerint elmélyedhet a Windows Phone-Silverlight-alkalmazás API-címkézés használatáról](mobile-engagement-windows-phone-use-engagement-api.md) az alábbi), mivel ezeket a statisztikákat alkalmazás függő.

##<a name="supported-versions"></a>Verziók

A mobil tetszés szerint elmélyedhet SDK a Windows Silverlight csak integrálhatók célba juttatása alkalmazások:

-   Windows Phone 8.0-ban
-   Windows Phone 8.1 Silverlight

> [AZURE.NOTE] Ha Windows Phone 8.1 (nem Silverlight) céloz majd olvassa el a [Windows univerzális integráció az eljárást](mobile-engagement-windows-store-integrate-engagement.md).

##<a name="install-the-mobile-engagement-silverlight-sdk"></a>A mobil tetszés szerint elmélyedhet a Silverlight SDK telepítése

A mobil tetszés szerint elmélyedhet SDK a Windows Silverlight *MicrosoftAzure.MobileEngagement*nevű Nuget csomag érhető el. Telepítheti a Visual Studio Nuget csomag Manager alkalmazásból. 

##<a name="add-the-capabilities"></a>A Funkciók hozzáadása

A tevékenységek SDK csomagjában talál van szüksége annak érdekében, hogy működik megfelelően a Windows Phone Silverlight SDK csomagjában talál néhány lehetőségeit.

Nyissa meg a `WMAppManifest.xml` fájl, és ügyeljen arra, hogy az alábbi funkciókat deklarált vannak a `Capabilities` panel:

-   `ID_CAP_NETWORKING`
-   `ID_CAP_IDENTITY_DEVICE`

##<a name="initialize-the-engagement-sdk"></a>A tevékenységek SDK inicializálni

### <a name="engagement-configuration"></a>Tetszés szerint elmélyedhet konfigurálása

A tevékenységek konfiguráció van központi a a `Resources\EngagementConfiguration.xml` a projekt fájlt.

Adja meg a fájlt szerkeszteni:

-   Az alkalmazás kapcsolati karakterlánc között címkék `<connectionString>` és `<\connectionString>`.

Szeretné inkább megadása futásidőben, ha felhívhatja a tetszés szerint elmélyedhet ügynök inicializálni előtt az alábbi módon:

    /* Engagement configuration. */
    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

    /* Initialize Engagement agent with above configuration. */
    EngagementAgent.Instance.Init(engagementConfiguration);

A kapcsolati karakterláncot, az alkalmazás megjelenik az Azure klasszikus portálon.

### <a name="engagement-initialization"></a>Tetszés szerint elmélyedhet inicializálni

Amikor egy új projektet hoz létre egy `App.xaml.cs` fájl jön létre. Az osztály örökli `Application` és számos fontos módszer tartalmaz. Is lesz a tetszés szerint elmélyedhet SDK inicializálni.

Módosítsa a `App.xaml.cs`:

-   Hozzáadása a `using` kimutatások:

        using Microsoft.Azure.Engagement;

-   Beszúrás `EngagementAgent.Instance.Init` a a `Application_Launching` módszer:

        private void Application_Launching(object sender, LaunchingEventArgs e)
        {
          EngagementAgent.Instance.Init();
        }

-   Beszúrás `EngagementAgent.Instance.OnActivated` a a `Application_Activated` módszer:

        private void Application_Activated(object sender, ActivatedEventArgs e)
        {
           EngagementAgent.Instance.OnActivated(e);
        }

> [AZURE.WARNING] Kifejezetten nem ajánlott a tetszés szerint elmélyedhet inicializálni felvenni az alkalmazás egy másik helyre. Jó helyen jár, amelyet érdemes szem előtt, hogy a `EngagementAgent.Instance.Init` módszer futtat egy dedikált szálon, nem pedig a felhasználói felület szál.

##<a name="basic-reporting"></a>Egyszerű jelentés

### <a name="recommended-method--overload-your-phoneapplicationpage-classes"></a>Ajánlott módszer: túlterhelés a `PhoneApplicationPage` osztályok

A jelentés tetszés szerint elmélyedhet számítja ki a felhasználókat, munkamenetek, tevékenységek, összeomlik és technikai statisztika által igényelt összes naplók aktiválásához egyszerűen teheti az összes a `PhoneApplicationPage` alszint osztályok örökölhet a `EngagementPage` osztályok.

Íme egy példa ennek módjáról az alkalmazás lap. Az alkalmazás minden oldalhoz ugyanazt műveleteket hajthat végre.

#### <a name="c-source-file"></a>C# forrásfájl

A lap módosítása `.xaml.cs` fájl:

-   Hozzáadása a `using` kimutatások:

        using Microsoft.Azure.Engagement;

-   Csere `PhoneApplicationPage` a `EngagementPage` :

**Tetszés szerint elmélyedhet: nélkül**

        namespace Example
        {
          public partial class ExamplePage : PhoneApplicationPage
          {
            [...]
          }
        }

**A tevékenységek:**

        using Microsoft.Azure.Engagement;
        
        namespace Example
        {
          public partial class ExamplePage : EngagementPage 
          {
            [...]
          }
        }

> [AZURE.WARNING] Ha az weblapon örökli a `OnNavigatedTo` módszer, ügyeljen arra, hogy a `base.OnNavigatedTo(e)` hívja. A tevékenység ellenkező esetben nem lesznek bejelentve. Sor kerülhet a `EngagementPage` hív `StartActivity` belül a `OnNavigatedTo` módot.

#### <a name="xaml-file"></a>XAML-fájl

A lap módosítása `.xaml` fájl:

-   A névtér deklarációs hozzáadása:

        xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP"

-   Csere `phone:PhoneApplicationPage` a `engagement:EngagementPage` :

**Tetszés szerint elmélyedhet: nélkül**

        <phone:PhoneApplicationPage>
            <!-- layout -->
        </phone:PhoneApplicationPage>

**A tevékenységek:**

        <engagement:EngagementPage 
            xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP">
        
            <!-- layout -->
        </engagement:EngagementPage >

#### <a name="override-the-default-behavior"></a>Az alapértelmezett működés felülbírálása

Alapértelmezés szerint az osztálynév, oldal készként a tevékenység neve, és nincs extra. Ha az osztály a "Lap" utótag használja, tetszés szerint elmélyedhet is törlődik.

Ha meg szeretné bírálja felül az alapértelmezett működés a jelölőnégyzetét, egyszerűen hozzáadása Ez a kód:

        // in the .xaml.cs file
        protected override string GetEngagementPageName()
        {
           /* your code */
           return "new name";
        }

Ha szeretne jelentést néhány további információt a tevékenységhez, ez a kód is felveheti:

        // in the .xaml.cs file
        protected override Dictionary<object,object> GetEngagementPageExtra()
        {
           /* your code */
           return extra;
        }

Ezeket a módszereket úgynevezett magából a `OnNavigatedTo` módszer a lapon.

### <a name="alternate-method-call-startactivity-manually"></a>Alternatív módszer: hívás `StartActivity()` manuálisan

Ha nem szeretne a többszörös vagy nem a `PhoneApplicationPage` osztályok, hívja fel a tevékenységek inkább elindíthatja `EngagementAgent` módszerek közvetlenül.

Azt javasoljuk, hogy hívó `StartActivity` belül a `OnNavigatedTo` a PhoneApplicationPage metódusát.

        protected override void OnNavigatedTo(NavigationEventArgs e)
        {
           base.OnNavigatedTo(e);
           EngagementAgent.Instance.StartActivity("MyPage");
        }

> [AZURE.IMPORTANT] Gondoskodjon arról, hogy helyesen befejezheti a munkamenetet.
>
> A SDK automatikusan felhívja a `EndActivity` módszer, ha az alkalmazás be van zárva. Így célszerű **erősen** ajánlott, ha fel szeretne hívni a `StartActivity` módszer, amikor a tevékenység annak a felhasználónak módosítása, és **Soha** hívja a `EndActivity` módot. Ez a módszer üzenetet küld a tetszés szerint elmélyedhet kiszolgálóhoz, hogy az aktuális felhasználó elhagyta az alkalmazást, és ez hatással van az összes alkalmazás naplók.

##<a name="advanced-reporting"></a>Speciális jelentése

Tetszés szerint érdemes jelentés alkalmazás bizonyos eseményeket, a hibák és a feladatok, így használja a többi módszerek található a `EngagementAgent` osztály. A tevékenységek API lehetővé teszi, hogy az összes tetszés szerint elmélyedhet a speciális funkciókat használja.

További tudnivalókért lásd [használatáról a speciális Mobile tetszés szerint elmélyedhet a Windows Phone-Silverlight-alkalmazás API-címkézés](mobile-engagement-windows-phone-use-engagement-api.md).

##<a name="advanced-configuration"></a>Speciális beállítások

### <a name="disable-automatic-crash-reporting"></a>Automatikus összeomlik jelentéskészítés letiltása

Az automatikus összeomlást jelentési funkció felvételi tilthatja le. Ezután, esetén nem kezelt kivételt akkor fordul elő, amikor tetszés szerint elmélyedhet semmi nem fogja elvégezni.

> [AZURE.WARNING] Ha ez a szolgáltatás letiltásához, ügyeljen arra, hogy egy esetén nem kezelt összeomlik akkor fordul elő, az alkalmazásban, ha tetszés szerint elmélyedhet nem küld az összeomlást, **és** nem lezárul a munkamenet és a feladatokat.

Automatikus összeomlik jelentéskészítés letiltásához testreszabása a deklarálva, akkor módja attól függően, hogy Ön konfigurációjának:

#### <a name="from-engagementconfigurationxml-file"></a>A `EngagementConfiguration.xml` fájl

Jelentés összeomlik beállítása `false` közötti `<reportCrash>` és `</reportCrash>` címkék.

#### <a name="from-engagementconfiguration-object-at-run-time"></a>A `EngagementConfiguration` futásidőben objektum

Állítsa a EngagementConfiguration objektuma segítségével hamis jelentés összeomlik.

        /* Engagement configuration. */

        EngagementConfiguration engagementConfiguration = new EngagementConfiguration(); engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";
        /\* Disable Engagement crash reporting. \*/ engagementConfiguration.Agent.ReportCrash = false;

### <a name="burst-mode"></a>Burst mód

Alapértelmezés szerint a tetszés szerint elmélyedhet szolgáltatás jelentések naplózza valós időben. Ha az alkalmazás jelentések naplók gyakran, akkor célszerűbb puffer a naplókat, és a jelentést őket egyszerre egy rendszeres időközönként alap (azaz "burst mód").

Ehhez hívás módszer:

        EngagementAgent.Instance.SetBurstThreshold(int everyMs);

Az argumentum értéke **ezredmásodpercben**. Bármikor szeretne a valós idejű naplózás újraaktiválása, ha csak hívja fel a módszerrel bármely paraméter nélkül, vagy a 0 értéket.

A burst mód kissé növelése az elem leírási_idő, de hatással van az tetszés szerint elmélyedhet monitoron: minden munkamenetek és a feladatok időtartamának (így a munkamenetek és a dolgozók rövidebb, mint a burst küszöbértékét nem láthatják a feladatok) burst küszöbérték lesz kerekítve. Egy mint 30000 (30s) burst küszöbérték használata ajánlott. Ügyeljen arra, hogy a naplók mentése korlátozódik 300 tételt van. Ha a Küldés túl hosszú elvesznek a néhány naplók.

> [AZURE.WARNING] A burst küszöbértékét nem állíthatók be egy kisebb időszak mint egy második. Ha ehhez a SDK jelennek meg a nyomkövetés a hibát, és automatikusan visszaállítása az alapértelmezett érték jelenik meg ez azt jelenti, hogy nulla másodperc. Ez elindítja a jelentés a naplókat valós idejű SDK csomagjában talál.
 
