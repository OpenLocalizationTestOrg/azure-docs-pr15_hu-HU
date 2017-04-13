<properties
 pageTitle="Tudnivalók a számítási igényű VMs Linux |} Microsoft Azure"
 description="Háttér-információk és a H – sorozat és A8, A9, A10 és A11 számítási igényű méretű Linux VMs verzióval kapcsolatos szempontok"
 services="virtual-machines-linux"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-resource-manager,azure-service-management"/>
<tags
ms.service="virtual-machines-linux"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-linux"
 ms.workload="infrastructure-services"
 ms.date="09/21/2016"
 ms.author="danlep"/>

# <a name="about-h-series-and-compute-intensive-a-series-vms"></a>Tudnivalók a H – adatsorok és a számítási igényű A-sorozat VMs 

Az alábbiakban néhány az újabb Azure H – számozási és a korábbi A8, A9, A10 és A11 méretű, más néven *számítási igényű* példányok használatával kapcsolatos szempontok és a háttér-információk. Ez a cikk a következő méretek használatának Linux VMs koncentrál. Ez a cikk a [Windows VMs](virtual-machines-windows-a8-a9-a10-a11-specs.md)is érhető el.




[AZURE.INCLUDE [virtual-machines-common-a8-a9-a10-a11-specs](../../includes/virtual-machines-common-a8-a9-a10-a11-specs.md)]

## <a name="access-to-the-rdma-network"></a>A RDMA hálózat elérését

Az, hogy a következő futtatása egy támogatott Linux HPC terjesztését és egy támogatott MPI végrehajtása kihasználhatja az Azure RDMA hálózati RDMA internetezésre alkalmas Linux VMs fürt hozhat létre. Lásd: a [MPI alkalmazások futtatásához Linux RDMA fürt beállítása](virtual-machines-linux-classic-rdma-cluster.md) telepítési lehetőségek és a minta konfigurálási lépéseket.

* **Felosztás** - VMs kell telepítenie a Microsoft Azure piactéren RDMA internetezésre alkalmas SUSE Linux vállalati kiszolgáló (SLES) vagy OpenLogic CentOS-alapú HPC képekből. Az alábbi piactér képeket a szükséges Linux RDMA illesztőprogramokat támogatja:

    * SLES 12 HPC, SLES 12 SP1 SP1 HPC (prémium verzióban)
    
    * SLES 12-HPC, SLES 12-HPC (prémium verzióban)
    
    * 7.1 HPC centOS-alapú
    
    * 6,5 HPC centOS-alapú
    
    >[AZURE.NOTE]H – sorozat VMs azt javasoljuk HPC kép SLES 12 SP1 vagy CentOS-alapú 7.1 HPC képe.
    >
    >A CentOS-alapú HPC képek kernelfrissítéseket le vannak tiltva a **yum** konfigurációs fájl. Ennek oka a Linux RDMA illesztőprogramok kerülnek terjesztésre egy RPM csomag és illesztőprogram-frissítések esetleg nem fog működni, ha a kernel frissül.

