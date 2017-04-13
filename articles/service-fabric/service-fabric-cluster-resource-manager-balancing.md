<properties
   pageTitle="A fürt együtt az Azure Service háló fürt erőforrás-kezelő terheléselosztás |} Microsoft Azure"
   description="A szolgáltatás háló fürt erőforrás-kezelő használatával fürt terheléselosztás bemutatása."
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

# <a name="balancing-your-service-fabric-cluster"></a>A szolgáltatás háló fürt terheléselosztás
A szolgáltatás háló fürt erőforrás-kezelő lehetővé teszi, hogy dinamikus terhelés jelentések, a módosításokat a fürt reagálni, a kényszer megsértése javítása és a szakismeretekre a fürt, ha szükséges. De gyakoriságának tartalmaz, végezze el az alábbi tényezőket, és mi elindította? Vannak olyan több vezérlők a kapcsolódó.

Az első készlet vezérlők terheléselosztás körül olyan időzítő csoportja. Ezeket az időzítő szabályozzák, hogy milyen gyakran a fürt az erőforrás-kezelő megvizsgálja a fürt dolgot kell foglalkozni az állapotát. Munka, a saját megfelelő időzítő mindegyiket három különböző kategóriába sorolhatók. Ezek a következők:

1.  Elhelyezési – ebben a szakaszban foglalkozik úgy, hogy a bármely állapot-nyilvántartó kópiák vagy állapot nélküli példányok hiányoznak amelyeket. Ez bemutatja az új szolgáltatások és kezelési állapot-nyilvántartó kópiák vagy állapot nélküli példányok, amely nem sikerült, és szükség lehet az ismételt megadásukra. Törlése és kópiák vagy példányok húzással is itt kezeli.
2.  Kényszer ellenőrzi – ebben a szakaszban keres, és a különböző elhelyezése kényszereket (szabályok) rendszerében megsértése kijavítja. Példák a szabályok összetevőjét, például a biztosítása, hogy csomópontok nem kapacitás fölé, hogy teljesülnek-e a szolgáltatás elhelyezése korlátozásokkal (További tudnivalók a következő későbbi).
3.  Nagyfokú – ebben a szakaszban ellenőrzi a megelőző szakismeretekre szükség a különböző mértékek egyenleggel konfigurált kívánt szintjét alapján, és ha igen megtekintéséhez megpróbálja megkeresni elrendezés a további kiegyensúlyozott fürt.

## <a name="configuring-cluster-resource-manager-steps-and-timers"></a>Erőforrás-kezelő fürt lépéseket és időzítő konfigurálása
A gyakoriság szabályozó különböző időzítőt minden ezek különböző típusú fel, hogy az erőforrás-kezelő fürt javítások szabályozza. Így például ha csak szeretné kezelni az új szolgáltatás munkaterhelésekből elhelyezése a fürt minden óra (a köteg be őket), de a kívánt minden néhány másodpercet normál terheléselosztás ellenőrzi, beállíthatja, hogy a működését. Minden egyes időzítő akkor indul el, ha a tevékenység ütemezése. Alapértelmezés szerint az erőforrás-kezelő állapotába megvizsgálja, és alkalmazza a frissítéseket (a az utolsó ellenőrzés, például észre, hogy egy csomópont nem működik óta történt módosítások kötegelés) minden 1/10 másodperc, készletek elhelyezését és kényszer ellenőrizze jelölők minden második, és 5 másodpercenként a terheléselosztás jelző.

ClusterManifest.xml:

``` xml
        <Section Name="PlacementAndLoadBalancing">
            <Parameter Name="PLBRefreshGap" Value="0.1" />
            <Parameter Name="MinPlacementInterval" Value="1.0" />
            <Parameter Name="MinConstraintCheckInterval" Value="1.0" />
            <Parameter Name="MinLoadBalancingInterval" Value="5.0" />
        </Section>
```

