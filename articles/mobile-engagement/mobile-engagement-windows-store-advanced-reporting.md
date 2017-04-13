<properties
    pageTitle="Speciális jelentéskészítés MobileApps tetszés szerint elmélyedhet a Windows univerzális"
    description="Univerzális alkalmazásokat a Windows Azure mobil tetszés szerint elmélyedhet integrálása"                  
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows-store"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/12/2016"
    ms.author="piyushjo;ricksal" />

# <a name="advanced-reporting-with-the-windows-universal-apps-engagement-sdk"></a>Speciális, a Windows SDK univerzális alkalmazások részvételét a jelentéskészítés

> [AZURE.SELECTOR]
- [Univerzális Windows](mobile-engagement-windows-store-advanced-reporting.md)
- [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md)
- [iOS](mobile-engagement-ios-integrate-engagement.md)
- [Android-alapú](mobile-engagement-android-advanced-reporting.md)

Ez a témakör ismerteti a Windows univerzális alkalmazásban további jelentéskészítő forgatókönyvek. Ez a helyzet, amelyet választva az alkalmazás az [Első lépések](mobile-engagement-windows-store-dotnet-get-started.md) oktatóprogram során létre beállítások tartalmazza.

## <a name="prerequisites"></a>Előfeltételek

[AZURE.INCLUDE [Prereqs](../../includes/mobile-engagement-windows-store-prereqs.md)]

Ebben az oktatóanyagban indításához először végezze el az [Első lépések](mobile-engagement-windows-store-dotnet-get-started.md) oktatóprogram, amelyet kifejezetten közvetlen és egyszerű. Ebben az oktatóanyagban bemutatja a további lehetőségek közül választhat.

## <a name="specifying-engagement-configuration-at-runtime"></a>Tetszés szerint elmélyedhet konfigurációs futásidőben megadása

A tevékenységek konfiguráció van központi a a `Resources\EngagementConfiguration.xml` a projekt, vagyis ahol megadva az [Első lépések](mobile-engagement-windows-store-dotnet-get-started.md) a témakör a fájlt.

De futásidőben is megadhatja: felhívhatja a tetszés szerint elmélyedhet ügynök inicializálni előtt az alábbi módon:

          /* Engagement configuration. */
          EngagementConfiguration engagementConfiguration = new EngagementConfiguration();

          /* Set the Engagement connection string. */
          engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

          /* Initialize Engagement angent with above configuration. */
          EngagementAgent.Instance.Init(e, engagementConfiguration);



## <a name="recommended-method-overload-your-page-classes"></a>Ajánlott módszer: túlterhelés a `Page` osztályok

Tetszés szerint elmélyedhet számítja ki a felhasználókat, munkamenetek, tevékenységek, összeomlik és technikai statisztika által igényelt összes naplók jelentésének aktiválásához ellenőrizze az összes a `Page` alszint osztályok örökölhet a `EngagementPage` osztályok.

Íme egy példa az alkalmazás az weblapon. Az alkalmazás minden oldalhoz ugyanazt műveleteket hajthat végre.

### <a name="c-source-file"></a>C# forrásfájl

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

> [AZURE.IMPORTANT] Ha a lap felülírja a `OnNavigatedTo` módszer, ne felejtse el, hívja fel `base.OnNavigatedTo(e)`. Egyéb esetben a tevékenység nem kell jelenteni (a `EngagementPage` hívások `StartActivity` belül a `OnNavigatedTo` módszer).

### <a name="xaml-file"></a>XAML-fájl

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

### <a name="override-the-default-behaviour"></a>Az alapértelmezett viselkedésének felülbírálása

Alapértelmezés szerint az osztálynév, oldal készként a tevékenység neve, és nincs extra. Ha az osztály a "Lap" utótag használja, tetszés szerint elmélyedhet eltávolítja.

Bírálja felül az alapértelmezett működés a jelölőnégyzetét, vegyen fel ezt a kódot:

        // in the .xaml.cs file
        protected override string GetEngagementPageName()
        {
          /* your code */
          return "new name";
        }

További információt a tevékenységgel jelentést, a kód hozzáadása:

        // in the .xaml.cs file
        protected override Dictionary<object,object> GetEngagementPageExtra()
        {
          /* your code */
          return extra;
        }

Ezeket a módszereket úgynevezett magából a `OnNavigatedTo` módszer a lapon.

### <a name="alternate-method-call-startactivity-manually"></a>Alternatív módszer: hívás `StartActivity()` manuálisan

Ha nem szeretne a többszörös vagy nem a `Page` osztályok, hívja fel a tevékenységek inkább elindíthatja `EngagementAgent` módszerek közvetlenül.

Azt javasoljuk, hogy hívó `StartActivity` belül a `OnNavigatedTo` a lap metódusát.

            protected override void OnNavigatedTo(NavigationEventArgs e)
            {
              base.OnNavigatedTo(e);
              EngagementAgent.Instance.StartActivity("MyPage");
            }

> [AZURE.IMPORTANT]  Gondoskodjon arról, hogy helyesen befejezheti a munkamenetet.
>
> A Windows univerzális SDK automatikusan felhívja a `EndActivity` módszer, ha az alkalmazás be van zárva. Így célszerű **erősen** ajánlott, ha fel szeretne hívni a `StartActivity` módszer, amikor a tevékenység annak a felhasználónak módosítása, és **Soha** hívja a `EndActivity` módot. Ez a módszer jelzi, hogy az aktuális felhasználó elhagyta az alkalmazás, amely az összes alkalmazás naplók hatással lesz a tetszés szerint elmélyedhet kiszolgáló.

## <a name="advanced-reporting"></a>Speciális jelentése

Ha szükséges, előfordulhat, hogy jelenteni kívánt alkalmazás-specifikus eseményeket, a hibák és a feladatok ehhez használja a többi módszerek található a `EngagementAgent` osztály. A tevékenységek API lehetővé teszi, hogy az összes részvételét speciális funkciók használata.

További tudnivalókért lásd [használatáról a speciális Mobile tetszés szerint elmélyedhet címkézés API-t a Windows univerzális alkalmazásban](mobile-engagement-windows-store-use-engagement-api.md).
