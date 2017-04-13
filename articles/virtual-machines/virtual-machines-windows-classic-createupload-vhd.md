<properties
    pageTitle="Hozzon létre, és töltse fel a Powershell használatá virtuális képként |} Microsoft Azure"
    description="Ismerje meg, hozhat létre, és töltse fel az általános Windows Server képet (virtuális) klasszikus telepítési modell és Azure Powershell használatával."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/21/2016"
    ms.author="cynthn"/>

# <a name="create-and-upload-a-windows-server-vhd-to-azure"></a>Hozzon létre, és töltse fel a Windows Server virtuális Azure

Ez a cikk bemutatja, hogyan saját általános virtuális kép feltöltése egy virtuális merevlemez (virtuális), így hozhat létre virtuális gépeken futó használhatja. A lemez és a Microsoft Azure virtuális merevlemezek, lásd: [A lemez és a virtuális gépeken futó VHD](virtual-machines-linux-about-disks-vhds.md).


[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]. Is [feltöltése](virtual-machines-windows-upload-image.md) egy virtuális számítógépre, az erőforrás-kezelő modellt használja. 

## <a name="prerequisites"></a>Előfeltételek

Ez a cikk tartalma feltételezi, hogy van:

- **Az Azure-előfizetés** – Ha nincs telepítve egyik, akkor [Nyissa meg az ingyenes Azure-fiók](/pricing/free-trial/?WT.mc_id=A261C142F).

- **[Microsoft Azure PowerShell](../powershell-install-configure.md)** - van a Microsoft Azure PowerShell-modult telepítette és beállította az előfizetés használni. 

- **A . Virtuális fájl** - támogatott Windows operációs rendszer .vhd fájlokban tárolt és csatolni virtuális géphez. Ellenőrizze, hogy ha a kiszolgálói szerepkörök, a virtuális futó Sysprep által támogatott. További tudnivalókért olvassa el a [Kiszolgálói szerepkörök Sysprep támogatása](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)című témakört.

> [AZURE.IMPORTANT] A Microsoft Azure VHDX formátuma nem támogatott. A lemez átalakíthatja a Hyper-V kezelője vagy a [Konvertálás-virtuális parancsmag](http://technet.microsoft.com/library/hh848454.aspx)használatával virtuális formátum. Részletekért olvassa el a [blogpost](http://blogs.msdn.com/b/virtual_pc_guy/archive/2012/10/03/using-powershell-to-convert-a-vhd-to-a-vhdx.aspx).

## <a name="step-1-prep-the-vhd"></a>Lépés: 1: A virtuális előkészítése 

A virtuális feltöltése Azure, mielőtt kell kell általános az Sysprep eszközzel. Ez előkészíti a virtuális használandó képet. További információ a rendszer-előkészítő eszközről [Sysprep használatát mikéntjéről: Bevezetés az](http://technet.microsoft.com/library/bb457073.aspx). Sysprep futtatása előtt készítsen biztonsági másolatot a virtuális.

Az operációs rendszer telepítve van, a virtuális gépen hajtsa végre az alábbi lépésekkel:

1. Jelentkezzen be az operációs rendszer.

2. Nyisson meg egy parancssorablakot rendszergazdaként. A könyvtár módosítása **%windir%\system32\sysprep**, és futtassa `sysprep.exe`.

    ![Nyisson meg egy parancssorablakot](./media/virtual-machines-windows-classic-createupload-vhd/sysprep_commandprompt.png)

3.  A **Rendszer előkészítése eszköz** párbeszédpanel jelenik meg.

    ![Indítsa el a Sysprep](./media/virtual-machines-windows-classic-createupload-vhd/sysprepgeneral.png)

4.  A **Rendszer előkészítése eszköz**jelölje ki az **Adja meg rendszer Házon kívül párbeszédpanel élmény (OOBE)** , és győződjön meg arról, hogy be van-e jelölve **Generalize** .

5.  Válassza a **Leállítás** **Leállítási beállítások**.

6.  Kattintson az **OK gombra**.

## <a name="step-2-create-a-storage-account-and-a-container"></a>Lépés: 2: Hozzon létre egy tárterület-fiók és a tároló

A tároló fiókra Azure-ban van szüksége, így Önnek nem kell a .vhd fájl feltöltése egy helyen. Ebben a lépésben bemutatja, hogyan hozhat létre fiók és a szükséges adatok beolvasása a meglévő fiók. Cserélje le a változók &lsaquo; szögletes zárójelek között &rsaquo; a sajátjaira.

1. Bejelentkezés

        Add-AzureAccount

1. Az Azure előfizetés beállítása.

        Select-AzureSubscription -SubscriptionName <SubscriptionName> 

2. Hozzon létre egy új tárterület-fiókot. A tároló fiók egyedinek kell lennie, 3-24 karaktereket. A név lehet betűk és számok tetszőleges kombinációjával. Adjon meg egy helyet, például "US keleti" szükséges
        
        New-AzureStorageAccount –StorageAccountName <StorageAccountName> -Location <Location>

3. Az új tárterület-fiók beállítása alapértelmezettként.
        
        Set-AzureSubscription -CurrentStorageAccountName <StorageAccountName> -SubscriptionName <SubscriptionName>

4. Hozzon létre egy új tároló.

        New-AzureStorageContainer -Name <ContainerName> -Permission Off

 

## <a name="step-3-upload-the-vhd-file"></a>3 lépés: A .vhd fájl feltöltése

A [Hozzáadás-AzureVhd](http://msdn.microsoft.com/library/dn495173.aspx) segítségével töltse fel a virtuális.

Az ablakból Azure Powershellt használta az előző lépésben, írja be a következő parancsot, és a változók cseréje &lsaquo; szögletes zárójelek között &rsaquo; a sajátjaira.

        Add-AzureVhd -Destination "https://<StorageAccountName>.blob.core.windows.net/<ContainerName>/<vhdName>.vhd" -LocalFilePath <LocalPathtoVHDFile>


## <a name="step-4-add-the-image-to-your-list-of-custom-images"></a>Lépés: 4: A kép hozzáadása egyéni képek listájához

A [Hozzáadás-AzureVMImage](https://msdn.microsoft.com/library/mt589167.aspx) parancsmag használatával a kép elhelyezése a egyéni képek listája.

        Add-AzureVMImage -ImageName <ImageName> -MediaLocation "https://<StorageAccountName>.blob.core.windows.net/<ContainerName>/<vhdName>.vhd" -OS "Windows"


## <a name="next-steps"></a>Következő lépések

Azt is megteheti, hogy [Hozzon létre egy egyéni virtuális](virtual-machines-windows-classic-createportal.md) a feltöltött kép felhasználásával.

