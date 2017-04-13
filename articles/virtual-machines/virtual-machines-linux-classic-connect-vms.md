<properties
    pageTitle="Linux VMs csatlakozás egy felhőalapú szolgáltatásba a |} Microsoft Azure"
    description="Csatlakozás Linux rendszerhez készült Azure felhőszolgáltatásba vagy virtuális hálózati a klasszikus telepítési modell virtuális gépeken futó."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/06/2016"
    ms.author="cynthn"/>

# <a name="connect-linux-virtual-machines-created-with-the-classic-deployment-model-with-a-virtual-network-or-cloud-service"></a>Csatlakozás Linux virtuális gépeken futó virtuális hálózati vagy felhőalapú szolgáltatást a klasszikus telepítési modell készült

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

A klasszikus telepítési modell készült Linux virtuális gépeken futó mindig kerülnek egy felhőalapú szolgáltatásba. A tároló végpontként felhőszolgáltatásokból nyilvános DNS egyedi nevet, egy nyilvános IP-címet és egy sor olyan végpontjait a virtuális gép eléréséhez az interneten keresztül biztosít. A felhőbeli szolgáltatástól lehet virtuális hálózatot, de ez nem kötelező. Is [csatlakoztatása a Windows virtuális gépeken futó virtuális hálózati vagy felhőalapú szolgáltatást](virtual-machines-windows-classic-connect-vms.md).

Ha egy felhőalapú szolgáltatásba nem egy virtuális hálózaton, azt egy *különálló* felhőszolgáltatásba hívják. A virtuális gépeken futó egy különálló felhőszolgáltatásában csak kommunikálni más virtuális gépeken futó a többi virtuális gépeken futó nyilvános DNS-nevekkel, és a forgalom utazik az interneten keresztül. Ha egy felhőalapú szolgáltatásba virtuális hálózat, a virtuális gépeken futó felhőalapú szolgáltatás a kommunikálni az összes többi virtuális gépeken futó a virtuális hálózaton anélkül, hogy minden forgalom küldése az interneten keresztül.

Ha a virtuális gépeken futó helyez ugyanazt a különálló felhőalapú szolgáltatást, terheléselosztás és elérhetőségének beállítása továbbra is használhatja. A részletekért olvassa [terheléselosztás virtuális gépeken futó](virtual-machines-linux-load-balance.md) és [kezelése a virtuális gépeken futó elérhetőségét](virtual-machines-linux-manage-availability.md). Azonban nem rendszerezése a virtuális gépeken futó a alhálózat, vagy egy különálló felhőszolgáltatásba csatlakoztatása a helyszíni hálózaton. Lássunk egy példát:

[AZURE.INCLUDE [virtual-machines-common-classic-connect-vms](../../includes/virtual-machines-common-classic-connect-vms.md)]

## <a name="next-steps"></a>Következő lépések

Miután létrehozott egy virtuális gép, azt, aminek következtében célszerű az [adatok lemez hozzáadása](virtual-machines-linux-classic-attach-disk.md) a szolgáltatások és a feladatok kell egy helyen tárolt adatok. 



