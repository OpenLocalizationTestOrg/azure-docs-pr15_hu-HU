<properties
   pageTitle="A klasszikus telepítési modell PowerShell használatával több hálózati kártya VMs telepítése |} Microsoft Azure"
   description="Megtudhatja, hogy miként telepítése több hálózati kártya VMs a klasszikus telepítési modell PowerShell használatával"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor=""
   tags="azure-service-management"
/>
<tags  
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/02/2016"
   ms.author="jdial" />

#<a name="deploy-multi-nic-vms-classic-using-powershell"></a>Hálózati kártya VMs (klasszikus) PowerShell használatával több terjesztése

[AZURE.INCLUDE [virtual-network-deploy-multinic-classic-selectors-include.md](../../includes/virtual-network-deploy-multinic-classic-selectors-include.md)]

Azure virtuális gépeken futó (VMs) létrehozása és a csatolása a VMs mindegyike több hálózati kapcsolaton (NIC). Több forgalom típusú elkülönítésének engedélyezése a NIC keresztül. Például egy hálózati lehet kommunikálni az interneten, miközben egy másik kommunikál csak belső erőforrások nem csatlakozik az internethez. Több különböző hálózati forgalmának engedélyezésére elkülönítésére lehetősége szükség, sok hálózati virtuális készülékek, például az alkalmazás szállítási és a WAN-optimalizálási megoldásokért.

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-classic-include.md)]Megtudhatja, miként [végezheti el ezeket a lépéseket az erőforrás-kezelő modellt használja](virtual-network-deploy-multinic-arm-ps.md).

[AZURE.INCLUDE [virtual-network-deploy-multinic-scenario-include.md](../../includes/virtual-network-deploy-multinic-scenario-include.md)]

Jelenleg nem lehet VMs a egy egyetlen hálózati kártya és a több VMs ugyanazt a felhőalapú szolgáltatást. Ezért kell a hátsó vége kiszolgálók megvalósítása valamelyik másik felhőszolgáltatásában mint és más összetevőket az alkalmazási példát. Az alábbi lépésekkel használata egy felhőalapú szolgáltatásba, *IaaSStory* nevű fő erőforrásokat, illetve *IaaSStory-Kódmentes* a hátsó vége kiszolgálókhoz.

## <a name="prerequisites"></a>Előfeltételek

A visszahívási vége kiszolgálókat alkalmazhat, mielőtt kell ebben az esetben a szükséges erőforrások fő felhőalapú szolgáltatás telepítése. Legalább egy virtuális hálózati létrehozását a kódmentes alhálózat szükséges. Látogasson el a [PowerShell használatával virtuális hálózat létrehozása](virtual-networks-create-vnet-classic-netcfg-ps.md) egy virtuális hálózati telepítése című témakörből.

