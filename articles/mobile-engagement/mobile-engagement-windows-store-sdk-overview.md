<properties
    pageTitle="Windows SDK univerzális integrációja"
    description="A Windows univerzális integrációját SDK az Azure mobil tetszés szerint elmélyedhet"                                     
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
    ms.date="08/12/2016"
    ms.author="piyushjo;ricksal" />

#<a name="windows-universal-sdk-integration-for-azure-mobile-engagement"></a>A Windows Azure mobil tetszés szerint elmélyedhet univerzális SDK integrációját

A dokumentum összes az integráció és konfigurációs lehetőségek a Azure Mobile tetszés szerint elmélyedhet Windows univerzális SDK ismerteti.

## <a name="prerequisites"></a>Előfeltételek

Ebben az oktatóanyagban kezdve, előbb végre kell hajtania a [15 perces oktatóprogram](mobile-engagement-windows-store-dotnet-get-started.md).

## <a name="advanced-features"></a>Speciális szolgáltatásaira

### <a name="reporting-features"></a>Jelentéskészítési funkciók
Ezek a funkciók adhat meg:

1. [Speciális beállítások jelentéskészítési](mobile-engagement-windows-store-advanced-reporting.md)

2. [Speciális konfigurációs lehetőségek](mobile-engagement-windows-store-advanced-configuration.md)

### <a name="notifications"></a>Értesítések

[Hogyan vannak (értesítések) integráció a Windows univerzális alkalmazásban](mobile-engagement-windows-store-integrate-engagement-reach.md)

### <a name="tag-plan-implementation"></a>Címke csomag végrehajtása:

[A speciális Mobile tetszés szerint elmélyedhet címkézés API-t a Windows univerzális alkalmazás használata](mobile-engagement-windows-store-use-engagement-api.md)

##<a name="release-notes"></a>Kibocsátási megjegyzések

###<a name="340-04192016"></a>3.4.0 (04/19 vagy 2016)

-   További fejlesztések elérni.
-   A felvett "TestLogLevel" API-val való engedélyezése vagy letiltása és szűrés konzol naplók a SDK által kibocsátott.
-   Indítsa el a rögzített a tevékenység értesítések nem jelennek meg az alkalmazást az első tevékenység kiválasztásával.

Korábbi verziók olvassa el a [teljes kibocsátási megjegyzések](mobile-engagement-windows-store-release-notes.md) című témakört.

##<a name="upgrade-procedures"></a>Frissítési eljárások

Ha Ön már rendelkezik integrált tetszés szerint elmélyedhet a régebbi verzióját az alkalmazásba, akkor a SDK frissítéskor, vegye figyelembe az alábbiakat.

Ha Ön kihagyott a SDK különböző verzióiban, előfordulhat kövesse több eljárásokat. A teljes [Frissítése eljárások](mobile-engagement-windows-store-upgrade-procedure.md)találhat. Ha például telepít át 0.10.1 0.11.0 először menete a "feladó 0.9.0 való 0.10.1" kell majd a "from 0.10.1 való 0.11.0" eljárást.

###<a name="from-330-to-340"></a>A 3.3.0 való 3.4.0

####<a name="test-logs"></a>Naplók tesztelése

A SDK készített konzol naplók lehet most engedélyezett/letiltott/szűrt. Ha testre szeretné szabni, a tulajdonság frissítéséhez `EngagementAgent.Instance.TestLogEnabled` az egyik elérhető értékeket a `EngagementTestLogLevel` számbavétel, például:

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();

####<a name="resources"></a>Erőforrások

Továbbfejlesztettük a gyermekektől átfedő. A SDK NuGet csomag forrásai része.

Az új verziójára a SDK frissítésekor választhat, hogy szeretné-e meg, hogy a meglévő fájlok erőforrásait a átfedő mappát, vagy nem:

* Ha az előző átfedés akkor működik, vagy integrálni kívánja-e a `WebView` elemeket manuálisan, majd úgy is dönt, hogy a Kilépés fájlokat, akkor is működnek.
* Az új átfedés frissítéséhez cserélje le a teljes `overlay` az erőforrások és a SDK csomagból az új egy mappájából (UWP alkalmazások: a frissítés után, amely letölthető az új átfedő mappa használata %\\.nuget\packages\MicrosoftAzure.MobileEngagement\3.4.0\content\win81\Resources).

> [AZURE.WARNING] Az új átfedés használatával felülírja a tartalomtípusokon végzett az előző verzióhoz képest.

### <a name="upgrade-from-older-versions"></a>Régebbi verziójával frissítése

Lásd: a [eljárások frissítése](mobile-engagement-windows-store-upgrade-procedure.md)
