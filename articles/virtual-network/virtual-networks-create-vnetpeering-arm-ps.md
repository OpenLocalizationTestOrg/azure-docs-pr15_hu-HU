<properties
   pageTitle="Hozzon létre a VNet Peering Powershell-parancsmagok használata |} Microsoft Azure"
   description="Megtudhatja, hogyan hozhat létre az erőforrás-kezelő az Azure portal segítségével virtuális hálózat."
   services="virtual-network"
   documentationCenter=""
   authors="NarayanAnnamalai"
   manager="jefco"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/14/2016"
   ms.author="narayanannamalai; annahar"/>

# <a name="create-vnet-peering-using-powershell-cmdlets"></a>Hozzon létre a VNet Peering Powershell-parancsmagok használata

[AZURE.INCLUDE [virtual-networks-create-vnet-selectors-arm-include](../../includes/virtual-networks-create-vnetpeering-selectors-arm-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnetpeering-intro-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-basic-include](../../includes/virtual-networks-create-vnetpeering-scenario-basic-include.md)]

A PowerShell használatával peering VNet létrehozásához kövesse az alábbi lépéseket:

1. Ha még sosem használt Azure PowerShell, megtudhatja, [hogy miként telepítése és beállítása Azure PowerShell](../powershell-install-configure.md) , és a képernyőn megjelenő utasításokat követve teljes mértékben a végén jelentkezzen be az Azure, és jelölje ki azt az előfizetést.

