<properties
 pageTitle="Javasolt értékek kiszámítására az Linux VMs |} Microsoft Azure"
 description="Az Azure VMs Linux operációs rendszert futtató CoreMark számítási javasolt értékek összehasonlítása"
 services="virtual-machines-linux"
 documentationCenter=""
 authors="cynthn"
 manager="timlt"
 editor=""
 tags="azure-resource-manager,azure-service-management"/>
<tags
ms.service="virtual-machines-linux"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-linux"
 ms.workload="infrastructure-services"
 ms.date="09/22/2016"
 ms.author="cynthn"/>

# <a name="compute-benchmark-scores-for-linux-vms"></a>Javasolt értékek kiszámítására az Linux VMs

A következő CoreMark javasolt értékek megjelenítése Azure a nagy teljesítményű virtuális következőhöz Ubuntu futó számítási teljesítményt. Számítási javasolt értékek [Windows VMs](virtual-machines-windows-compute-benchmark-scores.md)is elérhetők.




## <a name="a-series---compute-intensive"></a>A sorozat-számítási igényű


Mérete | vCPUs | NUMA csomópontok | PROCESSZOR | Fut | Iterációk száma/sec | Szórás
------- | ------ | ---- | -------| ---- | ---- | -----
Standard_A8 | 8 | 1 | Intel Xeon Processzor E5-2670 0 @ 2.6 GHz-es| 179 | 110,294 | 554
Standard_A9 | 16 | 2 | Intel Xeon Processzor E5-2670 0 @ 2.6 GHz-es| 189 | 210,816| 2,126
Standard_A10 | 8 | 1 | Intel Xeon Processzor E5-2670 0 @ 2.6 GHz-es| 188 | 110,025 | 1,045
Standard_A11 | 16 | 2 | Intel Xeon Processzor E5-2670 0 @ 2.6 GHz-es| 188 | 210,727| 2,073

## <a name="dv2-series"></a>Dv2-sorozat


Mérete | vCPUs | NUMA csomópontok | PROCESSZOR | Fut | Iterációk száma/sec | Szórás
------- | ------ | ---- | -------| ---- | ---- | -----
Standard_D1_v2 | 1 | 1 | Intel Xeon E5-2673 v3 @ 2,4 GHz-es | 140 | 14,852 | 780
Standard_D2_v2 | 2 | 1 | Intel Xeon E5-2673 v3 @ 2,4 GHz-es | 133 | 29,467 | 1,863
Standard_D3_v2 | 4 | 1 | Intel Xeon E5-2673 v3 @ 2,4 GHz-es | 139 | 56,205 | 1,167
Standard_D4_v2 | 8 | 1 | Intel Xeon E5-2673 v3 @ 2,4 GHz-es | 126 | 108,543 | 3,446
Standard_D5_v2 | 16 | 2 | Intel Xeon E5-2673 v3 @ 2,4 GHz-es | 126 | 205,332 | 9,998
Standard_D11_v2 | 2 | 1 | Intel Xeon E5-2673 v3 @ 2,4 GHz-es | 125 | 28,598 | 1,510
Standard_D12_v2 | 4 | 1 | Intel Xeon E5-2673 v3 @ 2,4 GHz-es | 131 | 55,673 | 1,418
Standard_D13_v2 | 8 | 1 | Intel Xeon E5-2673 v3 @ 2,4 GHz-es | 140 | 107,986 | 3,089
Standard_D14_v2 | 16 | 2 | Intel Xeon E5-2673 v3 @ 2,4 GHz-es | 140 | 208,186 | 8,839
Standard_D15_v2 | 20 | 2 | Intel Xeon E5-2673 v3 @ 2,4 GHz-es |28 | 268,560 | 4,667

## <a name="f-series"></a>Az F-sorozat

Mérete | vCPUs | NUMA csomópontok | PROCESSZOR | Fut | Iterációk száma/sec | Szórás
------- | ------ | ---- | -------| ---- | ---- | -----
Standard_F1 | 1 | 1 | Intel Xeon E5-2673 v3 @ 2,4 GHz-es | 154 | 15,602 | 787
Standard_F2 | 2 | 1 | Intel Xeon E5-2673 v3 @ 2,4 GHz-es | 126 | 29,519 | 1,233
Standard_F4 | 4 | 1 | Intel Xeon E5-2673 v3 @ 2,4 GHz-es | 147 | 58,709 | 1,227
Standard_F8 | 8 | 1 | Intel Xeon E5-2673 v3 @ 2,4 GHz-es | 224 | 112,772 | 3,006
Standard_F16 | 16 | 2 | Intel Xeon E5-2673 v3 @ 2,4 GHz-es | 42 | 218,571 | 5,113



