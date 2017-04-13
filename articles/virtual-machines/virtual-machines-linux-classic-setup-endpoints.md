<properties
    pageTitle="A klasszikus Linux virtuális végpontjait beállítása |} Microsoft Azure"
    description="Ismerje meg, hogyan Linux virtuális gépen való kommunikáció engedélyezése a Azure-ban egy Linux virtuális az Azure klasszikus portálon végpontok beállítása"
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
    ms.date="07/13/2016"
    ms.author="cynthn"/>

# <a name="how-to-set-up-endpoints-on-a-linux-classic-virtual-machine-in-azure"></a>Hogy miként állíthatja be a végpontok Linux klasszikus virtuális gépen Azure-ban

Az összes Linux virtuális gépeken futó használata a klasszikus telepítési modell Azure-ban létrehozott automatikusan kommunikálhat a többi virtuális gépeken futó a egy felhőalapú szolgáltatásba vagy egy virtuális hálózati magánhálózaton csatornán keresztül. Az interneten vagy más virtuális hálózatok számítógépek azonban irányítsa át a bejövő hálózati forgalmat virtuális gép végpontok van szükség. Ez a cikk a [Windows virtuális gépeken futó](virtual-machines-windows-classic-setup-endpoints.md)is érhető el.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Az **Erőforrás-kezelő** telepítési modell, a végpontok állíthatók be **Hálózati biztonsági csoportok (NSGs)**. További tudnivalókért lásd: az [portok megnyitása és végpontok] (ezek olyan virtuális-gépek-linux-nsg-quickstart.md).

Az Azure klasszikus portálon Linux virtuális gép létrehozásakor a biztonságos rendszerhéj (SSH) zárólap általában létrejön automatikusan. A virtuális gép létrehozásakor vagy később igény szerint további végpontok konfigurálhatja.
 

[AZURE.INCLUDE [virtual-machines-common-classic-setup-endpoints](../../includes/virtual-machines-common-classic-setup-endpoints.md)]

## <a name="next-steps"></a>Következő lépések

* A virtuális végpont az [Azure parancssor](../virtual-machines-command-line-tools.md)programmal is létrehozhat. Futtassa az **azure virtuális végpont létrehozása** parancsot.

* Ha létrehozott egy virtuális gép az erőforrás-kezelő telepítési modell, erőforrás-kezelő módban vezérlőelem-alapú forgalmat a virtuális [hálózati biztonsági csoportok létrehozása](../virtual-network/virtual-networks-create-nsg-arm-cli.md) az Azure CLI is használhatja.