> [AZURE.NOTE] Modul VNet peering kezelésére szolgáló PowerShell-parancsmag a [Azure PowerShell 1,6.](http://www.powershellgallery.com/packages/Azure/1.6.0)

2. Olvassa el a virtuális hálózati objektumok:

        $vnet1 = Get-AzureRmVirtualNetwork -ResourceGroupName vnet101 -Name vnet1
        $vnet2 = Get-AzureRmVirtualNetwork -ResourceGroupName vnet101 -Name vnet2

3. VNet peering, meg kell hozzon létre egy minden irányhoz két hivatkozást. A következő lépés fog VNet peering hivatkozás létrehozása a VNet1 VNet2 az első:

        Add-AzureRmVirtualNetworkPeering -Name LinkToVNet2 -VirtualNetwork $vnet1 -RemoteVirtualNetworkId $vnet2.Id

    Kimeneti jeleníti meg:

        Name            : LinkToVNet2
        Id: /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet1/virtualNetworkPeerings/LinkToVNet2
        Etag            : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName   : vnet101
        VirtualNetworkName  : vnet1
        PeeringState        : Initiated
        ProvisioningState   : Succeeded
        RemoteVirtualNetwork    : {
                                            "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet2"
                                        }
        AllowVirtualNetworkAccess   : True
        AllowForwardedTraffic   : False
        AllowGatewayTransit : False
        UseRemoteGateways   : False
        RemoteGateways      : null
        RemoteVirtualNetworkAddressSpace : null

4. Ebben a lépésben való VNet1 VNet2 VNet peering hivatkozást hoz létre:

        Add-AzureRmVirtualNetworkPeering -Name LinkToVNet1 -VirtualNetwork $vnet2 -RemoteVirtualNetworkId $vnet1.Id

    Kimeneti jeleníti meg:

        Name            : LinkToVNet1
        Id              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet2/virtualNetworkPeerings/LinkToVNet1
        Etag            : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName   : vnet101
        VirtualNetworkName  : vnet2
        PeeringState        : Connected
        ProvisioningState   : Succeeded
        RemoteVirtualNetwork    : {
                                            "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet1"
                                        }
        AllowVirtualNetworkAccess   : True
        AllowForwardedTraffic   : False
        AllowGatewayTransit : False
        UseRemoteGateways   : False
        RemoteGateways      : null
        RemoteVirtualNetworkAddressSpace : null

5. Miután a VNet peering hivatkozást hoz létre, megjelenik a kapcsolat állapotát az alábbi:

        Get-AzureRmVirtualNetworkPeering -VirtualNetworkName vnet1 -ResourceGroupName vnet101 -Name linktovnet2

    Kimeneti jeleníti meg:

        Name            : LinkToVNet2
        Id              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet1/virtualNetworkPeerings/LinkToVNet2
        Etag            : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName   : vnet101
        VirtualNetworkName  : vnet1
        PeeringState        : Connected
        ProvisioningState   : Succeeded
        RemoteVirtualNetwork    : {
                                             "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet2"
                                        }
        AllowVirtualNetworkAccess   : True
        AllowForwardedTraffic            : False
        AllowGatewayTransit              : False
        UseRemoteGateways                : False
        RemoteGateways                   : null
        RemoteVirtualNetworkAddressSpace : null

    Létezik néhány konfigurálható tulajdonságai VNet peering:

  	|A beállítás|Leírás|Alapértelmezett|
  	|:-----|:----------|:------|
  	|AllowVirtualNetworkAccess|E a Peer VNet a Virtual_network címke része lesz a hely címe|igen|
  	|AllowForwardedTraffic|Egy peered VNet nem származó forgalom elfogadását vagy kihagyott e|nem|
  	|AllowGatewayTransit|Lehetővé teszi, hogy a partner VNet a VNet átjáró használata|nem|
  	|UseRemoteGateways|A partner VNet átjáró használata. A partner VNet rendelkeznie kell konfigurálni az átjárók és AllowGatewayTransit kijelölve. Ez a beállítás nem használható, ha van beállítva az átjárók|nem|

    Minden egyes VNet peering hivatkozásra a fenti tulajdonságok tartozik. Például AllowVirtualNetworkAccess beállítása TRUE VNet peering hivatkozás VNet1 VNet2, és állítsa be a VNet peering hivatkozás más irányban hamis.

        $LinktoVNet2 = Get-AzureRmVirtualNetworkPeering -VirtualNetworkName vnet1 -ResourceGroupName vnet101 -Name LinkToVNet2
        $LinktoVNet2.AllowForwardedTraffic = $true
        Set-AzureRmVirtualNetworkPeering -VirtualNetworkPeering $LinktoVNet2

    Get-AzureRmVirtualNetworkPeering való futtatható dupla jelölőnégyzet a tulajdonság értékét a módosítás után. A eredményéből tekintheti meg a fenti parancsmagok futtatása után AllowForwardedTraffic módosítja az IGAZ beállítása.

        Name            : LinkToVNet2
        Id          : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet1/virtualNetworkPeerings/LinkToVNet2
        Etag            : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName   : vnet101
        VirtualNetworkName  : vnet1
        PeeringState        : Connected
        ProvisioningState   : Succeeded
        RemoteVirtualNetwork    : {
                                            "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet2"
                                        }
        AllowVirtualNetworkAccess   : True
        AllowForwardedTraffic       : True
        AllowGatewayTransit     : False
        UseRemoteGateways       : False
        RemoteGateways      : null
        RemoteVirtualNetworkAddressSpace : null

    Ebben az esetben folyamatban van a peering után kell kapcsolatainak olyan virtuális gépről bármely virtuális géphez mindkét VNets kezdeményezhet. Alapértelmezés szerint AllowVirtualNetworkAccess értéke igaz, és VNet peering kiépítése a megfelelő hozzáférés-vezérlési listák engedélyezi a kommunikációt VNets között. Hálózati biztonsági csoport (NSG) szabályok adott alhálózat vagy virtuális gépeken futó Access virtuális hálózatok között finom szemek irányítását közötti kapcsolatot blokkolása továbbra is alkalmazhat.  NSG szabályok létrehozásával kapcsolatos további tudnivalókért olvassa el a [cikk](virtual-networks-create-nsg-arm-ps.md).

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-crosssub-include](../../includes/virtual-networks-create-vnetpeering-scenario-crosssub-include.md)]

PowerShell használatával előfizetésekben peering VNet létrehozásához kövesse az alábbi lépéseket:

