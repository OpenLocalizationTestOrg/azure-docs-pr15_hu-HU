<properties
   pageTitle="Szolgáltatás háló fürt erőforrás-kezelő - elhelyezése házirendek |} Microsoft Azure"
   description="További elhelyezése és háló szolgáltatásokért szabályok – áttekintés"
   services="service-fabric"
   documentationCenter=".net"
   authors="masnider"
   manager="timlt"
   editor=""/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/19/2016"
   ms.author="masnider"/>

# <a name="placement-policies-for-service-fabric-services"></a>Elhelyezési házirendek szolgáltatást háló
Számos különböző további szabályok, akkor előfordulhat, hogy a végeredmény ápolásáért kapcsolatban, ha a szolgáltatás háló fürt van átnyúló végig a földrajzi távolságokat mondja ki több adatközpontokkal vagy Azure területek, vagy ha a környezet geopolitikai vezérlőelem (vagy egyes eset hol Önt érdeklő jogi vagy házirend határai rendelkezik, vagy az érintett távolságokat tényleges időtartam/teljesítmény hatást) egyszerre több területre fogja át. Ezek a legtöbb konfigurálható csomópont tulajdonságait és elhelyezését kényszerek, de néhány bonyolultabb. A műveletet egyszerűbbé teszik a kínálunk, amely ezeket további commmands. Hasonlóan a többi elhelyezési korlátozásokkal elhelyezési házirendek beállíthatók szolgáltatás használati nevű példány alapon.

## <a name="specifying-invalid-domains"></a>Érvénytelen tartományok megadása
A InvalidDomain elhelyezési házirendet, hogy egy adott hibafa tartomány érvénytelen-e a terhelést megadását teszi lehetővé. Ezzel a házirenddel biztosítja, hogy egy adott szolgáltatás soha nem fut egy adott terület, például a geopolitikai vagy vállalati házirendek miatt. Érvénytelen több tartományt is megadható külön házirendek keresztül.

![Érvénytelen tartomány példa][Image1]

Kód:

```csharp
ServicePlacementInvalidDomainPolicyDescription invalidDomain = new ServicePlacementInvalidDomainPolicyDescription();
invalidDomain.DomainName = "fd:/DCEast"; //regulations prohibit this workload here
serviceDescription.PlacementPolicies.Add(invalidDomain);
```

A PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 2 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("InvalidDomain,fd:/DCEast”)
```
## <a name="specifying-required-domains"></a>Adja meg a szükséges tartományok
A szükséges tartományi elhelyezése házirend szükséges összes állapot-nyilvántartó kópiák vagy a szolgáltatás állapot nélküli szolgáltatás példányainak megtalálható a megadott tartomány. Több szükséges tartomány keresztül külön házirendek adható meg.

![Szükséges tartományi példa][Image2]

Kód:

```csharp
ServicePlacementRequiredDomainPolicyDescription requiredDomain = new ServicePlacementRequiredDomainPolicyDescription();
requiredDomain.DomainName = "fd:/DC01/RK03/BL2";
serviceDescription.PlacementPolicies.Add(requiredDomain);
```

A PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 2 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("RequiredDomain,fd:/DC01/RK03/BL2")
```

## <a name="specifying-a-preferred-domain-for-the-primary-replicas"></a>Az elsődleges replikáinak elsődleges tartomány megadása
Az előnyben részesített elsődleges tartomány egy érdekes vezérlő, mivel lehetővé teszi a kijelölést, amelyben az elsődleges kell elhelyezni, ha lehetséges ehhez hibafa tartomány. Ha mindent megfelelő az elsődleges fejeződik ebben a tartományban. A tartomány vagy a elsődleges replika nem kell, vagy valamilyen okból az elsődleges áttelepítendőként néhány más helyre leállítása. Ha ezen a helyen nincs, a használni kívánt tartományt, majd amikor csak lehetséges a fürt erőforrás-kezelő fog helyezze át azt vissza a használni kívánt tartományt. Természetesen ezt a beállítást csak értelme az állapot-nyilvántartó szolgáltatások. Ezzel a házirenddel a legegyszerűbb fürt, amely több Azure régiók és több adatközpontokkal átnyúló vannak. Ezekben az esetekben redundancia az összes hely használja, de inkább az, hogy az az elsődleges kópiák egy bizonyos helyre kerüljenek, nyissa meg az elsődleges műveletekhez alsó késés biztosítása érdekében (ír is alapértelmezés szerint minden olvasás vannak, melyet az elsődleges és).

![Elsődleges tartományok és feladatátvételi előnyben részesített][Image3]

```csharp
ServicePlacementPreferPrimaryDomainPolicyDescription primaryDomain = new ServicePlacementPreferPrimaryDomainPolicyDescription();
primaryDomain.DomainName = "fd:/EastUS/";
serviceDescription.PlacementPolicies.Add(invalidDomain);
```

A PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 2 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("PreferredPrimaryDomain,fd:/EastUS")
```

## <a name="requiring-replicas-to-be-distributed-among-all-domains-and-disallowing-packing"></a>Tartományok és a emellett csomagolást között felosztandó kópiák megkövetelése
Megadhat egy másik házirend mindig között felosztandó a rendelkezésre álló hibafa tartományok kópiák megkövetelésére. Ez akkor fordulhat elő a legtöbb olyan esetekben, amikor a fürt megfelelő alapértelmezés szerint vannak azonban degenerate olyan esetekben, ahol kópiák az adott partíciót előfordulhat, hogy a végeredmény ideiglenes csomagolt áthelyezése egyetlen hiba vagy frissítési a tartomány. Ha például tegyük fel, bár a fürt 9 csomópontok rendelkezik a 3 hibafa domains (0, 1 és 2), és a szolgáltatás 3 kópiák, az adott hibafa tartományok 1 és 2 kópiájába használt csomópontok csökkent, és kapacitás problémák miatt az adott tartományban lévő többi csomópontok sem érvényes. Szolgáltatás háló össze a kópiák pótlása mintha az erőforrás-kezelő fürt kellene helyezze át őket az hibafa tartomány 0, de egy olyan helyzet, ha a hiba tartomány kényszer sérti hoz létre. Szintén növeli az esélye annak, hogy a teljes kópiakészlet elveszhetnek (ha FD 0 elveszett permananently). (További információt a korlátok és a kényszer prioritásának Általánosságban elmondható, olvassa el [Ebben a témakörben](service-fabric-cluster-resource-manager-management-integration.md#constraint-priorities) )

Ha minden eddiginél láthatta, jelennek állapota figyelmeztetés "a terheléselosztó észlelt a replika: háló a kényszer szabálytalanság: /<some service name> másodlagos Partition <some partition ID> van megsértésével a korlátozó feltételt: FaultDomain" már gombra kattint vagy az alábbihoz hasonló, akkor ez a feltétel. Ezekben az esetekben általában tranziens (csomópontok ne maradjon le hosszú, vagy ugyanúgy működnek, ha a létrehozásához szükséges javaslatok ott-e a megfelelő hibafa tartományok érvényes más csomópontok), de inkább volna kereskedelmi elérhetőségét a kockázat összes másolatnál elvesztése bizonyos feladatok. Azt a "RequireDomainDistribution" házirendet, amely garantálja, hogy az azonos partíciót a két replika minden eddiginél engedélyezettek ugyanabban a tartományban hibafa vagy a frissítés megadásával teheti meg.

Néhány munkaterhelésekből használnának olyan kópiák (állapot példányainak) cél száma egyáltalán időpontok (fogadási teljes tartomány hibák ellen, és arra, hogy ezek általában helyreállítani a helyi, állami), míg mások inkább tenni, ha az állásidőt kockázat helyességét és dataloss kérdésére választ adhatnak-nél korábbi. A legtöbb gyártási munkaterhelésekből legfeljebb 3 kópiákkal futtatni, mivel az alapértelmezett, hogy nem kell tartományt terjesztési terheléselosztási és esetek a szokásos módon kezelésére, még akkor is, ha, amely azt jelenti, hogy ideiglenes tartomány jelentek meg a mikrofonba több kópiák feladatátvételi engedélyezni.

Kód:

```csharp
ServicePlacementRequireDomainDistributionPolicyDescription distributeDomain = new ServicePlacementRequireDomainDistributionPolicyDescription();
serviceDescription.PlacementPolicies.Add(distributeDomain);
```

A PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 2 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("RequiredDomainDistribution")
```

Most akkor lehet szolgáltatások, amelyek nem földrajzilag átnyúló fürt őket használni? Biztos abban, hogy sikerült! De nem áll remek okának túl – különösen a kötelező, a érvénytelen és a használni kívánt tartományra vonatkozó beállítások kerülendő, kivéve, ha a ténylegesen futtat egy földrajzilag átnyúló fürt - nem értelme bármely próbál meg egy adott terhelést egyetlen állvány futhat, vagy inkább a helyi fürtre néhány szakaszában fölé egy másik, csak a különböző típusú megy hardvereszközt vagy terhelést szegmens kikényszerítése , és az azokra az esetekre normál elhelyezése kényszerek keresztül is kezelhető.

## <a name="next-steps"></a>Következő lépések
- Az elérhető lehetőségekről további információt a témakör nézze át az erőforrás-kezelő fürt konfigurációk Services elérhető [szolgáltatások beállításával kapcsolatos további tudnivalók](service-fabric-cluster-resource-manager-configure-services.md)

[Image1]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies/cluster-invalid-placement-domain.png
[Image2]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies/cluster-required-placement-domain.png
[Image3]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies/cluster-preferred-primary-domain.png