## <a name="g-series"></a>G-sorozat


Mérete | vCPUs | NUMA csomópontok | PROCESSZOR | Fut | Iterációk száma/sec | Szórás
------- | ------ | ---- | -------| ---- | ---- | -----
Standard_G1 | 2 | 1 | Intel Xeon E5-2698B v3 @ 2 GHz-es | 83 | 31,310 | 2,891
Standard_G2 | 4 | 1 | Intel Xeon E5-2698B v3 @ 2 GHz-es | 84 | 60,112 | 3,537
Standard_G3 | 8 | 1 | Intel Xeon E5-2698B v3 @ 2 GHz-es | 84 | 107,522 | 4,537
Standard_G4 | 16 | 1 | Intel Xeon E5-2698B v3 @ 2 GHz-es | 83 | 195,116 | 5,024
Standard_G5 | 32 | 2 | Intel Xeon E5-2698B v3 @ 2 GHz-es | 84 | 360,329 | 14,212

## <a name="gs-series"></a>GS-sorozat


Mérete | vCPUs | NUMA csomópontok | PROCESSZOR | Fut | Iterációk száma/sec | Szórás
------- | ------ | ---- | -------| ---- | ---- | -----
Standard_GS1 | 2 | 1 | Intel Xeon E5-2698B v3 @ 2 GHz-es | 84 | 28,613 | 1,884
Standard_GS2 | 4 | 1 | Intel Xeon E5-2698B v3 @ 2 GHz-es | 83 | 54,348 | 3,474
Standard_GS3 | 8 | 1 | Intel Xeon E5-2698B v3 @ 2 GHz-es | 83 | 104,564 | 1,834
Standard_GS4 | 16 | 1 | Intel Xeon E5-2698B v3 @ 2 GHz-es | 84 | 194,111 | 4,735
Standard_GS5 | 32 | 2 | Intel Xeon E5-2698B v3 @ 2 GHz-es | 84 | 357,396 | 16,228


## <a name="h-series"></a>H – sorozat

Mérete | vCPUs | NUMA csomópontok | PROCESSZOR | Fut | Iterációk száma/sec | Szórás
------- | ------ | ---- | -------| ---- | ---- | -----
Standard_H8 | 8 | 1 | Intel Xeon E5-2667 v3 @ 3,2 GHz-es | 28 | 140,782 | 2,512
Standard_H16 | 16 | 2 | Intel Xeon E5-2667 v3 @ 3,2 GHz-es | 35-tel | 275,289 | 7,110 
Standard_H18m | 8 | 1 | Intel Xeon E5-2667 v3 @ 3,2 GHz-es | 28 | 139,071 | 3,988 
Standard_H16m | 16 | 2 | Intel Xeon E5-2667 v3 @ 3,2 GHz-es | 28 | 275,988 | 6,963 
Standard_H16r | 16 | 2 | Intel Xeon E5-2667 v3 @ 3,2 GHz-es | 28 | 273,982 | 6,069 
Standard_H16mr | 16 | 2 | Intel Xeon E5-2667 v3 @ 3,2 GHz-es | 28 | 274,523 | 5 698. 



## <a name="about-coremark"></a>Tudnivalók a CoreMark

Linux számok számítja ki [CoreMark](http://www.eembc.org/coremark/faq.php) futó Ubuntu volt. CoreMark és a szám beállítása a virtuális processzorok száma szálak konfigurálásáról, és feldolgozási PThreads értékűre. A cél közelítések számának lett beállítani egy futtatókörnyezet legalább 20 másodperc (általában sokkal hosszabb) megadására várható teljesítmény alapján. A végeredmény a befejeződött közelítések számának osztva a vizsgálat futtatásához tartott másodpercek számát jelenti. Minden vizsgálat minden virtuális legalább hét hányszor volt futtathat. Azt vizsgálja, (kivéve a H – series_ több VMs minden Azure nyilvános területen kattintson a Skype 2015 vállalati október futtatása a virtuális támogatja az azon a napon futtatása.

## <a name="next-steps"></a>Következő lépések



* Tárterület kapacitás lemez és a további szempontok virtuális méretek közül kiválasztja a témakörben [virtuális gépeken futó méretét](virtual-machines-linux-sizes.md).

* A CoreMark parancsprogramokat futtassanak Linux VMs, töltse le a [CoreMark parancsfájl csomagot](http://download.microsoft.com/download/3/0/5/305A3707-4D3A-4599-9670-AAEB423B4663/AzureCoreMarkScriptPack.zip).