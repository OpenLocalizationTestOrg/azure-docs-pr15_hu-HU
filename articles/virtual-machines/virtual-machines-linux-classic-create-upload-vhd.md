<properties
    pageTitle="Hozzon létre, és töltse fel a Linux virtuális |} Microsoft Azure"
    description="Hozzon létre, és töltse fel az Azure virtuális merevlemez (virtuális) és a klasszikus telepítési modellt, amely tartalmazza a Linux operációs rendszer."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/01/2016"
    ms.author="iainfou"/>

# <a name="creating-and-uploading-a-virtual-hard-disk-that-contains-the-linux-operating-system"></a>Létrehozásáról és a Linux operációs rendszert tartalmazó virtuális merevlemez feltöltése

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Is [Töltse fel az erőforrás-kezelő Azure használatával egyéni lemez képet](virtual-machines-linux-upload-vhd.md).

Ez a cikk bemutatja, hogyan hozhat létre és feltöltése egy virtuális merevlemez (virtuális), hogy a vele, a saját kép létrehozása Azure virtuális gépeken futó. Megtudhatja, hogy miként előkészítése az operációs rendszer, így több virtuális gépeken futó alapján, hogy a kép létrehozásához használhatja. 

>  [AZURE.NOTE] Ha egy darabig, kérjük, segítse a szolgáltatás javítható az Azure Linux virtuális át ezt a [rövid felmérés](https://aka.ms/linuxdocsurvey) a tapasztalok vételével. Minden válasz segít a munka elvégzéséhez kaphat segítséget.

## <a name="prerequisites"></a>Előfeltételek
Ez a cikk tartalma feltételezi, hogy rendelkezik-e az alábbiakat:

- **Linux operációs rendszereken .vhd fájlban** - telepítve van az [Azure záradékkal Linux eloszlás](virtual-machines-linux-endorsed-distros.md) (vagy lásd az [információk nem záradékkal terjesztését](virtual-machines-linux-create-upload-generic.md)) virtuális lemezre virtuális formátumban. Több eszközök hozhat létre egy virtuális és virtuális létezik:
    - Telepítése és beállítása a [QEMU](https://en.wikibooks.org/wiki/QEMU/Installing_QEMU) vagy [KVM](http://www.linux-kvm.org/page/RunningKVM), ügyelve arra, hogy a kép formázása virtuális használni. Ha szükséges, használatával [kép konvertálása](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) `qemu-img convert`.
    - A Hyper-V [a Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_install) , vagy [a Windows Server 2012/2012 R2](https://technet.microsoft.com/library/hh846766.aspx)is használhatja.

> [AZURE.NOTE] Az új VHDX formátumot Azure-ban nem támogatott. Amikor létrehoz egy virtuális, adja meg a virtuális a formátumban. Ha szükséges, is átalakíthat VHDX lemez használatáról virtuális [`qemu-img convert`](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) vagy a [`Convert-VHD`](https://technet.microsoft.com/library/hh848454.aspx) PowerShell-parancsmag. További Azure nem támogatja a feltöltés a dinamikus VHD, így például a lemez átalakítása statikus VHD feltöltése előtt kell. Dinamikus lemez konvertálása Azure feltöltése során eszközök, például az [Azure virtuális ismerheti meg az Ugrás](https://github.com/Microsoft/azure-vhd-utils-for-go) is használhatja.

- **Azure parancssori felület** - telepítse a legújabb [Azure parancssor](../virtual-machines-command-line-tools.md) töltse fel a virtuális.

<a id="prepimage"> </a>
## <a name="step-1-prepare-the-image-to-be-uploaded"></a>Lépés: 1: Fel kell tölteni a kép előkészítése

Azure támogatja a különböző Linux terjesztését (lásd: [Záradékkal terjesztését](virtual-machines-linux-endorsed-distros.md)). Az alábbi cikkekben végigvezeti Önt a különböző Linux terjesztését Azure támogatott előkészítése. Miután az alábbi útmutatókat lépéseit, munkaterületre vissza Ha már készen áll a töltse fel a Azure virtuális fájlt:

- **[Felosztás centOS-alapú](virtual-machines-linux-create-upload-centos.md)**
- **[Debian Linux](virtual-machines-linux-debian-create-upload-vhd.md)**
- **[Az Oracle Linux](virtual-machines-linux-oracle-create-upload-vhd.md)**
- **[Piros kalap vállalati Linux](virtual-machines-linux-redhat-create-upload-vhd.md)**
- **[SLES & openSUSE](virtual-machines-linux-suse-create-upload-vhd.md)**
- **[Ubuntu](virtual-machines-linux-create-upload-ubuntu.md)**
- **[Egyéb - nem záradékkal terjesztését](virtual-machines-linux-create-upload-generic.md)**

> [AZURE.NOTE] Az Azure platform SLA virtuális gépeken futó operációs rendszer használata a Linux csak akkor, amikor a hitelesített terjesztését közül a "Támogatott verziója a" [Linux Azure-Endorsed terjesztését a](virtual-machines-linux-endorsed-distros.md)megadott módon konfigurációs adatokkal szolgál vonatkozik. Azure kép gyűjteményben lévő összes Linux terjesztését a szükséges konfigurációs hitelesített varianciája.

A **[Linux telepítéssel kapcsolatos megjegyzések](virtual-machines-linux-create-upload-generic.md#general-linux-installation-notes)** általánosabb tippek a képek Linux előkészítése Azure is megjelenik.


<a id="connect"> </a>
## <a name="step-2-prepare-the-connection-to-azure"></a>Lépés: 2: Azure csatlakozásának előkészítése

Ellenőrizze, hogy az Azure CLI esetén a klasszikus telepítési modell (`azure config mode asm`), majd jelentkezzen be a fiókjához:

```
azure login
```


<a id="upload"> </a>
## <a name="step-3-upload-the-image-to-azure"></a>3 lépés: A kép feltöltése Azure

A virtuális fájl feltöltése a tárterület-fiókra van szüksége. Választhat vagy egy meglévő tárterület-fiókot vagy [Hozzon létre egy újat](../storage/storage-create-storage-account.md).

Az Azure CLI segítségével a kép feltöltése a következő parancs használatával:

```bash
azure vm image create <ImageName> `
    --blob-url <BlobStorageURL>/<YourImagesFolder>/<VHDName> `
    --os Linux <PathToVHDFile>
```

Az előző példában:

- **BlobStorageURL** a tárterület-fiók használni kívánt URL-címe
- **YourImagesFolder** a tároló blob-tárolóhoz belül, amelyre a képeket tárolja.
- **VHDName** a címkét, amely azonosítja a virtuális merevlemez portálon jelenik meg.
- **PathToVHDFile** , a teljes elérési útját és nevét a .vhd fájlt a számítógépen.

Az alábbi példában egy kész példa látható:

```bash
azure vm image create UbuntuLTS `
    --blob-url https://teststorage.blob.core.windows.net/vhds/UbuntuLTS.vhd `
    --os Linux /home/ahmet/UbuntuLTS.vhd
```

## <a name="step-4-create-a-vm-from-the-image"></a>Lépés: 4: A kép egy virtuális létrehozása
A virtuális használatával létrehozott `azure vm create` a ugyanúgy, mint a hagyományos virtuális. Adja meg a képet az előző lépésben megadott névnek. A következő példa azt használja az előző lépésben megadott **UbuntuLTS** kép neve nevet:

```bash
azure vm create --userName ops --password P@ssw0rd! --vm-size Small --ssh `
    --location "West US" "DeployedUbuntu" UbuntuLTS
```

A saját VMs létrehozásához adja meg a saját felhasználónevét + jelszót, hely, tartománynév és kép neve nevet.

## <a name="next-steps"></a>Következő lépések

További tudnivalókért lásd: [Azure klasszikus telepítési modell Azure CLI hivatkozását](../virtual-machines-command-line-tools.md).

[Step 1: Prepare the image to be uploaded]: #prepimage
[Step 2: Prepare the connection to Azure]: #connect
[Step 3: Upload the image to Azure]: #upload
