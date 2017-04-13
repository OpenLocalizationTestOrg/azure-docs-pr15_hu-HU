<properties 
   pageTitle="Útválasztási határozzák meg, és használja a PowerShell használata a klasszikus telepítési modell virtuális készülékek |} Microsoft Azure"
   description="Megtudhatja, hogy miként szabályozhatja a PowerShell használata a klasszikus telepítési modell VNets köröztetése"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor=""
   tags="azure-service-management"
/>
<tags  
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/02/2016"
   ms.author="jdial" />

#<a name="control-routing-and-use-virtual-appliances-classic-using-powershell"></a>Továbbítás határozzák meg, és használja a virtuális készülékek (klasszikus) PowerShell használatával

[AZURE.INCLUDE [virtual-network-create-udr-classic-selectors-include.md](../../includes/virtual-network-create-udr-classic-selectors-include.md)]

[AZURE.INCLUDE [virtual-network-create-udr-intro-include.md](../../includes/virtual-network-create-udr-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Ez a cikk bemutatja a klasszikus telepítési modell.

[AZURE.INCLUDE [virtual-network-create-udr-scenario-include.md](../../includes/virtual-network-create-udr-scenario-include.md)]

A minta Azure PowerShell-parancsok az alábbi fejlemények már létrehozott egy egyszerű környezet alapján az alkalmazási példát. Ha szeretne a dokumentumban megjelenített parancsokat, hozzon létre a környezet, [Hozzon létre egy (klasszikus) PowerShell használatával VNet](virtual-networks-create-vnet-classic-netcfg-ps.md)látható.

[AZURE.INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="create-the-udr-for-the-front-end-subnet"></a>Az előtér-alhálózat a UDR létrehozása
Az útvonal tábla és a alapján a forgatókönyv a fenti az előtér-alhálózat szükséges útvonal létrehozásához kövesse az alábbi lépéseket.

3. Futtassa a **`New-AzureRouteTable`** parancsmag az előtér-alhálózat útvonal táblázatot szeretne létrehozni.

        New-AzureRouteTable -Name UDR-FrontEnd `
            -Location uswest `
            -Label "Route table for front end subnet"

    Kimenet:

        Name         Location   Label                          
        ----         --------   -----                          
        UDR-FrontEnd West US    Route table for front end subnet

4. Futtassa a **`Set-AzureRoute`** parancsmag útvonalat az útvonal táblázat létrehozásához előbb létrehozott küldése az összes adatforgalmat a hátsó vége alhálózathoz (192.168.2.0/24) **FW1** virtuális (192.168.0.4).
    
        Get-AzureRouteTable UDR-FrontEnd `
            |Set-AzureRoute -RouteName RouteToBackEnd -AddressPrefix 192.168.2.0/24 `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress 192.168.0.4

    Kimenet:

        Name     : UDR-FrontEnd
        Location : West US
        Label    : Route table for frontend subnet
        Routes   : 
                   Name                 Address Prefix    Next hop type        Next hop IP address
                   ----                 --------------    -------------        -------------------
                   RouteToBackEnd       192.168.2.0/24    VirtualAppliance     192.168.0.4  

5. Futtassa a **`Set-AzureSubnetRouteTable`** a **FrontEnd** alhálózat az előbb létrehozott parancsmag társíthatja az útvonal táblázat.

        Set-AzureSubnetRouteTable -VirtualNetworkName TestVNet `
            -SubnetName FrontEnd `
            -RouteTableName UDR-FrontEnd
 
## <a name="create-the-udr-for-the-back-end-subnet"></a>A visszahívási vége alhálózat a UDR létrehozása
Az útvonal táblázat és az alkalmazási példát alapján vissza vége alhálózat szükséges útvonal létrehozásához kövesse az alábbi lépéseket.

3. Futtassa a **`New-AzureRouteTable`** parancsmagot a hátsó vége alhálózat útvonal táblázatot szeretne létrehozni.

        New-AzureRouteTable -Name UDR-BackEnd `
            -Location uswest `
            -Label "Route table for back end subnet"

4. Futtassa a **`Set-AzureRoute`** parancsmag útvonalat az útvonal táblázat létrehozásához előbb létrehozott küldése az összes adatforgalmat az előtér alhálózathoz (192.168.1.0/24) **FW1** virtuális (192.168.0.4).

        Get-AzureRouteTable UDR-BackEnd `
            |Set-AzureRoute -RouteName RouteToFrontEnd -AddressPrefix 192.168.1.0/24 `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress 192.168.0.4

5. Futtassa a **`Set-AzureSubnetRouteTable`** a **Kódmentes** alhálózat az előbb létrehozott parancsmag társíthatja az útválasztási tábla.

        Set-AzureSubnetRouteTable -VirtualNetworkName TestVNet `
            -SubnetName BackEnd `
            -RouteTableName UDR-BackEnd

## <a name="enable-ip-forwarding-on-the-fw1-vm"></a>Kattintson a FW1 virtuális IP-továbbítás engedélyezése
Ha engedélyezni szeretné a hívásátirányítás a FW1 virtuális IP, kövesse az alábbi lépéseket.

1. Futtassa a **`Get-AzureIPForwarding`** parancsmag IP-továbbítás állapotának ellenőrzése

        Get-AzureVM -Name FW1 -ServiceName TestRGFW `
            | Get-AzureIPForwarding

    Kimenet:

        Disabled

2. Futtassa a **`Set-AzureIPForwarding`** ahhoz, hogy az *FW1* virtuális IP-továbbítás parancsot.

        Get-AzureVM -Name FW1 -ServiceName TestRGFW `
            | Set-AzureIPForwarding -Enable
