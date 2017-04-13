<properties
   pageTitle="Azure Service háló vészhelyreállítás |} Microsoft Azure"
   description="Azure Service háló katasztrófák diagramtípusokat foglalkozni a lehetőségeket kínál. Ez a cikk ismerteti a katasztrófák, amely akkor fordulhat elő, és hogyan kell kezelni az őket."
   services="service-fabric"
   documentationCenter=".net"
   authors="seanmck"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/10/2016"
   ms.author="seanmck"/>

# <a name="disaster-recovery-in-azure-service-fabric"></a>Az Azure Service háló vészhelyreállítás

A magas rendelkezésre állás felhő alkalmazások előadásához kritikus része annak ellenőrzése, hogy a hibák, beleértve azokat, amelyek nem tagjai a vezérlőelem különböző típusú pusztítani azt. Ez a cikk ismerteti a potenciális katasztrófák környezetében Azure Service háló fürt fizikai elrendezését és útmutatást a hogyan szeretné kezelni az ilyen katasztrófák korlátozni vagy legrövidebb leállás vagy adatvesztés okozzanak.

## <a name="physical-layout-of-service-fabric-clusters-in-azure"></a>Fizikai elrendezését szolgáltatás háló fürtök Azure-ban

Különböző típusú hibák miatti kockázat megértéséhez, akkor hasznos lehet ahhoz, hogy hogyan fürt fizikailag elrendezésének Azure-ban.

A szolgáltatás háló fürtre Azure-ban létrehozott, válasszon egy területet, ahol fog tárolni is szükség van. Az Azure infrastruktúra majd kiépítése az erőforrások fürtre belül a régió, mégpedig a virtuális gépeken futó (VMs) kért számát. Nézzük meg közelebbről hol és hogyan lehet ilyen VMs kiépített környezetet nyújt.

### <a name="fault-domains"></a>Hibafa-tartományok

Alapértelmezés szerint a VMs a fürt egyenletesen helyezkednek logikai csoportok neve hibafa tartományok (FDs), amely oszthatja fel a lehetséges hibák a host hardver alapján gépek között. Kifejezetten Ha két VMs két különböző FDs tárolnak, biztos lehet, hogy ne ugyanazt az adott power forráslista vagy a hálózati kapcsolót. Emiatt a helyi hálózaton vagy egy virtuális érintő áramszünet nem érinti a a másik, lehetővé téve a munka-terhelés a nem válaszoló gép belül a fürt szolgáltatás háló.

Az elrendezés, a fürt a nyújtott [szolgáltatás háló](service-fabric-visualizing-your-cluster.md)Explorer fürt térképpel hibafa tartományokban is ábrázolása:

![Húzza szét a szolgáltatás háló Explorer hiba tartományokban csomópontok][sfx-cluster-map]

>[AZURE.NOTE] A többi tengely a fürt térkép logikailag csoportosíthatja a tevékenységek a tervezett karbantartásokkal kapcsolatos információk alapján csomópontok frissítési tartományok jeleníti meg. Az Azure Service háló fürt mindig elrendezésének öt frissítési tartományok.

### <a name="geographic-distribution"></a>Földrajzi ki.

Jelenleg [világszerte 26 Azure régiók][azure-regions], több további jelenti be. Egy adott terület egy vagy több fizikai adatközpontokban attól függően, hogy az igény szerinti és alkalmas helyekre, többek között az elérhető is tartalmazhat. Ne feledje, még akkor is, több fizikai adatközpontokban tartalmazó régiók, az sem garantálja, hogy a fürt VMs egyenletesen elosztva fizikai helyekre keresztül. Valójában jelenleg az egy adott fürt összes VMs kiépített fizikai webhely belül.

## <a name="dealing-with-failures"></a>Hibák kezelése

Többféle a hibák, amelyek befolyásolják a fürt, a saját kezelési mindegyiket. Fog megnézi az őket valószínűsége fennáll sorrendjét.

### <a name="individual-machine-failures"></a>Egyes gépi hibák

