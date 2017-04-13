<properties
    pageTitle="A klasszikus Windows virtuális végpontjait beállítása |} Microsoft Azure"
    description="Ismerkedjen meg a Windows virtuális az Azure klasszikus portálon végpontok beállítása engedélyezi a kommunikációt a virtuális gép Windows Azure-ban."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="cynthn"/>

# <a name="how-to-set-up-endpoints-on-a-classic-windows-virtual-machine-in-azure"></a>Hogy miként állíthatja be a végpontok a klasszikus Windows virtuális gépen Azure-ban


Az összes Windows Azure a klasszikus telepítési modell használatával létrehozott virtuális gépeken futó is automatikusan kommunikálni keresztül magánjellegű más virtuális gépeken futó a egy felhőalapú szolgáltatásba vagy egy virtuális hálózati hálózati csatornát. Az interneten vagy más virtuális hálózatok számítógépek azonban irányítsa át a bejövő hálózati forgalmat virtuális gép végpontok van szükség. Ez a cikk olyan [Linux virtuális gépeken futó](virtual-machines-linux-classic-setup-endpoints.md)is érhető el.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Az **Erőforrás-kezelő** telepítési modell, a végpontok állíthatók be **Hálózati biztonsági csoportok (NSGs)**. További tudnivalókért lásd: az [a külső hozzáférés engedélyezése a virtuális az Azure-portálon] (virtual-machines-windows-nsg-quickstart-portal.md).

A Windows virtuális gép az Azure klasszikus portálon létrehozásakor közös végpontok, például a távoli asztali és a Windows PowerShell távelérési általában meg automatikusan létrehozott. A virtuális gép létrehozásakor vagy később igény szerint további végpontok konfigurálhatja.



[AZURE.INCLUDE [virtual-machines-common-classic-setup-endpoints](../../includes/virtual-machines-common-classic-setup-endpoints.md)]

## <a name="next-steps"></a>Következő lépések

* Az Azure PowerShell-parancsmag egy virtuális végpontot beállításához, olvassa el [Hozzáadása-AzureEndpoint](https://msdn.microsoft.com/library/azure/dn495300.aspx).

* Az Azure PowerShell-parancsmag egy végpont vezérlés kezelésére, olvassa el [kezelése access (-szabályozási listák) végpontok PowerShell használatával](../virtual-network/virtual-networks-acl-powershell.md).

* Ha létrehozott egy virtuális gép az erőforrás-kezelő telepítési modell, a [hálózati biztonsági csoportok létrehozása](../virtual-network/virtual-networks-create-nsg-arm-ps.md) a vezérlőelem-alapú forgalmat a virtuális Azure PowerShell is használhatja.
