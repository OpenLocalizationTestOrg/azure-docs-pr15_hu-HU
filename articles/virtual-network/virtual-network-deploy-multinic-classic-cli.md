<properties
   pageTitle="Több hálózati kártya VMs az Azure CLI használata a klasszikus telepítési modell telepítése |} Microsoft Azure"
   description="Megtudhatja, hogy miként több hálózati kártya VMs az Azure CLI használata a klasszikus telepítési modell terjesztése"
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

#<a name="deploy-multi-nic-vms-classic-using-the-azure-cli"></a>Több hálózati kártya VMs (klasszikus) használ az Azure CLI terjesztése

[AZURE.INCLUDE [virtual-network-deploy-multinic-classic-selectors-include.md](../../includes/virtual-network-deploy-multinic-classic-selectors-include.md)]

Azure virtuális gépeken futó (VMs) létrehozása és a csatolása a VMs mindegyike több hálózati kapcsolaton (NIC). Több forgalom típusú elkülönítésének engedélyezése a NIC keresztül. Például egy hálózati lehet kommunikálni az interneten, miközben egy másik kommunikál csak belső erőforrások nem csatlakozik az internethez. Több különböző hálózati forgalmának engedélyezésére elkülönítésére lehetősége szükség, sok hálózati virtuális készülékek, például az alkalmazás szállítási és a WAN-optimalizálási megoldásokért.

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-classic-include.md)]Megtudhatja, miként [végezheti el ezeket a lépéseket az erőforrás-kezelő modellt használja](virtual-network-deploy-multinic-arm-cli.md).

[AZURE.INCLUDE [virtual-network-deploy-multinic-scenario-include.md](../../includes/virtual-network-deploy-multinic-scenario-include.md)]

Jelenleg nem lehet VMs a egy egyetlen hálózati kártya és a több VMs ugyanazt a felhőalapú szolgáltatást. Ezért kell a hátsó vége kiszolgálók megvalósítása valamelyik másik felhőszolgáltatásában mint és más összetevőket az alkalmazási példát. Az alábbi lépésekkel egy felhőalapú szolgáltatásba, *IaaSStory* nevű fő erőforrásokat, illetve *IaaSStory-Kódmentes* vissza vége kiszolgálók használja.

## <a name="prerequisites"></a>Előfeltételek

A visszahívási vége kiszolgálókat alkalmazhat, mielőtt kell ebben az esetben a szükséges erőforrások fő felhőalapú szolgáltatás telepítése. Legalább egy virtuális hálózati létrehozását a kódmentes alhálózat szükséges. Látogasson el a megtudhatja, hogy miként üzembe virtuális hálózat [létrehozása az Azure CLI segítségével virtuális hálózat](virtual-networks-create-vnet-classic-cli.md) .

[AZURE.INCLUDE [azure-cli-prerequisites-include.md](../../includes/azure-cli-prerequisites-include.md)]

## <a name="deploy-the-back-end-vms"></a>A háttér VMs terjesztése

A kódmentes VMs attól függenek, hogy az alább felsorolt erőforrások létrehozását.

- **Adatok lemezt tárterület-fiók**. A jobb teljesítmény elérése érdekében az adatok lemezre adatbázis kiszolgálókon prémium tárterület-fiók szükséges egyszínű állapota (SSD) meghajtó technológia fogja használni. Ellenőrizze, hogy a támogatási prémium tároló rendszerbe Azure helyét.
- **NIC**. Minden egyes virtuális lesz két kártyát egy adatbázis eléréséhez, és egy kezelésére.
- **Elérhetőség beállítása**. Adatbázis-kiszolgálók egy egyetlen elérhetőségének beállítása, biztosítása érdekében a VMs legalább egyike be és futtatása közben karbantartási hozzáadódik.

### <a name="step-1---start-your-script"></a>Lépés: 1 – útmutató a parancsprogram

Töltse le a teljes bash parancsfájl, használja [az alábbi](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/classic/virtual-network-deploy-multinic-classic-cli.sh). Kövesse az alábbi lépésekkel módosíthatja a parancsfájl a munkát a környezetben.

