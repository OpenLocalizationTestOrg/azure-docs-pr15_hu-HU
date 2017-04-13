<properties
   pageTitle="Nyissa meg a PowerShell használatá virtuális portok |} Microsoft Azure"
   description="Megtudhatja, hogy miként nyissa meg a olyan portot / Azure erőforrás manager telepítési mód és Azure PowerShell használatával a Windows-virtuális gép végpont létrehozása"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="10/27/2016"
   ms.author="iainfou"/>

# <a name="opening-ports-and-endpoints-to-a-vm-in-azure-using-powershell"></a>Megnyitja a portokat és egy virtuális végpontok az Azure PowerShell használatával
[AZURE.INCLUDE [virtual-machines-common-nsg-quickstart](../../includes/virtual-machines-common-nsg-quickstart.md)]

## <a name="quick-commands"></a>Gyors parancsok
Hozzon létre egy hálózati biztonsági csoport és a vezérlés szabályok szüksége [az Azure PowerShell telepítése legújabb verzióját](../powershell-install-configure.md). Is [végezniük ezeket a lépéseket az Azure portálon](virtual-machines-windows-nsg-quickstart-portal.md).

Jelentkezzen be az Azure-fiók:

```powershell
Login-AzureRmAccount
```

Az alábbi példák cserélje ki a kívánt értékét például paraméterek neve. Példa paraméterek neve szereplő `myResourceGroup`, `myNetworkSecurityGroup`, és `myVnet`.

Hozzon létre egy szabályt. Az alábbi példa létrehoz egy szabályt, nevű `myNetworkSecurityGroupRule` TCP-forgalmat a 80-as port engedélyezése:

```powershell
$httprule = New-AzureRmNetworkSecurityRuleConfig -Name "myNetworkSecurityGroupRule" `
    -Description "Allow HTTP" -Access "Allow" -Protocol "Tcp" -Direction "Inbound" `
    -Priority "100" -SourceAddressPrefix "Internet" -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 80
```

Ezután a hálózati biztonsági csoport létrehozása és hozzárendelése a HTTP szabályt az imént létrehozott az alábbi képlettel történik. Az alábbi példa létrehoz egy hálózati biztonsági csoport nevű `myNetworkSecurityGroup`:

```powershell
$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName "myResourceGroup" `
    -Location "WestUS" -Name "myNetworkSecurityGroup" -SecurityRules $httprule
```

Most vegyük hozzárendelése a hálózati biztonsági csoport alhálózat. A következő példa rendeli hozzá egy meglévő virtuális hálózat nevű `myVnet` a változó `$vnet`:

```powershell
$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName "myResourceGroup" `
    -Name "myVnet"
```

A hálózati biztonsági csoport társítása alhálózathoz. Az alábbi példa az alhálózathoz nevű társít `mySubnet` a hálózati biztonsági csoporttal:

```powershell
$subnetPrefix = $vnet.Subnets|?{$_.Name -eq 'mySubnet'}

Set-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name "mySubnet" `
    -AddressPrefix $subnetPrefix.AddressPrefix `
    -NetworkSecurityGroup $nsg
```

Végezetül frissítése annak érdekében, hogy a módosítások érvénybelépéséhez virtuális hálózatához:

```powershell
Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
```


## <a name="more-information-on-network-security-groups"></a>További információt a hálózat biztonsági csoportok
A gyors parancsok kezdeti lépéseket a forgalmat a virtuális halad teszi lehetővé. Hálózati biztonsági csoportok számos nagyszerű szolgáltatás és az erőforrások való hozzáférés szabályozása Granularitás biztosít. Erről további [hálózati biztonsági csoportokat, és itt vezérlés szabályok létrehozásával](../virtual-network/virtual-networks-create-nsg-arm-ps.md)kapcsolatban.

Hálózati biztonsági csoportokat és a vezérlés szabályok definiálhatók Azure erőforrás-kezelő sablonok részeként. További információk a [hálózati biztonsági csoportokat a sablonok létrehozásáról](../virtual-network/virtual-networks-create-nsg-arm-template.md).

Ha használni egy egyedi külső port hozzárendelése egy belső portjához a virtuális port továbbítási, használja az egy terheléselosztó és a hálózati cím fordítási-(hálózati Címfordítást) szabályokat. Például érdemes portot 8080 külső felekkel, és a TCP-portjához 80 egy virtuális irányított forgalom van. Megtudhatja, hogy [egy internetes terheléselosztó létrehozásával](../load-balancer/load-balancer-get-started-internet-arm-ps.md)kapcsolatban.

## <a name="next-steps"></a>Következő lépések
Ebben a példában létrehozott egy egyszerű szabályt, amely lehetővé teszi a HTTP-forgalmat. Információkat részletesebb környezetek az alábbi cikkekben talál:

- [Azure erőforrás szolgáltatásának áttekintése](../azure-resource-manager/resource-group-overview.md)
- [Mi az, hogy egy hálózati biztonsági csoport (NSG)?](../virtual-network/virtual-networks-nsg.md)
- [Azure terheléselosztókkal erőforrás-kezelő áttekintése](../load-balancer/load-balancer-arm.md)