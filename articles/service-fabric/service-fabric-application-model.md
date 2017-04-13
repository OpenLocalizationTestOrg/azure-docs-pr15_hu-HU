<properties
   pageTitle="Szolgáltatás háló alkalmazásmodell |} Microsoft Azure"
   description="Hogyan modell és -alkalmazások és szolgáltatások szolgáltatás háló ismertetik."
   services="service-fabric"
   documentationCenter=".net"
   authors="rwike77"
   manager="timlt"
   editor="mani-ramaswamy"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/10/2016"   
   ms.author="seanmck"/>

# <a name="model-an-application-in-service-fabric"></a>A szolgáltatás háló céljával modell

Ez a cikk áttekintést nyújt az Azure Service háló alkalmazás modell. Azt is megtudhatja, hogy miként határozza meg az alkalmazás és a via jegyzékfájlok szolgáltatás és az alkalmazás csomagolt, és készen áll a telepítés.

## <a name="understand-the-application-model"></a>A alkalmazásmodell ismertetése

Az alkalmazás, amely egy bizonyos függvény és a függvények alkotó szolgáltatások gyűjteménye. Egy szolgáltatás hajt végre (képes indítása és más szolgáltatások független futtatása) teljes és a különálló függvény és a kódot, beállításainak és adatok tevődik össze. Az egyes szolgáltatásokhoz az végrehajtható bináris kód áll, és konfigurációs áll futásidőben betöltött szolgáltatás beállításai szolgáltatás felhasználandó tetszőleges statikus adatokat tartalmaz. A hierarchikus alkalmazásmodell minden összetevő lehet verziószámmal és frissített egymástól függetlenül.

![A szolgáltatás háló alkalmazásmodell][appmodel-diagram]


Egy alkalmazás sorolható, hogy egy alkalmazást, és egy köteg szolgáltatás típusú áll. A szolgáltatás típusa a szolgáltatás kategorizálást. A kategorizálás is rendelkezik, különböző beállítások és konfigurációk, de az alapvető funkcionalitást változatlan marad. A szolgáltatás példányát a másik szolgáltatáscsaládba konfigurációs változatok szolgáltatás azonos típusú.  

Osztályok (vagy "típus") az alkalmazások és szolgáltatások leírása keresztül XML-fájlok (alkalmazás és szolgáltatás jegyzékfájlok), amelyek a sablonokat, amelyek ellen alkalmazások lehet létrehozni a fürt kép áruházból. A ServiceManifest.xml és ApplicationManifest.xml fájl sémadefiníciója telepítve van a szolgáltatás háló SDK és a *C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*eszközöket.

A különböző szolgáltatásalkalmazás-példányok kódját, akkor is, ha a azonos szolgáltatás háló csomópont külön folyamatok fog futni. Ezenkívül a minden alkalmazás példányának életciklusáról (azaz kezelhető frissített) egymástól függetlenül. Az alábbi ábra mutatja, hogy hogyan alkalmazás típusú szolgáltatás-típusok viszont tevődnek össze kódot, beállításainak és csomagok állnak. A diagram leegyszerűsítése csak a kód / / adatainak webhelyoszlopai csomagok `ServiceType4` láthatók, mintha minden szolgáltatás típusa tartalmaz néhány vagy az összes csomag típusú.

![Szolgáltatás háló alkalmazás és szolgáltatás típusainak][cluster-imagestore-apptypes]

Két különböző jegyzékfájlok használt alkalmazások és szolgáltatások leírása: a szolgáltatás jegyzék és az alkalmazás jegyzék. Ezek tartoznak, az ezt követő szakaszok részletesen.

A szolgáltatás típusú egy vagy több példány lehet a fürt aktív. Például állapot-nyilvántartó szolgáltatás példányainak vagy replikák elérése magas megbízhatóság állam megjegyzésekről a fürt csomópontjai található kópiák közötti úgy. A replikáció lényegében biztosít a szolgáltatása számára érhető el, ha egy fürt egy csomópontjának nem sikerül redundancia. [Particionált szolgáltatás](service-fabric-concepts-partitioning.md) további elosztja az állapotába (és az access mintázatú kiemelésével, hogy az állapot) végig a fürt csomópontjai.

Az alábbi ábra mutatja az alkalmazások és a szolgáltatás példányainak, partíciót és kópiák közötti kapcsolat.

![Partíciók és kópiák szolgáltatás belül][cluster-application-instances]


