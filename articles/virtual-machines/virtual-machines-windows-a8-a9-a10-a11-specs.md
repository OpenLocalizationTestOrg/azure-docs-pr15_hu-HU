<properties
 pageTitle="Tudnivalók a számítási igényű VMs Windows |} Microsoft Azure"
 description="Háttér-információk és Azure H – sorozat és A8, A9, A10 és A11 számítási igényű méretű használatához Windows VMs és felhőbeli szolgáltatások kapcsolatos szempontok"
 services="virtual-machines-windows, cloud-services"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-resource-manager,azure-service-management"/>
<tags
ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-windows"
 ms.workload="infrastructure-services"
 ms.date="09/21/2016"
 ms.author="danlep"/>

# <a name="about-h-series-and-compute-intensive-a-series-vms"></a>Tudnivalók a H – adatsorok és a számítási igényű A-sorozat VMs

Az alábbiakban néhány az újabb Azure H – számozási és a korábbi A8, A9, A10 és A11-példányok, más néven *számítási igényű* példányok használatával kapcsolatos szempontok és a háttér-információk. Ez a cikk a Windows VMs verzióval ezekben az esetekben koncentrál. Ez a cikk olyan [Linux VMs](virtual-machines-linux-a8-a9-a10-a11-specs.md)is érhető el.


[AZURE.INCLUDE [virtual-machines-common-a8-a9-a10-a11-specs](../../includes/virtual-machines-common-a8-a9-a10-a11-specs.md)]

## <a name="access-to-the-rdma-network"></a>A RDMA hálózat elérését

RDMA internetezésre alkalmas Windows Server-példányok fürt létrehozhat és üzembe a támogatott MPI megvalósítás kihasználhatja az Azure RDMA hálózati közül. A kis-késés, a nagy átviteli hálózati csak MPI forgalmához van fenntartva.

* **Operációs rendszer**
    * **Virtuális gépeken futó** – a Windows Server 2012 R2, Windows Server 2012-ben
    * **Cloud services** – a Windows Server 2012 R2, a Windows Server 2012-ben, a Windows Server 2008 R2 vendég operációs rendszer család

* **MPI** – Microsoft MPI (MS-MPI) 2012 R2 vagy újabb, Intel MPI tár 5.x

Támogatott MPI megvalósítás használja a Microsoft hálózati közvetlen felületét példányok közötti kommunikációt. Lásd: [HPC Pack MPI alkalmazások futtatásához a Windows RDMA fürtre beállítása](virtual-machines-windows-classic-hpcpack-rdma-cluster.md) és [használata több példány feladatok Azure köteg az üzenet továbbítása felület (MPI) alkalmazások futtatásához](../batch/batch-mpi.md) a telepítési lehetőségek és a minta konfigurálási lépéseket.


>[AZURE.NOTE]A RDMA internetezésre alkalmas számítási igényű VMs a HpcVmDrivers bővítmény hozzá kell adnia a VMs RDMA kapcsolódási van szükség Windows hálózati eszköz illesztőprogramjainak telepítését. A legtöbb környezetekben a HpcVmDrivers bővítmény automatikusan felkerül. Ha a bővítmény hozzáadása a saját magát, akkor olvassa el a [Bővítmények kezelése virtuális](virtual-machines-windows-classic-manage-extensions.md)van szüksége.

## <a name="considerations-for-hpc-pack-and-windows"></a>A HPC csomag és a Windows kapcsolatos szempontok

[A Microsoft HPC csomag](https://technet.microsoft.com/library/jj899572.aspx), a Microsoft ingyenes HPC fürt és a feladat projektmenedzsment megoldás, nem áll, hogy a számítási igényű példányok használata a Windows Server szükséges. Azonban még több beállítás, amellyel a számítási fürtre létrehozása az Azure futtatásához a Windows-alapú MPI-alkalmazások és más HPC munkaterhelésekből. HPC csomag 2012 R2 és újabb verziók egy futtatókörnyezet használja az Azure RDMA hálózathoz, amikor rendszerbe RDMA internetezésre alkalmas VMs MS MPI tartalmazzák.

További információt és a számítási igényű példányok használata a Windows Server HPC csomag feladatlisták olvassa el a [HPC Pack MPI alkalmazások futtatásához a Windows RDMA fürtre beállítása](virtual-machines-windows-classic-hpcpack-rdma-cluster.md)című témakört.




## <a name="next-steps"></a>Következő lépések

* Elérhetőség és a számítási igényű méretű árak kapcsolatos részletekért lásd: a [virtuális gépeken futó árak](https://azure.microsoft.com/pricing/details/virtual-machines/#Windows) , és [árak Cloud Services](https://azure.microsoft.com/pricing/details/cloud-services/).

* Tárterület kapacitás és lemez részletei című témakörben talál [virtuális gépeken futó méretét](virtual-machines-linux-sizes.md).

* Első lépésként telepítésével és használatával számítási igényű példányok HPC Pack a Windows olvassa el a [HPC Pack MPI alkalmazások futtatásához a Windows RDMA fürtre beállítása](virtual-machines-windows-classic-hpcpack-rdma-cluster.md)című témakört.

* Az Azure köteg MPI alkalmazások futtatásához A8 és A9 példányok használatáról további tudnivalókért lásd [használata több példány feladatok Azure köteg az üzenet továbbítása felület (MPI) alkalmazások futtatásához](../batch/batch-mpi.md).
