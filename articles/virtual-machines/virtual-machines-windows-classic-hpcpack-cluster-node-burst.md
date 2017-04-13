<properties
 pageTitle="Burst csomópontok hozzáadása egy HPC csomag fürthöz |} Microsoft Azure"
 description="Megtudhatja, hogy miként bontsa ki az Azure igény szerinti HPC csomag fürt dolgozó szerepkör-példányok futtatása egy felhőalapú szolgáltatásba hozzáadásával"
 services="virtual-machines-windows"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-service-management,hpc-pack"/>
<tags
ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-multiple"
 ms.workload="big-compute"
 ms.date="10/14/2016"
 ms.author="danlep"/>

# <a name="add-on-demand-burst-nodes-to-an-hpc-pack-cluster-in-azure"></a>Igény szerinti "burst" csomópontok hozzáadása egy HPC Pack fürthöz Azure-ban



Ha állította be a [Microsoft HPC csomag](https://technet.microsoft.com/library/cc514029) fürtre Azure-ban, érdemes lehet gyorsan méretezni a fürt kapacitás felfelé vagy lefelé, egy sor olyan előre beállított számítási csomópont VMs fenntartása nélkül lehetőséget. Ez a cikk bemutatja, hogyan igény szerinti "burst" csomópontok (dolgozó szerepkör példányok futtatása egy felhőalapú szolgáltatásba) hozzáadása egy központi csomópontjának Azure számítási erőforrásként. 

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

![Csomópontok burst][burst]

Ez a cikk lépéseinek Azure csomópontok gyors hozzáadása egy felhőalapú HPC Pack fő csomópont virtuális próba vagy vásárlási, amely telepítés nyújt segítséget. A magas szintű lépések megegyeznek a lépéseket követve "burst az Azure" hozzáadása felhő kapacitása ahhoz, hogy egy helyszíni HPC csomag fürthöz számítja ki. Egy oktatóprogram [állítson be egy hibrid számítja ki a Microsoft HPC csomag fürt](../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md)című témakör tartalmaz. Részletes útmutatást és munkakörnyezeti telepítésekhez kapcsolatos szempontok című témakörben talál [az Azure Microsoft HPC Pack Burst](https://technet.microsoft.com/library/gg481749.aspx).


## <a name="prerequisites"></a>Előfeltételek

* A **fő csomópont HPC csomag telepítését, az az Azure virtuális** - egy önálló fő csomópont virtuális vagy egy része a nagyobb fürtre is használhatja. Hozzon létre egy önálló központi csomópontot, olvassa el [az Azure virtuális egy HPC csomag vezetője csomópontjának Deploy](virtual-machines-windows-hpcpack-cluster-headnode.md). Az automatikus HPC csomag fürt telepítési lehetőségek a [beállítások létrehozása és kezelése a Windows HPC fürtre HPC csomaggal Microsoft Azure-ban](virtual-machines-windows-hpcpack-cluster-options.md)című cikkben olvashat.

    >[AZURE.TIP] A [HPC csomag IaaS telepítési parancsfájlt](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md) használatával a csoport létrehozása az Azure-ban, ha az automatikus telepítési Azure burst csomópontok is felvehet. Példák ebben a cikkben.

* **Azure előfizetés** - Azure csomópontok hozzáadása esetén választhat ugyanabban az előfizetésben telepíthető a fő csomópont virtuális, vagy egy másik előfizetést (vagy előfizetések).

* **Magmintákat kvóta** - szükség lehet az egyes kvóta növelése különösen akkor, ha úgy dönt, hogy a multicore méretű több Azure csomópontok telepítése. [Nyissa meg az online ügyfél-támogatási kérelmet](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) ingyenesen kvóta növeléséhez.

## <a name="step-1-create-a-cloud-service-and-a-storage-account-for-the-azure-nodes"></a>Lépés: 1: Hozzon létre egy felhőalapú szolgáltatásba, és az Azure csomópontok tároló fiókkal

Az Azure klasszikus portál vagy az egyenértékű eszközök használatával állítsa be az alábbi források, amelyek az Azure csomópontok telepítéséhez szükséges:

* Azure felhőszolgáltatásba
* Új Azure tárterület-fiókba

>[AZURE.NOTE] Egy meglévő felhőalapú szolgáltatást, az előfizetése nem fel újra. 

**Megfontolandó szempontok**

* Állítsa be az egyes Azure csomópont sablont, amelyet használni szeretne létrehozni egy külön felhőszolgáltatásba. Azonban több csomópont sablonok ugyanazt a tárterület-fiókot is használhatja.

* Azt javasoljuk, hogy, keresse meg a felhőbeli szolgáltatástól és a tárterület-fiókot a telepítéshez ugyanabban a Azure régióban.




## <a name="step-2-configure-an-azure-management-certificate"></a>Lépés: 2: Az adatkezelési Azure-tanúsítvány beállítása

Azure csomópontok számítási erőforrásként, szeretne felvenni a fő csomópont és feltöltése a hozzá tartozó tanúsítványt az Azure előfizetés a telepítéshez használni a kezelés tanúsítvány.

Ebben az esetben választhat az **Alapértelmezett HPC Azure Management tanúsítvány** HPC Pack telepíti, és automatikusan konfigurálja a központi csomópontra. A tanúsítvány esetén hasznos tesztelés céljából és vásárlási, amely telepítések. A tanúsítvány használni, töltse fel az előfizetés a központi csomópont virtuális C:\Program Files\Microsoft HPC csomag fájl 2012\Bin\hpccert.cer. A tanúsítvány az [Azure klasszikus portálon](https://manage.windowsazure.com)feltöltése, kattintson a **Beállítások** > **Management tanúsítványok**.

További beállítások konfigurálása a kezelés tanúsítvány [forgatókönyvek Azure Burst telepítésekhez az Azure Management-tanúsítvány beállítása](http://technet.microsoft.com/library/gg481759.aspx)című témakör tartalmaz.

## <a name="step-3-deploy-azure-nodes-to-the-cluster"></a>Lépés 3: Azure csomópontok üzembe a fürthöz



A vehet fel, és indítsa el az Azure csomópontok ebben az esetben szereplő lépések általában megegyezik egy helyszíni központi csomópont lépéseit. További tudnivalókért lásd: az alábbi szakaszok a [lépéseket követve Azure csomópontok üzembe Microsoft HPC Pack](https://technet.microsoft.com/library/gg481758.aspx):

* Azure csomópont-sablon létrehozása

* Azure csomópontok hozzáadása a Windows HPC fürthöz

* Indítsa el a (rendelkezés) az Azure csomópontok

Miután hozzáadta, és indítsa el a csomópontok, azok fürt feladatok futtatásához készen állnak.

Ha problémákat tapasztal, ha telepíti az Azure csomópontok, [Csomópontok](http://technet.microsoft.com/library/jj159097.aspx)témakörben talál kapcsolatos hibák elhárítása telepítések az Azure Microsoft HPC Pack.

## <a name="next-steps"></a>Következő lépések

* A számítási igényű példány méretet a burst csomópontok, olvassa el a szempontok [H – adatsorok és a számítási igényű A-sorozat VMs kapcsolatban](virtual-machines-windows-a8-a9-a10-a11-specs.md).

* Ha automatikusan a nagyobb vagy kisebb az Azure számítógépes erőforrásokat megfelelően fürt terhelését szeretné, olvassa el a [automatikusan a nagyobb és Azure számítási erőforrások HPC csomag fürt kisebb](virtual-machines-windows-classic-hpcpack-cluster-node-autogrowshrink.md).

<!--Image references-->
[burst]: ./media/virtual-machines-windows-classic-hpcpack-cluster-node-burst/burst.png