1. Jelentkezzen be az Azure-jogosultsággal rendelkező felhasználó-A adatait a fiókhoz előfizetés-és futtatása a következő parancsmagot:

        New-AzureRmRoleAssignment -SignInName <UserB ID> -RoleDefinitionName "Network Contributor" -Scope /subscriptions/<Subscription-A-ID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/VirtualNetworks/VNet5

    Ez a követelmény, nem peering hozható létre, ha a felhasználóknak egyenként előléptetése peering kérelmek esetében a megfelelő VNets, mindaddig, amíg a kérések megfelelően. A többi VNet jogosultsággal rendelkező felhasználó hozzáadása a helyi VNet felhasználói megkönnyíti a beállítási lehetőségek.

2. Jelentkezzen be az Azure-fiók jogosultsággal rendelkező felhasználó-B előfizetés-b, és futtassa a következő parancsmagot:

        New-AzureRmRoleAssignment -SignInName <UserA ID> -RoleDefinitionName "Network Contributor" -Scope /subscriptions/<Subscription-B-ID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/VirtualNetworks/VNet3

3. A felhasználó-A adatait a bejelentkezési folyamat, futtassa az alábbi:

        $vnet3 = Get-AzureRmVirtualNetwork -ResourceGroupName hr-vnets -Name vnet3

        Add-AzureRmVirtualNetworkPeering -Name LinkToVNet5 -VirtualNetwork $vnet3 -RemoteVirtualNetworkId "/subscriptions/<Subscription-B-Id>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/virtualNetworks/VNet5" -BlockVirtualNetworkAccess

4. Felhasználó-B bejelentkezési munkamenetre futtassa az alábbi parancsmagot:

        $vnet5 = Get-AzureRmVirtualNetwork -ResourceGroupName vendor-vnets -Name vnet5

        Add-AzureRmVirtualNetworkPeering -Name LinkToVNet3 -VirtualNetwork $vnet5 -RemoteVirtualNetworkId "/subscriptions/<Subscriptoin-A-Id>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/virtualNetworks/VNet3" -BlockVirtualNetworkAccess

5. Után peering folyamatban van, a VNet3 virtuális gépi látnia kell bármely virtuális gép VNet5 kommunikálni.

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-transit-include](../../includes/virtual-networks-create-vnetpeering-scenario-transit-include.md)]

1. Ebben az esetben a VNet peering létrehozásához az alábbi PowerShell-parancsmagok futtathatja.  Kell a AllowForwardedTraffic tulajdonság igaz és VNET1 csatolása HubVNet, amely lehetővé teszi a bejövő forgalmat a kívül a peering VNet címterület használatára.

        $hubVNet = Get-AzureRmVirtualNetwork -ResourceGroupName vnet101 -Name HubVNet
        $vnet1 = Get-AzureRmVirtualNetwork -ResourceGroupName vnet101 -Name vnet1

        Add-AzureRmVirtualNetworkPeering -Name LinkToHub -VirtualNetwork $vnet1 -RemoteVirtualNetworkId $HubVNet.Id -AllowForwardedTraffic

        Add-AzureRmVirtualNetworkPeering -Name LinkToVNet1 -VirtualNetwork $HubVNet -RemoteVirtualNetworkId $vnet1.Id

2. Miután peering folyamatban van, olvassa el a [cikk](virtual-network-create-udr-arm-ps.md) , és megadása a felhasználó által definiált útvonalat (UDR) VNet1 forgalom átirányítása egy virtuális készülék képességeit használandó keresztül. Az útvonal a következő azonosítható címet ad meg, amikor beállíthatja a virtuális készülék a partner VNet HubVNet az IP-címére. Az alábbi minta a következő:

        $route = New-AzureRmRouteConfig -Name TestNVA -AddressPrefix 10.3.0.0/16 -NextHopType VirtualAppliance -NextHopIpAddress 192.0.1.5

        $routeTable = New-AzureRmRouteTable -ResourceGroupName VNet101 -Location brazilsouth -Name TestRT -Route $route

        $vnet1 = Get-AzureRmVirtualNetwork -ResourceGroupName VNet101 -Name VNet1

        Set-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet1 -Name subnet-1 -AddressPrefix 10.1.1.0/24 -RouteTable $routeTable

        Set-AzureRmVirtualNetwork -VirtualNetwork $vnet1

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-asmtoarm-include](../../includes/virtual-networks-create-vnetpeering-scenario-asmtoarm-include.md)]