Ma azt csak akkor hajtsa végre az alábbi műveletek egyikét egyszerre, egymás után (ezért hivatkozunk őket mint "minimális időközönként")). Ez a, így például, hogy már válaszolt bármely várakozó kérelmek hozhat létre új kópiák, mielőtt azt lépjen tovább a fürt terheléselosztás. Amint látható által megadott alapértelmezett időintervallumra, azt a beolvasás és az összes szükség végezze el a gyakran, ami azt jelenti, hogy változások csoportja VÁLLALUNK lépésein végén jelölőnégyzet általában kisebb: azt nem fényképhez módosításokat a fürt órán keresztül, és próbálja kijavítani őket egyszerre, azt próbál dolog, amit kezelje a több vagy kevesebb mint amikor megtörténnek, de néhány kötegelés számos dolgot fordulhat elő amikor a egy időben. Emiatt a szolgáltatás háló erőforrás manager nagyon válaszol, amelyek akkor aktiválódnak elemeinek a fürt.

Ezek a legtöbb során feladatok egyszerűek (ha vannak olyan kényszer megsértése, fix őket, ha nincsenek szolgáltatások létre kell hozni, hozzon létre őket), az erőforrás-kezelő fürt is a néhány határozza meg, ha további információra van szüksége a imbalanced fürt. Az adott van még más konfigurációs két csatolt: *Küszöbértékek terheléselosztás* és a *Tevékenység küszöbértékek*.

## <a name="balancing-thresholds"></a>Nagyfokú küszöbérték
Nagyfokú küszöbértéket a fő vezérlő, a megelőző szakismeretekre kiváltó (ne feledje, hogy az időzítő csak az, hogy milyen gyakran az erőforrás-kezelő fürt ellenőriznie kell - nem jelenti, hogy semmit történik). A terheléselosztás küszöbértékét határozza meg, hogyan imbalanced a fürt kell lennie az egy adott mérőszám sorrendben esetében a fürt erőforrás-kezelő imbalanced és eseményindító terheléselosztás figyelembe.

Terheléselosztó küszöbértékek meghatározása fürt részeként metrikus alapon határozzák meg. Mértékek kivétel [Ez a cikk](service-fabric-cluster-resource-manager-metrics.md)további információt.

ClusterManifest.xml

``` xml
    <Section Name="MetricBalancingThresholds">
      <Parameter Name="MetricName1" Value="2"/>
      <Parameter Name="MetricName2" Value="3.5"/>
    </Section>
```

Egy mérőszám terheléselosztás küszöbértékét a megtartásával. Ha betöltés mennyiségét a legalább betöltött csomópontra osztva a legtöbb betöltött csomóponton betöltés összege meghaladja a száma, majd a fürt imbalanced számít terheléselosztás aktiválódik a következő alkalommal az erőforrás-kezelő fürt ellenőrzi, (Ez alapértelmezés szerint minden eddiginél 5 másodperc, amelyeket a MinLoadBalancingInterval fent látható).

![Nagyfokú küszöb példa][Image1]

Egyszerű ebben a példában az egyes szolgáltatásokhoz néhány metrikus egységnyi van felhasználás. A felső példában csomóponton legnagyobb terhelés az 5 és a minimális érték 2. Tegyük fel, hogy a mérőszám terheléselosztó küszöbértékét-e a 3. Emiatt a felső példában a fürt számít megfelel, és nincs terheléselosztás aktiválódik amikor ellenőrzi az erőforrás-kezelő fürt (mivel a fürt arány 5/2, = 2,5 és, amely kisebb, mint a megadott terheléselosztó küszöbértékét, 3).

