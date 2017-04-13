<properties 
   pageTitle="Hogyan kell beállítani a statikus magánjellegű IP-cím használata a CLI ARM módban |} Microsoft Azure"
   description="Statikus IP-címei (immerzióban), és hogyan kezelhetők ARM módban használja a CLI ismertetése"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
   tags="azure-resource-manager"
/>
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/15/2016"
   ms.author="jdial" />

# <a name="how-to-set-a-static-private-ip-address-in-azure-cli"></a>Hogyan kell beállítani egy privát statikus IP-címet az Azure CLI

[AZURE.INCLUDE [virtual-networks-static-private-ip-selectors-arm-include](../../includes/virtual-networks-static-private-ip-selectors-arm-include.md)]

[AZURE.INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Ez a cikk bemutatja az erőforrás-kezelő telepítési modell. Kezelheti [a klasszikus telepítési modell statikus magánjellegű IP-címet](virtual-networks-static-private-ip-classic-cli.md).

[AZURE.INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

A minta Azure CLI parancsok az alábbi fejlemények már létrehozott egy egyszerű környezetben. Ha szeretne a dokumentumban megjelenített parancsokat, először összeállítása a [Hozzon létre egy vnet](virtual-networks-create-vnet-arm-cli.md)ismertetett tesztkörnyezetben.

## <a name="how-to-specify-a-static-private-ip-address-when-creating-a-vm"></a>A magánjellegű statikus IP-cím megadása, egy virtuális létrehozásakor
A virtuális *DNS01* nevű fájlt, egy névvel ellátott *TestVNet* egy *192.168.1.101*magánjellegű statikus IP-VNet a *FrontEnd* alhálózat létrehozásához kövesse az alábbi lépéseket:

1. Ha még sosem használt Azure CLI, lásd: [Telepítse és állítsa be az Azure CLI](../xplat-cli-install.md) , és kövesse az utasításokat a pont, ahol be az Azure-fiók és az előfizetés felfelé.

2. Futtassa az erőforrás-kezelő mód, váltson a **azure konfiguráció mód** parancsot, alább látható módon.

        azure config mode arm

    Várt kimenet:

        info:    New mode is arm

3. Futtassa az **azure hálózati nyilvános ip-hozzon létre** egy nyilvános IP létrehozása a virtuális. A kimenet után megjelenik a paramétereket ismerteti.

        azure network public-ip create -g TestRG -n TestPIP -l centralus
    
    Várt kimenet:

        info:    Executing command network public-ip create
        + Looking up the public ip "TestPIP"
        + Creating public ip address "TestPIP"
        + Looking up the public ip "TestPIP"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/publicIPAddresses/TestPIP
        data:    Name                            : TestPIP
        data:    Type                            : Microsoft.Network/publicIPAddresses
        data:    Location                        : centralus
        data:    Provisioning state              : Succeeded
        data:    Allocation method               : Dynamic
        data:    Idle timeout                    : 4
        info:    network public-ip create command OK

    - **-g (vagy – erőforrás-csoport)**. A csoport nevét, az erőforrás a nyilvános IP jön létre.
    - **-n (vagy--neve)**. A nyilvános IP-nevét.
    - **-l (vagy--helyről)**. Azure terület, ahol hozható létre a nyilvános IP. A példában *centralus*.

3. Hozzon létre egy hálózati kártya statikus magánjellegű IP-cím az **azure hálózati hálózati kártya létrehozása** parancs futtatásával. A kimenet után megjelenik a paramétereket ismerteti.

        azure network nic create -g TestRG -n TestNIC -l centralus -a 192.168.1.101 -m TestVNet -k FrontEnd

    Várt kimenet:

        + Looking up the network interface "TestNIC"
        + Looking up the subnet "FrontEnd"
        + Creating network interface "TestNIC"
        + Looking up the network interface "TestNIC"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC
        data:    Name                            : TestNIC
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : centralus
        data:    Provisioning state              : Succeeded
        data:    Enable IP forwarding            : false
        data:    IP configurations:
        data:      Name                          : NIC-config
        data:      Provisioning state            : Succeeded
        data:      Private IP address            : 192.168.1.101
        data:      Private IP Allocation Method  : Static
        data:      Subnet                        : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
        data:
        info:    network nic create command OK

    - **-(vagy – magánjellegű-ip-cím)**. A hálózati statikus saját IP-cím
    - **m. (vagy – alhálózat-vnet-neve)**. A VNet, ahol a hálózati kártya létrejön neve.
    - **-k (vagy – alhálózat neve)**. Az alhálózathoz, ahol hozható létre a hálózati kártya neve.

4. Futtassa a virtuális használata a nyilvános IP- és hálózati az előbb létrehozott létrehozása **azure virtuális létrehozása** parancsot. A kimenet után megjelenik a paramétereket ismerteti.

        azure vm create -g TestRG -n DNS01 -l centralus -y Windows -f TestNIC -i TestPIP -F TestVNet -j FrontEnd -o vnetstorage -q bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2 -u adminuser -p AdminP@ssw0rd

    Várt kimenet:

        info:    Executing command vm create
        + Looking up the VM "DNS01"
        info:    Using the VM Size "Standard_A1"
        warn:    The image "bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2" will be used for VM
        info:    The [OS, Data] Disk or image configuration requires storage account
        + Looking up the storage account vnetstorage
        + Looking up the NIC "TestNIC"
        info:    Found an existing NIC "TestNIC"
        info:    Found an IP configuration with virtual network subnet id "/subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd" in the NIC "TestNIC"
        info:    Found public ip parameters, trying to setup PublicIP profile
        + Looking up the public ip "TestPIP"
        info:    Found an existing PublicIP "TestPIP"
        info:    Configuring identified NIC IP configuration with PublicIP "TestPIP"
        + Updating NIC "TestNIC"
        + Looking up the NIC "TestNIC"
        + Creating VM "DNS01"
        info:    vm create command OK

    - **-y (vagy – os típusú)**. A virtuális gép operációs rendszert *Windows* vagy *Linux rendszerhez*.
    - **az f-(vagy a hálózati kártya – neve)**. A virtuális fogja használni a hálózati neve.
    - **-i (vagy – nyilvános ip-neve)**. A virtuális nyilvános IP-neve fogja használni.
    - **Az F-(vagy--vnet neve)**. A VNet, ahol a virtuális létrejön neve.
    - **-j (vagy – vnet-alhálózat-neve)**. Az alhálózathoz, ahol hozható létre a virtuális neve.

## <a name="how-to-retrieve-static-private-ip-address-information-for-a-vm"></a>Hogyan statikus magánjellegű IP-cím adatait egy virtuális beolvasása

Statikus magánjellegű IP-cím információit a fenti forgatókönyvvel létre virtuális megtekintéséhez futtassa a következő Azure CLI parancsot, és tekintse meg az értékeket a *magánjellegű IP lefoglalása-módszert* és a *személyes IP-címe*:

    azure vm show -g TestRG -n DNS01

Várt kimenet:

    info:    Executing command vm show
    + Looking up the VM "DNS01"
    + Looking up the NIC "TestNIC"
    + Looking up the public ip "TestPIP
    data:    Id                              :/subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Compute/virtualMachines/DNS01
    data:    ProvisioningState               :Succeeded
    data:    Name                            :DNS01
    data:    Location                        :centralus
    data:    Type                            :Microsoft.Compute/virtualMachines
    data:
    data:    Hardware Profile:
    data:      Size                          :Standard_A1
    data:
    data:    Storage Profile:
    data:      Source image:
    data:        Id                          :/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/services/images/bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2
    data:
    data:      OS Disk:
    data:        OSType                      :Windows
    data:        Name                        :cli08d7bd987a0112a8-os-1441774961355
    data:        Caching                     :ReadWrite
    data:        CreateOption                :FromImage
    data:        Vhd:
    data:          Uri                       :https://vnetstorage2.blob.core.windows.net/vhds/cli08d7bd987a0112a8-os-1441774961355vhd
    data:
    data:    OS Profile:
    data:      Computer Name                 :DNS01
    data:      User Name                     :adminuser
    data:      Windows Configuration:
    data:        Provision VM Agent          :true
    data:        Enable automatic updates    :true
    data:
    data:    Network Profile:
    data:      Network Interfaces:
    data:        Network Interface #1:
    data:          Id                        :/subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC
    data:          Primary                   :true
    data:          MAC Address               :00-0D-3A-90-1A-A8
    data:          Provisioning State        :Succeeded
    data:          Name                      :TestNIC
    data:          Location                  :centralus
    data:            Private IP alloc-method :Static
    data:            Private IP address      :192.168.1.101
    data:            Public IP address       :40.122.213.159
    info:    vm show command OK

## <a name="how-to-remove-a-static-private-ip-address-from-a-vm"></a>A magánjellegű statikus IP-cím eltávolítása egy virtuális
Az erőforrás-kezelő Azure CLI egy hálózati nem távolítható el egy privát statikus IP-címet. Meg kell hozzon létre egy új hálózati kártya egy dinamikus IP használó, az előző hálózati kártya eltávolítása a virtuális, és adja hozzá az új hálózati kártya a virtuális. A hálózati a használt virtuális int eh a fenti parancsok módosításához kövesse az alábbi lépéseket.
    
1. Futtassa a hozhat létre egy új hálózati kártya használata a dinamikus IP-terhelés **azure hálózati hálózati kártya létrehozása** parancsot. Ne feledje, hogy hogyan nem kell az IP-cím ezúttal megadása.

        azure network nic create -g TestRG -n TestNIC2 -l centralus -m TestVNet -k FrontEnd

    Várt kimenet:

        info:    Executing command network nic create
        + Looking up the network interface "TestNIC2"
        + Looking up the subnet "FrontEnd"
        + Creating network interface "TestNIC2"
        + Looking up the network interface "TestNIC2"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC2
        data:    Name                            : TestNIC2
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : centralus
        data:    Provisioning state              : Succeeded
        data:    Enable IP forwarding            : false
        data:    IP configurations:
        data:      Name                          : NIC-config
        data:      Provisioning state            : Succeeded
        data:      Private IP address            : 192.168.1.6
        data:      Private IP Allocation Method  : Dynamic
        data:      Subnet                        : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
        data:
        info:    network nic create command OK

2. Futtassa a hálózati kártya a virtuális által használt módosítása **azure virtuális beállítása** parancsot.

        azure vm set -g TestRG -n DNS01 -N TestNIC2

    Várt kimenet:

        info:    Executing command vm set
        + Looking up the VM "DNS01"
        + Looking up the NIC "TestNIC2"
        + Updating VM "DNS01"
        info:    vm set command OK

3. Ha megy végbe, futtassa a **azure hálózati hálózati törlése** parancsot a régi adaptert törlése

        azure network nic delete -g TestRG -n TestNIC --quiet

    Várt kimenet:

        info:    Executing command network nic delete
        + Looking up the network interface "TestNIC"
        + Deleting network interface "TestNIC"
        info:    network nic delete command OK

## <a name="how-to-add-a-static-private-ip-address-to-an-existing-vm"></a>A magánjellegű statikus IP-cím felvétele egy meglévő virtuális
A magánjellegű statikus IP-cím hozzáadása a hálózati kártya a virtuális a fenti parancsfájl használatával létrehozott által használt, futtassa a következő parancsot:

    azure network nic set -g TestRG -n TestNIC2 -a 192.168.1.101

Várt kimenet:

    info:    Executing command network nic set
    + Looking up the network interface "TestNIC2"
    + Updating network interface "TestNIC2"
    + Looking up the network interface "TestNIC2"
    data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC2
    data:    Name                            : TestNIC2
    data:    Type                            : Microsoft.Network/networkInterfaces
    data:    Location                        : centralus
    data:    Provisioning state              : Succeeded
    data:    MAC address                     : 00-0D-3A-90-29-25
    data:    Enable IP forwarding            : false
    data:    Virtual machine                 : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Compute/virtualMachines/DNS01
    data:    IP configurations:
    data:      Name                          : NIC-config
    data:      Provisioning state            : Succeeded
    data:      Private IP address            : 192.168.1.101
    data:      Private IP Allocation Method  : Static
    data:      Subnet                        : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
    data:
    info:    network nic set command OK

## <a name="next-steps"></a>Következő lépések

- [Fenntartott nyilvános IP](virtual-networks-reserved-public-ip.md) -címekről bővebb ismertetőt.
- [Példány szintű nyilvános IP (ILPIP)](virtual-networks-instance-level-public-ip.md) -címekről bővebb ismertetőt.
- Nézze meg a [fenntartott IP REST API-khoz](https://msdn.microsoft.com/library/azure/dn722420.aspx).
