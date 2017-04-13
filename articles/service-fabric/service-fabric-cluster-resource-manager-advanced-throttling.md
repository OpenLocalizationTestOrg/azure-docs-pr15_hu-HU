<properties
   pageTitle="A szolgáltatás háló felülete erőforrás-szabályozás |} Microsoft Azure"
   description="Ismerkedjen meg a szolgáltatás háló fürt erőforrás-kezelő által biztosított a szabályozás konfigurálása."
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


# <a name="throttling-the-behavior-of-the-service-fabric-cluster-resource-manager"></a>A vezérlő viselkedése, a szolgáltatás háló fürt erőforrás-kezelő szabályozása
Még ha megfelelően, hogy az erőforrás-kezelő fürt beállított, a fürt is első megszakadását. Ha például lehet egyidejű csomópont vagy hibafa tartomány hibák – mi lenne fordulhat elő, ha a frissítés során bekövetkezett? Az erőforrás-kezelő megpróbálja a lehető legjobb javítás mindent, de az időpontok jelennek meg, hogy a fürthöz a névjegyadatok stabilizálásához, fontolja meg egy backstop érdemes (hamarosan vissza tegye halad csomópontokat a hálózati feltételek javítandó maguk, korrigált bittel üzemelnek). Az alábbi helyzetek rendezése érdekében a szolgáltatás háló fürt erőforrás-kezelő több szabályozás tartalmazza. Jegyzet, hogy ezek szabályozás meglehetősen zavaró, általában kerülni a Webhelyfiókok használható, ha volt néhány ügyeljen köré, hogy valójában lehet végrehajtani a fürt, valamint a gyakori párhuzamos munkamennyiség kész matematikai kell reagálni ezek rendezi a (ahem) tervezett makroszkopikus konfigurálás események (AKA: "Nagyon hibás nap").

Általánosságban elmondható javasoljuk, egyéb beállításokat (például a normál kód frissítések, és nem vész el, a fürt rajta overscheduling) keresztül nagyon hibás nap elkerülése a fürt megakadályozhatja a javítás magát közben az erőforrásokat használó szabályozásának helyett). A szabályozás van alapértelmezett értéke, által talált keresztül kell ok alapértelmezett funkcióit, de valószínűleg kell nézze meg, és állítson be őket a várt tényleges betöltés. Miközben nem túl korlátozza vagy betöltése a fürt meghatározhatják, hogy a legjobb esetek, amelyek (mindaddig, amíg a orvosolja azokat) hol kell néhány szabályozás már a helyükön, még akkor is, ha azt jelenti, hogy a fürt hosszabb ideig fog tartani stabilizálásához.

##<a name="configuring-the-throttles"></a>A szabályozás konfigurálása
A szabályozás, amely alapértelmezés szerint látható a következők:

-   GlobalMovementThrottleThreshold – ez szabályozza a fürt mozgását száma az egyes idő (GlobalMovementThrottleCountingInterval, a másodpercben megadott érték szerint megadva)
-   MovementPerPartitionThrottleThreshold – ez vezérlőket bármely szolgáltatás partíciót mozgását száma fölé egy kis időt (a MovementPerPartitionThrottleCountingInterval, érték másodperc)

``` xml
<Section Name="PlacementAndLoadBalancing">
     <Parameter Name="GlobalMovementThrottleThreshold" Value="1000" />
     <Parameter Name="GlobalMovementThrottleCountingInterval" Value="600" />
     <Parameter Name="MovementPerPartitionThrottleThreshold" Value="50" />
     <Parameter Name="MovementPerPartitionThrottleCountingInterval" Value="600" />
</Section>
```

Ügyeljen arra, hogy azt láthatta, használja az alábbi szabályozás már ügyfelek legtöbbször mert nem voltak már korlátozott erőforrás környezetben (például korlátozott hálózati sávszélesség egyes csomópontok vagy a lemezt, amelynek felfelé párhuzamos replika követelményeinek nem hoz létre, azokat meg lettek helyezés), hogy ezek a műveletek azt jelenti, amely nem sikerül vagy mégis lenne lassú.  Ezekben az esetekben az ügyfelek voltak kényelmes arra, hogy azok volt esetleg bővítése stabil állapot, köztük azokat is, üzembe helyezéséhez alsó általános megbízhatóságát a közben ezek azok szabályozott végződő ismerete elérje a fürt időbe idő mennyiségét.

## <a name="next-steps"></a>Következő lépések
- Megtudhatja, hogyan fürt az erőforrás-kezelő kezeli, és a fürt betöltés egyenlege, olvassa el a cikk a [terheléselosztás betöltése](service-fabric-cluster-resource-manager-balancing.md)
- Az erőforrás-kezelő fürt túl sok a fürt leíró lehetőséget. Megtudhatja, hogy több róluk nézze meg a [szolgáltatás háló fürt ismertető](service-fabric-cluster-resource-manager-cluster-description.md) cikkben
