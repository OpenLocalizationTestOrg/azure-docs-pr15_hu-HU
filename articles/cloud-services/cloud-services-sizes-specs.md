<properties
 pageTitle="A felhőszolgáltatások méretű |} Microsoft Azure"
 description="Azure felhőalapú szolgáltatás webes és dolgozó szerepkörök különböző virtuális gép méretű (és azonosítók) sorolja fel."
 services="cloud-services"
 documentationCenter=""
 authors="Thraka"
 manager="timlt"
 editor=""/>
<tags
 ms.service="cloud-services"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="tbd"
 ms.date="10/27/2016"
 ms.author="adegeo"/>

# <a name="sizes-for-cloud-services"></a>A Cloud Services méretek

Ez a témakör ismerteti a rendelkezésre álló méretét és a beállítások Felhőszolgáltatásba szerepkör-példányok (webes és dolgozó szerepeket). Telepítése során megfontolandó kérdések tudatában kell lennie, ha azt tervezi, hogy használja az alábbi forrásokat is tartalmaz. Minden méretét fog helyezi el a [szolgáltatás-definíciós fájl](cloud-services-model-and-package.md#csdef)jelző Azonosítóra van.

Cloud Services egyike, a különböző típusú Azure által kínált számítási erőforrásokat. Kattintson [ide](cloud-services-choose-me.md) Cloud Services további információt.

> [AZURE.NOTE]Kapcsolódó Azure korlátai című témakör tartalmazza [Azure előfizetés és szolgáltatás korlátozások, kvótákat, és korlátozások](../azure-subscription-service-limits.md)

## <a name="sizes-for-web-and-worker-role-instances"></a>A webhely és a dolgozói szerepkör-példányok méretek

Vannak olyan több szokásos méretű a Azure közül választhat. Megfontolandó szempontok következő méretek közül a következők:

* D-sorozat VMs, amely nagyobb számítási és ideiglenes lemez teljesítmény igény alkalmazások futtatásához készültek. D-sorozat VMs a gyorsabb processzor, magasabb memória-core arány és félvezető meghajtó (SSD) az ideiglenes lemez szükséges. Részletekért olvassa el az értesítés az Azure blogjában [Új D-sorozat virtuális gép méretét](https://azure.microsoft.com/blog/2014/09/22/new-d-series-virtual-machine-sizes/).

* Dv2 sorozat, a Folytatás eredeti D-sorozat, egy nagyobb teljesítményű Processzor szolgáltatásait. A Dv2-sorozat Processzor körülbelül 35 %-kal gyorsabb, mint a D-sorozat Processzor. A legújabb generációs alapul 2.4 GHz-es Intel Xeon® E5-2673 v3 processzor (Haswell), és az Intel Turbo erősítése technológia 2.0-s, válassza a legfeljebb 3.1 GHz-es. A Dv2 sorozat az azonos memóriát, valamint a lemez konfigurációk a D adatsorként tartalmaz.

*   G-sorozat VMs ajánlja fel a legtöbb memóriát, és futtassa a hosts Intel Xeon E5 V3 családi processzorokkal.

*   Az A-sorozat VMs számos hardver-típusok és processzorok telepíthetők. A méret folyamatban van, a hardver egységes processzor teljesítményét futó példány, függetlenül a telepítve van a hardver kínálatát alapján. Annak megállapításához, a fizikai hardver, amelyen telepítve van a méretét, a virtuális hardver virtuális gépen belüli lekérdezés.

*   A0 mérete a fizikai hardveren túlságosan előfizetett. Csak a megadott méretét más felhasználói telepítések hatással lehetnek a futó terhelést teljesítményét. A relatív teljesítményét fizetnie egy közelítő változatosságáról 15 % a várt alapjául keret alatt.


A virtuális gép méretének hatással van a árak. A méret is érinti a virtuális gép feldolgozása, a memória és a tárhely kapacitása. Tárolási költségek számítása a tárterület-fiókhoz használt lapok külön-külön alapján történik. A részletekért olvassa [Virtuális gépeken futó árak részletek](https://azure.microsoft.com/pricing/details/virtual-machines/) és [Azure tároló árak](https://azure.microsoft.com/pricing/details/storage/). 


A következő szempontokat mérlegelve előfordulhat, hogy könnyebben eldöntheti, a méretet:


* A A8-A11 és H – sorozat méret más néven *számítási igényű példányok*. A következő méretek futtató hardver tervezett és számítási igényű optimalizálva, és hálózati igényű alkalmazásai, köztük a nagy teljesítményű számítógépes (HPC) fürt alkalmazások, modellezési és szimulációk. A A8-A11 sorozat használja az Intel Xeon E5-2670 @ 2.6 GHz-es és a H sorozat használja az Intel Xeon E5-2667 v3 @ 3,2 GHz-es. Részletes tudnivalókat és a következő méretek használatával kapcsolatos szempontok című témakörben [olvashat a H – adatsorok és a számítási igényű A-sorozat VMs](../virtual-machines/virtual-machines-windows-a8-a9-a10-a11-specs.md). 

* Dv2 sorozat, D-sorozat, G-sorozat, ideálisak gyorsabb processzor igény alkalmazásokhoz, jobban helyi teljesítmény lemezre manuálisan, vagy nagyobb memória igényeivel.  Több vállalati szintű alkalmazás hatékony együttes kínálnak.

*   A fizikai állomások az Azure adatközpontokban részét nem támogatja a virtuális gép nagyobb méretben, például az A5 – A11. Emiatt, jelenhet meg a **virtuális gép {számítógépnév} konfigurálása sikertelen** hibaüzenet jelenik meg vagy **nem sikerült létrehozni a virtuális gép {számítógépnév}** ; új csökkentése egy meglévő virtuális gép átméretezés egy új virtuális gép létrehozása előtt április 16, 2013; létre virtuális hálózatban vagy egy új virtuális gép hozzáadása egy meglévő felhőalapú szolgáltatást. Lásd: [Hiba: "Nem sikerült beállítani a virtuális gép"](https://social.msdn.microsoft.com/Forums/9693f56c-fcd3-4d42-850e-5e3b56c7d6be/error-failed-to-configure-virtual-machine-with-a5-a6-or-a7-vm-size?forum=WAVirtualMachinesforWindows) a támogatási fórumon az egyes telepítési forgatókönyv lehetséges megoldások.  

* Az előfizetés is előfordulhat, hogy az egyes méret családok terjeszthet magmintákat számának korlátozása. Ha növelni szeretné a kvóta, a Azure ügyfélszolgálatát.


## <a name="performance-considerations"></a>Teljesítménnyel kapcsolatos szempontok

Azt hozott létre, amely a az Azure kiszámítania egység (ACU) számítási (Processzor) teljesítményét összehasonlítása keresztül Azure termékváltozatok kínál a. Ez segítségével egyszerűen azonosítani tudja Termékváltozat vagyis legvalószínűbb teljesítmény igényeinek kielégítéséhez.  A kis (Standard_A1) jelenleg szabványos a ACU alatt kattintson a 100 és minden termékváltozatban virtuális jelenítik meg körülbelül mennyi gyorsabb, hogy Termékváltozat futtathat egy szabványos javasolt. 

>[AZURE.IMPORTANT] A ACU található útmutatást csak.  Az eredmények a terhelést eltérőek lehetnek. 

<br>

|Termékváltozat család |ACU/Core |
|---|---|
|[Standard_A0](#a-series)   |50 |
|[Standard_A1-4-es](#a-series) |100 |
|[Standard_A5-7](#a-series) |100 |
|[A8-A11](#a-series)    |225 *|
|[D1-14](#d-series) |160 |
|[D1-15v2](#dv2-series) |210 – 250 *|
|[G1 – 5](#g-series)  |180 - 240 *|
|[H](#h-series) |290 – 300 *|

Jelölt ACUs egy * Intel® Turbo technológia segítségével növelje a Processzor gyakoriság, és adja meg a a teljesítmény erősítése.  A erősítése mennyiségét a virtuális méretét, a terhelést és a többi azonos állomáson futó feladatok függően változhat.

## <a name="size-tables"></a>Táblák átméretezése

Az alábbi táblázatokban megjelenítése a méret és a kapacitás biztosítanak.

* Tárolókapacitású látható orrot vagy 1024 mértékegysége ^ 3 bájt. Amikor összehasonlítása a lemezt mért GB (1000 ^ 3 bájt) orrot mért lemezre (1024 ^ 3) ne feledje, hogy a megadott orrot kapacitás számok kisebb jelenhet. Ha például 1023 orrot = 1098.4 GB

* Lemezen átviteli bemeneti és kimeneti műveletek / szekundum (IOPS) mérése és MB amennyiben MB = 10 ^ 6 bájt/sec.

* Adatok lemezt a gyorsítótárban tárolt vagy nem gyorsítótárazott módokban is működnek. A host gyorsítótár mód gyorsítótárban tárolt adatokat a lemez művelethez, **csak olvasható** vagy **ReadWrite**beállítása.  A host gyorsítótár mód nem gyorsítótárazott adatokat lemez működéséhez **nincs**értékre van állítva.

* Maximális hálózati sávszélességre kiosztva, és a virtuális típusonként kiosztott maximális összesített sávszélesség. A maximális sávszélesség itt érhető el útmutató ahhoz, hogy megfelelő a hálózati kapacitás jobb virtuális típusának kiválasztása. Alacsony, mérsékelt, a magas, nagyon magas közötti áthelyezése, amikor a átviteli megfelelően megnöveli. Tényleges hálózati teljesítmény számos tényező, többek között a hálózati és az alkalmazás terhelések és a hálózati Alkalmazásbeállítások függ.


## <a name="a-series"></a>A-sorozat

| Mérete        | Processzormagok | Memória: orrot | Helyi merevlemez: orrot | Max adatok lemez | Max fájlmegosztásra lemez: IOPS | Max NIC / hálózati sávszélességet |
|-------------|-----------|--------------|-----------------------|----------------|--------------------|-----------------------|
| Standard_A0 | 1         | 0.768        | 20                    | 1              | 1 x 500              | 1 / alacsony                   |
| Standard_A1 | 1         | 1.75         | a 70                    | 2              | 2 x 500              | 1 / mérsékelt              |
| Standard_A2 | 2         | 3,5 GB       | 135                   | 4              | 4 x 500              | 1 / mérsékelt              |
| Standard_A3 | 4         | 7            | 285                   | 8              | 8 x 500              | 2 / magas                  |
| Standard_A4 | 8         | 14           | 605                   | 16             | 16 x 500             | 4 / magas                  |
| Standard_A5 | 2         | 14           | 135                   | 4              | 4 X 500              | 1 / mérsékelt              |
| Standard_A6 | 4         | 28           | 285                   | 8              | 8 x 500              | 2 / magas                  |
| Standard_A7 | 8         | 56           | 605                   | 16             | 16 x 500             | 4 / magas                  |

## <a name="a-series---compute-intensive-instances"></a>A sorozat-számítási igényű példányok

Az adatok és a következő méretek használatával kapcsolatos szempontok című témakörben [olvashat a H – adatsorok és a számítási igényű A-sorozat VMs](../virtual-machines/virtual-machines-windows-a8-a9-a10-a11-specs.md).


| Mérete         | Processzormagok | Memória: orrot | Helyi merevlemez: orrot | Max adatok lemez | Max fájlmegosztásra lemez: IOPS | Max NIC / hálózati sávszélességet |
|--------------|-----------|--------------|-----------------------|----------------|--------------------|-----------------------|
| Standard_A8 * | 8         | 56           | 382                   | 16             | 16 x 500             | 2 / magas                  |
| Standard_A9 * | 16        | 112          | 382                   | 16             | 16 x 500             | 4 / nagyon magas             |
| Standard_A10 | 8         | 56           | 382                   | 16             | 16 x 500             | 2 / magas                  |
| Standard_A11 | 16        | 112          | 382                   | 16             | 16 x 500             | 4 / nagyon magas             |

* RDMA alkalmas

## <a name="d-series"></a>D-sorozat


| Mérete         | Processzormagok | Memória: orrot | Helyi SSD: orrot | Max adatok lemez | Max fájlmegosztásra lemez: IOPS | Max NIC / hálózati sávszélességet |
|--------------|-----------|--------------|----------------------|----------------|--------------------|-----------------------|
| Standard_D1  | 1         | 3.5-ös          | 50                   | 2              | 2 x 500              | 1 / közepes              |
| Standard_D2  | 2         | 7            | 100                  | 4              | 4 x 500              | 2 / magas                  |
| Standard_D3  | 4         | 14           | 200                  | 8              | 8 x 500              | 4 / magas                  |
| Standard_D4  | 8         | 28           | 400                  | 16             | 16 x 500             | 8 / magas                  |
| Standard_D11 | 2         | 14           | 100                  | 4              | 4 x 500              | 2 / magas                  |
| Standard_D12 | 4         | 28           | 200                  | 8              | 8 x 500              | 4 / magas                  |
| Standard_D13 | 8         | 56           | 400                  | 16             | 16 x 500             | 8 / magas                  |
| Standard_D14 | 16        | 112          | 800                  | 32             | 32 x 500             | 8 / nagyon magas             |

## <a name="dv2-series"></a>Dv2-sorozat

| Mérete            | Processzormagok | Memória: orrot | Helyi SSD: orrot | Max adatok lemez | Max fájlmegosztásra lemez: IOPS | Max NIC / hálózati sávszélességet |
|-----------------|-----------|--------------|----------------------|----------------|--------------------|-----------------------|
| Standard_D1_v2  | 1         | 3.5-ös          | 50                   | 2              | 2 x 500              | 1 / mérsékelt              |
| Standard_D2_v2  | 2         | 7            | 100                  | 4              | 4 x 500              | 2 / magas                  |
| Standard_D3_v2  | 4         | 14           | 200                  | 8              | 8 x 500              | 4 / magas                  |
| Standard_D4_v2  | 8         | 28           | 400                  | 16             | 16 x 500             | 8 / magas                  |
| Standard_D5_v2  | 16        | 56           | 800                  | 32             | 32 x 500             | 8 / nagyon magas        |
| Standard_D11_v2 | 2         | 14           | 100                  | 4              | 4 x 500              | 2 / magas                  |
| Standard_D12_v2 | 4         | 28           | 200                  | 8              | 8 x 500              | 4 / magas                  |
| Standard_D13_v2 | 8         | 56           | 400                  | 16             | 16 x 500             | 8 / magas                  |
| Standard_D14_v2 | 16        | 112          | 800                  | 32             | 32 x 500             | 8 / nagyon magas        |
| Standard_D15_v2 | 20        | 140          | 1000                | 40             | 40 x 500             | 8 / nagyon magas        |

## <a name="g-series"></a>G-sorozat

| Mérete        | Processzormagok | Memória: orrot  | Helyi SSD: orrot  | Max adatok lemez | Max lemez átviteli: IOPS | Max NIC / hálózati sávszélességet |
|-------------|-----------|--------------|----------------------|----------------|--------------------|-----------------------|
| Standard_G1 | 2         | 28           | 384                  | 4              | 4 x 500            | 1 / magas                  |
| Standard_G2 | 4         | 56           | 768                  | 8              | 8 x 500            | 2 / magas                  |
| Standard_G3 | 8         | 112          | 1,536                | 16             | 16 x 500           | 4 / nagyon magas             |
| Standard_G4 | 16        | 224          | 3072                | 32             | 32 x 500           | 8 / nagyon magas        |
| Standard_G5 | 32        | 448          | 6,144                | 64             | 64 x 500           | 8 / nagyon magas        |


## <a name="h-series"></a>H – sorozat

Azure H – adatsor virtuális gépeken futó számítások VMs magas vége számítási igényeinek, például a molekuláris modellezési és a számítási tételéhez dynamics célja a következő generációs nagy teljesítményű. Ezek a 8 és 16 core VMs az Intel Haswell E5-2667 V3 processzor technológiára DDR4 memória és a helyi alapú SSD tároló vásárolt készültek. 

A H sorozat a mellett a jelentős Processzor power a kis késés RDMA hálózati FDR InfiniBand és több memóriát konfiguráció használata támogató memóriahasználat intenzív számítási követelmények különböző lehetőséget is kínál.


| Mérete           | Processzormagok | Memória: orrot | Helyi SSD: orrot | Max adatok lemez | Max lemezre kapacitása: IOPS | Max NIC / hálózati sávszélességet |
|----------------|-----------|-------------|--------------------------|----------------|---------------------------|------------------------------|
| Standard_H8    | 8         | 56          | 1000                     | 16             | 16 x 500                    | 8 / magas                      |
| Standard_H16   | 16        | 112         | 2000                     | 32             | 32 x 500                    | 8 / nagyon magas                  |
| Standard_H8m   | 8         | 112         | 1000                     | 16             | 16 x 500                    | 8 / magas                      |
| Standard_H16m  | 16        | 224         | 2000                     | 32             | 32 x 500                    | 8 / nagyon magas                 |
| Standard_H16r * | 16        | 112         | 2000                     | 32             | 32 x 500                    | 8 / nagyon magas                  |
| Standard_H16mr * | 16        | 224         | 2000                     | 32             | 32 x 500                    | 8 / nagyon magas                  |


* RDMA alkalmas

## <a name="notes-standard-a0---a4-using-cli-and-powershell"></a>Megjegyzések: Szabványos A0 – A4 CLI és a PowerShell használatával 

A klasszikus telepítési modell megjelennek a CLI és PowerShell némileg eltérő néhány virtuális méretét nevek:

* Standard_A0 ExtraSmall 
* Standard_A1 kis
* Standard_A2: Közepes
* Standard_A3 nagy
* Standard_A4 ExtraLarge

## <a name="configure-sizes-for-cloud-services"></a>Méretű Cloud Services konfigurálása

Szerepkör-példány virtuális géphez méretének adhatja meg a [szolgáltatás-definíciós fájl](cloud-services-model-and-package.md#csdef)által megjelölt szolgáltatás modell részeként. A szerepkör méretének határozza meg, hogy a Processzor magmintákat, a memóriahasználat beosztását, és a helyi rendszer fájlméret futó példány kiosztott számát. Válassza az alkalmazás erőforrás követelmény alapján szerepkör méretét.

Íme egy példa a szerepkör méretét [Standard_D2](#general-purpose-d) a webes szerepkör-példány beállításához:

```xml
<WorkerRole name="Worker1" vmsize="<mark>Standard_D2</mark>">
...
</WorkerRole>
```

## <a name="next-steps"></a>Következő lépések

- Tudnivalók [azure előfizetés és szolgáltatás korlátozások, kvótákat, és korlátok](../azure-subscription-service-limits.md).
- Ismerje meg a további [tudnivalók a H – adatsorok és a számítási igényű A-sorozat VMs](../virtual-machines/virtual-machines-windows-a8-a9-a10-a11-specs.md) munkaterhelésekből, például a nagy teljesítményű számítások (HPC).

