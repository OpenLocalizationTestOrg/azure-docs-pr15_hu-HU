<properties
   pageTitle="Virtuális hálózat VNet átjáró hozzá az erőforrás-kezelő és Powershellhez készült ExpressRoute |} Microsoft Azure"
   description="Ez a cikk bemutatja az egy korábban létrehozott erőforrás-kezelő VNet számára készült ExpressRoute átjáró Vnet hozzáadása"
   documentationCenter="na"
   services="expressroute"
   authors="charwen"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>

<tags 
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services" 
   ms.date="10/10/2016"
   ms.author="charwen"/>

# <a name="configure-a-virtual-network-gateway-for-expressroute-using-resource-manager-and-powershell"></a>A virtuális hálózati átjáró beállítása erőforrás-kezelő és a PowerShell használatával készült ExpressRoute


> [AZURE.SELECTOR]
- [A PowerShell - erőforrás-kezelő](expressroute-howto-add-gateway-resource-manager.md)
- [A PowerShell - klasszikus](expressroute-howto-add-gateway-classic.md)


Ez a cikk azt ismerteti, hogy a hozzáadása, átméretezése és távolíthatja el a virtuális hálózati (VNet) átjáró egy már meglévő VNet lépéseivel. A lépések ebben a konfigurációban kifejezetten a létrehozott VNets használja az **erőforrás-kezelő telepítési modell** és lesz, amely használható egy készült ExpressRoute konfigurációt. 

**Azure környezetben modellek**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

## <a name="before-beginning"></a>Mielőtt elkezdené

Ellenőrizze, hogy telepítette a fenti konfiguráció szükséges Azure PowerShell-parancsmagok (1.0.2-es verziójú vagy újabb). Ha még nem telepítette a parancsmagok, kell megtenni beállítási lépések megkezdése előtt. Azure PowerShell telepítésével kapcsolatos további tudnivalókért megtudhatja, [hogy miként telepítheti, állíthatja Azure PowerShell](../powershell-install-configure.md).


[AZURE.INCLUDE [expressroute-gateway-rm-ps](../../includes/expressroute-gateway-rm-ps-include.md)]

    
## <a name="next-steps"></a>Következő lépések

Miután létrehozta a VNet átjárót, a VNet hozzákapcsolhatja egy készült ExpressRoute áramkör. Lásd: [hivatkozás egy készült ExpressRoute áramkör virtuális hálózaton](expressroute-howto-linkvnet-arm.md).
