<properties
   pageTitle="Hozzon létre egy Linux virtuális több |} Microsoft Azure"
   description="Megtudhatja, hogy miként készíthet egy Linux virtuális csatlakozik az Azure CLI vagy az erőforrás-kezelő sablonok használatával több NIC."
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
   ms.workload="infrastructure"
   ms.date="10/27/2016"
   ms.author="iainfou"/>

# <a name="creating-a-linux-vm-with-multiple-nics"></a>Több egy Linux virtuális létrehozása
Létrehozhat egy virtuális gép (virtuális), amelynek több virtuális hálózati kapcsolat (NIC) kapcsolódik Azure-ban. A leggyakoribb helyzet, hogy az előtér és a kapcsolat vagy megfigyelések vagy biztonsági megoldássá dedikált hálózati különböző alhálózathoz lenne. Ebben a cikkben egy virtuális létrehozását csatolva több NIC gyors parancsok. Részletes információt, például hogy miként hozhat létre saját Bash parancsfájlok belül több NIC további [többszörös-hálózati VMs telepítésével](../virtual-network/virtual-network-deploy-multinic-arm-cli.md)kapcsolatos. Különböző [méretű virtuális](virtual-machines-linux-sizes.md) NIC eltérő számú támogatják, így méretezés ennek megfelelően a virtuális.

>[AZURE.WARNING] Ha létrehozott egy virtuális – nem adható hozzá NIC egy meglévő virtuális csatlakoztatnia kell több. Létrehozhat [egy virtuális az eredeti virtuális lemez(ek) alapján](virtual-machines-linux-copy-vm.md) , és hozzon létre több, mint a virtuális rendszerbe.

## <a name="quick-commands"></a>Gyors parancsok
Győződjön meg arról, hogy rendelkezik-e jelentkezve, és erőforrás-kezelő üzemmód használata az [Azure CLI](../xplat-cli-install.md) :

```bash
azure config mode arm
```

Az alábbi példák cserélje ki a kívánt értékét például paraméterek neve. Példa paraméterek neve szereplő `myResourceGroup`, `mystorageaccount`, és `myVM`.

Első lépésként hozzon létre egy erőforráscsoport. Az alábbi példa létrehoz egy névvel ellátott erőforráscsoport `myResourceGroup` a a `WestUS` helyét:

```bash
azure group create myResourceGroup -l WestUS
```

Ahhoz, hogy a VMs tárterület-fiók létrehozása. Az alábbi példa létrehoz egy tároló nevű fiókot `mystorageaccount`:

```bash
azure storage account create mystorageaccount -g myResourceGroup \
    -l WestUS --kind Storage --sku-name PLRS
```

Hozzon létre egy virtuális hálózati a VMs csatlakozni. Az alábbi példa létrehoz egy virtuális hálózati nevű `myVnet` egy címet előtaggal a `192.168.0.0/16`:

```bash
azure network vnet create -g myResourceGroup -l WestUS \
    -n myVnet -a 192.168.0.0/16
```

Hozzon létre két virtuális hálózati alhálózat - egyik az előtér-forgalmat, és a háttér-forgalmat. Az alábbi példa létrehoz két alhálózat nevű `mySubnetFrontEnd` és `mySubnetBackEnd`:

```bash
azure network vnet subnet create -g myResourceGroup -e myVnet \
    -n mySubnetFrontEnd -a 192.168.1.0/24
azure network vnet subnet create -g myResourceGroup -e myVnet \
    -n mySubnetBackEnd -a 192.168.2.0/24
```

Hozzon létre két kártyát az előtér-alhálózathoz egy hálózati és egy hálózati csatolása a háttéradatbázist alhálózat. Az alábbi példa létrehoz két kártyát nevű `myNic1` és `myNic2`, és csatolja ezeket az alhálózathoz:

```bash
azure network nic create -g myResourceGroup -l WestUS \
    -n myNic1 -m myVnet -k mySubnetFrontEnd
azure network nic create -g myResourceGroup -l WestUS \
    -n myNic2 -m myVnet -k mySubnetBackEnd
```

Végül hozza létre a virtuális csatolása a korábban létrehozott két kártyát. Az alábbi példa létrehoz egy virtuális nevű `myVM`:

```bash
azure vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --location WestUS \
    --os-type linux \
    --nic-names myNic1,myNic2 \
    --vm-size Standard_DS2_v2 \
    --storage-account-name mystorageaccount \
    --image-urn UbuntuLTS \
    --admin-username ops \
    --ssh-publickey-file ~/.ssh/id_rsa.pub
```