[AZURE.INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="deploy-the-back-end-vms"></a>A háttér VMs terjesztése

A kódmentes VMs attól függenek, hogy az alább felsorolt erőforrások létrehozását.

- **Kódmentes alhálózat**. Az adatbázis-kiszolgálók külön alhálózathoz, a forgalom újból része lesz. Az alábbi parancsfájl vár, egy névvel ellátott *WTestVnet*vnet olyan alhálózat.
- **Adatok lemezt tárterület-fiók**. A jobb teljesítmény elérése érdekében az adatok lemezt adatbázis kiszolgálókon prémium tárterület-fiók szükséges egyszínű állapota (SSD) meghajtó technológia fogja használni. Ellenőrizze, hogy a támogatási prémium tároló rendszerbe Azure helyét.
- **Elérhetőség beállítása**. Egy egyetlen elérhetőségének beállítása, győződjön meg róla a VMs legalább egyike be és operációs rendszert futtató karbantartáskor kell használni hozzáadódik az összes adatbázis-kiszolgálók.

### <a name="step-1---start-your-script"></a>Lépés: 1 – útmutató a parancsprogram

Töltse le a teljes PowerShell-parancsprogramot, használja [az alábbi](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/classic/virtual-network-deploy-multinic-classic-ps.ps1). Kövesse az alábbi lépésekkel módosíthatja a parancsfájl a munkát a környezetben.

1. Módosítsa a meglévő erőforráscsoport üzembe feletti [vonatkozó követelmények](#Prerequisites)alapján változót az alábbi értékeket.

        $location              = "West US"
        $vnetName              = "WTestVNet"
        $backendSubnetName     = "BackEnd"

2. Módosítsa a kódmentes telepítéshez használni kívánt értékek alapján változót az alábbi értékeket.

        $backendCSName         = "IaaSStory-Backend"
        $prmStorageAccountName = "iaasstoryprmstorage"
        $avSetName             = "ASDB"
        $vmSize                = "Standard_DS3"
        $diskSize              = 127
        $vmNamePrefix          = "DB"
        $dataDiskSuffix        = "datadisk"
        $ipAddressPrefix       = "192.168.2."
        $numberOfVMs           = 2

### <a name="step-2---create-necessary-resources-for-your-vms"></a>Lépés: 2 – a szükséges erőforrások létrehozása a VMs

Egy új, felhőalapú szolgáltatásba, és adatok lemezt a tárterület-fiók létrehozása az összes VMs szüksége. Meg kell egy képet, és a helyi rendszergazdafiók megadása a VMs. Ezek az erőforrások létrehozásához hajtsa végre az alábbi lépéseket.

1. Hozzon létre egy új felhőszolgáltatásba.

        New-AzureService -ServiceName $backendCSName -Location $location

2. Prémium tároló új ügyfél létrehozása.

        New-AzureStorageAccount -StorageAccountName $prmStorageAccountName `
            -Location $location `
            -Type Premium_LRS

3. Az előfizetés aktuális tároló fiókként az előbb létrehozott tároló fiókot beállítani.

        $subscription = Get-AzureSubscription `
            | where {$_.IsCurrent -eq $true}  
        Set-AzureSubscription -SubscriptionName $subscription.SubscriptionName `
            -CurrentStorageAccountName $prmStorageAccountName

4. Jelöljön ki egy képet a virtuális.

        $image = Get-AzureVMImage `
            | where{$_.ImageFamily -eq "SQL Server 2014 RTM Web on Windows Server 2012 R2"} `
            | sort PublishedDate -Descending `
            | select -ExpandProperty ImageName -First 1

5. Adja meg a helyi rendszergazdai fiók hitelesítő adatait.

        $cred = Get-Credential -Message "Enter username and password for local admin account"

### <a name="step-3---create-vms"></a>Lépés a 3 - VMs létrehozása

Akkor használja a ismétlődve hozhat létre, és hozzon létre a szükséges hálózati kártyák és VMs belül a leállításig annyi VMs. A hálózati kártyák és VMs létrehozásához hajtsa végre az alábbi lépéseket.

1. Indítsa el a `for` ismétlése hozhat létre egy virtuális és két kártyát, ahányszor szükséges, a parancsok ismétlődő értékét alapján a `$numberOfVMs` változó.

        for ($suffixNumber = 1; $suffixNumber -le $numberOfVMs; $suffixNumber++){

2. Hozzon létre egy `VMConfig` adja meg a képet, méretét és a virtuális beállítása elérhetősége objektumot.

            $vmName = $vmNamePrefix + $suffixNumber
            $vmConfig = New-AzureVMConfig -Name $vmName `
                            -ImageName $image `
                            -InstanceSize $vmSize `
                            -AvailabilitySetName $avSetName  

3. A virtuális kiépítése virtuális Windows szerint.

            Add-AzureProvisioningConfig -VM $vmConfig -Windows `
                -AdminUsername $cred.UserName `
                -Password $cred.Password

4. Az alapértelmezett hálózati, és rendelje hozzá a statikus IP-címet.

            Set-AzureSubnet -SubnetNames $backendSubnetName -VM $vmConfig
            Set-AzureStaticVNetIP -IPAddress ($ipAddressPrefix+$suffixNumber+3) -VM $vmConfig

5. Adja hozzá a második hálózati kártya minden virtuális.

            Add-AzureNetworkInterfaceConfig -Name ("RemoteAccessNIC"+$suffixNumber) `
                -SubnetName $backendSubnetName `
                -StaticVNetIPAddress ($ipAddressPrefix+(53+$suffixNumber)) `
                -VM $vmConfig

6. Minden egyes virtuális adatok lemezre hozhat létre.

            $dataDisk1Name = $vmName + "-" + $dataDiskSuffix + "-1"    
            Add-AzureDataDisk -CreateNew -VM $vmConfig `
                -DiskSizeInGB $diskSize `
                -DiskLabel $dataDisk1Name `
                -LUN 0       

            $dataDisk2Name = $vmName + "-" + $dataDiskSuffix + "-2"   
            Add-AzureDataDisk -CreateNew -VM $vmConfig `
                -DiskSizeInGB $diskSize `
                -DiskLabel $dataDisk2Name `
                -LUN 1

7. Minden egyes virtuális létrehozása és a Befejezés le.

            New-AzureVM -VM $vmConfig `
                -ServiceName $backendCSName `
                -Location $location `
                -VNetName $vnetName
        }

### <a name="step-4---run-the-script"></a>Lépés: 4 - futtatása

Most, hogy letölti-megváltozott a szükségletek parancsfájl, runt ő parancsfájlok létrehozását a háttér adatbázis VMs több.

1. A parancsfájlok mentse, és futtassa a **PowerShell** a parancssor parancsot, vagy a **PowerShell ISE**. A kezdeti kimenet alább látható módon jelenik meg.

        OperationDescription    OperationId                          OperationStatus
        --------------------    -----------                          ---------------
        New-AzureService        xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx Succeeded      
        New-AzureStorageAccount xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx Succeeded      

        WARNING: No deployment found in service: 'IaaSStory-Backend'.

2. Adja meg a hitelesítő adatok parancssorában szükséges adatokat, és kattintson az **OK gombra**. A kimenet az alábbi jelennek meg.

        New-AzureVM             xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx Succeeded
        New-AzureVM             xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx Succeeded
