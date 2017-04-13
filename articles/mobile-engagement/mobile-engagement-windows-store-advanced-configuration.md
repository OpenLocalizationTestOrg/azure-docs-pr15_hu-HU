<properties
    pageTitle="Speciális konfiguráció a Windows univerzális alkalmazások tetszés szerint elmélyedhet SDK"
    description="Speciális az univerzális alkalmazásokat a Windows Azure Mobile tetszés szerint elmélyedhet beállítási lehetőségei"                    
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
    ms.date="10/04/2016"
    ms.author="piyushjo;ricksal" />

# <a name="advanced-configuration-for-windows-universal-apps-engagement-sdk"></a>Speciális konfiguráció a Windows univerzális alkalmazások tetszés szerint elmélyedhet SDK

> [AZURE.SELECTOR]
- [Univerzális Windows](mobile-engagement-windows-store-advanced-configuration.md)
- [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md)
- [iOS](mobile-engagement-ios-integrate-engagement.md)
- [Android-alapú](mobile-engagement-android-advanced-configuration.md)

Ez az eljárás különböző konfigurációs beállításainak megadása Azure Mobile tetszés szerint elmélyedhet Android-alkalmazással ismerteti.

## <a name="prerequisites"></a>Előfeltételek

[AZURE.INCLUDE [Prereqs](../../includes/mobile-engagement-windows-store-prereqs.md)]

##<a name="advanced-configuration"></a>Speciális beállítások

### <a name="disable-automatic-crash-reporting"></a>Automatikus összeomlik jelentéskészítés letiltása

Az automatikus összeomlást jelentési funkció felvételi tilthatja le. Ezután esetén nem kezelt kivételt fordul elő, ha a tevékenységek nem módosítja.

> [AZURE.WARNING] Ha letiltja ezt a szolgáltatást, ha egy esetén nem kezelt összeomlik fordul elő, az alkalmazást, az tetszés szerint elmélyedhet nem küldje el az összeomlást, **és** zárja be a munkamenetet, és a feladatok.

Le szeretné tiltani a jelentési automatikus összeomlik, attól függően, hogy a módon deklarálva, akkor konfiguráció testreszabása:

#### <a name="from-engagementconfigurationxml-file"></a>A `EngagementConfiguration.xml` fájl

Jelentés összeomlik beállítása `false` közötti `<reportCrash>` és `</reportCrash>` címkék.

#### <a name="from-engagementconfiguration-object-at-run-time"></a>A `EngagementConfiguration` futásidőben objektum

Állítsa a EngagementConfiguration objektuma segítségével hamis jelentés összeomlik.

        /* Engagement configuration. */
        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

        /* Disable Engagement crash reporting. */
        engagementConfiguration.Agent.ReportCrash = false;

### <a name="disable-real-time-reporting"></a>Valós idejű jelentéskészítés letiltása

Alapértelmezés szerint a tetszés szerint elmélyedhet szolgáltatás jelentések naplózza valós időben. Ha az alkalmazás jelentések naplók gyakran, akkor célszerűbb puffer a naplókat, és a jelentés őket egyszerre rendszeres időközönként alapon. "Burst mód" nevezik.

Ehhez hívás módszer:

        EngagementAgent.Instance.SetBurstThreshold(int everyMs);

Az argumentum értéke **ezredmásodpercben**. Bármikor újraaktiválása a valós idejű naplózás, hívja fel a módszerrel bármely paraméter nélkül, vagy a 0 értéket.

Burst mód kissé növeli az elem leírási_idő, de hatással van az tetszés szerint elmélyedhet monitoron: minden munkamenetek és a feladatok időtartamának (így a munkamenetek és a dolgozók rövidebb, mint a burst küszöbértékét nem láthatják a feladatok) burst küszöbértéknél kerekítve. Egy mint 30000 (30s) burst küszöbérték használata ajánlott. Mentett naplók fognak korlátozódni 300 tételt. Ha a Küldés túl hosszú, elvesznek a néhány naplók.

> [AZURE.WARNING] Nem lehet beállítani a burst küszöbértékét, időszakra kisebb, mint egy második. Ha így tesz, a SDK jeleníti meg a nyomkövetési a hibával, és automatikusan visszaállítja az alapértelmezett érték nulla másodperc. Ez elindítja a jelentés a naplókat valós idejű SDK csomagjában talál.

[here]:http://www.nuget.org/packages/Capptain.WindowsCS
[NuGet website]:http://docs.nuget.org/docs/start-here/overview
