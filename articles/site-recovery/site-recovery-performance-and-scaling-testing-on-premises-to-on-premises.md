<properties
    pageTitle="Teljesítmény tesztelése és méretezése eredmények helyszíni webhely helyreállítási helyszíni a Hyper-V replikáció |} Microsoft Azure"
    description="Ez a cikk a helyszíni Azure webhely helyreállítás segítségével a helyszíni replikációs vizsgálata teljesítmény információt tartalmaz."
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="jwhit"
    editor="tysonn"/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="07/06/2016"
    ms.author="raynew"/>

# <a name="performance-test-and-scale-results-for-on-premises-to-on-premises-hyper-v-replication-with-site-recovery"></a>Teljesítmény tesztelése és méretezése a helyszíni telepítésű helyszíni a Hyper-V replikációs webhely helyreállítási az eredmények

Microsoft Azure webhely helyreállítási téve, illetve kezelhet replikációs virtuális gépeken futó és Azure, illetve egy másodlagos adatközponthoz fizikai kiszolgálók is használhatja. Ebben a cikkben a teljesítmény tesztelése azt megváltozott, amikor a Hyper-V virtuális gépeken futó replikálása két között a helyszíni adatközpontokkal eredményét.



## <a name="overview"></a>– Áttekintés

A tesztelés cél volt, hogy vizsgálja meg, hogyan Azure webhely helyreállítási hajt végre, állandó replikáció során. Állandó replikációs fordul elő, ha a virtuális gépeken futó kezdeti replikációs befejeződött, és delta módosítások szinkronizálása. Fontos mérésére teljesítmény állandó használ, mert az állapot, amelyben a legtöbb virtuális gépeken futó marad, kivéve, ha a váratlan kimaradások fordul elő.


A próba-példányban áll, amelyek két helyszíni webhelyek minden helyen VMM-kiszolgálóval. A próba-telepítés fellépő az elsődleges webhely és a másodlagos vagy helyreállítási helyként az irodában székhelye egy központi office/ág office telepítési jellemző lesz.

### <a name="what-we-did"></a>Azt a művelet

Az alábbiakban mi azt a próba felelt meg:

1. Virtuális gépeken futó VMM sablonok használatával létrehozott.

1. Virtuális gépeken futó és a rögzítés eredeti teljesítménymutatók 12 óra feletti lépések.

1. Létrehozott felhőket elsődleges és helyreállítási VMM kiszolgálókon.

1. Azure webhely helyreállítási, beleértve a forrás- és helyreállítási felhőket megfeleltetésének konfigurált felhő védelmet.

1. Engedélyezve van a virtuális gépeken futó védelmét, és engedélyezze az első teljes replikációs.

1. Néhány órát rendszer stabilizációjának megvárta.

1. Rögzített teljesítménymutatók 12 óra feletti biztosítva, hogy az összes virtuális gépeken futó tartózkodott az várható replikációs állapotú-e a 12 órával.

1. Mérje le a delta az eredeti teljesítménymutatók és a replikáció teljesítménymutatók között.

## <a name="test-deployment-results"></a>Telepítési teszteredményei

### <a name="primary-server-performance"></a>Elsődleges kiszolgáló teljesítménye

- A Hyper-V replika aszinkron állapotváltozásait egy naplófájlban, a minimális tárolási terhelést az elsődleges kiszolgálón.

- A Hyper-V replika önálló karbantartott gyorsítótárban IOPS felsőbb lekicsinyítheti a nyomon követés céljából használja. Tárolja a memóriában VHDX ír, és az idő küldött a napló a helyreállítási webhely előtt be a naplófájl ürítése őket. Síkban lemezen is akkor történik, ha az írások találati előre meghatározott korlátozott.

- Az alábbi grafikonon az állandó IOPS terhelést a replikáció. Győződjön meg arról, hogy a replikáció miatt felsőbb IOPS körülbelül 5 %, amelyben igazán alacsony is láthatja.

