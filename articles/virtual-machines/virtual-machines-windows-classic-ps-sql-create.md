<properties
    pageTitle="Az SQL Server virtuális gép létrehozása az Azure PowerShell (klasszikus) |} Microsoft Azure"
    description="Az Azure virtuális létrehozásához az SQL Server virtuális gépek galéria képekkel biztosít a lépéseket, és a PowerShell-parancsfájlokat. Ez a témakör a klasszikus telepítési mód használ."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="rothja"
    manager="jhubbard"
    editor=""
    tags="azure-service-management" />
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="10/07/2016"
    ms.author="jroth" />

# <a name="provision-a-sql-server-virtual-machine-using-azure-powershell-classic"></a>Az SQL Server Azure PowerShell (klasszikus) használatával virtuális gép kiépítése

## <a name="overview"></a>– Áttekintés

Ez a cikk bemutatja, hogyan hozhat létre virtuális gép SQL Server Azure-ban a PowerShell-parancsmagokkal.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Ez a témakör erőforrás-kezelő verziójának [rendelkezni az SQL Server Azure PowerShell-kezelővel virtuális gép](virtual-machines-windows-ps-sql-create.md)talál.

### <a name="install-and-configure-powershell"></a>Telepítse és állítsa be a PowerShell:

1. Ha nem rendelkezik az Azure-fiók, látogasson el a [Azure próbaverzió szabad](https://azure.microsoft.com/pricing/free-trial/).

2. [Töltse le és telepítse a legújabb Azure PowerShell-parancsait](../powershell-install-configure.md).

3. Indítsa el a Windows Powershellt, és csatlakoztassa a **Hozzáadás-AzureAccount** paranccsal Azure előfizetését.

        Add-AzureAccount

## <a name="determine-your-target-azure-region"></a>A target Azure terület meghatározása

Az SQL Server virtuális gép fog tárolni a egy felhőalapú szolgáltatásba, amely egy adott Azure területre. Az alábbi lépéseket a Súgó alapján megállapíthatja, hogy a régió, a tárterület-fiók és a felhő az oktatóprogram hátralevő használt szolgáltatás.

1. Határozza meg az Adatközpont használatával szolgáltató az SQL Server virtuális kívánt. Az alábbi PowerShell-parancsait, megjelenik a rendelkezésre álló régiók végén összefoglaló listával részletesen.

        Get-AzureLocation
        (Get-AzureLocation).Name

2.  Miután azonosította, hogy a használni kívánt helyre, állítsa a **$dcLocation** a régióba nevű változó.

        $dcLocation = "<region name>"

## <a name="set-your-subscription-and-storage-account"></a>A tárhely és előfizetési fiókot beállítani

1. Határozza meg az új virtuális gép használni az Azure előfizetést.

        (Get-AzureSubscription).SubscriptionName

1. A target Azure előfizetés rendelhet a **$subscr** változó. Kattintson a beállításokat, az aktuális Azure előfizetés.

        $subscr="<subscription name>"
        Select-AzureSubscription -SubscriptionName $subscr –Current

1. Ellenőrizze a meglévő tárterület-fiókokat. A következő parancsfájl a kiválasztott területi megtalálható minden tárterület-fiók jeleníti meg:

        (Get-AzureStorageAccount | where { $_.GeoPrimaryLocation -eq $dcLocation }).StorageAccountName

    >[AZURE.NOTE] Ha szüksége van egy új tárterület-fiókot, előbb hozza létre az összes kisbetű tároló fióknévben a New-AzureStorageAccount parancs, ahogy az alábbi példa: **Új-AzureStorageAccount - StorageAccountName "<storage account name>"-hely $dcLocation**

1. A cél tároló fióknév rendelhet a **$staccount**. **Set-AzureSubscription** segítségével előfizetést és a jelenlegi tárterület-fiók beállítása.

        $staccount="<storage account name>"
        Set-AzureSubscription -SubscriptionName $subscr -CurrentStorageAccountName $staccount

## <a name="select-a-sql-server-virtual-machine-image"></a>Jelöljön ki egy SQL Server virtuális gép képet

1. Megtudhatja, hogy a listában elérhető SQL Server virtuális gépeken futó képek a gyűjteményből. Ezeket a képeket az összes olyan **ImageFamily** tulajdonság "SQL" kezdetű van. A következő lekérdezés a kép család érhető el, akkor az SQL Server előtelepített jeleníti meg.

        Get-AzureVMImage | where { $_.ImageFamily -like "SQL*" } | select ImageFamily -Unique | Sort-Object -Property ImageFamily

1. Amikor megtalálta a virtuális gép kép család, is okozhatja több közzétett képeket a család. Az alábbi parancsprogramot használva keresse meg a legfrissebb közzétett virtuális gép kép nevét (például **SQL Server 2016 a Windows Server 2012 R2 RTM Enterprise**) a kijelölt kép család:

        $family="<ImageFamily value>"
        $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

        echo "Selected SQL Server image name:"
        echo "   $image"

## <a name="create-the-virtual-machine"></a>A virtuális gép létrehozása

Végül a PowerShell a virtuális gép létrehozása:

1. Hozzon létre egy felhőalapú szolgáltatásba, az új virtuális tárolni. Figyelje meg, hogy az is lehetséges, használja helyette egy meglévő felhőalapú szolgáltatást. Hozzon létre egy új változó **$svcname** a felhőbeli szolgáltatástól rövid nevét.

        $svcname = "<cloud service name>"
        New-AzureService -ServiceName $svcname -Label $svcname -Location $dcLocation

2. Adja meg a virtuális számítógépnek a neve és a méretet. Virtuális gép méretű kapcsolatos további tudnivalókért olvassa el a [Azure virtuális gép méretben](virtual-machines-windows-sizes.md)című témakört.

        $vmname="<machine name>"
        $vmsize="<Specify one: Large, ExtraLarge, A5, A6, A7, A8, A9, or see the link to the other VM sizes>"
        $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

3. Adja meg a helyi rendszergazdafiók és a jelszavát.

        $cred=Get-Credential -Message "Type the name and password of the local administrator account."
        $vm1 | Add-AzureProvisioningConfig -Windows -AdminUsername $cred.GetNetworkCredential().Username -Password $cred.GetNetworkCredential().Password

4. Futtassa a következő a virtuális gép létrehozása.

        New-AzureVM –ServiceName $svcname -VMs $vm1

>[AZURE.NOTE] [Azure PowerShell használatá létrehozása, és adja meg a Windows-alapú virtuális gépeken futó előre](virtual-machines-windows-classic-create-powershell.md)további magyarázata és a beállítások, **a set parancs összeállítása** című.

## <a name="example-powershell-script"></a>Példa PowerShell-parancsprogramot

A következő parancsfájl tartalmaz, illetve teljes parancsfájl, amely létrehoz egy **SQL Server 2016 a Windows Server 2012 R2 RTM Enterprise** virtuális számítógépre. Ha használja ezt a parancsfájlt, testre kell szabnia a kezdeti változók az előző lépéseket a jelen témakör alapján.

    # Customize these variables based on your settings and requirements:
    $dcLocation = "East US"
    $subscr="mysubscription"
    $staccount="mystorageaccount"
    $family="SQL Server 2016 RTM Enterprise on Windows Server 2012 R2"
    $svcname = "mycloudservice"
    $vmname="myvirtualmachine"
    $vmsize="A5"

    # Set the current subscription and storage account
    # Comment out the New-AzureStorageAccount line if the account already exists
    Select-AzureSubscription -SubscriptionName $subscr –Current
    New-AzureStorageAccount -StorageAccountName $staccount -Location $dcLocation
    Set-AzureSubscription -SubscriptionName $subscr -CurrentStorageAccountName $staccount

    # Select the most recent VM image in this image family:
    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

    # Create the new cloud service; comment out this line if cloud service exists already:
    New-AzureService -ServiceName $svcname -Label $svcname -Location $dcLocation

    # Create the VM config:
    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

    # Set administrator credentials:
    $cred=Get-Credential -Message "Type the name and password of the local administrator account."
    $vm1 | Add-AzureProvisioningConfig -Windows -AdminUsername $cred.GetNetworkCredential().Username -Password $cred.GetNetworkCredential().Password

    # Create the SQL Server VM:
    New-AzureVM –ServiceName $svcname -VMs $vm1


## <a name="connect-with-remote-desktop"></a>Csatlakozás távoli asztali

1. Hozzon létre a. Indítsa el az alábbi virtuális gépeken futó telepítés befejezéséhez az aktuális felhasználó dokumentum mappában lévő fájlok RDP:

        $documentspath = [environment]::getfolderpath("mydocuments")
        Get-AzureRemoteDesktopFile -ServiceName $svcname -Name $vmname -LocalPath "$documentspath\vm1.rdp"

1. A Dokumentumok könyvtárban indítsa el az RDP-fájlt. Kapcsolatba a rendszergazdai felhasználónév és jelszó korábban megadott (például felhasználónevét VMAdmin volt, ha "\VMAdmin" adja meg a felhasználót, és adja meg a jelszót).

        cd $documentspath
        .\vm1.rdp

## <a name="complete-the-configuration-of-the-sql-server-machine-for-remote-access"></a>Teljes távoli eléréséhez, az SQL Server számítógépen konfigurálása

Távoli asztali készülék bejelentkezés után állítsa be az SQL Server alapján a [lépéseket az SQL Server-Azure virtuális való csatlakozással beállítása](virtual-machines-windows-classic-sql-connect.md#steps-for-configuring-sql-server-connectivity-in-an-azure-vm)című témakörben leírt útmutatást.

## <a name="next-steps"></a>Következő lépések

A virtuális gépeken futó PowerShell [virtuális gépeken futó dokumentáció](virtual-machines-windows-classic-create-powershell.md)kiépítéséhez további útmutatást talál. Az SQL Server és a prémium tároló kapcsolatos további parancsfájlok a [Használati Azure prémium tárhely a virtuális gépeken futó SQL Server](virtual-machines-windows-classic-sql-server-premium-storage.md)című témakör.

Sok esetben a következő lépésként az adatbázisok áttelepítése a új SQL Server virtuális. Adatbázis áttelepítése, olvassa el az [SQL Server-Azure virtuális az adatbázis áttelepítése](virtual-machines-windows-migrate-sql.md).

Ha is az Azure portál használatával hozhat létre SQL virtuális gépeken futó érdekli, olvassa el a [virtuális gép SQL Server Azure a kiépítési](virtual-machines-windows-portal-sql-server-provision.md)című témakört. Ne feledje, hogy az oktatóanyagot, amelyek végigvezetik Önt a portálon VMs PowerShell témakör alkalmazott Klasszikus modell helyett a javasolt erőforrás-kezelő modell.

Ezek az erőforrások kívül azt javasoljuk, hogy tekintse át [az Azure virtuális gépeken futó SQL Server futtatásához kapcsolódó témakörök](virtual-machines-windows-sql-server-iaas-overview.md).
