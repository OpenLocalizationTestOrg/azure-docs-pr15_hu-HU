<properties
   pageTitle="Alkalmazás frissítése – hibaelhárítás |} Microsoft Azure"
   description="Ez a cikk bemutatja a gyakori problémákat körül a szolgáltatás háló-alkalmazások és azok megoldását."
   services="service-fabric"
   documentationCenter=".net"
   authors="mani-ramaswamy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/14/2016"
   ms.author="subramar"/>

# <a name="troubleshoot-application-upgrades"></a>Alkalmazás frissítése – problémamegoldás

Ez a cikk bemutatja, hogy néhány gyakori hibák körül az Azure Service háló alkalmazások és azok megoldását.

## <a name="troubleshoot-a-failed-application-upgrade"></a>A sikertelen alkalmazás frissítése – problémamegoldás

Ha nem sikerül a frissítést, a a **Get-ServiceFabricApplicationUpgrade** parancs a hibakereséshez a hibát további információkat tartalmaz.  Az alábbi listában adja meg, hogyan használható a további információt:

1. A hiba típusának azonosítása.
2. Azonosítsa a hiba oka.
3. Egy vagy több további vizsgálatához hibás összetevők elkülönítése.

Ezt az információt a szolgáltatás háló észleli a hibát, függetlenül attól, hogy a **FailureAction** visszaállítása vagy a frissítés felfüggesztése esetén érhető el.

### <a name="identify-the-failure-type"></a>A hiba típusának azonosítása

A **Get-ServiceFabricApplicationUpgrade**kimenetét **FailureTimestampUtc** azonosítja az időbélyegző (a UTC szerint), amelynél frissítési hibát észlelt szolgáltatás háló és **FailureAction** indított volt. **FailureReason** azonosítja a hiba három lehetséges magas szintű okok egyike:

1. UpgradeDomainTimeout - azt jelzi, hogy egy adott frissítési tartomány túl sokáig befejezéséhez **UpgradeDomainTimeout** lejárt.
2. OverallUpgradeTimeout - azt jelzi, hogy a teljes frissítés végrehajtása túl sokáig tartott **UpgradeTimeout** lejárt.
3. HealthCheck - azt jelzi, hogy miután Frissítettem az update domain, az alkalmazás maradt sérült aszerint, hogy a megadott állapot házirendek **HealthCheckRetryTimeout** lejárt.

Ezeket a bejegyzéseket csak jelenik meg az eredményt ad, amikor a frissítés sikertelen lesz, és elindítja a visszaállítása. További információ a hibát típusától függően jelenik meg.

### <a name="investigate-upgrade-timeouts"></a>Vizsgálja meg a frissítési időtúllépései

Frissítse a hibák leggyakrabban okozta elérhetősége szolgáltatásproblémák időtúllépés. A következő a bekezdés kimenet a frissítési tipikus hol a szolgáltatás kópiák vagy példányok nem indítsa el az új kód verzióban. A **UpgradeDomainProgressAtFailure** mező rögzíti a hiba idején függőben frissítési munkáját pillanatképét.

~~~
PS D:\temp> Get-ServiceFabricApplicationUpgrade fabric:/DemoApp

ApplicationName                : fabric:/DemoApp
ApplicationTypeName            : DemoAppType
TargetApplicationTypeVersion   : v2
ApplicationParameters          : {}
StartTimestampUtc              : 4/14/2015 9:26:38 PM
FailureTimestampUtc            : 4/14/2015 9:27:05 PM
FailureReason                  : UpgradeDomainTimeout
UpgradeDomainProgressAtFailure : MYUD1

                                 NodeName            : Node4
                                 UpgradePhase        : PostUpgradeSafetyCheck
                                 PendingSafetyChecks :
                                     WaitForPrimaryPlacement - PartitionId: 744c8d9f-1d26-417e-a60e-cd48f5c098f0

                                 NodeName            : Node1
                                 UpgradePhase        : PostUpgradeSafetyCheck
                                 PendingSafetyChecks :
                                     WaitForPrimaryPlacement - PartitionId: 4b43f4d8-b26b-424e-9307-7a7a62e79750
