<properties
    pageTitle="Hozzon létre egy Linux virtuális Azure-sablon segítségével |} Microsoft Azure"
    description="Hozzon létre egy Linux virtuális Azure erőforrás-kezelő Azure-sablon segítségével."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="vlivech"
    manager="timlt"
    editor=""
    tags="azure-service-management,azure-resource-manager" />

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="10/24/2016"
    ms.author="v-livech"/>

# <a name="create-a-linux-vm-using-an-azure-template"></a>Hozzon létre egy Linux virtuális Azure-sablon használatával

Ez a cikk bemutatja, hogyan gyorsan bevezetését tervezi a Azure virtuális Linux gép Azure-sablon segítségével.  A cikk van szükség:

- az Azure-fiók (az[első ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/)).

- bejelentkezett az [Azure CLI](../xplat-cli-install.md) `azure login`.

- az Azure CLI _kell lennie az_ erőforrás-kezelő Azure mód `azure config mode arm`.

Az [Azure portál](virtual-machines-linux-quick-create-portal.md)Linux virtuális sablon is gyorsan telepítheti.

## <a name="quick-command-summary"></a>A parancsok rövid összefoglalása

```bash
azure group create \
-n myResourceGroup \
-l westus \
--template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
```

## <a name="detailed-walkthrough"></a>Részletes útmutató

Sablonok létrehozása VMs Azure első indításakor testre szabni kívánt beállításokat, a beállításokat, például a felhasználóneveket és állomásnevekké teszi lehetővé. Ez a cikk a azt vannak indítása az Azure-sablon egy Ubuntu virtuális együtt hálózati biztonsági csoport (NSG) használó port nyitva 22 SSH számára.

Azure erőforrás-kezelő sablonok a JSON fájlok egyszerű egyszeri műveleteket, például egy Ubuntu virtuális indítása, a jelen cikkben elvégzettként használhatók.  Azure sablonokat is használható, például tesztelése fejlesztők és munkakörnyezeti telepítési Papírhalom teljes környezetekben összetett Azure konfigurációban Egyenletszerkesztővel.

## <a name="create-the-linux-vm"></a>A Linux virtuális létrehozása

Az alábbi példa szemlélteti hívás `azure group create` hozzon létre egy erőforrás csoportot, és helyezhetnek üzembe a egy SSH védett Linux virtuális egyszerre [a erőforrás-kezelő Azure-sablon](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json)segítségével. Ne feledje, hogy a példában, akkor használja a nevek egyediek a környezetben. Ez a példa `myResourceGroup` , az erőforrás-csoport nevét, és `myVM` virtuális neveként.

```bash
azure group create \
--name myResourceGroup \
--location westus \
--template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
```

A kimenet így néz a következő kimeneti tiltása:

```bash
info:    Executing command group create
+ Getting resource group myResourceGroup
+ Creating resource group myResourceGroup
info:    Created resource group myResourceGroup
info:    Supply values for the following parameters
sshKeyData: ssh-rsa AAAAB3Nza<..ssh public key text..>VQgwjNjQ== myAdminUser@myVM
+ Initializing template configurations and parameters
+ Creating a deployment
info:    Created template deployment "azuredeploy"
data:    Id:                  /subscriptions/<..subid text..>/resourceGroups/myResourceGroup
data:    Name:                myResourceGroup
data:    Location:            westus
data:    Provisioning State:  Succeeded
data:    Tags: null
data:
info:    group create command OK
```

A példa rendszerbe egy virtuális használata a `--template-uri` paraméter.  Is töltse le vagy a helyi meghajtóra sablon létrehozása és adják át, a sablon használatával a `--template-file` egy fájl elérési útját sablon argumentumaként paramétert. Az Azure CLI kéri a paraméterek követel meg a sablont.

## <a name="next-steps"></a>Következő lépések

Keresse meg a [sablongyűjteményben](https://azure.microsoft.com/documentation/templates/) megtudhatja, milyen alkalmazás keretek bevezetését tervezi a Tovább gombra.
