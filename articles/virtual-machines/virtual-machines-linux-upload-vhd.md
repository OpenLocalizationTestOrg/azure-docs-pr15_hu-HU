<properties
    pageTitle="Hozzon létre, és töltse fel az egyéni Linux képet |} Microsoft Azure"
    description="Hozzon létre és Azure virtuális merevlemez (virtuális) feltöltése a képpel egy egyéni Linux az erőforrás-kezelő telepítési modell használata."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="iainfou"/>

# <a name="upload-and-create-a-linux-vm-from-custom-disk-image"></a>Töltse fel, és hozzon létre egy Linux virtuális egyéni lemez képből

Ez a cikk bemutatja, hogyan töltse fel az erőforrás-kezelő telepítési modell Azure virtuális merevlemez (virtuális), és a saját kép létrehozása a Linux VMs. Ez a funkció lehetővé teszi, hogy telepítése és konfigurálása a Linux distro igényei, valamint hogy virtuális használatával gyorsan létrehozhat az Azure virtuális gépeken futó (VMs).

## <a name="quick-commands"></a>Gyors parancsok
Ha gyorsan teljesítenie a tevékenységhez, az alábbiakat szakaszt a következő parancsokat tartalmaz egy virtuális feltöltendő Azure. Részletesebb információkat és az egyes lépések környezetben a többi a dokumentumot, a [kezdési itt](#requirements)találhatók.

Győződjön meg arról, hogy rendelkezik-e jelentkezve, és erőforrás-kezelő üzemmód használata [Az Azure CLI](../xplat-cli-install.md) :

```bash
azure config mode arm
```

Az alábbi példák cserélje ki a kívánt értékét például paraméterek neve. Példa paraméterek neve szereplő `myResourceGroup`, `mystorageaccount`, és `myimages`.

Első lépésként hozzon létre egy erőforráscsoport. Az alábbi példa létrehoz egy névvel ellátott erőforráscsoport `myResourceGroup` a a `WestUs` helyét:

```bash
azure group create myResourceGroup --location "WestUS"
```

A virtuális merevlemezeken tartása tárterület-fiók létrehozása. Az alábbi példa létrehoz egy tárolási nevű fiókot `mystorageaccount`:

```bash
azure storage account create mystorageaccount --resource-group myResourceGroup \
    --location "WestUS" --kind Storage --sku-name PLRS
```

A hívóbetűk tárolási fiókjának sorolja fel. Jegyezze le `key1`:

```bash
azure storage account keys list mystorageaccount --resource-group myResourceGroup
```

Hozzon létre tároló belül szerezte be tároló kulccsal tárterület-fiókját. Az alábbi példa létrehoz egy tároló `myimages` a tárhely fő érték használatáról `key1`:

```bash
azure storage container create --account-name mystorageaccount \
    --account-key key1 --container myimages
```

Végül a létrehozott tároló töltse fel a virtuális. Adja meg a helyi elérési útját a virtuális a `/path/to/disk/mydisk.vhd`:

```bash
azure storage blob upload --blobtype page --account-name mystorageaccount \
    --account-key key1 --container myimages /path/to/disk/mydisk.vhd
```

Most már létrehozhat egy virtuális a feltöltött pillanatkép [erőforrás-kezelő sablon használatával](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd). A lemezen a URI megadásával is használhatja a CLI (`--image-urn`). Az alábbi példa létrehoz egy virtuális nevű `myVM` a virtuális lemezzel korábban feltöltött:

```bash
azure vm create myVM -l "WestUS" --resource-group myResourceGroup \
    --image-urn https://mystorageaccount.blob.core.windows.net/myimages/mydisk.vhd
```

A cél tárolási fiókot nem lehet ugyanaz, mint a virtuális lemez feltöltött hol. Is meg kell adnia, vagy választ kér, a további paramétereket követel meg az `azure vm create` például virtuális hálózati, nyilvános IP-cím, felhasználónév vagy SSH kulcsok parancsot. Erről további tudnivalók a [rendelkezésre álló CLI erőforrás-kezelő paramétereket](azure-cli-arm-commands.md#azure-vm-commands-to-manage-your-azure-virtual-machines).

## <a name="requirements"></a>Követelmények
Az alábbi lépések elvégzésével van szükség:

- **Linux operációs rendszer .vhd fájlban telepített** - telepítse az [Azure záradékkal Linux eloszlás](virtual-machines-linux-endorsed-distros.md) (vagy lásd: [nem záradékkal terjesztését adatait](virtual-machines-linux-create-upload-generic.md)) virtuális lemezre virtuális formátumban. Több eszközök hozhat létre egy virtuális és virtuális létezik:
    - Telepítése és beállítása a [QEMU](https://en.wikibooks.org/wiki/QEMU/Installing_QEMU) vagy [KVM](http://www.linux-kvm.org/page/RunningKVM), ügyelve arra, hogy a kép formázása virtuális használni. Ha szükséges, használatával [kép konvertálása](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) `qemu-img convert`.
    - A Hyper-V [a Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_install) , vagy [a Windows Server 2012/2012 R2](https://technet.microsoft.com/library/hh846766.aspx)is használhatja.

> [AZURE.NOTE] Újabb VHDX formátumúvá Azure-ban nem támogatott. Amikor létrehoz egy virtuális, adja meg a virtuális a formátumban. Ha szükséges, is átalakíthat VHDX lemez használatáról virtuális [`qemu-img convert`](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) vagy a [`Convert-VHD`](https://technet.microsoft.com/library/hh848454.aspx) PowerShell-parancsmag. További Azure nem támogatja a feltöltés a dinamikus VHD, így például a lemez átalakítása statikus VHD feltöltése előtt kell. Dinamikus lemez konvertálása Azure feltöltése során eszközök, például az [Azure virtuális ismerheti meg az Ugrás](https://github.com/Microsoft/azure-vhd-utils-for-go) is használhatja.

- A saját kép létrehozott VMs ugyanazt a tárterület-fiókot, magát a képet a kell lennie.
    - Tárterület-fiók és a saját kép és a létrehozott VMs tartása tároló létrehozása
    - Miután létrehozta a VMs, törölheti a kép biztonságosan

Győződjön meg arról, hogy rendelkezik-e jelentkezve, és erőforrás-kezelő üzemmód használata [Az Azure CLI](../xplat-cli-install.md) :

```bash
azure config mode arm
```

Az alábbi példák cserélje ki a kívánt értékét például paraméterek neve. Példa paraméterek neve szereplő `myResourceGroup`, `mystorageaccount`, és `myimages`.


<a id="prepimage"> </a>
## <a name="prepare-the-image-to-be-uploaded"></a>Fel kell tölteni a kép előkészítése

Azure támogatja a különböző Linux terjesztését (lásd: [Záradékkal terjesztését](virtual-machines-linux-endorsed-distros.md)). Az alábbi cikkekben végigvezeti Önt a különböző Linux terjesztését Azure támogatott előkészítése:

- **[Felosztás centOS-alapú](virtual-machines-linux-create-upload-centos.md)**
- **[Debian Linux](virtual-machines-linux-debian-create-upload-vhd.md)**
- **[Az Oracle Linux](virtual-machines-linux-oracle-create-upload-vhd.md)**
- **[Piros kalap vállalati Linux](virtual-machines-linux-redhat-create-upload-vhd.md)**
- **[SLES & openSUSE](virtual-machines-linux-suse-create-upload-vhd.md)**
- **[Ubuntu](virtual-machines-linux-create-upload-ubuntu.md)**
- **[Egyéb - nem záradékkal terjesztését](virtual-machines-linux-create-upload-generic.md)**

A **[Linux telepítéssel kapcsolatos megjegyzések](virtual-machines-linux-create-upload-generic.md#general-linux-installation-notes)** általánosabb tippek a képek Linux előkészítése Azure is megjelenik.

> [AZURE.NOTE] Az [Azure platform SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/) futó Linux csak a hitelesített terjesztését egyikét használja az konfigurációs adataival, a "Támogatott verziója a" [Linux Azure-Endorsed terjesztését a](virtual-machines-linux-endorsed-distros.md)megadott módon VMs vonatkozik.


## <a name="create-a-resource-group"></a>Erőforráscsoport létrehozása
Az erőforrás csoportok logikailag eredményét a Azure erőforrások támogatja a virtuális gépeken futó, például a virtuális hálózati és tárolási. További információt [az alábbi Azure erőforráscsoport](../azure-resource-manager/resource-group-overview.md). Az egyéni lemezre kép feltöltése és a VMs létrehozása előtt először kell hozzon létre egy erőforrás csoportot. 

Az alábbi példa létrehoz egy névvel ellátott erőforráscsoport `myResourceGroup` a a `WestUS` helyét:

```bash
azure group create myResourceGroup --location "WestUS"
```

## <a name="create-a-storage-account"></a>Tárterület-fiók létrehozása
VMs tárolja, oldal BLOB belül egy tárterület-fiókkal. További információt [az alábbi Azure blob-tárolóhoz](../storage/storage-introduction.md#blob-storage). Az egyéni lemez kép és VMs tárterület-fiók létrehozása Bármelyik Ön által létrehozott az egyéni lemez kép VMs kell ugyanazt a tárterület-fiókot, hogy a képe.

Az alábbi példa létrehoz egy tároló nevű fiókot `mystorageaccount` a korábban létrehozott erőforrás csoportban:

```bash
azure storage account create mystorageaccount --resource-group myResourceGroup \
    --location "WestUS" --kind Storage --sku-name PLRS
```

## <a name="list-storage-account-keys"></a>Listát tároló fiók kulcsok
Azure minden tárolási fiók két 512 bites hívóbetűk hoz létre. A hívóbetűk írási műveletek elvégzésére használatosak, amikor a tárterület-fiókjába, például hitelesítése. További információk: [Itt tároló való hozzáférés kezelése](../storage/storage-create-storage-account.md#manage-your-storage-account). A hívóbetűk megtekintheti a `azure storage account keys list` parancsot.

A létrehozott tárterület-fiókom a hívóbetűk megjelenítése:

```bash
azure storage account keys list mystorageaccount --resource-group myResourceGroup
```

A kimenet hasonlít:

```
info:    Executing command storage account keys list
+ Getting storage account keys
data:    Name  Key                                                                                       Permissions
data:    ----  ----------------------------------------------------------------------------------------  -----------
data:    key1  d4XAvZzlGAgWdvhlWfkZ9q4k9bYZkXkuPCJ15NTsQOeDeowCDAdB80r9zA/tUINApdSGQ94H9zkszYyxpe8erw==  Full
data:    key2  Ww0T7g4UyYLaBnLYcxIOTVziGAAHvU+wpwuPvK4ZG0CDFwu/mAxS/YYvAQGHocq1w7/3HcalbnfxtFdqoXOw8g==  Full
info:    storage account keys list command OK

```
Jegyezze le `key1` , a következő lépésekkel tárterület-fiókjába vezérléséhez fogja használni.

## <a name="create-a-storage-container"></a>Tárhely a tároló létrehozása
Különböző könyvtárak logikailag rendszerezheti a helyi fájlrendszerben létrehozott ugyanúgy hozzon létre egy virtuális lemez és képek rendszerezése tárterület-fiókkal belül tárolók. A tároló fiók tárolók tetszőleges számú is tartalmazhat. 

Az alábbi példa létrehoz egy tároló `myimages`, az előző lépésben a hívóbetű megadása kapott (`key1`):

```bash
azure storage container create --account-name mystorageaccount \
    --account-key key1 --container myimages
```

## <a name="upload-vhd"></a>Virtuális feltöltése
Most már ténylegesen feltöltheti a egyéni lemez képe. Az összes virtuális lemez VMs által használt tölt fel és tárolni a egyéni lemezre kép lap blob-ként.

Adja meg a hívóbetű, a tároló létrehozott az előző lépést, majd az egyéni lemez kép elérési útjának a helyi számítógépen:

```bash
azure storage blob upload --blobtype page --account-name mystorageaccount \
    --account-key key1 --container myimages /path/to/disk/mydisk.vhd
```

## <a name="create-vm-from-custom-image"></a>Virtuális létrehozása egyéni képe
Az egyéni lemez kép VMs létrehozásakor adja meg a URI a lemez képet. Győződjön meg arról, hogy az egyéni lemezre kép tároló cél tárolási fiók egyezéseket. A virtuális az Azure CLI vagy az erőforrás-kezelő JSON sablon használatával hozhat létre.


### <a name="create-a-vm-using-the-azure-cli"></a>Hozzon létre egy virtuális az Azure CLI használatával
Adja meg, hogy a `--image-urn` paramétert a `azure vm create` mutasson az egyéni lemez kép parancsot. Győződjön meg arról, hogy `--storage-account-name` a tárterület-fiókot az egyéni lemez kép tároló megegyezik. Nem kell használnia a azonos tároló az egyéni lemez képe, a VMs tárolásához. Ellenőrizze, hogy minden további tárolók létrehozása ugyanúgy, mint a korábbi lépéseket az egyéni lemez képek feltöltése előtt.

Az alábbi példa létrehoz egy virtuális nevű `myVM` az egyéni lemezre kép:

```bash
azure vm create myVM -l "WestUS" --resource-group myResourceGroup \
    --image-urn https://mystorageaccount.blob.core.windows.net/myimages/mydisk.vhd
    --storage-account-name mystorageaccount
```

Meg kell adnia, vagy választ kér, a további paramétereket követel meg az `azure vm create` például virtuális hálózati, nyilvános IP-cím, felhasználónév vagy SSH kulcsok parancsot. További információ a [rendelkezésre álló CLI erőforrás-kezelő paramétereket](azure-cli-arm-commands.md#azure-vm-commands-to-manage-your-azure-virtual-machines).

### <a name="create-a-vm-using-a-json-template"></a>Hozzon létre egy virtuális JSON sablon használatával
Azure erőforrás-kezelő sablonok a JavaScript objektum jelölés (JSON) fájlok határozza meg a kívánt összeállítása környezetet. A sablonok vannak lebontva különböző erőforrás szolgáltatók például számítási vagy hálózati. A meglévő sablonok használata, vagy írja be a saját. További információ [az erőforrás-kezelő és a sablonok](../azure-resource-manager/resource-group-overview.md).

Belül a `Microsoft.Compute/virtualMachines` a sablon szolgáltató, akkor a `storageProfile` a virtuális beállításainak részleteit tartalmazó csomópontot. A két fő paraméter szerkesztése a `image` és `vhd` az egyéni lemez kép és az új virtuális pillanatkép mutató URL-címe. Az alábbi példa az egyéni lemez képként használatához a JSON:

```bash
"storageProfile": {
          "osDisk": {
            "name": "myVM",
            "osType": "Linux",
            "caching": "ReadWrite",
            "createOption": "FromImage",
            "image": {
              "uri": "https://mystorageaccount.blob.core.windows.net/myimages/mydisk.vhd"
            },
            "vhd": {
              "uri": "https://mystorageaccount.blob.core.windows.net/vhds/newvmname.vhd"
            }
          }
```

[Ez](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image) a meglévő sablon létrehozása egy virtuális egy egyéni kép, vagy ismerje meg [a saját erőforrás-kezelő Azure-sablonok létrehozása](../resource-group-authoring-templates.md). 

Ha már beállított sablon, hoz létre a VMs használata a `azure group deployment create` parancsot. A JSON sablon URI-adja meg a `--template-uri` paraméter:

```bash
azure group deployment create --resource-group myResourceGroup
    --template-uri https://uri.to.template/mytemplate.json
```

Ha a számítógépen helyben tárolt fájl JSON, használhatja a `--template-file` paraméter helyett:

```bash
azure group deployment create --resource-group myResourceGroup
    --template-file /path/to/mytemplate.json
```


## <a name="next-steps"></a>Következő lépések
Kész és az egyéni virtuális lemezre feltölteni, erről további [erőforrás-kezelő és a sablonok](../azure-resource-manager/resource-group-overview.md)használatáról. Is célszerű [adatok lemezen](virtual-machines-linux-add-disk.md) hozzáadni az új VMs. Ha a VMs való hozzáférést igénylő futó alkalmazások, feltétlenül [Nyissa meg a portokat és a végpontok](virtual-machines-linux-nsg-quickstart.md).