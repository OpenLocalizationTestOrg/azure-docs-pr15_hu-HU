<properties
   pageTitle="Azure Service háló személyek megtekintése összesíti állapot |} Microsoft Azure"
   description="A lekérdezés, megtekintése és Azure Service háló szervezetek összesített állapot keresztül állapot lekérdezések és az általános lekérdezések felmérése ismerteti."
   services="service-fabric"
   documentationCenter=".net"
   authors="oanapl"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/28/2016"
   ms.author="oanapl"/>

# <a name="view-service-fabric-health-reports"></a>Szolgáltatás háló állapotjelentések megtekintése
Azure Service háló [állapot modell](service-fabric-health-introduction.md) , amely magában foglalja a rendszerállapot szervezetek milyen rendszer összetevők és watchdogs jelentést küldhet be, hogy azok figyelése helyi feltételek vezet be. [Állapot tárolása](service-fabric-health-introduction.md#health-store) összesíti összes egészségügyi adatok megfelelő személyek megállapításához.

Beépített a fürt kitölti a rendszer összetevők által küldött állapotjelentések. A [rendszer állapota a jelentésekkel kapcsolatos hibák elhárítása](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)című cikkben olvashat.

Szolgáltatás háló több lehetőség közül választhat a személyek összesített állapotának nyújtja:

- [Szolgáltatás háló Explorer](service-fabric-visualizing-your-cluster.md) vagy más képi megjelenítés eszközök

- Állapot lekérdezések (PowerShell, az API-val, vagy keresztül többi)

- Általános lekérdezések (PowerShell, az API-val, vagy keresztül többi) tulajdonságainak egyikeként állapot rendelkező szervezetek, amely visszatérési listája

Ezeket a beállításokat szemléltetésére használata egy helyi fürt öt csomópontokkal. Az a **háló: / rendszer** (amely létezik beépített) alkalmazást, néhány más alkalmazás van telepítve. Ezeket az alkalmazásokat egyik **háló: / WordCount**. Ez az alkalmazás állapot-nyilvántartó szolgáltatás hét kópiákkal konfigurált tartalmazza. Mivel vannak csak öt csomópontot, a rendszer összetevők megjelenítése figyelmezteti, hogy a partíciót nem éri el a cél száma

```xml
<Service Name="WordCountService">
    <StatefulService ServiceTypeName="WordCountServiceType" TargetReplicaSetSize="7" MinReplicaSetSize="2">
      <UniformInt64Partition PartitionCount="1" LowKey="1" HighKey="26" />
    </StatefulService>
</Service>
```

## <a name="health-in-service-fabric-explorer"></a>A szolgáltatás háló Intézőben állapota
Szolgáltatás háló Explorer biztosít a fürt vizuális megjelenítése. Az alábbi képen az látható, hogy:

- Az alkalmazás **háló: / WordCount** piros (a hiba), mert az **Elérhetőség**tulajdonság **MyWatchdog** által jelzett hibát esemény.

- Saját szolgáltatások közül **háló: / WordCount/WordCountService** sárga (a figyelmeztetés jelenik meg). A szolgáltatás hét kópiákkal van beállítva, mivel azok nem az összes elhelyezni, ahány csak öt csomópont. Bár ez nem látható az alábbi, a szolgáltatás partíciót sárga miatt nem a jelentésben. A sárga partíciót elindítja a sárga szolgáltatást.

- A piros alkalmazás miatt a fürt piros.

A kiértékelés használja az alapértelmezett házirendek a fürt jegyzék és az alkalmazás jegyzék. Szigorú házirendek, és nem elviselni bármilyen hiba.

A szolgáltatás háló Intézővel fürt nézet:

![A szolgáltatás háló Intézővel fürt nézete.][1]

[1]: ./media/service-fabric-view-entities-aggregated-health/servicefabric-explorer-cluster-health.png


> [AZURE.NOTE] További információ a [Szolgáltatás háló Explorer](service-fabric-visualizing-your-cluster.md).

## <a name="health-queries"></a>Állapot-lekérdezések
Szolgáltatás háló állapot lekérdezések közzététele az a [személy típusú](service-fabric-health-introduction.md#health-entities-and-hierarchy)támogatott. Azok az API-val (módszerek **FabricClient.HealthManager**megtalálható), a PowerShell-parancsmagok és a többi keresztül is elérhető. Ezek a lekérdezések visszatérési entitás teljes rendszerállapot adatait: a összesített rendszerállapot, a személy állapota események, a gyermek állapot államok (ha alkalmazható) és a sérült értékelést, ha a szervezet nem megfelelő.

> [AZURE.NOTE] Egy állapot entitás ad vissza, amikor azt teljes mértékben a az állapot áruházban töltve. A szervezet legyen aktív (nem törli), és a rendszer miként. A hierarchia lánc annak szülő személyek is rendelkeznie kell a rendszer jelentéseket. Ha e feltételek valamelyike nem teljesül, a az állapot lekérdezések kivételhibát, amely mutatja, hogy miért nem jelenik meg a szervezet adja eredményül.

A rendszerállapot lekérdezések az a személy azonosítóval attól függ, hogy a szervezet adja meg kell felelnie. A lekérdezések fogadja el a nem kötelező állapot házirend-paraméterek. Ha nincs állapot házirendek adja meg, az [állapot házirendek](service-fabric-health-introduction.md#health-policies) a fürt vagy alkalmazás jegyzék a kiértékelés segítségével. A lekérdezések is fogadja el a szűrők csak részleges gyermekek vagy események – a projektfeladatokat a megadott szűrőkkel tiszteletben visszaadása.

> [AZURE.NOTE] A kimeneti szűrőket a kiszolgálóoldalon érvényesek, az üzenet válasz méretét. Ajánlott, hogy Ön visszaadott adatok korlátozásához használni a kimeneti szűrőket helyett ügyféloldali szűrők alkalmazásával.

Egy adott személy állapota tartalmazza:

- A szervezet összesített állapotának. Az állapot tároló entitás állapotjelentések, a gyermek állapot államok (ha alkalmazható) és az állapot házirendek alapján számítja ki. További információ a [személy állapota kiértékelés](service-fabric-health-introduction.md#entity-health-evaluation).  

- Az a személy állapota eseményeket.

- A webhelycsoport állapot államok, a személyek, a gyermekek beállíthatja, hogy az összes gyermekek. Az állapot államok entitás azonosítóját és az összesített rendszerállapot tartalmazzák. Szeretne letölteni a gyermek készültségi állapotát, hívja fel a lekérdezés állapota a gyermek személy mezőtípushoz fázisban és a gyermek azonosítója.

- Mutasson a jelentést, amelyen a egységek, állapotát, ha a szervezet nem megfelelő sérült értékelést.

## <a name="get-cluster-health"></a>Első fürt állapota
A fürt entitás állapotának adja eredményül, és az állapot államok az alkalmazások és a csomópontok (a fürt gyermekekkel) tartalmazza. Beviteli:

- [Választható] A csomópontok és a fürt események ki szeretné számítani használt fürt állapot házirend.

- [Választható] Az alkalmazás állapot házirend térkép, az állapot házirendek az alkalmazás nyilvánvalóan házirendek felülbírálják használt.

- [Választható] Az események, csomópontok és alkalmazásokat, adja meg, mely bejegyzéseket érdeklődésére számot tartó, és az eredmény (például csak, a hibák vagy figyelmeztetések és hibák) a visszaadandó szűrőket. Események, csomópontok és alkalmazások felmérése összesíti entitás állapotát jelző, függetlenül a szűrő szolgálnak.

### <a name="api"></a>API
Úgy juthat az fürt állapot, hozzon létre egy `FabricClient` , és hívja fel a [GetClusterHealthAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient.getclusterhealthasync.aspx) módszer a saját **HealthManager**.

A következő hívás fürt állapotát jelző kapja:

```csharp
ClusterHealth clusterHealth = await fabricClient.HealthManager.GetClusterHealthAsync();
```

A következő kódot fürt állapotát jelző kap egy egyéni fürt állapot házirendet, és a szűrők használata csomópontok és alkalmazásokat. [ClusterHealthQueryDescription](https://msdn.microsoft.com/library/azure/system.fabric.description.clusterhealthquerydescription.aspx), a bemeneti adatokat tartalmazó hoz létre.

```csharp
var policy = new ClusterHealthPolicy()
{
    MaxPercentUnhealthyNodes = 20
};
var nodesFilter = new NodeHealthStatesFilter()
{
    HealthStateFilterValue = HealthStateFilter.Error | HealthStateFilter.Warning
};
var applicationsFilter = new ApplicationHealthStatesFilter()
{
    HealthStateFilterValue = HealthStateFilter.Error
};
var queryDescription = new ClusterHealthQueryDescription()
{
    HealthPolicy = policy,
    ApplicationsFilter = applicationsFilter,
    NodesFilter = nodesFilter,
};

ClusterHealth clusterHealth = await fabricClient.HealthManager.GetClusterHealthAsync(queryDescription);
```

### <a name="powershell"></a>A PowerShell
Az első fürt állapotát jelző parancsmag [Get-ServiceFabricClusterHealth](https://msdn.microsoft.com/library/mt125850.aspx). Első lépésként csatlakozzon a fürthöz a [Csatlakozás-ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx) parancsmag használatával.

A fürt állapota öt csomópontot, a rendszer az alkalmazást és háló: / WordCount konfigurált leírt módon.

A következő parancsmagot fürt állapot kapja meg az alapértelmezett állapot házirendek használatával. A összesített rendszerállapot riasztás, van, mert a háló: / alkalmazása WordCount figyelmeztetés. Fontos tudni, hogyan a sérült értékelést adja meg, amelyen a összesített vonatkozó feltételek részleteket.

```xml
PS C:\> Get-ServiceFabricClusterHealth

AggregatedHealthState   : Warning
UnhealthyEvaluations    :
                          Unhealthy applications: 100% (1/1), MaxPercentUnhealthyApplications=0%.

                          Unhealthy application: ApplicationName='fabric:/WordCount', AggregatedHealthState='Warning'.

                              Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.

                              Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Warning'.

                                  Unhealthy event: SourceId='System.PLB',
                          Property='ServiceReplicaUnplacedHealth_Secondary_a1f83a35-d6bf-4d39-b90d-28d15f39599b', HealthState='Warning',
                          ConsiderWarningAsError=false.


NodeHealthStates        :
                          NodeName              : _Node_2
                          AggregatedHealthState : Ok

                          NodeName              : _Node_0
                          AggregatedHealthState : Ok

                          NodeName              : _Node_1
                          AggregatedHealthState : Ok

                          NodeName              : _Node_3
                          AggregatedHealthState : Ok

                          NodeName              : _Node_4
                          AggregatedHealthState : Ok

ApplicationHealthStates :
                          ApplicationName       : fabric:/System
                          AggregatedHealthState : Ok

                          ApplicationName       : fabric:/WordCount
                          AggregatedHealthState : Warning

HealthEvents            : None
```

A PowerShell-parancsmagot a fürt állapotának kap egy egyéni használati szabályzat használatával. A szeletelő szűri csak hibát vagy figyelmeztetést alkalmazásokat és csomópontok eredmények. Emiatt nem csomópontok ad vissza, mint az összes megfelelő. Csak a háló: / WordCount alkalmazás megőrzi az alkalmazás szűrő. Mivel az egyéni házirend azt határozza meg, a háló hibák, figyelmeztetések szempontok: / WordCount alkalmazást, az alkalmazás kiértékelése hibát ahogy, és így a fürt.

```powershell
PS c:\> $appHealthPolicy = New-Object -TypeName System.Fabric.Health.ApplicationHealthPolicy
$appHealthPolicy.ConsiderWarningAsError = $true
$appHealthPolicyMap = New-Object -TypeName System.Fabric.Health.ApplicationHealthPolicyMap
$appUri1 = New-Object -TypeName System.Uri -ArgumentList "fabric:/WordCount"
$appHealthPolicyMap.Add($appUri1, $appHealthPolicy)
Get-ServiceFabricClusterHealth -ApplicationHealthPolicyMap $appHealthPolicyMap -ApplicationsFilter "Warning,Error" -NodesFilter "Warning,Error"


AggregatedHealthState   : Error
UnhealthyEvaluations    :
                          Unhealthy applications: 100% (1/1), MaxPercentUnhealthyApplications=0%.

                          Unhealthy application: ApplicationName='fabric:/WordCount', AggregatedHealthState='Error'.

                              Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.

                              Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Error'.

                                  Unhealthy event: SourceId='System.PLB',
                          Property='ServiceReplicaUnplacedHealth_Secondary_a1f83a35-d6bf-4d39-b90d-28d15f39599b', HealthState='Warning',
                          ConsiderWarningAsError=true.


NodeHealthStates        : None
ApplicationHealthStates :
                          ApplicationName       : fabric:/WordCount
                          AggregatedHealthState : Error

HealthEvents            : None

```

### <a name="rest"></a>TÖBBI
[GET kérés](https://msdn.microsoft.com/library/azure/dn707669.aspx) vagy törzsébe leírt állapot házirendek tartalmazó [POST kérelem](https://msdn.microsoft.com/library/azure/dn707696.aspx) fürt állapotát úgy is megnyithatja.

## <a name="get-node-health"></a>Ismerkedés a csomópont állapota
A csomópont entitás állapotának adja eredményül, és az állapot események a csomóponton jelentett tartalmazza. Beviteli:

- [Szükséges] A csomópont nevét, amely azonosítja a csomópontot.

- [Választható] A fürt állapot házirend-beállítások állapot ki szeretné használni.

- [Választható] Adja meg, mely bejegyzéseket érdeklődésére számot tartó, és az eredmény (például csak, a hibák vagy figyelmeztetések és hibák) a visszaadandó események szűrőket. Összes eseményének összesíti entitás állapotának, függetlenül a szűrő kiértékeléséhez használt.

### <a name="api"></a>API
Csomópont állapot juthat az API-k, hozzon létre egy `FabricClient` , és hívja fel a [GetNodeHealthAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient.getnodehealthasync.aspx) módszer a saját HealthManager.

A következő kódot a csomópont állapota a megadott csomópont nevét kapja:

```csharp
NodeHealth nodeHealth = await fabricClient.HealthManager.GetNodeHealthAsync(nodeName);
```

A következő kódot a csomópont állapot kapja a megadott csomópont jelölőnégyzetét, és az események szűrő és az egyéni házirend halad [NodeHealthQueryDescription](https://msdn.microsoft.com/library/azure/system.fabric.description.nodehealthquerydescription.aspx):

```csharp
var queryDescription = new NodeHealthQueryDescription(nodeName)
{
    HealthPolicy = new ClusterHealthPolicy() {  ConsiderWarningAsError = true },
    EventsFilter = new HealthEventsFilter() { HealthStateFilterValue = HealthStateFilter.Warning },
};

NodeHealth nodeHealth = await fabricClient.HealthManager.GetNodeHealthAsync(queryDescription);
```

### <a name="powershell"></a>A PowerShell
A csomópont állapot megszerezni parancsmag [Get-ServiceFabricNodeHealth](https://msdn.microsoft.com/library/mt125937.aspx). Első lépésként csatlakozzon a fürthöz a [Csatlakozás-ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx) parancsmag használatával.
A következő parancsmagot a csomópont állapot használja az alapértelmezett házirendek kap:

```powershell
PS C:\> Get-ServiceFabricNodeHealth _Node_1


NodeName              : _Node_1
AggregatedHealthState : Ok
HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 6
                        SentAt                : 3/22/2016 7:47:56 PM
                        ReceivedAt            : 3/22/2016 7:48:19 PM
                        TTL                   : Infinite
                        Description           : Fabric node is up.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 3/22/2016 7:48:19 PM, LastWarning = 1/1/0001 12:00:00 AM
```

A következő parancsmagot a fürt csomópontjait állapotának előnyökben részesül:

```powershell
PS C:\> Get-ServiceFabricNode | Get-ServiceFabricNodeHealth | select NodeName, AggregatedHealthState | ft -AutoSize

NodeName AggregatedHealthState
-------- ---------------------
_Node_2                     Ok
_Node_0                     Ok
_Node_1                     Ok
_Node_3                     Ok
_Node_4                     Ok
```

### <a name="rest"></a>TÖBBI
[GET kérés](https://msdn.microsoft.com/library/azure/dn707650.aspx) vagy törzsébe leírt állapot házirendek tartalmazó [POST kérelem](https://msdn.microsoft.com/library/azure/dn707665.aspx) csomópont állapotát úgy is megnyithatja.

## <a name="get-application-health"></a>Alkalmazás állapot beszerzése
Az alkalmazás egyed állapotának lekérdezése A telepített alkalmazások és a gyermekek szolgáltatás állapota államban tartalmaz. Beviteli:

- [Szükséges] Az alkalmazás nevét (URI), amely azonosítja az adott alkalmazást.

- [Választható] Az alkalmazás nyilvánvalóan házirendek felülbírálják használt alkalmazás állapot házirend.

- [Választható] Az események, szolgáltatások és üzembe helyezett alkalmazások, adja meg, milyen tételek szűrők érdeklődésére számot tartó, és az eredmény (például csak, a hibák vagy figyelmeztetések és hibák) a visszaadandó. Események, szolgáltatások és üzembe helyezett alkalmazások felmérése összesíti entitás állapotát jelző, függetlenül a szűrő szolgálnak.

### <a name="api"></a>API
Úgy juthat az alkalmazás állapot, hozzon létre egy `FabricClient` , és hívja fel a [GetApplicationHealthAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient.getapplicationhealthasync.aspx) módszer a saját HealthManager.

A következő kódot az alkalmazás állapota az adott alkalmazás nevét (URI) kapja:

```csharp
ApplicationHealth applicationHealth = await fabricClient.HealthManager.GetApplicationHealthAsync(applicationName);
```

A következő kódot az alkalmazás állapota az adott alkalmazás nevét (URI), szűrők és [ApplicationHealthQueryDescription](https://msdn.microsoft.com/library/azure/system.fabric.description.applicationhealthquerydescription.aspx)keresztül megadott egyéni házirendek kap.

```csharp
HealthStateFilter warningAndErrors = HealthStateFilter.Error | HealthStateFilter.Warning;
var serviceTypePolicy = new ServiceTypeHealthPolicy()
{
    MaxPercentUnhealthyPartitionsPerService = 0,
    MaxPercentUnhealthyReplicasPerPartition = 5,
    MaxPercentUnhealthyServices = 0,
};
var policy = new ApplicationHealthPolicy()
{
    ConsiderWarningAsError = false,
    DefaultServiceTypeHealthPolicy = serviceTypePolicy,
    MaxPercentUnhealthyDeployedApplications = 0,
};

var queryDescription = new ApplicationHealthQueryDescription(applicationName)
{
    HealthPolicy = policy,
    EventsFilter = new HealthEventsFilter() { HealthStateFilterValue = warningAndErrors },
    ServicesFilter = new ServiceHealthStatesFilter() { HealthStateFilterValue = warningAndErrors },
    DeployedApplicationsFilter = new DeployedApplicationHealthStatesFilter() { HealthStateFilterValue = warningAndErrors },
};

ApplicationHealth applicationHealth = await fabricClient.HealthManager.GetApplicationHealthAsync(queryDescription);
```

### <a name="powershell"></a>A PowerShell
Az alkalmazás állapot megszerezni parancsmag [Get-ServiceFabricApplicationHealth](https://msdn.microsoft.com/library/mt125976.aspx). Első lépésként csatlakozzon a fürthöz a [Csatlakozás-ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx) parancsmag használatával.

A következő parancsmagot állapotának adja vissza az **háló: / WordCount** alkalmazás:

```powershell
PS c:\>
PS C:\WINDOWS\system32>  Get-ServiceFabricApplicationHealth fabric:/WordCount


ApplicationName                 : fabric:/WordCount
AggregatedHealthState           : Warning
UnhealthyEvaluations            :
                                  Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.

                                  Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Warning'.

                                      Unhealthy event: SourceId='System.PLB',
                                  Property='ServiceReplicaUnplacedHealth_Secondary_a1f83a35-d6bf-4d39-b90d-28d15f39599b', HealthState='Warning',
                                  ConsiderWarningAsError=false.

ServiceHealthStates             :
                                  ServiceName           : fabric:/WordCount/WordCountService
                                  AggregatedHealthState : Warning

                                  ServiceName           : fabric:/WordCount/WordCountWebService
                                  AggregatedHealthState : Ok

DeployedApplicationHealthStates :
                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_0
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_2
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_3
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_4
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_1
                                  AggregatedHealthState : Ok

HealthEvents                    :
                                  SourceId              : System.CM
                                  Property              : State
                                  HealthState           : Ok
                                  SequenceNumber        : 360
                                  SentAt                : 3/22/2016 7:56:53 PM
                                  ReceivedAt            : 3/22/2016 7:56:53 PM
                                  TTL                   : Infinite
                                  Description           : Application has been created.
                                  RemoveWhenExpired     : False
                                  IsExpired             : False
                                  Transitions           : Error->Ok = 3/22/2016 7:56:53 PM, LastWarning = 1/1/0001 12:00:00 AM

                                  SourceId              : MyWatchdog
                                  Property              : Availability
                                  HealthState           : Ok
                                  SequenceNumber        : 131031545225930951
                                  SentAt                : 3/22/2016 9:08:42 PM
                                  ReceivedAt            : 3/22/2016 9:08:42 PM
                                  TTL                   : Infinite
                                  Description           : Availability checked successfully, latency ok
                                  RemoveWhenExpired     : False
                                  IsExpired             : False
                                  Transitions           : Error->Ok = 3/22/2016 8:55:39 PM, LastWarning = 1/1/0001 12:00:00 AM
```

Az alábbi PowerShell-parancsmag adja meg az egyéni házirendek. Szűrők is gyermekek és eseményeket.

```powershell
PS C:\> Get-ServiceFabricApplicationHealth -ApplicationName fabric:/WordCount -ConsiderWarningAsError $true -ServicesFilter Error -EventsFilter Error -DeployedApplicationsFilter Error

ApplicationName                 : fabric:/WordCount
AggregatedHealthState           : Error
UnhealthyEvaluations            :
                                  Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.

                                  Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Error'.

                                      Unhealthy event: SourceId='System.PLB',
                                  Property='ServiceReplicaUnplacedHealth_Secondary_a1f83a35-d6bf-4d39-b90d-28d15f39599b', HealthState='Warning',
                                  ConsiderWarningAsError=true.

ServiceHealthStates             :
                                  ServiceName           : fabric:/WordCount/WordCountService
                                  AggregatedHealthState : Error

DeployedApplicationHealthStates : None
HealthEvents                    : None
```

### <a name="rest"></a>TÖBBI
[GET kérés](https://msdn.microsoft.com/library/azure/dn707681.aspx) vagy törzsébe leírt állapot házirendek tartalmazó [POST kérelem](https://msdn.microsoft.com/library/azure/dn707643.aspx) alkalmazás állapotát úgy is megnyithatja.

## <a name="get-service-health"></a>Ismerkedés a szolgáltatás állapota
A szolgáltatás entitás állapotának lekérdezése A partíciók állapot államok tartalmaz. Beviteli:

- [Szükséges] A szolgáltatás neve (URI), amely azonosítja a szolgáltatást.

- [Választható] Az alkalmazás állapot használt házirendet felülbírálása az alkalmazás nyilvánvalóan házirend.

- [Választható] Az események és partíciók, adja meg, mely bejegyzéseket érdeklődésére számot tartó, és az eredmény (például csak, a hibák vagy figyelmeztetések és hibák) a visszaadandó szűrőket. Események és a partíciók összesíti entitás állapotának, függetlenül a szűrő kiértékeléséhez használt.

### <a name="api"></a>API
Az API olvas be a szolgáltatás állapota, hozzon létre egy `FabricClient` , és hívja fel a [GetServiceHealthAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient.getservicehealthasync.aspx) módszer a saját HealthManager.

Az alábbi példa a megadott szolgáltatás nevével (URI) szolgáltatás állapota kapja:

```charp
ServiceHealth serviceHealth = await fabricClient.HealthManager.GetServiceHealthAsync(serviceName);
```

A következő kódot a Szolgáltatásállapot a megadott szolgáltatás neve (URI), szűrők és egyéni házirend [ServiceHealthQueryDescription](https://msdn.microsoft.com/library/azure/system.fabric.description.servicehealthquerydescription.aspx)keresztül kapja:

```csharp
var queryDescription = new ServiceHealthQueryDescription(serviceName)
{
    EventsFilter = new HealthEventsFilter() { HealthStateFilterValue = HealthStateFilter.All },
    PartitionsFilter = new PartitionHealthStatesFilter() { HealthStateFilterValue = HealthStateFilter.Error },
};

ServiceHealth serviceHealth = await fabricClient.HealthManager.GetServiceHealthAsync(queryDescription);
```

### <a name="powershell"></a>A PowerShell
A parancsmaggal a Szolgáltatásállapot megszerezni a [Get-ServiceFabricServiceHealth](https://msdn.microsoft.com/library/mt125984.aspx). Első lépésként csatlakozzon a fürthöz a [Csatlakozás-ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx) parancsmag használatával.

A következő parancsmagot a Szolgáltatásállapot kap az alapértelmezett állapot házirendek használatával:

```powershell
PS C:\> Get-ServiceFabricServiceHealth -ServiceName fabric:/WordCount/WordCountService


ServiceName           : fabric:/WordCount/WordCountService
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='System.PLB',
                        Property='ServiceReplicaUnplacedHealth_Secondary_a1f83a35-d6bf-4d39-b90d-28d15f39599b', HealthState='Warning',
                        ConsiderWarningAsError=false.

PartitionHealthStates :
                        PartitionId           : a1f83a35-d6bf-4d39-b90d-28d15f39599b
                        AggregatedHealthState : Warning

HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 10
                        SentAt                : 3/22/2016 7:56:53 PM
                        ReceivedAt            : 3/22/2016 7:57:18 PM
                        TTL                   : Infinite
                        Description           : Service has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 3/22/2016 7:57:18 PM, LastWarning = 1/1/0001 12:00:00 AM

                        SourceId              : System.PLB
                        Property              : ServiceReplicaUnplacedHealth_Secondary_a1f83a35-d6bf-4d39-b90d-28d15f39599b
                        HealthState           : Warning
                        SequenceNumber        : 131031547693687021
                        SentAt                : 3/22/2016 9:12:49 PM
                        ReceivedAt            : 3/22/2016 9:12:49 PM
                        TTL                   : 00:01:05
                        Description           : The Load Balancer was unable to find a placement for one or more of the Service's Replicas:
                        fabric:/WordCount/WordCountService Secondary Partition a1f83a35-d6bf-4d39-b90d-28d15f39599b could not be placed, possibly,
                        due to the following constraints and properties:  
                        Placement Constraint: N/A
                        Depended Service: N/A

                        Constraint Elimination Sequence:
                        ReplicaExclusionStatic eliminated 4 possible node(s) for placement -- 1/5 node(s) remain.
                        ReplicaExclusionDynamic eliminated 1 possible node(s) for placement -- 0/5 node(s) remain.

                        Nodes Eliminated By Constraints:

                        ReplicaExclusionStatic:
                        FaultDomain:fd:/0 NodeName:_Node_0 NodeType:NodeType0 UpgradeDomain:0 UpgradeDomain: ud:/0 Deactivation Intent/Status:
                        None/None
                        FaultDomain:fd:/1 NodeName:_Node_1 NodeType:NodeType1 UpgradeDomain:1 UpgradeDomain: ud:/1 Deactivation Intent/Status:
                        None/None
                        FaultDomain:fd:/3 NodeName:_Node_3 NodeType:NodeType3 UpgradeDomain:3 UpgradeDomain: ud:/3 Deactivation Intent/Status:
                        None/None
                        FaultDomain:fd:/4 NodeName:_Node_4 NodeType:NodeType4 UpgradeDomain:4 UpgradeDomain: ud:/4 Deactivation Intent/Status:
                        None/None

                        ReplicaExclusionDynamic:
                        FaultDomain:fd:/2 NodeName:_Node_2 NodeType:NodeType2 UpgradeDomain:2 UpgradeDomain: ud:/2 Deactivation Intent/Status:
                        None/None


                        RemoveWhenExpired     : True
                        IsExpired             : False
```

### <a name="rest"></a>TÖBBI
Szolgáltatás állapota [GET kérés](https://msdn.microsoft.com/library/azure/dn707609.aspx) , vagy egy [bejegyzés kérelem](https://msdn.microsoft.com/library/azure/dn707646.aspx) leírt állapot házirendek szerepel a szervezet úgy is megnyithatja.

## <a name="get-partition-health"></a>Partition állapot beszerzése
Ad vissza egy partíciót személy állapota. A replika állapot államok tartalmaz. Beviteli:

- [Szükséges] Azonosító (GUID), amely azonosítja a partíciót partíciót.

- [Választható] Az alkalmazás állapot használt házirendet felülbírálása az alkalmazás nyilvánvalóan házirend.

- [Választható] Az események és adja meg, mely bejegyzéseket érdeklődésére számot tartó, és az eredmény (például csak, a hibák vagy figyelmeztetések és hibák) a visszaadandó kópia szűrőket. Az összes események és kópiák összesíti entitás állapotának, függetlenül a szűrő kiértékeléséhez szolgálnak.

### <a name="api"></a>API
Partition állapot juthat az API-k, hozzon létre egy `FabricClient` , és hívja fel a [GetPartitionHealthAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient.getpartitionhealthasync.aspx) módszer a saját HealthManager. Nem kötelező paramétert, hozzon létre [PartitionHealthQueryDescription](https://msdn.microsoft.com/library/azure/system.fabric.description.partitionhealthquerydescription.aspx).

```csharp
PartitionHealth partitionHealth = await fabricClient.HealthManager.GetPartitionHealthAsync(partitionId);
```

### <a name="powershell"></a>A PowerShell
Partition állapotát jelző megszerezni parancsmag [Get-ServiceFabricPartitionHealth](https://msdn.microsoft.com/library/mt125869.aspx). Első lépésként csatlakozzon a fürthöz a [Csatlakozás-ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx) parancsmag használatával.

A következő parancsmagot a állapota az összes partíciót a kapja a **háló: / WordCount/WordCountService** szolgáltatás:

```powershell
PS C:\> Get-ServiceFabricPartition fabric:/WordCount/WordCountService | Get-ServiceFabricPartitionHealth


PartitionId           : a1f83a35-d6bf-4d39-b90d-28d15f39599b
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='System.FM', Property='State', HealthState='Warning', ConsiderWarningAsError=false.

ReplicaHealthStates   :
                        ReplicaId             : 131031502143040223
                        AggregatedHealthState : Ok

                        ReplicaId             : 131031502346844060
                        AggregatedHealthState : Ok

                        ReplicaId             : 131031502346844059
                        AggregatedHealthState : Ok

                        ReplicaId             : 131031502346844061
                        AggregatedHealthState : Ok

                        ReplicaId             : 131031502346844058
                        AggregatedHealthState : Ok

HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Warning
                        SequenceNumber        : 76
                        SentAt                : 3/22/2016 7:57:26 PM
                        ReceivedAt            : 3/22/2016 7:57:48 PM
                        TTL                   : Infinite
                        Description           : Partition is below target replica or instance count.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Warning = 3/22/2016 7:57:48 PM, LastOk = 1/1/0001 12:00:00 AM
```

### <a name="rest"></a>TÖBBI
Partition állapot [GET kérés](https://msdn.microsoft.com/library/azure/dn707683.aspx) , vagy egy [bejegyzés kérelem](https://msdn.microsoft.com/library/azure/dn707680.aspx) leírt állapot házirendek szerepel a szervezet úgy is megnyithatja.

## <a name="get-replica-health"></a>Első kópia állapota
Állapot-nyilvántartó szolgáltatás kópia vagy egy állapot nélküli szolgáltatás példány állapotának lekérdezése Beviteli:

- [Szükséges] Partition azonosító (GUID) és replika azonosítója, amely azonosítja a replika.

- [Választható] Az alkalmazás állapot házirend használt paraméterek felülbírálása az alkalmazás nyilvánvalóan házirendeket.

- [Választható] Adja meg, mely bejegyzéseket érdeklődésére számot tartó, és az eredmény (például csak, a hibák vagy figyelmeztetések és hibák) a visszaadandó események szűrőket. Összes eseményének összesíti entitás állapotának, függetlenül a szűrő kiértékeléséhez használt.

### <a name="api"></a>API
Első kópia állapota az API-k, hozzon létre egy `FabricClient` , és hívja fel a [GetReplicaHealthAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient.getreplicahealthasync.aspx) módszer a saját HealthManager. [ReplicaHealthQueryDescription](https://msdn.microsoft.com/library/azure/system.fabric.description.replicahealthquerydescription.aspx)segítségével speciális paramétert.

```csharp
ReplicaHealth replicaHealth = await fabricClient.HealthManager.GetReplicaHealthAsync(partitionId, replicaId);
```

### <a name="powershell"></a>A PowerShell
Replika állapotát jelző megszerezni parancsmag [Get-ServiceFabricReplicaHealth](https://msdn.microsoft.com/library/mt125808.aspx). Első lépésként csatlakozzon a fürthöz a [Csatlakozás-ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx) parancsmag használatával.

A következő parancsmagot a elsődleges másolat az összes partíciót a szolgáltatás állapota kapja:

```powershell
PS C:\> Get-ServiceFabricPartition fabric:/WordCount/WordCountService | Get-ServiceFabricReplica | where {$_.ReplicaRole -eq "Primary"} | Get-ServiceFabricReplicaHealth


PartitionId           : a1f83a35-d6bf-4d39-b90d-28d15f39599b
ReplicaId             : 131031502143040223
AggregatedHealthState : Ok
HealthEvents          :
                        SourceId              : System.RA
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 131031502145556748
                        SentAt                : 3/22/2016 7:56:54 PM
                        ReceivedAt            : 3/22/2016 7:57:12 PM
                        TTL                   : Infinite
                        Description           : Replica has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 3/22/2016 7:57:12 PM, LastWarning = 1/1/0001 12:00:00 AM
```

### <a name="rest"></a>TÖBBI
[GET kérés](https://msdn.microsoft.com/library/azure/dn707673.aspx) , vagy egy [bejegyzés kérelem](https://msdn.microsoft.com/library/azure/dn707641.aspx) leírt állapot házirendek szerepel a szervezet replika állapot elérheti.

## <a name="get-deployed-application-health"></a>A telepített alkalmazások állapot beszerzése
Az alkalmazások egy csomópont entitás telepítve állapotát jelző lekérdezése A telepített szolgáltatási csomag állapot állapotok tartalmaz. Beviteli:

- [Szükséges] Az alkalmazás nevét (URI) és csomópont nevét (karakterlánc), amely azonosítja a telepített alkalmazások.

- [Választható] Az alkalmazás nyilvánvalóan házirendek felülbírálják használt alkalmazás állapot házirend.

- [Választható] Az események és adja meg, mely bejegyzéseket érdeklődésére számot tartó, és az eredmény (például csak, a hibák vagy figyelmeztetések és hibák) a visszaadandó telepített szolgáltatást csomagok szűrők. Események és a telepített szolgáltatást csomagok szolgálnak az összesített entitás állapot, függetlenül a szűrő ki szeretné számítani.

### <a name="api"></a>API
Úgy juthat az alkalmazások az API-k csomópontok telepítve állapotát jelző, hozzon létre egy `FabricClient` , és hívja fel a [GetDeployedApplicationHealthAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient.getdeployedapplicationhealthasync.aspx) módszer a saját HealthManager. Nem kötelező paramétert [DeployedApplicationHealthQueryDescription](https://msdn.microsoft.com/library/azure/system.fabric.description.deployedapplicationhealthquerydescription.aspx)használja.

```csharp
DeployedApplicationHealth health = await fabricClient.HealthManager.GetDeployedApplicationHealthAsync(
    new DeployedApplicationHealthQueryDescription(applicationName, nodeName));
```

### <a name="powershell"></a>A PowerShell
A parancsmag a telepített alkalmazások állapot megszerezni a [Get-ServiceFabricDeployedApplicationHealth](https://msdn.microsoft.com/library/mt163523.aspx). Első lépésként csatlakozzon a fürthöz a [Csatlakozás-ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx) parancsmag használatával. Szeretné megtudni, ahol az alkalmazás telepítve van, futtassa a [Get-ServiceFabricApplicationHealth](https://msdn.microsoft.com/library/mt125976.aspx) , és tekintse meg a telepített alkalmazások gyermekektől.

A következő parancsmagot állapotának kapja a **háló: / WordCount** alkalmazás **_Node_2**rendszerre telepíthető.

```powershell
PS C:\> Get-ServiceFabricDeployedApplicationHealth -ApplicationName fabric:/WordCount -NodeName _Node_2


ApplicationName                    : fabric:/WordCount
NodeName                           : _Node_2
AggregatedHealthState              : Ok
DeployedServicePackageHealthStates :
                                     ServiceManifestName   : WordCountServicePkg
                                     NodeName              : _Node_2
                                     AggregatedHealthState : Ok

                                     ServiceManifestName   : WordCountWebServicePkg
                                     NodeName              : _Node_2
                                     AggregatedHealthState : Ok

HealthEvents                       :
                                     SourceId              : System.Hosting
                                     Property              : Activation
                                     HealthState           : Ok
                                     SequenceNumber        : 131031502143710698
                                     SentAt                : 3/22/2016 7:56:54 PM
                                     ReceivedAt            : 3/22/2016 7:57:12 PM
                                     TTL                   : Infinite
                                     Description           : The application was activated successfully.
                                     RemoveWhenExpired     : False
                                     IsExpired             : False
                                     Transitions           : Error->Ok = 3/22/2016 7:57:12 PM, LastWarning = 1/1/0001 12:00:00 AM
```

### <a name="rest"></a>TÖBBI
A telepített alkalmazások állapot [GET kérés](https://msdn.microsoft.com/library/azure/dn707644.aspx) , vagy egy [bejegyzés kérelem](https://msdn.microsoft.com/library/azure/dn707688.aspx) leírt állapot házirendek szerepel a szervezet úgy is megnyithatja.

## <a name="get-deployed-service-package-health"></a>A telepített szolgáltatásállapot csomag beszerzése
A telepített szolgáltatást csomag entitás állapotának lekérdezése Beviteli:

- [Szükséges] Az alkalmazás nevét (URI) csomópont nevét (karakterlánc) és szolgáltatás nyilvánvalóan neve (karakterlánc), amely azonosítja a telepített szolgáltatást csomag.

- [Választható] Az alkalmazás állapot használt házirendet felülbírálása az alkalmazás nyilvánvalóan házirend.

- [Választható] Adja meg, mely bejegyzéseket érdeklődésére számot tartó, és az eredmény (például csak, a hibák vagy figyelmeztetések és hibák) a visszaadandó események szűrőket. Összes eseményének összesíti entitás állapotának, függetlenül a szűrő kiértékeléséhez használt.

### <a name="api"></a>API
Telepített szolgáltatást csomag állapotának juthat az API-k, hozzon létre egy `FabricClient` , és hívja fel a [GetDeployedServicePackageHealthAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient.getdeployedservicepackagehealthasync.aspx) módszer a saját HealthManager. Nem kötelező paramétert [DeployedServicePackageHealthQueryDescription](https://msdn.microsoft.com/library/azure/system.fabric.description.deployedservicepackagehealthquerydescription.aspx)használja.

```csharp
DeployedServicePackageHealth health = await fabricClient.HealthManager.GetDeployedServicePackageHealthAsync(
    new DeployedServicePackageHealthQueryDescription(applicationName, nodeName, serviceManifestName));
```

### <a name="powershell"></a>A PowerShell
A parancsmag a telepített csomag szolgáltatásállapot megszerezni a [Get-ServiceFabricDeployedServicePackageHealth](https://msdn.microsoft.com/library/mt163525.aspx). Első lépésként csatlakozzon a fürthöz a [Csatlakozás-ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx) parancsmag használatával. Szeretné látni, amikor az alkalmazás telepítve van, futtassa a [Get-ServiceFabricApplicationHealth](https://msdn.microsoft.com/library/mt125976.aspx) , és tekintse meg a telepített alkalmazások. Melyik az alkalmazásokban azok a csomagok szolgáltatás megtekintéséhez nézze meg a telepített szolgáltatást csomag gyermekek a [Get-ServiceFabricDeployedApplicationHealth](https://msdn.microsoft.com/library/mt163523.aspx) eredményt ad.

A következő parancsmagot a **WordCountServicePkg** szolgáltatás csomagjában állapotának kapja a **háló: / WordCount** alkalmazás **_Node_2**rendszerre telepíthető. A szervezet **System.Hosting** jelentések sikeres szervizcsomagot bejegyzés pontos aktiválási és sikeres szolgáltatás típusa regisztrációs rendelkezik.

```powershell
PS C:\> Get-ServiceFabricDeployedApplication -ApplicationName fabric:/WordCount -NodeName _Node_2 | Get-ServiceFabricDeployedServicePackageHealth -ServiceManifestName WordCountServicePkg


ApplicationName       : fabric:/WordCount
ServiceManifestName   : WordCountServicePkg
NodeName              : _Node_2
AggregatedHealthState : Ok
HealthEvents          :
                        SourceId              : System.Hosting
                        Property              : Activation
                        HealthState           : Ok
                        SequenceNumber        : 131031502301306211
                        SentAt                : 3/22/2016 7:57:10 PM
                        ReceivedAt            : 3/22/2016 7:57:12 PM
                        TTL                   : Infinite
                        Description           : The ServicePackage was activated successfully.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 3/22/2016 7:57:12 PM, LastWarning = 1/1/0001 12:00:00 AM

                        SourceId              : System.Hosting
                        Property              : CodePackageActivation:Code:EntryPoint
                        HealthState           : Ok
                        SequenceNumber        : 131031502301568982
                        SentAt                : 3/22/2016 7:57:10 PM
                        ReceivedAt            : 3/22/2016 7:57:12 PM
                        TTL                   : Infinite
                        Description           : The CodePackage was activated successfully.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 3/22/2016 7:57:12 PM, LastWarning = 1/1/0001 12:00:00 AM

                        SourceId              : System.Hosting
                        Property              : ServiceTypeRegistration:WordCountServiceType
                        HealthState           : Ok
                        SequenceNumber        : 131031502314788519
                        SentAt                : 3/22/2016 7:57:11 PM
                        ReceivedAt            : 3/22/2016 7:57:12 PM
                        TTL                   : Infinite
                        Description           : The ServiceType was registered successfully.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 3/22/2016 7:57:12 PM, LastWarning = 1/1/0001 12:00:00 AM
```

### <a name="rest"></a>TÖBBI
A telepített csomag szolgáltatásállapot [GET kérés](https://msdn.microsoft.com/library/azure/dn707677.aspx) , vagy egy [bejegyzés kérelem](https://msdn.microsoft.com/library/azure/dn707689.aspx) leírt állapot házirendek szerepel a szervezet úgy is megnyithatja.

## <a name="health-chunk-queries"></a>Állapot adattömb lekérdezések
A rendszerállapot adattömb lekérdezések többszintű fürt gyermekek (rekurzív), a bemeneti szűrők / térhet vissza. Speciális szűrők, amelyek lehetővé teszik sok rugalmasan vissza, mely adott gyermekek express az egyedi azonosító vagy más csoport azonosítóját, illetve a rendszerállapot jelölt támogat. Alapértelmezés szerint nincs gyermek tartoznak, nem pedig a mindig vegyen fel az első szintű gyermekek állapot parancsait.

A [rendszerállapot lekérdezések](service-fabric-view-entities-aggregated-health.md#health-queries) vissza a megadott egységek szükséges szűrők / csak az első szintű gyermekeinek. Úgy juthat az alárendelt kifejezésekkel gyermekeinek, a további állapot API-khoz hívja fel minden személyhez érdeklődésre számot tartó. Hasonlóképpen úgy juthat az adott személyek állapotának, meg kell hívni egy állapot API minden kívánt személyhez. Az Irányított szűrés adattömb lekérdezés lehetővé teszi, hogy a fontos az üzenet mérete és az üzenetek számát minimalizálása egy lekérdezésben több elem kérése.

A adattömb lekérdezés értéke, hogy szerezhet be rendszerállapot fürt szervezetek (potenciálisan összes fürt entitás szükséges legfelső szintű kezdve) egy hívásban vesz részt. Összetett állapot lekérdezés például is express:

- Csak a hibát, és ezeket az alkalmazásokat az alkalmazások felvétele a figyelmeztetés szolgáltatások visszatérési |} hiba. Visszaadott szolgáltatásokhoz az összes partíciót tartalmazza.

- Csak 4 alkalmazások, a megadott nevük állapotának vissza.

- Csak a kívánt alkalmazás típusú alkalmazásokat állapotának vissza.

- Az összes telepített szervezetek térjen csomópontot. Alkalmazások lekérdezésére, az összes telepített alkalmazások, a megadott csomóponton és a csomóponton telepített szolgáltatást csomagok.

- A hiba összes másolatnál vissza. Alkalmazások, szolgáltatások, partíciót és csak a hiba kópiák adja eredményül.

- Lépjen vissza az összes alkalmazás. Egy adott szolgáltatás hozzáadása az összes partíciót.

Az állapot adattömb lekérdezés jelenleg csak az fürt entitás megjelenő. Akkor adja vissza az állapot fürt adattömb, amely tartalmazza:

- A rendszerállapot összesíti fürt.

- Az állapot állapot adattömb lista bemeneti szűrők tiszteletben csomópontok.

- Az állapot állam adattömb alkalmazáslistából tiszteletben bemeneti szűrők. Egyes alkalmazások állapot állam adattömb a tiszteletben bemeneti szűrők és az összes telepített alkalmazások, a szűrők tiszteletben adattömb lista összes szolgáltatásokkal adattömb listáját tartalmazza. Ugyanaz a szolgáltatások és a telepített alkalmazások a gyermek számára. Ezzel a módszerrel a fürt összes entitás esetleg visszaadható kérésére, hierarchikusan.

### <a name="cluster-health-chunk-query"></a>Fürt állapot adattömb lekérdezés
A fürt entitás állapotának adja eredményül, és a szükséges gyermekek hierarchikus állapot állam mennyiségű tartalmazza. Beviteli:

- [Választható] A csomópontok és a fürt események ki szeretné számítani használt fürt állapot házirend.

- [Választható] Az alkalmazás állapot házirend térkép, az állapot házirendek az alkalmazás nyilvánvalóan házirendek felülbírálják használt.

- [Választható] Szűrők csomópontok, és adja meg, milyen tételek alkalmazásokat az érdeklődésére számot tartó, és vissza kell az eredményben. A szűrők kötődnek egy személy vagy csoport személyek vagy a szinten összes entitás vonatkoznak. A szűrők listáját általános egyetlen szűrő és/vagy a szűrők adott azonosítók a lekérdezés által visszaadott finom-szemek szervezetek számára is tartalmazhatnak. Ha üres, a gyermekek nem visszaadott alapértelmezés szerint.
További információ a szűrők [NodeHealthStateFilter](https://msdn.microsoft.com/library/azure/system.fabric.health.nodehealthstatefilter.aspx) és [ApplicationHealthStateFilter](https://msdn.microsoft.com/library/azure/system.fabric.health.applicationhealthstatefilter.aspx). Az alkalmazás szűrők lehet rekurzív speciális szűrők gyermekek adja meg.

Adattömb eredménye magában foglalja a szűrők tiszteletben gyermekekkel.

Jelenleg a adattömb lekérdezés nem ad vissza sérült értékelést vagy entitás eseményeket. Hogy többletinformációt szerezhető be a meglévő fürt állapot lekérdezés használatával.

### <a name="api"></a>API
Fürt állapot adattömb juthat, hozzon létre egy `FabricClient` , és hívja fel a [GetClusterHealthChunkAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient.getclusterhealthchunkasync.aspx) módszer a saját **HealthManager**. [ClusterHealthQueryDescription](https://msdn.microsoft.com/library/azure/system.fabric.description.clusterhealthchunkquerydescription.aspx) állapot házirendek leírására átviheti és speciális szűrők.

A következő kódot fürt állapot adattömb kapja meg a speciális szűrők.

```csharp
var queryDescription = new ClusterHealthChunkQueryDescription();
queryDescription.ApplicationFilters.Add(new ApplicationHealthStateFilter()
    {
        // Return applications only if they are in error
        HealthStateFilter = HealthStateFilter.Error
    });

// Return all replicas
var wordCountServiceReplicaFilter = new ReplicaHealthStateFilter()
    {
        HealthStateFilter = HealthStateFilter.All
    };

// Return all replicas and all partitions
var wordCountServicePartitionFilter = new PartitionHealthStateFilter()
    {
        HealthStateFilter = HealthStateFilter.All
    };
wordCountServicePartitionFilter.ReplicaFilters.Add(wordCountServiceReplicaFilter);

// For specific service, return all partitions and all replicas
var wordCountServiceFilter = new ServiceHealthStateFilter()
{
    ServiceNameFilter = new Uri("fabric:/WordCount/WordCountService"),
};
wordCountServiceFilter.PartitionFilters.Add(wordCountServicePartitionFilter);

// Application filter: for specific application, return no services except the ones of interest
var wordCountApplicationFilter = new ApplicationHealthStateFilter()
    {
        // Always return fabric:/WordCount application
        ApplicationNameFilter = new Uri("fabric:/WordCount"),
    };
wordCountApplicationFilter.ServiceFilters.Add(wordCountServiceFilter);

queryDescription.ApplicationFilters.Add(wordCountApplicationFilter);

var result = await fabricClient.HealthManager.GetClusterHealthChunkAsync(queryDescription);
```

### <a name="powershell"></a>A PowerShell
Az első fürt állapotát jelző parancsmag [Get-ServiceFabricClusterChunkHealth](https://msdn.microsoft.com/library/mt644772.aspx). Első lépésként csatlakozzon a fürthöz a [Csatlakozás-ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx) parancsmag használatával.

A következő kódot csomópontok kap, csak ha az a hiba egy adott csomópontot, és mindig vissza kell kivételével.

```xml
PS C:\> $errorFilter = [System.Fabric.Health.HealthStateFilter]::Error;
$allFilter = [System.Fabric.Health.HealthStateFilter]::All;

$nodeFilter1 = New-Object System.Fabric.Health.NodeHealthStateFilter -Property @{HealthStateFilter=$errorFilter}
$nodeFilter2 = New-Object System.Fabric.Health.NodeHealthStateFilter -Property @{NodeNameFilter="_Node_1";HealthStateFilter=$allFilter}
# Create node filter list that will be passed in the cmdlet
$nodeFilters = New-Object System.Collections.Generic.List[System.Fabric.Health.NodeHealthStateFilter]
$nodeFilters.Add($nodeFilter1)
$nodeFilters.Add($nodeFilter2)

Get-ServiceFabricClusterHealthChunk -NodeFilters $nodeFilters

HealthState                  : Error
NodeHealthStateChunks        :
                               TotalCount            : 1

                               NodeName              : _Node_1
                               HealthState           : Ok

ApplicationHealthStateChunks : None
```

A következő parancsmagot a alkalmazás szűrőkkel fürt adattömb kap.

```xml
$errorFilter = [System.Fabric.Health.HealthStateFilter]::Error;
$allFilter = [System.Fabric.Health.HealthStateFilter]::All;

# All replicas
$replicaFilter = New-Object System.Fabric.Health.ReplicaHealthStateFilter -Property @{HealthStateFilter=$allFilter}

# All partitions
$partitionFilter = New-Object System.Fabric.Health.PartitionHealthStateFilter -Property @{HealthStateFilter=$allFilter}
$partitionFilter.ReplicaFilters.Add($replicaFilter)

# For WordCountService, return all partitions and all replicas
$svcFilter1 = New-Object System.Fabric.Health.ServiceHealthStateFilter -Property @{ServiceNameFilter="fabric:/WordCount/WordCountService"}
$svcFilter1.PartitionFilters.Add($partitionFilter)

$svcFilter2 = New-Object System.Fabric.Health.ServiceHealthStateFilter -Property @{HealthStateFilter=$errorFilter}

$appFilter = New-Object System.Fabric.Health.ApplicationHealthStateFilter -Property @{ApplicationNameFilter="fabric:/WordCount"}
$appFilter.ServiceFilters.Add($svcFilter1)
$appFilter.ServiceFilters.Add($svcFilter2)

$appFilters = New-Object System.Collections.Generic.List[System.Fabric.Health.ApplicationHealthStateFilter]
$appFilters.Add($appFilter)

Get-ServiceFabricClusterHealthChunk -ApplicationFilters $appFilters

HealthState                  : Error
NodeHealthStateChunks        : None
ApplicationHealthStateChunks :
                               TotalCount            : 1

                               ApplicationName       : fabric:/WordCount
                               ApplicationTypeName   : WordCount
                               HealthState           : Error
                               ServiceHealthStateChunks :
                                   TotalCount            : 1

                                   ServiceName           : fabric:/WordCount/WordCountService
                                   HealthState           : Error
                                   PartitionHealthStateChunks :
                                       TotalCount            : 1

                                       PartitionId           : a1f83a35-d6bf-4d39-b90d-28d15f39599b
                                       HealthState           : Error
                                       ReplicaHealthStateChunks :
                                           TotalCount            : 5

                                           ReplicaOrInstanceId   : 131031502143040223
                                           HealthState           : Ok

                                           ReplicaOrInstanceId   : 131031502346844060
                                           HealthState           : Ok

                                           ReplicaOrInstanceId   : 131031502346844059
                                           HealthState           : Ok

                                           ReplicaOrInstanceId   : 131031502346844061
                                           HealthState           : Ok

                                           ReplicaOrInstanceId   : 131031502346844058
                                           HealthState           : Error
```

A következő parancsmagot a telepített összes entitás csomóponton adja eredményül.

```xml
$errorFilter = [System.Fabric.Health.HealthStateFilter]::Error;
$allFilter = [System.Fabric.Health.HealthStateFilter]::All;

$dspFilter = New-Object System.Fabric.Health.DeployedServicePackageHealthStateFilter -Property @{HealthStateFilter=$allFilter}
$daFilter =  New-Object System.Fabric.Health.DeployedApplicationHealthStateFilter -Property @{HealthStateFilter=$allFilter;NodeNameFilter="_Node_2"}
$daFilter.DeployedServicePackageFilters.Add($dspFilter)

$appFilter = New-Object System.Fabric.Health.ApplicationHealthStateFilter -Property @{HealthStateFilter=$allFilter}
$appFilter.DeployedApplicationFilters.Add($daFilter)

$appFilters = New-Object System.Collections.Generic.List[System.Fabric.Health.ApplicationHealthStateFilter]
$appFilters.Add($appFilter)
Get-ServiceFabricClusterHealthChunk -ApplicationFilters $appFilters


HealthState                  : Error
NodeHealthStateChunks        : None
ApplicationHealthStateChunks :
                               TotalCount            : 2

                               ApplicationName       : fabric:/System
                               HealthState           : Ok
                               DeployedApplicationHealthStateChunks :
                                   TotalCount            : 1

                                   NodeName              : _Node_2
                                   HealthState           : Ok
                                   DeployedServicePackageHealthStateChunks :
                                       TotalCount            : 1

                                       ServiceManifestName   : FAS
                                       HealthState           : Ok



                               ApplicationName       : fabric:/WordCount
                               ApplicationTypeName   : WordCount
                               HealthState           : Error
                               DeployedApplicationHealthStateChunks :
                                   TotalCount            : 1

                                   NodeName              : _Node_2
                                   HealthState           : Ok
                                   DeployedServicePackageHealthStateChunks :
                                       TotalCount            : 2

                                       ServiceManifestName   : WordCountServicePkg
                                       HealthState           : Ok

                                       ServiceManifestName   : WordCountWebServicePkg
                                       HealthState           : Ok
```

### <a name="rest"></a>TÖBBI
Fürt állapot adattömb [GET kérés](https://msdn.microsoft.com/library/azure/mt656722.aspx) , vagy egy [bejegyzés kérés](https://msdn.microsoft.com/library/azure/mt656721.aspx) , amelynek szövegében szerepel a szervezet állapot házirendek és leírt speciális szűrők elérheti.

## <a name="general-queries"></a>Általános lekérdezések
Általános lekérdezések vissza a megadott típusú szolgáltatás háló személyek listáját. Azok az API-val (keresztül **FabricClient.QueryManager**módszerek), a PowerShell-parancsmagok és a többi elérhető. Ezek a lekérdezések segédlekérdezések összesíteni, a több összetevőt. Őket egyik a [állapot tárolni](service-fabric-health-introduction.md#health-store), amely feltölti a összesített rendszerállapot minden lekérdezés eredményét.  

> [AZURE.NOTE] Általános lekérdezések a szervezet összesített állapotának vissza, és nem tartalmaz adatokat gazdag állapota. Ha entitás nem megfelelő, követheti állapot lekérdezések összes a rendszerállapot adatait, beleértve eseményeket, a gyermek állapot államok és a sérült értékelést eléréséhez.

Általános lekérdezések eredményül egy ismeretlen rendszerállapot entitás, akkor lehet, hogy az állapot tároló nincs-e a szervezet teljes adatait. Hogy az is lehetséges, hogy az állapot Store segédlekérdezés nem lett sikeres (például egy kapcsolati hiba vagy az állapot tároló lett szabályozott). A szervezet állapot lekérdezéssel lett beállítva. Ha kulcsszóval a segédlekérdezés hibába ütközött tranziens, például hálózati hibák, a követési lekérdezés befejeződhet. Azt is kaphat további részleteket az állapot áruházból kapcsolatban, hogy miért nem elérhetővé tett a szervezet.

A lekérdezések entitás **HealthState** tartalmazó a következők:

- Csomópont lista: a lista csomópontok a fürt (lapozható) adja eredményül.
  - [FabricClient.QueryClient.GetNodeListAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.queryclient.getnodelistasync.aspx) API:
  - PowerShell: Get-ServiceFabricNode
- Alkalmazás lista: az alkalmazáslistából a fürt (lapozható) adja eredményül.
  - [FabricClient.QueryClient.GetApplicationListAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.queryclient.getapplicationlistasync.aspx) API:
  - PowerShell: Get-ServiceFabricApplication
- Szolgáltatáslistával: az alkalmazásokban (lapozható) adja eredményül a szolgáltatások listája.
  - [FabricClient.QueryClient.GetServiceListAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.queryclient.getservicelistasync.aspx) API:
  - PowerShell: Get-ServiceFabricService
- Partition lista: az listája (lapozható) szolgáltatás adja eredményül.
  - [FabricClient.QueryClient.GetPartitionListAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.queryclient.getpartitionlistasync.aspx) API:
  - PowerShell: Get-ServiceFabricPartition
- Replika lista: (lapozható) partíciót a kópiák listáját adja vissza.
  - [FabricClient.QueryClient.GetReplicaListAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.queryclient.getreplicalistasync.aspx) API:
  - PowerShell: Get-ServiceFabricReplica
- Telepített alkalmazások listája: értéket adja vissza, a telepített alkalmazások listája csomópontot.
  - [FabricClient.QueryClient.GetDeployedApplicationListAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.queryclient.getdeployedapplicationlistasync.aspx) API:
  - PowerShell: Get-ServiceFabricDeployedApplication
- Rendszerbe szolgáltatás csomagok listája: a szolgáltatás csomagok listája a telepített alkalmazások adja eredményül.
  - [FabricClient.QueryClient.GetDeployedServicePackageListAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.queryclient.getdeployedservicepackagelistasync.aspx) API:
  - PowerShell: Get-ServiceFabricDeployedApplication

> [AZURE.NOTE] A lekérdezések néhány lapozható eredményt adnak. Ezek a lekérdezések visszaadását felsoroljuk származás [PagedList<T>](https://msdn.microsoft.com/library/azure/mt280056.aspx). Az eredmények nem férnek el egy üzenetet, ha csak egy oldalt ad vissza, és egy ContinuationToken nyomon követő hol számbavétel leállt. Hívja fel a ugyanazt a lekérdezést, és a folytatását jelző jogkivonat át a következő eredményt előző lekérdezést kell folytassa.

### <a name="examples"></a>Példák

A következő kódot a fürt megkapja a sérült alkalmazások:

```csharp
var applications = fabricClient.QueryManager.GetApplicationListAsync().Result.Where(
  app => app.HealthState == HealthState.Error);
```

A következő parancsmagot a háló lekérése az alkalmazás részleteit: / WordCount alkalmazást. Figyelje meg, hogy rendszerállapot figyelmeztetés.

```powershell
PS C:\> Get-ServiceFabricApplication -ApplicationName fabric:/WordCount

ApplicationName        : fabric:/WordCount
ApplicationTypeName    : WordCount
ApplicationTypeVersion : 1.0.0
ApplicationStatus      : Ready
HealthState            : Warning
ApplicationParameters  : { "WordCountWebService_InstanceCount" = "1";
                         "_WFDebugParams_" = "[{"ServiceManifestName":"WordCountWebServicePkg","CodePackageName":"Code","EntryPointType":"Main","Debug
                         ExePath":"C:\\Program Files (x86)\\Microsoft Visual Studio
                         14.0\\Common7\\Packages\\Debugger\\VsDebugLaunchNotify.exe","DebugArguments":" {74f7e5d5-71a9-47e2-a8cd-1878ec4734f1} -p
                         [ProcessId] -tid [ThreadId]","EnvironmentBlock":"_NO_DEBUG_HEAP=1\u0000"},{"ServiceManifestName":"WordCountServicePkg","CodeP
                         ackageName":"Code","EntryPointType":"Main","DebugExePath":"C:\\Program Files (x86)\\Microsoft Visual Studio
                         14.0\\Common7\\Packages\\Debugger\\VsDebugLaunchNotify.exe","DebugArguments":" {2ab462e6-e0d1-4fda-a844-972f561fe751} -p
                         [ProcessId] -tid [ThreadId]","EnvironmentBlock":"_NO_DEBUG_HEAP=1\u0000"}]" }
```

A következő parancsmagot a szolgáltatások egy figyelmeztetés állapotának kapja:

```powershell
PS C:\> Get-ServiceFabricApplication | Get-ServiceFabricService | where {$_.HealthState -eq "Warning"}


ServiceName            : fabric:/WordCount/WordCountService
ServiceKind            : Stateful
ServiceTypeName        : WordCountServiceType
IsServiceGroup         : False
ServiceManifestVersion : 1.0.0
HasPersistedState      : True
ServiceStatus          : Active
HealthState            : Warning
```

## <a name="cluster-and-application-upgrades"></a>Fürt és az alkalmazás frissítése
A fürt és az alkalmazás felügyelt frissítés során szolgáltatás háló ellenőrzi, győződjön meg arról, hogy mindent marad a megfelelő állapota. Ha entitás sérült, mint kiértékelt konfigurált állapot házirendek használatával, a frissítés alkalmazza a frissítés-specifikus házirendeket határozza meg a következő műveletet. A frissítés engedélyezése felhasználói beavatkozás (például hiba feltételek rögzítéséről vagy házirendek módosításával) is felfüggesztett szinkronizálással vagy, előfordulhat, hogy automatikusan visszatér az előző kellő verzió.

Egy *fürt* frissítés során fürt frissítési állapotát úgy is megnyithatja. A frissítési állapot sérült értékelést, mutasson a Mi az a fürt sérült tartalmazza. Ha a frissítés visszaállítja szolgáltatásállapot-hiba miatt, a frissítési állapotát megjegyzi az utolsó sérült okok miatt. Ezek az információk segítségével, hogy kivizsgáljuk a probléma oka visszaállítja a frissítés, illetve leállítása után rendszergazdák.

Hasonlóképpen az *alkalmazás* a frissítés során bármely sérült értékelést szereplő alkalmazás frissítési állapotát.

Az alábbi alkalmazások frissítési állapotát láthatók a módosított háló: / WordCount alkalmazást. Egy figyelő hibajelzés annak kópiáit egyik. A frissítés vissza, mert az állapot-ellenőrzései elmaradása esetén értéke beállítással.

```powershell
PS C:\> Get-ServiceFabricApplicationUpgrade fabric:/WordCount

ApplicationName               : fabric:/WordCount
ApplicationTypeName           : WordCount
TargetApplicationTypeVersion  : 1.0.0.0
ApplicationParameters         : {}
StartTimestampUtc             : 4/21/2015 5:23:26 PM
FailureTimestampUtc           : 4/21/2015 5:23:37 PM
FailureReason                 : HealthCheck
UpgradeState                  : RollingBackInProgress
UpgradeDuration               : 00:00:23
CurrentUpgradeDomainDuration  : 00:00:00
CurrentUpgradeDomainProgress  : UD1

                                NodeName            : _Node_1
                                UpgradePhase        : Upgrading

                                NodeName            : _Node_2
                                UpgradePhase        : Upgrading

                                NodeName            : _Node_3
                                UpgradePhase        : PreUpgradeSafetyCheck
                                PendingSafetyChecks :
                                EnsurePartitionQuorum - PartitionId: 30db5be6-4e20-4698-8185-4bd7ca744020
NextUpgradeDomain             : UD2
UpgradeDomainsStatus          : { "UD1" = "Completed";
                                "UD2" = "Pending";
                                "UD3" = "Pending";
                                "UD4" = "Pending" }
UnhealthyEvaluations          :
                                Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.

                                  Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Error'.

                                      Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.

                                      Unhealthy partition: PartitionId='a1f83a35-d6bf-4d39-b90d-28d15f39599b', AggregatedHealthState='Error'.

                                          Unhealthy replicas: 20% (1/5), MaxPercentUnhealthyReplicasPerPartition=0%.

                                          Unhealthy replica: PartitionId='a1f83a35-d6bf-4d39-b90d-28d15f39599b',
                                  ReplicaOrInstanceId='131031502346844058', AggregatedHealthState='Error'.

                                              Error event: SourceId='DiskWatcher', Property='Disk'.

UpgradeKind                   : Rolling
RollingUpgradeMode            : UnmonitoredAuto
ForceRestart                  : False
UpgradeReplicaSetCheckTimeout : 00:15:00
```

További információ a [szolgáltatás háló alkalmazás frissítése](service-fabric-application-upgrade.md).

## <a name="use-health-evaluations-to-troubleshoot"></a>Állapot értékelést használatával kapcsolatos hibák elhárítása
Ha a fürt vagy az alkalmazás problémát, tekintse meg a mi pinpoint fürt vagy alkalmazás állapota. A sérült értékelést adja meg, hogy milyen indított az aktuális sérült állapotba olvashat. Ha kell, akkor is részletezést azonosítja a kiváltóok sérült gyermek szervezetek.

> [AZURE.NOTE] A sérült értékelést jelenjen meg az első oka az entitás kiértékelt aktuális állapota állapotára. Előfordulhat, hogy több más kiváltó események Ez az állapot, de azok nem lehet megjelennek a értékelést. Ha további információkra, részletezést megállapítása a sérült jelentések a fürt az állapot szervezetek.

## <a name="next-steps"></a>Következő lépések
[Használat rendszer állapotjelentések kapcsolatos hibák elhárítása](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)

[Adja hozzá egyéni szolgáltatás háló állapotjelentések](service-fabric-report-health.md)

[Hogyan lehet jelenteni, és ellenőrizze a szolgáltatás állapota](service-fabric-diagnostics-how-to-report-and-check-service-health.md)

[Figyelésére és szolgáltatások helyileg diagnosztizálása](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[Szolgáltatás háló alkalmazás frissítése](service-fabric-application-upgrade.md)