UpgradeState                   : RollingBackCompleted
UpgradeDuration                : 00:00:46
CurrentUpgradeDomainDuration   : 00:00:00
NextUpgradeDomain              :
UpgradeDomainsStatus           : { "MYUD1" = "Completed";
                                 "MYUD2" = "Completed";
                                 "MYUD3" = "Completed" }
UpgradeKind                    : Rolling
RollingUpgradeMode             : UnmonitoredAuto
ForceRestart                   : False
UpgradeReplicaSetCheckTimeout  : 00:00:00
~~~

Ebben a példában a frissítés sikertelen frissítési tartományban *MYUD1* , és két partíciót (*744c8d9f-1d26-417e-a60e-cd48f5c098f0* és *4b43f4d8-b26b-424e-9307-7a7a62e79750*) ragadt volt. A partíciók voltak problémákat tapasztal, mert a futtatókörnyezet nem tudott elsődleges kópiák (*WaitForPrimaryPlacement*) elhelyezése a cél csomópontok *Node1* és *Node4*.

Győződjön meg róla, hogy a két csomópont frissítési tartományban *MYUD1*a **Get-ServiceFabricNode** parancs használható. A *UpgradePhase* arról tájékoztat, hogy *PostUpgradeSafetyCheck*, ami azt jelenti, hogy, hogy ezek biztonsági ellenőrzés csomópontjait frissítési a tartományban végzett frissítése után fordul elő. Ezt az információt az alkalmazás-kódot az új verzióval lehetséges problémát mutat. A leggyakoribb problémákat szolgáltatás hibák, a Megnyitás vagy a promóciós kód elsődleges elérési utakra állnak.

Egy *UpgradePhase* *PreUpgradeSafetyCheck* az azt jelenti, hogy volt-frissítési a tartomány előkészítése, mielőtt történt meg, hogy problémákat. A leggyakoribb problémákat ebben az esetben a a Bezárás gombra vagy a lefokozás elsődleges kód elérési út a szolgáltatás-hibák történnek.

Az aktuális **UpgradeState** *RollingBackCompleted*,, így az eredeti frissítés kell elvégezte a egy visszaállítása **FailureAction**, amely automatikusan visszaállítja a frissítés sikertelenség. Az eredeti frissítés végezték egy kézi **FailureAction**, ha majd a frissítés szeretné helyette kell egy függesztve engedélyezni élő hibakeresése során az alkalmazás.

### <a name="investigate-health-check-failures"></a>Állapot-ellenőrzés hibák vizsgálata

Állapot-ellenőrzés hibák is váltja a különböző problémákat, amelyek akkor fordulhat elő, egy frissítési tartományban lévő összes csomópontok verziójáról vált, és az összes biztonsági ellenőrzés átadása után. A kimenet ezen a bekezdésen követő frissítési hiba miatt sikertelen állapot-ellenőrzései jellemző. A **UnhealthyEvaluations** mezőben pillanatképet aszerint, hogy a megadott [állapot házirendet](service-fabric-health-introduction.md)a frissítés során nem sikerült állapot-ellenőrzései rögzíti.

~~~
PS D:\temp> Get-ServiceFabricApplicationUpgrade fabric:/DemoApp

ApplicationName                         : fabric:/DemoApp
ApplicationTypeName                     : DemoAppType
TargetApplicationTypeVersion            : v4
ApplicationParameters                   : {}
StartTimestampUtc                       : 4/24/2015 2:42:31 AM
UpgradeState                            : RollingForwardPending
UpgradeDuration                         : 00:00:27
CurrentUpgradeDomainDuration            : 00:00:27
NextUpgradeDomain                       : MYUD2
UpgradeDomainsStatus                    : { "MYUD1" = "Completed";
                                          "MYUD2" = "Pending";
                                          "MYUD3" = "Pending" }
