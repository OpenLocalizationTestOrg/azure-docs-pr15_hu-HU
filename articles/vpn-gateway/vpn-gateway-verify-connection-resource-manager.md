<properties
   pageTitle="Ellenőrizze az átjáró kapcsolat |} Microsoft Azure"
   description="Ebből a cikkből megtudhatja, hogy az erőforrás-kezelő telepítési modell átjáró kapcsolat ellenőrzése"
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/14/2016"
   ms.author="cherylmc"/>

# <a name="verify-a-gateway-connection"></a>Ellenőrizze az átjáró kapcsolat

Az átjáró kapcsolat néhány különböző módon ellenőrizheti. Ez a cikk bemutatja, hogyan ellenőrizheti az erőforrás-kezelő átjáró kapcsolat állapotának, az Azure portál használatával, és megismerkedhet a PowerShell használatával.


## <a name="verify-using-powershell"></a>Ellenőrizze a PowerShell használatával

Telepítse az Azure erőforrás-kezelő PowerShell-parancsmagok legújabb verzióját kell. PowerShell-parancsmagok telepítésével kapcsolatos további tudnivalókért megtudhatja, [hogy miként telepítheti, állíthatja Azure PowerShell](../powershell-install-configure.md). Erőforrás-kezelő parancsmagok használatával kapcsolatos további tudnivalókért olvassa el a [Windows PowerShell használatá az erőforrás-kezelő](../powershell-azure-resource-manager.md)című témakört.

### <a name="step-1-log-in-to-your-azure-account"></a>Lépés: 1: Jelentkezzen be az Azure-fiók

1. Nyissa meg a PowerShell konzol magasabb szintű jogosultságokkal rendelkező, és csatlakozni a fiókjához.

        Login-AzureRmAccount

2. Jelölje be az előfizetések a fiókhoz.

        Get-AzureRmSubscription 

3. Adja meg az előfizetést, szeretné használni.

        Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"

### <a name="step-2-verify-your-connection"></a>2 lépés: Ellenőrizze a kapcsolat


[AZURE.INCLUDE [verify connection powershell](../../includes/vpn-gateway-verify-connection-ps-rm-include.md)] 


## <a name="verify-using-the-azure-portal"></a>Ellenőrizze, hogy az Azure portál használatával

[AZURE.INCLUDE [verify connection portal](../../includes/vpn-gateway-verify-connection-portal-rm-include.md)] 


## <a name="next-steps"></a>Következő lépések

- A virtuális hálózatok virtuális gépeken futó vehet. A lépések [létrehozása egy virtuális gép](../virtual-machines/virtual-machines-windows-hero-tutorial.md) témakörben találhat.