* **MPI** – Intel MPI tár 5.x

    Attól függően, hogy a piactér képet úgy dönt, külön licencelési, a telepítés, illetve Intel MPI konfigurálása szükség lehet, az alábbi képlettel történik: 
    
    * **SLES 12 SP1 HPC kép** - telepítés az Intel MPI csomagok elosztott a virtuális meg a következő parancs futtatásával:
    
            sudo rpm -v -i --nodeps /opt/intelMPI/intel_mpi_packages/*.rpm

    * **Kép HPC SLES 12** - külön-külön regisztrálnia kell töltse le és telepítse az Intel MPI. Olvassa el az [Intel MPI tár telepítési útmutatóját](https://software.intel.com/sites/default/files/managed/7c/2c/intelmpi-2017-installguide-linux.pdf).
    
    * **CentOS-alapú HPC képek** – Intel MPI 5.1 már telepítve van.  

    További rendszerkonfiguráció MPI feladat futtatható csoportosított VMs van szükség. Ha például egy VMs fürtre, kell megbízhatónak számítási csomópontjai között. Tipikus beállítások olvassa el a [MPI alkalmazások futtatásához Linux RDMA fürt beállítása](virtual-machines-linux-classic-rdma-cluster.md)című témakört.


## <a name="considerations-for-hpc-pack-and-linux"></a>A HPC csomag és Linux kapcsolatos szempontok

[HPC csomag](https://technet.microsoft.com/library/jj899572.aspx), a Microsoft ingyenes HPC fürt és a feladat projektmenedzsment megoldás, egy beállítás, amellyel a számítási igényű példányok használata Linux biztosít. A legújabb operációs rendszere HPC csomag 2012 R2 támogatási futtatni szeretne több Linux terjesztését telepítését az Azure VMs, a Windows Server központi csomópont által felügyelt csomópontok számítja ki. RDMA internetezésre alkalmas Linux számítási csomópontjainak Intel MPI futtatása után HPC csomag is ütemezése és futtatása Linux MPI alkalmazások, a RDMA hálózathoz. Első lépésként olvassa el a [Linux számítási csomópontok HPC csomag fürt Azure-ban – első lépések](virtual-machines-linux-classic-hpcpack-cluster.md)című témakört.

## <a name="network-topology-considerations"></a>Hálózati topológiája kapcsolatos szempontok

* RDMA engedélyező Linux VMs Azure-ban, a Eth1 van fenntartva RDMA hálózati forgalmának engedélyezésére. Bármely Eth1 beállításai vagy a minden információt a hálózat hivatkozó konfigurációs fájl nem változik. Normál Azure hálózati forgalmának engedélyezésére Eth0 van fenntartva.

* Az Azure-IP fölé InfiniBand (IB) nem támogatott. Csak fölé I.B. RDMA használata támogatott.

## <a name="rdma-driver-updates-for-sles-12"></a>RDMA illesztőprogram-frissítések SLES 12

Miután létrehozott egy virtuális SLES 12 HPC képként alapján, előfordulhat, hogy módosítania kell a VMs RDMA a hálózati kapcsolat RDMA illesztőprogramokat. 

>[AZURE.IMPORTANT]Ez a lépés nem **kötelező** SLES 12 Azure régiókban HPC virtuális telepítésekhez. 
>Ez a lépés nem szükség, ha a SLES 12 SP1 HPC, 7.1 HPC CentOS-alapú vagy 6,5 HPC CentOS-alapú virtuális rendszerbe. 

Előtt frissíti a illesztőprogramok, állítsa le a folyamatok **zypper** vagy a zárolás a SUSE repó adatbázisokat a virtuális folyamatokat. Egyéb esetben a illesztőprogramok nem lehet, hogy megfelelően frissíti.  

Az egyes virtuális Linux RDMA illesztőprogramok frissítéséhez futtassa a következő készleteken Azure CLI parancsok az ügyfélszámítógép.

**A klasszikus telepítési modell kiépítve HPC virtuális SLES 12**

```
azure config mode asm

azure vm extension set <vm-name> RDMAUpdateForLinux Microsoft.OSTCExtensions 0.1
```

**Az erőforrás-kezelő telepítési modell kiépítve HPC virtuális SLES 12**

```
azure config mode arm

azure vm extension set <resource-group> <vm-name> RDMAUpdateForLinux Microsoft.OSTCExtensions 0.1
```

>[AZURE.NOTE]Eltarthat egy kis időt, a illesztőprogramjainak telepítését, és a parancs nélkül kimeneti adja eredményül. A frissítés után a virtuális újraindul, és néhány percig, amíg a használatra kész kell lennie.

### <a name="sample-script-for-driver-updates"></a>Mintaparancsfájl illesztőprogram frissítések keresése

Ha SLES 12 fürt HPC VMs telepítve van, is a az illesztőprogram frissítése minden csomópontra parancsfájlok a fürt. Például a következőt frissíti a 8-csomópont fürt illesztőprogramok.

```

#!/bin/bash -x

# Define a prefix naming scheme for compute nodes, e.g., cluster11, cluster12, etc.

vmname=cluster

# Plug in appropriate numbers in the for loop below.

for (( i=11; i<19; i++ )); do

# For VMs created in the classic deployment model, use the following command in your script.

azure vm extension set $vmname$i RDMAUpdateForLinux Microsoft.OSTCExtensions 0.1

# For VMs created in the Resource Manager deployment model, use the following command in your script.

# azure vm extension set <resource-group> $vmname$i RDMAUpdateForLinux Microsoft.OSTCExtensions 0.1

done

```


## <a name="next-steps"></a>Következő lépések

* Elérhetőségét, és a számítási igényű méretű árak kapcsolatos részletekért olvassa el a [virtuális gépeken futó árak](https://azure.microsoft.com/pricing/details/virtual-machines/#Linux)című témakört.

* Tárterület kapacitás és lemez részletei című témakörben talál [virtuális gépeken futó méretét](virtual-machines-linux-sizes.md).

* Telepítésével és a számítási igényű méretű használata RDMA Linux, című cikkben ismerkedhet [MPI alkalmazások futtatásához Linux RDMA fürt beállítása](virtual-machines-linux-classic-rdma-cluster.md).