UnhealthyEvaluations                    :
                                          Unhealthy services: 50% (2/4), ServiceType='PersistedServiceType', MaxPercentUnhealthyServices=0%.

                                          Unhealthy service: ServiceName='fabric:/DemoApp/Svc3', AggregatedHealthState='Error'.

                                              Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.

                                              Unhealthy partition: PartitionId='3a9911f6-a2e5-452d-89a8-09271e7e49a8', AggregatedHealthState='Error'.

                                                  Error event: SourceId='Replica', Property='InjectedFault'.

                                          Unhealthy service: ServiceName='fabric:/DemoApp/Svc2', AggregatedHealthState='Error'.

                                              Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.

                                              Unhealthy partition: PartitionId='744c8d9f-1d26-417e-a60e-cd48f5c098f0', AggregatedHealthState='Error'.

                                                  Error event: SourceId='Replica', Property='InjectedFault'.

UpgradeKind                             : Rolling
RollingUpgradeMode                      : Monitored
FailureAction                           : Manual
ForceRestart                            : False
UpgradeReplicaSetCheckTimeout           : 49710.06:28:15
HealthCheckWaitDuration                 : 00:00:00
HealthCheckStableDuration               : 00:00:10
HealthCheckRetryTimeout                 : 00:00:10
UpgradeDomainTimeout                    : 10675199.02:48:05.4775807
UpgradeTimeout                          : 10675199.02:48:05.4775807
ConsiderWarningAsError                  :
MaxPercentUnhealthyPartitionsPerService :
MaxPercentUnhealthyReplicasPerPartition :
MaxPercentUnhealthyServices             :
MaxPercentUnhealthyDeployedApplications :
ServiceTypeHealthPolicyMap              :
~~~

A szolgáltatás háló állapot modell megértéséhez először állapot ellenőrzése hibák kivizsgálása szükséges. Az ilyen meg szeretné vizsgálni egyetértést, nélkül is azt láthatja, hogy a két szolgáltatás sérült is, de: *háló: / DemoApp/Svc3* és *háló: / DemoApp/Svc2*, és a hiba állapotjelentések (ebben az esetben a "InjectedFault" jelöli). Ebben a példában két "Házon kívül" négy szolgáltatások sérült, amely nem éri el az alapértelmezett cél 0 %-os sérült (*MaxPercentUnhealthyServices*).

A frissítés után sikertelen volt felfüggesztett egy **FailureAction** kézi megadásával, a frissítés indításakor. Ebben az üzemmódban lehetővé teszi, hogy vizsgálja meg a hibás állapotú az élő rendszer véve bármely további művelet előtt, hogy mi.

### <a name="recover-from-a-suspended-upgrade"></a>A felfüggesztett frissítése helyreállítása

A visszaállítás **FailureAction**nincs szükség a frissítés automatikusan visszaállítja hibás, mivel helyreállítás nem. A kézi **FailureAction**több helyreállítási lehetőség van:

1. Kézzel indítja el egy visszaállítása
2. Haladjon végig a frissítés maradéka manuálisan
3. A felügyelt frissítés folytatása

A **Kezdés-ServiceFabricApplicationRollback** parancs visszaállítása az alkalmazás indítása bármikor használható. A parancsot sikerült ad vissza, a visszaállítás kérelem van regisztrálva, a rendszer, és hamarosan azt követően kezdődik.

Az **Önéletrajz-ServiceFabricApplicationUpgrade** parancs használható haladjon végig a frissítés maradéka manuálisan, egyszerre egy frissítési tartománnyal. Ebben az üzemmódban csak biztonsági ellenőrzés végez a rendszer. Nincs több állapot-ellenőrzései hajt végre. Ez a parancs csak használható, ha a *UpgradeState* *RollingForwardPending*, ami azt jelenti, hogy az aktuális frissítési tartomány frissítése nem fejeződött jeleníti meg, de a következő dokumentumot nem indult el (függőben).