Egy között egy klasszikus virtuális és egy erőforrás-kezelő Azure virtuális hálózat a PowerShellben peering VNet létrehozásához kövesse az alábbi lépéseket:

1. További virtuális hálózati objektum **VNET1**, az erőforrás-kezelő Azure virtuális hálózati az az alábbi képlettel történik:

        $vnet1 = Get-AzureRmVirtualNetwork -ResourceGroupName vnet101 -Name vnet1

2. Ebben az esetben peering VNet meghatározásához csak egy hivatkozás van szükség, konkrétan a **VNET1** mutató hivatkozás **VNET2**. Ezt a lépést kell arra a klasszikus VNet erőforrás-azonosító. Az erőforrás ID-formátum néz ki:

        /subscriptions/{SubscriptionID}/resourceGroups/{ResourceGroupName}/providers/Microsoft.ClassicNetwork/virtualNetworks/{VirtualNetworkName}

    Ügyeljen arra, hogy SubscriptionID ResourceGroupName és VirtualNetworkName cserélje ki a megfelelő neveket.

    Ez lehet megvalósítani a következőket:

        Add-AzureRmVirtualNetworkPeering -Name LinkToVNet2 -VirtualNetwork $vnet1 -RemoteVirtualNetworkId /subscriptions/xxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxx/resourceGroups/MyResourceGroup/providers/Microsoft.ClassicNetwork/virtualNetworks/VNET2

3. Miután a VNet peering hivatkozást hoz létre, megjelenik a kapcsolati állapot a kimenet alább látható módon:

        Name                             : LinkToVNet2
        Id                               : /subscriptions/xxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxx/resourceGroups/MyResourceGroup/providers/Microsoft.Network/virtualNetworks/VNET1/virtualNetworkPeerings/LinkToVNet2
        Etag                             : W/"acecbd0f-766c-46be-aa7e-d03e41c46b16"
        ResourceGroupName                : MyResourceGroup
        VirtualNetworkName               : VNET1
        PeeringState                     : Connected
        ProvisioningState                : Succeeded
        RemoteVirtualNetwork             : {
                                         "Id": "/subscriptions/xxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxx/resourceGroups/MyResourceGroup/providers/Microsoft.ClassicNetwork/virtualNetworks/VNET2"
                                       }
        AllowVirtualNetworkAccess        : True
        AllowForwardedTraffic            : False
        AllowGatewayTransit              : False
        UseRemoteGateways                : False
        RemoteGateways                   : null
        RemoteVirtualNetworkAddressSpace : null

## <a name="remove-vnet-peering"></a>VNet Peering eltávolítása

1.  El szeretné távolítani a peering VNet, kell futtassa a következő parancsmagot:

        Remove-AzureRmVirtualNetworkPeering  

        Remove both links, using the following commands:

        Remove-AzureRmVirtualNetworkPeering -ResourceGroupName vnet101 -VirtualNetworkName vnet1 -Name linktovnet2
        Remove-AzureRmVirtualNetworkPeering -ResourceGroupName vnet101 -VirtualNetworkName vnet1 -Name linktovnet2

2. Miután egy hivatkozást eltávolít egy VNET peering, a peer kapcsolati állapot ugrik leválasztott. Ezt az állapotot, nem lehet újra létrehozni a hivatkozást mindaddig, amíg a peer kapcsolati állapot kezdeményezett változik. Azt javasoljuk, hogy Ön hozza létre újból a VNet peering előtt távolítsa el a két hivatkozások.