>[AZURE.TIP] Az alkalmazások az Elrendezés megtekintése a eszközzel szolgáltatás háló Explorer érhető el a http:// fürt&lt;yourclusteraddress&gt;: 19080/Explorer. További részletekért olvassa el [a fürt háló Intézővel megjelenítésére](service-fabric-visualizing-your-cluster.md).

## <a name="describe-a-service"></a>A szolgáltatás leírása

A szolgáltatás jegyzék deklaratív határozza meg a szolgáltatás típusa és verzióját. Azt adja meg a szolgáltatás metaadatokat, például a szolgáltatás típusa, állapot tulajdonságait, terheléselosztási mértékek, szolgáltatás bináris és konfigurációs fájlokat.  Helyezi úgy, azt mutatja be a kódot, beállításainak és adatok csomagok a egy vagy több szolgáltatás típusa támogatási szolgáltatás csomag alkotó. Íme egy egyszerű példa szolgáltatás jegyzék:

~~~
<?xml version="1.0" encoding="utf-8" ?>
<ServiceManifest Name="MyServiceManifest" Version="SvcManifestVersion1" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Description>An example service manifest</Description>
  <ServiceTypes>
    <StatelessServiceType ServiceTypeName="MyServiceType" />
  </ServiceTypes>
  <CodePackage Name="MyCode" Version="CodeVersion1">
    <SetupEntryPoint>
      <ExeHost>
        <Program>MySetup.bat</Program>
      </ExeHost>
    </SetupEntryPoint>
    <EntryPoint>
      <ExeHost>
        <Program>MyServiceHost.exe</Program>
      </ExeHost>
    </EntryPoint>
  </CodePackage>
  <ConfigPackage Name="MyConfig" Version="ConfigVersion1" />
  <DataPackage Name="MyData" Version="DataVersion1" />
</ServiceManifest>
~~~

**Verzió** attribútumok strukturálatlan karakterláncok, és nem elemzi a rendszer. Ezek a verzióra használt minden összetevő frissítések.

**ServiceTypes** deklarálása szolgáltatás milyen az e jegyzék **CodePackages** által támogatott. Amikor szolgáltatás létrejön egy ilyen szolgáltatás típusú szemben, a jegyzék deklarált csomagokhoz kód azok belépési pontról futtatásával aktiválva vannak. Az eredményül kapott folyamatok várhatóan regisztrálhatja a szolgáltatás által támogatott fájltípusok futásidőben. Figyelje meg, hogy a szolgáltatás típusok vannak deklarálva a nyilvánvalóan szinten, és nem a kód csomag szintet. Így több kódot csomagok esetén azok minden aktiválódnak, amikor a rendszer feltüntetett szolgáltatás fájltípusok valamelyikét keresi.

**SetupEntryPoint** található a Yammerhez, hogy fut, a hitelesítő adatait *(általában a rendszerfiókot)* szolgáltatás háló mint bármely más belépési pontról előtt. **Belépési** által megadott végrehajtható az általában a hosszabb ideig futó szolgáltatás szolgáltató. Egy külön beállítási belépési pontjához jelenlétét elkerülhetők a nem kell a service host futtatása magas jogosultságokkal rendelkező hosszabb ideig. Miután sikeresen kijelentkezik a **SetupEntryPoint** futtatása **belépési** által meghatározott végrehajtható fájl Az eredményül kapott folyamat van ellenőrizni, és újraindul (kezdődő újra **SetupEntryPoint**), ha minden eddiginél ér véget, vagy összeomlik.

**DataPackage** deklarálása egy, a **név** attribútum, a folyamat futásidőben felhasználandó tetszőleges statikus adatokat tartalmazó nevű mappát.

**ConfigPackage** deklarálása szerint attribútum **nevét** , *Settings.xml* fájlt tartalmazó mappa. Ez a fájl a felhasználó által definiált, a kulcs-érték pár beállításokat, amelyek a folyamat olvashatják vissza futásidőben szakaszt tartalmaz. A frissítés során Ha csak a **ConfigPackage** **verzió** megváltozott, majd a folytonos folyamat nem újraindítása. A visszahívási ehelyett értesíti a konfigurációs beállítások megváltoztak, így azok dinamikusan kell tölteni a folyamat. Íme egy példa *Settings.xml* fájl:

~~~
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Section Name="MyConfigurationSecion">
    <Parameter Name="MySettingA" Value="Example1" />
    <Parameter Name="MySettingB" Value="Example2" />
  </Section>
</Settings>
~~~

