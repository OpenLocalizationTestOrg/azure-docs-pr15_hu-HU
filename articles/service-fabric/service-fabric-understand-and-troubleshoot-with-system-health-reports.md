<properties
   pageTitle="A rendszer állapotjelentések elhárítása |} Microsoft Azure"
   description="A állapotjelentések küldte hibaelhárítási fürt vagy alkalmazás hibák Azure Service háló összetevők és a használatát ismerteti."
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

# <a name="use-system-health-reports-to-troubleshoot"></a>Használat rendszer állapotjelentések kapcsolatos hibák elhárítása

Azure Service háló összetevők beépített szóló jelentést a fürt összes entitás. [Állapot tárolása](service-fabric-health-introduction.md#health-store) létrehozása és törlése a rendszer jelentések alapján szervezetek az. Azt is rendezi őket, amely jellemzi egyed-kapcsolati hierarchiát.

> [AZURE.NOTE] Állapot kapcsolatos fogalmak megértéséhez, további információ a [szolgáltatás háló állapot modell](service-fabric-health-introduction.md).

Rendszer állapotjelentések betekintést kap abba, fürt és az alkalmazás működésének és keresztül állapotát jelző problémák biztosítanak. Az alkalmazások és szolgáltatások rendszer állapotjelentések ellenőrizze, hogy szervezetek végrehajtását szolgáltatás háló szemszögéből megfelelően viselkedik. A jelentések hírcsatornájának nincsenek olyan bármely állapot ellenőrzése a szolgáltatás üzleti logikai funkcióinak vagy lefagyott folyamatok kimutatása. Felhasználói szolgáltatások is kiegészítése azok összefüggés-specifikus adatait a rendszerállapot adatait.

> [AZURE.NOTE] Watchdogs állapotjelentések csak *után* a rendszer összetevők létrehozása egy egyed jelennek meg. Entitás törlésekor az állapot tároló automatikusan törli az összes állapotjelentések társítva. Ugyanez igaz a szervezet egy új példányát létrehozásakor (például egy új szolgáltatás replikált példány jön létre). A régi példányhoz társított összes jelentés törlődnek, és a áruházból törlődnek.

A rendszer összetevő-jelentések a forrás, amely kezdődik azonosítják az "**rendszert.**" előtag. Watchdogs nem használható az azonos előtag forrásuk, mint érvénytelen paraméterekkel jelentések elutasították.
Nézzük meg, bizonyos rendszer jelentéseket megtudhatja, hogy mi váltja őket, és az esetleges problémák jelölnek módját.

> [AZURE.NOTE] Jelentések felvétele a feltételnek érdeklődésre számot tartó betekintést kap abba, hogy mi történik a fürt és az alkalmazás javítása szolgáltatás háló továbbra is.

## <a name="cluster-system-health-reports"></a>Fürt rendszer állapotjelentések
A fürt állapot entitás automatikusan létrejön az állapot áruházban. Minden megfelelően működik, ha a rendszer jelentés nincsenek.

### <a name="neighborhood-loss"></a>Helyek veszteség
**System.Federation** jelentések hibát, ha azt észleli, hogy a hálózatok veszteség. A jelentés egyes csomópontok származik, és a csomópont-azonosító szerepel a tulajdonság neve. A teljes szolgáltatás háló kicsengetés egy hálózatok vesznek el, ha a szokásos számíthat két esemény (kétoldalas a térköz jelentés). További környékeken. elvesznek, esetén több esemény.

A jelentés a globális bérleti idejéhez adja meg a time to live dátumnak megfelelően. A jelentés minden felében a TTL (élettartam) időtartama Újraküldte mindaddig, amíg a feltétel aktív. Lejáratakor a rendszer automatikusan törli az eseményt. Ha a lejárt viselkedése biztosítja az, hogy a jelentés törlődnek az állapot áruházból megfelelő, akkor is, ha a jelentéskészítési csomópont nem működik, törölje.

- **SourceId forrásazonosító**: System.Federation
- **Tulajdonság**: **hálózatok** kezdődik és csomópont információkat tartalmaz
- **Következő lépések**: vizsgálja meg, miért a hálózatok elvész (például ellenőrizze a fürt csomópontok közötti kommunikáció).

## <a name="node-system-health-reports"></a>Csomópont rendszer állapotjelentések
**System.FM**, amely a Feladatátvevő kezelő szolgáltatás jelent, a szervezet fürt csomópontok információt kezelő. Az egyes csomópontok állapotát megjelenítő System.FM egy jelentés kell rendelkeznie. A csomópont személyek csomópont állapota törlődik (lásd: [RemoveNodeStateAsync](https://msdn.microsoft.com/library/azure/mt161348.aspx)) törlődnek.

### <a name="node-updown"></a>Csomópont felfelé/lefelé
System.FM jelentések, az OK gombra, ha a csomópont csatlakozik a gyűrű (van szó lépéseket). Jelentések hiba esetén a csomópontra a gyűrű indulását (kerül, lefelé, vagy-re történő frissítésének vagy egyszerűen, mert nem járt sikerrel). A beépített az állapot üzlet állapot hierarchiában a korrelációs System.FM csomópont jelentéseket a telepített szervezetek hajt végre műveletet. Az összes telepített entitás virtuális szülő megítélése szerint a csomópontot. A telepített személyek a csomóponton megjelenített lekérdezések keresztül, a csomópontot, akár készként által System.FM azonos példányával társított a személyek példányt. Ha System.FM jelzi, hogy a csomópont nem működik, vagy újra (új-példány), az állapot tároló automatikusan törli a köteggel a telepített személyek is megtalálható, csak a csomóponton lefelé vagy kattintson a csomópontra korábbi példányát.

- **SourceId forrásazonosító**: System.FM
- **Tulajdonság**: állapot
- **Következő lépések**: a csomópont-frissítés nem működik, ha azt érkezik vissza van a frissítése után. Ebben az esetben a rendszerállapot kell kapcsolni, vissza az OK gombra. Ha a csomópont térjen vissza nem vagy sikertelen, a probléma a további vizsgálat van szüksége.

Az alábbi példában egy állapotának OK csomópont System.FM esemény jelenik meg:

```powershell

PS C:\> Get-ServiceFabricNodeHealth -NodeName Node.1
NodeName              : Node.1
AggregatedHealthState : Ok
HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 2
                        SentAt                : 4/24/2015 5:27:33 PM
                        ReceivedAt            : 4/24/2015 5:28:50 PM
                        TTL                   : Infinite
                        Description           : Fabric node is up.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/24/2015 5:28:50 PM

```


### <a name="certificate-expiration"></a>Tanúsítvány érvényességi
**System.FabricNode** figyelmeztetés jelentések, a csomópont által használt tanúsítványokat közeli lejárati mellett. Vannak az egyes csomópontok három tanúsítványok: **Certificate_cluster** **Certificate_server**és **Certificate_default_client**. Ha a lejárat legalább két héttel nem vagyok a gépnél, a jelentés állapot állapota az OK gombra. A lejárati két héttel esik, a jelentés típusa esetén egy figyelmeztető üzenetet. TTL (élettartam), az alábbi események végtelen, és amikor csomópont elhagyja a fürt törlődnek.

- **SourceId forrásazonosító**: System.FabricNode
- **Tulajdonság**: **tanúsítvány** kezdődik, és a tanúsítvány típusa további információt tartalmaz
- **Következő lépések**: a tanúsítványok frissítése, ha lejárati közelében.

### <a name="load-capacity-violation"></a>Kapacitás bejelentése betöltése
A szolgáltatás háló terheléselosztó jelentések egy figyelmeztető üzenetet, ha azt észleli, hogy csomópont kapacitás szabálytalanság.

 - **SourceId forrásazonosító**: System.PLB
 - **Tulajdonság**: **kapacitás** karakterekkel kezdődik.
 - **Következő lépések**: megadott mértékek és olvassa el a jelenlegi kapacitás a csomópontra.

## <a name="application-system-health-reports"></a>Alkalmazás rendszer állapotjelentések
**System.CM**, amely a kezelő szolgáltatás jelent, a szervezet céljával kapcsolatos információk kezelő.

### <a name="state"></a>Állam
System.CM jelentések az OK gombra az alkalmazás létre vagy a változásáról. Erről tájékoztatást az állapot tároló alkalmazása a törölt, hogy el kell távolítani áruházból.

- **SourceId forrásazonosító**: System.CM
- **Tulajdonság**: állapot
- **Következő lépések**: Ha az alkalmazás már elkészült, a kezelő állapotjelentés tartalmaznia kell. Egyéb esetben azáltal, hogy a lekérdezés a az alkalmazás állapotának ellenőrzése (például a PowerShell-parancsmag * *Get-ServiceFabricApplication-ApplicationName *applicationName***).

A következő példa bemutatja az állapot esemény meg a **háló: / WordCount** alkalmazás:

```powershell
PS C:\> Get-ServiceFabricApplicationHealth fabric:/WordCount -ServicesFilter None -DeployedApplicationsFilter None

ApplicationName                 : fabric:/WordCount
AggregatedHealthState           : Ok
ServiceHealthStates             : None
DeployedApplicationHealthStates : None
HealthEvents                    :
                                  SourceId              : System.CM
                                  Property              : State
                                  HealthState           : Ok
                                  SequenceNumber        : 82
                                  SentAt                : 4/24/2015 6:12:51 PM
                                  ReceivedAt            : 4/24/2015 6:12:51 PM
                                  TTL                   : Infinite
                                  Description           : Application has been created.
                                  RemoveWhenExpired     : False
                                  IsExpired             : False
                                  Transitions           : ->Ok = 4/24/2015 6:12:51 PM
```

## <a name="service-system-health-reports"></a>Szolgáltatás rendszer állapotjelentések
**System.FM**, amely a Feladatátvevő kezelő szolgáltatás jelent, a kezelő szolgáltatással kapcsolatban további információt a szervezet.

### <a name="state"></a>Állam
System.FM jelentések az OK gombra a szolgáltatás létrehozásakor. A szervezet törlése az állapot áruházból a szolgáltatás törlésekor.

- **SourceId forrásazonosító**: System.FM
- **Tulajdonság**: állapot

A következő példa bemutatja az állapot esemény az szolgáltatása **háló: / WordCount/WordCountService**:

```powershell
PS C:\> Get-ServiceFabricServiceHealth fabric:/WordCount/WordCountService

ServiceName           : fabric:/WordCount/WordCountService
AggregatedHealthState : Ok
PartitionHealthStates :
                        PartitionId           : 875a1caa-d79f-43bd-ac9d-43ee89a9891c
                        AggregatedHealthState : Ok

HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 3
                        SentAt                : 4/24/2015 6:12:51 PM
                        ReceivedAt            : 4/24/2015 6:13:01 PM
                        TTL                   : Infinite
                        Description           : Service has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/24/2015 6:13:01 PM
```

### <a name="unplaced-replicas-violation"></a>Helyezett kópiák bejelentése
**System.PLB** jelentések egy figyelmeztető üzenetet, ha nem talál egy egy vagy több szolgáltatás kópiák elhelyezését. A jelentés törlődik lejáratakor.

- **SourceId forrásazonosító**: System.FM
- **Tulajdonság**: állapot
- **Következő lépés**: Ellenőrizze a szolgáltatás kényszerek és elhelyezése az aktuális állapotát.

A következő példa bemutatja egy 5 csomópontokkal fürt kópiájába 7 cél konfigurált szolgáltatás szabálytalanság:

```xml
PS C:\> Get-ServiceFabricServiceHealth fabric:/WordCount/WordCountService


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
                        SequenceNumber        : 131032232425505477
                        SentAt                : 3/23/2016 4:14:02 PM
                        ReceivedAt            : 3/23/2016 4:14:03 PM
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
                        Transitions           : Error->Warning = 3/22/2016 7:57:48 PM, LastOk = 1/1/0001 12:00:00 AM
```

## <a name="partition-system-health-reports"></a>Partition rendszer állapotjelentések
**System.FM**, amely a Feladatátvevő kezelő szolgáltatás jelent, a szervezet kezeli a szolgáltatási partíciót.

### <a name="state"></a>Állam
System.FM jelentések az OK gombra a partíciót létrehozás és megfelelő. A szervezet törlése az állapot áruházból partíciót törlésekor.

Ha a partíciót nem éri el a minimális replika számát, hiba történt jelenti. Ha a partíciót nem alatt a minimális replika count, de a cél replika count alatt, a program figyelmeztetésben jelenti. Ha a partíciót kvórumvesztést, a System.FM jelentések hibát jelez.

Egyéb fontos események jeleníteni a figyelmeztetés, amikor a konfigurálás több időt vesz igénybe a vártnál, és a vártnál hosszabb ideig tart a összeállítása. A várt az összeállítás és konfigurálás, konfigurálható a szolgáltatás felhasználási területei alapján időpontok. Például ha egy szolgáltatást tartalmaz egy állam SQL-adatbázissal, például adjon összeállítása tovább tart, mint a szolgáltatás állapota kis mennyiségű.

- **SourceId forrásazonosító**: System.FM
- **Tulajdonság**: állapot
- **Következő lépések**: Ha nem az OK gombra a rendszerállapot, akkor lehetséges, hogy néhány kópiát nem létrehozott, megnyitása, vagy az elsődleges és másodlagos megfelelően megszerzése. Sok esetben a legfelső szintű oka Megnyitás vagy szerepkörének módosítása végrehajtása szolgáltatás hibáját.

A következő példa bemutatja a megfelelő partíciót:

```powershell
PS C:\> Get-ServiceFabricPartition fabric:/StatelessPiApplication/StatelessPiService | Get-ServiceFabricPartitionHealth
PartitionId           : 29da484c-2c08-40c5-b5d9-03774af9a9bf
AggregatedHealthState : Ok
ReplicaHealthStates   : None
HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 38
                        SentAt                : 4/24/2015 6:33:10 PM
                        ReceivedAt            : 4/24/2015 6:33:31 PM
                        TTL                   : Infinite
                        Description           : Partition is healthy.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/24/2015 6:33:31 PM
```

A következő példa bemutatja egy partíciót, amely nem éri el cél replika darab állapotának. A következő lépés: a partíciók, a leírás, amely mutatja, hogy miként van konfigurálva kihozni: **MinReplicaSetSize** két pedig **TargetReplicaSetSize** hét. Kattintson a fürt kapcsolatfelvétel csomópontok számának: öt. Ebben az esetben két kópia nem kell elhelyezni.

```powershell
PS C:\> Get-ServiceFabricPartition fabric:/WordCount/WordCountService | Get-ServiceFabricPartitionHealth -ReplicasFilter None

PartitionId           : 875a1caa-d79f-43bd-ac9d-43ee89a9891c
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='System.FM', Property='State', HealthState='Warning', ConsiderWarningAsError=false.

ReplicaHealthStates   : None
HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Warning
                        SequenceNumber        : 37
                        SentAt                : 4/24/2015 6:13:12 PM
                        ReceivedAt            : 4/24/2015 6:13:31 PM
                        TTL                   : Infinite
                        Description           : Partition is below target replica or instance count.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Ok->Warning = 4/24/2015 6:13:31 PM

PS C:\> Get-ServiceFabricPartition fabric:/WordCount/WordCountService

PartitionId            : 875a1caa-d79f-43bd-ac9d-43ee89a9891c
PartitionKind          : Int64Range
PartitionLowKey        : 1
PartitionHighKey       : 26
PartitionStatus        : Ready
LastQuorumLossDuration : 00:00:00
MinReplicaSetSize      : 2
TargetReplicaSetSize   : 7
HealthState            : Warning
DataLossNumber         : 130743727710830900
ConfigurationNumber    : 8589934592


PS C:\> @(Get-ServiceFabricNode).Count
5
```

### <a name="replica-constraint-violation"></a>Replika kényszer bejelentése
**System.PLB** jelentések egy figyelmeztető üzenetet, ha replika kényszer szabálytalanság észleli és nem helyezhetők el a partíciót replikáit.

- **SourceId forrásazonosító**: System.PLB
- **Tulajdonság**: **ReplicaConstraintViolation** karakterekkel kezdődik.

## <a name="replica-system-health-reports"></a>Replika rendszer állapotjelentések
**System.RA**, amely a konfigurálás ügynök összetevő jelent, a szervezet az replika állapot.

### <a name="state"></a>Állam
**System.RA** jelentések az OK gombra a replika létrehozásakor.

- **SourceId forrásazonosító**: System.RA
- **Tulajdonság**: állapot

A következő példa bemutatja a megfelelő kópia:

```powershell
PS C:\> Get-ServiceFabricPartition fabric:/WordCount/WordCountService | Get-ServiceFabricReplica | where {$_.ReplicaRole -eq "Primary"} | Get-ServiceFabricReplicaHealth
PartitionId           : 875a1caa-d79f-43bd-ac9d-43ee89a9891c
ReplicaId             : 130743727717237310
AggregatedHealthState : Ok
HealthEvents          :
                        SourceId              : System.RA
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 130743727718018580
                        SentAt                : 4/24/2015 6:12:51 PM
                        ReceivedAt            : 4/24/2015 6:13:02 PM
                        TTL                   : Infinite
                        Description           : Replica has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/24/2015 6:13:02 PM
```

### <a name="replica-open-status"></a>Replika megnyitott állapota
Az API-hívás elindították a állapotjelentés leírását tartalmazza a kezdési időpontot (egyezményes világidő).

**System.RA** jelentések egy figyelmeztető üzenetet, ha a megnyitott replika tovább tart, mint a beállított időszak (alapértelmezett: 30 perc). Ha az API szolgáltatáselérhetőség hatással van, a jelentés kibocsátott sokkal gyorsabb (az alapértelmezés 30 másodperc egy konfigurálható interval). Mért idő a replikációs meg van nyitva, és nyissa meg a szolgáltatás idő tartalmazza. A tulajdonság módosításaira az OK gombra, ha meg van nyitva az végrehajtotta.

- **SourceId forrásazonosító**: System.RA
- **Tulajdonság**: **ReplicaOpenStatus**
- **Következő lépések**: Ha nem az OK gombra a rendszerállapot, vizsgálja meg, miért nyissa meg a replika vártnál hosszabb ideig tart.

### <a name="slow-service-api-call"></a>API-hívást lassú
**System.RAP** és **System.Replicator** jelentés egy figyelmeztetés, ha a hívást kezdeményez, a felhasználó szolgáltatáskód konfigurált időnél tovább tart. A figyelmeztetés nincs bejelölve, ha befejezte a hívást.

- **SourceId forrásazonosító**: System.RAP vagy System.Replicator
- **Tulajdonság**: a lassú API nevét. A leírás biztosít az idő az API függőben olvashat bővebben.
- **Következő lépések**: vizsgálja meg, miért vártnál hosszabb ideig tart a hívást.

A következő példa bemutatja a partíciók kvórumvesztést és való tudni, hogy miért vizsgálat lépéseit. A figyelmeztetés rendszerállapot egyik replikán tartoznak, így az állapotát kap. Azt mutatja, hogy a szolgáltatási művelet vártnál hosszabb idő alatt, System.RAP által jelzett esemény. Miután ezt az információt érkezik, a következő lépésként tekintse meg a szolgáltatáskód, és ott vizsgálja meg. Az ebben az esetben a az állapot-nyilvántartó szolgáltatás **RunAsync** végrehajtása esetén nem kezelt kivételt okoz. A replikák újrafelhasználás vannak, így esetleg nem látható az állapot bármely kópiájába. A rendszerállapot első újra, és keressen bármely közötti különbségek a replika azonosítóját. Bizonyos esetekben a próbálkozások is megjelenhetnek jelek.

```powershell
PS C:\> Get-ServiceFabricPartition fabric:/HelloWorldStatefulApplication/HelloWorldStateful | Get-ServiceFabricPartitionHealth

PartitionId           : 72a0fb3e-53ec-44f2-9983-2f272aca3e38
AggregatedHealthState : Error
UnhealthyEvaluations  :
                        Error event: SourceId='System.FM', Property='State'.

ReplicaHealthStates   :
                        ReplicaId             : 130743748372546446
                        AggregatedHealthState : Ok

                        ReplicaId             : 130743746168084332
                        AggregatedHealthState : Ok

                        ReplicaId             : 130743746195428808
                        AggregatedHealthState : Warning

                        ReplicaId             : 130743746195428807
                        AggregatedHealthState : Ok

HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Error
                        SequenceNumber        : 182
                        SentAt                : 4/24/2015 7:00:17 PM
                        ReceivedAt            : 4/24/2015 7:00:31 PM
                        TTL                   : Infinite
                        Description           : Partition is in quorum loss.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Warning->Error = 4/24/2015 6:51:31 PM

PS C:\> Get-ServiceFabricPartition fabric:/HelloWorldStatefulApplication/HelloWorldStateful

PartitionId            : 72a0fb3e-53ec-44f2-9983-2f272aca3e38
PartitionKind          : Int64Range
PartitionLowKey        : -9223372036854775808
PartitionHighKey       : 9223372036854775807
PartitionStatus        : InQuorumLoss
LastQuorumLossDuration : 00:00:13
MinReplicaSetSize      : 2
TargetReplicaSetSize   : 3
HealthState            : Error
DataLossNumber         : 130743746152927699
ConfigurationNumber    : 227633266688

PS C:\> Get-ServiceFabricReplica 72a0fb3e-53ec-44f2-9983-2f272aca3e38 130743746195428808

ReplicaId           : 130743746195428808
ReplicaAddress      : PartitionId: 72a0fb3e-53ec-44f2-9983-2f272aca3e38, ReplicaId: 130743746195428808
ReplicaRole         : Primary
NodeName            : Node.3
ReplicaStatus       : Ready
LastInBuildDuration : 00:00:01
HealthState         : Warning

PS C:\> Get-ServiceFabricReplicaHealth 72a0fb3e-53ec-44f2-9983-2f272aca3e38 130743746195428808

PartitionId           : 72a0fb3e-53ec-44f2-9983-2f272aca3e38
ReplicaId             : 130743746195428808
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='System.RAP', Property='ServiceOpenOperationDuration', HealthState='Warning', ConsiderWarningAsError=false.

HealthEvents          :
                        SourceId              : System.RA
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 130743756170185892
                        SentAt                : 4/24/2015 7:00:17 PM
                        ReceivedAt            : 4/24/2015 7:00:33 PM
                        TTL                   : Infinite
                        Description           : Replica has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/24/2015 7:00:33 PM

                        SourceId              : System.RAP
                        Property              : ServiceOpenOperationDuration
                        HealthState           : Warning
                        SequenceNumber        : 130743756399407044
                        SentAt                : 4/24/2015 7:00:39 PM
                        ReceivedAt            : 4/24/2015 7:00:59 PM
                        TTL                   : Infinite
                        Description           : Start Time (UTC): 2015-04-24 19:00:17.019
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Warning = 4/24/2015 7:00:59 PM
```

A debugger csoportban a hibás alkalmazás indításakor a a diagnosztikai események windows megjelenítése a kivétel, a RunAsync lépett fel:

![Visual Studio 2015 diagnosztikai események: háló RunAsync-hiba: / HelloWorldStatefulApplication.][1]

Visual Studio 2015 diagnosztikai események: nem sikerült RunAsync **háló: / HelloWorldStatefulApplication**.

[1]: ./media/service-fabric-understand-and-troubleshoot-with-system-health-reports/servicefabric-health-vs-runasync-exception.png


### <a name="replication-queue-full"></a>Teljes replikációs sor
**System.Replicator** jelentések egy figyelmeztető üzenetet, ha a replikáció várólista teljes. Az elsődleges, a Ez általában oka az, hogy egy vagy több másodlagos kópiák lassú elismerjük műveletek. Kattintson a másodlagos Ez általában történik, ha a szolgáltatás lassú a műveletek alkalmazásához. A figyelmeztetés nincs bejelölve, ha a sor már nem teljes.

- **SourceId forrásazonosító**: System.Replicator
- **Tulajdonság**: **PrimaryReplicationQueueStatus** vagy **SecondaryReplicationQueueStatus**attól függően, hogy a replikált szerepkör

### <a name="slow-naming-operations"></a>Lassú a műveletek elnevezése

Az elsődleges replikakészlet, amikor egy névhasználati művelet tovább tart, mint elfogadható **System.NamingService** a jelentések állapota. Névhasználati műveletek példák [CreateServiceAsync](https://msdn.microsoft.com/library/azure/mt124028.aspx) vagy [DeleteServiceAsync](https://msdn.microsoft.com/library/azure/mt124029.aspx). További módszerek találhatók a FabricClient, például a [szolgáltatás felügyeleti módszerek](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.servicemanagementclient.aspx) vagy [tulajdonság management módszereket](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.propertymanagementclient.aspx).

> [AZURE.NOTE] A névhasználati szolgáltatás szolgáltatásnevek megszünteti a fürt helyre, és lehetővé teszi a felhasználóknak szolgáltatásnevek és a tulajdonságok kezelése. A szolgáltatás háló particionálva állandó szolgáltatás. A partíciók közül jelzi, hogy a szervezet tulajdonosa, amely tartalmazza az összes szolgáltatás háló nevek és services metaadatok. A szolgáltatás háló neveket különböző partíciók nevezik tulajdonost partíciók,, a szolgáltatás nem bővíthető vannak megfeleltetve. További információ a [névhasználati szolgáltatás](service-fabric-architecture.md).

Amikor egy névhasználati művelet a vártnál hosszabb ideig tart, a művelet megjelöli a figyelmeztetés az *elsődleges replikáját a névhasználati szolgáltatás partíciót, amely szolgál, hogy a művelet*a jelentéssel együtt. Ha a művelet sikeresen befejeződött, a figyelmeztető üzenet törlődik. A művelet befejeződött, a hibát, ha a az állapotjelentés tartalmazni a hiba részleteit.

- **SourceId forrásazonosító**: System.NamingService
- **Tulajdonság**: kezdődő **Duration_** előtag és azonosítja a lassú művelet és a szolgáltatás háló neve, amelyen a művelet történik. Ha például ha neve háló szolgáltatás hozzon létre: / SajátPr/MyService túl sokáig tart, a tulajdonság értéke Duration_AOCreateService.fabric:/MyApp/MyService. A név és a művelet a névhasználati partíciót szerepe AO mutat.
- **Következő lépések**: Miért nem sikerült a névhasználati művelet jelölőnégyzetet. Az egyes műveletek beállíthatja, hogy más okát. Például törlése csomópont ragadt előfordulhat, hogy a szolgáltatás, mert az alkalmazás host csomópontra a szolgáltatáskód felhasználói hiba miatt továbbra is összeomló.

A következő példa bemutatja a létrehozási szolgáltatás művelet. A művelet tartott hosszabb, mint a beállított időtartam. AO próbálkozások, és értesítést küld a munka értékre. NINCS időkorlát az utolsó művelet befejeződött. Ebben az esetben kópiakészlet az elsődleges, a AO és a nincs szerepkörök.

```powershell
PartitionId           : 00000000-0000-0000-0000-000000001000
ReplicaId             : 131064359253133577
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='System.NamingService', Property='Duration_AOCreateService.fabric:/MyApp/MyService', HealthState='Warning', ConsiderWarningAsError=false.

HealthEvents          :
                        SourceId              : System.RA
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 131064359308715535
                        SentAt                : 4/29/2016 8:38:50 PM
                        ReceivedAt            : 4/29/2016 8:39:08 PM
                        TTL                   : Infinite
                        Description           : Replica has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 4/29/2016 8:39:08 PM, LastWarning = 1/1/0001 12:00:00 AM

                        SourceId              : System.NamingService
                        Property              : Duration_AOCreateService.fabric:/MyApp/MyService
                        HealthState           : Warning
                        SequenceNumber        : 131064359526778775
                        SentAt                : 4/29/2016 8:39:12 PM
                        ReceivedAt            : 4/29/2016 8:39:38 PM
                        TTL                   : 00:05:00
                        Description           : The AOCreateService started at 2016-04-29 20:39:08.677 is taking longer than 30.000.
                        RemoveWhenExpired     : True
                        IsExpired             : False
                        Transitions           : Error->Warning = 4/29/2016 8:39:38 PM, LastOk = 1/1/0001 12:00:00 AM

                        SourceId              : System.NamingService
                        Property              : Duration_NOCreateService.fabric:/MyApp/MyService
                        HealthState           : Warning
                        SequenceNumber        : 131064360657607311
                        SentAt                : 4/29/2016 8:41:05 PM
                        ReceivedAt            : 4/29/2016 8:41:08 PM
                        TTL                   : 00:00:15
                        Description           : The NOCreateService started at 2016-04-29 20:39:08.689 completed with FABRIC_E_TIMEOUT in more than 30.000.
                        RemoveWhenExpired     : True
                        IsExpired             : False
                        Transitions           : Error->Warning = 4/29/2016 8:39:38 PM, LastOk = 1/1/0001 12:00:00 AM
```

## <a name="deployedapplication-system-health-reports"></a>DeployedApplication rendszer állapotjelentések
**System.Hosting** a szervezet telepített szervezetek.

### <a name="activation"></a>Az aktiválás
System.Hosting jelentések az OK gombra, ha az alkalmazások aktiválása sikeresen megtörtént a csomópontra. Egyéb esetben jelenti hibát jelez.

- **SourceId forrásazonosító**: System.Hosting
- **Tulajdonság**: aktiválási, beleértve a bevezetés verzió
- **Következő lépések**: Ha az alkalmazás sérült, vizsgálja meg, miért nem sikerült az aktiválás.

A következő példa bemutatja a sikeres aktiválásához:

```powershell
PS C:\> Get-ServiceFabricDeployedApplicationHealth -NodeName Node.1 -ApplicationName fabric:/WordCount

ApplicationName                    : fabric:/WordCount
NodeName                           : Node.1
AggregatedHealthState              : Ok
DeployedServicePackageHealthStates :
                                     ServiceManifestName   : WordCountServicePkg
                                     NodeName              : Node.1
                                     AggregatedHealthState : Ok

HealthEvents                       :
                                     SourceId              : System.Hosting
                                     Property              : Activation
                                     HealthState           : Ok
                                     SequenceNumber        : 130743727751144415
                                     SentAt                : 4/24/2015 6:12:55 PM
                                     ReceivedAt            : 4/24/2015 6:13:03 PM
                                     TTL                   : Infinite
                                     Description           : The application was activated successfully.
                                     RemoveWhenExpired     : False
                                     IsExpired             : False
                                     Transitions           : ->Ok = 4/24/2015 6:13:03 PM
```

### <a name="download"></a>Letöltés
**System.Hosting** jelentések hibát, ha az alkalmazás csomag letöltése sikertelen.

- **SourceId forrásazonosító**: System.Hosting
- **Tulajdonság**: * *letöltése:*RolloutVersion***
- **Következő lépések**: vizsgálja meg, miért nem sikerült a letöltés a csomópontra.

## <a name="deployedservicepackage-system-health-reports"></a>DeployedServicePackage rendszer állapotjelentések
**System.Hosting** a szervezet telepített szervezetek.

### <a name="service-package-activation"></a>Szolgáltatás-csomag aktiválása
System.Hosting jelzi, az OK gombra, ha a szolgáltatás csomag aktiválása a csomóponton sikeres. Egyéb esetben jelenti hibát jelez.

- **SourceId forrásazonosító**: System.Hosting
- **Tulajdonság**: aktiválása
- **Következő lépések**: vizsgálja meg, miért nem sikerült az aktiválás.

### <a name="code-package-activation"></a>Kód csomag aktiválása
**System.Hosting** jelentések, az OK gombra az egyes kód csomagok sikeres az aktiválás esetén. Ha nem sikerül az aktiválás, jelenti Figyelmeztetés beállítva. Ha **CodePackage** aktiválása sikertelen, vagy nagyobb, mint a beállított **CodePackageHealthErrorThreshold**,-jelentéseket tartalmazó hibával ér véget hibát jelez. Ha a szolgáltatás csomag több kódot csomagot tartalmaz, az aktiválási jelentés mindegyik jön létre.

- **SourceId forrásazonosító**: System.Hosting
- **Tulajdonság**: az előtag **CodePackageActivation** használ, és a nevét a kód csomag és a belépési pontjához, mint * *CodePackageActivation:*CodePackageName*:*SetupEntryPoint/belépési* ** (például **CodePackageActivation:Code:SetupEntryPoint**)

### <a name="service-type-registration"></a>Regisztráció a szolgáltatás típusa
Ha a szolgáltatás típusa sikerült regisztrálni **System.Hosting** OK, jelentést. Ha a regisztráció nem lett történik, az idő (szerint állítható be **ServiceTypeRegistrationTimeout**) hiba jelenti. Ha a szolgáltatás nem regisztrált a csomópont, ennek oka a futási idő megszűnt. Figyelmeztetés szolgáltatója jelentést.

- **SourceId forrásazonosító**: System.Hosting
- **Tulajdonság**: az előtag **ServiceTypeRegistration** használ, és a szolgáltatás neve (például a **ServiceTypeRegistration:FileStoreServiceType**) tartalma

A következő példa bemutatja a megfelelő telepített szolgáltatást csomag:

```powershell
PS C:\> Get-ServiceFabricDeployedServicePackageHealth -NodeName Node.1 -ApplicationName fabric:/WordCount -ServiceManifestName WordCountServicePkg


ApplicationName       : fabric:/WordCount
ServiceManifestName   : WordCountServicePkg
NodeName              : Node.1
AggregatedHealthState : Ok
HealthEvents          :
                        SourceId              : System.Hosting
                        Property              : Activation
                        HealthState           : Ok
                        SequenceNumber        : 130743727751456915
                        SentAt                : 4/24/2015 6:12:55 PM
                        ReceivedAt            : 4/24/2015 6:13:03 PM
                        TTL                   : Infinite
                        Description           : The ServicePackage was activated successfully.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/24/2015 6:13:03 PM

                        SourceId              : System.Hosting
                        Property              : CodePackageActivation:Code:EntryPoint
                        HealthState           : Ok
                        SequenceNumber        : 130743727751613185
                        SentAt                : 4/24/2015 6:12:55 PM
                        ReceivedAt            : 4/24/2015 6:13:03 PM
                        TTL                   : Infinite
                        Description           : The CodePackage was activated successfully.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/24/2015 6:13:03 PM

                        SourceId              : System.Hosting
                        Property              : ServiceTypeRegistration:WordCountServiceType
                        HealthState           : Ok
                        SequenceNumber        : 130743727753644473
                        SentAt                : 4/24/2015 6:12:55 PM
                        ReceivedAt            : 4/24/2015 6:13:03 PM
                        TTL                   : Infinite
                        Description           : The ServiceType was registered successfully.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/24/2015 6:13:03 PM
```

### <a name="download"></a>Letöltés
**System.Hosting** jelentések hibát, ha nem sikerült letölteni a szolgáltatást a csomagot.

- **SourceId forrásazonosító**: System.Hosting
- **Tulajdonság**: * *letöltése:*RolloutVersion***
- **Következő lépések**: vizsgálja meg, miért nem sikerült a letöltés a csomópontra.

### <a name="upgrade-validation"></a>Frissítési ellenőrzése
**System.Hosting** jelentések hibát, ha nem sikerül egy adatérvényesítési a frissítés során, vagy ha a frissítés sikertelen lesz a csomópontra.

- **SourceId forrásazonosító**: System.Hosting
- **Tulajdonság**: az előtag **FabricUpgradeValidation** használ, és frissítésre tartalmazza.
- **Leírás**: a hiba mutat

## <a name="next-steps"></a>Következő lépések
[Szolgáltatás háló állapotjelentések megtekintése](service-fabric-view-entities-aggregated-health.md)

[Hogyan lehet jelenteni, és ellenőrizze a szolgáltatás állapota](service-fabric-diagnostics-how-to-report-and-check-service-health.md)

[Figyelésére és szolgáltatások helyileg diagnosztizálása](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[Szolgáltatás háló alkalmazás frissítése](service-fabric-application-upgrade.md)
