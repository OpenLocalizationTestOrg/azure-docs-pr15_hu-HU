<properties
   pageTitle="Nyissa meg az Azure portálon egy virtuális portok |} Microsoft Azure"
   description="Megtudhatja, hogy miként nyissa meg a olyan portot és a Windows virtuális erőforrás manager telepítési modellt használja az Azure-portálon található végpont létrehozása"
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

# <a name="opening-ports-to-a-vm-in-azure-using-the-azure-portal"></a>A virtuális portok megnyitása az Azure portálon Azure-ban
[AZURE.INCLUDE [virtual-machines-common-nsg-quickstart](../../includes/virtual-machines-common-nsg-quickstart.md)]

## <a name="quick-commands"></a>Gyors parancsok
Is [végezniük ezeket a lépéseket Azure PowerShell használatával](virtual-machines-windows-nsg-quickstart-powershell.md).

Első lépésként hozzon létre a Network biztonsági csoport. Jelölje ki a erőforráscsoport a portálon, kattintson a **Hozzáadás**gombra, majd keresése, és jelölje be a "Hálózati biztonsági csoport":

![Hálózati biztonsági csoport hozzáadása](./media/virtual-machines-windows-nsg-quickstart-portal/add-nsg.png)

Adja meg a hálózati biztonsági csoport nevét, jelölje ki vagy hozzon létre egy erőforrás csoportot, és jelöljön ki egy helyet. Kattintson a **Létrehozás** befejezéskor:

![Hálózati biztonsági csoport létrehozása](./media/virtual-machines-windows-nsg-quickstart-portal/create-nsg.png)

Jelölje be az új hálózati biztonsági csoportot. Válassza a "bejövő biztonsági szabályok", majd kattintson a **Hozzáadás** gombra kattintva hozzon létre egy szabályt:

![Bejövő szabály hozzáadása](./media/virtual-machines-windows-nsg-quickstart-portal/add-inbound-rule.png)

Adja meg az új szabály nevét. 80-as port már meg van adva alapértelmezés szerint. Ez a lap, itt volna módosíthatja a forrás, protokoll és cél további szabályok hozzáadása a hálózati biztonsági csoporthoz. Kattintson az **OK gombra** a szabály létrehozásához:

![Bejövő szabály létrehozása](./media/virtual-machines-windows-nsg-quickstart-portal/create-inbound-rule.png)

Az utolsó lépésként a hálózati biztonsági csoport társítása alhálózat vagy egy bizonyos hálózati kapcsolat. A hálózati biztonsági csoport lássunk társíthat alhálózat. Válassza a "Alhálózat", majd kattintson a **társítani**:

![Hálózati biztonsági csoport társítása alhálózat](./media/virtual-machines-windows-nsg-quickstart-portal/associate-subnet.png)

Jelölje ki a virtuális hálózatához, és válassza ki a megfelelő alhálózat:

![Hálózati biztonsági csoport társítása virtuális hálózatok](./media/virtual-machines-windows-nsg-quickstart-portal/select-vnet-subnet.png)

Most már létrehozott egy bejövő szabályt, amely lehetővé teszi, hogy a forgalmat a 80-as port, és azt társított alhálózat létrehozott egy hálózati biztonsági csoport. Bármely alhálózat való csatlakozás VMs 80-as port érhetők el.


## <a name="more-information-on-network-security-groups"></a>További információt a hálózat biztonsági csoportok
A gyors parancsok kezdeti lépéseket a forgalmat a virtuális halad teszi lehetővé. Hálózati biztonsági csoportok számos nagyszerű szolgáltatás és az erőforrások való hozzáférés szabályozása Granularitás biztosít. Erről további [hálózati biztonsági csoportokat, és itt vezérlés szabályok létrehozásával](../virtual-network/virtual-networks-create-nsg-arm-ps.md)kapcsolatban.

Hálózati biztonsági csoportokat és a vezérlés szabályok definiálhatók Azure erőforrás-kezelő sablonok részeként. További információk a [hálózati biztonsági csoportokat a sablonok létrehozásáról](../virtual-network/virtual-networks-create-nsg-arm-template.md).

Ha használni egy egyedi külső port hozzárendelése egy belső portjához a virtuális port továbbítási, használja az egy terheléselosztó és a hálózati cím fordítási-(hálózati Címfordítást) szabályokat. Például érdemes portot 8080 külső felekkel, és a TCP-portjához 80 egy virtuális irányított forgalom van. Megtudhatja, hogy [egy internetes terheléselosztó létrehozásával](../load-balancer/load-balancer-get-started-internet-arm-ps.md)kapcsolatban.

## <a name="next-steps"></a>Következő lépések
Ebben a példában létrehozott egy egyszerű szabályt, amely lehetővé teszi a HTTP-forgalmat. Információkat részletesebb környezetek az alábbi cikkekben talál:

- [Azure erőforrás szolgáltatásának áttekintése](../azure-resource-manager/resource-group-overview.md)
- [Mi az, hogy egy hálózati biztonsági csoport (NSG)?](../virtual-network/virtual-networks-nsg.md)
- [Azure terheléselosztókkal erőforrás-kezelő áttekintése](../load-balancer/load-balancer-arm.md)
