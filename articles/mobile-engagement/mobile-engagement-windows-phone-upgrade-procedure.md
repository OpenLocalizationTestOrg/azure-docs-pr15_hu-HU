<properties 
    pageTitle="Windows Phone Silverlight SDK frissítési eljárások" 
    description="Windows Phone Silverlight SDK frissítési eljárás az Azure mobil tetszés szerint elmélyedhet"                  
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

#<a name="windows-phone-silverlight-sdk-upgrade-procedures"></a>Windows Phone Silverlight SDK frissítési eljárások

Ha akkor már van beépül a SDK régebbi verzióját az alkalmazást, akkor a SDK frissítéskor, vegye figyelembe az alábbiakat.

Előfordulhat, hogy végre kell hajtani több eljárás, ha Ön kihagyott a SDK különböző verzióiban. Ha például telepít át 0.10.1 0.11.0 először menete a "feladó 0.9.0 való 0.10.1" kell majd a "from 0.10.1 való 0.11.0" eljárást.

##<a name="from-200-to-330"></a>A 2.0.0 való 3.3.0

### <a name="test-logs"></a>Naplók tesztelése

A SDK készített konzol naplók lehet most engedélyezett/letiltott/szűrt. Testreszabásához ez a tulajdonság frissítéséhez `EngagementAgent.Instance.TestLogEnabled` az egyik elérhető értékét a `EngagementTestLogLevel` számbavétel, például:

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();

##<a name="from-111-to-200"></a>A 2.0.0 való 1.1.1

Az alábbi leírja, hogy miként telepítheti át SDK integrációs-alkalmazásba, Azure Mobile tetszés szerint elmélyedhet hajtott Capptain Társítások által kínált Capptain szolgáltatás. 

> [Azure.IMPORTANT] Capptain és Mobile tetszés szerint elmélyedhet nem ugyanazok a szolgáltatások és az alábbi eljárás csak kiemeli az ügyfél alkalmazás áttelepítése. Az alkalmazásban a SDK áttelepítése nem áttelepíti az adatok a Capptain kiszolgálókról Mobile tetszés szerint elmélyedhet kiszolgálókhoz

Ha egy korábbi verzióról, olvassa el a Capptain webhely 1.1.1 először áttelepítése, majd az alábbi eljárást alkalmazni

### <a name="nuget-package"></a>Nuget csomag

**Capptain.WindowsPhone** felülírásához **MicrosoftAzure.MobileEngagement** Nuget csomagot.

### <a name="applying-mobile-engagement"></a>Mobil tetszés szerint elmélyedhet alkalmazása

A SDK csomagjában talál a kifejezést használja `Engagement`. Módosítania kell a projekthez, ezt a módosítást megfelelően.

Akkor el kell távolítania az aktuális Capptain nuget csomagolásán látható termékszámmal. Fontolja meg, hogy Capptain erőforrások mappában található összes módosítás törlődik. Ha meg szeretné őrizni a azokat a fájlokat végezze el egy példányát.

Ezt követően telepítse a Microsoft Azure Engagement nuget új csomag a projekt. Közvetlenül a [Nuget](http://www.nuget.org/packages/MicrosoftAzure.MobileEngagement)megtalálhatja. Ez a művelet felülírja az összes erőforrások fájlokat tetszés szerint elmélyedhet által használt, és felveszi az új tetszés szerint elmélyedhet DLL a projekt hivatkozásait.

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

4. A további erőforrások, mint a Capptain képek, a Felhívjuk a figyelmét arra, hogy azok is átnevezett "Tevékenységek" használni.

### <a name="application-id--sdk-key"></a>Azonosítója / SDK billentyűt

Tetszés szerint elmélyedhet a kapcsolati karakterlánc használja. Nem kell az azonosítója, és egy SDK kulcs Mobile tetszés szerint elmélyedhet adja meg, csak akkor adjon meg egy kapcsolati karakterláncot. Beállíthatja azt a EngagementConfiguration a fájlon.

A tevékenységek konfiguráció állíthatók be a `Resources\EngagementConfiguration.xml` a projekt fájlt.

Adja meg a fájlt szerkeszteni:

-   Az alkalmazás kapcsolati karakterlánc között címkék `<connectionString>` és `<\connectionString>`.

Szeretné inkább megadása futásidőben, ha felhívhatja a tetszés szerint elmélyedhet ügynök inicializálni előtt az alábbi módon:

        /* Engagement configuration. */
        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";
        
        /* Initialize Engagement angent with above configuration. */
        EngagementAgent.Instance.Init(engagementConfiguration);

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



 