![Elsődleges eredménye](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744913.png)

A Hyper-V replika használja a memória optimalizálása a lemez teljesítménye az elsődleges kiszolgálón. Ahogy a következő ábra, beleszámítva a elsődleges fürt az összes kiszolgálón a memória állás. A memória felsőbb látható replikációs és összehasonlítása az összes telepített memória a Hyper-V-kiszolgáló által használt memória százaléka.

![Elsődleges eredménye](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744914.png)

A Hyper-V replika minimális Processzor terhelést tartalmaz. Ahogy a diagramot, replikációs terhelést van, a 2-3 %-os tartományban.

![Elsődleges eredménye](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744915.png)

### <a name="secondary-recovery-server-performance"></a>Másodlagos (helyreállítási) kiszolgáló teljesítménye

A Hyper-V replika memória kis mennyiségű helyreállítási kiszolgálói tárhely műveletek száma optimalizálása használja. A diagram összefoglalja a memóriahasználat, a helyreállítási kiszolgálón. A memória felsőbb látható replikációs és összehasonlítása az összes telepített memória a Hyper-V-kiszolgáló által használt memória százaléka.

![Másodlagos eredménye](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744916.png)

A bemeneti és kimeneti műveletet a helyreállítási webhelyen összeg függvény a szám írási műveletek az elsődleges webhelyen. Vegyük tekintse meg a teljes perifériaműveletek a helyreállítási webhelyen, a teljes bemeneti és kimeneti műveletet leíró, és írja be az elsődleges webhely műveleteket. A diagramok mutatják, hogy az összes IOPS a helyreállítási webhelyen

- Körül 1,5 időpontok az írási IOPS az elsődleges.

- Az elsődleges webhelyen a teljes IOPS körülbelül 37 %-át.

![Másodlagos eredménye](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744917.png)

![Másodlagos eredménye](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744918.png)

### <a name="effect-of-replication-on-network-utilization"></a>A hálózati kihasználtsági replikációs hatását

Hálózati sávszélesség másodpercenként 275 MB-os átlagra ellen 5 GB / szekundum meglévő sávszélesség használta az elsődleges és helyreállítási csomópontok (a tömörítés engedélyezése) között.

![Eredmények hálózati kihasználtság](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744919.png)

### <a name="effect-of-replication-on-virtual-machine-performance"></a>Replikációs virtuális gép teljesítményre gyakorolt hatásának

Fontos tényező a virtuális gépeken futó termelési feladatok a replikáció hatása. Ha az elsődleges webhely megfelelően a replikáció már kiépítve, ne létezhetnek bármely hatása a feladatok a. Mechanizmusa nyomon követése a Hyper-V replika lightweight biztosítja, hogy a virtuális gépeken futó futó feladatok nem érintik munkafolyamat állandó replikáció során. A következő diagramokban ezen eset.

Ez a diagram IOPS elvégzett virtuális gépeken futó futtatása előtt különböző munkaterhelésekből és replikációs futtatásának engedélyezése után ábrázolja. Láthatja, hogy nincs-e a kettő közötti különbség.

![Replika effektus eredménye](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744920.png)

A következő grafikonon virtuális gépeken futó futtatása előtt különböző munkaterhelésekből és replikációs futtatásának engedélyezése után a kapacitása. Láthatja, hogy a replikáció még nincs jelentős hatást.

![Eredmények replika effektusok](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744921.png)

### <a name="conclusion"></a>Elfogadásáról

