<properties
 pageTitle="Linux HPC csomag fürt beállítások a felhőben |} Microsoft Azure"
 description="Lehetőségek létrehozhatja és kezelheti a Linux nagy teljesítményű számítások (HPC) fürt Azure felhőbeli Microsoft HPC csomaggal kapcsolatos további tudnivalók"
 services="virtual-machines-linux,cloud-services"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-resource-manager,azure-service-management,hpc-pack"/>
<tags
ms.service="virtual-machines-linux"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-linux"
 ms.workload="big-compute"
 ms.date="09/26/2016"
 ms.author="danlep"/>

# <a name="options-with-hpc-pack-to-create-and-manage-an-hpc-cluster-in-azure-for-linux-workloads"></a>Beállítások a HPC csomag létrehozása és kezelése egy HPC fürthöz Linux munkaterhelésekből az Azure-ban

[AZURE.INCLUDE [virtual-machines-common-hpcpack-cluster-options](../../includes/virtual-machines-common-hpcpack-cluster-options.md)]

Ez a cikk HPC csomag Linux munkaterhelésekből futtatásához használt beállításaival koncentrál. Is futó [Windows HPC munkaterhelésekből HPC Pack](virtual-machines-windows-hpcpack-cluster-options.md)lehetőségeit.

## <a name="run-an-hpc-pack-cluster-in-azure-vms"></a>Azure VMs egy HPC csomag fürthöz futtatása

### <a name="azure-templates"></a>Azure sablonok


* (Piactér) [Linux munkaterhelésekből HPC csomag fürthöz](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/)

* (Quickstart útmutató) [Létrehozás egy HPC fürthöz Linux csomópontok számítja ki.](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster-linux-cn)


### <a name="powershell-deployment-script"></a>A PowerShell telepítési parancsfájlt

* [Hozzon létre egy Linux HPC fürt a HPC csomag IaaS telepítési parancsfájlt](virtual-machines-linux-classic-hpcpack-cluster-powershell-script.md)

### <a name="tutorials"></a>Oktatóanyagok

* [Oktatóprogram: Első lépések a Linux számítási csomópontok HPC csomag fürt Azure-ban](virtual-machines-linux-classic-hpcpack-cluster.md)

* [Oktatóprogram: Futtatás Microsoft HPC Pack NAMD Linux kiszámítania csomópontok Azure-ban](virtual-machines-linux-classic-hpcpack-cluster-namd.md)

* [Oktatóprogram: Futtatás OpenFOAM Microsoft HPC Pack Linux RDMA fürtre Azure-ban](virtual-machines-linux-classic-hpcpack-cluster-openfoam.md)

* [Oktatóprogram: Futtatás csillag-ÉKM + Microsoft HPC Pack egy Linux RDMA a fürt Azure-ban](virtual-machines-linux-classic-hpcpack-cluster-starccm.md)

### <a name="cluster-management"></a>Csoport kezelése

* [Azure-ban HPC csomag fürthöz feladatok elküldése](virtual-machines-windows-hpcpack-cluster-submit-jobs.md)

* [A HPC csomag feladatkezelés](https://technet.microsoft.com/library/jj899585.aspx)


## <a name="create-rdma-clusters-for-mpi-workloads"></a>Az MPI munkaterhelésekből RDMA fürt létrehozása

* [Oktatóprogram: Futtatás OpenFOAM Microsoft HPC Pack Linux RDMA fürtre Azure-ban](virtual-machines-linux-classic-hpcpack-cluster-openfoam.md)

* [MPI alkalmazások futtatásához a Linux RDMA fürtre beállítása](virtual-machines-linux-classic-rdma-cluster.md)

