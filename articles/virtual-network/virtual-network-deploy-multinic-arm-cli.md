<properties
   pageTitle="Az erőforrás-kezelő az Azure CLI használatával több hálózati kártya VMs telepítése |} Microsoft Azure"
   description="Megtudhatja, hogy miként az Azure CLI az erőforrás-kezelő használatával több hálózati kártya VMs terjesztése"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags  
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/02/2016"
   ms.author="jdial" />

#<a name="deploy-multi-nic-vms-using-the-azure-cli"></a>Az Azure CLI használatával több hálózati kártya VMs terjesztése

[AZURE.INCLUDE [virtual-network-deploy-multinic-arm-selectors-include.md](../../includes/virtual-network-deploy-multinic-arm-selectors-include.md)]

[AZURE.INCLUDE [virtual-network-deploy-multinic-intro-include.md](../../includes/virtual-network-deploy-multinic-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)][Klasszikus telepítési modell](virtual-network-deploy-multinic-classic-cli.md).

[AZURE.INCLUDE [virtual-network-deploy-multinic-scenario-include.md](../../includes/virtual-network-deploy-multinic-scenario-include.md)]

Jelenleg VMs a egy egyetlen hálózati kártya és a több VMs nem lehet ugyanaz az erőforrás csoportjában található. Ezért kell a hátsó vége kiszolgálók megvalósítását a különböző erőforráscsoport, minden összetevő-nál. Az alábbi lépésekkel *IaaSStory* nevű fő erőforráscsoport, illetve *IaaSStory-Kódmentes* vissza vége kiszolgálók erőforráscsoport használja.

## <a name="prerequisites"></a>Előfeltételek

A visszahívási vége kiszolgálókat alkalmazhat, mielőtt kell az összes szükséges erőforrások példánkban a fő erőforráscsoport üzembe. Ezek az erőforrások telepítéséhez kövesse az alábbi lépéseket.

1. Nyissa meg [a sablon lapot](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC).
2. Kattintson a sablonként lap jobb oldalán a **szülő erőforráscsoport** **az Azure Deploy**.
3. Ha szükséges, módosítsa a paraméterértékeket, majd az Azure előzetes portál erőforráscsoport telepítéséhez kövesse a.

> [AZURE.IMPORTANT] Ellenőrizze, hogy a tárhely fióknevét egyediek. Ismétlődő tárolási fióknevét Azure-ban nem lehet.

[AZURE.INCLUDE [azure-cli-prerequisites-include.md](../../includes/azure-cli-prerequisites-include.md)]

## <a name="deploy-the-back-end-vms"></a>A háttér VMs terjesztése

A kódmentes VMs attól függenek, hogy az alább felsorolt erőforrások létrehozását.

- **Adatok lemezt tárterület-fiók**. A jobb teljesítmény elérése érdekében az adatok lemezt adatbázis kiszolgálókon prémium tárterület-fiók szükséges egyszínű állapota (SSD) meghajtó technológia fogja használni. Ellenőrizze, hogy a támogatási prémium tároló rendszerbe Azure helyét.
- **NIC**. Minden egyes virtuális lesz két kártyát egy adatbázis eléréséhez, és egy kezelésére.
- **Elérhetőség beállítása**. Egy egyetlen elérhetőségének beállítása, győződjön meg róla a VMs legalább egyike be és operációs rendszert futtató karbantartáskor kell használni hozzáadódik az összes adatbázis-kiszolgálók.

### <a name="step-1---start-your-script"></a>Lépés: 1 – útmutató a parancsprogram

Töltse le a teljes bash parancsfájl, használja [az alábbi](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/arm/virtual-network-deploy-multinic-arm-cli.sh). Kövesse az alábbi lépésekkel módosíthatja a parancsfájl a munkát a környezetben.

