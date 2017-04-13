<properties
   pageTitle="Módosíthatja a helyi hálózaton átjáró IP-cím prefixumokban és gateway IP |} Microsoft Azure"
   description="Ez a cikk bemutatja az IP-cím prefixumokban a helyi hálózaton átjáró módosítása"
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
   ms.date="08/08/2016"
   ms.author="cherylmc"/>

# <a name="modify-local-network-gateway-settings-using-powershell"></a>A PowerShell használatával a helyi hálózati átjáró beállításainak módosítása

Előfordul, hogy a helyi hálózati átjáró AddressPrefix vagy GatewayIPAddress beállításainak módosítása Az alábbi útmutatást segítséget nyújt a helyi hálózati átjáró beállításainak módosítása. Ezeket a beállításokat az Azure-portálon is módosíthatók.

## <a name="before-you-begin"></a>Első lépések
    
Telepítse az Azure erőforrás-kezelő PowerShell-parancsmagok legújabb verzióját kell. Megtudhatja, [hogy miként telepítheti, állíthatja Azure PowerShell](../powershell-install-configure.md) PowerShell-parancsmagok telepítésével kapcsolatos további információt.

## <a name="to-modify-ip-address-prefixes"></a>IP-cím prefixumokban módosítása

[AZURE.INCLUDE [vpn-gateway-modify-ip-prefix-rm](../../includes/vpn-gateway-modify-ip-prefix-rm-include.md)]

## <a name="to-modify-the-gateway-ip-address"></a>Az átjáró IP-cím módosítása

[AZURE.INCLUDE [vpn-gateway-modify-lng-gateway-ip-rm](../../includes/vpn-gateway-modify-lng-gateway-ip-rm-include.md)]

## <a name="next-steps"></a>Következő lépések

Ellenőrizheti, hogy az átjáró-kapcsolatot. Lásd: az [átjáró kapcsolat ellenőrzése](vpn-gateway-verify-connection-resource-manager.md).