Alsó példában, a max betöltés csomóponton áll 10, miközben a minimális érték, 2, így az 5 arány. A tervezett terheléselosztó küszöbértékét, 3, hogy mérőszám fölé a fürt ez helyezi. Emiatt a globális rebalancing Futtatás is ütemezett akkor indul el, a terheléselosztó időzítő. Pusztán azért, mert a terheléselosztó keresés van megrúgni nem olyan semmit, amely a Megjegyzés helyezi - előfordul, hogy a fürt imbalanced, de nem javítani a helyzet -, de ehhez hasonló helyzetben (legalább alapértelmezés szerint) néhány a betöltés majdnem biztosan juttatni 3. Figyelje meg, hogy mivel azt nem használja a greedy megközelítés néhány betöltés is juttatni Node2 óta csomópontok általános eltérések minimalizálásáról eredményezne, amely, de azt az elvárt, hogy a legtöbb a betöltés való 3 volna flow.

![Nagyfokú küszöb példa műveletek][Image2]

Jegyzet, hogy az első terheléselosztó küszöbérték alatt a nem explicit cél – terheléselosztás küszöbértékek csak az *eseményindító* , amely arra utasítja az erőforrás felülete, amely azt határozza meg a milyen javításokat teheti, ha vannak ilyenek a fürt be kell kinéznie.

## <a name="activity-thresholds"></a>Tevékenység küszöbérték
Időnként előfordulhat viszonylag imbalanced csomópontok ugyan *a fürt betöltés mennyiségét* halkítva. Lehet egyszerűen miatt időpontja, vagy mert a fürt új, csak keresztüli bootstrapped. Bármelyik lehetőséget választja előfordulhat, hogy nem szeretne időt a fürt terheléselosztás, mert nincs a ténylegesen nagyon kevés kell szerzett – akkor fogja csak kell kiadások hálózati és a számítási erőforrások áthelyezése dolog, amit köré, anélkül, hogy minden olyan különbség abszolút értéke. Ennek elkerülése érdekében szeretnénk, mert van egy másik vezérlőjére belül az erőforrás-kezelő, sok betöltés tevékenység küszöbértékek, amely lehetővé teszi, hogy bizonyos abszolút alsó határ tevékenység – Itt adhatja meg, ha nincs csomópont nem legalább ezt neve, majd a terheléselosztás nem aktiválódik még akkor is, ha a terheléselosztás küszöbértékét teljesül.

Példaként tegyük fel, hogy a csomópontok fogyasztási következő összesítéssel jelentések van. Tegyük fel is, hogy azt őrizni, a terheléselosztás küszöb 3 a mérőszám, de most egy tevékenység küszöbérték 1536 is van. Az első lehetőséget választja a fürt imbalanced, a terheléselosztás küszöbértékét per állapotában nincs csomópont megfelel-e minimális tevékenység küszöbérték, így azt az dolog, amit önállóan hagyja. Az alsó példában Node1 módja a tevékenység küszöbértékét keresztül, a terheléselosztás fog történni (mert túllépik a terheléselosztás küszöbértékét és a a tevékenység küszöbértékét a mérőszám)

![Példa a tevékenység küszöbérték][Image3]

Nagyfokú küszöbértékek hasonlóan a tevékenység küszöbértékek definiált per metrikus keresztül fürt meghatározása:

ClusterManifest.xml

``` xml
    <Section Name="MetricActivityThresholds">
      <Parameter Name="Memory" Value="1536"/>
    </Section>
```

Ne feledje, hogy és a tevékenység küszöbértékek mindkét területhez tartozik a mérőszám - terheléselosztás csak aktiválódik Ha terheléselosztás és tevékenység küszöbértékek túllépik az azonos mérőszám. Tehát azt, mint a terheléselosztás küszöbértékét memória és a tevékenység küszöbértékét processzor, ha terheléselosztás nem indítja mindaddig, amíg a hátralévő küszöbértékek (terheléselosztás küszöbértékét Processzor) és a memóriahasználat figyelése küszöbértékét ne lépjék túl.

## <a name="balancing-services-together"></a>Közös terheléselosztás szolgáltatások
Valami megjegyezni, hogy akár a fürt imbalanced-e vagy sem fürtre kiterjedő döntés, de egyes szolgáltatás kópiák és a példányok körül áthelyezi kapcsolatos javítása megyünk módja. Ez értelemszerű, jobbra? Ha memória egy csomóponton több kópiák van halmozott vagy példányok rá lehet járult hozzá, így azt igényel, az állapot-nyilvántartó kópiák vagy állapot nélküli példányai, amelyek a szóban forgó, imbalanced mérőszám áthelyezése.

