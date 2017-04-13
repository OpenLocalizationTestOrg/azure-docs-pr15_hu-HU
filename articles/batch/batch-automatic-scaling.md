<properties
    pageTitle="Automatikus méretezés kiszámítása a csomópontok az Azure köteg készletben |} Microsoft Azure"
    description="Engedélyezze az automatikus méretezés a felhőben készletébe dinamikusan módosítsa a készletben számítási csomópontok számának."
    services="batch"
    documentationCenter=""
    authors="mmacy"
    manager="timlt"
    editor="tysonn"/>

<tags
    ms.service="batch"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows"
    ms.workload="multiple"
    ms.date="10/14/2016"
    ms.author="marsma"/>

# <a name="automatically-scale-compute-nodes-in-an-azure-batch-pool"></a>Az Azure köteg készletben számítási csomópontok automatikus méretezése

Az automatikus méretezés a köteg Azure szolgáltatás dinamikusan hozzáadása vagy eltávolítása számítási csomópontok erőforráskészlethez tartozik, amely csak paraméterek alapján. Esetleg mentheti idő és a pénzt automatikusan eredményhez tartozó az összeg, az alkalmazás – által használt számítási power csomópontok hozzáadása a feladatok tevékenység igények növekedésével, és eltávolíthatja őket, ha az azok csökkentése.

Automatikus méretezés a számítási csomópontok erőforráskészlethez tartozik egy *Automatikus méretezéssel képlet* határozza meg, mint például az általa társításával engedélyezi a [PoolOperations.EnableAutoScale] a[ net_enableautoscale] módszer a [Köteg .NET](batch-dotnet-get-started.md) tárban. A köteg szolgáltatás majd használja ezt a képletet, amely a terhelést végrehajtásához szükséges számítási csomópontok számának meghatározásához. Köteg válaszol szolgáltatás mértékek rendszeres gyűjtött adatokat a minták, és a következő képlet alapján konfigurálható időközre készletben számítási csomópontok számának igazítása.

Automatikus méretezés a készlet létrehozásakor, vagy egy meglévő készlet engedélyezheti. Egy készlet, amely a "automatikus" méretezéssel engedélyezve van a meglévő képleteket is módosíthatja. Köteg teszi lehetővé a képletek kiértékelése előtt készletek való hozzárendelésükre, valamint figyelheti az automatikus méretezés fut állapotát.

## <a name="automatic-scaling-formulas"></a>Automatikus méretezés képletek

Az automatikus méretezés képlet értéke karakterlánc, amely csak egy vagy több utasítást tartalmazza, és van hozzárendelve egy készlet [autoScaleFormula] [ rest_autoscaleformula] elem (köteg REST) vagy a [CloudPool.AutoScaleFormula] [ net_cloudpool_autoscaleformula] tulajdonság (köteg .NET). Amikor egy csoportba, a köteg szolgáltatás használja a képlet határozza meg a készlet számítási csomópontok cél száma a következő intervallum feldolgozási (vagy újabb intervallumok több). A képlet karakterlánc 8 KB nem haladhatja meg a méretét, tartalmazhatnak legfeljebb 100 kimutatások vannak elválasztva, amely sortörések és megjegyzéseket is előfordulhatnak.

Érdemes elképzelnie automatikus méretezés képletek használatával a köteg Automatikus méretezéssel "nyelv." Képlet kimutatások ingyenes formázott kifejezések, melyek tartalmazhatnak egyaránt szolgáltatás által definiált (a köteg szolgáltatás által meghatározott változók) és felhasználó által definiált változók (változók, amely csak). Ezeket az értékeket a különféle műveletek elvégezhessenek beépített típusok, operátorok és függvény használatával. Ha például egy kimutatást igénybe vehet a következő formában:

```
$myNewVariable = function($ServiceDefinedVariable, $myCustomVariable);
```

Képletek általában az előző kimutatásokban kapott értékek műveletek hajthatók végre több kimutatásokat tartalmaznak. Ha például azt szereznie egy értéknek `variable1`, átadni kitöltéséhez függvény `variable2`:

```
$variable1 = function1($ServiceDefinedVariable);
$variable2 = function2($OtherServiceDefinedVariable, $variable1);
```

