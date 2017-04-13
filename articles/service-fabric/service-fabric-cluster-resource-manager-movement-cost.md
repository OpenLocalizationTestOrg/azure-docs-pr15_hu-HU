<properties
   pageTitle="Szolgáltatás háló fürt erőforrás-kezelő: mozgását költség |} Microsoft Azure"
   description="Mozgás költség szolgáltatás háló szolgáltatások áttekintése"
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

# <a name="service-movement-cost-for-influencing-cluster-resource-manager-choices"></a>Mozgás-szolgáltatás költségét befolyásoló fürt erőforrás-kezelő választási lehetőségek
Fontos tényező is célszerű figyelembe venni, amikor meghatározzák, hogy mi az, hogy a fürt változásokat szeretné, és megoldást érték a teljes költsége elérése érdekében, hogy a megoldás.

Áthelyezés szolgáltatás példányok vagy kópiák költségek Processzor időt és a hálózati sávszélesség legalább. Az állapot-nyilvántartó szolgáltatások azt is mennyibe kerül a távolságot a lemezen, létre kell hoznia az állapot másolatát a régi replikák leállítása előtt. Világosan kívánt csökkentése érdekében, hogy az Azure Service háló fürt erőforrás-kezelő felfelé az megoldások a költség. De még nem kívánt figyelmen megoldásokat, amelyek jelentősen javíthatják az erőforrások lefoglalása, a fürt.

Erőforrás-kezelő fürt kétféleképpen költségeket számítógépes és korlátozása őket, próbálja meg megfelelően a más céljait a fürt kezelése közben is tartalmaz. Az első, hogy tervezésekor felülete erőforrás van a fürt új elrendezés, azt számolja meg minden áthelyezés, hogy ez tenné. Egy egyszerű lehetőséget választja Ha két megoldása nagyjából teljes egyenleg (eredmény) végén, majd készítsen azt, amelyik a legalacsonyabb költség (a kurzor teljes számát).

Nagyjából jól működik. De az alapértelmezett vagy statikus terhelések valószínű, hogy a kurzor az összes egyenlők bármely összetett rendszerben. Néhány valószínűleg sokkal több drága.

## <a name="changing-a-replicas-move-cost-and-factors-to-consider"></a>A kópia áthelyezés költség és megfontolandó tényezők módosítása
Mint jelentéskészítéssel terhelés (az erőforrás felülete más szolgáltatás) adhat a szolgáltatást önálló jelentési hogyan költséges a szolgáltatást, ha az adott időpontban áthelyezése lehetőséget.

Kód:

```csharp
this.ServicePartition.ReportMoveCost(MoveCost.Medium);
```

MoveCost négyszintű: nulla, a Min, a közepes és nagy. Az alábbiakban egymáshoz viszonyított kivételével nulla. Nulla azt jelenti, hogy áthelyezése kópia ingyenes, és meg nem számítanak bele a megoldás lévő pontszámhoz. Az áthelyezés költség beállítás magas *nem* biztosítja, hogy a replika nem áthelyezni, közvetlenül az, hogy nem kerülnek, hacsak egy jó ok.

![Költség lépés tényező mozgását kópiák kijelölése][Image1]

MoveCost segít, hogy a teljes zavartalanul okozhat, amelyek a legegyszerűbb, miközben továbbra is megérkezni az egyenértékű egyenleg eléréséhez megoldások megkeresésére. A szolgáltatás állomástól költség viszonyított számos dolgot lehet. A leggyakoribb tényezők az áthelyezés költség kiszámítása a következők:

- Állapot vagy a szolgáltatás áthelyezése tartalmazó adatok mennyiségét.
- Az ügyfelek leválasztó költségét. Elsődleges kópia helyezi át a költség általában magasabban másodlagos kópia helyezi át a költség jelenik meg.
- Egy repülés művelet megszakítása költségét. Néhány művelet, az adatok tárolására szint vagy egy ügyfél hívásba válaszként végrehajtott műveletek költséges. Egy bizonyos ponton után nem kívánt leállítása őket, ha nem rendelkezik. Így a tevékenység az időtartam, elrejtőzzünk be a költség annak valószínűsége, hogy a szolgáltatás kópiában vagy a példány helyezi csökkentése érdekében. A művelet befejezése után, visszatérhet a normál formában.

## <a name="next-steps"></a>Következő lépések
- Háló fürt erőforrás-kezelő szolgáltatás felhasználási és a fürt kapacitás kezelése mértékek használja. Többet szeretne tudni a mértékek és konfigurálásáról őket, szinonimaszótárral [kezelése erőforrás-felhasználás és a mértékek szolgáltatás háló betöltése](service-fabric-cluster-resource-manager-metrics.md).
- Megtudhatja, hogyan fürt az erőforrás-kezelő kezeli, és a fürt betöltés egyenlege kivétele [a szolgáltatás háló fürt terheléselosztás](service-fabric-cluster-resource-manager-balancing.md).

[Image1]:./media/service-fabric-cluster-resource-manager-movement-cost/service-most-cost-example.png