Említett, egyes gépi hibák, a virtuális belül vagy a hardver vagy Szoftverüzemeltetési ezt a hibát tartományban veszélyt nincs önállóan. Szolgáltatás háló általában képes észlelni a hibát másodpercek, és válaszoljon a fürt állapotának megfelelően alapján. Például a csomópontra a elsődleges replikák olyan partíciót volt szolgáltatónál, ha egy új elsődleges kijelölt a másodlagos partícióreplikáinak. Azure ezt a lehetőséget választva a sikertelen gép vissza, ha azt fog automatikusan újracsatlakozás a fürt, és tanulmányozza a terhelést a részesedése még egyszer.

### <a name="multiple-concurrent-machine-failures"></a>Több egyidejű gépi hibák

Hibafa-tartományok jelentősen csökkenti a kockázat egyidejű gépi hibák, miközben mindig legyen az esetleges egyszerre több gépek fürt leállíthatja több véletlen hibák.

Az általános mindaddig, amíg a csomópontok többsége elérhetők maradnak, a fürt továbbra is működnek, jóllehet az alsó kapacitás állapot-nyilvántartó kópiák első csomagolt gépek kevesebb beállítási be, és kevesebb állapot nélküli példányok betöltés széttáró érhetők el.

#### <a name="quorum-loss"></a>Kvórumvesztést

Az állapot-nyilvántartó szolgáltatás partíciót a replikák többsége nyissa, ha adott partíciót állapotba egy úgynevezett "kvórumvesztést." Ezen a ponton a szolgáltatás háló leállítja a lehetővé teszi, hogy annak biztosítására, hogy állapotába marad konzisztens és megbízható partíciót ír. Gyakorlatilag azt mondja hogy kiválasztása időtartamra elérhetetlenség annak érdekében, hogy ügyfelek nem rendszergazda, hogy azok az adatok mentette valójában volt nem fogadja el. Figyelje meg, ha azt választotta, hogy az olvasás engedélyezi a másodlagos kópiák állapot-nyilvántartó szolgáltatás, továbbra is azokat, olvassa el az Ez az állapot műveletek elvégzéséhez. Egy partíciót kvórumvesztést mappában marad, amíg a megfelelő számú kópiák, térjen vissza, illetve mindaddig, amíg a csoport rendszergazdája kezd a rendszer helyezze a [Javítás-ServiceFabricPartition API][repair-partition-ps].

>[AZURE.WARNING] A javítási művelet végrehajtása, miközben az elsődleges replika nem működik az adatok elvesztését eredményezi.

Rendszer-szolgáltatások is adatmennyiség problémákat okozhat kvórumvesztést, az ütközés a szóban forgó szolgáltatással együtt. Például a elnevezési szolgáltatásban kvórumvesztést hatással lesz névfeloldás, mivel a Feladatátvevő kezelő szolgáltatás kvórumvesztést blokkolja a új szolgáltatás létrehozása és feladatátadás. Ne feledje, hogy nem kedvelik-e a saját szolgáltatások kísérel meg javítása a rendszer *nem* ajánlott. Célszerű Ehelyett egyszerűen várja meg, amíg a lefelé replikák vissza.

#### <a name="minimizing-the-risk-of-quorum-loss"></a>A kockázat kvórumvesztést a kis méretre

A szolgáltatás a cél replika beállítása méretének növelésével kis méretűre a kvórumvesztést kockázata. Célszerű kópiák számának érdemes van szüksége, miután a fennmaradó az írások érhető el, miközben megőrzési figyelmét arra, hogy az alkalmazás elviseli címen érhető el csomópontok számának értelmez és fürt frissítések is, hogy csomópontok átmenetileg nem érhető el, nemcsak a hardver hibák.