A **Frissítés-ServiceFabricApplicationUpgrade** parancs koppintva folytathatja a megfigyelt frissítés mindkét biztonsági is használható, és állapota ellenőrzi, hogy végrehajtását.

~~~
PS D:\temp> Update-ServiceFabricApplicationUpgrade fabric:/DemoApp -UpgradeMode Monitored

UpgradeMode                             : Monitored
ForceRestart                            :
UpgradeReplicaSetCheckTimeout           :
FailureAction                           :
HealthCheckWaitDuration                 :
HealthCheckStableDuration               :
HealthCheckRetryTimeout                 :
UpgradeTimeout                          :
UpgradeDomainTimeout                    :
ConsiderWarningAsError                  :
MaxPercentUnhealthyPartitionsPerService :
MaxPercentUnhealthyReplicasPerPartition :
MaxPercentUnhealthyServices             :
MaxPercentUnhealthyDeployedApplications :
ServiceTypeHealthPolicyMap              :

PS D:\temp>
~~~

A frissítés továbbra is fennáll, a hol utolsó felfüggesztett frissítési tartományt, és a azonos frissítése paramétereket és állapot házirendek, mielőtt használata. Ha szükséges, frissítési paramétereket és az előző eredményben látható állapot házirendek megváltoztathatja a parancs ugyanaz folytatja a frissítés során. Ebben a példában a frissítés folytatódik figyelt módban, a paramétereket és az állapot házirendeket, nem változik.

## <a name="further-troubleshooting"></a>További hibaelhárítási

### <a name="service-fabric-is-not-following-the-specified-health-policies"></a>Szolgáltatás háló nem követő a megadott állapot házirendek

Lehetséges ok 1:

Szolgáltatás összes százalékértékek fordítja tényleges számok egységek (például kópiák partíciót és szolgáltatások) kiértékelési állapot és a teljes személyek mindig felfelé kerekít. Például ha a maximális _MaxPercentUnhealthyReplicasPerPartition_ 21 %, és nincsenek öt kópiák, majd szolgáltatás háló lehetővé teszi, hogy legfeljebb két sérült kópia (adott is,'Math.Ceiling (5\*0.21)). Állapot házirendek így lehetőséget kell beállítani.

Lehetséges ok 2:

Állapot házirendek értelmez teljes szolgáltatások és a nem adott szolgáltatás példányainak százalékértékek vannak megadva. Például a frissítést, ha egy alkalmazás négy előtt példányok A, B, C és a D, szolgáltatás, ahol a szolgáltatás D sérült, de az alkalmazás kis hatása. Szeretnénk figyelmen kívül hagyása az ismert sérült szolgáltatás D frissítés és a paraméter *MaxPercentUnhealthyServices* 25 %-kal, feltéve, hogy csak A, B és C kell megfelelő beállítása során.

Azonban a frissítés során, D válhat megfelelő közben C sérült válik. A frissítés továbbra is tenné sikerrel, mert a 25 %-csak a szolgáltatások megsérült. Azonban eredményezheti váratlan hibák miatt éppen váratlanul sérült helyett d C Ebben az esetben a D kell modellezni egy másik szolgáltatáscsaládba típusúként a, B és C. Állapot házirendek szolgáltatást típusonként nincs megadva, mivel sérült százalékértéket küszöbértékek különböző szolgáltatások alkalmazhatók. 

### <a name="i-did-not-specify-a-health-policy-for-application-upgrade-but-the-upgrade-still-fails-for-some-time-outs-that-i-never-specified"></a>Nem lehet adta meg egy olyan házirendet alkalmazás frissítésre, de a frissítés sikertelen lesz továbbra is, néhány időtúllépési, hogy soha nem meghatározott

Amennyiben az állapot házirendek nem szeretné a frissítési kérést, azok a *ApplicationManifest.xml* alkalmazás jelenlegi verziójával kell venni. Például ha alkalmazás X frissít, 3.0.0.0-s verziója a 1.0 alkalmazás állapot házirendek 2.0-s verzióval a 1.0 verziójú megadott használják. Ha egy másik állapot házirendet a frissítés kell használni, majd a házirend szüksége van az alkalmazás frissítési API-hívás részeként kell megadni. A házirendek az API-hívás részeként megadott csak alkalmazása a frissítés során. Amikor befejeződik a frissítés, a megadott a *ApplicationManifest.xml* házirendeket használja.

### <a name="incorrect-time-outs-are-specified"></a>Helytelen adatokon való műveletvégzés van megadva.

Akkor is rendelkezik testreszabásakor nem tudta kapcsolatban, mi történik, ha az adatokon való műveletvégzés érvénytelenként vannak-e beállítva. Előfordulhat például, egy kisebb, mint a *UpgradeDomainTimeout* *UpgradeTimeout* . A választ ki kell, hogy hibát jelez. Ha a *UpgradeDomainTimeout* kisebb, mint *HealthCheckWaitDuration* és *HealthCheckRetryTimeout*összege, vagy ha *UpgradeDomainTimeout* *HealthCheckWaitDuration* és *HealthCheckStableDuration*összege kisebb visszaküldött hibák.

### <a name="my-upgrades-are-taking-too-long"></a>A frissítések túl sok időbe telik

A frissítés elvégzéséhez időt az állapot-ellenőrzései és az adatokon való műveletvégzés megadott függ. Állapot-ellenőrzései és az adatokon való műveletvégzés függenek, mennyi ideig tart, telepítése, és az alkalmazás stabilizálásához. Túl olyan szigorú adatokon való műveletvégzés az éppen jelentheti a további sikertelen frissítések, ezért azt javasoljuk, amelyekről kezdődő hosszabb adatokon való műveletvégzés.

Íme egy rövid emlékeztető a és a frissítési időpontok az adatokon való műveletvégzés együttműködését:

Frissíti egy frissítési tartomány gyorsabban *HealthCheckWaitDuration*nem sikerül elvégezni a + *HealthCheckStableDuration*.

Nem lehet nagyobb, mint *HealthCheckWaitDuration*fordulhat elő, frissítési hiba + *HealthCheckRetryTimeout*.

Frissítési tartomány esetén a frissítés ideje *UpgradeDomainTimeout*korlátozza.  Ha az alkalmazás állapotának oda-vissza továbbra is vált *HealthCheckRetryTimeout* és *HealthCheckStableDuration* egyaránt nullától, majd a frissítés végül időtúllépés történik a *UpgradeDomainTimeout*. *UpgradeDomainTimeout* elindul számlálás az aktuális frissítési tartományhoz tartozó a frissítés megkezdése után.

## <a name="next-steps"></a>Következő lépések

[Az alkalmazás használatáról a Visual Studio frissítése](service-fabric-application-upgrade-tutorial.md) végigvezeti az alkalmazás frissítésre Visual Studio segítségével.

[Az alkalmazás Powershell használatá frissítés](service-fabric-application-upgrade-tutorial-powershell.md) végigvezeti az alkalmazás frissítésre PowerShell használatával.

Szabályozhatja, hogy hogyan az alkalmazás [Frissítése](service-fabric-application-upgrade-parameters.md)paramétereket frissíti.

Úgy, hogy az alkalmazás frissítések kompatibilis tanulási [Adatszerializáció](service-fabric-application-upgrade-data-serialization.md)használatáról.

Megtudhatja, hogy miként használja a speciális funkcióit, az alkalmazás [Speciális témakörökre](service-fabric-application-upgrade-advanced.md)mutató hivatkozással frissítésekor.

Gyakori problémák megoldása az alkalmazás frissítések hivatkozva [Alkalmazás frissítése – hibaelhárítás](service-fabric-application-upgrade-troubleshooting.md)című témakör lépéseit.
 
