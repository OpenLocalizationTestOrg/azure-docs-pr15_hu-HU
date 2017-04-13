<properties
   pageTitle="Nyissa meg a portokat és egy Linux virtuális végpontok |} Microsoft Azure"
   description="Megtudhatja, hogy miként nyissa meg a olyan portot / a Linux virtuális gép az Azure erőforrás manager telepítési modell és az Azure CLI végpont létrehozása"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="10/27/2016"
   ms.author="iainfou"/>

# <a name="opening-ports-and-endpoints-to-a-linux-vm-in-azure"></a>Portok megnyitása és a végpontok egy Linux virtuális Azure-ban
Nyissa meg a olyan portot, vagy hozzon létre egy virtuális géphez (virtuális) zárólap Azure-ban: hozzon létre egy hálózati szűrő alhálózat vagy virtuális hálózati kapcsolat. Ezek a bejövő és kimenő forgalmának szabályozni, szűrők helyez el, amelyek a forgalom kap az erőforráshoz csatolt hálózati biztonsági csoport. A 80-as port használata a közös példa internetes forgalmat.

## <a name="quick-commands"></a>Gyors parancsok
Hálózati biztonsági csoport és a szabályok létrehozása [Az Azure CLI](../xplat-cli-install.md) telepítve és erőforrás-kezelő mód van szükség:

```bash
azure config mode arm
```

Az alábbi példák cserélje ki a kívánt értékét például paraméterek neve. Példa paraméterek neve szereplő `myResourceGroup`, `myNetworkSecurityGroup`, és `myVnet`.

A saját neve és a hely megadására megfelelően a hálózati biztonsági csoport létrehozása Az alábbi példa létrehoz egy hálózati biztonsági csoport nevű `myNetworkSecurityGroup` a a `WestUS` helyét:

```
azure network nsg create --resource-group myResourceGroup --location westus \
    --name myNetworkSecurityGroup
```

Adja hozzá egy szabályt, amely lehetővé teszi a HTTP-alapú forgalmat a webkiszolgáló (vagy forgatókönyvet az például SSH access vagy az adatbázis-kapcsolat módosítása). Az alábbi példa létrehoz egy szabályt, nevű `myNetworkSecurityGroupRule` TCP-forgalmat a 80-as port engedélyezése:

```
azure network nsg rule create --resource-group myResourceGroup \
    --nsg-name myNetworkSecurityGroup --name myNetworkSecurityGroupRule \
    --protocol tcp --direction inbound --priority 1000 \
    --destination-port-range 80 --access allow
```

A hálózati biztonsági csoport társítani a virtuális hálózati kapcsolat (hálózati). Az alábbi példában egy meglévő hálózati kártya elnevezett társít `myNic` nevű hálózati biztonsági csoport `myNetworkSecurityGroup`:

```
azure network nic set --resource-group myResourceGroup \
    --network-security-group-name myNetworkSecurityGroup --name myNic
```

Másik lehetőségként társíthat a hálózati biztonsági csoport virtuális hálózati alhálózat, hanem csak a hálózati kártya egy egyetlen virtuális a. Az alábbi példában egy meglévő alhálózat nevű társít `mySubnet` a a `myVnet` virtuális-hálózaton: a hálózati biztonsági csoport nevű `myNetworkSecurityGroup`:

```
azure network vnet subnet set --resource-group myResourceGroup \
    --network-security-group-name myNetworkSecurityGroup \
    --vnet-name myVnet --name mySubnet
```

## <a name="more-information-on-network-security-groups"></a>További információt a hálózat biztonsági csoportok
A gyors parancsok kezdeti lépéseket a forgalmat a virtuális halad teszi lehetővé. Hálózati biztonsági csoportok számos nagyszerű szolgáltatás és az erőforrások való hozzáférés szabályozása Granularitás biztosít. Erről további [hálózati biztonsági csoportokat, és itt vezérlés szabályok létrehozásával](../virtual-network/virtual-networks-create-nsg-arm-cli.md)kapcsolatban.

Hálózati biztonsági csoportokat és a vezérlés szabályok definiálhatók Azure erőforrás-kezelő sablonok részeként. További információk a [hálózati biztonsági csoportokat a sablonok létrehozásáról](../virtual-network/virtual-networks-create-nsg-arm-template.md).

Ha használni egy egyedi külső port hozzárendelése egy belső portjához a virtuális port továbbítási, használja az egy terheléselosztó és a hálózati cím fordítási-(hálózati Címfordítást) szabályokat. Például érdemes portot 8080 külső felekkel, és a TCP-portjához 80 egy virtuális irányított forgalom van. Megtudhatja, hogy [egy internetes terheléselosztó létrehozásával](../load-balancer/load-balancer-get-started-internet-arm-cli.md)kapcsolatban.

## <a name="next-steps"></a>Következő lépések
Ebben a példában létrehozott egy egyszerű szabályt, amely lehetővé teszi a HTTP-forgalmat. Információkat részletesebb környezetek az alábbi cikkekben talál:

- [Azure erőforrás szolgáltatásának áttekintése](../azure-resource-manager/resource-group-overview.md)
- [Mi az, hogy egy hálózati biztonsági csoport (NSG)?](../virtual-network/virtual-networks-nsg.md)
- [Azure terheléselosztókkal erőforrás-kezelő áttekintése](../load-balancer2    /load-balancer-arm.md)