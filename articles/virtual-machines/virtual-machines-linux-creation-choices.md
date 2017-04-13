<properties
    pageTitle="Hozzon létre egy Linux virtuális különféle módjai |} Microsoft Azure"
    description="Különböző módjairól az Azure, többek között a hivatkozásokat tartalmaz az eszközök és oktatóanyagok az mindegyik módszernek Linux virtuális gép létrehozásához."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="vm-linux"
    ms.workload="infrastructure-services"
    ms.date="09/27/2016"
    ms.author="iainfou"/>

# <a name="different-ways-to-create-a-linux-virtual-machine-in-azure"></a>Változatos módszerekkel, amelyekkel Linux virtuális gép létrehozása az Azure-ban

Az a lehetősége van egy Linux virtuális gép (virtuális) eszközökkel és kényelmes Önnek munkafolyamatok létrehozása az Azure-ban. Ez a cikk azt mutatja, ezek különbségek és a példák a Linux VMs hozhat létre.


## <a name="azure-cli"></a>Azure CLI 

Az Azure CLI platformon egy npm csomagot, feltéve distro csomagok vagy Docker tároló keresztül érhető el. Erről további arról, [hogy miként szeretné telepíteni és beállítani az Azure CLI](../xplat-cli-install.md). Az alábbi oktatóanyagok példákon mutatják az Azure CLI használatáról. Olvassa el az egyes a cikk további információt a CLI rövid parancsok jelennek meg:

- [Egy Linux virtuális létrehozása az Azure CLI fejlesztők és tesztelése](virtual-machines-linux-quick-create-cli.md)
    - Az alábbi példa létrehoz egy nyilvános kulccsal nevű CoreOS virtuális `azure_id_rsa.pub`:

    ```bash
    azure vm quick-create -ssh-publickey-file ~/.ssh/azure_id_rsa.pub \
        --image-urn CoreOS
    ```

- [Hozzon létre egy védett Linux virtuális Azure-sablon használatával](virtual-machines-linux-create-ssh-secured-vm-from-template.md)
    - Az alábbi példa létrehoz egy virtuális GitHub-on tárolt sablon használatával:

    ```bash
    azure group create --name TestRG --location WestUS 
        --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
    ```

- [Egy teljes Linux környezet használatával a Azure CLI létrehozása](virtual-machines-linux-create-cli-complete.md)
    - Tartalmaz egy elérhetőségének beállítása egy terheléselosztó és a több VMs létrehozására.

- [Lemezen hozzáadása egy Linux virtuális](virtual-machines-linux-add-disk.md)
    - Az alábbi példában egy 5Gb szabad ad egy meglévő virtuális nevű `TestVM`:

    ```bash
    azure vm disk attach-new --resource-group TestRG --vm-name TestVM \
        --size-in-GB 5
    ```

## <a name="azure-portal"></a>Azure portál

Az [Azure portál](https://portal.azure.com) gyorsan létrehozhat egy virtuális, mivel a rendszer telepítése semmi nem teszi lehetővé. Az Azure portal segítségével a virtuális létrehozása:

- [Hozzon létre egy Linux virtuális az Azure portál használatával](virtual-machines-linux-quick-create-portal.md) 
- [Az Azure portálon lemezen csatolása](virtual-machines-linux-attach-disk-portal.md)


## <a name="operating-system-and-image-choices"></a>Operációs rendszer és programkódok
A virtuális létrehozásakor kiválaszthatja a operációs rendszere alapján használni kívánt képet. Azure és partnerei kínálnak sok képek, amelyek közé tartozik az egyes alkalmazások és eszközök előtelepítve. Vagy töltse fel a saját képeket (lásd [az alábbi szakaszt](#use-your-own-image)).

### <a name="azure-images"></a>Azure képek
Használja a `azure vm image` CLI parancsok megtudhatja, mi a publisher, distro Megjelenés és buildjeiben érhető el.

A lista közzétevők érhető el az alábbi képlettel történik:

```bash
azure vm image list-publishers --location WestUS
```

A lista, elérhető termékek (ajánlatok) egy adott gyártó az alábbi képlettel történik:

```bash
azure vm image list-offers --location WestUS --publisher Canonical
```

A lista egy adott ajánlat a rendelkezésre álló termékváltozatok (distro verziókban) az alábbi képlettel történik:

```bash
azure vm image list-skus --location WestUS --publisher Canonical --offer UbuntuServer
```

Az egy adott megjelenés követi a lista összes elérhető képek:

```bash
azure vm image list --location WestUS --publisher Canonical --offer UbuntuServer --sku 16.04.0-LTS
```

További példákat a böngészéséről és a rendelkezésre álló képek olvassa el a [Navigálás és az Azure CLI választó Azure virtuális gép képekkel](virtual-machines-linux-cli-ps-findimage.md)című témakört.

A `azure vm quick-create` és `azure vm create` parancsokat használhatja a gyakrabban distros és a legújabb kiadásairól gyors eléréséhez aliast van. Aliasok használata gyakran gyorsabb, mint a publisher, ajánlat, Termékváltozat és verzió minden alkalommal, amikor hoz létre egy virtuális megadása:

| Alias     | A Publisher | Ajánlat        | RAKTÁRI SZÁM         | Verzió |
|:----------|:----------|:-------------|:------------|:--------|
| CentOS    | OpenLogic | Centos       | 7.2.         | legújabb  |
| CoreOS    | CoreOS    | CoreOS       | Állandó      | legújabb  |
| Debian    | credativ  | Debian       | 8           | legújabb  |
| openSUSE  | SUSE      | openSUSE     | 13.2        | legújabb  |
| RHEL      | Redhat    | RHEL         | 7.2.         | legújabb  |
| SLES      | SLES      | SLES         | 12-SP1      | legújabb  |
| UbuntuLTS | Kanonikus | UbuntuServer | 14.04.4-LTS | legújabb  |

### <a name="use-your-own-image"></a>A saját kép használata:

Ha adott testreszabások van szüksége, kép alapján egy meglévő Azure virtuális *rögzítése* , hogy virtuális. Töltse fel a helyszíni létrehozott képet is. Támogatott distros és a saját képek használatáról további információt a következő cikkekben talál:

- [Azure hitelesítettek a felosztott](virtual-machines-linux-endorsed-distros.md)

- [Nem záradékkal terjesztését adatait](virtual-machines-linux-create-upload-generic.md)

- [Hogyan rögzítheti az erőforrás-kezelő sablonként Linux virtuális géphez](virtual-machines-linux-capture-image.md).
    - Rövid az példa parancs, amellyel rögzítheti egy meglévő virtuális:

    ```bash
    azure vm deallocate --resource-group TestRG --vm-name TestVM
    azure vm generalize --resource-group TestRG --vm-name TestVM
    azure vm capture --resource-group TestRG --vm-name TestVM --vhd-name-prefix CapturedVM
    ```

## <a name="next-steps"></a>Következő lépések

- Hozzon létre egy Linux virtuális a [portálon](virtual-machines-linux-quick-create-portal.md)a [CLI](virtual-machines-linux-quick-create-cli.md)vagy az [erőforrás-kezelő Azure-sablon](virtual-machines-linux-cli-deploy-templates.md)segítségével.

- Miután létrehozott egy Linux virtuális, [adatok lemez hozzáadása](virtual-machines-linux-add-disk.md).

- A rövid lépéseket követve [állítsa alaphelyzetbe a jelszó vagy a SSH is, és a felhasználók kezelése](virtual-machines-linux-using-vmaccess-extension.md)
