<properties
 pageTitle="Javasolt értékek kiszámítására az Windows VMs |} Microsoft Azure"
 description="Az Azure VMs Windows Server operációs rendszert futtató SPECint számítási javasolt magyarázata összehasonlítása"
 services="virtual-machines-windows"
 documentationCenter=""
 authors="cynthn"
 manager="timlt"
 editor=""
 tags="azure-resource-manager,azure-service-management"/>
<tags
ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-windows"
 ms.workload="infrastructure-services"
 ms.date="09/22/2016"
 ms.author="cynthn"/>

# <a name="compute-benchmark-scores-for-windows-vms"></a>A Windows VMs számítja ki a javasolt statisztikákat?

A következő SPECInt javasolt értékek megjelenítése Azure a nagy teljesítményű virtuális következőhöz Windows Server operációs rendszert futtató számítási teljesítményt. Számítási javasolt értékek [Linux VMs](virtual-machines-linux-compute-benchmark-scores.md)is elérhetők.



## <a name="a-series---compute-intensive"></a>A sorozat-számítási igényű


Mérete | vCPUs | NUMA csomópontok | PROCESSZOR | Fut | AVG alap ráta | Szórás
------- | ------ | ---- | -------| ---- | ---- | -----
Standard_A8 | 8 | 1 | Intel Xeon Processzor E5-2670 0 @ 2.6 GHz-es | 10 | 236.1 | 1.1
Standard_A9 | 16 | 2 | Intel Xeon Processzor E5-2670 0 @ 2.6 GHz-es | 10 | 450.3 | 7.0-s
Standard_A10 | 8 | 1 | Intel Xeon Processzor E5-2670 0 @ 2.6 GHz-es | 5 | 235.6 | 0,9
Standard_A11 | 16 | 2 | Intel Xeon Processzor E5-2670 0 @ 2.6 GHz-es |7 | 454.7 | 4.8.

## <a name="dv2-series"></a>Dv2-sorozat


Mérete | vCPUs | NUMA csomópontok | PROCESSZOR | Fut | AVG alap ráta | Szórás
------- | ------ | ---- | -------| ---- | ---- | -----
Standard_D1_v2 | 1 | 1 | Intel Xeon E5-2673 v3 @ 2,4 GHz-es | 83 | 36.6 | 2.6
Standard_D2_v2 | 2 | 1 | Intel Xeon E5-2673 v3 @ 2,4 GHz-es | 27 | 70.0 | a 3,7
Standard_D3_v2 | 4 | 1 | Intel Xeon E5-2673 v3 @ 2,4 GHz-es | 19 | 130.5 | 4.4.
Standard_D4_v2 | 8 | 1 | Intel Xeon E5-2673 v3 @ 2,4 GHz-es | 19 | 238.1 | 5,2
Standard_D5_v2 | 16 | 2 | Intel Xeon E5-2673 v3 @ 2,4 GHz-es | 14 | 460.9 | 15.4
Standard_D11_v2 | 2 | 1 | Intel Xeon E5-2673 v3 @ 2,4 GHz-es | 19 | 70.1 | a 3,7
Standard_D12_v2 | 4 | 1 | Intel Xeon E5-2673 v3 @ 2,4 GHz-es | 2 | 132.0 | 1.4
Standard_D13_v2 | 8 | 1 | Intel Xeon E5-2673 v3 @ 2,4 GHz-es | 17 | 235.8 | 3.8.
Standard_D14_v2 | 16 | 2 | Intel Xeon E5-2673 v3 @ 2,4 GHz-es | 15 | 460.8 | 6.5 rendszeren


## <a name="g-series-gs-series"></a>G sorozat, GS-sorozat


Mérete | vCPUs | NUMA csomópontok | PROCESSZOR | Fut | AVG alap ráta | Szórás
------- | ------ | ---- | -------| ---- | ---- | -----
Standard_G1, Standard_GS1  | 2 | 1 | Intel Xeon E5-2698B v3 @ 2 GHz-es | 31. | 71.8 | 6.5 rendszeren
Standard_G2, Standard_GS2 | 4 | 1 | Intel Xeon E5-2698B v3 @ 2 GHz-es | 5 | 133.4 | 13.0
Standard_G3, Standard_GS3 | 8 | 1 | Intel Xeon E5-2698B v3 @ 2 GHz-es | 6 | 242.3 | 6.0 alkalmazásban
Standard_G4, Standard_GS4 | 16 | 1 | Intel Xeon E5-2698B v3 @ 2 GHz-es | 15 | 398.9 | 6.0 alkalmazásban
Standard_G5, Standard_GS5 | 32 | 2 | Intel Xeon E5-2698B v3 @ 2 GHz-es | 22-es hibakód | 762.8 | a 3,7

## <a name="h-series"></a>H – sorozat

Mérete | vCPUs | NUMA csomópontok | PROCESSZOR | Fut | Iterációk száma/sec | Szórás
------- | ------ | ---- | -------| ---- | ---- | -----
Standard_H8 | 8 | 1 | Intel Xeon E5-2667 v3 @ 3,2 GHz-es | 5 | 297.4 | 0,9
Standard_H16 | 16 | 2 | Intel Xeon E5-2667 v3 @ 3,2 GHz-es | 5 | 575.8 | 6.8
Standard_H8m | 8 | 1 | Intel Xeon E5-2667 v3 @ 3,2 GHz-es | 5 | 297.0 | az 1.2
Standard_H16m | 16 | 2 | Intel Xeon E5-2667 v3 @ 3,2 GHz-es | 5 | 572.2 | 3.9.
Standard_H16r | 16 | 2 | Intel Xeon E5-2667 v3 @ 3,2 GHz-es | 5 | 573.2 | 2.9
Standard_H16mr | 16 | 2 | Intel Xeon E5-2667 v3 @ 3,2 GHz-es | 7 | 569.6 | 2,8

## <a name="about-specint"></a>Tudnivalók a SPECint

A Windows számok számítja ki [SPECint 2006](https://www.spec.org/cpu2006/results/rint2006.html) Windows kiszolgálón futó volt. SPECint futtatásának funkcióval alap ráta (SPECint_rate2006), az alapvető egy példányt. SPECint, ahol a 12 külön vizsgálat, minden egyes háromszor futtatása, a Medián mezőbe írja be az egyes vizsgálatok véve és súlyozási őket a képernyő egy összetett pontszámhoz. E vizsgálatok futtassa volt látható az átlagos értékek megadását több VMs keresztül.


## <a name="next-steps"></a>Következő lépések

* Tárterület kapacitás lemez és a további szempontok virtuális méretek közül kiválasztja a témakörben [virtuális gépeken futó méretét](virtual-machines-windows-sizes.md).
