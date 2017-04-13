<properties 
   pageTitle="Kezelése a PowerShell használatával az erőforrás-kezelő NSGs |} Microsoft Azure"
   description="Megtudhatja, hogy miként kezelése a PowerShell használatával az erőforrás-kezelő NSGs exising"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags  
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/14/2016"
   ms.author="jdial" />

# <a name="manage-nsgs-using-powershell"></a>Kezelése a PowerShell használatá NSGs

[AZURE.INCLUDE [virtual-network-manage-arm-selectors-include.md](../../includes/virtual-network-manage-nsg-arm-selectors-include.md)]

[AZURE.INCLUDE [virtual-network-manage-nsg-intro-include.md](../../includes/virtual-network-manage-nsg-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)]klasszikus telepítési modell.

[AZURE.INCLUDE [virtual-network-manage-nsg-arm-scenario-include.md](../../includes/virtual-network-manage-nsg-arm-scenario-include.md)]

[AZURE.INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="retrieve-information"></a>Adatok beolvasása

A meglévő NSGs megtekintése, szabályok lekérése egy meglévő NSG, és megtudhatja, milyen erőforrások egy NSG társítva.

### <a name="view-existing-nsgs"></a>Meglévő NSGs megtekintése
Az előfizetés minden meglévő NSGs megtekintéséhez futtassa a `Get-AzureRmNetworkSecurityGroup` parancsmag alább látható módon.

    Get-AzureRmNetworkSecurityGroup

A várt eredmény:

    Name                 : NSG-BackEnd
    ResourceGroupName    : RG-NSG
    Location             : westus
    Id                   : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/
                           Microsoft.Network/networkSecurityGroups/NSG-BackEnd
    Etag                 : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    ResourceGuid         : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
    ProvisioningState    : Succeeded
    Tags                 :                         
    SecurityRules        : [...]
    DefaultSecurityRules : [...]
    NetworkInterfaces    : [...]
    Subnets              : [...]
    
    Name                 : NSG-FrontEnd
    ResourceGroupName    : RG-NSG
    Location             : eastus
    Id                   : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/NRP-RG/providers/
                           Microsoft.Network/networkSecurityGroups/NSG-FrontEnd
    Etag                 : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    ResourceGuid         : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
    ProvisioningState    : Succeeded
    Tags                 : 
    SecurityRules        : [...]
    DefaultSecurityRules : [...]
    NetworkInterfaces    : [...]
    Subnets              : [...]
                            
    Name                 : WEB1
    ResourceGroupName    : RG101
    Location             : eastus2
    Id                   : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG101/providers/M
                           icrosoft.Network/networkSecurityGroups/WEB1
    Etag                 : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    ResourceGuid         : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
    ProvisioningState    : Succeeded
    Tags                 : 
    SecurityRules        : [...]
    DefaultSecurityRules : [...]
    NetworkInterfaces    : [...]
    Subnets              : [...]


Egy adott erőforráscsoport NSGs listájának megtekintéséhez futtassa a `Get-AzureRmNetworkSecurityGroup` parancsmag alább látható módon. 

    Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG

Várt kimenet:

    Name                 : NSG-BackEnd
    ResourceGroupName    : RG-NSG
    Location             : westus
    Id                   : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/
                           Microsoft.Network/networkSecurityGroups/NSG-BackEnd
    Etag                 : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    ResourceGuid         : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
    ProvisioningState    : Succeeded
    Tags                 :                         
    SecurityRules        : [...]
    DefaultSecurityRules : [...]
    NetworkInterfaces    : [...]
    Subnets              : [...]
    
    Name                 : NSG-FrontEnd
    ResourceGroupName    : RG-NSG
    Location             : eastus
    Id                   : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/NRP-RG/providers/
                           Microsoft.Network/networkSecurityGroups/NSG-FrontEnd
    Etag                 : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    ResourceGuid         : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
    ProvisioningState    : Succeeded
    Tags                 : 
    SecurityRules        : [...]
    DefaultSecurityRules : [...]
    NetworkInterfaces    : [...]
    Subnets              : [...]
         
### <a name="list-all-rules-for-an-nsg"></a>A lista összes szabály egy NSG

Egy névvel ellátott **NSG-FrontEnd**NSG szabályok megtekintéséhez futtassa a `Get-AzureRmNetworkSecurityGroup` parancsmag alább látható módon. 

    Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd | Select SecurityRules -ExpandProperty SecurityRules

Várt kimenet:
    
    Name                     : rdp-rule
    Id                       : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/                        Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/rdp-rule
    Etag                     : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    ProvisioningState        : Succeeded
    Description              : Allow RDP
    Protocol                 : Tcp
    SourcePortRange          : *
    DestinationPortRange     : 3389
    SourceAddressPrefix      : Internet
    DestinationAddressPrefix : *
    Access                   : Allow
    Priority                 : 100
    Direction                : Inbound
    
    Name                     : web-rule
    Id                       : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/                        Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/web-rule
    Etag                     : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    ProvisioningState        : Succeeded
    Description              : Allow HTTP
    Protocol                 : Tcp
    SourcePortRange          : *
    DestinationPortRange     : 80
    SourceAddressPrefix      : Internet
    DestinationAddressPrefix : *
    Access                   : Allow
    Priority                 : 101
    Direction                : Inbound

>[AZURE.NOTE] Is használhatja `Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name "NSG-FrontEnd" | Select DefaultSecurityRules -ExpandProperty DefaultSecurityRules` **NSG-FrontEnd** NSG az alapértelmezett szabályok listáját.

### <a name="view-nsgs-associations"></a>NSGs társítások megtekintése

Ha szeretné megtekinteni, milyen erőforrások a **NSG-FrontEnd** NSG társítása, futtassa a `Get-AzureRmNetworkSecurityGroup` parancsmag alább látható módon.

    Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd

Keresse meg a következőképpen **NetworkInterfaces** és **alhálózat** tulajdonságai:

    NetworkInterfaces    : []
    Subnets              : [
                             {
                               "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                               "IpConfigurations": []
                             }
                           ]

A fenti példában a NSG nem tartozik minden olyan hálózati kapcsolatok (NIC), és hozzá rendelve **FrontEnd**nevű alhálózat.

## <a name="manage-rules"></a>Szabályok kezelése

Szabályok hozzáadása egy meglévő NSG, meglévő szabályok szerkesztése és eltávolítása a szabályok.

### <a name="add-a-rule"></a>A szabály hozzáadása

**Bejövő** forgalmat a bármely számítógép **NSG-FrontEnd** NSG akiknek engedélyezi a **443-as** port szabály hozzáadásához kövesse az alábbi lépéseket.

1. Futtassa a `Get-AzureRmNetworkSecurityGroup` parancsmagot a meglévő NSG beolvasásához, és egy változó, tárolja azt alább látható módon.

        $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG `
            -Name NSG-FrontEnd

2. Futtassa a `Add-AzureRmNetworkSecurityRuleConfig` parancsmag, az alább látható módon.

        Add-AzureRmNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg `
            -Name https-rule `
            -Description "Allow HTTPS" `
            -Access Allow `
            -Protocol Tcp `
            -Direction Inbound `
            -Priority 102 `
            -SourceAddressPrefix * `
            -SourcePortRange * `
            -DestinationAddressPrefix * `
            -DestinationPortRange 443

3. A NSG módosításainak mentéséhez futtassa a `Set-AzureRmNetworkSecurityGroup` parancsmag alább látható módon.

        Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg

    Várt eredményt ad, csak a biztonsági szabályokat tartalmazó:

        Name                 : NSG-FrontEnd
        ...
        SecurityRules        : [
                                 {
                                   "Name": "rdp-rule",
                                   ...
                                 },
                                 {
                                   "Name": "web-rule",
                                   ...
                                 },
                                 {
                                   "Name": "https-rule",
                                   "Etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                                   "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/https-rule",
                                   "Description": "Allow HTTPS",
                                   "Protocol": "Tcp",
                                   "SourcePortRange": "*",
                                   "DestinationPortRange": "443",
                                   "SourceAddressPrefix": "*",
                                   "DestinationAddressPrefix": "*",
                                   "Access": "Allow",
                                   "Priority": 102,
                                   "Direction": "Inbound",
                                   "ProvisioningState": "Succeeded"
                                 }
                               ]

### <a name="change-a-rule"></a>A szabály módosítása

Ha módosítani szeretné a bejövő forgalom engedélyezése az **Internet** csak az előbb létrehozott szabályt, kövesse az alábbi lépéseket.

1. Futtassa a `Get-AzureRmNetworkSecurityGroup` parancsmagot a meglévő NSG beolvasásához, és egy változó, tárolja azt alább látható módon.

        $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG `
            -Name NSG-FrontEnd

2. Futtassa a `Set-AzureRmNetworkSecurityRuleConfig` parancsmag, az alább látható módon.

        Set-AzureRmNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg `
            -Name https-rule `
            -Description "Allow HTTPS" `
            -Access Allow `
            -Protocol Tcp `
            -Direction Inbound `
            -Priority 102 `
            -SourceAddressPrefix * `
            -SourcePortRange Internet `
            -DestinationAddressPrefix * `
            -DestinationPortRange 443

3. A NSG módosításainak mentéséhez futtassa a `Set-AzureRmNetworkSecurityGroup` parancsmag alább látható módon.

        Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg

    Várt eredményt ad, csak a biztonsági szabályokat tartalmazó:

        Name                 : NSG-FrontEnd
        ...
        SecurityRules        : [
                                 {
                                   "Name": "rdp-rule",
                                   ...
                                 },
                                 {
                                   "Name": "web-rule",
                                   ...
                                 },
                                 {
                                   "Name": "https-rule",
                                   "Etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                                   "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/https-rule",
                                   "Description": "Allow HTTPS",
                                   "Protocol": "Tcp",
                                   "SourcePortRange": "*",
                                   "DestinationPortRange": "443",
                                   "SourceAddressPrefix": "Internet",
                                   "DestinationAddressPrefix": "*",
                                   "Access": "Allow",
                                   "Priority": 102,
                                   "Direction": "Inbound",
                                   "ProvisioningState": "Succeeded"
                                 }
                               ]

### <a name="delete-a-rule"></a>Szabály törlése

1. Futtassa a `Get-AzureRmNetworkSecurityGroup` parancsmagot a meglévő NSG beolvasásához, és egy változó, tárolja azt alább látható módon.

        $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG `
            -Name NSG-FrontEnd

2. Futtassa a `Remove-AzureRmNetworkSecurityRuleConfig` parancsmag, az alább látható módon.

        Remove-AzureRmNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg `
            -Name https-rule

3. A NSG módosításainak mentéséhez futtassa a `Set-AzureRmNetworkSecurityGroup` parancsmag alább látható módon.

        Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg

    Várt eredményt ad, csak a biztonsági szabályokat, már nem szerepel a **https-szabály** értesítés megjelenítése:

        Name                 : NSG-FrontEnd
        ...
        SecurityRules        : [
                                 {
                                   "Name": "rdp-rule",
                                   ...
                                 },
                                 {
                                   "Name": "web-rule",
                                   ...
                                 }
                               ]

## <a name="manage-associations"></a>Társítások kezelése

Az alhálózathoz NSG és NIC társíthat. Akkor is is elkülöníteni egy NSG bármely erőforrás van rendelve.

### <a name="associate-an-nsg-to-a-nic"></a>Egy NSG, hogy egy hálózati hozzárendelése

**NSG-FrontEnd** NSG **TestNICWeb1** hálózati kártya szeretne társítani, kövesse az alábbi lépéseket.

1. Futtassa a `Get-AzureRmNetworkSecurityGroup` parancsmagot a meglévő NSG beolvasásához, és egy változó, tárolja azt alább látható módon.

        $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG `
            -Name NSG-FrontEnd

2. Futtassa a `Get-AzureRmNetworkInterface` parancsmag meglévő a hálózati és tároljuk egy változóban, alább látható módon.

        $nic = Get-AzureRmNetworkInterface -ResourceGroupName RG-NSG `
            -Name TestNICWeb1

3. A tulajdonság **NetworkSecurityGroup** a **hálózati kártya** változó a **NSG** változó értékének alább látható módon.

        $nic.NetworkSecurityGroup = $nsg

4. A hálózati kártya módosításainak mentéséhez futtassa a `Set-AzureRmNetworkInterface` parancsmag alább látható módon.

        Set-AzureRmNetworkInterface -NetworkInterface $nic

    Várt eredményt ad, csak a **NetworkSecurityGroup** tulajdonság megjelenítő:

        NetworkSecurityGroup : {
                                 "SecurityRules": [],
                                 "DefaultSecurityRules": [],
                                 "NetworkInterfaces": [],
                                 "Subnets": [],
                                 "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd"
                               }

### <a name="dissociate-an-nsg-from-a-nic"></a>Elkülöníteni egy NSG a egy hálózati kártya

**NSG-FrontEnd** a **TestNICWeb1** hálózati kártya NSG elkülöníteni, kövesse az alábbi lépéseket.

1. Futtassa a `Get-AzureRmNetworkSecurityGroup` parancsmagot a meglévő NSG beolvasásához, és egy változó, tárolja azt alább látható módon.

        $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG `
            -Name NSG-FrontEnd

2. Futtassa a `Get-AzureRmNetworkInterface` parancsmag meglévő a hálózati és tároljuk egy változóban, alább látható módon.

        $nic = Get-AzureRmNetworkInterface -ResourceGroupName RG-NSG `
            -Name TestNICWeb1

3. A tulajdonság **NetworkSecurityGroup** a **hálózati kártya** változó **$null**, az alább látható módon.

        $nic.NetworkSecurityGroup = $null

4. A hálózati kártya módosításainak mentéséhez futtassa a `Set-AzureRmNetworkInterface` parancsmag alább látható módon.

        Set-AzureRmNetworkInterface -NetworkInterface $nic

    Várt eredményt ad, csak a **NetworkSecurityGroup** tulajdonság megjelenítő:

        NetworkSecurityGroup : null

### <a name="dissociate-an-nsg-from-a-subnet"></a>Az alhálózathoz egy NSG elkülöníteni

A **NSG-FrontEnd** NSG elkülöníteni a **FrontEnd** alhálózat, kövesse az alábbi lépéseket.

1. Futtassa a `Get-AzureRmVirtualNetwork` parancsmagot a meglévő VNet beolvasásához, és egy változó, tárolja azt alább látható módon.

        $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName RG-NSG `
            -Name TestVNet

2. Futtassa a `Get-AzureRmVirtualNetworkSubnetConfig` parancsmagot a **FrontEnd** alhálózat beolvasásához, és egy változó, tárolja azt alább látható módon.

        $subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet `
            -Name FrontEnd 

3. A tulajdonság **NetworkSecurityGroup** **alhálózat** változó **$null**, az alább látható módon.

        $subnet.NetworkSecurityGroup = $null

4. Az alhálózathoz végrehajtott módosítások mentéséhez, futtassa a `Set-AzureRmVirtualNetwork` parancsmag alább látható módon.

        Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

    Várt eredményt ad, csak a **FrontEnd** alhálózat tulajdonságainak megjelenítése. Figyelje meg, nem **NetworkSecurityGroup**tulajdonságait:

            ...
            Subnets           : [
                                  {
                                    "Name": "FrontEnd",
                                    "Etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                                    "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                                    "AddressPrefix": "192.168.1.0/24",
                                    "IpConfigurations": [
                                      {
                                        "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkInterfaces/TestNICWeb2/ipConfigurations/ipconfig1"
                                      },
                                      {
                                        "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkInterfaces/TestNICWeb1/ipConfigurations/ipconfig1"
                                      }
                                    ],
                                    "ProvisioningState": "Succeeded"
                                  },
                                    ...
                                ]

### <a name="associate-an-nsg-to-a-subnet"></a>Egy olyan alhálózathoz NSG hozzárendelése

Újra társítani a **NSG-FrontEnd** NSG **FronEnd** az alhálózathoz, kövesse az alábbi lépéseket.

1. Futtassa a `Get-AzureRmVirtualNetwork` parancsmagot a meglévő VNet beolvasásához, és egy változó, tárolja azt alább látható módon.

        $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName RG-NSG `
            -Name TestVNet

2. Futtassa a `Get-AzureRmVirtualNetworkSubnetConfig` parancsmagot a **FrontEnd** alhálózat beolvasásához, és egy változó, tárolja azt alább látható módon.

        $subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet `
            -Name FrontEnd 

1. Futtassa a `Get-AzureRmNetworkSecurityGroup` parancsmagot a meglévő NSG beolvasásához, és egy változó, tárolja azt alább látható módon.

        $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG `
            -Name NSG-FrontEnd

3. A tulajdonság **NetworkSecurityGroup** **alhálózat** változó **$null**, az alább látható módon.

        $subnet.NetworkSecurityGroup = $nsg

4. Az alhálózathoz végrehajtott módosítások mentéséhez, futtassa a `Set-AzureRmVirtualNetwork` parancsmag alább látható módon.

        Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

    A várt; csak a **FrontEnd** alhálózat **NetworkSecurityGroup** tulajdonságban kimenet:

        ...
        "NetworkSecurityGroup": {
                                  "SecurityRules": [],
                                  "DefaultSecurityRules": [],
                                  "NetworkInterfaces": [],
                                  "Subnets": [],
                                  "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd"
                                }
        ...

## <a name="delete-an-nsg"></a>Egy NSG törlése

Ha nem tartozik minden erőforrás egy NSG csak törölheti. Ha törölni szeretne egy NSG, kövesse az alábbi lépéseket.

1. Jelölje be a egy NSG rendelt erőforrások, futtassa a `azure network nsg show` [Nézet NSGs társítások](#View-NSGs-associations)látható módon.
2. Ha a NSG bármely NIC van társítva, futtassa a `azure network nic set` [Dissociate egy NSG egy hálózati kártya a](#Dissociate-an-NSG-from-a-NIC) az egyes adaptert látható módon 
3. A NSG is tartozik minden olyan alhálózathoz, futtassa a `azure network vnet subnet set` minden alhálózathoz [Dissociate-alhálózat a NSG](#Dissociate-an-NSG-from-a-subnet) látható módon.
4. Futtassa a NSG törléséhez a `Remove-AzureRmNetworkSecurityGroup` parancsmag alább látható módon.

        Remove-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd -Force

    >[AZURE.NOTE] A **-hatályba** paraméter biztosítja, hogy nem kell a törlés megerősítéséhez.

## <a name="next-steps"></a>Következő lépések

- A [Naplózás engedélyezése](virtual-network-nsg-manage-log.md) NSGs.