> [AZURE.NOTE] A szolgáltatás jegyzék is tartalmazhatnak, több kódot, beállításainak és adatok csomagok. Minden egyes azok lehet verziószámmal egymástól függetlenül.

<!--
For more information about other features supported by service manifests, refer to the following articles:

*TODO: LoadMetrics, PlacementConstraints, ServicePlacementPolicies
*TODO: Resources
*TODO: Health properties
*TODO: Trace filters
*TODO: Configuration overrides
-->

## <a name="describe-an-application"></a>Az alkalmazás leírása


Az alkalmazás jegyzék deklaratív alkalmazás típusa és verzió ismerteti. Adja meg a szolgáltatási kialakítási metaadatok például az állandó nevét, példány darab/replikációs tényező, biztonsági/elkülönítési házirendet, elhelyezése kényszerek, konfigurációs felülbírálása, és a alkotó szolgáltatás típusa. A terheléselosztási tartományokat, amelybe az alkalmazás felépítése is ismerteti.

Így-alkalmazás jegyzék ismerteti az alkalmazás szintjén elemek, és egy vagy több szolgáltatás jegyzékfájlok-alkalmazás típusa írhat hivatkozik. Íme egy egyszerű példa alkalmazás jegyzék:

~~~
<?xml version="1.0" encoding="utf-8" ?>
<ApplicationManifest
      ApplicationTypeName="MyApplicationType"
      ApplicationTypeVersion="AppManifestVersion1"
      xmlns="http://schemas.microsoft.com/2011/01/fabric"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Description>An example application manifest</Description>
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="MyServiceManifest" ServiceManifestVersion="SvcManifestVersion1"/>
  </ServiceManifestImport>
  <DefaultServices>
     <Service Name="MyService">
         <StatelessService ServiceTypeName="MyServiceType" InstanceCount="1">
             <SingletonPartition/>
         </StatelessService>
     </Service>
  </DefaultServices>
</ApplicationManifest>
~~~

Szolgáltatás-jegyzékfájlok, például **verzió** attribútumok strukturálatlan karakterláncok, és a rendszer nem elemzésének. Az alábbiakban is használható verzióra minden összetevő frissítések.

**ServiceManifestImport** szolgáltatás jegyzékfájlok alkalmazás típus alkotó hivatkozásokat tartalmaz. Importált szolgáltatás jegyzékfájlok megállapíthatja, hogy milyen szolgáltatás típusok érvényesek alkalmazás típus belül.

**DefaultServices** deklarálása szolgáltatás példányai, amelyek automatikusan jönnek létre, amikor az alkalmazás ellen alkalmazás típus jön létre. Alapértelmezett szolgáltatások csak a könnyebb, és úgy viselkednek, mint normál szolgáltatások minden szempontból után hozott létre. Ezeket az alkalmazás-példány bármely más szolgáltatásokkal együtt frissülnek, és is eltávolítható.

> [AZURE.NOTE] Az alkalmazás jegyzék több szolgáltatás nyilvánvalóan importálások és alapértelmezett szolgáltatások is tartalmazhat. Minden egyes szolgáltatás nyilvánvalóan importálása független verziószámmal lehet.

Másik alkalmazás és a szolgáltatás paramétereinek egyedi környezetek kezelése című témakörben olvashat, olvassa el a [kezelése alkalmazás paraméterei több környezetben](service-fabric-manage-multiple-environment-app-configuration.md)című témakört.

<!--
For more information about other features supported by application manifests, refer to the following articles:

*TODO: Application parameters
*TODO: Security, Principals, RunAs, SecurityAccessPolicy
*TODO: Service Templates
-->

## <a name="package-an-application"></a>Az alkalmazások csomag

### <a name="package-layout"></a>Csomag elrendezés

Az alkalmazás jegyzék szolgáltatás manifest(s) és más szükséges csomag fájlok be a szolgáltatás háló fürtre telepítéshez adott elrendezésben kell rendezve. A példa jegyzékfájlok ebben a cikkben az alábbi címtár-struktúra szervezett kellene:

~~~
PS D:\temp> tree /f .\MyApplicationType

D:\TEMP\MYAPPLICATIONTYPE
│   ApplicationManifest.xml
│
└───MyServiceManifest
    │   ServiceManifest.xml
    │
    ├───MyCode
    │       MyServiceHost.exe
    │
    ├───MyConfig
    │       Settings.xml
    │
    └───MyData
            init.dat
~~~

A mappák megfelelően **a nevük minden megfelelő elem** neve. Például ha a szolgáltatás jegyzék **MyCodeA** és **MyCodeB**neveivel két kód csomagot, majd két mappa megegyező nevű tartalmaz az egyes kód csomag szükséges bináris.