Időnként abban az esetben, ha egy ügyfél visszahívja us feljebb vagy a fájlnévre, hogy a szolgáltatás, amely nem imbalanced mondanak jegy használ került. Hogyan lehet fordul elő, hogy a szolgáltatás kap áthelyezése, ha az összes adott szolgáltatás mértékek voltak kiegyensúlyozott, még akkor is tökéletesen Igen, a többi egyensúly idején? Lássuk!

Négy szolgáltatások, Service1, Service2, Service3 és Service4, hogy az például. Service1 jelentések mértékek Metric1 és Metric2, Metric2 és Mmetric3, Service3 Metric3 és Metric4 ellen, és néhány metrikus Metric99 elleni Service4 elleni Service2 szemben. Minden bizonnyal megtekintheti, ahol megyünk itt. Van még egy lánc! A szempontjából az fürt erőforrás-kezelő nem igazán felkínálunk négy független szolgáltatást, felkínálunk egy olyan szolgáltatásokat, amelyek a kapcsolódó (Service1 Service2 és Service3) és az, hogy saját maga ki van kapcsolva, hogy.

![Közös terheléselosztás szolgáltatások][Image4]

Akkor lehet, hogy a Metric1 egyensúly okozhatják kópiák vagy Service3 való navigálás tartozó példányok. Általában mozgást meglehetősen korlátozott, de is lehet attól függően, hogy pontosan hogyan imbalanced Metric1 nagyobb használ, és milyen volt szükség a fürt annak érdekében, hogy javítsa ki. Azt is is fel, hogy biztosan, hogy a mértékek, 1, 2 vagy 3 egyensúly soha nem okozhat mozgását Service4 – akkor is nincs pont áthelyezése a replikák óta vagy körül lehet Service4 tartozó példányok figyelmen kívül hagyás teljes mértékben hatással az 1, 2 vagy 3 mértékek egyenlege.

Az erőforrás-kezelő fürt automatikusan számadatok meg, hogy milyen szolgáltatásokra kapcsolódnak, mivel szolgáltatásokat előfordulhat, hogy hozzáadta a, távolítja el, vagy azok metrikus konfigurációs módosítást – például volna, között két fut, a terheléselosztás Service2 előfordulhat, hogy van már újrakonfigurálni Metric2 eltávolításához. Ez a töréspontok a lánc Service1 és Service2 között. Két csoportját a szolgáltatások helyett ekkor három:

![Közös terheléselosztás szolgáltatások][Image5]

## <a name="next-steps"></a>Következő lépések
- Mértékek, hogyan kezeli az szolgáltatás háló fürt erőforrás-kezelő felhasználás és a fürt kapacitása. Ha tudni szeretné őket, és hogyan konfigurálhatók többet tanulmányozza [Ez a cikk](service-fabric-cluster-resource-manager-metrics.md)
- Mozgás költség-a fürt erőforrás-kezelő jelzés, hogy egyes szolgáltatások további drága, mint a többi áthelyezése-e a egy lehetőség. További információ mozgásának költség, olvassa el [Ebben](service-fabric-cluster-resource-manager-movement-cost.md) a cikkben
- Az erőforrás felülete több szabályozás csökkentheti a fürt tejeskanna konfigurálható tulajdonsággal tartalmaz. Azok nem a szokásos módon szükséges, de ha szüksége van rá megismerheti azokat [az alábbi](service-fabric-cluster-resource-manager-advanced-throttling.md)


[Image1]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resrouce-manager-balancing-thresholds.png
[Image2]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resource-manager-balancing-threshold-triggered-results.png
[Image3]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resource-manager-activity-thresholds.png
[Image4]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resource-manager-balancing-services-together1.png
[Image5]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resource-manager-balancing-services-together2.png