Az alábbi utasítások a képletben, a célja, hogy egy szám, amely a készlet kell-e méretezni – számítási csomópontok érkeznek **dedikált csomópontok** **cél** száma. Lehet, hogy a szám nagyobb, a kisbetű és a készlet csomópontok aktuális száma megegyezik. Köteg Kiértékel egy készlet Automatikus méretezéssel képlet meghatározott időközönként ([Automatikus méretezés intervallumok](#automatic-scaling-interval) tárgyalja alább). Azt fogja adja meg a készlet a számra, amely a Automatikus méretezéssel képlet adja meg a kiértékelés idején csomópontok cél száma.

Egy gyors példa kétsoros Automatikus méretezéssel képlet határozza meg, hogy a csomópontok számának aszerint, hogy az aktív feladatok, legfeljebb 10 számítási csomópontok számának kell beállítani:

```
$averageActiveTaskCount = avg($ActiveTasks.GetSample(TimeInterval_Minute * 15));
$TargetDedicated = min(10, $averageActiveTaskCount);
```

Ez a cikk a következő néhány szakaszokból megtudhatja, hogy az egyes személyek a Automatikus méretezéssel képletekkel, beleértve a változók, operátorok, műveletek és funkciók alkotó. Hogyan szerezheti be a különböző számítási tevékenység- és erőforrás mértékek köteg belül találhatók. Ezek a mértékek Ezután módosítsa a készlet csomópont száma az Erőforrás kihasználtsága és a tevékenység állapota alapján is használhatja. Meg kell majd megtudhatja, hogy miként képletet, és az automatikus méretezés meg a készlet a köteg többi és a .NET API-k engedélyezése. Azt fogja fejeződött be néhány példa képleteket.

> [AZURE.IMPORTANT] A maximális száma magmintákat (és ezért számítási csomópontok) feldolgozásra használható minden Azure köteg fiók korlátozódik. A köteg szolgáltatás csak ezt a korlátot core felfelé csomópontok hoz létre. Emiatt nem elérheti a cél számítási csomópontok számának képlet által megadott. Megtekintés és a fiók kvótákat növelése kapcsolatos információk találhatók [kvótáinak és -korlátozások az Azure köteg szolgáltatást](batch-quota-limit.md) .

## <a name="variables"></a>Változók

Az Automatikus méretezéssel képletekben **szolgáltatás által definiált** és a **felhasználó által definiált** is változót használhat. A szolgáltatás által definiált változók a köteg szolgáltatás – beépített írási és olvasási, és csak olvasható. Felhasználó által definiált változók változót is, amelyeket *Ön* határozza meg. A fenti példa kétsoros képletben `$TargetDedicated` szolgáltatás által definiált változó, miközben `$averageActiveTaskCount` felhasználói változó.

Az alábbi táblázatok a köteg szolgáltatás által definiált változók írási és olvasási és az írásvédett.

Lehet **beszerzése** és **beállítani** az értékeket a szolgáltatás által definiált változót egy készletben számítási csomópontok számának kezelése:

| Írási és olvasási szolgáltatás által definiált változók | Leírás |
| --- | --- |
| $TargetDedicated | **Cél** száma a **számítási csomópontok dedikált** gyűjtő. Az, hogy az erőforráskészlet kell méretezni, hogy számítási csomópontok számának. Érdemes "cél" szám, mivel előfordulhat, hogy nem a cél csomópontok számának eléréséhez erőforráskészlethez tartozik. Ez akkor fordulhat elő, ha a csomópontok számának céltábladiagram módosított ismét a későbbi Automatikus méretezéssel kiértékelés előtt a készlet megérkezett a kezdeti céltábladiagram. Ha egy köteg fiók csomópont vagy core kvóta csomópontok számának cél elérése előtt eléri akkor történhet. |
| $NodeDeallocationOption | A művelet, amelyet a fordul elő, ha a számítási csomópontok készletből törlődnek. Lehetséges értékek a következők:<ul><li>**requeue**– azonnal megszakítja a tevékenységek, és jelentkezzen be a feladatsorban helyezi őket, hogy azok átütemezése.<li>a **Leállítás**– azonnal megszakítja a tevékenységek, és eltávolítja azokat a feladatsorban.<li>**taskcompletion**– Várakozás futó feladatok befejezéséhez, és majd eltávolítja a csomópontra a készletből.<li>**retaineddata**– minden helyi tevékenységhez tartott adatokhoz vár, mielőtt eltávolítana a csomópontra a készletből kiüríteni a csomóponton.</ul> |

**Első** érték a szolgáltatás által definiált változót vonatkozó mérőszámok a köteg szolgáltatásból alapuló közül választhat:

| Csak olvasható szolgáltatás által definiált változók | Leírás |
| --- | --- |
| $CPUPercent | Processzorhasználata hányada. |
| $WallClockSeconds | Felhasznált másodpercek számát. |
| $MemoryBytes | A használt megabájt átlagát. |
| $DiskBytes | A helyi lemezen használt gigabájt átlagos száma. |
| $DiskReadBytes | Olvassa el a bájtok számát. |
| $DiskWriteBytes | Írt bájtok számát. |
| $DiskReadOps | Olvasási lemezen a végrehajtott műveletek számát. |
| $DiskWriteOps | A végrehajtott írási lemez műveletek száma. |
| $NetworkInBytes | Bejövő bájtok számát. |
| $NetworkOutBytes | Kimenő bájtok számát. |
| $SampleNodeCount | Számítási csomópontok számának. |
| $ActiveTasks | Aktív állapotú tevékenységek száma. |
| $RunningTasks | Fut tevékenységek száma. |
| $PendingTasks | $ActiveTasks és $RunningTasks összege. |
| $SucceededTasks | Sikeresen befejeződött tevékenységek száma. |
| $FailedTasks | Nem sikerült tevékenységek száma. |
| $CurrentDedicated | Az aktuális száma dedikált csomópontok számítja ki. |

> [AZURE.TIP] A fent látható csak olvasható, a szolgáltatás által definiált változók olyan *objektumok* , amelyek magyarázatot adnak a hozzájuk kapcsolódó adatok eléréséhez különböző módszereket. [Mintaadatok beszerzése](#getsampledata) alatti talál további információt.

## <a name="types"></a>Típusai

Képlet az ilyen **típusú** támogatott.

- dupla
- doubleVec
- doubleVecList
- karakterlánc
- időbélyeg – az időbélyegző egy összetett struktúra, amely tartalmazza a következő tagok:

    - év
    - hónap (1 – 12)
    - nap (1 – 31)
    - Weekday (formátumban, például 1-es számú hétfő a)
    - óra (24 órás formátum, például 13 azt jelenti, 1 du.)
    - perc (00 – 59)
    - második (00 – 59)
- TimeInterval

    - TimeInterval_Zero
    - TimeInterval_100ns
    - TimeInterval_Microsecond
    - TimeInterval_Millisecond
    - TimeInterval_Second
    - TimeInterval_Minute
    - TimeInterval_Hour
    - TimeInterval_Day
    - TimeInterval_Week
    - TimeInterval_Year

## <a name="operations"></a>Műveletek

Ezeket a **műveleteket** a fent ismertetett típusok engedélyezettek.

| Művelet                             | Támogatott operátorok   | Eredmény típusa   |
| ------------------------------------- | --------------------- | ------------- |
| dupla dupla *operátor*              | +, -, *, /            | dupla            |
| dupla *operátor* timeinterval        | *                     | TimeInterval      |
| doubleVec *operátor* dupla           | +, -, *, /            | doubleVec         |
| doubleVec *operátor* doubleVec        | +, -, *, /            | doubleVec         |
| TimeInterval *operátor* dupla        | *, /                  | TimeInterval      |
| TimeInterval *operátor* timeinterval  | +, -                  | TimeInterval      |
| TimeInterval *operátor* időbélyeg     | +                     | időbélyeg         |
| időbélyeg *operátor* timeinterval     | +                     | időbélyeg         |
| időbélyeg *operátor* időbélyeg        | -                     | TimeInterval      |
| dupla *operátor*                      | -, !                  | dupla            |
| *operátor*timeinterval                | -                     | TimeInterval      |
| dupla dupla *operátor*              | < < =, ==, > =, >,! =  | dupla            |
| karakterlánc *operátor* karakterlánc              | < < =, ==, > =, >,! =  | dupla            |
| időbélyeg *operátor* időbélyeg        | < < =, ==, > =, >,! =  | dupla            |
| TimeInterval *operátor* timeinterval  | < < =, ==, > =, >,! =  | dupla            |
| dupla dupla *operátor*              | & & & #124; & #124;      | dupla            |

Egy Ternáris operátorral dupla tesztelésekor (`double ? statement1 : statement2`), a nullától különböző értéke **Igaz**, és a nulla érték **hamis**.

## <a name="functions"></a>Függvények

Ezeket a előre definiált **függvények** szeretne használni az automatikus méretezés képlet definiálásakor érhetők el.

| Függvény                          | Visszatérési típus   | Leírás
| --------------------------------- | ------------- | --------- |
| AVG(doubleVecList)                | dupla        | A doubleVecList az összes érték átlagos értékét adja eredményül.
| len(doubleVecList)                | dupla        | A vektorra a doubleVecList alapján létrehozott hosszát adja vissza.
| LG(Double)                        | dupla        | A napló a duplán az alap 2 adja eredményül.
| LG(doubleVecList)                 | doubleVec     | A componentwise napló a doubleVecList az alap 2 adja eredményül. Egy vec(double) kifejezetten át kell adni a paraméter. Egyéb esetben a dupla lg(double) verzió tekinti.
| ln(Double)                        | dupla        | A természetes naplója a dupla típusú adatot ad eredményül
| ln(doubleVecList)                 | doubleVec     | A componentwise napló a doubleVecList az alap 2 adja eredményül. Egy vec(double) kifejezetten át kell adni a paraméter. Egyéb esetben a dupla lg(double) verzió tekinti.
| log(Double)                       | dupla        | A napló a dupla alap 10 adja eredményül.
| log(doubleVecList)                | doubleVec     | A componentwise napló a doubleVecList alap 10 adja eredményül. Egy vec(double) kifejezetten át kell adni a egyetlen dupla paraméter. Egyéb esetben a dupla log(double) verzió tekinti.
| Max(doubleVecList)                | dupla        | A doubleVecList a legnagyobb értéket adja vissza.
| Min(doubleVecList)                | dupla        | A doubleVecList szereplő legkisebb számot adja eredményül.
| NORM(doubleVecList)               | dupla        | Adja vissza két használ a doubleVecList alapján létrehozott vektor.
| PERCENTILIS (dupla p doubleVec v) | dupla        | A PERCENTILIS elem a vektor v lekérdezése
| rand()                            | dupla        | 0.0 és 1,0 közötti véletlen értéket ad eredményül.
| Range(doubleVecList)              | dupla        | Min és max értékek közötti különbséget a doubleVecList adja eredményül.
| STD(doubleVecList)                | dupla        | A minta szórása, ha az értékek a doubleVecList adja eredményül.
| Stop()                            |               | A autoscaling kifejezés kiértékelése leáll.
| Sum(doubleVecList)                | dupla        | A doubleVecList összetevői összegét adja eredményül.
| időpont (dateTime karakterlánc = "")          | időbélyeg     | Adja vissza az aktuális idő, ha nincs átadott paraméter időbélyege, az időbélyegző, a dateTime karakterlánc átadott, ha. Támogatott dateTime formátumú, hogy a W3C-DTF és RFC 1123.
| val (doubleVec v, dupla i)        | dupla        | Az elem, amely helyen i vektoros v, az index létrehozása kezdő nulla értékét adja eredményül.

Lista argumentumaként elfogadhatja a fenti táblázatban ismertetett funkciók közül. A vesszővel tagolt lista *dupla* és *doubleVec*tetszőleges kombinációját. Példa:

`doubleVecList := ( (double | doubleVec)+(, (double | doubleVec) )* )?`

A *doubleVecList* érték alakul át egyetlen *doubleVec* kiértékelés előtt Például ha `v = [1,2,3]`, majd a hívás `avg(v)` függvény egyenértékű a hívó `avg(1,2,3)`. Hívás `avg(v, 7)` függvény egyenértékű a hívó `avg(1,2,3,7)`.

## <a name="getsampledata"></a>Mintaadatok beszerzése

A képletek Automatikus méretezéssel mértékek adatok (minták), a köteg szolgáltatás által biztosított működésbe lépnek. Képlet megnő, vagy a készlet mérete a megadott megszerzi a szolgáltatásból származó értékek alapján zsugorodik össze. A szolgáltatás által definiált változók fentiekben ismertetett olyan objektumok, adja meg az objektum társított adatok eléréséhez különböző módszereket. A következő kifejezés például processzorhasználata az utolsó öt perc get kérés jeleníti meg:

```
$CPUPercent.GetSample(TimeInterval_Minute * 5)
```

| A módszer | Leírás |
| --- | --- |
| GetSample() | A `GetSample()` módszer adatok minták vektornak adja eredményül.<br/><br/>Minta: a mértékek adatok értékű 30 másodperc. Más szóval a minták 30 másodpercenként kapnak. De amint alább, amikor egy minta gyűjtött, és mikor érhető el egy képlet között késleltetést. Ilyenként nem az összes minta egy adott időszakra feltétlenül vehető igénybe az kiértékelés képlettel.<ul><li>`doubleVec GetSample(double count)`<br/>Szerezze be a legutóbbi minták, amely gyűjtése minták számát adja meg.<br/><br/>`GetSample(1)`a legutóbbi elérhető minta lekérdezése A mértékek, mint `$CPUPercent`, azonban ez nem alkalmazható, mert nem lehetséges, hogy *Ha* a minta összegyűjtött. Elképzelhető, hogy a legutóbbi, vagy a rendszer problémák miatt előfordulhat, sokkal régebbi. Akkor célszerűbb ezekben az esetekben az egyik időszakra alább látható módon használni.<li>`doubleVec GetSample((timestamp or timeinterval) startTime [, double samplePercent])`<br/>Adja meg a mintaadatok összegyűjtéséhez időkeretet. Ha szükséges azt is, minták kell a kért időkereten százalékát.<br/><br/>`$CPUPercent.GetSample(TimeInterval_Minute * 10)`adja vissza 20 mintát, ha az utolsó tíz perccel az összes minta-e a CPUPercent előzményeket. Az utolsó percben előzmény nem érhető el, ha azonban csak a 18 minták vissza. Ebben az esetben:<br/><br/>`$CPUPercent.GetSample(TimeInterval_Minute * 10, 95)`nem felelnek meg, mert a minták csak 90 százaléka érhetők el.<br/><br/>`$CPUPercent.GetSample(TimeInterval_Minute * 10, 80)`járnak.<li>`doubleVec GetSample((timestamp or timeinterval) startTime, (timestamp or timeinterval) endTime [, double samplePercent])`<br/>Adja meg a kezdés és a befejező időpont adatgyűjtés időkeretet.<br/><br/>Fent említett van egy késleltetés, amikor egy minta gyűjtött, és mikor érhető el egy képlet között. Meg kell figyelembe venni használata esetén a `GetSample` módot. Lásd: `GetSamplePercent` alatt.|
| GetSamplePeriod() | Az időszak, amely készítésének minták egy korábbi minta adathalmaz adja eredményül. |
| Count() | A metrikus előzmények minták teljes számát adja eredményül. |
| HistoryBeginTime() | A legrégebbi rendelkezésre álló adatokat minta a mérőszám időbélyegzőjét lekérdezése |
| GetSamplePercent() |Adja eredményül a megadott időközönként a rendelkezésre álló minták százalékát. Példa:<br/><br/>`doubleVec GetSamplePercent( (timestamp or timeinterval) startTime [, (timestamp or timeinterval) endTime] )`<br/><br/>Mivel a `GetSample` módszer sikertelen lesz, ha a minta százalékos visszaadott kisebb, mint a `samplePercent` megadva, akkor használhatja a `GetSamplePercent` először ellenőrizze a módszert. Kattintson egy alternatív műveleteket végezhet Ha elég minták jelen, az automatikus méretezés kiértékelés leállítás nélkül.|

### <a name="samples-sample-percentage-and-the-getsample-method"></a>A minták, minta százalékos és a *GetSample()* módszer

Az alapvető művelet egy Automatikus méretezéssel képlet, hogy tevékenység- és metrikus adatokat olvas be, és adja meg, hogy az adatokon alapuló készlet mérete. Ilyenként fontos, hogy megismerje hogyan kezeljék a képletek Automatikus méretezéssel mértékek adatok vagy a "minták."

**A minták**

A köteg szolgáltatás rendszeres elfoglalja tevékenység- és mérőszámok *minták* és elérhetővé válnak a Automatikus méretezéssel képletek. Ezek a minták 30 másodpercenként rögzítése a köteg szolgáltatás. Nincs azonban általában néhány késés, hogy mikor minták rögzített között, és hogy rendelkezésre álljanak (és olvashatják) késését Automatikus méretezéssel képleteket. Továbbá más infrastruktúra problémák a hálózattal vagy például különböző tényezők miatt minták előfordulhat, hogy nem lett felvéve meghatározott ideig tárolódnak. Ez a "Hiányzó" minták eredményez.

**Minta százalékos**

Ha `samplePercent` átadott a `GetSample()` módszer vagy a `GetSamplePercent()` módszer neve, "százalék" az összes *lehetséges* a köteg szolgáltatás által rögzített minták és a minták ténylegesen *érhető el* az Automatikus méretezéssel képlet számának összehasonlítása hivatkozik.

Nézzük meg a 10 perc időszak példaként. Mivel minták rögzítése a 10 perc időszak belül 30 másodpercenként a köteg által rögzített minták maximális száma 20 mintát (2 percenként) lenne. Azonban a belső várakozási a jelentéskészítési mechanizmusa vagy az Azure infrastruktúra belül bizonyos egyéb probléma miatt lehetnek csak a képlet Automatikus méretezéssel olvasási elérhető 15 mintát. Ez azt jelenti, hogy az adott 10 perc alatt csak **75 %** felvett minta a teljes oldalszám ténylegesen érhető el, a képlet.

**GetSample() és minta tartománya**

Az Automatikus méretezéssel képletek halad növekvő, és a készletek – csomópontok hozzáadásáról és eltávolításáról a csomópontok méretének csökkentése. Csomópontok költség pénz, mert érdemes biztosíthatja, hogy a képletek, hogy van elegendő adatokon alapuló elemzése az intelligens metódusát. Emiatt azt javasoljuk, hogy Ön trendfigyelésre típus elemzés használata a képletekben. Típus a nagyobb, és a kisebb az összegyűjtött mintákat *tartományon* alapuló készletek.

Ehhez használja a `GetSample(interval look-back start, interval look-back end)` való visszatéréshez a **vektoros** minták:

```
$runningTasksSample = $RunningTasks.GetSample(1 * TimeInterval_Minute, 6 * TimeInterval_Minute);
```

Kiértékeli a fenti sor a köteget, ha az értékek egy vektornak minták tartomány ad vissza. Példa:

```
$runningTasksSample=[1,1,1,1,1,1,1,1,1,1];
```

Után a minták vektorra által gyűjtött, majd használhatja funkciók, mint például `min()`, `max()`, és `avg()` értelmes értékek forrásául a összegyűjtött tartományból.

A nagyobb biztonság érdekében a kiértékelési *meghiúsító* is lehetséges, ha kisebb, mint egy bizonyos mintának százalék érhető el egy adott időszakra. Kényszeríti a kiértékelési sikertelen, amikor, kérje meg a köteg további értékelési a képlet megszűnik, ha a megadott százalékos értéket minták nem érhető el –, és a készlet ugyanakkora történik. A sikeres értékeléséhez minták szükséges százalékértéket, adja meg azt a harmadik paraméterként `GetSample()`. Ebben az esetben 75 % minták felvétele szükséges adható:

```
$runningTasksSample = $RunningTasks.GetSample(60 * TimeInterval_Second, 120 * TimeInterval_Second, 75);
```

Fontos is, mindig adja meg egy időtartományt a Megjelenés – vissza Kezdés, amely egy percnél régebbi minta elérhetőségét a korábban már említettük késleltetés miatt. Ennek oka, hogy mintákon keresztül, a rendszer a szülőtől körülbelül egy percig tart a tartomány így minták `(0 * TimeInterval_Second, 60 * TimeInterval_Second)` gyakran nem lesznek elérhetők. Ismét a százalékos paramétere: használható `GetSample()` egy adott minta százalékos követelmény kényszerítése.

> [AZURE.IMPORTANT] Azt **ajánljuk** , hogy Ön * *elkerülése használna *csak* a `GetSample(1)` be az Automatikus méretezéssel képletek **. Ennek oka, hogy `GetSample(1)` lényegében szerint a köteg szolgáltatás "engedi van, függetlenül attól, hogy hogyan nemrég használ, a legutóbbi minta." Mivel csak egy mintája, és lehet, ha egy régebbi mintát, nem lehet nagyobb képe, a legutóbbi tevékenység vagy erőforrás állapotának képviselőjének. Ha `GetSample(1)`, győződjön meg arról, hogy-e egy nagyobb kimutatást, és nem a csak az adatpont, a képlet támaszkodik részét.

## <a name="metrics"></a>Mértékek

**Erőforrás** és a **tevékenység** mértékek egy képlet megadásakor is használhatja. A készletben beszerzése és felmérése mértékek adatokon alapuló dedikált csomópontok cél számának módosítása Című [változók](#variables) feletti további információt a minden metrikus.

<table>
  <tr>
    <th>Metrikus</th>
    <th>Leírás</th>
  </tr>
  <tr>
    <td><b>Erőforrás</b></td>
    <td><p><b>Erőforrás-mértékek</b> Processzor, a sávszélesség és a memória használatát a számítási csomópontok, valamint a csomópontok száma alapján.</p>
        <p> Ezek a szolgáltatás által definiált változók hasznosak lehetnek a korrekciók csomópont száma alapján:</p>
    <p><ul>
      <li>$TargetDedicated</li>
            <li>$CurrentDedicated</li>
            <li>$SampleNodeCount</li>
    </ul></p>
    <p>Ezek a szolgáltatás által definiált változók hasznosak lehetnek a korrekciók csomópont erőforrás-kihasználtság alapján:</p>
    <p><ul>
      <li>$CPUPercent</li>
      <li>$WallClockSeconds</li>
      <li>$MemoryBytes</li>
      <li>$DiskBytes</li>
      <li>$DiskReadBytes</li>
      <li>$DiskWriteBytes</li>
      <li>$DiskReadOps</li>
      <li>$DiskWriteOps</li>
      <li>$NetworkInBytes</li>
      <li>$NetworkOutBytes</li></ul></p>
  </tr>
  <tr>
    <td><b>Tevékenység</b></td>
    <td><p><b>A tevékenység mértékek</b> függőben, például az aktív, a tevékenységek állapotának alapulnak, és befejeződött. A következő szolgáltatás által definiált változók tevékenység mértékek alapján készlet méretű korrekciók hasznosak:</p>
    <p><ul>
      <li>$ActiveTasks</li>
      <li>$RunningTasks</li>
      <li>$PendingTasks</li>
      <li>$SucceededTasks</li>
            <li>$FailedTasks</li></ul></p>
        </td>
  </tr>
</table>

## <a name="write-an-autoscale-formula"></a>Írja be az Automatikus méretezéssel képlet

Hozza létre az Automatikus méretezéssel képletet úgy, hogy a fenti összetevőket használó kimutatásokat alkotó, majd ezek kimutatások teljes a képletbe kombinálása. Ebben a részben azt létre fogja hozni egy példa Automatikus méretezéssel képletet, amely az egyes valós életből méretezési döntéseket hajthatják végre.

Először nézzük definiálása az új Automatikus méretezéssel képletet követelményei. A képlet feladatokat kell végrehajtania:

1. **Növelje** a számítási csomópontok Ha processzorhasználata magas készletben cél számát.
2. **Csökkentése** egy készletben, amikor processzorhasználata alacsony számítási csomópontok cél számát.
3. Mindig korlátozása 400 csomópontok **maximális** száma.

*Növelje* során processzort csomópontok számának, azt határozza meg a felhasználó által definiált változó feltöltő utasítás (`$totalNodes`), amelynek 110 százalék csomópontok, de csak ha az aktuális cél szám az utolsó 10 perc alatt a minimális átlagos processzorhasználata meghaladta 70 %. Egyéb esetben azt be az aktuális dedikált értéket.

```
$totalNodes =
    (min($CPUPercent.GetSample(TimeInterval_Minute * 10)) > 0.7) ?
    ($CurrentDedicated * 1.1) : $CurrentDedicated;
```

*Csökkentse* az alatt alacsony processzorhasználata csomópontok számának, a képlet a következő utasítás állítja be az azonos `$totalNodes` változó 90 százalékát csomópontok, ha 20 százalékát csoportban volt az átlagos processzorhasználata az elmúlt 60 percben aktuális cél számát. Egyéb esetben használja az aktuális értékét `$totalNodes` , amely azt az utasításban töltve.

```
$totalNodes =
    (avg($CPUPercent.GetSample(TimeInterval_Minute * 60)) < 0.2) ?
    ($CurrentDedicated * 0.9) : $totalNodes;
```

Most egy **maximális** 400 dedikált számítási csomópontok cél számának korlátozása:

```
$TargetDedicated = min(400, $totalNodes)
```

Az alábbiakban a teljes képletet:

```
$totalNodes =
    (min($CPUPercent.GetSample(TimeInterval_Minute * 10)) > 0.7) ?
    ($CurrentDedicated * 1.1) : $CurrentDedicated;
$totalNodes =
    (avg($CPUPercent.GetSample(TimeInterval_Minute * 60)) < 0.2) ?
    ($CurrentDedicated * 0.9) : $totalNodes;
$TargetDedicated = min(400, $totalNodes)
```

## <a name="create-an-autoscale-enabled-pool"></a>Az Automatikus méretezéssel engedélyező készlet létrehozása

Új készlet létrehozása a autoscaling engedélyezve, használhatja a következő módszerek egyikét:

**A köteg .NET**

1. A készlet létrehozása [BatchClient.PoolOperations.CreatePool](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.createpool.aspx).
1. A tulajdonsága [CloudPool.AutoScaleEnabled](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.autoscaleenabled.aspx) `true`.
1. A [CloudPool.AutoScaleFormula](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.autoscaleformula.aspx) tulajdonsága az Automatikus méretezéssel képlettel.
1. (Nem kötelező) A [CloudPool.AutoScaleEvaluationInterval](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.autoScaleevaluationinterval.aspx) tulajdonság (az alapértelmezett érték 15 perc).
1. Hajtsa végre a készlet [CloudPool.Commit](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.commit.aspx) vagy [CommitAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.commitasync.aspx).

**A köteg REST API-val**

* [A fiók-készlet hozzáadása](https://msdn.microsoft.com/library/azure/dn820174.aspx): Adja meg a `enableAutoScale` és `autoScaleFormula` elemeket az automatikus méretezés készlet létrehozásakor konfigurálása REST API-összehívásban.

A következő kódrészletet létrehoz egy Automatikus méretezéssel engedélyező készlet segítségével a [Köteg .NET] [ net_api] tárat. A készlet Automatikus méretezéssel képlet állítja be az 5-ös hétfőn csomópontok céltábladiagram számát, és 1, a hét minden más napján. Az [Automatikus méretezés intervallum](#automatic-scaling-interval) 30 percig van beállítva. Ebben és a többi C# kódrészletek ebben a cikkben "myBatchClient"-e a [BatchClient]megfelelően inicializált példány[net_batchclient].

```csharp
CloudPool pool = myBatchClient.PoolOperations.CreatePool("mypool", "3", "small");
pool.AutoScaleEnabled = true;
pool.AutoScaleFormula = "$TargetDedicated = (time().weekday == 1 ? 5:1);";
pool.AutoScaleEvaluationInterval = TimeSpan.FromMinutes(30);
pool.Commit();
```

A köteg REST API-val és a .NET SDK mellett is használhatja a [Köteg SDK](batch-technical-overview.md#batch-development-apis)más, a [Köteg PowerShell-parancsmagok](batch-powershell-cmdlets-get-started.md)és a [Köteg CLI](batch-cli-get-started.md) autoscaling végezhető.

> [AZURE.IMPORTANT] Amikor létrehoz egy Automatikus méretezéssel engedélyező készlet, be kell **nem** adja meg a `targetDedicated` paraméter. Is, ha szeretné kézzel átméretezése egy Automatikus méretezéssel engedélyező készlet (például a [BatchClient.PoolOperations.ResizePool][net_poolops_resizepool]), majd be kell első **letiltja** az automatikus méretezés meg a készlet, majd méretezze át.

### <a name="automatic-scaling-interval"></a>Automatikus méretezés intervallum

Alapértelmezés szerint a köteg szolgáltatás minden **15 percet**megadja egy készlet méretét annak Automatikus méretezéssel képletnek megfelelően. Az intervallum módosítható, azonban a következő készlet tulajdonságok használatával:

* [CloudPool.AutoScaleEvaluationInterval] [ net_cloudpool_autoscaleevalinterval] (.NET köteg)
* [autoScaleEvaluationInterval] [ rest_autoscaleinterval] (REST API-val)

A minimális időköz öt perc, és a legnagyobb érték 168 óra. Ha elvégezve e tartományon kívül van megadva, akkor a köteg szolgáltatás egy hibás kérés (400) hibát ad eredményül.

> [AZURE.NOTE] Autoscaling jelenleg nem szánt egy perc alatt módosításokat szeretne válaszolni, de inkább célja, hogy a készlet fokozatosan közben terhelési futtatása méretének módosítása.

## <a name="enable-autoscaling-on-an-existing-pool"></a>Kattintson egy meglévő készlet autoscaling engedélyezése

Ha a *targetDedicated* paraméter használatával beállítása a számítási csomópontok száma a már létrehozott egy készlet, akkor is engedélyezheti a autoscaling meg a készlet. Minden egyes köteg SDK "engedélyezése Automatikus méretezéssel" műveletet, például nyújtja:

* [BatchClient.PoolOperations.EnableAutoScale] [ net_enableautoscale] (.NET köteg)
*  [Automatikus méretezés meg a készlet engedélyezése] [ rest_enableautoscale] (REST API-val)

Ha engedélyezi a egy meglévő készlet autoscaling, az alábbi vonatkozik:

* Ha az automatikus méretezés jelenleg pedig **letiltotta** a "engedélyezése automatikus méretezéssel" kérés elküldésekor, be *kell* meg egy érvényes Automatikus méretezéssel képletet a kérelem elküldésekor. *Tetszőlegesen* választhat egy Automatikus méretezéssel frissítési időköz adja meg. Ha nem adja meg a elvégezve, az alapértelmezett érték a 15 perces használják.

* Ha Automatikus méretezéssel jelenleg **engedélyezve van** a készlet, megadhatja egy Automatikus méretezéssel képletet, egy frissítési időköz vagy mindkettőt. Nem adunk mindkét tulajdonságot.

  * Ha megad egy új Automatikus méretezéssel frissítési időköz, a meglévő frissítési ütemezés van állítva, és azonnal elindul, egy új kimutatást. Az új ütemezés kezdetének az az idő, amelyen a "Automatikus méretezéssel engedélyezése" kérés adta ki.

  * Ha a paramétert a Automatikus méretezéssel képletet vagy a frissítési időköz, a a köteg szolgáltatás továbbra is használja ezt a beállítást a jelenlegi értékét.

> [AZURE.NOTE] Ha egy értéket a *targetDedicated* paraméterhez megadott pedig létrehozásakor, figyelmen kívül az automatikus méretezés képlet kiértékelése során.

Ez a C# kódtöredék használja a [Köteg .NET] [ net_api] ahhoz, hogy a egy meglévő készlet autoscaling tár:

```csharp
// Define the autoscaling formula. This formula sets the target number of nodes
// to 5 on Mondays, and 1 on every other day of the week
string myAutoScaleFormula = "$TargetDedicated = (time().weekday == 1 ? 5:1);";

// Set the autoscale formula on the existing pool
myBatchClient.PoolOperations.EnableAutoScale(
    "myexistingpool",
    autoscaleFormula: myAutoScaleFormula);
```

### <a name="update-an-autoscale-formula"></a>Az Automatikus méretezéssel képlet frissítése

A "Automatikus méretezéssel engedélyezése" kérésben *frissíti* a képletet egy meglévő Automatikus méretezéssel engedélyező erőforráskészlet használata (például a [EnableAutoScale] [ net_enableautoscale] a köteg .NET). Speciális "Automatikus méretezéssel frissítése" művelet nem. Például ha autoscaling már engedélyezni "myexistingpool", a következő kódot végrehajtásakor, az Automatikus méretezéssel képlet cseréli tartalmának `myNewFormula`.

```csharp
myBatchClient.PoolOperations.EnableAutoScale(
    "myexistingpool",
    autoscaleFormula: myNewFormula);
```

### <a name="update-the-autoscale-interval"></a>Az Automatikus méretezéssel időközének módosítása

Az Automatikus méretezéssel képlet frissítése, az azonos [EnableAutoScale] használata[ net_enableautoscale] módszer a meglévő Automatikus méretezéssel engedélyező készlet Automatikus méretezéssel kiértékelés intervallum módosításához. Tegyük fel például, és már Automatikus méretezéssel engedélyező készlet 60 perces Automatikus méretezéssel frissítési időköz beállítása:

```csharp
myBatchClient.PoolOperations.EnableAutoScale(
    "myexistingpool",
    autoscaleEvaluationInterval: TimeSpan.FromMinutes(60));
```

## <a name="evaluate-an-autoscale-formula"></a>Az Automatikus méretezéssel Képletkiértékelő

A Képletkiértékelő meg a készlet alkalmazása előtt. Ezzel a módszerrel végezheti el a "próba futtatása" a képlet tekintheti meg, hogyan kimutatásaiban logikai értéket, a képlet üzembe gyártási előtt.

Az Automatikus méretezéssel Képletkiértékelő kell első **autoscaling engedélyezése** meg a készlet **érvényes képlet**. Egy képlet még nincs engedélyezve autoscaling erőforráskészlethez tartozik a vizsgálni kívánt, ha a egysoros képletet használhatja `$TargetDedicated = 0` mikor engedélyeznie autoscaling. A vizsgálni kívánt képletet ki szeretné számítani majd, használja az alábbiak egyikét:

* [BatchClient.PoolOperations.EvaluateAutoScale](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.evaluateautoscale.aspx) vagy [EvaluateAutoScaleAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.evaluateautoscaleasync.aspx)

    A köteg .NET módszerekről az alábbi szükség egy meglévő készlet és az Automatikus méretezéssel képletet tartalmazó karakterlánc azonosítója ki szeretné számítani. A visszaadott [AutoScaleEvaluation](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.autoscaleevaluation.aspx) példány értékelési eredmények tartalmazzák.

* [Az automatikus méretezés Képletkiértékelő](https://msdn.microsoft.com/library/azure/dn820183.aspx)

    Adja meg a készlet Azonosítóját az URI és az Automatikus méretezéssel képlet összehívás törzsébe *autoScaleFormula* eleme a REST API-összehívásban. A válasz, a művelet bármilyen hiba információkat tartalmaz, a képlet lehetnek kapcsolatban.

A [Köteg .NET] a[ net_api] kódtöredék, azt Képletkiértékelő egy alkalmazása, a [CloudPool]előtt[net_cloudpool]. Ha pedig nincs engedélyezve autoscaling, hogy engedélyezze azt először.

```csharp
// First obtain a reference to an existing pool
CloudPool pool = batchClient.PoolOperations.GetPool("myExistingPool");

// If autoscaling isn't already enabled on the pool, enable it.
// You can't evaluate an autoscale formula on non-autoscale-enabled pool.
if (pool.AutoScaleEnabled == false)
{
    // We need a valid autoscale formula to enable autoscaling on the
    // pool. This formula is valid, but won't resize the pool:
    pool.EnableAutoScale(
        autoscaleFormula: $"$TargetDedicated = {pool.CurrentDedicated};",
        autoscaleEvaluationInterval: TimeSpan.FromMinutes(5));

    // Batch limits EnableAutoScale calls to once every 30 seconds.
    // Because we want to apply our new autoscale formula below if it
    // evaluates successfully, and we *just* enabled autoscaling on
    // this pool, we pause here to ensure we pass that threshold.
    Thread.Sleep(TimeSpan.FromSeconds(31));

    // Refresh the properties of the pool so that we've got the
    // latest value for AutoScaleEnabled
    pool.Refresh();
}

// We must ensure that autoscaling is enabled on the pool prior to
// evaluating a formula
if (pool.AutoScaleEnabled == true)
{
    // The formula to evaluate - adjusts target number of nodes based on
    // day of week and time of day
    string myFormula = @"
        $curTime = time();
        $workHours = $curTime.hour >= 8 && $curTime.hour < 18;
        $isWeekday = $curTime.weekday >= 1 && $curTime.weekday <= 5;
        $isWorkingWeekdayHour = $workHours && $isWeekday;
        $TargetDedicated = $isWorkingWeekdayHour ? 20:10;
    ";

    // Perform the autoscale formula evaluation. Note that this does not
    // actually apply the formula to the pool.
    AutoScaleRun eval =
        batchClient.PoolOperations.EvaluateAutoScale(pool.Id, myFormula);

    if (eval.Error == null)
    {
        // Evaluation success - print the results of the AutoScaleRun.
        // This will display the values of each variable as evaluated by the
        // autoscale formula.
        Console.WriteLine("AutoScaleRun.Results: " +
            eval.Results.Replace("$", "\n    $"));

        // Apply the formula to the pool since it evaluated successfully
        batchClient.PoolOperations.EnableAutoScale(pool.Id, myFormula);
    }
    else
    {
        // Evaluation failed, output the message associated with the error
        Console.WriteLine("AutoScaleRun.Error.Message: " +
            eval.Error.Message);
    }
}
```

A képlet a kódtöredék sikeres értékelése az alábbihoz hasonló kimeneti eredményezi:

```
AutoScaleRun.Results:
    $TargetDedicated=10;
    $NodeDeallocationOption=requeue;
    $curTime=2016-10-13T19:18:47.805Z;
    $isWeekday=1;
    $isWorkingWeekdayHour=0;
    $workHours=0
```

## <a name="get-information-about-autoscale-runs"></a>Automatikus méretezéssel fut adatainak megtekintése

Annak érdekében, hogy a képlet vártnak hajt végre, javasoljuk, a "lefut" köteg elvégzi a készlet autoscaling eredményének rendszeres ellenőrzésére. Igen, az első (vagy frissítése) hivatkozást a kvótáját, és az utolsó Automatikus méretezéssel futtatása tulajdonságainak.

A köteg .NET a [CloudPool.AutoScaleRun](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.autoscalerun.aspx) tulajdonságnak több tulajdonságának és információt nyújt a legújabb automatikus méretezés futtatása elvégzett meg a készlet köteg szolgáltatás.

* [AutoScaleRun.Timestamp](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.autoscalerun.timestamp.aspx)
* [AutoScaleRun.Results](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.autoscalerun.results.aspx)
* [AutoScaleRun.Error](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.autoscalerun.error.aspx)

A REST API [erőforráskészlethez tartozik adatainak](https://msdn.microsoft.com/library/dn820165.aspx) kérésére adatokat ad eredményül a készlet, a legújabb automatikus méretezés információk futtatja a [autoScaleRun](https://msdn.microsoft.com/library/dn820165.aspx#bk_autrun)tartalmazó.

A következő C# kódrészletet a köteg .NET tár használja az utolsó autoscaling készlet "myPool" futtatni adatainak nyomtatása:

```csharp
Cloud pool = myBatchClient.PoolOperations.GetPool("myPool");
Console.WriteLine("Last execution: " + pool.AutoScaleRun.Timestamp);
Console.WriteLine("Result:" + pool.AutoScaleRun.Results.Replace("$", "\n  $"));
Console.WriteLine("Error: " + pool.AutoScaleRun.Error);
```

Az előző kódtöredékének minta kimenetét:

```
Last execution: 10/14/2016 18:36:43
Result:
  $TargetDedicated=10;
  $NodeDeallocationOption=requeue;
  $curTime=2016-10-14T18:36:43.282Z;
  $isWeekday=1;
  $isWorkingWeekdayHour=0;
  $workHours=0
Error:
```

## <a name="example-autoscale-formulas"></a>Automatikus méretezéssel mintaképletek

Most tekintsünk át egy néhány képletek megjelenítése a változatos módszerekkel, amelyekkel egy készletben számítási erőforrások méretének beállításához.

### <a name="example-1-time-based-adjustment"></a>Példa: 1: Időalapú kapcsolatos korrekció

Esetleg módosítani kívánt a készlet méretét, a hét napját és időpontját, növelheti vagy csökkentheti a készletben csomópontok számának megfelelően alapján.

Ez a képlet első beolvassa a pontos idő. Ha egy weekday (1-5), és munka órában (8 AM a 18: 00), a cél készlet mérete 20 csomópontok van beállítva. Egyéb esetben beállítás 10 csomópontot.

```
$curTime = time();
$workHours = $curTime.hour >= 8 && $curTime.hour < 18;
$isWeekday = $curTime.weekday >= 1 && $curTime.weekday <= 5;
$isWorkingWeekdayHour = $workHours && $isWeekday;
$TargetDedicated = $isWorkingWeekdayHour ? 20:10;
```

### <a name="example-2-task-based-adjustment"></a>Példa 2: Feladataival kapcsolatos korrekció

Ebben a példában a készlet méretét módosítani a várakozási sorban található feladatok száma alapján. Látható, és a megjegyzéseket is sortörések elfogadható, a képlet karakterláncokban.

```csharp
// Get pending tasks for the past 15 minutes.
$samples = $ActiveTasks.GetSamplePercent(TimeInterval_Minute * 15);
// If we have fewer than 70 percent data points, we use the last sample point,
// otherwise we use the maximum of last sample point and the history average.
$tasks = $samples < 70 ? max(0,$ActiveTasks.GetSample(1)) : max( $ActiveTasks.GetSample(1), avg($ActiveTasks.GetSample(TimeInterval_Minute * 15)));
// If number of pending tasks is not 0, set targetVM to pending tasks, otherwise
// half of current dedicated.
$targetVMs = $tasks > 0? $tasks:max(0, $TargetDedicated/2);
// The pool size is capped at 20, if target VM value is more than that, set it
// to 20. This value should be adjusted according to your use case.
$TargetDedicated = max(0, min($targetVMs, 20));
// Set node deallocation mode - keep nodes active only until tasks finish
$NodeDeallocationOption = taskcompletion;
```

### <a name="example-3-accounting-for-parallel-tasks"></a>Például: 3: Könyvelési párhuzamos feladatokhoz

Ez a egy másik példa látható tevékenységek száma alapján készlet méretének igazítása. Ez a képlet is figyelembe veszi a [MaxTasksPerComputeNode] [ net_maxtasks] érték, amely a készlet lett állítva. Az esetekben, ahol [a párhuzamos feladat-végrehajtás](batch-parallel-node-tasks.md) engedélyezve van a készlet különösen hasznos.

```csharp
// Determine whether 70 percent of the samples have been recorded in the past
// 15 minutes; if not, use last sample
$samples = $ActiveTasks.GetSamplePercent(TimeInterval_Minute * 15);
$tasks = $samples < 70 ? max(0,$ActiveTasks.GetSample(1)) : max( $ActiveTasks.GetSample(1),avg($ActiveTasks.GetSample(TimeInterval_Minute * 15)));
// Set the number of nodes to add to one-fourth the number of active tasks (the
// MaxTasksPerComputeNode property on this pool is set to 4, adjust this number
// for your use case)
$cores = $TargetDedicated * 4;
$extraVMs = (($tasks - $cores) + 3) / 4;
$targetVMs = ($TargetDedicated + $extraVMs);
// Attempt to grow the number of compute nodes to match the number of active
// tasks, with a maximum of 3
$TargetDedicated = max(0,min($targetVMs,3));
// Keep the nodes active until the tasks finish
$NodeDeallocationOption = taskcompletion;
```

### <a name="example-4-setting-an-initial-pool-size"></a>Példa 4: Az első készlet méretének beállítása

Ebben a példában a C# kódtöredék beállítja készlet csomópontok bizonyos számú kezdeti időszakra Automatikus méretezéssel képlettel. Majd azt megadja a készlet mérete futó száma alapján és az aktív feladatok az első időszak eltelte után.

A képlet a következő kódrészletet:

- Az első készlet mérete négy csomópontok állítja be.
- Nem készlet méretének igazítása az első 10 a készlet életciklus percen belül.
- 10 perc múlva beolvassa a számát a maximális értéke futtatása és az aktív feladatok az elmúlt 60 percen belül.
  - Ha mindkét érték 0 (az, hogy a feladat nem voltak futó vagy az elmúlt 60 percben aktív jelző), a készlet mérete értéke 0.
  - Ha bármelyik érték nullánál nagyobb, nem történik módosítás.

```csharp
string now = DateTime.UtcNow.ToString("r");
string formula = string.Format(@"
    $TargetDedicated = {1};
    lifespan         = time() - time(""{0}"");
    span             = TimeInterval_Minute * 60;
    startup          = TimeInterval_Minute * 10;
    ratio            = 50;

    $TargetDedicated = (lifespan > startup ? (max($RunningTasks.GetSample(span, ratio), $ActiveTasks.GetSample(span, ratio)) == 0 ? 0 : $TargetDedicated) : {1});
    ", now, 4);
```

## <a name="next-steps"></a>Következő lépések

* Hogyan hajthat végre több tevékenység egyidejű meg a készlet számítási csomópontjának részleteket [maximalizálása Azure köteg kiszámítania egyidejű csomópontjának tevékenységek erőforrás-kihasználtság](batch-parallel-node-tasks.md) tartalmazza. Nemcsak a autoscaling Ez a funkció szeretné lesüllyeszteni a feladat időtartamának néhány munkaterhelésekből, pénzt mentése, nyújthatnak segítséget:

* A másik hatékonyság gyorsító győződjön meg arról, hogy a köteg kérdezi le a köteg szolgáltatás a legtöbb optimális módon. [A köteg Azure szolgáltatás hatékonyan lekérdezés](batch-efficient-list-queries.md), a, fog megtudhatja, hogy miként korlátozza az adatokat tartalmazó a átutalásra vonatkozó előfordulhat, hogy több ezer számítási csomópontok vagy a tevékenységek állapotát.

[net_api]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[net_batchclient]: http://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_cloudpool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_cloudpool_autoscaleformula]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.autoscaleformula.aspx
[net_cloudpool_autoscaleevalinterval]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.autoscaleevaluationinterval.aspx
[net_enableautoscale]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.enableautoscale.aspx
[net_maxtasks]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.maxtaskspercomputenode.aspx
[net_poolops_resizepool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.resizepool.aspx

[rest_api]: https://msdn.microsoft.com/library/azure/dn820158.aspx
[rest_autoscaleformula]: https://msdn.microsoft.com/library/azure/dn820173.aspx
[rest_autoscaleinterval]: https://msdn.microsoft.com/library/azure/dn820173.aspx
[rest_enableautoscale]: https://msdn.microsoft.com/library/azure/dn820173.aspx