## <a name="creating-multiple-nics-using-azure-cli"></a>Azure CLI használatával több NIC létrehozása
Ha korábban már létrehozott egy virtuális az Azure CLI használatával, a quick parancsokat tisztában kell lennie. A folyamat megegyezik a hálózati egy vagy több létrehozásához. Erről részletesebben [az Azure CLI használatával több NIC üzembe helyezése](../virtual-network/virtual-network-deploy-multinic-arm-cli.md), beleértve az összes NIC létrehozása ismétlése folyamata parancsfájlokat.

Az alábbi példa létrehoz két kártyát nevű `myNic1` és `myNic2`, az egyes alhálózat kapcsolódás egy hálózati:

```bash
azure network nic create --resource-group myResourceGroup --location WestUS \
    -n myNic1 --subnet-vnet-name myVnet --subnet-name mySubnetFrontEnd
azure network nic create --resource-group myResourceGroup --location WestUS \
    -n myNic2 --subnet-vnet-name myVnet --subnet-name mySubnetBackEnd
```

A szokásos is létrehozhat egy [Hálózati biztonsági csoport](../virtual-network/virtual-networks-nsg.md) vagy [terheléselosztó](../load-balancer/load-balancer-overview.md) kezelheti és megoszthatja irányítja át a VMs érdekében. Ismét a parancsok megegyeznek több való munkavégzés során. Az alábbi példa létrehoz egy hálózati biztonsági csoport nevű `myNetworkSecurityGroup`:

```bash
azure network nsg create --resource-group myResourceGroup --location WestUS \
    --name myNetworkSecurityGroup
```

A hálózati biztonsági csoport használata a NIC kötni `azure network nic set`. A következő példa köti `myNic1` és `myNic2` a `myNetworkSecurityGroup`:

```bashazure 
azure network nic set --resource-group myResourceGroup --name myNic1 \
    --network-security-group-name myNetworkSecurityGroup
azure network nic set --resource-group myResourceGroup --name myNic2 \
    --network-security-group-name myNetworkSecurityGroup
```

A virtuális létrehozásakor most több adja meg. Inkább használatával `--nic-name` ahhoz, hogy egy egyetlen hálózati, hanem használhatja `--nic-names` , és adja meg a NIC vesszővel tagolt listáját. Meg kell gondoskodik, akkor jelölje be a virtuális méretét. Nincsenek korlátozások, amely egy virtuális vehet fel NIC száma. További információ a [Linux virtuális méretét](virtual-machines-linux-sizes.md). A következő példa bemutatja a több, majd egy virtuális mérete, amely támogatja a több megadása (`Standard_DS2_v2`):

```bash
azure vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --location WestUS \
    --os-type linux \
    --nic-names myNic1,myNic2 \
    --vm-size Standard_DS2_v2 \
    --storage-account-name mystorageaccount \
    --image-urn UbuntuLTS \
    --admin-username ops \
    --ssh-publickey-file ~/.ssh/id_rsa.pub
```

## <a name="creating-multiple-nics-using-resource-manager-templates"></a>Erőforrás-kezelő sablonok használatával több NIC létrehozása
Azure erőforrás-kezelő sablonok deklaráció JSON-fájlok használatával a környezet meghatározása. Erről [az Azure erőforrás-kezelő áttekintése](../azure-resource-manager/resource-group-overview.md). Erőforrás-kezelő sablonok létrehozása több példányának egy erőforrás például több létrehozása a telepítés során ad lehetőséget. *Másolat* használatával hozhat létre példányok számának megadása:

```bash
"copy": {
    "name": "multiplenics"
    "count": "[parameters('count')]"
}
```

[Több példányon *másolással*létrehozásával](../resource-group-create-multiple.md)kapcsolatban további. 

Is használhatja a `copyIndex()` hozzáfűzni majd szám erőforrás nevét, amely lehetővé teszi, hogy hozzon létre `myNic1`, `myNic2`stb. Az alábbi példa az index értékét hozzáfűzése:

```bash
"name": "[concat('myNic', copyIndex())]", 
```

[Erőforrás-kezelő sablonok használatával több NIC létrehozása](../virtual-network/virtual-network-deploy-multinic-arm-template.md)a teljes példa erről.

## <a name="next-steps"></a>Következő lépések
Győződjön meg arról, tekintse át a [Linux virtuális méretű](virtual-machines-linux-sizes.md) hoz létre egy virtuális több meg. Figyelje meg a minden virtuális méretét támogatja NIC maximális száma. 

Ne feledje, hogy egy meglévő virtuális nem vehet fel további NIC, amikor rendszerbe állítják a virtuális létre kell hoznia az összes NIC. Győződjön meg róla, hogy van-e kezdettől fogva szükséges hálózati kapcsolat a telepítés tervezésekor gondoskodik.