<properties
   pageTitle="Elasticsearch használ szolgáltatási háló alkalmazás nyomkövetési üzlet |} Microsoft Azure"
   description="Ismerteti, hogyan szolgáltatás háló alkalmazások segítségével Elasticsearch és Kibana tárolja, index, és keresni az alkalmazás halad (naplók)"
   services="service-fabric"
   documentationCenter=".net"
   authors="karolz-ms"
   manager="adegeo"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/09/2016"
   ms.author="karolz@microsoft.com"/>

# <a name="use-elasticsearch-as-a-service-fabric-application-trace-store"></a>Használata Elasticsearch, mint a szolgáltatás háló alkalmazás nyomkövetési tárolása
## <a name="introduction"></a>– Bevezetés
Ez a cikk azt ismerteti, hogyan [Azure Service háló](https://azure.microsoft.com/documentation/services/service-fabric/) alkalmazások használható **Elasticsearch** és **Kibana** alkalmazás nyomkövetési tárhely, az indexelés és a keresés. [Elasticsearch](https://www.elastic.co/guide/index.html) Megnyitás-forrást, elosztott és méretezhető valós idejű keresési és az elemzést motor, hogy jól illeszkedik ehhez a feladathoz. Telepíthető Windows- és Linux virtuális gépeken futó Microsoft Azure-ban. Elasticsearch hatékonyan dolgozhat előállított technológiák, például **Eseményvezérelt Tracing a Windows (esemény-nyomkövetés)**használata *a strukturált* nyomkövetések.

Az adatforrás diagnosztikai adatok (halad) szolgáltatás háló futtatókörnyezet esemény-nyomkövetés használják. Az ajánlott forrás túl a diagnosztikai adataikat, az alkalmazások szolgáltatás háló módszer. Az azonos eljárás használatával lehetővé teszi korrelációs futási idejű által biztosított és alkalmazástól kapott halad és hibaelhárítási egyszerűbbé teszi között. Szolgáltatás háló projektsablonok a Visual Studióban a naplózás API-val (a.NET **EventSource** osztály alapján), amely alapértelmezés szerint a esemény-nyomkövetés halad bocsát tartalmazza. Általános áttekintés esemény-nyomkövetés használ szolgáltatási háló alkalmazás nyomkövetés olvassa el a [Figyelés és a szolgáltatások egy helyi számítógép zónában fejlesztési beállítása diagnosztizálása](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)című témakört.

A nyomkövetési információk megjelennek a Elasticsearch azok kell rögzítse a valós idejű szolgáltatás háló fürt csomópontok (futtatásakor az alkalmazás) és a Elasticsearch zárólap küldött. Kétféleképpen lehet fő nyomkövetési rögzítéséhez:

+ **Az eljárás nyomkövetési rögzítése**  
Az alkalmazás, vagy még pontosabban szolgáltatási folyamat, a felelős a küldje el a diagnosztikai adatok a nyomkövetési áruház (Elasticsearch).

+ **A rögzítés a folyamaton nyomon követése**  
Egy külön ügynököt a szolgáltatási folyamat, illetve a folyamatok halad rögzítéséhez és elküldeni a nyomkövetési áruházból.

Az alábbi azt ismertetik, hogy hogyan állíthatja be a Azure Elasticsearch, megvitatása a szakemberek és a negatívumokat mindkét rögzítési beállítások bemutatják, hogy miként adatok küldése a Elasticsearch szolgáltatás háló szolgáltatás konfigurálása.


## <a name="set-up-elasticsearch-on-azure"></a>A Azure Elasticsearch beállítása
A legegyszerűbb módszer az Azure Elasticsearch szolgáltatás beállítása [**erőforrás-kezelő Azure sablonok**](../azure-resource-manager/resource-group-overview.md)között van. [Quickstart útmutató Azure erőforrás-kezelő sablon Elasticsearch](https://github.com/Azure/azure-quickstart-templates/tree/master/elasticsearch) átfogó Azure quickstart útmutató sablonok tárházba érhető el. Ezzel a sablonnal külön tároló fiókok skála egységek (csomópontok csoportok) használja. Külön ügyfél és a kiszolgáló csomópontok különböző konfigurációjának és a csatlakoztatott adatok lemezt különböző számú is kiépítése.

Ebben az esetben azt sablonnal egy másik, a [diagnosztikai eszközökkel Azure tárházba](https://github.com/Azure/azure-diagnostics-tools) **VÉGŰ-MultiNode** hívását. Ezzel a sablonnal könnyebben használható, és létrehoz egy Elasticsearch fürthöz HTTP alapszintű hitelesítés védi. Folytatás előtt töltse le a tárházba GitHub a számítógépen (által vagy a tár klónozhatja, vagy zip-fájl letöltése). A VÉGŰ-MultiNode sablon azonos nevű mappa található.

### <a name="prepare-a-machine-to-run-elasticsearch-installation-scripts"></a>A gép Elasticsearch telepítési parancsfájlok futtatásának előkészítése
Az ES-MultiNode sablonnal legegyszerűbben keresztül egy megadott Azure PowerShell-parancsprogramot nevű `CreateElasticSearchCluster`. A parancsfájl használatához kell PowerShell-modulok és **openssl**nevű eszköz telepítése. Az utóbbi van szükség egy SSH kulcsot a Elasticsearch fürt távolról felügyeletéhez használható hozhat létre.

`CreateElasticSearchCluster`Egyszerű használat Windows számítógépről VÉGŰ-MultiNode sablonnal parancsfájl készült. Lehetséges a sablon használatához-Windows gépen, de az eset a jelen cikk túlra.

1. Ha még nem telepítette őket már, telepítse az [**Azure PowerShell modulokat**](http://aka.ms/webpi-azps). Amikor a rendszer kéri, kattintson a **futtatása**, majd a **telepítés**gombra. Azure PowerShell 1.3-as vagy újabb szükség.

2. A **openssl** eszköz [**Windows mely számjegy**](http://www.git-scm.com/downloads)eloszlását is tartalmazni fogja. Ha még nem tette meg, [Mely számjegy a Windows](http://www.git-scm.com/downloads) most kell telepíteni. (Az alapértelmezett telepítési beállítások is az OK gombra.)

3. Feltételezve, hogy mely számjegy telepítve, de nem szerepel a rendszer elérési útját, nyissa meg a Microsoft Azure PowerShell ablakot, és futtassa az alábbi parancsokat:

    ```powershell
    $ENV:PATH += ";<Git installation folder>\usr\bin"
    $ENV:OPENSSL_CONF = "<Git installation folder>\usr\ssl\openssl.cnf"
    ```

    Cserélje le a `<Git installation folder>` mely számjegy helyét a számítógépen; az alapértelmezett érték az **"C:\Program Files\Git"**. Megjegyzés: az első elérési út az elején a pontosvessző karaktert.

4. Győződjön meg arról, hogy be van jelentkezve az Azure (keresztül [`Add-AzureRmAccount`](https://msdn.microsoft.com/library/mt619267.aspx) parancsmag), és bejelölte az előfizetést, amellyel a rugalmas keresési fürt létrehozása. Ellenőrizheti, hogy helyes-e előfizetés legyen kijelölve, használatával `Get-AzureRmContext` és `Get-AzureRmSubscription` parancsmagok.

5. Ha még nem tette meg, módosítsa az aktuális könyvtár a VÉGŰ-MultiNode mappába.

### <a name="run-the-createelasticsearchcluster-script"></a>Futtassa a CreateElasticSearchCluster
Mielőtt futtatja a parancsfájlt, nyissa meg a `azuredeploy-parameters.json` fájl, és ellenőrizze, vagy adjon meg értéket a parancsfájl-paraméterek. A következő paraméterek találhatók:

|Paraméterek neve           |Leírás|
|-----------------------  |--------------------------|
|dnsNameForLoadBalancerIP |Az a nyilvánosan látható tartománynév rugalmas keresése a létrehozásához használt név fürt (által az Azure régió tartomány hozzáfűzése a megadott nevet). Például ha a paraméter értéke "myBigCluster" és a választott Azure terület eléri a nyugati Amerikai Egyesült Államok, az eredményül kapott DNS nevét a fürt myBigCluster.westus.cloudapp.azure.com is. <br /><br />Ez a név is figyelmezteti arra rugalmas keresési fürthöz, például adatok csomópont nevek társított sok eltérések legfelső szintű nevét.|
|adminUsername           |A keresés rugalmas fürt kezelése a rendszergazdai fiók nevét (a megfelelő SSH kulcsok automatikusan jönnek létre).|
|dataNodeCount           |A keresés rugalmas fürt csomópontok számának. Az aktuális verziójának a parancsfájl adatok és a lekérdezés csomópontok; között nem tesz különbséget az összes csomópontok mindkét szerepet. Az alapértelmezett érték 3 csomópontot.|
|dataDiskSize            |A méret adatok hozza létre (GB), amely az egyes adatok csomópont kiosztva. Az egyes csomópontok 4 adatok lemezt, rugalmas keresési szolgáltatás kizárólagos hozzárendelve kap.|
|régió                  |Azure terület, ahol a rugalmas keresési fürt el szeretné helyezni a neve.|
|esUserName              |A felhasználó nevét, annak a felhasználónak van konfigurálva, hogy az access-ES fürthöz (fizetnie HTTP alapszintű hitelesítés). A jelszó nem része a paraméterek fájlt, és meg kell adni, amikor `CreateElasticSearchCluster` parancsfájl hivatkoznak.|
|vmSizeDataNodes         |Rugalmas keresési fürt csomópontok Azure virtuális gép méretét. Az alapértelmezett Standard_D2.|

Most már készen áll a parancsfájl futtatásához. A hiba az alábbi parancsot:

```powershell
CreateElasticSearchCluster -ResourceGroupName <es-group-name> -Region <azure-region> -EsPassword <es-password>
```

Ha 

|Parancsfájl paraméterek neve    |Leírás|
|-----------------------  |--------------------------|
|`<es-group-name>`        |Az Azure erőforráscsoport, amely tartalmazni fogja az összes rugalmas keresési fürt erőforrás neve.|
|`<azure-region>`         |Az Azure terület, ahol hozható létre a rugalmas keresési fürt neve.|         
|`<es-password>`          |A keresés rugalmas felhasználó jelszavát.|

>[AZURE.NOTE] A próba-AzureResourceGroup parancsmag NullReferenceException el, ha elfelejtette a jelentkezzen be az Azure (`Add-AzureRmAccount`).

Ha nem sikerül a parancsfájlok futtatásának és határozhatja meg, hogy a hiba okát sablon paraméterértéket adjak okozta, javítsa ki a paraméter fájlt, és futtassa újra a parancsfájl egy másik erőforrás nevű csoportot. Is újrafelhasználása az azonos erőforrás-csoport nevét és a parancsfájl karbantartása a régit hozzáadásával van a `-RemoveExistingResourceGroup` parancsfájl meghívását paramétert.

### <a name="result-of-running-the-createelasticsearchcluster-script"></a>A CreateElasticSearchCluster parancsfájlok futtatásának eredménye
Futtatása után a `CreateElasticSearchCluster` parancsfájl, az alábbi főbb eltérések jön létre. Ebben a példában feltételezzük, hogy a program "myBigCluster" használt a `dnsNameForLoadBalancerIP` paraméter, és hogy a régió, amelyen létrehozta a fürt nyugati US.

|Eltérés|Név, helyét és megjegyzések|
|----------------------------------|----------------------------------|
|A Távfelügyelet SSH kulcs |myBigCluster.key fájlt (a címtárban a CreateElasticSearchCluster futtassa). <br /><br />A fő fájl adatok csomópontok a fürt való csatlakozáshoz a felügyeleti csomópontot, és (keresztül a rendszergazda csomópont) használható.|
|Rendszergazdai csomópontot.                        |myBigCluster-admin.westus.cloudapp.azure.com <br /><br />Egy dedikált virtuális Elasticsearch fürt Távfelügyelet – az egyetlen, amely lehetővé teszi a külső SSH kapcsolatokat. A Elasticsearch fürt csomópontok azonos virtuális hálózaton futása, de az esetleges Elasticsearch szolgáltatások nem futtathatók.|
|Adatok csomópontok                        |myBigCluster1... myBigCluster*N* <br /><br />Adatok csomópontok Elasticsearch és Kibana services szolgáltatást futtató. Minden egyes csomópontra SSH keresztül, de csak a rendszergazda csomópont keresztül csatlakozhat.|
|Elasticsearch fürthöz             |http://myBigCluster.westus.cloudapp.Azure.com/es/ <br /><br />Az elsődleges végpontot, a Elasticsearch fürt (Megjegyzés: a /es utótag). Egyszerű HTTP-hitelesítés (a hitelesítő adatokat, amelyeket a megadott esUserName/esPassword paraméterek VÉGŰ-MultiNode sablon) védi. A fürt rendelkezik is a lapok fejlécéhez beépülő modul telepítve van (http://myBigCluster.westus.cloudapp.azure.com/es/_plugin/head) egyszerű fürt felügyelet.|
|Kibana szolgáltatás                    |http://myBigCluster.westus.cloudapp.Azure.com <br /><br />A Kibana szolgáltatás eltávolítása a létrehozott Elasticsearch adatok megjelenítése van beállítva. A fürthöz hitelesítő hitelesítési adatait védi.|

## <a name="in-process-versus-out-of-process-trace-capturing"></a>Az eljárás és a folyamaton-nyomkövetés rögzítése
A bevezető azt említett diagnosztikai adatok összegyűjtése két alapvető módja: a folyamatban és a folyamaton. Minden legyen erősségek, gyengeségek és.

A **folyamaton nyomkövetési rögzítéséhez** előnyei többek között:

1. *Egyszerű konfigurációs és telepítéséhez*

    * Diagnosztikai adatok gyűjtése beállítás csak az alkalmazás-konfiguráció része. Sokkal egyszerűbb mindig szinkronizálására, "", az alkalmazás a többi.

    * Alkalmazás vagy a szolgáltatás konfigurációja könnyen elérhető.

    * A folyamaton nyomkövetés rögzítéséhez általában külön telepítési és a szükséges konfigurációs a diagnosztikai Agent, amely további felügyeleti feladatot, és a lehetséges hibák forrást. Az adott ügynök technológia gyakran lehetővé teszi, hogy a csak egy példánya az ügynök egy virtuális gép (csomópont). Ez azt jelenti, hogy a konfigurációs a diagnosztikai konfiguráció a webhelycsoport összes alkalmazás között meg van osztva, és a csomóponton futó szolgáltatások.

2. *Rugalmas*

    * Az alkalmazás küldhet az adatokat, ott, ahová tabulátort szeretne lépni szüksége van, az mindaddig, amíg egy ügyfél tárban van, amely támogatja a célként megadott adatrendszerrel. Új mosdók felvehetők a kívánt módon.

    * Összetett rögzítési, szűrése és adat-összesítési szabályok végrehajthatók.

    * Egy folyamaton-nyomkövetés rögzítéséhez általi az adatokat, amely támogatja az ügynök gyakran korlátozott. Néhány ügynökök extensible olyan.

3. *Access-alkalmazás belső adatokkal és a helyi*

    * A diagnosztikai alrendszer belül az alkalmazás vagy szolgáltatás folyamat futtatása is egyszerűen kiegészítheti a halad környezetfüggő adatokkal.

    * A folyamaton, megközelítésben az adatok ügynökszoftvert néhány folyamatok közötti kommunikációt, például eseményvezérelt Tracing Windows keresztül kell küldeni. Ez az eljárás is írhat elő további korlátozások vonatkoznak.

A **folyamaton - nyomkövetés rögzítéséhez** előnyei többek között:

1. *Figyelje az alkalmazást, és az összeomlást összegyűjtése listázása*

    * A folyamat nyomkövetési rögzítéséhez sikertelen lehet, ha az alkalmazás nem indul el, vagy összeomlik. Független ügynökszoftvert sokkal jobban lehetőséget, hogy a kulcsfontosságú hibaelhárítási információkat tartalmaz.<br /><br />

2. *Lejárat robosztussági és igazolt teljesítmény*

    * A platform szállító (például a Microsoft Azure diagnosztika ügynök) által fejlesztett ügynökszoftvert már szigorú tesztelése és csata-megerősítéséhez.

    * A rögzítés folyamaton nyomkövetési kell ügyelni annak érdekében, hogy a tevékenység diagnosztikai adatok elküldésének frissítésétől-alkalmazás nem zavarja az alkalmazás fő feladatok vagy időzítési és teljesítménybeli problémáinak bevezetésére. Független futó ügynökszoftvert kevésbé érzékenyek problémákra, és szeretné korlátozni a rendszer gyakorolt kifejezetten.

Egyesítésére és mindkét megközelítésnek összekapcsolhatók lehetőség. Sor kerülhet célszerűbb lehet számos alkalmazáshoz a legjobb megoldás.

Ebben az esetben azt használja, a **Microsoft.Diagnostic.Listeners tár** és a rögzítés adatokat küldeni egy Elasticsearch fürthöz szolgáltatás háló alkalmazásból folyamaton követés.

## <a name="use-the-listeners-library-to-send-diagnostic-data-to-elasticsearch"></a>Diagnosztikai adatok küldése a Elasticsearch a hallgatók tár segítségével
A Microsoft.Diagnostic.Listeners tár PartyCluster minta szolgáltatás háló alkalmazás része. Használatához:

1. Töltse le [a PartyCluster minta](https://github.com/Azure-Samples/service-fabric-dotnet-management-party-cluster) GitHub.

2. Másolja a vágólapra a Microsoft.Diagnostics.Listeners és Microsoft.Diagnostics.Listeners.Fabric projektek (teljes mappákat) a PartyCluster minta címtárból az alkalmazást, az adatok küldése a Elasticsearch értendő megoldás mappájába.

3. Nyissa meg a cél megoldást, kattintson a jobb gombbal a megoldást csomópontot a megoldást Intéző elemre, és válassza a **Meglévő projekt hozzáadása**lehetőséget. A Microsoft.Diagnostics.Listeners projekt hozzáadása a megoldáshoz. Ismételje meg ugyanezt a Microsoft.Diagnostics.Listeners.Fabric projekt.

4. A szolgáltatás projekt(ek) a két hozzáadott projektek project hivatkozás hozzáadása (Egyes szolgáltatásokhoz adatok küldése a Elasticsearch fájlját kell hivatkozás Microsoft.Diagnostics.EventListeners és Microsoft.Diagnostics.EventListeners.Fabric).

    ![Projekt hivatkozások Microsoft.Diagnostics.EventListeners és Microsoft.Diagnostics.EventListeners.Fabric tárakhoz][1]

### <a name="service-fabric-general-availability-release-and-microsoftdiagnosticstracing-nuget-package"></a>Elérhetőség szolgáltatás háló általános megjelenés és Microsoft.Diagnostics.Tracing Nuget csomag
Elérhetőség szolgáltatás háló általános megjelenés (2.0.135, megjelenik az 2016 március 31) épülő alkalmazások Megcélozhat **.NET-keretrendszer 4.5.2.**. Ez a verzió áll a legmagasabb a .NET-keretrendszer, ám megjelenési idején Azure által támogatott. Keretében e verziójában sajnos nem rendelkezik az bizonyos EventListener API-hoz szükséges a Microsoft.Diagnostics.Listeners tárban. (A naplózás API-hoz készült alkalmazások alapját összetevő) EventSource és EventListener szorosan ahhoz, mert a minden projekt, a Microsoft.Diagnostics.Listeners tár használó EventSource alternatív megvalósítása kell használnia. Ez a megvalósítás a **Microsoft.Diagnostics.Tracing Nuget csomagot**a Microsoft által létrehozott által biztosított. A csomag teljesen visszamenőlegesen is kompatibilisek EventSource szereplő keretében, így kód módosítások nem szükséges hivatkozott névtér módosítások nem lesz.

A EventSource osztály Microsoft.Diagnostics.Tracing végrehajtásának használatának megkezdéséhez kövesse az alábbi lépéseket minden szolgáltatás projekt adatok küldése a Elasticsearch kell, hogy:

1. Kattintson a jobb gombbal a szolgáltatás projekten, és válassza a **Nuget csomagok kezelése**.

2. Váltás a nuget.org csomag forrás (Ha még nincs bejelölve), és keresse meg a "**Microsoft.Diagnostics.Tracing**".

3. Telepítse a `Microsoft.Diagnostics.Tracing.EventSource` csomag (és függőségét).

4. Nyissa meg a szolgáltatási projekt a **ServiceEventSource.cs** vagy **ActorEventSource.cs** fájlt, és cserélje le a `using System.Diagnostics.Tracing` irányelv fölött a fájlt a `using Microsoft.Diagnostics.Tracing` irányelv.

Ezeket a lépéseket nem lehet szükséges, miután a **.NET-keretrendszer 4.6** támogatja a Microsoft Azure.

### <a name="elasticsearch-listener-instantiation-and-configuration"></a>Elasticsearch figyelő példányosítási és konfiguráció
Diagnosztikai adatok küldéséhez Elasticsearch utolsó lépésként a hozzon létre egy példányának `ElasticSearchListener` , és konfigurálja úgy Elasticsearch kapcsolat adatokkal. A figyelő automatikusan rögzíti a szolgáltatási projekt definiált EventSource osztályok keresztül hatványát összes eseményének. Meg kell lennie a szolgáltatást, élettartama során életben, tehát a legjobb helyen szeretné létrehozni a dokumentumot a szolgáltatáskód inicializálni. Hogyan lehet következőhöz az állapot nélküli szolgáltatás inicializálni kódját után a szükséges módosításokat (hozzáadását kezdődő megjegyzések ki emelni, `****`):

```csharp
using System;
using System.Diagnostics;
using System.Fabric;
using System.Threading;
using System.Threading.Tasks;
using Microsoft.ServiceFabric.Services.Runtime;

// **** Add the following directives
using Microsoft.Diagnostics.EventListeners;
using Microsoft.Diagnostics.EventListeners.Fabric;

namespace Stateless1
{
    internal static class Program
    {
        /// <summary>
        /// This is the entry point of the service host process.
        /// </summary>        
        private static void Main()
        {
            try
            {
                // **** Instantiate ElasticSearchListener
                var configProvider = new FabricConfigurationProvider("ElasticSearchEventListener");
                ElasticSearchListener esListener = null;
                if (configProvider.HasConfiguration)
                {
                    esListener = new ElasticSearchListener(configProvider);
                }

                // The ServiceManifest.XML file defines one or more service type names.
                // Registering a service maps a service type name to a .NET type.
                // When Service Fabric creates an instance of this service type,
                // an instance of the class is created in this host process.

                ServiceRuntime.RegisterServiceAsync("Stateless1Type", 
                    context => new Stateless1(context)).GetAwaiter().GetResult();

                ServiceEventSource.Current.ServiceTypeRegistered(Process.GetCurrentProcess().Id, typeof(Stateless1).Name);

                // Prevents this host process from terminating so services keep running.
                Thread.Sleep(Timeout.Infinite);

                // **** Ensure that the ElasticSearchListner instance is not garbage-collected prematurely
                GC.KeepAlive(esListener);
            }
            catch (Exception e)
            {
                ServiceEventSource.Current.ServiceHostInitializationFailed(e);
                throw;
            }
        }
    }
}
```

A szolgáltatás konfigurációs fájl (**PackageRoot\Config\Settings.xml**) külön szakaszt Elasticsearch kapcsolat adatokat kell elhelyezni. A szakasz neve meg kell felelnie a átadott érték a `FabricConfigurationProvider` konstruktort, például:

```xml
<Section Name="ElasticSearchEventListener">
  <Parameter Name="serviceUri" Value="http://myBigCluster.westus.cloudapp.azure.com/es/" />
  <Parameter Name="userName" Value="(ES user name)" />
  <Parameter Name="password" Value="(ES user password)" />
  <Parameter Name="indexNamePrefix" Value="myapp" />
</Section>
```
Az értékeket a `serviceUri`, `userName` és `password` paraméterek felelnek meg a Elasticsearch fürt végpontjának címe Elasticsearch felhasználónevét és jelszavát, illetve. `indexNamePrefix`az előtag az indexek Elasticsearch; van a Microsoft.Diagnostics.Listeners tár naponta az adatok a új index létrehozása.

### <a name="verification"></a>Ellenőrzése
Az egész! Most a szolgáltatás fut, valahányszor elindítja a Elasticsearch szolgáltatás a konfigurációban megadott nyomkövetési naplók küldése. Ellenőrizheti, hogy ez úgy, hogy megnyitja a felhasználói felület Kibana cél Elasticsearch példányhoz társított. Ebben a példában a lap címét http://myBigCluster.westus.cloudapp.azure.com/. Ellenőrizze, hogy a választott név előtaggal indexek a `ElasticSearchListener` példány valóban létrehozott és töltve adatokkal.

![Kibana megjelenítő PartyCluster alkalmazás események][2]

## <a name="next-steps"></a>Következő lépések
- [További információ diagnosztizálása és a szolgáltatás háló szolgáltatás figyelése](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

<!--Image references-->
[1]: ./media/service-fabric-diagnostics-how-to-use-elasticsearch/listener-lib-references.png
[2]: ./media/service-fabric-diagnostics-how-to-use-elasticsearch/kibana.png