### <a name="use-setupentrypoint"></a>SetupEntryPoint használata

Tipikus esetei **SetupEntryPoint** használata esetén szüksége van egy végrehajtható fájl futtatása előtt a szolgáltatás indítása vagy a művelet végrehajtása, emelt szintű jogosultsággal kell. Példa:

- Állíthatja be, és a szolgáltatás végrehajtható fájl igénylő környezeti változók inicializálása. Ez nem csak a szolgáltatási háló programozási modellek keresztül írt végrehajtható korlátozódik. Például npm.exe kell néhány környezeti változók be van állítva egy node.js alkalmazás telepítése.

- Állítsa be a hozzáférés-vezérlés biztonsági tanúsítványok telepítésével.

### <a name="build-a-package-by-using-visual-studio"></a>Csomagot készíthet a Visual Studio használatával

Visual Studio 2015 hozhat létre az alkalmazás használatakor a csomag paranccsal automatikusan létrehoz egy csomagot, amely megfelel a fentebb ismertetett elrendezést.

Csomag létrehozása, kattintson a jobb gombbal a megoldást Intéző alkalmazást projekt, és válassza a csomag parancs, alább látható módon:

![A Visual Studio céljával csomagolása][vs-package-command]

Összecsomagolása befejeződése után található a csomag helyét a **kimeneti** ablakban. Figyelje meg, hogy a csomagolást lépés automatikusan megtörténik amikor telepítése vagy az alkalmazás a Visual Studióban hibakeresése.

### <a name="test-the-package"></a>A csomag tesztelése

A csomag szerkezet helyileg Powershellen keresztül ellenőrizheti a **Próba-ServiceFabricApplicationPackage** parancs használatával. Ez a parancs fog jegyzék elemzése a problémák ellenőrzése, és ellenőrizze az összes hivatkozást. Figyelje meg, hogy ez a parancs csak ellenőrzi a mappák és a fájlokat az előkészítés szerkezeti helyességét. Nem ellenőrzi a kódot vagy adatok csomag tartalma túl ellenőrzése, hogy az összes szükséges fájlok közül.

~~~
PS D:\temp> Test-ServiceFabricApplicationPackage .\MyApplicationType
False
Test-ServiceFabricApplicationPackage : The EntryPoint MySetup.bat is not found.
FileName: C:\Users\servicefabric\AppData\Local\Temp\TestApplicationPackage_7195781181\nrri205a.e2h\MyApplicationType\MyServiceManifest\ServiceManifest.xml
~~~

Ez a hiba azt jelzi, hogy a hivatkozott a szolgáltatás jegyzék **SetupEntryPoint** *MySetup.bat* fájl hiányzik a kód csomagból. A hiányzó fájl hozzáadása után továbbítja az alkalmazás ellenőrzése:

~~~
PS D:\temp> tree /f .\MyApplicationType

D:\TEMP\MYAPPLICATIONTYPE
│   ApplicationManifest.xml
│
└───MyServiceManifest
    │   ServiceManifest.xml
    │
    ├───MyCode
    │       MyServiceHost.exe
    │       MySetup.bat
    │
    ├───MyConfig
    │       Settings.xml
    │
    └───MyData
            init.dat

PS D:\temp> Test-ServiceFabricApplicationPackage .\MyApplicationType
True
PS D:\temp>
~~~

Miután az alkalmazás csomagolt helyesen, és átadja a tartomány ellenőrzéséhez, majd készen áll a telepítéshez.

## <a name="next-steps"></a>Következő lépések

[Üzembe helyezéséhez és alkalmazások eltávolítása][10]

[Több környezetekben alkalmazás paramétereinek kezelése][11]

[RunAs: Szolgáltatás háló alkalmazást futtató különböző biztonsági engedélyekkel][12]

<!--Image references-->
[appmodel-diagram]: ./media/service-fabric-application-model/application-model.png
[cluster-imagestore-apptypes]: ./media/service-fabric-application-model/cluster-imagestore-apptypes.png
[cluster-application-instances]: media/service-fabric-application-model/cluster-application-instances.png
[vs-package-command]: ./media/service-fabric-application-model/vs-package-command.png

<!--Link references--In actual articles, you only need a single period before the slash-->
[10]: service-fabric-deploy-remove-applications.md
[11]: service-fabric-manage-multiple-environment-app-configuration.md
[12]: service-fabric-application-runas-security.md
