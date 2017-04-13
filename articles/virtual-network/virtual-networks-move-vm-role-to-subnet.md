<properties 
   pageTitle="Egy virtuális vagy szerepkör-példány áthelyezése egy másik alhálózathoz"
   description="Megtudhatja, hogy miként VMs és szerepkör-példányok áthelyezése egy másik alhálózat"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/22/2016"
   ms.author="jdial" />

# <a name="how-to-move-a-vm-or-role-instance-to-a-different-subnet"></a>Egy virtuális vagy szerepkör-példány áthelyezése egy másik alhálózathoz

A PowerShell használatával váltani a VMs egy alhálózat ugyanazon virtuális a hálózaton (VNet). Szerepkör-példányok áthelyezhetők a CSCFG Szerkesztés helyett PowerShell használatával.

>[AZURE.NOTE] Ez a cikk csak az Azure klasszikus telepítések viszonyított információkat tartalmaz.

Miért át egy másik alhálózat VMs? Alhálózat áttelepítési akkor hasznos, ha a régebbi alhálózat túl kicsi, és nem bonthatók ki az adott alhálózat meglévő futó VMs miatt. Ebben az esetben hozzon létre egy új, nagyobb alhálózat, és a VMs áttelepítése az új alhálózat, majd áttelepítés befejeződése után törölheti a régi üres alhálózat.

## <a name="how-to-move-a-vm-to-another-subnet"></a>A virtuális áthelyezése egy másik alhálózathoz

A virtuális áthelyezéséhez futtassa a Set-AzureSubnet PowerShell-parancsmag, használja az alábbi példában sablonként. Az alábbi példában azt is áthelyezése TestVM a a bemutató alhálózat alhálózat-2. Győződjön meg arról, és módosíthatja a példában a környezet megfelelően. Figyelje meg, hogy a frissítés-AzureVM parancsmag eljárás részeként indításakor újra fog a virtuális a frissítési folyamat részeként.

    Get-AzureVM –ServiceName TestVMCloud –Name TestVM `
  	| Set-AzureSubnet –SubnetNames Subnet-2 `
  	| Update-AzureVM

Ha megad egy statikus belső magánjellegű IP-cím a virtuális, fognak rendelkezésére állni törölje ezt a beállítást, mielőtt a virtuális áthelyezése egy új alhálózat. Ebben az esetben használja az alábbiakat:

    Get-AzureVM -ServiceName TestVMCloud -Name TestVM `
  	| Remove-AzureStaticVNetIP `
  	| Update-AzureVM
    Get-AzureVM -ServiceName TestVMCloud -Name TestVM `
  	| Set-AzureSubnet -SubnetNames Subnet-2 `
  	| Update-AzureVM

## <a name="to-move-a-role-instance-to-another-subnet"></a>Egy szerepkör-példány áthelyezése egy másik alhálózat

Lépés egy szerepkör-példányt, szerkessze a CSCFG fájlt. Az alábbi példában azt is áthelyezése "Role0" a virtuális hálózati *VNETName* a saját bemutató alhálózat *alhálózat -*2. A szerepkör-példány már telepítették, mert csak kell megadnia a alhálózat neve = 2-alhálózat. Győződjön meg arról, és módosíthatja a példában a környezet megfelelően.

    <NetworkConfiguration>
        <VirtualNetworkSite name="VNETName" />
        <AddressAssignments>
           <InstanceAddress roleName="Role0">
                <Subnets><Subnet name="Subnet-2" /></Subnets>
           </InstanceAddress>
        </AddressAssignments>
    </NetworkConfiguration> 