1. Módosítsa a meglévő erőforráscsoport üzembe feletti [vonatkozó követelmények](#Prerequisites)alapján változót az alábbi értékeket.

        existingRGName="IaaSStory"
        location="westus"
        vnetName="WTestVNet"
        backendSubnetName="BackEnd"
        remoteAccessNSGName="NSG-RemoteAccess"

2. Módosítsa a kódmentes telepítéshez használni kívánt értékek alapján változót az alábbi értékeket.

        backendRGName="IaaSStory-Backend"
        prmStorageAccountName="wtestvnetstorageprm"
        avSetName="ASDB"
        vmSize="Standard_DS3"
        diskSize=127
        publisher="Canonical"
        offer="UbuntuServer"
        sku="14.04.2-LTS"
        version="latest"
        vmNamePrefix="DB"
        osDiskName="osdiskdb"
        dataDiskName="datadisk"
        nicNamePrefix="NICDB"
        ipAddressPrefix="192.168.2."
        username='adminuser'
        password='adminP@ssw0rd'
        numberOfVMs=2

3. Az azonosító számára beolvasása a `BackEnd` alhálózat, ahol a VMs létrejön. Szükséges művelet, mivel a NIC tartozó az alhálózathoz egy másik erőforrás csoportban.

        subnetId="$(azure network vnet subnet show --resource-group $existingRGName \
                        --vnet-name $vnetName \
                        --name $backendSubnetName|grep Id)"
        subnetId=${subnetId#*/}

    >[AZURE.TIP] A fenti az első parancs a [grep](http://tldp.org/LDP/Bash-Beginners-Guide/html/sect_04_02.html) és a [karakterlánc-kezelő](http://tldp.org/LDP/abs/html/string-manipulation.html) (pontosabban karakterlánc eltávolítása) használja.

4. Az azonosító számára beolvasása a `NSG-RemoteAccess` NSG. Szükséges művelet, mivel a kapcsolódó e NSG NIC egy másik erőforrás csoportban.

        nsgId="$(azure network nsg show --resource-group $existingRGName \
                        --name $remoteAccessNSGName|grep Id)"
        nsgId=${nsgId#*/}

### <a name="step-2---create-necessary-resources-for-your-vms"></a>Lépés: 2 – a szükséges erőforrások létrehozása a VMs

1. Hozzon létre egy új erőforráscsoport összes kódmentes erőforrás. Figyelje meg, a használatát a `$backendRGName` változó az erőforrás csoport neve és `$location` az Azure területhez tartozik.

        azure group create $backendRGName $location

2. Az operációs rendszer és az adatok lemezt Öné VMs való használatra prémium tárterület-fiók létrehozása.

        azure storage account create $prmStorageAccountName \
            --resource-group $backendRGName \
            --location $location \
            --type PLRS

3. Hozzon létre egy elérhetőségét a VMs beállítása.

        azure availset create --resource-group $backendRGName \
            --location $location \
            --name $avSetName

### <a name="step-3---create-the-nics-and-backend-vms"></a>Lépés a 3 – a hálózati kártyák és kódmentes VMs létrehozása

1. Indítsa el a több VMs alapján létrehozása hurkot a `numberOfVMs` változók.

        for ((suffixNumber=1;suffixNumber<=numberOfVMs;suffixNumber++));
        do

2. Minden egyes virtuális hozzon létre egy hálózati az adatbázis eléréséhez.

            nic1Name=$nicNamePrefix$suffixNumber-DA
            x=$((suffixNumber+3))
            ipAddress1=$ipAddressPrefix$x
            azure network nic create --name $nic1Name \
                --resource-group $backendRGName \
                --location $location \
                --private-ip-address $ipAddress1 \
                --subnet-id $subnetId

3. Minden egyes virtuális hozzon létre egy hálózati távoli eléréséhez. Értesítés a `--network-security-group` paraméter, használja a hálózati kártya egy NSG szeretne társítani.

            nic2Name=$nicNamePrefix$suffixNumber-RA
            x=$((suffixNumber+53))
            ipAddress2=$ipAddressPrefix$x
            azure network nic create --name $nic2Name \
                --resource-group $backendRGName \
                --location $location \
                --private-ip-address $ipAddress2 \
                --subnet-id $subnetId $vnetName \
                --network-security-group-id $nsgId

4. A virtuális létrehozása.

            azure vm create --resource-group $backendRGName \
                --name $vmNamePrefix$suffixNumber \
                --location $location \
                --vm-size $vmSize \
                --subnet-id $subnetId \
                --availset-name $avSetName \
                --nic-names $nic1Name,$nic2Name \
                --os-type linux \
                --image-urn $publisher:$offer:$sku:$version \
                --storage-account-name $prmStorageAccountName \
                --storage-account-container-name vhds \
                --os-disk-vhd $osDiskName$suffixNumber.vhd \
                --admin-username $username \
                --admin-password $password

5. Minden egyes virtuális, hozzon létre két adatok lemez és a Befejezés az le a `done` parancsot.

            azure vm disk attach-new --resource-group $backendRGName \
                --vm-name $vmNamePrefix$suffixNumber \        
                --storage-account-name $prmStorageAccountName \
                --storage-account-container-name vhds \
                --vhd-name $dataDiskName$suffixNumber-1.vhd \
                --size-in-gb $diskSize \
                --lun 0

            azure vm disk attach-new --resource-group $backendRGName \
                --vm-name $vmNamePrefix$suffixNumber \        
                --storage-account-name $prmStorageAccountName \
                --storage-account-container-name vhds \
                --vhd-name $dataDiskName$suffixNumber-2.vhd \
                --size-in-gb $diskSize \
                --lun 1
        done


### <a name="step-4---run-the-script"></a>Lépés: 4 - futtatása

Most, hogy letölti-megváltozott a szükségletek parancsfájl, futtassa a létrehozását a háttér adatbázis VMs több.

1. Mentse a parancsfájl, és a **Bash** Terminálszolgáltatások-ről. A kezdeti kimenet alább látható módon jelenik meg.

        info:    Executing command group create
        info:    Getting resource group IaaSStory-Backend
        info:    Creating resource group IaaSStory-Backend
        info:    Created resource group IaaSStory-Backend
        data:    Id:                  /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory-Backend
        data:    Name:                IaaSStory-Backend
        data:    Location:            westus
        data:    Provisioning State:  Succeeded
        data:    Tags: null
        data:
        info:    group create command OK
        info:    Executing command storage account create
        info:    Creating storage account
        info:    storage account create command OK
        info:    Executing command availset create
        info:    Looking up the availability set "ASDB"
        info:    Creating availability set "ASDB"
        info:    availset create command OK
        info:    Executing command network nic create
        info:    Looking up the network interface "NICDB1-DA"
        info:    Creating network interface "NICDB1-DA"
        info:    Looking up the network interface "NICDB1-DA"
        data:    Id                              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory-Backend/providers/Microsoft.Network/networkInterfaces/NICDB1-DA
        data:    Name                            : NICDB1-DA
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        data:    Enable IP forwarding            : false
        data:    IP configurations:
        data:      Name                          : NIC-config
        data:      Provisioning state            : Succeeded
        data:      Private IP address            : 192.168.2.4
        data:      Private IP Allocation Method  : Static
        data:      Subnet                        : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory/providers/Microsoft.Network/virtualNetworks/WTestVNet/subnets/BackEnd
        data:
        info:    network nic create command OK
        info:    Executing command network nic create
        info:    Looking up the network interface "NICDB1-RA"
        info:    Creating network interface "NICDB1-RA"
        info:    Looking up the network interface "NICDB1-RA"
        data:    Id                              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory-Backend/providers/Microsoft.Network/networkInterfaces/NICDB1-RA
        data:    Name                            : NICDB1-RA
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        data:    Enable IP forwarding            : false
        data:    Network security group          : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory/providers/Microsoft.Network/networkSecurityGroups/NSG-RemoteAccess
        data:    IP configurations:
        data:      Name                          : NIC-config
        data:      Provisioning state            : Succeeded
        data:      Private IP address            : 192.168.2.54
        data:      Private IP Allocation Method  : Static
        data:      Subnet                        : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory/providers/Microsoft.Network/virtualNetworks/WTestVNet/subnets/BackEnd
        data:
        info:    network nic create command OK
        info:    Executing command vm create
        info:    Looking up the VM "DB1"
        info:    Using the VM Size "Standard_DS3"
        info:    The [OS, Data] Disk or image configuration requires storage account
        info:    Looking up the storage account wtestvnetstorageprm
        info:    Looking up the availability set "ASDB"
        info:    Found an Availability set "ASDB"
        info:    Looking up the NIC "NICDB1-DA"
        info:    Looking up the NIC "NICDB1-RA"
        info:    Creating VM "DB1"

2. Néhány perc múlva végrehajtása befejeződik, és látni fogja a kimenet a többi alább látható módon.

        info:    vm create command OK
        info:    Executing command vm disk attach-new
        info:    Looking up the VM "DB1"
        info:    Looking up the storage account wtestvnetstorageprm
        info:    New data disk location: https://wtestvnetstorageprm.blob.core.windows.net/vhds/datadisk1-1.vhd
        info:    Updating VM "DB1"
        info:    vm disk attach-new command OK
        info:    Executing command vm disk attach-new
        info:    Looking up the VM "DB1"
        info:    Looking up the storage account wtestvnetstorageprm
        info:    New data disk location: https://wtestvnetstorageprm.blob.core.windows.net/vhds/datadisk1-2.vhd
        info:    Updating VM "DB1"
        info:    vm disk attach-new command OK
        info:    Executing command network nic create
        info:    Looking up the network interface "NICDB2-DA"
        info:    Creating network interface "NICDB2-DA"
        info:    Looking up the network interface "NICDB2-DA"
        data:    Id                              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory-Backend/providers/Microsoft.Network/networkInterfaces/NICDB2-DA
        data:    Name                            : NICDB2-DA
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        data:    Enable IP forwarding            : false
        data:    IP configurations:
        data:      Name                          : NIC-config
        data:      Provisioning state            : Succeeded
        data:      Private IP address            : 192.168.2.5
        data:      Private IP Allocation Method  : Static
        data:      Subnet                        : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory/providers/Microsoft.Network/virtualNetworks/WTestVNet/subnets/BackEnd
        data:
        info:    network nic create command OK
        info:    Executing command network nic create
        info:    Looking up the network interface "NICDB2-RA"
        info:    Creating network interface "NICDB2-RA"
        info:    Looking up the network interface "NICDB2-RA"
        data:    Id                              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory-Backend/providers/Microsoft.Network/networkInterfaces/NICDB2-RA
        data:    Name                            : NICDB2-RA
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        data:    Enable IP forwarding            : false
        data:    Network security group          : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory/providers/Microsoft.Network/networkSecurityGroups/NSG-RemoteAccess
        data:    IP configurations:
        data:      Name                          : NIC-config
        data:      Provisioning state            : Succeeded
        data:      Private IP address            : 192.168.2.55
        data:      Private IP Allocation Method  : Static
        data:      Subnet                        : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory/providers/Microsoft.Network/virtualNetworks/WTestVNet/subnets/BackEnd
        data:
        info:    network nic create command OK
        info:    Executing command vm create
        info:    Looking up the VM "DB2"
        info:    Using the VM Size "Standard_DS3"
        info:    The [OS, Data] Disk or image configuration requires storage account
        info:    Looking up the storage account wtestvnetstorageprm
        info:    Looking up the availability set "ASDB"
        info:    Found an Availability set "ASDB"
        info:    Looking up the NIC "NICDB2-DA"
        info:    Looking up the NIC "NICDB2-RA"
        info:    Creating VM "DB2"
        info:    vm create command OK
        info:    Executing command vm disk attach-new
        info:    Looking up the VM "DB2"
        info:    Looking up the storage account wtestvnetstorageprm
        info:    New data disk location: https://wtestvnetstorageprm.blob.core.windows.net/vhds/datadisk2-1.vhd
        info:    Updating VM "DB2"
        info:    vm disk attach-new command OK
        info:    Executing command vm disk attach-new
        info:    Looking up the VM "DB2"
        info:    Looking up the storage account wtestvnetstorageprm
        info:    New data disk location: https://wtestvnetstorageprm.blob.core.windows.net/vhds/datadisk2-2.vhd
        info:    Updating VM "DB2"
        info:    vm disk attach-new command OK
