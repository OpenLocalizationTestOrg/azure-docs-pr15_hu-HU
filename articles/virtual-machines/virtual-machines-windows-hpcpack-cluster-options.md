<properties
 pageTitle="A Windows HPC csomag fürt beállítások a felhőben |} Microsoft Azure"
 description="Tudnivalók a Microsoft HPC Pack létrehozása és kezelése a Windows nagy teljesítményű számítások (HPC) fürt Azure felhőbeli beállításai"
 services="virtual-machines-windows,cloud-services,batch"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-resource-manager,azure-service-management,hpc-pack"/>
<tags
ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-windows"
 ms.workload="big-compute"
 ms.date="09/26/2016"
 ms.author="danlep"/>

# <a name="options-with-hpc-pack-to-create-and-manage-a-windows-hpc-cluster-in-azure"></a>Beállítások a HPC csomag létrehozása és kezelése a Windows HPC fürtre Azure-ban

[AZURE.INCLUDE [virtual-machines-common-hpcpack-cluster-options](../../includes/virtual-machines-common-hpcpack-cluster-options.md)]

Ez a cikk a Windows-munkaterhelésekből futtatásához HPC csomag fürt létrehozására szolgáló koncentrál. Lehetőség is hozhat létre fürt [Linux HPC munkaterhelésekből HPC Pack](virtual-machines-linux-hpcpack-cluster-options.md)futtatásához.


## <a name="run-an-hpc-pack-cluster-in-azure-vms"></a>Azure VMs egy HPC csomag fürthöz futtatása

### <a name="azure-templates"></a>Azure sablonok

* (Piactér) [Windows-munkaterhelésekből HPC csomag fürthöz](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterwindowscn/)

* (Piactér) [Az Excel-munkaterhelésekből HPC csomag fürthöz](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterexcelcn/)

* (Quickstart útmutató) [Egy HPC fürthöz létrehozása](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster)

* (Quickstart útmutató) [Egyéni számítási csomópont képe egy HPC fürthöz létrehozása](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster-custom-image)

### <a name="azure-vm-images"></a>Azure virtuális képek

* [A Windows Server 2012 R2 HPC csomag](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/)

* [A Windows Server 2012 R2 számítási csomópont HPC csomag](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2computenodeonwindowsserver2012r2/)

* [HPC csomag csomópontot, az Excel alkalmazás esetén kattintson a Windows Server 2012 R2 számítja ki.](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2computenodewithexcelonwindowsserver2012r2/)



### <a name="powershell-deployment-script"></a>A PowerShell telepítési parancsfájlt

* [Hozzon létre egy HPC fürthöz a HPC csomag IaaS telepítési parancsfájlt](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md)

### <a name="tutorials"></a>Oktatóanyagok

* [Oktatóprogram: Az Excel és a SOA munkaterhelésekből futtatásához Azure-ban egy HPC csomag fürthöz – első lépések](virtual-machines-windows-excel-cluster-hpcpack.md)



### <a name="manual-deployment-with-the-azure-portal"></a>Manuális telepítés az Azure portálján

* [A központi csomópont-Azure virtuális gép HPC csomag fürt beállítása](virtual-machines-windows-hpcpack-cluster-headnode.md)

### <a name="cluster-management"></a>Csoport kezelése

* [Azure-ban HPC csomag fürt számítási csomópontok kezelése](virtual-machines-windows-classic-hpcpack-cluster-node-manage.md)

* [A nagyobb és Azure számítási erőforrások HPC csomag fürt zsugorítása](virtual-machines-windows-classic-hpcpack-cluster-node-autogrowshrink.md)

* [Azure-ban HPC csomag fürthöz feladatok elküldése](virtual-machines-windows-hpcpack-cluster-submit-jobs.md)

* [A HPC csomag feladatkezelés](https://technet.microsoft.com/library/jj899585.aspx)


## <a name="add-worker-role-nodes-to-an-hpc-pack-cluster"></a>Dolgozói szerepkör csomópontok hozzáadása egy HPC Pack fürthöz


* [Azure dolgozói példányaiban HPC Pack burst](https://technet.microsoft.com/library/gg481749.aspx)

* [Oktatóprogram: Állítsa be a hibrid fürtre HPC Pack Azure-ban](../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md)

* [Azure "burst" csomópontok hozzáadása egy HPC csomag fő csomópont Azure-ban](virtual-machines-windows-classic-hpcpack-cluster-node-burst.md)


## <a name="integrate-with-azure-batch"></a>Integrálása az Azure köteg 

* [Azure köteghez HPC Pack burst](https://technet.microsoft.com/library/mt612877.aspx)

## <a name="create-rdma-clusters-for-mpi-workloads"></a>Az MPI munkaterhelésekből RDMA fürt létrehozása

* [MPI alkalmazások futtatásához a Windows RDMA fürtre HPC Pack beállítása](virtual-machines-windows-classic-hpcpack-rdma-cluster.md)