Vegye figyelembe az alábbi példák feltételezve, hogy beállított-e, hogy egy MinReplicaSetSize három, az ajánlott gyártási szolgáltatások a legkisebb számtól a szolgáltatások. Egy három TargetReplicaSetSize az (egy elsődleges és két formátumú másodlagos zónák), (két kópia lefelé) frissítésének hardver sikertelensége kvórumvesztést eredményez, és a szolgáltatás csak olvasható válik. Azt is megteheti öt kópiák birtokában lenne is állnia (lefelé három kópiája) a frissítés során két hibák, mint a fennmaradó két kópia is továbbra is kvórumot a minimális replikakészleten belül.

### <a name="data-center-outages-or-destruction"></a>Adatok központ kimaradások vagy megsemmisítését

Ritkán átmenetileg nem érhető el, kiemelt vagy a hálózati kapcsolat elvesztése miatt válhat fizikai adatközpontokban. Ebben az esetben a szolgáltatás háló fürt és alkalmazások hasonlóan nem érhető el, de az adatok megőrződnek. Az Azure-ban futó fürt, kimaradások [Azure állapota lapon]megtekintheti a frissítések[azure-status-dashboard].

Az, hogy a teljes fizikai adatközpont megsemmisül erősen valószínű eseményhez bármely szolgáltatás háló fürt ott tárolt elvesznek, állapotukban együtt.

És vírusvédelmet ezt a lehetőséget, különösen fontos rendszeres [biztonsági másolat a állapotát](service-fabric-reliable-services-backup-restore.md) geo felesleges tárolóval, és biztosítani, hogy van-e érvényesítése az azt jelenti, hogy állítsa vissza. Milyen gyakran készítsen biztonsági másolatot a lesz a helyreállítási pont cél (Készletben.) függ. Akkor is, ha Ön nem teljes mértékben végrehajtotta biztonsági mentési és visszaállítási még, végre kell hajtania kezelőjét a `OnDataLoss` esemény, hogy akkor fordul elő, mint amikor naplózható követi:

```c#
protected virtual Task<bool> OnDataLoss(CancellationToken cancellationToken)
{
  ServiceEventSource.Current.ServiceMessage(this, "OnDataLoss event received.");
  return Task.FromResult(true);
}
```


### <a name="software-failures-and-other-sources-of-data-loss"></a>Szoftver hibák és az adatok elvesztését a más forrásokból

Az adatok elvesztését oka a szolgáltatások, az emberi működési hibákat és biztonsági szabályok megsértésével kód hibák általánosabb, mint kiterjedt adatok center sikertelen. Jó helyen jár, minden esetben a helyreállítási stratégia megegyezik a: az összes állapot-nyilvántartó szolgáltatás rendszeres biztonsági mentés készíthet, és gyakorolni az Ön számára, hogy az állapot visszaállításához.

## <a name="next-steps"></a>Következő lépések

- Megtudhatja, hogy miként hasonlóan az [tesztelhetőségi keretrendszer](service-fabric-testability-overview.md) használatával különböző hibák
- Olvassa el az egyéb katasztrófaelhárító és a Max-elérhetőség erőforrások. A Microsoft tett közzé az alábbi témakörök a nagy mennyiségű útmutatást. Miközben az egyes ezeket a dokumentumokat a többi terméktől használatra adott technikákat hivatkozik, sok általános gyakorlati tanácsok a szolgáltatás háló környezetben is alkalmazhat tartalmaznak:
 - [Elérhetőség ellenőrzőlista](../best-practices-availability-checklist.md)
 - [Egy katasztrófa helyreállítási részletező végrehajtása](../sql-database/sql-database-disaster-recovery-drills.md)
 - [Vészhelyreállítás és Azure-alkalmazásokhoz magas elérhetősége][dr-ha-guide]


<!-- External links -->

[repair-partition-ps]: https://msdn.microsoft.com/library/mt163522.aspx
[azure-status-dashboard]:https://azure.microsoft.com/status/
[azure-regions]: https://azure.microsoft.com/regions/
[dr-ha-guide]: https://msdn.microsoft.com/library/azure/dn251004.aspx


<!-- Images -->

[sfx-cluster-map]: ./media/service-fabric-disaster-recovery/sfx-clustermap.png