A jól eredmény, hogy a Hyper-V replikával párosított Azure webhely-helyreállítás mellett legalább egy nagy fürt esetén méretezze át.  Azure webhely helyreállítási tartalmaz egyszerű telepítési, replikációs, kezelése és figyelése. A Hyper-V replika szükséges infrastruktúrát biztosít a sikeres replikációs méretezését. Az optimális bevezetését tervezi a ajánlott a [Hyper-V replika kapacitás Csapattervező](https://www.microsoft.com/download/details.aspx?id=39057)letöltését.

## <a name="test-environment-details"></a>Teszt környezet részletei

### <a name="primary-site"></a>Elsődleges hely

- Az elsődleges webhely öt a Hyper-V kiszolgálón a 470 virtuális gépeken futó tartalmazó fürt tartalmaz.

- A virtuális gépeken futó futtassa a különböző munkaterhelésekből, és minden rendelkezik Azure webhely helyreállítási védelem engedélyezve van.

- Csomópont tárterület iSCSI SAN által biztosított. Minta – Hitachi HUS130.

- Minden fürt kiszolgáló négy hálózati kártya (NIC) az egyik GB/s minden tartalmaz.

- Két a hálózati kártyák egy iSCSI magánjellegű hálózathoz kapcsolódik, és két külső vállalati hálózatra csatlakozik. A külső hálózatok közül csak a fürt kommunikáció van fenntartva.

![Elsődleges hardverkövetelményei](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744922.png)

|Kiszolgáló|RAM|Modell|Processzor|Processzorok száma|HÁLÓZATI KÁRTYA|Szoftver|
|---|---|---|---|---|---|---|
|A Hyper-V kiszolgálók fürt: <br />ESTLAB-HOST11<br />ESTLAB-HOST12<br />ESTLAB-HOST13<br />ESTLAB-HOST14<br />ESTLAB-HOST25|128ESTLAB-HOST25 van 256|Dell™ PowerEdge™ R820|Intel(R) Xeon(R) Processzor E5-4620 0 @ 2,20 GHz-es|4|E GB/s x 4|A Windows Server adatközponthoz 2012 R2 (x64) + Hyper-V szerepkör|
|VMM kiszolgáló|2|||2|1 GB/s|A Windows Server-adatbázis 2012 R2 (x 64) + VMM 2012 R2|

### <a name="secondary-recovery-site"></a>Másodlagos (helyreállítási) webhely

- A másodlagos webhely hat-csomópont Feladatátvevőfürt rendelkezik.

- Csomópont tárterület iSCSI SAN által biztosított. Minta – Hitachi HUS130.

![Elsődleges hardver meghatározása](./media/site-recovery-performance-and-scaling-testing-on-premises-to-on-premises/IC744923.png)

|Kiszolgáló|RAM|Modell|Processzor|Processzorok száma|HÁLÓZATI KÁRTYA|Szoftver|
|---|---|---|---|---|---|---|
|A Hyper-V kiszolgálók fürt: <br />ESTLAB-HOST07<br />ESTLAB-HOST08<br />ESTLAB-HOST09<br />ESTLAB-HOST10|96|Dell™ PowerEdge™ R720|Intel(R) Xeon(R) Processzor E5-2630 0 @ 2.30-as GHz-es|2|E GB/s x 4|A Windows Server adatközponthoz 2012 R2 (x64) + Hyper-V szerepkör|
|ESTLAB-HOST17|128|Dell™ PowerEdge™ R820|Intel(R) Xeon(R) Processzor E5-4620 0 @ 2,20 GHz-es|4||A Windows Server adatközponthoz 2012 R2 (x64) + Hyper-V szerepkör|
|ESTLAB-HOST24|256|Dell™ PowerEdge™ R820|Intel(R) Xeon(R) Processzor E5-4620 0 @ 2,20 GHz-es|2||A Windows Server adatközponthoz 2012 R2 (x64) + Hyper-V szerepkör|
|VMM kiszolgáló|2|||2|1 GB/s|A Windows Server-adatbázis 2012 R2 (x 64) + VMM 2012 R2|

### <a name="server-workloads"></a>A kiszolgálói feladatok

- Tesztelése céljából azt kiválasztott munkaterhelésekből gyakran használt vállalati ügyfél környezetben.

- A táblázatban az szimulációk terhelést jellemző [IOMeter](http://www.iometer.org) azt használni.

- Az összes IOMeter profilok írni, hasonlóan a legrosszabb véletlen bájt írása mintájának megadása munkaterhelésekből vannak beállítva.

|Terhelést|I/O méret (KB)|Az access %|Olvasás %|Függőben lévő műveletekkel együtt|I/O mintával|
|---|---|---|---|---|---|
|Fájlkiszolgálóra|48163264|60 20 %5 %5 % 10 %|80 %-os 80 %-os 80 %-os 80 %-os 80 %-os|88888-as|Az összes 100 %-os véletlen|
|Az SQL Server (1 mennyiségi) az SQL Server (mennyiségi 2)|864|100 %-os 100 %-os|70 % -os 0 %|88|100 %-os random100 % egymás után következő|
|Exchange|32|100 %-os|67 %|8|100 %-os véletlen|
|Munkaállomás/VDI környezetben|464|66 %-kal 34 %-kal|70 %-os 95 %|11|Mindkét 100 %-os véletlen|
|Fájl webkiszolgálón|4864|33 % 34 % 33 %-kal|95 % 95 %-os 95 %|888|Véletlen 75 %|

### <a name="virtual-machine-configuration"></a>Virtuális gép konfigurálása

- 470 virtuális gépeken futó az elsődleges fürt.

- Az összes virtuális gépeken futó VHDX lemezzel.

- Virtuális gépeken futó futó feladatok a táblázatban. Az összes létrehozott VMM sablonokkal.

|Terhelést|# VMs|Minimális RAM (GB)|Maximális RAM (GB)|Virtuális logikai lemez hüvelyk (GB)|Maximális IOPS|
|---|---|---|---|---|---|
|Az SQL Server|51|1|4|167|10|
|Az Exchange Server|71|1|4|552|10|
|Fájlkiszolgálóra|50|1|2|552|22-es hibakód|
|VDI KÖRNYEZETBEN|149|.5|1|80|6|
|Webkiszolgáló|149|.5|1|80|6|
|ÖSSZESEN|470|||96.83 TB|4108|

### <a name="azure-site-recovery-settings"></a>Azure webhely helyreállítási beállításai

- Azure webhely helyreállítási konfigurált helyszíni telepítésű helyszíni védelem

- VMM kiszolgáló van, amelyben a Hyper-V fürt kiszolgálók és a virtuális gépeken futó beállítva, négy felhőket.

|Elsődleges VMM felhő|Védett virtuális gépeken futó a felhőben|Replikációs gyakorisága|További helyreállítási pontok|
|---|---|---|---|
|PrimaryCloudRpo15m|142|15 perc|Nincs lehetőség|
|PrimaryCloudRpo30s|47|30 másodperc|Nincs lehetőség|
|PrimaryCloudRpo30sArp1|47|30 másodperc|1|
|PrimaryCloudRpo5m|235|5 perc|Nincs lehetőség|

### <a name="performance-metrics"></a>Teljesítménymutatók

A táblázat összefoglalja a teljesítménymutatók és a számláló, amely a példányban mért volt.

|Metrikus|A számláló|
|---|---|
|PROCESSZOR|\Processor(_Total)\% processzor|
|Rendelkezésre álló memória|\Memory\Available megabájt|
|IOPS|\PhysicalDisk (_Total) \Disk átvitelek/sec|
|Virtuális további műveletek/sec (IOPS)|Virtuális tárolóeszközre \Hyper-V (<VHD>) \Read műveletek/Sec|
|Műveletek/sec virtuális írási (IOPS)|Virtuális tárolóeszközre \Hyper-V (<VHD>) \Write műveletek/S|
|Virtuális, olvassa el az átviteli|Virtuális tárolóeszközre \Hyper-V (<VHD>) \Read bájt/sec|
|Virtuális írási átviteli|Virtuális tárolóeszközre \Hyper-V (<VHD>) \Write bájt/sec|


## <a name="next-steps"></a>Következő lépések

- [Két helyszíni VMM webhelyek között védelem beállítása](site-recovery-vmm-to-vmm.md)