1. Módosítsa a meglévő erőforráscsoport üzembe feletti [vonatkozó követelmények](#Prerequisites)alapján változót az alábbi értékeket.

        location="useast2"
        vnetName="WTestVNet"
        backendSubnetName="BackEnd"

2. Módosítsa a kódmentes telepítéshez használni kívánt értékek alapján változót az alábbi értékeket.

        backendCSName="IaaSStory-Backend"
        prmStorageAccountName="iaasstoryprmstorage"
        image="0b11de9248dd4d87b18621318e037d37__RightImage-Ubuntu-14.04-x64-v14.2.1"
        avSetName="ASDB"
        vmSize="Standard_DS3"
        diskSize=127
        vmNamePrefix="DB"
        osDiskName="osdiskdb"
        dataDiskPrefix="db"
        dataDiskName="datadisk"
        ipAddressPrefix="192.168.2."
        username='adminuser'
        password='adminP@ssw0rd'
        numberOfVMs=2

### <a name="step-2---create-necessary-resources-for-your-vms"></a>Lépés: 2 – a szükséges erőforrások létrehozása a VMs

1. Hozzon létre egy új, az összes kódmentes VMs felhőszolgáltatásba. Figyelje meg, a használatát a `$backendCSName` változó az erőforrás csoport neve és `$location` az Azure területhez tartozik.

        azure service create --serviceName $backendCSName \
            --location $location

2. Az operációs rendszer és az adatok lemezt Öné VMs való használatra prémium tárterület-fiók létrehozása.

        azure storage account create $prmStorageAccountName \
            --location $location \
            --type PLRS

### <a name="step-3---create-vms-with-multiple-nics"></a>Lépés a 3 - több VMs létrehozása

1. Indítsa el a több VMs alapján létrehozása hurkot a `numberOfVMs` változók.

        for ((suffixNumber=1;suffixNumber<=numberOfVMs;suffixNumber++));
        do

2. Minden virtuális adja meg a nevét, és a két kártyát mindegyikét IP-címe.

            nic1Name=$vmNamePrefix$suffixNumber-DA
            x=$((suffixNumber+3))
            ipAddress1=$ipAddressPrefix$x

            nic2Name=$vmNamePrefix$suffixNumber-RA
            x=$((suffixNumber+53))
            ipAddress2=$ipAddressPrefix$x

4. A virtuális létrehozása. Figyelje meg, a használatát a `--nic-config` paraméter, ahol az összes NIC nevét, alhálózat és IP-cím.

            azure vm create $backendCSName $image $username $password \
                --connect $backendCSName \
                --vm-name $vmNamePrefix$suffixNumber \
                --vm-size $vmSize \
                --availability-set $avSetName \
                --blob-url $prmStorageAccountName.blob.core.windows.net/vhds/$osDiskName$suffixNumber.vhd \
                --virtual-network-name $vnetName \
                --subnet-names $backendSubnetName \
                --nic-config $nic1Name:$backendSubnetName:$ipAddress1::,$nic2Name:$backendSubnetName:$ipAddress2::

5. Minden egyes virtuális hozzon létre két adatok lemez.

            azure vm disk attach-new $vmNamePrefix$suffixNumber \
                $diskSize \
                vhds/$dataDiskPrefix$suffixNumber$dataDiskName-1.vhd

            azure vm disk attach-new $vmNamePrefix$suffixNumber \
                $diskSize \
                vhds/$dataDiskPrefix$suffixNumber$dataDiskName-2.vhd
        done

### <a name="step-4---run-the-script"></a>Lépés: 4 - futtatása

Most, hogy letölti-megváltozott a szükségletek parancsfájl, futtassa a létrehozását a háttér adatbázis VMs több.

1. Mentse a parancsfájl, és a **Bash** Terminálszolgáltatások-ről. A kezdeti kimenet alább látható módon jelenik meg.

        info:    Executing command service create
        info:    Creating cloud service
        data:    Cloud service name IaaSStory-Backend
        info:    service create command OK
        info:    Executing command storage account create
        info:    Creating storage account
        info:    storage account create command OK
        info:    Executing command vm create
        info:    Looking up image 0b11de9248dd4d87b18621318e037d37__RightImage-Ubuntu-14.04-x64-v14.2.1
        info:    Looking up virtual network
        info:    Looking up cloud service
        info:    Getting cloud service properties
        info:    Looking up deployment
        info:    Creating VM

2. Néhány perc múlva végrehajtása befejeződik, és látni fogja a kimenet a többi alább látható módon.

        info:    OK
        info:    vm create command OK
        info:    Executing command vm disk attach-new
        info:    Getting virtual machines
        info:    Adding Data-Disk
        info:    vm disk attach-new command OK
        info:    Executing command vm disk attach-new
        info:    Getting virtual machines
        info:    Adding Data-Disk
        info:    vm disk attach-new command OK
        info:    Executing command vm create
        info:    Looking up image 0b11de9248dd4d87b18621318e037d37__RightImage-Ubuntu-14.04-x64-v14.2.1
        info:    Looking up virtual network
        info:    Looking up cloud service
        info:    Getting cloud service properties
        info:    Looking up deployment
        info:    Creating VM
        info:    OK
        info:    vm create command OK
        info:    Executing command vm disk attach-new
        info:    Getting virtual machines
        info:    Adding Data-Disk
        info:    vm disk attach-new command OK
        info:    Executing command vm disk attach-new
        info:    Getting virtual machines
        info:    Adding Data-Disk
        info:    vm disk attach-new command OK
